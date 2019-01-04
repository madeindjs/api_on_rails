---
title: API on Rails
subtitle: Construction d'API REST avec Rails

author: Alexandre Rousseau <contact@rousseau-alexandre.fr>
creator: Alexandre Rousseau <contact@rousseau-alexandre.fr>

cover-image: img/cover.png

ibooks:
  version: 1.3.4

rights: © 2019 Alexandre Rousseau, CC BY-SA 4.0

toc: true
lang: fr-FR
documentclass: book
links-as-notes: true


---



## Avant-propos

"API on Rails 5" est une mise à jour et une traduction française du livre ["APIs on Rails: Building REST APIs with Rails"](http://apionrails.icalialabs.com/book/). Celui-ci fut initialement publié en 2014 par [Abraham Kuri](https://twitter.com/kurenn) sous les licences [MIT](http://opensource.org/licenses/MIT) et [Beerware](http://people.freebsd.org/~phk/).

## A propos de l'autheur original

[Alexandre Rousseau](http://rousseau-alexandre.fr) est un développeur Rails avec plus de 4 ans d'experience. Mon experienxe 

[Abraham Kuri](https://twitter.com/kurenn) est un développeur de Rails avec 5 ans d'expérience. Son expérience inclut le travail en tant que *freelance* dans la construction de produits logiciels et plus récemment dans la collaboration au sein de la communauté open source. Il a développé [Furatto](http://icalialabs.github.io/furatto/) un cadre frontal construit avec Sass, [Sabisu](https://github.com/IcaliaLabs/sabisu-rails) la prochaine génération d'explorateur d'API pour votre application Rails et a collaboré sur d'autres projets. Diplômé en informatique d'ITESM, il a fondé deux sociétés au Mexique ([Icalia Labs](http://icalialabs.com/) et [Codeando Mexico](http://codeandomexico.org/)).

## Droits d'auteur et licence

Cette traduction est disponible sous [licence MIT](http://opensource.org/licenses/MIT). Tout le code source de ce livre est disponible au format [Markdown](https://fr.wikipedia.org/wiki/Markdown) sur [Github][api_on_rails_git]

> La licence MIT Copyright (c) 2019 Alexandre Rousseau
>
> Permission est accordée, à titre gratuit, à toute personne obtenant une copie de ce logiciel et la documentation associée, pour faire des modification dans le logiciel sans restriction et sans limitation des droits d'utiliser, copier, modifier, fusionner, publier, distribuer, concéder sous licence, et / ou de vendre les copies du Logiciel, et à autoriser les personnes auxquelles le Logiciel est meublé de le faire, sous réserve des conditions suivantes:
>
> L'avis de copyright ci-dessus et cette autorisation doit être inclus dans toutes les copies ou parties substantielles du Logiciel.
>
> LE LOGICIEL EST FOURNI «TEL QUEL», SANS GARANTIE D'AUCUNE SORTE, EXPLICITE OU IMPLICITE, Y COMPRIS, MAIS SANS S'Y LIMITER, LES GARANTIES DE QUALITÉ MARCHANDE, ADAPTATION À UN USAGE PARTICULIER ET D'ABSENCE DE CONTREFAÇON. EN AUCUN CAS LES AUTEURS OU TITULAIRES DU ETRE TENU RESPONSABLE DE TOUT DOMMAGE, RÉCLAMATION OU AUTRES RESPONSABILITÉ, SOIT DANS UNE ACTION DE CONTRAT, UN TORT OU AUTRE, PROVENANT DE, DE OU EN RELATION AVEC LE LOGICIEL OU L'UTILISATION OU DE TRANSACTIONS AUTRES LE LOGICIEL.

"API on Rails 5" de [Alexandre Rousseau][api_on_rails_git] est mis à disposition selon les termes de la licence [Creative Commons Attribution - Partage dans les Mêmes Conditions 4.0 International](http://creativecommons.org/licenses/by-sa/4.0/). Fondé sur une œuvre à <http://apionrails.icalialabs.com/book/>.

---

# Introduction {#chapter:1}

Bienvenue sur API on Rails 5, un tutoriel sous stéroïdes à propos de la meilleur façon de construire votre prochaine API avec Rails. Le but de ce livre est de vous fournir une méthodologie complète pour développer une API RESTful en suivant les meilleures pratiques existantes. Lorsque vous en aurez fini avec ce livre, vous serez en mesure de créer votre propre API et de l'intégrer à n'importe quel client comme un navigateur Web ou votre une application mobile. Le code généré est construit avec Ruby on Rails 5.2 qui est la version actuelle (pour plus d'informations à ce sujet, consultez [rubyonrails.org](http://rubyonrails.org/)). La version la plus récente de APIs on Rails se trouve sur [Github][api_on_rails_git] ; n'oubliez pas de mettre à jour votre version hors ligne si c'est le cas.

L'intention de ce livre n'est pas seulement de vous apprendre à construire une API avec Rails mais plutôt de vous apprendre comment construire une API évolutive et maintenable avec Rails. C'est-à-dire améliorer vos connaissances actuelles avec Rails. Dans ce voyage, vous allez apprendrez à:

- Construire des réponses JSON
- Utiliser Git pour le contrôle de version
- Test de vos points finaux
- Optimiser et mettre en cache l'API

Je vous recommande fortement de suivre toutes les étapes de ce livre. Essayez de ne pas sauter des chapitres car je vais vous donner des conseils et des astuces pour vous améliorer tout au long du livre. Vous pouvez vous considérer comme le personnage principal d'un jeu vidéo qui obtient un niveau supérieur à chaque chapitre.

Dans ce premier chapitre, je vous expliquerai comment configurer votre environnement (au cas où vous ne l'auriez pas déjà). Nous allons ensuite créer une application appelée `market_place_api`. Je veillerai à vous enseigner les meilleures pratiques que j'ai pu apprendre au cours de mon expérience. Cela signifie qu'après avoir initialisé le projet, nous commencerons à utiliser **Git** .

Dans les prochains chapitres, nous allons construire l'application en suivant une métode de travail simple que j'utilise quotidiennement. Nous développerons toute l'application en utilisant le **développement piloté par les tests** (TDD). Je vous expliquerai aussi l'intérêt d'utiliser une API pour votre prochain projet et de choisir un format de réponse adapté comme le JSON ou le XML. Plus loin, nous mettrons les mains dans le code et nous compléterons les bases de l'application en construisant tous les routes nécessaires. Nous sécuriserons aussi l'accès à l'API en construisant une authentification par échange d'en-têtes HTTP. Enfin, dans le dernier chapitre, nous ajouterons quelques techniques d'optimisation pour améliorer la structure et les temps de réponse du serveur.

L'application finale sera une fonction pour une application de place de marché où les utilisateurs seront en mesure de passer des commandes, télécharger des produits et plus encore. Il existe de nombreuses options pour créer une boutique en ligne comme [Shopify](http://shopify.com/), [Spree](http://spreecommerce.com/) ou [Magento](http://magento.com/).

Tout au long de ce voyage (cela dépend vraiment de votre expertise), vous allez vous améliorer et être en mesure de mieux comprendre certaines des meilleures ressources Rails. J'ai aussi pris certaines des pratiques que j'ai trouvé sur ces sites:

- [Railscasts](http://railscasts.com/)
- [CodeSchool](http://codeschool.com/)
- [JSON API](http://jsonapi.org/format/)

## Conventions sur ce livre

Les conventions de ce livre sont basées sur celles du Tutoriel Ruby on Rails. Dans cette section, je vais en mentionner quelques-unes que vous ne connaissez peut-être pas.

Je vais utiliser de nombreux exemples en utilisant des ligne de commande. Je ne vais pas traiter avec Windows `cmd` (désolé les gars). Je vais baser tous les exemples en utilisant l'invite de ligne de commande de style Unix. Voici un exemple:

~~~bash
$ echo "A command-line command"
A command-line command
~~~

J'utiliserai quelques principes spécifiques à Ruby. C'est-à-dire:

- *"Éviter"* signifie que vous n'êtes pas censé le faire.
- *"Préférer"* indique que parmi les 2 options, la première est la plus appropriée.
- *"Utiliser"* signifie que vous êtes en mesure d'utiliser la ressource.

Si vous rencontrez une erreur quelconque lors de l'exécution d'une commande, je vous recommande d'utiliser votre moteur de recherche pour trouver votre solution. Malheureusement, je ne peux pas couvrir toutes les erreurs possibles. Si vous rencontrez des problèmes avec ce tutoriel, vous pouvez toujours [m'envoyer un email](mailto:contact@rousseau-alexandre.fr).


## Environnements de développement

Pour presque tous les développeurs, l'une des parties les plus douloureuses est de mettre en place un environnement de développement confortable. Si vous le faites correctement, les prochaines étapes devraient être un jeu d'enfant. Afin de vous faciliter la tâche et de vous motiver, je vais vous guider dans cette étape.

### Éditeurs de texte et Terminal

Les environnements de développement diffèrent d'un ordinateur à l'autre. Ce n'est pas le cas avec les éditeurs de texte. Je pense que pour le développement avec Rails, un IDE est beaucoup trop lourd. Cependant, certains pensent que c'est la meilleure façon de travailler. Si c'est votre cas, je vous recommande d'essayer [RadRails](http://www.aptana.com/products/radrails) ou [RubyMine](http://www.jetbrains.com/ruby/index.html). Tout deux sont bien maintenus et possèdent de nombreuses intégrations par défaut. Maintenant, pour ceux comme moi qui comme moi préfère des outils simples, je peux vous dire qu'il y a beaucoup d'outils disponibles que vous pourrez personnaliser via des plugins et plus.

- **Éditeur de texte**: J'utilise personnellement [Vim](http://www.vim.org/) comme éditeur. Au cas où vous n'êtes pas un fan de Vim, il y a beaucoup d'autres solutions comme [Sublime Text](http://www.sublimetext.com/) qui est facile à prendre en main et surtout multi-plateforme . Il est fortement inspiré par [TextMate](http://macromates.com/). Une troisième option est d'utiliser un éditeur de texte plus récent comme [Atom](https://atom.io/) de [Github](http://gitub.com/). C'est un éditeur de texte prometteur fait en Javascript. Il est facile à personnaliser pour répondre à vos besoins. N'importe lequel des éditeurs que je viens de vous présenter fera le travail. Choisissez donc celui ou vous êtes le plus à l'aise.

- **Terminal**: Je ne suis pas un fan de l'application Terminal par défault sous Mac OS. Je recommande [iTerm2](http://www.iterm2.com/#/section/home), qui est un remplacement de terminal pour Mac OS. Si vous êtes sous Linux, vous avez probablement déjà un bon terminal.

### Navigateur web

Quand il s'agit de navigateurs, je conseillerai directement [Firefox](http://www.mozilla.org/en-US/firefox/new/). Mais d'autres développeurs utilisent [Chrome](https://www.google.com/intl/en/chrome/browser/) ou même [Safari](https://www.apple.com/safari/). N'importe lequel d'entre eux vous aidera à construire l'application que vous voulez. Ils proposent tous un bon inspecteur pour le DOM, un analyseur de réseau et de nombreuses autres fonctionnalités que vous connaissez peut-être déjà.


### Gestionnaire de paquets

- **Mac OS**: Il existe de nombreuses options pour gérer la façon dont vous installez les paquets sur votre Mac, comme [Mac Ports](https://www.macports.org/) ou [Homebrew](http://brew.sh/). Les deux sont de bonnes options, mais je choisirais la dernière. J'ai rencontré moins de problèmes lors de l'installation de logiciels avec Homebrew. Pour installer `brew` il suffit d'exécuter la commande ci-dessous:
~~~bash
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
~~~

- **Linux**: Vous êtes déjà prêts! Peu importe si vous utilisez `apt`, `pacman`, `yum` tant que vous vous sentez à l'aise et que vous savez comment installer des paquets.

### Git

Nous utiliserons beaucoup Git et vous devriez aussi l'utiliser (non seulement pour ce tutoriel mais aussi pour tout vos projets). Pour l'installer, c'est très facile:

- sous Mac OS: `$ brew install git`
- sous Linux: `$ sudo apt-get install git`

### Ruby

Il existe de nombreuses façons d'installer et de gérer Ruby. Vous devriez probablement déjà avoir une version installée sur votre système. Pour connaître votre version, tapez simplement:

~~~bash
$ ruby -v
~~~

Rails 5 nécessite l'installation de la version 2.2.2 ou supérieure. Pour l'installer, je vous recommande d'utiliser [Ruby Version Manager (RVM)](http://rvm.io/) ou [rbenv](http://rbenv.org/). Ces outils vous permettrons d'installer plusieurs versions de `ruby`. Dans ce tutoriel, nous allons utiliser RVM mais peu importe laquelle de ces deux options que vous utiliserez.

Pour installer RVM, rendez vous sur <https://rvm.io/> et installez la clé GPG[^gpg]. Une fois fais

[^gpg]: La clé GPG vous permet de vérifier l'iddentité de l'auteur des sources que vous téléchargez.

~~~bash
$ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
$ \curl -sSL https://get.rvm.io | bash
~~~


Ensuite, vous pouvez installer la dernière version de Ruby:

~~~bash
$ rvm install 2.5
~~~

Si tout s'est bien passé, il est temps d'installer le reste des dépendances que nous allons utiliser.

#### Gemmes, Rails et bibliothèques manquantes

Tout d'abord, nous mettons à jour les Gemmes sur l'ensemble du système:

~~~bash
$ gem update --system
~~~

Dans la plupart des cas, si vous êtes sous Mac OS, vous devriez installer des bibliothèques supplémentaires:

~~~bash
$ brew install libtool libxslt libksba openssl
~~~

Nous installons ensuite les gemmes nécessaires et ignorons la documentation pour chaque gemme:

~~~bash
$ printf 'gem: --no-document' >> ~/.gemrc
$ gem install bundler
$ gem install foreman
$ gem install rails -v 5.2
~~~

Vérifiez que tout fonctionne bien:

~~~bash
$ rails -v 5.2
5.2.0
~~~

#### Bases de données

Je vous recommande fortement d'installer [Postgresql](http://www.postgresql.org/) pour gérer vos bases de données. Mais ici, plus de simplicité, nous allons utiliser [SQlite](http://www.sqlite.org/). Si vous utilisez Mac OS vous n'avez pas de bibliothèques supplémentaire à installer. Si vous êtes sous Linux, ne vous inquiétez pas, je vous guide:

~~~bash
$ sudo apt-get install libxslt-dev libxml2-dev libsqlite3-dev
~~~

ou

~~~bash
$ sudo yum install libxslt-devel libxml2-devel libsqlite3-devel
~~~

## Initialisation du projet

Vous devez sans doute déjà savoir comment initialiser une application Rails. Si ce n'est pas le cas, jetez un coup d'œil à cette section.

Sachez que nous utiliserons [Rspec](http://rspec.info/) comme suite de test. Assurez-vous donc d'inclure l'option `--skip-test` lors de la création de l'application[^5] et l'option `--api`. L'option `--api` est apparue lors de la version 5 de Rails. Elle permet de limiter les librairies et *Middleware* inclue dans l'application. Cela permet aussi d'éviter de générer les vues HTML lors de l'utilisation des générateurs de Rails

La commande est donc la suivante

~~~bash
$ mkdir ~/workspace
$ cd ~/workspace
$ rails new market_place_api --skip-test --api
~~~

Comme vous pouvez le deviner, les commandes ci-dessus généreront les éléments indispensables à votre application Rails. La prochaine étape est d'ajouter quelques gemmes que nous utiliserons pour construire l'API.

### Installer Pow ou Prax

Vous pouvez vous demander

> Pourquoi diable voudrais-je installer ce type de paquet?

La réponse est simple. Nous allons travailler avec des [sous-domaines](http://en.wikipedia.org/wiki/Subdomain). [Pow](http://pow.cx/) et [Prax](https://github.com/ysbaddaden/prax.cr) vont nous aider a les créer très facilement.

#### Installer Pow

Pow ne fonctionne que sous Mac OS. Ne vous inquiétez pas, il existe une alternative qui imite les fonctionnalités sous Linux. Pour l'installer, tapez simplement:

~~~bash
$ curl get.pow.cx | sh
~~~

Et c'est tout ce que vous avez à faire. Il suffit d'établir un lien symbolique avec l'application pour configurer l'application Rack. D'abord vous allez dans le répertoire `~/.pow`:

~~~bash
$ cd ~/.pow
~~~

Ensuite, vous pouvez créer le [lien symbolique](http://en.wikipedia.org/wiki/Symbolic_link)

~~~bash
$ ln -s ~/workspace/market_place_api
~~~

N'oubliez pas de changer le répertoire utilisateur pour celui qui correspond au votre. Vous pouvez maintenant accéder à l'application via <http://market_place_api.dev/>. Votre application devrait être en cours d'exécution.

#### Installer Prax

Pour les utilisateurs de Linux uniquement, [Prax](https://github.com/ysbaddaden/prax.cr) distribue des paquets déjà compilé pour les distributions Debian / Ubuntu. Il suffit donc de télécharger le paquet `.deb` et de l'installer avec `dpkg`.

~~~bash
$ cd /tmp
$ wget https://github.com/ysbaddaden/prax.cr/releases/download/v0.8.0/prax_0.8.0-1_amd64.deb
$ sudo dpkg -i prax_0.8.0-1_amd64.deb
~~~

Ensuite, il ne nous reste plus qu'à lier les applications:

~~~bash
$ cd ~/workspace/market_place_api
$ prax link
~~~

Si vous voulez démarrer le prax server automatiquement, ajoutez cette ligne au fichier `.profile`:

~~~
prax start
~~~

Lors de l'utilisation de [Prax](https://github.com/ysbaddaden/prax.cr), vous devez spécifier le port de l'URL, dans ce cas-ci: <http://market_place_api.dev:3000>: Vous devriez voir l'application en marche comme le montre l'image suivante.

![L'application tourne sur l'URL http://market_place_api.dev/](img/pow_running.png)

Une fois l'application Rails créée, l'étape suivante consiste à ajouter une gemme simple (mais très puissante) pour sérialiser les ressources que nous allons exposer avec l'API. La gemme s'appelle `active_model_serializers`. C'est un excellent choix pour la construction de ce type d'application car la librairie est bien maintenue et la [documentation](https://github.com/rails-api/active_model_serializers) est incroyable.

Votre `Gemfile` devrait donc ressembler à ceci après avoir ajouté la gemme `active _model_serializers`:


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

# ...
~~~

Notez que j'enlève les gemmes `jbuilder` et `turbolinks` et `coffee-rails` car nous n'allons pas les utiliser.

C'est une bonne pratique aussi d'inclure la version Ruby utilisée sur l'ensemble du projet, ce qui empêche les dépendances de casser si le code est partagé entre différents développeurs, que ce soit pour un projet privé ou public.

Il est également important que vous mettiez à jour le *Gemfile* pour regrouper les différentes gemmes dans l'environnement correct:


~~~ruby
# Gemfile
# ...
group :development do
  gem 'sqlite3'
end
# ...
~~~

Ceci, comme vous vous en souvenez peut-être, empêchera l'installation ou l'utilisation de sqlite lorsque vous déployez votre application chez un fournisseur de serveurs comme Heroku[^heroku].

[^heroku]: Heroku facilite le déploiement de votre application en installant les dépendances sur un serveur en analysant votre *Gemfile*

Une fois cette configuration configurée, il est temps d'exécuter la commande d'installation du paquet pour intégrer les dépendances correspondantes:

~~~bash
$ bundle install
~~~

Une fois que la commande a terminé son exécution, il est temps de commencer à **versionner le projet** avec Git.

## Contrôle de version

Rappelez-vous que Git vous aide à suivre et à maintenir l'historique de votre code. Gardez à l'esprit que le code source de l'application est publié sur Github. Vous pouvez suivre le projet sur [Github][api_on_rails_git]

À ce stade, je suppose que vous avez déjà configuré Git et que vous êtes prêt à l'utiliser pour suivre le projet. Si ce n'est pas votre cas, initialisez simplement les paramètres basiques suivants:

~~~bash
$ git config --global user.name "Type in your name"
$ git config --global user.email "Type in your email"
$ git config --global core.editor "vim"
~~~

Il est donc temps d'initier le projet avec Git. N'oubliez pas de naviguer dans le répertoire racine de l'application `market_place_api`:

~~~bash
$ git init
Initialized empty Git repository in ~/workspace/market_place_api/.git/
~~~

L'étape suivante est d'ignorer certains fichiers que nous ne voulons pas suivre. Votre fichier `.gitignore` devrait ressembler à celui montré ci-dessous:

~~~
# Ignore bundler config.
/.bundle

# Ignore the default SQLite database.
/db/*.sqlite3
/db/*.sqlite3-journal

# Ignore all logfiles and tempfiles.
/log/*
/tmp/*
!/log/.keep
!/tmp/.keep

# Ignore uploaded files in development
/storage/*

/node_modules
/yarn-error.log

/public/assets
.byebug_history

# Ignore master key for decrypting credentials and more.
/config/master.key
~~~

Après avoir modifié le fichier *.gitignore*, il suffit d'ajouter les fichiers et de valider les modifications. Les commandes nécessaires sont indiquées ci-dessous:

~~~bash
$ git add .
$ git commit -m "Initial commit"
~~~

> Bonne pratique: J'ai appris que commencer un message par un verbe au présent décrit ce que fait le commit et non ce qu'il a fait. De cette façon il est plus facile de lire et de comprendre l'historique du projet (ou du moins pour moi). Je vais suivre cette pratique jusqu'à la fin du tutoriel.

Enfin, et c'est une étape optionnelle, nous déployons le projet sur **Github** (je ne vais pas l'expliquer ici) et poussons notre code vers le serveur distant. On commence donc par ajouter un serveur distant:

~~~bash
$ git remote add origin git@github.com:madeindjs/market_place_api.git
~~~

Ensuite on pousse le code:

~~~bash
$ git push -u origin master
~~~

Au fur et à mesure que nous avançons dans le tutoriel, j'utiliserai les pratiques que j'utilise quotidiennement. Cela inclut le travail avec les branches, le rebasage, le squash et bien d'autres. Vous n'avez pas à vous inquiéter si vous ne connaissez pas tous ces termes, je les expliquerai le temps venu.

## Conclusion

Cela a été un chapitre assez long. Si vous êtes arrivé ici, permettez-moi de vous féliciter. Les choses vont s'améliorer à partir de ce point. Commençons à mettre les mains dans le code!

---

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


---


# Présentation des utilisateurs

Dans le dernier chapitre, nous avons réussi à mettre en place les bases de la configuration de notre application. Nous avons même ajouté le *versioning* par le biais des en-têtes HTTP. Dans le chapitre [8](#chapter:5){reference-type="ref" reference="chapter:5"}, nous traiterons l'authentification des utilisateurs à l'aide de jetons d'authentification ainsi que la définition de permissions pour limiter l'accès aux utilisateurs connectés. Dans les chapitres à venir, nous relierons les produits aux utilisateurs et leur donnerons la possibilité de passer des commandes.

Vous pouvez cloner le projet jusqu'à ce point avec:

~~~bash
$ git clone https://github.com/madeindjs/market_place_api/tree/chapitre_2
~~~

Comme vous pouvez déjà l'imaginer, il existe de nombreuses solutions d'authentification pour Rails comme [AuthLogic](https://github.com/binarylogic/authlogic), [Clearance](https://github.com/thoughtbot/clearance) et [Devise](https://github.com/plataformatec/devise). Nous utiliserons le dernier (Encadré [\[box:devise\_for\_authentification\]](#box:devise_for_authentification){reference-type="ref"reference="box:devise_for_authentification"}), qui offre un excellent moyen d'intégrer non seulement l'authentification de base, mais de nombreux autres modules pour une utilisation ultérieure.

### Devise pour l'authentification

Devise est livré avec jusqu'à 10 modules pour la gestion de l'authentification :

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

Si vous n'avez jamais travaillé avec devise auparavant, je vous recommande de visiter [le répertoire Github](https://github.com/plataformatec/devise) et de lire la documentation. Vous y trouverez beaucoup de bons exemples.

Ce chapitre sera complet. Il sera peut-être long mais j'essaie d'aborder autant de sujets que possible et d'aborder les meilleures pratiques. N'hésitez pas à vous prendre un café et allons-y. A la fin de ce chapitre, vous aurez construit toute la logique des utilisateurs ainsi que la validations et la gestion des erreurs.

Nous voulons suivre ce chapitre, c'est donc un bon moment pour créer une nouvelle branche:

~~~bash
$ git checkout -b chapter3
~~~

Assurez-vous simplement d'être sur la branche `master` avant.

## Modèle d'utilisateur

Nous devons d'abord ajouter la gemme Devise au `Gemfile`

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

Lancez ensuite la commande `bundle install` pour installer la Gem. Une fois que la commande est terminée, nous devons lancer le générateur d'installation de Devise:

~~~bash
$ rails g devise:install
  create  config/initializers/devise.rb
  create  config/locales/devise.en.yml
  ...
~~~

Maintenant et si tout s'est bien passé, nous serons en mesure de générer le modèle `user` à l'aide du générateur de Devise:

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

A partir de maintenant, chaque fois que nous créons un modèle, le générateur va également créer un fichier d'usine pour ce modèle. Cela nous aidera à créer facilement des utilisateurs de tests et facilitera la rédaction de nos tests.

~~~ruby
# spec/factories/users.rb
FactoryBot.define do
  factory :user do

  end
end
~~~

Ensuite, nous allons migrer la base de données et préparer la base de
données de test.

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

Faisons maintenant un `commit` afin de garder un historique concis:

~~~bash
$ git add .
$ git commit -m "Adds devise user model"
~~~

## Notre premier test sur le modèle User

Nous allons ajouter quelques spécifications pour nous assurer que le modèle utilisateur répond aux attributs `email`, `password` et `password_confirmation` fournis par Devise. Pour des raisons de commodité, nous allons modifier le fichier d'usine des utilisateurs pour ajouter les attributs correspondants.

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

Une fois les attributs ajoutés, il est temps de tester notre modèle `User`.

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

Vue que nous avons préparé la base de données de test avec `rake db:test:prepare`, nous pouvons exécuter les tests:

~~~bash
$ bundle exec rspec spec/models/user_spec.rb
....

Finished in 0.03231 seconds (files took 0.81624 seconds to load)
4 examples, 0 failures
~~~

C'était facile! Nous devrions maintenant faire un `commit`:

~~~bash
$ git add .
$ git commit -am 'Adds user firsts specs'
~~~

## Construire les utilisateurs

Il est temps de faire notre premier point d'entrée. Nous allons juste commencer à construire l'action `show` pour l'utilisateur qui va afficher un utilisateur en JSON. Nous devons d'abord générer le `users_controller`, ajouter les tests correspondants et ensuite construire le code réel.

Tout d'abord, nous générons le contrôleur utilisateur:

~~~bash
$ rails generate controller users
~~~

Cette commande var créer le fichier `users_controller_spec.rb` (Listing [\[lst:add\_show\_test\_to\_users\_controller\_spec\]](#lst:add_show_test_to_users_controller_spec){reference-type="ref"reference="lst:add_show_test_to_users_controller_spec"}). Avant d'entrer dans le vif du sujet, il y a deux choses que nous voulons tester pour une API:

- La structure du JSON renvoyée par le serveur
- Le code de réponse HTTP renvoyée par le serveur (encadré 3.2).

### Les codes HTTP courants

Le premier chiffre du code d'état spécifie l'une des cinq classes de réponse. Le strict minimum pour un client HTTP est qu'il utilise une ces cinq classes. Voici une liste des codes HTTP couramment utilisés:

- `200`: Réponse standard pour les requêtes HTTP réussies. C'est généralement sur les requêtes GET
- `201`: La demande a été satisfaite et a donné lieu à la création d'une nouvelle ressource. Après les demandes de POST
- `204`: Le serveur a traité la requête avec succès, mais ne renvoie aucun contenu. Il s'agit généralement d'une requête DELETE réussie.
- `400`: La requête ne peut pas être exécutée en raison d'une mauvaise syntaxe. Peut arriver pour tout type de requête.
- `401`: Similaire au 403 Interdit, mais spécifiquement pour une utilisation lorsque l'authentification est requise et qu'elle a échoué ou n'a pas encore été fournie. Peut arriver pour tout type de requête.
- `404`: La ressource demandée n'a pas pu être trouvée mais peut être à nouveau disponible à l'avenir. Habituellement, concerne les requêtes GET
- `500`: Un message d'erreur générique, donné lorsqu'une condition inattendue a été rencontrée et qu'aucun autre message spécifique ne convient. 

Peut arriver pour tout type de requête.

Pour une liste complète des codes de réponse HTTP, consultez l'[article sur Wikipedia](https://fr.wikipedia.org/wiki/Liste_des_codes_HTTP).

Pour garder notre code bien découpé, nous allons créer quelques répertoires sous le répertoire des tests des contrôleurs afin d'être cohérent la configuration (Listing [\[lst:mkdir\_controllers\_api\]](#lst:mkdir_controllers_api){reference-type="ref"reference="lst:mkdir_controllers_api"}). Il existe aussi une autre convention qui utilise à la place du répertoire `controllers` un répertoire de `request` ou `integration`. Dans notre cas, je préfère rester cohérent avec le répertoire `app/controllers`.

~~~bash
$ mkdir -p spec/controllers/api/v1
$ mv spec/controllers/users_controller_spec.rb spec/controllers/api/v1
~~~

Après avoir créé les répertoires, nous devons changer le nom de la classe `UsersController` en `Api::V1::UsersController`. Le fichier doit ressembler à ça:

~~~ruby
# ...

RSpec.describe Api::V1::UsersController, type: :controller do

end
~~~

Maintenant, voici le fichier avec les tests crée:

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

Il suffit ensuite d'ajouter l'action à notre contrôleur (Listing [\[lst:add\_show\_to\_users\_controller\_spec\]](#lst:add_show_to_users_controller_spec){reference-type="ref"reference="lst:add_show_to_users_controller_spec"}). C'est extrêmement simple:

~~~ruby
# app/controllers/api/v1/users\_controller.rb
class  Api::V1::UsersController < ApplicationController
  def show
    render json: User.find(params[:id])
  end
end
~~~

Les codes HTTP courants Il est possible qu'il faille activer le module `Devise::Test::ControllerHelpers` au fichier `spec/rails_helper.rb` afin de charger les utilitaires Devise aux test. Si c'est le cas, ajouter la ligne suivante

~~~ruby
#  ...
RSpec.configure do |config|
  #  ...
  config.include Devise::Test::ControllerHelpers, type: :controller
  #  ...
end
~~~

Si vous exécutez les tests avec `bundle exec rspec spec/controllers` vous obtenez l'erreur suivante:

~~~bash
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

Ce type d'erreur est très courant lorsque vous générer vos ressources à la main! En effet, nous avons totalement oublié les routes. Alors ajoutons-les (Listing [\[lst:add\_route\_users\_show\]](#lst:add_route_users_show){reference-type="ref"reference="lst:add_route_users_show"}):

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

Vos tests devraient désormais passer:

~~~bash
$ bundle exec rspec spec/controllers
..

Finished in 0.02652 seconds (files took 0.47291 seconds to load)
2 examples, 0 failures
~~~

Comme d'habitude, après avoir ajouté un des fonctionnalités dont nous sommes satisfaits, nous faisons un `commit`:

~~~bash
$ git add .
$ git commit -m "Adds show action the users controller"
~~~

### Tester notre ressource avec cURL

Nous avons donc enfin une ressource à tester. Nous avons plusieurs solutions pour la tester. La première qui me vient à l'esprit est l'utilisation de cURL qui est intégré dans presque toutes les distributions Linux. Alors, essayons:

Rappelez-vous que notre URI de base est `api.market_place_api.dev`.

~~~bash
$ curl -H 'Accept: application/vnd.marketplace.v1' http://api.market_place_api.dev/users/1
~~~

En faisant cela nous obtenons une erreur et c'est normal: l'utilisateur n'existe pas. Créons-le avec le terminal:

~~~bash
$ rails console
Loading development environment (Rails 5.2.1)
2.5.3 :001 >  User.create email: "example@marketplace.com", password: "12345678", password_confirmation: "12345678"
~~~

Après avoir créé l'utilisateur, notre appel cURL devrait fonctionner:

~~~bash
$ curl -H 'Accept: application/vnd.marketplace.v1' \
http://api.market_place_api.dev/users/1
{"id":1,"email":"example@marketplace.com", ...
~~~

Et voilà! Vous avez maintenant une entrée d'API d'enregistrement d'utilisateur. Si vous avez des problèmes avec la réponse, vous devrez peut-être modifier le fichier `application_controller.rb` et ajouter la ligne suivante:

~~~ruby
# app/controllers/application_controller.rb

class ApplicationController < ActionController::API
  # Prevent CSRF attacks by raising an exception.
  # For APIs, you may want to use :null_session instead.
  protect_from_forgery with: :null_session
end
~~~

Comme suggéré même par Rails, nous devrions utiliser `null_session` pour empêcher les attaques CSFR. Je vous recommande fortement de le faire sinon les requêtes POST et PUT ne fonctionnerons pas. Après avoir mis à jour le fichier `application_controller.rb` c'est le moment de faire un `commit`:

~~~bash
$ git add .
$ git commit -m "Updates application controller to prevent CSRF exception from being raised"
~~~

### Créer les utilisateurs {#subsec:create_users}

Maintenant que nous avons une meilleure compréhension de la façon de construire des points d'entrée, il est temps d'étendre notre API. Une des fonctionnalités les plus importante est de laisser les utilisateurs créer un profil sur notre application. Comme d'habitude, nous allons écrire des tests avant d'implémenter notre code pour étendre notre suite de tests.

Assurez-vous que votre répertoire Git est propre et que vous n'avez pas de fichier en `staging`. Si c'est le cas, *committez*-les pour que nous puissions recommencer à zéro.

Commençons donc par écrire notre test tests en ajoutant une entrée pour créer un utilisateur sur le fichier `users_controller_spec.rb` :

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

Cela fait beaucoup de code (Listing [\[lst:add\_spec\_users\_controller\_create\]](#lst:add_spec_users_controller_create){reference-type="ref"reference="lst:add_spec_users_controller_create"}). Ne vous inquiétez pas, je vous explique tout:

- Nous devons valider les états sur lesquels l'enregistrement peut être valide ou invalide. Dans ce cas, nous utilisons les `context` pour réaliser ces scénarios.
- Au cas où tout se passe bien, nous devrions retourner un code HTTP 201[^12] ainsi que la représentation JSON de cet objet.
- En cas d'erreur, nous devons renvoyer un code HTTP 422[^13]. Nous retournons également un JSON représentant la raison pour laquelle la ressource n'a pas pu être sauvegardée.

A ce moment là, Les tests doivent échouer:

~~~bash
$ bundle exec rspec spec/controllers/api/v1/users_controller_spec.rb
.FFFFFF
~~~

Il est donc temps d'implémenter le code pour que nos tests reussissent (Listing [\[lst:add\_users\_controller\_create\]](#lst:add_users_controller_create){reference-type="ref"reference="lst:add_users_controller_create"})

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

Rappelez-vous qu'à chaque fois que nous ajoutons une entrée dans notre API il faut aussi ajouter cette action dans notre fichier `routes.rb`.

~~~ruby
# config/routes.rb

Rails.application.routes.draw do
  # ...
  resources :users, only: [:show, :create]
  # ...
end
~~~

Comme vous pouvez le constater, l'implémentation est assez simple. Nous avons également ajouté la méthode privée `user_params` pour protéger les assignation d'attributs en masse. Maintenant, nos tests devraient passer:

~~~bash
$ bundle exec rspec spec/controllers/api/v1/users_controller_spec.rb
.......

Finished in 0.05967 seconds (files took 0.4673 seconds to load)
7 examples, 0 failures

~~~

Oura! *Committons* les changements et continuons à construire notre application:

~~~bash
$ git add .
$ git commit -m "Adds the user create endpoint"

~~~

### Mettre à jour les utilisateurs

Le schéma de mise à jour des utilisateurs est très similaire à celui de la création (Section [6.3.2](#subsec:create_users){reference-type="ref"reference="subsec:create_users"}). Si vous êtes un développeur de Rails expérimenté, vous connaissez peut-être déjà les différences entre ces deux actions:

- L'action de mise à jour répond à une requête PUT/PATCH .
- Seul un utilisateur connecté devrait être en mesure de mettre à jour ses informations. Ce qui signifie que nous devrons forcer un utilisateur à s'authentifier. Nous en parlerons au chapitre 5.

Comme d'habitude, nous commençons par écrire nos tests (Listing [\[lst:add\_spec\_users\_controller\_update\]](#lst:add_spec_users_controller_update){reference-type="ref"reference="lst:add_spec_users_controller_update"}):

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

Pour que les tests réussissent, nous devons construire l'action de mise à jour sur le fichier `users_controller.rb` et ajouter la route au fichier `routes.rb`. Comme vous pouvez le voir, nous avons trop de code dupliqué, nous remanierons nos tests au chapitre 4.

Tout d'abord nous ajoutons l'action le fichier routes.rb:

~~~ruby
# config/routes.rb

Rails.application.routes.draw do
  # ...
  resources :users, only: [:show, :create, :update]
  # ...
end
~~~

Ensuite nous implémentons l'action de mise à jour sur le contrôleur
utilisateur et faisons passer nos tests:

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

Tous nos tests devraient maintenant passer:

~~~bash
$ bundle exec rspec spec/controllers/api/v1/users_controller_spec.rb
............

Finished in 0.08826 seconds (files took 0.47286 seconds to load)
12 examples, 0 failures
~~~

Vue que tout fonctionne, on effectue un `commit`:

~~~bash
$ git add .
$ git commit -m "Adds update action the users controller"
~~~

### Supprimer l'utilisateur

Jusqu'à présent, nous avons construit un tas d'actions sur le contrôleur des utilisateurs avec leurs tests mais ce n'est terminé. Il nous en manque juste une dernière qui est l'action de destruction. Créons donc le test (liste [\[lst:add\_spec\_users\_controller\_destroy\]](#lst:add_spec_users_controller_destroy){reference-type="ref"reference="lst:add_spec_users_controller_destroy"}):

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

Comme vous pouvez le voir, le test est très simple. Nous ne répondons qu'avec un statut de 204 qui signifie `No Content`[^14]. Nous pourrions aussi retourner un code d'état de 200, mais je trouve plus naturel de répondre sans rien dans ce cas car nous supprimons une ressource et une réponse réussie peut suffire.

La mise en œuvre de l'action de destruction est également assez simple:

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

N'oubliez pas d'ajouter l'action `destroy` dans le fichier `routes.rb`:

~~~ruby
# config/routes.rb

Rails.application.routes.draw do
  # ...
  resources :users, only: [:show, :create, :update, :destroy]
  # ...
end
~~~

Si tout est correct, vos tests devraient passer:

~~~bash
$ bundle exec rspec spec/controllers/api/v1/users_controller_spec.rb
.............

Finished in 0.09255 seconds (files took 0.4618 seconds to load)
13 examples, 0 failures
~~~

Rappelez-vous qu'après avoir apporté quelques modifications à notre code, il est de bonne pratique de les commettre afin que nous gardions un bon histoirique.

~~~bash
$ git add .
$ git commit -m "Adds destroy action to the users controller"
~~~

## Conclusion

Oh vous êtes là! Bien joué! Je sais que c'était probablement long mais n'abandonnez pas! Assurez-vous que vous comprenez chaque morceau de code, les choses vont s'améliorer, dans le chapitre 4, nous remanierons nos tests pour rendre le code plus lisible et plus maintenable. Alors restez avec moi!

---

# Factoriser les tests {#chapter:4}

Dans le chapitre [6](#chapter:3){reference-type="ref"reference="chapter:3"} nous avons mis en place des entrée de ressources utilisateur. Si vous avez sauté ce chapitre ou si vous n'avez pas tout compris, je vous recommande vivement de le regarder. Il couvre les premières bases des tests et c'est une introduction aux réponses JSON.

Vous pouvez cloner le projet à cette étape avec:

~~~bash
$ git clone --branch chapitre_3 https://github.com/madeindjs/market_place_api
~~~

Dans ce chapitre, nous allons factoriser nos spécifications de test en ajoutant des méthodes d'aide, supprimer le paramètre de format envoyé sur chaque requête et le faire à travers les en-têtes, et nous espérons construire une suite de test plus cohérente et évolutive.

Jetons donc un coup d'oeil au fichier `users_controller_spec.rb` (Listing [\[lst:users\_controller\_spec\_before\_factorization\]](#lst:users_controller_spec_before_factorization){reference-type="ref"reference="lst:users_controller_spec_before_factorization"}):

~~~ruby
# spec/controllers/api/v1/users_controller_spec.rb

# ...

RSpec.describe Api::V1::UsersController, type: :controller do
  before(:each) { request.headers['Accept'] = 'application/vnd.marketplace.v1' }

  describe 'GET #show' do
    before(:each) do
      @user = FactoryBot.create :user
      get :show, params: { id: @user.id, format: :json }
    end

    it 'returns the information about a reporter on a hash' do
      user_response = JSON.parse(response.body, symbolize_names: true)
      expect(user_response[:email]).to eql @user.email
    end

    it { expect(response.response_code).to eq(200) }
  end

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

  describe "DELETE #destroy" do
    before(:each) do
      @user = FactoryBot.create :user
      delete :destroy, params: { id: @user.id }, format: :json
    end

    it { expect(response.response_code).to eq(204) }
  end
end
~~~

Comme vous pouvez le voir, il y a beaucoup de code dupliqué. Deux possibilité de factorisation sont:

- La méthode `JSON.parse` peut être encapsulée sur une méthode.
- Le paramètre format est envoyé à chaque demande. Bien que ce ne soit pas une mauvaise pratique, il est préférable de gérer le type de réponse à l'aide des en-têtes.

Ajoutons donc une méthode pour gérer la réponse JSON. Mais avant de
continuer, et si vous avez suivi le tutoriel, vous savez peut-être que
nous créons une branche pour chaque chapitre. Alors faisons-le:

~~~bash
$ git checkout -b chapter4
~~~

## Refactorisation de la réponse JSON

De retour à notre factorisation, nous allons créer un fichier sous le répertoire `spec/support`. Actuellement, nous n'avons pas ce répertoire, alors créons-le:

~~~bash
$ mkdir spec/support
~~~

Ensuite, nous créons un fichier `request_helpers.rb` sous le répertoire `support` que nous venons de créer:

~~~bash
$ touch spec/support/request_helpers.rb
~~~

Il est temps d'extraire la méthode `JSON.parse` dans notre propre méthode de support (Listing [\[lst:create\_request\_helpers\]](#lst:create_request_helpers){reference-type="ref"reference="lst:create_request_helpers"}):

~~~ruby
# spec/support/request_helpers.rb
module Request
  module JsonHelpers
    def json_response
      @json_response ||= JSON.parse(response.body, symbolize_names: true)
    end
  end
end
~~~

Nous allons intégrer la méthode dans certains `modules` afin de garder notre code bien organisé. L'étape suivante consiste à mettre à jour le fichier `users_controller_spec.rb` pour utiliser la méthode. Un exemple rapide est présenté ci-dessous:

~~~ruby
# spec/controllers/api/v1/users_controller_spec.rb

# ...
it 'returns the information about a reporter on a hash' do
  user_response = json_response # c'est cette ligne qui est maj
  expect(user_response[:email]).to eql @user.email
end
# ...
~~~

C'est maintenant à votre tour de mettre à jour l'ensemble du fichier!

Si vous essayez maintenant d'exécuter vos tests avec `bundle exec rspec spec/controllers` vous allez avoir une erreur. C'est normal. La méthode `json_response` n'est pas chargée dans le fichier `rails_helper.rb`. Il faut donc modifier un peu notre `rails_helper` qui s'occupe de configurer nos tests:

~~~ruby
# spec/rails_helper.rb

# chargement de tous les fichiers Ruby dans le dossier spec/support
Dir[Rails.root.join('spec', 'support', '**', '*.rb')].each do |f|
  require f
end

RSpec.configure do |config|
  #  ...
  # Nous devons aussi inclure ces methodes dans rspec en tant
  # qu'aides de type controleur
  config.include Request::JsonHelpers, :type => :controller
  #  ...
end
~~~

Une fois le fichier modifié, nos tests devraient passer à nouveau! *Commitons* donc ceci avant d'aller plus loin:

~~~bash
$ git add .
$ git commit -m "Refactors the json parse method"
~~~

## Factoriser le paramètre du format

Nous voulons supprimer les paramètres `format: :json` envoyé sur chaque requête. Pour le faire c'est extrêmement facile. Il suffit simplement d'ajouter une ligne à notre fichier `users_controller_spec.rb`:

~~~ruby
# spec/controllers/api/v1/users_controller_spec.rb

RSpec.describe Api::V1::UsersController, type: :controller do
  before(:each) { request.headers['Accept'] = "application/vnd.marketplace.v1, application/json" }
~~~

En ajoutant cette ligne, vous pouvez maintenant supprimer tous les paramètres de `format` que nous envoyions sur chaque requête!

Attendez, ce n'est pas encore fini! Nous pouvons ajouter un autre en-tête à notre demande qui nous aidera à décrire les données que nous attendons du serveur à livrer. Nous pouvons y parvenir assez facilement en ajoutant une ligne supplémentaire spécifiant l'en-tête `Content-Type`:

~~~ruby
# spec/controllers/api/v1/users_controller_spec.rb

RSpec.describe Api::V1::UsersController, type: :controller do
  before(:each) { request.headers['Accept'] = "application/vnd.marketplace.v1, application/json" }
  before(:each) { request.headers['Content-Type'] = 'application/json' }
~~~

Et encore une fois,nous lançons nos tests pour voir si tout est bon:

~~~bash
$ bundle exec rspec spec/controllers/api/v1/users_controller_spec.rb
.............

Finished in 1.44 seconds (files took 0.4734 seconds to load)
13 examples, 0 failures
~~~

Et comme à chaque fois, c'est le bon moment pour `commit`:

~~~bash
$ git commit -am "Factorize format for unit tests"
~~~

## Factoriser le paramètre du format

Je suis vraiment satisfait du code que nous avons obtenu, mais nous pouvons faire encore mieux. La première chose qui me vient à l'esprit est de regrouper les 3 en-têtes personnalisés ajoutés avant chaque requête:

~~~ruby
# spec/controllers/api/v1/users_controller_spec.rb
#...

before(:each) { request.headers['Accept'] = "application/vnd.marketplace.v1, application/json" }
before(:each) { request.headers['Content-Type'] = 'application/json' }
~~~

C'est bien mais on peut mieux faire. En effet, nous devrons ajouter ces cinq lignes de code pour chaque fichier. Si pour une raison quelconque, nous changeons le type de réponse en XML, nous devrions modifier les cinq fichiers manuellement. Ne vous inquiétez pas, je vais vous proposer une solution qui résoudra tous ces problèmes.

Tout d'abord, nous devons étendre notre fichier `request_helpers.rb` pour inclure un autre module que j'ai nommé `HeadersHelpers` et qui aura les méthodes nécessaires pour gérer ces en-têtes personnalisés (Listing [\[lst:add\_headers\_to\_request\_helpers\]](#lst:add_headers_to_request_helpers){reference-type="ref"reference="lst:add_headers_to_request_helpers"})

~~~ruby
# spec/support/request_helpers.rb

module Request
  # ...

  module HeadersHelpers
    def api_header(version = 1)
      request.headers['Accept'] = "application/vnd.marketplace.v#{version}"
    end

    def api_response_format(format ='application/json')
      request.headers['Accept'] = "#{request.headers['Accept']}, #{format}"
      request.headers['Content-Type'] = format
    end

    def include_default_accept_headers
      api_header
      api_response_format
    end
  end
end
~~~

Comme vous pouvez le voir, j'ai divisé les appels en deux méthodes: une pour définir l'en-tête API et l'autre pour définir le format de réponse. J'ai aussi écrit une méthode (`include_default_accept_headers`) pour appeler les deux.

Et maintenant, pour appeler cette méthode avant chacun de nos test, nous pouvons ajouter le `before` dans le bloc `Rspec.configure` du fichier `rails_helper.rb` (Listing [\[lst:complete\_spec\_rails\_helper\]](#lst:complete_spec_rails_helper){reference-type="ref"reference="lst:complete_spec_rails_helper"}), et nous assurer de spécifier le type au `:controller` car nous ne le faisons que pour les tests unitaires concernant les controlleurs.

~~~ruby
# spec/rails_helper.rb

# ...

RSpec.configure do |config|
  # ...
  config.include Request::HeadersHelpers, :type => :controller
  config.before(:each, type: :controller) do
    include_default_accept_headers
  end

  # ...
end
~~~

Après avoir ajouté ces lignes, nous pouvons supprimer les `before` avant sur le fichier `users_controller_spec.rb` et vérifier que nos tests passent toujours.

Vous pouvez consulter la version complète du fichier `spec_helper.rb` ci-dessous (Listing [\[lst:complete\_spec\_rails\_helper\]](#lst:complete_spec_rails_helper){reference-type="ref"reference="lst:complete_spec_rails_helper"}):

~~~ruby
# spec/rails_helper.rb

require 'spec_helper'
ENV['RAILS_ENV'] ||= 'test'
require File.expand_path('../../config/environment', __FILE__)
# Prevent database truncation if the environment is production
abort("The Rails environment is running in production mode!") if Rails.env.production?
require 'rspec/rails'

Dir[Rails.root.join('spec', 'support', '**', '*.rb')].each { |f| require f }

begin
  ActiveRecord::Migration.maintain_test_schema!
rescue ActiveRecord::PendingMigrationError => e
  puts e.to_s.strip
  exit 1
end

RSpec.configure do |config|
  config.fixture_path = "#{::Rails.root}/spec/fixtures"

  config.use_transactional_fixtures = true

  config.include Devise::Test::ControllerHelpers, type: :controller
  config.include Request::JsonHelpers, :type => :controller
  config.include Request::HeadersHelpers, :type => :controller
  config.before(:each, type: :controller) do
    include_default_accept_headers
  end


  config.infer_spec_type_from_file_location!

  config.filter_rails_from_backtrace!
end
~~~

Et bien maintenant je suis satisfait du code. *Commitons* nos changements:

~~~bash
$ git commit -am "Refactors test headers for each request"
~~~

Rappelez-vous que vous pouvez revoir le code jusqu'à ce point dans le [dépôt github](https://github.com/madeindjs/market_place_api).

## Conclusion

Pour finir ce chapitre, bien qu'il ait été court, c'était une étape cruciale car cela nous aidera à écrire des tests plus rapides. Au chapitre [8](#chapter:5){reference-type="ref" reference="chapter:5"}, nous ajouterons le mécanisme d'authentification que nous utiliserons à travers l'application ainsi que la restriction de l'accès à certaines actions.

---

# Authentification des utilisateurs {#chapter:5}

Cela fait longtemps que vous avez commencé. J'espère que vous appréciez ce voyage autant que moi. Au chapitre 4, nous avons factorisé notre suite de tests et, comme nous n'avons pas ajouté beaucoup de code, cela n'a pas pris trop de temps. Si vous avez sauté ce chapitre, je vous recommande de le lire, car nous allons utiliser certaines méthodes dans les chapitres à venir.

Vous pouvez cloner le projet jusqu'ici:

~~~bash
$ git clone --branch chapitre_4 https://github.com/madeindjs/market_place_api
~~~

Dans ce chapitre, les choses vont devenir très intéressantes. Nous allons mettre en place notre mécanisme d'authentification. À mon avis, ce sera l'un des chapitres les plus intéressants car nous allons introduire beaucoup de nouveaux concepts. A la fin, vous aurez un système d'authentification simple mais puissant. Ne paniquez pas, nous y arriverons.

Commençons par le commencement. Comme d'habitude quand nous commençons un nouveau chapitre, nous allons créer une nouvelle branche:

~~~bash
$ git checkout -b chapter5
~~~

## Session sans état and sign in failure

Avant d'aller plus loin, quelque chose doit être clair: une API ne gère pas les sessions. Si vous n'avez pas d'expérience dans la création de ce genre d'applications, cela peut paraître un peu fou. Une API doit être sans état. Ce qui signifie par définition qu'il s'agit d'une API qui fournit une réponse après votre demande et qui ne nécessite aucune autre attention. Cela à pour conséquence qu'aucun état antérieur ou futur n'est nécessaire pour que le système fonctionne.

Le processus d'authentification de l'utilisateur via une API est très simple:

- Le client demande une ressource de sessions avec les informations d'identification correspondantes (généralement un e-mail et un mot de passe).
- Le serveur renvoie la ressource utilisateur avec son jeton d'authentification correspondant.
- Pour chaque page qui nécessite une authentification, le client doit envoyer ce jeton d'authentification.

Dans cette section et la suivante, nous nous concentrerons sur la construction d'un contrôleur de sessions avec ses actions correspondantes. Nous compléterons ensuite le flux de demandes en ajoutant l'accès d'autorisation nécessaire.

### Le jeton d'authentification

Avant de procéder à la logique sur le contrôleur de sessions, nous devons d'abord ajouter le champ du jeton d'authentification au modèle utilisateur. Ensuite, il faudra ajouter une méthode pour définir ce jeton.

Nous començons donc par générer un fichier de migration:

~~~bash
$ rails generate migration add_authentification_token_to_users auth_token:string
~~~

Comme une bonne pratique, j'aime configurer les valeurs `String` à une chaîne vide. Et dans ce cas, nous allons ajouter un index avec une condition d'unicité. Ainsi, nous garantissons qu'il n'y a pas d'utilisateurs avec le même jeton. Alors faisons-le:

~~~ruby
# db/migrate/20181114134521_add_authentification_token_to_users.rb
class AddauthentificationTokenToUsers < ActiveRecord::Migration[5.2]
  def change
    add_column :users, :auth_token, :string, default: ''
    add_index :users, :auth_token, unique: true
  end
end
~~~

Ensuite, nous exécutons les migrations pour ajouter le champ et préparer la base de données de test:

~~~bash
$ rake db:migrate
== 20181114134521 AddauthentificationTokenToUsers: migrating ====================
-- add_column(:users, :auth_token, :string, {:default=>""})
   -> 0.0004s
-- add_index(:users, :auth_token, {:unique=>true})
   -> 0.0010s
== 20181114134521 AddauthentificationTokenToUsers: migrated (0.0016s) ===========
~~~

Il est maintenant temps d'ajouter quelques tests de réponse et d'unicité à nos spécifications de modèle utilisateur (Listing [\[lst:add\_uniq\_test\_to\_user\_spec\]](#lst:add_uniq_test_to_user_spec){reference-type="ref"reference="lst:add_uniq_test_to_user_spec"}).

~~~ruby
RSpec.describe User, type: :model do
  # ...

  it { should respond_to(:auth_token) }
  it { should validate_uniqueness_of(:auth_token)}
end
~~~

Nous passons ensuite au fichier `user.rb` et ajoutons le code nécessaire pour faire passer nos tests:

~~~ruby
# app/models/user.rb

class User < ApplicationRecord
  validates :auth_token, uniqueness: true
  # ...
end
~~~

Maintenant nous allons implémenter une méthode qui générera un jeton d'authentification unique pour chaque utilisateur afin de les authentifier plus tard via l'API. Construisons donc d'abord les tests (Listing [\[lst:test\_token\_to\_user\_spec\]](#lst:test_token_to_user_spec){reference-type="ref"reference="lst:test_token_to_user_spec"}).

~~~ruby
# spec/models/user_spec.rb

RSpec.describe User, type: :model do
  # ...

  describe "#generate_authentification_token!" do
    it "generates a unique token" do
      @user.generate_authentification_token!
      expect(@user.auth_token).not_to be_nil
    end

    it "generates another token when one already has been taken" do
      existing_user = FactoryBot.create(:user, auth_token: "auniquetoken123")
      @user.generate_authentification_token!
      expect(@user.auth_token).not_to eql existing_user.auth_token
    end
  end
end
~~~

Afin d'utiliser la méthode `validate_uniqueness_of`, il faut installer la gemme `shoulda-matchers`. Pour cela, nous rajoutons la gemme dans le `Gemfile`:

~~~ruby
# Gemfile

# ...
group :test do
  # ...
  gem 'shoulda-matchers'
end
~~~

Et nous la chargeons dans le fichier `rails_helper.rb`:

~~~ruby
# spec/rails_helper.rb

# ...
RSpec.configure do |config|
  # ...
  RSpec.configure do |config|
    config.include(Shoulda::Matchers::ActiveModel, type: :model)
    config.include(Shoulda::Matchers::ActiveRecord, type: :model)
  end
end
~~~

Les tests échouent comme prévu:

~~~bash
$ bundle exec rspec spec/models/user_spec.rb
.......FF

Failures:

  1) User#generate_authentification_token! generates a unique token
     Failure/Error: @user.generate_authentification_token!

     NoMethodError:
       undefined method `generate_authentification_token!' for #<User:0x0000558948d23760>
     # ./spec/models/user_spec.rb:23:in `block (3 levels) in <top (required)>'

  2) User#generate_authentification_token! generates another token when one already has been taken
     Failure/Error: @user.generate_authentification_token!

     NoMethodError:
       undefined method `generate_authentification_token!' for #<User:0x0000558948d18720>
     # ./spec/models/user_spec.rb:29:in `block (3 levels) in <top (required)>'
~~~

C'est normal, la méthode `generate_authentification_token` n'existe pas encore. Nous allons l'implémenter et l'accrocher à l'appel `before_create` pour garantir que chaque utilisateur a un jeton d'authentification. Pour créer le token il y a beaucoup de solutions. Je vais utiliser la méthode `friendly_token` qui conçoit déjà des jetons mais je pourrais aussi le faire avec la méthode `hex` de la classe
`SecureRandom`.

Le code pour générer le jeton est assez simple:

~~~ruby
# app/models/user.rb
class User < ApplicationRecord
  before_create :generate_authentification_token!
  # ...

  def generate_authentification_token!
    begin
      self.auth_token = Devise.friendly_token
    end while self.class.exists?(auth_token: auth_token)
  end
end
~~~

Après avoir fait cela, nous tests devraient passer:

~~~bash
$ bundle exec rspec spec/models/user_spec.rb
        .........

        Finished in 0.05079 seconds (files took 0.49029 seconds to load)
        9 examples, 0 failures

~~~

Comme d'habitude, nous *commitons* nos modifications:

~~~bash
$ git add .
$ git commit -m "Adds user authentification token"
~~~

### Le contrôleur de session

De retour au contrôleur de sessions. Les actions que nous allons implémenter seront gérées en tant que services RESTful: la connexion sera gérée par une demande POST à l'action `create` (Section 5.1.2.1) et la déconnexion par une demande DELETE à l'action `destroy` (Rappel pour la liste [\[desc:http\_verbs\]](#desc:http_verbs){reference-type="ref"reference="desc:http_verbs"} des verbes HTTP).

Pour commencer, nous allons commencer par créer le contrôleur de sessions:

~~~bash
$ rails generate controller sessions
~~~

Ensuite, nous devons déplacer les fichiers dans le répertoire `api/v1`, à la fois pour le dossier `app` mais aussi pour le dossier `spec`:

~~~bash
$ mv app/controllers/sessions_controller.rb app/controllers/api/v1
$ mv spec/controllers/sessions_controller_spec.rb spec/controllers/api/v1
~~~

Après avoir déplacé les fichiers, nous devons les mettre à jour pour qu'ils correspondent à la structure des répertoires que nous avons actuellement. Comme le montrent les listes [\[lst:add\_namespace\_for\_sessions\_controller\]](#lst:add_namespace_for_sessions_controller){reference-type="ref"reference="lst:add_namespace_for_sessions_controller"} et [\[lst:add\_namespace\_for\_sessions\_controller\_spec\]](#lst:add_namespace_for_sessions_controller_spec){reference-type="ref"reference="lst:add_namespace_for_sessions_controller_spec"}.

~~~ruby
# app/controllers/api/v1/sessions_controller.rb
class Api::V1::SessionsController < ApplicationController
end
~~~

~~~ruby
# spec/controllers/api/v1/sessions_controller_spec.rb
# ...

RSpec.describe Api::V1::SessionsController, type: :controller do
end
~~~

#### Connexion réussie

Notre premier arrêt sera l'action de création. Mais d'abord, et comme d'habitude, générons nos tests (Listing 5.5).

~~~ruby
# spec/controllers/api/v1/sessions_controller_spec.rb
# ...

RSpec.describe Api::V1::SessionsController, type: :controller do
  describe 'POST #create' do
    before(:each) do
      @user = FactoryBot.create :user
    end

    context 'when the credentials are correct' do
      before(:each) do
        post :create, params: {
          session: { email: @user.email, password: '12345678' }
        }
      end

      it 'returns the user record corresponding to the given credentials' do
        @user.reload
        expect(json_response[:auth_token]).to eql @user.auth_token
      end

      it { expect(response.response_code).to eq(200) }
    end

    context 'when the credentials are incorrect' do
      before(:each) do
        post :create, params: {
          session: { email: @user.email, password: 'invalidpassword' }
        }
      end

      it 'returns a json with an error' do
        expect(json_response[:errors]).to eql 'Invalid email or password'
      end

      it { expect(response.response_code).to eq(422) }
    end
  end
end
~~~

Les tests sont assez simples. Nous renvoyons simplement l'utilisateur au format JSON si les informations d'identification sont correctes, sinon nous envoyons simplement un JSON avec le message d'erreur.

Nous devons maintenant implémenter le code pour que nos tests passent (Listing [\[lst:sessions\_controller\_create\]](#lst:sessions_controller_create){reference-type="ref"reference="lst:sessions_controller_create"}). Mais avant cela, nous ajouterons les routes à notre fichier `route.rb` (Listing [\[lst:add\_sessions\_routes\]](#lst:add_sessions_routes){reference-type="ref"reference="lst:add_sessions_routes"}).

~~~ruby
# config/routes.rb
require 'api_constraints'

Rails.application.routes.draw do
  # ...
  resources :sessions, :only => [:create, :destroy]
end
~~~

~~~ruby
# app/controllers/api/v1/sessions_controller.rb
class Api::V1::SessionsController < ApplicationController
  def create
    user_password = params[:session][:password]
    user_email = params[:session][:email]
    user = user_email.present? && User.find_by(email: user_email)

    if user.valid_password? user_password
      sign_in user
      user.generate_authentification_token!
      user.save
      render json: user, status: 200, location: [:api, user]
    else
      render json: { errors: 'Invalid email or password' }, status: 422
    end
  end
end
~~~

Avant d'exécuter nos tests, il est nécessaire d'ajouter les `helpers` dans le fichier `rails_helper.rb`:

~~~ruby
# spec/rails_helper.rb
require 'api_constraints'

RSpec.configure do |config|
  # ...
  config.include Devise::Test::ControllerHelpers, :type => :controller
end
~~~

Et maintenant nous pouvons lancer nos tests:

~~~bash
$ bundle exec rspec spec/controllers/api/v1/sessions_controller_spec.rb
        ....

        Finished in 0.06515 seconds (files took 0.49218 seconds to load)
        4 examples, 0 failures
~~~

Ourah! Faisons un `commit`:

~~~bash
$ git add .
$ git commit -m "Adds sessions controller create action"
~~~

#### Déconnexion

Nous avons maintenant l'entrée de l'API. Il est temps de construire un point de sortie. Vous vous demandez peut-être pourquoi, puisque nous ne gérons pas les sessions et qu'il n'y a rien à détruire. Et c'est bien vrai! Lors d'une destruction, nous devons mettre à jour le jeton d'authentification pour que ce dernier devienne inutile et ne puisse plus être utilisé.

Il n'est en fait pas nécessaire d'inclure ce point final, mais j'aime l'inclure pour expirer les jetons d'authentification.

Comme d'habitude, nous commençons par les tests (Listing [\[lst:add\_sessions\_controller\_spec\_for\_destroy\]](#lst:add_sessions_controller_spec_for_destroy){reference-type="ref"reference="lst:add_sessions_controller_spec_for_destroy"}):

~~~ruby
# spec/controllers/api/v1/sessions_controller_spec.rb
# ...

RSpec.describe Api::V1::SessionsController, type: :controller do
  # ...

  describe "DELETE #destroy" do

    before(:each) do
      @user = FactoryBot.create :user
      sign_in @user, store: false
      delete :destroy, params: { id: @user.auth_token }
    end

    it { expect(response.response_code).to eq(204) }

  end
end
~~~

Comme vous pouvez le voir le test est super simple! Maintenant nous avons juste besoin d'implémenter le code nécessaire pour faire passer nos tests (Listing [\[lst:add\_sessions\_controller\_for\_destroy\]](#lst:add_sessions_controller_for_destroy){reference-type="ref"reference="lst:add_sessions_controller_for_destroy"})

~~~ruby
# app/controllers/api/v1/sessions_controller.rb
class Api::V1::SessionsController < ApplicationController
  # ...
  def destroy
    user = User.find_by(auth_token: params[:id])
    user.generate_authentication_token!
    user.save
    head 204
  end
end
~~~

Ici, nous nous attendons à ce qu'un identifiant soit envoyé sur la requête (qui doit correspondre au jeton d'authentification de l'utilisateur).,Nous ajouterons une méthode `current_user` pour gérer cela plus facilement lors de la section 5. Pour l'instant, *commitons* notre avancée:

~~~bash
$ git add .
$ git commit -m "Adds destroy session action added"
~~~

## Utilisateur connecté

Si vous avez déjà travaillé avec [devise](https://github.com/plataformatec/devise), vous connaissez sûrement déjà les méthodes générées pour gérer l'authentification ou bien pour obtenir l'utilisateur connecté (voir la [documentation](https://github.com/plataformatec/devise#getting-started)).

Dans notre cas, nous allons remplacer la méthode `current_user` pour répondre à nos besoins. C'est-à-dire retrouver l'utilisateur gràce à son jeton d'authentification qui envoyé sur chaque requête. Laissez moi clarifier ce point.

Une fois que le client se connecte, l'API lui retourne son jeton d'authentification. A chaque fois que ce client demande une page protégée, nous devrons retrouver l'utilisateur à partir de ce jeton d'authentification que l'utilisateur aura passé en paramètre ou dans l'en-tête HTTP.

Dans notre cas, nous utiliserons l'en-tête HTTP `Authorization` qui est souvent utilisé pour ça. Personnellement, je le trouve que c'est la meilleur manière parce que cela donne un contexte à la requête sans polluer l'URL avec des paramètres supplémentaires.

Quand il s'agit de l'authentification, j'aime ajouter toutes les méthodes associées dans un fichier séparé. Il suffit ensuite d'inclure le fichier dans le `ApplicationController`. De cette façon, il est très facile à tester de manière isolée.

Créons-donc le fichier dans le répertoire `controllers/concerns`:

~~~bash
$ touch app/controllers/concerns/authenticable.rb
~~~

Ensuite, créons un répertoire `concerns` sous `spec/controllers/` et un
fichier `authenticable_spec.rb` pour nos tests d'authentification:

~~~bash
$ mkdir spec/controllers/concerns
$ touch spec/controllers/concerns/authenticable_spec.rb
~~~

Comme d'habitude, nous commençons par écrire nos tests. Dans ce cas,
notre méthode `current_user`, va chercher un utilisateur par le jeton
d'authentification dans l'en-tête HTTP `Authorization`.

~~~ruby
# spec/controllers/concerns/authenticable_spec.rb
# ...

class Authentication < ActionController::API
  include Authenticable
end


RSpec.describe Authenticable do
  let(:authentication) { Authentication.new }
  subject { authentication }

  describe "#current_user" do
    before do
      @user = FactoryBot.create :user
      request.headers["Authorization"] = @user.auth_token
      authentication.stub(:request).and_return(request)
    end
    it "returns the user from the authorization header" do
      expect(authentication.current_user.auth_token).to eql @user.auth_token
    end
  end
end
~~~

[^15]

Notre test doit échouer. Implémentons donc le code pour qu'il passe (Listing [\[lst:create\_authenticable\]](#lst:create_authenticable){reference-type="ref"reference="lst:create_authenticable"}):

~~~ruby
# app/controllers/concerns/authenticable.rb
module Authenticable
  # Devise methods overwrites
  def current_user
    @current_user ||= User.find_by(auth_token: request.headers['Authorization'])
  end
end
~~~

Maintenant notre test devrait passer:

~~~bash
$ bundle exec rspec spec/controllers/concerns/authenticable_spec.rb
.

Finished in 0.0149 seconds (files took 0.49496 seconds to load)
1 example, 0 failures
~~~

Nous n'avons plus qu'à inclure le module `Authenticable` dans la classe
`ApplicationController`:

~~~ruby
# app/controllers/application_controller.rb
class ApplicationController < ActionController::API
  # Prevent CSRF attacks by raising an exception.
  # For APIs, you may want to use :null_session instead.
  protect_from_forgery with: :null_session

  include Authenticable
end
~~~

Et maintenant il est temps de *commiter* nos changements:

~~~bash
$ git add .
$ git commit -m "Adds authenticable module for managing authentication methods"

~~~

## Authentification avec le jeton

L'autorisation joue un rôle important dans la construction des applications car, contrairement à l'authentification qui permet d'identifier l'utilisateur, l'autorisation nous aide à définir ce qu'il a le droit de faire.

Nous avons une route pour mettre à jour l'utilisateur mais il y a un problème: n'importe qui peut mettre à jour n'importe quel utilisateur. Dans cette section, nous allons mettre en œuvre une méthode qui exigera que l'utilisateur soit connecté afin d'empêcher tout accès non autorisé. Nous retournerons un message d'erreur JSON avec un code HTTP
correspondant.

Tout d'abord, nous allons ajouter quelques tests sur le fichier `authenticable_spec.rb` pour la méthode `authenticate_with_token` (Listing [\[lst:add\_authenticate\_with\_token\_test\]](#lst:add_authenticate_with_token_test){reference-type="ref"reference="lst:add_authenticate_with_token_test"}).

~~~ruby
# spec/controllers/concerns/authenticable_spec.rb
# ...

class Authentication < ActionController::API
  include Authenticable
end

RSpec.describe Authenticable do

  # ...

  describe '#authenticate_with_token' do
    before do
      @user = FactoryBot.create :user
      authentication.stub(:current_user).and_return(nil)
      response.stub(:response_code).and_return(401)
      response.stub(:body).and_return({ 'errors' => 'Not authenticated' }.to_json)
      authentication.stub(:response).and_return(response)
    end

    it 'render a json error message' do
      expect(json_response[:errors]).to eql 'Not authenticated'
    end

    it { expect(response.response_code).to eq(401) }
  end
end
~~~

Comme vous pouvez le voir, nous utilisons à nouveau la classe Authentification et nous écrasons la requête et la réponse pour traiter la réponse attendue du serveur. Il est maintenant temps d'implémenter le code pour faire passer nos tests (Listing [\[lst:add\_authenticate\_with\_token\]](#lst:add_authenticate_with_token){reference-type="ref"reference="lst:add_authenticate_with_token"}).

~~~ruby
# app/controllers/concerns/authenticable.rb
module Authenticable

  # ...

  def authenticate_with_token!
    unless current_user.present?
      render json: { errors: 'Not authenticated' },
             status: :unauthorized
    end
  end
end
~~~

A ce stade, nous venons de construire un mécanisme d'autorisation très simple pour empêcher les utilisateurs non signés d'accéder à l'API. Il suffit de mettre à jour le fichier `users\_controller.rb` avec la méthode `current_user` et d'empêcher l'accès avec la commande `authenticate_with_token!`!

*Commitons* ces changements et continuons d'avancer:

~~~bash
$ git commit -m "Adds the authenticate with token method to handle access to actions"
~~~

## Autoriser les actions

Il est maintenant temps de mettre à jour notre fichier `users_controller.rb` pour refuser l'accès à certaines actions. Nous allons aussi implémenter la méthode `current_user` sur l'action `update` et `destroy` afin de s'assurer que l'utilisateur qui est connecté ne sera capable que de mettre à jour que ses données et qu'il ne pourra supprimer que (et uniquement) son compte.

Nous allons commencer par l'action `update` (Listing [\[lst:use\_current\_user\_on\_users\_update\]](#lst:use_current_user_on_users_update){reference-type="ref"reference="lst:use_current_user_on_users_update"}). Nous n'irons plus chercher l'utilisateur par son identifiant mais par l' `auth_token` sur l'en-tête `Authorization` fourni par la méthode `current_user`.

~~~ruby
# app/controllers/api/v1/users_controller.rb
class Api::V1::UsersController < ApplicationController

  # ...

  def update
    # on change juste la methode ici
    user = current_user

    if user.update(user_params)
      render json: user, status: 200, location: [:api, user]
    else
      render json: { errors: user.errors }, status: 422
    end
  end

  # ...

end
~~~

Et comme vous pouvez vous y attendre, si nous exécutons les tests de
notre *controller* utilisateurs, ils devraient échouer:

~~~bash
$ rspec spec/controllers/api/v1/users_controller_spec.rb
.......FFFFF.

Failures:

  1) Api::V1::UsersController PUT/PATCH #update when is successfully updated renders the json representation for the updated user
     Failure/Error: if user.update(user_params)

     NoMethodError:
       undefined method 'update' for nil:NilClass

   ...
~~~

La solution est assez simple, il suffit d'ajouter l'en-tête `Authorization` à la requête:

~~~ruby
# spec/controllers/api/v1/users_controller_spec.rb

# ...

RSpec.describe Api::V1::UsersController, type: :controller do

  # ...

  describe 'PUT/PATCH #update' do
    context 'when is successfully updated' do
      before(:each) do
        @user = FactoryBot.create :user
        request.headers['Authorization'] = @user.auth_token
        patch :update, params: { id: @user.id, user: { email: 'newmail@example.com' } }, format: :json
      end

      # ...
    end

    context 'when is not created' do
      before(:each) do
        @user = FactoryBot.create :user
        request.headers['Authorization'] = @user.auth_token
        patch :update, params: { id: @user.id, user: { email: 'bademail.com' } }, format: :json
      end

      # ...
    end
  end

  # ...
end
~~~

Maintenant, les tests devraient passer. Mais attendez, quelque chose ne va pas, n'est-ce pas? Nous pouvons factoriser la ligne que nous venons d'ajouter et la mettre dans le module `HeadersHelpers` que nous construisons au chapitre [7](#chapter:4){reference-type="ref"reference="chapter:4"} (Listing [\[lst:add\_api\_authorization\_helper\]](#lst:add_api_authorization_helper){reference-type="ref"reference="lst:add_api_authorization_helper"}):

~~~ruby
# spec/support/request_helpers.rb

module Request
  # ...

  module HeadersHelpers
    # ...

    def api_authorization_header(token)
      request.headers['Authorization'] = token
    end
  end
end
~~~

Maintenant, chaque fois que nous avons besoin d'avoir l'utilisateur courant sur nos tests, nous appelons simplement la méthode `api_authorization_header`. Je vous laisse le faire avec `users_controller_spec.rb` pour la spécification de mise à jour:

~~~ruby
# spec/controllers/api/v1/users_controller_spec.rb

# ...

RSpec.describe Api::V1::UsersController, type: :controller do

  # ...

  describe 'PUT/PATCH #update' do
    context 'when is successfully updated' do
      before(:each) do
        @user = FactoryBot.create :user
        api_authorization_header @user.auth_token
        patch :update, params: { id: @user.id, user: { email: 'newmail@example.com' } }, format: :json
      end

      # ...
    end

    context 'when is not created' do
      before(:each) do
        @user = FactoryBot.create :user
        api_authorization_header @user.auth_token
        patch :update, params: { id: @user.id, user: { email: 'bademail.com' } }, format: :json
      end

      # ...
    end
  end

  # ...
end
~~~

Pour l'action `destroy`, nous ferons la même chose car nous devons juste nous assurer qu'un utilisateur est capable de se supprimer (Listing [\[lst:add\_current\_user\_to\_users\_destroy\]](#lst:add_current_user_to_users_destroy){reference-type="ref"reference="lst:add_current_user_to_users_destroy"}):

~~~ruby
# app/controllers/api/v1/users_controller.rb
class Api::V1::UsersController < ApplicationController

  # ...

  def destroy
    current_user.destroy
    head 204
  end

  # ...
end
~~~

Maintenant, pour le fichier de spécification et comme mentionné précédemment, nous avons juste besoin d'ajouter l'en-tête `api_authorization_header`:

~~~ruby
# spec/controllers/api/v1/users_controller_spec.rb

# ...

RSpec.describe Api::V1::UsersController, type: :controller do
  # ...

  describe 'DELETE #destroy' do
    before(:each) do
      @user = FactoryBot.create :user
      api_authorization_header @user.auth_token
      delete :destroy, params: { id: @user.id }
    end

    it { expect(response.response_code).to eq(204) }
  end
end
~~~

Tout nos tests devraient passer. La dernière étape de cette section consiste à ajouter les droits d'accès correspondants pour ces deux dernières actions.

Il est courant de simplement empêcher les actions sur lesquelles l'utilisateur effectue des actions sur le modèle lui-même. Dans ce cas l'action `update` et `destroy`.

Sur le `users_controller.rb` nous devons filtrer certaines de ces actions pour empêcher l'accès (Listing [\[lst:add\_before\_action\_to\_users\]](#lst:add_before_action_to_users){reference-type="ref"reference="lst:add_before_action_to_users"}).

~~~ruby
# app/controllers/api/v1/users_controller.rb
class Api::V1::UsersController < ApplicationController
  before_action :authenticate_with_token!, only: %i[update destroy]
  respond_to :json

  # ...
end
~~~

Nos tests devraient être encore bons. Et à partir de maintenant, chaque fois que nous voulons éviter qu'une action ne soit déclenchée, nous ajoutons simplement la méthode `authenticate_with_token!` sur un hook `before_action`.

*Commitons* tout ça:

~~~bash
$ git add .
$ git commit -m "Adds authorization for the users controller"
~~~

Enfin, nous terminerons le chapitre en remaniant la méthode `authenticate_with_token!`, C'est une petite amélioration mais cela rendra la méthode plus parlante. Vous comprendrez ce que je veux dire dans une minute (Listing [\[lst:add\_user\_signed\_in\]](#lst:add_user_signed_in){reference-type="ref"reference="lst:add_user_signed_in"}), mais avant tout, ajoutons quelques tests.

~~~ruby
# spec/controllers/concerns/authenticable_spec.rb
# ...

# ...

RSpec.describe Authenticable do
  # ...

  describe '#user_signed_in?' do
    context "when there is a user on 'session'" do
      before do
        @user = FactoryBot.create :user
        authentication.stub(:current_user).and_return(@user)
      end

      it { should be_user_signed_in }
    end

    context "when there is no user on 'session'" do
      before do
        @user = FactoryBot.create :user
        authentication.stub(:current_user).and_return(nil)
      end

      it { should_not be_user_signed_in }
    end
  end
end
~~~

Comme vous pouvez le voir, nous avons ajouté deux simples tests pour savoir si l'utilisateur est connecté ou non. Et comme je l'ai déjà dis, c'est juste pour la clarté visuelle. Mais continuons et ajoutons l'implémentation. (Liste [\[lst:add\_user\_signed\_in\]](#lst:add_user_signed_in){reference-type="ref"reference="lst:add_user_signed_in"}):

~~~ruby
# app/controllers/concerns/authenticable.rb
module Authenticable
  # ...

  def authenticate_with_token!
    unless user_signed_in?
      render json: { errors: 'Not authenticated' },
             status: :unauthorized
    end
  end

  def user_signed_in?
    current_user.present?
  end
end
~~~

Comme vous pouvez le voir, maintenant `authenticate_with_token!` est plus facile à lire non seulement pour vous mais pour aussi pour les autres développeurs qui rejoignerons le projet. Cette approche a également un avantage secondaire: si vous voulez modifier ou améliorer la façon de valider, vous pouvez simplement le faire sur la méthode `user_signed_in?`.

Maintenant, nos tests devraient être tous verts:

~~~bash
$ rspec spec/controllers/concerns/authenticable_spec.rb
.....

Finished in 0.07415 seconds (files took 0.702 seconds to load)
5 examples, 0 failures
~~~

*Commitons* tout ça:

~~~bash
$ git add .
$ git commit -m "Adds user_signed_in? method to know whether the user is logged in or not"
~~~

## Conclusion

Vous l'avez fait! Vous êtes à mi-chemin! Ce chapitre a été long et difficile, mais c'est un grand pas en avant sur la mise en place d'un mécanisme solide pour gérer l'authentification utilisateur et nous grattons même la surface pour de simples règles d'autorisation.

Dans le prochain chapitre, nous nous concentrerons sur la personnalisation de la sortie JSON pour l'utilisateur avec `active_model_serializers` et l'ajout d'un modèle de produit en donnant à l'utilisateur la possibilité de créer un produit et le publier pour la vente.

Produits des utilisateurs
=========================

Dans le chapitre précédent, nous avons implémenté le mécanisme d'authentification que nous allons utiliser tout au long de l'application. Pour l'instant, nous avons une implémentation très simple du modèle `User`, mais le moment de vérité est venu où nous allons personnaliser la sortie JSON et ajouter une deuxième ressource : les produits utilisateurs. Ce sont les éléments que l'utilisateur vendra dans l'application, et par conséquent sera directement associé. Si vous êtes familier avec Rails, vous savez peut-être déjà de quoi je parle. Mais pour ceux qui ne le savent pas, nous allons associer le modèle `User` au modèle `Product` en utilisant les méthodes `has_many` et `belongs_to` de *Active Record*. (voir figure [\[fig:data\_model\]](#fig:data_model){reference-type="ref"
reference="fig:data_model"}).

Dans ce chapitre, nous allons construire le modèle de `Product` à partir de zéro, l'associer à l'utilisateur et créer les entrées nécessaires pour que tout client puisse accéder aux informations.

Vous pouvez cloner le projet jusqu'à ce point:

~~~bash
$ git clone --branch chapter6 https://github.com/madeindjs/market_place_api
~~~

Avant de commencer,et comme d'habitude quand nous commençons de nouvelles fonctionnalités, nous créons une nouvelle branche:

~~~bash
$ git checkout -b chapter6
~~~

## Le modèle du produit

Nous commencerons d'abord par créer un modèle de `Product` puis nous y ajouterons quelques validations et enfin nous l'associons au modèle `User`. Comme le modèle `User`, le `Product` sera entièrement testé et sera automatiquement supprimé si l'utilisateur est supprimé.

### Les fondements du produit

Le modèle `Product` aura besoin de plusieurs champs: un attribut `price` pour le prix du produit, un booléen `published` pour savoir si le produit est prêt à être vendu ou non, un `title` pour définir un titre de produit sexy, et enfin et surtout un `user_id` pour associer ce produit particulier à un utilisateur. Comme vous le savez peut-être déjà, nous le générons avec la commande `rails generate`:

~~~bash
$ rails generate model Product title:string price:decimal published:boolean user_id:integer:index
    invoke  active_record
    create    db/migrate/20181218064350_create_products.rb
    create    app/models/product.rb
    invoke    rspec
    create      spec/models/product_spec.rb
    invoke      factory_bot
    create        spec/factories/products.rb
~~~

Comme vous pouvez le remarquer, nous avons également ajouté un `index` à l'attribut `user_id`. C'est une bonne pratique pour les clés d'association car cela optimise les requêtes de la base de données. Ce n'est pas obligatoire, mais je vous le recommande vivement.

Le fichier de migration devrait ressembler à ceci:

~~~ruby
# db/migrate/20181218064350_create_products.rb
class CreateProducts < ActiveRecord::Migration[5.2]
  def change
    create_table :products do |t|
      t.string :title
      t.decimal :price
      t.boolean :published
      t.integer :user_id

      t.timestamps
    end
    add_index :products, :user_id
  end
end
~~~

Notez que nous avons défini des valeurs par défaut pour tous les attributs à l'exception de `user_id`, de cette façon nous gardons un haut niveau de cohérence dans notre base de données car nous ne traitons pas beaucoup de valeurs `NULL`.

Ensuite, nous ajouterons quelques tests de base au modèle de produit. Nous nous assurerons simplement que l'objet répond aux champs que nous avons ajoutés, comme indiqué dans le listing [\[lst:create\_product\_spec\]](#lst:create_product_spec){reference-type="ref"reference="lst:create_product_spec"}:

~~~ruby
# spec/models/product_spec.rb
# ...

RSpec.describe Product, type: :model do
  let(:product) { FactoryBot.build :product }
  subject { product }

  it { should respond_to(:title) }
  it { should respond_to(:price) }
  it { should respond_to(:published) }
  it { should respond_to(:user_id) }
end
~~~

Il suffit ensuite de lancer les migrations:

~~~bash
$ rake db:migrate
~~~

Et maintenant nous pouvons nous assurer que les tests passent:

~~~bash
$ rspec spec/models/product_spec.rb
~~~

Bien que nos tests réussissent, nous avons besoin de faire un peu de travail au niveau de la *factory* des produits. Pour l'instant, tout est codé en dur. Comme vous vous en souvenez, nous avons utilisé un `Faker` pour falsifier les valeurs de nos modèles de tests (Listing [\[lst:user\_factory\_with\_attrs\]](#lst:user_factory_with_attrs){reference-type="ref"reference="lst:user_factory_with_attrs"}). Il est donc temps de faire de même avec le modèle de `Product`. Voir le listing [\[lst:create\_factories\_products\]](#lst:create_factories_products){reference-type="ref"reference="lst:create_factories_products"}:

~~~ruby
# spec/factories/products.rb
FactoryBot.define do
  factory :product do
    title { FFaker::Product.product_name }
    price { rand * 100 }
    published { false }
    user_id { 1 }
  end
end
~~~

Maintenant, chaque produit que nous créons ressemblera un peu plus à un vrai produit. Nous devons encore travailler sur le `user_id` tel quel, mais nous y reviendrons à la section [9.1.3](#subsec:user_has_products){reference-type="ref"reference="subsec:user_has_products"}.

### Validations des produits

Comme nous l'avons vu avec l'utilisateur, les validations sont une partie importante lors de la construction de tout type d'application. Cela nous permet d'empêcher toute donnée indésirable d'être enregistrée dans la base de données. Pour le produit, nous devons nous assurer, par exemple, que le prix est un nombre et qu'il n'est pas négatif.

Une autre chose importante à propos de la validation, lorsque l'on travaille avec des associations, est de valider que chaque `Product` a un `User`. Donc, dans ce cas, nous devons valider la présence de l'`user_id`. Dans Listing [\[lst:add\_presence\_to\_product\_test\]](#lst:add_presence_to_product_test){reference-type="ref"reference="lst:add_presence_to_product_test"} vous pouvez voir de quoi je parle.

~~~ruby
# spec/models/product_spec.rb
# ...

RSpec.describe Product, type: :model do
  # ...

  it { should validate_presence_of :title }
  it { should validate_presence_of :price }
  it { should validate_numericality_of(:price).is_greater_than_or_equal_to(0) }
  it { should validate_presence_of :user_id }
end
~~~

Il nous faut maintenant ajouter l'implémentation pour faire passer les
tests:

~~~ruby
# app/models/product.rb
class Product < ApplicationRecord
  validates :title, :user_id, presence: true
  validates :price, numericality: { greater_than_or_equal_to: 0 }, presence: true
end
~~~

Les tests passent désormais:

~~~bash
$ rspec spec/models/product_spec.rb
........

Finished in 0.04173 seconds (files took 0.74322 seconds to load)
8 examples, 0 failures
~~~

*Commitons* ces changements et continuons d'avancer:

~~~bash
$ git add .
$ git commit -m "Adds product model bare bones along with some validations"

~~~

### Liaison des produits et des utilisateurs {#subsec:user_has_products}

Dans cette section, nous allons construire l'association entre le produit et le modèle utilisateur. Nous avons déjà les champs nécessaires, nous avons donc juste besoin de mettre à jour quelques fichiers et nous serons prêts à commencer. Tout d'abord, nous devons modifier la *factory* de `Product` pour la relier à l'utilisateur. Alors comment faire?

~~~ruby
# spec/factories/products.rb
FactoryBot.define do
  factory :product do
    title { FFaker::Product.product_name }
    price { rand * 100 }
    published { false }
    user
  end
end
~~~

Comme vous pouvez le voir, nous venons de renommer l'attribut `user_id` en `user` et nous n'avons pas spécifié de valeur. FactoryBot est assez intelligent pour créer un objet `user` pour chaque produit et les associer automatiquement. Maintenant nous devons ajouter quelques tests pour l'association (Listing [\[lst:add\_should\_belong\_to\_user\]](#lst:add_should_belong_to_user){reference-type="ref"reference="lst:add_should_belong_to_user"}):

~~~ruby
# spec/models/product_spec.rb
# ...

RSpec.describe Product, type: :model do
  # ...

  it { should belong_to :user }
end
~~~

Comme vous pouvez le voir, le test que nous avons ajouté est très simple, grâce à la puissance des *shoulda-matchers*. Nous poursuivons la mise en œuvre maintenant:

~~~ruby
# app/models/product.rb
class Product < ApplicationRecord
  belongs_to :user
  #...
end
~~~

N'oubliez pas de faire le test que nous avons ajouté juste pour vous assurer que tout va bien:

~~~bash
$ rspec spec/models/product_spec.rb
.........

Finished in 0.08815 seconds (files took 0.75134 seconds to load)
9 examples, 0 failures
~~~

Actuellement, nous n'avons qu'une partie de l'association. Mais comme vous vous en doutez peut-être déjà, nous devons ajouter une association `has_many` au modèle `User`.

Tout d'abord, nous ajoutons le test sur le fichier `user_spec.rb`:

~~~ruby
# spec/models/user_spec.rb
# ...

RSpec.describe User, type: :model do
  # ...
  it { should have_many(:products) }
  # ...
end
~~~

L'implémentation sur le modèle utilisateur est extrêmement simple:

~~~ruby
# app/models/user.rb
class User < ApplicationRecord
  has_many :products
  # ...
end
~~~

Maintenant, si nous exécutons les tests de l'utilisateur, elles
devraient toutes être correctes:

~~~bash
$ rspec spec/models/user_spec.rb
..........

Finished in 0.08411 seconds (files took 0.74624 seconds to load)
10 examples, 0 failures
~~~

### Suppression en cascade

Ce que j'ai vu dans le code d'autres développeurs, lorsqu'ils travaillent avec des associations, c'est qu'ils oublient la destruction des dépendances entre les modèles. Ce que je veux dire par là, c'est que si un utilisateur est supprimé, les produits de l'utilisateur devraient l'être aussi.

Donc pour tester cette interaction entre les modèles, nous avons besoin d'un utilisateur avec un des produits. Puis, nous supprimerons cet utilisateur en espérant que les produits disparaissent avec lui. Une implémentation simple ressemblerait à ceci:

~~~ruby
products = user.products
user.destroy
products.each do |product|
  expect(Product.find(product.id)).to raise_error ActiveRecord::RecordNotFound
end
~~~

Nous sauvegardons d'abord les produits dans une variable pour un accès ultérieur, puis nous détruisons l'utilisateur et bouclons la variable des produits en nous attendant à ce que chacun des produits lance une exception. Tout mettre ensemble devrait ressembler au code dans Listing [\[lst:create\_destroy\_user\_spec\]](#lst:create_destroy_user_spec){reference-type="ref"reference="lst:create_destroy_user_spec"}:

~~~ruby
# spec/models/user_spec.rb
# ...

RSpec.describe User, type: :model do
  # ...

  describe '#products association' do
    before do
      @user.save
      3.times { FactoryBot.create :product, user: @user }
    end

    it 'destroys the associated products on self destruct' do
      products = @user.products
      @user.destroy
      products.each do |product|
        expect { Product.find(product.id) }.to raise_error ActiveRecord::RecordNotFound
      end
    end
  end
end
~~~

Le code nécessaire pour rendre le code sur Listing [\[lst:create\_destroy\_user\_spec\]](#lst:create_destroy_user_spec){reference-type="ref"reference="lst:create_destroy_user_spec"} à passer est juste une option sur la méthode d'association `has_many`:

~~~ruby
# app/models/user.rb
class User < ApplicationRecord
  has_many :products, dependent: :destroy
  # ...
end
~~~

Avec ce code ajouté, tous nos tests devraient passer:

~~~bash
$ rspec spec/
...........................................

Finished in 0.44188 seconds (files took 0.8351 seconds to load)
43 examples, 0 failures
~~~

*Commitons* ces changements et continuons d'avancer:

~~~bash
$ git add .
$ git commit -m "Finishes modeling the product model along with user associations"
~~~

## Point d'entrée pour nos produits

Il est maintenant temps de commencer à construire les points d'entrée des produits. Pour l'instant, nous allons juste construire cinq actions REST et certaines d'entre elles seront imbriquées dans la ressource utilisateur. Dans le prochain chapitre, nous allons personnaliser la sortie JSON en implémentant la gemme `active_model_serializers`.

Nous devons d'abord créer le `products_controller`, et nous pouvons facilement y parvenir avec la commande ci-dessous:

~~~bash
$ rails generate controller api/v1/products
~~~

La commande ci-dessus va générer pas mal de fichiers qui nous permettre de commencer à travailler rapidement. Ce que je veux dire par là, c'est qu'il va générer le contrôleur et les fichiers de test déjà *scopés* à la version 1 de l'API (Listing [\[lst:generate\_products\_controller\]](#lst:generate_products_controller){reference-type="ref"reference="lst:generate_products_controller"} et [\[lst:generate\_products\_controller\_spec\]](#lst:generate_products_controller_spec){reference-type="ref"reference="lst:generate_products_controller_spec"}).

~~~ruby
# app/controllers/api/v1/products_controller.rb
class Api::V1::ProductsController < ApplicationController
end

~~~

~~~ruby
# spec/controllers/api/v1/products_controller_spec.rb
# ...

RSpec.describe Api::V1::ProductsController, type: :controller do
end
~~~

En guise d'échauffement, nous allons commencer par construire l'action du `show` pour le produit.

### Action d'affichage d'un produit

Comme d'habitude, nous commençons par ajouter quelques test du contrôleur des produits (Listing [\[lst:products\_controller\_spec\_show\]](#lst:products_controller_spec_show){reference-type="ref"reference="lst:products_controller_spec_show"}). La stratégie ici est très simple, il suffit de créer un seul produit et de s'assurer que la réponse du serveur est celle que nous attendons.

~~~ruby
# spec/controllers/api/v1/products_controller_spec.rb
# ...

RSpec.describe Api::V1::ProductsController, type: :controller do
  describe 'GET #show' do
    before(:each) do
      @product = FactoryBot.create :product
      get :show, params: { id: @product.id }
    end

    it 'returns the information about a reporter on a hash' do
      product_response = json_response
      expect(product_response[:title]).to eql @product.title
    end

    it { expect(response.response_code).to eq(200) }
  end
end
~~~

Nous ajoutons ensuite le code pour faire passer le test:

~~~ruby
# app/controllers/api/v1/products_controller.rb
class Api::V1::ProductsController < ApplicationController
  def show
    render json: Product.find(params[:id])
  end
end
~~~

Attendez! N'exécutez pas encore les tests. N'oubliez pas que nous devons
ajouter la route au fichier `routes.rb`:

~~~ruby
# config/routes.rb
require 'api_constraints'

Rails.application.routes.draw do
  # ...
  namespace :api, defaults: { format: :json }, constraints: { subdomain: 'api' }, path: '/' do
    scope module: :v1, constraints: ApiConstraints.new(version: 1, default: true) do
      # ...
      resources :products, only: [:show]
    end
  end
end
~~~

Maintenant, on s'assure que les tests passent:

~~~bash
$ rspec spec/controllers/api/v1/products_controller_spec.rb
..

Finished in 0.05474 seconds (files took 0.75052 seconds to load)
2 examples, 0 failures
~~~

Comme vous pouvez déjà le constater, les tests et l'implémentation sont très simples. En fait, cela ressemble beaucoup à ce que nous avons fait pour les utilisateurs.

### Liste des produits

Il est maintenant temps de créer une entrée pour liste de produits, qui pourrait permettre d'afficher le catalogue de produits d'un marché par exemple. Pour ce point d'accès, nous n'exigeons pas que l'utilisateur soit connecté. Comme d'habitude, nous allons commencer à écrire quelques tests (Listing [\[lst:create\_index\_products\_controller\_spec\]](#lst:create_index_products_controller_spec){reference-type="ref"reference="lst:create_index_products_controller_spec"}):

~~~ruby
# spec/controllers/api/v1/products_controller_spec.rb
# ...

RSpec.describe Api::V1::ProductsController, type: :controller do

  # ...

  describe 'GET #index' do
    before(:each) do
      4.times { FactoryBot.create :product }
      get :index
    end

    it 'returns 4 records from the database' do
      products_response = json_response
      expect(products_response).to have(4).items
    end

    it { expect(response.response_code).to eq(200) }
  end
end

~~~

Attention, la méthode `have` que nous utilisons dance ce test par exemple:

~~~ruby
expect(products_response[:products]).to have(4).items
~~~

n'est plus disponible depuis Rspec 3.0. Il faut donc installer une librairie supplémentaire:

~~~ruby
# Gemfile
# ...
group :test do
  # ...
  gem 'rspec-collection_matchers', '~> 1.1'
end
~~~

Passons maintenant à la mise en œuvre, qui, pour l'instant, va être une triste méthode toutes classes (Listing [\[lst:create\_index\_products\_controller\]](#lst:create_index_products_controller){reference-type="ref"reference="lst:create_index_products_controller"}):

~~~ruby
# app/controllers/api/v1/products_controller.rb
class Api::V1::ProductsController < ApplicationController
  def index
    render json: Product.all
  end

  #...
end
~~~

Et n'oubliez pas, vous devez ajouter la route correspondante dans le fichier `config/routes.rb`:

~~~ruby
resources :products, only: %i[show index]
~~~

Dans les chapitres suivants, nous allons améliorer ce point d'entré et donner la possibilité de recevoir des paramètres pour les filtrer.

*Commitons* ces changements et continuons d'avancer:

~~~bash
$ git add .
$ git commit -m "Finishes modeling the product model along with user associations"
~~~

### Création des produits {#subsec:create_products}

Créer des produits est un peu plus délicat parce que nous aurons besoin d'une configuration supplémentaire pour donner une meilleure structure à ce point d'entré. La stratégie que nous suivrons est d'imbriquer les produits, dans les actions des utilisateurs. Ceci nous permettra d'avoir un point d'entrée plus descriptif comme `/users/:user_id/products`.

Notre premier arrêt sera donc le fichier `products_controller_spec.rb` (Listing [\[lst:create\_create\_products\_controller\_spec\]](#lst:create_create_products_controller_spec){reference-type="ref"reference="lst:create_create_products_controller_spec"}).

~~~ruby
# spec/controllers/api/v1/products_controller_spec.rb
# ...

RSpec.describe Api::V1::ProductsController, type: :controller do

  # ...

  describe 'POST #create' do
    context 'when is successfully created' do
      before(:each) do
        user = FactoryBot.create :user
        @product_attributes = FactoryBot.attributes_for :product
        api_authorization_header user.auth_token
        post :create, params: { user_id: user.id, product: @product_attributes }
      end

      it 'renders the json representation for the product record just created' do
        product_response = json_response
        expect(product_response[:title]).to eql @product_attributes[:title]
      end

      it { expect(response.response_code).to eq(201) }
    end

    context 'when is not created' do
      before(:each) do
        user = FactoryBot.create :user
        @invalid_product_attributes = { title: 'Smart TV', price: 'Twelve dollars' }
        api_authorization_header user.auth_token
        post :create, params: { user_id: user.id, product: @invalid_product_attributes }
      end

      it 'renders an errors json' do
        product_response = json_response
        expect(product_response).to have_key(:errors)
      end

      it 'renders the json errors on whye the user could not be created' do
        product_response = json_response
        expect(product_response[:errors][:price]).to include 'is not a number'
      end

      it { expect(response.response_code).to eq(422) }
    end
  end
end
~~~

Wow! Nous avons ajouté beaucoup de code. Si vous vous souvenez de la Section [6.3.2](#subsec:create_users){reference-type="ref"reference="subsec:create_users"}, les tests sont en fait les mêmes que ceux de la création de l'utilisateur exépté quelques changements mineurs. Rappelez-vous que nous avons cette route imbriquée, nous devons donc nous assurer d'envoyer le paramètre `user_id` à chaque requête, comme vous pouvez le voir sur:

~~~ruby
post :create, params: { user_id: user.id, product: @product_attributes }
~~~

De cette façon, nous pouvons voir l'utilisateur et lui créer un produit qui lui est associé. Mais attendez il y a mieux, si nous adoptons cette approche, nous pouvons augmenter la portée de notre mécanisme d'autorisation. Dans ce cas, si vous vous souvenez, nous avons construit la logique pour obtenir l'utilisateur à partir de l'en-tête `Authorization` et lui avons assigné une méthode `current_user`. C'est donc assez facile à mettre en place en ajoutant simplement l'en-tête d'autorisation dans la requête et en récupérant l'utilisateur à partir de celui-ci. Alors faisons-le (Listing [\[lst:create\_create\_products\_controller\]](#lst:create_create_products_controller){reference-type="ref"reference="lst:create_create_products_controller"}):

~~~ruby
# app/controllers/api/v1/products_controller.rb
class Api::V1::ProductsController < ApplicationController
  before_action :authenticate_with_token!, only: [:create]

  # ...

  def create
    product = current_user.products.build(product_params)
    if product.save
      render json: product, status: 201, location: [:api, product]
    else
      render json: { errors: product.errors }, status: 422
    end
  end

  private

  def product_params
    params.require(:product).permit(:title, :price, :published)
  end
end
~~~

Comme vous pouvez le voir, nous protégeons l'action de création avec la méthode `authenticate_with_token!`, et sur l'action `create` nous construisons le produit en associant l'utilisateur courant.

A ce stade, vous vous demandez peut-être s'il est vraiment nécessaire d'imbriquer l'action? Parce qu'en fait, nous n'utilisons pas vraiment le paramètre `user_id` fournis de l'URL. Vous avez tout à fait raison, mon seul argument ici est qu'avec cette approche, la route est beaucoup plus descriptive de l'extérieur, car nous disons aux développeurs que pour créer un produit, il nous faut un utilisateur.

Alors c'est vraiment à vous de décider comment vous voulez organiser vos routes et les exposer au monde. Ma façon n'est pas la seule et cela ne signifie pas non plus que c'est la bonne. En fait, je vous encourage à jouer avec différentes approches et choisir celle que vous trouvez le mieux.

Une dernière chose avant de faire vos tests: la route nécessaire:

~~~ruby
# config/routes.rb
require 'api_constraints'

Rails.application.routes.draw do
  devise_for :users
  # Api definition
  namespace :api, defaults: { format: :json }, constraints: { subdomain: 'api' }, path: '/' do
    scope module: :v1, constraints: ApiConstraints.new(version: 1, default: true) do
      resources :users, only: %i[show create update destroy] do
        resources :products, only: [:create]
      end
      resources :sessions, only: %i[create destroy]
      resources :products, only: %i[show index]
    end
  end
end
~~~

Si vous faites les tests maintenant, ils devraient tous passer:

~~~bash
$ rspec spec/controllers/api/v1/products_controller_spec.rb
.........

Finished in 0.21831 seconds (files took 0.75823 seconds to load)
9 examples, 0 failures
~~~

### Mise à jour des produits

J'espère que maintenant vous comprenez la logique pour construire les actions à venir. Dans cette section, nous nous concentrerons sur l'action de mise à jour qui fonctionnera de manière similaire à celle de création. Nous avons juste besoin d'aller chercher le produit dans la base de données et de le mettre à jour.

Nous ajoutons d'abord l'action aux routes pour ne pas oublier plus tard:

~~~bash
# config/routes.rb
require 'api_constraints'

Rails.application.routes.draw do
  devise_for :users
  # Api definition
  namespace :api, defaults: { format: :json }, constraints: { subdomain: 'api' }, path: '/' do
    scope module: :v1, constraints: ApiConstraints.new(version: 1, default: true) do
      resources :users, only: %i[show create update destroy] do
        resources :products, only: %i[create update]
      end
      resources :sessions, only: %i[create destroy]
      resources :products, only: %i[show index]
    end
  end
end
~~~

Avant de commencer à coder certains tests je veux juste préciser que, de la même manière que pour l'action `create`, nous allons délimiter le produit à l'utilisateur courant. Nous voulons nous assurer que le produit que nous mettons à jour appartient bien à l'utilisateur. Nous allons donc chercher ce produit dans l'association `user.products` fournie par *Active Record*.

Tout d'abord, nous ajoutons quelques tests (Listing [\[lst:update\_index\_products\_controller\_spec\]](#lst:update_index_products_controller_spec){reference-type="ref"reference="lst:update_index_products_controller_spec"}):

~~~ruby
# spec/controllers/api/v1/products_controller_spec.rb
# ...

RSpec.describe Api::V1::ProductsController, type: :controller do
  # ...

  describe 'PUT/PATCH #update' do
    before(:each) do
      @user = FactoryBot.create :user
      @product = FactoryBot.create :product, user: @user
      api_authorization_header @user.auth_token
    end

    context 'when is successfully updated' do
      before(:each) do
        patch :update, params: { user_id: @user.id, id: @product.id, product: { title: 'An expensive TV' } }
      end

      it 'renders the json representation for the updated user' do
        product_response = json_response
        expect(product_response[:title]).to eql 'An expensive TV'
      end

      it { expect(response.response_code).to eq(200) }
    end

    context 'when is not updated' do
      before(:each) do
        patch :update, params: { user_id: @user.id, id: @product.id, product: { price: 'two hundred' } }
      end

      it 'renders an errors json' do
        product_response = json_response
        expect(product_response).to have_key(:errors)
      end

      it 'renders the json errors on whye the user could not be created' do
        product_response = json_response
        expect(product_response[:errors][:price]).to include 'is not a number'
      end

      it { expect(response.response_code).to eq(422) }
    end
  end
end
~~~

Les tests peuvent paraître complexes, mais en jetant un coup d'oeil, ils sont presque identiques à ceux des utilisateurs (Listing [\[lst:add\_spec\_users\_controller\_update\]](#lst:add_spec_users_controller_update){reference-type="ref"reference="lst:add_spec_users_controller_update"}). La seule différence ici étant que les routes sont imbriquées comme nous l'avons vu à la Section [9.2.3](#subsec:create_products){reference-type="ref"reference="subsec:create_products"}. Nous devons donc envoyer le `user_id` comme paramètre.

Maintenant implémentons le code pour faire passer nos tests avec succès (Listing [\[lst:update\_index\_products\_controller\]](#lst:update_index_products_controller){reference-type="ref"reference="lst:update_index_products_controller"}):

~~~ruby
# app/controllers/api/v1/products_controller.rb
class Api::V1::ProductsController < ApplicationController
  before_action :authenticate_with_token!, only: %i[create update]

  # ...

  def update
    product = current_user.products.find(params[:id])
    if product.update(product_params)
      render json: product, status: 200, location: [:api, product]
    else
      render json: { errors: product.errors }, status: 422
    end
  end

  # ...
end
~~~

Comme vous pouvez le constater, l'implémentation est assez simple. Nous allons simplement récupéré le produit auprès de l'utilisateur connecté et nous le mettons simplement à jour. Nous avons également ajouté cette action au `before_action`, pour empêcher tout utilisateur non autorisé de mettre à jour un produit.

Si on lance les tests, ils devraient passer:

~~~bash
$ rspec spec/controllers/api/v1/products_controller_spec.rb
..............

Finished in 0.24404 seconds (files took 0.75973 seconds to load)
14 examples, 0 failures
~~~

### Suppression des produits

Notre dernier arrêt pour les route des produits, sera l'action `destroy`. Vous pouvez maintenant imaginer à quoi cela ressemblerait. La stratégie ici sera assez similaire à l'action de `create` et `update`. Ce qui signifie que nous allons imbriquer la route dans les ressources des utilisateurs, puis récupérer le produit auprès de l'association `user.products` et enfin le supprimer en retournant un code 204.

Recommençons par ajouter la route:

~~~ruby
# config/routes.rb
require 'api_constraints'

Rails.application.routes.draw do
  # ...
  namespace :api, defaults: { format: :json }, constraints: { subdomain: 'api' }, path: '/' do
    scope module: :v1, constraints: ApiConstraints.new(version: 1, default: true) do
      resources :users, only: %i[show create update destroy] do
        resources :products, only: %i[create update destroy]
      end
      # ...
    end
  end
end
~~~

Après cela, nous devons ajouter quelques tests comme indiqué dans le listing [\[lst:create\_destroy\_products\_controller\_spec\]](#lst:create_destroy_products_controller_spec){reference-type="ref"reference="lst:create_destroy_products_controller_spec"}:

~~~ruby
# spec/controllers/api/v1/products_controller_spec.rb
# ...

RSpec.describe Api::V1::ProductsController, type: :controller do
  # ...

  describe 'DELETE #destroy' do
    before(:each) do
      @user = FactoryBot.create :user
      @product = FactoryBot.create :product, user: @user
      api_authorization_header @user.auth_token
      delete :destroy, params: { user_id: @user.id, id: @product.id }
    end

    it { expect(response.response_code).to eq(204) }
  end
end
~~~

Maintenant, ajoutons simplement le code nécessaire pour faire passer les tests (Listing [\[lst:create\_destroy\_products\_controller\]](#lst:create_destroy_products_controller){reference-type="ref"reference="lst:create_destroy_products_controller"}):

~~~ruby
# app/controllers/api/v1/products_controller.rb
class Api::V1::ProductsController < ApplicationController
  before_action :authenticate_with_token!, only: %i[create update destroy]

  # ...

  def destroy
    product = current_user.products.find(params[:id])
    product.destroy
    head 204
  end

  # ...
end
~~~

Comme vous pouvez le voir, l'implémentation fait le travail en trois lignes. Nous pouvons lancer les tests pour nous assurer que tout est bon.

~~~bash
$ rspec spec/controllers/api/v1/products_controller_spec.rb
...............

Finished in 0.25959 seconds (files took 0.80248 seconds to load)
15 examples, 0 failures
~~~

Après cela, nous *commitons* les changements.

~~~bash
$ git add .
$ git commit -m "Adds the products create, update and destroy action nested on the user resources"
~~~

## Remplir la base de données

Avant de continuer avec plus de code, remplissons la base de données avec de fausses données. Nous avons des usines qui devraient faire le travail à notre place. Alors utilisons-les.

Tout d'abord, nous exécutons la commande de la console Rails à partir du Terminal:

~~~bash
$ rails console
~~~

Nous créons ensuite un tas d'objets produits avec la gemme FactoryBot:

~~~ruby
Loading development environment (Rails 5.2.1)
2.5.3 :001 > 20.times { FactoryBot.create :product }
~~~

Oups, vous avez probablement des erreurs qui se sont produites:

~~~bahs
Traceback (most recent call last):
        3: from (irb):1
        2: from (irb):1:in `times'
        1: from (irb):1:in `block in irb_binding'
NameError (uninitialized constant FactoryBot)
~~~

C'est parce que nous utilisons la console sur l'environnement de développement. Mais ça n'a pas de sens avec notre `Gemfile` qui ressemble actuellement à ceci:

~~~ruby
# Gemfile

# ...
group :test do
  gem 'factory_bot_rails'
  gem 'ffaker', '~> 2.10'
  gem 'rspec-collection_matchers', '~> 1.1'
  gem 'rspec-rails', '~> 3.8'
  gem 'shoulda-matchers'
end

~~~

Vous voyez où est le problème? Si vous faites attention, vous remarquerez que la gemme `factory_bot_rails` n'est disponible que pour l'environnement de test et non pour le développement. Cela peut être corrigé très rapidement:

~~~ruby
# Gemfile

# ...
group :development, :test do
  gem 'factory_bot_rails'
  gem 'ffaker', '~> 2.10'
end

group :test do
  # ...
end
~~~

Notez que nous avons déplacé la gemme `ffaker` vers le groupe partagé comme nous l'utilisons à l'intérieur des usines que nous décrivons plus haut. Lancez maintenant la commande `bundle` pour mettre à jour les bibliothèques. Alors construisez les produits que vous voulez comme ça:

~~~bash
$ rails console
Loading development environment (Rails 5.2.1)
2.5.3 :001 > 20.times { FactoryBot.create :product }

~~~

Désormais, vous pourrez créer n'importe quel objet à partir d'usines, comme les utilisateurs, les produits, les commandes, etc.

*commitons* les changements!

~~~bash
$ git add .
$ git commit -m "Updates test environment factory gems to work on development"
~~~

## Conclusion

Dans le chapitre suivant, nous allons nous concentrer sur la personnalisation de la sortie des modèles utilisateur et produit à l'aide de la gemme *active model serializers*. Elle nous permettra de filtrer facilement les attributs à afficher et à gérer les associations comme des objets embarqués par exemple.

---

# Modélisation du JSON {#chapter:7}

Dans le chapitre précédent, nous avons ajouté les produits à l'application et construit tous les routes nécessaires. Nous avons également associé un produit à un utilisateur et restreint certaines des actions de `products_controller`.

Maintenant, vous devriez être satisfait de tout ce travail. Mais nous avons encore du pain sur la planche. Actuellement, nous avons une sortie JSON qui n'est pas parfaite. La sortie JSON ressemble au Listing [\[lst:products\_json\]](#lst:products_json){reference-type="ref"reference="lst:products_json"} or nous voulons un sortie correspondante au Listing [\[lst:wanted\_products\_json\]](#lst:wanted_products_json){reference-type="ref"reference="lst:wanted_products_json"}.

~~~json
{
  "products": [
      {
          "id": 1,
          "title": "Tag Case",
          "price": "98.7761933800815",
          "published": false,
          "user_id": 1,
          "created_at": "2018-12-20T12:47:26.686Z",
          "updated_at": "2018-12-20T12:47:26.686Z"
      },
    ]
}
~~~

~~~ruby
{
  "products": [
      {
          "id": 1,
          "title": "Tag Case",
      },
    ]
}
~~~

Dans ce chapitre, nous allons personnaliser la sortie JSON en utilisant la gemme `active_model_serializers`. Pour plus d'informations vous pouvez consulter le [répertoire sur Github](https://github.com/rails-api/active_model_serializers). Je vais couvrir quelques points ici, de l'installation à l'implémentation, mais je vous recommande de jeter un œil à la documentation.

Vous pouvez cloner le projet jusqu'à ce point avec:

~~~bash
$ git clone https://github.com/madeindjs/market_place_api.git -b chapter6
~~~

Commençons une nouvelle branche pour ce chapitre:

~~~bash
$ git checkout -b chapter7
~~~

## Mise en place de la gemme

Si vous avez suivi le tutoriel depuis le début, vous devriez déjà avoir la gemme installée. Mais au cas où vous auriez atterri ici, je vais vous guider dans l'installation.

Ajoutez la ligne suivante à votre `Gemfile`:

~~~ruby
# Gemfile
# ...
gem 'active_model_serializers', '~> 0.10.8'
~~~

Exécutez la commande d'installation du paquet pour installer la gemme... et c'est tout! Vous devriez être prêt à continuer avec ce tutoriel.

## Sérialiser l'utilisateur

Nous devons d'abord ajouter un fichier `user_serializer`. Nous pouvons le faire manuellement, mais la gemme fournit une interface en ligne de commande pour le faire:

~~~bash
$ rails generate serializer user
  create  app/serializers/user_serializer.rb

~~~

Ceci a créé un fichier appelé `user_serializer.rb` sous le répertoire `app/serializers`, qui devrait ressembler à celui de Listing [\[lst:generated\_user\_serializer\]](#lst:generated_user_serializer){reference-type="ref"reference="lst:generated_user_serializer"}:

~~~ruby
# app/serializers/user_serializer.rb
class UserSerializer < ActiveModel::Serializer
  attributes :id
end

~~~

Nous devrions avoir des tests qui échouent. Essayez par vous même:

~~~bash
$ rspec spec/controllers/api/v1/users_controller_spec.rb
F.F....F.....

~~~

Si vous jetez un coup d'oeil rapide au `users_controller`, vous devez avoir quelque chose correspondant au listing [\[lst:add\_show\_to\_users\_controller\_spec\]](#lst:add_show_to_users_controller_spec){reference-type="ref"reference="lst:add_show_to_users_controller_spec"} pour l'action `show`. La documentation de la gemme explique ce qui vient de ce passer:

> Rails recherchera automatiquement un sérialiseur nommé `PostSerializer`, et s'il existe, l'utilisera pour sérialiser un `Post`.

Cela fonctionne aussi avec `respond_with`, qui utilise `to_json` sous le capot. Notez également que toutes les options passées pour rendre `:json` seront passées à votre sérialiseur et disponibles comme `@options` à l'intérieur.

Cela signifie que peu importe si nous utilisons la méthode `render json` ou `respond_with`. A partir de maintenant, Rails recherchera le sérialiseur correspondant en premier.

Maintenant, de retour aux tests, vous pouvez voir que pour une raison quelconque, la réponse n'est pas tout à fait ce que nous attendons. C'est parce que la gemme réduit la réponse que nous avions précédemment définie. Donc pour faire passer les tests, il suffit d'ajouter les attributs à sérialiser dans le `user_serializer.rb` et de mettre à jour le fichier `users_controller_spec.rb`:

~~~ruby
# app/serializers/user_serializer.rb
class UserSerializer < ActiveModel::Serializer
  attributes :id, :email, :created_at, :updated_at, :auth_token
end
~~~

Si vous faites les tests maintenant, ils devraient passer:

~~~bash
$ rspec spec/controllers/api/v1/users_controller_spec.rb
.............

Finished in 0.16712 seconds (files took 0.80637 seconds to load)
13 examples, 0 failures
~~~

*Commitons* ces changements et continuons d'avancer:

~~~bash
$ git add .
$ git commit -am "Adds user serializer for customizing the json output"
~~~

Nous pouvons également tester les objets du sérialiseur, comme indiqué dans la documentation, mais je vous laisse le soin de décider si vous voulez tester ou non.

## Sérialiser les produits

Maintenant que nous comprenons comment fonctionne la gemme de sérialisation, il est temps de personnaliser la sortie des produits. La première étape et comme pour l'utilisateur, nous avons besoin d'un sérialiseur de produit, alors faisons-le:

~~~bash
$ rails generate serializer product
    create  app/serializers/product_serializer.rb

~~~

Ajoutons maintenant les attributs à sérialiser pour le produit, comme nous l'avons fait avec l'utilisateur dans la Section [10.2](#sec:serialize_user){reference-type="ref"reference="sec:serialize_user"}:

~~~ruby
# app/serializers/product_serializer.rb
class ProductSerializer < ActiveModel::Serializer
  attributes :id, :title, :price, :published
end
~~~

Et voilà. Ce n'est pas plus compliqué que ça. Vous pouvez lancer les tests pour vérifier mais ils devraient encore être bons.

*Commitons* ces petits changements:

~~~bash
$ git add .
$ git commit -a "Adds product serializer for custom json output"
~~~

## Sérialiser les associations

Nous avons travaillé avec des sérialiseurs et vous remarquerez peut-être que c'est très simple. Dans certains cas, la décision difficile est de savoir comment nommer vos routes ou comment structurer la sortie JSON afin que votre solution soit pérenne.

Lorsque vous travaillez avec des associations entre les modèles sur une API, il existe de nombreuses approches que vous pouvez prendre. Ici, je vais expliquer ce que j'ai trouvé et ce fonctionne pour moi. Ce n'est pas la seule manière de faire, je vous laisse juger si elle vous convient. Dans cette section, nous allons étendre notre API pour gérer l'association produit/utilisateur. Je vais aussi vous expliquer certaines des erreurs courantes dans lesquels vous pouvez tomber.

Pour résumer, nous avons une association de type `has_many` entre l'utilisateur et le modèle de produit. Regardez le Listing [\[lst:user\_assocations\]](#lst:user_assocations){reference-type="ref"reference="lst:user_assocations"} et Listing [\[lst:product\_assocations\]](#lst:product_assocations){reference-type="ref"reference="lst:product_assocations"}.

~~~ruby
# app/models/user.rb
class User < ApplicationRecord
  has_many :products, dependent: :destroy
  # ...
end
~~~

~~~ruby
# app/models/product.rb
class Product < ApplicationRecord
  belongs_to :user
  # ...
end
~~~

C'est une bonne idée d'intégrer des modèles dans d'autres modèles dans d'autres modèle car cela évite au client de l'API d'exécuter plusieurs requêtes. Cela rendra la sortie un peu plus lourde mais lorsque vous récupérez de nombreux enregistrements, cela peut vous éviter un énorme goulet d'étranglement. Permettez-moi d'expliquer, à l'aide d'un cas d'utilisation, l'application réelle telle qu'elle est illustrée dans l'encadré [\[box:association\_embeded\]](#box:association_embeded){reference-type="ref"reference="box:association_embeded"}.

## Cas d'utilisation d'un objet incorporé dans une association

Imaginez un scénario où vous allez chercher les produits dans l'API, mais dans ce cas, vous devez afficher une partie des informations de l'utilisateur.

Une solution possible serait d'ajouter l'attribut `user_id` au `product_serializer` pour que nous puissions récupérer l'utilisateur correspondant plus tard. Cela peut sembler être une bonne idée, mais si vous vous souciez de la performance, ou si les transactions de votre base de données ne sont pas assez rapides, vous devriez reconsidérer cette approche. Vous devez comprendre que pour chaque produit que vous récupérez, vous allez devoir récupérer son utilisateur correspondant.

Face à ce problème, je suis venu avec deux alternatives possibles:

- Une bonne solution à mon avis est d'intégrer les identifiants des utilisateurs liés aux produits dans un attribut meta, donc nous avons une sortie JSON comme:
~~~json
{
  "meta": { "user_ids": [1,2,3] },
  "products": [
  //  ...
  ]
}
~~~
Cela peut nécessiter une configuration supplémentaire sur le terminal de l'utilisateur, afin que le client puisse récupérer ces utilisateurs à partir de ces `user_ids`.
- Une autre solution, et celle que j'utiliserai ici, est d'incorporer l'objet `user` dans l'objet `product`. Ce qui peut rendre la première requête un peu plus lente, mais de cette façon le client n'a pas besoin de faire une autre requête supplémentaire. Un exemple des résultats escomptés est présenté ci-dessous:
~~~json
{
  "products":
  [
      {
         "id": 1,
         "title": "Digital Portable System",
         "price": "25.0277354166289",
         "published": false,
         "user": {
            "id": 2,
            "email": "stephany@lind.co.uk",
            "created_at": "2014-07-29T03:52:07.432Z",
            "updated_at": "2014-07-29T03:52:07.432Z",
            "auth_token": "Xbnzbf3YkquUrF_1bNkZ"
          }
      }
      // ...
   ]
}
~~~

Donc, selon l'encadré [\[box:association\_embeded\]](#box:association_embeded){reference-type="ref"reference="box:association_embeded"}, nous allons incorporer l'objet utilisateur dans le produit. Commençons par ajouter quelques tests. Nous allons simplement modifier les tests des routes `Products#index` et `Products#show` Listing [\[lst:spec\_product\_assocations\]](#lst:spec_product_assocations){reference-type="ref"reference="lst:spec_product_assocations"}.

~~~ruby
# spec/controllers/api/v1/products_controller_spec.rb
# ...

RSpec.describe Api::V1::ProductsController, type: :controller do
  describe 'GET #show' do
    # ...

    it 'has the user as a embeded object' do
      expect(json_response[:user][:email]).to eql @product.user.email
    end
  end

  describe 'GET #index' do
    # ...

    it 'returns the user object into each product' do
      json_response.each do |product_response|
        expect(product_response[:user]).to be_present
      end
    end
  end

  # ...
end
~~~

L'implémentation est très simple, il suffit d'ajouter une ligne au sérialiseur du produit:

~~~ruby
# app/serializers/product_serializer.rb
class ProductSerializer < ActiveModel::Serializer
  attributes :id, :title, :price, :published
  has_one :user
end
~~~

Maintenant, tous les tests devraient passer:

~~~ruby
$ rspec spec
............................................................

Finished in 0.57068 seconds (files took 0.67788 seconds to load)
60 examples, 0 failures

~~~

### Récupérer les produits pour des utilisateurs

Maintenant, vous vous demandez peut-être si vous devriez intégrer les produits dans l'utilisateur (comme dans la section précédente). Même si cela peut sembler bien, cela peut entraîner de graves problèmes d'optimisation. Vous pourriez être en train de charger d'énormes quantités d'informations et il est vraiment facile de tomber dans le problème des **dépendances circulaires** qui boucle le programme jusqu'à épuiser la mémoire.

Mais ne vous inquiétez pas, tout n'est pas perdu, nous pouvons facilement résoudre ce problème en intégrant seulement les identifiants des produits dans l'utilisateur. Cela donnera à votre API une meilleure performance et cela évitera de charger des données supplémentaires. Dans cette section, nous allons donc étendre notre route de l'index des produits pour traiter un paramètre `product_ids` et formater la sortie JSON en conséquence.

Tout d'abord, nous nous assurons que le `product_ids` qu'il fait partie de l'objet sérialisé de l'utilisateur:

~~~ruby
# spec/controllers/api/v1/users_controller_spec.rb
# ...

RSpec.describe Api::V1::UsersController, type: :controller do
  describe 'GET #show' do
    # ...

    it 'has the product ids as an embeded object' do
      expect(json_response[:product_ids]).to eql []
    end
  end

  # ...
end
~~~

L'implémentation est très simple, comme décrit dans [la documentation](https://github.com/rails-api/active_model_serializers/blob/0-10-stable/docs/howto/add_relationship_links.md#links-as-an-attribute-of-a-resource) de la gemme `active_model_serializers`:

~~~ruby
# app/serializers/user_serializer.rb
class UserSerializer < ActiveModel::Serializer
  attribute :product_ids do
    object.products.map(&:id)
  end
  # ...
end
~~~

Nos tests devraient passer:

~~~bash
$ rspec spec/controllers/api/v1/users_controller_spec.rb
..............

Finished in 0.16791 seconds (files took 0.65902 seconds to load)
14 examples, 0 failures
~~~

Nous devons maintenant étendre l'action `index` depuis le `products_controller` pour qu'il puisse gérer le paramètre `product_ids` et afficher les enregistrements scopés. Commençons par ajouter quelques tests (Listing [\[lst:parameter\_products\_controller\_spec\]](#lst:parameter_products_controller_spec){reference-type="ref"reference="lst:parameter_products_controller_spec"}):

~~~ruby
# spec/controllers/api/v1/products_controller_spec.rb
# ...

RSpec.describe Api::V1::ProductsController, type: :controller do
  # ...

  describe 'GET #index' do
    before(:each) do
      4.times { FactoryBot.create :product }
      get :index
    end

    context 'when is not receiving any product_ids parameter' do
      before(:each) do
        get :index
      end

      it 'returns 4 records from the database' do
        expect(json_response).to have(4).items
      end

      it 'returns the user object into each product' do
        json_response.each do |product_response|
          expect(product_response[:user]).to be_present
        end
      end

      it { expect(response.response_code).to eq(200) }
    end

    context 'when product_ids parameter is sent' do
      before(:each) do
        @user = FactoryBot.create :user
        3.times { FactoryBot.create :product, user: @user }
        get :index, params: { product_ids: @user.product_ids }
      end

      it 'returns just the products that belong to the user' do
        json_response.each do |product_response|
          expect(product_response[:user][:email]).to eql @user.email
        end
      end
    end
  end

  # ...
end
~~~

Comme vous pouvez le voir dans Listing [\[lst:parameter\_products\_controller\_spec\]](#lst:parameter_products_controller_spec){reference-type="ref"reference="lst:parameter_products_controller_spec"}, nous venons d'envelopper l'action index dans deux contextes séparés: l'un qui recevra les `product_ids` et l'autre l'ancien que nous avions écris. 

Ajoutons le code nécessaire pour faire passer les tests:

~~~ruby
# app/controllers/api/v1/products_controller.rb
class Api::V1::ProductsController < ApplicationController
  before_action :authenticate_with_token!, only: %i[create update destroy]

  def index
    products = params[:product_ids].present? ? Product.find(params[:product_ids]) : Product.all
    render json: products
  end

  # ...
end
~~~

Comme vous pouvez le voir l'implémentation est super simple. Nous allons simplement chercher les produits avec le paramètre `product_ids` au cas où ils seraient présents, sinon nous les cherchons tous. Assurons-nous que les tests sont bons:

~~~bash
$ rspec spec/controllers/api/v1/products_controller_spec.rb
..................

Finished in 0.35027 seconds (files took 0.65369 seconds to load)
18 examples, 0 failures
~~~

*Commitons* nos changements:

~~~bash
$ git commit -am "Embeds the products_ids into the user serialiser and fetches the correct products from the index action endpoint"
~~~

Rechercher les produits
-----------------------

Dans cette dernière section, nous continuerons à renforcer l'action d'`Products#index` en mettant en place un mécanisme de recherche très simple pour permettre à n'importe quel client de filtrer les résultats. Cette section est facultative car elle n'aura aucun impact sur les modules de l'application. Mais si vous voulez pratiquer davantage avec le TDD, je vous recommande de compléter cette dernière étape.

J'utilise [Ransack](https://github.com/activerecord-hackery/ransack) pour construire des formulaires de recherche avancée extrêmement rapidement. Mais ici, comme le but est dapprendre et que la recherche que nous allons effectuer est très simple, je pense que nous pouvons construire un moteur de recherche à partir de zéro. Nous devons simplement considérer les critères par lesquels nous allons filtrer les attributs. Accrochez-vous bien à vos sièges, ça va être un voyage difficile.

Nous filtrerons donc les produits selon les critères suivants:

- Par titre
- Par prix
- Trier par date de création

Cela peut sembler court et facile, mais croyez-moi, cela vous donnera mal à la tête si vous ne le planifiez pas.

### Le mot-clé by

Nous allons créer un *scope* pour trouver les enregistrements qui correspondent à un motif particulier de caractères. Appelons-le `filter_by_title`. Ajoutons d'abord quelques tests (Listing [\[lst:product\_filter\_by\_title\_spec\]](#lst:product_filter_by_title_spec){reference-type="ref"reference="lst:product_filter_by_title_spec"}):

~~~ruby
# spec/models/product_spec.rb
# ...

RSpec.describe Product, type: :model do
  # ...

  describe '.filter_by_title' do
    before(:each) do
      @product1 = FactoryBot.create :product, title: 'A plasma TV'
      @product2 = FactoryBot.create :product, title: 'Fastest Laptop'
      @product3 = FactoryBot.create :product, title: 'CD player'
      @product4 = FactoryBot.create :product, title: 'LCD TV'
    end

    context "when a 'TV' title pattern is sent" do
      it 'returns the 2 products matching' do
        expect(Product.filter_by_title('TV')).to have(2).items
      end

      it 'returns the products matching' do
        expect(Product.filter_by_title('TV').sort).to match_array([@product1, @product4])
      end
    end
  end
end
~~~

Le test ici est de s'assurer que quel que soit le cas du titre envoyé, nous devons l'aseptiser afin de faire la comparaison appropriée. Dans notre cas nous utiliserons l'approche en minuscules. Implémentons le code nécessaire (Listing [\[lst:product\_filter\_by\_title\]](#lst:product_filter_by_title){reference-type="ref"reference="lst:product_filter_by_title"}):

~~~ruby
# app/models/product.rb
class Product < ApplicationRecord
  # ...
  scope :filter_by_title, lambda { |keyword|
    where('lower(title) LIKE ?', "%#{keyword.downcase}%")
  }
end
~~~

L'implémentation est suffisante pour que nos tests passent:

~~~bash
$ rspec spec/models/product_spec.rb
...........

Finished in 0.17178 seconds (files took 3.59 seconds to load)
11 examples, 0 failures
~~~

### Par prix

Pour filtrer par prix, les choses peuvent devenir un peu plus délicates. Nous allons briser la logique de filtrer par prix en deux méthodes différentes: l'une qui va chercher les produits plus grands que le prix reçu et l'autre qui va chercher ceux qui sont sous ce prix. De cette façon, nous garderons une certaine flexibilité et nous pouvons facilement tester les *scope*.

Commençons par construire les tests du *scope* `above_or_equal_to_price` (Listing [\[lst:product\_above\_or\_equal\_to\_price\_spec\]](#lst:product_above_or_equal_to_price_spec){reference-type="ref"reference="lst:product_above_or_equal_to_price_spec"}):

~~~ruby
# spec/models/product_spec.rb
# ...

RSpec.describe Product, type: :model do
  # ...

  describe '.above_or_equal_to_price' do
    before(:each) do
      @product1 = FactoryBot.create :product, price: 100
      @product2 = FactoryBot.create :product, price: 50
      @product3 = FactoryBot.create :product, price: 150
      @product4 = FactoryBot.create :product, price: 99
    end

    it 'returns the products which are above or equal to the price' do
      expect(Product.above_or_equal_to_price(100).sort).to match_array([@product1, @product3])
    end
  end
end
~~~

L'implémentation est très très simple (Listing [\[lst:product\_above\_or\_equal\_to\_price\]](#lst:product_above_or_equal_to_price){reference-type="ref"reference="lst:product_above_or_equal_to_price"}):

~~~ruby
# app/models/product.rb
class Product < ApplicationRecord
  # ...
  scope :above_or_equal_to_price, lambda { |price|
    where('price >= ?', price)
  }
end
~~~

L'implémentation est suffisante pour que nos tests passent:

~~~bash
$ rspec spec/models/product_spec.rb
............

Finished in 0.1566 seconds (files took 0.64782 seconds to load)
12 examples, 0 failures
~~~

Vous pouvez maintenant imaginer le comportement de la méthode opposée. Voici les tests:

~~~ruby
# spec/models/product_spec.rb
# ...

RSpec.describe Product, type: :model do
  # ...

  describe '.below_or_equal_to_price' do
    before(:each) do
      @product1 = FactoryBot.create :product, price: 100
      @product2 = FactoryBot.create :product, price: 50
      @product3 = FactoryBot.create :product, price: 150
      @product4 = FactoryBot.create :product, price: 99
    end

    it 'returns the products which are above or equal to the price' do
      expect(Product.below_or_equal_to_price(99).sort).to match_array([@product2, @product4])
    end
  end
end
~~~

Et l'implémentation (Listing [\[lst:product\_below\_or\_equal\_to\_price\]](#lst:product_below_or_equal_to_price){reference-type="ref"reference="lst:product_below_or_equal_to_price"}):

~~~ruby
# app/models/product.rb
class Product < ApplicationRecord
  # ...
  scope :below_or_equal_to_price, lambda { |price|
    where('price <= ?', price)
  }
end
~~~

Pour notre bien, faisons les tests et vérifions que tout est beau et vert:

~~~bash
$ rspec spec/models/product_spec.rb
.............

Finished in 0.18008 seconds (files took 0.6544 seconds to load)
13 examples, 0 failures
~~~

Comme vous pouvez le voir, nous n'avons pas eu beaucoup de problèmes. Ajoutons simplement une autre *scope* pour trier les enregistrements par date de dernière mise à jour. Dans le cas où le propriétaire des produits décide de mettre à jour certaines données, il veut toujours les trier par enregistrements les plus récents.

### Tri par date de création

Ce *scope* est très facile. Ajoutons d'abord quelques tests (Listing [\[lst:product\_recent\_spec\]](#lst:product_recent_spec){reference-type="ref"reference="lst:product_recent_spec"}):

~~~ruby
# spec/models/product_spec.rb
# ...

RSpec.describe Product, type: :model do
  # ...

  describe '.recent' do
    before(:each) do
      @product1 = FactoryBot.create :product, price: 100
      @product2 = FactoryBot.create :product, price: 50
      @product3 = FactoryBot.create :product, price: 150
      @product4 = FactoryBot.create :product, price: 99

      # we will touch some products to update them
      @product2.touch
      @product3.touch
    end

    it 'returns the most updated records' do
      expect(Product.recent).to match_array([@product3, @product2, @product4, @product1])
    end
  end
end
~~~

Et l'implémentation (Listing [\[lst:product\_recent\]](#lst:product_recent){reference-type="ref"reference="lst:product_recent"}):

~~~ruby
# app/models/product.rb
class Product < ApplicationRecord
  # ...

  scope :recent, lambda {
    order(:updated_at)
  }
end
~~~

Tous nos tests devraient passer:

~~~bash
$ rspec spec/models/product_spec.rb
.............

Finished in 0.18008 seconds (files took 0.6544 seconds to load)
13 examples, 0 failures
~~~

### Moteur de recherche

Maintenant que nous avons la base pour le moteur de recherche que nous utiliserons dans l'application, il est temps de mettre en œuvre une méthode de recherche simple mais puissante. Elle s'occupera de gérer toute la logique pour récupérer les enregistrements des produits.

La méthode consistera à enchaîner tous les `scope` que nous avons construits précédemment et à retourner le résultat. Commençons par ajouter quelques tests:

~~~ruby
# spec/models/product_spec.rb
# ...

RSpec.describe Product, type: :model do
  # ...

  describe '.search' do
    before(:each) do
      @product1 = FactoryBot.create :product, price: 100, title: 'Plasma tv'
      @product2 = FactoryBot.create :product, price: 50, title: 'Videogame console'
      @product3 = FactoryBot.create :product, price: 150, title: 'MP3'
      @product4 = FactoryBot.create :product, price: 99, title: 'Laptop'
    end

    context "when title 'videogame' and '100' a min price are set" do
      it 'returns an empty array' do
        search_hash = { keyword: 'videogame', min_price: 100 }
        expect(Product.search(search_hash)).to be_empty
      end
    end

    context "when title 'tv', '150' as max price, and '50' as min price are set" do
      it 'returns the product1' do
        search_hash = { keyword: 'tv', min_price: 50, max_price: 150 }
        expect(Product.search(search_hash)).to match_array([@product1])
      end
    end

    context 'when an empty hash is sent' do
      it 'returns all the products' do
        expect(Product.search({})).to match_array([@product1, @product2, @product3, @product4])
      end
    end

    context 'when product_ids is present' do
      it 'returns the product from the ids' do
        search_hash = { product_ids: [@product1.id, @product2.id] }
        expect(Product.search(search_hash)).to match_array([@product1, @product2])
      end
    end
  end
end
~~~

Nous avons ajouté un tas de code mais je vous assure que l'implémentation est très facile. Vous pouvez aller plus loin et ajouter quelques tests supplémentaires mais, dans mon cas, je n'ai pas trouvé cela nécessaire.

~~~ruby
# app/models/product.rb
class Product < ApplicationRecord
  # ...

  def self.search(params = {})
    products = params[:product_ids].present? ? Product.find(params[:product_ids]) : Product.all

    products = products.filter_by_title(params[:keyword]) if params[:keyword]
    products = products.above_or_equal_to_price(params[:min_price].to_f) if params[:min_price]
    products = products.below_or_equal_to_price(params[:max_price].to_f) if params[:max_price]
    products = products.recent(params[:recent]) if params[:recent].present?

    products
  end
end
~~~

Il est important de noter que nous retournons les produits en tant qu'objet `ActiveRelation` afin de pouvoir enchaîner d'autres méthodes en cas de besoin ou les paginer comme nous allons le voir dans les derniers chapitres. Il suffit de mettre à jour l'action `Product#index` pour récupérer les produits à partir de la méthode de recherche:

~~~ruby
# app/models/product.rb
# app/controllers/api/v1/products_controller.rb
class Api::V1::ProductsController < ApplicationController
  before_action :authenticate_with_token!, only: %i[create update destroy]

  def index
    render json: Product.search(params)
  end

  # ...
end
~~~

Nous pouvons exécuter l'ensemble de la suite de tests, pour nous assurer que l'application est en bonne santé jusqu'ici:

~~~bash
$ rspec spec
.......................................................................

Finished in 1.49 seconds (files took 6.53 seconds to load)
71 examples, 0 failures
~~~

*Commitons* ces changements:

~~~bash
$ git commit -am "Adds search class method to filter products"
~~~

## Conclusion

Jusqu'à présent, et grâce à la gemme [active\_model\_serializers](https://github.com/rails-api/active_model_serializers), c'était facile. Sur les chapitres à venir, nous allons commencer à construire le modèle `Order` qui associera les utilisateurs aux produits.

---

# Création des commandes {#chapter:8}

Dans le chapitre [10](#chapter:7){reference-type="ref"reference="chapter:7"} nous avons traité les associations entre les produit et les modèles utilisateurs. Nous avons aussi vu comment bien les sérialiser en optimisant afin de pouvoir *scaler*[^16] rapidement et facilement. Maintenant, il est temps de commencer à passer des commandes. Cela va être une situation plus complexe parce que nous allons gérer les associations entre 3 modèles. Nous devons être assez intelligents pour gérer la sortie JSON que nous fournissons.

Dans ce chapitre, nous allons faire plusieurs choses que j'énumère ci-dessous:

- Créer un modèle de commande avec les spécifications correspondantes
- Gérer l'association de sortie JSON entre l'utilisateur de la commande et les modèles de produits
- Envoyer un courriel de confirmation avec le récapitulatif de la commande

Maintenant que tout est clair, on peut commencer à travailler. Vous pouvez cloner le projet jusqu'à ce point avec:

~~~bash
$ git clone --branch chapter7 https://github.com/madeindjs/market_place_api
~~~

Créons une nouvelle branche afin de commencer à travailler:

~~~bash
$ git checkout -b chapter8
~~~

## Modélisation de l'ordre

Si vous vous souvenez de la Figure [\[fig:data\_model\]](#fig:data_model){reference-type="ref"reference="fig:data_model"}, le modèle `Order` est associé aux `User` et `Product`. C'est en fait très simple de gérer cela avec Rails. La partie délicate est quand vient la sérialisation de ces objets. J'en parlerai plus en détail à la section 8.3.

Commençons par créer le modèle de la commande:

~~~bash
$ rails generate model order user:references total:decimal
~~~

La commande ci-dessus va générer le modèle `Order`. Je profite de la méthode des `references` pour créer la clé étrangère correspondante pour que la commande appartienne à un utilisateur. Elle ajoute aussi la directive `belongs_to` dans le modèle des commandes. Migrons la base de données:

~~~bash
$ rake db:migrate
~~~

Il est maintenant temps de créer quelques tests dans le fichier `order_spec.rb`:

~~~ruby
# spec/models/order_spec.rb
# ...

RSpec.describe Order, type: :model do
  let(:order) { FactoryBot.build :order }
  subject { order }

  it { should respond_to(:total) }
  it { should respond_to(:user_id) }

  it { should validate_presence_of :user_id }
  it { should validate_presence_of :total }
  it { should validate_numericality_of(:total).is_greater_than_or_equal_to(0) }

  it { should belong_to :user }
end
~~~

L'implémentation est assez simple:

~~~ruby
# app/models/order.rb
class Order < ApplicationRecord
  belongs_to :user
  validates :total, numericality: { greater_than_or_equal_to: 0 }
  validates :total, presence: true
  validates :user_id, presence: true
end
~~~

Les tests devraient passer:

~~~ruby
$ rspec spec/models/order_spec.rb
......

Finished in 0.16229 seconds (files took 4.08 seconds to load)
6 examples, 0 failures
~~~

### Les commandes et les produits

Nous devons établir l'association entre la commande et le produit. Cela se fait avec une association de plusieurs à plusieurs car de nombreux produits seront placés sur plusieurs commandes et les commandes auront plusieurs produits. Dans ce cas, nous avons donc besoin d'un modèle supplémentaire qui joindra ces deux autres objets et mappera l'association appropriée. Générons ce modèle:

~~~bash
$ rails generate model placement order:references product:references
~~~

Migrons la base de données:

~~~bash
$ rake db:migrate
~~~

Ajoutons d'abord les tests de l'association des commandes aux produits:

~~~ruby
# spec/models/order_spec.rb
# ...

RSpec.describe Order, type: :model do
  # ...

  it { should have_many(:placements) }
  it { should have_many(:products).through(:placements) }
end
~~~

L'implémentation est la suivante:

~~~ruby
# app/models/order.rb
class Order < ApplicationRecord
  has_many :placements
  has_many :products, through: :placements
  # ...
end
~~~

Il est maintenant temps de se lancer dans l'association `product-placement`:

~~~ruby
# spec/models/product_spec.rb
# ...

RSpec.describe Product, type: :model do
  # ...
  it { should have_many(:placements) }
  it { should have_many(:orders).through(:placements) }
  # ...
end
~~~

Ajoutons le code pour le faire passer:

~~~ruby
# app/models/product.rb
class Product < ApplicationRecord
  belongs_to :user
  has_many :placements
  has_many :orders, through: :placements
  # ...
end
~~~

Enfin, et ce n'est pas le moins important, les tests de placement:

~~~ruby
# spec/models/placement_spec.rb
# ...

RSpec.describe Placement, type: :model do
  let(:placement) { FactoryBot.build :placement }
  subject { placement }

  it { should respond_to :order_id }
  it { should respond_to :product_id }

  it { should belong_to :order }
  it { should belong_to :product }
end

~~~

Si vous avez suivi le tutoriel jusqu'à présent, l'implémentation est déjà là, à cause du type de références que nous passons au générateur de commandes du modèle. Nous devrions ajouter l'option inverse au modèle de placement pour chaque appel aux `belongs_to`. Cela donne un petit coup de pouce lors du référencement de l'objet parent.

~~~ruby
# app/models/placement.rb
class Placement < ApplicationRecord
  belongs_to :order, inverse_of: :placements
  belongs_to :product, inverse_of: :placements
end
~~~

Et maintenant, lançons tous les tests des modèles afin de nous assurer
que tout est bon:

~~~bash
$ rspec spec/models
...........................................

Finished in 0.53127 seconds (files took 0.73125 seconds to load)
43 examples, 0 failures
~~~

Maintenant que tout est beau et vert, *commitons* les changements:

~~~bash
$ git add .
$ git commit -m "Associates products and orders with a placements model"

~~~

Commandes de l'utilisateur
--------------------------

Il nous manque juste une petite (mais très importante) partie qui est de relier l'utilisateur aux commandes. Alors faisons-le :

Ouvrez d'abord le fichier `user_model_spec.rb` pour ajouter les tests correspondants:

~~~ruby
# spec/models/user_spec.rb
# ...

RSpec.describe User, type: :model do
  # ...
  it { should have_many(:orders) }
  # ...
end
~~~

Et puis il suffit d'ajouter l'implémentation qui est très simple:

~~~ruby
# app/models/user.rb
class User < ApplicationRecord
  # ...
  has_many :orders, dependent: :destroy
  # ...
end
~~~

Vous pouvez exécuter les tests pour les deux fichiers, et ils devraient
tous passer:

~~~ruby
$ rspec spec/models/{order,user}_spec.rb
....................

Finished in 0.14279 seconds (files took 0.72848 seconds to load)
20 examples, 0 failures
~~~

*Commitons* ces changements et continuons d'avancer:

~~~ruby
$ git add .
$ git commit -m 'Adds user order has many relation'
~~~

## Exposer le modèle d'utilisateur

Il est maintenant temps de préparer le contrôleur de commandes à exposer la bonne commande. Si vous vous souvenez des chapitres précédents ou l'on avait utilisé [ActiveModelSerializers](https://github.com/rails-api/active_model_serializers) vous devez vous rappeler que c'était vraiment facile.

"Mais attendez, que sommes-nous censés exposer?" vous vous demandez peut-être. Vous avez raison! Définissons d'abord quelles actions nous allons mettre en place:

- Une action d'indexation pour récupérer les commandes des utilisateurs en cours
- Une action show pour récupérer une commande particulière de l'utilisateur courant
- Une action de création pour passer réellement la commande

Commençons par l'action `index`. Nous devons d'abord créer le contrôleur de commandes:

~~~ruby
$ rails g controller api/v1/orders
~~~

Jusqu'ici, et avant de commencer à taper du code, nous devons nous demander, "Est-ce que je dois laisser les routes de ma commande imbriqués dans le UsersController ou bien je dois les isoler?". La réponse est vraiment simple. Je dirais que cela dépend de la quantité d'informations dans ce cas en particulier que vous voulez exposer au développeur Pas du point de vue de la sortie JSON mais au niveau de l'URI.

Dans notre cas, je vais emboîter les routes, parce que j'aime donner ce type d'information aux développeurs. Je pense que cela donne plus de contexte à la requête elle-même.

Commençons par quelques tests:

~~~ruby
# spec/controllers/api/v1/orders_controller_spec.rb
# ...

RSpec.describe Api::V1::OrdersController, type: :controller do
  describe 'GET #index' do
    before(:each) do
      current_user = FactoryBot.create :user
      api_authorization_header current_user.auth_token
      4.times { FactoryBot.create :order, user: current_user }
      get :index, params: { user_id: current_user.id }
    end

    it 'returns 4 order records from the user' do
      expect(json_response).to have(4).items
    end

    it { expect(response.response_code).to eq(200) }
  end
end
~~~

Si nous exécutons la suite de tests maintenant, comme vous pouvez vous y attendre, les deux tests échoueront. C'est normal car nous n'avons même pas défini ni les bonnes routes ni l'action. Commençons donc par ajouter les routes:

~~~ruby
# config/routes.rb
# ...

Rails.application.routes.draw do
  # ...
  namespace :api, defaults: { format: :json }, constraints: { subdomain: 'api' }, path: '/' do
    scope module: :v1, constraints: ApiConstraints.new(version: 1, default: true) do
      resources :users, only: %i[show create update destroy] do
        # ...
        resources :orders, only: [:index]
      end
      # ...
    end
  end
end
~~~

Il est maintenant temps d'implémenter le contrôleur des commandes:

~~~ruby
# app/controllers/api/v1/orders_controller.rb
class Api::V1::OrdersController < ApplicationController
  before_action :authenticate_with_token!

  def index
    render json: current_user.orders
  end
end
~~~

Et maintenant nos tests devraient passer:

~~~bash
$ rspec spec/controllers/api/v1/orders_controller_spec.rb
..

Finished in 0.07943 seconds (files took 0.7232 seconds to load)
2 examples, 0 failures

~~~

Nous aimons nos commits très petits. Alors *commitons* dès maintenant:

~~~bash
$ git add .
$ git commit -m "Adds the show action for order"

~~~

### Afficher une seule commande

Comme vous pouvez déjà l'imaginer, cette route est très facile. Nous
n'avons qu'à mettre en place quelques configurations (routes, action du
contrôleur) et ce sera tout pour cette section.

Commençons par ajouter quelques tests:

~~~ruby
# spec/controllers/api/v1/orders_controller_spec.rb
# ...

RSpec.describe Api::V1::OrdersController, type: :controller do
  # ...

  describe 'GET #show' do
    before(:each) do
      current_user = FactoryBot.create :user
      api_authorization_header current_user.auth_token
      @order = FactoryBot.create :order, user: current_user
      get :show, params: { user_id: current_user.id, id: @order.id }
    end

    it 'returns the user order record matching the id' do
      expect(json_response[:id]).to eql @order.id
    end

    it { expect(response.response_code).to eq(200) }
  end
end
~~~

Ajoutons l'implémentation pour faire passer nos tests. Sur le fichier
`routes.rb` ajoutez l'action `show` aux routes des commandes:

~~~ruby
# config/routes.rb
# ...

Rails.application.routes.draw do
  # ...
  resources :orders, only: [:index, :show]
  # ...
end

~~~

Et l'implémentation devrait ressembler à ceci:

~~~ruby
# app/controllers/api/v1/orders_controller.rb
class Api::V1::OrdersController < ApplicationController
  # ...

  def show
    render json: current_user.orders.find(params[:id])
  end
end

~~~

Tous nos tests passent désormais:

~~~bash
$ rspec spec/controllers/api/v1/orders_controller_spec.rb
....

Finished in 0.12767 seconds (files took 0.73322 seconds to load)
4 examples, 0 failures

~~~

*Commitons* les changements et passons à l'action `Product#create`.

~~~bash
$ git add .
$ git commit -m "Adds the show action for order"

~~~

### Placement et commandes

Il est maintenant temps de donner la possibilité à l'utilisateur de passer quelques commandes. Cela ajoutera de la complexité à l'application, mais ne vous inquiétez pas, nous allons faire les choses une étape à la fois.

Avant de lancer cette fonctionnalité, prenons le temps de réfléchir aux implications de la création d'une commande dans l'application. Je ne parle pas de la mise en place d'un service de transactions comme [Stripe](https://stripe.com/) ou [Braintree](https://www.braintreepayments.com/) mais de choses comme:

- la gestion des produits en rupture de stock
- la diminution de l'inventaire de produits
- ajouter une certaine validation pour le placement de la commande pour s'assurer qu'il y a suffisamment de produits au moment où la commande est passée

On dirait qu'il reste un paquet de chose à faire mais croyez-moi: vous êtes plus près que vous ne le pensez et ce n'est pas aussi dur que ça en a l'air. Pour l'instant, gardons les choses simples et supposons que nous avons toujours assez de produits pour passer un nombre quelconque de commandes. Nous nous soucions juste de la réponse du serveur pour le moment.

Si vous vous rappelez le modèle de commande sur la section 8.1, nous avons besoin de 3 choses: un total pour la commande, l'utilisateur qui passe la commande et les produits pour la commande. Compte tenu de cette information, nous pouvons commencer à ajouter quelques tests:

~~~ruby
# spec/controllers/api/v1/orders_controller_spec.rb
# ...

RSpec.describe Api::V1::OrdersController, type: :controller do
  # ...

  describe 'POST #create' do
    before(:each) do
      current_user = FactoryBot.create :user
      api_authorization_header current_user.auth_token

      product_1 = FactoryBot.create :product
      product_2 = FactoryBot.create :product
      order_params = { total: 50, user_id: current_user.id, product_ids: [product_1.id, product_2.id] }
      post :create, params: { user_id: current_user.id, order: order_params }
    end

    it 'returns the just user order record' do
      expect(json_response[:id]).to be_present
    end

    it { expect(response.response_code).to eq(201) }
  end
end

~~~

Comme vous pouvez le voir, nous sommes en train de créer une variable `order_params` avec les données de la commande. Vous voyez le problème ici? Je l'expliquerai plus tard. Ajoutons simplement le code nécessaire pour faire passer ce test.

Nous devons d'abord ajouter l'action aux routes:

~~~ruby
# config/routes.rb
# ...

Rails.application.routes.draw do
  # ...
  resources :orders, only: %i[index show create]
  # ...
end

~~~

Ensuite, la mise en œuvre qui est facile:

~~~ruby
# app/controllers/api/v1/orders_controller.rb
class Api::V1::OrdersController < ApplicationController
  # ...

  def create
    order = current_user.orders.build(order_params)

    if order.save
      render json: order, status: 201, location: [:api, current_user, order]
    else
      render json: { errors: order.errors }, status: 422
    end
  end

  private

  def order_params
    params.require(:order).permit(:total, :user_id, product_ids: [])
  end
end

~~~

Et maintenant, nos tests devraient tous passer:

~~~bash
$ rspec spec/controllers/api/v1/orders_controller_spec.rb
......

Finished in 0.16817 seconds (files took 0.64624 seconds to load)
6 examples, 0 failures

~~~

Ok donc tout va bien. Nous devrions maintenant passer au chapitre suivant, non? Laissez-moi faire une pause avant. Nous avons de graves erreurs sur l'application et elles ne sont pas liées au code lui-même mais sur la partie métier.

Ce n'est pas parce que les tests passent que l'application remplit la partie métier de l'application. Je voulais en parler parce que dans de nombreux cas, c'est super facile de simplement recevoir des paramètres et de construire des objets à partir de ces paramètres. Dans notre cas, nous ne pouvons pas nous fier aux données que nous recevons. En effet, nous laissons ici le client fixer le total de la commande! Ouais, c'est fou!

Nous devons donc ajouter quelques validations et calculer le total de la commande dans le modèle. De cette façon, nous ne recevons plus cet attribut total et nous avons un contrôle complet sur cet attribut. Alors faisons-le.

Nous devons d'abord ajouter quelques tests pour le modèle de commande:

~~~ruby
# spec/models/order_spec.rb
# ...

RSpec.describe Order, type: :model do
  # ...

  describe '#set_total!' do
    before(:each) do
      product_1 = FactoryBot.create :product, price: 100
      product_2 = FactoryBot.create :product, price: 85

      @order = FactoryBot.build :order, product_ids: [product_1.id, product_2.id]
    end

    it 'returns the total amount to pay for the products' do
      expect { @order.set_total! }.to change { @order.total }.from(0).to(185)
    end
  end
end

~~~

Nous pouvons maintenant ajouter l'implémentation:

~~~ruby
# app/models/order.rb
class Order < ApplicationRecord
  # ...

  def set_total!
    self.total = products.map(&:price).sum
  end
end

~~~

Juste avant que vous ne lanciez vos tests, nous avons besoin de mettre à
jour l'usine de commande:

~~~ruby
# spec/factories/orders.rb
FactoryBot.define do
  factory :order do
    user { nil }
    total { 0.0 }
  end
end

~~~

Nous pouvons maintenant *hooker*[^17] la méthode `set_total!` à un rappel `before_validation` pour s'assurer qu'il a le bon total avant la
validation.

~~~ruby
# app/models/order.rb
class Order < ApplicationRecord
  before_validation :set_total!
  # ...
end

~~~

A ce stade, nous nous assurons que le total est toujours présent et supérieur ou égal à zéro, ce qui signifie que nous pouvons supprimer ces validations et supprimer les spécifications. Nos tests devraient passer maintenant:

~~~bash
$ rspec spec/models/order_spec.rb
.........

Finished in 0.06807 seconds (files took 0.66165 seconds to load)
9 examples, 0 failures

~~~

C'est maintenant le moment de voir le fichier `orders_controller_spec.rb` et de factoriser du code. Actuellement, nous avons quelque chose comme:

~~~bash
# spec/controllers/api/v1/orders_controller_spec.rb
# ...

RSpec.describe Api::V1::OrdersController, type: :controller do
  # ...

  describe 'POST #create' do
    before(:each) do
      current_user = FactoryBot.create :user
      api_authorization_header current_user.auth_token

      product_1 = FactoryBot.create :product
      product_2 = FactoryBot.create :product
      order_params = { total: 50, user_id: current_user.id, product_ids: [product_1.id, product_2.id] }
      post :create, params: { user_id: current_user.id, order: order_params }
    end

    it 'returns the just user order record' do
      expect(json_response[:id]).to be_present
    end

    it { expect(response.response_code).to eq(201) }
  end
end

~~~

Il suffit de supprimer l'`user_id` et les paramètres `total` car l'identifiant utilisateur n'est pas vraiment nécessaire et le total est calculé par le modèle. Après avoir effectué les modifications, le code doit ressembler à ce qui suit:

~~~ruby
# spec/controllers/api/v1/orders_controller_spec.rb
# ...

RSpec.describe Api::V1::OrdersController, type: :controller do
  # ...

  describe 'POST #create' do
    before(:each) do
      current_user = FactoryBot.create :user
      api_authorization_header current_user.auth_token

      product_1 = FactoryBot.create :product
      product_2 = FactoryBot.create :product
      # changes heres
      order_params = { product_ids: [product_1.id, product_2.id] }
      post :create, params: { user_id: current_user.id, order: order_params }
    end

    it 'returns the just user order record' do
      expect(json_response[:id]).to be_present
    end

    it { expect(response.response_code).to eq(201) }
  end
end

~~~

Si vous exécutez les tests maintenant, ils passeront. Mais avant, supprimons le `total` et `user_id` des paramètres autorisés et évitons l'affectation en masse. La méthode `order_params` devrait ressembler à ceci:

~~~ruby
# app/controllers/api/v1/orders_controller.rb
class Api::V1::OrdersController < ApplicationController
  # ...

  private

  def order_params
    params.require(:order).permit(product_ids: [])
  end
end
~~~

Nos tests doivent continuer à passer. *Commitons* nos changements:

~~~bash
$ git commit -am "Adds the create method for the orders controller"
~~~

## Customiser l'affichage JSON des commandes

Maintenant que nous avons construit les routes nécessaires pour les commandes nous pouvons personnaliser les informations que nous voulons rendre sur la sortie JSON pour chaque commande.

Si vous vous souvenez du chapitre [10](#chapter:7){reference-type="ref"reference="chapter:7"}, nous allons ici aussi utiliser *Active Model Serializers*. Commençons donc par créer un sérialiseur pour les commandes:

~~~bash
$ rails generate serializer order
~~~

Ensuite, ouvrons le fichier `order_serializer.rb` qui doit ressembler à ça:

~~~bash
# app/serializers/order_serializer.rb
class OrderSerializer < ActiveModel::Serializer
  attributes :id
end
~~~

Nous allons ajouter l'association des produits et l'attribut `total` à la sortie JSON. Pour nous assurer que tout fonctionne bien, et comme d'habitude, nous ferons quelques tests. Afin d'éviter la duplication de code sur les tests, je vais juste ajouter une spécification pour le spectacle et m'assurer que les données supplémentaires sont rendues. C'est parce que j'utilise le même sérialiseur chaque fois qu'un objet d'ordre est analysé à JSON, donc dans ce cas, je dirais qu'il est très bien:

~~~ruby
# spec/controllers/api/v1/orders_controller_spec.rb
# ...

RSpec.describe Api::V1::OrdersController, type: :controller do
  # ...

  describe 'GET #show' do
    before(:each) do
      current_user = FactoryBot.create :user
      api_authorization_header current_user.auth_token
      @order = FactoryGirl.create :order, user: current_user, product_ids: [@product.id]
      get :show, params: { user_id: current_user.id, id: @order.id }
    end

    it 'returns the user order record matching the id' do
      expect(json_response[:id]).to eql @order.id
    end

    it 'includes the total for the order' do
      expect(json_response[:total]).to eql @order.total.to_s
    end

    it 'includes the products on the order' do
      expect(json_response[:products]).to have(1).item
    end

    # ...
  end

  # ...
end
~~~

Ces tests devraient échouer mais ils sont faciles à faire passer sur le sérialiseur de commande:

~~~bash
# app/serializers/order_serializer.rb
class OrderSerializer < ActiveModel::Serializer
  attributes :id, :total
  has_many :products
end
~~~

Et désormais nos tests devraient passer:

~~~bash
$ rspec spec/controllers/api/v1/orders_controller_spec.rb
........

Finished in 0.22865 seconds (files took 0.70506 seconds to load)
8 examples, 0 failures
~~~

Si vous vous souvenez du chapitre [10](#chapter:7){reference-type="ref"reference="chapter:7"} et de l'encadré [\[box:association\_embeded\]](#box:association_embeded){reference-type="ref"reference="box:association_embeded"}, nous avons intégré l'utilisateur dans le produit. Mais ici, cela ne sert à rien sachant nous connaissons toujours l'utilisateur car il s'agit de l'utilisateur courant. Il n'y a pas lieu de l'ajouter car ce n'est pas efficace. Corrigeons cela en ajoutant un nouveau sérialiseur:

~~~bash
$ rails g serializer order_product
~~~

Nous voulons que les informations sur les produits restent cohérentes avec celles que nous avons actuellement de sorte que nous puissions simplement en hériter le comportement comme cela:

~~~ruby
# app/serializers/order_product_serializer.rb
class OrderProductSerializer < OrderSerializer
end
~~~

De cette façon, les deux sérialiseurs seront maintenant lié et il suffira d'ajouter un champ à `ProductSerializer` et il sera ajouté à `OrderProductSerializer`. Maintenant, nous voulons supprimer l'utilisateur associé. Nous ajouterons simplement une méthode qui permet de faire cela[^18]:

~~~ruby
# app/serializers/order_product_serializer.rb
class OrderProductSerializer < ProductSerializer
  def include_user?
    false
  end
end

~~~

Après avoir fait ce changement, nous devons dire au `order_serializer` d'utiliser le sérialiseur que nous venons de créer en passant simplement une option à l'association `has_many` sur le `order_serializer`:

~~~ruby
# app/serializers/order_product_serializer.rb
class OrderProductSerializer < ProductSerializer
  def include_user?
    false
  end
end

~~~

Et nos tests devraient continuer à passer:

~~~bash
$ rspec spec/controllers/api/v1/orders_controller_spec.rb
........

Finished in 0.24024 seconds (files took 0.70072 seconds to load)
8 examples, 0 failures

~~~

*Commitons* nos changements et passons à la section suivante:

~~~bash
$ git add .
$ git commit -m "Adds a custom order product serializer to remove the user association"
~~~

## Envoyer un email de confirmation

La dernière section de ce chapitre sera d'envoyer un courriel de confirmation à l'utilisateur qui vient de créer une commande. Si vous voulez sauter cette étape et passer au chapitre [12](#chapter:9){reference-type="ref" reference="chapter:9"} suivant, allez-y! Cette section est plus à un échauffement.

Vous êtes peut-être familier avec la manipulation des courriels avec Rails, je vais essayer de rendre cela simple et rapide:

Nous commençons par créer le `order_mailer`:

~~~bash
$ rails generate mailer order_mailer
~~~

Pour faciliter le test de l'email, nous utiliserons une gemme appelée [email\_spec](https://github.com/email-spec/email-spec). Elle inclut un tas de méthodes utiles pour tester les courriels.

Donc d'abord ajoutons la gemme au `Gemfile`:

~~~ruby
# Gemfile
# ...

group :test do
  gem 'rspec-collection_matchers', '~> 1.1'
  gem 'rspec-rails', '~> 3.8'
  gem "email_spec"
  gem 'shoulda-matchers'
end

# ...
~~~

Lancez maintenant la commande `bundle install` pour installer toutes les dépendances. Je vais suivre [les étapes de documentation](https://github.com/email-spec/email-spec#rspec-31) pour configurer la gemme. Lorsque vous avez terminé, votre fichier `rails_helper.rb` devrait ressembler à ça:

~~~ruby
# spec/rails_helper.rb

require File.expand_path('../config/environment', __dir__)
ENV['RAILS_ENV'] ||= 'test'
# Prevent database truncation if the environment is production
abort('The Rails environment is running in production mode!') if Rails.env.production?

require 'spec_helper'
require 'email_spec'
require 'email_spec/rspec'
require 'rspec/rails'

# ...

~~~

Maintenant, nous pouvons ajouter quelques tests pour les mails de commandes que nous venons de créer:

~~~ruby
# spec/mailers/order_mailer_spec.rb
require 'rails_helper'

RSpec.describe OrderMailer, type: :mailer do
  include Rails.application.routes.url_helpers

  describe '.send_confirmation' do
    before(:all) do
      @user = FactoryBot.create :user
      @order = FactoryBot.create :order, user: @user
      @order_mailer = OrderMailer.send_confirmation(@order)
    end

    it 'should be set to be delivered to the user from the order passed in' do
      expect(@order_mailer).to deliver_to(@user.email)
    end

    it 'should be set to be send from no-reply@marketplace.com' do
      expect(@order_mailer).to deliver_from('no-reply@marketplace.com')
    end

    it "should contain the user's message in the mail body" do
      expect(@order_mailer).to have_body_text(/Order: ##{@order.id}/)
    end

    it 'should have the correct subject' do
      expect(@order_mailer).to have_subject(/Order Confirmation/)
    end

    it 'should have the products count' do
      expect(@order_mailer).to have_body_text(/You ordered #{@order.products.count} products:/)
    end
  end
end
~~~

J'ai simplement copié/collé les tests de la documentation et je l'ai adapté à nos besoins. Nous devons maintenant nous assurer que ces tests passent.

Tout d'abord, nous ajoutons la méthode `OrderMailer#send_confirmation`:

~~~ruby
# app/mailers/order_mailer.rb
class OrderMailer < ApplicationMailer
  default from: 'no-reply@marketplace.com'
  def send_confirmation(order)
    @order = order
    @user = @order.user
    mail to: @user.email, subject: 'Order Confirmation'
  end
end
~~~

Après avoir ajouté ce code, nous devons maintenant ajouter les vues correspondantes. C'est une bonne pratique d'inclure une version texte (Listing [\[lst:order\_mailer\_send\_confirmation\_text\]](#lst:order_mailer_send_confirmation_text){reference-type="ref"reference="lst:order_mailer_send_confirmation_text"}) en plus de la version HTML (Listing [\[lst:order\_mailer\_send\_confirmation\_html\]](#lst:order_mailer_send_confirmation_html){reference-type="ref"reference="lst:order_mailer_send_confirmation_html"}).

~~~erb
<%# app/views/order_mailer/send_confirmation.txt.erb %>
Order: #<%= @order.id %>
You ordered <%= @order.products.count %> products:
<% @order.products.each do |product| %>
  <%= product.title %> - <%= number_to_currency product.price %>
<% end %>
~~~

~~~erb
<!-- app/views/order_mailer/send_confirmation.html.erb -->
<h1>Order: #<%= @order.id %></h1>
<p>You ordered <%= @order.products.count %> products:</p>
<ul>
  <% @order.products.each do |product| %>
    <li><%= product.title %> - <%= number_to_currency product.price %></li>
  <% end %>
</ul>
~~~

Maintenant, nos tests devraient passer:

~~~bash
$ rspec spec/mailers/order_mailer_spec.rb
.....

Finished in 0.24919 seconds (files took 0.75369 seconds to load)
5 examples, 0 failures
~~~

Et maintenant, il suffit d'appeler la méthode `OrderMailer#send_confirmation` dans l'action de création sur le contrôleur des ordres:

~~~ruby
# app/controllers/api/v1/orders_controller.rb
class Api::V1::OrdersController < ApplicationController
  # ...

  def create
    order = current_user.orders.build(order_params)

    if order.save
      OrderMailer.send_confirmation(order).deliver
      render json: order, status: 201, location: [:api, current_user, order]
    else
      render json: { errors: order.errors }, status: 422
    end
  end

  # ...
end

~~~

Pour être sûr que nous n'avons rien cassé, lançons tous les tests:

~~~bash
$ rspec spec
..................................................................................................

Finished in 1.82 seconds (files took 0.78532 seconds to load)
98 examples, 0 failures

~~~

*Commitons* tout ce que nous venons de faire pour terminer cette section:

~~~bash
$ git add .
$ git commit -m "Adds order confirmation mailer"

~~~

Conclusion
----------

Ça y est! Vous avez réussi! Vous pouvez vous applaudir. Je sais que ça a été long mais c'est presque finit, croyez moi.

Sur les chapitres à venir, nous continuerons à travailler sur le modèle de commande pour ajouter des validations lors de la passation d'une commande. Certains scénarios sont:

- Que se passe-t-il lorsque les produits ne sont pas disponibles?
- Diminuer la quantité du produit en cours lors de la passation d'une commande

Le prochain chapitre sera court, mais il est très important pour la santé l'application. Alors ne le sautez pas.


---

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

---

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

[api_on_rails_git]: https://github.com/madeindjs/api_on_rails



[ruby_hash]: https://ruby-doc.org/core-2.6/Hash.html


[^1]: Kaishi ne fonctionne actuellement que pour Mac OS
[^4]: Note pour Mac OS: si vous utilisez Mac, n'oubliez pas que vous devez avoir installé les [outils en ligne de commande pour Xcode](https://developer.apple.com/downloads/).





[^9]: Il y a un excellent
    [railscast](http://railscasts.com/episodes/350-rest-api-versioning)
    qui explique cela

[^10]: Il en existe beaucoup d'autres. La liste complète est disponnible
    [sur
    Wikipedia](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields).

[^11]: Développement Dirigé par les tests

[^12]: signifie qu'un enregistrement vient d'être créé

[^13]: signifie Unprocessable Entity c'est à dire que le serveur ne peut
    pas sauvegarder l'enregistrement

[^14]: Cela qui signifie que le serveur a traité la demande avec succès,
    mais ne renvoie aucun contenu

[^15]: Si vous vous demandez "pourquoi diable avons-nous créé une classe
    d'authentification dans le fichier spec?", La réponse est simple.
    Quand il s'agit de tester des modules, je trouve facile de les
    inclure dans une classe temporaire et de stub toutes les autres
    méthodes dont j'ai besoin plus tard, comme la requête montrée
    ci-dessus.

[^16]: La scalabilité désigne la capacité d'un produit à s'adapter à une
    forte demande (montée en charge)

[^17]: Je n'ai pas trouvé de traductions française correcte pour ce
    terme. Il s'agit de lancer une actions lorsque un évènement est
    appelé.

[^18]: Pour plus d'informations, jetez un œil à la documentation de
    ActiveModelSerializer

[^19]: C'est juste pour garder les choses plus propres et vous montrer
    une autre technique pour réaliser des validations personnalisées
