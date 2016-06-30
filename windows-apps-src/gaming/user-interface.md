---
author: mtoepke
title: Modelos de projeto de jogo DirectX
description: Saiba mais sobre os modelos para criar um jogo da Plataforma Universal do Windows (UWP) e DirectX.
ms.assetid: 41b6cd76-5c9a-e2b7-ef6f-bfbf6ef7331d
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: b7d01fda0bc8dbafebb7485ec01acb5c7431a815

---

# Modelos de projeto de jogo DirectX


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Os modelos DirectX e UWP (Plataforma Universal do Windows) permitem criar rapidamente um projeto como ponto de partida para seu jogo.

## Pré-requisitos


Para criar o projeto, você precisa:

-   [Baixar o Microsoft Visual Studio 2015](https://www.visualstudio.com/vs-2015-product-editions). O Visual Studio 2015 possui ferramentas de programação gráfica, como ferramentas de depuração. Para uma visão geral dos recursos e das ferramentas de elementos gráficos e jogos em DirectX, veja [Ferramentas do Visual Studio para desenvolvimento de jogos em DirectX](set-up-visual-studio-for-game-development.md).

## Escolhendo um modelo


O Visual Studio 2015 inclui três modelos DirectX e UWP:

-   Aplicativo DirectX 11 (Universal Windows) – o modelo Aplicativo DirectX 11 (Universal Windows) cria um projeto UWP, que renderiza diretamente em uma janela de aplicativo usando o DirectX 11.
-   Aplicativo DirectX 12 (Universal Windows) – o modelo Aplicativo DirectX 12 (Universal Windows) cria um projeto UWP, que renderiza diretamente em uma janela de aplicativo usando o DirectX 12.
-   Aplicativo DirectX 11 e XAML (Universal Windows) – O modelo Aplicativo DirectX 11 e XAML (Universal Windows) cria um projeto UWP, que renderiza dentro de um controle XAML usando DirectX 11. Esse modelo usa um [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834), de maneira que você possa usar controles da interface do usuário XAML. Isso pode tornar mais fácil adicionar elementos de interface do usuário, mas usar o modelo XAML pode resultar em um desempenho inferior.

O modelo escolhido depende do desempenho e das tecnologias que você deseja usar.

## Estrutura do modelo


Os modelos DirectX universais do Windows contêm os seguintes arquivos:

-   pch.h e pch.cpp – Suporte de cabeçalho pré-compilado.
-   Package.appxmanifest – As propriedades do pacote de implantação do aplicativo.
-   \*.pfx – Certificados do aplicativo.
-   Dependências externas – Links para arquivos externos usados pelo projeto.
-   \*Main.h and \*Main.cpp – Métodos para gerenciar ativos de aplicativo, atualizando o estado do aplicativo e renderizando o quadro.
-   App.h e App.cpp – Ponto de entrada principal do aplicativo. Conecta o aplicativo ao shell do Windows e manipula eventos de ciclo de vida do aplicativo. Esses arquivos só aparecem nos modelos de aplicativo DirectX 11 (Universal Windows) e aplicativo DirectX 12 (Universal Windows).
-   App.xaml, App.xaml.cpp e App.xaml.h – Ponto de entrada principal do aplicativo. Conecta o aplicativo ao shell do Windows e manipula eventos de ciclo de vida do aplicativo. Esses arquivos só aparecem no modelo de aplicativo DirectX 11 e XAML (Universal Windows).
-   DirectXPage.xaml, DirectXPage.xaml.cpp e DirectXPage.xaml.h – Uma página que hospeda um SwapChainPanel DirectX. Esses arquivos só aparecem no modelo de aplicativo DirectX 11 e XAML (Universal Windows).
-   Conteúdo
    -   Sample3DSceneRenderer.h e Sample3DSceneRenderer.cpp – um exemplo de renderizador que instancia um pipeline de renderização básica.
    -   SampleFpsTextRenderer.h e SampleFpsTextRenderer.cpp – renderiza o valor atual de FPS no canto inferior direito da tela usando Direct2D e DirectWrite. Esses arquivos só aparecem nos modelos de aplicativo DirectX 11 (Universal Windows) e aplicativo DirectX 11 e XAML (Universal Windows).
    -   SamplePixelShader.hlsl – um exemplo simples de sombreador de pixel.
    -   SampleVertexShader.hlsl – um exemplo simples de sombreador de vértice.
    -   ShaderStructures.h – estruturas usadas para enviar data para o exemplo de sombreador de vértice.
-   Comum
    -   StepTimer.h – uma classe auxiliar de tempo de animação e simulação.
    -   DirectXHelper.h – funções auxiliares diversas.
    -   DeviceResources.h e Device Resources.cpp – fornece uma interface para um aplicativo que possui DeviceResources ser notificado do dispositivo que está sendo perdido ou criado.
    -   d3dx12.h – contém a biblioteca de utilitários D3DX12. Esse arquivo aparece apenas no aplicativo DirectX 12 (Universal Windows).
-   Ativos – imagens de logotipo e splashscreen usadas pelo aplicativo.

## Próximas etapas


Agora que você tem um ponto de partida, adicione essas informações para criar seus próprios conhecimentos e habilidades de desenvolvimento de jogos da Windows Store.

Caso esteja fazendo a portabilidade de um jogo existente, consulte os tópicos a seguir.

-   [Portar de OpenGL ES 2.0 para Direct3D 11.1](port-from-opengl-es-2-0-to-directx-11-1.md)
-   [Portar do DirectX 9 para a Plataforma Universal do Windows](porting-your-directx-9-game-to-windows-store.md)

Caso esteja criando um novo jogo em DirectX, consulte os tópicos a seguir.

-   [Criar um jogo UWP simples com o DirectX](tutorial--create-your-first-metro-style-directx-game.md)
-   [Desenvolvendo o Marble Maze, um jogo da Plataforma Universal do Windows em C++ e DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

> **Observação**  
Este artigo se destina a desenvolvedores do Windows 10 que escrevem aplicativos UWP (Plataforma Universal do Windows). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

 

 







<!--HONumber=Jun16_HO4-->


