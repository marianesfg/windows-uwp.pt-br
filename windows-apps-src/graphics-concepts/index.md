---
title: Conceitos de elementos gráficos do Direct3D
description: Descreve os conceitos de gráficos sobre os quais o Microsoft Direct3D foi construído.
ms.assetid: c3850a92-4d05-4f72-bf0f-6a0c79e841eb
keywords:
- Guia de aprendizagem de Gráficos do Direct3D
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3e1fed9eaf3003bc67a86a5282fccbb923019a36
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1653015"
---
# <a name="direct3d-graphics-concepts"></a>Conceitos de elementos gráficos do Direct3D


Descreve conceitos de elementos gráficos do Microsoft Direct3D. Este guia explica os conceitos gerais de elementos gráficos 3D no computador que são usados no desenvolvimento de jogos e aplicativos Direct3D.

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
<td align="left"><p><a href="coordinate-systems-and-geometry.md">Sistemas de coordenadas e geometria</a></p></td>
<td align="left"><p>A programação de apps Direct3D requer familiaridade ao trabalhar com princípios geométricos 3D. Esta seção apresenta os conceitos geométricos mais importantes para a criação de cenas 3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="vertex-and-index-buffers.md">Buffers de vértice e índice</a></p></td>
<td align="left"><p>Os <em>buffers de vértice</em> são buffers de memória que contêm dados de vértice; vértices em um buffer de vértice são processados para executar a transformação, iluminação e corte. <em>Buffers de índice</em> são buffers de memória que contêm dados de índice, que são deslocamentos de inteiro em buffers de vértice usados para renderizar primitivas.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="devices.md">Dispositivos</a></p></td>
<td align="left"><p>Um dispositivo Direct3D é o componente de renderização do Direct3D. Um dispositivo encapsula e armazena o estado de renderização, executa transformações e operações de iluminação e rasteriza uma imagem para uma superfície.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="lights-and-materials.md">Iluminação</a></p></td>
<td align="left"><p>As luzes são usadas para iluminar objetos em uma cena. A cor de cada vértice de objeto tem por base o mapa de textura atual, as cores do vértice e as fontes de iluminação.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="depth-and-stencil-buffers.md">Buffers de profundidade e estêncil</a></p></td>
<td align="left"><p>Um <em>buffer de profundidade</em> armazena informações de profundidade para controlar quais áreas de polígonos são renderizadas em vez de serem ocultas da exibição. Um <em>buffer de estêncil</em> é usado para mascarar pixels em uma imagem, para produzir efeitos especiais, incluindo composição; decalque; dissolver, esmaecimento e deslizar o dedo; contornos e silhuetas; e estêncil de dois lados.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="textures.md">Texturas</a></p></td>
<td align="left"><p>Texturas são uma ferramenta poderosa para criar realismo em imagens 3D geradas por computador. Direct3D dá suporte a um amplo conjunto de recursos texturização, fornecendo aos desenvolvedores acesso fácil a técnicas avançadas de texturização.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="graphics-pipeline.md">Pipeline de elementos gráficos</a></p></td>
<td align="left"><p>O pipeline de gráficos do Direct3D foi projetado para a geração de elementos gráficos para aplicativos de jogos em tempo real. Fluxos de dados de entrada para saída por cada um dos estágios configuráveis ou programáveis.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="views.md">Modos de exibição</a></p></td>
<td align="left"><p>O termo &quot;modo de exibição&quot; é usado para indicar &quot;dados no formato exigido&quot;. Por exemplo, um modo de exibição de buffer constante (CBV) seriam dados de buffer constante formatados corretamente. Esta seção descreve os modos de exibição mais comuns e úteis.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="compute-pipeline.md">Calcular o pipeline</a></p></td>
<td align="left"><p>O pipeline de computação do Direct3D foi projetado para cálculos que podem ser feitos principalmente em paralelo com o pipeline de gráficos.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="resources.md">Recursos</a></p></td>
<td align="left"><p>Um recurso é uma área na memória que pode ser acessada pelo pipeline de Direct3D. Para que o pipeline acesse a memória com eficiência, os dados fornecidos ao pipeline (como geometria de entrada, recursos de sombreador e texturas) devem ser armazenados em um recurso. Existem dois tipos de recursos a partir dos quais todos os recursos de Direct3D são derivados: um buffer ou uma textura. Até 128 recursos podem estar ativos para cada estágio do pipeline.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="streaming-resources.md">Recursos de streaming</a></p></td>
<td align="left"><p>Os <em>recursos de streaming</em> são enormes recursos lógicos que utilizam pequenas quantias de memória física. Em vez de transferir um recurso grande inteiro, pequenas partes do recurso são transmitidas conforme necessário. Recursos de streaming eram anteriormente chamados de <em>recursos em bloco</em>.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="appendix.md">Apêndices</a></p></td>
<td align="left"><p>Essas seções fornecem detalhes técnicos detalhados.</p></td>
</tr>
</tbody>
</table>

 

 

 