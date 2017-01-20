---
author: mijacobs
Description: "Use animações pop-up para mostrar e ocultar a interface do usuário pop-up para submenus ou elementos de interface do usuário pop-up personalizados. Os elementos pop-up são contêineres que aparecem sobre o conteúdo do aplicativo e desaparecem quando o usuário toca ou clica fora do elemento pop-up."
title: "Animações de interface do usuário pop-up em aplicativos UWP"
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: cb5d70784e758b4e18092b75df9e0d77243af7fc

---

# <a name="pop-up-ui-animations"></a>Animações de interface pop-up

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Use animações pop-up para mostrar e ocultar a interface do usuário pop-up para submenus ou elementos de interface do usuário pop-up personalizados. Os elementos pop-up são contêineres que aparecem sobre o conteúdo do aplicativo e desaparecem quando o usuário toca ou clica fora do elemento pop-up.

<div class="important-apis" >
<b>APIs importantes</b><br/>
<ul>
<li>[**Classe PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/br210383)</li>
<li>[**Classe PopupThemeTransition**](https://msdn.microsoft.com/library/windows/apps/hh969172)</li>
</ul>
</div>


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

 

 







<!--HONumber=Dec16_HO2-->


