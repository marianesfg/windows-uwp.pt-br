---
author: mcleanbyron
ms.assetid: 2383296e-c3d7-4b49-bcd2-621391228fdb
description: Saiba como manipular os eventos da classe AdControl.
title: Eventos AdControl em JavaScript
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, anúncios, publicidade, AdControl, eventos, javascript"
ms.openlocfilehash: 62363ebce006f8ad21645d907c3e63360472887d
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="adcontrol-events-in-javascript"></a>Eventos AdControl em JavaScript

Os exemplos a seguir demonstram manipuladores de eventos básicos para os seguintes eventos [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx): [ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.erroroccurred.aspx), [AdRefreshed](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.adrefreshed.aspx) e [IsEngagedChanged](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.isengagedchanged.aspx). Esses exemplos pressupõem que você já atribuiu os manipuladores de eventos aos eventos em sua marcação HTML. Para obter mais informações sobre como fazer isso, consulte [Exemplo de propriedades HTML](html-properties-example.md).

Em JavaScript, os eventos **AdControl** devem ser encapsulados pela função [MarkSupportedForProcessing](http://msdn.microsoft.com/library/windows/apps/Hh967819.aspx). Para obter mais informações sobre como manipular eventos em JavaScript, consulte [Codificando aplicativos básicos (HTML)](https://msdn.microsoft.com/library/windows/apps/hh780660.aspx#adding-event-handlers).

## <a name="examples"></a>Exemplos

> [!div class="tabbedCodeSnippets"]
[!code-javascript[AdControl](./code/AdvertisingSamples/AdControlSamples/js/main.js#EventHandlers)]

## <a name="related-topics"></a>Tópicos relacionados

* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)
* [Tratamento de erros de AdControl](adcontrol-error-handling.md)
* [Classe RoutedEventArgs](http://msdn.microsoft.com/library/system.windows.routedeventargs.aspx)

 

 
