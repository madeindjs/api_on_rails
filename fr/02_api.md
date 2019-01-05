# L'API

Dans ce chapitre, je vais vous donner les grandes lignes de l'application. Vous devriez avoir lu le chapitre précedent. Si ce n'est pas le cas, je vous recommande de le faire.

Vous pouvez cloner le projet jusqu'ici avec :

~~~bash
$ git clone https://github.com/madeindjs/market_place_api
$ cd market_place_api
$ git checkout -b chapter1 b98a9a7a328017640482af95beebc1d6e612e0ac
~~~

Pour résumer, nous avons mis à jour le *Gemfile* pour ajouter la Gem `active_model_serializers`.

## Planification de l'application

Notre application sera assez simple. Elle se composera de 5 modèles. Ne vous inquiétez pas si vous ne comprenez pas bien ce qui se passe, nous reverrons et développerons chacune de ces ressources au fur et à mesure que nous avancerons avec le tutoriel.

![Schéma des liaisons entre les différent modèles](img/data_model.png)

En bref, nous avons l'utilisateur (`User`) qui sera en mesure de passer de nombreuses commandes (`Order`), télécharger de multiples produits (`product`) qui peuvent avoir de nombreuses images (`Image`) ou commentaires (`Comment`) d'autres utilisateurs sur l'application.

