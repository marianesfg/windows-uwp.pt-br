---
title: Introdução a texturas
description: Um recurso de textura é uma estrutura de dados para armazenar texels, que são a unidade menor de uma textura que pode ser lida ou gravada. Quando a textura é lida por um sombreador, é possível filtrá-la por amostras de texturas.
ms.assetid: 6F3C76A8-F762-4296-AE02-BFBD6476A5A8
keywords:
- Introdução a texturas
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3cd5ca66635b57b79c2fca3e6ff10b8debb43fd0
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8693933"
---
# <a name="introduction-to-textures"></a>Introdução a texturas


Um recurso de textura é uma estrutura de dados para armazenar texels, que são a unidade menor de uma textura que pode ser lida ou gravada. Quando a textura é lida por um sombreador, é possível filtrá-la por amostras de texturas.

Um recurso de textura é uma coleção estruturada de dados projetada para armazenar texels. Um texel representa a menor unidade de uma textura que pode ser lida ou gravada pelo pipeline. Diferente dos buffers, as texturas podem ser filtradas por amostras de texturas à medida que são lidas por unidades de sombreador. O tipo de textura afeta como a textura é filtrada. Cada texel contém 1 a 4 componentes, organizados em um dos formatos DXGI definidos pela enumeração DXGI\_FORMAT.

As texturas são criadas como um recurso estruturado de tamanho conhecido. Entretanto, cada textura pode ter tipos ou não quando o recurso é criado, contanto que o tipo seja especificado usando um modo de exibição quando a textura está vinculada ao pipeline.

## <a name="span-idtexturetypesspanspan-idtexturetypesspanspan-idtexturetypesspantexture-types"></a><span id="Texture_Types"></span><span id="texture_types"></span><span id="TEXTURE_TYPES"></span>Tipo de textura


O Direct3D oferece suporte a várias representações de ponto flutuante. Todos os cálculos de ponto flutuante operam em um subconjunto definido de regras de ponto flutuante de precisão única IEEE 754 de 32 bits.

Existem vários tipos de texturas: 1D, 2D, 3D, e cada uma pode ser criada com ou sem mipmaps. O Direct3D também oferece suporte a matrizes de textura e texturas multisample.

