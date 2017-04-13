---
author: mcleanbyron
ms.assetid: 2fba38c4-11be-4058-bfa3-5f979390791c
description: Saiba como manipular os eventos da classe AdControl.
title: Eventos AdControl em C#
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, anúncios, publicidade, AdControl, eventos"
ms.openlocfilehash: a0b1130f8177166eef5b524ac4c4f1b065176eeb
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="adcontrol-events-in-c"></a>Eventos AdControl em C\# #  


Os exemplos a seguir demonstram manipuladores de eventos básicos para os seguintes eventos [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx): [ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.erroroccurred.aspx), [AdRefreshed](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.adrefreshed.aspx) e [IsEngagedChanged](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.isengagedchanged.aspx). Esses exemplos pressupõem que você já atribuiu os manipuladores de eventos aos eventos em seu código XAML. Para obter mais informações sobre como fazer isso, consulte [Exemplo de propriedades XAML](xaml-properties-example.md).

Para obter mais informações sobre como manipular eventos em C#, consulte [Visão geral de eventos e eventos roteados (aplicativos universais do Windows em C#/VB/C++ e XAML)](http://msdn.microsoft.com/library/windows/apps/hh758286).

## <a name="examples"></a>Exemplos

> [!div class="tabbedCodeSnippets"]
[!code-cs[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MainPage.xaml.cs#EventHandlers)]

## <a name="related-topics"></a>Tópicos relacionados

* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)
* [Tratamento de erros de AdControl](adcontrol-error-handling.md)
* [Classe RoutedEventArgs](http://msdn.microsoft.com/library/system.windows.routedeventargs.aspx)

 

 
