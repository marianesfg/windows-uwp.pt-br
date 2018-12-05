---
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: Saiba como usar a classe AdControl para exibir anúncios em faixa em um aplicativo JavaScript/HTML para Windows 10 (UWP).
title: AdControl em HTML 5 e JavaScript
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, anúncios, publicidade, AdControl, controle de anúncios, javascript, HTML
ms.localizationpriority: medium
ms.openlocfilehash: 08b834343aafb91fee1e75f9df7ed2a752992fa2
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8688820"
---
# <a name="adcontrol-in-html-5-and-javascript"></a>AdControl em HTML 5 e JavaScript

Este guia passo a passo mostra como usar a classe [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) para exibir anúncios em faixa em um aplicativo JavaScript/HTML da Plataforma Universal do Windows (UWP) para Windows 10.

Para um projeto de exemplo completo que demonstra como adicionar anúncios em um aplicativo JavaScript/HTML, consulte os [exemplos de publicidade no GitHub](http://aka.ms/githubads).

## <a name="prerequisites"></a>Pré-requisitos

* Instale o [SDK do Microsoft Advertising](http://aka.ms/ads-sdk-uwp) com o Visual Studio 2015 ou uma versão posterior do Visual Studio. Para obter instruções de instalação, consulte [este artigo](install-the-microsoft-advertising-libraries.md).

> [!NOTE]
> Se você tiver instalado a versão do SDK do Windows 10 10.0.14393 (atualização de aniversário) ou uma versão posterior do SDK do Windows, você também deve instalar a biblioteca [WinJS](https://github.com/winjs/winjs) . Essa biblioteca costumava ser incluída em versões anteriores do SDK do Windows para Windows 10, mas a partir da versão 10.0.14393 do SDK do Windows 10 (Atualização de Aniversário), ela deve ser instalada separadamente. 

## <a name="integrate-a-banner-ad-into-your-app"></a>Integrar um anúncio em faixa ao seu aplicativo

1. No Visual Studio, abra o projeto ou crie um novo projeto.

    > [!NOTE]
    > Se você estiver usando um projeto existente, abra o arquivo Package. appxmanifest em seu projeto e certifique-se de que o recurso da **Internet (cliente)** está selecionado. Seu aplicativo precisa dessa funcionalidade para receber anúncios de teste e anúncios ativos.

2. Se o seu projeto tem direcionamento **Any CPU**, atualize-o para usar uma saída de compilação de arquitetura específica (por exemplo, **x86**). Se o seu projeto tem direcionamento **Any CPU**, você não conseguirá adicionar uma referência à biblioteca do Microsoft Advertising nas etapas a seguir. Para obter mais informações, consulte [Erros de referência causados pelo direcionamento Any CPU em seu projeto](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Adicione uma referência ao SDK do Microsoft Advertising em seu projeto:

    1. Na janela **Gerenciador de Soluções**, clique com o botão direito do mouse em **Referências** e selecione **Adicionar Referência...**
    2.  No **Gerenciador de Referências**, expanda **Universal do Windows**, clique em **Extensões** e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para JavaScript** (versão 10.0).
    3.  No **Gerenciador de Referências**, clique em OK.

6.  Abra o arquivo index.html (ou outro arquivo html apropriado para o seu projeto).

7.  Na seção **&lt;head&gt;**, após as referências JavaScript de default.css e main.js do projeto, adicione a referência ao ad.js.

    ``` HTML
    <!-- Advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

    > [!NOTE]
    > Esta linha deve ser colocada na seção **&lt;topo&gt;** após a inclusão do main.js; caso contrário, você encontrará um erro ao compilar seu projeto.

8.  Modifique a seção **&lt;body&gt;** no arquivo default.html (ou outro arquivo html apropriado para o seu projeto) para incluir o elemento **div** do **AdControl**. Atribua as propriedades **applicationId** e **adUnitId** no **AdControl** aos valores de teste fornecidos em [valores de unidade publicitária de teste](set-up-ad-units-in-your-app.md#test-ad-units). Ajuste também a **altura** e a **largura** do controle para que ele tenha um dos [tamanhos de anúncio compatíveis para anúncios em faixa](supported-ad-sizes-for-banner-ads.md).

    > [!NOTE]
    > Cada **AdControl** tem uma *unidade publicitária* correspondente que é usado por nossos serviços para veicular anúncios para o controle, e cada unidade publicitária consiste em uma *ID da unidade publicitária* e *ID do aplicativo*. Nestas etapas, você atribui os valores da ID da unidade publicitária de teste e da ID do aplicativo para seu controle. Esses valores de teste só podem ser usados em uma versão de teste do seu app. Antes de publicar seu aplicativo para a loja, você deve [substituir esses valores por valores dinâmicos de teste](#release) do Partner Center.

    ``` HTML
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
        data-win-control="MicrosoftNSJS.Advertising.AdControl"
        data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```

9.  Compile e execute o app para vê-lo com um anúncio.

O exemplo a seguir mostra o index.html completo para um aplicativo simples.

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

### <a name="create-an-adcontrol-programmatically-in-javascript"></a>Criar um AdControl de forma programática no JavaScript

As etapas anteriores mostram como declarar um **AdControl** na marcação HTML. Como alternativa, você pode criar programaticamente um **AdControl** usando JavaScript. Este exemplo considera que você esteja usando um **div** existente em seu HTML com a ID **myAd**.

> [!div class="tabbedCodeSnippets"]
[!code-javascript[AdControl](./code/AdvertisingSamples/AdControlSamples/js/main.js#DeclareAdControl)]

Este exemplo presume que você já tenha declarado os métodos de manipulador de eventos chamados **myAdError**, **myAdRefreshed** e **myAdEngagedChanged**.

Se você usa esse código e não vê anúncios, tente inserir um atributo **position:relative** no **div** que contém o **AdControl**. Isso substituirá a configuração padrão do **IFrame**. Os anúncios serão exibidos corretamente, a menos que eles não estejam sendo mostrados devido ao valor desse atributo. Observe que novas unidades de anúncios podem não estar disponíveis por até 30 minutos.

> [!NOTE]
> Os valores de *applicationId* e *adUnitId* mostrados neste exemplo são [valores de modo de teste](set-up-ad-units-in-your-app.md#test-ad-units). Você deve [substituir esses valores por valores dinâmicos](set-up-ad-units-in-your-app.md#live-ad-units) do Partner Center antes de enviar seu aplicativo para envio.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Liberar seu aplicativo com anúncios ativos

1. Verifique se o uso de anúncios em faixa no aplicativo segue as [diretrizes para anúncios em faixa](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads).

1.  No Partner Center, vá para a página de [anúncios no aplicativo](../publish/in-app-ads.md) e [criar uma unidade de anúncio](set-up-ad-units-in-your-app.md#live-ad-units). No tipo de unidade publicitária, especifique **Faixa**. Anote a ID da unidade publicitária e a ID do aplicativo.
    > [!NOTE]
    > Os valores da ID de aplicativo para unidades publicitárias de teste e unidades publicitárias dinâmicas UWP têm formatos diferentes. Valores de ID de aplicativo de teste são GUIDs. Quando você cria uma unidade de publicitária dinâmica UWP no Partner Center, o valor de ID do aplicativo para a unidade publicitária sempre corresponde a ID da loja do aplicativo (um valor de ID da loja de exemplo é semelhante a 9NBLGGH4R315).

2. Como alternativa, você pode habilitar o controle de anúncios para **AdControl** ao definir as configurações na seção [Configurações de controle](../publish/in-app-ads.md#mediation) na página [Anúncios no app](../publish/in-app-ads.md). O controle de anúncios permite que você maximize seus recursos de promoção de apps e receita de anúncios exibindo anúncios de várias redes de anúncios, incluindo os anúncios de outras redes de anúncios pagas, como Taboola e Smaato e anúncios para campanhas promocionais de aplicativos da Microsoft.

3.  Em seu código, substitua os valores de unidade de anúncio de teste (**applicationId** e **adUnitId**) pelos valores dinâmicos gerados no Partner Center.

4.  [Enviar seu aplicativo](../publish/app-submissions.md) para a loja usando o Partner Center.

5.  Examine os [relatórios de desempenho de publicidade](../publish/advertising-performance-report.md) no Partner Center.             

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Gerenciar unidades publicitárias para vários controles de anúncios em seu app

Você pode usar vários objetos **AdControl** em um único app (por exemplo, cada página em seu app pode hospedar um objeto **AdControl** diferente). Nesse cenário, nós recomendamos que você atribua uma unidade publicitária diferente para cada controle. O uso de unidades publicitárias diferentes para cada controle permite que você [defina as configurações de mediação](../publish/in-app-ads.md#mediation) separadamente e obtenha [dados de relatório](../publish/advertising-performance-report.md) discretos para cada controle. Isso também permite que nossos serviços melhorem a otimização dos anúncios veiculados em seu app.

> [!IMPORTANT]
> Você pode usar cada unidade publicitária em apenas um app. Se você usar uma unidade publicitária em mais de um app, os anúncios não serão veiculados para essa unidade publicitária.

## <a name="related-topics"></a>Tópicos relacionados

* [Diretrizes para anúncios em faixa](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)
* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)
* [Configurar unidades publicitárias para seu aplicativo](set-up-ad-units-in-your-app.md)
* [Processamento de erros no guia passo a passo do JavaScript](error-handling-in-javascript-walkthrough.md)
