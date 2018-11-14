---
author: jwmsft
description: Fornece um identificador exclusivo para elementos de marcação. Para o XAML da UWP (Plataforma Universal do Windows), este identificador exclusivo é usado por processos e ferramentas de localização XAML, como o uso de recursos de um arquivo de recursos .resw.
title: Diretiva xUid
ms.assetid: 9FD6B62E-D345-44C6-B739-17ED1A187D69
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4bec4bd5d35fc2bb3013b37c1386520a769ddeb6
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/08/2018
ms.locfileid: "6141788"
---
# <a name="xuid-directive"></a>Diretiva x:Uid


Fornece um identificador exclusivo para elementos de marcação. Para o XAML da UWP (Plataforma Universal do Windows), este identificador exclusivo é usado por processos e ferramentas de localização XAML, como o uso de recursos de um arquivo de recursos .resw.

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML

``` syntax
<object x:Uid="stringID".../>
```

## <a name="xaml-values"></a>Valores XAML

| Termo | Descrição |
|------|-------------|
| stringID | Uma cadeia de caracteres que identifica um elemento XAML em um aplicativo e torna-se parte do caminho do recurso em um arquivo de recurso. Veja os comentários.| 

## <a name="remarks"></a>Comentários

Use **x:Uid** para identificar um elemento de objeto em seu XAML. Normalmente, esse elemento de objeto é uma instância de uma classe de controle ou outro elemento exibido em uma interface do usuário. A relação entre a cadeia de caracteres que você usa em **X:UID** e as cadeias de caracteres que você usa em um arquivo de recursos é que as cadeias de caracteres do arquivo de recurso são o **X:UID** seguido por um ponto (.) e, em seguida, pelo nome de uma propriedade específica do elemento que está sendo localizada. Considere este exemplo:

``` syntax
<Button x:Uid="GoButton" Content="Go"/>
```

Para especificar o conteúdo para substituir o texto de exibição **Go**, você deve especificar um novo recurso proveniente de um arquivo de recursos. O arquivo de recursos deve conter uma entrada para o recurso nomeado "GoButton.Content". [**Content**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content) neste caso é uma propriedade específica herdada da classe [**Button**](/uwp/api/windows.ui.xaml.controls.button). Você também pode fornecer valores localizados para outras propriedades desse botão, por exemplo, um valor baseado em recursos para "GoButton.FlowDirection". Para obter mais informações sobre como usar **x:Uid** e os arquivos de recursos juntos, consulte [Localizar cadeias de caracteres na interface do usuário e no manifesto do pacote do aplicativo](../app-resources/localize-strings-ui-manifest.md).

A validade de quais cadeias de caracteres podem ser usadas para um valor **x:Uid** é controlada de maneira prática, por meio da qual as cadeias de caracteres são legais enquanto identificadores em um arquivo de recursos e um caminho de recurso.

**x:Uid** é separado de **x:Name** em função do cenário de localização de XAML declarado, de tal forma que os identificadores usados para localização não têm dependências nas implicações do modelo de programação de **x:Name**. Além disso, **x:Name** é controlado pelo conceito de namescope XAML, enquanto a exclusividade para **x:Uid** é controlada pelo sistema PRI (índice de recursos de pacote). Para saber mais, veja [Sistema de Gerenciamento de Recursos](../app-resources/resource-management-system.md).

A linguagem XAML da UWP tem regras um pouco diferentes de exclusividade **x:Uid** em relação às tecnologias anteriores que usam XAML. Para a linguagem XAML da UWP, a existência do mesmo valor de ID **x:Uid** como diretiva em vários elementos XAML é legal. Entretanto, cada elemento deve compartilhar a mesma lógica de resolução ao resolver os recursos em um arquivo de recursos. Além disso, todos os arquivos XAML em um projeto compartilham um único escopo de recurso para fins de resolução de **x:Uid**. Não há um conceito de alinhamento de escopos de **x:Uid** em arquivos XAML individuais.

Em alguns casos, você usará um caminho de recurso, em vez de funcionalidade interna do sistema PRI (índice de recursos de pacote). Qualquer cadeia de caracteres usada como um valor **x:Uid** define um caminho de recurso que começa com ms-resource:///Resources/ e inclui a cadeia de caracteres **x:Uid**. O caminho é completado pelos nomes das propriedades que você especifica em um arquivo de recursos ou de outro modo está direcionando.

Não coloque **x:Uid** em elementos de propriedade, o que não é permitido no XAML do Windows Runtime.

