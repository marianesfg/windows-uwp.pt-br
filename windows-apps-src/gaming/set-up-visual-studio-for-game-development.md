---
title: Ferramentas do Visual Studio para programação de jogos
description: Uma visão geral das ferramentas específicas do DirectX disponíveis no Visual Studio.
ms.assetid: 43137bfc-7876-70e0-515c-4722f68bd064
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, o visual studio, ferramentas, directx
ms.localizationpriority: medium
ms.openlocfilehash: f4ff471ae4c45761a1c1dad5819e97c8245746a1
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368131"
---
# <a name="visual-studio-tools-for-game-programming"></a>Ferramentas do Visual Studio para programação de jogos



**Resumo**

-   [Criar um projeto de jogo do DirectX de um modelo](user-interface.md)
-   Ferramentas do Visual Studio para programação de jogos em DirectX


Caso você use o Visual Studio Ultimate para desenvolver aplicativos em DirectX, saiba que há ferramentas adicionais disponíveis para criar, editar, visualizar e exportar recursos de imagens, modelos e sombreadores. Há também ferramentas que você pode usar para converter recursos no momento da compilação e depurar o código de elementos gráficos do DirectX.

Este tópico apresenta uma visão geral dessas ferramentas gráficas.

## <a name="image-editor"></a>Editor de imagens


Use o editor de imagens para trabalhar com formatos avançados de textura e imagem usados pelo DirectX. O editor de imagem é compatível com os formatos a seguir.

-   .png
-   .jpg, .jpeg, .jpe, .jfif
-   .dds
-   .gif
-   .bmp
-   .dib
-   .tif, .tiff
-   .tga

Crie [arquivos de compilação personalizada](#build-customizations-for-3d-assets) para convertê-los em arquivos .dds no momento da compilação.

Para obter mais informações, consulte [Trabalhando com texturas e imagens](https://docs.microsoft.com/visualstudio/designers/working-with-textures-and-images?view=vs-2015).

> **Observação**  o Editor de imagens não pretende ser uma substituição para uma aplicativo de edição de imagem completo de recursos, mas é adequado para muitos exibição simples e cenários de edição.

 

## <a name="model-editor"></a>Editor de modelos


Você pode usar o editor de modelos para criar modelos 3D básicos do zero. Use-o também para visualizar e modificar modelos 3D mais complexos de ferramentas completas de modelagem 3D. O editor de modelos oferece suporte a vários formatos de modelo 3D usados no desenvolvimento de aplicativos em DirectX. Você pode criar [arquivos de compilação personalizada](#build-customizations-for-3d-assets) para convertê-los em arquivos .cmo no momento da compilação.

-   .fbx
-   .dae
-   .obj

Consulte uma captura de tela de um modelo no editor com iluminação aplicada.

![bule](images/modeleditor.png)

Para obter mais informações, consulte [Trabalhando com modelos 3D](https://docs.microsoft.com/visualstudio/designers/working-with-3-d-models?view=vs-2015).

> **Observação**  o Editor de modelo não pretende ser uma substituição para um modelo de recurso completo editando o aplicativo, mas é adequado para muitos exibição simples e cenários de edição.

 

## <a name="shader-designer"></a>Designer de sombreador


Use o designer de sombreador para criar efeitos visuais personalizados no jogo ou aplicativo, mesmo sem conhecer programação HLSL.

O sombreador é criado visualmente, como uma imagem. Cada nó contém uma visualização da saída até a operação correspondente. Consulte um exemplo de aplicação de iluminação Lambert na visualização de uma esfera.

![gráfico do sombreador visual](images/shaderdesigner.png)

Use o editor de sombreador para criar, editar e salvar sombreadores no formato .dgsl. Ele também permite exportações para os formatos a seguir.

-   .hlsl (código-fonte)
-   .cso (código de bytes)
-   .h (matriz de códigos de bytes HLSL)

Crie [arquivos de compilação personalizada](#build-customizations-for-3d-assets) para converter qualquer um dos formatos a seguir em arquivos .cso no momento da compilação.

Esta é uma parte do código HLSL exportado pelo editor de sombreador. Este é apenas o código do nó de iluminação Lambert.

```hlsl
//
// Lambert lighting function
//
float3 LambertLighting(
    float3 lightNormal,
    float3 surfaceNormal,
    float3 materialAmbient,
    float3 lightAmbient,
    float3 lightColor,
    float3 pixelColor
    )
{
    // Compute the amount of contribution per light.
    float diffuseAmount = saturate(dot(lightNormal, surfaceNormal));
    float3 diffuse = diffuseAmount * lightColor * pixelColor;

    // Combine ambient with diffuse.
    return saturate((materialAmbient * lightAmbient) + diffuse);
}
```

Para obter mais informações, consulte [Trabalhando com sombreadores](https://docs.microsoft.com/visualstudio/designers/working-with-shaders?view=vs-2015).

## <a name="build-customizations-for-3d-assets"></a>Compilações personalizadas para ativos 3D


Você pode adicionar compilações personalizadas ao seu projeto para que o Visual Studio converta recursos em formatos utilizáveis. Depois disso, você pode carregar os ativos no aplicativo e usá-los ao criar e preencher recursos DirectX, igual faria em qualquer outro aplicativo em DirectX.

Para adicionar uma compilação personalizada, no **Gerenciador de Soluções**, clique com o botão direito no projeto e selecione **Compilações Personalizadas...** . Você pode adicionar os tipos de compilação personalizada a seguir ao seu projeto.

-   O pipeline de conteúdo de imagem usa arquivos de imagem como entrada e produz arquivos de superfície do DirectDraw (.dds).
-   O pipeline de conteúdo de malha usa arquivos de malha (como .fbx) e produz arquivos de malha .cmo.
-   O pipeline de conteúdo de sombreador usa Visual Shader Graph (.dgsl) do editor de sombreador do Visual Studio e produz um arquivo de saída de sombreador compilado (.cso).

Para obter mais informações, consulte o tópico sobre [uso de ativos 3D em jogos ou aplicativos](https://docs.microsoft.com/visualstudio/designers/using-3-d-assets-in-your-game-or-app?view=vs-2015).

## <a name="debugging-directx-graphics"></a>Depurando gráficos em DirectX


O Visual Studio oferece ferramentas de depuração específicas a gráficos. Use-as para depurar elementos como:

-   O pipeline gráfico.
-   A pilha de chamadas a eventos.
-   A tabela de objetos.
-   O estado do dispositivo.
-   Bugs do sombreador.
-   Parâmetros e buffers constantes não inicializados ou incorretos.
-   Compatibilidade entre versões do DirectX.
-   Suporte limitado a Direct2D.
-   Requisitos de sistema operacional e SDK.

Para obter mais informações, consulte o tópico sobre [depuração de elementos gráficos do DirectX](https://docs.microsoft.com/visualstudio/debugger/visual-studio-graphics-diagnostics?view=vs-2015).


 

 

 




