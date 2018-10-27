---
title: Modo de exibição de buffer constante (CBV)
description: Buffers constantes contêm dados constantes de sombreador. O valor deles é que os dados são preservados e podem ser acessados por qualquer sombreador de GPU, até que seja necessário alterar os dados.
ms.assetid: 99AEC6B0-A43B-4B61-8C3A-ECC8DE1B69A7
keywords:
- Modo de exibição de buffer constante (CBV)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cca90c705c7bb4dd1c7e283a9c3ed267cd282b56
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5705342"
---
# <a name="constant-buffer-view-cbv"></a>Modo de exibição de buffer constante (CBV)


Buffers constantes contêm dados constantes de sombreador. O valor deles é que os dados são preservados e podem ser acessados por qualquer sombreador de GPU, até que seja necessário alterar os dados.

Dados típicos para um buffer constante seria mundo, projeção e matrizes de modo de exibição, que permanecem constantes durante todo o desenho de um quadro.

O layout de buffer constante deve corresponder ao layout HLSL (consulte [Regras de Empacotamento de Variáveis Constantes](https://msdn.microsoft.com/library/windows/desktop/bb509632.aspx)).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Modos de exibição](views.md)

 

 




