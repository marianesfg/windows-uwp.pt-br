---
description: Esse tutorial demonstra como adicionar interfaces de usuário XAML da UWP, criar pacotes MSIX e incorporar outros componentes modernos em seu aplicativo do WPF.
title: 'Tutorial: Modernizar um aplicativo WPF'
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, ilhas xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 21049c995d467209b22fe8ea5c40d303911f2c2c
ms.sourcegitcommit: 0a319e2e69ef88b55d472b009b3061a7b82e3ab1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/21/2020
ms.locfileid: "77521269"
---
# <a name="tutorial-modernize-a-wpf-app"></a>Tutorial: Modernizar um aplicativo WPF 

Há várias maneiras de [modernizar](index.md) aplicativos da área de trabalho existentes integrando os recursos mais recentes do Windows ao código-fonte existente em vez de reescrever os aplicativos do zero. Neste tutorial, exploraremos várias maneiras de modernizar um aplicativo de linha de negócios existente do WPF usando estes recursos:

* .NET Core 3
* Controles XAML da UWP com Ilhas XAML
* Cartões Adaptáveis e notificações do Windows 10
* Implantação de MSIX

Este tutorial requer as seguintes habilidades de desenvolvimento:

* Experiência no desenvolvimento de aplicativos da área de trabalho do Windows com o WPF.
* Conhecimento básico de C# e XAML.
* Conhecimento básico da UWP.

## <a name="overview"></a>Visão geral

Este tutorial fornece o código para um aplicativo de linha de negócios simples do WPF chamado Contoso Expenses. No cenário fictício do tutorial, o Contoso Expenses é um aplicativo interno usado pelos gerentes da Contoso Corporation para manter o controle das despesas enviadas por seus subordinados. Agora, os gerentes estão equipados com dispositivos habilitados para toque e gostariam de usar o aplicativo Contoso Expenses sem mouse e teclado. Infelizmente, a versão atual do aplicativo não é amigável para controle por toque.

A Contoso deseja modernizar o aplicativo com novos recursos do Windows para permitir que os funcionários criem relatórios de despesas com mais eficiência. Muitos dos recursos podem ser facilmente implementados com a criação de um novo aplicativo UWP. No entanto, o aplicativo existente é complexo e é resultado de muitos anos de desenvolvimento por equipes diferentes. Sendo assim, reescrevê-lo do zero com uma nova tecnologia não é uma opção. A equipe está buscando a melhor abordagem para adicionar recursos à base de código existente.

No início do tutorial, o Contoso Expenses tem como alvo o .NET Framework 4.7.2 e usa as seguintes bibliotecas de terceiros:

* MVVM Light, uma implementação básica do padrão MVVM.
* Unity, um contêiner de injeção de dependência.
* LiteDb, uma solução NoSQL inserida para armazenar os dados.
* Bogus, uma ferramenta para gerar dados falsos.

No tutorial, você aprimorará o Contoso Expenses com novos recursos do Windows:

* Migrar um aplicativo do WPF para o .NET Core 3.0. Isso possibilitará cenários novos e importantes no futuro.
* Usar Ilhas XAML para hospedar os controles encapsulados **InkCanvas** e **MapControl** fornecidos pelo Windows Community Toolkit.
* Use Ilhas XAML para hospedar qualquer controle XAML padrão da UWP (nesse caso, um **CalendardView**).
* Integrar Cartões Adaptáveis e notificações do Windows 10 ao aplicativo.
* Empacote o aplicativo com MSIX e configure um pipeline de CI/CD no Azure DevOps para que você possa fornecer automaticamente novas versões do aplicativo para testadores e usuários assim que elas estiverem disponíveis.

## <a name="prerequisites"></a>Pré-requisitos

Para executar este tutorial, o computador de desenvolvimento precisará ter estes pré-requisitos instalados:

