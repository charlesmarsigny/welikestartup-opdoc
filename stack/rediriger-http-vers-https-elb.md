---
description: 'Quand le client fait une requête en http, le ELB lui renvoie le site en http.'
---

# Rediriger HTTP vers HTTPS \(ELB\)

IL faut modifier les écouteurs de ELB :

* Aller dans la console du **service EC2**, onglet **Equilibreur de charge**
* Sur cette page, aller dans l'onglet **Ecouteurs**.
* Sélectionner l'écouteur **HTTP : 80**
* Cliquez sur **Modifier**
* Protocole : port =&gt; **HTTP : 80**
* Action par défaut =&gt; Supprimer l'action par défaut et choisir **Rediriger vers...** :
* **HTTP 443, Utiliser l'hôte, le chemin, la requête par défaut..., 301 - Déplacé de façon permanente**



**Source :**

{% embed url="https://stackoverflow.com/questions/24603620/redirecting-ec2-elastic-load-balancer-from-http-to-https" %}





