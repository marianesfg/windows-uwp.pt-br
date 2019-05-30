---
Description: Animações de transição de conteúdo permitem que você mude o conteúdo de uma área da tela enquanto mantém constante o contêiner ou o plano de fundo. O novo conteúdo aparece. Se houver conteúdo existente para ser substituído, esse conteúdo desaparecerá.
title: Diretrizes para animações de transição de conteúdo
ms.assetid: 0188FDB4-E183-466f-8A03-EE3FF5C474B1
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 5ec8778f590ba9b50c67209eaf4b80e2cbed2f16
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66364808"
---
# <a name="content-transition-animations"></a>Animações de transição de conteúdo



Animações de transição de conteúdo permitem que você mude o conteúdo de uma área da tela enquanto mantém constante o contêiner ou o plano de fundo. O novo conteúdo aparece. Se houver conteúdo existente para ser substituído, esse conteúdo desaparecerá.

> **APIs importantes**: [**Classe ContentThemeTransition (XAML)** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.contentthemetransition.)

## <a name="dos-and-donts"></a>O que fazer e o que não fazer


-   Use uma animação de entrada quando houver um conjunto de novos itens a serem inseridos em um contêiner vazio. Por exemplo, após o carregamento inicial de um aplicativo, parte do conteúdo do aplicativo pode não estar disponível imediatamente para exibição. Quando esse conteúdo estiver pronto para ser exibido, use uma animação de transição de conteúdo para inserir esse conteúdo atrasado na exibição.
-   Use as transições de conteúdo para substituir um conjunto de conteúdo por outro que já está no mesmo contêiner em uma exibição.
-   Ao inserir novo conteúdo, deslize esse conteúdo (de baixo para cima) até a exibição contra o fluxo geral da página ou a ordem de leitura.
-   Introduza novo conteúdo de maneira lógica, por exemplo, introduza o conteúdo mais importante por último.
-   Se houver mais de um contêiner cujo conteúdo será atualizado, dispare todas as animações de transição simultaneamente sem escalonamento ou atraso.
-   Não use animações de transição de conteúdo quando toda a página está mudando. Nesse caso, use as animações de transição de página.
-   Não use animações de transição de conteúdo se o conteúdo só estiver sendo atualizado. Animações de transição de conteúdo devem mostrar movimento. Para atualizações, use animações de fade.



## <a name="related-articles"></a>Artigos relacionados

**Para desenvolvedores (XAML)**
* [Visão geral de animações](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [Animando as transições de conteúdo](https://docs.microsoft.com/previous-versions/windows/apps/jj649426(v=win.10))
* [Guia de início rápido: Animando sua interface do usuário usando animações de biblioteca](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**Classe ContentThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.contentthemetransition.)

 

 




