---
author: mijacobs
Description: Use animações de fade para trazer itens para uma exibição ou retirá-los de uma exibição. As duas animações de fade comuns são fade in e fade out.
title: Animações de fade em aplicativos UWP
ms.assetid: 975E5EE3-EFBE-4159-8D10-3C94143DD07F
label: Motion--fades
template: detail.hbs
---

# Animações de fade

Use animações de fade para trazer itens para uma exibição ou retirá-los de uma exibição. As duas animações de fade comuns são fade in e fade out.

**APIs importantes**

-   [**Classe FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210298)
-   [**Classe FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210302)


## O que fazer e o que não fazer


-   Quando ocorrerem transições entre elementos não relacionados ou com uso intenso de texto no aplicativo, use uma ação de fade out seguida de uma ação de fade in. Isso permite que o objeto de saída desapareça completamente antes que o objeto de entrada fique visível.
-   Faça fade in do(s) elemento(s) de entrada sobre os elementos de saída se o tamanho dos elementos permanecer constante e se você quiser que o usuário perceba que está olhando para o mesmo item. Assim que o fade in estiver concluído, o item de saída poderá ser removido. Essa será uma opção viável somente quando o item de saída estiver completamente coberto pelo item de entrada.
-   Evite animações de fade para adicionar ou excluir itens em uma lista. Em vez disso, use as animações de lista criadas especificamente para essa finalidade.
-   Evite animações de fade para mudar todo o conteúdo de uma página. Em vez disso, use as animações de transição de página criadas para essa finalidade.
-   O fade out é uma forma sutil de remover um elemento.
## Artigos relacionados

**Para desenvolvedores (XAML)**
* [Visão geral de animações](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animando fades](https://msdn.microsoft.com/library/windows/apps/xaml/jj649429)
* [Guia de início rápido: animando sua interface do usuário usando animações da biblioteca](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Classe FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210298)
* [**Classe FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210302)

 

 






<!--HONumber=May16_HO2-->


