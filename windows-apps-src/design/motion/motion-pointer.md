---
Description: Use animações de ponteiro para fornecer aos usuários feedback visual quando o usuário tocar em um item.
title: Animações para clique de ponteiro em aplicativos UWP
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ee87a62796ed51d09d44cabecd0b49873145bd90
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366081"
---
# <a name="pointer-click-animations"></a>Animações para clique de ponteiro



Use animações de ponteiro para fornecer aos usuários feedback visual quando o usuário tocar em um item. A animação de ponteiro para baixo encolhe e inclina um pouco o item pressionado e aparece quando um item é tocado pela primeira vez. A animação de ponteiro para cima, que restaura o item à sua posição original, aparece quando o usuário libera o ponteiro.


> **APIs importantes**: [**Classe PointerUpThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation), [ **PointerDownThemeAnimation classe**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)


## <a name="dos-and-donts"></a>O que fazer e o que não fazer

-   Ao usar uma animação de ponteiro para cima, dispare imediatamente a animação quando o usuário liberar o ponteiro. Isso faz com que o usuário tenha feedback instantâneo de que sua ação foi reconhecida, mesmo que a ação disparada pelo toque (por exemplo, navegar até uma nova página) esteja com uma resposta mais lenta.

## <a name="related-articles"></a>Artigos relacionados

* [Visão geral de animações](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [Animando cliques de ponteiro](https://docs.microsoft.com/previous-versions/windows/apps/jj649432(v=win.10))
* [Guia de início rápido: Animando sua interface do usuário usando animações de biblioteca](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**Classe PointerUpThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation)
* [**Classe PointerDownThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)

 

 




