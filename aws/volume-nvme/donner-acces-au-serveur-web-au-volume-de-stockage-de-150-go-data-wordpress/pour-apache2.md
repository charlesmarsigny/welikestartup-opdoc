# Pour apache2



* `sudo nano /etc/apache2/apache2.conf`

  Ajouter l'accès à **wordpress** dans **/data** mais pas à **/data** lui même :

```text
<Directory /data/wordpress />
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
</Directory>
```

* `sudo nano /etc/apache2/sites-available/000-default.conf` : Changer le **DocumentRoot** : `DocumentRoot /data/wordpress`
* `sudo nano /etc/apache2/sites-available/default-ssl.conf` : Changer le **DocumentRoot** : `DocumentRoot /data/wordpress`
* Relancer les services apache et fpm : `sudo systemctl restart php7.1-fpm apache2`

