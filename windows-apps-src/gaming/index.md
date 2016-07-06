---
author: mtoepke
title: Jogos e DirectX
description: A UWP (Plataforma Universal do Windows) oferece novas oportunidades para criar, distribuir e monetizar jogos. Saiba como iniciar um novo jogo ou portar um jogo existente.
ms.assetid: 4073b835-c900-4ff2-9fc5-da52f9432a1f
ms.sourcegitcommit: 41ee0d2a45408b5b1a0dbc0b102f1b59843814b2
ms.openlocfilehash: e5447f6238ece768513d160579e1c7e89b04e509

---

# Jogos e DirectX


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

A UWP (Plataforma Universal do Windows) oferece novas oportunidades para criar, distribuir e monetizar jogos. Saiba como iniciar um novo jogo ou portar um jogo existente.

| Tópico | Descrição |
|---------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Guia de desenvolvimento de jogos do Windows 10](e2e.md) | Um guia completo de recursos e informações para desenvolver jogos UWP. |
| [Tecnologias de jogos para aplicativos da Plataforma Universal do Windows](game-development-platform-guide.md) | Neste guia, você aprenderá sobre as tecnologias disponíveis para o desenvolvimento de jogos UWP. |
| [Modelos de projetos e ferramentas para jogos](prepare-your-dev-environment-for-windows-store-directx-game-development.md) | Mostra o que é necessário para começar a programar jogos no DirectX para a UWP. |
| [Objeto de aplicativo e DirectX](about-the-metro-style-user-interface-and-directx.md) | A UWP com jogos do DirectX não usa muitos elementos e objetos da interface do usuário do Windows. Em vez disso, por serem executados em um nível mais baixo na pilha do Windows Runtime, eles devem interoperam com a estrutura da IU de uma maneira mais básica: acessando e interoperando com o objeto do aplicativo diretamente. Saiba como e quando essa interoperação ocorre e como você, como desenvolvedor do DirectX, pode usar esse modelo no desenvolvimento de seu aplicativo UWP de forma eficaz. |
| [Iniciando e retomando aplicativos](launching-and-resuming-apps-directx-and-cpp.md) | Saiba como iniciar, suspender e retomar seu aplicativo UWP do DirectX. |
| [Elementos gráficos 2D para jogos DirectX](working-with-2d-graphics-in-your-directx-game.md) | Falaremos sobre o uso de gráficos e efeitos de bitmap 2D e como usá-los em seu jogo. |
| [Elementos gráficos 3D básicos para jogos em DirectX](an-introduction-to-3d-graphics-with-directx.md) | Mostramos como usar programação do DirectX para implementar os conceitos fundamentais de elementos gráficos 3D. |
| [Carregar recursos no jogo em DirectX](load-a-game-asset.md) | A maioria dos jogos, em algum momento, carrega recursos e ativos (por exemplo, sombreadores, texturas, malhas predefinidas ou outros dados gráficos) do armazenamento local ou de algum outro fluxo de dados. Aqui, vamos examinar uma exibição de alto nível daquilo que é necessário considerar ao carregar esses arquivos para uso em seu jogo UWP. |
| [Criar um jogo UWP simples com o DirectX](tutorial--create-your-first-metro-style-directx-game.md) | Neste conjunto de tutoriais você aprenderá como criar um jogo UWP básico com o DirectX e o C++. Abordamos todas as partes principais de um jogo, inclusive os processos de carregamento de ativos, como artes e malhas, criação de um loop principal do jogo, implementação de um pipeline de renderização simples e adição de som e controles. |
| [Desenvolvendo o Marble Maze, um jogo da Plataforma Universal do Windows no C++ e no DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md) | Esta seção da documentação descreve como usar o DirectX e o Visual C++ para criar um jogo UWP em 3D. Esta documentação mostra como criar um jogo 3D denominado Marble Maze que adota novos fatores forma, como tablets, e também funciona em desktops e notebooks tradicionais. |
| [Suporte à orientação de tela](supporting-screen-rotation-directx-and-cpp.md) | Discutiremos neste documento as práticas recomendadas para manipular a rotação da tela no aplicativo UWP do DirectX, para que o hardware gráfico do dispositivo Windows 10 seja usado de forma eficiente e eficaz. |
| [Áudio para jogos](working-with-audio-in-your-directx-game.md) | Aprenda como desenvolver e incorporar música e sons a seu jogo do DirectX e como processar sinais de áudio para criar sons dinâmicos e posicionais. |
| [Controles de toque para jogos](tutorial--adding-touch-controls-to-your-directx-game.md) | Aprenda como adicionar controles de toque básicos ao seu jogo UWP do C++ com o DirectX. Mostraremos como adicionar controles baseados em toque para mover uma câmera com plano fixo em um ambiente Direct3D, no qual arrastar com o dedo ou a caneta muda a perspectiva da câmera. |
| [Controles move-look para jogos](tutorial--adding-move-look-controls-to-your-directx-game.md) | Aprenda como adicionar controles move-look tradicionais (também conhecidos como controle mouselook) usando o mouse e o teclado ao seu jogo do DirectX. |
| [Otimizar entrada e loop de renderização](optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md) | A latência de entrada pode impactar significativamente a experiência de um jogo, e a sua otimização pode tornar o jogo mais bem-acabado. Além disso, a otimização adequada dos eventos de entrada pode aumentar a vida útil da bateria. Saiba como escolher as opções corretas de processamento dos eventos de entrada do [CoreDispatcher](optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md) para verificar se seu jogo controla a entrada da melhor forma possível. |
| [Dimensionamento e sobreposições de cadeia de troca](multisampling--scaling--and-overlay-swap-chains.md) | Saiba como criar cadeias de troca dimensionadas para permitir renderização mais rápida em dispositivos móveis e usar cadeias de troca sobrepostas (quando disponíveis) para aumentar a qualidade visual. |
| [Reduzir a latência com cadeias de troca DXGI 1.3](reduce-latency-with-dxgi-1-3-swap-chains.md) | Use o DXGI 1.3 para reduzir a latência de quadros eficaz aguardando a cadeia de troca sinalizar o horário apropriado para começar a renderizar um novo quadro. |
| [Multisampling em aplicativos UWP](multisampling--multi-sample-anti-aliasing--in-windows-store-apps.md) | Aprenda a usar multisampling em aplicativos UWP criados com o Direct3D. |
| [Manipular cenários removidos de dispositivos no Direct3D 11](handling-device-lost-scenarios.md) | Este tópico explica como recriar a cadeia de interface do dispositivo Direct3D e DXGI quando o adaptador gráfico é removido ou reinicializado. |
| [Programação assíncrona para jogos](asynchronous-programming-directx-and-cpp.md) | Este tópico cobre vários pontos a serem considerados ao utilizar a programação assíncrona e threading com o DirectX. |
| [Rede para jogos](work-with-networking-in-your-directx-game.md) | Saiba como desenvolver e incorporar recursos de rede em seu jogo com o DirectX. |
| [Acessibilidade para jogos](accessibility-for-games.md) | Aprenda a tornar os jogos mais acessíveis. |
| [Nuvem para jogos](cloud-for-games.md) | Aprenda a fazer uso de tecnologias de nuvem para desenvolvimento de jogos. |
| [Interoperabilidade entre DirectX e XAML](directx-and-xaml-interop.md) | Você pode usar o XAML (Extensible Application Markup Language) e o Microsoft DirectX juntos em seu jogo UWP. |
| [Empacotar seu jogo](package-your-windows-store-directx-game.md) | Os jogos UWP maiores, especialmente os que dão suporte a vários idiomas com ativos específicos à região ou que possuem ativos opcionais de alta definição podem ser facilmente dimensionados para tamanhos grandes. Neste tópico, você aprenderá a usar pacotes e lotes de aplicativos para personalizar seu aplicativo de forma que os clientes recebam apenas os recursos de que realmente precisam. |
| [Guias de portabilidade para jogos](porting-guides.md) | Fornece guias para portabilidade de seus jogos existentes para o Direct3D 11, a UWP e o Windows 10. |
| [Recursos de programação de jogos](additional-directx-game-programming-resources.md) | Para saber mais sobre programação de jogos no Windows, confira os recursos a seguir. |

 

> **Observação**  
Este artigo se destina a desenvolvedores do Windows 10 que escrevem aplicativos UWP (Plataforma Universal do Windows). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

Para fazer o melhor uso das visões gerais e dos tutoriais de desenvolvimento de jogos, você deve estar familiarizado com os seguintes assuntos:

-   Microsoft C++ com Extensões de Componentes (C++/CX). Esta é uma atualização do Microsoft C++ que incorpora contagem automática de referências, além de ser a linguagem de desenvolvimento de jogos UWP com DirectX 11.1 ou versões posteriores.
-   Terminologia básica de programação gráfica.
-   Conceitos básicos de programação no Windows.
-   Familiaridade básica com APIs do Direct3D 9 ou 11.

 

 







<!--HONumber=Jun16_HO4-->


