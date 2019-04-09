---
title: Buffers de estêncil
description: O buffer de estêncil é usado para mascarar pixels em uma imagem, para gerar efeitos especiais.
ms.assetid: 544B3B9E-31E3-41DA-8081-CC3477447E94
keywords:
- Buffers de estêncil
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bd59c1d32b4f09b58b7e78281e468fbb00a777d9
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291874"
---
# <a name="stencil-buffers"></a>Buffers de estêncil

Um *buffer de estêncil* é usado para mascarar pixels em uma imagem, para gerar efeitos especiais. A máscara controla se o pixel é desenhado ou não. Esses efeitos especiais incluem composição, decalque, desintegração, esmaecimento, deslizar o dedo, contornos, silhuetas, e estêncil de dois lados. Alguns dos efeitos mais comuns são mostrados abaixo.

O buffer de estêncil habilita ou desabilita o desenho na superfície de destino de renderização em uma base de pixel por pixel. No nível mais fundamental, ele permite que aplicativos mascarem seções da imagem renderizada para que elas não sejam exibidas. Aplicativos geralmente usam buffers de estêncil de efeitos especiais, como desintegração, decalque e contorno.

Informações de buffer de estêncil são incorporadas nos dados do buffer z.

## <a name="span-idhowthestencilbufferworksspanspan-idhowthestencilbufferworksspanspan-idhowthestencilbufferworksspanhow-the-stencil-buffer-works"></a><span id="How_the_Stencil_Buffer_Works"></span><span id="how_the_stencil_buffer_works"></span><span id="HOW_THE_STENCIL_BUFFER_WORKS"></span>Como funciona o buffer de estêncil

O Direct3D realiza um teste no conteúdo do buffer de estêncil em uma base de pixel por pixel. Para cada pixel na superfície do destino, ele executa um teste usando o valor correspondente no buffer de estêncil, um valor de referência de estêncil e um valor de máscara de estêncil. Se o teste for aprovado, o Direct3D executa uma ação. O teste é realizado usando as seguintes etapas.

1.  Execute uma operação AND bit a bit do valor de referência do estêncil com a máscara de estêncil.
2.  Execute uma operação AND bit a bit do valor de buffer do estêncil do pixel atual com a máscara de estêncil.
3.  Compare o resultado da etapa 1 com o resultado da etapa 2, usando a função de comparação.

As etapas acima são mostradas na seguinte linha de código:

```cpp
(StencilRef & StencilMask) CompFunc (StencilBufferValue & StencilMask)
```

-   StencilRef representa o valor de referência de estêncil.
-   StencilMask representa o valor da máscara de estêncil.
-   CompFunc é a função de comparação.
-   StencilBufferValue é o conteúdo do buffer de estêncil do pixel atual.
-   O símbolo de E comercial (&) representa a operação AND bit a bit.

O pixel atual é escrito na superfície de destino se o teste de estêncil for aprovado, caso contrário, é ignorado. O comportamento padrão de comparação é a gravação do pixel, independentemente de como cada operação bit a bit acontece. Você pode alterar esse comportamento alterando o valor de um tipo enumerado para identificar a função de comparação desejada.

Seu aplicativo pode personalizar a operação do buffer de estêncil. Ele pode definir a função de comparação, a máscara de estêncil e o valor de referência de estêncil. Ele também pode controlar a ação que o Direct3D executa quando o teste de estêncil passa ou falha.

## <a name="span-idcompositingspanspan-idcompositingspanspan-idcompositingspancompositing"></a><span id="Compositing"></span><span id="compositing"></span><span id="COMPOSITING"></span>Composição


Seu app pode usar o buffer de estêncil para compor imagens 2D ou 3D em uma cena 3D. Uma máscara no buffer de estêncil é usada para ocultar uma área da superfície de destino de renderização. Informações armazenadas 2D, como texto ou bitmaps, podem então ser escritas na área obstruída. Como alternativa, seu app pode renderizar objetos primitivos 3D adicionais para a região mascarada por estêncil da superfície do destino de renderização. Ele pode até mesmo renderizar uma cena inteira.

