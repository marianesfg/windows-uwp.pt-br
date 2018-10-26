---
author: stevewhims
ms.assetid: 08C8F359-E8B6-4A45-8F4B-8A1962F0CE38
description: O Microsoft Visual Studio está para o Windows assim como o Xcode está para o iOS e o Mac OS. Neste guia passo a passo, ajudaremos você a se familiarizar com o Visual Studio.
title: Criando um projeto no Visual Studio
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3b10d615146c8989231c4fe36ad9588716c59c34
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5547565"
---
# <a name="getting-started-creating-a-project"></a>Ponto de Partida: criando um projeto

## <a name="creating-a-project"></a>Criando um projeto

O Microsoft Visual Studio está para o Windows assim como o Xcode está para o iOS e o Mac OS. Neste guia passo a passo, ajudaremos você a se familiarizar com o Visual Studio. Ele mostra as noções básicas absolutas que você precisa saber antes de começar. Sempre que for criar um aplicativo, você seguirá etapas semelhantes a estas.

O vídeo a seguir compara o Xcode e o Visual Studio.

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Comparing-Xcode-to-Visual-Studio/player]

Você também achará [Compilando aplicativos para postagem de blog do Windows](https://blogs.windows.com/buildingapps/2016/01/27/visual-studio-walkthrough-for-ios-developers/) muito útil.

Criar um aplicativo para Windows 10 (mais formalmente chamado de um aplicativo da plataforma Universal do Windows (UWP)) é bastante parecido com criar um aplicativo iOS usando Storyboards. O aplicativo do Windows 10 geralmente é construído ao longo de várias páginas, cada página que contém uma parte diferente da interface do usuário, como um site da web. Cada página geralmente tem dois arquivos de origem associados: um para armazenar a interface do usuário em formato [XAML (visão geral)](https://msdn.microsoft.com/library/windows/apps/mt185595) e um que contém o código-fonte, frequentemente em C#. Conforme seu usuário interage com seu aplicativo, ele navega entre essas páginas. Neste passo a passo, você criará um aplicativo com duas páginas.

**Observação**um recurso importante dos aplicativos do Windows 10 é o fato de que o mesmo código-fonte e o mesmo conjunto de API está disponível para você independentemente da plataforma. Como você sabe, ao gravar um aplicativo iOS universal para iPhone e iPad, você pode determinar em tempo de execução em qual plataforma seu aplicativo está sendo executado e tomar a ação apropriada. De maneira semelhante, aplicativos do Windows 10 podem dizer, em tempo de execução, o dispositivo estão sendo executados. Com um aplicativo UWP, não é preciso usar \#ifdef's em seu código fonte para criar compilações para telefone versus para desktop. Convenientemente, aplicativos do Windows 10 também usam de modo inteligente os controles de interface do usuário dependendo do dispositivo: por exemplo, seu aplicativo pode fazer referência a um controle do seletor de data, e o controle automaticamente parecerá e funcionará de maneira diferente dependendo se ele tem em execução em um desktop ou na tela de um telefone. No entanto, seu código-fonte permanece o mesmo.

Vamos ver como podemos criar um aplicativo do Windows 10. Comece executando o Visual Studio. Ao ser executado pela primeira vez, o Visual Studio pedirá que você obtenha uma licença de desenvolvedor. Uma licença de desenvolvedor permite instalar e testar os aplicativos UWP em seu computador local antes de enviá-los à Microsoft Store. Para obter uma licença, siga as instruções na tela para entrar em uma conta da Microsoft. Se não tiver uma, clique no link **Inscrever-se** na caixa de diálogo **Licença de Desenvolvedor** e siga as instruções na tela.

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

## <a name="next-step"></a>Próxima etapa

[Introdução: escolhendo uma linguagem de programação](getting-started-choosing-a-programming-language.md)
