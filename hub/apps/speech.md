---
title: Fala, voz e conversa no Windows 10
description: Esta página fornece as informações para você começar a desenvolver aplicativos do Windows habilitados para fala.
ms.topic: article
ms.date: 09/12/2019
keywords: Fala no Windows 10, fala, voz, conversa, aplicativos de fala Win32, aplicativos de fala UWP, aplicativos de fala do WPF, aplicativos de fala do WinForms
ms.author: kbridge
author: Karl-Bridge-Microsoft
ms.openlocfilehash: 7ac8d782591ce8f3716e491714c4cbf241e80b6c
ms.sourcegitcommit: 8a88a05ad89aa180d41a93152632413694f14ef8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726546"
---
# <a name="speech-voice-and-conversation-in-windows-10"></a>Fala, voz e conversa no Windows 10

![Imagem do Hero de fala](images/hero-speech-composite-small.png)

A fala pode ser uma maneira eficaz, natural e agradável para as pessoas interagirem com seus aplicativos do Windows, complementando ou até mesmo substituindo experiências de interação tradicionais com base no mouse, teclado, toque, controlador ou gestos.

Recursos baseados em fala, como reconhecimento de fala, ditado, síntese de fala (também conhecido como conversão de texto em fala ou TTS) e assistentes de voz de conversação (como Cortana ou Alexa), podem fornecer experiências de usuário acessíveis e inclusivas que permitem que as pessoas usem seus aplicativos quando outros dispositivos de entrada podem não ser suficientes.

Esta página fornece informações sobre como as várias estruturas de desenvolvimento do Windows fornecem reconhecimento de fala, síntese de fala e suporte a conversas para desenvolvedores que criam aplicativos para Windows.

## <a name="platform-specific-documentation"></a>Documentação específica à plataforma

