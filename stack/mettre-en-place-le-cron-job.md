---
description: Il faut que le cronjob soit appliqué sur une seule instance
---

# Mettre en place le cron job

* Le service de cron de l'instance est actif par défaut =&gt; Il faut l'arrêter avant de créer l'AMI pour l'auto scaling.

```bash
sudo service cron status # checks if cron is running
sudo service cron start  # starts it
sudo service cron stop   # stops it
```

* Désactive le déclenchement du cron Wordpress pour utiliser un vrai cron jobs. Ca évite le déclenchement par deux systèmes concurrent \(visites et cron externe\) :

```bash
sudo nano /data/wordpress/wp-config.php
```

```php
// Ajouter juste après define( 'WP_DEBUG', false); :
define('DISABLE_WP_CRON', true);
```

* Editer la configuration du cron du serveur :

```bash
sudo crontab -e
```

Choisir l'éditeur nano dans la liste et ajouter à la fin du fichier :

```bash
SHELL=/bin/bash
HOME=/
MAILTO="guillaume.dubois@welikestartup.fr"
# Test : cron run this commande every minute
* * * * * /usr/bin/php /data/wordpress/wp-cron.php
# Test : send 'Coucou, c'est le cron' to guillaume.dubois@welikestartup.fr
# * * * * * /usr/bin/php /data/wordpress/wp-cron.php
```

Redémarrer le service de cron :

```bash
sudo service cron restart
```

### Tester le cron :

* Créer un fichier `compteur.txt` par exemple dans le thème actif de wordpress. Lui donner comme propriétaire `www-data` \(utilisateur du serveur nginx\).
* Installer le plugin **Advanced Cron Manager** pour gérer les tâche cron de l'application. Y créer un évènement \(hook\) nommé `test_cron_hook` exécuté toutes les 2 minutes \(le cronjob de l'instance est réglé sur toutes les minutes\).
* Ajouter à la fin du fichier `functions.php` :

```php
// Hook
add_action( 'test_cron_hook', 'test_cron' );
// Function
function test_cron() {
        define ('Path',ABSPATH.'wp-content/themes/twentynineteen/');
        $monfichier = fopen(Path.'compteur.txt', 'a+');
        if ( $monfichier != false) {echo 'ouvert';}
        else {echo 'echec ouverture';}
        fputs($monfichier, 'Texte à écrire ' . date("Y-m-d H:i:s") .  "\n");
        fclose($monfichier);
}
```

{% hint style="success" %}
**Écrie toutes les 2 minutes :**  
"Texte à écrire année-mois-jour heure:minute:seconde actuel".
{% endhint %}



* Créer une nouvelle clé d'API dans le tableau de bord de l'application sendgrid.
* Installer le plugin **sendgrid** sur notre wordpress. Dans la configuration : coller la clé d'API généré et choisir de mettre comme adresse d'envoi [**elb.welikestartup.io**](http://elb.welikestartup.io/).
* Ajouter à la fin du fichier `functions.php` :

```php
// Function to send an HTML mail
function test_mail_cron() {
// destinataires
     $to  = 'guillaume.dubois@welikestartup.fr'; // notez la virgule

     // Sujet
     $subject = 'Calendrier des anniversaires pour Août';

     // message
     $message = '
Tata
     ';

        // Envoi
        wp_mail($to, $subject, $message);

}
```

{% hint style="success" %}
**Envoi un mail toutes les 2 minutes**
{% endhint %}

\*\*\*\*

**Sources :**

{% embed url="https://www.digitalocean.com/community/tutorials/how-to-use-cron-to-automate-tasks-on-a-vps" %}

{% embed url="https://www.digitalocean.com/community/questions/set-up-cron-job-ubuntu-nginx-server%0Ahttps://www.100son.net/cron-wordpress-problemes-solutions/" %}



