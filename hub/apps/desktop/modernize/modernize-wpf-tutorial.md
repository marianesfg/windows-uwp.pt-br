---
description: Este tutorial demonstra como adicionar interfaces de usuário do UWP XAML, criar pacotes do MSIX e incorporar outros componentes modernos ao seu aplicativo do WPF.
title: 'Tutorial: modernizar um aplicativo do WPF'
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, Ilhas XAML
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: de84cbb2e1927d9426eefaaf7b0d70d604427da1
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683809"
---
# <a name="tutorial-modernize-a-wpf-app"></a>Tutorial: modernizar um aplicativo do WPF 

Há várias maneiras de [modernizar](index.md) os aplicativos de área de trabalho existentes integrando os recursos mais recentes do Windows ao código-fonte existente em vez de reescrever os aplicativos do zero. Neste tutorial, exploraremos várias maneiras de modernizar um aplicativo de linha de negócios existente do WPF usando estes recursos:

* .NET Core 3
* Controles XAML do UWP com ilhas XAML
* Cartões adaptáveis e notificações do Windows 10
* Implantação de MSIX

Este tutorial requer as seguintes habilidades de desenvolvimento:

* Experiência no desenvolvimento de aplicativos da área de trabalho do Windows com o WPF.
* Conhecimento básico de C# e XAML.
* Conhecimento básico do UWP.

## <a name="overview"></a>Visão geral

Este tutorial fornece o código para um aplicativo de linha de negócios simples do WPF chamado despesas da contoso. No cenário fictício do tutorial, as despesas da Contoso são um aplicativo interno usado pelos gerentes da Contoso Corporation para manter o controle das despesas enviadas por seus relatórios. Agora, os gerentes estão equipados com dispositivos habilitados para toque e eles gostariam de usar o aplicativo de despesas da Contoso sem mouse ou teclado. Infelizmente, a versão atual do aplicativo não é amigável para toque.

A contoso deseja modernizar esse aplicativo com novos recursos do Windows para permitir que os funcionários criem relatórios de despesas com mais eficiência. Muitos dos recursos podem ser facilmente implementados com a criação de um novo aplicativo UWP. No entanto, o aplicativo existente é complexo e é o resultado de muitos anos de desenvolvimento por equipes diferentes. Como tal, reescrevê-lo do zero com uma nova tecnologia não é uma opção. A equipe está procurando a melhor abordagem para adicionar novos recursos à base de código existente.

No início do tutorial, as despesas da Contoso visam o .NET Framework 4.7.2 e usam as seguintes bibliotecas de terceiros:

* MVVM Light, uma implementação básica para o padrão MVVM.
* Unity, um contêiner de injeção de dependência.
* LiteDb, uma solução NoSQL incorporada para armazenar os dados.
* Falso, uma ferramenta para gerar dados falsos.

No tutorial, você aprimorará as despesas da contoso com novos recursos do Windows:

* Migre um aplicativo existente do WPF para o .NET Core 3,0. Isso abrirá cenários novos e importantes no futuro.
* Use as ilhas XAML para hospedar os controles de **InkCanvas** e **MapControl** encapsulados fornecidos pelo kit de ferramentas da Comunidade do Windows.
* Use as ilhas XAML para hospedar qualquer controle padrão UWP XAML (nesse caso, um **CalendardView**).
* Integre os cartões adaptáveis e as notificações do Windows 10 ao aplicativo.
* Empacote o aplicativo com MSIX e configure um pipeline de CI/CD no Azure DevOps para que você possa fornecer automaticamente novas versões do aplicativo para testadores e usuários assim que ele estiver disponível.

## <a name="prerequisites"></a>Pré-requisitos

Para executar este tutorial, o computador de desenvolvimento deve ter estes pré-requisitos instalados:

