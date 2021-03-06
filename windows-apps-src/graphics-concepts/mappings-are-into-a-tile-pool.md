---
title: Mapeamentos estão em um pool de blocos
description: Quando um recurso é criado como um recurso de streaming, os blocos que compõem o recurso são provenientes do apontamento para locais em um pool de blocos. Um pool de blocos é um pool de memória (sustentado por uma ou mais alocações nos bastidores - nunca vistos pelo app).
ms.assetid: 58B8DBD5-62F5-4B94-8DD1-C7D57A812185
keywords:
- Mapeamentos estão em um pool de blocos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a68623b0a61672426c9b6eef85cb7d1ddc990a19
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370997"
---
# <a name="mappings-are-into-a-tile-pool"></a>Mapeamentos estão em um pool de blocos


Quando um recurso é criado como um recurso de streaming, os blocos que compõem o recurso são provenientes do apontamento para locais em um pool de blocos. Um pool de blocos é um pool de memória (sustentado por uma ou mais alocações nos bastidores - nunca vistos pelo app). O sistema operacional e o driver de vídeo gerenciam esse pool de memória, e o volume da memória é entendido facilmente por um app. Os recursos de streaming mapeiam regiões de 64 KB apontando para locais em um pool de blocos. Um resultado dessa configuração é permitir que vários recursos compartilhem e reutilizem os mesmos blocos, e também que os mesmos blocos sejam reutilizados em locais diferentes em um recurso, se desejado.

O custo da flexibilidade de preenchimento dos blocos de um recurso de um pool de blocos é que ele precisa definir e manter o mapeamento dos blocos no pool que representam aqueles necessários para o recurso. Os mapeamentos de blocos podem ser alterados. Além disso, nem todos os blocos em um recurso precisam ser mapeadas simultaneamente; um recurso pode ter mapeamentos **NULOS**. Um mapeamento **NULO** define um bloco como não disponível do ponto de vista do recurso que o acessa.

É possível criar vários pools de bloco e qualquer número de recursos de streaming pode ser mapeado em qualquer pool de blocos ao mesmo tempo. Os pools de bloco também podem ser aumentados ou reduzidos. Para obter mais informações, consulte [Redimensionamento do pool de blocos](tile-pool-resizing.md). Uma restrição que existe para simplificar a implementação do tempo de execução e o driver de vídeo é que um determinado recurso de streaming pode ter mapeamentos no máximo em um pool de blocos de cada vez (em vez de precisar de mapeamento simultâneo de vários pools de blocos).

A quantidade de armazenamento associada a um recurso de streaming (ou seja, a memória do pool de blocos independente) é aproximadamente proporcional ao número de blocos realmente mapeado para o pool a qualquer momento. No hardware, esse fato se resume ao dimensionamento do volume de memória para armazenamento da tabela de página aproximadamente com a quantidade de blocos mapeados (por exemplo, usando um esquema de tabela de vários níveis de página conforme apropriado).

O pool de blocos pode ser pensado como uma abstração de software que permite aos apps Direct3D estarem efetivamente prontos para programar as tabelas de página na unidade de processamento gráfico (GPU) sem precisar saber os detalhes de implementação de nível baixo (ou processar endereços de ponteiro diretamente). Os pools de blocos não se aplicam a quaisquer níveis adicionais de indireção no hardware. As otimizações de uma única tabela de página de nível único usando construções como diretórios de página são independentes do conceito de pool de blocos.

Vamos explorar qual armazenamento a tabela de página pode exigir na pior das hipóteses (embora na prática as implementações exigem apenas armazenamento aproximadamente proporcional ao que é mapeado).

Considere que cada entrada da tabela de página é de 64 bits.

Para a tabela de página de pior caso de ocorrências de uma única superfície, considerando os limites de recursos no Direct3D 11, suponha que um recurso de streaming é criado com um formato de 128 bits por elemento (por exemplo, um float RGBA), portanto, um bloco de 64KB de tamanho contém apenas 4096 pixels. O máximo com suporte [ **Texture2DArray** ](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2darray) tamanho de 16384\*16384\*2048 (mas com apenas um único mipmap) exigiria cerca de 1 GB de armazenamento na tabela de página se totalmente populada (não incluindo mapas MIP) usando as entradas da tabela de 64 bits. A adição de mipmaps implica no crescimento do armazenamento de tabela de página totalmente mapeada (pior hipótese) em aproximadamente um terço, cerca de 1,3 GB.

Nesse caso, seria equivalente a fornecer acesso a aproximadamente 10,6 terabytes de memória endereçável. Pode haver um limite na quantidade de memória endereçável. Porém, isso reduziria esses valores, talvez próximos do intervalo de terabytes.

Outro caso a considerar é uma única [ **Texture2D** ](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2d) streaming o recurso de 16384\*16384 com um formato de 32 bits por elemento, incluindo mapas MIP. O espaço necessário em uma tabela de página totalmente preenchida é de aproximadamente 170 KB com entradas de tabela de 64 bits.

Por fim, considere um exemplo usando um formato BC, diga BC7 com 128 bits por bloco de 4 x 4 pixels. Isso equivale a um byte por pixel. Um [ **Texture2DArray** ](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2darray) de 16384\*16384\*2048 mipmaps incluindo exigiria mais ou menos de 85 MB preencher completamente essa memória em uma tabela de página. Isso não é ruim considerando que permite a um recurso de streaming abranger 550 gigapixels (512 GB de memória nesse caso).

Na prática, essa quantidade de mapeamentos totais não seria definido considerando que a quantidade de memória física disponível não permite que essa quantidade seja mapeada e referenciada ao mesmo tempo. No entanto, com um pool de blocos, os apps podem optar por reutilizar blocos (um exemplo simples, reutilizar um bloco colorido "preto" para grandes regiões de preto em uma imagem), usando efetivamente o pool de blocos (ou seja, os mapeamentos de tabela de página) como uma ferramenta de compactação de memória.

O conteúdo inicial da tabela da página é **NULO** para todas as entradas. Os apps também não podem passar dados iniciais do conteúdo da memória da superfície, pois já começa sem suporte da memória.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>Nesta seção


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tópico</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="tile-pool-creation.md">Criação do pool de bloco</a></p></td>
<td align="left"><p>Os apps podem criar um ou mais pools de blocos por dispositivo Direct3D. O tamanho total de cada pool de bloco é restrito para o limite de tamanho de recursos do Direct3D 11, que é de aproximadamente 1 e 4 da RAM de GPU.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tile-pool-resizing.md">Redimensionamento de pool de bloco</a></p></td>
<td align="left"><p>Redimensione um pool de blocos para ampliar um pool de blocos se o app precisar de mais trabalho para os recursos de streaming mapeados neles ou reduzir se precisar de menos espaço.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="hazard-tracking-versus-tile-pool-resources.md">Em comparação com os recursos do pool de bloco de controle de risco</a></p></td>
<td align="left"><p>Para recursos não streaming, o Direct3D pode impedir determinadas condições de risco durante a renderização, mas como o controle de risco estaria em um nível de bloco para os recursos de streaming, monitorar as condições de risco durante a renderização de streaming de recursos pode ser muito caro.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Criar recursos de streaming](creating-streaming-resources.md)

 

 




