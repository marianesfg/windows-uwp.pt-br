---
title: Acessibilidade no Windows 10
description: Esta página fornece as informações necessárias para que você comece a desenvolver aplicativos Windows acessíveis.
ms.topic: article
ms.date: 09/12/2019
keywords: Acessibilidade no Windows 10, acessibilidade, criação de aplicativos Win32 acessíveis, criação de aplicativos UWP acessíveis, criação de aplicativos WPF acessíveis, criação de aplicativos WinForms acessíveis
ms.author: kbridge
author: Karl-Bridge-Microsoft
ms.openlocfilehash: 76bbd3f0e04bbb2f729ad0950bae190b2fffb6ac
ms.sourcegitcommit: 6e7665b457ec4585db19b70acfa2554791ad6e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/14/2019
ms.locfileid: "70987192"
---
# <a name="accessibility-in-windows-10"></a>Acessibilidade no Windows 10

![hero-accessibility-bar-smaller.png](images/hero-accessibility-bar-smaller.png)

## <a name="build-accessibility-into-your-applications-to-empower-people-of-all-abilities"></a>Incluir recursos de acessibilidade em seus aplicativos para que todas as pessoas possam usá-los

Produtos e serviços – incluindo mídia eletrônica – são acessíveis quando são projetados para fornecer experiências completas e bem-sucedidas para o máximo de pessoas possível.

Crie aplicativos do Windows acessíveis e inclusivos, com funcionalidade e usabilidade aprimoradas, para pessoas com deficiências (temporárias e permanentes), preferências pessoais, estilos de trabalho específicos ou restrições circunstanciais (como espaços de trabalho compartilhados, condução, culinária, brilho intenso e assim por diante). Algumas soluções comuns incluem o fornecimento de informações em formatos alternativos (como legendas em um vídeo) ou a habilitação do uso de tecnologias adaptativas (como leitores de tela).

**Todos devem ter acesso às mesmas salas de um edifício, quer precisem usar as escadas ou o elevador.**

Esta página fornece informações sobre como as diversas estruturas de desenvolvimento do Windows fornecem suporte de acessibilidade para desenvolvedores que criam aplicativos para Windows, desenvolvedores de tecnologias adaptativas que criam ferramentas como leitores de tela e amplificadores e engenheiros de teste de software que criam scripts automatizados para testar aplicativos.

## <a name="platform-specific-documentation"></a>Documentação específica à plataforma

