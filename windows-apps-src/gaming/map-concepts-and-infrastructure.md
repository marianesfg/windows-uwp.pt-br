---
title: Correlacionar OpenGL ES 2.0 com Direct3D 11
description: Ao iniciar o processo de portabilidade da sua arquitetura de elementos gráficos do OpenGL ES 2.0 para o Direct3D pela primeira vez, familiarize-se com as diferenças principais entre as APIs.
ms.assetid: 7f9b136c-aa22-04b3-d385-6e9e1f38b948
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, opengl, direct3d, portabilidade
ms.localizationpriority: medium
ms.openlocfilehash: 525b97700b1362bb19a1b328183f3cbf9da3b006
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368523"
---
# <a name="map-opengl-es-20-to-direct3d-11"></a>Correlacionar OpenGL ES 2.0 com Direct3D 11



Ao iniciar o processo de portabilidade da sua arquitetura de elementos gráficos do OpenGL ES 2.0 para o Direct3D pela primeira vez, familiarize-se com as diferenças principais entre as APIs. Os tópicos desta seção ajudam você a planejar sua estratégia de portabilidade e as alterações de API que você deve fazer ao mover o processamento de elementos gráficos para Direct3D.
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
<td align="left"><p><a href="compare-opengl-es-2-0-api-design-to-directx.md">Planejar sua porta do OpenGL ES 2.0 ao Direct3D</a></p></td>
<td align="left"><p>Caso esteja fazendo a portabilidade de um jogo das plataformas iOS ou Android, você provavelmente investiu bastante no OpenGL ES 2.0. Durante a preparação para mudar a base de código do pipeline gráfico para o Direct3D 11 e o Windows Runtime, há algumas coisas a se pensar antes de começar.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="moving-from-egl-to-dxgi.md">Compare o código EGL DXGI e Direct3D</a></p></td>
<td align="left"><p>A DXGI (interface gráfica do DirectX) e várias APIs Direct3D desempenham a mesma função que EGL. Este tópico o ajuda a entender a DXGI e o Direct3D 11 do ponto de vista do EGL.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="porting-uniforms-and-attributes.md">Comparar buffers, uniformes e atributos de vértice para o Direct3D OpenGL ES 2.0</a></p></td>
<td align="left"><p>Durante o processo de portabilidade do OpenGL ES 2.0 para Direct3D 11, você deve alterar a sintaxe e o comportamento da API para passar dados entre o aplicativo e os programas de sombreador.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="change-your-shader-loading-code.md">Comparar o pipeline de sombreador do OpenGL ES 2.0 ao Direct3D</a></p></td>
<td align="left"><p>Conceitualmente, o pipeline de sombreador do Direct3D 11 é bem parecido com o do OpenGL ES 2.0. Mas em termos de design da API, os componentes principais para a criação e o gerenciamento dos estágios do sombreador fazem parte de duas interfaces primárias, <a href="https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1"><strong>ID3D11Device1</strong></a> e <a href="https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1"><strong>ID3D11DeviceContext1</strong></a>. O objetivo deste tópico é correlacionar os padrões das APIs do pipeline de sombreador do OpenGL ES 2.0 com os equivalentes em Direct3D 11 nessas interfaces.</p></td>
</tr>
</tbody>
</table>

 

## <a name="notes-on-specific-opengl-es-20-providers"></a>Observações sobre provedores específicos do OpenGL ES 2.0


Estes tópicos usam a especificação Khronos OpenGL ES 2.0 com C sem plataforma. O iOS e o Android utilizam a mesma especificação, e o código OpenGL ES 2.0 desenvolvido para essas plataformas é muito semelhante aos trechos de código mostrados aqui, embora sejam normalmente expostos como APIs orientadas a objetos. Além disso, em função das complexidades e diferenças de linguagem de cada plataforma, pode haver pequenas diferenças, especialmente em tipos de parâmetros de métodos ou na sintaxe geral da linguagem. O iOS, por exemplo, usa Objective-C. O Android pode usar o C++, mas alguns desenvolvedores podem usar uma implementação Java pura. Com isso em mente, estes tópicos devem continuar sendo úteis, pois os conceitos, estrutura e usos gerais das APIs do OpenGL ES não são diferentes.

 

 




