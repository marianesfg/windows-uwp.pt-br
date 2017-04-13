---
author: mcleanbyron
ms.assetid: d074e9d5-b3e0-4f16-b1e4-02b32ac99b2c
description: Saiba como atribuir propriedades **AdControl** a valores.
title: Exemplo de propriedades XAML AdControl
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, anúncios, publicidade, AdControl, XAML, propriedades"
ms.openlocfilehash: 3c5dae93ab6ee58fac7d4593257d357f268c241a
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="adcontrol-xaml-properties-example"></a>Exemplo de propriedades XAML AdControl

O exemplo XAML a seguir demonstra como atribuir propriedades [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) a valores. Se uma propriedade não estiver definida, o **AdControl** usará valores padrão para criar um anúncio consistente com a experiência do usuário do aplicativo.

Os valores são exemplos. Em seu código, você definirá os valores dessas funções e propriedades apropriados para seu aplicativo.

> [!div class="tabbedCodeSnippets"]
``` xml
<UI:AdControl Width="300",
    Height="250",
    AdUnitId="10865270",
    ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1",
    IsAutoRefreshEnabled="false",
    AdRefreshed="OnAdRefresh",
    ErrorOcurred="OnAdError",
    IsEngagedChanged="OnAdEngagedChanged" />
```

## <a name="related-topics"></a>Tópicos relacionados

* [AdControl em XAML e .NET](adcontrol-in-xaml-and--net.md)
* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)

 
