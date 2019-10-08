---
description: >-
  Le champ Text crée une entrée de texte simple. Ce champ est utile pour stocker
  une chaîne de caractères.
---

# Text

## Captures d'écran <a id="screenshots"></a>

![Param&#xE8;tres](../../.gitbook/assets/acf-text-field-edit.png)

![Interface](../../.gitbook/assets/acf-text-field-interface.png)

## Settings

| **Name** | **Description** |
| :--- | :--- |
| **Default value** | Set a default value for this field when creating a new post |
| **Placeholder** | Appears within input when no value exists |
| **Prepend** | Adds visual text element before the input |
| **Append** | Adds a visual text element after the input |
| **Character Limit** | Limits the number of characters allowed |

## Template usage <a id="template-usage"></a>

The API will return a string.

```php
<h2><?php the_field('text'); ?></h2>
```

{% hint style="info" %}
[Lien vers la documentation ACF &gt; Field Types &gt; Text](https://www.advancedcustomfields.com/resources/text/)
{% endhint %}

