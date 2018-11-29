---
title: Criar um jogo UWP (Plataforma Universal do Windows) com DirectX
description: Este conjunto de tutoriais mostra como criar um jogo básico UWP (Plataforma Universal do Windows) em DirectX e C++.
ms.assetid: 9edc5868-38cf-58cc-1fb3-8fb85a7ab2c9
keywords: jogo de exemplo em DirectX, exemplo de jogo, UWP (Plataforma Universal do Windows), jogo em Direct3D 11
ms.date: 12/01/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: dc602e2dd29231c1e6554d7ef55e9666a373fa31
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8193829"
---
# <a name="create-a-simple-universal-windows-platform-uwp-game-with-directx"></a>Criar um jogo simples UWP (Plataforma Universal do Windows) com DirectX

Este conjunto de tutoriais mostra como criar um jogo básico UWP (Plataforma Universal do Windows) em DirectX e C++. Abordamos todas as partes principais de um jogo, inclusive os processos de carregamento de ativos, como artes e malhas, criação de um loop principal do jogo, implementação de um pipeline de renderização simples e adição de som e controles.

Apresentamos técnicas e considerações referentes ao desenvolvimento de jogos UWP. Não fornecemos um jogo completo de ponta a ponta. Em vez disso, nos concentramos em conceitos de desenvolvimento de jogos UWP DirectX e fazemos considerações específicas sobre o Windows Runtime referentes a esses conceitos.

## <a name="objective"></a>Objetivo

Usar os conceitos e componentes básicos de um jogo DirectX UWP e ficar mais confortável com o design de jogos UWP com DirectX.

## <a name="what-you-need-to-know-before-starting"></a>O que você precisa saber antes de começar


Antes de começar este tutorial, você precisar se familiarizar com estes assuntos.

-   Microsoft C++ com Extensões de Linguagem do Windows Runtime (C++/CX). Esta é uma atualização do Microsoft C++ que incorpora contagem automática de referências, além de ser a linguagem de desenvolvimento de jogos UWP com DirectX 11.1 ou versões posteriores.
-   Álgebra linear básica com conceitos de física Newtoniana.
-   Terminologia básica de programação gráfica.
-   Conceitos básicos de programação no Windows.
-   Familiaridade básica com APIs de [Direct2D](https://msdn.microsoft.com/library/windows/apps/dd370990.aspx) e [Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/hh404569).

##  <a name="direct3d-uwp-shooting-game-sample"></a>Exemplo de jogo de tiro em Direct3D UWP


Essa amostra implementa uma galeria de tiro simples em primeira pessoa, onde o jogador lança bolas contra alvos em movimento. Cada tiro no alvo concede um certo número de pontos, e o jogador pode passar por seis níveis com desafio crescente. No fim dos níveis, os pontos são calculados e o jogador recebe uma pontuação final.

O exemplo demonstra os conceitos do jogo:

-   Interoperação entre DirectX 11.1 e o Windows Runtime
-   Câmera e perspectiva 3D em primeira pessoa
-   Efeitos 3D estereoscópicos
-   Detecção de colisão entre objetos em 3D
-   Manipulação de entrada do usuário para controles por mouse, toque e joystick do Xbox
-   Mixagem e reprodução de áudio
-   Uma máquina de estado do jogo simples

![o exemplo do jogo em ação](images/simple-dx-game-overview.png)

| Tópico | Descrição |
|-------|-------------|
|[Configurar o projeto de jogo](tutorial--setting-up-the-games-infrastructure.md) | O primeiro passo na montagem do jogo é configurar um projeto no Microsoft Visual Studio de modo a minimizar a quantidade de trabalho de infraestrutura de código necessária. Você pode poupar muito tempo e problemas usando os modelos certos e configurando o projeto especificamente para o desenvolvimento de jogos. Guiaremos você em meio à instalação e à configuração de um projeto de jogo simples. |
| [Definir a estrutura do aplicativo UWP do jogo](tutorial--building-the-games-uwp-app-framework.md) | Crie uma estrutura que permite ao objeto do jogo DirectX UWP interagir com o Windows. Isso inclui propriedades do Windows Runtime, como a manipulação de eventos de suspensão/retomada, o foco da anela e os ajustes.  |
| [Gerenciamento de fluxo de jogo](tutorial-game-flow-management.md) | Defina a máquina de estado de alto nível para habilitar o player e a interação do sistema. Saiba como a interface do usuário interage com a máquina de estado do jogo geral e como criar manipuladores de eventos para jogos da UWP. |
| [Definir o objeto principal do jogo](tutorial--defining-the-main-game-loop.md) | Defina como o jogo será reproduzido por meio da criação de regras. |
| [Estrutura de renderização I: introdução à renderização](tutorial--assembling-the-rendering-pipeline.md) | Monte uma estrutura de renderização para exibir elementos gráficos. Este tópico está dividido em duas partes. A introdução à renderização explica como apresentar os objetos de cena para exibição na tela. |
| [Estrutura de renderização II: introdução ao jogo](tutorial-game-rendering.md) | Na segunda parte do tópico renderização, saiba como preparar os dados necessários antes da renderização. |
| [Adicionar uma interface do usuário](tutorial--adding-a-user-interface.md) | Adicione opções de menu simples e componentes de painel transparente, fornecendo comentários ao player. |
| [Adicionar controles](tutorial--adding-controls.md) | Adicionar controles move-look ao jogo &mdash; controles básicos de toque, de mouse e de controlador de jogo. |
| [Adicionar som](tutorial--adding-sound.md) | Saiba como criar sons para o jogo usando APIs de [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813). |
| [Estender o exemplo de jogo](tutorial-resources.md) | Recursos para aprofundar seu conhecimento sobre desenvolvimento de jogos DirectX, inclui o uso de XAML para criar sobreposições. |