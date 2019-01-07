# Presenting the users

In the last chapter we manage to set up the bare bones for our application endpoints configuration, we even added versioning through headers. In a next chapter we will handle users authentication through authentication tokens as well as setting permissions to limit access for let’s say signed in users. In coming chapters we will relate `products` to users and give them the ability to place orders.

You can clone the project until this point with:

~~~bash
$ git clone https://github.com/madeindjs/market_place_api/tree/chapitre_2
~~~

As you can already imagine there are a lot of authentication solutions for Rails, [AuthLogic](https://github.com/binarylogic/authlogic), [Clearance](https://github.com/thoughtbot/clearance) and [Devise](https://github.com/plataformatec/devise). We will be using the last one, which offers a great way to integrate not just basic authentication, but many other modules for further use.

Devise comes with up to 10 modules for handling authentication::

- Database Authenticable
- Omniauthable
- Confirmable
- Recoverable
- Registerable
- Rememberable
- Trackable
- Timeoutable
- Validatable
- Lockable

If you have not work with devise before, I recommend you visit the [reposity page](https://github.com/plataformatec/devise) and read the documentation, there are a lot of good examples out there too.

This is going to be a full-packed chapter, it may be long, but I’m trying to cover as many topics along with best practices on the way, so feel free to grab a cup of coffee and let’s get going. By the end of the chapter you will have full user endpoints, along with validations and error server responses.

We want to track this chapter, so it would be a good time to create a new branch for this:

~~~bash
$ git checkout -b chapter3
~~~

Just make sure you are on the `master` branch before checking out.

## User model

We need to first add the `devise` gem into the `Gemfile`

~~~ruby
# Gemfile
source 'https://rubygems.org'
git_source(:github) { |repo| "https://github.com/#{repo}.git" }

ruby '2.5.3'

# Bundle edge Rails instead: gem 'rails', github: 'rails/rails'
gem 'rails', '~> 5.2.0'
# Use sqlite3 as the database for Active Record
gem 'sqlite3'
# Use Puma as the app server
gem 'puma', '~> 3.11'
# Use SCSS for stylesheets
gem 'sass-rails', '~> 5.0'
# Use Uglifier as compressor for JavaScript assets
gem 'uglifier', '>= 1.3.0'

# Api gems
gem 'active_model_serializers'

# Reduces boot times through caching; required in config/boot.rb
gem 'bootsnap', '>= 1.1.0', require: false

group :development, :test do
  # Call 'byebug' anywhere in the code to stop execution and get a debugger console
  gem 'byebug', platforms: %i[mri mingw x64_mingw]
end

group :development do
  # Access an interactive console on exception pages or by calling 'console' anywhere in the code.
  gem 'listen', '>= 3.0.5', '< 3.2'
  gem 'web-console', '>= 3.3.0'
  # Spring speeds up development by keeping your application running in the background. Read more: https://github.com/rails/spring
  gem 'spring'
  gem 'spring-watcher-listen', '~> 2.0.0'
end

group :test do
  gem 'factory_bot_rails', '~> 4.9'
  gem 'ffaker', '~> 2.10'
  gem 'rspec-rails', '~> 3.8'
end

# Windows does not include zoneinfo files, so bundle the tzinfo-data gem
gem 'tzinfo-data', platforms: %i[mingw mswin x64_mingw jruby]

gem 'devise'
~~~

Then run the `bundle install` command to install it. Once the bundle command finishes, we need to run the devise install generator:

~~~bash
$ rails g devise:install
  create  config/initializers/devise.rb
  create  config/locales/devise.en.yml
  ...
~~~

By now and if everything went well we will be able to generate the `user` model through the `devise` generator:

~~~bash
$ rails g devise User
    invoke  active_record
    create    db/migrate/20181113070805_devise_create_users.rb
    create    app/models/user.rb
    invoke    rspec
    create      spec/models/user_spec.rb
    invoke      factory_bot
    create        spec/factories/users.rb
    insert    app/models/user.rb
    route  devise_for :users
~~~

From now every time we create a model, the generator will also create a factory file for that model. This will help us to easily create test users and facilitate our tests writing.

~~~ruby
# spec/factories/users.rb
FactoryBot.define do
  factory :user do

  end
end
~~~

Next we migrate the database and prepare the test database.

~~~bash
$ rake db:migrate
== 20181113070805 DeviseCreateUsers: migrating ================================
-- create_table(:users)
   -> 0.0008s
-- add_index(:users, :email, {:unique=>true})
   -> 0.0005s
-- add_index(:users, :reset_password_token, {:unique=>true})
   -> 0.0007s
== 20181113070805 DeviseCreateUsers: migrated (0.0023s) =======================
~~~

~~~bash
$ rake db:test:prepare
~~~

Let’s commit this, just to keep our history points very atomic.

~~~bash
$ git add .
$ git commit -m "Adds devise user model"
~~~

## First user tests

We will add some specs to make sure the `user` model responds to the `email`, `password` and `password_confirmation` attributes provided by devise, let’s add them. Also for convenience we will modify the `users` factory file to add the corresponding attributes.

~~~ruby
# spec/factories/users.rb
FactoryBot.define do
  factory :user do
    email { FFaker::Internet.email }
    password { '12345678' }
    password_confirmation { '12345678' }
  end
end
~~~

Once we’d added the attributes it is time to test our `User` model.

~~~ruby
# spec/models/user_spec.rb
# ...

RSpec.describe User, type: :model do
  before { @user = FactoryBot.build(:user) }

  subject { @user }

  it { should respond_to(:email) }
  it { should respond_to(:password) }
  it { should respond_to(:password_confirmation) }

  it { should be_valid }
end
~~~

Because we previously prepare the test database, with `rake db:test:prepare`, we just simply run the tests:

~~~bash
$ bundle exec rspec spec/models/user_spec.rb
....

Finished in 0.03231 seconds (files took 0.81624 seconds to load)
4 examples, 0 failures
~~~

That was easy, we should probably commit this changes:

~~~bash
$ git add .
$ git commit -am 'Adds user firsts specs'
~~~

## Improving validation tests

It is showtime people, we are building our first endpoint. We are just going to start building the `show` action for the user which is going to expose a `user` record in plain old `json`. We first need to generate the `users_controller`, add the corresponding tests and then build the actual code.

First we generate the `users` controller:

~~~bash
$ rails generate controller users
~~~

This command will create a `users_controller_spec.rb`. Before we get into that, there are 2 basic steps we should be expecting when testing `api` endpoints.

-   The JSON structure to be returned from the server
-   The status code we are expecting to receive from the server

### Most common http codes

The first digit of the status code specifies one of five classes of response; the bare minimum for an HTTP client is that it recognize these five classes. A common list of used http codes is presented below:

- `200`: Standard response for successful HTTP requests (It is commonly on GET requests)
- `201`: The request has been fulfilled and resulted in a new resource being created (After POST requests)
- `204`: The server successfully processed the request, but is not returning any content (It is usually a successful DELETE request)
- `400`: The request cannot be fulfilled due to bad syntax.
- `401`: Similar to 403 Forbidden, but specifically for use when authentication is required and has failed or has not yet been provided
- `404`: The requested resource could not be found but may be available again in the future (Usually GET requests)
- `500`: A generic error message, given when an unexpected condition was encountered and no more specific message is suitable.

> For a full list of HTTP method check out the article on [Wikipedia](http://en.wikipedia.org/wiki/List_of_HTTP_status_codes) talking about it

To keep our code nicely organized, we will create some directories under the controller specs directory in order to be consistent with our current setup. There is also another set up out there which uses instead of the `controllers` directory a `request` or `integration` directory, I this case I like to be consistent with the `app/controllers` directory.

~~~bash
$ mkdir -p spec/controllers/api/v1
$ mv spec/controllers/users_controller_spec.rb spec/controllers/api/v1
~~~

After creating the corresponding directories we need to change the file `describe` name from `UsersController` to `Api::V1::UsersController`, the updated file should look like:

~~~ruby
# spec/controllers/api/v1/users_controller_spec.rb
RSpec.describe Api::V1::UsersController, type: :controller do

end
~~~

Now with tests added your file should look like:

~~~ruby
# spec/controllers/api/v1/users_controller_spec.rb
# ...

RSpec.describe Api::V1::UsersController, type: :controller do
  before(:each) { request.headers['Accept'] = "application/vnd.marketplace.v1" }

    describe "GET #show" do
      before(:each) do
        @user = FactoryBot.create :user
        get :show, params: { id: @user.id, format: :json}
      end

      it "returns the information about a reporter on a hash" do
        user_response = JSON.parse(response.body, symbolize_names: true)
        expect(user_response[:email]).to eql @user.email
      end

      it { expect(response).to be_success }
    end
end
~~~

So far, the tests look good, we just need to add the implementation. It is extremely simple:

~~~ruby
# app/controllers/api/v1/users\_controller.rb
class  Api::V1::UsersController < ApplicationController
  def show
    render json: User.find(params[:id])
  end
end
~~~

You may activate `Devise::Test::ControllerHelpers` module in `spec/rails_helper.rb` file to load helpers. To do so you only have to add this line:

~~~ruby
#  ...
RSpec.configure do |config|
  #  ...
  config.include Devise::Test::ControllerHelpers, type: :controller
  #  ...
end
~~~

If you run the tests now with `rspec spec/controllers` you will see an error message similar to this:

~~~
$ bundle exec rspec spec/controllers
FF

Failures:

  1) Api::V1::UsersController GET #show returns the information about a reporter on a hash
    Failure/Error: get :show, params: { id: @user.id, format: :json}

    ActionController::UrlGenerationError:
    No route matches {:action=>"show", :controller=>"api/v1/users", :format=>:json, :id=>1}
      ...

  2) Api::V1::UsersController GET #show
    Failure/Error: get :show, params: { id: @user.id, format: :json}


    ActionController::UrlGenerationError:
    No route matches {:action=>"show", :controller=>"api/v1/users", :format=>:json, :id=>1}
      ...

Finished in 0.01632 seconds (files took 0.47675 seconds to load)
  2 examples, 2 failures
~~~

This kind of error if very common when generating endpoints manually, we totally forgot the `routes`. So let’s add them:

~~~ruby
# config/routes.rb
require 'api_constraints'

Rails.application.routes.draw do
  devise_for :usersto
  # Api definition
  namespace :api, defaults: { format: :json }, constraints: { subdomain: 'api' }, path: '/' do
    scope module: :v1, constraints: ApiConstraints.new(version: 1, default: true) do
      resources :users, only: [:show]
    end
  end
end
~~~

Tests should now pass:

~~~bash
$ bundle exec rspec spec/controllers
..

Finished in 0.02652 seconds (files took 0.47291 seconds to load)
2 examples, 0 failures
~~~

As usual and after adding some bunch of code we are satisfied with, we commit the changes:

~~~bash
$ git add .
$ git commit -m "Adds show action the users controller"
~~~

### Testing endpoints with CURL

So we finally have an endpoint to test, there are plenty of options to start playing with. The first that come to my mind is using [cURL](http://curl.haxx.se/), which comes built-in on almost any Linux distribution and of course on your Mac OSX. So let’s try it out:

> Remember our base uri is `api.market_place_api.dev`.

~~~bash
$ curl -H 'Accept: application/vnd.marketplace.v1' http://api.market_place_api.dev/users/1
~~~

This will throw us an error, well you might expect that already, we don’t have a user with id 1, let’s create it first through the terminal:

~~~bash
$ rails console
Loading development environment (Rails 5.2.1)
2.5.3 :001 >  User.create email: "example@marketplace.com", password: "12345678", password_confirmation: "12345678"
~~~

After creating the user successfully our endpoint should work:

~~~bash
$ curl -H 'Accept: application/vnd.marketplace.v1' \
http://api.market_place_api.dev/users/1
{"id":1,"email":"example@marketplace.com", ...
~~~

So there you go, you now have a user record api endpoint. If you are having problems with the response and double checked everything is well assembled, well then you might need to visit the `application_controller.rb` file and update it a little bit like so

~~~ruby
# app/controllers/application_controller.rb

class ApplicationController < ActionController::API
  # Prevent CSRF attacks by raising an exception.
  # For APIs, you may want to use :null_session instead.
  protect_from_forgery with: :null_session
end
~~~

As suggested even by Rails we should be using `null_session` to prevent CSFR attacks from being raised, so **I highly recommend you do it as this will not allow POST or PUT requests to work**. After updating the `application_controller.rb` file it is probably a good point to place a commit:

~~~bash
$ git add .
$ git commit -m "Updates application controller to prevent CSRF exception from being raised"
~~~

### Creating users

Now that we have a better understanding on how to build endpoints and how they work, it’s time to add more abilities to the api, one of the most important is letting the users actually create a profile on our application. As usual we will write tests before implementing our code extending our testing suite.

Creating records in Rails as you may know is really easy, the trick when building an api is which is the best fit for the HTTP codes to send on the response, as well as the actual `json response`. If you don’t totally get this, it will probably be more easy on the code:

**Make sure your repository is clean and that you don’t have any commits left, if so place them so we can start fresh.**

Let’s proceed with our test-driven development by adding a `create` endpoint on the `users_controller_spec.rb` file

~~~ruby
# spec/controllers/api/v1/users_controller_spec.rb
# ...

RSpec.describe Api::V1::UsersController, type: :controller do
  # ...

  describe 'POST #create' do
    context 'when is successfully created' do
      before(:each) do
        @user_attributes = FactoryBot.attributes_for :user
        post :create, params: { user: @user_attributes }, format: :json
      end

      it 'renders the json representation for the user record just created' do
        user_response = JSON.parse(response.body, symbolize_names: true)
        expect(user_response[:email]).to eql @user_attributes[:email]
      end

      it { expect(response.response_code).to eq(201) }
    end

    context 'when is not created' do
      before(:each) do
        # notice I'm not including the email
        @invalid_user_attributes = { password: '12345678',
                                     password_confirmation: '12345678' }
        post :create, params: { user: @invalid_user_attributes }, format: :json
      end

      it 'renders an errors json' do
        user_response = JSON.parse(response.body, symbolize_names: true)
        expect(user_response).to have_key(:errors)
      end

      it 'renders the json errors on why the user could not be created' do
        user_response = JSON.parse(response.body, symbolize_names: true)
        expect(user_response[:errors][:email]).to include "can't be blank"
      end

      it {  expect(response.response_code).to eq(422) }
    end
  end
end
~~~

There is a lot of code up there but don’t worry I’ll walk you through it:

-   We need to validate to states on which the record can be, valid or invalid. In this case we are using the `context` clause to achieve this scenarios.
-   In case everything goes smooth, we should return a `201` HTTP code which means a record just got `created`, as well as the JSON representation of that object.
-   In case of any errors, we have to return a `422` HTTP code which stands for `Unprocessable Entity` meaning the server could save the record. We also return a JSON representation of why the resource could not be saved.

If we run our tests now, they should fail:

~~~bash
$ rspec spec/controllers/api/v1/users_controller_spec.rb
.FFFFFF
~~~

Time to implement some code and make our tests pass:

~~~ruby
# app/controllers/api/v1/users_controller.rb

class Api::V1::UsersController < ApplicationController

  # ...

  def create
    user = User.new user_params
    if user.save
      render json: user, status: 201, location: [:api, user]
    else
      render json: { errors: user.errors }, status: 422
    end
  end

  private

  def user_params
    params.require(:user).permit(:email, :password, :password_confirmation)
  end
end
~~~

Remember that each time we add an enpoint we have to add that action into our `routes.rb` file

~~~ruby
# config/routes.rb

Rails.application.routes.draw do
  # ...
  resources :users, only: [:show, :create]
  # ...
end
~~~

As you can see the implementation is fairly simple, we also added the `user_params` private method to sanitize the attribute to be assigned through mass-assignment. Now if we run our tests, they all should be nice and green:

~~~bash
$ bundle exec rspec spec/controllers/api/v1/users_controller_spec.rb
.......

Finished in 0.05967 seconds (files took 0.4673 seconds to load)
7 examples, 0 failures

~~~

Let’s commit the changes and continue building our application:

~~~bash
$ git add .
$ git commit -m "Adds the user create endpoint"

~~~

### Mettre à jour les utilisateurs

The pattern for `updating` users is very similar as `creating` new ones. If you are an experienced rails developer you may already know the differences between these two actions, and the implications:

- The `update` action responds to a PUT/PATCH request.
- Only the `current user` should be able to update their information, meaning we have to enforce a user to be authenticated. We will cover that on next chapters

As usual we start by writing our tests:

~~~ruby
# spec/controllers/api/v1/users_controller_spec.rb

RSpec.describe Api::V1::UsersController, type: :controller do
  # ...

  describe "PUT/PATCH #update" do

   context "when is successfully updated" do
     before(:each) do
       @user = FactoryBot.create :user
       patch :update, params: {
         id: @user.id,
         user: { email: "newmail@example.com" } },
         format: :json
     end

     it "renders the json representation for the updated user" do
       user_response = JSON.parse(response.body, symbolize_names: true)
       expect(user_response[:email]).to eql "newmail@example.com"
     end

     it {  expect(response.response_code).to eq(200) }
   end

   context "when is not created" do
     before(:each) do
       @user = FactoryBot.create :user
       patch :update, params: {
         id: @user.id,
         user: { email: "bademail.com" } },
         format: :json
     end

     it "renders an errors json" do
       user_response = JSON.parse(response.body, symbolize_names: true)
       expect(user_response).to have_key(:errors)
     end

     it "renders the json errors on whye the user could not be created" do
       user_response = JSON.parse(response.body, symbolize_names: true)
       expect(user_response[:errors][:email]).to include "is invalid"
     end

     it {  expect(response.response_code).to eq(422) }
   end
 end
end
~~~

Getting the tests to pass requires us to build the `update` action on the `users_controller.rb` file as well as adding it to the `routes.rb`. As you can see we have to much code duplicated, we’ll refactor our tests in next chapter.

First we add the action the `routes.rb` file

~~~ruby
# config/routes.rb

Rails.application.routes.draw do
  # ...
  resources :users, only: [:show, :create, :update]
  # ...
end
~~~

Then we implement the `update` action on the users controller and make our tests pass:

~~~ruby
# app/controllers/api/v1/users_controller.rb
class Api::V1::UsersController < ApplicationController
  # ...

  def update
    user = User.find(params[:id])

    if user.update(user_params)
      render json: user, status: 200, location: [:api, user]
    else
      render json: { errors: user.errors }, status: 422
    end
  end

  # ...
end
~~~

If we run our tests, we should now have all of our tests passing.

~~~bash
$ bundle exec rspec spec/controllers/api/v1/users_controller_spec.rb
............

Finished in 0.08826 seconds (files took 0.47286 seconds to load)
12 examples, 0 failures
~~~

We commit the changes as we added a bunch of working code:

~~~bash
$ git add .
$ git commit -m "Adds update action the users controller"
~~~

### Destroying users

So far we have built a bunch of actions on the users controller along with their tests, but we have not ended yet, we are just missing one more which is the destroy action. So let’s do that:

~~~ruby
# spec/controllers/api/v1/users_controller_spec.rb
# ...

RSpec.describe Api::V1::UsersController, type: :controller do
  before(:each) { request.headers['Accept'] = 'application/vnd.marketplace.v1' }

  # ...

  describe "DELETE #destroy" do
    before(:each) do
      @user = FactoryBot.create :user
      delete :destroy, params: { id: @user.id }, format: :json
    end

    it { expect(response.response_code).to eq(204) }
  end
end
~~~

As you can see the spec is very simple, as we only respond with a status of `204` which stands for `No Content`, meaning that the server successfully processed the request, but is not returning any content. We could also return a `200` status code, but I find more natural to respond with nothing in this case as we are deleting a resource and a success response may be enough.

The implementation for the destroy action is fairly simple as well:

~~~ruby
# app/controllers/api/v1/users_controller.rb
class Api::V1::UsersController < ApplicationController
  # ...

  def destroy
    user = User.find(params[:id])
    user.destroy
    head 204
  end

  # ...
end
~~~

Remember to add the `destroy` action to the user resources on the `routes.rb` file:

~~~ruby
# config/routes.rb

Rails.application.routes.draw do
  # ...
  resources :users, only: [:show, :create, :update, :destroy]
  # ...
end
~~~

If you run your tests now, they should be all green:

~~~bash
$ bundle exec rspec spec/controllers/api/v1/users_controller_spec.rb
.............

Finished in 0.09255 seconds (files took 0.4618 seconds to load)
13 examples, 0 failures
~~~

Remember after making some changes to our code, it is good practice to commit them so that we keep our history very atomic.

~~~bash
$ git add .
$ git commit -m "Adds destroy action to the users controller"
~~~

## Conclusion

Oh you are here!, great job! I know it probably was a long way, but don’t give up you are doing it great. Make sure you are understanding every piece of code, things will get better, in next chapter we will refactor our tests to clean our code a bit and make it easy to extend the test suite more. So stay with me guys!
