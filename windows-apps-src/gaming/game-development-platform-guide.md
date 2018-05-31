---
author: mtoepke
title: Tecnologias de jogos para aplicativos UWP (Plataforma Universal do Windows)
description: Neste guia, você aprenderá sobre as tecnologias disponíveis para o desenvolvimento de jogos UWP (Plataforma Universal do Windows).
ms.assetid: bc4d4648-0d6e-efbb-7608-80bd09decd6e
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, jogos, tecnologia, directx
ms.localizationpriority: medium
ms.openlocfilehash: 496e0f8386b60247090035d4c4d1f7aa986f8560
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
ms.locfileid: "1690752"
---
# <a name="game-technologies-for-uwp-apps"></a>Tecnologias de jogos para aplicativos UWP



Neste guia, você aprenderá sobre as tecnologias disponíveis para o desenvolvimento de jogos UWP (Plataforma Universal do Windows).

##  <a name="benefits-of-windows-10-for-game-development"></a>Benefícios do Windows 10 para desenvolvimento de jogos


Com a introdução da UWP no Windows 10, os títulos do Windows 10 poderão abranger todas as plataformas da Microsoft. Com a migração gratuita de versões anteriores do Windows, há um número cada vez maior de clientes do Windows 10. A combinação dessas duas coisas significa que seus títulos do Windows 10 serão capazes de chegar a um grande número de clientes por meio da Microsoft Store.

Além disso, o Windows 10 oferece muitos recursos novos que são particularmente benéficos para jogos:

-   Paginação de memória reduzida e tamanho de sistema de memória geral reduzido
-   O gerenciamento de memória de elementos gráficos aprimorado aloca ativamente e protege mais memória para o jogo de primeiro plano

## <a name="uwp-games-with-c-and-directx"></a>Jogos UWP com C++ e DirectX


Os jogos em tempo real que exigem alto desempenho devem usar as APIs do DirectX. O DirectX é uma coleção de APIs nativas para a criação de jogos e aplicativos multimídia que exigem alto desempenho, como jogos 3D.

## <a name="development-environment"></a>Ambiente de desenvolvimento


Para criar jogos para UWP, você precisará configurar o ambiente de desenvolvimento instalando o Visual Studio 2015 ou posterior. O Visual Studio 2015 permite que você crie aplicativos UWP e fornece ferramentas para desenvolvimento de jogos:

-   Ferramentas do Visual Studio para programação de jogos DX - O Visual Studio fornece ferramentas para criar, editar, visualizar e exportar recursos de imagem, modelo e sombreador. Há também ferramentas que você pode usar para converter recursos no momento da compilação e depurar o código gráfico do DirectX. Para obter mais informações, consulte [Usar ferramentas do Visual Studio para programação de jogos](set-up-visual-studio-for-game-development.md).
-   Recursos de diagnóstico de gráficos do Visual Studio – As ferramentas de diagnóstico de elementos gráficos agora estão disponíveis no Windows como um recurso opcional. As ferramentas de diagnóstico permitem que você faça depuração e análise do quadro de elementos gráficos e monitore o uso da GPU em tempo real. Para obter mais informações, consulte [Usar o tempo de execução do DirectX e recursos de diagnóstico de elementos gráficos do Visual Studio](use-the-directx-runtime-and-visual-studio-graphics-diagnostic-features.md).

Para obter mais informações, consulte Preparar a Plataforma Universal do Windows e [programação no DirectX](directx-programming.md).

## <a name="getting-started-with-directx-game-project-templates"></a>Introdução aos modelos de projeto de jogo em DirectX


Depois de configurar o ambiente de desenvolvimento, você pode usar um dos modelos de projeto DirectX relacionados para criar seu jogo DirectX UWP. O Visual Studio 2015 tem três modelos disponíveis para criar novos projetos UWP no DirectX: **Aplicativo DirectX 11 (Universal do Windows)**, **Aplicativo DirectX 12 (Universal do Windows)** e **Aplicativo DirectX 11 e XAML (Universal do Windows)**. Para obter mais informações, consulte [Criar um projeto de jogo em Plataforma Universal do Windows e DirectX usando um modelo](user-interface.md).

