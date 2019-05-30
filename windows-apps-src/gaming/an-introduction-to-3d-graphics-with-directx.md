---
title: Elementos gráficos 3D básicos para jogos DirectX
description: Mostramos como usar programação em DirectX para implementar os conceitos fundamentais de elementos gráficos 3D.
ms.assetid: 2989c91f-7b45-7377-4e83-9daa0325e92e
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, directx, elementos gráficos
ms.localizationpriority: medium
ms.openlocfilehash: 556c5c74e5c8284e56047b4b8a9b2c2bf9c0155c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369070"
---
# <a name="basic-3d-graphics-for-directx-games"></a>Elementos gráficos 3D básicos para jogos DirectX



Mostramos como usar programação em DirectX para implementar os conceitos fundamentais de elementos gráficos 3D.

**Objetivo:** Aprenda a programar um aplicativo de gráficos 3D.

## <a name="prerequisites"></a>Pré-requisitos


Partimos do princípio de que você conhece C++. Você também precisa ter experiência básica com conceitos de programação de elementos gráficos.

**Tempo total para concluir:** 30 minutos.

## <a name="where-to-go-from-here"></a>Para onde ir a partir daqui


Aqui, podemos falar sobre como desenvolver elementos gráficos 3D com o DirectX e C++\\Cx. Este tutorial em cinco partes introduz a API [Direct3D](https://docs.microsoft.com/windows/desktop/direct3d) e os conceitos e códigos que também são usados em muitas das outras amostras de DirectX. Essas partes se complementam, desde a configuração do DirectX para seu aplicativo em C++ da UWP até a texturização de primitivos e a adição de efeitos.

> **Observação**  este tutorial usa um sistema de coordenadas destro com vetores de coluna. Muitos exemplos e aplicativos em DirectX usam um sistema de coordenadas à esquerda, com vetores de linha. Para uma solução matemática de elementos gráficos mais completa e que suporte um sistema de coordenadas canhoto com vetores linha, considere o uso de [DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/directxmath-portal). Para obter mais informações, consulte [Usando DirectXMath com Direct3D](https://docs.microsoft.com/windows/desktop/dxmath/pg-xnamath-migration-d3dx).

 

Nós lhe mostramos como:

-   Inicializar as interfaces [Direct3D](https://docs.microsoft.com/windows/desktop/direct3d) usando o Windows Runtime
-   Aplicar operações de sombreador por vértice
-   Configurar a geometria
-   Rasterize a cena (achatando a cena 3D para uma projeção 2D)
-   Selecione as superfícies ocultas

> **Observação**  

 

Em seguida, criamos um dispositivo Direct3D, uma cadeia de troca e um modo de exibição de destino de processamento e apresentamos a imagem renderizada para exibição.

[Guia de início rápido: configurar recursos do DirectX e exibir uma imagem](setting-up-directx-resources.md)

## <a name="related-topics"></a>Tópicos relacionados


* [Direct3D 11 gráficos](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)
* [DXGI](https://docs.microsoft.com/windows/desktop/direct3ddxgi/dx-graphics-dxgi)
* [HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl)

 

 




