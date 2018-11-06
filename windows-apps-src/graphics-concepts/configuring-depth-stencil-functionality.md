---
title: Configurando a funcionalidade de estêncil de profundidade
description: Esta seção abrange as etapas para configurar o buffer de estêncil de profundidade e o estado de estêncil de profundidade para o estágio de fusão de saída.
ms.assetid: B3F6CDAA-93ED-4DC1-8E69-972C557C7920
keywords:
- Configurando a funcionalidade de estêncil de profundidade
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3c15ff6aa43540b61b410525e6bb20a0de3da821
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6047664"
---
# <a name="span-iddirect3dconceptsconfiguringdepth-stencilfunctionalityspanconfiguring-depth-stencil-functionality"></a><span id="direct3dconcepts.configuring_depth-stencil_functionality"></span>Configurando a funcionalidade de estêncil de profundidade


Esta seção abrange as etapas para configurar o buffer de estêncil de profundidade e o estado de estêncil de profundidade para o estágio de fusão de saída.

Assim que você souber como usar o buffer de estêncil de profundidade e o estado de estêncil de profundidade correspondente, consulte [técnicas avançadas de estêncil](#advanced-stencil-techniques).

## <a name="span-idcreatedepthstencilstatespanspan-idcreatedepthstencilstatespanspan-idcreatedepthstencilstatespancreate-depth-stencil-state"></a><span id="Create_Depth_Stencil_State"></span><span id="create_depth_stencil_state"></span><span id="CREATE_DEPTH_STENCIL_STATE"></span>Criar estado de estêncil de profundidade


O estado de estêncil de profundidade informa o estágio de fusão de saída sobre como realizar o [teste de profundidade e estêncil](https://msdn.microsoft.com/library/windows/desktop/bb205120). O teste de profundidade e estêncil determina se um determinado pixel deve ser desenhado.

## <a name="span-idbinddepthstenciltotheomstagespanspan-idbinddepthstenciltotheomstagespanspan-idbinddepthstenciltotheomstagespanbind-depth-stencil-data-to-the-om-stage"></a><span id="Bind_Depth_Stencil_to_the_OM_Stage"></span><span id="bind_depth_stencil_to_the_om_stage"></span><span id="BIND_DEPTH_STENCIL_TO_THE_OM_STAGE"></span>Associar dados de estêncil de profundidade ao estágio de OM


Associar o estado de estêncil de profundidade.

Associe o recurso de estêncil de profundidade usando um modo de exibição.

Os destinos de renderização devem todos ser do mesmo tipo de recurso. Se a suavização de várias amostras for usada, todos os destinos de renderização associados e buffers de profundidade devem ter as mesmas contagens de amostra.

Quando um buffer é usado como um destino de renderização, os testes de estêncil de profundidade e os destinos de renderização múltiplos não são permitidos.

-   Até 8 destinos de renderização podem ser vinculados ao mesmo tempo.
-   Todos os destinos de renderização devem ter o mesmo tamanho em todas as dimensões (largura, altura, e profundidade para 3D ou tamanho de matriz para os tipos \*Array).
-   Cada destino de renderização pode ter um formato de dados diferente.
-   As máscaras de gravação controlam quais dados são gravados em um destino de renderização. As máscaras de gravação de saída controlam, a um nível de componente e de destino de renderização, quais dados são gravados nos destinos de renderização.

## <a name="span-idadvancedstenciltechniquesspanspan-idadvancedstenciltechniquesspanspan-idadvancedstenciltechniquesspanspan-idadvanced-stencil-techniquesspanadvanced-stencil-techniques"></a><span id="Advanced_Stencil_Techniques"></span><span id="advanced_stencil_techniques"></span><span id="ADVANCED_STENCIL_TECHNIQUES"></span><span id="advanced-stencil-techniques"></span>Técnicas avançadas de estêncil


A parte de estêncil do buffer de estêncil de profundidade pode ser usada para criar efeitos de renderização como composição, decalque e contornos.

-   [Composição](#compositing)
-   [Decalque](#decaling)
-   [Contornos e silhuetas](#outlines-and-silhouettes)
-   [Estêncil de dois lados](#two-sided-stencil)
-   [Ler o buffer de estêncil de profundidade como uma textura](#reading-the-depth-stencil-buffer-as-a-texture)

### <a name="span-idcompositingspanspan-idcompositingspanspan-idcompositingspancompositing"></a><span id="Compositing"></span><span id="compositing"></span><span id="COMPOSITING"></span>Composição

Seu app pode usar o buffer de estêncil para compor imagens 2D ou 3D em uma cena 3D. Uma máscara no buffer de estêncil é usada para ocultar uma área da superfície de destino de renderização. Informações armazenadas 2D, como texto ou bitmaps, podem então ser escritas na área obstruída. Como alternativa, seu app pode renderizar objetos primitivos 3D adicionais para a região mascarada por estêncil da superfície do destino de renderização. Ele pode até mesmo renderizar uma cena inteira.

Jogos geralmente são compostos por diversas cenas 3D juntas. Por exemplo, jogos de direção normalmente exibem um espelho retrovisor. O espelho contém o modo de exibição da cena 3D atrás do condutor. Ele é essencialmente uma segunda cena 3D composta pela visão frontal do condutor.

### <a name="span-iddecalingspanspan-iddecalingspanspan-iddecalingspandecaling"></a><span id="Decaling"></span><span id="decaling"></span><span id="DECALING"></span>Decalque

Aplicativos Direct3D usam decalque para controlar quais pixels de uma determinada imagem primitiva são desenhados na superfície de destino de renderização. Os apps aplicam os decalques às imagens de objetos primitivos para permitir que polígonos coplanares sejam renderizados corretamente.

Por exemplo, ao aplicar marcas de pneus e linhas amarelas em uma estrada, as marcas devem aparecer diretamente sobre a pista. No entanto, os valores z das marcas e da estrada são os mesmos. Portanto, o buffer de profundidade pode não produzir uma separação clara entre os dois. Alguns pixels da parte primitiva traseira podem ser renderizados na parte superior da parte primitiva frontal e vice-versa. A imagem resultante parece tremida de quadro a quadro. Esse efeito é chamado luta z ou flimmering.

Para resolver esse problema, use um estêncil para mascarar a seção da parte traseira primitiva onde aparecerá o decalque. Desligue o buffer z e renderize a imagem da parte primitiva dianteira na área sem máscara da superfície de destino de renderização.

Várias mesclagens de textura podem ser usadas para resolver esse problema.

### <a name="span-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanspan-idoutlines-and-silhouettesoutlines-and-silhouettes"></a><span id="Outlines_and_Silhouettes"></span><span id="outlines_and_silhouettes"></span><span id="OUTLINES_AND_SILHOUETTES"></span><span id="outlines-and-silhouettes">Contornos e silhuetas

Você pode usar o buffer de estêncil para efeitos mais abstratos, como contornos e silhuetas.

Se seu app realiza duas passagens de renderização - uma para gerar a máscara de estêncil e a segunda para aplicar a máscara de estêncil à imagem, mas com os primitivas um pouco menores na segunda passagem - a imagem resultante conterá apenas contorno da primitiva. O app pode preencher a área com máscara de estêncil da imagem com uma cor sólida, dando à primitiva uma aparência em alto-relevo.

Se a máscara de estêncial tiver o mesmo tamanho e formato da primitiva que está sendo renderizado, a imagem resultante terá um buraco onde o primitivo deveria estar. Seu app poderá preencher o buraco com cor preta para produzir uma silhueta da primitiva.

### <a name="span-idtwosidedstencilspanspan-idtwosidedstencilspanspan-idtwosidedstencilspantwo-sided-stencil"></a><span id="Two_Sided_Stencil"></span><span id="two_sided_stencil"></span><span id="TWO_SIDED_STENCIL"></span>Estêncil de dois lados

Os volumes de sombra são usados para desenhar sombras com o buffer de estêncil. O app calcula os volumes de sombra convertidos sobrepondo a geometria, calculando as bordas da silhueta e afastando-as da luz em um conjunto de volumes 3D. Em seguida, esses volumes são renderizados duas vezes no buffer de estêncil.

A primeira renderização desenha polígonos voltados para frente e aumenta os valores de buffer de estêncil. A segunda renderização desenha polígonos voltados para trás do volume de sombra e diminui os valores de buffer de estêncil.

Normalmente, todos os valores incrementados e diminuídos cancelam uns aos outros. No entanto, a cena já foi renderizada com geometria normal, fazendo com que alguns pixels falhem no teste de buffer z, conforme o volume de sombra é renderizado. Os valores restantes no buffer de estêncil correspondem aos pixels que estão na sombra. Esse conteúdo restante do buffer de estêncil é usado como uma máscara, para fazer a combinação alfa de um quadrupleto preto grande e abrangente na cena. Com o buffer de estêncil atuando como uma máscara, o resultado será o escurecimento dos pixels que estão nas sombras.

Isso significa que a geometria de sombra é desenhada duas vezes por fonte de luz, exercendo assim pressão sobre a taxa de transferência de vértice da GPU. O recurso de estêncil de dois lados foi projetado para atenuar essa situação. Nesta abordagem, existem dois conjuntos de estado de estêncil (nomeados abaixo), um conjunto para os triângulos voltados para a frente e outro para os triângulos voltados para trás. Dessa forma, somente uma única passagem é desenhada por volume de sombra, por luz.

### <a name="span-idreadingthedepth-stencilbufferasatexturespanspan-idreadingthedepth-stencilbufferasatexturespanspan-idreadingthedepth-stencilbufferasatexturespanspan-idreading-the-depth-stencil-buffer-as-a-texturespanreading-the-depth-stencil-buffer-as-a-texture"></a><span id="Reading_the_Depth-Stencil_Buffer_as_a_Texture"></span><span id="reading_the_depth-stencil_buffer_as_a_texture"></span><span id="READING_THE_DEPTH-STENCIL_BUFFER_AS_A_TEXTURE"></span><span id="reading-the-depth-stencil-buffer-as-a-texture"></span>Ler o buffer de estêncil de profundidade como uma textura

Um buffer de estêncil de profundidade inativo pode ser lido por um sombreador como uma textura. Um app que lê um buffer de estêncil de profundidade como uma textura renderiza em duas passagens, a primeira passagem grava o buffer de estêncil de profundidade e a segunda passagem lê do buffer. Isso permite que um sombreador compare os valores de profundidade ou estêncil gravados anteriormente no buffer com o valor do pixel que está sendo renderizado atualmente. O resultado da comparação pode ser usado para criar efeitos, como o mapeamento de sombra ou partículas suaves em um sistema de partículas.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Estágio de fusão de saída (OM)](output-merger-stage--om-.md)

[Pipeline de elementos gráficos](graphics-pipeline.md)

[Estágio de fusão de saída](https://msdn.microsoft.com/library/windows/desktop/bb205120)
