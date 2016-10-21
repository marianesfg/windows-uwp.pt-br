---
author: mijacobs
Description: "Use animações de arrastar e soltar quando os usuários moverem objetos, como ao mover um item dentro de uma lista ou ao soltar um item em cima de outro."
title: "Animações de arrastar em aplicativos UWP"
ms.assetid: 6064755F-6E24-4901-A4FF-263F05F0DFD6
label: Motion--Drag and drop
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 58934982554aa3caf0060c84df113d483e62e1d2

---

# Animações da operação arrastar




Use animações de arrastar e soltar quando os usuários moverem objetos, como ao mover um item dentro de uma lista ou ao soltar um item em cima de outro.

**APIs importantes**

-   [**Classe DragItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br243174)


## O que fazer e o que não fazer


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


## Artigos relacionados

**Para desenvolvedores (XAML)**
* [Visão geral de animações](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animando sequências de arrastar e soltar](https://msdn.microsoft.com/library/windows/apps/xaml/jj649427)
* [Guia de início rápido: animando sua interface do usuário usando animações da biblioteca](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Classe DragItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br243174)
* [**Classe DropTargetItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br243186)
* [**Classe DragOverThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br243180)


 







<!--HONumber=Aug16_HO3-->


