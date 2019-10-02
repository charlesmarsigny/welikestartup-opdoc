# Voir l'espace de stockage utilisé sur  tous les volumes \(NVMe dans ce cas\)

`df -hT /dev/nvme?n*` Résultats :

```text
Filesystem     Type      Size  Used Avail Use% Mounted on
udev           devtmpfs  7.6G     0  7.6G   0% /dev
udev           devtmpfs  7.6G     0  7.6G   0% /dev
/dev/nvme1n1p1 ext4      7.7G  2.3G  5.5G  30% /
```

Pour tous les volumes : `df -hT`

=&gt; On est sur la partition **/dev/nvme1n1p1** dont la racine est `/` \(Mounted on\)

_Source :_ [https://docs.aws.amazon.com/fr\_fr/AWSEC2/latest/UserGuide/ebs-describing-volumes.html](https://docs.aws.amazon.com/fr_fr/AWSEC2/latest/UserGuide/ebs-describing-volumes.html) __