Nous n'allons pas construire d'interface pour l'interaction avec l'API afin de ne pas surcharger le tutoriel. Si vous voulez construire des vues, il existe de nombreuses options comme des frameworks javascript ([Angular](https://angularjs.org/), [Vue.JS](https://vuejs.org/), [React](https://reactjs.org/)) ou des librairies mobile ([AFNetworking](https://github.com/AFNetworking/AFNetworking)).

À ce stade, vous devriez vous poser cette question:

> D'accord, mais j'ai besoin d'explorer et de visualiser l'API que je vais construire, non?

C'est juste. Si vous *googlez* quelque chose lié à l'exploration d'une API, vous allez trouvez pas mal de résultats. Vous pouvez par exemple utiliser [Postman](https://www.getpostman.com/) qui est devnu incontournable.

## Mettre en place l'API

Une API est définie par [wikipedia](https://fr.wikipedia.org/wiki/Interface_de_programmation) comme une interface de programmation d'application (API) qui est un ensemble normalisé de composants qui sert de façade par laquelle un logiciel offre des services à d'autres logiciels. En d'autres termes, il s'agit d'une façon dont les systèmes interagissent les uns avec les autres via une interface (dans notre cas un service web construit avec JSON)[^json_method].

[^json_method]: Il existe d'autres types de protocoles de communication comme SOAP, mais nous n'en parlons pas ici.

JSON est devenu incontournable en tant sur média Internet en raison de sa lisibilité, de son extensibilité et de sa facilité à mettre en œuvre. Beaucoup de frameworks JavaScript l'utilisent comme protocole par défaut comme [Angular](https://angularjs.org/) ou [EmberJS](http://emberjs.com/). D'autres grandes bibliothèques en Objective-C l'utilisent comme [AFNetworking](https://github.com/AFNetworking/AFNetworking) ou [RESTKit](http://restkit.org/). Il existe probablement de bonnes solutions pour Android, mais en raison de mon manque d'expérience sur cette plate-forme de développement je ne suis peut-être pas la bonne personne pour vous recommander quelque chose.

Nous allons donc l'utiliser pour construire notre API. La première chose qui pourrait vous venir à l'esprit serait de commencer à créer des routes en vrac. Le problème est quelles ne seraient pas normalisées. Un utilisateur ne pourrait pas deviner qu'elle ressource est renvoyée par une route.

C'est pourquoi une norme existe: **REST** *(Representational State Transfer)*. REST impose une norme pour les routes qui crée, lise, mette à jour ou supprime des informations sur un serveur en utilisant de simples appels HTTP. C'est une alternative aux mécanismes plus complexes comme SOAP, CORBA et RPC. Un appel REST est simplement une requête GET HTTP vers le serveur.

~~~soap
aService.getUser("1")
~~~

Et avec REST, vous pouvez appeler une URL avec une requête HTTP spécifique. Dans ce cas avec une requête GET:

~~~
http://domain.com/resources_name/uri_pattern
~~~

Les API RESTful doivent suivre aux minimum trois règles:

- Une URI de base comme <http://example.com/resources/>
- Un type de média Internet pour représenter les données, il est communément JSON et est communément défini par l'échange d'en-têtes.
- Suivez les méthodes [HTTP](https://fr.wikipedia.org/wiki/Hypertext_Transfer_Protocol) standard telles que GET, POST, PUT, PUT, DELETE.

    - **GET**: Lit la ou les ressources définies par le modèle URI
    - **POST**: Crée une nouvelle entrée dans la collection de ressources
    - **PUT**: Mise à jour d'une collection ou d'un membre des ressources
    - **DELETE**: Détruit une collection ou un membre des ressources

Cela peut sembler compliqué mais au fur et à mesure que nous avancerons dans le tutoriel cela deviendra beaucoup plus facile à comprendre.

### Routes, contraintes et *Namespaces*

Avant de commencer à taper du code, nous allons préparer le répertoire Git. Le *workflow* que nous allons suivre est le suivant:

- Nous allons créer une branche par chapitre
- Une fois terminé, nous le pousserons la branche sur github
- Nous la fusionnerons avec master

Commençons donc par ouvrir le terminal dans le répertoire `market_place_api` et tapez la commande suivante pour créer la branche:

~~~bash
$ git checkout -b setting-api
Switched to a new branch 'setting-api'
~~~

Nous allons seulement travailler sur le fichier `config/routes.rb` car nous allons simplement définir les contraintes et le format de réponse par défaut pour chaque requête.

~~~ruby
# config/routes.rb
Rails.application.routes.draw do
  # ...
end
~~~

Effacez tout le code commenté qui se trouve dans le fichier. Nous n'en aurons pas besoin. Ensuite, faites un *commit*, juste pour s'échauffer:

~~~bash
$ git add config/routes.rb
$ git commit -m "Removes comments from the routes file"
~~~

Nous allons isoler les contrôleurs API dans des *Namespace*. Avec Rails, c'est assez simple. Il suffit de créer un dossier sous `app/controllers` nommé `api`. Le nom est important car c'est le *Namespace* que nous allons utiliser pour gérer les contrôleurs pour les points d'entrée de l'API

~~~bash
$ mkdir app/controllers/api
~~~

Nous ajoutons ensuite ce *Namespace* dans notre fichier `routes.rb`:

~~~ruby
# config/routes.rb
Rails.application.routes.draw do
  # Api definition
  namespace :api do
    # We are going to list our resources here
  end
end
~~~

En définissant un *Namespace* dans le fichier `routes.rb`, Rails mappera automatiquement ce *Namespace* à un répertoire correspondant au nom sous le dossier controllers (dans notre cas le répertoire `api/`).

Rails supporte jusqu'à 35 types de médias différents! Vous pouvez les lister en accédant à la classe `SET` sous le module de `Mime`:

~~~bash
$ rails c
Loading development environment (Rails 5.2.1)
irb(main):001:0> Mime::SET.collect(&:to_s)
=> ["text/html", "text/plain", "text/javascript", "text/css", "text/calendar", "text/csv", "text/vcard", "text/vtt", "image/png", "image/jpeg", "image/gif", "image/bmp", "image/tiff", "image/svg+xml", "video/mpeg", "audio/mpeg", "audio/ogg", "audio/aac", "video/webm", "video/mp4", "font/otf", "font/ttf", "font/woff", "font/woff2", "application/xml", "application/rss+xml", "application/atom+xml", "application/x-yaml", "multipart/form-data", "application/x-www-form-urlencoded", "application/json", "application/pdf", "application/zip", "application/gzip", "application/vnd.web-console.v2"]
~~~

C'est important parce que nous allons travailler avec JSON, l'un des types MIME intégrés par Rails. Ainsi nous avons juste besoin de spécifier ce format comme format par défaut:

~~~ruby
# config/routes.rb
Rails.application.routes.draw do
  # Api definition
  namespace :api, defaults: { format: :json }  do
    # We are going to list our resources here
  end
end
~~~

Jusqu'à présent, nous n'avons rien fait de compliqué. Nous voulons maintenant générer une `base_uri` sous un sous-domaine. C'est-à-dire quelque chose comme `api.market_place_api.dev`. Définir l'API sous un sous-domaine est une bonne pratique car cela permet d'adapter l'application à un niveau DNS. Alors, comment y parvenir?

~~~ruby
# config/routes.rb
Rails.application.routes.draw do
  # Api definition
  namespace :api, defaults: { format: :json }, constraints: { subdomain: 'api' }, path: '/'  do
    # We are going to list our resources here
  end
end
~~~

Vous voyez la différence? Nous n'avons pas seulement ajouté un [`Hash`][ruby_hash] de contraintes pour spécifier le sous-domaine, nous avons aussi ajouté l'option chemin d'accès et lui avons donné un *backslash*. Cel indique à Rails que le chemin de départ pour chaque requête est la racine par rapport au sous-domaine.

### Les conventions des API

Vous pouvez trouver de nombreuses approches pour configurer la `base_uri` d'une API. En supposant que nous versionnons notre api:

- `api.example.com/`: Je suis d'avis que c'est la voie à suivre, vous donne une meilleure interface et l'isolement, et à long terme peut vous aider à [mettre rapidement à l'échelle](http://www.makeuseof.com/tag/optimize-your-dns-for-faster-internet/)
- `example.com/api/`: Ce modèle est très commun. C'est un bon moyen de commencer quand vous ne voulez pas de Namespace de votre API avec sous un sous-domaine
- `example.com/api/v1`: Cela semble être une bonne idée. En définissant la version de l'API par l'URL semble être un modèle plus descriptif. Cependant, vous forcez à inclure la version àl'URL sur chaque demande. Cela devient un problème Si vous décidez de changer ce modèle

Ne vous inquiétez pas, nous rentrerons plus en détails à propos du versionnement plus tard. Il est temps de *commiter*:

~~~bash
$ git add config/routes.rb
$ git commit -m "Set the routes contraints for the api"
~~~

## Versionnement de l'API

A ce stade, nous devrions avoir un bon mappage des routes utilisant un sous-domaine pour l'espacement des noms des requêtes. Votre fichier `routes.rb` devrait ressembler à ceci:

~~~ruby
# config/routes.rb
Rails.application.routes.draw do
  # Api definition
  namespace :api, defaults: { format: :json }, constraints: { subdomain: 'api' }, path: '/'  do
    # We are going to list our resources here
  end
end
~~~

Il est maintenant temps de mettre en place d'autres contraintes pour le *versioning*. Vous devriez vous soucier de versionner votre application dès le début car cela donnera une **meilleure structure** à votre API. Lorsque des changements interviendrons sur votre API, vous pouvez ainsi proposer aux développeurs de s'adapter aux nouvelles fonctionnalités pendant que les anciennes sont dépréciées.

Afin de définir la version de l'API, nous devons d'abord ajouter un autre répertoire sous le dossier `api/` que nous avons créé:

~~~bash
$ mkdir app/controllers/api/v1
~~~

De cette façon, nous pouvons très facilement définir la portée de notre API dans différentes versions. Il ne nous reste plus qu'à ajouter le code nécessaire dans le fichier `routes.rb`

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

L'API est désormais *scopée* via l'URL. Par exemple, avec la configuration actuelle, la récupération d'un produit via l'API se ferait avec cette url: <http://api.marketplace.dev/v1/products/1>.


## Améliorer le versionnement

Jusqu'à présent, l'API est versionnée via l'URL. Mais quelque chose ne va pas. De mon point de vue, le développeur ne devrait pas être au courant de la version qu'il utilise. Par défaut, il devrait utiliser la dernière version. Mais comment pouvons-nous y parvenir?

Tout d'abord, nous devons améliorer l'accès à la version de l'API via les [en-têtes HTTP](http://en.wikipedia.org/wiki/List_of_HTTP_header_fields). Cela permet de supprimer la version de l'API situé dans l'URL.

## Description des en-têtes de requête

Les champs d'en-tête HTTP sont des composants de l'en-tête de demandes et de réponses dans le protocole HTTP. Ils définissent les paramètres de fonctionnement d'une transaction HTTP.

Une liste commune des en-têtes utilisés est présentée ci-dessous:

- **Accept**: Types de contenu acceptables pour la réponse. Exemple: `Accept: text/plain`
- **Authorization**: Identifiants d'authentification pour l'authentification HTTP. Exemple: `Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==`
- **Content-Type**: Le type MIME du corps de la requête (utilisé avec les requêtes POST et PUT). Exemple: `Content-Type: application/x-www-form-urlencoded`
- **Origin**: Lance une demande de partage de ressources d'origine croisée (demande au serveur un en-tête de réponse `Access-Controle-Autorisation-Autorisation-Origin`). Exemple: `Origin: http://www.example-social-network.com`
- **User-Agent**: La chaîne d'agent utilisateur de l'agent utilisateur. Exemple: `User-Agent: Mozilla/5.0`

Il est important que vous vous sentiez à l'aise et que vous les compreniez ces en-tête HTTP.

Avec Rails, il est très facile d'ajouter ce type de versionnement par le biais d'un en-tête HTTP `Accept`. Nous allons créer une classe sous le répertoire `lib`. N'oubliez pas que nous faisons du TDD[^11] donc nous allons commencer par un test.

Tout d'abord, nous devons ajouter notre suite de tests, qui dans notre cas sera [Rspec](http://rspec.info/):

~~~ruby
# Gemfile
group :test do
  gem 'rspec-rails', '~> 3.8'
  gem 'factory_bot_rails', '~> 4.9'
  gem 'ffaker', '~> 2.10'
end
~~~

Ensuite nous lançons la commande `bundle` pour installer les gemmes:

~~~bash
$ bundle install
~~~

Enfin, nous installons `rspec` et ajoutons de la configuration pour éviter que des *views* et des *helpers* ne soient générés:

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

Si tout s'est bien passé, il est maintenant temps d'ajouter un répertoire `spec` sous `lib` et d'ajouter le fichier `api_constraints_spec.rb`:

~~~bash
$ mkdir lib/spec
$ touch lib/spec/api_constraints_spec.rb
~~~

Nous ajoutons ensuite une série de spécifications décrivant notre classe:

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

Laissez-moi vous expliquer le code. Nous initialisons la classe avec un [`Hash`][ruby_hash] d'options qui contiendra:

- la version de l'API
- une valeur par défaut pour gérer la version par défaut

Nous fournissons une méthode `match?` afin que le routeur devine si la version par défaut est requise ou si l'en-tête `Accept` correspond à la chaîne donnée.

L'implémentation ressemble à ceci

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

Comme vous l'imaginez, nous devons ajouter la classe à notre fichier `routes.rb` et la définir comme option de portée de contrainte:

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

La configuration ci-dessus gère maintenant le *versioning* par le biais des en-têtes HTTP. Pour l'instant la version 1 est la version par défaut. Chaque requête sera redirigée vers cette version, peu importe si l'en-tête avec la version est présent ou non. Avant de nous dire au revoir, faisons nos premiers tests et assurons-nous que notre test fonctionne:

~~~bash
$ bundle exec rspec lib/spec/api_constraints_spec.rb
..

Finished in 0.00294 seconds (files took 0.06292 seconds to load)
2 examples, 0 failures
~~~

## Conclusion

Ça a été un long, je sais, mais vous avez réussi! N'abandonnez pas, c'est juste notre petite fondation pour quelque chose de grand, alors continuez comme ça. Sachez qu'il y a des gemmes qui gèrent ce genre de configuration pour nous:

- [RocketPants](https://github.com/Sutto/rocket_pants)
- [Versionist](https://github.com/bploetz/versionist)

Je n'en parle pas ici puisque nous essayons d'apprendre comment mettre en œuvre ce genre de fonctionnalité. Le code jusqu'ici est disponnible
[ici](https://github.com/madeindjs/market_place_api/commit/124873774b578af3df21136df5ee80f4d50da3bd).
