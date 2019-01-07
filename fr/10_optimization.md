# Optimisations

Bienvenue dans le dernier chapitre du livre. Le chemin a été long mais vous n'êtes qu'à un pas de la fin. Dans le chapitre précédent, nous avons terminé la modélisation du modèle de commandes. Nous pourrions dire que le projet est maintenant terminé mais je veux couvrir quelques détails importants sur l'optimisation. Les sujets que je vais aborder ici seront:

- La mise en place de la spécification [JSON:API](https://jsonapi.org/)
- la pagination
- les tâches en arrière plan
- la mise en cache

J'essaierai d'aller aussi loin que possible en essayant de couvrir certains scénarios courants. J'espère que ces scénario vous serons utiles pour certains de vos projets.

Si vous commencez à lire à ce stade, vous voudrez probablement que le code fonctionne, vous pouvez le cloner comme ça:

~~~bash
$ git clone https://github.com/madeindjs/market_place_api.git -b chapter9
~~~

Créons une nouvelle branche pour ce chapitre:

~~~bash
$ git checkout -b chapter10
~~~

## Mettre en conformité notre structure JSON avec celle de [JSON:API][jsonapi]

Comme je vous le dis depuis le début de ce livre, une partie importante et difficile lors de la création de votre API est de décider le format de sortie. Heureusement, certaines organisations ont déjà fait face à ce genre de problème et elles ont ainsi établies certaines conventions.

Une des convention les plus appliquée est très certainement [JSON:API][jsonapi]. Cette convention nous permettra d'aborder la pagination plus sereinement dans la prochaine section.

Ainsi, la [documentation de JSON:API][jsonapi_documentation] nous donne quelques règles à suivre concernant le formatage du document JSON.

Ainsi, notre document **doit** contenir ces clefs:

- `data`: qui doit contenir les données que nous renvoyons
- `errors` qui doit contenir un tableau des erreurs qui sont survenues.
- `meta` qui contient un [objet meta][jsonapi_meta]

> Les clés `data` et `errors` ne doivent pas être présente en même temps et c'est logique puisque si une erreur survient nous ne devrions pas être en mesure de rendre des données correctes.

Le contenu de la clé `data`est lui aussi assez stricte:

- il doit posséder une clé `type`qui décrit le type du modèle JSON (si c'est un article, un utilisateur, etc..)
- les proprités de l'objets doivent être placées dans une clé `attributes` et les undescore (`_`) sont remplacés par des tirets (`-`)
- les liaisons de l'objets doivent être placées dans une clé `relationships`

Cela risque de nous faire pas mal de changement puisque nous n'avons implémenté aucune de ses règles. Ne vous inquiétéz pas, nous avons mis en place des tests unitaires afin de nous assurer qu'il n'y aura pas de régression. Commençons doc par les mettre à jour

### Les utilisateurs

Commençons donc par le contrôlleur des commandes. Intéressons nous à l'action `show`:

~~~ruby
# spec/controllers/api/v1/users_controller_spec.rb
require 'rails_helper'

RSpec.describe Api::V1::UsersController, type: :controller do
  describe 'GET #show' do
    # ...
    it 'returns the information about a reporter on a hash' do
      expect(json_response[:email]).to eql @user.email
    end

    it 'has the product ids as an embeded object' do
      expect(json_response[:product_ids]).to eql []
    end
  end
  # ...
end
~~~

Nous devons mettre à jour les emplacement. Voici le fichier complet

~~~ruby
# spec/controllers/api/v1/users_controller_spec.rb
require 'rails_helper'

RSpec.describe Api::V1::UsersController, type: :controller do
  describe 'GET #show' do
    # ...

    it 'returns the information about a reporter on a hash' do
      expect(json_response[:data][:attributes][:email]).to eql @user.email
    end

    it 'has the product ids as an embeded object' do
      expect(json_response[:data][:attributes][:'product-ids']).to eql []
    end
  end

  describe 'POST #create' do
    context 'when is successfully created' do
      # ...

      it 'renders the json representation for the user record just created' do
        expect(json_response[:data][:attributes][:email]).to eql @user_attributes[:email]
      end
    end

    # ...
  end

  describe 'PUT/PATCH #update' do
    context 'when is successfully updated' do
      # ...

      it 'renders the json representation for the updated user' do
        expect(json_response[:data][:attributes][:email]).to eql 'newmail@example.com'
      end
    end

    # ...
  end

  # ...
end
~~~

Et voilà, ça fait beaucoup de code mais les changements sont minimes

### Les Sesions

Pour les sessions, un seul test doit être mis à jour: celui qui récupère le `auth_token`.

~~~ruby
# spec/controllers/api/v1/sessions_controller_spec.rb
require 'rails_helper'

RSpec.describe Api::V1::SessionsController, type: :controller do
  describe 'POST #create' do
    # ...

    context 'when the credentials are correct' do
      # ...
      it 'returns the user record corresponding to the given credentials' do
        @user.reload
        expect(json_response[:data][:attributes][:'auth-token']).to eql @user.auth_token
      end
      # ...
    end

    # ...
end
~~~

> Rappelez vos bien que dans les spécification JSON:API, les undescore (`_`) sont remplacés par des tirets (`-`)

### Les commandes

Pour les commandes, il y a une petite spécifité car pour récupérer l'utilisateur associé, nous devons passer par la clé `:relationships`. En dehors de ça, le principe reste iddentique:

~~~ruby
# spec/controllers/api/v1/products_controller_spec.rb
require 'rails_helper'

RSpec.describe Api::V1::ProductsController, type: :controller do
  describe 'GET #show' do
    # ...
    it 'returns the information about a reporter on a hash' do
      expect(json_response[:data][:attributes][:title]).to eql @product.title
    end

    it 'has the user as a embeded object' do
      puts json_response.inspect
      expect(json_response[:data][:relationships][:user][:attributes][:email]).to eql @product.user.email
    end
    # ...
  end

  describe 'GET #index' do
    # ...

    context 'when is not receiving any product_ids parameter' do
      # ...
      it 'returns 4 records from the database' do
        expect(json_response[:data]).to have(4).items
      end
      it 'returns the user object into each product' do
        json_response.each do |product_response|
          expect(product_response[:data][:relationships][:user]).to be_present
        end
      end
      # ...
    end

    context 'when product_ids parameter is sent' do
      # ...
      it 'returns just the products that belong to the user' do
        json_response.each do |product_response|
          expect(product_response[:data][:relationships][:user][:attributes][:email]).to eql @user.email
        end
      end
    end
  end

  describe 'POST #create' do
    context 'when is successfully created' do
      # ...
      it 'renders the json representation for the product record just created' do
        expect(json_response[:data][:attributes][:title]).to eql @product_attributes[:title]
      end
      # ...
    end
    # ...
  end

  describe 'PUT/PATCH #update' do
    # ...
    context 'when is successfully updated' do
      # ...
      it 'renders the json representation for the updated user' do
        expect(json_response[:data][:attributes][:title]).to eql 'An expensive TV'
      end
      # ...
    end
    # ...
  end
  # ...
end
~~~

#### Les produits

Là encore, cela fait beaucoup de code mais en réalité il y a très peu de changement.

~~~ruby
# spec/controllers/api/v1/products_controller_spec.rb
# ...

RSpec.describe Api::V1::ProductsController, type: :controller do
  describe 'GET #show' do
    # ...

    it 'returns the information about a reporter on a hash' do
      expect(json_response[:data][:attributes][:title]).to eql @product.title
    end

    it 'has the user as a embeded object' do
      expect(json_response[:data][:relationships][:user][:attributes][:email]).to eql @product.user.email
    end
  end

  describe 'GET #index' do
    # ...
    context 'when is not receiving any product_ids parameter' do
      # ...
      it 'returns 4 records from the database' do
        expect(json_response[:data]).to have(4).items
      end

      it 'returns the user object into each product' do
        json_response.each do |product_response|
          expect(product_response[:data][:relationships][:user]).to be_present
        end
      end
    end

    context 'when product_ids parameter is sent' do
      # ...
      it 'returns just the products that belong to the user' do
        json_response.each do |product_response|
          expect(product_response[:data][:relationships][:user][:attributes][:email]).to eql @user.email
        end
      end
    end
  end

  describe 'POST #create' do
    context 'when is successfully created' do
      # ...
      it 'renders the json representation for the product record just created' do
        product_response = json_response
        expect(product_response[:data][:attributes][:title]).to eql @product_attributes[:title]
      end
      # ...
    end

    context 'when is not created' do
      # ...
      it 'renders the json errors on whye the user could not be created' do
        product_response = json_response
        expect(product_response[:errors][:price]).to include 'is not a number'
      end
      # ...
    end
  end

  describe 'PUT/PATCH #update' do
    # ...
    context 'when is successfully updated' do
      # ...
      it 'renders the json representation for the updated user' do
        expect(json_response[:data][:attributes][:title]).to eql 'An expensive TV'
      end
      # ...
    end
    # ...
  end
  # ...
end
~~~

### L'implémentation

Depuis le début, afin de sérialiser nos modèles, nous vons utilisé *Active Model Serializer*. Heureusement pour nous, cette librairie propose plusieurs **adaptateurs**. Les adapateurs sont en quelques sorte des modèles de JSON à appliquer à tous nos sérialiseur. C'est parfait.

La [documentation de *Active Model Serializer*][jsonapi_adapter] nous montre propose une liste des adaptateurs existants. Et, si vous voyez ou je veux en venir, il en existe une toute prête pour le modèle JSON:API! Pour le mettre en place, il suffit simplement d'activer l'adapter en créant le fichier suivant:

~~~ruby
# config/initializers/activemodel_serializer.rb
ActiveModelSerializers.config.adapter = :json_api
~~~

Nous devons aussi indique le type de l'objet du serialiseur. *Active Model Serializer* propose une méthode toute fate pour cela: `type`. L'implémentation est donc très facile:

~~~ruby
# app/serializers/order_serializer.rb
class OrderSerializer < ActiveModel::Serializer
  type :order
  # ...
end
~~~

~~~ruby
# app/serializers/product_serializer.rb
class ProductSerializer < ActiveModel::Serializer
  type :product
  # ...
end
~~~

~~~ruby
# app/serializers/user_serializer.rb
class UserSerializer < ActiveModel::Serializer
  type :user
  # ...
end
~~~

Et c'est tout! Lançon maintenant **tous** nos tests pour voir s'ils passent:

~~~bash
$ rspec spec
...........F.F.F.......................................................................................

Failures:

  1) Api::V1::ProductsController GET #show has the user as a embeded object
     Failure/Error: expect(json_response[:data][:relationships][:user][:attributes][:email]).to eql @product.user.email
     ...

  2) Api::V1::ProductsController GET #index when is not receiving any product_ids parameter returns the user object into each product
     Failure/Error: expect(product_response[:data][:relationships][:user]).to be_present
     ...

  3) Api::V1::ProductsController GET #index when product_ids parameter is sent returns just the products that belong to the user
     Failure/Error: expect(product_response[:data][:relationships][:user][:attributes][:email]).to eql @user.email
     ...

