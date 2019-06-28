---
description: Este tutorial demonstra como adicionar interfaces do usuário XAML UWP, criar pacotes MSIX e incorporar outros componentes modernos em seu aplicativo do WPF.
title: 'Tutorial: Modernize um aplicativo WPF'
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, uwp, do windows forms, wpf, Ilhas de xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 5e7179d4aeb66cad547e31e2456da2e8264ebbcd
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420078"
---
# <a name="tutorial-modernize-a-wpf-app"></a>Tutorial: Modernize um aplicativo WPF 

Há muitas maneiras [modernize](index.md) código em vez de reescrever os aplicativos do zero de origem de aplicativos da área de trabalho existentes, integrando os recursos mais recentes do Windows à existente. Neste tutorial, vamos explorar de várias maneiras para modernizar um aplicativo de linha de negócios existente do WPF ao usar esses recursos:

* .NET Core 3
* Controles UWP XAML com ilhas de XAML
* Os cartões adaptáveis e Windows 10 notificações
* Implantação de MSIX

Este tutorial requer as habilidades de desenvolvimento a seguir:

* Experiência no desenvolvimento de aplicativos da área de trabalho do Windows com o WPF.
* Conhecimento básico do C# e XAML.
* Conhecimento básico da UWP.

## <a name="overview"></a>Visão geral

Este tutorial fornece o código para um aplicativo de linha de negócios simples do WPF chamado Contoso despesas. No cenário do tutorial de fictícia, Contoso despesas é um aplicativo interno usado pelos gerentes da Contoso Corporation para manter o controle de despesas enviadas por seus relatórios. Os gerentes agora são equipados com dispositivos sensíveis ao toque e gostaria de usar o aplicativo de despesas de Contoso sem um mouse ou teclado. Infelizmente, a versão atual do aplicativo não é sensível ao toque amigável.

A Contoso deseja modernizar este aplicativo com os novos recursos do Windows para permitir que funcionários criem relatórios de despesas com mais eficiência. Muitos dos recursos pode ser implementados com facilidade, criando um novo aplicativo UWP. No entanto, o aplicativo existente é complexo e é o resultado de muitos anos de desenvolvimento por equipes diferentes. Como tal, reescrevê-los a partir do zero com uma nova tecnologia não é uma opção. A equipe está procurando a melhor abordagem adicionar novos recursos para a base de código existente.

No início do tutorial, despesas de Contoso tem como alvo o .NET Framework 4.7.2 e usa as seguintes bibliotecas de terceiros 3ª:

* O MVVM Light, uma implementação básica para o padrão MVVM.
* Unity, um contêiner de injeção de dependência.
* LiteDb, uma solução NoSQL inserida para armazenar os dados.
* FALSO, uma ferramenta para gerar dados fictícios.

O tutorial, você aprimorará a Contoso despesas com novos recursos do Windows:

* Migre um aplicativo WPF existente para o .NET Core 3.0. Isso abrirá o cenários novos e importantes no futuro.
* Ilhas de XAML de uso para hospedar os **InkCanvas** e **MapControl** encapsulado controles fornecidos pelo Kit de ferramentas de comunidade do Windows.
* Ilhas de XAML de uso para hospedar qualquer controle UWP XAML padrão (nesse caso, uma **CalendardView**).
* Integre os cartões adaptáveis e Windows 10 notificações no aplicativo.
* Pacote de aplicativo com MSIX e configure um CI/CD pipeline no DevOps do Azure para que você pode fornecer automaticamente as novas versões do aplicativo para testadores e usuários assim que ele está disponível.

## <a name="prerequisites"></a>Pré-requisitos

Para executar este tutorial, seu computador de desenvolvimento deve ter esses pré-requisitos instalados:

