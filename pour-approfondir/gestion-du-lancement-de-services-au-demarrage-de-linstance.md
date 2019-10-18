---
description: Utilisation de update-rc.d
---

# Gestion du lancement de services au démarrage de l'instance

update-rc.d met à jour automatiquement les liens vers les scripts d'initialisation.

```text
update-rc.d [-n] nom disable|enable [S|2|3|4|5]

# Exemple :
# Ne lance plus le service cron au démarrage de l'instance
update-rc.d cron disable
# Lance le service cron au démarrage de l'instance
update-rc.d cron enable # ce qu'on a par défaut sur les instances EC2
```



**Source :**

{% embed url="http://manpages.ubuntu.com/manpages/trusty/fr/man8/update-rc.d.8.html" %}