Finished in 1.35 seconds (files took 1.1 seconds to load)
103 examples, 3 failures
~~~

Arf... Tous nos tests passent mais on voit que l'utilisateur associé au produit n'est pas intégré dans la réponse. Ceci est en fait tout à fait normal. [La documentation de JSON:API][jsonapi_includes] préconise l'utilsation d'une clé `include` plutôt que d'imbriquer les modèles entre eux.

Metons donc à jour notre test:

~~~ruby
# spec/controllers/api/v1/products_controller_spec.rb
require 'rails_helper'

RSpec.describe Api::V1::ProductsController, type: :controller do
  describe 'GET #show' do
    # ...
    it 'has the user as a embeded object' do
      expect(json_response[:included].first[:attributes][:email]).to eql @product.user.email
    end
  end

  describe 'GET #index' do
    # ...
    context 'when is not receiving any product_ids parameter' do
      # ...
      it 'returns the user object into each product' do
        expect(json_response[:included]).to be_present
      end
      # ...
    end

    context 'when product_ids parameter is sent' do
      # ...
      it 'returns just the products that belong to the user' do
        expect(json_response[:included].first[:id].to_i).to eql @user.id
      end
    end
  end
  # ...
end

~~~

Là aussi, l'implémentation est très facile. Il nous suffit d'ajouter l'otpion `ìnclude` directement dans l'action du controlleur.

