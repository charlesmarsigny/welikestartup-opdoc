# Installer Nginx

Ubuntu 18.04

`sudo apt update` `sudo apt upgrade` `sudo apt-get update` `sudo apt-get upgrade`

_Source :_ [https://howto.wared.fr/installation-wordpress-ubuntu-nginx/](https://howto.wared.fr/installation-wordpress-ubuntu-nginx/) \_\_

`sudo apt-get -y install nginx`

Configuration :

Modifier le nombre de workers en mettant autant de workers que de coeurs disponible sur le serveur. Connaitre le nombre de coeurs : `grep processor /proc/cpuinfo | wc -l` -&gt; _4_ pour notre EC2 m5d.

Editer le fichier de config d'Nginx : `sudo nano /etc/nginx/nginx.conf`

```text
worker_processes 4;
server_tokens off;
```

Par mesure de sécurité, bloquer l'envoi d'informations telles que le numéro de version de notre Nginx \(Décommenter _server\_tokens off;_\).

Connaitre le document root : `grep -R "root" /etc/nginx/sites-enabled`

