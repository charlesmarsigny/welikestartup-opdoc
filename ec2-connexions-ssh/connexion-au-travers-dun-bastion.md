# Connexion au travers d’un bastion



```text
# Ajout des clefs à mon agent SSH
ssh-add -c ~/.ssh/my_private_key	# (linux) OU
ssh-add -K ~/.ssh/my_private_key	# (mac -> Nous)

# Connection SSH au bastion linux
# Utilisation de l’option "AgentForwarding" en spécifiant le flag "-A" 
ssh -A ec2-user@62.23.55.220

# Connection SSH au serveur privé ubuntu
ssh ubuntu@192.168.0.10
```

{% hint style="success" %}
Pas de copie de la clé privé sur le bastion, + de sécurité. Accès donné qu'au personne ayant la clé privée.
{% endhint %}