-   [Texturas 1D](#texture1d-resource)
-   [Matrizes de textura 1D](#texture1d-array-resource)
-   [Texturas 2D e matrizes de textura 2D](#texture2d-resource)
-   [Texturas 3D](#texture3d-resource)

### <a name="span-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1d-resourcespan1d-textures"></a><span id="Texture1D_Resource"></span><span id="texture1d_resource"></span><span id="TEXTURE1D_RESOURCE"></span><span id="texture1d-resource"></span>Texturas 1D

Uma textura 1D em sua forma mais simples contém dados de textura que podem ser tratados com uma coordenada de textura única; pode ser visualizada como uma matriz de texels, conforme mostrado na ilustração a seguir.

A ilustração a seguir mostra um exemplo de textura 1D:

![uma textura 1d](images/d3d10-1d-texture.png)

Cada texel contém um número de componentes de cor dependendo do formato dos dados armazenados. Ao adicionar mais complexidade, você pode criar uma textura 1D com níveis de mipmap, conforme mostrado na ilustração a seguir.

![uma textura 1D com níveis de mipmap](images/d3d10-resource-texture1d.png)

Um nível de mipmap é uma textura menor do que o nível superior por uma potência de dois. O nível superior é o mais detalhado, sendo que cada nível subsequente é menor. Para uma mipmap 1D, o menor nível contém um texel. Além disso, os níveis MIP sempre são reduzidos até 1:1.

Quando mipmaps são gerados para uma textura de tamanho ímpar, o próximo nível inferior tem sempre o mesmo tamanho (exceto quando o nível mais baixo atinge 1). Por exemplo, o diagrama ilustra uma textura de 5 x 1, cujo próximo nível mais baixo é uma textura de 2 x 1, cujo próximo (e último) nível de mip é uma textura tamanho 1 x 1. Os níveis são identificados por um índice denominado LOD (nível de detalhe) utilizado para acessar a textura menor ao renderizar uma geometria que não está próxima da câmera.

### <a name="span-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1d-array-resourcespan1d-texture-arrays"></a><span id="Texture1D_Array_Resource"></span><span id="texture1d_array_resource"></span><span id="TEXTURE1D_ARRAY_RESOURCE"></span><span id="texture1d-array-resource"></span>Matrizes de textura 1D

O Direct3D também oferece suporte às matrizes de texturas. Uma matriz de texturas 1D conceitualmente parece com a ilustração a seguir.

![uma matriz de texturas 1D](images/d3d10-resource-texture1darray.png)

Essa matriz de textura contém três texturas. Cada uma das três texturas tem uma largura de 5 (o número de elementos na primeira camada). Cada textura também contém uma mipmap de 3 camadas.

Todas as matrizes de textura no Direct3D são uma matriz homogênea de texturas; ou seja, cada textura em uma matriz de texturas deve ter o mesmo formato de dados e o tamanho (incluindo a largura de textura e o número de níveis de mipmap. Você pode criar matrizes de textura de diferentes tamanhos, contanto que todas as texturas em cada matriz correspondam em tamanho.

### <a name="span-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2d-resourcespan2d-textures-and-2d-texture-arrays"></a><span id="Texture2D_Resource"></span><span id="texture2d_resource"></span><span id="TEXTURE2D_RESOURCE"></span><span id="texture2d-resource"></span>Texturas 2D e matrizes de textura 2D

Um recurso Texture2D contém uma grade 2D de texels. Cada texel é endereçável por um vetor u, v. Como é um recurso de textura, ele pode conter níveis de mipmap e sub-recursos. Um recurso de textura 2D totalmente preenchida é semelhante à ilustração a seguir.

![um recurso de textura 2d](images/d3d10-resource-texture2d.png)

Esse recurso de textura contém uma única textura 3 x 5 com três níveis de mipmap.

Um recurso de matriz de textura 2D é uma matriz homogênea de texturas 2D; ou seja, cada textura tem o mesmo formato de dados e dimensões (incluindo níveis de mipmap). Contém um layout semelhante ao da matriz de textura 1D, exceto que as texturas agora contêm dados 2D, como mostrado na ilustração a seguir.

![uma matriz de texturas 2d](images/d3d10-resource-texture2darray.png)

Essa matriz de textura contém três texturas; cada uma de 3 x 5 com dois níveis de mipmap.

### <a name="span-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanusing-a-2d-texture-array-as-a-texture-cube"></a><span id="Texture2DArray_Resource_as_a_Texture_Cube"></span><span id="texture2darray_resource_as_a_texture_cube"></span><span id="TEXTURE2DARRAY_RESOURCE_AS_A_TEXTURE_CUBE"></span>Usar uma matriz de textura 2D como um cubo de textura

Um cubo de textura é uma matriz de textura 2D com 6 texturas, uma para cada face do cubo. Um cubo de textura totalmente preenchida se parece com a ilustração a seguir.

![uma matriz de texturas 2d que representa um cubo de textura](images/d3d10-resource-texturecube.png)

Uma matriz de textura 2D com 6 texturas pode ser lida nos sombreadores com as funções intrínsecas do mapa do cubo após a vinculação ao pipeline com um modo de exibição de textura de cubo. Os cubos de textura são endereçados do sombreador com um vetor 3D apontado para o centro do cubo de textura.

### <a name="span-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3d-resourcespan3d-textures"></a><span id="Texture3D_Resource"></span><span id="texture3d_resource"></span><span id="TEXTURE3D_RESOURCE"></span><span id="texture3d-resource"></span>Texturas 3D

Um recurso de textura 3D (também conhecida como uma textura de volume) contém um volume 3D de texels. Como é um recurso de textura, ela pode conter níveis de mipmap. Uma textura 3D totalmente preenchida se parece com a ilustração a seguir.

![um recurso de textura 3d](images/d3d10-resource-texture3d.png)

Quando uma fatia de mipmap da textura 3D é vinculada como uma saída de destino de renderização (com um modo de exibição de renderização de destino), a textura 3D se comporta de forma idêntica à matriz de textura 2D com n fatias. A fatia de renderização específica é escolhida no estágio do sombreador de geometria.

Não há nenhum conceito de matriz de textura 3D; portanto, um sub-recurso de textura 3D é um nível de mipmap único.

Os sistemas de coordenadas para Direct3D são definidos para pixels e texels.

## <a name="span-idpixelspanspan-idpixelspanspan-idpixelspanpixel-coordinate-system"></a><span id="Pixel"></span><span id="pixel"></span><span id="PIXEL"></span>Sistema de coordenadas de pixels


O sistema de coordenadas de pixels no Direct3D define a origem de um destino de renderização no canto superior esquerdo, conforme mostrado no diagrama a seguir. Os centros de pixels são separados por (0,5f;0,5f) de locais de números inteiros.

![diagrama do sistema de coordenadas de pixels no direct3d 10](images/d3d10-coordspix10.png)

## <a name="span-idtexelspanspan-idtexelspanspan-idtexelspantexel-coordinate-system"></a><span id="Texel"></span><span id="texel"></span><span id="TEXEL"></span>Sistema de coordenadas de texel


O sistema de coordenadas de texel é originado no canto superior esquerdo da textura, conforme mostrado no diagrama a seguir. Isso facilita a renderização de texturas alinhadas à tela, pois o sistema de coordenadas de pixels está alinhado ao sistema de coordenadas de texel.

![diagrama do sistema de coordenadas de texel](images/d3d10-coordstex10.png)

As coordenadas de textura são representadas por um número normalizado ou dimensionado; cada coordenada de textura é mapeada de acordo com um texel específico da seguinte maneira:

Para uma coordenada normalizada:

-   Amostragem de ponto: Texel \# = base(U \* largura)
-   Amostragem linear: Texel esquerdo \# = base(U \* largura), Texel direito \# = Texel esquerdo \# + 1

Para uma coordenada dimensionada:

-   Amostragem de pontos: Texel \# = piso(U)
-   Amostragem linear: Texel esquerdo \# = base(U - 0,5), Texel direito \# = Texel esquerdo \# + 1

Onde a largura é a largura da textura (em texels).

O encapsulamento do endereço da textura ocorre após o cálculo da localização de texel.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Texturas](textures.md)
