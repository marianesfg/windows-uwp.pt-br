---
title: Ferramentas do Visual Studio para programação de jogos
description: Uma visão geral de ferramentas específicas do DirectX disponíveis no Visual Studio.
ms.assetid: 43137bfc-7876-70e0-515c-4722f68bd064
---

# Ferramentas do Visual Studio para programação de jogos


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos do Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**Resumo**

-   [Criar um projeto de jogo em DirectX usando um modelo](user-interface.md)
-   Ferramentas do Visual Studio para programação de jogos em DirectX


Caso você use o Visual Studio Ultimate para desenvolver aplicativos em DirectX, saiba que há ferramentas adicionais disponíveis para criar, editar, visualizar e exportar recursos de imagens, modelos e sombreadores. Há também ferramentas que você pode usar para converter recursos no momento da compilação e depurar o código de elementos gráficos do DirectX.

Este tópico apresenta uma visão geral dessas ferramentas gráficas.

## Editor de imagens


Use o editor de imagens para trabalhar com formatos avançados de textura e imagem usados pelo DirectX. O editor de imagem é compatível com os formatos a seguir.

-   .png
-   .jpg, .jpeg, .jpe, .jfif
-   .dds
-   .gif
-   .bmp
-   .dib
-   .tif, .tiff
-   .tga

Crie [arquivos de compilação personalizada](#custom) para convertê-los em arquivos .dds no momento da compilação.

Para obter mais informações, consulte [Trabalhando com texturas e imagens](https://msdn.microsoft.com/library/windows/apps/hh873119.aspx).

> **Observação**  Não use o editor de imagens como substituto de um aplicativo completo de edição de imagens, embora ele possa ser usado em muitos cenários simples de exibição e edição.

 

## Editor de modelos


Você pode usar o editor de modelos para criar modelos 3D básicos do zero. Use-o também para visualizar e modificar modelos 3D mais complexos de ferramentas completas de modelagem 3D. O editor de modelos oferece suporte a vários formatos de modelo 3D usados no desenvolvimento de aplicativos em DirectX. Você pode criar [arquivos de compilação personalizada](#custom) para convertê-los em arquivos .cmo no momento da compilação.

-   .fbx
-   .dae
-   .obj

Consulte uma captura de tela de um modelo no editor com iluminação aplicada.

![bule](images/modeleditor.png)

Para obter mais informações, consulte [Trabalhando com modelos 3D](https://msdn.microsoft.com/library/windows/apps/hh873114.aspx).

> **Observação**  Não use o editor de modelos como substituto de um aplicativo completo de edição de modelos, embora ele possa ser usado em muitos cenários simples de exibição e edição.

 

## Designer de sombreador


Use o designer de sombreador para criar efeitos visuais personalizados no jogo ou aplicativo, mesmo sem conhecer programação HLSL.

O sombreador é criado visualmente, como uma imagem. Cada nó contém uma visualização da saída até a operação correspondente. Consulte um exemplo de aplicação de iluminação Lambert na visualização de uma esfera.

![gráfico do sombreador visual](images/shaderdesigner.png)

Use o editor de sombreador para criar, editar e salvar sombreadores no formato .dgsl. Ele também permite exportações para os formatos a seguir.

-   .hlsl (código-fonte)
-   .cso (código de bytes)
-   .h (matriz de códigos de bytes HLSL)

Crie [arquivos de compilação personalizada](#custom) para converter qualquer um dos formatos a seguir em arquivos .cso no momento da compilação.

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

Para obter mais informações, consulte [Trabalhando com sombreadores](https://msdn.microsoft.com/library/windows/apps/hh873117.aspx).

## Compilações personalizadas para ativos 3D


Você pode adicionar compilações personalizadas ao seu projeto para que o Visual Studio converta recursos em formatos utilizáveis. Depois disso, você pode carregar os ativos no aplicativo e usá-los ao criar e preencher recursos DirectX, igual faria em qualquer outro aplicativo em DirectX.

Para adicionar uma compilação personalizada, no **Gerenciador de Soluções**, clique com o botão direito no projeto e selecione **Compilações Personalizadas...**. Você pode adicionar os tipos de compilação personalizada a seguir ao seu projeto.

-   O pipeline de conteúdo de imagem usa arquivos de imagem como entrada e produz arquivos de superfície do DirectDraw (.dds).
-   O pipeline de conteúdo de malha usa arquivos de malha (como .fbx) e produz arquivos de malha .cmo.
-   O pipeline de conteúdo de sombreador usa Visual Shader Graph (.dgsl) do editor de sombreador do Visual Studio e produz um arquivo de saída de sombreador compilado (.cso).

Para obter mais informações, consulte o tópico sobre [uso de ativos 3D em jogos ou aplicativos](https://msdn.microsoft.com/library/windows/apps/hh972446.aspx).

## Depurando gráficos em DirectX


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

Para obter mais informações, consulte o tópico sobre [depuração de elementos gráficos do DirectX](https://msdn.microsoft.com/library/windows/apps/hh315751.aspx).

> **Observação**  Este artigo se destina a desenvolvedores do Windows 10 que elaboram aplicativos UWP (Plataforma Universal do Windows). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

 

 






<!--HONumber=Mar16_HO1-->


