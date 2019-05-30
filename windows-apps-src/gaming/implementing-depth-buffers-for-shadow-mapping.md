---
title: Passo a passo-- Buffers de profundidade de volume de sombra do Direct3D 11
description: Este guia passo a passo demonstra como renderizar volumes de sombra com mapas de profundidade, usando o Direct3D 11 em dispositivos com todos os níveis de recursos do Direct3D.
ms.assetid: d15e6501-1a1d-d99c-d1d8-ad79b849db90
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, directx, volumes de sombra, buffers de profundidade, directx 11
ms.localizationpriority: medium
ms.openlocfilehash: 2ce0cbd310ea89c5fa7b5c68033402f559768a24
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368516"
---
# <a name="walkthrough-implement-shadow-volumes-using-depth-buffers-in-direct3d-11"></a>Passo a passo: Implementar os volumes de sombra usando buffers de profundidade em Direct3D 11



Este guia passo a passo demonstra como renderizar volumes de sombra com mapas de profundidade, usando o Direct3D 11 em dispositivos com todos os níveis de recursos do Direct3D.
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
<td align="left"><p><a href="create-depth-buffer-resource--view--and-sampler-state.md">Criar recursos de dispositivo do buffer de profundidade</a></p></td>
<td align="left"><p>Aprenda a criar recursos de dispositivos Direct3D necessários para dar suporte a testes de profundidade para volumes de sombra.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="render-the-shadow-map-to-the-depth-buffer.md">Renderizar o mapa de sombra para o buffer de profundidade</a></p></td>
<td align="left"><p>Faça a renderização do ponto de vista da luz para criar um mapa de profundidade bidimensional que representa o volume de sombra.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="render-the-scene-with-depth-testing.md">Renderizar a cena com o teste de profundidade</a></p></td>
<td align="left"><p>Crie um efeito de sombra adicionando testes de profundidade ao sombreador de vértice (ou geometria) e ao sombreador de pixel.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="target-a-range-of-hardware.md">Suporte a mapas de sombra em um intervalo de hardware</a></p></td>
<td align="left"><p>Renderize sombras de alta fidelidade em dispositivos mais rápidos e sombras mais velozes em dispositivos com menor desempenho.</p></td>
</tr>
</tbody>
</table>

 

## <a name="shadow-mapping-application-to-direct3d-9-desktop-porting"></a>Aplicativo de mapeamento de sombra para portabilidade de área de trabalho no Direct3D 9


Windows 8 adde funcionalidade de comparação de profundidade d para o nível de recurso 9\_1 e 9\_3. Agora você pode migrar o código de renderização com volumes de sombra para o DirectX 11, e o renderizador do Direct3D 11 será compatível com dispositivos de nível 9. Este guia passo a passo mostra como qualquer aplicativo ou jogo em Direct3D 11 pode implementar volumes de sombra tradicionais usando testes de profundidade. O código abrange o seguinte processo:

1.  Criando recursos de dispositivo Direct3D para mapeamento de sombra.
2.  Adicionando uma passagem de renderização para criar o mapa de profundidade.
3.  Adicionando teste de profundidade para a passagem de renderização principal.
4.  Implementando o código de sombreador necessário.
5.  Opções para renderização rápida em hardware de nível inferior.

Depois de concluir este passo a passo, você deve estar familiarizado com como implementar uma técnica de volume básico de sombra compatível em Direct3D 11 que seja compatível com o nível de recurso 9\_1 e acima.

## <a name="prerequisites"></a>Pré-requisitos


Você deve [Preparar seu ambiente de desenvolvimento de jogos UWP (Plataforma Universal do Windows) no DirectX](prepare-your-dev-environment-for-windows-store-directx-game-development.md). Não é necessário um modelo ainda, mas será necessário o Microsoft Visual Studio 2015 para criar o exemplo de código para este passo a passo.

## <a name="related-topics"></a>Tópicos relacionados


**Direct3D**

* [Escrevendo sombreadores HLSL no Direct3D 9](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-writing-shaders-9)
* [Criar um novo projeto do DirectX 11 para UWP](user-interface.md)

**Artigos técnicos de mapeamento de sombra**

* [Técnicas comuns para melhorar os mapas de intensidade da sombra](https://docs.microsoft.com/windows/desktop/DxTechArts/common-techniques-to-improve-shadow-depth-maps)
* [Mapas de sombra em cascata](https://docs.microsoft.com/windows/desktop/DxTechArts/cascaded-shadow-maps)

 

 




