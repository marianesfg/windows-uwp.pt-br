---
author: mtoepke
title: Desenvolvendo o Marble Maze, um jogo da UWP em C++ e DirectX
description: "Esta seção da documentação descreve como usar o DirectX e o Visual C++ para criar um jogo 3D UWP (Plataforma Universal do Windows)."
ms.assetid: 43f1977a-7e1d-614c-696e-7669dd8a9cc7
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: d660e05ab43f1c45f21b028a78c6cfa3e0897164

---

# Desenvolvendo o Marble Maze, um jogo da UWP em C++ e DirectX


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Esta seção da documentação descreve como usar o DirectX e o Visual C++ para criar um jogo 3D UWP (Plataforma Universal do Windows). Esta documentação mostra como criar um jogo 3D denominado Marble Maze que adota novos fatores forma, como tablets, e também funciona em desktops e notebooks tradicionais.

> **Observação**   Para baixar o código-fonte do Marble Maze, consulte [Amostra do jogo DirectX Marble Maze](http://go.microsoft.com/fwlink/?LinkId=624011).

 

> **Importante**  O Marble Maze ilustra padrões de design que consideramos ser as práticas recomendadas para a criação de jogos UWP. Você pode adaptar muitos dos detalhes de implementação às próprias práticas e aos requisitos específicos do jogo que você está desenvolvendo. Sinta-se à vontade para usar técnicas ou bibliotecas diferentes se estas forem mais adequadas para as suas necessidades. (No entanto, sempre garanta que seu código é aprovado no Kit de Certificação de Aplicativos Windows.) Quando consideramos que uma implementação do Marble Maze é essencial para o desenvolvimento de jogos com êxito, enfatizamos isso nesta documentação.

 

## Apresentando o Marble Maze


Escolhemos o Marble Maze porque ele é relativamente básico, mas ainda demonstra a amplitude de recursos que estão disponíveis na maioria dos jogos. Ele mostra como usar elementos gráficos, manipulação de entrada e áudio. Ele também demonstra a mecânica do jogo, como regras e metas.

O Marble Maze se parece com o jogo de labirinto de mesa que é normalmente construído a partir de uma caixa que contém buracos e uma bolinha de vidro ou aço. O objetivo do Marble Maze é o mesmo da versão de mesa: inclinar o labirinto para direcionar a bolinha do início ao fim do labirinto no espaço de tempo mais curto possível, sem deixar que ela caia em nenhum dos buracos. O Marble Maze adiciona o conceito de pontos de controle. Se a bolinha cai em um buraco, o jogo é reiniciado no último ponto de controle pelo qual a bolinha passou.

O Marble Maze oferece várias maneiras de um usuário interagir com o tabuleiro do jogo. Se você tiver um dispositivo habilitado para toque ou acelerômetro, poderá usá-lo para mover o tabuleiro do jogo. Você também pode usar um mouse ou controlador do Xbox 360 para controlar o jogo.

![captura de tela do jogo marble maze.](images/marblemaze.png)

## Pré-requisitos


-   Windows 10
-   Microsoft Visual Studio 2015
-   Conhecimento em programação C++
-   Familiaridade com DirectX e terminologia do DirectX
-   Conhecimentos básicos de COM

## Quem deve ler isto?


Se você está interessado em criar jogos 3D ou outros aplicativos do Windows 10 que usam elementos gráficos intensivamente, este documento é ideal para você. Esperamos que você use os princípios e as práticas descritos nesta documentação para criar seu próprio jogo UWP. Ter conhecimento ou interesse em programação C++ e DirectX irá ajudar você a tirar o máximo proveito desta documentação. Se você não tem experiência com DirectX, ainda poderá se beneficiar se tiver experiência com ambientes semelhantes de programação de elementos gráficos 3D.

O documento [Passo a passo: criar um jogo simples da UWP com DirectX](tutorial--create-your-first-metro-style-directx-game.md) descreve outra amostra que implementa um jogo básico de tiro 3D usando DirectX e C++.

## Conteúdo discutido nesta documentação


Esta documentação ensina a:

-   Usar a API do Windows Runtime e o DirectX para criar um jogo UWP.
-   Usar o [Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476080) e o [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990) para trabalhar com conteúdo visual, como modelos, texturas, sombreadores de vértice e pixel e sobreposições 2D.
-   Integrar mecanismos de entrada, como toque, acelerômetro e o controlador do Xbox 360.
-   Usar o [XAudio2](https://msdn.microsoft.com/library/windows/desktop/hh405049) para incorporar música e efeitos sonoros.

## Conteúdo não discutido nesta documentação


Esta documentação não discute os seguintes aspectos do desenvolvimento de jogos. Esses aspectos são seguidos por recursos adicionais que os discutem.

-   Princípios de design de jogos 3D.
-   Noções básicas sobre programação C++ ou DirectX.
-   Como projetar recursos, como texturas, modelos ou áudio.
-   Como solucionar problemas de comportamento ou desempenho no seu jogo.
-   Como preparar seu jogo para uso em outras partes do mundo.
-   Como certificar e publicar seu jogo na Windows Store.

O Marble Maze também usa a biblioteca [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833) para trabalhar com geometria 3D e realizar cálculos de física, como colisões. O DirectXMath não é discutido com detalhes nesta seção. Para saber mais sobre o DirectXMath, consulte o [Guia de Programação para DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833). Para saber mais detalhes sobre como o Marble Maze usa o DirectXMath, consulte o código-fonte.

Embora o Marble Maze forneça muitos componentes reutilizáveis, ele não é uma estrutura completa para o desenvolvimento de jogos. Quando consideramos que um componente do Marble Maze é reutilizável no seu jogo, enfatizamos isso na documentação.

## Próximas etapas


Recomendamos que você comece com os princípios básicos da amostra do Marble Maze para saber mais sobre a estrutura do Marble Maze e algumas das diretrizes de codificação e estilo seguidas pelo seu código-fonte. A tabela a seguir apresenta os documentos nesta seção para que você possa consultá-los com mais facilidade.

## Tópicos relacionados


| Título                                                                                                                    | Descrição                                                                                                                                                                                                                                        |
|--------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Princípios básicos da amostra do Marble Maze](marble-maze-sample-fundamentals.md)                                                   | Fornece uma visão geral da estrutura do jogo e algumas das orientações de código e estilo seguidas pelo código-fonte.                                                                                                                                 |
| [Estrutura do aplicativo Marble Maze](marble-maze-application-structure.md)                                               | Descreve como o código do aplicativo Marble Maze é estruturado e como a estrutura de um aplicativo UWP DirectX é diferente daquela de um aplicativo de área de trabalho tradicional.                                                                                    |
| [Adicionando conteúdo visual ao exemplo do Marble Maze](adding-visual-content-to-the-marble-maze-sample.md)                   | Descreve algumas das práticas fundamentais que você deve ter em mente ao trabalhar com o Direct3D e o Direct2D. Também descreve como o Marble Maze aplica essas práticas para conteúdo visual.                                                                           |
| [Adicionando entrada e interatividade ao exemplo do Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md) | Descreve como o Marble Maze trabalha com dispositivos de acelerômetro, toque e controlador do Xbox 360 para permitir que os usuários naveguem pelos menus e interajam com o tabuleiro do jogo. Também descreve algumas das práticas recomendadas que você deve ter em mente ao trabalhar com entrada. |
| [Adicionando áudio ao exemplo do Marble Maze](adding-audio-to-the-marble-maze-sample.md)                                     | Descreve como o Marble Maze trabalha com áudio para adicionar música e efeitos sonoros à experiência do jogo.                                                                                                                                                  |

 

 

 







<!--HONumber=Aug16_HO3-->


