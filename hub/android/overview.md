---
title: Visão geral do desenvolvimento do Android no Windows
description: Um guia para ajudá-lo a começar a desenvolver para Android no Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android no Windows, xamarin. Android, reagir nativo, Cordova, Ionic, PhoneGap, jogo Android do c++, Windows Defender, emulador
ms.date: 04/28/2020
ms.openlocfilehash: d43420f442fd5dfcb2b885fb0369964a113e9bac
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255240"
---
# <a name="overview-of-android-development-on-windows"></a>Visão geral do desenvolvimento do Android no Windows

Há vários caminhos para o desenvolvimento de um aplicativo de dispositivo Android usando o sistema operacional Windows. Esses caminhos se enquadram em três tipos principais: **[desenvolvimento Android nativo](#native-android)**, **[desenvolvimento de plataforma cruzada](#cross-platform)** e **[desenvolvimento de jogos Android](#game-development)**. Esta visão geral ajudará você a decidir qual caminho de desenvolvimento deve ser seguido para desenvolver um aplicativo Android e, em seguida, fornecer [as próximas etapas](#next-steps) para ajudá-lo a começar a usar o Windows para desenvolver com:

- [Android nativo](native-android.md)
- [Xamarin.Android](xamarin-android.md)
- [Xamarin.Forms](xamarin-forms.md)
- [React Native](react-native.md)
- [Cordova, Ionic ou PhoneGap](pwa.md)
- [C/C++ para desenvolvimento de jogos](native-android.md#use-c-or-c-for-android-game-development)

Além disso, este guia fornecerá dicas sobre como usar o Windows para:

- [Testar em um dispositivo Android ou emulador](emulator.md)
- [Atualizar as configurações do Windows Defender para aprimorar o desempenho](defender-settings.md)
- [Desenvolver aplicativos de tela dupla para Android e obter o SDK do dispositivo Surface Duo](https://docs.microsoft.com/dual-screen/android/)

## <a name="native-android"></a>Android nativo

O [desenvolvimento do Android nativo no Windows](./native-android.md) significa que seu aplicativo destina-se somente ao Android (não dispositivos IOS ou Windows). Você pode usar o [Android Studio](https://developer.android.com/studio/install#windows) ou o [Visual Studio](https://visualstudio.microsoft.com/vs/android/) para desenvolver dentro do ecossistema projetado especificamente para o sistema operacional Android. O desempenho será otimizado para dispositivos Android, a aparência da interface do usuário será consistente com outros aplicativos nativos no dispositivo, e quaisquer recursos ou capacidades do dispositivo do usuário serão diretos para acessar e utilizar. Desenvolver seu aplicativo em um formato nativo ajudará a ti a simplesmente "sentir-se certo" porque ele segue todos os padrões de interação e padrões de experiência do usuário estabelecidos especificamente para dispositivos Android.

## <a name="cross-platform"></a>Multiplataforma

As estruturas de plataforma cruzada fornecem uma única base de código que pode (principalmente) ser compartilhada entre dispositivos Android, iOS e Windows. O uso de uma estrutura de plataforma cruzada pode ajudar seu aplicativo a manter a mesma aparência e experiência em plataformas de dispositivos, além de se beneficiar da distribuição automática de atualizações e correções. Em vez de precisar entender uma variedade de linguagens de código específicas do dispositivo, o aplicativo é desenvolvido em uma base de códigos de bases compartilhada, normalmente em uma linguagem.

Embora as estruturas entre plataformas tenham o objetivo de parecer o mais próximo possível dos aplicativos nativos, elas nunca serão tão integradas como um aplicativo desenvolvido nativamente e podem ser prejudicadas com velocidade reduzida e desempenho degradado. Além disso, as ferramentas usadas para criar aplicativos de plataforma cruzada podem não ter todos os recursos oferecidos por cada plataforma de dispositivo diferente, potencialmente exigindo soluções alternativas.

Uma codebase normalmente é composta por **código da interface do**usuário, para criar a interface de usuários, como páginas, controles de botões, rótulos, listas, etc., e **código lógico**, para chamar serviços Web, acessar um banco de dados, invocar recursos de hardware e gerenciar o estado. Em média, 90% disso pode ser reutilizado, embora normalmente haja alguma necessidade de personalizar o código para cada plataforma de dispositivo. Essa generalização depende muito do tipo de aplicativo que você está criando, mas fornece um pouco de contexto que esperamos ajudar com a tomada de decisões.  

## <a name="choosing-a-cross-platform-framework"></a>Escolhendo uma estrutura de plataforma cruzada

[Xamarin nativo (Xamarin. Android)](xamarin-android.md)

- Código da interface do usuário: XML com Designer Android e tema do material
- Código lógico: C# ou F #
- Ainda é possível tocar em alguns elementos nativos do Android, mas é bom para reutilização da base de código para outras plataformas (iOS, Windows).
- Somente o código lógico é compartilhado entre plataformas, não o código da interface do usuário.
- Ótimo para aplicativos mais complexos com uma interface de usuário específica do dispositivo.

[Formulários do xamarin (Xamarin. Forms)](xamarin-forms.md)

- Código da interface do usuário: XAML e .NET (com Visual Studio)
- Código lógico: C #
- Compartilha cerca de 60 a 90% da lógica e do código da interface do usuário em aplicativos de dispositivos Android, iOS e Windows. 
- Usa controles de usuário comuns como botão, rótulo, entrada, ListView, StackLayout, calendário, TabbedPage, etc. Criar um botão e formulários do Xamarin descobrirá como chamar o botão nativo para cada plataforma usando a biblioteca de associação para chamar código Java ou Swift do C#.
- Ótimo para aplicativos simples, como aplicativos internos ou de linha de negócios (LOB), protótipos ou MVPs. Qualquer aplicativo que possa parecer, de certa forma, padrão ou genérico, utilizando uma interface do usuário simples.

[React Native](react-native.md)

- Código da interface do usuário: JavaScript
- Código lógico: JavaScript
- O objetivo de reagir nativo não é escrever o código uma vez e executá-lo em qualquer plataforma, em vez de aprender uma vez (a forma de reagir) e escrever em qualquer lugar.
- A Comunidade adicionou ferramentas como exposição e cria o aplicativo nativo reagir para ajudá-los a criar aplicativos sem usar o Xcode ou o Android Studio.
- Semelhante ao Xamarin (C#), reagir chamadas nativas (JavaScript) chama elementos nativos da interface do usuário (sem a necessidade de escrever Java/Kotlin ou Swift).

[PWAs (Aplicativos Web Progressivos)](pwa.md)

- Código da interface do usuário: HTML, CSS, JavaScript
- Código lógico: JavaScript
- PWAs são aplicativos Web criados com padrões padrão para permitir que eles aproveitem os recursos de aplicativos nativos e da Web. Eles podem ser criados sem uma estrutura, mas algumas estruturas populares a serem consideradas são [Ionic](https://ionicframework.com/docs/intro) e [PhoneGap](https://phonegap.com/about/).
- O PWAs pode ser instalado em um dispositivo (Android, iOS ou Windows) e pode trabalhar offline graças à incorporação de um trabalhador de serviço.
- O PWAs pode ser distribuído e instalado sem uma loja de aplicativos usando apenas uma URL da Web. O Microsoft Store e Google Play Store permitir que PWAs sejam listados, a Apple Store atualmente não, embora eles ainda possam ser instalados em qualquer dispositivo iOS executando o 12,2 ou posterior.
- Para saber mais, Confira esta [introdução ao PWAs](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Introduction) no MDN.

## <a name="game-development"></a>Desenvolvimento de jogos

O desenvolvimento de jogos para Android é, muitas vezes, exclusivo do desenvolvimento de um aplicativo Android padrão, já que os jogos normalmente usam lógica de renderização personalizada, geralmente escritas em OpenGL ou Vulkan. Por esse motivo, e devido às muitas bibliotecas de C disponíveis que dão suporte ao desenvolvimento de jogos, é comum que os desenvolvedores usem o [C/C++ com o Visual Studio](https://docs.microsoft.com/cpp/cross-platform/?view=vs-2019), juntamente com o [NDK (Kit de desenvolvimento nativo)](https://docs.microsoft.com/cpp/cross-platform/create-an-android-native-activity-app?view=vs-2019)do Android, para criar jogos para Android. [Introdução ao C/C++ para desenvolvimento de jogos](native-android.md#use-c-or-c-for-android-game-development).

Outro caminho comum para desenvolver jogos para Android é usar um mecanismo de jogo. Há muitos mecanismos gratuitos e de código-fonte aberto disponíveis, como o [Unity com o Visual Studio](https://docs.microsoft.com/visualstudio/cross-platform/visual-studio-tools-for-unity?view=vs-2019), [mecanismo inreal](https://docs.unrealengine.com/en-US/Platforms/Mobile/Android/GettingStarted/index.html), [monojogo com xamarin](https://docs.microsoft.com/xamarin/graphics-games/monogame/introduction/), [UrhoSharp com xamarin](https://docs.microsoft.com/xamarin/graphics-games/urhosharp/introduction), [SkiaSharp com Xamarin. Forms](https://docs.microsoft.com/xamarin/xamarin-forms/user-interface/graphics/skiasharp/) CocoonJS, app Game Kit, Fusion, Corona SDK, cocos 2D e muito mais.

## <a name="next-steps"></a>Próximas etapas

- [Introdução ao desenvolvimento do Android nativo no Windows](native-android.md)
- [Comece a desenvolver para Android usando o Xamarin. Android](xamarin-android.md)
- [Comece a desenvolver para Android usando o Xamarin. Forms](xamarin-forms.md)
- [Introdução ao desenvolvimento para Android usando reagir nativo](react-native.md)
- [Introdução ao desenvolvimento de um PWA para Android](pwa.md)
- [Desenvolver aplicativos de tela dupla para Android e obter o SDK do dispositivo Surface Duo](https://docs.microsoft.com/dual-screen/android/)
- [Adicionar exclusões do Windows Defender para melhorar o desempenho](defender-settings.md)
- [Habilitar o suporte de virtualização para melhorar o desempenho do emulador](emulator.md#enable-virtualization-support)
