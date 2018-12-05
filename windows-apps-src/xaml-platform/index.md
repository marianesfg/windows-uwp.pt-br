---
ms.assetid: b632a6cc-3503-4ab8-bfd1-dde731bd89ab
description: Esta seção inclui tópicos que explicam a estrutura XAML para aplicativos UWP (Plataforma Universal do Windows).
title: Plataforma XAML
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b725a823f31309c2419bcdc5095a78994d1929c0
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8711653"
---
# <a name="xaml-platform"></a>Plataforma XAML


Esta seção inclui tópicos que explicam os conceitos de programação que geralmente são aplicáveis a qualquer aplicativo que você escreve, se você estiver usando c#, Microsoft Visual Basic ou VisualC + + extensões de componente (C++ c++ /CX) como a linguagem de programação e XAML para sua interface do usuário definição. Isso inclui conceitos de programação básicos, tais como o uso de propriedades e eventos, e como eles se aplicam à programação de aplicativos da Plataforma Universal do Windows (UWP). A Plataforma Universal do Windows (UWP) estende os conceitos de propriedades do C#, Visual Basic ou C++/CX e seus valores, adicionando o sistema de propriedade de dependência. Os tópicos desta seção também documentam a linguagem XAML como é usada pela UWP e aborda cenários básicos e tópicos avançados que explicam como usar o XAML para definir a interface do usuário de seu aplicativo UWP.

| Tópico | Descrição |
|-------|-------------|
| [Visão geral do XAML](xaml-overview.md) | Apresentamos a linguagem XAML e os conceitos de XAML para o público de desenvolvedores de aplicativos do Windows Runtime e descrevemos as diferentes maneiras de declarar objetos e definir atributos no XAML durante seu uso para criar um aplicativo do Windows Runtime. |
| [Visão geral das propriedades de dependência](dependency-properties-overview.md) | Este tópico explica o sistema de propriedades de dependência que está disponível quando você escreve um aplicativo do Windows Runtime em C++, C# ou Visual Basic com definições de XAML para a interface do usuário. |
| [Propriedades de dependência personalizadas](custom-dependency-properties.md) | Explica como definir e implementar propriedades de dependência personalizadas para um aplicativo do Windows Runtime em C++, C# ou Visual Basic. |
| [Visão geral das propriedades anexadas](attached-properties-overview.md) | Explica o conceito de uma propriedade anexada no XAML e fornece alguns exemplos. |
| [Propriedades anexadas personalizadas](custom-attached-properties.md) | Explica como implementar uma propriedade anexada XAML como uma propriedade de dependência e como definir a convenção do acessador necessária para que a propriedade anexada possa ser usada no XAML. |
| [Visão geral de eventos e eventos roteados](events-and-routed-events-overview.md) | Descrevemos o conceito de programação de eventos em um aplicativo do Windows Runtime quando você usa as extensões de componente C#, Visual Basic ou C++/CX como a linguagem de programação e o XAML para a definição da interface do usuário. Você pode atribuir manipuladores de eventos como parte das declarações para elementos da interface do usuário em XAML ou pode adicionar manipuladores no código. O Windows Runtime dá suporte a **eventos roteados**: determinados eventos de entrada e eventos de dados podem ser manipulados por outros objetos além do objeto que acionou o evento. Eventos roteados são úteis quando você define modelos de controle ou usa páginas ou contêineres de layout. |
|[Hospedar controles UWP em aplicativos WPF e Windows Forms](xaml-host-controls.md)| Explica como usar controles XAML da UWP para aprimorar a interface do usuário de um formulário do Windows ou aplicativo da área de trabalho do WPF.|
