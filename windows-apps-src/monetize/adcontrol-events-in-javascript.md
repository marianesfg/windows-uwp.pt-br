---
author: mcleanbyron
ms.assetid: 2383296e-c3d7-4b49-bcd2-621391228fdb
description: Saiba como manipular os eventos da classe AdControl.
title: Eventos AdControl em JavaScript
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: d24030dfae92451924000ba4f1ac19cf6c4d4abe


---

# Eventos AdControl em JavaScript




Os exemplos a seguir demonstram como manipular os eventos da classe [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx). Esses exemplos pressupõem que você atribuiu anteriormente os manipuladores de eventos aos eventos **AdControl**. Para obter mais informações sobre como fazer isso, consulte [Exemplo das propriedades HTML](html-properties-example.md).

Em JavaScript, os eventos **AdControl** devem ser encapsulados pela função [MarkSupportedForProcessing](http://msdn.microsoft.com/library/windows/apps/Hh967819.aspx). Para obter mais informações sobre como manipular eventos em JavaScript, consulte [Codificando aplicativos básicos (HTML)](https://msdn.microsoft.com/library/windows/apps/hh780660.aspx#adding-event-handlers).

## Exemplos

``` syntax
WinJS.Utilities.markSupportedForProcessing(
window.myAdError = function (sender, msg) {
  // place code here for when there is an error serving an ad.
  // e.g. you may opt to show a default experience, or reclaim the div for other purposes.
});

WinJS.Utilities.markSupportedForProcessing(
window.myAdRefreshed = function (sender) {
  // place code here that you wish to execute when the ad refreshes.
});

WinJS.Utilities.markSupportedForProcessing(
window.myAdEngagedChanged = function (sender) {
  if (true == sender.isEngaged) {
    // code here for when user engaged with ad, e.g. if a game, pause it.
  }
  else {
    // user no longer engaged with ad, include code to unpause.
  }
});
```

## Tópicos relacionados

* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)
* [Tratamento de erros de AdControl](adcontrol-error-handling.md)
* [Classe RoutedEventArgs](http://msdn.microsoft.com/library/system.windows.routedeventargs.aspx)

 

 



<!--HONumber=Aug16_HO3-->


