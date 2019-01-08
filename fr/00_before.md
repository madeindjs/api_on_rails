---
title: API on Rails 5
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

"API on Rails 5" est basé sur ["APIs on Rails: Building REST APIs with Rails"](http://apionrails.icalialabs.com/book/). Celui-ci fut initialement publié en 2014 par [Abraham Kuri](https://twitter.com/kurenn) sous les licences [MIT](http://opensource.org/licenses/MIT) et [Beerware](http://people.freebsd.org/~phk/).

L'oeuvre originale étant non maintenue, j'ai voulu mettre à jour cet excellent ouvrage et contribuer à la communauté francophone en le traduisant moi-même. Cette mise à jour est aussi disponnible dans la langue de Shakespeare.

## A propos de l’auteur original

[Abraham Kuri](https://twitter.com/kurenn) est un développeur Rails avec 5 ans d'expérience (sûrement plus maintenant). Son expérience inclut le travail en tant que *freelance* dans la construction de produits logiciels et plus récemment dans la collaboration au sein de la communauté open source. Diplômé en informatique d'ITESM, il a fondé deux sociétés au Mexique ([Icalia Labs](http://icalialabs.com/) et [Codeando Mexico](http://codeandomexico.org/)).

De mon côté, je m'appelle [Alexandre Rousseau](http://rousseau-alexandre.fr) et je suis un développeur Rails avec plus de 4 ans d’expérience (à l'heure ou j'écris ces lignes). Je suis actuellement associé dans une entreprise ([iSignif](https://isignif.fr)) ou je construit et maintient un produit de type SAAS en utilisant Rails. Je contribue aussi à la communauté Ruby en produisant et maintenant quelques gemmes que vous pouvez consulter sur [mon profil Rubygems.org](https://rubygems.org/profiles/madeindjs). La plupart de mes projets sont sur Github donc n'[hésitez pas à me suivre](http://github.com/madeindjs/).

Tout le code source de ce livre est disponible au format [Markdown](https://fr.wikipedia.org/wiki/Markdown) sur [Github][api_on_rails_git]. Ainsi n'hesitez pas à [forker](https://github.com/madeindjs/api_on_rails/fork) le projet si vous voulez l'améliorer ou corrigé une faute qui m'aurais échappé.

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


[api_on_rails_git]: https://github.com/madeindjs/api_on_rails
[ruby_hash]: https://ruby-doc.org/core-2.6/Hash.html