~~~ruby
# app/controllers/api/v1/products_controller.rb
class Api::V1::ProductsController < ApplicationController
  #...
  def index
    render json: Product.search(params), include: [:user]
  end

  def show
    render json: Product.find(params[:id]), include: [:user]
  end
  #...
end
~~~

Relançons tous les tests pour être sûr que notre implémentation finale est correct:


~~~bash
$ rspec spec
.......................................................................................................

Finished in 2.12 seconds (files took 1.4 seconds to load)
103 examples, 0 failures
~~~

Et voilà le travail. Vu que nous sommes content de notre travail, faisons un *commit*:

~~~bash
$ git add .
$ git commit -m "Respect JSON:API response format"
~~~

## Pagination

Une stratégie très commune pour optimiser la récupération d'enregistrements dans une base de données est de charger seulement une quantité limité en les paginant. Si vous êtes familier avec cette technique, vous savez qu'avec Rails c'est vraiment très facile à mettre en place avec des gemmes telles que [will\_paginate](https://github.com/mislav/will_paginate) ou [kaminari](https://github.com/kaminari/kaminari).

La seule partie délicate ici est de savoir comment gérer la sortie JSON pour donner assez d'informations au client sur la façon dont le tableau est paginé. Dans la section précédente, j'ai partagé quelques ressources sur les pratiques que j'allais suivre ici. L'une d'entre elles était <http://jsonapi.org/> qui est une page incontournable des signets.

