---
author: payzer
title: Como desativar a colocação em escala
description: Instruções para desativar o fator de escala padrão.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 6e68c1fc-a407-4c0b-b0f4-e445ccb72ff3
ms.localizationpriority: medium
ms.openlocfilehash: 82b42b25d3894a82e92af9a520ee5f951a5ba344
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5947937"
---
# <a name="how-to-turn-off-scaling"></a>Como desativar a colocação em escala   
Por padrão, os aplicativos são dimensionados em 200% para XAML e em 150% para aplicativos HTML. É possível desativar o fator de escala padrão. Isso fará com que seu aplicativo use as dimensões em pixels reais do dispositivo (1910x1080 pixels).   
   
## <a name="html"></a>HTML   
Você pode recusar o fator de escala usando o seguinte trecho de código: 
   
```
var result = Windows.UI.ViewManagement.ApplicationViewScaling.trySetDisableLayoutScaling(true);
```

Ou, você pode usar um método próprio para a Web:   

```   
@media (max-height: 1080px) {   
    @-ms-viewport {   
        height: 1080px;   
    }   
}   
```

## <a name="xaml"></a>XAML
Você pode recusar o fator de escala usando o seguinte trecho de código:   
   
```
bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```
   
## <a name="directxc"></a>DirectX/C++   
Os aplicativos em DirectX/C++ não são dimensionados. O dimensionamento automático só se aplica a aplicativos HTML e XAML.  

## <a name="see-also"></a>Consulte também
- [Práticas recomendadas para Xbox](tailoring-for-xbox.md)
- [UWP no Xbox One](index.md)
