# Problème de HTTP et HTTPS

Nginx ne gère que le trafique entrant en HTTPS  
Apache doit gérer le HTTPS en sortant

Étant donné que les visites initiales se font en HTTPS mais que Varnish requête Wordpress via le serveur web en HTTP \(port 80\), Wordpress va retourner des pages pouvant contenir des liens en HTTP \(l'inclusion des styles ou des javascripts dans certains cas particuliers\).

1. Ecrire un .htaccess contenant ces lignes \(optionnel\) :

```text
# BEGIN WordPress
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]
RewriteCond %{HTTPS} off
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
</IfModule>

# END WordPress
```

1. Dans la table wp\_options, il faut remplacer http par https pour `site_url` et `home`.
2. Modifier le fichier `wp-config.php`. Il faut taper cette ligne au début du fichier.

```text
if ($_SERVER['HTTP_X_FORWARDED_PROTO'] === 'https'){ $_SERVER['HTTPS']='on'; }
```



Le test dans l'onglet network du navigateur de varnish fonctionne :

* Le fichier php a X-Cache: MISS \(Ne vient pas du cache\)
* Les fichiers js ont X-Cache: HIT \(Les assets viennent du cache\)



**Sources :**

{% embed url="https://www.unixy.net/secure/knowledgebase/255/How-do-I-configure-WordPress-to-work-with-Varnish-SSL.html" %}

{% embed url="https://happyculture.coop/blog/drupal-8-https-et-varnish" %}



