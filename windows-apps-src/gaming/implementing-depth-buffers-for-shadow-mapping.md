---
author: mtoepke
title: Passo a passo-- Buffers de profundidade de volume de sombra do Direct3D 11
description: Este guia passo a passo demonstra como renderizar volumes de sombra com mapas de profundidade, usando o Direct3D 11 em dispositivos com todos os níveis de recursos do Direct3D.
ms.assetid: d15e6501-1a1d-d99c-d1d8-ad79b849db90
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, directx, volumes de sombra, buffers de profundidade, directx 11
ms.localizationpriority: medium
ms.openlocfilehash: 269f8423795d7c2515ce555cf8e65f14a8c63431
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/14/2018
ms.locfileid: "6834686"
---
# <a name="walkthrough-implement-shadow-volumes-using-depth-buffers-in-direct3d-11"></a>Guia passo a passo: implementar volumes de sombra usando buffers de profundidade no Direct3D 11



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
<td align="left"><p><a href="create-depth-buffer-resource--view--and-sampler-state.md">Criar recursos de dispositivo de buffer de profundidade</a></p></td>
<td align="left"><p>Aprenda a criar recursos de dispositivos Direct3D necessários para dar suporte a testes de profundidade para volumes de sombra.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="render-the-shadow-map-to-the-depth-buffer.md">Renderizar o mapa de sombra para o buffer de profundidade</a></p></td>
<td align="left"><p>Faça a renderização do ponto de vista da luz para criar um mapa de profundidade bidimensional que representa o volume de sombra.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="render-the-scene-with-depth-testing.md">Renderizar a cena com teste de profundidade</a></p></td>
<td align="left"><p>Crie um efeito de sombra adicionando testes de profundidade ao sombreador de vértice (ou geometria) e ao sombreador de pixel.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="target-a-range-of-hardware.md">Suporte a mapas de sombra em diversos hardwares</a></p></td>
<td align="left"><p>Renderize sombras de alta fidelidade em dispositivos mais rápidos e sombras mais velozes em dispositivos com menor desempenho.</p></td>
</tr>
</tbody>
</table>

 

## <a name="shadow-mapping-application-to-direct3d-9-desktop-porting"></a>Aplicativo de mapeamento de sombra para portabilidade de área de trabalho no Direct3D 9


Windows8 adicionou funcionalidade de comparação de profundidade para recurso nível 9 \ 1 e 9\_3. Agora você pode migrar o código de renderização com volumes de sombra para o DirectX 11, e o renderizador do Direct3D 11 será compatível com dispositivos de nível 9. Este guia passo a passo mostra como qualquer aplicativo ou jogo em Direct3D 11 pode implementar volumes de sombra tradicionais usando testes de profundidade. O código abrange o seguinte processo:

1.  Criando recursos de dispositivo Direct3D para mapeamento de sombra.
2.  Adicionando uma passagem de renderização para criar o mapa de profundidade.
3.  Adicionando teste de profundidade para a passagem de renderização principal.
4.  Implementando o código de sombreador necessário.
5.  Opções para renderização rápida em hardware de nível inferior.

Após a conclusão deste guia passo a passo, você estará familiarizado com a implementação de uma técnica básica de volume de sombra no Direct3D 11 que é compatível com o nível 9\_1 e acima.

## <a name="prerequisites"></a>Pré-requisitos


Você deve [Preparar seu ambiente de desenvolvimento de jogos UWP (Plataforma Universal do Windows) no DirectX](prepare-your-dev-environment-for-windows-store-directx-game-development.md). Você ainda não precisa de um modelo, mas você precisará Microsoft Studio2015 Visual para criar o código de exemplo neste passo a passo.

## <a name="related-topics"></a>Tópicos relacionados


**Direct3D**

* [Escrevendo sombreadores HLSL no Direct3D 9](https://msdn.microsoft.com/library/windows/desktop/bb944006)
* [Criar um novo projeto do DirectX 11 para a UWP](user-interface.md)

**Artigos técnicos sobre mapeamento de sombra**

* [Técnicas comuns para aprimorar os mapas de profundidade de sombra](https://msdn.microsoft.com/library/windows/desktop/ee416324)
* [Mapas de sombra em cascata](https://msdn.microsoft.com/library/windows/desktop/ee416307)

 

 




