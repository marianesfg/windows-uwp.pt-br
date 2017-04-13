---
title: "Como uma área de recurso de streaming é colocada lado a lado"
description: "Quando você cria um recurso de streaming, as dimensões, o tamanho do elemento de formato e o número de mipmaps e/ou fatias de matriz (se aplicável) determinam o número de blocos que são necessários para toda a área de superfície."
ms.assetid: 3485FD8D-2A06-4B0A-8810-ECF37736F94B
keywords:
- "Como uma área de recurso de streaming é colocada lado a lado"
- "área de recurso, lado a lado"
- tabelas de tamanho, recursos, lado a lado
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 0718557245d9072adb0db608a7692dc7674ceca8
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="how-a-streaming-resources-area-is-tiled"></a>Como uma área de recurso de streaming é colocada lado a lado


Quando você cria um recurso de streaming, as dimensões, o tamanho do elemento de formato e o número de mipmaps e/ou fatias de matriz (se aplicável) determinam o número de blocos que são necessários para toda a área de superfície. O layout de pixel/bytes dentro de blocos é determinado pela implementação. O número de pixels que se enquadram em um bloco, dependendo do tamanho do elemento de formato, é fixo e idêntico se você usa um padrão com swizzling ou não.

O número de blocos que serão usados por uma largura de elemento de tamanho e formato de superfície fornecida é bem definido e previsível com base nas tabelas das seções a seguir. Para recursos que contêm mipmaps ou casos em que as dimensões da superfície não preenchem um bloco, existem algumas restrições; consulte [Compactação mipmap](mipmap-packing.md).

Diferentes recursos de streaming podem apontar para memória idêntica com formatos diferentes, desde que os aplicativos não confiem nos resultados da gravação na memória com um formato e leitura com outro. Mas os aplicativos podem confiar nos resultados da gravação na memória com um formato e leitura ocm outro se os formatos estiverem na mesma família de formato (ou seja, eles têm o mesmo formato do tipo pai). Por exemplo, DXGI\_FORMAT\_R8G8B8A8\_UNORM e DXGI\_FORMAT\_R8G8B8A8\_UINT são compatíveis entre si, mas não com DXGI\_FORMAT\_R16G16\_UNORM.

Uma exceção é onde o sangramento de dados de suavização de um formato para outro está bem definido: se um bloco completamente contém 0 para todos os seus bits, esse bloco pode ser usado com qualquer formato que interpreta esses conteúdos de memória como 0 (independentemente do layout de memória). Portanto, um bloco pode ser limpo para 0x00 com o formato DXGI\_FORMAT\_R8\_UNORM e, em seguida, usado com um formato como DXGI\_FORMAT\_R32G32\_FLOAT e apareceria o conteúdo (que ainda é 0.0f,0.0f).

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
<td align="left"><p>[Sub-recursos lado a lado de Texture2D e Texture2DArray](texture2d-and-texture2darray-subresource-tiling.md)</p></td>
<td align="left"><p>Estas tabelas mostram como os sub-recursos [<strong>Texture2D</strong>](https://msdn.microsoft.com/library/windows/desktop/ff471525) e [<strong>Texture2DArray</strong>](https://msdn.microsoft.com/library/windows/desktop/ff471526) são colocadas lado a lado.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Sub-recursos lado a lado de Texture3D](texture3d-subresource-tiling.md)</p></td>
<td align="left"><p>Esta tabela mostra como os sub-recursos [<strong>Texture3D</strong>](https://msdn.microsoft.com/library/windows/desktop/ff471562) são colocadas lado a lado.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Buffers colocados lado a lado](buffer-tiling.md)</p></td>
<td align="left"><p>Um recurso de [buffer](introduction-to-buffers.md) é dividido em blocos de 64KB, com algum espaço vazio no último bloco se o tamanho não é um múltiplo de 64KB.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Compactação de mipmap](mipmap-packing.md)</p></td>
<td align="left"><p>Um número de mips (por fatia de matriz) pode ser incluído em um número de blocos, dependendo das dimensões de um recurso de streaming, formato, número de mipmaps e fatias da matriz.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Criar recursos de streaming](creating-streaming-resources.md)

 

 




