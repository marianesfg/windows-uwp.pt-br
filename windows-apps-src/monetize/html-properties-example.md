---
author: mcleanbyron
ms.assetid: 5fa16a27-fdc0-43b2-84cd-8547fd4915de
description: Saiba como atribuir propriedades ** AdControl ** em HTML.
title: Exemplo de propriedades HTML
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: 1898ed2ccad74ac33c5130c627363e0a9daebceb


---

# Exemplo de propriedades HTML




O exemplo a seguir mostra como atribuir propriedades [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) em HTML. O **applicationId** e o **adUnitId** são propriedades obrigatórias. As outras propriedades e eventos são opcionais. Se você não fornecer valores de propriedades opcionais, o controle usará os valores padrão que criar um anúncio consistente com a experiência do usuário do aplicativo.

As última cinco linhas são um exemplo de registro de funções para eventos **AdControl**. Você só pode registrar funções definidas no seu código JavaScript.

Estes valores são exemplos. Em seu código, você definirá os valores dessas funções e propriedades para que sejam apropriados para seu aplicativo.

``` syntax
data-win-control="MicrosoftNSJS.Advertising.AdControl"
data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1',
                    adUnitId: '10865270',
                    isAutoRefreshEnabled: false,
                    onAdRefreshed: myAdRefreshed,
                    onErrorOccurred: myAdError,
                    onEngagedChanged: myAdEngagedChanged,
                    onPointerDown: myPointerDown,
                    onPointerUp: myPointerUp }"
```

## Tópicos relacionados

* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)

 



<!--HONumber=Aug16_HO3-->