Si nous lisons la section sur le format, nous arriverons à une sous-section appelée [Top Level](https://jsonapi.org/format/#document-top-level). Pour vous expliquer rapidement, ils mentionnent quelque chose sur la pagination:

> "meta": méta-information sur une ressource, telle que la pagination.

Ce n'est pas très descriptif mais au moins nous avons un indice sur ce qu'il faut regarder ensuite au sujet de l'implémentation de la pagination. Ne vous inquiétez pas, c'est exactement ce que nous allons faire ici.

Commençons par la liste des produits.

### Les produits

Nous allons commencer par paginer la liste des produits car nous n'avons aucune restriction d'accès. Cela nous facilitera les tests.

Nous devons d'abord ajouter la gemme de kaminari à notre `Gemfile`:

~~~bash
$ bundle add kaminari
~~~

Maintenant nous pouvons aller à l'action `Products#index` et ajouter les méthodes de pagination comme indiqué dans la documentation:

~~~ruby
# app/controllers/api/v1/products_controller.rb
class Api::V1::ProductsController < ApplicationController
  # ...
  def index
    render json: Product.page(params[:page]).per(params[:per_page]).search(params)
  end
  # ...
end
~~~

Jusqu'à présent, la seule chose qui a changé est la requête sur la base de données pour limiter le résultat à 25 par page (ce qui est la valeur par défaut). Mais nous n'avons toujours pas ajouté d'informations supplémentaires à la sortie JSON.

Nous devons fournir les informations de pagination sur la balise meta dans le formulaire suivant:

~~~json
"meta": {
    "pagination": {
        "per_page": 25,
        "total_page": 6,
        "total_objects": 11
    }
}
~~~

Maintenant que nous avons la structure finale de la balise meta, il ne nous reste plus qu'à la sortir sur la réponse JSON. Ajoutons d'abord quelques tests:

~~~ruby
# spec/controllers/api/v1/products_controller_spec.rb
require 'rails_helper'

RSpec.describe Api::V1::ProductsController, type: :controller do
  # ...
  describe 'GET #index' do
    before(:each) do
      4.times { FactoryBot.create :product }
      get :index
    end
    # ...
    it 'Have a meta pagination tag' do
      expect(json_response).to have_key(:meta)
      expect(json_response[:meta]).to have_key(:pagination)
      expect(json_response[:meta][:pagination]).to have_key(:'per-page')
      expect(json_response[:meta][:pagination]).to have_key(:'total-pages')
      expect(json_response[:meta][:pagination]).to have_key(:'total-objects')
    end

    it { expect(response.response_code).to eq(200) }
  end
  # ...
end
~~~

Le test que nous venons d'ajouter devrait échouer or, si nous exécutons les tests, deux tests échouent. Cela veux dire que nous avons cassé quelque chose d'autre:

~~~bash
$ bundle exec rspec spec/controllers/api/v1/products_controller_spec.rb
...F....F...........

Failures:

  1) Api::V1::ProductsController GET #index Have a meta pagination tag
     ...

  2) Api::V1::ProductsController GET #index when product_ids parameter is sent returns just the products that belong to the user
     Failure/Error: total_pages: products.total_pages,

     NoMethodError:
       undefined method 'total_pages' for #<Array:0x0000556f1ef85c68>
     # ./app/controllers/api/v1/products_controller.rb:12:in 'index'
     ...

