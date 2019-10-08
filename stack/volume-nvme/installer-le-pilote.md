# Installer le pilote et trouver le bon volume

Mettez à jour le cache de votre package :

```bash
sudo apt-get update -y
```

* Mettez à niveau le package linux-aws pour recevoir la version la plus récente :

```bash
sudo apt-get upgrade -y linux-aws
```

* Redémarrez votre instance pour charger la dernière version du noyau :

```bash
sudo reboot
```

Voir ses volume de stockage :

```bash
lsblk
```

```bash
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
loop0         7:0    0    89M  1 loop /snap/core/7713
loop1         7:1    0    18M  1 loop /snap/amazon-ssm-agent/1335
loop2         7:2    0    18M  1 loop /snap/amazon-ssm-agent/1455
loop3         7:3    0  88.7M  1 loop /snap/core/7396
nvme0n1     259:0    0 139.7G  0 disk
nvme1n1     259:1    0     8G  0 disk
└─nvme1n1p1 259:2    0     8G  0 part /
```

{% hint style="success" %}
Le volume qui nous intéresse est le **nvme0n1** qui a 140 Go.
{% endhint %}

* Installation de l'interface de commandes `nvme-cli` \(pour pouvoir utiliser les commandes `nvme` : 

```bash
sudo apt-get update
sudo apt-get install nvme-cli
```

* ID de volume et nom de périphérique : 

```bash
sudo nvme id-ctrl -v /dev/nvme1n1
```

```bash
NVME Identify Controller:
vid     : 0x1d0f
ssvid   : 0x1d0f
sn      : vol0c0f4d389002671da
mn      : Amazon Elastic Block Store
...
...
0000: 73 64 61 31 20 20 20 20 20 20 20 20 20 20 20 20 "sda1............"
...
```

\*\*\*\*

**Source :**

{% embed url="https://docs.aws.amazon.com/fr\_fr/AWSEC2/latest/UserGuide/nvme-ebs-volumes.html" %}

{% embed url="https://docs.aws.amazon.com/fr\_fr/AWSEC2/latest/UserGuide/ebs-using-volumes.html" %}





