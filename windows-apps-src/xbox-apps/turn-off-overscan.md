---
author: payzer
title: Como desenhar a interface para a borda da tela
description: 
translationtype: Human Translation
ms.sourcegitcommit: b5961d3266a031ab09a9da63319e9883cf050789
ms.openlocfilehash: cddde27a17e897ab8a68bbed099e532a8cd48f07

---

# Como desenhar a interface para a borda da tela   
Por padrão, os aplicativos terão bordas colocadas nas bordas do visor para levar em conta a área de segurança da TV (para obter mais informações, consulte [Projetando para Xbox e TV](../input-and-devices/designing-for-tv.md#tv-safe-area)). 

Recomendamos desativar esse recurso e desenhar para a borda da tela. Você pode desenhar para a borda da tela, adicionando o código a seguir quando iniciar seu aplicativo:
   
```
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```
   
> [!NOTE]
> Aplicativos C++/DirectX não precisam se preocupar com isso. O sistema sempre renderizará seu aplicativo para a borda da tela.

## Consulte também
- [Práticas recomendadas para Xbox](tailoring-for-xbox.md)
- [UWP no Xbox One](index.md)



<!--HONumber=Aug16_HO3-->


