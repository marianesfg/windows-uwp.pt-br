---
Description: Este tópico descreve o zoom e o redimensionamento de elementos do Windows e fornece as diretrizes da experiência do usuário para o uso desses mecanismos de interação em seus aplicativos.
title: Diretrizes de zoom óptico e redimensionamento
ms.assetid: 51a0007c-8a5d-4c44-ac9f-bbbf092b8a00
label: Optical zoom and resizing
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b63c9191489ecae54b17cb75b8aa1af32f09fcb8
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66363595"
---
# <a name="optical-zoom-and-resizing"></a>Zoom óptico e redimensionamento



Este artigo descreve o zoom e o redimensionamento de elementos do Windows e fornece as diretrizes da experiência do usuário para o uso desses mecanismos de interação em seus aplicativos.

> **APIs importantes**: [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [ **entrada (XAML)** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

O zoom óptico permite que os usuários ampliem o modo de exibição do conteúdo em uma área de conteúdo (isso é executado na própria área de conteúdo), enquanto o redimensionamento permite que alterem o tamanho relativo de um ou mais objetos sem alterar o modo de exibição da área de conteúdo (isso é executado em objetos na área de conteúdo).

As interações de zoom óptico e de redimensionamento são realizadas com os gestos de pinçagem e ampliação (afastar os dedos aumenta o zoom e aproximar os dedos reduz o zoom) ou mantendo a tecla Ctrl pressionada e girando a roda de rolagem do mouse ou mantendo a tecla Ctrl pressionada (com a tecla Shift caso o teclado numérico não esteja disponível) e pressionando a tecla mais (+) ou menos (-).

Os diagramas a seguir demonstram as diferenças entre redimensionamento e zoom óptico.

**Zoom ótico**: Usuário seleciona uma área e, em seguida, aplica zoom em toda a área.

![juntar os dedos aumenta o zoom e afastar os dedos o diminui](images/areazoom.png)

**Resize**: Usuário seleciona um objeto dentro de uma área e redimensiona esse objeto.

![juntar os dedos reduz um objeto e separá-los o aumenta](images/objectresize.png)

**Observação**    zoom ótico não deve ser confundido com [Zoom semântico](../controls-and-patterns/semantic-zoom.md). Embora essas interações compartilhem os mesmos gestos, o zoom semântico refere-se à apresentação e à navegação de conteúdo organizado em um único modo de exibição (como a estrutura de pastas de um computador, uma biblioteca de documentos ou um álbum de fotografias).

 

## <a name="dos-and-donts"></a>O que fazer e o que não fazer


Use as diretrizes a seguir para aplicativos compatíveis com redimensionamento ou zoom óptico:

-   Se forem definidas restrições ou limites de tamanho máximo e mínimo, use o retorno visual para demonstrar quando o usuário atingiu ou excedeu esses limites.
-   Use pontos de ajuste para influenciar o comportamento de zoom e de redimensionamento fornecendo pontos lógicos em que é necessário parar a manipulação e garantir que um subconjunto de conteúdo específico seja exibido no visor. Forneça pontos de ajuste para níveis de zoom comuns ou modos de exibição lógicos para que seja mais fácil para um usuário escolher esses níveis. Por exemplo, aplicativos de fotos podem fornecer um ponto de ajuste a 100% para o redimensionamento ou, no caso de aplicativos de mapas, os pontos de ajuste podem ser úteis para modos de exibição de cidade, estado e país.

    Pontos de ajuste permitem que os usuários sejam imprecisos e ainda atinjam suas metas. Se estiver usando XAML, veja as propriedades dos pontos de ajuste do [**ScrollViewer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer). Para JavaScript e HTML, use [ **-ms-content-zoom-snap-points**](https://msdn.microsoft.com/library/hh771895).

    Existem dois tipos de pontos de ajuste:

    -   Proximidade: depois que o contato for levantado, um ponto de ajuste será selecionado se a inércia parar dentro do limite de distância do ponto de ajuste. Os pontos de ajuste de proximidade ainda permitem que um zoom ou redimensionamento seja finalizado entre pontos de ajuste.
    -   Obrigatório: o ponto de alinhamento escolhido é aquele que antecede ou sucede imediatamente o último ponto de alinhamento cruzado antes que o contato seja retirado (dependendo da direção e da velocidade do gesto). Uma manipulação precisa ser finalizada em um ponto de ajuste obrigatório.
-   Use a física da inércia. Entre eles estão os seguintes:
    -   Desaceleração: Ocorre quando o usuário para de apertar ou alongando. Isso é semelhante a deslizar até parar em uma superfície escorregadia.
    -   Elástico: Um pequeno efeito de retorno ocorre quando uma restrição de tamanho ou o limite é passado.
-   Posicione os controles de acordo com as [Diretrizes de direcionamento](guidelines-for-targeting.md).
-   Forneça alças de dimensionamento para redimensionamento restrito. O redimensionamento isométrico ou proporcional será o padrão se as alças não forem especificadas.
-   Não use o zoom para navegar pela interface do usuário ou expor controles adicionais no seu aplicativo. Em vez disso, use uma região de movimento panorâmico. Para saber mais sobre movimento panorâmico, veja [Diretrizes de movimento panorâmico](guidelines-for-panning.md).
-   Não coloque objetos redimensionáveis em uma área de conteúdo redimensionável. Algumas exceções:
    -   Aplicativos de desenho em que itens redimensionáveis podem aparecer em uma tela ou quadro redimensionável.
    -   Páginas da Web com um objeto incorporado, como um mapa.

    **Observação**    em todos os casos, a área de conteúdo é redimensionada, a menos que todos os pontos de toque estão dentro do objeto redimensionável.

     

## <a name="related-articles"></a>Artigos relacionados


**Exemplos**
* [Exemplo de entrada básico](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Exemplo de entrada de baixa latência](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Amostra do modo de interação do usuário](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Amostra de elementos visuais de foco](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**Amostras de arquivo-morto**
* [Entrada: Exemplo de eventos de entrada do usuário XAML](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [Entrada: Exemplo de recursos do dispositivo](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrada: Exemplo de teste de hit de toque](https://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML de rolagem, movimento panorâmico e zoom de exemplo](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrada: Exemplo simplificado de tinta](https://go.microsoft.com/fwlink/p/?linkid=246570)
* [Entrada: Exemplo de gestos do Windows 8](https://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Entrada: Manipulações e exemplo de gestos (C++)](https://go.microsoft.com/fwlink/p/?linkid=231605)
* [Exemplo de entrada de toque do DirectX](https://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 




