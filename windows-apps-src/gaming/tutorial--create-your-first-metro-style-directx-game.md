---
author: mtoepke
title: Criar um jogo simples UWP (Plataforma Universal do Windows) com DirectX
description: "Este conjunto de tutoriais mostra como criar um jogo básico UWP (Plataforma Universal do Windows) em DirectX e C++."
ms.assetid: 9edc5868-38cf-58cc-1fb3-8fb85a7ab2c9
keywords:
- Jogo de exemplo DirectX
- exemplo de jogo, Plataforma Universal do Windows (UWP)
- Jogo Direct3D 11
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 3ffce29c3ad7088dd24b848cb159b85a4db158e3
ms.lasthandoff: 02/07/2017

---

# <a name="create-a-simple-universal-windows-platform-uwp-game-with-directx"></a>Criar um jogo simples UWP (Plataforma Universal do Windows) com DirectX


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este conjunto de tutoriais mostra como criar um jogo básico UWP (Plataforma Universal do Windows) em DirectX e C++. Abordamos todas as partes principais de um jogo, inclusive os processos de carregamento de ativos, como artes e malhas, criação de um loop principal do jogo, implementação de um pipeline de renderização simples e adição de som e controles.

Apresentamos técnicas e considerações referentes ao desenvolvimento de jogos UWP. Não fornecemos um jogo completo de ponta a ponta. Em vez disso, nos concentramos em conceitos de desenvolvimento de jogos UWP DirectX e fazemos considerações específicas sobre o Windows Runtime referentes a esses conceitos.

## <a name="objective"></a>Objetivo


-   Usar os conceitos e componentes básicos de um jogo DirectX UWP e ficar mais confortável com o design de jogos UWP com DirectX.

## <a name="what-you-need-to-know-before-starting"></a>O que você precisa saber antes de começar


Antes de começar este tutorial, você precisar se familiarizar com estes assuntos.

-   Microsoft C++ com Extensões de Componentes (C++/CX). Esta é uma atualização do Microsoft C++ que incorpora contagem automática de referências, além de ser a linguagem de desenvolvimento de jogos UWP com DirectX 11.1 ou versões posteriores.
-   Álgebra linear básica com conceitos de física Newtoniana.
-   Terminologia básica de programação gráfica.
-   Conceitos básicos de programação no Windows.
-   Familiaridade básica com APIs de [Direct2D](https://msdn.microsoft.com/library/windows/apps/dd370990.aspx) e [Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/hh404569).

##  <a name="the-windows-store-direct3d-shooting-game-sample"></a>O jogo de tiro de exemplo da Windows Store em Direct3D


Essa amostra implementa uma galeria de tiro simples em primeira pessoa, onde o jogador lança bolas contra alvos em movimento. Cada tiro no alvo concede um certo número de pontos, e o jogador pode passar por seis níveis com desafio crescente. No fim dos níveis, os pontos são calculados e o jogador recebe uma pontuação final.

O exemplo demonstra os conceitos do jogo:

-   Interoperação entre DirectX 11.1 e o Windows Runtime
-   Câmera e perspectiva 3D em primeira pessoa
-   Efeitos 3D estereoscópicos
-   Detecção de colisão entre objetos em 3D
-   Manipulação de entrada do usuário para controles por mouse, toque e joystick do Xbox 360
-   Mixagem e reprodução de áudio
-   Uma máquina de estado do jogo simples

![o exemplo do jogo em ação](images/simple3dgame-display.png)


| Tópico | Descrição |
|---------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Configurar o projeto de jogo](tutorial--setting-up-the-games-infrastructure.md) | O primeiro passo na montagem do jogo é configurar um projeto no Microsoft Visual Studio de modo a minimizar a quantidade de trabalho de infraestrutura de código necessária. Você pode poupar muito tempo e problemas usando os modelos certos e configurando o projeto especificamente para o desenvolvimento de jogos. Guiaremos você pela instalação e configuração de um projeto de jogo simples. |
| [Definir a estrutura do aplicativo UWP do jogo](tutorial--building-the-games-metro-style-app-framework.md) | A primeira parte da codificação de um jogo UWP com DirectX é criar a estrutura que permite que o objeto do jogo interaja com o Windows. Isso inclui propriedades do Windows Runtime, como manipulação de eventos de suspensão/retomada, foco da janela e ajuste, além de eventos, interações e transições da interface do usuário. Vamos examinar como o jogo de exemplo é estruturado e como ele define a máquina de estado de alto nível para a interação entre jogador e sistema. |
| [Definir o objeto principal do jogo](tutorial--defining-the-main-game-loop.md) | Agora, observaremos os detalhes do objeto principal do exemplo de jogo e como as regras implementadas são convertidas em interações com o ambiente do jogo. |
| [Montar a estrutura de renderização](tutorial--assembling-the-rendering-pipeline.md) | Agora chegou o momento de examinar como o jogo de exemplo usa essa estrutura e esse estado para exibir seus elementos gráficos. Aqui vemos a implementação de uma estrutura de renderização, desde a inicialização do dispositivo gráfico até a apresentação dos objetos gráficos para exibição. |
| [Adicionar uma interface do usuário](tutorial--adding-a-user-interface.md) | Você viu no exemplo como é implementado o objeto principal do jogo, bem como a estrutura básica de renderização. Agora examinaremos como o exemplo fornece feedback sobre o estado do jogo ao jogador. Aqui você aprenderá como adicionar opções de menu simples e componentes do visor frontal HUD (Heads-Up Display) sobre a saída do pipeline gráfico 3D. |
| [Adicionar controles](tutorial--adding-controls.md) | Agora veremos como o jogo de exemplo implementa controles move-look em um jogo 3D e como desenvolver controles básicos de toque, mouse e controlador de jogo. |
| [Adicionar som](tutorial--adding-sound.md) | Nesta etapa, examinaremos como o exemplo de jogo de tiro cria um objeto para reprodução de som usando as APIs [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813). |
| [Estender o exemplo de jogo](tutorial-resources.md) | Parabéns! A esta altura, você já tem um bom entendimento dos componentes principais de um jogo UWP em DirectX 3D. Você pode configurar a estrutura de um jogo, inclusive o provedor de exibição e o pipeline de renderização, e implementar um loop básico de jogo. Você também pode criar uma sobreposição de interface do usuário básica, além de incorporar sons e controles. Você está a caminho de criar um jogo por conta própria. Aqui estão alguns recursos para aprofundar seus conhecimentos sobre o desenvolvimento de jogos em DirectX. |
 

 

 





