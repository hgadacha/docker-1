FROM debian

MAINTAINER Honsse Gadacha <hgadacha@student.42.fr>

ENV TS3SERVER_LICENSE=accept
# Acceptez le contrat de licence pour démarrer un serveur TeamSpeak. (voir dokcerhub)

EXPOSE 9987/udp 10011 30033
# L'instruction EXPOSE indique les ports sur lesquels un conteneur écoute les connexions. (voir dockerhub)
# Par défaut, EXPOSE suppose TCP.

RUN apt-get -y update && \
	apt-get -y upgrade && \
	apt-get -y install wget bzip2 && \
	rm -rf /var/lib/apt/lists/* && \
	groupadd -r teamspeak && \
	useradd -r -m -g teamspeak teamspeak_usr1
# apt-get : Programme en ligne de commande pour la gestion des paquets.
# update : Mettre à jour l'index des packages disponibles.
# upgrade : Mettre à niveau les packages actuellement installés.
#
# Pour télécharger et extraire le logiciel serveur de TeamSpeak, nous avons besoin de logiciels
# supplémentaires, dans ce cas un gestionnaire de téléchargement (wget) et l'utilitaire pour extraire
# le logiciel serveur (bzip2).
#
# Wget : Wget est un programme non interactif (c'est-à-dire qu'il peut travailler en arrière-plan,
# sans intervention de l'utilisateur) de téléchargement de fichiers depuis le Web. C'est un logiciel
# libre. Il supporte les protocoles HTTP, HTTPS et FTP ainsi que le téléchargement au travers des proxies HTTP.
#
# bzip2 : bzip2 est un algorithme de compression de données. L'extension des fichiers compressés avec
# ce logiciel est généralement ".bz2".
#
# Nettoyage du cache apt en supprimant /var/lib/apt/lists, cela réduit la taille de l'image,
# car le cache apt n'est pas stocké dans un layer, de plus ils sont gros et deviennent rapidement caduques.
# (Les images officielles Debian et Ubuntu exécutent automatiquement apt-get clean, donc une invocation
# explicite n'est pas requise).
#
# groupadd : Creation d'un nouveau groupe ou on ajoutera le nouvel utilisateur. Lorsque l'utilisateur
# n'a pas de groupe principal, l'image (ou les instructions suivantes) sera exécutée avec le groupe
# root (voir documentation docker). (-r = Cette option indique à groupadd d'ajouter un compte système).
#
# useradd : Ajouter d'un utilisateur (et l'assigner a un groupe (otpion -g)) non-root (Teamspeak3
# nécessite d'être lancé avec un utilisateur qui lui est dédié). (-r = utilisée pour créer un compte
# système) (-m = permet de créer en même temps le répertoire personnel de l'utilisateur).
#
# L'instruction RUN exécutera toutes les commandes dans un nouveau calque au-dessus de l'image
# actuelle et validera les résultats. L'image validée résultante sera utilisée pour l'étape
# suivante du Dockerfile.

USER teamspeak_usr1
# Définit le nom d'utilisateur à utiliser lors de l'exécution des instructions RUN, CMD et ENTRYPOINT
# qui le suivent dans le Dockerfile.

WORKDIR /home/teamspeak_usr1
# WORKDIR doit etre utilise au lieu de proliférer des instructions comme RUN cd ... && ..., qui sont
# difficiles à lire, à dépanner et à maintenir. Si le WORKDIR n'existe pas, il sera créé même s'il
# n'est utilisé dans aucune instruction Dockerfile ultérieure.

RUN wget https://files.teamspeak-services.com/releases/server/3.11.0/teamspeak3-server_linux_amd64-3.11.0.tar.bz2 && \
	tar -xvf teamspeak3-server_linux_amd64-3.11.0.tar.bz2 && \
	rm -rf teamspeak3-server_linux_amd64-3.11.0.tar.bz2
# Telechargement de la derniere version du serveur Teamspeak3 (https://files.teamspeak-services.com/releases/server/ ||
# http://dl.4players.de/ts/releases/) via la commande wget. Extraction de l'archive avant de la supprimer (desormais
# inutile et permettra d'avoir une image plus legere). (-x = restaure les fichiers contenus dans une archive)
# (-v = affiche la liste des fichiers traités)(-f = extrait un fichier donné).

WORKDIR /home/teamspeak_usr1/teamspeak3-server_linux_amd64
# Redirection dans le repertoire extrait.

CMD ["/home/teamspeak_usr1/teamspeak3-server_linux_amd64/ts3server_minimal_runscript.sh"]
# Executable du lancement du server Teamspeak lor du lancement du container.
#
#
#
# for build : docker image build -t debian_ts .
#
# TeamSpeak : Teamspeak est un logiciel propriétaire d’audioconférence sur Internet
# (voix sur réseau IP). Il permet de discuter à plusieurs dans des canaux, avec d'autres utilisateurs.
#
# liens utiles :
# https://hub.docker.com/_/teamspeak
# https://www.bennetrichter.de/en/tutorials/teamspeak3-server-linux/
# https://unixblogger.com/setup-a-teamspeak-3-server-on-linux-ubuntu-debian/
# https://console.nodilex.com/fr/community/tutorials/13/installer-un-serveur-vocal-teamspeak-3-sur-linux-debian-ubuntu/
# https://debian-facile.org/doc:systeme:useradd
# https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
# https://github.com/moby/moby/blob/03e2923e42446dbb830c654d0eec323a0b4ef02a/contrib/mkimage/debootstrap#L82-L105
# https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
# https://docs.docker.com/engine/reference/builder/#env
# https://docs.docker.com/engine/reference/builder/#expose
# https://docs.docker.com/engine/reference/builder/#user
# https://docs.docker.com/engine/reference/builder/#workdir
