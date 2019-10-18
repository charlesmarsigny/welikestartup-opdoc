---
description: >-
  Le module PHP-FPM permet la communication entre le serveur Nginx et PHP, basée
  sur le protocole FastCGI.
---

# Configurer l'utilisation de PHP-FPM

On ne va pas passer par un listenner sur le port 9000 comme d'habitude \(_listen = 127.0.0.1:9000_\) mais plutôt utiliser un _socket_ \(_listen = /var/run/welike.sock_\).

* Création du pool _welike_ : `sudo nano /etc/php/7.1/fpm/pool.d/welike.conf`

```text
[welike]
listen = /var/run/welike.sock

listen.owner = www-data
listen.group = www-data

user = www-data
group = www-data

pm = ondemand
pm.max_children = 500
pm.process_idle_timeout = 60s
pm.max_requests = 500
```

_On a choisi de donner le même nom à l'utilisateur et au groupe 'en référence au nom d'utilisateur et de groupe d'Apache\) mais on peut en choisir des différents._

* Choix de la directive **pm** : **ondemand** qui présente l’avantage de créer des processus en fonction de la demande et de libérer des ressources sur le serveur lors de période de faible affluence \(aucun processus fils n’est lancé au démarrage du serveur, ils s'activent à la demande\).
* Choix de **pm.max\_children** \(nombre maximum de processus fils\) : Afficher la mémoire disponible \(RAM\) : `free -m`. On peut voir la RAM disponible dans la ligne _mem_ et la colonne _available_ \(_sur notre EC2 m5d, on a 15096 M\). On choisi de prendre 13 Go pour wordpress. Affichez la mémoire utilisée par un processus fils php-fpm : `sudo systemctl start php7.1-fpm.service && ps --no-headers -o "rss,cmd" -C php-fpm7.1 | awk '{ sum+=$1 } END { printf ("%d%s\n", sum/NR/1024,"M") }'` \(_Résultat : 26M\*\) pm.max\_children = mémoire allouée \(en Mo\) / mémoire utilisée par un processus fils pm.max\_children = 13 000 / 26 = 500.
* Choix de **pm.max\_requests** : 500 est recommandé \(0 par défaut\)
* Config du virtual host par défaut \(pour le moment\) : `sudo nano /etc/nginx/sites-available/default` Ajouter / modifier :

```text
upstream php-wp {
    server            unix:/var/run/welike.sock;
}
...
root     /data/wordpress;
...
location ~ .php$ {
    include       fastcgi.conf;
    fastcgi_pass  php-wp;
}
```

* Relancer les services : `sudo systemctl restart php7.1-fpm.service` `sudo systemctl restart nginx.service`



**Source :**

{% embed url="https://howto.wared.fr/installation-wordpress-ubuntu-nginx/" %}

{% embed url="https://www.php.net/manual/fr/install.fpm.configuration.php" %}







