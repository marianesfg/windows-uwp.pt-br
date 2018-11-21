---
author: mtoepke
title: Correlacionar OpenGL ES 2.0 com Direct3D 11
description: Ao iniciar o processo de portabilidade da arquitetura gráfica do OpenGL ES 2.0 para o Direct3D pela primeira vez, familiarize-se com as principais diferenças entre APIs.
ms.assetid: 7f9b136c-aa22-04b3-d385-6e9e1f38b948
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, opengl, direct3d, portabilidade
ms.localizationpriority: medium
ms.openlocfilehash: 532c2a0a9779ae3eaedb2217175dc0805514f792
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7434706"
---
# <a name="map-opengl-es-20-to-direct3d-11"></a>Correlacionar OpenGL ES 2.0 com Direct3D 11



Ao iniciar o processo de portabilidade da arquitetura gráfica do OpenGL ES 2.0 para o Direct3D pela primeira vez, familiarize-se com as principais diferenças entre APIs. Os tópicos desta seção ajudam você a planejar sua estratégia de compatibilização e as alterações de API que você deve fazer ao mover o processamento de elementos gráficos para Direct3D.
## 
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
<td align="left"><p><a href="compare-opengl-es-2-0-api-design-to-directx.md">Planejar a portabilidade do OpenGL ES 2.0 para o Direct3D</a></p></td>
<td align="left"><p>Caso esteja fazendo a portabilidade de um jogo das plataformas iOS ou Android, você provavelmente investiu bastante no OpenGL ES 2.0. Durante a preparação para mudar a base de código do pipeline gráfico para o Direct3D 11 e o Windows Runtime, há algumas coisas a se pensar antes de começar.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="moving-from-egl-to-dxgi.md">Comparar código EGL com DXGI e Direct3D</a></p></td>
<td align="left"><p>A DXGI (interface gráfica do DirectX) e várias APIs Direct3D desempenham a mesma função que EGL. Este tópico o ajuda a entender a DXGI e o Direct3D 11 do ponto de vista do EGL.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="porting-uniforms-and-attributes.md">Comparação de buffers, uniformes e atributos de vértice do OpenGL ES 2.0 com os do Direct3D</a></p></td>
<td align="left"><p>Durante o processo de compatibilização para o Direct3D 11 a partir do OpenGL ES 2.0, você deve mudar a sintaxe e o comportamento de API para passar dados entre o aplicativo e os programas sombreadores.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="change-your-shader-loading-code.md">Comparar o pipeline do sombreador do OpenGL ES 2.0 com Direct3D</a></p></td>
<td align="left"><p>Conceitualmente, o pipeline de sombreador do Direct3D 11 é bem parecido com o do OpenGL ES 2.0. Mas em termos de design da API, os componentes principais para a criação e o gerenciamento dos estágios do sombreador fazem parte de duas interfaces primárias, <a href="https://msdn.microsoft.com/library/windows/desktop/hh404575"><strong>ID3D11Device1</strong></a> e <a href="https://msdn.microsoft.com/library/windows/desktop/hh404598"><strong>ID3D11DeviceContext1</strong></a>. O objetivo deste tópico é correlacionar os padrões das APIs do pipeline de sombreador do OpenGL ES 2.0 com os equivalentes em Direct3D 11 nessas interfaces.</p></td>
</tr>
</tbody>
</table>

 

## <a name="notes-on-specific-opengl-es-20-providers"></a>Observações sobre provedores específicos do OpenGL ES 2.0


Estes tópicos usam a especificação Khronos OpenGL ES 2.0 com C sem plataforma. O iOS e o Android utilizam a mesma especificação, e o código OpenGL ES 2.0 desenvolvido para essas plataformas é muito semelhante aos trechos de código mostrados aqui, embora sejam normalmente expostos como APIs orientadas a objetos. Além disso, em função das complexidades e diferenças de linguagem de cada plataforma, pode haver pequenas diferenças, especialmente em tipos de parâmetros de métodos ou na sintaxe geral da linguagem. O iOS, por exemplo, usa Objective-C. O Android pode usar o C++, mas alguns desenvolvedores podem usar uma implementação Java pura. Com isso em mente, estes tópicos devem continuar sendo úteis, pois os conceitos, estrutura e usos gerais das APIs do OpenGL ES não são diferentes.

 

 




