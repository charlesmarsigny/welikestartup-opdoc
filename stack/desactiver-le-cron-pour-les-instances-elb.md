---
description: A faire sur toutes les instances sauf celle du cron
---

# Désactiver le cron pour les instances ELB

Lors d'un reboot d'une instance ou du lancement d'une nouvelle, le cronjob est actif par défaut. Ce qui est à éviter pour toutes les instances sauf celle du cron.

Il faut modifier les paramètres de lancement au démarrage du service de cron :

```bash
sudo update-rc.d cron disable
```



{% hint style="info" %}
Détails sur [Gestion du lancement de services au démarrage de l'instance](https://app.gitbook.com/@welikestartup/s/app-operations/~/edit/drafts/-LrSpdYmWxMcyupIK61l/v/master/pour-approfondir/gestion-du-lancement-de-services-au-demarrage-de-linstance)
{% endhint %}



**Source :** 

{% embed url="https://askubuntu.com/questions/9382/how-can-i-configure-a-service-to-run-at-startup" %}

{% embed url="http://manpages.ubuntu.com/manpages/trusty/fr/man8/update-rc.d.8.html" %}



