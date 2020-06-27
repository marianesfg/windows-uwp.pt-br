---
title: Criar um jogo UWP (Plataforma Universal do Windows) com DirectX
description: Neste conjunto de tutoriais, você aprenderá a usar o DirectX e o [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/) para criar o jogo de exemplo de plataforma universal do Windows básico (UWP) chamado **Simple3DGameDX**.
ms.assetid: 9edc5868-38cf-58cc-1fb3-8fb85a7ab2c9
keywords: Jogo de exemplo do DirectX, jogo de exemplo, Plataforma Universal do Windows (UWP), jogo do Direct3D 11
ms.date: 06/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2e3007cd79546cba8961000cb2aae44b0b0536fe
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409565"
---
# <a name="create-a-simple-universal-windows-platform-uwp-game-with-directx"></a>Criar um jogo simples UWP (Plataforma Universal do Windows) com DirectX

Neste conjunto de tutoriais, você aprenderá a usar o DirectX e o [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/) para criar o jogo de exemplo de plataforma universal do Windows básico (UWP) chamado **Simple3DGameDX**. O jogo ocorre em uma galeria simples de capturas 3D de primeira pessoa.

> [!NOTE]
> O link do qual você pode baixar o próprio jogo de exemplo do **Simple3DGameDX** é o [jogo de exemplo do Direct3D](/samples/microsoft/windows-universal-samples/simple3dgamedx/). O código-fonte do C++/WinRT está na pasta chamada `cppwinrt` . Para obter informações sobre outros aplicativos de exemplo UWP, consulte [obter exemplos de aplicativos UWP](/windows/uwp/get-started/get-uwp-app-samples).

Esses tutoriais abrangem todas as partes principais de um jogo, incluindo os processos para carregar ativos, como artes e malhas, criar um loop de jogo principal, implementar um pipeline de renderização simples e adicionar som e controles.

Você também verá as técnicas e considerações de desenvolvimento de jogos UWP. Vamos nos concentrar nos principais conceitos de desenvolvimento de jogos do DirectX e chamar considerações específicas de tempo de execução do Windows em relação a esses conceitos.

## <a name="objective"></a>Objetivo

Para saber mais sobre os conceitos básicos e componentes de um jogo de DirectX do UWP e se tornar mais confortável a criação de jogos de UWP com o DirectX.

## <a name="what-you-need-to-know"></a>O que você precisa saber

Para este tutorial, você precisa estar familiarizado com esses assuntos.

- [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/). C++/WinRT é uma projeção de linguagem C++ 17 padrão moderna para APIs de Windows Runtime (WinRT), implementada como uma biblioteca baseada em arquivo de cabeçalho e projetada para fornecer a você acesso de primeira classe às APIs modernas do Windows.
- Álgebra linear básica com conceitos de física Newtoniana.
- Terminologia básica de programação gráfica.
- Conceitos básicos de programação no Windows.
- Familiaridade básica com APIs de [Direct2D](/windows/desktop/Direct2D/direct2d-portal) e [Direct3D 11](/windows/desktop/direct3d11/how-to-use-direct3d-11).

##  <a name="direct3d-uwp-shooting-gallery-sample"></a>Exemplo da Galeria de captura do Direct3D UWP

O jogo de exemplo **Simple3DGameDX** implementa uma galeria simples de introdução 3D de primeira pessoa, em que o jogador aciona bolas em destinos de movimentação. Cada tiro no alvo concede um certo número de pontos, e o jogador pode passar por seis níveis com desafio crescente. No fim dos níveis, os pontos são calculados e o jogador recebe uma pontuação final.

O exemplo demonstra esses conceitos de jogos.

- Interoperação entre DirectX 11.1 e o Windows Runtime
- Câmera e perspectiva 3D em primeira pessoa
- Efeitos 3D estereoscópicos
- Detecção de colisão entre objetos em 3D
- Manipulação de entrada do usuário para controles por mouse, toque e joystick do Xbox
- Mixagem e reprodução de áudio
- Um estado de jogo básico-máquina

![o jogo de exemplo em ação](images/simple-dx-game-overview.png)

|Tópico|Descrição|
|-------|-------------|
|[Configurar o projeto de jogo](tutorial--setting-up-the-games-infrastructure.md)|A primeira etapa no desenvolvimento do jogo é configurar um projeto no Microsoft Visual Studio. Depois de configurar um projeto especificamente para desenvolvimento de jogos, você poderá reutilizá-lo posteriormente como um tipo de modelo.|
|[Definir a estrutura do aplicativo UWP do jogo](tutorial--building-the-games-uwp-app-framework.md)|A primeira etapa na codificação de um jogo de Plataforma Universal do Windows (UWP) é a criação da estrutura que permite ao objeto de aplicativo interagir com o Windows.|
|[Gerenciamento de fluxo de jogo](tutorial-game-flow-management.md)|Defina a máquina de estado de alto nível para habilitar o player e a interação do sistema. Saiba como a interface do usuário interage com a máquina de estado do jogo geral e como criar manipuladores de eventos para jogos da UWP.|
|[Definir o objeto principal do jogo](tutorial--defining-the-main-game-loop.md)|Agora, veremos os detalhes do objeto principal do jogo de exemplo e como as regras que ele implementa são traduzidas em interações com o mundo do jogo.|
|[Estrutura de renderização I: introdução à renderização](tutorial--assembling-the-rendering-pipeline.md)|Saiba como desenvolver o pipeline de renderização para exibir gráficos. Introdução à renderização.|
|[Estrutura de renderização II: introdução ao jogo](tutorial-game-rendering.md)|Saiba como montar o pipeline de renderização para exibir elementos gráficos. Renderização de jogos, configurar e preparar dados.|
|[Adicionar uma interface do usuário](tutorial--adding-a-user-interface.md)|Saiba como adicionar uma sobreposição de interface de usuário 2D a um jogo do DirectX UWP.|
|[Adicionar controles](tutorial--adding-controls.md)|Agora, veremos como o jogo de exemplo implementa controles de mudança de aparência em um jogo 3D e como desenvolver controles de toque, mouse e controlador de jogo básicos.|
|[Adicionar som](tutorial--adding-sound.md)|Desenvolva um mecanismo de som simples usando APIs XAudio2 para reproduzir música de jogos e efeitos sonoros.|
|[Estender o jogo de exemplo](tutorial-resources.md)|Saiba como implementar uma sobreposição XAML para um jogo UWP DirectX.|
