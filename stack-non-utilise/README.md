---
description: >-
  Apache2 est utilisé comme serveur web, Varnish comme cache, Nginx comme
  reverse-proxy. Cette configuration ne prend pas en compte l'utilisation d'un
  ELB. Utilisation d'un certificat Lets encrypt.
---

# Stack non utilisé \(Apache + Varnish + Nginx



Choisir un groupe de sécurité avec entrant HTTP, HTTPS, SSH : 0.0.0.0/0 pour l'instance EC2.

Connaitre sa version Ubuntu :  
`lsb_release -a`

## Sur Ubuntu 18.04

`sudo apt-get update`  
\(`sudo apt-get upgrade` pas pour le moment\)

### Installer Apache :

`sudo apt update`  
`sudo apt install -y apache2`

Lancer Apache \(mettre `sudo` pour éviter le pb de password\) \(image _Lancement Apache - Pb key pair_\) :  
`sudo systemctl start apache2`  
`sudo systemctl enable apache2`

Connaitre la version d'apache :  
`apache2 -v`

### Installer PHP \(defaut\) :

`sudo apt -y install php php-cgi libapache2-mod-php php-common php-pear php-mbstring` \(installe PHP7.2 par défaut\)  
`sudo a2enconf php7.2-cgi`  
`sudo nano /etc/php/7.2/apache2/php.ini`  
ligne 939 : date.zone = Europe/Paris  
`sudo systemctl restart apache2`

Test : `sudo nano /var/www/html/index.php`

```text
<html>
<body>
<div style="width: 100%; font-size: 40px; font-weight: bold; text-align: center;">
<?php
    print "PHP Test Page";
?>
</div>
</body>
</html>
```

Connaitre sa version de PHP : `php -v`

### Installer PHP 7.1 :

Pour installer PHP 7.1, il faut utiliser le dépôt PPA d'un mec \(Ondrej\) :  
`sudo add-apt-repository ppa:ondrej/php`  
`sudo apt-get update`  
Installation :  
`sudo apt install --no-install-recommends php7.1 libapache2-mod-php7.1 php7.1-mysql php7.1-curl php7.1-json php7.1-gd php7.1-mcrypt php7.1-msgpack php7.1-memcached php7.1-intl php7.1-sqlite3 php7.1-gmp php7.1-geoip php7.1-mbstring php7.1-redis php7.1-xml php7.1-zip php7.1-cgi`

Voir la version de PHP :  
`php -v` \(7.1.32\)

`sudo a2enconf php7.1-cgi`  
`sudo nano /etc/php/7.1/apache2/php.ini`  
ligne 939 : date.timezone = Europe/Paris  
`sudo systemctl restart apache2`

Test : `sudo nano /var/www/html/index.php`

```text
<html>
<body>
<div style="width: 100%; font-size: 40px; font-weight: bold; text-align: center;">
<?php
    print "PHP Test Page";
?>
</div>
</body>
</html>
```

### Firewall ufw : on le garde désactivé pour le moment.

Accès à tout le monde sur le firewall par défaut de Ubuntu UFW :  
sudo ufw allow ssh  
sudo ufw allow http  
sudo ufw allow https

Lancer ufw :  
sudo ufw enable  
Confirmer avec 'y'

### Installer PHP-FPM \(pour PHP7.1\) :

`sudo apt -y install php7.1-fpm`  
`sudo nano /etc/apache2/sites-available/default-ssl.conf`

```text
Dans/à la fin de <VirtualHost _default_:443>, taper :
    <FilesMatch \.php$>
        SetHandler "proxy:unix:/var/run/php/php7.1-fpm.sock|fcgi://localhost/"
    </FilesMatch>
```

`sudo a2enmod proxy_fcgi setenvif`  
`sudo a2enconf php7.1-fpm`  
`sudo systemctl restart php7.1-fpm apache2`  
Créer le fichier info.php :  
`sudo nano /var/www/html/info.php` : &lt;?php phpinfo\(\); ?&gt;

