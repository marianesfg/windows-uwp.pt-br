---
Description: Panning or scrolling lets users navigate within a single view, to display the content of the view that does not fit within the viewport. Examples of views include the folder structure of a computer, a library of documents, or a photo album.
title: Movimento panorâmico
ms.assetid: b419f538-c7fb-4e7c-9547-5fb2494c0b71
label: Panning
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 340289c16cfd9c63f578c63827b1c0b35162cdfd
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8344619"
---
# <a name="guidelines-for-panning"></a>Diretrizes de movimento panorâmico


O movimento panorâmico ou rolagem permite aos usuários navegar dentro de uma única exibição, para ver o conteúdo da exibição que não se encaixa no visor. Exemplos de exibição incluem a estrutura de pastas de um computador, uma biblioteca de documentos ou um álbum de fotos.

> **APIs importantes**: [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084), [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994)


## <a name="dos-and-donts"></a>O que fazer e o que não fazer


**Indicadores de movimento panorâmico e barras de rolagem**

-   Verifique se o movimento panorâmico/rolagem é possível antes de carregar o conteúdo no seu aplicativo.

-   Exiba indicadores de movimento panorâmico e barras de rolagem para fornecer dicas de localização e tamanho. Oculte-os se o aplicativo fornecer um recurso de navegação personalizado.

    **Observação**ao contrário de rolagem barras, indicadores de movimento panorâmico são puramente informativos. Eles não são expostos para dispositivos de entrada e não podem ser manipulados de nenhuma maneira.

     

**Movimento panorâmico de eixo único (estouro de capacidade unidimensional)**

-   Use o movimento panorâmico de eixo único para regiões de conteúdo que vão além do limite de um visor (vertical ou horizontal).

    -   Movimento panorâmico vertical para uma lista de itens unidimensional.
    -   Movimento panorâmico horizontal para uma grade de itens.
-   Não use pontos de ajuste obrigatórios com movimento panorâmico de eixo único se for permitir que o usuário gire e pare entre os pontos de ajuste. Pontos de ajuste obrigatórios garantem que o usuário irá "parar" em um ponto de ajuste. Em vez disso, use pontos de ajuste de proximidade.

**Movimento panorâmico de forma livre (estouro de capacidade bidimensional)**

-   Use o movimento panorâmico de dois eixos para regiões de conteúdo que vão além dos limites de visor (vertical ou horizontal).

    -   Substitua o comportamento de trilhos padrão e use o movimento panorâmico de forma livre para conteúdo não estruturado em que o usuário provavelmente realizará deslocamento em várias direções.
-   O movimento panorâmico de forma livre é adequado para navegação em imagens ou mapas.

**Exibição paginada**

-   Use pontos de ajuste obrigatórios quando o conteúdo for composto por elementos discretos ou você quiser exibir um elemento inteiro. Isso pode incluir páginas de um livro ou uma revista, uma coluna de itens ou imagens individuais.

    -   Um ponto de ajuste deve ser colocado em cada limite lógico.
    -   Cada elemento deve ser dimensionado ou escalonado para caber no modo de exibição.

**Pontos lógicos e chaves**

-   Use pontos de ajuste de proximidade se houver pontos chave ou casas lógicas no conteúdo em que o usuário provavelmente irá parar. Por exemplo, um cabeçalho de seção.

-   Se forem definidas restrições de tamanho máximo e mínimo ou limites, use o retorno visual para demonstrar quando o usuário atinge ou excede esses limites.

**Encadeamento incorporado ou conteúdo aninhado**

-   Use o movimento panorâmico de eixo único (tipicamente horizontal) e layouts de colunas para conteúdo baseado em texto e grade. Nesses casos, o conteúdo costuma se ajustar e fluir naturalmente de coluna para coluna e manter a experiência do usuário consistente e detectável nos aplicativos UWP.

-   Não use regiões giráveis incorporadas para exibir texto ou listas de itens. Como os indicadores de movimento panorâmico e as barras de rolagem são exibidos apenas quando o contato de entrada é detectado na região, essa não é uma experiência do usuário intuitiva ou detectável.

