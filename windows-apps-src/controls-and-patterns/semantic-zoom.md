---
author: Jwmsft
Description: "Um controle de zoom semântico permite que o usuário aplique zoom entre duas diferentes exibições do mesmo conjunto de dados."
title: "Zoom semântico"
ms.assetid: B5C21FE7-BA83-4940-9CC1-96F6A2DC28C7
label: Semantic zoom
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 47e39290a63408fc66783617ad2f12345eab2fa3

---

# Zoom semântico



O controle de zoom semântico permite que o usuário aplique zoom entre duas exibições diferentes do mesmo conteúdo, tornando possível navegar rapidamente por um grande conjunto de dados. A exibição ampliada é o modo de exibição principal do conteúdo. Você pode mostrar o conjunto de dados completo nesse modo de exibição. A exibição reduzida é um modo de exibição de alto nível do mesmo conteúdo. Normalmente, você mostra os cabeçalhos de grupo para um conjunto de dados agrupado nesse modo de exibição. Por exemplo, ao ver um catálogo de endereços, o usuário poderia ampliar uma letra para ver os nomes associados à letra. 

**APIs importantes**

-   [**Classe SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601)


            **Recursos**:

-   O tamanho da exibição reduzida é restrito pelos limites do controle de zoom semântico.
-   Ao tocar em um cabeçalho de grupo, os modos de exibição são alternados. É possível habilitar a pinçagem para alternar entre os modos de exibição.
-   Os cabeçalhos ativos alternam entre os modos de exibição.

## Exemplos

Um zoom semântico usado no aplicativo Fotos.

![Um zoom semântico usado no aplicativo Fotos](images/control-examples/semantic-zoom-photos.png)

Um catálogo de endereços é um exemplo de conjunto de dados que pode ser muito mais fácil de navegar com um controle de zoom semântico. Em um modo de exibição, fica a visão geral completa, alfanumérica, de pessoas no catálogo de endereços (imagem esquerda), e a exibição ampliada mostra os dados em ordem e com mais detalhes (imagem direita).

![exemplo de zoom semântico usado em uma lista de contatos](images/semanticzoom-win10.png)

## Recomendações

-   Ao usar o zoom semântico em seu aplicativo, o layout do item e a direção do movimento panorâmico não devem mudar de acordo com o nível de zoom. Os layouts e as interações de movimento panorâmico devem ser consistentes e previsíveis em todos os níveis de zoom.
-   Como o zoom semântico, o usuário pode saltar rapidamente para o conteúdo. Então, limite o número de páginas/telas a três no modo com zoom reduzido. Muito movimento panorâmico diminui a viabilidade do zoom semântico.
-   Evite usar o zoom semântico para alterar o escopo do conteúdo. Por exemplo, um álbum de fotos não deve alternar para um modo de exibição de pasta no Gerenciador de Arquivos.
-   Use uma estrutura e a semântica essenciais ao modo de exibição.
-   Use os nomes dos grupos para os itens em uma coleção agrupada.
-   Use ordens de classificação para uma coleção desagrupada, porém classificada; por exemplo, a ordem cronológica para datas e ordem alfabética para uma lista de nomes.



## Artigos relacionados

* [Diretrizes para interações comuns do usuário](https://dev.windows.com/design/inputs-devices)


**Amostras (XAML)**
* [Entrada: amostra de eventos de entrada do usuário XAML](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Amostra de rolagem, movimento panorâmico e aplicação de zoom em XAML](http://go.microsoft.com/fwlink/p/?linkid=251717)

**Amostras (DirectX)**
* [Amostra de entrada por toque do DirectX](http://go.microsoft.com/fwlink/p/?LinkID=231627)
* [Entrada: amostra de interações e gestos (C++)](http://go.microsoft.com/fwlink/p/?linkid=231605)
 

 







<!--HONumber=Jun16_HO4-->


