# Cache serveur avec Varnish

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

```text
sudo curl -L https://packagecloud.io/varnishcache/varnish60lts/gpgkey | sudo apt-key add -
```

Créer un fichier :

```bash
sudo nano /etc/apt/sources.list.d/varnishcache_varnish60lts.list
```

```bash
deb https://packagecloud.io/varnishcache/varnish60lts/ubuntu/ bionic main
deb-src https://packagecloud.io/varnishcache/varnish60lts/ubuntu/ bionic main
```

Mettre à jour la liste des packets apt :  
`sudo apt-get update`

Installer Varnish :  
`sudo apt-get install varnish`





**Source :**

{% embed url="https://packagecloud.io/varnishcache/varnish60lts/install\#manual-deb" %}

{% embed url="https://www.linode.com/docs/websites/varnish/use-varnish-and-nginx-to-serve-wordpress-over-ssl-and-http-on-debian-8/" %}





## Notabene : Varnish standard, c'est mieux

En installant Varnish normalement sans aller chercher des packages spécifiques pour une version version spécifique \(varnish-6.0.5 lts\), il suffit de faire :

```bash
sudo apt-get update
sudo apt-get install varnish

# Pour connaitre la version de notre Varnish :
varnishd -V

sudo systemctl disable varnish --now
```

On obtient la version varnish-5.2.1 avec laquelle sont fournis les fichiers qui n'existent pas dans la version 6 et qu'on retrouve pourtant partout dans les docs et tutos sur le web.

```bash
# Ces fichiers existent bien dans la version standard de Varnish :
sudo nano /etc/default/varnish
sudo nano /lib/systemd/system/varnish.service
sudo nano /etc/varnish/secret
# Ce qui n'était pas le cas avec Varnish 6 lts
```





```bash
sudo cp -r /var/www/html/wordpress/wp-content/plugins/vcaching/varnish-conf/v5/conf /etc/varnish/
```



Récupérer les fichiers de config de Varnish depuis le répertoire du plugin "vcaching" :

```bash
# Changer le nom de default.vcl d'origine pour pas l'écraser
sudo mv /etc/varnish/default.vcl /etc/varnish/default-original.vcl

# Copier coller les fichiers du plugin
sudo cp -r /data/wordpress/wp-content/plugins/vcaching/varnish-conf/v5/conf /etc/varnish/
sudo cp -r /data/wordpress/wp-content/plugins/vcaching/varnish-conf/v5/lib /etc/varnish/
sudo cp /data/wordpress/wp-content/plugins/vcaching/varnish-conf/v5/default.vcl /etc/varnish/


```



### Adapter les fichiers de config de Varnish à notre stack :

```bash
sudo nano /etc/varnish/lib/purge.vcl
# Changer la clé de purge
set req.http.X-VC-My-Purge-Key = "chnhebq8vhelat20u8au0dem1svgfwy89cd4561zcytclo2mfa7xhkb1tpyj1qvk";

sudo nano /etc/varnish/conf/acl.vcl
acl purge {
	"localhost";
	"127.0.0.1";
	# IP privé de l'instance
	"172.62.63.169";
	# Mettre le CIDR des sous réseaux privés
	
sudo nano /etc/varnish/conf/backend.vcl
backend backend1 {
	.host = "127.0.0.1";
	.port = "8080";
}
```





### Remplacer le port 6081 par le 80 :

Modifier le fichier `sudo nano /etc/default/varnish`:

```bash
DAEMON_OPTS="-a :80 \
             -T localhost:6082 \
             -f /etc/varnish/default.vcl \
             -S /etc/varnish/secret \
             -s malloc,256m"
# 256m : taille de la mémoire réservé pour le cache
```

```bash
sudo nano /lib/systemd/system/varnish.service

# Modifier la ligne ExecStart :
ExecStart=/usr/sbin/varnishd -j unix,user=vcache -F -a :80 -T localhost:6082 -f /etc/varnish/default.vcl -S /etc/varnish/secret -s malloc,256m
```

```bash
# Recharger systemd
sudo systemctl daemon-reload
```

```bash
sudo nano /etc/nginx/fastcgi_params

# Ajouter à la fin du fichier :
fastcgi_param  SCRIPT_FILENAME    $request_filename;
fastcgi_param  PATH_INFO          $fastcgi_path_info;

```



Adapter Nginx :

```bash
sudo nano /etc/nginx/sites-available/varnish.conf
listen 8080;

sudo service nginx restart
sudo service varnish start
```









Sources :

A ne pas suivre entièrement :

{% embed url="https://www.linode.com/docs/websites/varnish/use-varnish-and-nginx-to-serve-wordpress-over-ssl-and-http-on-debian-8/" %}