:::row:::
   :::column:::
      ![UWP (Plataforma Universal do Windows)](images/platform-uwp.png)

      **UWP (Plataforma Universal do Windows)**

      Crie aplicativos habilitados para fala na plataforma moderna para aplicativos e jogos do Windows 10, em qualquer dispositivo Windows (incluindo PCs, telefones, Xbox One, HoloLens e outros) e publique-os no Microsoft Store.

      [Interações de controle por voz](https://docs.microsoft.com/windows/uwp/design/input/speech-interactions)

      [Reconhecimento de fala](https://docs.microsoft.com/windows/uwp/design/input/speech-recognition)

      [Ditado contínuo](https://docs.microsoft.com/windows/uwp/design/input/enable-continuous-dictation)

      [Síntese de fala](https://docs.microsoft.com/uwp/api/windows.media.speechsynthesis)

      [Agentes de conversação](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent)

      [Comandos de voz da Cortana](https://docs.microsoft.com/cortana/voice-commands/vcd)
   :::column-end:::
   :::column:::
      ![Aplicativos da plataforma Win32](images/platform-win32.png)

      **Plataforma Win32**

      Desenvolva aplicativos habilitados para fala para Windows desktop e Windows Server usando as ferramentas, as informações e os mecanismos de exemplo e os aplicativos fornecidos aqui.

      [Microsoft Speech Platform – SDK (Software Development Kit) (versão 11)](https://www.microsoft.com/download/details.aspx?id=27226)
      
      [SDK do Microsoft Speech, versão 5.1](https://www.microsoft.com/download/details.aspx?id=10121)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      ![.NET](images/platform-dotnet.png)

      **.NET Framework**

      Desenvolva aplicativos e ferramentas acessíveis na plataforma estabelecida para aplicativos do Windows gerenciados com um modelo XAML de interface do usuário e o .NET Framework.

      [Guia de programação do System.Speech para .NET Framework](https://docs.microsoft.com/previous-versions/office/developer/speech-technologies/hh361625(v=office.14))
   :::column-end:::
   :::column:::
      ![Serviços de fala do Azure](images/platform-azure-speech.png)

      **Serviços de fala do Azure**

      Projete, crie e teste sites acessíveis com os serviços de fala do Azure.

      [Fala para texto](https://azure.microsoft.com/services/cognitive-services/speech-to-text/)

      [Conversão de texto em fala](https://azure.microsoft.com/services/cognitive-services/text-to-speech/)
      
      [Tradução de fala](https://azure.microsoft.com/services/cognitive-services/speech-translation/)

      [Assistentes virtuais da primeira voz](https://docs.microsoft.com/azure/cognitive-services/speech-service/voice-first-virtual-assistants)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      **Recursos herdados**

      Versões herdadas, preteridas e/ou sem suporte da tecnologia Microsoft Speech.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Agente da Microsoft](https://docs.microsoft.com/windows/win32/lwef/microsoft-agent)

      [Kit de desenvolvimento de software de fala da Microsoft (SASDK) versão 1,0](https://www.microsoft.com/download/details.aspx?id=2200)
   :::column-end:::
   :::column:::
      [Microsoft Speech API (SAPI) 5,3](https://docs.microsoft.com/previous-versions/windows/desktop/ms723627(v=vs.85))

      [Microsoft Speech API (SAPI) 5,4](https://docs.microsoft.com/previous-versions/windows/desktop/ee125663(v=vs.85))

      [O controle de reconhecimento de Fala do Bing](https://docs.microsoft.com/previous-versions/bing/speech/dn434583(v%3dmsdn.10))
   :::column-end:::
:::row-end:::

## <a name="samples"></a>Amostras

Baixe e execute exemplos completos do Windows que demonstram vários recursos e funcionalidades de acessibilidade.

:::row:::
   :::column:::
      [Navegador de exemplo de código](https://docs.microsoft.com/samples/browse/?term=speech)

      O novo navegador de exemplos (Substitui a Galeria de códigos do MSDN).
   :::column-end:::
   :::column:::
      [Exemplos clássicos do Windows no GitHub](https://github.com/microsoft/Windows-classic-samples/search?q=speech&unscoped_q=speech)

      Esses exemplos demonstram a funcionalidade e o modelo de programação para Windows e Windows Server. 
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Exemplos da UWP (Plataforma Universal do Windows) no GitHub](https://github.com/microsoft/Windows-universal-samples/search?q=speech&unscoped_q=speech)

      Esses exemplos demonstram os padrões de uso da API para a UWP (Plataforma Universal do Windows) no SDK (kit de desenvolvimento de software) do Windows para Windows 10.
   :::column-end:::
   :::column:::
      [XAML Controls Gallery](https://github.com/microsoft/Xaml-Controls-Gallery)

      Este aplicativo demonstra os inúmeros controles XAML compatíveis com o Sistema de Design Fluente.
   :::column-end:::
:::row-end:::

## <a name="videos"></a>Vídeos

Vários vídeos que abordam como criar aplicativos do Windows que incorpom interações de fala.

:::row:::
   :::column:::
      **Cortana e a plataforma de fala em detalhes**
   :::column-end:::
   :::column:::
      **Extensibilidade da Cortana em aplicativos universais do Windows**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2015/3-716/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2015/2-691/player]
   :::column-end:::
:::row-end:::

## <a name="other-resources"></a>Outros recursos

:::row:::
   :::column:::
      **Blogs e notícias**

      O mais recente do mundo do Microsoft Speech.
   :::column-end:::
   :::column:::
      **Comunidade e suporte**

      Onde os desenvolvedores e os usuários do Windows se encontram e aprendem juntos.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Nas notícias](https://news.microsoft.com/?s=speech)

      [Blogs de fala](https://blogs.windows.com/windowsdeveloper/?s=speech)
   :::column-end:::
   :::column:::
      [Comunidade do Windows-fala](https://community.windows.com/search?q=speech)

      [Fórum do desenvolvedor de fala do Windows](https://social.msdn.microsoft.com/Forums/home?filter=alltypes&sort=firstpostdesc&searchTerm=speech)
   :::column-end:::
:::row-end:::
