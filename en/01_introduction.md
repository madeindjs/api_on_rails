# Introduction

Welcome to [APIs on Rails][api_on_rails_git] a tutorial on steroids on how to build your next API with Rails. The goal of this book is to provide an answer on how to develop a RESTful API following the best practices out there, along with my own experience. By the time you are done with _API’s on Rails_ you should be able to build your own `API` and integrate it with any clients such as a web browser or your next mobile app. The code generated is built on top of Rails 4 which is the current version, for more information about this check out [http://rubyonrails.org/](http://rubyonrails.org/). The most up-to-date version of the _API’s on Rails_ can be found on [APIs on Rails][api_on_rails_git]; don’t forget to update your offline version if that is the case.

The intention with this book it’s not teach just how to build an API with Rails rather to teach you how to build scalable and maintainable API with Rails, which means taking your current Rails knowledge to the next level when on this approach. In this journey we are going to take, you will learn to:

- Build JSON responses
- Use Git for version controlling
- Testing your endpoints
- Optimize and cache the API

I highly recommend you go step by step on this book, try not to skip chapters, as I mention tips and interesting facts for improving your skills on each on them. You can think yourself as the main character of a video game and with each chapter you’ll get a higher level.

In this first chapter I will walk you through on how to setup your environment in case you don’t have it already. We’ll then create the application called `market_place_api`. I’ll emphasize all my effort into teaching you all the best practices I’ve learned along the years, so this means right after initializing the project we will start tracking it with Git.

In the next chapters we will be building the application to demonstrate a simple workflow I use on my daily basis. We’ll develop the whole application using **test driven development** (TDD), getting started by explaining why you want to build an API’s for your next project and deciding whether to use JSON or XML as the response format. From Chapter 3 to Chapter 8 we’ll get our hands dirty and complete the foundation for the application by building all the necessary endpoints, securing the API access and handling authentication through headers exchange. Finally on the last chapter (Chapter 11) we’ll add some optimization techniques for improving the server responses.

The final application will scratch the surface of being a market place where users will be able to place orders, upload products and more. There are plenty of options out there to set up an online store, such as [Shopify](http://shopify.com), [Spree](http://spreecommerce.com/) or [Magento](http://magento.com).

By the end or during the process (it really depends on your expertise), you will get better and be able to better understand some of the bests Rails resources out there. I also took some of the practices from these guys and brought them to you:

- [Railscasts](http://railscasts.com/)
- [CodeSchool](http://codeschool.com/)
- [JSON API](http://jsonapi.org/format/)

## Conventions on this book

The conventions on this book are based on the ones from [Ruby on Rails Tutorial](http://www.railstutorial.org/book/beginning#sec-conventions). In this section I’ll mention some that may not be so clear.

I’ll be using many examples using command-line commands. I won’t deal with windows `cmd` (sorry guys), so I’ll based all the examples using Unix-style command line prompt, as follows:

~~~bash
$ echo "A command-line command"
A command-line command
~~~

I’ll be using some guidelines related to the language, what I mean by this is:

[Chapter 1: Introduction | APIs on Rails | Softcover.io](http://apionrails.icalialabs.com/book/chapter_one#cid2)

- **Avoid** means you are not supposed to do it
- **Prefer** indicates that from the 2 options, the first it’s a better fit
- **Use** means you are good to use the resource

If for any reason you encounter some errors when running a command, rather than trying to explain every possible outcome, I’ll will recommend you to ‘google it’, which I don’t consider a bad practice or whatsoever. But if you feel like want to grab a beer or have troubles with the tutorial you can always [email me](mailto:contact@rousseau-alexandre.fr).


## Development environments

One of the most painful parts for almost every developer is setting everything up, but as long as you get it done, the next steps should be a piece of cake and well rewarded. So I will guide you to keep you motivated.

### Text editors and Terminal

There are many cases in which development environments may differ from computer to computer. That is not the case with text editors or IDE’s. I think for Rails development an IDE is way to much, but some other might find that the best way to go, so if that it’s your case I recommend you go with [RadRails](http://www.aptana.com/products/radrails) or [RubyMine](http://www.jetbrains.com/ruby/index.html), both are well supported and comes with many integrations out of the box.

- **Text editor**: I personally use [vim](http://www.vim.org/) as my default editor with [janus](https://github.com/carlhuda/janus) which will add and handle many of the plugins you are probably going to use. In case you are not a _vim_ fan like me, there are a lot of other solutions such as [Sublime Text](http://www.sublimetext.com/) which is a cross-platform easy to learn and customize (this is probably your best option), it is highly inspired by [TextMate](http://macromates.com/) (only available for Mac OS). A third option is to use a more recent text editor from the guys at [Github](http://gitub.com) called [Atom](https://atom.io/), it’s a promising text editor made with Javascript, it is easy to extend and customize to meet your needs, give it a try. Any of the editors I present will do the job, so I’ll let you decide which one fits your eye.

- **Terminal**: If you decided to go with [kaishi](http://icalialabs.github.io/kaishi/) for setting the environment you will notice that it sets the default shell to `zsh`, which I highly recommend. For the terminal, I’m not a fan of the _Terminal_ app that comes out of the box if you are on Mac OS, so check out [iTerm2](http://www.iterm2.com/#/section/home), which is a terminal replacement for Mac OS. If you are on Linux you probable have a nice terminal already, but the default should work just fine.

### Browsers

When it comes to browsers I would say [Firefox](http://www.mozilla.org/en-US/firefox/new/) immediately, but some other developers may say [Chrome](https://www.google.com/intl/en/chrome/browser/) or even [Safari](https://www.apple.com/safari/). Any of those will help you build the application you want, they come with nice inspector not just for the DOM but for network analysis and many other features you might know already.


### Package manager

- **Mac OS**: There are many options to manage how you install packages on your Mac, such as [Mac Ports](https://www.macports.org/) or [Homebrew](http://brew.sh/), both are good options but I would choose the last one, I’ve encountered less troubles when installing software and managing it. To install `brew` just run the command below:
~~~bash
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
~~~

- **Linux**: You are all set!, it really does not matter if you are using `apt`, `pacman`, `yum` as long you feel comfortable with it and know how to install packages so you can keep moving forward.

### Git

We will be using Git a lot, and you should use it too not just for the purpose of this tutorial but for every single project.

- sous Mac OS: `$ brew install git`
- sous Linux: `$ sudo apt-get install git`

### Ruby

There are many ways in which you can install and manage ruby, and by now you should probably have some version installed if you are on Mac OS, to see which version you have, just type:

~~~bash
$ ruby -v
~~~

Rails 5 requires you to install version 2.2.2 or higher. I recommend you to start using [Ruby Version Manager (RVM)](http://rvm.io/) or [rbenv](http://rbenv.org/), any of these will allow you to install multiple versions of `ruby`. We will use RVM in this tutorial any of these two options you choose is fine.

To install RVM go on <https://rvm.io/> and get GPG key[^gpg]. Then:

[^gpg]: The GPG allow you to verify author identity of the software you download.

~~~bash
$ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
$ \curl -sSL https://get.rvm.io | bash
~~~

Next it is time to install ruby:

~~~bash
$ rvm install 2.5
~~~

If everything went smooth, it is time to install the rest of the dependencies we will be using.

#### Gems, Rails & Missing libraries

First we update the gems on the whole system:

~~~bash
$ gem update --system
~~~

On some cases if you are on a Mac OS, you will need to install some extra libraries:

~~~bash
$ brew install libtool libxslt libksba openssl
~~~

We then install the necessary gems and ignore documentation for each gem:

~~~bash
$ printf 'gem: --no-document' >> ~/.gemrc
$ gem install bundler
$ gem install foreman
$ gem install rails -v 5.2
~~~

Check for everything to be running nice and smooth:

~~~bash
$ rails -v 5.2
5.2.0
~~~

#### Bases de données

I highly recommend you install [Postgresql](http://www.postgresql.org/) to manage your databases, but for simplicity we’ll be using [SQlite](http://www.sqlite.org/). If you are using Mac OS you should be ready to go, in case you are on Linux, don’t worry we have you covered:

~~~bash
$ sudo apt-get install libxslt-dev libxml2-dev libsqlite3-dev
~~~

or

~~~bash
$ sudo yum install libxslt-devel libxml2-devel libsqlite3-devel
~~~

## Initialisation du projet

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

Si vous voulez démarrer le Prax automatiquement, ajoutez cette ligne au fichier `.profile`:

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

Il est également important que vous mettiez à jour le `Gemfile` pour regrouper les différentes gemmes dans l'environnement correct:


~~~ruby
# Gemfile
# ...
group :development do
  gem 'sqlite3'
end
# ...
~~~

Ceci, comme vous vous en souvenez peut-être, empêchera l'installation ou l'utilisation de Sqlite lorsque vous déployez votre application chez un fournisseur de serveurs comme Heroku[^heroku].

[^heroku]: Heroku facilite le déploiement de votre application en installant les dépendances sur un serveur en analysant votre *Gemfile*

Une fois cette configuration effectuée, il est temps d'exécuter la commande d'installation du paquet pour intégrer les dépendances correspondantes:

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

Après avoir modifié le fichier `.gitignore`, il suffit d'ajouter les fichiers et de valider les modifications. Les commandes nécessaires sont indiquées ci-dessous:

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