## <a name="windows-10-apis"></a>APIs do Windows 10


O Windows 10 oferece um amplo conjunto de APIs que são úteis para o desenvolvimento de jogos. Existem APIs para quase todos os aspectos de jogos, incluindo elementos gráficos 3D e 2D, áudio, entrada, recursos de texto, interface do usuário e rede.

Há muitas APIs relacionadas ao desenvolvimento de jogos, mas nem todos os jogos precisam usar todas as APIs. Por exemplo, alguns jogos só usam elementos gráficos 3D e só usam o Direct3D, alguns jogos podem usar somente os elementos gráficos 2D e usar somente o Direct2D e ainda outros jogos podem fazer uso de ambos. O diagrama a seguir mostra as APIs relacionadas ao desenvolvimento de jogos agrupadas por tipo de funcionalidade.

![tecnologias de plataforma de jogos](images/gameplatformtechnologies.png)

-   Elementos gráficos 3D – o Windows 10 dá suporte a dois conjuntos de APIs de elementos gráficos 3D, Direct3D 11 e [Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/dn899121). As duas APIs oferecem a capacidade de criar elementos gráficos 3D e 2D. Direct3D 11 e Direct3D 12 não são usados juntos, mas ambos podem ser usados com qualquer uma das APIs do grupo de interfaces do usuário e elementos gráficos 2D. Para obter mais informações sobre como usar as APIs de elementos gráficos em seu jogo, consulte [Elementos gráficos 3D básicos para jogos DirectX](an-introduction-to-3d-graphics-with-directx.md).

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Descrição</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">Direct3D 12</td>
    <td align="left"><p>O Direct3D 12 apresenta a próxima versão do Direct3D, a API gráfica 3D na base do DirectX. Essa versão do Direct3D foi projetada para ser mais rápida e mais eficiente do que as versões anteriores do Direct3D. A desvantagem da maior velocidade do Direct3D 12 é que ele está em um nível inferior e exige que você gerencie seus recursos gráficos por conta própria, além de ter uma experiência de programação de elementos gráficos mais abrangente para que se observe a maior velocidade.</p>
    <p><strong>Quando usar</strong></p>
    <p>Use o Direct3D 12 quando você precisa maximizar o desempenho do jogo e seu jogo está vinculado à CPU.</p>
    <p><strong>Para obter mais informações</strong></p>
    <p>Consulte a documentação do <a href="https://msdn.microsoft.com/library/windows/desktop/dn899121">Direct3d 12</a>.</p></td>
    </tr>
    <tr class="even">
    <td align="left">Direct3D 11</td>
    <td align="left"><p>O Direct3D 11 é a versão anterior do Direct3D e permite criar elementos gráficos 3D usando nível superior de abstração de hardware que o D3D 12.</p>
    <p><strong>Quando usar</strong></p>
    <p>Use o Direct3D 11 caso você tenha código do Direct3D 11 existente, seu jogo não esteja vinculado à CPU ou queira aproveitar recursos gerenciados para você.</p>
    <p><strong>Para obter mais informações</strong></p>
    <p>Consulte a documentação do <a href="https://msdn.microsoft.com/library/windows/desktop/ff476080">Direct3D 11</a>.</p></td>
    </tr>
    </tbody>
    </table>

     