:::row:::
   :::column:::
      ![UWP (Plataforma Universal do Windows)](images/platform-uwp.png)

      **UWP (Plataforma Universal do Windows)**

      Desenvolva aplicativos e ferramentas acessíveis na plataforma moderna para aplicativos e jogos do Windows 10 em qualquer dispositivo Windows (incluindo PCs, telefones, Xbox One, HoloLens e muito mais) e publique-os na Microsoft Store.

      [Projetando software inclusivo](https://docs.microsoft.com/windows/uwp/accessibility/designing-inclusive-software)

      [Desenvolvendo aplicativos inclusivos do Windows](https://docs.microsoft.com/windows/uwp/accessibility/developing-inclusive-windows-apps)

      [Testes de acessibilidade](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-testing)

      [Acessibilidade na Loja](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-in-the-store)
   :::column-end:::
   :::column:::
      ![Aplicativos da plataforma Win32](images/platform-win32.png)

      **Plataforma Win32**

      Desenvolva aplicativos e ferramentas acessíveis na plataforma original para aplicativos do Windows escritos em C++/C.

      [Quais as novidades em termos de acessibilidade e automação do Windows](https://docs.microsoft.com/windows/desktop/accessibility-whatsnew)

      [Desenvolvimento de aplicativos acessíveis para Windows](https://docs.microsoft.com/windows/desktop/accessibility-appdev)

      [Desenvolvimento de estruturas da interface do usuário acessíveis para Windows](https://docs.microsoft.com/windows/desktop/accessibility-uiframeworkdev)

      [Desenvolvimento de tecnologias adaptativas para Windows](https://docs.microsoft.com/windows/desktop/accessibility-atdev)

      [Teste de acessibilidade](https://docs.microsoft.com/windows/desktop/accessibility-testwithuia)

      [Tecnologia de acessibilidade e automação herdada – MSAA para automação da interface do usuário](https://docs.microsoft.com/windows/desktop/accessibility-legacy)

      [Recursos de acessibilidade do Windows](https://docs.microsoft.com/windows/desktop/winauto/about-windows-accessibility-features)

      [Diretrizes para projetar aplicativos acessíveis](https://docs.microsoft.com/windows/desktop/uxguide/inter-accessibility)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      ![Plataforma do WPF](images/platform-wpf2-small.png)

      **WPF (Windows Presentation Foundation)**

      Desenvolva aplicativos e ferramentas acessíveis na plataforma estabelecida para aplicativos do Windows gerenciados com um modelo XAML de interface do usuário e o .NET Framework.

      [Melhores práticas de acessibilidade](https://docs.microsoft.com/dotnet/framework/ui-automation/accessibility-best-practices)

      [Conceitos básicos de automação da interface do usuário](https://docs.microsoft.com/dotnet/framework/ui-automation/index)

      [Provedores de automação da interface do usuário para código gerenciado](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-providers-for-managed-code)

      [Clientes de automação da interface do usuário para código gerenciado](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-clients-for-managed-code)

      [Padrões de controle da automação da interface do usuário](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-control-patterns)

      [Padrões de texto da	 automação da interface do usuário](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-text-pattern)

      [Tipos de controle da automação da interface do usuário](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-control-types)

      [Especificação da automação da interface do usuário e promessa da comunidade](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-specification-and-community-promise)
   :::column-end:::
   :::column:::
      ![Aplicativos da plataforma Windows Forms](images/platform-winforms.png)

      **WinForms (Windows Forms)**

      Desenvolva aplicativos e ferramentas acessíveis para aplicativos do Windows gerenciados com um modelo XAML de interface do usuário e o .NET Framework.

      [Acessibilidade do Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)

      [Criação de um aplicativo do Windows acessível](https://docs.microsoft.com/dotnet/framework/winforms/advanced/walkthrough-creating-an-accessible-windows-based-application)

      [Propriedades de controles do Windows Forms que são compatíveis com diretrizes de acessibilidade](https://docs.microsoft.com/dotnet/framework/winforms/advanced/properties-on-windows-forms-controls-that-support-accessibility-guidelines)

      [Fornecendo informações de acessibilidade para os controles de um Windows Form](https://docs.microsoft.com/dotnet/framework/winforms/controls/providing-accessibility-information-for-controls-on-a-windows-form)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      **Acessibilidade na Web**

      Projete, crie e teste sites acessíveis no Microsoft Edge.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Introdução à acessibilidade na Web](https://docs.microsoft.com/microsoft-edge/accessibility)

      [Projetando sites acessíveis](https://docs.microsoft.com/microsoft-edge/accessibility/design)
   :::column-end:::
   :::column:::
      [Criando sites acessíveis ](https://docs.microsoft.com/microsoft-edge/accessibility/build)

      [Testando sites acessíveis](https://docs.microsoft.com/microsoft-edge/accessibility/test)
   :::column-end:::
:::row-end:::

## <a name="samples"></a>Exemplos

Baixe e execute exemplos completos do Windows que demonstram vários recursos e funcionalidades de acessibilidade.

:::row:::
   :::column:::
      [Navegador de exemplo de código](https://docs.microsoft.com/en-us/samples/browse/)

      O novo navegador de exemplos que substitui a Galeria de Códigos da MSDN.
   :::column-end:::
   :::column:::
      [Galeria de Códigos da MSDN (desativada)](https://code.msdn.microsoft.com/site/search?query=accessibility&f%5B0%5D.Value=accessibility&f%5B0%5D.Type=SearchText&ac=2)

      Baixe exemplos para Windows, Windows Phone, Microsoft Azure, Office, SharePoint, Silverlight e outros produtos.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Exemplos clássicos do Windows no GitHub](https://github.com/microsoft/Windows-classic-samples/search?q=accessibility&unscoped_q=accessibility)

      Esses exemplos demonstram a funcionalidade e o modelo de programação para Windows e Windows Server. 
   :::column-end:::
   :::column:::
      [Exemplos da UWP (Plataforma Universal do Windows) no GitHub](https://github.com/microsoft/Windows-universal-samples/search?q=accessibility&unscoped_q=accessibility)

      Esses exemplos demonstram os padrões de uso da API para a UWP (Plataforma Universal do Windows) no SDK (kit de desenvolvimento de software) do Windows para Windows 10.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [Galeria de controles XAML](https://github.com/microsoft/Xaml-Controls-Gallery)

      Este aplicativo demonstra os inúmeros controles XAML compatíveis com o Sistema de Design Fluente.
   :::column-end:::
:::row-end:::

## <a name="videos"></a>Vídeos

Vários vídeos que abrangem como criar aplicativos do Windows acessíveis para atender a demandas gerais de acessibilidade e como a Microsoft os aborda.

:::row:::
   :::column:::
      **Como começar a usar a acessibilidade em aplicativos do Windows**
   :::column-end:::
   :::column:::
      **One Dev Minute: desenvolvimento de aplicativos para proporcionar acessibilidade**
   :::column-end:::
   :::column:::
      **Introdução à deficiências e acessibilidade**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2017/P4072/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Developing-Apps-for-Accessibility/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/Kl4CT4DaypM]
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      **Do raqueamento ao produto – controle com os olhos para Windows 10**
   :::column-end:::
   :::column:::
      **Acessibilidade no Windows 10**
   :::column-end:::
   :::column:::
      **Introdução à criação de aplicativos UWP acessíveis**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/AShNPfmAkvY]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2016/P541/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2016/P497/player]
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      **Criação de recursos de acessibilidade do Windows**
   :::column-end:::
   :::column:::
      **Os recursos de acessibilidade do Windows 10 capacitam a todos**
   :::column-end:::
   :::column:::
      **Facilitando a visualização dos ponteiros do mouse**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/Y_NJbE7wxlk]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/BseTf-4q9GA]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/4UzaF7_T3bw]
   :::column-end:::
:::row-end:::

## <a name="other-resources"></a>Outros recursos

:::row:::
   :::column span="3":::
      **Blogs e notícias**

      O que há de mais recente no mundo da acessibilidade da Microsoft.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Nas notícias](https://news.microsoft.com/presskits/accessibility/)
   :::column-end:::
   :::column:::
      [Blogs de acessibilidade](https://blogs.microsoft.com/accessibility/)
   :::column-end:::
   :::column:::
      [Blogs de automação da interface do usuário do Windows](https://blogs.msdn.microsoft.com/winuiautomation/)
   :::column-end:::
:::row-end:::

:::row:::
   :::column span="3":::
      **Comunidade e suporte**

      Um local em que os desenvolvedores e usuários do Windows se encontram e aprendem juntos.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Comunidade do Windows – Acessibilidade](https://community.windows.com/search?q=accessibility)
   :::column-end:::
   :::column:::
      [Fórum de desenvolvimento de acessibilidade e automação do Windows](https://social.msdn.microsoft.com/Forums/windows/home?forum=windowsaccessibilityandautomation)
   :::column-end:::
   :::column:::
      [Answer Desk para pessoas com deficiência](https://www.microsoft.com/Accessibility/disability-answer-desk)
   :::column-end:::
:::row-end:::