* Windows 10, versão 1903 (Build 18362) ou uma versão posterior.
* [Visual Studio 2019](https://www.visualstudio.com).
* [SDK do .NET Core 3](https://dotnet.microsoft.com/download/dotnet-core/3.0) (instale a versão mais recente).

Certifique-se de instalar as seguintes cargas de trabalho e recursos opcionais com o Visual Studio 2019:

* Desenvolvimento de área de trabalho do .NET
* Desenvolvimento para a Plataforma Universal do Windows
* SDK do Windows 10 (10.0.18362.0 ou posterior)

## <a name="get-the-contoso-expenses-sample-app"></a>Obter o aplicativo de exemplo de despesas da contoso

Antes de começar o tutorial, baixe o código-fonte do aplicativo de despesas da Contoso e verifique se você pode criar o código no Visual Studio.

1. Baixe o código-fonte do aplicativo na guia **versões** do [repositório do AppConsult WinAppsModernization Workshop](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop). O link direto é [https://aka.ms/wamwc](https://aka.ms/wamwc).
2. Abra o arquivo zip e extraia todo o conteúdo para a raiz da unidade **C:\\** . Ele criará uma pasta chamada **C:\WinAppsModernizationWorkshop**.
3. Abra o Visual Studio 2019 e clique duas vezes no arquivo **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses\ContosoExpenses.sln** para abrir a solução.
4. Verifique se você pode criar, executar e depurar o projeto do WPF de despesas da Contoso pressionando o botão **Iniciar** ou CTRL + F5.

## <a name="get-started"></a>Introdução

Depois de ter o código-fonte do aplicativo de exemplo de despesas da Contoso e você pode confirmar que pode criá-lo no Visual Studio, você está pronto para iniciar o tutorial:

* [Parte 1: migrar o aplicativo de despesas da Contoso para o .NET Core 3](modernize-wpf-tutorial-1.md)
* [Parte 2: adicionar um controle InkCanvas do UWP usando ilhas XAML](modernize-wpf-tutorial-2.md)
* [Parte 3: adicionar um controle CalendarView UWP usando ilhas XAML](modernize-wpf-tutorial-3.md)
* [Parte 4: Adicionar notificações e atividades de usuário do Windows 10](modernize-wpf-tutorial-4.md)
* [Parte 5: empacotar e implantar com MSIX](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>Conceitos importantes

As seções a seguir fornecem um plano de fundo para alguns dos principais conceitos discutidos neste tutorial. Se você já estiver familiarizado com esses conceitos, poderá ignorar esta seção.

### <a name="universal-windows-platform-uwp"></a>UWP (Plataforma Universal do Windows)

No Windows 8, a Microsoft introduziu uma nova estrutura chamada WinRT (Windows Runtime). Ao contrário da .NET Framework, o WinRT é uma camada nativa de APIs que são expostas diretamente aos aplicativos. O WinRT também introduziu projeções de linguagem, que são camadas adicionadas sobre o tempo de execução para permitir que os desenvolvedores interajam C# com ela usando linguagens C++como e JavaScript além do. As projeções permitem que os desenvolvedores criem aplicativos sobre o WinRT que aproveitam o mesmo C# e o conhecimento XAML adquiridos na criação de aplicativos com o .NET Framework. 

No Windows 10, a Microsoft introduziu o [plataforma universal do Windows (UWP)](/windows/uwp/get-started/universal-application-platform-guide), que se baseia no WinRT. O recurso mais importante do UWP é que ele oferece um conjunto comum de APIs em todas as plataformas de dispositivo: não importa se o aplicativo está em execução em um desktop, em um Xbox ou em um HoloLens, você pode usar as mesmas APIs.

No futuro, a maioria dos novos recursos do Windows 10 são expostos por meio de APIs do WinRT, incluindo recursos como linha do tempo, projeto Roma e Windows Hello.

### <a name="msix-packaging"></a>Empacotamento de MSIX

[MSIX](/windows/msix/) (anteriormente conhecido como AppX) é o modelo de empacotamento moderno para aplicativos do Windows. O MSIX dá suporte a aplicativos UWP, bem como a criação de aplicativos de desktop usando tecnologias como Win32, WPF, Windows Forms, Java, aplicativos de área de trabalho e muito mais. Ao empacotar um aplicativo de área de trabalho em um pacote MSIX, você pode publicar seu aplicativo no Microsoft Store. Seu aplicativo de desktop também obtém a identidade do pacote quando ele é instalado, o que permite que seu aplicativo de Desktop use um conjunto mais amplo de APIs do WinRT.

Para saber mais, confira estes tópicos:

* [Aplicativos de desktop de pacote](/windows/uwp/porting/desktop-to-uwp-root)
* [Nos bastidores do aplicativo de área de trabalho empacotado](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>Ilhas XAML

A partir do Windows 10, versão 1903, você pode hospedar controles UWP em aplicativos de área de trabalho não UWP usando um recurso chamado *ilhas XAML*. Esse recurso permite aprimorar a aparência e a funcionalidade de seus aplicativos de área de trabalho existentes com os recursos mais recentes da interface do usuário do Windows 10 que estão disponíveis apenas por meio de controles UWP. Isso significa que você pode usar recursos de UWP, como o Windows Ink e controles que dão suporte ao sistema de design fluente em seus aplicativos WPF C++ , Windows Forms e Win32 existentes.

Para obter mais informações, consulte [controles UWP em aplicativos de área de trabalho (Ilhas XAML)](/windows/uwp/xaml-platform/xaml-host-controls). Este tutorial orienta você pelo processo de uso de dois tipos diferentes de controles de ilha XAML:

* [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) e [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) no kit de ferramentas da Comunidade do Windows. Esses controles WPF encapsulam a interface e a funcionalidade dos controles UWP correspondentes e podem ser usados como qualquer outro controle WPF no designer do Visual Studio.

* O controle de [exibição de calendário](/windows/uwp/design/controls-and-patterns/calendar-view) UWP. Esse é um controle UWP padrão que você hospedará usando o controle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) no kit de ferramentas da Comunidade do Windows.

### <a name="net-core-3"></a>.NET Core 3

O [.NET Core](https://docs.microsoft.com/dotnet/core/) é uma estrutura de código-fonte aberto que implementa uma versão de plataforma cruzada, leve e facilmente extensível do .NET Framework completo. Em comparação com o .NET Framework completo, o tempo de inicialização do .NET Core é muito mais rápido e muitas das APIs foram otimizadas.

Por meio de suas primeiras versões, o foco do .NET Core era o suporte a aplicativos Web ou de back-end. Com o .NET Core, você pode criar facilmente aplicativos Web escalonáveis ou APIs que podem ser hospedados no Windows, no Linux ou em arquiteturas de micro serviço, como contêineres do Docker.

O .NET Core 3 é a versão mais recente do .NET Core. O destaque dessa versão é o suporte para aplicativos da área de trabalho do Windows, incluindo aplicativos Windows Forms e WPF. Você pode executar aplicativos da área de trabalho do Windows novos e existentes no .NET Core 3 e aproveitar todos os benefícios que o .NET Core tem a oferecer. Controles da UWP que são hospedados nas [ilhas de XAML](xaml-islands.md) também podem ser usados em aplicativos do Windows Forms e WPF destinados ao .NET Core 3.

> [!NOTE]
> O WPF e o Windows Forms não estão se tornando entre plataformas, e você não pode executar um WPF ou Windows Forms no Linux e no MacOS. Os componentes da interface do usuário do WPF e Windows Forms ainda têm uma dependência no sistema de renderização do Windows.

Para obter mais informações, consulte [Novidades do .NET Core 3.0](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0).