* Windows 10, versão 1903 (build 18362) ou uma versão posterior.
* [Visual Studio 2019](https://www.visualstudio.com).
* [SDK de visualização 3 do .NET core](https://dotnet.microsoft.com/download/dotnet-core/3.0) (instalar a versão de visualização disponível mais recente).

Certifique-se de que instalar as seguintes cargas de trabalho e os recursos opcionais com 2019 do Visual Studio:

* Desenvolvimento de área de trabalho do .NET
* Desenvolvimento para a Plataforma Universal do Windows
* SDK do Windows 10 (10.0.18362.0 ou posterior)

## <a name="get-the-contoso-expenses-sample-app"></a>Obtenha o aplicativo de exemplo da Contoso despesas

Antes de começar o tutorial, baixe o código-fonte para o aplicativo de despesas de Contoso e certificar-se de que você pode compilar o código no Visual Studio.

1. Baixe o código-fonte do aplicativo a **versões** guia da [repositório de workshop AppConsult WinAppsModernization](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop). É o link direto [ https://aka.ms/wamwc ](https://aka.ms/wamwc).
2. Abra o arquivo zip e extraia todo o conteúdo na raiz do seu **c:\\**  unidade. Ele criará uma pasta chamada **C:\WinAppsModernizationWorkshop**.
3. Abra o Visual Studio de 2019 e clique duas vezes no **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses\ContosoExpenses.sln** arquivo para abrir a solução.
4. Verifique se que você pode compilar, executar e depurar o projeto WPF de despesas de Contoso, pressionando as **iniciar** botão ou CTRL + F5.

## <a name="get-started"></a>Introdução

Depois que você tiver o código-fonte para o aplicativo de exemplo da Contoso despesas e você pode confirmar que você pode compilá-lo no Visual Studio, você está pronto para iniciar o tutorial:

* [Parte 1: Migrar o Contoso aplicativo de despesas para o .NET Core 3](modernize-wpf-tutorial-1.md)
* [Parte 2: Adicionar um controle InkCanvas UWP usando ilhas de XAML](modernize-wpf-tutorial-2.md)
* [Parte 3: Adicionar um controle de exibição de calendário UWP usando ilhas de XAML](modernize-wpf-tutorial-3.md)
* [Parte 4: Adicionar notificações e atividades de usuário do Windows 10](modernize-wpf-tutorial-4.md)
* [Parte 5: Empacotar e implantar com MSIX](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>Conceitos importantes

As seções a seguir fornecem um plano de fundo para alguns dos principais conceitos discutidos neste tutorial. Se você já estiver familiarizado com esses conceitos, você pode ignorar esta seção.

### <a name="universal-windows-platform-uwp"></a>UWP (Plataforma Universal do Windows)

No Windows 8, a Microsoft introduziu uma nova estrutura chamada o tempo de execução do Windows (WinRT). Diferentemente do .NET Framework, o WinRT é uma camada nativa de APIs que são expostas diretamente para os aplicativos. WinRT também introduziu as projeções de linguagem, que são camadas adicionadas sobre o tempo de execução para permitir aos desenvolvedores interagir com ele usando linguagens, como C# e o JavaScript além C++. Projeções permitem aos desenvolvedores criar aplicativos na parte superior do WinRT que utilizam o mesmo C# e o conhecimento XAML que adquiriu na criação de aplicativos com o .NET Framework. 

No Windows 10, a Microsoft introduziu o [plataforma Universal do Windows (UWP)](/windows/uwp/get-started/universal-application-platform-guide), que se baseia no WinRT. O recurso mais importante da UWP é que ele oferece um conjunto comum de APIs entre todas as plataformas de dispositivo: não importa se o aplicativo está em execução em uma área de trabalho, no Xbox One ou em um HoloLens, é possível usar as mesmas APIs.

No futuro, Windows 10 mais novos recursos são expostos por meio de APIs do WinRT, incluindo recursos, como a linha do tempo, projeto Roma e Windows Hello.

### <a name="msix-packaging"></a>Empacotamento de MSIX

[MSIX](http://aka.ms/msix) (anteriormente conhecido como AppX) é o modelo de empacotamento modernos para aplicativos do Windows. MSIX dá suporte a aplicativos UWP, bem como aplicativos de desktop para criação usando tecnologias como Win32, WPF, Windows Forms, Java, Electron e muito mais. Ao empacotar um aplicativo da área de trabalho em um pacote MSIX, você pode publicar seu aplicativo para a Microsoft Store. Seu aplicativo de desktop também obter a identidade do pacote quando ele é instalado, o que permite que seu aplicativo da área de trabalho usar um conjunto mais amplo de APIs do WinRT.

Para obter mais informações, consulte estes artigos:

* [Aplicativos de área de trabalho do pacote](/windows/uwp/porting/desktop-to-uwp-root)
* [Nos bastidores do seu aplicativo da área de trabalho de pacote](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>Ilhas de XAML

A partir do Windows 10, versão 1903, você pode hospedar controles UWP em aplicativos da área de trabalho não UWP usando um recurso chamado *XAML Ilhas*. Esse recurso permite que você aprimorar a aparência, a aparência e a funcionalidade dos seus aplicativos de área de trabalho existentes com os recursos mais recentes de interface do usuário do Windows 10 que só estão disponíveis por meio de controles da UWP. Isso significa que você pode usar recursos UWP, como Windows tinta e controles compatíveis com o sistema de Design Fluent em seu WPF existente, o Windows Forms, e C++ aplicativos do Win32.

Para obter mais informações, consulte [controles UWP em aplicativos da área de trabalho (Ilhas de XAML)](/windows/uwp/xaml-platform/xaml-host-controls). Este tutorial o orienta pelo processo de usar dois tipos diferentes de controles XAML ilha:

* O [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) e [MapControl](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) no Kit de ferramentas de comunidade do Windows. Esses controles WPF encapsulam a interface e a funcionalidade dos controles UWP correspondentes e podem ser usados como qualquer outro controle do WPF no designer do Visual Studio.

* A UWP [modo de exibição de calendário](/windows/uwp/design/controls-and-patterns/calendar-view) controle. Este é um controle padrão de UWP que você hospedará usando o [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) controle no Kit de ferramentas de comunidade do Windows.

### <a name="net-core-3"></a>.NET Core 3

[.NET core](https://docs.microsoft.com/dotnet/core/) é uma estrutura de código-fonte aberto que implementa uma versão de plataforma cruzada, leve e extensível com facilidade do .NET Framework completo. Em comparação com o .NET Framework completo, o tempo de inicialização do .NET Core é muito mais rápido e muitas das APIs foram otimizadas.

Por meio de suas primeiras versões, o foco do .NET Core foi para oferecer suporte a web ou aplicativos de back-end. Com o .NET Core, você pode criar facilmente aplicativos web escalonáveis ou APIs que podem ser hospedadas no Windows, Linux, ou em arquiteturas de microsserviço como contêineres do Docker.

O .NET Core 3 é a próxima versão principal do .NET Core. O destaque desse futuro lançamento é o suporte para aplicativos da área de trabalho do Windows, incluindo aplicativos Windows Forms e WPF. Você pode executar aplicativos de desktop do Windows novos e existentes em 3 do .NET Core e aproveitar os benefícios que o .NET Core tem a oferecer. Controles da UWP que são hospedados nas [ilhas de XAML](xaml-islands.md) também podem ser usados em aplicativos do Windows Forms e WPF destinados ao .NET Core 3.

> [!NOTE]
> WPF e Windows Forms não estão ficando entre plataformas e você não pode executar um WPF ou Windows Forms no Linux e MacOS. Os componentes de interface do usuário do WPF e do Windows Forms ainda têm uma dependência no sistema de renderização do Windows.

Para obter mais informações, consulte os seguintes artigos:

* [Anúncio do .NET Core 3.0 Versão Prévia 1](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-1-and-open-sourcing-windows-desktop-frameworks/)
* [Anúncio do .NET Core 3.0 Versão Prévia 2](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-2/)
* [Anúncio do .NET Core 3.0 Versão Prévia 2](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-3/)
* [Anúncio do .NET Core 3.0 Versão Prévia 4](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-4/)
* [Novidades no .NET Core 3.0](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0).