-   Não encadeie nem coloque uma região com movimento panorâmico dentro de outra região com movimento panorâmico se ambas girarem na mesma direção, como mostrado aqui. Isso pode fazer com que a área pai seja girada involuntariamente quando um limite da área filho for atingido. Considere tornar o eixo de movimento panorâmico perpendicular.

    ![imagem demonstrando uma área incorporada com capacidade de movimento panorâmico que rola na mesma direção do seu contêiner.](images/scrolling-embedded3.png)

## <a name="additional-usage-guidance"></a>Diretrizes de uso adicionais

O movimento horizontal com o uso de toque, com gestos de passar ou deslizar o dedo usando um ou mais dedos, é semelhante à rolagem com o mouse. A interação do deslocamento horizontal é similar ao ato de girar a roda do mouse ou deslizar a barra de rolagem, ao invés de clicar na barra de rolagem. A menos que uma distinção é feita em uma API ou exigida por alguns WindowsUI específicas ao dispositivo, simplesmente nos referimos às duas interações como movimento panorâmico.

> <div id="main">
> <strong>Windows 10 Fall Creators Update - Mudança de comportamento</strong>
> </div>
> Por padrão, em vez de seleção de texto, uma caneta ativa agora fará rolagem/movimento panorâmico em aplicativos UWP (como touch, touchpad e caneta passiva).  
> Se o seu aplicativo depende do comportamento anterior, você pode substituir a rolagem com caneta e reverter para o comportamento anterior. Consulte o tópico de referência de API da [Classe ScrollViewer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer) para obter detalhes.

Dependendo do dispositivo de entrada, o usuário faz o deslocamento horizontal dentro da região com o movimento panorâmico usando uma das seguintes opções:

-   Um mouse, um touchpad ou uma caneta para clicar nas setas de rolagem, arrastar a caixa de rolagem ou clicar na barra de rolagem.
-   O botão de roda do mouse para emular a caixa de rolagem sendo arrastada.
-   Os botões estendidos (XBUTTON1 e XBUTTON2), se tiverem suporte no mouse.
-   As teclas de seta do teclado para emular a caixa de rolagem sendo arrastada ou as teclas de página para emular o clique na barra de rolagem.
-   Toque, touchpad ou caneta passiva para deslizar ou passar o dedo na direção desejada.

Deslizar envolve mover os dedos lentamente na direção do movimento panorâmico. Esta ação gera movimentos de um-em-um, nos quais o conteúdo é deslocado horizontalmente na mesma velocidade e distância dos dedos. O deslizamento (deslizar e levantar os dedos rapidamente) resulta na aplicação da seguinte função à animação panorâmica:

-   Desaceleração (inércia): levantar os dedos faz com que o movimento panorâmico seja desacelerado. Isso é semelhante a deslizar até parar em uma superfície escorregadia.
-   Absorção: a cinética de movimento panorâmico durante a desaceleração provoca um ligeiro efeito de recuperação se um ponto de ajuste ou um limite da área de conteúdo for atingido.

**Tipos de movimento panorâmico**

Windows8 dá suporte a três tipos de movimento panorâmico:

-   Eixo único: o movimento panorâmico é possível somente em uma direção (horizontal ou vertical).
-   Trilhos: o movimento panorâmico é possível em todas as direções. No entanto, depois que o usuário cruza um limite de distância em uma direção específica, o movimento panorâmico fica limitado ao eixo em questão.
-   Forma livre: o movimento panorâmico é possível em todas as direções.

**Interface do usuário do movimento panorâmico**

A experiência de interação do movimento panorâmico é exclusiva no dispositivo de entrada, embora ele ainda forneça funcionalidade similar.

**Regiões que permitem movimento panorâmico** Os comportamentos das regiões que permitem movimento panorâmico são expostos no aplicativo UWP usando desenvolvedores de JavaScript no tempo de design, por meio de CSS (Folhas de Estilos em Cascata).

