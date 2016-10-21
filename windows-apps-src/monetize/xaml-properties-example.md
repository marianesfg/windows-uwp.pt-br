---
author: mcleanbyron
ms.assetid: d074e9d5-b3e0-4f16-b1e4-02b32ac99b2c
description: Saiba como atribuir propriedades **AdControl** a valores.
title: Exemplo de propriedades XAML
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: fb0533aa0ea760bca686276f886f0afcb21bf6f7


---

# Exemplo de propriedades XAML




O exemplo XAML a seguir demonstra como atribuir propriedades [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) a valores. Se uma propriedade não estiver definida, o **AdControl** usará valores padrão para criar um anúncio consistente com a experiência do usuário do aplicativo.

Os valores são exemplos. Em seu código, você definirá os valores dessas funções e propriedades apropriados para seu aplicativo.

``` syntax
Width="300",
Height="250",
AdUnitId="10865270",
ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1",
IsAutoRefreshEnabled="false",
AdRefreshed="OnAdRefresh",
ErrorOcurred="OnAdError",
IsEngagedChanged="OnAdEngagedChanged"
```

## Tópicos relacionados

* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)

 



<!--HONumber=Aug16_HO3-->


