# The API

In this section I’ll outline the application. By now you should have the bare bones of the application. If you did not read it I recommend you to do it.

You can clone the project until this point with:

~~~bash
$ git clone https://github.com/madeindjs/market_place_api
$ cd market_place_api
$ git checkout -b chapter1 b98a9a7a328017640482af95beebc1d6e612e0ac
~~~

And as a quick recap, we really just update the `Gemfile` to add the `active_model_serializers` gem.

## Planification de l'application

As we want to go simple with the application, it consists on 5 models. Don’t worry if you don’t fully understand what is going on, we will review and build each of these resources as we move on with the tutorial.

![Schéma des liaisons entre les différent modèles](img/data_model.png)

In short terms we have the `user` who will be able to place many `orders`, upload multiple `products` which can have many `images` or `comments` from another users on the app.

We are not going to build views for displaying or interacting with the API, so not to make this a huge tutorial, I’ll let that to you. There are plenty of options out there, from javascript frameworks([Angular](https://angularjs.org/), [EmberJS](http://emberjs.com/), [Backbone](http://backbonejs.org/)) to mobile consumption([AFNetworking](https://github.com/AFNetworking/AFNetworking)).

By this point you must be asking yourself, all right but I need to explore or visualize the api we are going to be building, and that’s fair. Probably if you google something related to api exploring, an application called [Postman](https://chrome.google.com/webstore/detail/postman-rest-client/fdmmgilgnpjigdojojpjoooidkmcomcm?hl=en) will pop, it is a great add on if you are using chrome, but we won’t be using that anyway, as probably not every developer uses the Google browser. Instead of that we will be using a gem I built called `sabisu_rails` which is a powerful postman-like engine client to explore your Rails application api’s. We will cover the gem integration

## Setting the API

An API is defined by [wikipedia](http://en.wikipedia.org/wiki/Application_programming_interface) as _an application programming interface (API) specifies how some software components should interact with each other._ In other words the way systems interact with each other through a common interface, in our case a web service built with `json`. There are other kinds of communication protocols like SOAP, but we are not covering that in here.

JSON as the Internet media type is highly accepted because of readability, extensibility and easy to implement in fact many of the current frameworks consume json api’s by default, in Javascript there is [Angular](https://angularjs.org/) or [EmberJS](http://emberjs.com/), but there are great libraries for objective-c too like [AFNetworking](https://github.com/AFNetworking/AFNetworking) or [RESTKit](http://restkit.org/). There are probably good solutions for Android, but because of my lack of experience on that development platform I might not be the right person to recommend you something.

All right, so we are building our api with `json`, but there are many ways to achieve this, the first thing that could come to your mind would be just to start dropping some routes defining the [end points](http://en.wikipedia.org/wiki/Web_Services_Description_Language#Objects_in_WSDL_1.1_.2F_WSDL_2.0) but they may not have a [URI pattern](http://www.w3.org/2005/Incubator/wcl/matching.html) clear enough to know which resource is being exposed. The protocol or structure I’m talking about is [REST](http://en.wikipedia.org/wiki/Representational_state_transfer) which stands for Representational State Transfer and by wikipedia definition _is a way to create, read, update or delete information on a server using simple HTTP calls. It is an alternative to more complex mechanisms like SOAP, CORBA and RPC. A REST call is simply a GET HTTP request to the server._

~~~soap
aService.getUser("1")
~~~

And in REST you may call a URL with an especific HTTP request, in this case with a GET request:
~~~
http://domain.com/resources_name/uri_pattern
~~~

RESTful APIs must follow at least 3 simple guidelines:

- A base [URI](http://en.wikipedia.org/wiki/Uniform_resource_identifier), such as `http://example.com/resources/`.
- An Internet media type to represent the data, it is commonly `JSON` and is commonly set through headers exchange.
- Follow the standard [HTTP Methods](http://en.wikipedia.org/wiki/HTTP_method#Request_methods) such as GET, POST, PUT, DELETE.

    - **GET**: Reads the resource or resources defined by the URI pattern
    - **POST**: Creates a new entry into the resources collection
    - **PUT**: Updates a collection or member of the resources
    - **DELETE**: Destroys a collection or member of the resources

This might not be clear enough or may look like a lot of information to digest but as we move on with the tutorial, hopefully it’ll get a lot easier to understand.

### Routes, Constraints and Namespaces

Before start typing any code, we prepare the code with git, the workflow we’ll be using a branch per chapter, upload it to github and then merge it with master, so let’s get started open the terminal, `cd` to the `market_place_api` directory and type in the following:

~~~bash
$ git checkout -b setting-api
Switched to a new branch 'setting-api'
~~~

We are only going to be working on the `config/routes.rb`, as we are just going to set the `constraints`, the `base_uri` and the default response `format` for each request.

~~~ruby
# config/routes.rb
Rails.application.routes.draw do
  # ...
end
~~~

First of all erase all commented code that comes within the file, we are not gonna need it. Then commit it, just as a warm up:

~~~bash
$ git add config/routes.rb
$ git commit -m "Removes comments from the routes file"
~~~

We are going to isolate the api controllers under a namespace, in Rails this is fairly simple, just create a folder under the `app/controllers` named `api`, the name is important as it is the namespace we’ll use for managing the controllers for the api endpoints.

~~~bash
$ mkdir app/controllers/api
~~~

We then add that namespace into our `routes.rb` file:

~~~ruby
# config/routes.rb
Rails.application.routes.draw do
  # Api definition
  namespace :api do
    # We are going to list our resources here
  end
end
~~~

By defining a namespace under the `routes.rb` file. Rails will automatically map that namespace to a directory matching the name under the `controllers` folder, in our case the `api/` directory.

Rails can handle up to 35 different media types, you can list them by accessing the SET class under de Mime module:

~~~bash
$ rails c
Loading development environment (Rails 5.2.1)
irb(main):001:0> Mime::SET.collect(&:to_s)
=> ["text/html", "text/plain", "text/javascript", "text/css", "text/calendar", "text/csv", "text/vcard", "text/vtt", "image/png", "image/jpeg", "image/gif", "image/bmp", "image/tiff", "image/svg+xml", "video/mpeg", "audio/mpeg", "audio/ogg", "audio/aac", "video/webm", "video/mp4", "font/otf", "font/ttf", "font/woff", "font/woff2", "application/xml", "application/rss+xml", "application/atom+xml", "application/x-yaml", "multipart/form-data", "application/x-www-form-urlencoded", "application/json", "application/pdf", "application/zip", "application/gzip", "application/vnd.web-console.v2"]
~~~

This is important because we are going to be working with JSON, one of the built-in [MIME types](http://en.wikipedia.org/wiki/Internet_media_type) accepted by Rails, so we just need to specify this format as the default one:

~~~ruby
# config/routes.rb
Rails.application.routes.draw do
  # Api definition
  namespace :api, defaults: { format: :json }  do
    # We are going to list our resources here
  end
end
~~~

Up to this point we have not made anything crazy, what we want to achieve next is how to generate a *base_uri* under a subdomain, in our case something like `api.market_place_api.dev`. Setting the api under a subdomain is a good practice because it allows to scale the application to a DNS level. So how do we achieve that?

~~~ruby
# config/routes.rb
Rails.application.routes.draw do
  # Api definition
  namespace :api, defaults: { format: :json }, constraints: { subdomain: 'api' }, path: '/'  do
    # We are going to list our resources here
  end
end
~~~

Notice the changes?, we didn’t just add a `constraints` hash to specify the subdomain, but we also add the `path` option, and set it a _backslash_. This is telling Rails to set the starting path for each request to be root in relation to the subdomain, achieving what we are looking for.

### Common api patterns

You can find many approaches to set up the *base_uri* when building an api following different patterns, assuming we are versioning our api:

- `api.example.com/`: I my opinion this is the way to go, gives you a better interface and isolation, and in the long term can help you to [quickly scalate](http://www.makeuseof.com/tag/optimize-your-dns-for-faster-internet/)
- `example.com/api/`: This pattern is very common, and it is actually a good way to go when you don’t want to namespace your api under a subdomain
- `example.com/api/v1`: his seems like a good idea, by setting the version of the api through the URL seems like a more descriptive pattern, but this way you enforce the version to be included on URL on each request, so if you ever decide to change this pattern, this becomes a problem of maintenance in the long-term

Don’t worry about versioning right now, I’ll walk through it later. Time to commit:

~~~bash
$ git add config/routes.rb
$ git commit -m "Set the routes contraints for the api"
~~~

All right take a deep breath, drink some water, and let’s get going.

## Api versioning

At this point we should have a nice routes mapping using a subdomain for name spacing the requests, your `routes.rb` file should look like this:

~~~ruby
# config/routes.rb
Rails.application.routes.draw do
  # Api definition
  namespace :api, defaults: { format: :json }, constraints: { subdomain: 'api' }, path: '/'  do
    # We are going to list our resources here
  end
end
~~~

Now it is time to set up some other constraints for versioning purposes. You should care about versioning your application from the beginning since this will give a better structure to your api, and when changes need to be done, you can give developers who are consuming your api the opportunity to adapt for the new features while the old ones are being deprecated. There is an excellent [railscast](http://railscasts.com/episodes/350-rest-api-versioning) explaining this.

In order to set the version for the api, we first need to add another directory under the `api` we created

~~~bash
$ mkdir app/controllers/api/v1
~~~

This way we can scope our api into different versions very easily, now we just need to add the necessary code to the `routes.rb` file

~~~ruby
# config/routes.rb
Rails.application.routes.draw do
  # Api definition
  namespace :api, defaults: { format: :json }, constraints: { subdomain: 'api' }, path: '/'  do
    scope module: :v1 do
      # We are going to list our resources here
    end
  end
end
~~~

By this point the API is now scoped via de URL. For example with the current configuration an end point for retrieving a product would be like: <http://api.marketplace.dev/v1/products/1>.


## Improving the versioning

So far we have the API versioned scoped via the URL, but something doesn’t feel quite right, isn’t it?. What I mean by this is that from my point of view the developer should not be aware of the version using it, as by default they should be using the last version of your endpoints, but how do we accomplish this?.

Well first of all, we need to improve the API version access through [HTTP Headers](http://en.wikipedia.org/wiki/List_of_HTTP_header_fields). This has two benefits:

- Removes the version from the URL
- The API description is handle through request headers

HTTP header fields are components of the message header of requests and responses in the Hypertext Transfer Protocol (HTTP). They define operating parameters of an HTTP transaction. A common list of used headers is presented below:

- **Accept**: Content-Types that are acceptable for the response. Example: `Accept: text/plain`
- **Authorization**: Authentication credentials for HTTP authentication. Example: `Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==`
- **Content-Type**: The MIME type of the body of the request (used with POST and PUT requests). Example: `Content-Type: application/x-www-form-urlencoded`
- **Origin**: Initiates a request for cross-origin resource sharing (asks server for an ‘Access-Control-Allow-Origin’ response header). Example: `Origin: http://www.example-social-network.com`
- **User-Agent**: The user agent string of the user agent. Example: `User-Agent: Mozilla/5.0`

It is important that you feel comfortable with this ones and understand them.

In Rails is very easy to add this type versioning through an _Accept_ header. We will create a class under the `lib` directory of your rails app, and remember we are doing [TDD](http://en.wikipedia.org/wiki/Test-driven_development) so first things first.

First we need to add our testing suite, which in our case is going to be [Rspe](http://rspec.info/):

~~~ruby
# Gemfile
group :test do
  gem 'rspec-rails', '~> 3.8'
  gem 'factory_bot_rails', '~> 4.9'
  gem 'ffaker', '~> 2.10'
end
~~~

Then we run the bundle command to install the gems

~~~bash
$ bundle install
~~~

Finally we install the `rspec` and add some configuration to prevent views and helpers tests from being generated:

~~~bash
$ rails generate rspec:install
~~~

~~~ruby
# config/application.rb
# ...
module MarketPlaceApi
  class Application < Rails::Application
    # Initialize configuration defaults for originally generated Rails version.
    config.load_defaults 5.2

    config.generators do |g|
      g.test_framework :rspec, fixture: true
      g.fixture_replacement :factory_bot, dir: 'spec/factories'
      g.view_specs false
      g.helper_specs false
      g.stylesheets = false
      g.javascripts = false
      g.helper = false
    end

    config.autoload_paths += %W(\#{config.root}/lib)

    # Don't generate system test files.
    config.generators.system_tests = nil
  end
end
~~~

If everything went well it is now time to add a `spec` directory under `lib` and add the `api_constraints_spec.rb`:

~~~bash
$ mkdir lib/spec
$ touch lib/spec/api_constraints_spec.rb
~~~

We then add a bunch of specs describing our class:

~~~ruby
# lib/spec/api_constraints_spec.rb
require 'spec_helper'
require './lib/api_constraints'

describe ApiConstraints do
  let(:api_constraints_v1) { ApiConstraints.new(version: 1) }
  let(:api_constraints_v2) { ApiConstraints.new(version: 2, default: true) }

  describe 'matches?' do
    it "returns true when the version matches the 'Accept' header" do
      request = double(host: 'api.marketplace.dev',
                       headers: { 'Accept' => 'application/vnd.marketplace.v1' })
      expect(api_constraints_v1.matches?(request)).to be_truthy
    end

    it "returns the default version when 'default' option is specified" do
      request = double(host: 'api.marketplace.dev')
      expect(api_constraints_v2.matches?(request)).to be_truthy
    end
  end
end
~~~

Let me walk you through the code. We are initializing the class with an options hash, which will contain the version of the api, and a default value for handling the default version. We provide a `matches?` method which the router will trigger for the constraint to see if the default version is required or the `Accept` header matches the given string.

The implementation looks likes this

~~~ruby
# lib/api_constraints.rb
class ApiConstraints
  def initialize(options)
    @version = options[:version]
    @default = options[:default]
  end

  def matches?(req)
    @default || req.headers['Accept'].include?("application/vnd.marketplace.v#{@version}")
  end
end
~~~

As you imagine we need to add the class to our `routes.rb` file and set it as a constraint scope option.

~~~ruby
# config/routes.rb
require 'api_constraints'

Rails.application.routes.draw do
  # Api definition
  namespace :api, defaults: { format: :json }, constraints: { subdomain: 'api' }, path: '/' do
    scope module: :v1, constraints: ApiConstraints.new(version: 1, default: true) do
      # We are going to list our resources here
    end
  end
end
~~~

The configuration above now handles versioning through headers, and for now the version 1 is the default one, so every request will be redirected to that version, no matter if the header with the version is present or not.

Before we say goodbye, let’s run our first tests and make sure everything is nice and green:

~~~bash
$ bundle exec rspec lib/spec/api_constraints_spec.rb
..

Finished in 0.00294 seconds (files took 0.06292 seconds to load)
2 examples, 0 failures
~~~

## Conclusion

It’s been a long way, I know, but you made it, don’t give up this is just our small scaffolding for something big, so keep it up. In the meantime and I you feel curious there are some gems that handle this kind of configuration:

- [RocketPants](https://github.com/Sutto/rocket_pants)
- [Versionist](https://github.com/bploetz/versionist)

I’m not covering those in here, since we are trying to learn how to actually implement this kind of functionality, but it is good to know though. By the way the code up to this point is
[here](https://github.com/madeindjs/market_place_api/commit/124873774b578af3df21136df5ee80f4d50da3bd).
