---
author: mcleanbyron
ms.assetid: cb7380d0-bc14-4936-aa1c-206304b3dc70
description: Saiba como manipular erros gerados pela classe AdControl nas bibliotecas do Microsoft Advertising.
title: Tratamento de erros com as bibliotecas do Microsoft Advertising
---

# Tratamento de erros com as bibliotecas do Microsoft Advertising


\[ Atualizado para aplicativos UWP no Windows 10. Para artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este tópico fornece informações básicas sobre como manipular erros gerados pela classe [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) nas bibliotecas do Microsoft Advertising.

<span id="bkmk-javascript"/>
## Aplicativos JavaScript/HTML

Para manipular erros de **AdControl** em um aplicativo JavaScript:

1.  Atribuir o evento **onErrorOccurred** a um manipulador de eventos.

2.  Codifique o manipulador de eventos.

O manipulador de eventos **onErrorOccurred** é definido no atributo **data-win-options** do **div** do **AdControl**. No exemplo a seguir, o evento **onErrorOccurred** é definido para ser manipulado por uma função chamada **errorLogger**.

``` syntax
<div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 250px; height: 250px; z-index: 1"
     data-win-control="MicrosoftNSJS.Advertising.AdControl"
     data-win-options="{applicationId: 'd25517cb-12d4-4699-8bdc-52040c712cab', adUnitId: 'ADPT33', onErrorOccurred: errorLogger}">
</div>
```

A função de tratamento de erros é declarativa e deve ser colocada entre a função [markSupportedForProcessing](http://msdn.microsoft.com/library/windows/apps/Hh967819.aspx).

O manipulador de erros detecta o objeto de erro de JavaScript quando um erro de **AdControl** ocorre. O objeto de erro fornece dois argumentos para o manipulador de erros. Para obter mais informações, consulte [Propriedades de erro especiais de métodos assíncronos do Windows Runtime](http://msdn.microsoft.com/library/windows/apps/hh994690.aspx).

Aqui está um exemplo de uma função de tratamento de erros chamada **errorLogger** que manipula o evento **onErrorOccurred**.

``` syntax
WinJS.Utilities.markSupportedForProcessing(
window.errorLogger = function (sender, evt) {
    console.log(new Date()).toLocaleTimeString() + ": " + sender.element.id + " error: " + evt.errorMessage + " error code: " + evt.errorCode + \n");
});
```

Consulte [Tratamento de erros no guia passo a passo do JavaScript](error-handling-in-javascript-walkthrough.md) para ver um guia passo a passo que demonstra o tratamento de erros de **AdControl** em JavaScript.

<span id="bkmk-dotnet"/>
## Aplicativos XAML

Para manipular erros de **AdControl** em um aplicativo XAML:

* Atribua o evento **ErrorOccurred** para o nome de um representante do manipulador de eventos.

* Codifique o representante de tratamento de eventos de erro para que ele use dois parâmetros: **Object** para o remetente e um objeto **AdErrorEventArgs**.

Aqui está um exemplo que atribui um representante chamado **OnAdError** ao evento **ErrorOccurred**.

``` syntax
this.ErrorOccurred = OnAdError;
```

Aqui está uma definição de exemplo do representante **OnAdError** que grava as informações de erro na janela de saída no Visual Studio.

``` syntax
private void OnAdError(object sender, AdErrorEventArgs e)
{
    System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name + "): " + e.Error + " ErrorCode: " + e.ErrorCode.ToString());
}
```

Consulte [Tratamento de erros no guia passo a passo XAML/C#](error-handling-in-xamlc-walkthrough.md) para um guia passo a passo que demonstra o tratamento de erros de **AdControl** em XAML e C#.

 

 


<!--HONumber=May16_HO2-->


