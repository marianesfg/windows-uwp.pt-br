---
author: QuinnRadich
Description: The hub control uses a hierarchical navigation pattern to support apps with a relational information architecture.
title: Controles hub
ms.assetid: F1319960-63C6-4A8B-8DA1-451D59A01AC2
label: Hub
template: detail.hbs
ms.author: quradic
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4fecff4fdd836f271b9fb066c58f159111c37772
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/30/2018
ms.locfileid: "3127273"
---
# <a name="hub-controlpattern"></a>Controle/padrão hub

 


Um controle Hub permite organizar o conteúdo do aplicativo em seções ou categorias distintas, mas relacionadas. A seções de um hub devem ser percorridas em uma ordem preferencial e podem servir como ponto de partida para experiências mais detalhadas.

> **APIs importantes**: [classe Hub](https://msdn.microsoft.com/library/windows/apps/dn251843), [classe HubSection](https://msdn.microsoft.com/library/windows/apps/dn251845)

![Exemplo de um hub](images/hub_example_tablet.png)

O conteúdo em um hub pode ser mostrado em uma exibição panorâmica em que os usuários têm uma prévia das novidades, do que está disponível e do que é relevante. Normalmente os hubs têm um cabeçalho de página, e cada uma das várias seções de conteúdo tem um cabeçalho de seção.


## <a name="is-this-the-right-control"></a>Este é o controle correto?

O controle Hub funciona bem para exibir grandes volumes de conteúdo organizado em uma hierarquia. Os hubs priorizam a navegação e a descoberta de novo conteúdo, tornando-se úteis para exibir itens em uma loja ou uma coleção de mídias.

O controle hub tem vários recursos que o fazem funcionar bem para a criação de um padrão de navegação de conteúdo.

-   **Navegação visual**

    Um hub permite que o conteúdo seja exibido em uma matriz diversificada, breve e fácil de pesquisar.

-   **Categorização**

    Cada seção do hub permite que seu conteúdo seja organizado em uma ordem lógica.

-   **Tipos de conteúdo misto**

    Com tipos de conteúdo misto, razões e tamanhos de ativos variáveis são comuns. Um hub permite que cada tipo de conteúdo seja dispostos de forma exclusiva e organizada em cada seção.

-   **Larguras de conteúdo e página variáveis**

    Por ser um modelo panorâmico, o hub permite a variação nas larguras da seção. Isso é ótimo para conteúdo de diferentes profundidades ou volumes.

-   **Arquitetura flexível**

    Se preferir manter a arquitetura do aplicativo superficial, você poderá ajustar todo o conteúdo de canal em um resumo de seções do Hub.

Um Hub é apenas um dos vários elementos de navegação que você pode usar. Para saber mais sobre padrões de navegação e os outros elementos de navegação, consulte [Noções básicas de design de navegação de aplicativos UWP (Plataforma Universal do Windows)](../basics/navigation-basics.md).

## <a name="hub-architecture"></a>Arquitetura do hub

O controle hub tem um padrão de navegação hierárquico que dá suporte a aplicativos com uma arquitetura de informações relacionais. Um hub consiste em categorias de conteúdo diferentes, cada uma mapeada para as páginas de seção do aplicativo. As páginas de seção podem ser exibidas na forma que melhor represente o cenário e o conteúdo contido na seção.

![estrutura delineada de um aplicativo hierárquico Food with Friends](images/navigation_diagram_food_with_friends_app_new.png)

## <a name="layouts-and-panningscrolling"></a>Layouts e movimento panorâmico/rolagem

Há várias maneiras de criar o dispor e navegar pelo conteúdo em um hub; basta ter certeza de que as listas de conteúdo em um hub sempre tenham movimento panorâmico na direção perpendicular à da rolagem do hub.

**Movimento panorâmico horizontal**

![Exemplo de hub de movimento panorâmico horizontal](images/controls_hub_horizontal_pan.png)
**Movimento panorâmico vertical**

![Exemplo de hub de movimento panorâmico vertical](images/controls_hub_vertical_pan.png)
**Movimento panorâmico horizontal com lista/grade de rolagem vertical**

![Exemplo de hub de movimento panorâmico horizontal com uma lista de rolagem vertical](images/controls_hub_horizontal_vertical_scroll.png)
**Movimento panorâmico vertical com lista/grade de rolagem horizontal**

![Exemplo de um hub de movimento panorâmico horizontal](images/controls_hub_vertical_horizontal_scroll.png)

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/Hub">abrir o aplicativo e ver o Hub em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obter o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

O hub proporciona uma grande flexibilidade de design. Isso permite projetar aplicativos que possuam uma grande variedade de experiências atraentes e visualmente ricas. Você pode usar uma imagem de celebridade ou uma seção de conteúdo para o primeiro grupo; uma imagem grande da celebridade pode ser cortada tanto na vertical quanto na horizontal, sem perder o centro de interesse. Veja a seguir um exemplo de uma única imagem de celebridade e como ela pode ser cortada para paisagem, retrato e largura estreita.

![imagem de celebridade cortada para diferentes tamanhos de janela](images/hub_hero_cropped2.png)

Em dispositivos móveis, uma seção de hub está visível por vez.

![Exemplo de um padrão de hub em uma tela pequena](images/phone_hub_example.png)

## <a name="recommendations"></a>Recomendações

-   Para que os usuários saibam que há mais conteúdo em uma seção do hub, recomendamos recortar o conteúdo de forma que uma parte dele seja mostrada.
-   Dependendo das necessidades do aplicativo, é possível adicionar diversas seções de hub ao controle hub, cada uma oferecendo seu próprio propósito funcional. Por exemplo, uma seção poderia conter uma série de links e controles, enquanto outra poderia ser um repositório de miniaturas. Um usuário pode se movimentar entre essas seções usando o suporte a gestos integrado no controle hub.
-   O refluxo dinâmico de conteúdo é a melhor maneira de acomodar tamanhos de janela diferentes.
-   Se houver muitas seções no hub, considere a adição do zoom semântico. Isso também facilita a localização de seções quando o aplicativo é redimensionado para uma largura estreita.
-   Recomendamos que um item em uma seção de hub não leve a outro hub; em vez disso, você pode usar cabeçalhos interativos para navegar para seções ou páginas de outro hub.
-   O hub é um ponto de partida que deve ser personalizado para atender às necessidades do seu aplicativo. É possível alterar os seguintes aspectos de um hub:
    -   Número de seções
    -   Tipo de conteúdo em cada seção
    -   Colocação e ordem das seções
    -   Tamanho das seções
    -   Espaçamento entre seções
    -   Espaçamento entre uma seção e a parte superior ou inferior do hub
    -   Estilo do texto e tamanho em cabeçalhos e conteúdo
    -   Cor do plano de fundo, seções, cabeçalhos de seção e conteúdo de seção

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

- [Classe Hub](https://msdn.microsoft.com/library/windows/apps/dn251843)
- [Noções básicas de navegação](../basics/navigation-basics.md)
- [Como usar um hub](https://msdn.microsoft.com/library/windows/apps/xaml/dn308518)
- [Exemplo de controle do Hub em XAML](http://go.microsoft.com/fwlink/p/?LinkID=310072)
