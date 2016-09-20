---
author: mcleblanc
ms.assetid: 08C8F359-E8B6-4A45-8F4B-8A1962F0CE38
description: "O Microsoft Visual Studio está para o Windows assim como o Xcode está para o iOS e o Mac OS. Neste guia passo a passo, ajudaremos você a se familiarizar com o Visual Studio."
title: Criando um projeto no Visual Studio
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 23d9ed066e2909a15b3106fd19bf6ce5ab09e7a9

---

# Introdução: criando um projeto

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

## Criando um projeto

O Microsoft Visual Studio está para o Windows assim como o Xcode está para o iOS e o Mac OS. Neste guia passo a passo, ajudaremos você a se familiarizar com o Visual Studio. Ele mostra as noções básicas absolutas que você precisa saber antes de começar. Sempre que for criar um aplicativo, você seguirá etapas semelhantes a estas.

O vídeo a seguir compara o Xcode e o Visual Studio.

<iframe src="https://hubs-video.ssl.catalog.video.msn.com/embed/5b7bd91f-6a2f-40b6-9b19-eb2994931d0a/IA?csid=ux-en-us&MsnPlayerLeadsWith=html&PlaybackMode=Inline&MsnPlayerDisplayShareBar=false&MsnPlayerDisplayInfoButton=false&iframe=true&QualityOverride=HD" width="720" height="405" allowFullScreen="true" frameBorder="0" scrolling="no">One dev minute - Comparando o Xcode com o Visual Studio</iframe>

Você também achará [Compilando aplicativos para postagem de blog do Windows](https://blogs.windows.com/buildingapps/2016/01/27/visual-studio-walkthrough-for-ios-developers/) muito útil.

Criar um aplicativo para Windows 10 (mais formalmente chamado de aplicativo da Plataforma Universal do Windows (UWP)) é bastante parecido com criar um aplicativo iOS usando Storyboards. O aplicativo Windows 10 costuma ser construído ao longo de diversas páginas, cada uma contendo uma parte diferente da interface do usuário, como um site da web. Cada página geralmente tem dois arquivos de origem associados: um para armazenar a interface do usuário em formato [XAML (visão geral)](https://msdn.microsoft.com/library/windows/apps/mt185595) e um que contém o código-fonte, frequentemente em C#. Conforme seu usuário interage com seu aplicativo, ele navega entre essas páginas. Neste passo a passo, você criará um aplicativo com duas páginas.


            **Observação**  Um recurso importante dos aplicativos Windows 10 é o fato de que o mesmo código fonte e o mesmo conjunto de API está disponível para você independentemente da plataforma. Como você sabe, ao gravar um aplicativo iOS universal para iPhone e iPad, você pode determinar em tempo de execução em qual plataforma seu aplicativo está sendo executado e tomar a ação apropriada. De modo parecido, os aplicativos Windows 10 podem dizer, em tempo de execução, em qual dispositivo estão sendo executados. Com um aplicativo UWP, não é preciso usar \#ifdef's em seu código fonte para criar compilações para telefone versus para desktop. Convenientemente, os aplicativos Windows 10 também usam de modo inteligente os controles de interface do usuário dependendo do dispositivo; por exemplo, seu aplicativo pode fazer referência a um controle seletor de data e o controle automaticamente parecerá e funcionará de maneira diferente, de acordo com a execução em um desktop ou na tela de um telefone. No entanto, seu código-fonte permanece o mesmo.

Vamos ver como podemos criar um aplicativo Windows 10. Comece executando o Visual Studio. Ao ser executado pela primeira vez, o Visual Studio pedirá que você obtenha uma licença de desenvolvedor. Uma licença de desenvolvedor permite instalar e testar os aplicativos da Windows Store em seu computador local antes de enviá-los à Windows Store. Para obter uma licença, siga as instruções na tela para entrar em uma conta da Microsoft. Se não tiver uma, clique no link **Inscrever-se** na caixa de diálogo **Licença de Desenvolvedor** e siga as instruções na tela.

Para fazer uma comparação, quando você inicia o Xcode, a primeira coisa que vê é a tela **Bem-vindo ao Xcode**, similar à imagem a seguir.

![tela de boas-vindas do xcode](images/ios-to-uwp/ios-to-uwp-xcode-welcome.png)

O Visual Studio é muito similar. Você verá a **Página inicial**, como mostrado na imagem a seguir

![tela inicial do visual studio](images/ios-to-uwp/ios-to-uwp-vs-welcome.png)

Para criar um novo aplicativo, comece fazendo um projeto de acordo com o seguinte:

-   Na área **Iniciar**, toque em **Novo Projeto**.
-   Toque no menu **Arquivo** e toque em **Novo Projeto**.

Em comparação, quando você cria um novo projeto no Xcode, você vê uma lista de modelos de projeto como os mostrados na imagem a seguir.

![caixa de diálogo de novo projeto do xcode](images/ios-to-uwp/ios-to-uwp-xcode-choose-template.png)

No Visual Studio, também há vários modelos de projeto para escolher, como mostrado na imagem a seguir.

![caixa de diálogo novo projeto do visual studio](images/ios-to-uwp/ios-to-uwp-vs-choose-template.png) Neste guia passo a passo, toque em **Visual C#** e, em seguida, toque em **Windows**, **Universal do Windows** e **Aplicativo em Branco (Universal do Windows)**. Na caixa **Nome**, digite "MyApp" e toque em **OK**. O Visual Studio cria e exibe seu primeiro projeto. Agora, você pode começar a criar seu aplicativo e adicionar código a ele.

## Próxima etapa

[Introdução: escolhendo uma linguagem de programação](getting-started-choosing-a-programming-language.md)



<!--HONumber=Jun16_HO4-->


