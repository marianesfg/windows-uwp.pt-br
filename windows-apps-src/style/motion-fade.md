---
author: mijacobs
Description: "Use animações de fade para trazer itens para uma exibição ou retirá-los de uma exibição. As duas animações de fade comuns são fade in e fade out."
title: "Animações de fade em aplicativos UWP"
ms.assetid: 975E5EE3-EFBE-4159-8D10-3C94143DD07F
label: Motion--fades
template: detail.hbs
ms.openlocfilehash: cbe1a4ca77f0d6e15b3d12b2b3a0f2f8363530ae
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="fade-animations"></a>Animações de fade

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Use animações de fade para trazer itens para uma exibição ou retirá-los de uma exibição. As duas animações de fade comuns são fade in e fade out.

<div class="important-apis" >
<b>APIs importantes</b><br/>
<ul>
<li>[**Classe FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210298)</li>
<li>[**Classe FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210302)</li>
</ul>
</div>


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

 

 




