---
description: Une fois que wordpress est installé
---

# Droits d'accès aux fichiers

Les répertoires doivent avoir des droits d'accès 755 et les fichiers 644.

Les fichiers sont certainement détenus par l'utilisateur root. Il faut les réduire à un utilisateur et un groupe.  
Sous Apache, le propriétaire est `www-data` et le groupe `www-data`.   
Sous Nginx le propriétaire est aussi  `www-data` , il est possible de le changer. On choisit  `www-data` comme groupe.



* **Pour une instance utilisant un NVMe \(md5.xlarge\) :**

`sudo chown -R www-data:www-data /data/wordpress`

Répertoire wordpress 755  
`find /data/wordpress -type d -exec chmod 755 {} +`  
_L'argument -type d indique que l'on recherche les directories \(répertoires\)._

Fichiers wordpress 644  
`find /data/wordpress -type f -exec chmod 644 {} +`  
_L'argument -type f indique que l'on recherche les files \(fichiers\)._

\_\_

* **Pour une instance utilisant l'EBS de base \(t2.micro\) :**

`sudo chown -R www-data:www-data /var/www/html/wordpress`

Répertoire wordpress 755  
`sudo find /var/www/html -type d -exec chmod 755 {} +`  
_L'argument -type d indique que l'on recherche les directories \(répertoires\)._

Fichiers wordpress 644  
`sudo find /var/www/html -type f -exec chmod 644 {} +`  
_L'argument -type f indique que l'on recherche les files \(fichiers\)._

\_\_

{% hint style="info" %}
**Pour aller plus loin consultez la page** [Gérer les utilisateurs et les groupes](https://app.gitbook.com/@welikestartup/s/app-operations/~/edit/drafts/-LrSpdYmWxMcyupIK61l/v/master/pour-approfondir/gerer-les-utilisateurs-et-les-groupes)
{% endhint %}

\*\*\*\*

**Source :**

{% embed url="https://www.skyminds.net/wordpress-accorder-les-bonnes-permissions-aux-fichiers-et-dossiers-avec-chown-et-chmod/" %}



