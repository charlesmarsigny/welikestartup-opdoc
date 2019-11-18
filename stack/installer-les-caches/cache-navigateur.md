---
description: Cache Nginx
---

# Cache navigateur ou d'origine

### Configurations :

Editer le fichier **default** :

```bash
sudo nano /etc/nginx/sites-available/default
```

```bash
map $sent_http_content_type $expires {
    default                    off;
    # epoch : Force le naviguateur à demander la version à jour de ce fichier
    text/html                  epoch;
    # max : Temps de cache maximal offert par le navigateur
    # On peut mettre max ou un temps en seconde
    text/css                   max;
    application/javascript     max;
    ~image/                    max;
}

server {
        listen 80 default_server;
        listen [::]:80 default_server;

        expires $expires;
```

Vérifier qu'il n'y a pas d'erreur dans la configuration de Nginx \(nginx.conf\) :

```bash
sudo nginx -t
```

Redémarrer Nginx pour appliquer les changements :

```bash
sudo service nginx restart
```





Filtrer les mises en cache



Purge du cache

Plugin Nginx Helper





### Tests : 

Créer des fichiers html, jpg, css et js dans le répertoire par défaut \(/data/wordpress\) :

```bash
# Option -s = SIZE : permet de définir la taille du fichier
sudo truncate -s 3k /data/wordpress/cacheTest.html
sudo truncate -s 3k /data/wordpress/cacheTest.jpg
sudo truncate -s 3k /data/wordpress/cacheTest.css
sudo truncate -s 3k /data/wordpress/cacheTest.js
```

#### 

```bash
# Option -s = SIZE : permet de définir la taille du fichier
sudo truncate -s 3k /var/www/html/cacheTest.html
sudo truncate -s 3k /var/www/html/cacheTest.jpg
sudo truncate -s 3k /var/www/html/cacheTest.css
sudo truncate -s 3k /var/www/html/cacheTest.js
```



#### Vérification du header HTTP par défaut **avant la configuration du cache** :

```bash
curl -I http://localhost/cacheTest.html
```

{% code title="Réponse" %}
```bash
HTTP/1.1 200 OK
Server: nginx/1.10.1 (Ubuntu)
Date: Wed, 19 Oct 2016 03:30:35 GMT
Content-Type: text/html
Content-Length: 1024
Last-Modified: Wed, 19 Oct 2016 03:10:54 GMT
Connection: keep-alive
ETag: "5806e43e-400"
Accept-Ranges: bytes
```
{% endcode %}

Le header **ETag** correspond à un ID unique de la version du fichier demandé \(cacheTest.html\).

```bash
curl -I -H 'If-None-Match: "5806e43e-400"' http://localhost/cacheTest.html
```

{% code title="Réponse" %}
```bash
HTTP/1.1 304 Not Modified
Server: nginx/1.10.1 (Ubuntu)
Date: Wed, 19 Oct 2016 03:40:31 GMT
Last-Modified: Wed, 19 Oct 2016 03:10:54 GMT
Connection: keep-alive
ETag: "5806e43e-400"
```
{% endcode %}

Réponse 304 : ce qui veut dire que le fichier n'a pas été modifier et que le serveur ne renverra pas le fichier mais indique au navigateur qu'il peut réutiliser le fichier déjà téléchargé. Ceci est bien pour faire du caching mais pas assez car même si le serveur ne renvoie pas le fichier, le navigateur lui envoie à chaque fois une requête demandant s'il peut réutiliser le fichier.



#### **Résultats** du header HTTP par défaut **après la configuration du cache** :

```bash
curl -I http://localhost/cacheTest.html
```

{% code title="Réponse" %}
```bash
HTTP/1.1 200 OK
Server: nginx/1.10.1 (Ubuntu)
Date: Wed, 19 Oct 2016 16:28:50 GMT
Content-Type: text/html
Content-Length: 1024
Last-Modified: Wed, 19 Oct 2016 03:10:54 GMT
Connection: keep-alive
ETag: "5806e43e-400"
Expires: Thu, 01 Jan 1970 00:00:01 GMT
Cache-Control: no-cache
Accept-Ranges: bytes
```
{% endcode %}

**Pas de cache sur les fichiers html.**

```bash
curl -I http://localhost/cacheTest.css
```

{% code title="Réponse" %}
```bash
HTTP/1.1 200 OK
Server: nginx/1.10.1 (Ubuntu)
Date: Wed, 19 Oct 2016 16:39:29 GMT
Content-Type: text/css
Content-Length: 1024
Last-Modified: Wed, 19 Oct 2016 03:11:06 GMT
Connection: keep-alive
ETag: "5806e44a-400"
Expires: Thu, 31 Dec 2037 23:55:55 GMT
Cache-Control: max-age=315360000
Accept-Ranges: bytes
```
{% endcode %}

**Cache actif** sur les fichier CSS. **Cache-Control** donne la durée du cache**. Expire** donne la date d'expiration.



**Sources :** 

{% embed url="https://www.supinfo.com/articles/single/2843-implementer-cache-navigateur-avec-nginx" %}

{% embed url="https://linoxide.com/linux-how-to/use-truncate-command-linux/" %}

\*\*\*\*











