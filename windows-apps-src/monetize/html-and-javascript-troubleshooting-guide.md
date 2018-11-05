---
author: Xansky
ms.assetid: 7a61c328-77be-4614-b117-a32a592c9efe
description: Leia sobre soluções para problemas comuns de desenvolvimento com as bibliotecas do Microsoft Advertising em aplicativos JavaScript/HTML.
title: Guia de solução de problemas em HTML e JavaScript
ms.author: mhopkins
ms.date: 08/23/2017
ms.topic: article
keywords: windows 10, uwp, anúncios, publicidade, AdControl, solução de problemas, HTML, javascript
ms.localizationpriority: medium
ms.openlocfilehash: 38f0b36769d13d119965e7d15c5812b9ba1d6ecd
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6039301"
---
# <a name="html-and-javascript-troubleshooting-guide"></a>Guia de solução de problemas em HTML e JavaScript

Este tópico contém soluções para problemas comuns de desenvolvimento com as bibliotecas do Microsoft Advertising em aplicativos JavaScript/HTML.

* [HTML](#html)
  * [AdControl não aparece](#html-notappearing)
  * [Caixa preta pisca e desaparece](#html-blackboxblinksdisappears)
  * [Anúncios não são atualizados](#html-adsnotrefreshing)

* [JavaScript](#js)
  * [AdControl não aparece](#js-adcontrolnotappearing)
  * [Caixa preta pisca e desaparece](#js-blackboxblinksdisappears)
  * [Anúncios não são atualizados](#js-adsnotrefreshing)

## <a name="html"></a>HTML

<span id="html-notappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl não aparece

1.  Certifique-se de que a funcionalidade **Internet (Cliente)** esteja selecionada em Package.appxmanifest.

2.  Certifique-se de que a referência de JavaScript esteja presente. Sem a referência ad.js na seção &lt;head&gt; (após a referência default. js), o **AdControl** não poderá ser exibido e ocorrerá um erro durante a compilação.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <head>
        ...
        <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
        ...
    </head>
    ```

3.  Verifique a ID do aplicativo e a ID de unidade publicitária. Esses IDs devem coincidir com a ID do aplicativo e a ID de unidade de anúncio que você obteve no Partner Center. Para obter mais informações, consulte [Configurar unidades de anúncios em seu aplicativo](set-up-ad-units-in-your-app.md#live-ad-units).

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 50px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

4.  Verifique as propriedades **height** e **width**. Elas devem ser definidas como um dos [tamanhos de anúncio com suporte para anúncios em faixa](supported-ad-sizes-for-banner-ads.md).

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 50px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

5.  Verifique o posicionamento do elemento. O [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) deve estar dentro da área visível.

6.  Verifique a propriedade **visibility**. Essa propriedade não deve ser configurada para ser recolhida nem oculta. Essa propriedade pode ser definida embutida (como mostrado abaixo) ou em uma folha de estilos externa.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

7.  Verifique a propriedade **position**. A propriedade position deve ser definida como um valor apropriado de acordo com as outras propriedades do elemento (por exemplo, as margens no elemento pai e no índice z). Essa propriedade pode ser definida embutida (como mostrado abaixo) ou em uma folha de estilos externa.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

8.  Verifique a propriedade **z-index**. A propriedade **z-index** deve ser definida alta o suficiente para que o **AdControl** sempre apareça na parte superior dos outros elementos. Essa propriedade pode ser definida embutida (como mostrado abaixo) ou em uma folha de estilos externa.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

9.  Verifique as folhas de estilos externas. Se as propriedades forem definidas no elemento **AdControl** por meio de uma folha de estilos externa, certifique-se de que todas as propriedades acima estejam definidas corretamente.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

10. Verifique o pai do **AdControl**. Se o **AdControl** residir em um elemento pai, o pai deverá estar ativo e visível.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div style="position: absolute; width: 500px; height: 500px;">
        <div id="myAd" style="position: relative; top: 0px; left: 100px;
                              width: 250px; height: 250px; z-index: 1"
             data-win-control="MicrosoftNSJS.Advertising.AdControl"
             data-win-options="{applicationId: 'ApplicationID',
                                adUnitId: 'AdUnitID'}">
        </div>
    </div>
    ```

11. Certifique-se de que o **AdControl** não esteja oculto no visor. O **AdControl** deve estar visível para que os anúncios sejam exibidos corretamente.

12. Valores dinâmicos para [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) e [AdUnitId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) não devem ser testados no emulador. Para garantir que o **AdControl** está funcionando como esperado, use os [valores de teste](set-up-ad-units-in-your-app.md#test-ad-units) para **ApplicationId** e **AdUnitId**.

<span id="html-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>Caixa preta pisca e desaparece

1.  Verifique novamente todas as etapas na seção [AdControl não aparece](#html-notappearing) anterior.

2.  Manipule o evento **onErrorOccurred** e use a mensagem que é transmitida ao manipulador de eventos para determinar se ocorreu um erro e qual tipo de erro foi gerado. É possível encontrar mais detalhes em [Passo a passo do tratamento de erros em JavaScript](error-handling-in-javascript-walkthrough.md).

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 728px; height: 90px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger}">
    </div>
    <div style="position:absolute; width:100%; height:130px; top:300px; left:0px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

    O erro mais comum que gera uma caixa preta é "Nenhum anúncio disponível." Esse erro significa que não há anúncios disponíveis para serem retornados da solicitação.

3.  O **AdControl** está funcionando normalmente. Por padrão, o **AdControl** será recolhido quando não conseguir exibir um anúncio. Se outros elementos forem filhos do mesmo pai, eles poderão se mover para preencher a lacuna do **AdControl** recolhido e se expandir quando a próxima solicitação for feita.

<span id="html-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>Anúncios não são atualizados

1.  Verifique a propriedade **isAutoRefreshEnabled**. Por padrão, essa propriedade opcional é definida como verdadeira. Quando definida como false, o método **refresh** deve ser usado para recuperar outro anúncio.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{ applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger,
                            isAutoRefreshEnabled: true}">
    </div>
    ```

2.  Verifique as chamadas para o método **refresh**. Ao usar a atualização automática, não é possível usar **refresh** para recuperar outro anúncio. Ao usar a atualização manual, **refresh** só deve ser chamado depois de um intervalo mínimo de 30 a 60 segundos dependendo da conexão de dados atual do dispositivo.

    Este exemplo demonstra como usar o método **refresh**. O código HTML a seguir mostra um exemplo de como instanciar o **AdControl** com **isAutoRefreshEnabled** definido como falso.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{ applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger,
                            isAutoRefreshEnabled: false}">
    </div>
    ```

    Este exemplo demonstra como usar a função **refresh**.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    args.setPromise(WinJS.UI.processAll()
        .then(function (args) {
            window.setInterval(function()
            {
                document.getElementById("myAd").winControl.refresh();
            }, 60000)
        })
    );
    ```

3.  O **AdControl** está funcionando normalmente. Às vezes, o mesmo anúncio aparecerá mais do que uma vez em uma linha, dando a impressão de que os anúncios não estão sendo atualizados.

<span id="js"/>

## <a name="javascript"></a>JavaScript

<span id="js-adcontrolnotappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl não aparece

1.  Certifique-se de que a funcionalidade **Internet (Client)** esteja selecionada em Package.appxmanifest.

2.  Verifique se o **AdControl** está instanciado. Se o **AdControl** não estiver instanciado, ele não estará disponível.

    Os trechos de código a seguir mostram um exemplo de como instanciar o **AdControl**. Este código HTML mostra um exemplo de como configurar a interface do usuário para o **AdControl**

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl">
    </div>
    ```

    O código JavaScript a seguir mostra um exemplo de como instanciar o **AdControl**

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !==
                    activation.ApplicationExecutionState.terminated)
            {
                var adDiv = document.getElementById("myAd");
                var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
                {
                    applicationId: "{ApplicationID}",
                    adUnitId: "{AdUnitID}"
                 });                
                 myAdControl.onErrorOccurred = myAdError;
            } else {
                ...
            }
        }
    }
    ```

3.  Verifique o elemento pai. O **&lt;div&gt;** pai deve estar corretamente atribuído, ativo e visível.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var adDiv = document.getElementById("myAd");
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv, {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });  
    ```

4.  Verifique a ID do aplicativo e a ID de unidade de anúncios. Esses IDs devem coincidir com a ID do aplicativo e a ID de unidade de anúncio que você obteve no Partner Center. Para obter mais informações, consulte [Configurar unidades de anúncios em seu aplicativo](set-up-ad-units-in-your-app.md#live-ad-units).

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv, {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });  
    ```

5.  Verifique o elemento pai do **AdControl**. O pai deve estar ativo e visível.

6.  Valores dinâmicos para **ApplicationId** e **AdUnitId** não devem ser testados no emulador. Para garantir que o **AdControl** está funcionando como esperado, use os [valores de teste](set-up-ad-units-in-your-app.md#test-ad-units) para **ApplicationId** e **AdUnitId**.

<span id="js-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>Caixa preta pisca e desaparece

1.  Verifique novamente todas as etapas na seção [AdControl não aparece](#js-adcontrolnotappearing).

2.  Manipule o evento **onErrorOccurred** e use a mensagem que é transmitida ao manipulador de eventos para determinar se ocorreu um erro e qual tipo de erro foi gerado. É possível encontrar mais detalhes em [Passo a passo do tratamento de erros em JavaScript](error-handling-in-javascript-walkthrough.md).

    Este exemplo demonstra como implementar um manipulador de erros que informa as mensagens de erro. Este trecho de código HTML fornece um exemplo de como configurar a interface do usuário para exibir mensagens de erro.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div style="position:absolute; width:100%; height:130px; top:300px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

    Este exemplo demonstra como instanciar o **AdControl**. Esta função deve ser inserida no arquivo app.onactivated.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });                
    myAdControl.onErrorOccurred = myAdError;
    ```

    Este exemplo demonstra como relatar erros. Esta função deve ser inserida abaixo da função de execução automática no arquivo default. js.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    WinJS.Utilities.markSupportedForProcessing
    (
        window.errorLogger = function (sender, evt)
        {
            adEvents.innerHTML = (new Date()).toLocaleTimeString() + ": " +
            sender.element.id + " error: " + evt.errorMessage + " error code: " +
            evt.errorCode + "<br>" + adEvents.innerHTML;
        }
    );
    ```

    O erro mais comum que gera uma caixa preta é “Nenhum anúncio disponível”. Esse erro significa que não há anúncios disponíveis para serem retornados da solicitação.

3.  O **AdControl** está funcionando normalmente. Às vezes, o mesmo anúncio aparecerá mais do que uma vez em uma linha, dando a impressão de que anúncios não estão sendo atualizados.

<span id="js-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>Anúncios não são atualizados

1.  Verifique se a propriedade [IsAutoRefreshEnabled](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled.aspx) do **AdControl** está definida como false. Por padrão, essa propriedade opcional é definida como **true**. Quando definida como **false**, o método **Refresh** deve ser usado para recuperar outro anúncio.

2.  Verifique as chamadas para o método [Refresh](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.refresh.aspx). Ao usar a atualização automática (**IsAutoRefreshEnabled** definido como **true**), não é possível usar **Refresh** para recuperar outro anúncio. Quando se usar a atualização manual (**IsAutoRefreshEnabled** definido como **false**), **Refresh** só deverá ser chamado depois de um intervalo mínimo de 30 a 60 segundos dependendo da conexão de dados atual do dispositivo.

    Este exemplo demonstra como criar o **div** para o **AdControl**.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl">
    </div>
    ```

    Este exemplo mostra como usar a função **Atualizar**

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
      applicationId: "{ApplicationID}",
      adUnitId: "{AdUnitID}",
      isAutoRefreshEnabled: false
    });
    ...
    args.setPromise(WinJS.UI.processAll()
        .then(function (args) {
            window.setInterval(function()
            {
                document.getElementById("myAd").winControl.refresh();
            }, 60000)
        })
    );
    ```

3.  O **AdControl** está funcionando normalmente. Às vezes, o mesmo anúncio aparecerá mais do que uma vez em uma linha, dando a impressão de que anúncios não estão sendo atualizados.

 

 
