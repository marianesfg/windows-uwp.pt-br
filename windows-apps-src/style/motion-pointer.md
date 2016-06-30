---
author: mijacobs
Description: "Use animações de ponteiro para fornecer aos usuários feedback visual quando o usuário tocar em um item."
title: "Animações para clique de ponteiro em aplicativos UWP"
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
label: Motion--Pointer animations
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 24175bb7c822ceb53af5e1ec70bf83f701fd1cce

---

# Animações para clique de ponteiro

Use animações de ponteiro para fornecer aos usuários feedback visual quando o usuário tocar em um item. A animação de ponteiro para baixo encolhe e inclina um pouco o item pressionado e aparece quando um item é tocado pela primeira vez. A animação de ponteiro para cima, que restaura o item à sua posição original, aparece quando o usuário libera o ponteiro.




**APIs importantes**

-   [**Classe PointerUpThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969168)
-   [**Classe PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969164)



## O que fazer e o que não fazer


-   Ao usar uma animação de ponteiro para cima, dispare imediatamente a animação quando o usuário liberar o ponteiro. Isso faz com que o usuário tenha feedback instantâneo de que sua ação foi reconhecida, mesmo que a ação disparada pelo toque (por exemplo, navegar até uma nova página) esteja com uma resposta mais lenta.

## Artigos relacionados

**Para desenvolvedores (XAML)**
* [Visão geral de animações](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animando cliques de ponteiro](https://msdn.microsoft.com/library/windows/apps/xaml/jj649432)
* [Guia de início rápido: animando sua interface do usuário usando animações da biblioteca](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Classe PointerUpThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969168)
* [**Classe PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969164)

 

 







<!--HONumber=Jun16_HO4-->