-   Elementos gráficos 2D e interface do usuário – APIs referentes a elementos gráficos 2D, como texto e interfaces do usuário. Todos os elementos gráficos 2D e APIs de interface do usuário são opcionais.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Descrição</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">Direct2D</td>
    <td align="left"><p>Direct2D é uma API de elementos gráficos 2D com aceleração de hardware e modo imediato que fornece alto desempenho e renderização de alta qualidade para geometria 2D, bitmaps e texto. A API Direct2D foi compilada no Direct3D e projetada para interoperar bem com GDI, GDI+ e Direct3D.</p>
    <p><strong>Quando usar</strong></p>
    <p>O Direct2D pode ser usado em vez do Direct3D para fornecer elementos gráficos para jogos 2D puros, como um jogo de tabuleiro ou de controle de rolagem ou pode ser usado com o Direct3D para simplificar a criação de elementos gráficos 2D em um jogo 3D, como uma interface do usuário ou uma tela imediata.</p>
    <p><strong>Para obter mais informações</strong></p>
    <p>Consulte a documentação do <a href="https://msdn.microsoft.com/library/windows/desktop/dd370990">Direct2D</a>.</p></td>
    </tr>
    <tr class="even">
    <td align="left">DirectWrite</td>
    <td align="left"><p>O DirectWrite oferece recursos extras para trabalhar com texto e pode ser usado com o Direct3D ou o Direct2D para fornecer saída de texto para interfaces do usuário ou outras áreas onde o texto é obrigatório. DirectWrite dá suporte à medição, ao desenho e ao teste de clique de texto em vários formatos. DirectWrite identifica texto em todos os idiomas compatíveis para aplicativos globais e localizados. O DirectWrite também oferece uma API de renderização de glifos de nível inferior para desenvolvedores que queiram executar seu próprio processamento Unicode em glifo.</p>
    <p><strong>Quando usar</strong></p>
    <p></p>
    <p><strong>Para obter mais informações</strong></p>
    <p>Consulte a documentação do <a href="https://msdn.microsoft.com/library/windows/desktop/dd368038">DirectWrite</a>.</p></td>
    </tr>
    <tr class="odd">
    <td align="left">DirectComposition</td>
    <td align="left"><p>O DirectComposition é um componente do Windows que permite a composição de bitmap de alto desempenho com transformações, efeitos e animações. Os desenvolvedores de aplicativos podem usar a API DirectComposition para criar interfaces do usuário visualmente atraentes que contam com transições animadas avançadas e fluidas de um elemento visual para outro.</p>
    <p><strong>Quando usar</strong></p>
    <p>O DirectComposition foi projetado para simplificar o processo de composição de elementos visuais e criação de transições animadas. Caso seu jogo exija interfaces do usuário complexas, você pode usar DirectComposition para simplificar a criação e o gerenciamento da interface do usuário.</p>
    <p><strong>Para obter mais informações</strong></p>
    <p>Consulte a documentação do <a href="https://msdn.microsoft.com/library/windows/desktop/hh437371">DirectComposition</a>.</p></td>
    </tr>
    </tbody>
    </table>

     

-   Áudio – APIs referentes à reprodução de áudio e à aplicação de efeitos de áudio. Para saber como usar as APIs de áudio em seu jogo, veja [Áudio para jogos](working-with-audio-in-your-directx-game.md).

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Descrição</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">XAudio2</td>
    <td align="left"><p>XAudio2 é uma API de áudio de nível inferior que oferece uma base para processamento e mixagem de sinais. O XAudio foi projetado para responder muito bem a mecanismos de áudio de jogos ao mesmo tempo em que mantém a capacidade de criar efeitos de áudio personalizados e cadeias complexas de efeitos e filtros de áudio.</p>
    <p><strong>Quando usar</strong></p>
    <p>Use o XAudio2 quando seu jogo precisar reproduzir sons com sobrecarga e atraso mínimos.</p>
    <p><strong>Para obter mais informações</strong></p>
    <p>Consulte a documentação do <a href="https://msdn.microsoft.com/library/windows/desktop/hh405049">XAudio2</a>.</p></td>
    </tr>
    <tr class="even">
    <td align="left">Media Foundation</td>
    <td align="left"><p>O Microsoft Media Foundation foi projetado para a reprodução de arquivos e fluxos de mídia de áudio e de vídeo, mas também pode ser usado em jogos, quando uma funcionalidade de nível superior à do XAudio2 for necessária, e certa sobrecarga adicional for aceitável.</p>
    <p><strong>Quando usar</strong></p>
    <p>O Media Foundation é especialmente útil para cenas cinematográficas ou componentes não interativos do jogo. O Media Foundation também é útil para decodificar arquivos de áudio para reprodução usando-se XAudio2.</p>
    <p><strong>Para obter mais informações</strong></p>
    <p>Consulte a visão geral do <a href="https://msdn.microsoft.com/library/windows/desktop/ms694197">Microsoft Media Foundation</a>.</p></td>
    </tr>
    </tbody>
    </table>

     

