---
author: QuinnRadich
Description: The master/detail pattern displays a master list and the details for the currently selected item. This pattern is frequently used for email and contact lists/address books.
title: Mestre/detalhes
ms.assetid: 45C9FE8B-ECA6-44BF-8DDE-7D12ED34A7F7
label: Master/details
template: detail.hbs
ms.author: quradic
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 28a6160019fd3bf64dd1f75bc5c6df29cd3165ad
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4389634"
---
# <a name="masterdetails-pattern"></a>Padrão mestre/detalhes

 

O padrão mestre/detalhes tem um painel mestre (geralmente com uma [exibição de lista](lists.md)) e um painel de detalhes para o conteúdo. Quando um item na lista mestra é selecionado, o painel de detalhes é atualizado. Esse padrão é usado com frequência para email e catálogos de endereços.

> **APIs importantes**: [classe ListView](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.ListView), [classe SplitView](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.splitview)

![Exemplo do padrão mestre/detalhes](images/HIGSecOne_MasterDetail.png)

## <a name="is-this-the-right-pattern"></a>Este é o padrão certo?

O padrão mestre/detalhes funciona bem para:

-   Criar um aplicativo de email, catálogo de endereços ou qualquer aplicativo que se baseia em um layout de lista-detalhes.
-   Localizar e priorizar uma grande coleção de conteúdo.
-   Permitir a rápida adição e remoção de itens de uma lista enquanto se alterna entre contextos.

## <a name="choose-the-right-style"></a>Escolher o estilo certo

Ao implementar o padrão mestre/detalhes, recomendamos usar o estilo empilhado ou lado a lado, dependendo do espaço disponível na tela.

| Largura da janela disponível | Estilo recomendado |
|------------------------|-------------------|
| 320 epx-640 epx        | Empilhado           |
| 641 epx ou mais larga       | Lado a lado      |

 
## <a name="stacked-style"></a>Estilo empilhado

No estilo empilhado, apenas um painel fica visível por vez: o mestre ou os detalhes.

![Mestre/detalhes no modo empilhado](images/patterns-md-stacked.png)

O usuário começa no painel mestre e "faz drill down" até o painel de detalhes, selecionando um item na lista mestra. Para o usuário, parece que os modos de exibição mestre e detalhado existem em duas páginas separadas.

### <a name="create-a-stacked-masterdetails-pattern"></a>Criar um padrão mestre/detalhes empilhado

Uma maneira de criar o padrão mestre/detalhes empilhado é usar páginas separadas para o painel mestre e para o painel de detalhes. Coloque o modo de exibição mestre em uma página e o painel de detalhes em uma página separada.

![Partes do mestre/detalhes no estilo empilhado](images/patterns-md-stacked-parts.png)

Para página de visualização mestre, um controle de [exibição de lista](lists.md) funciona bem para apresentar listas que podem conter imagens e texto. 

Para o painel de detalhes, use o  [content element](../layout/layout-panels.md) que faz mais sentido. Se tiver muitos campos separados, considere o uso de um layout de **Grid** para organizar os elementos em um formulário.

Para navegação entre páginas, consulte [histórico de navegação e navegação regressiva para aplicativos UWP](../basics/navigation-history-and-backwards-navigation.md).

## <a name="side-by-side-style"></a>Estilo lado a lado

No estilo lado a lado, o painel mestre e o painel de detalhes ficam visíveis ao mesmo tempo.

![O padrão mestre/detalhes](images/patterns-masterdetail-400x227.png)

A lista no painel mestre tem uma seleção visual para indicar o item atualmente selecionado. Ao selecionar um novo item na lista mestra, o painel de detalhes é atualizado.

### <a name="create-a-side-by-side-masterdetails-pattern"></a>Criar um padrão de mestre/detalhes lado a lado

Uma maneira de criar um padrão mestre/detalhes lado a lado é usar o controle [modo divisão](split-view.md). Coloque o modo de exibição mestre no painel de modo de exibição dividido e o modo de exibição de detalhes no conteúdo de modo de exibição dividido.

![partes de exibição de divisão de detalhe mestre](images/patterns_md_splitview_parts.png)

Para o painel mestre, um controle de [exibição de lista](lists.md) funciona bem para apresentar listas que podem conter imagens e texto.

Para o conteúdo de detalhes, use o [content element](../layout/layout-panels.md) que faz mais sentido. Se tiver muitos campos separados, considere o uso de um layout de **Grid** para organizar os elementos em um formulário.

## <a name="adaptive-layout"></a>Layout adaptável

Para implementar um padrão de mestre/detalhes para qualquer tamanho de tela, crie uma interface do usuário responsiva com um [layout adaptável ](../layout/layouts-with-xaml.md).

![layout de detalhe mestre adaptável](images/patterns_masterdetail.png)

### <a name="create-an-adaptive-masterdetails-pattern"></a>Criar um mestre adaptável/padrão de detalhes
Para criar um layout adaptável, definir diferentes [**VisualStates**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.visualstate) para sua interface do usuário e declare pontos de interrupção para os estados diferentes com [**AdaptiveTriggers **](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.AdaptiveTrigger).

## <a name="get-the-sample-code"></a>Obter o código de exemplo

Os exemplos a seguir implementam o padrão de mestre/detalhes com layouts adaptáveis e demonstram a vinculação de dados para estático, banco de dados e recursos online: 
- [Amostra de mestre/detalhes](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlMasterDetail) 
- [Exemplo de seleção além de mestre/detalhes](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)
- [Exemplo do Windows Template Studio de mestre/detalhes](https://github.com/Microsoft/WindowsTemplateStudio/tree/master/templates/Uwp/Pages/MasterDetail)
- [Exemplo de banco de dados de pedidos do cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
- [Exemplo de leitor RSS](https://github.com/Microsoft/Windows-appsample-rssreader)

## <a name="related-articles"></a>Artigos relacionados

- [Listas](lists.md)
- [Pesquisa](search.md)
- [Aplicativo e barras de comandos](app-bars.md)
- [Classe ListView](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [Classe SplitView](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.splitview)
