---
author: joannaleecy
title: "Otimização e tópicos avançados para jogos DirectX"
description: "Otimização e tópicos avançados para programação de jogos DirectX."
ms.assetid: b5f29fb2-3bcf-43b2-9a68-f8819473bf62
ms.author: joanlee
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, jogo, directx, otimizar, multisampling, cadeias de troca
ms.openlocfilehash: 9fb2c8c92cdf5a498f3e3db4d9250c9dcdfe204f
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="optimization-and-advanced-topics-for-directx-games"></a>Otimização e tópicos avançados para jogos DirectX

Esta seção fornece informações sobre como otimizar o desempenho do jogo DirectX e outros tópicos avançados.

O tópico sobre programação assíncrona para jogos discute os diversos pontos a serem considerados quando você quiser usar a programação assíncrona para paralelizar alguns dos componentes e usar multithreading para maximizar o uso de uma GPU poderosa.

O tópico sobre como lidar com cenários removidos de dispositivo no Direct3D usa um passo a passo para explicar como os jogos desenvolvidos usando o Direct3D 11 podem detectar e responder a situações onde o adaptador gráfico é restaurado, removido ou alterado.

O tópico sobre multisampling em aplicativos UWP fornece uma visão geral de como usar a suavização de multisampling, uma técnica gráfica para reduzir a aparência de bordas suavizadas em jogos UWP criados com o Direct3D.

O tópico sobre como otimizar o loop de entrada e de renderização fornece diretrizes sobre como escolher a opção correta de processamento de eventos de entrada para gerenciar a latência de entrada e otimizar o loop de renderização.

O tópico sobre a redução da latência com cadeias de troca do DXGI 1.3 explica como reduzir a latência de quadros de forma efetiva ao aguardar a cadeia de troca para sinalizar o tempo apropriado para começar a renderizar um novo quadro.

O tópico sobre o dimensionamento e as sobreposições de cadeia de troca explica como melhorar os tempos de renderização usando cadeias de troca dimensionadas para renderizar conteúdo de jogo em tempo real em resolução mais baixa do que a capacidade nativa da tela. Ele também explica como criar cadeias de troca de sobreposição para dispositivos com o recurso de sobreposição de hardware; essa técnica pode ser usada para aliviar o problema de uma interface do usuário de escala reduzida decorrente do uso de dimensionamento da cadeia de troca.

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
<td align="left"><p>[Programação assíncrona para jogos](asynchronous-programming-directx-and-cpp.md)</p></td>
<td align="left"><p>Entenda a programação assíncrona e o threading com o DirectX.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Lidar com cenários removidos de dispositivos no Direct3D 11](handling-device-lost-scenarios.md)</p></td>
<td align="left"><p>Recrie a cadeia de interface do dispositivo Direct3D e DXGI quando o adaptador gráfico é removido ou reinicializado.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Multisampling em aplicativos UWP](multisampling--multi-sample-anti-aliasing--in-windows-store-apps.md)</p></td>
<td align="left"><p>Use multisampling em jogos UWP criados usando o Direct3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Otimizar entrada e loop de renderização](optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md)</p></td>
<td align="left"><p>Reduza a latência de entrada e otimize o loop de renderização.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Reduzir a latência com cadeias de troca do DXGI 1.3](reduce-latency-with-dxgi-1-3-swap-chains.md)</p></td>
<td align="left"><p>Use o DXGI 1.3 para reduzir a latência de quadros efetiva.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Dimensionamento e sobreposições de cadeia de troca](multisampling--scaling--and-overlay-swap-chains.md)</p></td>
<td align="left"><p>Crie cadeias de troca dimensionadas para permitir renderização mais rápida em dispositivos móveis e usar cadeias de troca sobrepostas para aumentar a qualidade visual.</p></td>
</tr>
</tbody>
</table>