[https://www.vincentliefooghe.net/content/configuration-apache-24-php-fpm](https://www.vincentliefooghe.net/content/configuration-apache-24-php-fpm)  
`sudo nano /etc/apache2/sites-available/000-default.conf` \(Cas où le répertoire des sites est /var/www/html/\) :

```text
Dans/à la fin de <VirtualHost *:80>
ProxyPassMatch ^/(.*\.php)$ fcgi://127.0.0.1:9000/var/www/html/$1
```

`sudo nano /etc/php/7.1/fpm/pool.d/www.conf` :

```text
;listen = /run/php/php7.1-fpm.sock
listen = 127.0.0.1:9000
listen.allowed_clients = 127.0.0.1
```

Puis :  
`sudo systemctl restart php7.1-fpm apache2`

### Installer le cache Varnish :

Varnish fonctionne sur le port 80 et traite les requêtes HTTP entrantes, y compris celles de Nginx , directement depuis le cache ou transférées à Apache.  
Varnish ne fonctionne pas pour les demandes HTTPS entrantes. Il faut utiliser Nginx pour cela qui s'exécute donc sur le port 443.  
Apache fonctionne sur le port 8080.

Sources :  
[https://packagecloud.io/varnishcache/varnish60lts/install\#manual-deb](https://packagecloud.io/varnishcache/varnish60lts/install#manual-deb)  
[http://www.servermom.org/install-varnish-3-to-run-with-apache-2-on-ubuntu-server/](http://www.servermom.org/install-varnish-3-to-run-with-apache-2-on-ubuntu-server/)

`sudo apt-get update`

Vérifier qu'on a les bons outils :  
`sudo apt-get install curl gnupg apt-transport-https`

Installation de la clé GPG pour avoir accès aux ressources du serveur de varnishcache :  
`sudo curl -L https://packagecloud.io/varnishcache/varnish60lts/gpgkey | sudo apt-key add`

~~Ajouter à la fin du fichier de la liste des source pour apt :  
`sudo nano /etc/apt/sources.list`~~~

Créer un fichier `sudo nano /etc/apt/sources.list.d/varnishcache_varnish60lts.list`

```text
deb https://packagecloud.io/varnishcache/varnish60lts/ubuntu/ bionic main
deb-src https://packagecloud.io/varnishcache/varnish60lts/ubuntu/ bionic main
```

Mettre à jour la liste des packets apt :  
`sudo apt-get update`

Installer Varnish :  
`sudo apt-get install varnish`

Un test \(qu'on peut pas faire\) :  
[https://varnish-cache.org/docs/4.1/tutorial/starting\_varnish.html](https://varnish-cache.org/docs/4.1/tutorial/starting_varnish.html)  
`sudo nano /etc/varnish/default.vcl`  
et dans le navigateur, aller sur [http://127.0.0.1:6081/](http://127.0.0.1:6081/)

Arrêter le service varnish :  
`sudo service varnish stop` ou plutôt `sudo systemctl disable varnish --now`

Pour Ubuntu supérieur à la v15.04 :  
Dans `/etc/systemd/system/` créer le répertoire :  
`mkdir varnish.service.d`  
~~Créer le fichier `/etc/systemd/system/varnish.service.d/customexec.conf`  
`sudo nano /etc/systemd/system/varnish.service.d/customexec.conf` et y écrire :~~

Le fichier doit porté un nom précis et être créé par la méthode suivante \([https://docs.varnish-software.com/tutorials/configuring-systemd-services/](https://docs.varnish-software.com/tutorials/configuring-systemd-services/)\) :

Désactiver et arrêter le service Varnish :  
`sudo systemctl disable varnish --now`

Apache2 doit écouter sur le port 8080 au lieu du 80 :  
`sudo nano /etc/apache2/ports.conf`

```text
Listen 8080
```

Il faut aussi faire le changement pour l'hôte virtuel par défaut :  
`sudo nano /etc/apache2/sites-available/000-default.conf`  
Changer `<VirtualHost *:80>` par `<VirtualHost *:8080>`

Relancer Apache :  
`sudo service apache2 restart`

Créer un nouveau fichier de configuration qui sera nommé automatiquement \(_/etc/systemd/system/varnish.service.d/override.conf_\) :  
`sudo systemctl edit varnish.service`  
On se retrouve dans un fichier vierge où on tape les lignes suivantes :

```text
[Service]
ExecStart=
ExecStart=/usr/sbin/varnishd -a :80 -T localhost:6082 -f /etc/varnish/default.vcl -s malloc,256m
```

Problème encore présent :  
La partie `-S /etc/varnish/secret` ne peut pas être validé, il faut l'effacer pour le moment.

~~Problème : ce qui est utilisé est :~~  
~~`ExecStart=/usr/sbin/varnishd -a :6081 -f /etc/varnish/default.vcl -s malloc,256m (code=exited, status`~~

~~Alternative : écrire comme un porc dans le fichier original de la ligne ExecStart `sudo nano /lib/systemd/system/varnish.service`. Mais si on fait une mise à jour de Varnish, on perd notre configuration perso écrite en dure.~~

~~Fonctionne dans le fichier source :~~  
~~`ExecStart=/usr/sbin/varnishd -a :80 -T localhost:6082 -f /etc/varnish/default.vcl -s malloc,256m`~~  
~~Mais `-S /etc/varnish/secret` ne peut pas être mis.~~

\( \(  
_Prendre en compte les changements sans redémarrer le service varnish :_  
_`sudo systemctl daemon-reload`_

_Relancer varnish :_  
_`sudo service varnish start`_  
\) \)

Activer le service Varnish :  
`sudo systemctl enable varnish --now`

`sudo service varnish stop`

Vérifier le fichier de configuration de varnish. Il doit être écrit :  
`sudo nano /etc/varnish/default.vcl`

```text
backend default {
    .host = "127.0.0.1";
    .port = "8080";
}
```

Relancer les services :  
`sudo service apache2 restart`  
`sudo service varnish restart`

Faire apparaitre des infos dans le network de l'outil de dev de chrome :  
`sudo nano /etc/varnish/default.vcl`  
Coller le contenu de la pièce jointe `varnish/default.vcl`  
Quand on clique sur le premier fichier de la liste :  
Response Headers :  
X-Cache: HIT -&gt; vient de Varnish  
X-Cache: MISS -&gt; vient de Apache  
`sudo service varnish restart`

Statut de varnish :  
`sudo systemctl status varnish.service`

Sortie des ports et qui les utilise :  
`sudo netstat -taupen`

Statistiques de varnish :  
`sudo varnishstat`

### Créer un certificat SSL \(certainement pas nécessaire\) :

Aller sur :  
[https://certbot.eff.org/lets-encrypt/ubuntubionic-other](https://certbot.eff.org/lets-encrypt/ubuntubionic-other)  
Choisir la machine adéquate \(Ubuntu, CentOS ou Debian, etc\) et aucune version de serveur web pré configurée \(ni Apache, ni Nginx, ni etc\).

Faire :

```text
sudo apt-get update
sudo apt-get install software-properties-common
sudo add-apt-repository universe
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
```

Installation de certbot :  
`sudo apt-get install certbot`

Arrêter Nginx, Apache et Varnish :  
`sudo systemctl disable varnish --now` \(si varnish ne veu pas se lancer, c'est parceque nginx est activé et sur le port 80 `sudo service nginx stop`\).  
`sudo service apache2 stop`  
`sudo service nginx stop`

Obtention du certificat :  
`sudo certbot certonly --standalone`  
Si on ne veut pas arrêter le serveur, il faut plutôt utiliser cette commande : `sudo certbot certonly --webroot` \(moins rapide\).

Le certificat et la clé privée sont dans `/etc/letsencrypt/live/nom_du_domaine.com` \(voir pièce jointe certificats SSL...\).

### Installer le Reverse-proxy NGINX \(certainement pas nécessaire\) :

Dans cette situation, Nginx devient un proxy. Il ne fait aucun traitement du site Web, et il n'exécute pas PHP ni ne se connecte à la base de données. Il ne fait qu'accepter les demandes HTTPS et les renvoyer à Varnish. Varnish décide ensuite de restituer une copie en cache ou de la remettre à Apache pour en obtenir une nouvelle, en utilisant les règles Varnish déjà définies.

**Tests pour HTTPS, port 443 :**  
On utilise le certificat SSL qu'on a déjà créé sur Cloudflare pour \*.[welikestartup.io](http://welikestartup.io/).  
Nom de sous-domaine : [nginx.welikestartup.io](http://nginx.welikestartup.io/). Pour ce nom de domaine, on renvoie vers les adresses IP des instances EC2 qu'on teste.

Configuration dans `default.vcl` de Varnish \(voir pièce jointe\) pour obtenir des réponses HEADER visible dans l'outil de développement de Chrome, onglet Network et cliquer sur le premier élément de la liste \([nginx.welikestartup.io](http://nginx.welikestartup.io/)\)  
- `X-Cache : HIT` -&gt; la page vient de Varnish  
- `X-Cache : MISS` -&gt; la page vient de Apache

[https://www.smashingmagazine.com/2015/09/https-everywhere-with-nginx-varnish-apache/](https://www.smashingmagazine.com/2015/09/https-everywhere-with-nginx-varnish-apache/)

Installer :  
`sudo apt-get install nginx`

Supprimer ou déplacer le fichier de configuration par défaut de Nginx `/etc/nginx/sites-enabled/default`

Créer dans `/etc/nginx/sites-available/mon_domaine.com.conf` un nouveau fichier de configuration portant le nom de notre site web :  
`sudo nano /etc/nginx/sites-available/nginx.welikestartup.io.conf` \(voir pièce jointe\)\(attention : pas d'espace à la fin du fichier\).

`sudo ln -s /etc/nginx/sites-available/nginx.welikestartup.io.conf /etc/nginx/sites-enabled/nginx.welikestartup.io.conf`

Relancer les services :  
`sudo systemctl enable varnish --now`  
`sudo service apache2 start`  
`sudo service nginx start`

`sudo service nginx restart`  
Vérifier la syntax de la configuration de Nginx :  
`sudo nginx -t`

Vérifications :

* Dans le navigateur web, avoir le cadenas de connexion sécurisée : [https://nginx.welikestartup.io](https://nginx.welikestartup.io/)
* Tester la certification sur SSL Labs : [https://www.ssllabs.com/ssltest/analyze.html?d=nginx.welikestartup.io](https://www.ssllabs.com/ssltest/analyze.html?d=nginx.welikestartup.io)
* Vérifier les ports : `sudo netstat -taupen`
* Statistiques de varnish : `sudo varnishstat` Il faut qu'il y est une augmentation dans les chiffres à chaque fois qu'on recharge le site dans le navigateur.
* Regarder dans l'onglet network de l'outil de dev de chrome si on a une réponse Headers X-Cache : HIT et Server: nginx/1.14.0 \(Ubuntu\) pour être sûr que Nginx utilise le cache Varnish.

### Rediriger les requêtes HTTP vers du HTTPS :

Configuration dans `default.vcl` de Varnish \(voir pièce jointe\)  
`sudo nano /etc/varnish/default.vcl`

```text
# handles redirecting from http to https
sub vcl_synth {
  if (resp.status == 750) {
    set resp.status = 301;
    set resp.http.Location = req.http.x-redir;
    return(deliver);
  }
}
```

Et dans le block `sub vcl_recv` :

```text
if ( (req.http.host ~ "^(?i)nginx.welikestartup.io") && req.http.X-Forwarded-Proto !~ "(?i)https") {
  set req.http.x-redir = "https://" + req.http.host + req.url;
  return (synth(750, ""));
}
```

Ces lignes de commande sont déjà présentes dans la pièce jointe _default.vcl_ mais il faut vérifier et/ou remplacer `smashing_ssl_one.tutorials.eoms` par `nginx.welikestartup.io`.

Tests :

* Taper l'url avec http:// et voir si elle se transforme en https://
* Constater la redirection dans l'onglet network en cliquant sur le premier fichier : Request URL: [http://nginx.welikestartup.io/info.php](http://nginx.welikestartup.io/info.php), Status Code: 301 Moved Permanently, Location: [https://nginx.welikestartup.io/info.php](https://nginx.welikestartup.io/info.php).
* Regarder dans l'onglet network de l'outil de dev de chrome si on a une réponse Headers X-Cache : HIT et Server: nginx/1.14.0 \(Ubuntu\) pour être sûr qu'on utilise Nginx avec le cache Varnish.

