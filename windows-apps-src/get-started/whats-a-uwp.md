---
author: GrantMeStrength
ms.assetid: C9787269-B54F-4FFA-A884-D4A3BF28F80D
title: "O que é um aplicativo da Plataforma Universal do Windows (UWP)?"
description: Saiba mais sobre os diferentes tipos de aplicativos Universais do Windows - aplicativos da Windows Store, aplicativos da Loja do Windows Phone e aplicativos do Windows Runtime.
ms.author: susanw
ms.date: 03/22/2017
ms.topic: article
pms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 2afb5cbc74b381e85fa861562e7de57d877b0c7f
ms.sourcegitcommit: 253ed634522773e15199084a6f74a3a465c2b218
translationtype: HT
---
# <a name="whats-a-universal-windows-platform-uwp-app"></a>O que é um aplicativo da Plataforma Universal do Windows (UWP)?

A Plataforma Universal do Windows (UWP) é a plataforma de aplicativos do Windows 10. Você pode desenvolver aplicativos para UWP com apenas um conjunto de APIs, um pacote do aplicativo e uma loja para alcançar todos os dispositivos Windows 10: computadores, tablets, telefones, Xbox, HoloLens, Surface Hub e muito mais. É fácil dar suporte a vários tamanhos de tela e também a vários modelos de interação, seja com toque, com mouse e teclado, com um controlador de jogo ou com uma caneta. Na essência dos aplicativos UWP está a ideia de que os usuários querem que suas *experiências* sejam móveis em todos os seus dispositivos, e querem usar o dispositivo que for mais conveniente ou produtivo para a tarefa em questão.

A UWP também é flexível: você não precisa usar c# e XAML, se não quiser. Você gosta de desenvolver no Unity ou MonoGame? Prefere JavaScript? Não é um problema, use todos os que desejar. Você tem um aplicativo de área de trabalho do C++ que deseja estender com recursos UWP e vender na loja? Tudo bem. 

O resultado: você pode dedicar seu tempo trabalhando com linguagens de programação, frameworks e APIs, tudo em um único projeto e ter o mesmo código executado na vasta gama hardware Windows existente atualmente. Uma vez escrito o seu aplicativo UWP, você pode publicá-lo na loja para o mundo ver.

![Dispositivos Windows](images/1894834-hig-device-primer-01-500.png)
 
##<a name="so-what-exactly-is-a-uwp-app"></a>Assim, o que *exatamente* é um aplicativo UWP?

O que torna um aplicativo UWP especial? Estas são algumas das características que tornam os aplicativos UWP no Windows 10 diferentes.

-   **Há uma superfície de API comum entre todos os dispositivos.**

    As APIs básicas da Plataforma Universal do Windows (UWP) são iguais para todas as classes de dispositivos Windows. Se o aplicativo usa apenas as APIs básicas, ele será executado em qualquer dispositivo Windows 10, independentemente se você está direcionando um computador desktop, um Xbox ou um fone de ouvido de realidade mista.

-   **Os SDKs de extensão permitem que o aplicativo execute diversas tarefas em tipos de dispositivo específicos.**

    Os SDKs de extensão agregam APIs especializadas para cada classe de dispositivos. Por exemplo, se o aplicativo UWP é direcionado para HoloLens, você pode adicionar recursos de HoloLens além das APIs básicas da UWP.
    Se você estiver direcionado as APIs universais, o pacote do aplicativo pode funcionar em todos os dispositivos que executam o Windows 10. Mas, se quiser que o aplicativo UWP aproveite as APIs específicas do dispositivo caso sejam executadas em uma determinada classe de dispositivo, você pode verificar no tempo de execução se uma API existe antes de chamá-la. 

-   **Os aplicativos são empacotados e distribuídos pela Loja utilizando o formato de empacotamento .AppX.**

    Todos os aplicativos UWP são distribuídos como um pacote AppX. Isso oferece um mecanismo de instalação confiável e garante que seus aplicativos possam ser implantados e atualizados de forma integrada.

-   **Existe uma loja para todos os dispositivos.**

    Depois de se registrar como um desenvolvedor de aplicativos, você poderá enviar seu aplicativo para a loja e disponibilizá-lo em todas os tipos de dispositivos ou apenas os escolhidos. Você envia e gerencia todos os aplicativos para dispositivos Windows em um só lugar.

-   **Os aplicativos oferecem suporte a controles e entrada adaptáveis**

    Os elementos da interface do usuário usam *pixels efetivos* (consulte [Design responsivo 101 para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/Dn958435)), assim responderem com um layout que funciona com base no número de pixels da tela disponível no dispositivo. E eles funcionam bem com vários tipos de entrada, como teclado, mouse, toque, caneta e controladores do Xbox One. Caso você precise personalizar mais sua interface do usuário para um tamanho de tela ou dispositivo específico, novos painéis de layout e ferramentas ajudam a adaptar a interface do usuário aos dispositivos nos quais seu aplicativo pode ser executado.



