---
author: Jwmsft
Description: O padrão mestre/detalhes exibe uma lista mestra e os detalhes do item atualmente selecionado. Esse padrão é usado com frequência para email e listas de contatos/catálogos de endereços.
title: Mestre/detalhes
ms.assetid: 45C9FE8B-ECA6-44BF-8DDE-7D12ED34A7F7
label: Master/details
template: detail.hbs
---
# Padrão mestre/detalhes

O padrão mestre/detalhes tem um painel mestre (geralmente com uma [exibição de lista](lists.md)) e um painel de detalhes para o conteúdo. Quando um item na lista mestra é selecionado, o painel de detalhes é atualizado. Esse padrão é usado com frequência para email e catálogos de endereços.

![Exemplo do padrão mestre/detalhes](images/HIGSecOne_MasterDetail.png)

## Este é o padrão certo?

O padrão mestre/detalhes funciona bem para:

-   Criar um aplicativo de email, catálogo de endereços ou qualquer aplicativo que se baseia em um layout de lista-detalhes.
-   Localizar e priorizar uma grande coleção de conteúdo.
-   Permitir a rápida adição e remoção de itens de uma lista enquanto se alterna entre contextos.

## Escolher o estilo certo

Ao implementar o padrão mestre/detalhes, recomendamos usar o estilo empilhado ou lado a lado, dependendo do espaço disponível na tela.

| Largura da janela disponível | Estilo recomendado |
|------------------------|-------------------|
| 320 epx-719 epx        | Empilhado           |
| 720 epx ou mais larga       | Lado a lado      |

 
## Estilo empilhado

No estilo empilhado, apenas um painel fica visível por vez: o mestre ou os detalhes.

![Mestre/detalhes no modo empilhado](images/patterns-md-stacked.png)

O usuário começa no painel mestre e "faz drill down" até o painel de detalhes, selecionando um item na lista mestra. Para o usuário, parece que os modos de exibição mestre e detalhado existem em duas páginas separadas.

### Criar um padrão mestre/detalhes empilhado

Uma maneira de criar o padrão mestre/detalhes empilhado é usar páginas separadas para o painel mestre e para o painel de detalhes. Coloque a exibição de lista que fornece a lista mestra em uma página, e o elemento de conteúdo do painel de detalhes em uma página separada.

![Partes do mestre/detalhes no estilo empilhado](images/patterns-md-stacked-parts.png)

Para o painel mestre, um controle de [exibição de lista](lists.md) funciona bem para apresentar listas que podem conter imagens e texto.

Para o painel de detalhes, use o elemento de conteúdo mais lógico. Se tiver muitos campos separados, considere o uso de um layout de grade para organizar os elementos em um formulário.

## Estilo lado a lado

No estilo lado a lado, o painel mestre e o painel de detalhes ficam visíveis ao mesmo tempo.

![O padrão mestre/detalhes](images/patterns-masterdetail-400x227.png)

A lista no painel mestre tem uma seleção visual para indicar o item atualmente selecionado. Ao selecionar um novo item na lista mestra, o painel de detalhes é atualizado.

### Criar um padrão de mestre/detalhes lado a lado

Para o painel mestre, um controle de [exibição de lista](lists.md) funciona bem para apresentar listas que podem conter imagens e texto.

Para o painel de detalhes, use o elemento de conteúdo mais lógico. Se tiver muitos campos separados, considere o uso de um layout de grade para organizar os elementos em um formulário.

## Exemplos

Este design de aplicativo que rastreia o mercado de ações usa um padrão mestre/detalhes. Neste exemplo do aplicativo como apareceria no telefone, o painel/lista mestra encontra-se à esquerda, com o painel de detalhes à direita.

![Exemplo de aplicativo usando o padrão mestre-detalhes no telefone](images/uap-finance-phone-masterdetails-600.png)

Este design de aplicativo que rastreia o mercado de ações usa um padrão mestre/detalhes. Neste exemplo do aplicativo como apareceria na área de trabalho, o painel/lista mestra e o painel de detalhes ficam visíveis e em tela inteira. O painel mestre apresenta uma caixa de pesquisa na parte superior e uma barra de comandos na parte inferior.

![Exemplo de aplicativo usando o padrão mestre-detalhes na área de trabalho](images/uap-finance-desktop700.png)



## Artigos relacionados

- [Listas](lists.md)
- [Pesquisar](search.md)
- [Aplicativo e barras de comandos](app-bars.md)
- [**Classe ListView (XAML)**](https://msdn.microsoft.com/library/windows/apps/br242878)


<!--HONumber=May16_HO2-->


