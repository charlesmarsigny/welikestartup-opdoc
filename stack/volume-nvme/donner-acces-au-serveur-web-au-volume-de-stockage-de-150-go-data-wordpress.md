---
description: 'Pour le moment, on  a choisi /data/wordpress'
---

# Donner accès à Nginx au bon volume

Il n'y a rien à faire de particulier.‌

Il faut changer juste changer la cible de l'emplacement de l'application dans le fichier de configuration du site web :‌

`sudo nano /etc/nginx/sites-available/default`‌

Par exemple :

```text
root    /data/wordpress
```

​

