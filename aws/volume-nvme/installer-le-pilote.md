# Installer le pilote

_Source :_ [https://docs.aws.amazon.com/fr\_fr/AWSEC2/latest/UserGuide/nvme-ebs-volumes.html](https://docs.aws.amazon.com/fr_fr/AWSEC2/latest/UserGuide/nvme-ebs-volumes.html) __

* Mettez à jour le cache de votre package :

  `sudo apt-get update -y`

* Mettez à niveau le package linux-aws pour recevoir la version la plus récente :

  `sudo apt-get upgrade -y linux-aws`

* Redémarrez votre instance pour charger la dernière version du noyau :

  `sudo reboot`

  Voir ses volume de stockage :

  `lsblk`

```text
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
loop0         7:0    0    89M  1 loop /snap/core/7713
loop1         7:1    0    18M  1 loop /snap/amazon-ssm-agent/1335
loop2         7:2    0    18M  1 loop /snap/amazon-ssm-agent/1455
loop3         7:3    0  88.7M  1 loop /snap/core/7396
nvme0n1     259:0    0 139.7G  0 disk
nvme1n1     259:1    0     8G  0 disk
└─nvme1n1p1 259:2    0     8G  0 part /
```

* Installation de l'interface de commandes `nvme-cli` \(pour pouvoir utiliser les commandes `nvme` : `sudo apt-get update` `sudo apt-get install nvme-cli`
* ID de volume et nom de périphérique : `sudo nvme id-ctrl -v /dev/nvme1n1`

```text
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

_Source :_ [https://docs.aws.amazon.com/fr\_fr/AWSEC2/latest/UserGuide/ebs-using-volumes.html](https://docs.aws.amazon.com/fr_fr/AWSEC2/latest/UserGuide/ebs-using-volumes.html) __

\_\_

