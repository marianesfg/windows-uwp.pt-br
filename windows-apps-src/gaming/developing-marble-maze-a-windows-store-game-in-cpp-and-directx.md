---
title: Desenvolvendo *Marble Maze* &mdash; um jogo de plataforma universal do Windows de mármore (UWP) criado com C++ para DirectX
description: Esta seção da documentação descreve como usar o DirectX e C++ para criar um jogo de Plataforma Universal do Windows 3D (UWP).
ms.assetid: 43f1977a-7e1d-614c-696e-7669dd8a9cc7
ms.date: 08/10/2017
ms.topic: article
keywords: windows 10, uwp, jogos, amostra, directx, 3d
ms.localizationpriority: medium
ms.openlocfilehash: d2c0c630090c178a54a0452ab3cc430ffee4a176
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409495"
---
# <a name="developing-marble-mazemdasha-universal-windows-platform-uwp-game-built-with-c-for-directx"></a>Desenvolvendo *Marble Maze* &mdash; um jogo de plataforma universal do Windows de mármore (UWP) criado com C++ para DirectX

Este tópico descreve como usar o DirectX e C++ para criar um jogo de Plataforma Universal do Windows 3D (UWP). O jogo, chamado de *labirinto de mármore*, adota vários fatores forma, como tablets, PCs desktop tradicionais e laptops.

