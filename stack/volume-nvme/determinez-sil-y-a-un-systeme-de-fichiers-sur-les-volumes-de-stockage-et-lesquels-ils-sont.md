# Système de fichiers du bon volume

Déterminez s'il y a un système de fichiers sur les volumes de stockage et lesquels ils sont :

{% code title="" %}
```bash
sudo file -s /dev/nvme?n*
```
{% endcode %}

{% code title="Résultats" %}
```bash
/dev/nvme0n1:   data # Pas de système de fichiers.
/dev/nvme1n1:   DOS/MBR boot sector # Secteur de démarrage DOS/MBR (peut-être utilisé pour faire des partitions).
/dev/nvme1n1p1: Linux rev 1.0 ext4 filesystem data, UUID=8abb4f2d-c7e0-4daf-80b5-24e1c814cf55, volume name "cloudimg-rootfs" (needs journal recovery) (extents) (64bit) (large files) (huge files) # Doté d'un système de fichiers ext4.
```
{% endcode %}

* `data` : Pas de système de fichiers.
* `DOS/MBR boot sector` : Secteur de démarrage DOS/MBR \(peut-être utilisé pour faire des partitions\).
* `Linux rev 1.0 ext4 filesystem data` : doté d'un système de fichiers ext4.

{% hint style="danger" %}
Il n'y a pas de système de fichiers sur le volume qui nous intéresse. 
{% endhint %}



#### Source :

{% embed url="https://docs.aws.amazon.com/fr\_fr/AWSEC2/latest/UserGuide/recognize-expanded-volume-linux.html" %}



