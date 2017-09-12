---
author: mcleanbyron
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: "Saiba como usar a classe AdControl para exibir anúncios em faixa em um aplicativo JavaScript/HTML para Windows 10 (UWP), Windows 8.1 ou Windows Phone 8.1."
title: AdControl em HTML 5 e JavaScript
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, anúncios, publicidade, AdControl, controle de anúncios, javascript, HTML"
ms.openlocfilehash: 44417516d773ea4faf103f6d4cdaf0bc8f290921
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2017
---
# <a name="adcontrol-in-html-5-and-javascript"></a>AdControl em HTML 5 e JavaScript

Este guia passo a passo mostra como usar a classe [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) para exibir anúncios em faixa em um aplicativo JavaScript/HTML para Windows 10 (UWP), Windows 8.1 ou Windows Phone 8.1.

Para um projeto de exemplo completo que demonstra como adicionar anúncios em um aplicativo JavaScript/HTML, consulte os [exemplos de publicidade no GitHub](http://aka.ms/githubads).

## <a name="prerequisites"></a>Pré-requisitos


* Para aplicativos UWP: instalar o [SDK do Microsoft Advertising](http://aka.ms/ads-sdk-uwp) com o Visual Studio 2015 ou uma versão posterior.
* Em aplicativos do Windows 8.1 ou Windows Phone 8.1: instale o [SDK do Microsoft Advertising para Windows e Windows Phone 8.x](http://aka.ms/store-8-sdk) com o Visual Studio 2015 ou o Visual Studio 2013.

> [!NOTE]
> Se você estiver adicionando anúncios em faixa a um aplicativo UWP e se tiver instalado o SDK do Windows 10 versão 10.0.14393 (Atualização de Aniversário) ou uma versão mais recente do SDK do Windows, também deverá instalar a biblioteca WinJS. Essa biblioteca costumava ser incluída em versões anteriores do SDK do Windows para Windows 10, mas a partir da versão 10.0.14393 do SDK do Windows 10 (Atualização de Aniversário), ela deve ser instalada separadamente. Para instalar o WinJS, consulte [Baixar o WinJS](http://try.buildwinjs.com/download/GetWinJS/).

## <a name="code-development"></a>Desenvolvimento de código

1. No Visual Studio, abra o projeto ou crie um novo projeto.

2. Se o seu projeto tem direcionamento **Any CPU**, atualize-o para usar uma saída de compilação de arquitetura específica (por exemplo, **x86**). Se o seu projeto tem direcionamento **Any CPU**, você não conseguirá adicionar uma referência à biblioteca do Microsoft Advertising nas etapas a seguir. Para obter mais informações, consulte [Erros de referência causados pelo direcionamento Any CPU em seu projeto](known-issues-for-the-advertising-libraries.md#reference_errors).

3.  Na janela **Gerenciador de Soluções**, clique com o botão direito do mouse em **Referências** e selecione **Adicionar Referência...**

4.  Em **Gerenciador de Referências**, selecione uma das seguintes referências dependendo do tipo de projeto:

    -   Para um projeto da Plataforma Universal do Windows (UWP): expanda **Universal Windows**, clique em **Extensões**e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para JavaScript** (versão 10.0).

    -   Para um projeto do Windows 8.1: expanda **Windows 8.1**, clique em **Extensões**e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para Windows 8.1 nativo (JS)**.

    -   Para um projeto do Windows Phone 8.1: Expanda o **Windows Phone 8.1**, clique em **Extensões**, e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para Windows Phone 8.1 Native (JS)**.

    ![javascriptaddreference](images/13-f7f6d6a6-161e-4f17-995d-1236d0b5d9f2.png)

5.  No **Gerenciador de Referências**, clique em OK.

6.  Abra o arquivo index.html (ou outro arquivo html apropriado para o seu projeto).

7.  Na seção **&lt;head&gt;**, após as referências JavaScript de default.css e main.js do projeto, adicione a referência ao ad.js.

    Em um projeto UWP, adicione o código a seguir.

    ``` HTML
    <!-- Advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

    Em um projeto do Windows 8.1 ou Windows Phone 8.1, adicione o código a seguir.

    ``` HTML
    <!-- Advertising required references -->
    <script src="/MSAdvertisingJS/ads/ad.js"></script>
    ```

    > [!NOTE]
    > Esta linha deve ser colocada na seção **&lt;topo&gt;** após a inclusão do main.js; caso contrário, você encontrará um erro ao compilar seu projeto. Se seu projeto for direcionado ao Windows 8.1 ou Windows Phone 8.1, o arquivo HTML padrão em seu projeto será denominado default.html em vez de index.html, e o arquivo JavaScript padrão em seu projeto será denominado default.js em vez de main.js.

8.  Modifique a seção **&lt;body&gt;** no arquivo default.html (ou outro arquivo html apropriado para o seu projeto) para incluir o elemento div do **AdControl**. Atribua as propriedades **applicationId** e **adUnitId** no **AdControl** aos valores de teste fornecidos em [Valores de modo de teste](test-mode-values.md). Ajuste também a altura e a largura do controle para que ele tenha um dos [tamanhos de anúncio compatíveis para anúncios em faixa](supported-ad-sizes-for-banner-ads.md).

    > [!NOTE]
    > Cada **AdControl** tem uma *unidade publicitária* correspondente que é usado por nossos serviços para veicular anúncios para o controle, e cada unidade de anúncio consiste em uma *ID da unidade publicitária* e *ID do aplicativo*. Nestas etapas, você atribui os valores da ID da unidade publicitária de teste e da ID do aplicativo para seu controle. Esses valores de teste só podem ser usados em uma versão de teste do seu app. Antes de publicar seu app na Loja, [substitua os valores de teste por valores reais](#release) do Centro de Desenvolvimento do Windows.

    ``` HTML
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
        data-win-control="MicrosoftNSJS.Advertising.AdControl"
        data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```

9.  Compile e execute o app para vê-lo com um anúncio.

O exemplo a seguir mostra o index.html completo para um aplicativo UWP simples.

``` HTML
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>AdControlExampleApp</title>
    <!-- WinJS references -->
    <link href="lib/winjs-4.0.1/css/ui-light.css" rel="stylesheet" />
    <script src="lib/winjs-4.0.1/js/base.js"></script>
    <script src="lib/winjs-4.0.1/js/ui.js"></script>
    <!-- AdControlExampleApp references -->
    <link href="css/default.css" rel="stylesheet" />
    <script src="js/main.js"></script>
    <!-- Required reference for AdControl -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
</head>
<body>
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    <p>Content goes here</p>
</body>
</html>
```

<span id="release" />
## <a name="release-your-app-with-live-ads-using-windows-dev-center"></a>Lançar o app com anúncios ativos usando o Centro de Desenvolvimento do Windows

1.  No painel do Centro de Desenvolvimento, vá até a página [Monetizar com anúncios](../publish/monetize-with-ads.md) do app e [crie uma unidade publicitária](../monetize/set-up-ad-units-in-your-app.md). Para o tipo de unidade publicitária, especifique **Faixa**. Anote a ID da unidade publicitária e a ID do aplicativo.

2. Se o seu app for um aplicativo UWP para o Windows 10, você pode, opcionalmente, ativar o controle de anúncios para o **AdControl** definindo as configurações na seção [Controle de anúncios](../publish/monetize-with-ads.md#mediation) na página [Monetizar com anúncios](../publish/monetize-with-ads.md). O controle de anúncios permite que você maximize seus recursos de promoção de apps e receita de anúncios exibindo anúncios de várias redes de anúncios, incluindo os anúncios de outras redes de anúncios pagas, como Taboola e Smaato e anúncios para campanhas promocionais de aplicativos da Microsoft.

3.  Em seu código, substitua os valores da unidade publicitária de teste (**applicationId** e **adUnitId**) pelos valores dinâmicos gerados no Centro de Desenvolvimento.

4.  [Envie seu aplicativo](../publish/app-submissions.md) para a Loja usando o painel do Centro de Desenvolvimento.

5.  Analise seus [relatórios de desempenho de publicidade](../publish/advertising-performance-report.md) no painel do Centro de Desenvolvimento.             

<span id="manage" />
## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Gerenciar unidades publicitárias para vários controles de anúncios em seu app

Você pode usar vários objetos **AdControl** em um único app (por exemplo, cada página em seu app pode hospedar um objeto **AdControl** diferente). Nesse cenário, nós recomendamos que você atribua uma unidade publicitária diferente para cada controle. O uso de unidades publicitárias diferentes para cada controle permite que você [defina as configurações de mediação](../publish/monetize-with-ads.md#mediation) separadamente e obtenha [dados de relatório](../publish/advertising-performance-report.md) discretos para cada controle. Isso também permite que nossos serviços melhorem a otimização dos anúncios veiculados em seu app.

> [!IMPORTANT]
> Você pode usar cada unidade publicitária em apenas um app. Se você usar uma unidade publicitária em mais de um app, os anúncios não serão veiculados para essa unidade publicitária.

## <a name="related-topics"></a>Tópicos relacionados

* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)
* [Configurar unidades publicitárias para seu app](../monetize/set-up-ad-units-in-your-app.md)
