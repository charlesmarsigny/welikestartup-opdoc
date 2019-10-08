# Ai-je un NVMe ?

_Source :_  
[_https://docs.aws.amazon.com/fr\_fr/AWSEC2/latest/UserGuide/ssd-instance-store.html_](https://docs.aws.amazon.com/fr_fr/AWSEC2/latest/UserGuide/ssd-instance-store.html)

* Répertorier les périphériques NVMe à l'aide de la commande lspci : `lspci`

```text
00:00.0 Host bridge: Intel Corporation 440FX - 82441FX PMC [Natoma] (rev 02)
00:01.0 ISA bridge: Intel Corporation 82371SB PIIX3 ISA [Natoma/Triton II]
00:01.1 IDE interface: Intel Corporation 82371SB PIIX3 IDE [Natoma/Triton II]
00:01.3 Bridge: Intel Corporation 82371AB/EB/MB PIIX4 ACPI (rev 01)
00:02.0 VGA compatible controller: Cirrus Logic GD 5446
00:03.0 Ethernet controller: Device 1d0f:ec20
00:17.0 Non-Volatile memory controller: Device 1d0f:cd01
00:18.0 Non-Volatile memory controller: Device 1d0f:cd01
00:19.0 Non-Volatile memory controller: Device 1d0f:cd01
00:1a.0 Non-Volatile memory controller: Device 1d0f:cd01
00:1f.0 Unassigned class [ff80]: XenSource, Inc. Xen Platform Device (rev 01)
```

* Si les périphériques NVMe ne sont pas visibles, vérifiez que le module NVMe est chargé : `cat /lib/modules/$(uname -r)/modules.builtin | grep nvme` Réponse :

```text
s/nvme/host/nvme-core.ko
kernel/drivers/nvme/host/nvme.ko
kernel/drivers/nvmem/nvmem_core.ko
```