-   Entrada – APIs referentes à entrada do teclado, mouse, gamepad e outras origens de entrada do usuário.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Descrição</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">XInput</td>
    <td align="left"><p>A API XInput Game Controller permite que aplicativos recebam a entrada de controladores de jogo.</p>
    <p><strong>Quando usar</strong></p>
    <p>Caso seu jogo precise dar suporte à entrada de gampad e você tenha um código XInput existente, você pode continuar usando o XInput. O XInput foi substituído por Windows.Gaming.Input para UWP e, caso esteja escrevendo um novo código de entrada, você deve usar Windows.Gaming.Input em vez de XInput.</p>
    <p><strong>Para obter mais informações</strong></p>
    <p>Consulte a documentação do <a href="https://msdn.microsoft.com/library/windows/desktop/hh405053">XInput</a>.</p></td>
    </tr>
    <tr class="even">
    <td align="left">Windows.Gaming.Input</td>
    <td align="left"><p>A API Windows.Gaming.Input substitui o XInput e oferece a mesma funcionalidade com as seguintes vantagens em relação ao Xinput:</p>
    <ul>
    <li>Menos uso de recursos</li>
    <li>Menos latência da chamada à API para recuperar a entrada</li>
    <li>A capacidade de funcionar com mais de 4 gamepads por vez</li>
    <li>A capacidade de acessar mais recursos do gamepad Xbox One, como motores de vibração de gatilho</li>
    <li>A capacidade para ser notificado quando controladores são conectados/desconectados por meio de eventos, em vez de sondagem</li>
    <li>A capacidade de atribuir uma entrada a um usuário específico (Windows.System.User)</li>
    </ul>
    <p><strong>Quando usar</strong></p>
    <p>Se seu jogo precisar dar suporte à entrada de gamepad e não estiver usando um código XInput existente ou se precisar de um dos benefícios listados acima, você deve usar Windows.Gaming.Input.</p>
    <p><strong>Para obter mais informações</strong></p>
    <p>Veja a documentação do <a href="https://msdn.microsoft.com/library/windows/apps/dn707817">Windows.Gaming.Input</a>.</p></td>
    </tr>
    <tr class="odd">
    <td align="left">Windows.UI.Core.CoreWindow</td>
    <td align="left"><p>A classe Windows.UI.Core.CoreWindow fornece eventos para acompanhar pressionamentos de ponteiro e movimento, além de eventos de pressionamento e liberação de teclas.</p>
    <p><strong>Quando usar</strong></p>
    <p>Use eventos Windows.UI.Core.CoreWindows quando você precisar acompanhar pressionamentos de mouse ou tecla no jogo.</p>
    <p><strong>Para obter mais informações</strong></p>
    <p>Veja <a href="https://docs.microsoft.com/windows/uwp/gaming/tutorial--adding-move-look-controls-to-your-directx-game">Controles move-look para jogos</a> para saber mais sobre como usar o mouse ou o teclado em seu jogo.</p></td>
    </tr>
    </tbody>
    </table>

     

-   Matemática – APIs referentes à simplificação das operações matemáticas mais usadas.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Descrição</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">DirectXMath</td>
    <td align="left"><p>A API DirectXMath oferece tipos e funções C++ amigáveis a SIMD para operações de álgebra linear comum e de matemática de elementos gráficos comuns a jogos.</p>
    <p><strong>Quando usar</strong></p>
    <p>O uso de DirectXMath é opcional e simplifica operações matemáticas comuns.</p>
    <p><strong>Para obter mais informações</strong></p>
    <p>Consulte a documentação <a href="https://msdn.microsoft.com/library/windows/desktop/hh437833">DirectXMath</a>.</p></td>
    </tr>
    </tbody>
    </table>

     

