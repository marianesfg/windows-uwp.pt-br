---
author: Xansky
ms.assetid: cb7380d0-bc14-4936-aa1c-206304b3dc70
description: Saiba como manipular erros gerados pela classe AdControl nas bibliotecas do Microsoft Advertising.
title: Processamento de erros de anúncio
ms.author: mhopkins
ms.date: 05/11/2018
ms.topic: article
keywords: windows 10, uwp, anúncios, publicidade, tratamento de erros, java script, XAML, c#
ms.localizationpriority: medium
ms.openlocfilehash: a9fa05ed2ee946fcec9ffb5ff21abd9011db0f2a
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7171775"
---
# <a name="handle-ad-errors"></a>Processamento de erros de anúncio

As classes [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol), [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) e [NativeAdsManagerV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2) têm um evento **ErrorOccurred** acionado quando ocorre um erro relacionados ao anúncio. O código do aplicativo pode manipular esse evento e examinar as propriedades [ErrorCode](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs.errorcode) e [ErrorMessage](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs.errormessage) do objeto de argumentos de evento para ajudar a determinar a causa do erro.

<span id="bkmk-dotnet"/>

## <a name="xaml-apps"></a>Aplicativos XAML

Para processar erros relacionados ao anúncio em um aplicativo XAML:

1. Atribua o evento **ErrorOccurred** do objeto **AdControl**, **InterstitialAd** ou **NativeAdsManagerV2** ao nome de um representante do manipulador de eventos.

2. Codifique o representante de tratamento de eventos de erro para que ele use dois parâmetros: **Object** para o remetente e um objeto [AdErrorEventArgs](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs).

Aqui está um exemplo que atribui um representante chamado **OnAdError** ao evento **ErrorOccurred** de um objeto **AdControl** chamado *myBannerAdControl*.

> [!div class="tabbedCodeSnippets"]
``` csharp
myBannerAdControl.ErrorOccurred = OnAdError;
```

Aqui está uma definição de exemplo do representante **OnAdError** que grava as informações de erro na janela de saída no Visual Studio.

> [!div class="tabbedCodeSnippets"]
``` csharp
private void OnAdError(object sender, AdErrorEventArgs e)
{
    System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name + "): " + e.Error +
        " ErrorCode: " + e.ErrorCode.ToString());
}
```

Consulte [Tratamento de erros no guia passo a passo XAML/C#](error-handling-in-xamlc-walkthrough.md) para um guia passo a passo que demonstra o tratamento de erros de **AdControl** em XAML e C#.

<span id="bkmk-javascript"/>

## <a name="javascripthtml-apps"></a>Aplicativos JavaScript/HTML

Para manipular erros de **ErrorOccur** em um aplicativo JavaScript:

1.  Atribuir o evento **onErrorOccurred** a um manipulador de eventos.

2.  Codifique o manipulador de eventos.

Aqui está um exemplo que atribui um manipulador de eventos chamado **errorLogger** ao evento **ErrorOccurred** de um objeto **AdControl**.

> [!div class="tabbedCodeSnippets"]
``` html
<div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 250px; height: 250px; z-index: 1"
     data-win-control="MicrosoftNSJS.Advertising.AdControl"
     data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test', onErrorOccurred: errorLogger}">
</div>
```

A função de tratamento de erros é declarativa e deve ser colocada entre a função [markSupportedForProcessing](http://msdn.microsoft.com/library/windows/apps/Hh967819.aspx).

O manipulador de erros detecta o objeto de erro do JavaScript quando um erro ocorre. O objeto de erro fornece dois argumentos para o manipulador de erros. Para obter mais informações, consulte [Propriedades de erro especiais de métodos assíncronos do Windows Runtime](http://msdn.microsoft.com/library/windows/apps/hh994690.aspx).

Aqui está um exemplo de uma função de tratamento de erros chamada **errorLogger** que manipula o evento **onErrorOccurred**.

> [!div class="tabbedCodeSnippets"]
``` javascript
WinJS.Utilities.markSupportedForProcessing(
window.errorLogger = function (sender, evt) {
    console.log(new Date()).toLocaleTimeString() + ": " + sender.element.id + " error: " + evt.errorMessage +
    " error code: " + evt.errorCode + \n");
});
```

Consulte [Tratamento de erros no guia passo a passo do JavaScript](error-handling-in-javascript-walkthrough.md) para ver um guia passo a passo que demonstra o tratamento de erros de **AdControl** em JavaScript.
