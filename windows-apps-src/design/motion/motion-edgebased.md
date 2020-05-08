---
Description: Animações de borda mostram ou ocultam a interface do usuário que tem origem na borda da tela.
title: Animações da interface de usuário baseadas em bordas
ms.assetid: 5A8F73B1-F4F6-424b-9EDF-A9766C5DEAE8
label: Motion--edge-based UI
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 02108ad2926fc1514ca94f08d11f565bc342a62d
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970311"
---
# <a name="edge-based-ui-animations"></a>Animações da interface de usuário baseadas em bordas





Animações de borda mostram ou ocultam a interface do usuário que tem origem na borda da tela. As ações de mostrar e ocultar podem ser iniciadas pelo usuário ou pelo aplicativo. A interface do usuário pode sobrepor o aplicativo ou fazer parte da superfície principal do aplicativo. Se a interface do usuário fizer parte da superfície do app, talvez seja necessário redimensionar o restante do app para acomodá-la.

> **APIs importantes**: [**classe EdgeUIThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EdgeUIThemeTransition)


## <a name="dos-and-donts"></a>O que fazer e o que não fazer


-   Use as animações de interface do usuário de borda para mostrar ou ocultar uma barra de erros ou de mensagens personalizada que não se estende muito pela tela.
-   Use as animações de painel para mostrar a interface do usuário que desliza em uma distância significativa na tela, por exemplo, um painel de tarefas ou um teclado virtual personalizado.
-   Deslize a interface do usuário para dentro da mesma borda em que ela será anexada.
-   Deslize a interface do usuário para fora da mesma borda da qual ela surgiu.
-   Se o conteúdo do aplicativo precisar ser redimensionado em resposta ao deslizamento da interface do usuário para dentro ou para fora, use as animações de fade para o redimensionamento.
    -   Se a interface do usuário estiver deslizando para dentro, use uma animação de fade depois da animação de painel ou de interface do usuário de borda.
    -   Se a interface do usuário estiver deslizando para fora, use uma animação de fade ao mesmo tempo em que a animação de painel ou de interface do usuário de borda.
-   Não aplique essas animações às notificações. As notificações não devem ser posicionadas na interface do usuário de borda.
-   Não aplique as animações de painel ou de interface do usuário de borda em qualquer controle ou contêiner de interface do usuário que não esteja na borda da tela. Essas animações são usadas apenas para mostrar, redimensionar e descartar a interface do usuário nas bordas da tela. Para mover outros tipos de interface do usuário, use as animações de reposicionamento.

    ![Ilustra quando usar as animações de painel ou de interface do usuário de borda e quando usar o reposicionamento.](images/edgevsreposition.png)

## <a name="related-articles"></a>Artigos relacionados


**Para desenvolvedores**
* [Visão geral de animações](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [Animando interface do usuário de borda](https://docs.microsoft.com/previous-versions/windows/apps/jj649428(v=win.10))
* [Guia de início rápido: animando sua interface do usuário usando animações da biblioteca](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**Classe EdgeUIThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EdgeUIThemeTransition)
* [**Classe PaneThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PaneThemeTransition)
* [Animando fades](https://docs.microsoft.com/previous-versions/windows/apps/jj649429(v=win.10))
* [Animando reposições](https://docs.microsoft.com/previous-versions/windows/apps/jj649434(v=win.10))

 

 




