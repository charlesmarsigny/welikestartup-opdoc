---
description: 'Nginx ne permet, par défaut, d''uploader que des fichiers < 1 Mo.'
---

# Configurer l'upload de gros fichiers

Augmenter la taille des fichiers uploadés :

* **Pour Nginx :**

`sudo nano /etc/nginx/nginx.conf`

```text
http {
    ...
    client_max_body_size 10M;
}
```

_Il est possible de restreindre la zone autorisé de stockage de gros fichier. Au lieu de cibler tous les block du serveur \(virtual hosts\) en visant le block http, on peut choisir de l'appliquer qu'à un répertoire en particulier \(ex : upload\_max\_filesize = 10M;`location /uploads { client_max_body_size 10M;}` \)._

\_\_

* **Pour PHP :** Il faut changer la taille acceptée par PHP : `sudo nano /etc/php/7.1/fpm/php.ini` Chercher \(Ctrl + w dans nano\) **upload\_max\_filesize** et changer sa valeur \(ex : upload\_max\_filesize = 10M\).



* Relancer les services **nginx et php-fpm** : `sudo systemctl restart php7.1-fpm.service` `sudo systemctl restart nginx.service`

