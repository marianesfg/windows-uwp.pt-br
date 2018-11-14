---
author: stevewhims
description: Quais são as opções ao desenvolver aplicativos de plataforma cruzada?
title: Selecionando uma abordagem para o desenvolvimento de aplicativos iOS e UWP
ms.assetid: 5CDAB313-07B7-4A32-A49B-026361DCC853
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c62624e83d1e8334cce711869088817e2f9dc5e0
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6652945"
---
# <a name="selecting-an-approach-to-ios-and-uwp-app-development"></a>Selecionando uma abordagem para o desenvolvimento de aplicativos iOS e UWP


Quais são as opções ao desenvolver aplicativos de plataforma cruzada?

## <a name="whats-the-best-way-to-support-both-ios-and-windows"></a>Qual é a melhor maneira de dar suporte a iOS e Windows?

O Windows e o iOS podem parecer criaturas muito diferentes, mas um número cada vez maior de ferramentas e técnicas pode ajudá-lo muito se você precisar escrever aplicativos que dão suporte a ambas as plataformas (e ao Android também). A melhor solução depende do tipo de aplicativo que você está escrevendo, e se você estiver começando a partir do zero ou fazendo a portabilidade de um projeto existente.

## <a name="writing-a-new-app"></a>Criando um novo aplicativo

Com um slate limpo, você tem muitas opções à sua disposição, incluindo:

-   [Xamarin](http://go.microsoft.com/fwlink/p/?LinkID=320484)

    Com Xamarin, é possível escrever o aplicativo em C#, executá-lo no Windows e criar aplicativos do iOS nativo também. O suporte para Xamarin é integrado ao Visual Studio; basta selecionar o tipo de projeto correto.

-   [Apache Cordova](http://go.microsoft.com/fwlink/p/?LinkID=400439)

    Se você estiver mais familiarizado com Javascript e HTML, o Apache Cordova (também conhecido como PhoneGap) ajudará a criar aplicativos de plataforma cruzada para iOS, Windows e Android. Esse tipo de projeto também é integrado ao Visual Studio.

-   Mecanismos de jogo

    Com ferramentas como [Unity3D](http://go.microsoft.com/fwlink/p/?LinkID=320479) e [Unreal Engine](http://go.microsoft.com/fwlink/p/?LinkID=394062) à disposição, é possível escrever jogos de qualidade AAA para o Windows e em muitas outras plataformas, inclusive iOS. O Unity dá suporte a scripts em C#; o Unreal usa C++.

-   [MonoGame](http://go.microsoft.com/fwlink/p/?LinkID=320483)

    O sucessor espiritual do XNA. Agora é uma estrutura de plataforma cruzada de código-fonte aberto, o que significa que é possível escrever aplicativos em C# para muitas plataformas com suporte para mecanismos físicos e elementos gráficos 2D e 3D.

## <a name="adapting-an-existing-app"></a>Adaptando um aplicativo existente

Com um aplicativo existente do iOS, suas opções são um pouco mais limitadas. No entanto, nem tudo certamente é perdido.

-   [Ponte do Windows para iOS](https://go.microsoft.com/fwlink/p/?LinkId=619014)

    Também conhecida como Project Islandwood, essa é uma ferramenta ainda em desenvolvimento que pode importar projetos Xcode diretamente para o Visual Studio. O código Objective-C pode ser criado e depurado no Visual Studio. Se o seu projeto faz uso de bibliotecas como Cocos para elementos gráficos, você pode considerar essa uma maneira útil de portar seu aplicativo rapidamente.

-   Reutilizar seu código C++.

    Se a sua lógica de negócios principal está escrita em C++, em vez de Objective-C ou Swift, você geralmente pode usar esse código com apenas algumas pequenas alterações em seu projeto. Em seguida, você pode usar XAML para definir sua interface do usuário, como com outros aplicativos Windows, e chamar no código C++ quando necessário.

-   [Use o ANGLE para executar o OpenGL ES no Windows](http://go.microsoft.com/fwlink/p/?linkid=618387)

    Uma etapa intermediária para portabilidade de seu projeto do OpenGL ES 2.0 é usar o ANGLE. O ANGLE permite executar conteúdo do OpenGL ES no Windows, o que converte chamadas de API do OpenGL ES em chamadas de API do DirectX 11.

## <a name="other-cross-platform-authoring-tools"></a>Outras ferramentas de criação de plataforma cruzada

-   [GameSalad](http://go.microsoft.com/fwlink/p/?LinkID=320480)

    Um ambiente de criação de jogos.

-   [Construct 2]( http://go.microsoft.com/fwlink/p/?LinkID=320481)

    Um ambiente de criação de jogos.

-   [Titanium Studio](http://go.microsoft.com/fwlink/p/?LinkID=320482)

    Um ambiente de criação para várias plataformas.

-   [Cocos2D-x](http://go.microsoft.com/fwlink/p/?LinkID=320485)

    Uma biblioteca de códigos para várias plataformas para manipulação de sprites e modelagem física.

-   [Impact.js](http://go.microsoft.com/fwlink/p/?LinkID=320486)

    Uma biblioteca de jogos em HTML.

-   [Marmalade](http://go.microsoft.com/fwlink/p/?LinkID=320487)

    Um SDK para várias plataformas.

-   [OpenFL](http://go.microsoft.com/fwlink/p/?LinkID=320488)

    Uma ferramenta de desenvolvimento para várias plataformas.

-   [GameMaker](http://go.microsoft.com/fwlink/p/?LinkID=320490)

    Um ambiente de criação especificamente para jogos.

-   [PlayCanvas](http://go.microsoft.com/fwlink/p/?LinkID=394061)

    Uma ferramenta para desenvolvimento de jogos baseados em HTML.

