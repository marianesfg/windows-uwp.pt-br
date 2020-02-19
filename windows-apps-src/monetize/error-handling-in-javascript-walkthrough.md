---
ms.assetid: 08b4ae43-69e8-4424-b3c0-a07c93d275c3
description: Saiba como detectar erros de AdControl em seu aplicativo.
title: Tratamento de erros no Guia passo a passo do JavaScript
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, anúncios, publicidade, tratamento de erros, java script
ms.localizationpriority: medium
ms.openlocfilehash: 01b9949a17d5653bf121018dc40058b99af21719
ms.sourcegitcommit: 6af7ce0e3c27f8e52922118deea1b7aad0ae026e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77463848"
---
# <a name="error-handling-in-javascript-walkthrough"></a>Tratamento de erros no Guia passo a passo do JavaScript

>[!WARNING]
> A partir de 1º de junho de 2020, a plataforma Microsoft ad monetização para aplicativos UWP do Windows será desligada. [Saiba mais](https://aka.ms/ad-monetization-shutdown)

Este guia passo a passo demonstra como detectar erros relacionados a anúncios em seu aplicativo JavaScript. O passo a passo usa um [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) para exibir um anúncio em faixa, mas os conceitos gerais nele também se aplicam a anúncios intersticiais e anúncios nativos.

Estes exemplos pressupõem que você tenha um aplicativo JavaScript que contém um **AdControl**. Para obter instruções passo a passo que demonstram como adicionar um **AdControl** ao seu aplicativo, consulte [AdControl em HTML 5 e Javascript](adcontrol-in-html-5-and-javascript.md). Para um projeto de exemplo completo que demonstra como adicionar anúncios em faixa a um aplicativo JavaScript/HTML, consulte os [Exemplos de publicidade no GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

1.  No arquivo default.html, adicione um valor para o evento **onErrorOccurred**, onde você define **data-win-options** no **div** do **AdControl**. Encontre o código a seguir no arquivo default.html.
    ``` HTML
    <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```
    Seguindo o atributo **adUnitId**, adicione o valor para o evento **onErrorOccurred**.
    ``` HTML
    <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test', onErrorOccurred: errorLogger}">
  </div>
  ```

2.  Crie um **div** que exibirá o texto para que você possa ver as mensagens que estão sendo geradas. Para fazer isso, adicione o seguinte código após o **div** de **myAd**.
    ``` HTML
    <div style="position:absolute; width:100%; height:130px; top:300px; left:0px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

3.  Crie um **AdControl** que disparará um evento de erro. Pode haver somente uma id de aplicativo para todos os objetos **AdControl** em um aplicativo. Portanto, criar uma id adicional com uma id de aplicativo diferente disparará um erro em runtime. Para fazer isso, após as seções **div** anteriores que você adicionou, adicione o código a seguir ao corpo da página default.html.
    ``` HTML
    <!-- Because only one applicationId can be used, the following ad control will fire an error event. -->
    <div id="liveAd" style="position: absolute; top:500px; left:0px; width:480px; height:80px"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '00000000-0000-0000-0000-000000000000', adUnitId: 'test', onErrorOccurred: errorLogger }" >
    </div>
    ```

4.  No arquivo default.js do projeto, após a função de inicialização padrão, você adicionará o manipulador de eventos de **errorLogger**. Role até o final do arquivo e coloque o código a seguir depois do último ponto e vírgula.
    ``` javascript
    WinJS.Utilities.markSupportedForProcessing(
    window.errorLogger = function (sender, evt) {
        adEvents.innerHTML = (new Date()).toLocaleTimeString() + ": " +
        sender.element.id + " error: " + evt.errorMessage + " error code: " +
        evt.errorCode + "<br>" + adEvents.innerHTML;
        console.log("errorhandler hit. \n");
    });
    ```

5.  Compile e execute o aplicativo. Você verá o anúncio original do aplicativo de exemplo criado anteriormente e o texto sob esse anúncio, descrevendo o erro. Você não verá o anúncio com a id do **liveAd**.

## <a name="related-topics"></a>Tópicos relacionados

* [Amostras de publicidade no GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
