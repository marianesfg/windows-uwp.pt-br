---
title: Perguntas frequentes de portabilidade do DirectX 11
description: Respostas a perguntas frequentes sobre a portabilidade de jogos para a Plataforma Universal do Windows (UWP).
ms.assetid: 79c3b4c0-86eb-5019-97bb-5feee5667a2d
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, directx 11
ms.localizationpriority: medium
ms.openlocfilehash: d2f883e62cf7c61560295673cf48cf891befed91
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662611"
---
# <a name="directx-11-porting-faq"></a>Perguntas frequentes de portabilidade do DirectX 11




Respostas a perguntas frequentes sobre a portabilidade de jogos para a Plataforma Universal do Windows (UWP).

## <a name="is-porting-my-game-going-to-be-a-set-of-search-and-replace-operations-on-api-methods-or-do-i-need-to-plan-for-a-more-thoughtful-porting-process"></a>A portabilidade de meu jogo será um conjunto de operações de localização e substituição em métodos de API? Ou é necessário planejar um processo de portabilidade mais cuidadoso?


O Direct3D 11 é uma atualização importante do Direct3D 9. Há diversas mudanças, incluindo APIs separadas para o adaptador gráfico virtualizado e seu contexto, além de uma nova camada de polimorfismo para recursos de dispositivo. Seu jogo ainda pode usar hardware gráfico do mesmo modo, mas você precisará aprender sobre a nova arquitetura de API do Direct3D 11 e atualizar cada parte do código gráfico para utilização dos componentes de API corretos. Veja os [conceitos e considerações de portabilidade](porting-considerations.md).

## <a name="what-is-the-new-device-context-for-am-i-supposed-to-replace-my-direct3d-9-device-with-the-direct3d-11-device-the-device-context-or-both"></a>Para que serve o novo contexto de dispositivo? Devo substituir o dispositivo do Direct3D 9 pelo dispositivo/contexto de dispositivo/ambos do Direct3D 11?


Agora o dispositivo Direct3D é usado para criar recursos na memória de vídeo, enquanto o contexto de dispositivo é usado para definir o estado do pipeline e gerar comandos de renderização. Para obter mais informações, consulte: [Quais são as mais importantes alterações desde o Direct3D 9?](understand-direct3d-11-1-concepts.md)

##  <a name="do-i-have-to-update-my-game-timer-for-uwp"></a>Preciso atualizar o cronômetro do jogo para a UWP?


