---
author: mtoepke
title: Elementos gráficos 3D básicos para jogos DirectX
description: Mostramos como usar programação em DirectX para implementar os conceitos fundamentais de elementos gráficos 3D.
ms.assetid: 2989c91f-7b45-7377-4e83-9daa0325e92e
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, jogos, directx, elementos gráficos
ms.openlocfilehash: 2ac11ce220bc1c62c81df12fbf9c2a41fda1d940
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.locfileid: "199255"
---
# <a name="basic-3d-graphics-for-directx-games"></a>Elementos gráficos 3D básicos para jogos DirectX


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Mostramos como usar programação em DirectX para implementar os conceitos fundamentais de elementos gráficos 3D.

**Objetivo:** aprender a programar um aplicativo com elementos gráficos 3D.

## <a name="prerequisites"></a>Pré-requisitos


Partimos do princípio de que você conhece C++. Você também precisa ter experiência básica com conceitos de programação de elementos gráficos.

**Tempo total para concluir:** 30 minutos.

## <a name="where-to-go-from-here"></a>Para onde ir a partir daqui


Aqui, falamos sobre como desenvolver elementos gráficos 3D com DirectX e C++\\Cx. Este tutorial em cinco partes introduz a API [Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466) e os conceitos e códigos que também são usados em muitas das outras amostras de DirectX. Essas partes se complementam, desde a configuração do DirectX para seu aplicativo em C++ da UWP até a texturização de primitivos e a adição de efeitos.

> **Observação**  Este tutorial usa um sistema de coordenadas destro com vetores coluna. Muitos exemplos e aplicativos em DirectX usam um sistema de coordenadas à esquerda, com vetores de linha. Para uma solução matemática de elementos gráficos mais completa e que suporte um sistema de coordenadas canhoto com vetores linha, considere o uso de [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833). Para obter mais informações, consulte [Usando DirectXMath com Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff729728#Use_DXMath_with_D3D).

 

Nós lhe mostramos como:

-   Inicializar as interfaces [Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466) usando o Windows Runtime
-   Aplicar operações de sombreador por vértice
-   Configurar a geometria
-   Rasterize a cena (achatando a cena 3D para uma projeção 2D)
-   Selecione as superfícies ocultas

> **Observação**  
Este artigo se destina a desenvolvedores do Windows 10 que escrevem aplicativos UWP (Plataforma Universal do Windows). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

Em seguida, criamos um dispositivo Direct3D, uma cadeia de troca e um modo de exibição de destino de processamento e apresentamos a imagem renderizada para exibição.

[Início rápido: configurando recursos de DirectX e exibindo uma imagem](setting-up-directx-resources.md)

## <a name="related-topics"></a>Tópicos relacionados


* [Elementos gráficos em Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476080)
* [DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534)
* [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561)

 

 




