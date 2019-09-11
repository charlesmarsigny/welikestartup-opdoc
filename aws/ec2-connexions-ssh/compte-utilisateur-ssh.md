# Compte utilisateur SSH

## Noms des utilisateurs par défaut selon l'AMI d'instance utilisée

> Chaque type d'instance Linux est lancé avec un compte d'utilisateur du système Linux par défaut. Le nom d’utilisateur par défaut est déterminé par l’AMI qui a été spécifiée au moment du lancement de l’instance.

{% hint style="info" %}
​Chaque type d'instance Linux est lancé avec un compte d'utilisateur du système Linux par défaut. Le nom d’utilisateur par défaut est déterminé par l’AMI qui a été spécifiée au moment du lancement de l’instance.
{% endhint %}



* Amazone Linux 2 ou Amazon Linux : `ec2-user`
* Ubuntu : `ubuntu`
* Debian : `admin` ou `root`
* CentOS : `centos`
* Fedora : `ec2-user` ou `fedora`
* RHEL : `ec2-uer` ou `root`
* SUSE : `ec2-user` ou `root`

## Gestion des comptes d'utilisateur sur votre instance Linux <a id="managing-users"></a>

> Il est possible de créer et supprimer d'autres comptes utilisateur.

{% embed url="https://docs.aws.amazon.com/fr\_fr/AWSEC2/latest/UserGuide/managing-users.html" %}



