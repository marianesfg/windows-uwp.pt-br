---
title: "Guia de aprendizagem de Gráficos do Direct3D"
description: "Descreve os conceitos de gráficos sobre os quais o Microsoft Direct3D foi construído."
ms.assetid: c3850a92-4d05-4f72-bf0f-6a0c79e841eb
keywords:
- "Guia de aprendizagem de Gráficos do Direct3D"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: e62f9cfde35580dd384ef69fe6e5658d927ce3d8
ms.lasthandoff: 02/07/2017

---

# <a name="direct3d-graphics-learning-guide"></a>Guia de aprendizagem de Gráficos do Direct3D


Descreve os conceitos de gráficos sobre os quais o Microsoft Direct3D foi construído. Esse conjunto de documentação é amplamente independente de qualquer versão do Direct3D e é para os desenvolvedores de gráficos que precisam de mais informações de fundo que as fornecidas na documentação de API específica da versão.

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
<td align="left"><p>[Sistemas de coordenadas e geometria](coordinate-systems-and-geometry.md)</p></td>
<td align="left"><p>A programação de apps de Direct3D requer familiaridade de trabalho com princípios geométricos 3D. Esta seção apresenta os conceitos geométricos mais importantes para a criação de cenas 3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Buffers de vértice e índice](vertex-and-index-buffers.md)</p></td>
<td align="left"><p><em>Buffers de vértice</em> são buffers de memória que contêm dados de vértice; vértices em um buffer de vértice são processados para executar a transformação, iluminação e corte. <em>Buffers de índice</em> são buffers de memória que contêm dados de índice, que são deslocamentos de inteiro em buffers de vértice, usados para renderizar primitivas.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Dispositivos](devices.md)</p></td>
<td align="left"><p>Um dispositivo Direct3D é o componente de renderização do Direct3D. Um dispositivo encapsula e armazena o estado de renderização, executa transformações e operações de iluminação e rasteriza uma imagem para uma superfície.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Iluminação](lights-and-materials.md)</p></td>
<td align="left"><p>As luzes são usadas para iluminar objetos em uma cena. A cor de cada vértice de objeto tem por base o mapa de textura atual, as cores do vértice e as fontes de iluminação.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Buffers de profundidade e estêncil](depth-and-stencil-buffers.md)</p></td>
<td align="left"><p>Um <em>buffer de profundidade</em> armazena informação de profundidade para controlar quais áreas de polígonos são renderizadas em vez de serem ocultadas da exibição. Um <em>buffer de estêncil</em> é usado para mascarar pixels em uma imagem, para produzir efeitos especiais, incluindo composição; decalque; dissolver, fade e deslizar; contornos e silhuetas; e estêncil de dois lados.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Texturas](textures.md)</p></td>
<td align="left"><p>Texturas são uma ferramenta poderosa na criação de realismo em imagens 3D geradas por computador. Direct3D dá suporte a um amplo conjunto de recursos texturização, fornecendo aos desenvolvedores acesso fácil a técnicas avançadas de texturização.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Pipeline de elementos gráficos](graphics-pipeline.md)</p></td>
<td align="left"><p>O pipeline de gráficos do Direct3D foi projetado para a geração de gráficos para apps de jogos em tempo real. Fluxos de dados de entrada para saída por cada um dos estágios configuráveis ou programáveis.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Modos de exibição](views.md)</p></td>
<td align="left"><p>O termo &quot;modo de exibição&quot; é usado para indicar &quot;dados no formato exigido&quot;. Por exemplo, um modo de exibição de buffer constante (CBV) seriam dados de buffer constante formatados corretamente. Esta seção descreve os modos de exibição mais comuns e úteis.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Calcular o pipeline](compute-pipeline.md)</p></td>
<td align="left"><p>O pipeline de computação do Direct3D foi projetado para cálculos que podem ser feitos principalmente em paralelo com o pipeline de gráficos.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Recursos](resources.md)</p></td>
<td align="left"><p>Um recurso é uma área na memória que pode ser acessada pelo pipeline de Direct3D. Para que o pipeline acesse a memória com eficiência, os dados fornecidos ao pipeline (como geometria de entrada, recursos de sombreador e texturas) devem ser armazenados em um recurso. Existem dois tipos de recursos a partir dos quais todos os recursos de Direct3D são derivados: um buffer ou uma textura. Até 128 recursos podem estar ativos para cada estágio do pipeline.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Recursos de streaming](streaming-resources.md)</p></td>
<td align="left"><p><em>Recursos de streaming</em> são grandes recursos lógicos que usam pequenas quantidades de memória física. Em vez de transferir um recurso grande inteiro, pequenas partes do recurso são transmitidas conforme necessário. Recursos de streaming eram anteriormente chamados de <em>recursos em bloco</em>.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Apêndices](appendix.md)</p></td>
<td align="left"><p>Essas seções fornecem detalhes técnicos detalhados.</p></td>
</tr>
</tbody>
</table>

 

 

 

