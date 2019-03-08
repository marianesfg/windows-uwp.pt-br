---
title: Como desativar a colocação em escala
description: Instruções para desativar o fator de escala padrão.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 6e68c1fc-a407-4c0b-b0f4-e445ccb72ff3
ms.localizationpriority: medium
ms.openlocfilehash: 44688ff40792ba2ee72cbd1d96bae1ac59834efa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604741"
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
