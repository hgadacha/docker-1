FROM alpine
# FROM : permet de définir l'image source.

MAINTAINER Honsse Gadacha <hgadacha@student.42.fr>
# MAINTAINER : L'instruction MAINTAINER permet de définir le champ Auteur des images générées.

RUN apk update && \
	apk upgrade && \
	apk add vim && \
	rm -rf /var/cache/apk/*
# RUN : permet d’exécuter des commandes dans le conteneur.
# (Utilisez la commande RUN avec des commandes shell. Vous pouvez séparer vos commandes avec ";" ou "&&".
# L'utilisation de "\" comme caractère de continuation de ligne pour les Dockerfiles améliore la lisibilité.
# Vous pouvez également placer toutes les commandes dans un script shell et demander à la commande RUN d'exécuter ce script shell.)
# 
# apk : est l'outil utilisé pour installer, mettre à niveau ou supprimer des logiciels
# sur un système Alpine en cours d'exécution.
# update : Mettre à jour l'index des packages disponibles.
# upgrade : Mettre à niveau les packages actuellement installés.
# add : Ajouter de nouveaux packages ou des packages de mise à niveau au système en cours d'exécution.
# rm -rf /var/cache/apk/* : lorsque vous nettoyez le cache apk, cela réduit la taille de l'image,
# car le cache apk n'est pas stocké dans un layer. Étant donné que l'instruction RUN commence par la
# mise à jour apk update, le cache du package est toujours actualisé avant l'installation d'apk.

CMD vim
# CMD : permet de définir la commande par défaut lors de l’exécution du conteneur Docker.
#
#
#
# for build : docker image build -t alpine_vim .
#
# liens utiles :
# https://openclassrooms.com/fr/courses/2035766-optimisez-votre-deploiement-en-creant-des-conteneurs-avec-docker/6211517-creez-votre-premier-dockerfile
# https://hub.docker.com/_/alpine
# https://wiki.alpinelinux.org/wiki/Alpine_Linux_package_management
# https://www.grafikart.fr/tutoriels/dockerfile-636
# https://docs.docker.com/engine/reference/builder/#cmd
# https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
