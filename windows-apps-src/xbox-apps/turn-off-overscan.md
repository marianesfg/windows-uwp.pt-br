---
author: payzer
title: Como desativar overscan
description: 
area: Xbox
ms.sourcegitcommit: 32a875348debac9aec9f5a26bc4e7e0af2a0a5b4
ms.openlocfilehash: abd06e78364ff32cc10d733e33b153b854dbc467

---

# Como desenhar a interface para a borda da tela   
Por padrão, os aplicativos terão bordas colocadas nas bordas do visor. Isso é para levar em conta a área de TV-safe. Para obter mais informações, consulte [Projetando para Xbox e TV](http://go.microsoft.com/fwlink/?LinkID=760736#tv-safe-area).  Recomendamos desativar esse recurso e desenhar para a borda da tela. Você pode desenhar para a borda da tela, adicionando o código a seguir quando iniciar seu aplicativo:
   
`Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);`
   
Observação: aplicativos C++/DirectX não precisam se preocupar com isso. O sistema sempre renderizará seu aplicativo para a borda da tela.



<!--HONumber=Jun16_HO5-->


