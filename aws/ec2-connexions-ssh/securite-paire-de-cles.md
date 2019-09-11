# Sécurité : Paire de clés

Afin de s'authentifier, il est nécessaire de créer une paire de clés composé d'une clé publique et d'une clé privé.

### Création de la paire de clés

La paire de clé peut être créée sur la [console Amazone EC2](https://console.aws.amazon.com/ec2). 

* cliquez sur l'onglet "**Paires de clés**" de la console EC2, 
* ensuite, cliquez sur "**Créer une paire de clés**", 
* puis entrez un **nom de clé**,
* enfin **téléchargez la clé** privé que le navigateur web propose de télécharger \(fichier .pem\).

### Permissions d'accès à la paire de clés

Une fois téléchargé, les permissions d'accès au fichier "**nom-paire-cles.pem**" doivent être changées dans votre terminal : `chmod 400 nom-paire-cles.pem` 





### Documentations AWS

{% embed url="https://docs.aws.amazon.com/fr\_fr/AWSEC2/latest/UserGuide/ec2-key-pairs.html" %}



