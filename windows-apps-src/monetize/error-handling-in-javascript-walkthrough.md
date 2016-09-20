---
author: mcleanbyron
ms.assetid: 08b4ae43-69e8-4424-b3c0-a07c93d275c3
description: Saiba como detectar erros de AdControl em seu aplicativo.
title: Tratamento de erros no Guia passo a passo do JavaScript
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: d26a8efeb253c6c793d8edd21d7452bbf15da261


---

# Tratamento de erros no Guia passo a passo do JavaScript


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este tópico demonstra como detectar erros de [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) em seu aplicativo.

Estes exemplos pressupõem que você tenha um aplicativo JavaScript/HTML que contém um **AdControl**. Para obter instruções passo a passo que demonstram como adicionar um **AdControl** ao seu aplicativo, consulte [AdControl em HTML 5 e Javascript](adcontrol-in-html-5-and-javascript.md). Para um projeto de exemplo completo que demonstra como adicionar anúncios em faixa a um aplicativo JavaScript/HTML, consulte os [Exemplos de publicidade no GitHub](http://aka.ms/githubads).

1.  No arquivo default.html, adicione um valor para o evento **onErrorOccurred**, onde você define **data-win-options** no **div** do **AdControl**. Encontre o código a seguir no arquivo default.html.

    ``` syntax
    <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: '10865270'}">
    </div>
    ```

    Seguindo o **adUnitId**, adicione o valor para o evento **onErrorOccurred**.

    ``` syntax
    onErrorOccurred: errorLogger
    ```

    Este é o código completo do **div**.

    ``` syntax
    <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: '10865270', onErrorOccurred: errorLogger}">
    </div>
    ```

2.  Crie um **div** que exibirá o texto para que você possa ver as mensagens que estão sendo geradas. Para fazer isso, adicione o seguinte código após o **div** de **myAd**.

    ``` syntax
    <div style="position:absolute; width:100%; height:130px; top:300px; left:0px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

3.  Crie um **AdControl** que disparará um evento de erro. Pode haver somente uma id de aplicativo para todos os objetos **AdControl** em um aplicativo. Portanto, criar uma id adicional com uma id de aplicativo diferente disparará um erro em tempo de execução. Para fazer isso, após as seções **div** anteriores que você adicionou, adicione o código a seguir ao corpo da página default.html.

    ``` syntax
    <!-- since only one applicationId can be used, the following ad control will fire an error event -->
    <div id="liveAd" style="position: absolute; top:500px; left:0px; width:480px; height:80px"
        data-win-control="MicrosoftNSJS.Advertising.AdControl"
        data-win-options="{applicationId: '00000000-0000-0000-0000-000000000000',
        adUnitId: '10865270', onErrorOccurred: errorLogger }" >
    </div>
    ```

4.  No arquivo default.js do projeto, após a função de inicialização padrão, você adicionará o manipulador de eventos de **errorLogger**. Role até o final do arquivo e coloque o código a seguir depois do último ponto e vírgula.

    ``` syntax
    WinJS.Utilities.markSupportedForProcessing(
    window.errorLogger = function (sender, evt) {
        adEvents.innerHTML = (new Date()).toLocaleTimeString() + ": " +
        sender.element.id + " error: " + evt.errorMessage + " error code: " +
        evt.errorCode + "<br>" + adEvents.innerHTML;
        console.log("errorhandler hit. \n");
    });
    ```

5.  Compile e execute o aplicativo.

Você verá o anúncio original do aplicativo de exemplo criado anteriormente e o texto sob esse anúncio, descrevendo o erro. Você não verá o anúncio com a id do **liveAd**.

## Tópicos relacionados

* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)

 



<!--HONumber=Jun16_HO4-->


