---
description: Installation d'atom et installation d'une préconfiguration.
---

# Atom Quickstarter

## Installation d'Atom

Télécharger atom :

* macOS : [https://atom.io/download/mac](https://atom.io/download/mac)
* Debian et dérivés : [https://atom.io/download/deb](https://atom.io/download/deb)

Une fois téléchargé, lancez l'installation.

## Installation de la configuration

Télécharger la [préconfiguration](https://slack-files.com/T277LFVDF-FB1RGNGSH-b936bdf3de) atom.

Décompressez l'archive, puis dans le terminal :

```text
$ rm -rf ~/.atom
$ mv ~/Downloads/.atom ~/
```

{% hint style="warning" %}
Adaptez le chemin `~/Downloads/` selon votre configuration pour correspondre au dossier où vous avez décompressé l'archive.
{% endhint %}

Vous pouvez lancez Atom.

Au lancement d'atom vous aurez un message vous demandant de désactiver Linter ou Diagnostic, sélectionnez Linter.

### Dépendances de la préconfiguration

Selon votre environnement, en lançant vous aurez des messages vous demandant d'installer des dépendances, acceptez les et redémarrez atom.

Certaines dépendances ont besoin d'être installé manuellement :

**Avec Homebrew :**

```text
$ brew install php-cs-fixer
```

**Avec composer :**

```text
$ composer global require friendsofphp/php-cs-fixer
$ export PATH="$PATH:$HOME/.composer/vendor/bin"
```

**Avec curl :**

```text
$ cd /tmp && curl -L https://cs.sensiolabs.org/download/php-cs-fixer-v2.phar -o php-cs-fixer
```

## Aller plus loin

### Utiliser les raccourcis clavier

Voici quelques raccourcis utiles :

| shift + ⌘ + p | Afficher la palette de commande |
| :--- | :--- |
| ⌘ + / | Commenter/Décommenter une ou plusieurs lignes |
| ⌘ + p | Aller à un fichier |
| ctrl + g | Aller à un numéro de ligne |
| ⌘ + F2 | Mettre un marque-page |
| F2 | Aller à un marque-page |

### Changer la police

Installer le plugin fonts

```text
$ apm install fonts
```

Puis dans les paramètres \(⌘ + ,\), allez dans packages, cherchez fonts, puis les paramètres du plugin sélectionnez une police

{% hint style="info" %}
Pour utiliser la même que dans la configuration du terminal, sélectionnez Source Code Pro.
{% endhint %}

