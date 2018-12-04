---
title: Como desenhar a interface para a borda da tela
description: Como desativar o dimensionamento automático para área de segurança do título.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 1adb221f-6f70-4255-9329-2046a486ca45
ms.localizationpriority: medium
ms.openlocfilehash: 1ac49d80f1d99a56eff565a0daa8f2f3e9289636
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8477318"
---
# <a name="how-to-draw-ui-to-the-edge-of-the-screen"></a>Como desenhar a interface para a borda da tela   
Por padrão, os aplicativos terão bordas colocadas nas bordas do visor para levar em conta a área de segurança da TV (para obter mais informações, consulte [Projetando para Xbox e TV](../design/devices/designing-for-tv.md#tv-safe-area)). 

Recomendamos desativar esse recurso e desenhar para a borda da tela. Você pode desenhar para a borda da tela, adicionando o código a seguir quando iniciar seu aplicativo:
   
```
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```
   
> [!NOTE]
> Aplicativos C++/DirectX não precisam se preocupar com isso. O sistema sempre renderizará seu aplicativo para a borda da tela.

## <a name="see-also"></a>Consulte também
- [Práticas recomendadas para Xbox](tailoring-for-xbox.md)
- [UWP no Xbox One](index.md)