Jogos geralmente são compostos por diversas cenas 3D juntas. Por exemplo, jogos de direção normalmente exibem um espelho retrovisor. O espelho contém o modo de exibição da cena 3D atrás do condutor. Ele é essencialmente uma segunda cena 3D composta pela visão frontal do condutor.

## <a name="span-iddecalingspanspan-iddecalingspanspan-iddecalingspandecaling"></a><span id="Decaling"></span><span id="decaling"></span><span id="DECALING"></span>Decaling


Aplicativos Direct3D usam decalque para controlar quais pixels de uma determinada imagem primitiva são desenhados na superfície de destino de renderização. Os apps aplicam os decalques às imagens de objetos primitivos para permitir que polígonos coplanares sejam renderizados corretamente.

Por exemplo, ao aplicar marcas de pneus e linhas amarelas em uma estrada, as marcas devem aparecer diretamente sobre a pista. No entanto, os valores z das marcas e da estrada são os mesmos. Portanto, o buffer de profundidade pode não produzir uma separação clara entre os dois. Alguns pixels da parte primitiva traseira podem ser renderizados na parte superior da parte primitiva frontal e vice-versa. A imagem resultante parece tremida de quadro a quadro. Esse efeito é chamado *luta z* ou *flimmering*.

Para resolver esse problema, use um estêncil para mascarar a seção da parte traseira primitiva onde aparecerá o decalque. Desligue o buffer z e renderize a imagem da parte primitiva dianteira na área sem máscara da superfície de destino de renderização.

Embora diversas mesclagens de textura possam ser usadas para resolver esse problema, fazer isso limitará o número de outros efeitos especiais que seu aplicativo pode produzir. Usar o buffer de estêncil para aplicar os decalques libera estágios de mesclagem de textura para outros efeitos.

## <a name="span-iddissolvesfadesandswipesspanspan-iddissolvesfadesandswipesspanspan-iddissolvesfadesandswipesspandissolves-fades-and-swipes"></a><span id="Dissolves__fades__and_swipes"></span><span id="dissolves__fades__and_swipes"></span><span id="DISSOLVES__FADES__AND_SWIPES"></span>Dissolver, fades e dedo


Cada vez mais, aplicativos empregam efeitos especiais que são comumente usados em filmes e vídeo, como desintegração, passagem de dedo e desvanecimento.

Em uma desintegração, uma imagem é gradualmente substituída por outra em uma sequência de quadros suavizada. Embora o Direct3D forneça métodos de utilizar várias mesclagens de textura para conseguir o mesmo efeito, os aplicativos que utilizam o buffer de estêncil para desintegrações podem usar recursos de mesclagem de textura para outros efeitos enquanto eles fazem uma desintegração.

Quando seu aplicativo executa uma desintegração, ele deve renderizar duas imagens diferentes. Ele usa o buffer de estêncil para controlar quais pixels de cada imagem são desenhadas na superfície de destino de renderização. Você pode definir uma série de máscaras de estêncil e copiá-las para o buffer de estêncil em quadros sucessivos. Como alternativa, você pode definir uma máscara de estêncil de base para o primeiro quadro e alterá-la de forma incremental.

No início da desintegração, seu aplicativo define a função de estêncil e máscara de estêncil para que a maioria dos pixels da imagem inicial passem no teste de estêncil. A maioria dos pixels da imagem final deve falhar no teste de estêncil. Em quadros sucessivos, a máscara de estêncil é atualizada para que cada vez menos pixels da imagem inicial passem no teste. À medida que os quadros progridem, cada vez menos pixels da imagem final falham no teste. Dessa forma, seu aplicativo pode executar uma desintegração usando qualquer padrão arbitrário de desintegração.

O desvanecimento para dentro ou para fora é um caso especial de desintegração. Ao desvanecer para dentro, o buffer de estêncil é usado para desintegrar a partir de uma imagem preta ou branca para uma renderização de uma cena 3D. O desvanecimento para fora é o oposto, seu aplicativo é iniciado com uma renderização de uma cena 3D e se desintegra em preto e branco. O desvanecimento pode ser feito usando qualquer padrão arbitrário que você deseja empregar.

