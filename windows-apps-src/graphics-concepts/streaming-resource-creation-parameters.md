---
title: "Parâmetros de criação de recursos de streaming"
description: "Há algumas restrições sobre o tipo de recursos do Direct3D que você pode criar como um recurso de streaming."
ms.assetid: 6FC5AD93-6F47-479E-947C-895C99B427BC
keywords: "Parâmetros de criação de recursos de streaming"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 8704705edb0ec516a03c13a4bf240fd00f31e44b
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="streaming-resource-creation-parameters"></a>Parâmetros de criação de recursos de streaming


Há algumas restrições sobre o tipo de recursos do Direct3D que você pode criar como um recurso de streaming.

<span id="Supported-Resource-Type"></span><span id="supported-resource-type"></span><span id="SUPPORTED-RESOURCE-TYPE"></span>**Tipo de recurso com suporte**  
Texture2D\[Array\] (incluindo TextureCube\[Array\], que é uma variante do Texture2D\[Array\]) ou Buffer.

**NÃO tem suporte: **Texture1D\[Array\].

<span id="Supported-Resource-Usage"></span><span id="supported-resource-usage"></span><span id="SUPPORTED-RESOURCE-USAGE"></span>**Uso de recursos com suporte**  
Uso do padrão.

**NÃO tem suporte:  **dinâmico, preparo ou imutável.

<span id="Supported-Resource-Misc-Flags"></span><span id="supported-resource-misc-flags"></span><span id="SUPPORTED-RESOURCE-MISC-FLAGS"></span>**Sinalizadores de diversos recursos com suporte**  
Lado a lado; ou seja, streaming (por definição), cubo de textura, desenhar argumentos indiretos, buffer permitir modos de exibição brutos, buffer estruturado, vinculação de recurso ou gerar mips.

**NÃO tem suporte:  **compartilhado, mutex com chave compartilhada, compatível com GDI, identificador NT compartilhado, conteúdo restrito, restringir o recurso compartilhado, restringir o driver de recurso compartilhado, protegidos ou pool de bloco.

<span id="Supported-Bind-Flags"></span><span id="supported-bind-flags"></span><span id="SUPPORTED-BIND-FLAGS"></span>**Sinalizadores de ligação com suporte**  
Associar como recurso de sombreador, acesso não ordenado, estêncil de profundidade ou destino de renderização.

**NÃO tem suporte:  **Associar como buffer constante, buffer de vértice (vincular um buffer em blocos como um SRV/UAV/RTV é suportado), buffer de índice, saída de fluxo, decodificador ou codificador de vídeo.

<span id="Supported-Formats"></span><span id="supported-formats"></span><span id="SUPPORTED-FORMATS"></span>**Formatos com suporte**  
Todos os formatos que devem estar disponíveis para determinada configuração, independentemente dela ser lado a lado, com algumas exceções.

<span id="Supported-Sample-Description--Multisample-count--quality-"></span><span id="supported-sample-description--multisample-count--quality-"></span><span id="SUPPORTED-SAMPLE-DESCRIPTION--MULTISAMPLE-COUNT--QUALITY-"></span>**Descrição de exemplo com suporte (contagem de várias amostras, qualidade)**  
Tudo o que seria suportado para a configuração fornecida, independentemente dela ser lado a lado, com algumas exceções.

<span id="Supported-Width-Height-MipLevels-ArraySize"></span><span id="supported-width-height-miplevels-arraysize"></span><span id="SUPPORTED-WIDTH-HEIGHT-MIPLEVELS-ARRAYSIZE"></span>**Com suporte largura/altura/MipLevels/ArraySize**  
Extensões completas compatíveis com o Direct3D. Os recursos de streaming não tem a restrição no tamanho de memória total imposto nos recursos não streaming. Os recursos de streaming somente são restringidos pelos limites do espaço de endereço virtual geral. Consulte [Lidar com espaço disponível para recursos de streaming](address-space-available-for-streaming-resources.md).

O conteúdo inicial da memória do pool de bloco é indefinido.

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
<td align="left"><p>[Lidar com espaço disponível para recursos de streaming](address-space-available-for-streaming-resources.md)</p></td>
<td align="left"><p>Esta seção especifica o espaço de endereço virtual que está disponível para recursos de streaming.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Criar recursos de streaming](creating-streaming-resources.md)

 

 



