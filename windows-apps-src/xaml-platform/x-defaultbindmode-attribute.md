---
author: tbd
description: "Na marcação XAML, especifica um modo padrão para x:Bind"
title: "Extensão de marcação xDefaultBindMode"
ms.assetid: 
ms.author: 
ms.date: 
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 0fa037b4c59566cb1b9bacd4d2e36520a86c508d
ms.sourcegitcommit: ba0d20f6fad75ce98c25ceead78aab6661250571
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2017
---
# <a name="xdefaultbindmode-markup-extension"></a>Extensão de marcação {x:DefaultBindMode}

\[ Atualizado para aplicativos UWP no Windows 10. Para artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Na marcação XAML, especifica um modo padrão para x:Bind.

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML

``` syntax
<object x:DefaultBindMode="OneTime \| OneWay \| TwoWay" .../>
```

## <a name="remarks"></a>Comentários

x:Bind apresenta um modo padrão de OneTime e foi escolhido por razões de desempenho, já que o uso de OneTime fará com que mais código seja gerado para conectar e manipular a detecção de alteração. **x:DefaultBindMode** pode ser usado para alterar o modo padrão para x:Bind de um segmento específico da árvore de marcação. O modo selecionado aplicará qualquer expressão x:Bind nesse elemento e seus filhos, que não especificam explicitamente um modo como parte da vinculação.

## <a name="related-topics"></a>Tópicos relacionados

* [**x:Bind**](https://docs.microsoft.com/en-us/windows/uwp/xaml-platform/x-bind-markup-extension)
