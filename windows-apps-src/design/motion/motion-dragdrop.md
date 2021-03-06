---
Description: Use animações de arrastar e soltar quando os usuários moverem objetos, como ao mover um item em uma lista ou ao soltar um item em cima de outro.
title: Animações de arrasto
ms.assetid: 6064755F-6E24-4901-A4FF-263F05F0DFD6
label: Motion--Drag and drop
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 503360bc72694881c69bc3afd796f85474da5cfc
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970291"
---
# <a name="drag-animations"></a>Animações de arrasto




Use animações de arrastar e soltar quando os usuários moverem objetos, como ao mover um item em uma lista ou ao soltar um item em cima de outro.

> **APIs importantes**: [**classe DragItemThemeAnimation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.dragitemthemeanimation)


## <a name="dos-and-donts"></a>O que fazer e o que não fazer


**Animação de início de arraste**

-   Use a animação de início de arraste quando o usuário começa a mover um objeto.
-   Inclua os objetos afetados na animação se, e somente se, houver outros objetos que possam ser afetados pela operação de arrastar e soltar.
-   Use a animação de término de arraste para concluir qualquer sequência de animação que começou com a animação de início de arraste. Isso reverte a alteração de tamanho no objeto arrastado que foi causada pela animação de início de arraste.

**Animação de término de arraste**

-   Use a animação de término de arraste quando o usuário solta um objeto arrastado.
-   Use a animação de término de arraste em combinação com animações de adição e exclusão para listas.
-   Inclua os objetos afetados na animação de término de arraste se, e somente se, você incluiu esses mesmos objetos afetados na animação de início de arraste.
-   Não use a animação de término de arraste se você não usou primeiro a animação de início de arraste. Ambas as animações precisam ser usadas para que os objetos retornem aos tamanhos originais depois que a sequência de arrastar e soltar for concluída.

**Animação de arraste intermediário na entrada**

-   Use a animação de arraste intermediário na entrada quando o usuário arrasta a origem de arraste para uma área de soltar em que ela pode ser solta entre dois outros objetos.
-   Escolha uma área de destino razoável para soltar. Essa área não deve ser muito pequena a ponto de dificultar que o usuário posicione a origem de arraste para a ação de soltar.
-   A direção recomendada para mover os objetos afetados de forma a mostrar a área de soltar é uns diretamente separados dos outros. Se o movimento será na vertical ou na horizontal depende da orientação dos objetos afetados entre si.
-   Não use a animação de arraste intermediário na entrada se não for possível soltar a origem de arraste em uma área. A animação de arraste intermediário na entrada informa ao usuário que a origem de arraste pode ser solta entre os objetos afetados.

**Animação de arraste intermediário na saída**

-   Use a animação de arraste intermediário na saída quando o usuário arrasta um objeto para fora de uma área em que ele poderia ter sido solto entre dois outros objetos.
-   Não use a animação de arraste intermediário na saída se você não usou primeiro a animação de arraste intermediário na entrada.


## <a name="related-articles"></a>Artigos relacionados

**Para desenvolvedores**
* [Visão geral de animações](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [Animando sequências de arrastar e soltar](https://docs.microsoft.com/previous-versions/windows/apps/jj649427(v=win.10))
* [Guia de início rápido: animando sua interface do usuário usando animações da biblioteca](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**Classe DragItemThemeAnimation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.dragitemthemeanimation)
* [**Classe DropTargetItemThemeAnimation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.droptargetitemthemeanimation)
* [**Classe DragOverThemeAnimation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.dragoverthemeanimation)


 




