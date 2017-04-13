---
author: mtoepke
title: Correlacionar OpenGL ES 2.0 com Direct3D 11
description: "Ao iniciar o processo de portabilidade da arquitetura gráfica do OpenGL ES 2.0 para o Direct3D pela primeira vez, familiarize-se com as principais diferenças entre APIs."
ms.assetid: 7f9b136c-aa22-04b3-d385-6e9e1f38b948
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, jogos, opengl, direct3d, portabilidade
ms.openlocfilehash: 1298f165444b31c75ca9d98f04eb82a58be46e5b
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="map-opengl-es-20-to-direct3d-11"></a>Correlacionar OpenGL ES 2.0 com Direct3D 11


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

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
<td align="left"><p>[Planejar a portabilidade do OpenGL ES 2.0 para o Direct3D](compare-opengl-es-2-0-api-design-to-directx.md)</p></td>
<td align="left"><p>Caso esteja fazendo a portabilidade de um jogo das plataformas iOS ou Android, você provavelmente investiu bastante no OpenGL ES 2.0. Durante a preparação para mudar a base de código do pipeline gráfico para o Direct3D 11 e o Windows Runtime, há algumas coisas a se pensar antes de começar.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Comparar código EGL com DXGI e Direct3D](moving-from-egl-to-dxgi.md)</p></td>
<td align="left"><p>A DXGI (interface gráfica do DirectX) e várias APIs Direct3D desempenham a mesma função que EGL. Este tópico o ajuda a entender a DXGI e o Direct3D 11 do ponto de vista do EGL.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Comparação de buffers, uniformes e atributos de vértice do OpenGL ES 2.0 com os do Direct3D](porting-uniforms-and-attributes.md)</p></td>
<td align="left"><p>Durante o processo de compatibilização para o Direct3D 11 a partir do OpenGL ES 2.0, você deve mudar a sintaxe e o comportamento de API para passar dados entre o aplicativo e os programas sombreadores.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Comparar o pipeline do sombreador do OpenGL ES 2.0 com Direct3D](change-your-shader-loading-code.md)</p></td>
<td align="left"><p>Conceitualmente, o pipeline de sombreador do Direct3D 11 é bem parecido com o do OpenGL ES 2.0. Em termos de design de API, no entanto, os componentes principais para criação e gerenciamento dos estágios de sombreador são partes de duas interfaces primárias, [<strong>ID3D11Device1</strong>] (https://msdn.microsoft.com/library/windows/desktop/hh404575) e [<strong>ID3D11DeviceContext1</strong>] (https://msdn.microsoft.com/library/windows/desktop/hh404598). O objetivo deste tópico é correlacionar os padrões das APIs do pipeline de sombreador do OpenGL ES 2.0 com os equivalentes em Direct3D 11 nessas interfaces.</p></td>
</tr>
</tbody>
</table>

 

## <a name="notes-on-specific-opengl-es-20-providers"></a>Observações sobre provedores específicos do OpenGL ES 2.0


Estes tópicos usam a especificação Khronos OpenGL ES 2.0 com C sem plataforma. O iOS e o Android utilizam a mesma especificação, e o código OpenGL ES 2.0 desenvolvido para essas plataformas é muito semelhante aos trechos de código mostrados aqui, embora sejam normalmente expostos como APIs orientadas a objetos. Além disso, em função das complexidades e diferenças de linguagem de cada plataforma, pode haver pequenas diferenças, especialmente em tipos de parâmetros de métodos ou na sintaxe geral da linguagem. O iOS, por exemplo, usa Objective-C. O Android pode usar o C++, mas alguns desenvolvedores podem usar uma implementação Java pura. Com isso em mente, estes tópicos devem continuar sendo úteis, pois os conceitos, estrutura e usos gerais das APIs do OpenGL ES não são diferentes.

 

 