Os aplicativos Direct3D usam uma técnica semelhante para as passagens de dedo. Por exemplo, quando um aplicativo realiza uma passagem de dedo da esquerda para a direita, a imagem final parece deslizar gradualmente na parte superior da imagem inicial da esquerda para a direita. Assim como em uma desintegração, você deve definir uma série de máscaras de estêncil que são carregadas no buffer de estêncil em quadros sucessivos, ou modificar sucessivamente a máscara de estêncil inicial. As máscaras de estêncil são usadas para desativar a gravação de pixels da imagem inicial e para habilitar a gravação de pixels da imagem final.

Uma passagem de dedo é um pouco mais complexa do que uma desintegração em que seu aplicativo deve ler os pixels da imagem final na ordem inversa da passagem do dedo. Ou seja, se a passagem do dedo acontecer da esquerda para a direita, seu aplicativo deverá ler pixels da imagem final da direita para a esquerda.

## <a name="span-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanoutlines-and-silhouettes"></a><span id="Outlines_and_silhouettes"></span><span id="outlines_and_silhouettes"></span><span id="OUTLINES_AND_SILHOUETTES"></span>Contornos e apenas


Você pode usar o buffer de estêncil para efeitos mais abstratos, como contornos e silhuetas.

Se seu aplicativo aplicar uma máscara de estêncil à imagem de um objeto primitivo com o mesmo formato, mas um pouco menor, a imagem resultante terá apenas o contorno do objeto primitivo. O app pode preencher a área com máscara de estêncil da imagem com uma cor sólida, dando à primitiva uma aparência em alto-relevo.

Se a máscara de estêncial tiver o mesmo tamanho e formato da primitiva que está sendo renderizado, a imagem resultante terá um buraco onde o primitivo deveria estar. Seu app poderá preencher o buraco com cor preta para produzir uma silhueta da primitiva.

## <a name="span-idtwo-sidedstencilspanspan-idtwo-sidedstencilspanspan-idtwo-sidedstencilspantwo-sided-stencil"></a><span id="Two-sided_stencil"></span><span id="two-sided_stencil"></span><span id="TWO-SIDED_STENCIL"></span>Estêncil de dois lados


Os volumes de sombra são usados para desenhar sombras com o buffer de estêncil. O app calcula os volumes de sombra convertidos sobrepondo a geometria, calculando as bordas da silhueta e afastando-as da luz em um conjunto de volumes 3D. Em seguida, esses volumes são renderizados duas vezes no buffer de estêncil.

A primeira renderização desenha polígonos voltados para frente e aumenta os valores de buffer de estêncil. A segunda renderização desenha polígonos voltados para trás do volume de sombra e diminui os valores de buffer de estêncil. Normalmente, todos os valores incrementados e diminuídos cancelam uns aos outros. No entanto, a cena já foi renderizada com geometria normal, fazendo com que alguns pixels falhem no teste de buffer z, conforme o volume de sombra é renderizado. Os valores restantes no buffer de estêncil correspondem aos pixels que estão na sombra. Esse conteúdo restante do buffer de estêncil é usado como uma máscara, para fazer a combinação alfa de um quadrupleto preto grande e abrangente na cena. Com o buffer de estêncil atuando como uma máscara, o resultado será o escurecimento dos pixels que estão nas sombras.

Isso significa que a geometria de sombra é desenhada duas vezes por fonte de luz, exercendo assim pressão sobre a taxa de transferência de vértice da GPU. O recurso de estêncil de dois lados foi projetado para atenuar essa situação. Nesta abordagem, existem dois conjuntos de estado de estêncil (nomeados abaixo), um conjunto para os triângulos voltados para a frente e outro para os triângulos voltados para trás. Dessa forma, somente uma única passagem é desenhada por volume de sombra, por luz.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Buffers de profundidade e estêncil](depth-and-stencil-buffers.md)

 

 




