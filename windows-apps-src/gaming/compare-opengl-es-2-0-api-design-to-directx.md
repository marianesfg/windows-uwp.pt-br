---
author: mtoepke
title: Planejar a portabilidade do OpenGL ES 2.0 para o Direct3D
description: "Caso esteja fazendo a portabilidade de um jogo das plataformas iOS ou Android, você provavelmente investiu bastante no OpenGL ES 2.0."
ms.assetid: a31b8c5a-5577-4142-fc60-53217302ec3a
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, jogos, opengl, direct3d
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: d2642abbfbfc6030aa00f68f30d4a45eb0e86ee1
ms.lasthandoff: 02/07/2017

---

# <a name="plan-your-port-from-opengl-es-20-to-direct3d"></a>Planejar a portabilidade do OpenGL ES 2.0 para o Direct3D


\[ Atualizado para apps UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**APIs Importantes**

-   [Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476080)
-   [Visual C++](https://msdn.microsoft.com/library/windows/apps/60k1461a.aspx)

Caso esteja fazendo a portabilidade de um jogo das plataformas iOS ou Android, você provavelmente investiu bastante no OpenGL ES 2.0. Durante a preparação para mudar a base de código do pipeline gráfico para o Direct3D 11 e o Windows Runtime, há algumas coisas a se pensar antes de começar.

Geralmente, a maioria dos esforços de portabilidade envolve a transição inicial da base de código e o mapeamento de APIs e padrões comuns entre os dois modelos. Você verá que esse processo fica um pouco mais simples quando você reserva um tempo para ler e consultar este tópico.

Consulte algumas coisas a se prestar atenção ao fazer a portabilidade dos gráficos do OpenGL ES 2.0 para o Direct3D 11.

## <a name="notes-on-specific-opengl-es-20-providers"></a>Observações sobre provedores específicos do OpenGL ES 2.0


Os tópicos sobre portabilidade nesta seção fazem referência à implementação de Windows da especificação OpenGL ES 2.0, criada pelo Khronos Group. Todos os exemplos de códigos do OpenGL ES 2.0 foram desenvolvidos usando o Visual Studio 2012 e a sintaxe C básica do Windows. Se você estiver mudando de uma base de código Objective-C (iOS) ou Java (Android), lembre-se de que os exemplos de código OpenGL ES 2.0 fornecidos talvez não usem sintaxe ou parâmetros semelhantes de chamada a APIs. Estas instruções foram elaboradas de modo a independerem de plataformas na medida do possível.

Esta documentação usa apenas a as APIs da especificação 2.0 do código e da referência OpenGL ES. Mesmo que você esteja realizando a portabilidade a partir do OpenGL ES 1.1 ou 3.0, este conteúdo ainda terá utilidade, embora alguns exemplos de códigos e contextos do OpenGL ES 2.0 sejam diferentes.

Os exemplos de Direct3D 11 destes tópicos usam o Microsoft Windows C++ com CX (extensões de componentes). Para saber mais sobre essa versão da sintaxe C++, leia os tópicos sobre [Visual C++](https://msdn.microsoft.com/library/windows/apps/60k1461a.aspx), [extensões de componentes para plataformas de Tempo de Execução](https://msdn.microsoft.com/library/windows/apps/xey702bw.aspx) e [referência rápida (C++\\CX)](https://msdn.microsoft.com/library/windows/apps/br212455.aspx).

## <a name="understand-your-hardware-requirements-and-resources"></a>Compreenda os requisitos e recursos de hardware


O conjunto de recursos de processamento gráfico compatível com o OpenGL ES 2.0 corresponde, em linhas gerais, aos recursos fornecidos no Direct3D 9.1. Se quiser aproveitar os recursos mais avançados fornecidos no Direct3D 11, leia a documentação do [Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476080) ao planejar a portabilidade ou os tópicos de [portabilidade do DirectX 9 para a UWP (Plataforma Universal do Windows)](porting-your-directx-9-game-to-windows-store.md) ao concluir o trabalho inicial.

Para facilitar o trabalho de portabilidade inicial, comece com um modelo de Direct3D do Visual Studio. Ele fornece um renderizador básico já configurado para você e é compatível com recursos de apps UWP, como a recriação de recursos depois de alterações em janelas e níveis de recursos do Direct3D.

## <a name="understand-direct3d-feature-levels"></a>Compreenda os níveis de recursos do Direct3D


O Direct3D 11 oferece suporte a "níveis de recursos" de hardware, do 9\_1 (Direct3D 9.1) ao 11\_1. Esses níveis indicam a disponibilidade de certos recursos gráficos. Normalmente, a maioria das plataformas do OpenGL ES 2.0 oferece suporte ao conjunto de recursos do Direct3D 9.1 (nível 9\_1).

## <a name="review-directx-graphics-features-and-apis"></a>Confira as APIs e os recursos gráficos do DirectX


| Família de APIs                                                | Descrição                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534)                     | A DXGI (infraestrutura gráfica do DirectX) oferece uma interface entre o hardware gráfico e o Direct3D. Ela define o adaptador do dispositivo e as configurações de hardware usando as interfaces COM [**IDXGIAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174523) e [**IDXGIDevice1**](https://msdn.microsoft.com/library/windows/desktop/hh404543). Use-a para criar e configurar os buffers e outros recursos de janela. Destaque para o padrão de fábrica [**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556), usado para adquirir os recursos gráficos, incluindo a cadeia de troca (um conjunto de buffers de quadro). Como a cadeia de troca pertence à DXGI, a interface [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631) é usada para apresentar os quadros na tela. |
| [Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476080)       | O Direct3D é um conjunto de APIs que fornecem uma representação visual da interface gráfica e permitem desenhar gráficos usando-a. Com relação aos recursos, de modo geral a versão 11 se compara ao OpenGL 4.3. (Por outro lado, o OpenGL ES 2.0 é semelhante ao DirectX9, quando se trata de recursos, e ao OpenGL 2.0, mas com o pipeline de sombreador unificado do OpenGL 3.0). A maior parte do trabalho pesado é feita com as interfaces ID3D11Device1 e ID3D11DeviceContext1, que fornecem, respectivamente: acesso a recursos e sub-recursos; e o contexto de renderização.                                                                                                                                          |
| [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990)                      | O Direct2D fornece um conjunto de APIs para renderização 2D acelerada por GPU. Sua finalidade pode ser considerada semelhante à do OpenVG.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038)            | O DirectWrite fornece um conjunto de APIs para renderização de fontes de alta qualidade acelerada por GPU.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833)                  | O DirectXMath fornece um conjunto de APIs e macros para lidar com tipos, valores e funções algébricas lineares e trigonométricas comuns. Esses tipos e funções foram desenvolvidos para desempenhar um bom trabalho com o Direct3D e suas operações de renderizador.                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| [DirectX HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509580) | Sintaxe HLSL atual usada por sombreadores do Direct3D. Ela implementa o modelo de sombreador 5.0 do Direct3D.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

 

## <a name="review-the-windows-runtime-apis-and-template-library"></a>Confira as APIs e a biblioteca de modelos do Windows Runtime


As APIs do Windows Runtime fornecem a infraestrutura geral dos apps UWP. Confira-as [aqui](https://msdn.microsoft.com/library/windows/apps/br211377).

As principais APIs do Windows Runtime usadas na portabilidade do pipeline de elementos gráficos incluem:

-   [**Windows::UI::Core::CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)
-   [**Windows::UI::Core::CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)
-   [**Windows::ApplicationModel::Core::IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478)
-   [**Windows::ApplicationModel::Core::CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017)

Além disso a WRL (Biblioteca de Modelos C++ do Windows Runtime) é uma biblioteca de modelos que proporciona um modo específico de modificar e usar os componentes do Tempo de Execução do Windows. As APIs do Direct3D 11 para apps UWP são mais bem utilizadas em conjunto com as interfaces e os tipos dessa biblioteca, como ponteiros inteligentes ([ComPtr](https://msdn.microsoft.com/library/windows/apps/br244983.aspx)). Para saber mais sobre a WRL, leia [Biblioteca de Modelos C++ do Windows Runtime (WRL)](https://msdn.microsoft.com/library/windows/apps/hh438466.aspx).

## <a name="change-your-coordinate-system"></a>Altere o sistema de coordenadas


Uma diferença que causa confusão durante o trabalho inicial de portabilidade é a mudança do sistema de coordenadas tradicional (mão direita) do OpenGL para o sistema de coordenadas padrão do Direct3D, de mão esquerda. Essa mudança no modelo de coordenadas afeta muitas partes de seu jogo, desde a instalação e configuração dos buffers de vértice até muitas das funções matemáticas de matrizes. As duas alterações mais importantes que devem ser feitas são:

-   Inverter a ordem dos vértices de triângulos, de modo que o Direct3D os percorra em sentido horário, a partir da frente. Por exemplo, caso os vértices sejam indexados como 0, 1, e 2 no pipeline do OpenGL, passe-os para o Direct3D como 0, 2, 1.
-   Use a matriz de exibição para dimensionar o espaço do ambiente por -1.0f no sentido de z, invertendo as coordenadas do eixo z. Para isso, inverta o sinal dos valores nas posições M31, M32 e M33 da matriz de exibição (ao fazer a portabilidade para o tipo [**Matrix**](https://msdn.microsoft.com/library/windows/desktop/bb147180)). Caso M34 não seja 0, inverta seu sinal também.

Mas o Direct3D é compatível com um sistema de coordenadas de mão direita. O DirectXMath oferece diversas funções operadas em e entre sistemas de coordenadas de mão direita e esquerda. Elas podem ser usadas para preservar alguns dos dados originais e o processamento de matriz da malha, Elas incluem:

| Função de matriz do DirectXMath                                                   | Descrição                                                                                                                 |
|-------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| [**XMMatrixLookAtLH**](https://msdn.microsoft.com/library/windows/desktop/ee419969)                               | Cria uma matriz de exibição para um sistema de coordenadas de mão esquerda usando uma posição da câmera, um sentido para cima e um ponto focal.       |
| [**XMMatrixLookAtRH**](https://msdn.microsoft.com/library/windows/desktop/ee419970)                               | Cria uma matriz de exibição para um sistema de coordenadas de mão direita usando uma posição da câmera, um sentido para cima e um ponto focal.      |
| [**XMMatrixLookToLH**](https://msdn.microsoft.com/library/windows/desktop/ee419971)                               | Cria uma matriz de exibição para um sistema de coordenadas de mão esquerda usando uma posição da câmera, um sentido para cima e uma direção da câmera.  |
| [**XMMatrixLookToRH**](https://msdn.microsoft.com/library/windows/desktop/ee419972)                               | Cria uma matriz de exibição para um sistema de coordenadas de mão direita usando uma posição da câmera, um sentido para cima e uma direção da câmera. |
| [**XMMatrixOrthographicLH**](https://msdn.microsoft.com/library/windows/desktop/ee419975)                   | Cria uma matriz de projeção ortogonal para um sistema de coordenadas de mão esquerda.                                                 |
| [**XMMatrixOrthographicOffCenterLH**](https://msdn.microsoft.com/library/windows/desktop/ee419976) | Cria uma matriz de projeção ortogonal personalizada para um sistema de coordenadas de mão esquerda.                                           |
| [**XMMatrixOrthographicOffCenterRH**](https://msdn.microsoft.com/library/windows/desktop/ee419977) | Cria uma matriz de projeção ortogonal personalizada para um sistema de coordenadas de mão direita.                                          |
| [**XMMatrixOrthographicRH**](https://msdn.microsoft.com/library/windows/desktop/ee419978)                   | Cria uma matriz de projeção ortogonal para um sistema de coordenadas de mão direita.                                                |
| [**XMMatrixPerspectiveFovLH**](https://msdn.microsoft.com/library/windows/desktop/ee419979)               | Cria uma matriz de projeção de perspectiva à esquerda com base em um campo de visão.                                                |
| [**XMMatrixPerspectiveFovRH**](https://msdn.microsoft.com/library/windows/desktop/ee419980)               | Cria uma matriz de projeção de perspectiva à direita com base em um campo de visão.                                               |
| [**XMMatrixPerspectiveLH**](https://msdn.microsoft.com/library/windows/desktop/ee419981)                     | Cria uma matriz de projeção de perspectiva à esquerda.                                                                         |
| [**XMMatrixPerspectiveOffCenterLH**](https://msdn.microsoft.com/library/windows/desktop/ee419982)   | Cria uma versão personalizada de uma matriz de projeção de perspectiva à esquerda.                                                     |
| [**XMMatrixPerspectiveOffCenterRH**](https://msdn.microsoft.com/library/windows/desktop/ee419983)   | Cria uma versão personalizada de uma matriz de projeção de perspectiva à direita.                                                    |
| [**XMMatrixPerspectiveRH**](https://msdn.microsoft.com/library/windows/desktop/ee419984)                     | Cria uma matriz de projeção de perspectiva à direita.                                                                        |

 

## <a name="opengl-es20-to-direct3d-11-porting-frequently-asked-questions"></a>Perguntas frequentes sobre portabilidade do OpenGL ES2.0 para Direct3D 11


-   Pergunta: "De modo geral, posso pesquisar certas cadeias ou padrões em meu código OpenGL e substituí-los por equivalentes em Direct3D?"
-   Resposta: Não. O OpenGL ES 2.0 e o Direct3D 11 vêm de gerações diferentes do modelo de pipeline gráfico. Embora haja algumas semelhanças entre os conceitos e APIs referentes à superfície, como o contexto de renderização e a instanciação de sombreadores, você deve conferir estas instruções e a referência do Direct3D 11 para fazer as melhores escolhas ao recriar o pipeline em vez de tentar um mapeamento de 1 para 1. Porém, se você estiver fazendo a portabilidade do GLSL para o HLSL, a criação de um conjunto de aliases comuns para variáveis, intrínsecos e funções GLSL pode tornar a portabilidade mais fácil e, além disso, permitir que você mantenha apenas um conjunto de arquivos de código do sombreador.

 

 





