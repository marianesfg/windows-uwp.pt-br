---
Description: O movimento panorâmico ou rolagem permite aos usuários navegar dentro de uma única exibição, para ver o conteúdo da exibição que não se encaixa no visor. Exemplos de exibição incluem a estrutura de pastas de um computador, uma biblioteca de documentos ou um álbum de fotos.
title: Movimento panorâmico
ms.assetid: b419f538-c7fb-4e7c-9547-5fb2494c0b71
label: Panning
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b6e391354b34f00460eb5988f4e03c1ff07a9296
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970111"
---
# <a name="guidelines-for-panning"></a>Diretrizes de movimento panorâmico


O movimento panorâmico ou rolagem permite aos usuários navegar dentro de uma única exibição, para ver o conteúdo da exibição que não se encaixa no visor. Exemplos de exibição incluem a estrutura de pastas de um computador, uma biblioteca de documentos ou um álbum de fotos.

> **APIs importantes**: [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)


## <a name="dos-and-donts"></a>Recomendações


**Indicadores de movimento panorâmico e barras de rolagem**

-   Verifique se o movimento panorâmico/rolagem é possível antes de carregar o conteúdo no seu aplicativo.

-   Exiba indicadores de movimento panorâmico e barras de rolagem para fornecer dicas de localização e tamanho. Oculte-os se o aplicativo fornecer um recurso de navegação personalizado.

    **Observação**  diferentemente das barras de rolagem padrão, os indicadores de panorâmica são puramente informativos. Eles não são expostos para dispositivos de entrada e não podem ser manipulados de nenhuma maneira.

     

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

-   Use o movimento panorâmico de eixo único (tipicamente horizontal) e layouts de colunas para conteúdo baseado em texto e grade. Nesses casos, o conteúdo normalmente encapsula e flui naturalmente de coluna para coluna e mantém a experiência do usuário consistente e detectável em aplicativos do Windows.

-   Não use regiões giráveis incorporadas para exibir texto ou listas de itens. Como os indicadores de movimento panorâmico e as barras de rolagem são exibidos apenas quando o contato de entrada é detectado na região, essa não é uma experiência do usuário intuitiva ou detectável.

-   Não encadeie nem coloque uma região com movimento panorâmico dentro de outra região com movimento panorâmico se ambas girarem na mesma direção, como mostrado aqui. Isso pode fazer com que a área pai seja girada involuntariamente quando um limite da área filho for atingido. Considere tornar o eixo de movimento panorâmico perpendicular.

    ![imagem demonstrando uma área incorporada com capacidade de movimento panorâmico que rola na mesma direção do seu contêiner.](images/scrolling-embedded3.png)

## <a name="additional-usage-guidance"></a>Diretrizes de uso adicional

O movimento horizontal com o uso de toque, com gestos de passar ou deslizar o dedo usando um ou mais dedos, é semelhante à rolagem com o mouse. A interação do deslocamento horizontal é similar ao ato de girar a roda do mouse ou deslizar a barra de rolagem, ao invés de clicar na barra de rolagem. A menos que seja feita uma distinção em uma API ou que haja qualquer exigência em alguma interface do usuário do Windows específica do dispositivo, simplesmente nos referimos às duas interações como movimento panorâmico.

> <div id="main">
> <strong>Atualização dos criadores de outono do Windows 10-alteração de comportamento</strong> Por padrão, em vez de seleção de texto, uma caneta ativa agora rola/se desloca em aplicativos do Windows (como Touch, Touchpad e caneta passiva).  
> Se o seu aplicativo depende do comportamento anterior, você pode substituir a rolagem com caneta e reverter para o comportamento anterior. Para obter detalhes, confira o tópico de referência de API para a <a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer">classe ScrollViewer</a>.
> </div>

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

O Windows 8 aceita três tipos de movimento panorâmico:

