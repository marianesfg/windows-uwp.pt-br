---
ms.assetid: 83b4be37-6613-4d00-a48a-0451a24a30fb
title: Vinculação de dados
description: A vinculação de dados é uma maneira de a interface do usuário do seu aplicativo exibir dados e, opcionalmente, ficar em sincronia com esses dados.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ccc0f6841334e1eee9dd83d19944d7ce18bbaed4
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63795687"
---
# <a name="data-binding"></a>Vinculação de dados

A vinculação de dados é uma maneira de a interface do usuário do seu aplicativo exibir dados e, opcionalmente, ficar em sincronia com esses dados. A vinculação de dados permite separar a questão dos dados da questão da interface do usuário, e isso resulta em um modelo conceitual mais simples, bem como melhor legibilidade, capacidade de teste e capacidade de manutenção do seu aplicativo. Na marcação, você pode optar por usar a [extensão de marcação {x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) ou a [extensão de marcação {Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782). E você ainda pode usar uma combinação das duas no mesmo aplicativo, inclusive no mesmo elemento de interface do usuário. A {x:Bind} é nova para Windows 10 e tem um desempenho melhor.

| Tópico | Descrição |
|-------|-------------|
| [Visão geral da vinculação de dados](data-binding-quickstart.md) | Este tópico mostra como associar um controle (ou outro elemento da interface do usuário) a um único item ou um controle de itens a uma coleção de itens em um aplicativo da Plataforma Universal do Windows (UWP). Este tópico também mostra como controlar a renderização de itens, implementar uma exibição de detalhes com base em uma seleção e converter dados para exibição. Para obter mais informações detalhadas, consulte [Vinculação de dados em detalhes](data-binding-in-depth.md). | 
| [Vinculação de dados em detalhes](data-binding-in-depth.md) | Este tópico descreve detalhadamente os recursos de vinculação de dados. |
| [Dados de exemplo na superfície de design e para a criação de protótipo](displaying-data-in-the-designer.md) | Para que seus controles sejam populados com dados no designer do Visual Studio (e você possa trabalhar no layout, nos modelos e em outras propriedades visuais do seu aplicativo), há várias maneiras de usar dados de amostra de tempo de design. Os dados de exemplo também podem ser muito úteis e economizar tempo se você estiver criando um aplicativo de esboço (ou protótipo). Você pode usar dados de exemplo em seu esboço ou protótipo em tempo de execução para ilustrar suas ideias sem a necessidade de conexão com dados reais e dinâmicos. |
| [Associar dados hierárquicos e criar um modo de exibição mestre/detalhes](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md) | Você pode criar um modo de exibição mestre/detalhes de vários níveis (também conhecido como lista/detalhes) de dados hierárquicos, associando controles de itens a instâncias [<strong>CollectionViewSource</strong>](https://msdn.microsoft.com/library/windows/apps/BR209833) que são associadas em uma cadeia. |
| [Vinculação de dados e MVVM](data-binding-and-mvvm.md) | Este tópico descreve o padrão de design arquitetônico da interface do usuário MVVM (Model-View-ViewModel). A associação de dados é a essência do MVVM e habilita um acoplamento entre o código da interface do usuário e o código que não é da interface do usuário. |
