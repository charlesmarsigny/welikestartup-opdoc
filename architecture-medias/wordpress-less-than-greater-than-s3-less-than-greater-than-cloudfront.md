---
description: Plugin Offload Media Lite
---

# Wordpress &lt;-&gt; S3 &lt;-&gt; CloudFront

Paramètrer Offload Media Lite

## Obtenir les 2 clés d'accès pour utiliser Offload :

On les obtient en créant un IAM user sur AWS. AWS nous donne l'access key id et la secret access key.

Suivre le quick start :

{% embed url="https://deliciousbrains.com/wp-offload-media/doc/amazon-s3-quick-start-guide/" %}



## Modifications en php

Dans le fichier wp-config.php, coller le code suivant avec les bonnes clés :

```text
// ACCES API Offload Media Lite
define( 'AS3CF_SETTINGS', serialize( array(
    'provider' => 'aws',
    'access-key-id' => ' AKIA5IFIMWUFTFX2WUPQ',
    'secret-access-key' => ' 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',
) ) );
```

## Paramètrage dans le backoffice

Dans les paramètre du plugin dans le backoffice :

* Choisir Amazon S3
* Choisir "Définir la clé d'API dans wp-config.php"
* Path : **wp-content/uploads/**
* Dans **Custom Domain** mettre celui de sa distribution CloudFront \(ex : dv77mj7ug7qr0.cloudfront.net\)
* Options sur ON :
  * Copy files to bucket
  * Path
  * Year/month
  * Object versioning
  * Rewrite Media URLs
  * Custom Domain
  * Peut-être d'autres comme Remove Files from server





