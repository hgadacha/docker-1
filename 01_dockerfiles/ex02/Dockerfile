FROM ruby:2.5.1
# Ruby est basé sur buildpack-deps:jessie qui est basé sur buildpack-deps:jessie-scm qui est basé sur
# buildpack-deps:jessie-curl, qui est finalement basé sur debian:jessie.
# J'ai résolu mon probleme en précisant cette version de ruby.

MAINTAINER Honsse Gadacha <hgadacha@student.42.fr>

RUN apt-get -y update && \
	apt-get -y upgrade && \
	apt-get -y install nodejs sqlite3 libsqlite3-dev && \
	gem install rails bundler && \
	apt-get clean && \
	rm -rf /var/lib/apt/lists/*
# (Les paquets "build-essential" et "libpq-dev" (et d'autres) peuvent etre ajouter en plus, car ils sont présents dans
# quelques documentations que j'ai pu lire, mais tout fonctionne deja avec ces paquets.)
# apt-get : Programme en ligne de commande pour la gestion des paquets.
# update : Mettre à jour l'index des packages disponibles.
# upgrade : Mettre à niveau les packages actuellement installés.
#
# nodejs : prérequis pour le bon fonctionnement de rails, il intervient dans la gestion des fichiers
# JavaScript des futurs sites.
#
# sqlite3 : Permet de manipuler une base de donees. Lorsque le framework web Ruby on Rails est utilise,
# notre application est configuree par defaut pour utiliser SQLite. SQLite3 présente l'avantage de n'avoir
# rien à configurer, rien à maintenir ou à administrer. C'est une base de données relationnelle légère et
# portable qui fonctionne particulièrement bien dans les environnements à faible mémoire et qui fonctionnera
# bien dans de nombreux cas.
# libSQLite : bibliothèque C qui implémente un moteur de base de données SQL, lié a SQLite.
# (Tout fonctione bien sans le package "build-essential" et "libpq-dev", je les ais quand meme ajoutés en plus
# et aussi car je les ais trouvé présents dans certaines documentations.)
#
# gem : est un programme qui vient avec Ruby. Il permet d’installer d’autres programmes, comme Rails.
#
# Nettoyer le cache apt en supprimant /var/lib/apt/lists, cela réduit la taille de l'image,
# car le cache apt n'est pas stocké dans un layer. Supprimer tous les fichiers de liste apt
# car ils sont gros et deviennent rapidement caduques.
# clean : supprime la totalité des paquets présents dans /var/cache/apt/archives.(aucun impact sur les paquets installés.)

COPY app /opt/app
WORKDIR /opt/app
# COPY : (COPY <src><dest>) L'instruction COPY copie les nouveaux fichiers ou répertoires de <src> et les
# ajoute au système de fichiers du conteneur sur le chemin <dest>. Si <src> est un répertoire, tout le
# contenu du répertoire est copié.(le répertoire lui-même n'est pas copié, juste son contenu.)
# (l'instruction WORKDIR ne fonctionne pas en "ONBUILD".)

ONBUILD RUN bundle update && bundle install
ONBUILD RUN rake db:migrate
ONBUILD RUN rake db:seed
# - ONBUILD : utile pour les images qui vont être construites à partir d'une image donnée.
# Une commande ONBUILD s'exécute une fois la génération Dockerfile actuelle terminée. ONBUILD s'exécute
# dans toute image enfant dérivée de l'image actuelle. Considérez la commande ONBUILD comme une instruction que
# le Dockerfile parent donne au Dockerfile enfant. Une génération Docker exécute les commandes ONBUILD avant
# toute commande dans un Dockerfile enfant. Le déclencheur sera exécuté dans le contexte de la génération en
# aval, comme s'il avait été inséré immédiatement après l'instruction FROM dans le Dockerfile en aval.
# Avertissement: l'instruction ONBUILD peut ne pas déclencher les instructions FROM ou MAINTAINER.
#
# - Bundler : Bundler fournit un environnement cohérent pour les projets Ruby en suivant et en installant les
# gemmes et les versions exactes qui sont nécessaires.
# bundle update : Met à jour vos gemmes vers les dernières versions disponibles.
# bundle install : Installez les dépendances spécifiées dans votre Gemfile.
#
# - Rake : Rake est un gestionnaire de tâches populaire dans Ruby.
# - rake db:migrate : migration de la database --> Une migration Rails est un outil permettant de modifier le schéma de base
# de données d'une application. Au lieu de gérer des scripts SQL, vous définissez les modifications de base de
# données dans un langage DSL (domain-specific language). Le code est indépendant de la base de données, vous pouvez
# donc facilement déplacer votre application vers une nouvelle plateforme. Les exigences changent tout le temps et
# ces changements entraînent souvent des changements dans la base de données. Les migrations permettent de modifier
# le schéma de base de données dans votre application Rails. Vous utilisez donc du code Ruby au lieu de SQL.
# - rake db:seed : population de la database --> Pour ajouter des données initiales après la création d'une base de
# données, Rails a une fonction intégrée de «seeds» qui rend le processus rapide et facile. Cela est particulièrement
# utile lors du rechargement fréquent de la base de données dans des environnements de développement et de test.
#
# Lorsque l’on ajoute une gemme au Gemfile, il faut la télécharger et l’installer. Ces deux choses se font
# automatiquement avec la commande bundle install.
# Pour les applications Rails, après le bundle install, il ne faut pas oublier le rake db:create
# (parfois facultatif) puis le rake db:migrate.
#
# Les gemmes sont des modules de codes produits par d'autres développeurs qui apportent des fonctionnalités à votre
# application Ruby – elles ne se limitent pas à Rails. Rails utilise lui-même quelques gemmes. La liste des gemmes
# utilisées se situe dans le fichier Gemfile à la racine de votre application.
# Le gemspec spécifie les informations sur une gemme telles que son nom, sa version, sa description, ses auteurs
# et sa page d'accueil.
#
#
#
# for build : docker build -t ft-rails:on-build .
#
# liens utiles :
# https://hub.docker.com/_/rails
# https://github.com/docker-library/ruby
# https://github.com/docker-library/rails/blob/e16e955a67f48c1e8dc0af87ba6c0b7f8302bad2/Dockerfile
# https://github.com/docker-library/ruby/blob/c43fef8a60cea31eb9e7d960a076d633cb62ba8d/2.4/jessie/onbuild/Dockerfile
# https://guides.rubyonrails.org/getting_started.html
# https://guides.rubyonrails.org/command_line.html
# https://guides.rubyonrails.org/development_dependencies_install.html
# https://guides.rubyonrails.org/v4.0/development_dependencies_install.html
# https://kyup.com/tutorials/install-ruby-rails/
# https://openclassrooms.com/fr/courses/3149156-initiez-vous-a-ruby-on-rails/3149171-installez-ruby-on-rails
# https://openclassrooms.com/fr/courses/3149156-initiez-vous-a-ruby-on-rails/3149246-les-gems
# https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
# https://docs.docker.com/engine/reference/builder/#onbuild
# https://docs.docker.com/engine/reference/builder/#copy
# https://guides.rubyonrails.org/command_line.html
# https://www.youtube.com/playlist?list=PLjwdMgw5TTLWfI1B2Wv2WPgR9iOyw12zi
# https://bundler.io/v2.0/man/bundle.1.html
# https://www.packet.com/blog/how-to-run-your-rails-app-on-docker/
# https://docs.docker.com/compose/rails/
# https://guides.rubygems.org/what-is-a-gem/
# https://guides.rubyonrails.org/v3.2/migrations.html
# https://www.rubyguides.com/2019/02/ruby-rake/