Finished in 0.40801 seconds (files took 0.62979 seconds to load)
20 examples, 2 failures
~~~

L'erreur est en fait sur la méthode `Product.search`. En fait, Kaminari attend une relation d'enregistrement au lieu d'un tableau. C'est très facile à réparer:

~~~ruby
# app/models/product.rb
class Product < ApplicationRecord
  # ...
  def self.search(params = {})
    products = params[:product_ids].present? ? Product.where(id: params[:product_ids]) : Product.all
    # ...
  end
end
~~~

Vous avez remarqué le changement? Laissez moi vous l'expliquer. Nous avons simplement remplacé la méthode `Product.find` par `Product.where` en utilisant les paramètres `product_ids`. La différence est que la méthode `where` retourne une `ActiveRecord::Relation` et c'est exactement ce dont nous avons besoin.

Maintenant, si nous relançons les tests, le test que nous avions cassé devrait maintenant passer:

~~~bash
$ bundle exec rspec spec/controllers/api/v1/products_controller_spec.rb
...F................

Failures:

  1) Api::V1::ProductsController GET #index Have a meta pagination tag
     ...

Finished in 0.41533 seconds (files took 0.5997 seconds to load)
20 examples, 1 failure
~~~

Maintenant que nous avons corrigé cela, ajoutons les informations de pagination. Nous devons le faire dans le fichier `products_controller.rb`:

~~~ruby
# app/controllers/api/v1/products_controller.rb
class Api::V1::ProductsController < ApplicationController
  before_action :authenticate_with_token!, only: %i[create update destroy]

  def index
    products = Product.search(params).page(params[:page]).per(params[:per_page])
    render(
      json: products,
      include: [:user],
      meta: {
        pagination: {
          per_page: params[:per_page],
          total_pages: products.total_pages,
          total_objects: products.total_count
        }
      }
    )
  end
  # ...
end
~~~

Maintenant, si on vérifie les spécifications, elles devraient toutes passer:

~~~bash
$ bundle exec rspec spec/controllers/api/v1/products_controller_spec.rb
....................

Finished in 0.66813 seconds (files took 2.72 seconds to load)
20 examples, 0 failures
~~~

Maintenant que nous avons fait une superbe optimisation pour la route de la liste des produits, c'est au client de récupérer la `page` avec le bon paramètre `per_page` pour les enregistrements.

*Commitons* ces changements et continuons avec la liste des commandes.

~~~bash
$ git add .
$ git commit -m "Adds pagination for the products index action to optimize response"
~~~

### Liste des commandes

Maintenant, il est temps de faire exactement la même chose pour la route de la liste des commandes. Cela devrait être très facile à mettre en œuvre. Mais d'abord, ajoutons quelques test au fichier `orders_controller_spec.rb`:


~~~ruby
# spec/controllers/api/v1/orders_controller_spec.rb
require 'rails_helper'

RSpec.describe Api::V1::OrdersController, type: :controller do
  describe 'GET #index' do
    before(:each) do
      current_user = FactoryBot.create :user
      api_authorization_header current_user.auth_token
      4.times { FactoryBot.create :order, user: current_user }
      get :index, params: { user_id: current_user.id }
    end

    it 'returns 4 order records from the user' do
      expect(json_response[:data]).to have(4).items
    end

    it 'Have a meta pagination tag' do
      expect(json_response).to have_key(:meta)
      expect(json_response[:meta]).to have_key(:pagination)
      expect(json_response[:meta][:pagination]).to have_key(:'per-page')
      expect(json_response[:meta][:pagination]).to have_key(:'total-pages')
      expect(json_response[:meta][:pagination]).to have_key(:'total-objects')
    end

    it { expect(response.response_code).to eq(200) }
  end
  # ...
end
~~~

Et, comme vous vous en doutez peut-être déjà, nos tests ne passent plus:

~~~bash
$ rspec spec/controllers/api/v1/orders_controller_spec.rb
.F........

