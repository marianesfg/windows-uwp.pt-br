---
author: mcleanbyron
ms.assetid: 48a1ef86-8514-4af8-9c93-81e869d36de7
description: Saiba como criar programaticamente um **AdControl** em JavaScript.
title: Criar uma AdControl em JavaScript
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 481f9d785181ca197debdb807bb0b0c7b4168632


---

# Criar uma AdControl em JavaScript


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este exemplo mostra como criar programaticamente um [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) em JavaScript.

## Div HTML para um AdControl

Um **AdControl** precisa ter um **div** na página html que mostrará o anúncio. O código a seguir fornece um exemplo de tal **div**.

``` syntax
<div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
    data-win-control="MicrosoftNSJS.Advertising.AdControl">
</div>
```

## JavaScript para a criação de um AdControl

O exemplo a seguir presume que você esteja usando um **div** existente em seu HTML com a ID **myAd**.

Instancie o **AdControl** na função **app.onactivated**.

``` syntax
// TODO: This application has been newly launched. Initialize
// your application here.
var adDiv = document.getElementById("myAd");
var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
        applicationId: "3f83fe91-d6be-434d-a0ae-7351c5a997f1",
        adUnitId: "10865270"
    });
myAdControl.isAutoRefreshEnabled = false;
myAdControl.onErrorOccurred = myAdError;
myAdControl.onAdRefreshed = myAdRefreshed;
myAdControl.onEngagedChanged = myAdEngagedChanged;
```

Os valores são exemplos. Em seu código, você definirá os valores dessas funções e propriedades apropriados para seu aplicativo.

Se você usa esse código e não vê anúncios, tente inserir um atributo **position:relative** no **div** que contém o **AdControl**. Isso substituirá a configuração padrão do **IFrame**. Os anúncios serão exibidos corretamente, a menos que eles não estejam sendo mostrados devido ao valor desse atributo. Observe que novas unidades de anúncios podem não estar disponíveis por até 30 minutos.

## Tópicos relacionados

* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)

 

 



<!--HONumber=Jun16_HO4-->


