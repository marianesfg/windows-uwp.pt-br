---
author: mtoepke
title: Elementos gráficos 3D básicos para jogos DirectX
description: Mostramos como usar programação em DirectX para implementar os conceitos fundamentais de elementos gráficos 3D.
ms.assetid: 2989c91f-7b45-7377-4e83-9daa0325e92e
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, directx, elementos gráficos
ms.localizationpriority: medium
ms.openlocfilehash: e9834a83620343f26acaabd0e05b30cc2c1dcfab
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5592822"
---
# <a name="basic-3d-graphics-for-directx-games"></a>Elementos gráficos 3D básicos para jogos DirectX



Mostramos como usar programação em DirectX para implementar os conceitos fundamentais de elementos gráficos 3D.

**Objetivo:** aprender a programar um aplicativo com elementos gráficos 3D.

## <a name="prerequisites"></a>Pré-requisitos


Partimos do princípio de que você conhece C++. Você também precisa ter experiência básica com conceitos de programação de elementos gráficos.

**Tempo total para concluir:** 30 minutos.

## <a name="where-to-go-from-here"></a>Para onde ir a partir daqui


Aqui, falamos sobre como desenvolver elementos gráficos 3D com DirectX e C++\\Cx. Este tutorial em cinco partes introduz a API [Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466) e os conceitos e códigos que também são usados em muitas das outras amostras de DirectX. Essas partes se complementam, desde a configuração do DirectX para seu aplicativo em C++ da UWP até a texturização de primitivos e a adição de efeitos.

> **Observação**este tutorial usa um sistema de coordenadas destro com vetores Coluna. Muitos exemplos e aplicativos em DirectX usam um sistema de coordenadas à esquerda, com vetores de linha. Para uma solução matemática de elementos gráficos mais completa e que suporte um sistema de coordenadas canhoto com vetores linha, considere o uso de [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833). Para obter mais informações, consulte [Usando DirectXMath com Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff729728#Use_DXMath_with_D3D).

 

Nós lhe mostramos como:

-   Inicializar as interfaces [Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466) usando o Windows Runtime
-   Aplicar operações de sombreador por vértice
-   Configurar a geometria
-   Rasterize a cena (achatando a cena 3D para uma projeção 2D)
-   Selecione as superfícies ocultas

> **Observação**  

 

Em seguida, criamos um dispositivo Direct3D, uma cadeia de troca e um modo de exibição de destino de processamento e apresentamos a imagem renderizada para exibição.

[Início rápido: configurando recursos de DirectX e exibindo uma imagem](setting-up-directx-resources.md)

## <a name="related-topics"></a>Tópicos relacionados


* [Elementos gráficos em Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476080)
* [DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534)
* [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561)

 

 




