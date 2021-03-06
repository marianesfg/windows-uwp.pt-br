---
title: Introdução a buffers
description: Um recurso de buffer é uma coleção de dados completamente tipados, agrupados em elementos.
ms.assetid: 494FDF57-0FBE-434C-B568-06F977B40263
keywords:
- Introdução a buffers
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: deeae0cc66a7e75da2e44c0d2aba2a9ed459b824
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602571"
---
# <a name="introduction-to-buffers"></a>Introdução a buffers


Um recurso de buffer é uma coleção de dados completamente tipados, agrupados em elementos. Os buffers armazenam dados, como as coordenadas de textura em um *buffer de vértice*, indexa em um *buffer de índice*, dados de constantes de sombreador em um *buffer constante*, vetores de posição, vetores normais ou estado do dispositivo.

Um elemento de buffer é composto de 1 a 4 componentes. Elementos de buffer podem incluir valores de dados de pacote (como valores de superfície R8G8B8A8), inteiros de 8 bits únicos ou quatro valores de ponto flutuante de 32 bits.

Um buffer é criado como um recurso não estruturado. Como ele não é estruturado, um buffer não pode conter níveis de mipmap, não pode ser filtrado quando lido e não pode conter várias amostras.

## <a name="span-idbuffertypesspanspan-idbuffertypesspanspan-idbuffertypesspanbuffer-types"></a><span id="Buffer_Types"></span><span id="buffer_types"></span><span id="BUFFER_TYPES"></span>Tipos de buffer


Os tipos de recursos de buffer seguintes são compatíveis com Direct3D 11.

-   [Buffer de vértice](#vertex-buffer)
-   [Buffer de índices](#index-buffer)
-   [Buffer de constantes](#shader-constant-buffer)

### <a name="span-idvertexbufferspanspan-idvertexbufferspanspan-idvertexbufferspanspan-idvertex-bufferspanvertex-buffer"></a><span id="Vertex_Buffer"></span><span id="vertex_buffer"></span><span id="VERTEX_BUFFER"></span><span id="vertex-buffer"></span>Buffer de vértice

Um buffer de vértice contém os dados de vértice usados para definir sua geometria. Dados de vértice incluem coordenadas de posição, dados de cor, dados de coordenadas de textura, dados normais e assim por diante.

O exemplo mais simples de um buffer de vértice é aquele que contém apenas dados de posição. Ele pode ser visualizado como na ilustração a seguir.

![ilustração de um buffer de vértices que contém dados de posição](images/d3d10-resources-single-element-vb2.png)

Com mais frequência, um buffer de vértices contém todos os dados necessários para especificar totalmente vértices 3D. Um exemplo disso pode ser um buffer de vértices que contém coordenadas de posição, normais e de textura por vértice. Esses dados geralmente são organizados como conjuntos de elementos por vértice, conforme mostrado na ilustração a seguir.

![ilustração de um buffer de vértices que contém dados de posição, normais e de textura](images/d3d10-vertex-buffer-element.png)

Esse buffer de vértice contém dados por vértice; cada vértice armazena três elementos (posição, normal e coordenadas de textura). A posição e a normal são cada frequentemente especificadas usando três flutuações de 32 bits e coordenadas de textura usando duas flutuações de 32 bits.

Para acessar os dados em um buffer de vértice, você precisa saber qual vértice acessar, além dos seguintes parâmetros de buffer adicionais:

-   Deslocamento -o número de bytes desde o início do buffer para os dados para o primeiro vértice.
-   BaseVertexLocation -o número de bytes do deslocamento até o primeiro vértice usado pela chamada de desenho apropriada.

Antes de criar um buffer de vértice, você precisa definir seu layout. Depois que o objeto de layout de entrada é criado, você o vincula ao [estágio do assembler de entrada (IA)](input-assembler-stage--ia-.md).

### <a name="span-idindexbufferspanspan-idindexbufferspanspan-idindexbufferspanspan-idindex-bufferspanindex-buffer"></a><span id="Index_Buffer"></span><span id="index_buffer"></span><span id="INDEX_BUFFER"></span><span id="index-buffer"></span>Buffer de índices

Buffers de índice contêm deslocamentos de inteiros em buffers de vértice e são usados para renderizar primitivas de forma mais eficiente. Um buffer de índice contém um conjunto sequencial de índices de 16 ou 32 bits; cada índice é usado para identificar um vértice em um buffer de vértices. Um buffer de índice pode ser visualizado como na ilustração a seguir.

![ilustração de um buffer de índice](images/d3d10-index-buffer.png)

Os índices sequenciais armazenados em um buffer de índice estão localizados com os seguintes parâmetros:

-   Deslocamento - o número de bytes do endereço base do buffer de índice.
-   StartIndexLocation - especifica o primeiro elemento de buffer de índice do endereço básico e do deslocamento. O local inicial representa o primeiro índice para renderizar.
-   IndexCount -o número de índices para renderizar.

Início do Buffer de índice = endereço de Base de Buffer de índice + deslocamento (bytes) + StartIndexLocation \* ElementSize (bytes).

Nesse cálculo, ElementSize é o tamanho de cada elemento de buffer de índice, que é dois ou quatro bytes.

### <a name="span-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshader-constant-bufferspanconstant-buffer"></a><span id="Shader_Constant_Buffer"></span><span id="shader_constant_buffer"></span><span id="SHADER_CONSTANT_BUFFER"></span><span id="shader-constant-buffer"></span>Buffer de constantes

Um buffer constante permite que você forneça com eficiência dados constantes do sombreador no pipeline. Você pode usar um buffer constante para armazenar os resultados do estágio de fluxo de saída. Conceitualmente, um buffer constante parece muito com um buffer de vértice de elemento único, conforme mostrado na ilustração a seguir.

![ilustração de um buffer constante de sombreador](images/d3d10-shader-resource-buffer.png)

Cada elemento armazena uma constante de 1 a 4 componentes determinada pelo formato dos dados armazenados.

Um buffer constante só pode usar um sinalizador de vinculação único, que não pode ser combinado com qualquer outro sinalizador de vinculação.

Para ler um buffer constante de sombreador de um sombreador, use uma função de carregamento HLSL. Cada estágio de sombreador permite até 15 buffers constantes de sombreador; cada buffer pode manter até 4.096 constantes.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Buffers de vértice e índice](vertex-and-index-buffers.md)

 

 




