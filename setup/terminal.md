---
description: >-
  Comment installer et configurer son terminal avec oh-my-zsh et le thème
  bullet-train.
---

# Terminal

## iTerm 2 et Homebrew pour macOS

### Installation de iTerm 2

1. Téléchargez iTerm 2 en suivant [ce lien](https://www.iterm2.com/downloads.html).
2. Allez dans votre dossier de Téléchargements et double-cliquer sur l'archive afin de la décompresser.
3. iTerm est prêt à être utiliser

### Installation de Homebrew

Homebrew est un gestionnaire de paquet pour macOS, c'est l'équivalent de apt sous Debian.

Dans le terminal \(iTerm\) :

```text
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## Installation de zsh et Oh-My-Zsh

### Installation de zsh

**Sous macOS installez zsh grâce à homebrew :**

```text
$ brew install zsh
```

**Sous Debian et ses dérives :**

```text
# apt install zsh
$ chsh -s $(wich zsh)
```

### Installation de Oh-My-Zsh

Il suffit d'utiliser `curl` pour installer la dernière version de Oh-My-Zsh.

```bash
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

#### Installation du thème zsh bullet-train

Télécharger le thème :

```bash
$ cd $ZSH_CUSTOM/themes/
```

Éditez le fichier ~/.zshrc avec la commande suivante :

```bash
$ nano ~/.zshrc
```

Trouvez la ligne **ZSH\_THEME** \(ctrl + w\) et changer pour cette configuration :

```text
#ZSH_THEME="robbyrussel"
ZSH_THEME="bullet-train"
```

{% hint style="info" %}
Le changement de thème ne s'applique qu'une fois le terminal réouvert.
{% endhint %}

#### Installation de la police Source Code Pro

Sans cette police il est possible que le thème bullet-train ne s'affiche pas correctement.

**Sous macOS :**

```bash
$ cd /tmp
$ git clone https://github.com/powerline/fonts.git --depth=1
$ cd fonts
$ ./install.sh
```

Changez ensuite la police dans votre profil iTerm \(⌘ + O &gt; Edit profiles &gt; Text &gt; Change Font\) et sélectionnez `Source Code Pro for Powerline`.

**Sous Debian et ses dérivés :**

Normalement la police de base \(monospace regular\) fonctionne bien avec le thème. Mais vous pouvez en installer Source Code Pro si vous le souhaitez :

```text
# apt install fonts-powerline
```

Changez la police dans le profil de votre terminal pour `Source Code Pro`.

#### Configurer le thème

Le thème bullet-train est configurable. Pour changer ses options par défauts il faut éditer le fichier ~/.zshrc, la liste des options est disponible ici : [https://github.com/caiogondim/bullet-train.zsh\#options](https://github.com/caiogondim/bullet-train.zsh#options)

