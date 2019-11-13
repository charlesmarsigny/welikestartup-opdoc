---
description: Cache OpCache
---

# Cache front-end

OpCache est fourni avec php. Il suffit de l'activer et de le configurer.

```bash
sudo nano /etc/php/7.1/fpm/php.ini
```

{% tabs %}
{% tab title="Modifications dans le fichier php.ini" %}
```bash
# Il faut décommenter les lignes suivantes et modifier leurs valeurs si besoin.
opcache.enable=1 
opcache.enable_cli=1 
opcache.interned_strings_buffer=8 
opcache.max_accelerated_files=10000 
opcache.memory_consumption=128 
opcache.save_comments=1 
opcache.revalidate_freq=1
```
{% endtab %}
{% endtabs %}

Redémarrer le service PHP-FPM :

```bash
sudo service php7.1-fpm restart
```

Redémarrer Nginx :

```bash
sudo service nginx restart
```

### Test : 

Faire un **var\_dump\(opcache\_get\_status\(\)\);** dans le fichier **header.php** : On obtient un tableau des caractéristiques de OpCache si c'est bon ou false s'il y a un problème.



**Sources :**

{% embed url="https://help.nextcloud.com/t/how-to-configure-opcache-and-nginx-without-modifying-global-php-ini/34905" %}

{% embed url="https://lastplaceonthe.net/how-to-enable-php-7-opcache-on-ubuntu-16-04/" %}

{% embed url="https://www.php.net/manual/fr/book.opcache.php" %}

{% embed url="https://www.php.net/manual/fr/function.opcache-get-status.php" %}





