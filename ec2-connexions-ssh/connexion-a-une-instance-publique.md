# Connexion à une instance publique

Se connecter à la [console Amazone EC2](https://console.aws.amazon.com/ec2).

Dans l'onglet "Instances", sélectionner l'instance sur laquelle vous voulez vous connecter en ssh.

Dans sa description, copiez le DNS public \(IPv4\) \(ex : ec2-35-180-91-160.eu-west-3.compute.amazonaws.com\) ou l'IP publique IPv4 \(ex : 35.180.91.160\).



### Méthode classique

Sur votre terminal :

```bash
#      /chemin sur l ordi/nom de la clé.pem        utilisateur@IP publique de l'instance
ssh -i /path/my-key-pair.pem ec2-user@ec2-198-51-100-1.compute-1.amazonaws.com
```

{% hint style="info" %}
Cet exemple s'applique pour une AMI Amazon Linux \(utilisateur **ec2-user\).** Pour une AMI Ubuntu, l'utilisateur est **ubuntu**.
{% endhint %}



### Méthode pratique

Il est possible aussi d'enregistrer la clé privée dans son terminal pour ne pas avoir à la retaper à chaque fois qu'on se connecte à une instance. La clé est enregistrée temps que le terminal reste ouvert, auquel cas il faudra la ré-enregistrer. Il est possible d'enregistrer plusieurs clé privées.

```bash
# Ajout des clefs à mon agent SSH
ssh-add -c ~/dossier_ssh/my_private_key.pem	# (ordi linux)
ssh-add -K ~/dossier_ssh/my_private_key.pem	# (ordi mac -> Nous)

# On peut ensuite se connecter à n'importe quelle instance qui utilise my_private_key.pem
# Utilisation de l’option "AgentForwarding" en spécifiant le flag "-A" 
ssh -A ubuntu@62.23.55.220      # Instance 1 Ubuntu
ssh -A ec2-user@125.10.45.159   # Instance 2 Linux Amazon
ssh ...                         # Instance n
```



## Déconnection

```text
exit
```









