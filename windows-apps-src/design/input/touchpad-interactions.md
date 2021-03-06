---
Description: Crie aplicativos do Windows com experiências de interação do usuário intuitiva e distinta que são otimizadas para o Touchpad, mas são funcionalmente consistentes entre dispositivos de entrada.
title: Interações por touchpad
ms.assetid: CEDEA30A-FE94-4553-A7FB-6C1FA44F06AB
label: Touchpad interactions
template: detail.hbs
keywords: touchpad, PTP, toque, ponteiro, entrada, interação do usuário
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ffc3ce96c7e8c2ad4a34aecd1ca85ff644bdef97
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234482"
---
# <a name="touchpad-design-guidelines"></a>Diretrizes de design do touchpad


Projete seu app de forma que os usuários possam interagir com ele por meio de um touchpad. Um touchpad combina a entrada multi-touch indireta com a entrada de precisão de um dispositivo apontador, como um mouse. Essa combinação torna o touchpad adequado para uma interface do usuário otimizada para touch e destinos menores de aplicativos de produtividade.

 

![touchpad](images/input-patterns/input-touchpad.jpg)


Interações de touchpad requerem três coisas:

-   Um touchpad padrão ou um Windows Precision Touchpad.

    Os touchpads de precisão são otimizados para dispositivos de aplicativos do Windows. Eles permitem que o sistema manipule determinados aspectos da experiência de touchpad de maneira nativa, como rastreamento do dedo e detecção da palma, para uma experiência mais consistente entre todos os dispositivos.

-   O contato direto de um ou mais dedos no touchpad.
-   Movimento de contatos por toque (ou a ausência deles, com base em um limite de tempo).

Os dados de entrada fornecidos pelo sensor de touchpad podem ser:

-   Interpretados como um gesto físico para a manipulação direta de um ou mais elementos de interface do usuário (como movimento panorâmico, giro, redimensionamento ou mudanças). Por outro lado, a interação com um elemento por meio de sua janela de propriedades ou outra caixa de diálogo é considerada uma manipulação indireta.
-   Reconhecidos como um método de entrada alternativo, como o mouse ou a caneta.
-   Usados para complementar ou modificar aspectos de outros métodos de entrada, como borrar um traço de tinta desenhado com uma caneta.

Um touchpad combina a entrada multitoque indireta com a entrada de precisão de um dispositivo apontador, como um mouse. Essa combinação torna o touchpad adequado à interface do usuário otimizada para toque e aos destinos geralmente menores de aplicativos de produtividade e do ambiente de área de trabalho. Otimize seu design de aplicativo do Windows para entrada por toque e obtenha suporte a Touchpad por padrão.

Devido à convergência de experiências de interação compatíveis com touchpads, recomendamos o uso do evento [**PointerEntered**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered) para fornecer comandos de interface do usuário estilo mouse, além do suporte interno para entrada touch. Por exemplo, use os botões anterior e próximo para permitir que os usuários virem as páginas de conteúdo e também movimentem o conteúdo panoramicamente.

Os gestos e as diretrizes discutidos neste tópico podem ajudar a garantir que seu aplicativo dê suporte à entrada por touchpad de forma integrada e com um mínimo de código.

## <a name="the-touchpad-language"></a>A linguagem do touchpad


Um conjunto conciso de interações por touchpad é usado de forma consistente em todo o sistema. Otimize seu aplicativo para a entrada por toque e mouse. Essa linguagem permite que o aplicativo se comporte de modo familiar aos seus usuários, aumentando o nível de confiança deles e tornando o aplicativo mais fácil de aprender e usar.

Os usuários podem definir muito mais gestos de touchpad de precisão e comportamentos de interação do que podem para um touchpad padrão. Essas duas imagens mostram as páginas de configurações de touchpad diferentes de Configurações&gt; Dispositivos&gt; Mouse e touchpad para um touchpad padrão e um touchpad de precisão, respectivamente.

![configurações de touchpad padrão](images/mouse-touchpad-settings-standard.png)

<sup>Configurações padrão do \\ Touchpad \\</sup>

![configurações do windows precision touchpad](images/mouse-touchpad-settings-ptp.png)

<sup>Configurações do Touchpad do Windows \\ Precision \\ \\</sup>

