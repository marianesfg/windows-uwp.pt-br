---
title: Processo cruzado de recursos de streaming e compartilhamento de dispositivos
description: "Os pools de bloco podem ser compartilhados com outros processos como recursos tradicionais. O streaming de recursos que faz referência a pools de bloco não pode ser compartilhado entre dispositivos e processos."
ms.assetid: D035AF4B-D71B-4F20-9A98-7C360BF9B285
keywords: Processo cruzado de recursos de streaming e compartilhamento de dispositivos
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 8f383805759e099144f13a5924b5da90240b4ef0
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="span-iddirect3dconceptsstreaming-resource-cross-process-and-device-sharingspanstreaming-resource-cross-process-and-device-sharing"></a><span id="direct3dconcepts.streaming-resource-cross-process-and-device-sharing"></span>Processo cruzado de recursos de streaming e compartilhamento de dispositivos


Os pools de bloco podem ser compartilhados com outros processos como recursos tradicionais. O streaming de recursos que faz referência a pools de bloco não pode ser compartilhado entre dispositivos e processos. Mas processos separados podem criar seus próprios recursos de streaming que fazem o mapeamento em pools de bloco que são compartilhados entre esses recursos de streaming.

Os pools de bloco compartilhado não podem ser redimensionados.

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
<td align="left"><p>[Não há suporte para formatos de estêncil com recursos de streaming](stencil-formats-not-supported-with-streaming-resources.md)</p></td>
<td align="left"><p>Formatos que contêm estêncil não são compatíveis com recursos de streaming.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Criar recursos de streaming](creating-streaming-resources.md)

 

 