> [!NOTE]
> Para baixar o código-fonte de *labirinto de mármore* , consulte o [exemplo no GitHub](https://github.com/microsoft/Windows-appsample-marble-maze).

> [!IMPORTANT]
> O *labirinto de mármore* ilustra os padrões de design que consideramos como práticas recomendadas para a criação de jogos UWP. Você pode adaptar muitos dos detalhes de implementação às próprias práticas e aos requisitos específicos do jogo que você está desenvolvendo. Sinta-se à vontade para usar técnicas ou bibliotecas diferentes se estas forem mais adequadas para as suas necessidades. (No entanto, sempre garanta que seu código seja aprovado no [Kit de Certificação de Aplicativos Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit).) Quando consideramos que uma implementação usada aqui é essencial para o êxito no desenvolvimento de jogos, enfatizamos isso nesta documentação.

## <a name="introducing-marble-maze"></a>Apresentando *labirinto de mármore*

Escolhemos o *labirinto de mármore* porque ele é relativamente básico, mas ainda demonstra a amplitude dos recursos encontrados na maioria dos jogos. Ele mostra como usar elementos gráficos, manipulação de entrada e áudio. Ele também demonstra a mecânica do jogo, como regras e metas.

O *labirinto de mármore* é semelhante ao jogo labirinto de tabela superior que normalmente é construído a partir de uma caixa que contém buracos e uma mármore de aço ou vidro. O objetivo do *labirinto de mármore* é o mesmo que a versão de tabela superior: Incline o labirinto para orientar a mármore desde o início até o fim do labirinto em menos tempo possível, sem deixar que a mármore se enquadrasse em qualquer um dos orifícios. *Labirinto de mármore* adiciona o conceito de pontos de verificação. Se a bolinha cai em um buraco, o jogo é reiniciado no último ponto de controle pelo qual a bolinha passou.

O *labirinto de mármore* oferece várias maneiras para um usuário interagir com o tabuleiro do jogo. Se você tiver um dispositivo habilitado para toque ou acelerômetro, poderá usá-lo para mover o tabuleiro do jogo. Você também pode usar um mouse ou controle Xbox One para controlar o jogo.

![captura de tela do jogo marble maze.](images/marblemaze-2.png)

## <a name="prerequisites"></a>Pré-requisitos

-   Atualização do Windows 10 para Criadores
-   [Microsoft Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
-   Conhecimento em programação C++
-   Familiaridade com DirectX e terminologia do DirectX
-   Conhecimentos básicos de COM

## <a name="who-should-read-this"></a>Quem deve ler isso?

Se você estiver interessado em criar jogos 3D ou outros aplicativos com uso intensivo de gráficos para o Windows 10, isso é para você. Esperamos que você use os princípios e as práticas descritos nesta documentação para criar seu próprio jogo UWP. Ter conhecimento ou interesse em programação C++ e DirectX irá ajudar você a tirar o máximo proveito desta documentação. Se você não tem experiência com DirectX, ainda poderá se beneficiar se tiver experiência com ambientes semelhantes de programação de elementos gráficos 3D.

O documento [explicativo: criar um jogo UWP simples com o DirectX](tutorial--create-your-first-uwp-directx-game.md) descreve outro exemplo que implementa um jogo básico de captura de 3D usando DirectX e C++.

## <a name="what-this-documentation-covers"></a>Conteúdo discutido nesta documentação

Esta documentação ensina a:

-   Usar a API do Windows Runtime e o DirectX para criar um jogo UWP.
-   Use o [Direct3D](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11) e o [Direct2D](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-portal) para trabalhar com conteúdo Visual como modelos, texturas, sombreadores de vértices e de pixel e sobreposições de 2D.
-   Integrar mecanismos de entrada, como toque, acelerômetro e o controle Xbox One.
-   Usar o [XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-apis-portal) para incorporar música e efeitos sonoros.

## <a name="what-this-documentation-does-not-cover"></a>Conteúdo não discutido nesta documentação

Esta documentação não discute os seguintes aspectos do desenvolvimento de jogos. Esses aspectos são seguidos por recursos adicionais que os discutem.

-   Princípios de design de jogos 3D.
-   Noções básicas sobre programação C++ ou DirectX.
-   Como projetar recursos, como texturas, modelos ou áudio.
-   Como solucionar problemas de comportamento ou desempenho no seu jogo.
-   Como preparar seu jogo para uso em outras partes do mundo.
-   Como certificar e publicar seu jogo na Microsoft Store.

O *labirinto de mármore* também usa a biblioteca [DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/directxmath-portal) para trabalhar com a geometria 3D e executar cálculos de física, como colisões. O DirectXMath não é discutido com detalhes nesta seção. Para obter detalhes sobre como o *labirinto de mármore* usa DirectXMath, consulte o código-fonte.

Embora o *labirinto de mármore* forneça muitos componentes reutilizáveis, não é uma estrutura de desenvolvimento de jogos completa. Quando consideramos um componente de *labirinto de mármore* para ser reutilizável em seu jogo, enfatizamos isso na documentação.

## <a name="next-steps"></a>Próximas etapas

Recomendamos que você comece com os [conceitos básicos de exemplo de labirinto de mármore](marble-maze-sample-fundamentals.md) para saber mais sobre a estrutura de labirinto de *mármore* e algumas das diretrizes de codificação e estilo que o código-fonte de *labirinto de mármore* segue. A tabela a seguir apresenta os documentos nesta seção para que você possa consultá-los com mais facilidade.

## <a name="in-this-section"></a>Nesta seção

| Title                                                                                                                    | Descrição                                                                                                                                                                                                                                        |
|--------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Princípios básicos da amostra do Marble Maze](marble-maze-sample-fundamentals.md)                                                   | Fornece uma visão geral da estrutura do jogo e algumas das orientações de código e estilo seguidas pelo código-fonte.                                                                                                                                 |
| [Estrutura do app Marble Maze](marble-maze-application-structure.md)                                               | Descreve como o código do aplicativo de *labirinto de mármore* é estruturado e como a estrutura de um aplicativo UWP do DirectX é diferente da de um aplicativo de área de trabalho tradicional.                                                                                    |
| [Adicionando conteúdo visual ao exemplo do Marble Maze](adding-visual-content-to-the-marble-maze-sample.md)                   | Descreve algumas das práticas fundamentais que você deve ter em mente ao trabalhar com o Direct3D e o Direct2D. Também descreve como o *labirinto de mármore* aplica essas práticas para conteúdo visual.                                                                           |
| [Adicionando entrada e interatividade ao exemplo do Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md) | Descreve como o *labirinto de mármore* funciona com as entradas de controlador do acelerômetro, do touch e do Xbox One para permitir que os usuários naveguem pelos menus e interajam com o tabuleiro do jogo. Também descreve algumas das práticas recomendadas que você deve ter em mente ao trabalhar com entrada. |
| [Adicionando áudio ao exemplo do Marble Maze](adding-audio-to-the-marble-maze-sample.md)                                     | Descreve como o *labirinto de mármore* funciona com áudio para adicionar efeitos musicais e sonoros à experiência do jogo.                                                                                                                                                  |
