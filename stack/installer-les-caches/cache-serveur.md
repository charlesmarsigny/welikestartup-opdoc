---
description: Cache Nginx statique et dynamique
---

# Cache serveur



### 2 serveurs \(nginx-nginx\) :

**Le \(ou les\) serveur d'origine** est le serveur contenant les fichiers statiques réels ou le code HTML généré de manière dynamique. Il a deux responsabilités:



* Servir le contenu dynamique et statique à la demande
* Décidez comment les fichiers \(et le contenu potentiellement dynamique\) doivent être mis en cache, via les en-têtes de cache HTTP

Configuration de nginx :

**Le serveur de cache** est \(généralement\) le "leader"; Il reçoit la requête HTTP initiale d'un client. Il traite ensuite la demande lui-même \(s'il dispose d'une nouvelle copie mise en cache de la ressource demandée\) ou transmet la demande au serveur d'origine à exécuter.

```bash
sudo nano /etc/nginx/nginx.conf
```



```bash
user www-data www-data;
worker_processes 4;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
        worker_connections 768;
        # multi_accept on;
}

http {

#       fastcgi_cache_path /data/nginx/cache levels=1:2 keys_zone=phpcache:100m max_size=10g inactive=60m use_temp_pa$
#       fastcgi_cache_key "$scheme$request_method$host$request_uri";

#       map $http_upgrade $connection_upgrade {
#               default upgrade;
#               ''      close;
#       }

#       proxy_cache_path /data/cache-serveur levels=1:2 keys_zone=my_cache:10m max_size=20g inactive=60m;
#       proxy_temp_path /data/cache-serveur/tmp;

#       server {
#               location / {
#                       proxy_cache my_cache;
#                       proxy_pass http://127.0.0.1:80;
#                       proxy_pass http://unix:/var/run/welike.sock;
#                       proxy_set_header Host $host;
#                       proxy_cache_revalidate on;
#        proxy_cache_min_uses 3;
#        proxy_cache_use_stale error timeout updating http_500 http_502 http_503 http_504;
#        proxy_cache_background_update on;
#        proxy_cache_lock on;

#               }
#       }

#       add_header X-Cache $upstream_cache_status;
#       add_header X-Fait-chier Oui;

        ##
        # Basic Settings
        ##

        sendfile on;
        tcp_nopush on;
        tcp_nodelay on;
        keepalive_timeout 65;
        types_hash_max_size 2048;
        # server_tokens off;

        client_max_body_size 10M;

        # server_names_hash_bucket_size 64;
        # server_name_in_redirect off;

        include /etc/nginx/mime.types;
        default_type application/octet-stream;

        ##
        # SSL Settings
        ##

        ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
        ssl_prefer_server_ciphers on;

        ##
        # Logging Settings
        ##

        access_log /var/log/nginx/access.log;
        error_log /var/log/nginx/error.log;

        ##
        # Gzip Settings
        ##

        gzip on;

        # gzip_vary on;
        # gzip_proxied any;
        # gzip_comp_level 6;
        # gzip_buffers 16 8k;
        # gzip_http_version 1.1;
        # gzip_types text/plain text/css application/json application/javascript text/xml application/xml application$

        # Caching
#       proxy_cache_path /data/cache-serveur levels=1:2 keys_zone=cache:30m max_size=500M;
#       proxy_temp_path /data/cache-serveur/temp 1 2;
#       proxy_cache_use_stale error timeout invalid_header http_502;
#       server {
#               location / {
#                       proxy_pass http://http://unix:/var/run/welike.sock;
#               }
#       }

        ##
        # Virtual Host Configs
        ##

        include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/sites-enabled/*;
}


#mail {
#       # See sample authentication script at:
#       # http://wiki.nginx.org/ImapAuthenticateWithApachePhpScript
#
#       # auth_http localhost/auth.php;
#       # pop3_capabilities "TOP" "USER";
#       # imap_capabilities "IMAP4rev1" "UIDPLUS";
#
#       server {
#               listen     localhost:110;
#               protocol   pop3;
#               proxy      on;
#       }
#
#       server {
#               listen     localhost:143;
#               protocol   imap;
#               proxy      on;
#       }
#}
```





```text
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
```



Ne pas utiliser le socket welike.sock. Utiliser celui d'origine de PHP7.1-FPM

Configuration serveur statique :

