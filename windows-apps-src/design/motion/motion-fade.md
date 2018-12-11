---
Description: Use fade animations to bring items into a view or to take items out of a view. The two common fade animations are fade-in and fade-out.
title: Animações de esmaecimento em aplicativos UWP
ms.assetid: 975E5EE3-EFBE-4159-8D10-3C94143DD07F
label: Motion--fades
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6d3fee78f3608466f588a79d2811f1464e27a0ab
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8872347"
---
# <a name="fade-animations"></a>Animações de fade



Use animações de fade para trazer itens para uma exibição ou retirá-los de uma exibição. As duas animações comuns de esmaecimento são fade-in e fade-out.

> **APIs importantes**: [**classe FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210298), [**classe FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210302)


## <a name="dos-and-donts"></a>O que fazer e o que não fazer


-   Quando ocorrerem transições entre elementos não relacionados ou com uso intenso de texto no aplicativo, use uma ação de fade out seguida de uma ação de fade in. Isso permite que o objeto de saída desapareça completamente antes que o objeto de entrada fique visível.
-   Faça fade in do(s) elemento(s) de entrada sobre os elementos de saída se o tamanho dos elementos permanecer constante e se você quiser que o usuário perceba que está olhando para o mesmo item. Assim que o fade in estiver concluído, o item de saída poderá ser removido. Essa será uma opção viável somente quando o item de saída estiver completamente coberto pelo item de entrada.
-   Evite animações de fade para adicionar ou excluir itens em uma lista. Em vez disso, use as animações de lista criadas especificamente para essa finalidade.
-   Evite animações de fade para mudar todo o conteúdo de uma página. Em vez disso, use as animações de transição de página criadas para essa finalidade.
-   O fade out é uma forma sutil de remover um elemento.
## <a name="related-articles"></a>Artigos relacionados

* [Visão geral de animações](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animando fades](https://msdn.microsoft.com/library/windows/apps/xaml/jj649429)
* [Guia de início rápido: animando sua interface do usuário usando animações da biblioteca](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Classe FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210298)
* [**Classe FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210302)

 

 