Há dois modos de exibição de movimento panorâmico baseados no dispositivo de entrada detectado:

-   Indicadores de movimento panorâmico para toque.
-   Barras de rolagem para outros dispositivos de entrada, incluindo mouse, touchpad, teclado e caneta.

**Observação**indicadores de movimento panorâmico ficam visíveis somente quando o contato por toque está dentro da região que permite movimento panorâmico. Da mesma forma, a barra de rolagem só fica visível quando o cursor do mouse, o cursor da caneta ou o foco do teclado está na região rolável.

 

**Indicadores de movimento panorâmico** Indicadores de movimento panorâmico são semelhantes à caixa de rolagem em uma barra de rolagem. Eles indicam a proporção do conteúdo exibido para a área total que permite movimento panorâmico e a posição relativa do conteúdo exibido na área que permite movimento panorâmico.

O diagrama a seguir mostra duas áreas que permitem movimento panorâmico de diferentes tamanhos e seus indicadores de movimento panorâmico.

![imagem mostrando duas áreas que permitem movimento panorâmico de diferentes tamanhos e seus indicadores de movimento panorâmico.](images/scrolling-indicators.png)

**Comportamentos de movimento panorâmico**
**Pontos de alinhamento** O movimento panorâmico com o gesto de passar o dedo introduz comportamento inercial na interação quando o contato por toque é retirado. Com inércia, o conteúdo continua a se mover panoramicamente até que um limite de distância seja atingido sem entrada direta do usuário. Use pontos de alinhamento para modificar esse comportamento inercial.

Pontos de alinhamento especificam paradas lógicas no conteúdo do aplicativo. De forma cognitiva, os pontos de alinhamento atuam como um mecanismo de paginação para o usuário e minimizam o trabalho cansativo de deslizar ou passar o dedo excessivamente em grandes regiões deslocáveis. Com eles, você pode manipular entradas imprecisas do usuário e garantir que um subconjunto específico de conteúdo ou informações importantes sejam exibidas no visor.

Existem dois tipos de pontos de ajuste:

-   Proximidade: depois que o contato for levantado, um ponto de ajuste será selecionado se a inércia parar dentro do limite de distância do ponto de ajuste. O movimento panorâmico ainda pode parar entre pontos de alinhamento de proximidade.
-   Obrigatório: o ponto de alinhamento escolhido é aquele que antecede ou sucede imediatamente o último ponto de alinhamento cruzado antes que o contato seja retirado (dependendo da direção e da velocidade do gesto). O movimento panorâmico deve parar em um ponto de alinhamento obrigatório.

Pontos de ajuste de movimento panorâmico são úteis para aplicativos como navegadores da Web e álbuns de fotos que emulam conteúdo paginado ou possuem agrupamentos lógicos de itens que podem ser reagrupados dinamicamente para caber em um visor ou tela.

Os diagramas a seguir mostram como o movimento panorâmico até um certo ponto e a liberação fazem com a panorâmica do conteúdo mude automaticamente para uma localização lógica.

|                                                                |                                                                                         |                                                                                                                 |
|----------------------------------------------------------------|-----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| ![imagem mostrando uma área que permite movimento panorâmico.](images/ux-panning-snap1.png) | ![imagem mostrando uma área que permite movimento panorâmico sendo movimentada para a esquerda.](images/ux-panning-snap2.png) | ![imagem mostrando uma área que permite movimento panorâmico que parou em um ponto de alinhamento lógico.](images/ux-panning-snap3.png) |
| Passe o dedo para realizar o movimento panorâmico                                                  | Retire o contato por toque.                                                                     | A região que permite movimento panorâmico para no ponto de alinhamento, não onde o contato por toque foi retirado.                                |

 

**Trilhos** O conteúdo pode ser mais largo ou mais longo do que a dimensão e a resolução de um dispositivo de exibição. Por isso, o movimento panorâmico bidimensional (horizontal e vertical) é geralmente necessário. Os trilhos melhoram a experiência do usuário nesses casos enfatizando o movimento panorâmico ao longo do eixo de movimento (vertical ou horizontal).

