---
author: mijacobs
Description: "Animações de borda mostram ou ocultam a interface do usuário que tem origem na borda da tela."
title: "Animações de interface de usuário baseadas em borda em aplicativos UWP"
ms.assetid: 5A8F73B1-F4F6-424b-9EDF-A9766C5DEAE8
label: Motion--edge-based UI
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 1be51a8ff4a63f32834c7eb04b70d17dc41de13b

---

# Animações de interface de usuário baseadas em borda




Animações de borda mostram ou ocultam a interface do usuário que tem origem na borda da tela. As ações de mostrar e ocultar podem ser iniciadas pelo usuário ou pelo aplicativo. A interface do usuário pode sobrepor o aplicativo ou fazer parte da superfície principal do aplicativo. Se a interface do usuário fizer parte da superfície do aplicativo, talvez seja necessário redimensionar o restante do aplicativo para acomodá-la.

**APIs importantes**

-   [**Classe EdgeUIThemeTransition**](https://msdn.microsoft.com/library/windows/apps/hh702324)


## O que fazer e o que não fazer


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

## Artigos relacionados


**Para desenvolvedores (XAML)**
* [Visão geral de animações](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animando interface do usuário de borda](https://msdn.microsoft.com/library/windows/apps/xaml/jj649428)
* [Guia de início rápido: animando sua interface do usuário usando animações da biblioteca](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Classe EdgeUIThemeTransition**](https://msdn.microsoft.com/library/windows/apps/hh702324)
* [**Classe PaneThemeTransition**](https://msdn.microsoft.com/library/windows/apps/hh969160)
* [Animando fades](https://msdn.microsoft.com/library/windows/apps/xaml/jj649429)
* [Animando reposições](https://msdn.microsoft.com/library/windows/apps/xaml/jj649434)

 

 







<!--HONumber=Jun16_HO4-->


