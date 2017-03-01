---
title: "Modos de exibição"
description: "O termo \\ 0034;modo de exibição \\ 0034; é usado para indicar \\ 0034;dados no formato exigido \\ 0034;. Por exemplo, um modo de exibição de buffer constante (CBV) deve ser dados de buffer constantes formatados corretamente. Esta seção descreve os modos de exibição mais comuns e úteis."
ms.assetid: 0C7FB99F-7391-472F-BA53-576888DFC171
keywords:
- "Modos de exibição"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: beaed3bbaf6cab56c826788d0361b1200c73a003
ms.lasthandoff: 02/07/2017

---

# <a name="views"></a>Modos de exibição


O termo "modo de exibição" é usado para indicar "dados no formato exigido". Por exemplo, um modo de exibição de buffer constante (CBV) deve ser dados de buffer constantes formatados corretamente. Esta seção descreve os modos de exibição mais comuns e úteis.

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
<td align="left"><p>[Modo de exibição de buffer constante (CBV)](constant-buffer-view--cbv-.md)</p></td>
<td align="left"><p>Buffers constantes contêm dados constantes de sombreador. O valor deles é que os dados são preservados e podem ser acessados por qualquer sombreador de GPU, até que seja necessário alterar os dados.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Modo de exibição de buffer de vértice (VBV) e modo de exibição de buffer de índice (IBV)](vertex-buffer-view--vbv-.md)</p></td>
<td align="left"><p>Um buffer de vértices contém dados para uma lista de vértices. Os dados de cada vértice podem incluir posição, cor, vetor normal, coordenadas de textura e assim por diante. Um buffer de índice mantém os índices de números inteiros (deslocamentos) em um buffer de vértice e é usado para definir e renderizar um objeto composto por um subconjunto da lista completa de vértices.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Modo de exibição de recurso de sombreador (SRV) e modo de exibição de acesso não ordenado (UAV)](shader-resource-view--srv-.md)</p></td>
<td align="left"><p>Os modos de exibição de recurso de sombreador geralmente envolvem as texturas em um formato que pode ser acessado pelos sombreadores. Um modo de exibição de acesso não ordenado fornece funcionalidade semelhante, mas permite a leitura e a gravação na textura (ou outro recurso) em qualquer ordem.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Amostra](sampler.md)</p></td>
<td align="left"><p>A amostragem é o processo de leitura de valores de entrada de uma textura ou outros recursos. Uma &quot;amostra&quot; é qualquer objeto que lê a partir de recursos.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Modo de exibição de destino de renderização (RTV)](render-target-view--rtv-.md)</p></td>
<td align="left"><p>Renderizar destinos permite que uma cena seja renderizada em um buffer intermediário temporário em vez do buffer de fundo ser renderizado na tela. Esse recurso permite o uso da cena complexa que pode ser renderizado, talvez como uma textura de reflexão ou outra finalidade dentro do pipeline gráfico, ou talvez para adicionar efeitos adicionais do sombreador de pixel para a cena antes da renderização.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Modo de exibição de estêncil de profundidade (DSV)](depth-stencil-view--dsv-.md)</p></td>
<td align="left"><p>Um modo de exibição de estêncil de profundidade fornece o formato e o buffer para manter informações de profundidade e estêncil. O buffer de profundidade é usado para remover o desenho de pixels que seria invisível para o visualizador, uma vez que eles são obstruídos por um objeto mais próximo. O buffer de estêncil pode ser usado para remover todos os desenhos fora de uma forma definida.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Modo de exibição de saída de fluxo (SOV)](stream-output-view--sov-.md)</p></td>
<td align="left"><p>Os modos de exibição de saída de fluxo permitem que as informações de vértice que os sombreadores de vértice, geometria e mosaico adotaram sejam transmitidas de volta para o aplicativo para uso adicional. Por exemplo, um objeto que tenha sido distorcido por esses sombreadores pode ser gravado de volta no aplicativo para fornecer uma entrada mais precisa para física ou outro mecanismo. Na prática, os modos de exibição de saída de fluxo são um recurso usado com pouca frequência do pipeline de elementos gráficos.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Modo de exibição organizado rasterizado (ROV)](rasterizer-ordered-view--rov-.md)</p></td>
<td align="left"><p>Rasterizar modos de exibição ordenados permite que algumas das limitações de um buffer de profundidade sejam abordadas, especialmente tendo várias texturas que contém transparência sendo aplicadas aos mesmos pixels.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Guia de aprendizagem de Gráficos do Direct3D](index.md)

 

 





