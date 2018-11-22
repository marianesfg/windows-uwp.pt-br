---
author: jwmsft
description: Na marcação XAML, especifica um modo padrão para x:Bind.
title: Atributo xDefaultBindMode
ms.author: jimwalk
ms.date: 02/08/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2696cb46591757421795b15083ea7fdab54943c5
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/22/2018
ms.locfileid: "7578771"
---
# <a name="xdefaultbindmode-attribute"></a>Atributo x:DefaultBindMode

Na marcação XAML, especifica um modo padrão para x:Bind.

**x:DefaultBindMode** está disponível a partir do Windows 10, versão 1607 (Atualização de Aniversário), versão do SDK 14393.

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML

``` syntax
<object x:DefaultBindMode="OneTime \| OneWay \| TwoWay" .../>
```

## <a name="remarks"></a>Comentários

[x:Bind](x-bind-markup-extension.md) possui um modo padrão de **OneTime**. Este foi escolhido por razões de desempenho, já que o uso de **OneWay** faz com que mais código seja gerado para conectar e manipular a detecção de alteração. É possível usar o **x:DefaultBindMode** para alterar o modo padrão para x:Bind de um segmento específico da árvore de marcação. O modo especificado aplica-se a qualquer expressão x:Bind nesse elemento e seus filhos, que não especificam explicitamente um modo como parte da vinculação.

## <a name="related-topics"></a>Tópicos relacionados

* [Extensão de marcação x:Bind](x-bind-markup-extension.md)
