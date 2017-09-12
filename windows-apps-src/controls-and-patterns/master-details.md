---
author: Jwmsft
Description: "O padrão mestre/detalhes exibe uma lista mestra e os detalhes do item atualmente selecionado. Esse padrão é usado com frequência para email e listas de contatos/catálogos de endereços."
title: Mestre/detalhes
ms.assetid: 45C9FE8B-ECA6-44BF-8DDE-7D12ED34A7F7
label: Master/details
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 49a586aac0c846cdad02f8448532238bd3eb8551
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2017
---
# <a name="masterdetails-pattern"></a>Padrão mestre/detalhes

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

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
| 320 epx-719 epx        | Empilhado           |
| 720 epx ou mais larga       | Lado a lado      |

 
## <a name="stacked-style"></a>Estilo empilhado

No estilo empilhado, apenas um painel fica visível por vez: o mestre ou os detalhes.

![Mestre/detalhes no modo empilhado](images/patterns-md-stacked.png)

O usuário começa no painel mestre e "faz drill down" até o painel de detalhes, selecionando um item na lista mestra. Para o usuário, parece que os modos de exibição mestre e detalhado existem em duas páginas separadas.

### <a name="create-a-stacked-masterdetails-pattern"></a>Criar um padrão mestre/detalhes empilhado

Uma maneira de criar o padrão mestre/detalhes empilhado é usar páginas separadas para o painel mestre e para o painel de detalhes. Coloque a exibição de lista que fornece a lista mestra em uma página, e o elemento de conteúdo do painel de detalhes em uma página separada.

![Partes do mestre/detalhes no estilo empilhado](images/patterns-md-stacked-parts.png)

Para o painel mestre, um controle de [exibição de lista](lists.md) funciona bem para apresentar listas que podem conter imagens e texto.

Para o painel de detalhes, use o elemento de conteúdo mais lógico. Se tiver muitos campos separados, considere o uso de um layout de grade para organizar os elementos em um formulário.

## <a name="side-by-side-style"></a>Estilo lado a lado

No estilo lado a lado, o painel mestre e o painel de detalhes ficam visíveis ao mesmo tempo.

![O padrão mestre/detalhes](images/patterns-masterdetail-400x227.png)

A lista no painel mestre tem uma seleção visual para indicar o item atualmente selecionado. Ao selecionar um novo item na lista mestra, o painel de detalhes é atualizado.

### <a name="create-a-side-by-side-masterdetails-pattern"></a>Criar um padrão de mestre/detalhes lado a lado

Para o painel mestre, um controle de [exibição de lista](lists.md) funciona bem para apresentar listas que podem conter imagens e texto.

Para o painel de detalhes, use o elemento de conteúdo mais lógico. Se tiver muitos campos separados, considere o uso de um layout de grade para organizar os elementos em um formulário.

## <a name="get-the-code-samples"></a>Obter as amostras de código

Para o código de exemplo que mostra o padrão mestre/detalhes, consulte estes exemplos: 

- [Exemplo de banco de dados de pedidos do cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database) 
- [Exemplo de ListView e GridView](http://go.microsoft.com/fwlink/p/?LinkId=619900)
- [Exemplo de leitor RSS](https://github.com/Microsoft/Windows-appsample-rssreader)

## <a name="related-articles"></a>Artigos relacionados

- [Listas](lists.md)
- [Pesquisa](search.md)
- [Aplicativo e barras de comandos](app-bars.md)
- [Classe ListView](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.ListView)
