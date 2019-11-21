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
ms.openlocfilehash: fbcb4510a5b3ecca80b388172fe30028ac511452
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257985"
---
# <a name="optical-zoom-and-resizing"></a>Zoom óptico e redimensionamento



Este artigo descreve o zoom e o redimensionamento de elementos do Windows e fornece as diretrizes da experiência do usuário para o uso desses mecanismos de interação em seus aplicativos.

> **APIs importantes**: [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Input (XAML)** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

O zoom óptico permite que os usuários ampliem o modo de exibição do conteúdo em uma área de conteúdo (isso é executado na própria área de conteúdo), enquanto o redimensionamento permite que alterem o tamanho relativo de um ou mais objetos sem alterar o modo de exibição da área de conteúdo (isso é executado em objetos na área de conteúdo).

As interações de zoom óptico e de redimensionamento são realizadas com os gestos de pinçagem e ampliação (afastar os dedos aumenta o zoom e aproximar os dedos reduz o zoom) ou mantendo a tecla Ctrl pressionada e girando a roda de rolagem do mouse ou mantendo a tecla Ctrl pressionada (com a tecla Shift caso o teclado numérico não esteja disponível) e pressionando a tecla mais (+) ou menos (-).

Os diagramas a seguir demonstram as diferenças entre redimensionamento e zoom óptico.

**Zoom óptico**: o usuário seleciona uma área e amplia a área inteira.

![juntar os dedos aumenta o zoom e afastar os dedos o diminui](images/areazoom.png)

**Redimensionar**: o usuário seleciona um objeto em uma área e redimensiona esse objeto.

![juntar os dedos reduz um objeto e separá-los o aumenta](images/objectresize.png)

**Observe**   zoom óptico não deve ser confundido com o [zoom semântico](../controls-and-patterns/semantic-zoom.md). Embora essas interações compartilhem os mesmos gestos, o zoom semântico refere-se à apresentação e à navegação de conteúdo organizado em um único modo de exibição (como a estrutura de pastas de um computador, uma biblioteca de documentos ou um álbum de fotografias).

 

## <a name="dos-and-donts"></a>O que fazer e o que não fazer


Use as diretrizes a seguir para aplicativos compatíveis com redimensionamento ou zoom óptico:

-   Se forem definidas restrições ou limites de tamanho máximo e mínimo, use o retorno visual para demonstrar quando o usuário atingiu ou excedeu esses limites.
-   Use pontos de ajuste para influenciar o comportamento de zoom e de redimensionamento fornecendo pontos lógicos em que é necessário parar a manipulação e garantir que um subconjunto de conteúdo específico seja exibido no visor. Forneça pontos de ajuste para níveis de zoom comuns ou modos de exibição lógicos para que seja mais fácil para um usuário escolher esses níveis. Por exemplo, aplicativos de fotos podem fornecer um ponto de ajuste a 100% para o redimensionamento ou, no caso de aplicativos de mapas, os pontos de ajuste podem ser úteis para modos de exibição de cidade, estado e país.

    Pontos de ajuste permitem que os usuários sejam imprecisos e ainda atinjam suas metas. Se estiver usando XAML, veja as propriedades dos pontos de ajuste do [**ScrollViewer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer). Para JavaScript e HTML, use [ **-ms-content-zoom-snap-points**](https://msdn.microsoft.com/library/hh771895).

    Existem dois tipos de pontos de ajuste:

    -   Proximidade: depois que o contato for levantado, um ponto de ajuste será selecionado se a inércia parar dentro do limite de distância do ponto de ajuste. Os pontos de ajuste de proximidade ainda permitem que um zoom ou redimensionamento seja finalizado entre pontos de ajuste.
    -   Obrigatório: o ponto de alinhamento escolhido é aquele que antecede ou sucede imediatamente o último ponto de alinhamento cruzado antes que o contato seja retirado (dependendo da direção e da velocidade do gesto). Uma manipulação precisa ser finalizada em um ponto de ajuste obrigatório.
-   Use a física da inércia. Entre eles estão os seguintes:
    -   Desaceleração: ocorre quando o usuário para de pinçar ou ampliar. Isso é semelhante a deslizar até parar em uma superfície escorregadia.
    -   Salto: um efeito de leve salto ocorre quando uma restrição ou limite de tamanho é passada.
-   Posicione os controles de acordo com as [Diretrizes de direcionamento](guidelines-for-targeting.md).
-   Forneça alças de dimensionamento para redimensionamento restrito. O redimensionamento isométrico ou proporcional será o padrão se as alças não forem especificadas.
-   Não use o zoom para navegar pela interface do usuário ou expor controles adicionais no seu aplicativo. Em vez disso, use uma região de movimento panorâmico. Para saber mais sobre movimento panorâmico, veja [Diretrizes de movimento panorâmico](guidelines-for-panning.md).
-   Não coloque objetos redimensionáveis em uma área de conteúdo redimensionável. Algumas exceções:
    -   Aplicativos de desenho em que itens redimensionáveis podem aparecer em uma tela ou quadro redimensionável.
    -   Páginas da Web com um objeto incorporado, como um mapa.

    **Observe**   em todos os casos, a área de conteúdo é redimensionada, a menos que todos os pontos de toque estejam dentro do objeto redimensionável.

     

## <a name="related-articles"></a>Artigos relacionados


**Exemplos**
* [Amostra de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
* [Exemplo de entrada de baixa latência](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
* [Amostra do modo de interação do usuário](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
* [Amostra de elementos visuais de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

**Amostras de arquivo-morto**
* [Entrada: exemplo de eventos de entrada do usuário XAML](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
* [Entrada: exemplo de recursos do dispositivo](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
* [Entrada: exemplo de teste de colisão de toque](https://code.msdn.microsoft.com/windowsapps/Touch-Hit-Testing-sample-5e35c690)
* [Exemplo de rolagem, panorâmica e zoom do XAML](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
* [Entrada: exemplo de tinta simplificada](https://code.msdn.microsoft.com/windowsapps/Input-simplified-ink-sample-11614bbf)
* [Entrada: exemplo de gestos do Windows 8](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
* [Entrada: exemplo de manipulações e gestos (C++)](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
* [Exemplo de entrada do DirectX Touch](https://code.msdn.microsoft.com/windowsapps/Simple-Direct3D-Touch-f98db97e)
 

 




