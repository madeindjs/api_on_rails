# Improving orders

Back in previous chapter we extended our API to place orders and send a confirmation email to the user (just to improve the user experience). This chapter will take care of some validations on the order model, just to make sure it is placeable, just like:

1. Decrement the current product quantity when an order is placed
2. What happens when the products are not available?

We’ll probably need to update a little bit the `json output` for the orders, but let’s not spoil things up.

So now that we have everything clear, we can get our hands dirty. You can clone the project up to this point with:

~~~ruby
$ git clone https://github.com/madeindjs/market_place_api.git -b chapter8
~~~

Let’s create a branch to start working:

~~~ruby
$ git checkout -b chapter9
~~~

## Decrementing the product quantity

On this first stop we will work on update the product quantity to make sure every order will deliver the actual product. Currently the `product` model doesn’t have a `quantity` attribute, so let’s do that:

~~~bash
$ rails generate migration add_quantity_to_products quantity:integer
~~~

Wait, don’t run the migrations just yet, we are making a small modification to it. As a good practice I like to add default values for the database just to make sure I don’t mess things up with `null` values. This is a perfect case!

Your migration file should look like this:

~~~ruby
# db/migrate/20181227092237_add_quantity_to_products.rb
class AddQuantityToProducts < ActiveRecord::Migration[5.2]
  def change
    add_column :products, :quantity, :integer, default: 0
  end
end
~~~

Now we can migrate database:

~~~bash
$ rake db:migrate
~~~

Now it is time to decrement the quantity for the `product` once an `order` is placed. Probably the first thing that comes to your mind is to take this to the `Order` model and this is a common mistake when working with _Many-to-Many_ associations, we totally forget about the joining model which in this case is `Placement`.

The `Placement` is a better place to handle this as we have access to the order and the product, so we can easily in this case decrement the product stock.

Before we start implementing the code for the decrement, we have to change the way we handle the `order` creation as we now have to accept a quantity for each product. Remember we expecting we are expecting an array of product ids. I’m going to try to keep things simple and I will send an array of arrays where the first position of each inner array will be the product id and the second the quantity.

A quick example on this would be something like:

~~~ruby
product_ids_and_quantities = [
  [1,4],
  [3,5]
]
~~~

This is going to be tricky so stay with me, let’s first build some unit tests:

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

Then into the implementation:

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

And if we run our tests, they should be all nice and green:

~~~bash
$ rspec spec/models/order_spec.rb
........

Finished in 0.33759 seconds (files took 3.54 seconds to load)
8 examples, 0 failures
~~~

The `build_placements_with_product_ids_and_quantities` will build the placement objects and once we trigger the `save` method for the order everything will be inserted into the database. One last step before commiting this is to update the `orders_controller_spec` along with its implementation.

First we update the `orders_controller_spec` file:

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

Then we need to update the `orders_controller`:

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

_**Notice we removed the `order_params` method as we are handling the creation for the placements.**_

And last but not least, we need to update the `products` factory file, to assign a high `quantity` value, to at least have some products to play around in stock.

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

Let’s commit this changes and keep moving:

~~~bash
$ git add .
$ git commit -m "Allows the order to be placed along with product quantity"
~~~

Did you notice we are not saving the quantity for each product anywhere?, there is no way to keep track of that. This can be fix really easy, by just adding a quantity attribute to the `Placement` model, so this way for each product we save its corresponding quantity. Let’s start by creating the migration:

~~~bash
$ rails generate migration add_quantity_to_placements quantity:integer
~~~

As with the product quantity attribute migration we should add a default value equal to 0, remember this is optional but I do like this approach. The migration file should look like:

~~~ruby
# db/migrate/20181227104830_add_quantity_to_placements.rb
class AddQuantityToPlacements < ActiveRecord::Migration[5.2]
  def change
    add_column :placements, :quantity, :integer, default: 0
  end
end
~~~

Then run the migrations:

~~~bash
$ rake db:migrate
~~~

Let’s document the `quantity` attribute through a unit test like so:

~~~ruby
# spec/models/placement_spec.rb
# ...
RSpec.describe Placement, type: :model do
  # ...
  it { should respond_to :quantity }
  # ...
end
~~~

Now we just need to update the `build_placements_with_product_ids_and_quantities` to add the `quantity` for the placements:

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

Our `order_spec.rb` should be still green:

~~~bash
$ rspec spec/models/order_spec.rb
........

Finished in 0.09898 seconds (files took 0.74936 seconds to load)
8 examples, 0 failures
~~~

Let’s commit the changes:

~~~bash
$ git add .
$ git commit -m "Adds quantity to placements"
~~~

### Extending the Placement model

It is time to update the product quantity once the order is saved, or more accurate once the placement is created. In order to achieve this we are going to add a method and then hook it up to an `after_create` callback.

Let’s first update our `placement` factory to make more sense:

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

And then we can simply add some specs:

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

The implementation is fairly easy as shown bellow:

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

## Validation du stock des produits

As you remember from the beginning of the chapter we added the `quantity` attribute to the `Product` model, now it is time to validate that there are enough products for the order to be placed.

In order to make things more interesting and spice things up we will do it thorugh a custom validator, just to keep things cleaner and show you another cool technique to achieve custom validations.

For **custom validators** you can head to the [documentation](http://guides.rubyonrails.org/active_record_validations.html#performing-custom-validations). Let’s get our hands dirty.

First we need to add a `validators` directory under the `app` directory (Rails will pick it up for so we do not need to load it).

~~~bash
$ mkdir app/validators
$ touch app/validators/enough_products_validator.rb
~~~

Before we drop any line of code, we need to make sure to add a spec to the `Order` model to check if the order can be placed.

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

As you can see on the spec, we first make sure that `placement_2` is trying to request more products than are available, so in this case the `order` is not supposed to be valid.

The test by now should be failing, let’s turn it into green by adding the code for the validator:

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

I manage to add a message for each of the products that are out of stock, but you can handle it differently if you want. Now we just need to add the validator to the `Order` model like so:

~~~ruby
# app/models/order.rb
class Order < ApplicationRecord
  # ...
  validates_with EnoughProductsValidator
  # ...
end
~~~

And now if you run your tests, everything should be nice and green:

~~~bash
$ rspec spec/models/order_spec.rb
.........

Finished in 0.19136 seconds (files took 0.74912 seconds to load)
9 examples, 0 failures
~~~

Let’s commit the changes:

~~~bash
$ git add .
$ git commit -m "Adds validator for order with not enough products on stock"
~~~

## Updating the total

Did you realize that the `total` is being calculated incorrectly, because currently it is just adding the price for the products on the order regardless of the quantity requested. Let me add the code to clarify the problem:

Currently in the `order` model we have this method to calculate the amount to pay:

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

Now instead of calculating the `total` by just adding the product prices, we need to multiply it by the quantity, so let’s update the spec first:

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

And the implementation is fairly easy:

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

And the specs should be green:

~~~bash
$ rspec spec/models/order_spec.rb
.........

Finished in 0.20537 seconds (files took 0.74555 seconds to load)
9 examples, 0 failures
~~~

Let’s commit the changes and wrap up.

~~~bash
$ git commit -am "Updates the total calculation for order"
~~~

## Conclusion

Oh you are here!, let me congratulate you, it’s been a long way since chapter 1, but you are 1 step closer. Actually the next chapter would be the last one, so try to take the most out of it.

The last chapter would be on how to optimize the API by using `pagination`, `caching` and `background jobs`, so buckle up, it is going to be a bumpy ride.
