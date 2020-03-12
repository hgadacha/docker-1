FROM ubuntu:16.04
# Avec la version 18.04 le package GPG ne s'installe pas. 

MAINTAINER Honsse Gadacha <hgadacha@student.42.fr>

EXPOSE 447 22 80

RUN apt-get -y update && apt-get -y upgrade && \
	apt-get install -y curl openssh-server ca-certificates && \
	apt-get install -y postfix tzdata
# Installation et configuration des dépendances requises.
# - curl : outil en ligne de commande pour transférer des données avec une syntaxe URL.
# - openssh-server : serveur shell sécurisé (SSH), pour accèder à des machines à distance.
# - ca-certificates : Ce paquet permet aux applications basées sur SSL de vérifier l'authenticité des connexions SSL.
# - Installation de Postfix pour envoyer des e-mails de notification.
# (Postfix est un serveur de messagerie électronique.)
# - tzdata : est une base de données qui contient différentes informations sur les fuseaux horaires du monde.
# l'installation de ce package est preconise pour le fonctionemment de la commande "runsvdir-start &" et
# la commande gitlab-ctl reconfigure.
# (https://gitlab.com/gitlab-org/omnibus-gitlab/issues/4679)(https://gitlab.com/gitlab-org/omnibus-gitlab/issues/2229)
RUN curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash
# Configuration du référentiel de packages GitLab.
# Script complet : https://packages.gitlab.com/gitlab/gitlab-ce/install
RUN apt-get -y install gitlab-ce && \
	rm -rf /var/lib/apt/lists/*
# Installation de gitlab.
# (Si on assgine un variable d'environnment 'EXTERNAL_URL="https://gitlab.example.com"', l'installation de gitlab
# ne s'effectue pas et bloque : https://gitlab.com/gitlab-org/omnibus-gitlab/issues/4257).

RUN sed -i "s|external_url 'http://gitlab.example.com'|external_url 'https://gitlab.hg.com'|" /etc/gitlab/gitlab.rb && \
	mkdir -p /etc/gitlab/ssl && \
	chmod 700 /etc/gitlab/ssl && \
	openssl req -x509 -newkey rsa:2048 -nodes -keyout /etc/gitlab/ssl/gitlab.hg.key -out /etc/gitlab/ssl/gitlab.hg.crt \
	-subj /C=FR/ST=IDF/L=PARIS/O=42/OU=THE_ASSEMBLY/CN=gitlab.hg.com
# -Remplacer l'url par defaut par notre url gitlab.
# sed : nous permet de modifier une chaine de caracteres dans un fichier.
# (https://doc.ubuntu-fr.org/sed)(https://www.grymoire.com/Unix/Sed.html)
# - Creation du fichier ssl pour y generer les cles ssh ainsi que le certificats ssl.
# (l'option -p permet de s'assurer que  chaque  répertoire  indiqué  existe)
# - openssl req : Generation du certficat ssl avec l'option -newkey pour generer les cles ssh.
# L'option -nodes permet de sortir la cle prive en clair, sans cette option "Enter PEM pass phrase:"
# s'affichera et attendra une reponse.
# https://www.openssl.org/docs/man1.1.0/man1/req.html
# https://stackoverflow.com/questions/40399690/enter-pem-pass-phrase-when-converting-pkcs12-certificate-into-pem
# https://support.dnsimple.com/articles/what-is-common-name/
RUN sed -i "s|# nginx\['redirect_http_to_https'\] = false|nginx\['redirect_http_to_https'\] = true|" /etc/gitlab/gitlab.rb && \
	sed -i "s|# nginx\['ssl_certificate'\] = \"/etc/gitlab/ssl/#{node\['fqdn'\]}.crt\"|nginx\['ssl_certificate'\] = \"/etc/gitlab/ssl/gitlab.hg.crt\"|" /etc/gitlab/gitlab.rb && \
	sed -i "s|# nginx\['ssl_certificate_key'\] = \"/etc/gitlab/ssl/#{node\['fqdn'\]}.key\"|nginx\['ssl_certificate_key'\] = \"/etc/gitlab/ssl/gitlab.hg.key\"|" /etc/gitlab/gitlab.rb
# Ajout des lignes dans le fichier de configuration de gitlab pour la redirection https et la definiton de l'emplacement
# des certificats ssl, via la commande sed qui permet de remplacer les expressions.

CMD service ssh start && (/opt/gitlab/embedded/bin/runsvdir-start &) && gitlab-ctl reconfigure && gitlab-ctl tail
# https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/docker/assets/wrapper#L112
# https://docs.gitlab.com/omnibus/settings/logs.html
#
#
#
# Gitlab a besoin d'au moins 4Go de RAM pour s'executer (https://docs.gitlab.com/ee/install/requirements.html#cpu) :
# Pour ajouter de la RAM a la machine, modifier directement les parametres de la machine dans virtualbox,
# ou tapez les commandes suivante une fois la machine ETEINTE :
# VBoxManage modifyvm Char --cpus 2
# VBoxManage modifyvm Char --memory 4096
#
# Build : docker build -t my_gitlab .
# Run : docker run -it --rm -p 80:80 -p 443:443 -p 2222:22 my_gitlab
# Acceder au serveur : https://192.168.99.101:443
# git clone : GIT_SSL_NO_VERIFY=true git clone https://192.168.99.101/path/path
# git push : GIT_SSL_NO_VERIFY=true git push
# https://gist.github.com/peschee/bbb65746088988fe3a035743804bc63e
#
# liens utiles :
# https://docs.gitlab.com/omnibus/README.html#installation-and-configuration-using-omnibus-package
# https://about.gitlab.com/install/#ubuntu
# https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-gitlab-on-ubuntu-18-04
# https://docs.gitlab.com/omnibus/settings/nginx.html#enable-https
# https://docs.gitlab.com/omnibus/settings/ssl.html
# https://about.gitlab.com/upgrade-to-package-repository/
# https://gitlab.com/gitlab-org/omnibus-gitlab/-/blob/master/docker/Dockerfile
# https://hub.docker.com/r/gitlab/gitlab-ce/dockerfile
