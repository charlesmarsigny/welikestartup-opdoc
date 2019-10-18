# Installer PHP-FPM7.1

`sudo apt -y install php7.1-fpm`

Il faut en tout cas avoir :

* PHP PDO : connecteur pour MariaDB
* PHP CURL : nécessaire pour certains plugins
* PHP GD : opérations sur les images
* PHP INTL : support de l’internationalisation. `sudo apt-get -y install php7.1-cli php7.1-mysql php7.1-curl php7.1-gd php7.1-intl` `sudo apt-get -y install php-cli php-mysql php-curl php-gd php-intl` \(dernière version\)

**Source :**

{% embed url="https://howto.wared.fr/installation-wordpress-ubuntu-nginx/" %}

{% embed url="https://www.php.net/manual/fr/install.fpm.configuration.php" %}