-   Eixo único: o movimento panorâmico é possível somente em uma direção (horizontal ou vertical).
-   Trilhos: o movimento panorâmico é possível em todas as direções. No entanto, depois que o usuário cruza um limite de distância em uma direção específica, o movimento panorâmico fica limitado ao eixo em questão.
-   Forma livre: o movimento panorâmico é possível em todas as direções.

**Interface do usuário do movimento panorâmico**

A experiência de interação do movimento panorâmico é exclusiva no dispositivo de entrada, embora ele ainda forneça funcionalidade similar.

**Regiões de panorâmica** Os comportamentos de região de visão panorâmica são expostos ao aplicativo do Windows usando desenvolvedores de JavaScript em tempo de design por meio de folhas de estilos em cascata (CSS).

Há dois modos de exibição de movimento panorâmico baseados no dispositivo de entrada detectado:

-   Indicadores de movimento panorâmico para toque.
-   Barras de rolagem para outros dispositivos de entrada, incluindo mouse, touchpad, teclado e caneta.

**Observação**  os indicadores de panorâmica só ficam visíveis quando o contato do toque está dentro da região de visão panorâmica. Da mesma forma, a barra de rolagem só fica visível quando o cursor do mouse, o cursor da caneta ou o foco do teclado está na região rolável.

 

**Indicadores de movimento panorâmico** Indicadores de movimento panorâmico são semelhantes à caixa de rolagem em uma barra de rolagem. Eles indicam a proporção do conteúdo exibido para a área total que permite movimento panorâmico e a posição relativa do conteúdo exibido na área que permite movimento panorâmico.

O diagrama a seguir mostra duas áreas que permitem movimento panorâmico de diferentes tamanhos e seus indicadores de movimento panorâmico.

![imagem mostrando duas áreas que permitem movimento panorâmico de diferentes tamanhos e seus indicadores de movimento panorâmico.](images/scrolling-indicators.png)

**Panning behaviors**Os pontos de ajuste de movimento panorâmico movimento panorâmico com o gesto de passar o dedo introduz o comportamento de inércia na interação quando o contato de toque é levantado.**Snap points** 
 Com inércia, o conteúdo continua a se mover panoramicamente até que um limite de distância seja atingido sem entrada direta do usuário. Use pontos de alinhamento para modificar esse comportamento inercial.

Pontos de alinhamento especificam paradas lógicas no conteúdo do aplicativo. De forma cognitiva, os pontos de alinhamento atuam como um mecanismo de paginação para o usuário e minimizam o trabalho cansativo de deslizar ou passar o dedo excessivamente em grandes regiões deslocáveis. Com eles, você pode manipular entradas imprecisas do usuário e garantir que um subconjunto específico de conteúdo ou informações importantes sejam exibidas no visor.

Existem dois tipos de pontos de ajuste:

-   Proximidade: depois que o contato for levantado, um ponto de ajuste será selecionado se a inércia parar dentro do limite de distância do ponto de ajuste. O movimento panorâmico ainda pode parar entre pontos de alinhamento de proximidade.
-   Obrigatório - o ponto de ajuste escolhido é aquele que antecede ou sucede imediatamente o último ponto de ajuste cruzado antes que o contato fosse erguido (dependendo da direção e velocidade do gesto). O movimento panorâmico deve parar em um ponto de alinhamento obrigatório.

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

- [Interações personalizadas do usuário](https://docs.microsoft.com/windows/uwp/design/layout/index)
- [Otimizar ListView e GridView](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview)
- [Acessibilidade do teclado](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility)

**Amostras**
- [Amostra de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Amostra de entrada de baixa latência](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Amostra do modo de interação do usuário](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Amostra de visuais de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

**Exemplos de arquivo-morto**
- [Entrada: amostra de eventos de entrada do usuário XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Entrada: amostra de funcionalidades do dispositivo](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Entrada: amostra de teste de hit de toque](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [Exemplo de rolagem, panorâmica e zoom do XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Entrada: amostra de tinta simplificada](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [Entrada: amostra de gestos no Windows 8](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
- [Entrada: exemplo de manipulações e gestos](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [Amostra de entrada por toque do DirectX](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
