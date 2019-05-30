---
title: Tipos de recurso
description: Diferentes tipos de recursos têm um layout distinto (ou volume de memória).
ms.assetid: BCDDF227-1837-44DA-ABD4-E39BCFF2B8EF
keywords:
- Tipos de recurso
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9712b4498b03460568d20d4c8e27172ad5c14360
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66362213"
---
# <a name="resource-types"></a>Tipos de recurso


Diferentes tipos de recursos têm um layout distinto (ou volume de memória). Todos os recursos usados pelo pipeline Direct3D derivam de dois tipos de recurso básicos: [buffers](#buffer-resources) e [texturas](#texture-resources). Um buffer é uma coleção de dados brutos (elementos); uma textura é uma coleção de texels (elementos de textura).

Há duas maneiras de especificar totalmente o layout (ou volume de memória) de um recurso:

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Item</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><span id="Typed"></span><span id="typed"></span><span id="TYPED"></span>Digitado</p></td>
<td align="left"><p>Especifique totalmente o tipo quando o recurso é criado.</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="Typeless"></span><span id="typeless"></span><span id="TYPELESS"></span>Sem especificação de tipo</p></td>
<td align="left"><p>Especifique totalmente o tipo quando o recurso é vinculado ao pipeline.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idbufferresourcesspanspan-idbufferresourcesspanspan-idbufferresourcesspanspan-idbuffer-resourcesspanbuffer-resources"></a><span id="Buffer_Resources"></span><span id="buffer_resources"></span><span id="BUFFER_RESOURCES"></span><span id="buffer-resources"></span>Recursos de buffer


Um recurso de buffer é uma coleção de dados completamente tipados; internamente, um buffer contém elementos. Um elemento é composto de 1 a 4 componentes. Exemplos de tipos de dados de elemento incluem: um valor de dados compactados (como R8G8B8A8), um único inteiro de 8 bits, quatro valores flutuantes de 32 bits. Esses tipos de dados são usados para armazenar dados, como um vetor de posição, um vetor normal, uma coordenada de textura em um buffer de vértice, um índice em um buffer de índice ou o estado do dispositivo.

Um buffer é criado como um recurso não estruturado. Como ele não é estruturado, um buffer não pode conter níveis de mipmap, não é filtrado quando lido e não pode conter várias amostras.

### <a name="span-idbuffertypesspanspan-idbuffertypesspanspan-idbuffertypesspanbuffer-types"></a><span id="Buffer_Types"></span><span id="buffer_types"></span><span id="BUFFER_TYPES"></span>Tipos de buffer

-   [Buffer de vértice](#vertex-buffer)
-   [Buffer de índices](#index-buffer)
-   [Buffer de constantes](#shader-constant-buffer)

### <a name="span-idvertexbufferspanspan-idvertexbufferspanspan-idvertexbufferspanspan-idvertex-bufferspanvertex-buffer"></a><span id="Vertex_Buffer"></span><span id="vertex_buffer"></span><span id="VERTEX_BUFFER"></span><span id="vertex-buffer"></span>Buffer de vértice

Um buffer é uma coleção de elementos; um buffer de vértices contém dados por vértice. O exemplo mais simples é um buffer de vértices que contém um tipo de dado, como dados de posição. Ele pode ser visualizado como na ilustração a seguir.

![ilustração de um buffer de vértices que contém dados de posição](images/d3d10-resources-single-element-vb2.png)

Com mais frequência, um buffer de vértices contém todos os dados necessários para especificar totalmente vértices 3D. Um exemplo disso pode ser um buffer de vértices que contém coordenadas de posição, normais e de textura por vértice. Esses dados geralmente são organizados como conjuntos de elementos por vértice, conforme mostrado na ilustração a seguir.

![ilustração de um buffer de vértices que contém dados de posição, normais e de textura](images/d3d10-vertex-buffer-element.png)

Esse buffer de vértices contém dados por vértice para oito vértices; cada vértice armazena três elementos (coordenadas de posição, normais e de textura). A posição e a normal são cada frequentemente especificadas usando três flutuações de 32 bits e coordenadas de textura usando duas flutuações de 32 bits.

Para acessar dados de um buffer de vértices, você precisa saber qual vértice acessar e estes outros parâmetros de buffer:

-   *Deslocamento* -o número de bytes desde o início do buffer para os dados para o primeiro vértice.
-   *BaseVertexLocation* -o número de bytes do deslocamento até o primeiro vértice usado pela chamada de desenho apropriada.

Antes de criar um buffer de vértices, você precisa definir seu layout criando um objeto de layout de entrada. Depois que o objeto de layout de entrada é criado, você deve associá-lo ao estágio de assembler de entrada (IA).

### <a name="span-idindexbufferspanspan-idindexbufferspanspan-idindexbufferspanspan-idindex-bufferspanindex-buffer"></a><span id="Index_Buffer"></span><span id="index_buffer"></span><span id="INDEX_BUFFER"></span><span id="index-buffer"></span>Buffer de índices

Um buffer de índice contém um conjunto sequencial de índices de 16 ou 32 bits; cada índice é usado para identificar um vértice em um buffer de vértices. Indexação é usar um buffer de índice com um ou mais buffers de vértices para fornecer dados ao estágio de IA. Um buffer de índice pode ser visualizado como na ilustração a seguir.

![ilustração de um buffer de índice](images/d3d10-index-buffer.png)

Os índices sequenciais armazenados em um buffer de índice estão localizados com os seguintes parâmetros:

-   *Deslocamento* -o número de bytes desde o início do buffer ao primeiro índice.
-   *StartIndexLocation* -o número de bytes do deslocamento até o primeiro vértice usado pela chamada de desenho apropriada.
-   *IndexCount* -o número de índices para renderizar.

Um buffer de índice pode unir várias faixas de linha ou de triângulo ([topologias primitivas](primitive-topologies.md)) separando cada uma com um índice de recorte de faixa. Um índice de recorte de faixa permite várias faixas de linhas ou de triângulos serem desenhadas com uma chamada de desenho único. Um índice de recorte de faixa é o valor máximo possível para o índice (0xffff para um índice de 16 bits, 0xffffffff para um índice de 32 bits). O índice de recorte de faixa redefine o sentido de giro em primitivos indexados e pode ser usado para eliminar a necessidade de triângulos degenerados que caso contrário, seriam necessários para manter o sentido correto de giro em uma faixa de triângulo. A ilustração a seguir mostra um exemplo de índice de recorte de faixa.

![ilustração de um índice de recorte de faixa](images/d3d10-ia-strips-cut-value.png)

### <a name="span-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshader-constant-bufferspanconstant-buffer"></a><span id="Shader_Constant_Buffer"></span><span id="shader_constant_buffer"></span><span id="SHADER_CONSTANT_BUFFER"></span><span id="shader-constant-buffer"></span>Buffer de constantes

Direct3D tem um buffer para fornecer constantes de sombreador chamado buffer constante de sombreador ou simplesmente buffer constante. Conceitualmente, ele se parece com um buffer de vértice de elemento único, conforme mostrado na ilustração a seguir.

![ilustração de um buffer constante de sombreador](images/d3d10-shader-resource-buffer.png)

Cada elemento armazena uma constante de 1 a 4 componentes determinada pelo formato dos dados armazenados.

Buffers constantes reduzem a largura de banda necessária para atualizar constantes de sombreador, permitindo que as constantes de sombreador sejam agrupadas e confirmadas ao mesmo tempo em vez de fazer chamadas individuais para confirmar cada constante separadamente.

Um sombreador continua a ler variáveis em um buffer constante diretamente pelo nome de variável da mesma maneira que são lidas variáveis que não fazem parte de um buffer constante.

Cada estágio de sombreador permite até 15 buffers constantes de sombreador; cada buffer pode manter até 4.096 constantes.

Use um buffer constante para armazenar os resultados da fase de fluxo de saída.

Consulte [Constantes de sombreador (DirectX HLSL)](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-constants) para obter um exemplo de declaração de um buffer constante em um sombreador.

## <a name="span-idtextureresourcesspanspan-idtextureresourcesspanspan-idtextureresourcesspanspan-idtexture-resourcesspantexture-resources"></a><span id="Texture_Resources"></span><span id="texture_resources"></span><span id="TEXTURE_RESOURCES"></span><span id="texture-resources"></span>Recursos de textura


Um recurso de textura é uma coleção estruturada de dados projetada para armazenar texels. Diferente dos buffers, as texturas podem ser filtradas por amostras de texturas à medida que são lidas por unidades de sombreador. O tipo de textura afeta como a textura é filtrada. Um texel representa a menor unidade de uma textura que pode ser lida ou gravada pelo pipeline. Cada texel contém componentes de 1 a 4, organizados em um dos formatos DXGI (consulte [ **DXGI\_formato**](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)).

Texturas são criadas como um recurso estruturado para que seu tamanho seja conhecido. Entretanto, cada textura pode ter tipos ou não no momento em que recurso é criado, contanto que o tipo seja especificado usando um modo de exibição quando a textura está vinculada ao pipeline.

-   [Tipos de textura](#texture-types)
-   [Sub-recursos](#subresources)
-   [Forte vs. Tipificação fraca](#typed)

### <a name="span-idtexturetypesspanspan-idtexturetypesspanspan-idtexturetypesspanspan-idtexture-typesspantexture-types"></a><span id="Texture_Types"></span><span id="texture_types"></span><span id="TEXTURE_TYPES"></span><span id="texture-types"></span>Tipos de textura

Há vários tipos de texturas: 1 D, 2D, 3D, cada um deles pode ser criada com ou sem mipmaps. O Direct3D também oferece suporte a matrizes de textura e texturas multisample.

-   [1D de textura](#texture1d-resource)
-   [Matriz de 1D de textura](#texture1d-array-resource)
-   [Textura 2D e matriz de textura 2D](#texture2d-resource)
-   [Textura 3D](#texture3d-resource)

### <a name="span-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1d-resourcespan1d-texture"></a><span id="Texture1D_Resource"></span><span id="texture1d_resource"></span><span id="TEXTURE1D_RESOURCE"></span><span id="texture1d-resource"></span>1D de textura

Uma textura 1D em sua forma mais simples contém dados de textura que podem ser tratados com uma coordenada de textura única; pode ser visualizada como uma matriz de texels, conforme mostrado na ilustração a seguir.

![ilustração de uma textura 1D](images/d3d10-1d-texture.png)

Cada texel contém um número de componentes de cor dependendo do formato dos dados armazenados. Ao adicionar mais complexidade, você pode criar uma textura 1D com níveis de mipmap, conforme mostrado na ilustração a seguir.

![ilustração de uma textura 1D com níveis de mipmap](images/d3d10-resource-texture1d.png)

Um nível de mipmap é uma textura menor do que o nível superior por uma potência de dois. O nível mais alto contém o máximo de detalhes, cada nível subsequente é menor; para mipmap 1D, o menor nível contém um texel. Os diferentes níveis são identificados por um índice chamado LOD (nível de detalhes); você pode usar o LOD para acessar uma textura menor na renderização de uma geometria que não está tão próxima da câmera.

### <a name="span-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1d-array-resourcespan1d-texture-array"></a><span id="Texture1D_Array_Resource"></span><span id="texture1d_array_resource"></span><span id="TEXTURE1D_ARRAY_RESOURCE"></span><span id="texture1d-array-resource"></span>Matriz de 1D de textura

Direct3D 10 também tem uma nova estrutura de dados para uma matriz de texturas. Uma matriz de texturas 1D conceitualmente parece com a ilustração a seguir.

![ilustração de uma matriz de textura 1D](images/d3d10-resource-texture1darray.png)

Essa matriz de textura contém três texturas. Cada uma das três texturas tem uma largura de 5 (o número de elementos na primeira camada). Cada textura também contém uma mipmap de 3 camadas.

Todas as matrizes de textura no Direct3D são uma matriz homogênea de texturas; ou seja, cada textura em uma matriz de texturas deve ter o mesmo formato de dados e o tamanho (incluindo a largura de textura e o número de níveis de mipmap. Você pode criar matrizes de textura de diferentes tamanhos, contanto que todas as texturas em cada matriz correspondam em tamanho.

### <a name="span-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2d-resourcespan2d-texture-and-2d-texture-array"></a><span id="Texture2D_Resource"></span><span id="texture2d_resource"></span><span id="TEXTURE2D_RESOURCE"></span><span id="texture2d-resource"></span>Textura 2D e matriz de textura 2D

Um recurso Texture2D contém uma grade 2D de texels. Cada texel é endereçável por um vetor u, v. Como é um recurso de textura, ele pode conter níveis de mipmap e sub-recursos. Um recurso de textura 2D totalmente preenchida é semelhante à ilustração a seguir.

![ilustração de um recurso de textura 2D](images/d3d10-resource-texture2d.png)

Esse recurso de textura contém uma única textura 3 x 5 com três níveis de mipmap.

Um recurso Texture2DArray é uma matriz homogênea de texturas 2D; ou seja, cada textura tem o mesmo formato de dados e dimensões (incluindo níveis de mipmap). Ele tem um layout semelhante ao da matriz de textura 1D, exceto que as texturas agora contêm dados 2D e, portanto, têm a aparência da ilustração a seguir.

![ilustração de uma matriz de recursos de textura 2D](images/d3d10-resource-texture2darray.png)

Essa matriz de textura contém três texturas; cada uma de 3 x 5 com dois níveis de mipmap.

### <a name="span-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanusing-a-texture2darray-as-a-texture-cube"></a><span id="Texture2DArray_Resource_as_a_Texture_Cube"></span><span id="texture2darray_resource_as_a_texture_cube"></span><span id="TEXTURE2DARRAY_RESOURCE_AS_A_TEXTURE_CUBE"></span>Usando uma Texture2DArray como um cubo de textura

Um cubo de textura é uma matriz de textura 2D com 6 texturas, uma para cada face do cubo. Um cubo de textura totalmente preenchida se parece com a ilustração a seguir.

![ilustração de uma matriz de recursos de textura 2D que representa um cubo de textura](images/d3d10-resource-texturecube.png)

Uma matriz de textura 2D com 6 texturas pode ser lida nos sombreadores com as funções intrínsecas do mapa do cubo após a vinculação ao pipeline com um modo de exibição de textura de cubo. Os cubos de textura são endereçados do sombreador com um vetor 3D apontado para o centro do cubo de textura.

### <a name="span-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3d-resourcespan3d-texture"></a><span id="Texture3D_Resource"></span><span id="texture3d_resource"></span><span id="TEXTURE3D_RESOURCE"></span><span id="texture3d-resource"></span>Textura 3D

Um recurso Texture3D (também conhecido como textura de volume) contém um volume 3D de texels. Como é um recurso de textura, pode conter níveis de mipmap. Uma textura 3D totalmente preenchida se parece com a ilustração a seguir.

![ilustração de um recurso de textura 3D](images/d3d10-resource-texture3d.png)

Quando uma fatia de mipmap da textura 3D é vinculada como uma saída de destino de renderização (com um modo de exibição de renderização de destino), a textura 3D se comporta de forma idêntica à matriz de textura 2D com n fatias. A fatia de renderização específica é escolhida no estágio do sombreador de geometria.

Não há nenhum conceito de matriz de textura 3D; portanto, um sub-recurso de textura 3D é um nível de mipmap único.

### <a name="span-idsubresourcesspanspan-idsubresourcesspanspan-idsubresourcesspansubresources"></a><span id="Subresources"></span><span id="subresources"></span><span id="SUBRESOURCES"></span>Sub-recursos

A API Direct3D faz referência a recursos inteiros ou subconjuntos de recursos. Para especificar a parte dos recursos, o Direct3D criou o termo *sub-recursos*, que significa que um subconjunto de um recurso.

Um buffer é definido como um sub-recurso único. Texturas são um pouco mais complicadas, já que existem vários tipos diferentes de textura (1D, 2D, etc), e algumas suportam níveis de mipmap e/ou matrizes de textura. Começando com o caso mais simples, uma textura 1D é definida como um sub-recurso único, conforme mostrado na ilustração a seguir.

![ilustração de uma textura 1D](images/d3d10-1d-texture.png)

Isso significa que a matriz de texels que compõe uma textura 1D está contida em um único sub-recurso.

Se você expandir uma textura 1D com três níveis de mipmap, ela poderá ser visualizada da seguinte forma.

![ilustração de uma textura 1D com níveis de mipmap](images/d3d10-resource-texture1d.png)

Pense nisso como uma textura única composta por três subtexturas. Cada subtextura conta como um sub-recurso, portanto, essa textura 1D contém 3 sub-recursos. Uma subtextura (ou sub-recurso) pode ser indexada usando o nível de detalhe (LOD) para uma única textura. Ao usar uma matriz de texturas, acessar uma determinada subtextura exige o LOD e a textura específica. Ou então, a API combina essas duas partes de informações em um índice de sub-recurso único baseado em zero, conforme mostrado aqui.

![ilustração de um índice de sub-recurso baseado em zero](images/d3d10-resource-texture1darray-sub-indexing.png)

### <a name="span-idselectingsubresourcesspanspan-idselectingsubresourcesspanspan-idselectingsubresourcesspanselecting-subresources"></a><span id="Selecting_Subresources"></span><span id="selecting_subresources"></span><span id="SELECTING_SUBRESOURCES"></span>Selecionando os sub-recursos

Algumas APIs acessam um recurso inteiro e outras acessam uma parte de um recurso. As APIs que acessam uma parte de um recurso geralmente usam uma descrição do modo de exibição para especificar os sub-recursos a acessar.

Essas figuras ilustram os termos usados por uma descrição do modo de exibição ao acessar uma matriz de texturas.

### <a name="span-idarrayslicespanspan-idarrayslicespanspan-idarrayslicespanarray-slice"></a><span id="Array_Slice"></span><span id="array_slice"></span><span id="ARRAY_SLICE"></span>Fatia de matriz

Dada uma matriz de texturas, cada textura com mipmaps, uma fatia de matriz (representada pelo retângulo branco) inclui uma textura e todas as suas subtexturas, conforme mostrado na ilustração a seguir.

![ilustração de uma fatia de matriz](images/d3d10-resource-array-slice.png)

### <a name="span-idmipslicespanspan-idmipslicespanspan-idmipslicespanmip-slice"></a><span id="Mip_Slice"></span><span id="mip_slice"></span><span id="MIP_SLICE"></span>Fatia de MIP

Um tamanho de MIP (representado pelo retângulo branco) inclui um nível de mipmap para cada textura em uma matriz, conforme mostrado na ilustração a seguir.

![ilustração de um tamanho de MIP](images/d3d10-resource-mip-slice.png)

### <a name="span-idselectingasinglesubresourcespanspan-idselectingasinglesubresourcespanspan-idselectingasinglesubresourcespanselecting-a-single-subresource"></a><span id="Selecting_a_Single_Subresource"></span><span id="selecting_a_single_subresource"></span><span id="SELECTING_A_SINGLE_SUBRESOURCE"></span>Selecionando um único recurso de secundário

Você pode usar esses dois tipos de fatias para escolher um sub-recurso único, conforme mostrado na ilustração a seguir.

![ilustração da escolha de um sub-recurso usando uma fatia de matriz e um tamanho de MIP](images/d3d10-resource-subresources-1.png)

### <a name="span-idselectingmultiplesubresourcesspanspan-idselectingmultiplesubresourcesspanspan-idselectingmultiplesubresourcesspanselecting-multiple-subresources"></a><span id="Selecting_Multiple_Subresources"></span><span id="selecting_multiple_subresources"></span><span id="SELECTING_MULTIPLE_SUBRESOURCES"></span>Selecionando vários sub-recursos

Ou você pode usar esses dois tipos de fatias com o número de níveis de mipmap e/ou número de texturas para escolher vários sub-recursos.

![ilustração de escolha de vários sub-recursos](images/d3d10-resource-subresources-2.png)

Independentemente do tipo de textura que você está usando, com ou sem mipmaps, com ou sem uma matriz de textura, geralmente há funções auxiliares fornecidas para calcular o índice de um sub-recurso específico.

### <a name="span-idtypedspanspan-idtypedspanspan-idtypedspanstrong-vs-weak-typing"></a><span id="Typed"></span><span id="typed"></span><span id="TYPED"></span>Forte vs. Tipificação fraca

Criar um recurso totalmente com tipo restringe o recurso ao formato com o qual ele foi criado. Isso permite que o tempo de execução otimize o acesso, especialmente se o recurso for criado com sinalizadores indicando que ele não pode ser mapeado pelo app. Recursos criados com um tipo específico não podem ser reinterpretados usando o mecanismo de exibição.

Em um recurso sem tipo, o tipo de dados é desconhecido quando o recurso é criado pela primeira vez. O app deve escolher entre os formatos sem tipo disponíveis. Você deve especificar o tamanho da memória a alocar e se o tempo de execução precisará gerar as subtexturas em mipmap.

No entanto, o formato exato dos dados (seja a memória interpretada como inteiros, valores de ponto flutuante, inteiros não atribuídos, etc.) não é determinado até que o recurso seja associado ao pipeline com um modo de exibição. Como o formato de textura permanece flexível até a textura ser associada ao pipeline, o recurso será referido como armazenamento com tipo fraco. O armazenamento com tipo fraco tem a vantagem de poder ser reutilizado ou reinterpretado (em outro formato), desde que o bit de componente do novo formato corresponda à contagem de bit do formato antigo.

Um recurso único pode ser vinculado a vários estágios de pipeline, desde que cada um tenha um modo de exibição exclusivo que qualifique totalmente os formatos em cada local. Por exemplo, um recurso criado com um formato sem tipo pode ser usado como um formato FLOAT e um formato UINT em locais diferentes no pipeline.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Recursos](resources.md)
