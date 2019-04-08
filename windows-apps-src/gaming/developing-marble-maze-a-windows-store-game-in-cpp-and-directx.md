---
title: Desenvolvendo o Marble Maze, um jogo da UWP em C++ e DirectX
description: Esta seção da documentação descreve como usar o DirectX e o Visual C++ para criar um jogo da UWP (Plataforma Universal do Windows) em 3D.
ms.assetid: 43f1977a-7e1d-614c-696e-7669dd8a9cc7
ms.date: 08/10/2017
ms.topic: article
keywords: windows 10, uwp, jogos, amostra, directx, 3d
ms.localizationpriority: medium
ms.openlocfilehash: 39f915ad9cf200a5c2c762976ab3c39c2ef85410
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662461"
---
# <a name="developing-marble-maze-a-uwp-game-in-c-and-directx"></a>Desenvolvendo o Marble Maze, um jogo da UWP em C++ e DirectX




Este tópico descreve como usar o DirectX e o Visual C++ para criar um jogo da UWP (Plataforma Universal do Windows) em 3D. O jogo, chamado Marble Maze, abrange vários fatores forma, como tablets, além de computadores desktop e notebooks tradicionais.

> [!NOTE]
> Para baixar o código-fonte do Marble Maze, consulte o [exemplo no GitHub](https://go.microsoft.com/fwlink/?LinkId=624011).

> [!IMPORTANT]
> O Marble Maze ilustra padrões de design que consideramos ser as práticas recomendadas para a criação de jogos UWP. Você pode adaptar muitos dos detalhes de implementação às próprias práticas e aos requisitos específicos do jogo que você está desenvolvendo. Sinta-se à vontade para usar técnicas ou bibliotecas diferentes se estas forem mais adequadas para as suas necessidades. (No entanto, sempre Certifique-se de que seu código passa o [Kit de certificação de aplicativos do Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit).) Quando consideramos uma implementação usada aqui para ser essencial para o desenvolvimento de jogos com êxito, enfatizamos isso nesta documentação.

 

## <a name="introducing-marble-maze"></a>Apresentando o Marble Maze


Escolhemos o Marble Maze porque ele é relativamente básico, mas ainda demonstra a amplitude de recursos que estão disponíveis na maioria dos jogos. Ele mostra como usar elementos gráficos, manipulação de entrada e áudio. Ele também demonstra a mecânica do jogo, como regras e metas.

O Marble Maze se parece com o jogo de labirinto de mesa que é normalmente construído a partir de uma caixa que contém buracos e uma bolinha de vidro ou aço. O objetivo do Marble Maze é o mesmo da versão de mesa: inclinar o labirinto para direcionar a bolinha do início ao fim do labirinto no espaço de tempo mais curto possível, sem deixar que ela caia em nenhum dos buracos. O Marble Maze adiciona o conceito de pontos de controle. Se a bolinha cai em um buraco, o jogo é reiniciado no último ponto de controle pelo qual a bolinha passou.

O Marble Maze oferece várias maneiras de um usuário interagir com o tabuleiro do jogo. Se você tiver um dispositivo habilitado para toque ou acelerômetro, poderá usá-lo para mover o tabuleiro do jogo. Você também pode usar um mouse ou controle Xbox One para controlar o jogo.

![captura de tela do jogo marble maze.](images/marblemaze-2.png)

## <a name="prerequisites"></a>Pré-requisitos


-   Windows 10 Creators Update
-   [Microsoft Visual Studio 2017](https://www.visualstudio.com/downloads/)
-   Conhecimento em programação C++
-   Familiaridade com DirectX e terminologia do DirectX
-   Conhecimentos básicos de COM

## <a name="who-should-read-this"></a>Quem deve ler isto?


Se você estiver interessado em criar jogos 3D ou outros aplicativos intensivos de elementos gráficos para Windows 10, isso é para você. Esperamos que você use os princípios e as práticas descritos nesta documentação para criar seu próprio jogo UWP. Ter conhecimento ou interesse em programação C++ e DirectX irá ajudar você a tirar o máximo proveito desta documentação. Se você não tem experiência com DirectX, ainda poderá se beneficiar se tiver experiência com ambientes semelhantes de programação de elementos gráficos 3D.

O documento [Passo a passo: criar um jogo UWP simples com DirectX](tutorial--create-your-first-uwp-directx-game.md) descreve outra amostra que implementa um jogo básico de tiro em 3D usando DirectX e C++.

## <a name="what-this-documentation-covers"></a>Conteúdo discutido nesta documentação


Esta documentação ensina a:

-   Usar a API do Windows Runtime e o DirectX para criar um jogo UWP.
-   Usar o [Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476080) e o [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990) para trabalhar com conteúdo visual, como modelos, texturas, sombreadores de vértice e pixel e sobreposições 2D.
-   Integrar mecanismos de entrada, como toque, acelerômetro e o controle Xbox One.
-   Usar o [XAudio2](https://msdn.microsoft.com/library/windows/desktop/hh405049) para incorporar música e efeitos sonoros.

## <a name="what-this-documentation-does-not-cover"></a>Conteúdo não discutido nesta documentação


Esta documentação não discute os seguintes aspectos do desenvolvimento de jogos. Esses aspectos são seguidos por recursos adicionais que os discutem.

-   Princípios de design de jogos 3D.
-   Noções básicas sobre programação C++ ou DirectX.
-   Como projetar recursos, como texturas, modelos ou áudio.
-   Como solucionar problemas de comportamento ou desempenho no seu jogo.
-   Como preparar seu jogo para uso em outras partes do mundo.
-   Como certificar e publicar seu jogo na Microsoft Store.

O Marble Maze também usa a biblioteca [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833) para trabalhar com geometria 3D e realizar cálculos de física, como colisões. O DirectXMath não é discutido com detalhes nesta seção. Para saber mais detalhes sobre como o Marble Maze usa o DirectXMath, consulte o código-fonte.

Embora o Marble Maze forneça muitos componentes reutilizáveis, ele não é uma estrutura completa para o desenvolvimento de jogos. Quando consideramos que um componente do Marble Maze é reutilizável no seu jogo, enfatizamos isso na documentação.

## <a name="next-steps"></a>Próximas etapas


Recomendamos que você comece com os [princípios básicos da amostra do Marble Maze](marble-maze-sample-fundamentals.md) para saber mais sobre a estrutura do Marble Maze e algumas das diretrizes de codificação e estilo seguidas pelo seu código-fonte. A tabela a seguir apresenta os documentos nesta seção para que você possa consultá-los com mais facilidade.

## <a name="in-this-section"></a>Nesta seção


| Título                                                                                                                    | Descrição                                                                                                                                                                                                                                        |
|--------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Conceitos básicos de exemplo Marble Maze](marble-maze-sample-fundamentals.md)                                                   | Fornece uma visão geral da estrutura do jogo e algumas das orientações de código e estilo seguidas pelo código-fonte.                                                                                                                                 |
| [Estrutura de aplicativo do Marble Maze](marble-maze-application-structure.md)                                               | Descreve como o código do aplicativo Marble Maze é estruturado e como a estrutura de um aplicativo UWP DirectX é diferente daquela de um aplicativo de área de trabalho tradicional.                                                                                    |
| [Adicionando conteúdo visual ao exemplo Marble Maze](adding-visual-content-to-the-marble-maze-sample.md)                   | Descreve algumas das práticas fundamentais que você deve ter em mente ao trabalhar com o Direct3D e o Direct2D. Também descreve como o Marble Maze aplica essas práticas para conteúdo visual.                                                                           |
| [Adicionando entrada e interatividade ao exemplo Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md) | Descreve como o Marble Maze trabalha com entradas de acelerômetro, toque e controle Xbox One para permitir que os usuários naveguem pelos menus e interajam com o tabuleiro do jogo. Também descreve algumas das práticas recomendadas que você deve ter em mente ao trabalhar com entrada. |
| [Adicionando áudio ao exemplo Marble Maze](adding-audio-to-the-marble-maze-sample.md)                                     | Descreve como o Marble Maze trabalha com áudio para adicionar música e efeitos sonoros à experiência do jogo.                                                                                                                                                  |

 

 

 




