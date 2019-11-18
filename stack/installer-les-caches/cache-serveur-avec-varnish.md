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





