---
author: mijacobs
Description: "Veja os elementos e os atributos que você usa para criar blocos adaptáveis."
title: "Esquema e modelos de blocos adaptáveis"
ms.assetid: 858FB05E-87A2-49CF-BE48-570980AD36C8
label: Adaptive tile schema and templates
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 08bdb46dba6fc93ada20b3fc585d3e24e29023a0

---
# Modelos de blocos adaptáveis: esquema e orientação

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Veja os elementos e os atributos que você usa para criar blocos adaptáveis. Para obter instruções e exemplos, consulte [Criar blocos adaptáveis](tiles-and-notifications-create-adaptive-tiles.md).

## elemento de bloco


``` xml
<tile>
  
  <!-- Child elements -->
  visual
  
</tile>
```

## elemento visual


``` xml
<visual
  version? = integer
  lang? = string
  baseUri? = anyURI
  branding? = "none" | "logo" | "name" | "nameAndLogo"
  addImageQuery? = boolean
  contentId? = string
  displayName? = string >
    
  <!-- Child elements -->
  binding+

</visual>
```

## elemento de associação


``` xml
<binding
  template = tileTemplateNameV3
  fallback? = tileTemplateNameV1
  lang? = string
  baseUri? = anyURI
  branding? = "none" | "logo" | "name" | "nameAndLogo"
  addImageQuery? = boolean
  contentId? = string
  displayName? = string
  hint-textStacking? = "top" | "center" | "bottom"
  hint-overlay? = [0-100] >

  <!-- Child elements -->
  ( image
  | text
  | group
  )*

</binding>
```

## elemento de imagem


``` xml
<image
  src = string
  placement? = "inline" | "background" | "peek"
  alt? = string
  addImageQuery? = boolean
  hint-crop? = "none" | "circle"
  hint-removeMargin? = boolean
  hint-align? = "stretch" | "left" | "center" | "right" />
```

## elemento de texto


``` xml
<text
  lang? = string
  hint-style? = textStyle
  hint-wrap? = boolean
  hint-maxLines? = integer
  hint-minLines? = integer
  hint-align? = "left" | "center" | "right" >

  <!-- text goes here -->

</text>
```

textStyle values: caption captionSubtle body bodySubtle base baseSubtle subtitle subtitleSubtle title titleSubtle titleNumeral subheader subheaderSubtle subheaderNumeral header headerSubtle headerNumber

## elemento de grupo


``` xml
<group>

  <!-- Child elements -->
  subgroup+

</group>
```

## elemento de subgrupo


``` xml
<subgroup
  hint-weight? = [0-100]
  hint-textStacking? = "top" | "center" | "bottom" >

  <!-- Child elements -->
  ( text
  | image
  )*

</subgroup>
```

## Tópicos relacionados


* [Criar blocos adaptáveis](tiles-and-notifications-create-adaptive-tiles.md)
 

 







<!--HONumber=Aug16_HO3-->


