---
author: mijacobs
Description: "Use animações de ponteiro para fornecer aos usuários feedback visual quando o usuário tocar em um item."
title: "Animações para clique de ponteiro em aplicativos UWP"
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
label: Motion--Pointer animations
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: c208b67829e2053302ec0cc7c6da013b89cee8b3

---

# <a name="pointer-click-animations"></a>Animações para clique de ponteiro

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Use animações de ponteiro para fornecer aos usuários feedback visual quando o usuário tocar em um item. A animação de ponteiro para baixo encolhe e inclina um pouco o item pressionado e aparece quando um item é tocado pela primeira vez. A animação de ponteiro para cima, que restaura o item à sua posição original, aparece quando o usuário libera o ponteiro.


<div class="important-apis" >
<b>APIs importantes</b><br/>
<ul>
<li>[**Classe PointerUpThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969168)</li>
<li>[**Classe PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969164)</li>
</ul>
</div>


## <a name="dos-and-donts"></a>O que fazer e o que não fazer

-   Ao usar uma animação de ponteiro para cima, dispare imediatamente a animação quando o usuário liberar o ponteiro. Isso faz com que o usuário tenha feedback instantâneo de que sua ação foi reconhecida, mesmo que a ação disparada pelo toque (por exemplo, navegar até uma nova página) esteja com uma resposta mais lenta.

## <a name="related-articles"></a>Artigos relacionados

* [Visão geral de animações](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animando cliques de ponteiro](https://msdn.microsoft.com/library/windows/apps/xaml/jj649432)
* [Guia de início rápido: animando sua interface do usuário usando animações da biblioteca](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Classe PointerUpThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969168)
* [**Classe PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969164)

 

 







<!--HONumber=Dec16_HO2-->


