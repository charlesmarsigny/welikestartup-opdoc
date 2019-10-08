# Créer une installation

## Prérequis

* PHP &gt;= 5.6
* [Composer](on-mac.md)

## Installation

Nous utilisons un package appelé [Bedrock](https://github.com/roots/bedrock) pour construire notre application.

1. Créer un nouveau projet :

```bash
composer create-project roots/bedrock
```

```text
ou 
```

```bash
php composer.phar create-project roots/bedrock
```

```text
puis
```

```text
mv bedrock/.* ./
rmdir bedrock
```

1. Copier `.env.example` en `.env` et mettre à jour les variables d'environnement :

```php
DB_NAME       # → Nom de la base de données
DB_USER       # → Nom d'utilisateur de la base
DB_PASSWORD   # → Mot de passe de la base
DB_HOST       # → Hôte de la base
WP_ENV        # → Environnement (development, staging, production)
WP_HOME       # → URL complète (http://example.com)
WP_SITEURL    # → URL complète avec le(s) sous-répertoire(s) (http://example.com/web/wp)

AUTH_KEY
SECURE_AUTH_KEY
LOGGED_IN_KEY
NONCE_KEY
AUTH_SALT
SECURE_AUTH_SALT
LOGGED_IN_SALT
NONCE_SALT
# → Generate with wp-cli-dotenv-command or from the Roots WordPress Salt Generator
```

Generate with [wp-cli-dotenv-command](https://github.com/aaemnnosttv/wp-cli-dotenv-command) or from the [Roots WordPress Salt Generator](https://cdn.roots.io/salts.html)

1. Ajouter le\(s\) thème\(s\) de l'application dans `web/app/themes` 
2. Set your site vhost document root to `/path/to/site/web/` \(`/path/to/site/current/web/` if using deploys\)
3. Access WP admin at `http://example.com/wp/wp-admin`

## Déploiement

## Structure de répertoires

```bash
├── composer.json             # → Manage versions of WordPress, plugins & dependencies
├── config                    # → WordPress configuration files
│   ├── application.php       # → Primary WP config file (wp-config.php equivalent)
│   └── environments          # → Environment specific configs
│       ├── development.php   # → Development config
│       ├── staging.php       # → Staging config
│       └── production.php    # → Production config
├── vendor                    # → Composer packages (never edit)
└── web                       # → Web root (vhost document root)
    ├── app                   # → wp-content equivalent
    │   ├── mu-plugins        # → Must use plugins
    │   ├── plugins           # → Plugins
    │   ├── themes            # → Themes
    │   └── uploads           # → Uploads
    ├── wp-config.php         # → Required by WP (never edit)
    ├── index.php             # → WordPress view bootstrapper
    └── wp                    # → WordPress core (never edit)
```

## Fichiers de configuration

## Variables d'environnements

