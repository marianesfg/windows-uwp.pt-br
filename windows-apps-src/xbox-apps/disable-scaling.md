---
author: payzer
title: "Como desativar a colocação em escala"
description: 
area: Xbox
ms.sourcegitcommit: 2fcccb9a045aad268afde615d31f8faa002b8a87
ms.openlocfilehash: 65416dd2b6c8656078b63c316f3972cda9c792fc

---

# Como desativar a colocação em escala   
Por padrão, os aplicativos são dimensionados em 200% para XAML e em 150% para aplicativos HTML. É possível desativar o fator de escala padrão. Isso fará com que seu aplicativo use as dimensões em pixels reais do dispositivo (1910 por 1080 pixels).   
   
## HTML   
Você pode recusar o fator de escala usando o seguinte trecho de código: 
   
`bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);` 

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
   
`bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);`   
   
## DirectX/C++   
Os aplicativos em DirectX/C++ não são dimensionados. O dimensionamento automático só se aplica a aplicativos HTML e XAML.   



<!--HONumber=Jun16_HO5-->


