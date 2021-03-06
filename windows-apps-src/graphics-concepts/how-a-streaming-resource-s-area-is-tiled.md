---
title: Como uma área de recurso de streaming é colocada lado a lado
description: Quando você cria um recurso de streaming, as dimensões, o tamanho do elemento de formato e o número de mipmaps e/ou fatias de matriz (se aplicável) determinam o número de blocos que são necessários para toda a área de superfície.
ms.assetid: 3485FD8D-2A06-4B0A-8810-ECF37736F94B
keywords:
- Como uma área de recurso de streaming é colocada lado a lado
- área de recurso, lado a lado
- tabelas de tamanho, recursos, lado a lado
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 28fac411ad814167dcef3b02424c866bd3453344
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370591"
---
# <a name="how-a-streaming-resources-area-is-tiled"></a>Como uma área de recurso de streaming é colocada lado a lado


Quando você cria um recurso de streaming, as dimensões, o tamanho do elemento de formato e o número de mipmaps e/ou fatias de matriz (se aplicável) determinam o número de blocos que são necessários para toda a área de superfície. O layout de pixel/bytes dentro de blocos é determinado pela implementação. O número de pixels que se enquadram em um bloco, dependendo do tamanho do elemento de formato, é fixo e idêntico se você usa um padrão com swizzling ou não.

O número de blocos que serão usados por uma largura de elemento de tamanho e formato de superfície fornecida é bem definido e previsível com base nas tabelas das seções a seguir. Para recursos que contêm mipmaps ou casos em que as dimensões da superfície não preenchem um bloco, existem algumas restrições; consulte [Compactação mipmap](mipmap-packing.md).

Diferentes recursos de streaming podem apontar para memória idêntica com formatos diferentes, desde que os aplicativos não confiem nos resultados da gravação na memória com um formato e leitura com outro. Mas os aplicativos podem confiar nos resultados da gravação na memória com um formato e leitura ocm outro se os formatos estiverem na mesma família de formato (ou seja, eles têm o mesmo formato do tipo pai). Por exemplo, o DXGI\_formato\_R8G8B8A8\_UNORM e DXGI\_formato\_R8G8B8A8\_UINT são compatíveis entre si, mas não com o DXGI\_formato\_R16G16\_UNORM.

Uma exceção é onde o sangramento de dados de suavização de um formato para outro está bem definido: se um bloco completamente contém 0 para todos os seus bits, esse bloco pode ser usado com qualquer formato que interpreta esses conteúdos de memória como 0 (independentemente do layout de memória). Portanto, um bloco pode ser limpo com 0x00 com o formato DXGI\_formato\_R8\_UNORM e, em seguida, usado com um formato como DXGI\_formato\_R32G32\_FLOAT e ela seriam exibido o conteúdo é ainda (0.0f, 0.0f).

O layout dos dados dentro de um bloco não depende de onde o bloco é mapeado em um recurso geral. Assim, por exemplo, um bloco pode ser reutilizado em locais diferentes de uma superfície de uma só vez com um comportamento consistente em todos os locais.

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
<td align="left"><p><a href="texture2d-and-texture2darray-subresource-tiling.md">Texture2D e Texture2DArray subrecursos lado a lado</a></p></td>
<td align="left"><p>Estas tabelas mostram como sub-recursos <a href="https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2d"><strong>Texture2D</strong></a> e <a href="https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2darray"><strong>Texture2DArray</strong></a> são agrupados lado a lado.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture3d-subresource-tiling.md">Texture3D subrecursos lado a lado</a></p></td>
<td align="left"><p>Esta tabela mostra como os sub-recursos de <a href="https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture3d"><strong>Texture3D</strong></a> são colocados lado a lado.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="buffer-tiling.md">Lado a lado do buffer</a></p></td>
<td align="left"><p>Um recurso de <a href="introduction-to-buffers.md">buffer</a> é dividido em blocos de 64KB, com algum espaço vazio no último bloco se o tamanho não é um múltiplo de 64KB.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="mipmap-packing.md">Empacotamento de Mipmap</a></p></td>
<td align="left"><p>Um número de mips (por fatia de matriz) pode ser incluído em um número de blocos, dependendo das dimensões de um recurso de streaming, formato, número de mipmaps e fatias da matriz.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Criar recursos de streaming](creating-streaming-resources.md)

 

 