Voir l'installation sur l'EC2 "Nginx FPM Cache OK", IP public : 15.188.50.23

```bash
sudo nano /etc/nginx/sites-available/static-elb.welikestartup.io.conf
```



```bash
proxy_cache_path /data/wordpress/cache-static levels=1:2 keys_zone=assets_zone:10m inactive=60m;
proxy_cache_key "$scheme$request_method$host$request_uri";


server {
        listen 80;
        root /data/wordpress;
        index index.php index.html index.htm index.nginx-debian.html;
        server_name _;


#       location ~ \.php$ {
#                include snippets/fastcgi-php.conf;
#                fastcgi_pass unix:/var/run/php/php7.1-fpm.sock;
#       }


        location / {
                proxy_cache assets_zone;
#                proxy_cache_bypass  $http_cache_control;
                add_header X-Proxy-Cache $upstream_cache_status;

                include proxy_params;
                proxy_pass http://127.0.0.1:81/;
                proxy_ignore_headers Cache-Control Expires Set-Cookie Vary;
        }
}
```





* Editer le fichier de configuration de Nginx :

Configuration serveur dynamique :

```bash
sudo nano /etc/nginx/nginx.conf
```

```bash
sudo nano /etc/nginx/sites-available/dynamic-elb.welikestartup.io.conf
```



```bash
fastcgi_cache_path /data/wordpress/cache-dynamic levels=1:2 keys_zone=phpcache:100m max_size=10g inactive=60m use_tem$
fastcgi_cache_key "$scheme$request_method$host$request_uri";

server {
        listen 81 default_server;
        listen [::]:81 default_server;

        root /data/wordpress;

        index index.php index.html index.htm index.nginx-debian.html;

        server_name _;


        location / {
                try_files $uri $uri/ =404;
        }


        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/var/run/php/php7.1-fpm.sock;

                include fastcgi_params;
                fastcgi_cache phpcache;
                fastcgi_cache_valid 200 301 302 10m;
                fastcgi_cache_use_stale error timeout updating invalid_header http_500 http_503;
                fastcgi_cache_min_uses 1;
                fastcgi_cache_lock on;

#               set $skip_cache 0;

                # POST requests and urls with a query string should always go to PHP
#               if ($request_method = POST) {
#                   set $skip_cache 1;
#               }
#               if ($query_string != "") {
#                   set $skip_cache 1;
#               }

                # Don't cache uris containing the following segments
#               if ($request_uri ~* "/wp-admin/|/xmlrpc.php|wp-.*.php|^/feed/*|/tag/.*/feed/*|index.php|/.*sitemap.*\$
#                   set $skip_cache 1;
#               }

                # Don't use the cache for logged in users or recent commenters
#               if ($http_cookie ~* "comment_author|wordpress_[a-f0-9]+|wp-postpass|wordpress_no_cache|wordpress_logg$
#                   set $skip_cache 1;
#               }

#               fastcgi_cache_bypass $skip_cache;
#               fastcgi_no_cache $skip_cache;

#               fastcgi_param RT_WP_NGINX_HELPER_CACHE_PATH /data/wordpress/cache-dynamic;
#               fastcgi_ignore_headers Cache-Control Expires Set-Cookie Vary;

                add_header X-FastCGI-Cache $upstream_cache_status;
        }


        # CSS and Javascript
        location ~* \.(?:css|js)$ {
                 expires 1y;
                 access_log off;
        }


}
```

* Mettre les 2 lignes suivantes dans le block `http` :



```bash
http {
    fastcgi_cache_path /data/nginx/cache levels=1:2 keys_zone=phpcache:100m max_size=10g inactive=60m use_temp_path=off;
    fastcgi_cache_key "$scheme$request_method$host$request_uri";
    
```

Passer d'une configuration à l'autre :

{% hint style="info" %}
Explications :

