---
author: mtoepke
title: Passo a passo -- Portabilidade de um aplicativo simples em Direct3D 9 para DirectX 11 e a Plataforma Universal do Windows (UWP)
description: Este exercício de portabilidade mostra como levar uma estrutura de renderização simples do Direct3D 9 para o Direct3D 11 e a Plataforma Universal do Windows (UWP).
ms.assetid: d4467e1f-929b-a4b8-b233-e142a8714c96
---

# Passo a passo: portabilidade de um aplicativo simples em Direct3D 9 para DirectX 11 e a Plataforma Universal do Windows (UWP)


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

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
<td align="left"><p>[Inicializar o Direct3D 11](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)</p></td>
<td align="left"><p>Mostra como converter o código de inicialização do Direct3D 9 para o Direct3D 11. Saiba também como obter identificadores para o dispositivo Direct3D e o contexto de dispositivo e como usar DXGI para configurar uma cadeia de troca.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Converter a estrutura de renderização](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)</p></td>
<td align="left"><p>Mostra como converter uma estrutura de renderização simples do Direct3D 9 para o Direct3D 11. Saiba também como fazer a portabilidade de buffers de geometria, como compilar e carregar programas sombreadores HLSL e como implementar a cadeia de renderização no Direct3D 11.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Fazer a portabilidade do loop do jogo](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md)</p></td>
<td align="left"><p>Mostra como implementar uma janela para um jogo UWP e como ativar o loop do jogo, inclusive como criar um [<strong>IFrameworkView</strong>](https://msdn.microsoft.com/library/windows/apps/hh700478) para controlar um [<strong>CoreWindow</strong>] de tela inteira (https://msdn.microsoft.com/library/windows/apps/br208225).</p></td>
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

## Pré-requisitos


Você deve [preparar seu ambiente para o desenvolvimento do jogo da UWP em DirectX](prepare-your-dev-environment-for-windows-store-directx-game-development.md). Você ainda não precisa de um modelo, mas é necessário que o Microsoft Visual Studio 2015 carregue os exemplos de códigos neste guia passo a passo.

Visite o tópico sobre [conceitos e considerações sobre portabilidade](porting-considerations.md) para conhecer melhor os conceitos de programação do DirectX 11 e da UWP mostrados neste guia.

## Tópicos relacionados


**Direct3D**
            
          
            [Escrevendo sombreadores HLSL no Direct3D 9](https://msdn.microsoft.com/library/windows/desktop/bb944006)

[Criar um novo projeto do DirectX 11 para a UWP](user-interface.md)

**Windows Store**
            
          
            [
              **Microsoft::WRL::ComPtr**
            ](https://msdn.microsoft.com/library/windows/apps/br244983.aspx)

[**Manipular o operador de objeto (^)**] https://msdn.microsoft.com/library/windows/apps/yk97tc08.aspx.

 

 






<!--HONumber=May16_HO2-->


