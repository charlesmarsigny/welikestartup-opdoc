---
description: >-
  Nous allons parler des bootstraps, ces scripts ou programmes d’amorçage qui
  permettent d’en lancer un plus gros, ici l'application.
---

# Les bootstraps

L'application contient de nombreuses APIs comme son API HTTP d'une grande simplicité, son API d’upload en une fonction, son API de base de données, son API de sécurité et bien d'autres.

On peut donc charger l'application sans l’interface, sans le front-end, juste pour utiliser ses APIs. Il est aussi possible de bâtir l'application sans les thèmes, on peut créer un accès membre, sans back-end etc.

> C’est ça la puissance même de l'application alimentée par WordPress.

## Bien charger l'application

L'application se charge tout seul, elle est grande ! Ce qui est vrai pour index.php par exemple. Mais l'application intègre jusqu'à 4 bootstraps en front-end ;\)

1. **index.php**
2. **wp-blog-header.php**
3. **wp-config.php \(déprécié\)**
4. **wp-load.php**

### index.php

Ce bootstrap est celui le plus utilisé, évidemment, de plus, vous n’avez rien à faire niveau code, un simple appel HTTP sur cette page et hop, l’index s’occupe de tout. Ce bootstrap est sert à charger l'application complète \(avec l'ensemble des composants de WordPress\), c’est à dire les thèmes, les plugins, la main query et toutes les APIs.

### wp-blog-header.php

En utilisant ce bootstrap, on ne chargera pas de template du thème et on ne déclenchera pas le hook `template_redirect`. La différence avec index.php est que la constante `WP_USE_THEMES` n’est pas définie, le comportement est donc déjà changé. Par contre, on charge tout de même les plugins, la main query et toutes les APIs.

### wp-config.php

{% hint style="danger" %}
Lancer l'application en faisant un appel à ce fichier est déprécié, donc à éviter.
{% endhint %}

Le soucis vient du fait que cet appel ne prends pas en compte la possibilité que le fichier _wp-config.php_ soit un rang au dessus de l’installation \(si l'architecture de fichier est modifiée\), sans être pour autant le _wp-config.php_ d’une autre installation.

### wp-load.php

Le meilleur bootstrap, le bootstrap avec lequel on chargera toutes les APIs de l'application et les plugins. Pas besoin des thèmes, pas besoin de la main query, juste le coeur de l'application, la puissance même de WordPress.  
Il existe aussi une astuce qui permet de réduire le temps de chargement et la mémoire en évitant de charger les plugins \(et mu-plugins\) et pleins d’autres APIs, juste pour avoir l'application utilisable rapidement, la constante `SHORTINIT`.

{% hint style="success" %}
On utilise déjà ce bootstrap lorsqu’une requête AJAX est faite, l'application n’a ainsi pas besoin de tout charger.
{% endhint %}

{% code-tabs %}
{% code-tabs-item title="Extrait du core /wp-admin/admin-ajax.php" %}
```php
/**
 * Executing Ajax process.
 *
 * @since 2.1.0
 */
define( 'DOING_AJAX', true );
if ( ! defined( 'WP_ADMIN' ) ) {
    define( 'WP_ADMIN', true );
}
/** Load WordPress Bootstrap */
require_once( dirname( dirname( __FILE__ ) ) . '/wp-load.php' );
```
{% endcode-tabs-item %}
{% endcode-tabs %}

[-&gt; Voir dans le fichier](https://github.com/WordPress/WordPress/blob/master/wp-admin/admin-ajax.php#L11)

## Utiliser un autre bootstrap que index.php

Vous trouverez des tutoriels et des plugins sur le net utilisant un de ces bootstraps mais malheureusement la plupart font n'importe quoi.

### Erreur à ne pas commettre

Disons que l'on est besoin de faire appel à un bootstrap depuis la racine de l'application, une simple ligne suffit.  
Mais si l'on est dans un plugin, voici ce qu’il NE FAUT PAS faire :

```php
include( '../../../wp-load.php' ); // Load WordPress
```

Pourquoi pas ! Étant dans `/wp-content/plugins/mon-plugin/` on sait qu’en remontant de 3 rangs je serais à la racine non ? NON car ceci est le chemin de l'arborescence de fichiers et les chemins peuvent être modifiés !  
Et si l'on met `/app/content/plugins/addons/myplugin/` et bien forcément … ça ne marche pas :/

{% hint style="info" %}
#### Rappel sur les structures de contrôle d'inclusion de fichier  :

* **include\(\)** : inclus un fichier, s’il n’est pas trouvé, le script déclenche un Warning et continue.
* **include\_once\(\)** : inclus un fichier une seule fois, si un second include de ce fichier \(chemin inclus\) est fait, il ne sera pas de nouveau inclus et encore, s’il n’est pas trouvé, le script déclenche un Warning et continue.
* **require\(\)** : inclus un fichier, s’il n’est pas trouvé, le script déclenche une Fatal Error et stoppe tout.
* **require\_once\(\)** : inclus un fichier une seule fois, si un second require de ce fichier \(chemin inclus\) est fait, il ne sera pas de nouveau requis et encore, s’il n’est pas trouvé, le script déclenche une Fatal Error et stoppe tout.
{% endhint %}

### Bien faire appel à un bootstrap

```php
// Load WordPress
$bootstrap = 'wp-load.php';
while( !is_file( $bootstrap ) ) {
    if( is_dir( '..' ) ) 
        chdir( '..' );
    else
        die( 'EN: Could not find WeLikeStartup App : Impossible de trouver WeLikeStartup App' );
}
require_once( $bootstrap );
```

De cette façon, on test si le bootstrap est dans le même dossier, si « non », on remonte d’un rang et recommence. Le script s’arrête quand le fichier est trouvé ou quand le `chdir()` ne peut plus remonter plus loin.