* Répertoire du cache : _**`/data/nginx/cache`**_
* Spécifier un répertoire de cache avec une hiérarchie à 2 niveau \(dossier principal et sous-dossiers\) : _**`levels=1:2`**_
* Nom de la zone de mémoire partagée _**`keys_zone=phpcache`**_ et sa taille _**`100m`**_
* Limite de la taille du cache _**`max_size=10g`**_ . S'il n'est pas spécifié, le peut utiliser tout l'espace disque restant.
* Temps pendant lequel les données sont gardées en cas d'inactivité du cache \(pas de visite sur l'application\) qu'elles aient expiré ou non : _**`inactive=60m`**_ \(60 minutes\) \(autres exemple : _**`12h`**_ 12 heures, _**`7d`**_ 7 jours\).
* Nginx écrit d’abord les fichiers destinés au cache dans une zone de stockage temporaire \( _**`/var/lib/nginx/fastcgi/`**_\). _**`use_temp_path=off`**_indique à Nginx de les écrire directement dans le répertoire de cache final afin d'éviter toute copie inutile de données entre systèmes de fichiers.



* La 2ème directive _**`fastcgi_cache_key`**_définit la clé de recherche du cache. Nginx appliquera une fonction de hachage MD5sum sur la clé de cache et utilisera le résultat de hachage comme nom des fichiers de cache.
{% endhint %}

```bash
sudo ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/static-elb.welikestartup.io.conf
sudo rm /etc/nginx/sites-enabled/dynamic-elb.welikestartup.io.conf
sudo service nginx restart
```





Modifier le fichier de config du site :

```bash
sudo ln -s /etc/nginx/sites-available/static-elb.welikestartup.io.conf /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/dynamic-elb.welikestartup.io.conf /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/default
sudo service nginx restart
```

```bash
sudo nano /etc/nginx/sites-availabe/default
```



Modifier le block `location ~ \.php$` :

```bash
    # pass PHP scripts to FastCGI server
    #
    location ~ \.php$ {
            include snippets/fastcgi-php.conf;
    #
    #       # With php-fpm (or other unix sockets):
            fastcgi_pass unix:/var/run/php/php7.1-fpm.sock;
    #       # With php-cgi (or other tcp sockets):
    #       fastcgi_pass 127.0.0.1:9000;
            fastcgi_cache phpcache;
            fastcgi_cache_valid 200 301 302 60m;
            fastcgi_cache_use_stale error timeout updating invalid_header http_500 http_503;
            fastcgi_cache_min_uses 1;
            fastcgi_cache_lock on;
            add_header X-FastCGI-Cache $upstream_cache_status;
    }
```

{% hint style="info" %}
Explications :

* _**`fastcgi_cache`**_ : directive qui active la mise en cache en utilisant la zone mémoire précédemment créée par la directive _**`fastcgi_cache_path`**_
* azerty
{% endhint %}









Pour voir quel socket est utilisé :

```bash
sudo systemctl status php7.1-fpm
```

{% tabs %}
{% tab title="Résultats" %}
```bash
● php7.1-fpm.service - The PHP 7.1 FastCGI Process Manager
   Loaded: loaded (/lib/systemd/system/php7.1-fpm.service; enabled; vendor preset: enabled)
   Active: active (running) since Tue 2019-10-22 11:41:25 UTC; 5 days ago
     Docs: man:php-fpm7.1(8)
 Main PID: 28190 (php-fpm7.1)
   Status: "Processes active: 0, idle: 4, Requests: 36641, slow: 0, Traffic: 0req/sec"
    Tasks: 5 (limit: 4915)
   CGroup: /system.slice/php7.1-fpm.service
           ├─ 5214 php-fpm: pool welike
           ├─ 5326 php-fpm: pool welike
           ├─28190 php-fpm: master process (/etc/php/7.1/fpm/php-fpm.conf)
           ├─28204 php-fpm: pool www
           └─28205 php-fpm: pool www

Oct 22 11:41:25 ip-172-62-42-132 systemd[1]: Starting The PHP 7.1 FastCGI Process Manager...
Oct 22 11:41:25 ip-172-62-42-132 systemd[1]: Started The PHP 7.1 FastCGI Process Manager.
```
{% endtab %}
{% endtabs %}

La pool `welike` est utilisé plutôt que celle par défaut `www`. Ce qui ne nous convient pas. Il suffit de retirer l'appel de la pool `welike` dans le fichier de config du site :

```bash
sudo nano /etc/nginx/sites-available/default
```

Supprimer ou commenter les lignes et tous ce qui se rapporte à `php-wp` :

```text
upstream php-wp {
    server            unix:/var/run/welike.sock;
}
```



#### Vérification de la configuration du pool par défaut de php-fpm

```bash
sudo nano /etc/php/7.1/fpm/pool.d/www.conf
```

Les informations importante à vérifier :

