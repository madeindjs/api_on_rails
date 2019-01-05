# Optimisations {#chapter:10}

Bienvenue dans le dernier chapitre du livre. Le chemin a été long mais vous n'êtes qu'à un pas de la fin. Dans le chapitre [12](#chapter:9){reference-type="ref" reference="chapter:9"}, nous avons terminé la modélisation du modèle de commandes. Nous pourrions dire que le projet est maintenant terminé mais je veux couvrir quelques détails importants sur l'optimisation. Les sujets que je vais aborder ici seront:

- La mise en place de la spécification [JSON:API](https://jsonapi.org/)
- la pagination
- les tâhces en arrière plan
- la mise en cache

J'essaierai d'aller aussi loin que possible en essayant de couvrir certains scénarios courants. J'espère que ces scénarions vous serons utiles pour certains de vos projets.

Si vous commencez à lire à ce stade, vous voudrez probablement que le code fonctionne, vous pouvez le cloner comme ça:

~~~bash
$ git clone https://github.com/madeindjs/market_place_api.git -b chapter9
~~~

Créons une nouvelle branche pour ce chapitre:

~~~bash
$ git checkout -b chapter10
~~~

Mise en place de la spécification [JSON:API](https://jsonapi.org/)
------------------------------------------------------------------

Comme je vous le dis depuis le début de ce livre, une partie importante et difficile lors de la création de votre API est de décider le format de sortie. Heuresement, certaines organisations ont déjà fait face à ce genre de problème et elles ont ainsi établies certaines conventions.

Une des convention les plus appliquée est très certainement [JSON:API](https://jsonapi.org/). JSON:API nous impose certaines règles
à suivre comme:

- Comment formater la présentation de

Cette convention nous permettra d'aborder la pagination (section [13.2](#sec:pagination){reference-type="ref"reference="sec:pagination"}) plus sereinement.

Pagination
----------

Une stratégie très commune pour optimiser la récupération d'enregistrements dans une base de données est de charger seulement une qunatité limité en les paginant. Si vous êtes familier avec cette technique, vous savez qu'avec Rails c'est vraiment très facile à mettre en place avec des gemmes telles que [will\_paginate](https://github.com/mislav/will_paginate) ou [kaminari](https://github.com/kaminari/kaminari).

La seule partie délicate ici est de savoir comment gérer la sortie JSON pour donner assez d'informations au client sur la façon dont le tableau est paginé. Si vous vous souvenez du chapitre [4](#chapter:1){reference-type="ref" reference="chapter:1"}, j'ai partagé quelques ressources sur les pratiques que j'allais suivre ici. L'une d'entre elles était <http://jsonapi.org/> qui est une page incontournable des signets.

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
      expect(json_response[:meta][:pagination]).to have_key(:per_page)
      expect(json_response[:meta][:pagination]).to have_key(:total_pages)
      expect(json_response[:meta][:pagination]).to have_key(:total_objects)
    end

    it { expect(response.response_code).to eq(200) }
  end

  # ...
end

~~~

Le test que nous venons d'ajouter devrait échouer or, si nous executons les tests, deux tests échouent. Cela veux dire que nous avons cassé quelque chose d'autre:

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