Aqui estão alguns exemplos de gestos otimizados de touchpad para realizar tarefas comuns.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Termo</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>Toque com três dedos</p></td>
<td align="left"><p>Preferência do usuário para pesquisar com a <strong>Cortana</strong> ou mostrar <strong>Central de Ações</strong>.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Deslizar usando três dedos</p></td>
<td align="left"><p>Preferência de usuário para abrir a Vista de Tarefa da área de trabalho virtual, mostrar área de trabalho ou alternar aplicativos abertos.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Toque único para ação primária</p></td>
<td align="left"><p>Com um único dedo, toque em um elemento e invoque sua ação primária (por exemplo, iniciar um aplicativo ou executar um comando).</p></td>
</tr>
<tr class="even">
<td align="left"><p>Toque com dois dedos para clique com o botão direito do mouse</p></td>
<td align="left"><p>Toque com dois dedos simultaneamente em um elemento para selecioná-lo e exibir os comandos contextuais.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Toque com dois dedos para movimento panorâmico</p></td>
<td align="left"><p>O deslizamento é usado principalmente para interações de movimento panorâmico, mas também pode ser usado para movimentação, desenho ou escrita.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Pinçar e ampliar para zoom</p></td>
<td align="left"><p>Os gestos de pinçar e ampliar são geralmente usados para redimensionamento e zoom semântico.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Pressionar com toque único e deslizar para reorganizar</p></td>
<td align="left"><p>Arraste um elemento.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Pressionar com toque único e deslizar para selecionar texto</p></td>
<td align="left"><p>Pressione dentro do texto selecionável e deslize para selecioná-lo. Toque duas vezes para selecionar uma palavra.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Zona de clique com o botão esquerdo e direito do mouse</p></td>
<td align="left"><p>Emule a funcionalidade do botão esquerdo e direito de um dispositivo de mouse.</p></td>
</tr>
</tbody>
</table>

 

## <a name="hardware"></a>Hardware


Consulte os recursos de dispositivo do mouse ([**MouseCapabilities**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input.MouseCapabilities)) para identificar os aspectos da interface do usuário do seu aplicativo que o hardware de touchpad pode acessar diretamente. Recomendamos que você forneça uma interface do usuário para entrada por toque e mouse.

Para saber mais sobre como consultar os recursos do dispositivo, veja [Identificar dispositivos de entrada](identify-input-devices.md).

## <a name="visual-feedback"></a>Feedback visual


-   Quando o cursor do touchpad for detectado (por eventos de movimentação ou focalização), mostre a interface do usuário específica do mouse para indicar a funcionalidade exposta pelo elemento. Se o cursor do touchpad não for movimentado por algum tempo ou se o usuário começar uma interação por toque, faça com que a interface do usuário do touchpad desapareça gradualmente. Dessa forma, a interface do usuário fica mais organizada.
-   Não use o cursor para comentário de foco; o comentário apresentado pelo elemento é suficiente (veja a seção Cursores, a seguir).
-   Não exiba comentários sobre elementos visuais quando o elemento não der suporte a interação (como um texto estático).
-   Não use retângulos de foco nas interações por touchpad; Reserve-os para as interações por teclado.
-   Exiba respostas visuais simultaneamente para todos os elementos que representam o mesmo destino de entrada.

Para obter diretrizes gerais sobre feedback visual, veja [Diretrizes de feedback visual](https://docs.microsoft.com/windows/uwp/input-and-devices/guidelines-for-visualfeedback).

## <a name="cursors"></a>Cursores


Um conjunto de cursores padrão está disponível para ponteiros de touchpad. São usados para indicar a ação primária de um elemento.

Cada cursor padrão tem uma imagem padrão correspondente associada. O usuário ou um aplicativo substitui a imagem padrão associada a qualquer cursor padrão quando desejado. Aplicativos UWP especificam uma imagem de cursor usando a função [**PointerCursor**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointercursor).

Se você precisa personalizar o cursor do mouse:

-   Sempre use o cursor de seta (![cursor de seta](images/cursor-arrow.png)) para elementos clicáveis. não use o de cursor de mão apontando (![cursor de mão apontando](images/cursor-pointinghand.png)) para links ou outros elementos interativos. No lugar dele, use os efeitos de focalização (descritos anteriormente).
-   Use o cursor de texto (![cursor de texto](images/cursor-text.png)) para texto selecionável.
-   Use o de cursor de movimentação (![cursor de movimentação](images/cursor-move.png)) quando o movimento é a ação principal (como arrastar ou cortar). Não use o cursor de movimentação para elementos em que a ação principal é navegar (como os blocos na tela inicial).
-   Use os cursores de redimensionamento horizontal, vertical e diagonal (![cursor de redimensionamento vertical](images/cursor-vertical.png), ![cursor de redimensionamento horizontal](images/cursor-horizontal.png), ![cursor de redimensionamento diagonal (inferior esquerdo, superior direito)](images/cursor-diagonal2.png), ![cursor de redimensionamento diagonal (superior esquerdo, inferior direito)](images/cursor-diagonal1.png)), quando um objeto for redimensionável.
-   Use os cursores de mão agarrando (![cursor de mão agarrando (aberto)](images/cursor-pan1.png), ![cursor de mão agarrando (fechado)](images/cursor-pan2.png)) durante o movimento panorâmico do conteúdo dentro de uma tela fixa (como um mapa).

## <a name="related-articles"></a>Artigos relacionados

- [Manipular entrada de ponteiro](handle-pointer-input.md)
- [Identificar dispositivos de entrada](identify-input-devices.md)

### <a name="samples"></a>Amostras

- [Amostra de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Amostra de entrada de baixa latência](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Amostra do modo de interação do usuário](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Amostra de visuais de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Exemplos de arquivo morto

- [Entrada: amostra de funcionalidades do dispositivo](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Entrada: amostra de eventos de entrada do usuário XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Exemplo de rolagem, panorâmica e zoom do XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Entrada: gestos e manipulações com o GestureRecognizer](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
