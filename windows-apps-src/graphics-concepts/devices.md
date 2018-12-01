---
title: Dispositivos
description: Um dispositivo Direct3D é o componente de renderização do Direct3D. Um dispositivo encapsula e armazena o estado de renderização, executa transformações e operações de iluminação e rasteriza uma imagem para uma superfície.
ms.assetid: BC903462-A32A-46BA-8411-FB294F5D2CD9
keywords:
- Dispositivos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9e49a9dcaa2638065946f01797cbea084a1432a6
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8341425"
---
# <a name="devices"></a>Dispositivos


Um dispositivo Direct3D é o componente de renderização do Direct3D. Um dispositivo encapsula e armazena o estado de renderização, executa transformações e operações de iluminação e rasteriza uma imagem para uma superfície.

Arquitetonicamente, os dispositivos Direct3D contêm um módulo de transformação, um módulo de iluminação e um módulo de rasterização, como mostra o diagrama a seguir.

![diagrama da arquitetura do dispositivo direct3d](images/d3ddev.png)

O Direct3D dá suporte a dois tipos principais de dispositivos Direct3D:

-   Um dispositivo hal com rasterização e sombreamento acelerados por hardware com processamento de vértice de hardware e software
-   Um dispositivo de referência

Esses dispositivos são dois drivers separados. Os dispositivos de referência e software são representados por drivers de software, e o dispositivo hal é representado por um driver de hardware. A maneira mais comum de tirar proveito desses dispositivos é usar o dispositivo hal para envio de aplicativos e o dispositivo de referência para o teste de recurso. Eles são fornecidos por terceiros para emular dispositivos específicos - por exemplo, um hardware de desenvolvimento que ainda não foi liberado.

O dispositivo Direct3D que cria um aplicativo deve corresponder aos recursos do hardware no qual o aplicativo é executado. O Direct3D fornece recursos de renderização, acessando o hardware 3D que está instalado no computador ou emulando as capacidades de hardware 3D no software. Portanto, o Direct3D fornece dispositivos para o acesso de hardware e a emulação de software.

Os dispositivos com aceleração de hardware oferecem desempenho muito melhor que dispositivos de software. O tipo de dispositivo hal está disponível em todos os adaptadores gráficos do Direct3D compatíveis. Na maioria dos casos, os aplicativos têm como foco computadores que têm aceleração de hardware e dependem de emulação de software para acomodar computadores inferiores.

Com exceção do dispositivo de referência, os dispositivos de software não dão suporte sempre aos mesmos recursos que um dispositivo de hardware. Os aplicativos sempre devem consultar recursos de dispositivo para determinar quais recursos têm suporte.

Como o comportamento dos dispositivos de referência e software fornecidos com o Direct3D 9 é idêntico ao do dispositivo hal, o código de aplicativo criado para funcionar com o dispositivo hal funcionará com os dispositivos de software ou referência sem modificações. O comportamento do dispositivo de referência ou software fornecido é idêntico ao do dispositivo hal, mas os recursos do dispositivo variam, e um dispositivo de software específico pode implementar um conjunto menor de recursos.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>Nesta seção


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
<td align="left"><p><a href="device-types.md">Tipos de dispositivos</a></p></td>
<td align="left"><p>Os tipos de dispositivos Direct3D incluem dispositivos de camada de abstração de hardware (hal) e o rasterizador de referência.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="windowed-vs--full-screen-mode.md">Modo de janela versus modo de tela inteira</a></p></td>
<td align="left"><p>Os aplicativos Direct3D podem ser executados em um destes modos: janela ou tela inteira. No <em>modo de janela</em>, o aplicativo compartilha o espaço disponível na tela da área de trabalho com todos os aplicativos em execução. No <em>modo de tela inteira</em>, a janela em que o aplicativo é executado abrange a área de trabalho inteira, ocultando todos os aplicativos em execução (incluindo o ambiente de desenvolvimento).</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="lost-devices.md">Dispositivos perdidos</a></p></td>
<td align="left"><p>Um dispositivo Direct3D pode estar em um estado operacional ou um estado perdido. O estado <em>operacional</em> é o estado normal do dispositivo no qual o dispositivo é executado e apresenta toda a renderização conforme o esperado. O dispositivo faz uma transição para o <em>perdido</em> estado quando um evento, como perda de foco do teclado em um aplicativo de tela inteira, faz com que a renderização para se tornar impossível.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="swap-chains.md">Cadeias de troca</a></p></td>
<td align="left"><p>Uma cadeia de troca é uma coleção de buffers que são usados para exibir quadros para o usuário. Cada vez que um aplicativo apresenta um novo quadro para exibição, o primeiro buffer da cadeia de troca assume o lugar do buffer exibido. Esse processo é chamado <em>troca</em> ou <em>inversão</em>.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="introduction-to-rasterization-rules.md">Introdução às regras de rasterização</a></p></td>
<td align="left"><p>Muitas vezes, os pontos especificados para vértices não correspondem exatamente aos pixels na tela. Quando isso acontece, o Direct3D aplica as regras de rasterização de triângulo para decidir quais pixels se aplicam a um determinado triângulo.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Guia de aprendizagem de Gráficos do Direct3D](index.md)

 

 




