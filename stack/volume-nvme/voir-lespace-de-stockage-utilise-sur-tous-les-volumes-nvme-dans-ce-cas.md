# Trouver le volume utilisé par Ubuntu

`df -hT /dev/nvme?n*` 

{% tabs %}
{% tab title="Résultat" %}
```bash
Filesystem     Type      Size  Used Avail Use% Mounted on
udev           devtmpfs  7.6G     0  7.6G   0% /dev
udev           devtmpfs  7.6G     0  7.6G   0% /dev
/dev/nvme1n1p1 ext4      7.7G  2.3G  5.5G  30% /
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
Nous pouvons en déduire que notre Ubuntu tourne sur la partition **nvme1n1p1** dont la racine est **`/`**
{% endhint %}

Pour voir l'espace de stockage sur tous les volumes, il suffit de taper simplement : `df -hT`



**Source :**

{% embed url="https://docs.aws.amazon.com/fr\_fr/AWSEC2/latest/UserGuide/ebs-describing-volumes.html" %}

\_\_

