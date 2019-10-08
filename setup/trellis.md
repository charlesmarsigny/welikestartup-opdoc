---
description: Comment installer l'application avec Trellis et l'architecture Bedrock
---

# Trellis

## Dépendances

* [Ansible](https://docs.ansible.com/ansible/intro_installation.html#latest-releases-via-pip) &gt;= 2.4.0
* [Virtualbox](https://www.virtualbox.org/wiki/Downloads) &gt;= 4.3.10
* [Vagrant](https://www.vagrantup.com/downloads.html) &gt;= 1.8.5

{% hint style="info" %}
Changements nécessaires : [https://github.com/treyssatvincent/wp-content/compare/bedrock-trellis](https://github.com/treyssatvincent/wp-content/compare/bedrock-trellis)
{% endhint %}

## Déploiement en développement

### Installation de la machine virtuelle

D'abord on récupère l'architecture.

```bash
git clone git@???.git && cd dev.app.welikestartup.io/trellis
```

On récupère le mot de passe de chiffrement

```bash
echo "lemotdepasse" > .vault_pass
```

On ajoute une sauvegarde de la base de données à `../site/wls_dev.sql`

```bash
???
```

On ajoute les uploads

???

On lance la création de la machine virtuelle

```bash
vagrant box update
vagrant up
```

{% hint style="info" %}
Soyez patient, cela prend du temps. Mais soyez attentif, vous allez devoir mettre le mot de passe système pendant le processus.
{% endhint %}

### Configuration de la machine virtuelle

La logique de Trellis veux que les actions avec Composer et WP-CLI soient fait dans la VM. Il faut donc se connecter à la VM

```bash
vagrant ssh
cd /srv/www/dev.app.welikestartup.io/current/
```

Import de la base de données

```bash
wp db import wls_dev.sql
```

Installation des plugins

```bash
composer update
```

Si votre base de données contient des urls ne correspondant pas à dev.app.welikestartup.io pensez à faire un search and replace :

```bash
wp search-replace 'app.welikestartup.io' 'dev.app.welikestartup.io'
```

{% hint style="success" %}
Testez votre application à [https://dev.app.welikestartup.io](https://dev.app.welikestartup.io)
{% endhint %}

### Aller plus loin

#### Utiliser Sequel Pro

Installer le plugin Vagrant pour Sequel Pro

```bash
vagrant plugin install vagrant-trellis-sequel
```

Il suffit de lancer la commande suivante dans le répertoire de Trellis :

```bash
vagrant trellis-sequel open
```

Remplacez l'utilisateur par `wls` et la base par `wls_dev`.

#### Faire confiance au certificat en local

Installer le plugin Vagrant

```bash
vagrant plugin install vagrant-trellis-cert
```

Il suffit de lancer la commande suivante dans le répertoire de Trellis :

```bash
vagrant trellis-cert trust
```

Mettez votre mot de passe système quand il vous est demandé.

{% hint style="info" %}
Pour être pris en compte, redémarrer votre ordinateur.
{% endhint %}

## Déploiement en production