Failures:

  1) Api::V1::OrdersController GET #index Have a meta pagination tag
     Failure/Error: expect(json_response).to have_key(:meta)
       expected #has_key?(:meta) to return true, got false
     # ./spec/controllers/api/v1/orders_controller_spec.rb:18:in `block (3 levels) in <top (required)>'

Finished in 0.66262 seconds (files took 2.74 seconds to load)
10 examples, 1 failure
~~~

Transformons le rouge en vert:

~~~ruby
# app/controllers/api/v1/orders_controller.rb
class Api::V1::OrdersController < ApplicationController
  before_action :authenticate_with_token!

  def index
    orders = current_user.orders.page(params[:page]).per(params[:per_page])
    render(
      json: orders,
      meta: {
        pagination: {
          per_page: params[:per_page],
          total_pages: orders.total_pages,
          total_objects: orders.total_count
        }
      }
    )
  end
  # ...
end
~~~

Les tests devraient maintenant passer:

~~~bash
$ rspec spec/controllers/api/v1/orders_controller_spec.rb
..........

Finished in 0.35201 seconds (files took 0.9404 seconds to load)
10 examples, 0 failures
~~~

Faisons un *commit* avant d'avancer

~~~bash
$ git commit -am "Adds pagination for orders index action"
~~~

### Factorisation de la pagination

Si vous avez suivi ce tutoriel ou si vous êtes un développeur Rails expérimenté, vous aimez probablement garder les choses DRY. Vous avez sûrement remarqué que le code que nous venons d'écrire est dupliqué. Je pense que c'est une bonne habitde de nettoyer un peu le code une fois la fonctionnalité implémentée.

Nous allons d'abord commencer par nettoyer ces tests qu'on a dupliqué dans le fichier `orders_controller_spec.rb` et `products_controller_spec.rb`:

~~~ruby
it 'Have a meta pagination tag' do
  expect(json_response).to have_key(:meta)
  expect(json_response[:meta]).to have_key(:pagination)
  expect(json_response[:meta][:pagination]).to have_key(:'per-page')
  expect(json_response[:meta][:pagination]).to have_key(:'total-pages')
  expect(json_response[:meta][:pagination]).to have_key(:'total-objects')
end
~~~

Afin de le factoriser, nous allons créer un dossier `shared_examples` dans le dossier `spec/support/`.

~~~bash
$ mkdir spec/support/shared_examples
~~~

Et maintenant, créons un fichier qui contiendra le code dupliqué

~~~ruby
# spec/support/shared_examples/pagination.rb
shared_examples 'paginated list' do
  it 'Have a meta pagination tag' do
    expect(json_response).to have_key(:meta)
    expect(json_response[:meta]).to have_key(:pagination)
    expect(json_response[:meta][:pagination]).to have_key(:'per-page')
    expect(json_response[:meta][:pagination]).to have_key(:'total-pages')
    expect(json_response[:meta][:pagination]).to have_key(:'total-objects')
  end
end
~~~

Cet exemple partagé peut maintenant être utilisé pour remplacer les cinq tests des fichiers `orders_controller_spec.rb` et `products_controller_spec.rb`:

~~~ruby
# spec/controllers/api/v1/orders_controller_spec.rb
# ...

RSpec.describe Api::V1::OrdersController, type: :controller do
  describe 'GET #index' do
    # ...
    it_behaves_like 'paginated list'
    # ...
  end
end
~~~

~~~ruby
# spec/controllers/api/v1/products_controller_spec.rb
# ...

RSpec.describe Api::V1::ProductsController, type: :controller do
  # ...
  describe 'GET #index' do
    # ...
    it_behaves_like 'paginated list'
    # ...
  end
  # ...
end
~~~

Et les deux tests devraient passer.

~~~bash
$ rspec spec/controllers/api/v1/
.................................................

Finished in 0.96778 seconds (files took 1.59 seconds to load)
49 examples, 0 failures
~~~

Maintenant que nous avons fait cette simple factorisation pour les tests, nous pouvons passer à l'implémentation de la pagination pour les contrôleurs et nettoyer les choses. Si vous vous souvenez de l'action d'indexation pour les deux contrôleurs de produits et de commandes, ils ont tous les deux le même format de pagination. Alors déplaçons cette logique dans une méthode appelée `pagination` sous le fichier `application_controller.rb`, de cette façon nous pouvons y accéder sur tout contrôleur qui aurait besoin de pagination.