```text
# Ces lignes doivent être décommentées
user = www-data
group = www-data

listen = /run/php/php7.1-fpm.sock
```



Créer la configuration du serveur dynamique `dynamic-elb.welikestartup.io.conf` :

```bash
sudo nano /etc/nginx/sites-available/dynamic-elb.welikestartup.io.conf
```

{% tabs %}
{% tab title="dynamic-elb.welikestartup.io.conf" %}
```bash
fastcgi_cache_path /data/cache/dynamic levels=1:2 keys_zone=phpcache:100m max_size=10g inactive=60m use_temp_path=off;
fastcgi_cache_key "$scheme$request_method$host$request_uri";

server {
        listen 81 default_server;
        listen [::]:81 default_server;

        root /data/wordpress;

        index index.php index.html index.htm index.nginx-debian.html;

        server_name _;


        location / {
                try_files $uri $uri/ =404;
        }


        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/var/run/php/php7.1-fpm.sock;

                fastcgi_cache phpcache;
                fastcgi_cache_valid 200 301 302 60m;
                fastcgi_cache_use_stale error timeout updating invalid_header http_500 http_503;
                fastcgi_cache_min_uses 1;
                fastcgi_cache_lock on;
                add_header X-FastCGI-Cache $upstream_cache_status;
        }


        # CSS and Javascript
        location ~* \.(?:css|js)$ {
                 expires 1y;
                 access_log off;
                 add_header Cache-Control "public";
         }


}
```
{% endtab %}
{% endtabs %}





Créer la configuration du serveur statique `static-elb.welikestartup.io.conf` :

```bash
sudo nano /etc/nginx/sites-available/static-elb.welikestartup.io.conf
```

{% tabs %}
{% tab title="static-elb.welikestartup.io.conf" %}
```bash
proxy_cache_path /data/cache/static levels=1:2 keys_zone=assets_zone:10m inactive=60m;
proxy_cache_key "$scheme$request_method$host$request_uri";


server {
        listen 80;
        root /data/wordpress;
        index index.php index.html index.htm index.nginx-debian.html;
        server_name _;


#       location ~ \.php$ {
#                include snippets/fastcgi-php.conf;
#                fastcgi_pass unix:/var/run/php/php7.1-fpm.sock;
#       }


        location / {
                proxy_cache assets_zone;
#                proxy_cache_bypass  $http_cache_control;
                add_header X-Proxy-Cache $upstream_cache_status;

                include proxy_params;
                proxy_pass http://127.0.0.1:81/;

        }
}

```
{% endtab %}
{% endtabs %}



Mettre à jour la configuration des serveurs de Nginx :

```bash
sudo ln -s /etc/nginx/sites-available/static-elb.welikestartup.io.conf /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/dynamic-elb.welikestartup.io.conf /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/default
sudo service nginx restart
```





Problème de https : utiliser le plugin SSL Insecure Content Fixer

Utiliser le fichier de configuration par défaut en supprimant le lien des statique et dynamique dans `sites-enabled` :

```bash
sudo ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/static-elb.welikestartup.io.conf
sudo rm /etc/nginx/sites-enabled/dynamic-elb.welikestartup.io.conf
sudo service nginx restart
```



Paramétrer le plugin SSL Insecure :

{% tabs %}
{% tab title="Réglages" %}
```bash
Corriger le contenu non sécurisé :
Simple

Détection HTTPS :
impossible de détecter un protocole HTTPS
```
{% endtab %}
{% endtabs %}



Attention aux problème de modifications du contenu avec le cache fastCGI





**Source :**

{% embed url="https://serversforhackers.com/c/nginx-caching" %}

{% embed url="https://www.linuxbabe.com/redhat/install-lemp-nginx-mariadb-php7-rhel-8-centos-8" %}

{% embed url="https://www.linuxbabe.com/nginx/setup-nginx-fastcgi-cache" %}

{% embed url="https://easyengine.io/wordpress-nginx/tutorials/single-site/fastcgi-cache-with-purging/" %}



Mega Source :

{% embed url="https://www.linuxbabe.com/nginx/setup-nginx-fastcgi-cache" %}

{% embed url="https://www.linuxbabe.com/redhat/install-lemp-nginx-mariadb-php7-rhel-8-centos-8" %}

{% embed url="https://graspingtech.com/nginx-proxy-caching-wordpress/" %}



