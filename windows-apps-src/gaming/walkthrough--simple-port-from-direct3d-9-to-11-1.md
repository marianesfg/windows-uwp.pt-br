---
title: Passo a passo-- Portabilidade de Direct3D 9 para DirectX 11 e UWP
description: Este exercício de portabilidade mostra como levar uma estrutura de renderização simples do Direct3D 9 para o Direct3D 11 e a Plataforma Universal do Windows (UWP).
ms.assetid: d4467e1f-929b-a4b8-b233-e142a8714c96
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, directx, portabilidade, direct3d 9, direct3d 11
ms.localizationpriority: medium
ms.openlocfilehash: c7569c6b2f041f5535e0eabe934a91da86b60b9a
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8188971"
---
# <a name="walkthrough-port-a-simple-direct3d-9-app-to-directx-11-and-universal-windows-platform-uwp"></a>Passo a passo: portabilidade de um aplicativo simples em Direct3D 9 para DirectX 11 e a Plataforma Universal do Windows (UWP)



Este exercício de portabilidade mostra como levar uma estrutura de renderização simples do Direct3D 9 para o Direct3D 11 e a Plataforma Universal do Windows (UWP).
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
<td align="left"><p><a href="simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md">Inicializar o Direct3D 11</a></p></td>
<td align="left"><p>Mostra como converter o código de inicialização do Direct3D 9 para o Direct3D 11. Saiba também como obter identificadores para o dispositivo Direct3D e o contexto de dispositivo e como usar DXGI para configurar uma cadeia de troca.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="simple-port-from-direct3d-9-to-11-1-part-2--rendering.md">Converter a estrutura de renderização</a></p></td>
<td align="left"><p>Mostra como converter uma estrutura de renderização simples do Direct3D 9 para o Direct3D 11. Saiba também como fazer a portabilidade de buffers de geometria, como compilar e carregar programas sombreadores HLSL e como implementar a cadeia de renderização no Direct3D 11.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md">Fazer a portabilidade do loop do jogo</a></p></td>
<td align="left"><p>Mostra como implementar uma janela para um jogo UWP e como ativar o loop do jogo, inclusive como criar uma <a href="https://msdn.microsoft.com/library/windows/apps/hh700478"><strong>IFrameworkView</strong></a> para controlar uma <a href="https://msdn.microsoft.com/library/windows/apps/br208225"><strong>CoreWindow</strong></a> em tela inteira.</p></td>
</tr>
</tbody>
</table>

 

Este tópico mostra dois códigos que realizam a mesma tarefa básica relativa a elementos gráficos: exibir um cubo giratório sombreado por vértices. Nos dois casos, o código abrange o seguinte processo:

1.  Criar um dispositivo em Direct3D e uma cadeia de permuta.
2.  Criar um buffer de vértices e um buffer de índices para representar uma malha cúbica colorida.
3.  Criar um sombreador de vértice que transforme esses vértices em espaço na tela e um sombreador de pixel que mescle valores de cores; compilar os sombreadores; e carregá-los como recursos do Direct3D.
4.  Implementar a cadeia de renderização e apresentar o cubo desenhado na tela.
5.  Criar uma janela, iniciar um loop principal e cuidar do processamento de mensagens da janela.

Depois de concluir este guia passo a passo, você conhecerá as seguintes diferenças básicas entre o Direct3D 9 e o Direct3D 11:

-   A diferença entre dispositivo, contexto de dispositivo e infraestrutura gráfica.
-   O processo de compilação de sombreadores e carregar códigos de bytes de sombreadores em tempo de execução.
-   Como configurar dados por vértices para o estágio de IA (assembler de entrada).
-   Como usar uma [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) para criar uma exibição CoreWindow.

Observe que este passo a passo usa [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) por questão de simplicidade e não abrange a interoperabilidade com XAML.

## <a name="prerequisites"></a>Pré-requisitos


Você deve [preparar seu ambiente para o desenvolvimento do jogo da UWP em DirectX](prepare-your-dev-environment-for-windows-store-directx-game-development.md). Você ainda não precisa de um modelo, mas você precisará Microsoft Visual Studio2015 carregue os exemplos de código neste passo a passo.

Visite o tópico sobre [conceitos e considerações sobre portabilidade](porting-considerations.md) para conhecer melhor os conceitos de programação do DirectX 11 e da UWP mostrados neste guia.

## <a name="related-topics"></a>Tópicos relacionados

**Direct3D**

* [Escrevendo sombreadores HLSL no Direct3D 9](https://msdn.microsoft.com/library/windows/desktop/bb944006)
* [Modelos de projeto de jogo DirectX](user-interface.md)

**Microsoft Store**

* [**Microsoft::WRL::ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx)
* [**Identificador para o operador de objeto (^)**](https://msdn.microsoft.com/library/windows/apps/yk97tc08.aspx)

