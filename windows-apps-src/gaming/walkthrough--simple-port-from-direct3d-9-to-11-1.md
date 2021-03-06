---
title: Passo a passo-- Portabilidade de Direct3D 9 para DirectX 11 e UWP
description: Este exercício de portabilidade mostra como levar uma estrutura de renderização simples do Direct3D 9 para o Direct3D 11 e a Plataforma Universal do Windows (UWP).
ms.assetid: d4467e1f-929b-a4b8-b233-e142a8714c96
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, directx, portabilidade, direct3d 9, direct3d 11
ms.localizationpriority: medium
ms.openlocfilehash: 5d4aef73b9b28d631a492436ff90761541134220
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367422"
---
# <a name="walkthrough-port-a-simple-direct3d-9-app-to-directx-11-and-universal-windows-platform-uwp"></a>Passo a passo: Portar um aplicativo simple do Direct3D 9 DirectX 11 e plataforma Universal do Windows (UWP)



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
<td align="left"><p><a href="simple-port-from-direct3d-9-to-11-1-part-2--rendering.md">Converter o framework de renderização</a></p></td>
<td align="left"><p>Mostra como converter uma estrutura de renderização simples do Direct3D 9 para o Direct3D 11. Saiba também como fazer a portabilidade de buffers de geometria, como compilar e carregar programas sombreadores HLSL e como implementar a cadeia de renderização no Direct3D 11.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md">O loop do jogo da porta</a></p></td>
<td align="left"><p>Mostra como implementar uma janela para um jogo UWP e como ativar o loop do jogo, inclusive como criar uma <a href="https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView"><strong>IFrameworkView</strong></a> para controlar uma <a href="https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow"><strong>CoreWindow</strong></a> em tela inteira.</p></td>
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
-   Como usar uma [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) para criar uma exibição CoreWindow.

Observe que este passo a passo usa [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) por questão de simplicidade e não abrange a interoperabilidade com XAML.

## <a name="prerequisites"></a>Pré-requisitos


Você deve [preparar seu ambiente para o desenvolvimento do jogo da UWP em DirectX](prepare-your-dev-environment-for-windows-store-directx-game-development.md). Não é necessário um modelo ainda, mas será necessário o Microsoft Visual Studio 2015 para carregar os exemplos de código para este passo a passo.

Visite o tópico sobre [conceitos e considerações sobre portabilidade](porting-considerations.md) para conhecer melhor os conceitos de programação do DirectX 11 e da UWP mostrados neste guia.

## <a name="related-topics"></a>Tópicos relacionados

**Direct3D**

* [Escrevendo sombreadores HLSL no Direct3D 9](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-writing-shaders-9)
* [Modelos de projeto de jogo do DirectX](user-interface.md)

**Microsoft Store**

* [**Microsoft::WRL::ComPtr**](https://docs.microsoft.com/cpp/windows/comptr-class)
* [**Operador Handle to Object (^)** ](https://docs.microsoft.com/cpp/windows/handle-to-object-operator-hat-cpp-component-extensions)

