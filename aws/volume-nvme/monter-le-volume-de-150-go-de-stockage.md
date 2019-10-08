# Monter le volume de 150 Go de stockage

_Source :_ [https://docs.aws.amazon.com/fr\_fr/AWSEC2/latest/UserGuide/ebs-using-volumes.html](https://docs.aws.amazon.com/fr_fr/AWSEC2/latest/UserGuide/ebs-using-volumes.html) \_\_

On a 2 périphériques de stockage : **nvme0n1** et **nvme1n1**.

* **nvme1n1** est celui qu'on utilise à la racine `/` et à 8 Go. C'est celui qu'on utilise par défaut pour tout.
* **nvme0n1** n'est pas utilisé et c'est lui qui a 150 Go. On veut l'utiliser pour tous les fichiers de notre app. On va lui donner comme répertoire de stockage `/data`.
* Création d'un système de fichiers XFS sur le volume nvme0n1 : `sudo mkfs -t xfs /dev/nvme0n1`
* Création d'un répertoire qu'on choisi de nommer **data** à la racine du serveur : `sudo mkdir /data`
* Monter le volume dans le répertoire précédemment créé : `sudo mount /dev/nvme0n1 /data`
* Vérifier que tout s'est bien passé : `df -hT`

```text
Filesystem     Type      Size  Used Avail Use% Mounted on
udev           devtmpfs  7.6G     0  7.6G   0% /dev
tmpfs          tmpfs     1.6G  744K  1.6G   1% /run
/dev/nvme1n1p1 ext4      7.7G  1.1G  6.7G  14% /
tmpfs          tmpfs     7.7G     0  7.7G   0% /dev/shm
tmpfs          tmpfs     5.0M     0  5.0M   0% /run/lock
tmpfs          tmpfs     7.7G     0  7.7G   0% /sys/fs/cgroup
/dev/loop0     squashfs   89M   89M     0 100% /snap/core/7270
/dev/loop1     squashfs   18M   18M     0 100% /snap/amazon-ssm-agent/1455
tmpfs          tmpfs     1.6G     0  1.6G   0% /run/user/1000
/dev/nvme0n1   xfs       140G  175M  140G   1% /data
```

