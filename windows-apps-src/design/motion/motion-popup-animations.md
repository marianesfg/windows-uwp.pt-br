---
Description: Use animações pop-up para mostrar e ocultar a interface do usuário pop-up para submenus ou elementos de interface do usuário pop-up personalizados. Os elementos pop-up são contêineres que aparecem sobre o conteúdo do aplicativo e desaparecem quando o usuário toca ou clica fora do elemento pop-up.
title: Animações de interface do usuário pop-up em aplicativos UWP
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ee3d6a7fc29ec2adfeb149a3bc84f27c482c3be7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366706"
---
# <a name="pop-up-ui-animations"></a>Animações de interface pop-up



Use animações pop-up para mostrar e ocultar a interface do usuário pop-up para submenus ou elementos de interface do usuário pop-up personalizados. Os elementos pop-up são contêineres que aparecem sobre o conteúdo do aplicativo e desaparecem quando o usuário toca ou clica fora do elemento pop-up.

> **APIs importantes**: [**Classe PopInThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation), [ **PopupThemeTransition classe**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)


## <a name="dos-and-donts"></a>O que fazer e o que não fazer


-   Use animações pop-up para mostrar ou ocultar elementos pop-up de interface do usuário personalizados que não fazem parte da página do aplicativo em si. Os controles comuns fornecidos pelo Windows já têm essas animações internas.
-   Não use animações pop-up para dicas de ferramentas ou caixas de diálogo.
-   Não use animações pop-up para mostrar ou ocultar a interface do usuário dentro do conteúdo principal do seu aplicativo. Só use animações pop-up para mostrar ou ocultar um contêiner pop-up que seja exibido sobre o conteúdo do aplicativo principal.

## <a name="related-articles"></a>Artigos relacionados

* [Visão geral de animações](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [Animar um pop-up da interface do usuário](https://docs.microsoft.com/previous-versions/windows/apps/jj649433(v=win.10))
* [Guia de início rápido: Animando sua interface do usuário usando animações de biblioteca](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**Classe PopInThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation)
* [**Classe PopOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopOutThemeAnimation)
* [**Classe PopupThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)

 

 




