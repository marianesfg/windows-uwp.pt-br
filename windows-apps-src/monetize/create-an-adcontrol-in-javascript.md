---
author: mcleanbyron
ms.assetid: 48a1ef86-8514-4af8-9c93-81e869d36de7
description: Saiba como criar programaticamente um **AdControl** usando JavaScript.
title: Criar um AdControl em JavaScript
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, anúncios, publicidade, AdControl, javascript"
ms.openlocfilehash: b669925c3b630ddbfe82086231c46c951072244b
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="create-an-adcontrol-in-javascript"></a>Criar um AdControl em JavaScript




O exemplo deste artigo mostra como criar programaticamente um [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) em JavaScript. Este artigo presume que você já adicionou as referências necessárias ao seu projeto para usar um **AdControl**. Para obter mais informações, incluindo um passo a passo detalhado para criar e inicializar um **AdControl** na marcação HTML em vez de JavaScript, consulte [AdControl em HTML 5 e Javascript](adcontrol-in-html-5-and-javascript.md).

## <a name="html-div-for-an-adcontrol"></a>Div HTML para um AdControl

Um **AdControl** precisa ter um **div** na página html que mostrará o anúncio. O código a seguir fornece um exemplo de tal **div**.

> [!div class="tabbedCodeSnippets"]
``` html
<div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
    data-win-control="MicrosoftNSJS.Advertising.AdControl">
</div>
```

## <a name="javascript-for-creating-an-adcontrol"></a>JavaScript para a criação de um AdControl

O exemplo a seguir presume que você esteja usando um **div** existente em seu HTML com a ID **myAd**.

Instancie o **AdControl** na função **app.onactivated**.

> [!div class="tabbedCodeSnippets"]
[!code-javascript[AdControl](./code/AdvertisingSamples/AdControlSamples/js/main.js#DeclareAdControl)]

Este exemplo presume que você já tenha declarado os métodos de manipulador de eventos chamados **myAdError**, **myAdRefreshed** e **myAdEngagedChanged**.

>**Observação**&nbsp;&nbsp;Os valores de *applicationId* e *adUnitId* mostrados neste exemplo são [valores de modo de teste](test-mode-values.md). Você deve [substituir esses valores por valores dinâmicos](set-up-ad-units-in-your-app.md) do Centro de Desenvolvimento do Windows antes de enviar seu app.

Se você usa esse código e não vê anúncios, tente inserir um atributo **position:relative** no **div** que contém o **AdControl**. Isso substituirá a configuração padrão do **IFrame**. Os anúncios serão exibidos corretamente, a menos que eles não estejam sendo mostrados devido ao valor desse atributo. Observe que novas unidades de anúncios podem não estar disponíveis por até 30 minutos.

## <a name="related-topics"></a>Tópicos relacionados

* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)

 

 
