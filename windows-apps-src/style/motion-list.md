---
author: mijacobs
Description: "Animações de lista permitem inserir ou remover um ou vários itens de uma coleção, como um álbum de fotos ou uma lista de resultados de pesquisa."
title: "Adicionar e excluir animações em aplicativos UWP"
ms.assetid: A85006AE-4992-457a-B514-500B8BEF5DC8
label: Motion--add and delete animations
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 40e4ca48132914140fcd4780876615a5864c3157
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2017
---
# <a name="add-and-delete-animations"></a>Adicionar e excluir animações

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Animações de lista permitem inserir ou remover um ou vários itens de uma coleção, como um álbum de fotos ou uma lista de resultados de pesquisa.

<div class="important-apis" >
<b>APIs importantes</b><br/>
<ul>
<li>[**Classe AddDeleteThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br243048)</li>
</ul>
</div>


## <a name="dos-and-donts"></a>O que fazer e o que não fazer


-   Use as animações de lista para adicionar um único item novo a um conjunto de itens existente. Por exemplo, use-as quando um novo email chegar ou quando uma nova foto for importada para um conjunto existente.
-   Use animações de lista para adicionar vários itens a um conjunto ao mesmo tempo. Por exemplo, use-as quando for importar um novo conjunto de fotos em uma coleção existente. A adição ou exclusão de vários itens deve acontecer ao mesmo tempo, sem atraso entre a ação sobre os objetos individuais.
-   Use as animações de adição e exclusão na lista como um par. Sempre que usar uma delas, escolha a animação correspondente para a ação oposta.
-   Use as animações de lista com uma lista de itens em que você pode adicionar ou excluir um elemento ou grupo de elementos de uma vez só.
-   Não use as animações de lista para exibir ou remover um contêiner. Essas animações são para membros de uma coleção ou conjunto que já esteja sendo exibido. Use as animações pop-up para mostrar ou ocultar um contêiner transitório sobre a superfície do aplicativo. Use as animações de transição de conteúdo para exibir ou substituir um contêiner que faça parte da superfície do aplicativo.
-   Não use as animações de lista em um conjunto inteiro de itens. Use as animações de transição de conteúdo para adicionar ou remover uma coleção inteira em seu contêiner.



## <a name="related-articles"></a>Artigos relacionados

* [Visão geral de animações](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animando adições e exclusões de lista](https://msdn.microsoft.com/library/windows/apps/xaml/jj649430)
* [Guia de início rápido: animando sua interface do usuário usando animações da biblioteca](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Classe AddDeleteThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br243048)

 

 




