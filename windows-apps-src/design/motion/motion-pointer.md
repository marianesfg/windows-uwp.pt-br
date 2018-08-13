---
author: mijacobs
Description: Use pointer animations to provide users with visual feedback when the user taps on an item.
title: Animações para clique de ponteiro em aplicativos UWP
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
ms.author: jimwalk
ms.date: 08/9/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 479da70c48fd28d6f877917f6a8cc6e411cf659b
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1652575"
---
# <a name="pointer-click-animations"></a>Animações para clique de ponteiro



Use animações de ponteiro para fornecer aos usuários feedback visual quando o usuário tocar em um item. A animação de ponteiro para baixo encolhe e inclina um pouco o item pressionado e aparece quando um item é tocado pela primeira vez. A animação de ponteiro para cima, que restaura o item à sua posição original, aparece quando o usuário libera o ponteiro.


> **APIs importantes**: [**classe PointerUpThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969168), [**classe PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969164)


## <a name="dos-and-donts"></a>O que fazer e o que não fazer

-   Ao usar uma animação de ponteiro para cima, dispare imediatamente a animação quando o usuário liberar o ponteiro. Isso faz com que o usuário tenha feedback instantâneo de que sua ação foi reconhecida, mesmo que a ação disparada pelo toque (por exemplo, navegar até uma nova página) esteja com uma resposta mais lenta.

## <a name="related-articles"></a>Artigos relacionados

* [Visão geral de animações](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animando cliques de ponteiro](https://msdn.microsoft.com/library/windows/apps/xaml/jj649432)
* [Guia de início rápido: animando sua interface do usuário usando animações da biblioteca](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Classe PointerUpThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969168)
* [**Classe PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969164)

 

 