## <a name="use-a-language-you-already-know"></a>Use uma linguagem que você já conhece


Os aplicativos UWP usam o Windows Runtime, uma API nativa interna do sistema operacional. Essa API é implementado em C++ e tem suporte em C#, Visual Basic, C++ e JavaScript. Algumas opções para criar aplicativos na UWP incluem:
-   Interface do usuário XAML e um back-end c#, VB ou C++
-   Interface do usuário do DirectX e um back-end em C++
-   JavaScript e HTML

O Microsoft Visual Studio 2017 oferece um modelo de aplicativo UWP para cada linguagem que permite criar um único projeto para todos os dispositivos. Quando seu trabalho estiver concluído, você poderá produzir um pacote do aplicativo e enviá-lo à Windows Store diretamente do Visual Studio, para disponibilizar o aplicativo aos clientes em qualquer dispositivo Windows 10.

## <a name="uwp-apps-come-to-life-on-windows"></a>Os aplicativos UWP ganham vida no Windows


No Windows, o seu aplicativo pode fornecer informações importantes em tempo real aos usuários, além de fazê-los voltar para saberem mais. Na economia moderna dos aplicativos, seu aplicativo precisa ser atraente para ser importante na vida de seus usuários. O Windows oferece vários recursos para ajudar a que os usuários retornem ao seu aplicativo:

-   Os blocos dinâmicos e a tela de bloqueio mostram informações contextualmente relevantes e oportunas rapidamente.

-   As notificações por push apresentam alertas em tempo real e de última hora para chamar a atenção do seu usuário quando ele precisar.

-   A Central de Ações é o local onde você pode organizar e exibir notificações e conteúdo que exigem ação dos usuários.

-   Execução em plano de fundo e gatilhos dão vida ao seu aplicativo quando os usuários precisam.

-   O aplicativo pode usar dispositivos de voz e Bluetooth LE para ajudar os usuários a interagir com o mundo ao redor.

-   Suporte para tinta digital avançada e o Controle circular inovador.

-   A Cortana acrescenta personalidade ao software.

-   O XAML fornece as ferramentas para criar interfaces do usuário suaves e animadas.

Finalmente, é possível usar os dados móveis e o Cofre de Credenciais do Windows para proporcionar uma experiência de roaming consistente em todas as telas do Windows onde os usuários executam seu aplicativo. Os dados móveis oferecem uma maneira fácil de armazenar as preferências e configurações de um usuário na nuvem, sem a necessidade de ter de construir a sua própria infraestrutura de sincronização. Além disso, é possível armazenar as credenciais do usuário no Cofre de Credenciais, onde segurança e confiabilidade são as principais prioridades.

##  <a name="monetize-your-app"></a>Monetizar seu aplicativo


No Windows, é possível rentabilizar o seu aplicativo a sua maneira: em vários telefones, tablets, computadores e outros dispositivos. Oferecemos uma série de formas de ganhar dinheiro com seu aplicativo e os serviços que ele proporciona. Você só precisa escolher o que faz sentido para o seu aplicativo:

-   Um download pago é a opção mais simples. Só precisa dizer sue preço.
-   As avaliações permitem que os usuários experimentem o aplicativo antes de comprá-lo, proporcionando uma descoberta e uma conversão mais fáceis do que as opções "freemium" mais tradicionais.
-   Use os preços de venda para aplicativos e complementos.
-   Compras no aplicativo e anúncios também estão disponíveis.

## <a name="lets-get-started"></a>Vamos começar


Para obter uma visão mais detalhada da UWP, leia o [Guia para Aplicativos da Plataforma Universal do Windows](universal-application-platform-guide.md). Em seguida, confira [Prepare-se](get-set-up.md) a fim de baixar as ferramentas necessárias para começar a criar aplicativos e, em seguida, crie [seu primeiro aplicativo](your-first-app.md)!


## <a name="more-advanced-topics"></a>Tópicos mais avançados

* [.NET Native - o que significa para os desenvolvedores da Plataforma Universal do Windows (UWP)](https://blogs.windows.com/buildingapps/2015/08/20/net-native-what-it-means-for-universal-windows-platform-uwp-developers/#TYsD3tJuBJpK3Hc7.97)
* [Aplicativos Universais do Windows em .NET](https://blogs.msdn.microsoft.com/dotnet/2015/07/30/universal-windows-apps-in-net)
* [.NET para aplicativos UWP](https://msdn.microsoft.com/en-us/library/mt185501.aspx)
