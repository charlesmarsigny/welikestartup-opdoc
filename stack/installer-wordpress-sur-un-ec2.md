---
description: Installation d'un wordpress vierge à partir du terminal
---

# Installer Wordpress sur un EC2

Récupérer wordpress :  
`sudo wget https://wordpress.org/latest.tar.gz`

Dézipper wordpress :  
`sudo tar -xzf latest.tar.gz`

Déplacer le dossier **wordpress** :  
`sudo mv /home/ubuntu/wordpress /var/www/html/`

Créer le fichier **wp-config.php** :  
`sudo cp wp-config-sample.php wp-config.php`

Modifier le fichier **wp-config.php** :  
`sudo nano wp-config.php`

```php
define( 'DB_NAME', '' );
define( 'DB_USER', 'admin' );
define( 'DB_PASSWORD', 'admin1234' );
define( 'DB_HOST', 'nginx-test.c9r2tjio6pcx.eu-west-3.rds.amazonaws.com' );

Coller la clé d'authentification depuis https://api.wordpress.org/secret-key/1.1/salt/
define('AUTH_KEY', ...
```

### **Attention !!! :** 

**Pour utiliser la même BDD qu'une autre installation il faut utiliser la même clé d'authentification que cette install \(**_**define\('AUTH\_KEY' , SECURE\_AUTH\_KEY, LOGGED\_IN\_KEY, NONCE\_KEY, AUTH\_SALT, ...**_**\).**



**Pour voir si on peux se connecter à la BDD depuis le serveur :**  
installer mycli :  
`sudo apt-get update`  
`sudo apt-get install mycli`  
\)\)

**Test :** `mycli mysql:://admin@test-nginx.c9r2tjio6pcx.eu-west-3.rds.amazonaws.com/test2` 

_**=&gt; Ne marche pas même avec une bonne configuration.**_

_\*\*\*\*_

**Groupe de sécurité dans le cas d'une instance EC2 sans ELB :**   
Les règles MySQL/Aurora doivent être faites pour les IPs publique et privée de l'instance EC2.



**Source :**

{% embed url="https://howto.wared.fr/installation-wordpress-ubuntu-nginx/" %}



