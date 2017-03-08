---
author: payzer
title: Como desenhar a interface para a borda da tela
description: 
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 1adb221f-6f70-4255-9329-2046a486ca45
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 9a221672391dfbfb4af664438448307800020c6f
ms.lasthandoff: 02/08/2017

---

# <a name="how-to-draw-ui-to-the-edge-of-the-screen"></a>Como desenhar a interface para a borda da tela   
Por padrão, os aplicativos terão bordas colocadas nas bordas do visor para levar em conta a área de segurança da TV (para obter mais informações, consulte [Projetando para Xbox e TV](../input-and-devices/designing-for-tv.md#tv-safe-area)). 

Recomendamos desativar esse recurso e desenhar para a borda da tela. Você pode desenhar para a borda da tela, adicionando o código a seguir quando iniciar seu aplicativo:
   
```
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```
   
> [!NOTE]
> Aplicativos C++/DirectX não precisam se preocupar com isso. O sistema sempre renderizará seu aplicativo para a borda da tela.

## <a name="see-also"></a>Consulte também
- [Práticas recomendadas para Xbox](tailoring-for-xbox.md)
- [UWP no Xbox One](index.md)