-   Rede - APIs referentes a comunicação com outros computadores e dispositivos pela Internet ou redes privadas.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Descrição</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">Windows.Networking.Sockets</td>
    <td align="left"><p>O namespace Windows.Networking.Sockets oferece soquetes TCP e UDP que permitem comunicação de rede confiável ou não confiável.</p>
    <p><strong>Quando usar</strong></p>
    <p>Use Windows.Networking.Sockets caso seu jogo precise se comunicar com outros computadores ou dispositivos via rede.</p>
    <p><strong>Para obter mais informações</strong></p>
    <p>Consulte <a href="https://docs.microsoft.com/windows/uwp/gaming/work-with-networking-in-your-directx-game">Trabalhar com rede em seu jogo</a>.</p></td>
    </tr>
    <tr class="even">
    <td align="left">Windows.Web.HTTP</td>
    <td align="left"><p>O namespace Windows.Web.HTTP oferece conexão confiável para servidores HTTP que pode ser usada para acessar um site.</p>
    <p><strong>Quando usar</strong></p>
    <p>Use Windows.Web.HTTP quando seu jogo precisar acessar um site para recuperar ou armazenar informações.</p>
    <p><strong>Para obter mais informações</strong></p>
    <p>Consulte <a href="https://docs.microsoft.com/windows/uwp/gaming/work-with-networking-in-your-directx-game">Trabalhar com rede em seu jogo</a>.</p></td>
    </tr>
    </tbody>
    </table>

     

-   Utilitários de suporte - Bibliotecas que se baseiam nas APIs do Windows 10.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">Biblioteca</th>
    <th align="left">Descrição</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">Kit de ferramentas do DirectX</td>
    <td align="left"><p>O Kit de ferramentas do DirectX (DirectXTK) é uma coleção de classes auxiliares para escrever código DirectX 11.x em C++.</p>
    <p><strong>Quando usar</strong></p>
    <p>Use o Kit de ferramentas do DirectX caso você seja um desenvolvedor C++ à procura de um substituto moderno pelo código de utilitário D3DX herdado ou você seja um desenvolvedor XNA Game Studio fazendo a transição para C++ nativo.</p>
    <p><strong>Para obter mais informações</strong></p>
    <p>Consulte a página de projeto do Kit de ferramentas do DirectX, <a href="https://github.com/Microsoft/DirectXTK">https://github.com/Microsoft/DirectXTK</a>.</p></td>
    </tr>
    <tr class="even">
    <td align="left">Win2D</td>
    <td align="left"><p>Win2D é uma API de Windows Runtime fácil de usar para renderizar elementos gráficos 2D de modo imediato.</p>
    <p><strong>Quando usar</strong></p>
    <p>Use Win2D caso você seja um desenvolvedor C++ e queira um wrapper WinRT mais fácil de usar para Direct2D e DirectWrite ou seja um desenvolvedor C# que queira usar Direct2D e DirectWrite.</p>
    <p><strong>Para obter mais informações</strong></p>
    <p>Consulte a página de projeto do Win2D, <a href="https://github.com/Microsoft/Win2D">https://github.com/Microsoft/Win2D</a>.</p></td>
    </tr>
    </tbody>
    </table>

     

## <a name="xbox-live-services"></a>Serviços Xbox Live

