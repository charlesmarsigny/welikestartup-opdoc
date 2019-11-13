---
description: Cache statique et dynamique avec Nginx
---

# Cache serveur avec Nginx

Caches statique et dynamique



Configuration de nginx :

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





Configuration serveur statique :

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



Configuration serveur dynamique :

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



Passer d'une configuration à l'autre :

```bash
sudo ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/static-elb.welikestartup.io.conf
sudo rm /etc/nginx/sites-enabled/dynamic-elb.welikestartup.io.conf
sudo service nginx restart
```



```bash
sudo ln -s /etc/nginx/sites-available/static-elb.welikestartup.io.conf /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/dynamic-elb.welikestartup.io.conf /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/default
sudo service nginx restart
```



