---
description: As entradas e as propriedades de uma folha de estilos de mapa
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Referência da folha de estilos de mapa
ms.date: 03/19/2017
ms.topic: article
keywords: windows 10, uwp, mapas, folha de estilos de mapa
ms.localizationpriority: medium
ms.openlocfilehash: ec30fd5d63dfa522ef721d2d8a2e4950e6e8e854
ms.sourcegitcommit: c9edb164356c0843d8a9b19ab43707d486e392e1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85377927"
---
# <a name="map-style-sheet-reference"></a>Referência da folha de estilos de mapa

As tecnologias de mapeamento da Microsoft usam [folhas de estilo de mapa](https://docs.microsoft.com/BingMaps/styling/map-style-sheets) para definir a aparência de mapas, incluindo aqueles exibidos em um [MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol)de aplicativo da Windows Store.  Uma folha de estilos de mapa é definida usando JavaScript Object Notation (JSON) por meio do método [MapStyleSheet. ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_) .

Uma folha de estilos consiste em [entradas](https://docs.microsoft.com/BingMaps/styling/map-style-sheet-entries) cujas propriedades são substituídas para alterar a aparência final do mapa.

Por exemplo, o JSON a seguir pode ser usado para fazer com que as áreas de água apareçam em vermelho, os rótulos de água aparecem em verde e as áreas de aterrissagem aparecem em azul:

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

As folhas de estilo podem ser criadas interativamente usando o aplicativo [Editor de folha de estilo de mapa](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) .
