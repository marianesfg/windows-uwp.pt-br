---
author: Xansky
ms.assetid: 4e7c2388-b94e-4828-a104-14fa33f6eb2d
description: Saiba como usar a classe AdControl para exibir anúncios em faixa em um aplicativo XAML para Windows 10 (UWP).
title: AdControl em XAML e .NET
ms.author: mhopkins
ms.date: 03/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, anúncios, publicidade, AdControl, controle de anúncios, XAML, .net, passo a passo
ms.localizationpriority: medium
ms.openlocfilehash: d7549e2fc73bfd5ca3132146248747037c5fffc2
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4753612"
---
# <a name="adcontrol-in-xaml-and-net"></a>AdControl em XAML e .NET


Este guia passo a passo mostra como usar a classe [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) para exibir anúncios em faixa em um aplicativo JavaScript/XAML da Plataforma Universal do Windows (UWP) para Windows 10 implementado usando C#.

> [!NOTE]
> O SDK do Microsoft Advertising também oferece suporte a aplicativos XAML implementados em C++. Para obter um projeto de exemplo completo, consulte os [exemplos de publicidade no GitHub](http://aka.ms/githubads).

## <a name="prerequisites"></a>Pré-requisitos

* Instale o [SDK do Microsoft Advertising](http://aka.ms/ads-sdk-uwp) com o Visual Studio 2015 ou uma versão posterior do Visual Studio. Para obter instruções de instalação, consulte [este artigo](install-the-microsoft-advertising-libraries.md).

## <a name="integrate-a-banner-ad-into-your-app"></a>Integrar um anúncio em faixa ao seu aplicativo

1. No Visual Studio, abra o projeto ou crie um novo projeto.

    > [!NOTE]
    > Se você estiver usando um projeto existente, abra o arquivo Package. appxmanifest em seu projeto e certifique-se de que o recurso da **Internet (cliente)** está selecionado. Seu aplicativo precisa dessa funcionalidade para receber anúncios de teste e anúncios ativos.

2. Se o seu projeto tem direcionamento **Any CPU**, atualize-o para usar uma saída de compilação de arquitetura específica (por exemplo, **x86**). Se o seu projeto tem direcionamento **Any CPU**, você não conseguirá adicionar uma referência à biblioteca do Microsoft Advertising nas etapas a seguir. Para obter mais informações, consulte [Erros de referência causados pelo direcionamento Any CPU em seu projeto](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Adicione uma referência ao SDK do Microsoft Advertising em seu projeto:

    1. Na janela **Gerenciador de Soluções**, clique com o botão direito do mouse em **Referências** e selecione **Adicionar Referência...**
    2.  No **Gerenciador de Referências**, expanda **Universal do Windows**, clique em **Extensões** e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para XAML** (versão 10.0).
    3.  No **Gerenciador de Referências**, clique em OK.

4.  Modifique o XAML da página em que você está inserindo anúncios para incluir o namespace **Microsoft.Advertising.WinRT.UI**. Por exemplo, no aplicativo de exemplo padrão gerado pelo Visual Studio (chamado, neste aplicativo, MyAdFundedWindows10AppXAML), a página XAML é **MainPage.XAML**.

    A seção **Página** do arquivo MainPage.xaml gerado pelo Visual Studio tem o código a seguir.

    ``` xml
    <Page
      x:Class="MyAdFundedWindows10AppXAML.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:MyAdFundedWindows10AppXAML"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      mc:Ignorable="d">
      <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      </Grid>
    </Page>
    ```

    Adicione a referência ao namespace **Microsoft.Advertising.WinRT.UI** para que a seção **Página** do arquivo MainPage.xaml tenha o código a seguir.

    ``` xml
    <Page
      x:Class="MyAdFundedWindows10AppXAML.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:MyAdFundedWindows10AppXAML"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:UI="using:Microsoft.Advertising.WinRT.UI"
      mc:Ignorable="d">
      <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      </Grid>
    </Page>
    ```

5. Na marca **Grade**, adicione o código do **AdControl**. Atribua as propriedades [AdUnitId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) e [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) aos [valores de unidade publicitária de teste](set-up-ad-units-in-your-app.md#test-ad-units). Ajuste também a **Altura** e a **Largura** do controle para que ele tenha um dos [tamanhos de anúncio compatíveis para anúncios em faixa](supported-ad-sizes-for-banner-ads.md).

    > [!NOTE]
    > Cada **AdControl** tem uma *unidade publicitária* correspondente que é usado por nossos serviços para veicular anúncios para o controle, e cada unidade publicitária consiste em uma *ID da unidade publicitária* e *ID do aplicativo*. Nestas etapas, você atribui os valores da ID da unidade publicitária de teste e da ID do aplicativo para seu controle. Esses valores de teste só podem ser usados em uma versão de teste do seu app. Antes de publicar seu app na Loja, [substitua os valores de teste por valores reais](#release) do Centro de Desenvolvimento do Windows.

    A marca **Grade** se parece com o código a seguir.

    ``` xml
    <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
            AdUnitId="test"
            HorizontalAlignment="Left"
            Height="250"
            VerticalAlignment="Top"
            Width="300"/>
    </Grid>
    ```

    O código completo do arquivo MainPage.xaml deve ter a aparência a seguir.

    ``` xml
    <Page
      x:Class="MyAdFundedWindows10AppXAML.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:MyAdFundedWindows10AppXAML"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:UI="using:Microsoft.Advertising.WinRT.UI"
      mc:Ignorable="d">
      <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
            <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
                  AdUnitId="test"
                  HorizontalAlignment="Left"
                  Height="250"
                  VerticalAlignment="Top"
                  Width="300"/>
      </Grid>
    </Page>
    ```

6.  Compile e execute o aplicativo para vê-lo com um anúncio.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Lançar o aplicativo com anúncios ativos

1. Verifique se o uso de anúncios em faixa no aplicativo segue as [diretrizes para anúncios em faixa](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads).

2.  No painel do Centro de Desenvolvimento, vá para a página [Anúncios no app](../publish/in-app-ads.md) e [crie uma unidade publicitária](set-up-ad-units-in-your-app.md#live-ad-units). No tipo de unidade publicitária, especifique **Faixa**. Anote a ID da unidade publicitária e a ID do aplicativo.
    > [!NOTE]
    > Os valores da ID de aplicativo para unidades publicitárias de teste e unidades publicitárias dinâmicas UWP têm formatos diferentes. Valores de ID de aplicativo de teste são GUIDs. Ao criar uma unidade publicitária dinâmica UWP no painel, o valor da ID de aplicativo da unidade publicitária sempre corresponde à ID da Microsoft Store do seu aplicativo (um valor de valor da ID da Microsoft Store é semelhante a 9NBLGGH4R315).

3. Como alternativa, você pode habilitar o controle de anúncios para **AdControl** ao definir as configurações na seção [Configurações de controle](../publish/in-app-ads.md#mediation) na página [Anúncios no app](../publish/in-app-ads.md). O controle de anúncios permite que você maximize seus recursos de promoção de apps e receita de anúncios exibindo anúncios de várias redes de anúncios, incluindo os anúncios de outras redes de anúncios pagas, como Taboola e Smaato e anúncios para campanhas promocionais de aplicativos da Microsoft.

4.  Em seu código, substitua os valores da unidade publicitária de teste (**ApplicationId** e **AdUnitId**) pelos valores dinâmicos gerados no Centro de Desenvolvimento.

5.  [Envie seu aplicativo](../publish/app-submissions.md) para a Loja usando o painel do Centro de Desenvolvimento.

6.  Analise seus [relatórios de desempenho de publicidade](../publish/advertising-performance-report.md) no painel do Centro de Desenvolvimento.

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Gerenciar unidades publicitárias para vários controles de anúncios em seu app

Você pode usar vários objetos **AdControl** em um único app (por exemplo, cada página em seu app pode hospedar um objeto **AdControl** diferente). Nesse cenário, nós recomendamos que você atribua uma unidade publicitária diferente para cada controle. O uso de unidades publicitárias diferentes para cada controle permite que você [defina as configurações de mediação](../publish/in-app-ads.md#mediation) separadamente e obtenha [dados de relatório](../publish/advertising-performance-report.md) discretos para cada controle. Isso também permite que nossos serviços melhorem a otimização dos anúncios veiculados em seu app.

> [!IMPORTANT]
> Você pode usar cada unidade publicitária em apenas um app. Se você usar uma unidade publicitária em mais de um app, os anúncios não serão veiculados para essa unidade publicitária.

## <a name="related-topics"></a>Tópicos relacionados

* [Diretrizes para anúncios em faixa](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)
* [Processamento de erros no passo a passo do XAML/C#](error-handling-in-xamlc-walkthrough.md).
* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)
* [Configurar unidades publicitárias para seu app](set-up-ad-units-in-your-app.md)