O diagrama a seguir demonstra o conceito de trilhos.

![diagrama de uma tela com trilhos que limitam o movimento panorâmico](images/ux-panning-rails.png)

**Encadeamento incorporado ou conteúdo aninhado**

Após o usuário atingir o limite de zoom ou rolagem em um elemento que foi aninhado dentro de outro elemento ampliável ou rolável, é possível especificar se o elemento pai deve continuar a operação de ampliar ou rolar iniciada no respectivo elemento filho. Isso é chamado encadeamento de zoom ou de rolagem.

O encadeamento é utilizado para movimento panorâmico em uma área de conteúdo de eixo único que contém uma ou mais regiões de movimento panorâmico de eixo único ou de formato livre (quando o contato por toque está dentro de uma dessas regiões filhas). Quando o limite de movimento panorâmico da região filha é atingido em uma direção específica, o movimento panorâmico é então ativado na região pai na mesma direção.

Quando uma região que permite movimento panorâmico é colocada dentro de outra região de mesmo tipo, é importante especificar espaço suficiente entre o contêiner e o conteúdo inserido. Nos diagramas a seguir, uma região que permite movimento panorâmico é colocada dentro de outra região de mesmo tipo, cada uma delas em direções perpendiculares. Há muito espaço para que os usuários realizem movimentos panorâmicos em cada região.

![imagem demonstrando uma área que permite movimento panorâmico incorporada.](images/scrolling-embedded.png)

Sem espaço suficiente, como mostrado no diagrama a seguir, a região que permite movimento panorâmico incorporada pode interferir no movimento panorâmico do contêiner e resultar em movimento panorâmico não intencional em uma ou mais das regiões que permitem movimento panorâmico.

![imagem demonstrando preenchimento insuficiente para uma área que permite movimento panorâmico incorporada.](images/ux-panning-embedded-wrong.png)

Essa diretriz também é útil para aplicativos como, por exemplo, álbuns de fotografias e aplicativos de mapeamento que dão suporte ao movimento panorâmico irrestrito em cada imagem ou mapa e que, ao mesmo tempo, dão suporte ao movimento panorâmico de eixo único no álbum (para as imagens anteriores ou seguintes) ou na área de detalhes. Em aplicativos que fornecem uma área de detalhes ou opções correspondente a uma imagem ou mapa de forma livre do movimento panorâmico, recomendamos que o layout da página comece com a área de detalhes e opções, pois a área de movimento panorâmico irrestrito da imagem ou do mapa pode interferir com o movimento panorâmico na área de detalhes.

## <a name="related-articles"></a>Artigos relacionados


* [Interações personalizadas do usuário](https://msdn.microsoft.com/library/windows/apps/mt185599)
* [Otimizar ListView e GridView](https://msdn.microsoft.com/library/windows/apps/mt204776)
* [Acessibilidade do teclado](https://msdn.microsoft.com/library/windows/apps/mt244347)

**Exemplos**
* [Amostra de entrada básica](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Amostra de entrada de baixa latência](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Amostra do modo de interação do usuário](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Amostra de elementos visuais do foco](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**Exemplos de arquivo-morto**
* [Entrada: amostra de eventos de entrada do usuário XAML](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [Entrada: exemplo de funcionalidades do dispositivo](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrada: amostra de teste de toque](https://go.microsoft.com/fwlink/p/?linkid=231590)
* [Amostra de rolagem, movimento panorâmico e aplicação de zoom em XAML](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrada: amostra de tinta simplificada](https://go.microsoft.com/fwlink/p/?linkid=246570)
* [Entrada: amostra de gestos no Windows 8](https://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Entrada: amostra de manipulações e gestos (C++)](https://go.microsoft.com/fwlink/p/?linkid=231605)
* [Amostra de entrada por toque do DirectX](https://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 




