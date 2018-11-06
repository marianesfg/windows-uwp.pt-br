---
author: mijacobs
Description: Use pop-up animations to show and hide pop-up UI for flyouts or custom pop-up UI elements. Pop-up elements are containers that appear over the app's content and are dismissed if the user taps or clicks outside of the pop-up element.
title: Animações pop-up de interface do usuário em aplicativos UWP
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1a304df30986c904f19cc2401c9a1fb468514f6f
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6047481"
---
# <a name="pop-up-ui-animations"></a>Animações pop-up da interface de usuário



Use animações pop-up para mostrar e ocultar a interface do usuário pop-up para submenus ou elementos de interface do usuário pop-up personalizados. Os elementos pop-up são contêineres que aparecem sobre o conteúdo do aplicativo e desaparecem quando o usuário toca ou clica fora do elemento pop-up.

> **APIs importantes**: [**classe PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210383), [**classe PopupThemeTransition**](https://msdn.microsoft.com/library/windows/apps/hh969172)


## <a name="dos-and-donts"></a>O que fazer e o que não fazer


-   Use animações pop-up para mostrar ou ocultar elementos pop-up de interface do usuário personalizados que não fazem parte da página do aplicativo em si. Os controles comuns fornecidos pelo Windows já têm essas animações internas.
-   Não use animações pop-up para dicas de ferramentas ou caixas de diálogo.
-   Não use animações pop-up para mostrar ou ocultar a interface do usuário dentro do conteúdo principal do seu aplicativo. Só use animações pop-up para mostrar ou ocultar um contêiner pop-up que seja exibido sobre o conteúdo do aplicativo principal.

## <a name="related-articles"></a>Artigos relacionados

* [Visão geral de animações](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animando a interface do usuário pop-up](https://msdn.microsoft.com/library/windows/apps/xaml/jj649433)
* [Guia de início rápido: animando sua interface do usuário usando animações da biblioteca](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Classe PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210383)
* [**Classe PopOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210391)
* [**Classe PopupThemeTransition**](https://msdn.microsoft.com/library/windows/apps/hh969172)

 

 