O [Programa de Criadores do Xbox Live](https://developer.microsoft.com/games/xbox/xboxlive/creator) permite que qualquer desenvolvedor integre o Xbox Live ao seu jogo UWP e publique no Xbox One e no Windows 10. Integre as experiências sociais do Xbox Live, como entrada, presença, placares de líderes e muito mais ao seu título, com tempo de desenvolvimento mínimo. Os recursos sociais do Xbox Live foram criados para aumentar seu público de forma orgânica, distribuindo reconhecimento para os mais de 55 milhões de jogadores ativos.

Se você deseja ter acesso a mais recursos do Xbox Live, marketing dedicado e suporte para desenvolvimento e a chance de ganhar destaque na loja principal do Xbox One, inscreva-se no programa [ID@Xbox](http://www.xbox.com/developers/id). Para ver quais recursos estão disponíveis para o Programa de Criadores do Xbox Live e para o programa ID@Xbox, consulte a [Tabela de recursos](../xbox-live/developer-program-overview.md#feature-table).

Para saber mais, acesse [Adicionando o Xbox Live ao seu jogo](e2e.md#adding-xbox-live-to-your-game).

##  <a name="alternatives-to-writing-games-with-directx-and-uwp"></a>Alternativas para escrever jogos com o DirectX e a UWP


### <a name="uwp-games-without-directx"></a>Jogos UWP sem o DirectX

Jogos mais simples com requisitos de desempenho mínimo, como jogos de cartas ou jogos de tabuleiro, podem ser gravados sem DirectX e não necessariamente precisam ser gravados em C++. Esses tipos de jogos podem usar qualquer linguagem suportada pela UWP como C#, Visual Basic, C++ e HTML/JavaScript. Se seu jogo não precisar ter desempenho ou elementos gráficos intensivos, confira a [Amostra de jogo por toque em JavaScript e HTML5](http://code.msdn.microsoft.com/windowsapps/JavaScript-and-HTML5-touch-d96f6031) como um exemplo.

### <a name="game-engines"></a>Mecanismos de jogo

Como uma alternativa à criação do próprio mecanismo de jogo usando as APIs de desenvolvimento de jogos do Windows, muitos mecanismos de jogo de alta qualidade compilados com base nas APIs de desenvolvimento de jogos do Windows estão à disposição para desenvolver jogos com base em plataformas do Windows. Ao levar em consideração um mecanismo ou uma biblioteca de jogos, você tem várias opções:

-   Mecanismo de jogo completo – Um mecanismo de jogo completo encapsula grande parte ou todas as APIs do Windows 10 que você usaria ao escrever um mecanismo de jogo do zero, como elementos gráficos, áudio, entrada e rede. Os mecanismos de jogo completo também podem oferecer uma funcionalidade lógica como inteligência artificial e localização de caminhos.
-   Mecanismo de elementos gráficos – Os mecanismos de gráficos encapsulam as APIs de elementos gráficos do Windows 10, gerenciam recursos de elementos gráficos e dão suporte a uma grande variedade de formatos de modelo e mundo.
-   Mecanismo de áudio – Os mecanismos de áudio encapsulam as APIs de áudio do Windows 10, gerenciam recursos de áudio e oferecem processamento e efeitos de áudio avançados.
-   Mecanismo de rede – Os mecanismos de rede encapsulam as APIs de Rede do Windows 10 para adicionar suporte a vários jogos ponto a ponto ou baseado em servidor ao jogo e podem incluir uma funcionalidade de rede avançada para dar suporte a um grande número de jogadores.
-   Inteligência artificial e mecanismo para encontrar caminhos – Os mecanismos de inteligência artificial e para encontrar caminhos oferecem uma estrutura para controlar o comportamento de agentes no jogo.
-   Mecanismos de finalidade especial – Existem vários mecanismos adicionais para identificar praticamente todas as tarefas relacionadas ao desenvolvimento de jogos como, por exemplo, criar sistemas de inventário e árvores de caixa de diálogo.

## <a name="submitting-a-game-to-the-store"></a>Enviar um jogo para a Loja


Depois que você estiver pronto para publicar seu jogo, será necessário criar uma conta de desenvolvedor e enviar seu jogo para a Microsoft Store.

Para obter informações sobre como enviar seu jogo para a Microsoft Store, veja [Enviando e publicando seu jogo](e2e.md#submitting-and-publishing-your-game).

 

 