[**QueryPerformanceCounter**](https://msdn.microsoft.com/library/windows/desktop/ms644904), juntamente com [ **QueryPerformanceFrequency com o propósito**](https://msdn.microsoft.com/library/windows/desktop/ms644905), ainda é a melhor maneira de implementar um temporizador de jogo para aplicativos UWP.

Você deve prestar atenção a um aspecto de temporizador e do ciclo de vida de aplicativos UWP. A suspensão/retomada é diferente da reexecução de um jogo de área de trabalho, pois seu jogo retomará um instantâneo do momento em que foi executado pela última vez. Caso tenha se passado muito tempo (por exemplo, algumas semanas), algumas implementações de cronômetro podem ter comportamento inadequado. Você pode usar eventos de ciclo de vida de aplicativo para redefinir o temporizador quando o jogo é retomado.

Os jogos que ainda usam a instrução RDTSC precisam de uma atualização. Consulte o tópico sobre [tempo do jogo e processadores com vários núcleos](https://msdn.microsoft.com/library/windows/desktop/ee417693).

## <a name="my-game-code-is-based-on-d3dx-and-dxut-is-there-anything-available-that-can-help-me-migrate-my-code"></a>O código do meu jogo é baseado em D3DX e DXUT. Há algum recurso disponível que me ajude a migrar esse código?


O projeto comunitário [DirectXTK (kit de ferramentas do DirectX)](https://go.microsoft.com/fwlink/p/?LinkID=248929) oferece classes de ajuda para usar com aplicativos em Direct3D 11.

##  <a name="how-do-i-maintain-code-paths-for-the-desktop-and-the-microsoft-store"></a>Como manter a caminhos de código para a área de trabalho e a Microsoft Store?


Série de artigos de Chuck Walbourn intitulada [técnicas de codificação de uso duplo para jogos](https://go.microsoft.com/fwlink/p/?LinkID=286210) oferece orientação sobre como compartilhar código entre a área de trabalho e os caminhos de código da Microsoft Store.

##  <a name="how-do-i-load-image-resources-in-my-directx-uwp-app"></a>Como carregar recursos de imagem em meu aplicativo UWP em DirectX?


Há dois caminhos de API para carregar imagens:

-   O pipeline de conteúdo converte imagens em arquivos DDS, usados como recursos de textura Direct3D. Consulte o tópico sobre [uso de ativos 3D em jogos ou aplicativos](https://msdn.microsoft.com/library/windows/apps/hh972446.aspx).
-   O [Windows Imaging Component](https://msdn.microsoft.com/library/windows/desktop/ee719902) pode ser usado para carregar imagens de diversos formatos, inclusive bitmaps Direct2D e recursos de textura Direct3D.

Você também pode usar DDSTextureLoader e WICTextureLoader a partir de [DirectXTK](https://go.microsoft.com/fwlink/p/?LinkID=248929) ou [DirectXTex](https://go.microsoft.com/fwlink/p/?LinkID=248926).

## <a name="where-is-the-directx-sdk"></a>Onde está o SDK do DirectX?


O SDK do DirectX faz parte do SDK do Windows. O SDK do DirectX mais recente separado do SDK do Windows é de junho de 2010. Os exemplos de Direct3D estão na galeria de códigos, juntamente com o restante dos exemplos de aplicativos do Windows.

## <a name="what-about-directx-redistributables"></a>E quanto aos componentes redistribuíveis do DirectX?


A grande maioria dos componentes do SDK do Windows já vem em versões compatíveis do sistema operacional ou não têm componente DLL (como DirectXMath). Todos os componentes de APIs Direct3D que os aplicativos UWP podem usar serão disponibilizados para o jogo; você não precisa redistribuí-los.

Os aplicativos da área de trabalho Win32 ainda usam DirectSetup. Por isso, se você também estiver atualizando a versão do jogo para área de trabalho, veja a [implantação do Direct3D 11 para desenvolvedores de jogos](https://msdn.microsoft.com/library/windows/desktop/ee416644).

## <a name="is-there-any-way-i-can-update-my-desktop-code-to-directx-11-before-moving-away-from-effects"></a>Há algum modo de atualizar meu código de área de trabalho para o DirectX 11 antes de sair do Effects?


Consulte a [atualização do Effects para Direct3D 11](https://go.microsoft.com/fwlink/p/?LinkId=271568). O Effects 11 o ajuda a eliminar dependências de cabeçalhos herdados do SDK do DirectX; ele deve ser usado com aplicativos da área de trabalho como um auxílio à portabilidade.

##  <a name="is-there-a-path-for-porting-my-directx-8-game-to-uwp"></a>Há algum modo de fazer a portabilidade de meu jogo em DirectX 8 para a UWP?


Sim:

-   Leia o tópico sobre [conversão para o Direct3D 9](https://msdn.microsoft.com/library/windows/desktop/bb204851).
-   Certifique-se de que o jogo não tenha restos do pipeline fixo; veja os [recursos preteridos](https://msdn.microsoft.com/library/windows/desktop/cc308047).
-   Em seguida, levar o caminho de portabilidade do DirectX 9: [Porta do D3D 9 para UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

##  <a name="can-i-port-my-directx-10-or-11-game-to-uwp"></a>Posso fazer a portabilidade de meu jogo em DirectX 10 ou 11 para a UWP?


A portabilidade de jogos da área de trabalho em DirectX 10.x e 11 para a UWP pode ser feita facilmente. Veja o tópico sobre [migração para Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476190).

## <a name="how-do-i-choose-the-right-display-device-in-a-multi-monitor-system"></a>Como escolher o dispositivo de exibição certo em um sistema com mais de um monitor?


O usuário pode selecionar o monitor que exibirá o aplicativo. Deixe que o Windows ofereça o adaptador correto chamando [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) com o primeiro parâmetro definido como **nullptr**. Em seguida, pegue a [**interface IDXGIDevice**](https://msdn.microsoft.com/library/windows/desktop/bb174527) do dispositivo, chame [**GetAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174531) e use o adaptador DXGI para criar uma cadeia de troca.

## <a name="how-do-i-turn-on-antialiasing"></a>Como ativo a suavização?


A suavização (multiamostragem) é habilitada quando você cria o dispositivo Direct3D. Enumerar suporte multisampling chamando [ **CheckMultisampleQualityLevels**](https://msdn.microsoft.com/library/windows/desktop/ff476499), em seguida, definir opções de multisample no [ **DXGI\_exemplo\_Estrutura DESC** ](https://msdn.microsoft.com/library/windows/desktop/bb173072) quando você chama [ **CreateSurface**](https://msdn.microsoft.com/library/windows/desktop/bb174530).

## <a name="my-game-renders-using-multithreading-andor-deferred-rendering-what-do-i-need-to-know-for-direct3d-11"></a>Meu jogo é renderizado usando multithreading e/ou renderização adiada. O que preciso saber no caso do Direct3D 11?


Visite a [introdução ao multithreading no Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476891) para começar. Consulte uma lista com as principais diferenças no tópico sobre [diferenças de threading entre versões do Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476890). Lembre-se de que a renderização adiada usa um *contexto adiado* de dispositivo em vez de um *contexto imediato*.

## <a name="where-can-i-read-more-about-the-programmable-pipeline-since-direct3d-9"></a>Onde posso saber mais sobre o pipeline programável desde o Direct3D 9?


Visite os seguintes tópicos:

-   [Guia de programação para HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509635)
-   [Direct3D 10 perguntas frequentes](https://msdn.microsoft.com/library/windows/desktop/ee416643)

## <a name="what-should-i-use-instead-of-the-x-file-format-for-my-models"></a>O que devo usar em vez do formato de arquivo .x para meus modelos?


Embora não tenhamos uma substituição oficial para o formato de arquivo .x, muitos dos exemplos usam o formato SDKMesh. O Visual Studio também tem um [pipeline de conteúdo](https://msdn.microsoft.com/library/windows/apps/hh972446.aspx) que compila diversos formatos populares em arquivos CMO, que podem ser carregados com o código do kit inicial do Visual Studio 3D ou usando [DirectXTK](https://go.microsoft.com/fwlink/p/?LinkID=248929).

## <a name="how-do-i-debug-my-shaders"></a>Como depurar meus sombreadores?


Microsoft Visual Studio 2015 inclui ferramentas de diagnóstico de gráficos do DirectX. Veja [Depurando gráficos em DirectX](https://msdn.microsoft.com/library/windows/apps/hh315751.aspx).

##  <a name="what-is-the-direct3d-11-equivalent-for-x-function"></a>Qual é o equivalente à função *x* no Direct3D 11?


Veja o [mapeamento de funções](feature-mapping.md#function-mapping) fornecido no tópico sobre correlação entre recursos do DirectX 9 e APIs do DirectX 11.

##  <a name="what-is-the-dxgiformat-equivalent-of-y-surface-format"></a>O que é o DXGI\_equivalente a formato *y* formato superfície?


Veja o [mapeamento de formatos de superfície](feature-mapping.md#surface-format-mapping) fornecido no tópico sobre correlação entre recursos do DirectX 9 e APIs do DirectX 11.

 

 