* Windows 10, versão 1903 (build 18362) ou uma versão posterior.
* [Visual Studio 2019](https://www.visualstudio.com).
* [SDK do .NET Core 3](https://dotnet.microsoft.com/download/dotnet-core/3.0) (instale a versão mais recente).

Instale as seguintes cargas de trabalho e recursos opcionais com o Visual Studio 2019:

* Desenvolvimento para Desktop com .NET
* Desenvolvimento para a Plataforma Universal do Windows
* SDK do Windows 10 (10.0.18362.0 ou posterior)

## <a name="get-the-contoso-expenses-sample-app"></a>Obter o aplicativo de exemplo Contoso Expenses

Antes de começar o tutorial, baixe o código-fonte do aplicativo Contoso Expenses e verifique se você pode compilar o código no Visual Studio.

1. Baixe o código-fonte do aplicativo na guia **Versões** do [Repositório do workshop AppConsult WinAppsModernization](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop). O link direto é [https://github.com/microsoft/AppConsult-WinAppsModernizationWorkshop/releases](https://github.com/microsoft/AppConsult-WinAppsModernizationWorkshop/releases).
2. Abra o arquivo zip e extraia todo o conteúdo para a raiz de sua unidade **C:\\** . Ele criará uma pasta chamada **C:\WinAppsModernizationWorkshop**.
3. Abra o Visual Studio 2019 e clique duas vezes no arquivo **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses\ContosoExpenses.sln** para abrir a solução.
4. Verifique se você pode compilar, executar e depurar o projeto do WPF Contoso Expenses pressionando o botão **Iniciar** ou CTRL + F5.

## <a name="get-started"></a>Introdução

Quanto tiver o código-fonte do aplicativo de exemplo Contoso Expenses e confirmar que pode compilá-lo no Visual Studio, você estará pronto para iniciar o tutorial:

* [Parte 1: Migrar o aplicativo Contoso Expenses para o .NET Core 3](modernize-wpf-tutorial-1.md)
* [Parte 2: Adicionar um controle InkCanvas da UWP usando ilhas de XAML](modernize-wpf-tutorial-2.md)
* [Parte 3: Adicionar um controle CalendarView da UWP usando ilhas de XAML](modernize-wpf-tutorial-3.md)
* [Parte 4: Adicionar atividades e notificações de usuário do Windows 10](modernize-wpf-tutorial-4.md)
* [Parte 5: Empacotar e implantar com o MSIX](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>Conceitos importantes

As seções a seguir fornecem o contexto de alguns dos principais conceitos discutidos neste tutorial. Se já estiver familiarizado com esses conceitos, ignore esta seção.

### <a name="universal-windows-platform-uwp"></a>UWP (Plataforma Universal do Windows)

No Windows 8, a Microsoft introduziu uma nova estrutura chamada WinRT (Windows Runtime). Diferente do .NET Framework, o WinRT é uma camada nativa de APIs que são expostas diretamente aos aplicativos. O WinRT também introduziu projeções de linguagem, que são camadas adicionadas sobre o runtime para permitir que os desenvolvedores interajam com ele usando linguagens como C# e JavaScript além de C++. As projeções permitem que os desenvolvedores criem aplicativos sobre o WinRT aproveitando os mesmos conhecimentos sobre o C# e o XAML adquiridos na criação de aplicativos com o .NET Framework. 

No Windows 10, a Microsoft introduziu a [UWP (Plataforma Universal do Windows)](/windows/uwp/get-started/universal-application-platform-guide), que se baseia no WinRT. O recurso mais importante da UWP é que ela oferece um conjunto comum de APIs para todas as plataformas de dispositivo: não importa se o aplicativo está em execução em um desktop, um Xbox ou um HoloLens, você pode usar as mesmas APIs.

No futuro, a maioria dos novos recursos do Windows 10 serão expostos por meio de APIs do WinRT, incluindo recursos como Linha do Tempo, Projeto Roma e Windows Hello.

### <a name="msix-packaging"></a>Empacotamento MSIX

[MSIX](/windows/msix/) é o modelo de empacotamento moderno para aplicativos do Windows. O MSIX dá suporte a aplicativos UWP, bem como à criação de aplicativos da área de trabalho usando tecnologias como Win32, WPF, Windows Forms, Java, Electron e muito mais. Quando você empacota um aplicativo da área de trabalho em um pacote MSIX, é possível publicá-lo na Microsoft Store. Seu aplicativo da área de trabalho também obtém o identificador de pacote quando é instalado, o que permite ele use um conjunto mais amplo de APIs do WinRT.

Para saber mais, confira estes artigos:

* [Empacotar aplicativos da área de trabalho](/windows/uwp/porting/desktop-to-uwp-root)
* [Os bastidores de seu aplicativo da área de trabalho empacotado](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>Ilhas XAML

No Windows 10 em diante, versão 1903, você pode hospedar controles UWP em aplicativos da área de trabalho que não sejam UWP usando um recurso chamado *Ilhas XAML*. Esse recurso permite que você aprimore a aparência e a funcionalidade de seus aplicativos da área de trabalho existentes, com os recursos mais recentes de interface do usuário do Windows 10 que só estão disponíveis por meio dos controles UWP. Isso significa que você pode usar recursos UWP, como o Windows Ink e controles que dão suporte ao Sistema Fluent Design em seus aplicativos WPF, Windows Forms e C++ Win32 existentes.

Para obter mais informações, confira [Controles UWP em aplicativos da área de trabalho (Ilhas XAML)](/windows/uwp/xaml-platform/xaml-host-controls). Este tutorial orienta você no processo de usar dois tipos diferentes de controles de Ilha XAML:

* [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) e [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) no Windows Community Toolkit. Esses controles do WPF encapsulam a interface e a funcionalidade dos controles da UWP correspondentes e podem ser usados como qualquer outro controle do WPF no designer do Visual Studio.

* O controle de [Exibição de calendário](/windows/uwp/design/controls-and-patterns/calendar-view) da UWP. Esse é um controle padrão da UWP que você hospedará usando o controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) no Windows Community Toolkit.

### <a name="net-core-3"></a>.NET Core 3

O [.NET Core](https://docs.microsoft.com/dotnet/core/) é uma estrutura de software livre que implementa uma versão multiplataforma, leve e facilmente extensível, do .NET Framework completo. Em comparação com o .NET Framework completo, o runtime do .NET Core é muito mais rápido e muitas das APIs foram otimizadas.

Nas primeiras versões, o foco do .NET Core era o suporte a aplicativos Web ou de back-end. Com o .NET Core, você pode criar facilmente aplicativos Web ou APIs escalonáveis que podem ser hospedados no Windows, no Linux ou em arquiteturas de microsserviço, como contêineres do Docker.

O .NET Core 3 é a versão mais recente do .NET Core. O destaque dessa versão é o suporte para aplicativos da área de trabalho do Windows, incluindo aplicativos Windows Forms e WPF. Você pode executar aplicativos da área de trabalho do Windows novos e existentes no .NET Core 3 e aproveitar todos os benefícios que o .NET Core tem a oferecer. Controles da UWP que são hospedados nas [ilhas de XAML](xaml-islands.md) também podem ser usados em aplicativos do Windows Forms e WPF destinados ao .NET Core 3.

> [!NOTE]
> O WPF e o Windows Forms não estão se tornando multiplataforma e você não pode executar um WPF ou Windows Forms no Linux e no MacOS. Os componentes da interface do usuário do WPF e do Windows Forms ainda têm dependência do sistema de renderização do Windows.

Para obter mais informações, consulte [Novidades do .NET Core 3.0](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0).
