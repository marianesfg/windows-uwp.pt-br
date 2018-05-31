---
author: jwmsft
description: Na marcação XAML, especifica um modo padrão para x:Bind.
title: Atributo xDefaultBindMode
ms.author: jimwalk
ms.date: 02/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4eae77a51f925eaff27a7919ecfa60552d0fbae5
ms.sourcegitcommit: b8c77ac8e40a27cf762328d730c121c28de5fbc4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2018
ms.locfileid: "1672973"
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
