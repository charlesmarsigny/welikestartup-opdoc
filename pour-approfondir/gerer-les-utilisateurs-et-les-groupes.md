---
description: 'Afficher, ajouter, supprimer des utilisateurs et des groupes sur Ubuntu'
---

# Gérer les utilisateurs et les groupes

**L'utilisateur et le groupe par défaut d'Apache et Nginx sont** _**www-data.**_ **Il faut vérifier qu'ils existent sur l'instance \(Ubuntu\) que c'est bien le cas :**

* Afficher la liste des utilisateurs : `cat /etc/passwd | awk -F: '{print $ 1}'`
* Afficher la liste des groupes : `cat /etc/group | awk -F: '{print $ 1}'`

**Autres commandes utiles :**

* Ajouter un utilisateur : `sudo adduser nom_utilisateur`
* Ajouter un groupe : `sudo addgroup nom_groupe`



* Suppression d'un utilisateur : `sudo deluser nom_utilisateur` Le dossier personnel de l'utilisateur est conservé, il faut donc le supprimer manuellement : `sudo rm -R /home/nom_utilisateur`
* Suppression d'un groupe : `sudo delgroup nom_groupe`



* Ajouter un utilisateur à un groupe : `sudo adduser nom_utilisateur nom_groupe`

\*\*\*\*

**Source :**

{% embed url="https://doc.ubuntu-fr.org/tutoriel/gestion\_utilisateurs\_et\_groupes\_en\_ligne\_de\_commande" %}



