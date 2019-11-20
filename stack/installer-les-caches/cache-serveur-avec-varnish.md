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
sudo systemctl daemon-reload
```



```bash
cp -r /var/www/html/wordpress/wp-content/plugins/vcaching/varnish-conf/v5/conf /etc/varnish/
```











