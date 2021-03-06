---
title: Fazer portabilidade de OpenGL ES 2.0 para Direct3D 11
description: Inclui artigos, visões gerais e guias passo a passo para a portabilidade de uma pipeline de elementos gráficos em OpenGL ES 2.0 para um Direct3D 11 e o Windows Runtime.
ms.assetid: 1e1cf668-a15f-0c7b-8daf-3260d27c6d9c
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, opengl, direct3d 11, portabilidade, gráficos
ms.localizationpriority: medium
ms.openlocfilehash: 9215c06c97fb8e45a445ccd3691f484a6b2ad513
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258461"
---
# <a name="port-from-opengl-es-20-to-direct3d-11"></a>Fazer portabilidade de OpenGL ES 2.0 para Direct3D 11



Inclui artigos, visões gerais e guias passo a passo para a portabilidade de uma pipeline de elementos gráficos em OpenGL ES 2.0 para um Direct3D 11 e o Windows Runtime.

> **Observe**   uma etapa intermediária para portar seu projeto OpenGL ES 2,0 é usar o ângulo para Microsoft Store. O ANGLE permite executar conteúdo do OpenGL ES no Windows, o que converte chamadas de API do OpenGL ES em chamadas de API do DirectX 11. Para obter mais informações sobre o ANGLE, vá para [ANGLE for Microsoft Store Wiki](https://github.com/microsoft/angle/wiki).

 

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
<td align="left"><p><a href="map-concepts-and-infrastructure.md">Mapear OpenGL ES 2,0 para o Direct3D 11,1</a></p></td>
<td align="left"><p>Ao iniciar o processo de portabilidade da arquitetura gráfica do OpenGL ES 2.0 para o Direct3D pela primeira vez, familiarize-se com as principais diferenças entre APIs. Os tópicos desta seção ajudam você a planejar sua estratégia de portabilidade e as alterações de API que você deve fazer ao mover o processamento de elementos gráficos para Direct3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md">Como: porta um processador OpenGL ES 2,0 simples para o Direct3D 11,1</a></p></td>
<td align="left"><p>Para este exercício de portabilidade, vamos começar com noções básicas: trazer um renderizador simples para um cubo giratório com sombreamento de vértice do OpenGL ES 2.0 para Direct3D, que corresponda ao modelo de aplicativo do DirectX 11 (Windows Universal) do Visual Studio 2015.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="opengl-es-2-0-to-directx-11-1-reference.md">Referência de OpenGL ES 2,0 a Direct3D 11,1</a></p></td>
<td align="left"><p>Use estes tópicos de referência para procurar por mapeamento de API e exemplos de código curtos quando fizer a portabilidade de OpenGL ES 2.0 para Direct3D 11.</p></td>
</tr>
</tbody>
</table>

 

 

 




