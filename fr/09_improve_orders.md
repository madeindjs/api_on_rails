# Améliorer les commandes {#chapter:9}

Dans le chapitre [11](#chapter:8){reference-type="ref"reference="chapter:8"}, nous avons amélioré notre API pour passer des commandes et envoyer un e-mail de confirmation à l'utilisateur (juste pour améliorer l'expérience utilisateur). Ce chapitre va s'occuper de quelques validations sur le modèle de commande afin de s'assurer qu'elle est valide. C'est-à-dire:

- Diminuer la quantité du produit en cours lors de la passation d'une commande
- Que se passe-t-il lorsque les produits ne sont pas disponibles?

Nous aurons aussi besoin de mettre à jour un peu la sortie JSON pour les
commandes Mais ne *spoilons* pas la suite.

Maintenant que tout est clair, on peut mettre les mains dans le
cambouis. Vous pouvez cloner le projet jusqu'à ce point avec:

~~~bash
$ git clone https://github.com/madeindjs/market_place_api.git -b chapter8

~~~

Créons une branche pour commencer

~~~bash
$ git checkout -b chapter9

~~~

## Diminution de la quantité de produit

Dans cette partie nous travaillerons sur la mise à jour de la quantité du produit pour nous assurer que chaque commande livrera le produit réel. Actuellement, le modèle de produit n'a pas d'attribut de quantité, alors faisons-le:

~~~bash
$ rails generate migration add_quantity_to_products quantity:integer
~~~

Attendez! N'exécutez pas encore cette migrations! Nous allons y apporter une petite modification. Comme bonne pratique, j'aime ajouter des valeurs par défaut pour la base de données juste pour être sûr de ne pas tout gâcher avec des valeurs nulles. C'est un cas parfait!

Votre fichier de migration devrait ressembler à ceci:

~~~ruby
# db/migrate/20181227092237_add_quantity_to_products.rb
class AddQuantityToProducts < ActiveRecord::Migration[5.2]
  def change
    add_column :products, :quantity, :integer, default: 0
  end
end
~~~

Maintenant nous pouvons lancer la migration:

~~~bash
$ rake db:migrate

~~~

Il est maintenant temps de diminuer la quantité du `Product` une fois l'`Order` passée. La première chose qui vous vient probablement à l'esprit est de le faire dans le modèle `Order` et c'est une erreur fréquente. Lorsque vous travaillez avec des associations *Many-to-Many*, nous oublions totalement le modèle de jointure qui dans ce cas est `Placement`. Le `Placement` est un meilleur endroit pour gérer cela car nous avons accès à la commande et au produit. Ainsi, nous pouvons facilement diminuer le stock du produit.

Avant de commencer à implémenter le code, nous devons changer la façon dont nous gérons la création de la commande car nous devons maintenant accepter une quantité pour chaque produit. Si vous vous souvenez de Listing [\[lst:orders\_controller\_create\]](#lst:orders_controller_create){reference-type="ref"reference="lst:orders_controller_create"}, nous attendons un tableau d'identifiants de produits. Je vais essayer de garder les choses simples et je vais envoyer un tableau de tableaux où la première position de chaque tableau interne sera l'identifiant du produit et la seconde la quantité.

Un exemple rapide serait quelque chose comme cela:

~~~ruby
product_ids_and_quantities = [
  [1,4],
  [3,5]
]
~~~

Ça va être difficile, alors restez avec moi. Construisons d'abord des tests unitaires:

~~~ruby
# spec/models/order_spec.rb
require 'rails_helper'

RSpec.describe Order, type: :model do
  # ...

  describe '#build_placements_with_product_ids_and_quantities' do
    before(:each) do
      product_1 = FactoryBot.create :product, price: 100, quantity: 5
      product_2 = FactoryBot.create :product, price: 85, quantity: 10

      @product_ids_and_quantities = [[product_1.id, 2], [product_2.id, 3]]
    end

    it 'builds 2 placements for the order' do
      expect { order.build_placements_with_product_ids_and_quantities(@product_ids_and_quantities) }.to change { order.placements.size }.from(0).to(2)
    end
  end
end
~~~

Et maintenant l'implémentation

~~~ruby
# app/models/order.rb
class Order < ApplicationRecord
  # ...

  # @param product_ids_and_quantities [Array] something like this
  #        `[[product_1.id, 2], [product_2.id, 3]]`. Where first item is
  #        `product_id` and the second is the quantity
  # @yield [Placement] placement build
  def build_placements_with_product_ids_and_quantities(product_ids_and_quantities)
    product_ids_and_quantities.each do |product_id_and_quantity|
      id, quantity = product_id_and_quantity # [1,5]
      placement = placements.build(product_id: id)
      yield placement if block_given?
    end
  end
end
~~~

Et maintenant, si nous lançons les tests, ils devraient passer:

~~~ruby
$ rspec spec/models/order_spec.rb
      ........

      Finished in 0.33759 seconds (files took 3.54 seconds to load)
      8 examples, 0 failures
~~~

Les `build_placements_with_product_ids_and_quantities` construiront les objets `Placement` et une fois que nous déclencherons la méthode de sauvegarde de l'ordre, tout sera inséré dans la base de données. Une dernière étape avant de valider ceci est de mettre à jour `orders_controller_spec` avec son implémentation.

Tout d'abord, nous mettons à jour le fichier `orders_controller_spec`:

~~~ruby
# spec/controllers/api/v1/orders_controller_spec.rb
require 'rails_helper'

RSpec.describe Api::V1::OrdersController, type: :controller do
  # ...

  describe 'POST #create' do
    before(:each) do
      current_user = FactoryBot.create :user
      api_authorization_header current_user.auth_token

      product_1 = FactoryBot.create :product
      product_2 = FactoryBot.create :product
      order_params = {
        product_ids_and_quantities: [[product_1.id, 2], [product_2.id, 3]]
      }
      post :create, params: { user_id: current_user.id, order: order_params }
    end

    it 'embeds the two product objects related to the order' do
      expect(json_response[:products].size).to eql 2
    end

    # ...
  end
end
~~~

Nous devons ensuite mettre un peu à jour notre contrôleur des commandes:

~~~ruby
# app/controllers/api/v1/orders_controller.rb
class Api::V1::OrdersController < ApplicationController
  # ...

  def create
    order = Order.create! user: current_user
    order.build_placements_with_product_ids_and_quantities(params[:order][:product_ids_and_quantities])

    if order.save
      order.reload # need to reload associations
      OrderMailer.send_confirmation(order).deliver
      render json: order, status: 201, location: [:api, current_user, order]
    else
      render json: { errors: order.errors }, status: 422
    end
  end
end
~~~

Notez que j'ai aussi supprimé la méthode `OrdersController#order_params` qui devient inutile.

Enfin et surtout, nous devons mettre à jour le fichier d'usine des produits afin d'attribuer une valeur de quantité élevée pour avoir au moins quelques produits en stock.

~~~ruby
# spec/factories/products.rb
FactoryBot.define do
  factory :product do
    title { FFaker::Product.product_name }
    price { rand * 100 }
    published { false }
    user
    quantity { 5 }
  end
end
~~~

*Commitons* nos changements avant d'aller plus loin:

~~~bash
$ git add .
$ git commit -m "Allows the order to be placed along with product quantity"

~~~

Avez-vous remarqué que nous ne mettons pas à jour la quantité des produits? Actuellement, il n'y a aucun moyen d'en faire le suivi. Cela peut être corrigé très facilement, en ajoutant simplement un attribut de quantité au modèle `Placement` de sorte que pour chaque produit, nous sauvegardons la quantité correspondante. Commençons par créer la migration:

~~~bash
$ rails generate migration add_quantity_to_placements quantity:integer
~~~

Comme pour la migration des attributs de quantité de produit, nous devrions ajouter une valeur par défaut égale à 0. N'oubliez pas que c'est facultatif mais c'est mieux. Le fichier de migration devrait ressembler à cela:

~~~ruby
# db/migrate/20181227104830_add_quantity_to_placements.rb
class AddQuantityToPlacements < ActiveRecord::Migration[5.2]
  def change
    add_column :placements, :quantity, :integer, default: 0
  end
end
~~~

Lancez ensuite la migration:

~~~bash
$ rake db:migrate
~~~

Documentons l'attribut `quantity` par un test unitaire:

~~~ruby
# spec/models/placement_spec.rb
# ...
RSpec.describe Placement, type: :model do
  # ...
  it { should respond_to :quantity }
  # ...
end
~~~

Il ne nous reste plus qu'à mettre à jour la méthode `build_placements_with_product_ids_and_quantities` pour ajouter la quantité pour les placements:

~~~ruby
# app/models/order.rb
class Order < ApplicationRecord
  # ...

  def build_placements_with_product_ids_and_quantities(product_ids_and_quantities)
    product_ids_and_quantities.each do |product_id_and_quantity|
      product_id, quantity = product_id_and_quantity # [1,5]
      placements.build(product_id: product_id, quantity: quantity)
    end
  end
end
~~~

Maintenant, nos tests devraient passer:

~~~bash
$ rspec spec/models/order_spec.rb
........

Finished in 0.09898 seconds (files took 0.74936 seconds to load)
8 examples, 0 failures
~~~

*Commitons* nos changement:

~~~bash
$ git add .
$ git commit -m "Adds quantity to placements"

~~~

### Étendre le modèle de placement

Il est temps de mettre à jour la quantité du produit une fois la commande enregistrée ou plus précisément: une fois le placement créé. Pour ce faire, nous allons ajouter une méthode et la connecter au *callback* `after_create`.

Commençons par mettre à jour notre usine de placement pour qu'elle soit plus logique:

~~~ruby
# spec/factories/placements.rb
FactoryBot.define do
  factory :placement do
    order
    product
    quantity { 1 }
  end
end
~~~

Et puis nous pouvons simplement ajouter quelques tests:

~~~ruby
# spec/models/placement_spec.rb
# ...

RSpec.describe Placement, type: :model do
  # ...
  it { should respond_to :quantity }
  # ...
  describe '#decrement_product_quantity!' do
    it 'decreases the product quantity by the placement quantity' do
      product = placement.product
      expect { placement.decrement_product_quantity! }.to change { product.quantity }.by(-placement.quantity)
    end
  end
end
~~~

La mise en œuvre est assez simple comme le montre le listing [\[lst:placement\_decrement\_product\_quantity\]](#lst:placement_decrement_product_quantity){reference-type="ref"reference="lst:placement_decrement_product_quantity"}.

~~~ruby
# app/models/placement.rb
class Placement < ApplicationRecord
  # ...
  after_create :decrement_product_quantity!

  def decrement_product_quantity!
    product.decrement!(:quantity, quantity)
  end
end
~~~

Validation du stock des produits
--------------------------------

Depuis le début du chapitre, nous avons ajouté l'attribut `quantity` au modèle de produit. il est maintenant temps de valider que la quantité de produit est suffisante pour que la commande soit passée. Afin de rendre les choses plus intéressantes, nous allons le faire à l'aide d'un validateur personnalisé[^19]. Pour les validateurs personnalisés, vous pouvez consulter la
[documentation](https://guides.rubyonrails.org/active_record_validations.html#performing-custom-validations).

Tout d'abord, nous devons créer un répertoire de `validators` dans le répertoire `app` (Rails le charge par défaut) et ensuite créons un fichier dedans:

~~~bash
$ mkdir app/validators
$ touch app/validators/enough_products_validator.rb
~~~

Avant de commencer à implémenter la classe, nous devons nous assurer d'ajouter un test au modèle de commande pour vérifier si la commande peut être passée (Listing [\[lst:order\_valid\_spec\]](#lst:order_valid_spec){reference-type="ref"reference="lst:order_valid_spec"}).

~~~ruby
# spec/models/order_spec.rb
require 'rails_helper'

RSpec.describe Order, type: :model do
  # ...

  describe "#valid?" do
    before do
      product_1 = FactoryBot.create :product, price: 100, quantity: 5
      product_2 = FactoryBot.create :product, price: 85, quantity: 10


      placement_1 = FactoryBot.build :placement, product: product_1, quantity: 3
      placement_2 = FactoryBot.build :placement, product: product_2, quantity: 15

      @order = FactoryBot.build :order

      @order.placements << placement_1
      @order.placements << placement_2
    end

    it "becomes invalid due to insufficient products" do
      expect(@order).to_not be_valid
    end
  end
end
~~~

Comme vous pouvez le voir sur les tests (Listing [\[lst:order\_valid\_spec\]](#lst:order_valid_spec){reference-type="ref"reference="lst:order_valid_spec"}), nous nous assurons d'abord que `placement_2` essaie de demander plus de produits que ce qui est disponible. Donc dans ce cas la commande n'est pas supposée être valide.

Le test est en train d'échouer. Faisons le passer en implémentant le code pour le validateur:

~~~ruby
# app/validators/enough_products_validator.rb
class EnoughProductsValidator < ActiveModel::Validator
  def validate(record)
    record.placements.each do |placement|
      product = placement.product
      if placement.quantity > product.quantity
        record.errors[product.title.to_s] << "Is out of stock, just #{product.quantity} left"
      end
    end
  end
end

~~~

J'ajoute simplement un message pour chacun des produits en rupture de stock, mais vous pouvez le gérer différemment si vous le souhaitez. Il ne nous reste plus qu'à ajouter ce validateur au modèle `Order` comme cela:

~~~ruby
# app/models/order.rb
class Order < ApplicationRecord
  # ...
  validates_with EnoughProductsValidator
  # ...
end
~~~

Et maintenant, si vous lancer vos tests, tout devrait être beau et vert:

~~~bash
$ rspec spec/models/order_spec.rb
.........

Finished in 0.19136 seconds (files took 0.74912 seconds to load)
9 examples, 0 failures
~~~

*Commitons* nos changements:

~~~bash
$ git add .
$ git commit -m "Adds validator for order with not enough products on stock"
~~~

Mettre à jour le prix total
---------------------------

Réalisez vous que le prix total est mal calculé? Actuellement, nous ajoutons le prix des produits sur la commande, quelle que soit la quantité demandée. Permettez-moi d'ajouter le code pour clarifier le problème:

Actuellement, dans le modèle de commande, nous avons cette méthode pour calculer le montant à payer:

~~~ruby
# app/models/order.rb
class Order < ApplicationRecord
  # ...

  def set_total!
    self.total = products.map(&:price).sum
  end

  # ...
end

~~~

Maintenant, au lieu de calculer le total en additionnant simplement les prix des produits, nous devons le multiplier par la quantité. Alors mettons d'abord à jour les tests:

~~~ruby
# spec/models/order_spec.rb
require 'rails_helper'

RSpec.describe Order, type: :model do
  # ...

  describe '#set_total!' do
    before(:each) do
      product_1 = FactoryBot.create :product, price: 100
      product_2 = FactoryBot.create :product, price: 85

      placement_1 = FactoryBot.build :placement, product: product_1, quantity: 3
      placement_2 = FactoryBot.build :placement, product: product_2, quantity: 15

      @order = FactoryBot.build :order

      @order.placements << placement_1
      @order.placements << placement_2
    end

    it 'returns the total amount to pay for the products' do
      expect { @order.set_total! }.to change { @order.total.to_f }.from(0).to(1575)
    end
  end

  # ...
end

~~~

L'implémentation est assez simple:

~~~ruby
# app/models/order.rb
class Order < ApplicationRecord
  # ...

  def set_total!
    self.total = 0.0
    placements.each do |placement|
      self.total += placement.product.price.to_f * placement.quantity
    end
  end

  # ...
end

~~~

Et maintenant, les tests devraient passer:

~~~bash
$ rspec spec/models/order_spec.rb
.........

Finished in 0.20537 seconds (files took 0.74555 seconds to load)
9 examples, 0 failures

~~~

*Commitons* nos changements et récapitulons tout ce que nous venons de
faire:

~~~bash
$ git commit -am "Updates the total calculation for order"

~~~

Conclusion
----------

Oh vous êtes ici! Permettez-moi de vous féliciter! Cela fait un long chemin depuis le chapitre [4](#chapter:1){reference-type="ref"reference="chapter:1"}, mais vous êtes à un pas de plus. En fait, le chapitre suivant serait le dernier. Alors essayez d'en tirer le meilleur.

Le dernier chapitre portera sur la façon d'optimiser l'API en utilisant la pagination, la mise en cache et les tâches d'arrière-plan. Donc bouclez vos ceintures, ça va être un parcours mouvementé.
