---
Description: Use animações pop-up para mostrar e ocultar a interface do usuário pop-up para submenus ou elementos de interface do usuário pop-up personalizados. Os elementos pop-up são contêineres que aparecem sobre o conteúdo do aplicativo e desaparecem quando o usuário toca ou clica fora do elemento pop-up.
title: Animações pop-up da interface de usuário
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 78adc32dccc66b98e413796d9cfe20b2a158261e
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970531"
---
# <a name="pop-up-ui-animations"></a>Animações pop-up da interface de usuário



Use animações pop-up para mostrar e ocultar a interface do usuário pop-up para submenus ou elementos de interface do usuário pop-up personalizados. Os elementos pop-up são contêineres que aparecem sobre o conteúdo do aplicativo e desaparecem quando o usuário toca ou clica fora do elemento pop-up.

> **APIs importantes**: [**classe PopInThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation), [**classe PopupThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)


## <a name="dos-and-donts"></a>O que fazer e o que não fazer


-   Use animações pop-up para mostrar ou ocultar elementos pop-up de interface do usuário personalizados que não fazem parte da página do aplicativo em si. Os controles comuns fornecidos pelo Windows já têm essas animações internas.
-   Não use animações pop-up para dicas de ferramentas ou caixas de diálogo.
-   Não use animações pop-up para mostrar ou ocultar a interface do usuário dentro do conteúdo principal do seu aplicativo. Só use animações pop-up para mostrar ou ocultar um contêiner pop-up que seja exibido sobre o conteúdo do aplicativo principal.

## <a name="related-articles"></a>Artigos relacionados

* [Visão geral de animações](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [Animando a interface do usuário pop-up](https://docs.microsoft.com/previous-versions/windows/apps/jj649433(v=win.10))
* [Guia de início rápido: animando sua interface do usuário usando animações da biblioteca](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**Classe PopInThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation)
* [**Classe PopOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopOutThemeAnimation)
* [**Classe PopupThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)

 

 




