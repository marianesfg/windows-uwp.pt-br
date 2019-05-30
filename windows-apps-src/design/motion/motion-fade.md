---
Description: Use animações de fade para trazer itens para uma exibição ou retirá-los de uma exibição. As duas animações de fade comuns são fade in e fade out.
title: Animações de fade em aplicativos UWP
ms.assetid: 975E5EE3-EFBE-4159-8D10-3C94143DD07F
label: Motion--fades
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3d8642e911a3ad4275e0a7a0f147ca9d70f415b0
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366793"
---
# <a name="fade-animations"></a>Animações de fade



Use animações de fade para trazer itens para uma exibição ou retirá-los de uma exibição. As duas animações de fade comuns são fade in e fade out.

> **APIs importantes**: [**Classe FadeInThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation), [ **FadeOutThemeAnimation classe**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation)


## <a name="dos-and-donts"></a>O que fazer e o que não fazer


-   Quando ocorrerem transições entre elementos não relacionados ou com uso intenso de texto no aplicativo, use uma ação de fade out seguida de uma ação de fade in. Isso permite que o objeto de saída desapareça completamente antes que o objeto de entrada fique visível.
-   Faça fade in do(s) elemento(s) de entrada sobre os elementos de saída se o tamanho dos elementos permanecer constante e se você quiser que o usuário perceba que está olhando para o mesmo item. Assim que o fade in estiver concluído, o item de saída poderá ser removido. Essa será uma opção viável somente quando o item de saída estiver completamente coberto pelo item de entrada.
-   Evite animações de fade para adicionar ou excluir itens em uma lista. Em vez disso, use as animações de lista criadas especificamente para essa finalidade.
-   Evite animações de fade para mudar todo o conteúdo de uma página. Em vez disso, use as animações de transição de página criadas para essa finalidade.
-   O fade out é uma forma sutil de remover um elemento.
## <a name="related-articles"></a>Artigos relacionados

* [Visão geral de animações](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [Animando esmaece](https://docs.microsoft.com/previous-versions/windows/apps/jj649429(v=win.10))
* [Guia de início rápido: Animando sua interface do usuário usando animações de biblioteca](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**Classe FadeInThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation)
* [**Classe FadeOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation)

 

 




