---
author: payzer
title: "Como desativar a colocação em escala"
description: "Instruções para desativar o fator de escala padrão."
translationtype: Human Translation
ms.sourcegitcommit: 582f5677c15f7cd62c398103b48743ba4bea6c5b
ms.openlocfilehash: 8079be9685558277565766fa8d0ebbfd4a555904

---

# Como desativar a colocação em escala   
Por padrão, os aplicativos são dimensionados em 200% para XAML e em 150% para aplicativos HTML. É possível desativar o fator de escala padrão. Isso fará com que seu aplicativo use as dimensões em pixels reais do dispositivo (1910x1080 pixels).   
   
## HTML   
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

## XAML
Você pode recusar o fator de escala usando o seguinte trecho de código:   
   
```
bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```
   
## DirectX/C++   
Os aplicativos em DirectX/C++ não são dimensionados. O dimensionamento automático só se aplica a aplicativos HTML e XAML.  

## Consulte também
- [Práticas recomendadas para Xbox](tailoring-for-xbox.md)
- [UWP no Xbox One](index.md)



<!--HONumber=Aug16_HO3-->


