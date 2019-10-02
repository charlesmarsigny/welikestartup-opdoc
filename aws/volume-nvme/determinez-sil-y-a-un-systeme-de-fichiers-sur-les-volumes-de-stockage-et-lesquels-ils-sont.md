# Déterminez s'il y a un système de fichiers sur les volumes de stockage et lesquels ils sont

`sudo file -s /dev/nvme?n*`

```text
/dev/nvme0n1:   data
/dev/nvme1n1:   DOS/MBR boot sector
/dev/nvme1n1p1: Linux rev 1.0 ext4 filesystem data, UUID=8abb4f2d-c7e0-4daf-80b5-24e1c814cf55, volume name "cloudimg-rootfs" (needs journal recovery) (extents) (64bit) (large files) (huge files)
```

* `data` : Pas de système de fichiers.
* `DOS/MBR boot sector` : Secteur de démarrage DOS/MBR \(peut-être utilisé pour faire des partitions\).
* `Linux rev 1.0 ext4 filesystem data` : doté d'un système de fichiers ext4.

_Source :_ [https://docs.aws.amazon.com/fr\_fr/AWSEC2/latest/UserGuide/recognize-expanded-volume-linux.html](https://docs.aws.amazon.com/fr_fr/AWSEC2/latest/UserGuide/recognize-expanded-volume-linux.html) __

