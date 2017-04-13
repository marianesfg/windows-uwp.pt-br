---
author: mcleanbyron
ms.assetid: 5fa16a27-fdc0-43b2-84cd-8547fd4915de
description: Saiba como atribuir propriedades **AdControl** em HTML.
title: Exemplo de propriedades HTML AdControl
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, anúncios, publicidade, AdControl, HTML, propriedades"
ms.openlocfilehash: efc94b723b3bfe97b35484882ae784e9ef4d6807
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="adcontrol-html-properties-example"></a>Exemplo de propriedades HTML AdControl

O exemplo a seguir mostra como atribuir propriedades [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) em HTML. O **applicationId** e o **adUnitId** são propriedades obrigatórias. As outras propriedades e eventos são opcionais. Se você não fornecer valores de propriedades opcionais, o controle usará os valores padrão que criar um anúncio consistente com a experiência do usuário do aplicativo.

As última cinco linhas são um exemplo de registro de funções para eventos **AdControl**. Você só pode registrar funções definidas no seu código JavaScript.

Estes valores são exemplos. No código, você definirá os valores dessas funções e propriedades para que sejam indicados ao aplicativo.

> [!div class="tabbedCodeSnippets"]
``` html
<div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
    data-win-control="MicrosoftNSJS.Advertising.AdControl"
    data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1',
                       adUnitId: '10865270',
                       isAutoRefreshEnabled: false,
                       onAdRefreshed: myAdRefreshed,
                       onErrorOccurred: myAdError,
                       onEngagedChanged: myAdEngagedChanged,
                       onPointerDown: myPointerDown,
                       onPointerUp: myPointerUp }" />
```

## <a name="related-topics"></a>Tópicos relacionados

* [AdControl em HTML 5 e JavaScript](adcontrol-in-html-5-and-javascript.md)
* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)

 