~~~ruby
# app/controllers/application_controller.rb
class ApplicationController < ActionController::API
  include Authenticable

  # @return [Hash]
  def pagination(paginated_array)
    {
      pagination: {
        per_page: params[:per_page],
        total_pages: paginated_array.total_pages,
        total_objects: paginated_array.total_count
      }
    }
  end
end
~~~

Il suffit ensuite d'utiliser cette méthode dans nos deux contrôlleurs:

~~~ruby
# app/controllers/api/v1/orders_controller.rb
class Api::V1::OrdersController < ApplicationController
  # ...
  def index
    orders = current_user.orders.page(params[:page]).per(params[:per_page])
    render(
      json: orders,
      meta: pagination(orders)
    )
  end
  # ...
end
~~~

~~~ruby
# app/controllers/api/v1/products_controller.rb
class Api::V1::ProductsController < ApplicationController
  # ...
  def index
    products = Product.search(params).page(params[:page]).per(params[:per_page])
    render(
      json: products,
      include: [:user],
      meta: pagination(products)
    )
  end
  # ...
end
~~~

Lançons les tests pour nous assurer que tout fonctionne:

~~~bash
$ rspec spec/controllers/api/v1/
.................................................

Finished in 0.92996 seconds (files took 0.95615 seconds to load)
49 examples, 0 failures
~~~

Ce serait un bon moment pour *commiter* les changements et passer à la prochaine section sur la mise en cache.

~~~bash
$ git add .
~~~

## Mise en cache

Il y a actuellement une implémentation pour faire de la mise en cache avec la gemme `active_model_serializers` qui est vraiment facile à manipuler. Bien que dans les anciennes versions de la gemme, cette implémentation peut changer, elle fait le travail.

Si nous effectuons une demande à la liste des produits, nous remarquerons que le temps de réponse prend environ 174 milisecondes en utilisant cURL

~~~bash
$ curl -w 'Total: %{time_total}\n' -o /dev/null -s http://api.marketplace.dev/products
Total: 0,174111
~~~

> L'option `-w` nous permet de récupérer le temps de la requête, `-o` redirige la réponse vers un fichier et `-s` masque l'affichage de cURL


En ajoutant seulement une ligne à la classe `ProductSerializer`, nous verrons une nette amélioration du temps de réponse!

~~~ruby
# app/serializers/product_serializer.rb
class ProductSerializer < ActiveModel::Serializer
  # ...
  cache key: 'product', expires_in: 3.hours
end
~~~

~~~ruby
# app/serializers/order_serializer.rb
class OrderSerializer < ActiveModel::Serializer
  # ...
  cache key: 'order', expires_in: 3.hours
end
~~~

~~~ruby
# app/serializers/user_serializer.rb
class UserSerializer < ActiveModel::Serializer
  # ...
  cache key: 'user', expires_in: 3.hours
end
~~~


Et c'est tout! Vérifions l'amélioration:

~~~bash
$ curl -w 'Total: %{time_total}\n' -o /dev/null -s http://api.marketplace.dev/products
Total: 0,021599
$ curl -w 'Total: %{time_total}\n' -o /dev/null -s http://api.marketplace.dev/products
Total: 0,021979
~~~

Nous sommes donc passé de 174 ms à 21 ms. L'amélioration est donc énorme! *Comittons* une dernière fois nos changements.

## Conclusion

Si vous arrivez à ce point, cela signifie que vous en avez fini avec le livre. Bon travail! Vous venez de devenir un grand développeur API Rails, c'est sûr.

Merci d'avoir emmené cette grande aventure avec moi, j'espère que vous avez apprécié le voyage autant que moi. On devrait prendre une bière un de ces jours.

~~~ruby
$ git commit -am "Adds caching for the serializers"
~~~



[jsonapi]: https://jsonapi.org/
[jsonapi_includes]: https://jsonapi.org/format/#fetching-includes
[jsonapi_documentation]: https://jsonapi.org/format/#document-structure
[jsonapi_adapter]: https://github.com/rails-api/active_model_serializers/blob/v0.10.6/docs/general/adapters.md
[jsonapi_meta]: https://jsonapi.org/format/#document-meta
