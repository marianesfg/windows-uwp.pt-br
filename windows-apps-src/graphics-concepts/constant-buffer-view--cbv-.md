---
title: Modo de exibição de buffer constante (CBV)
description: Buffers constantes contêm dados constantes de sombreador. O valor deles é que os dados são preservados e podem ser acessados por qualquer sombreador de GPU, até que seja necessário alterar os dados.
ms.assetid: 99AEC6B0-A43B-4B61-8C3A-ECC8DE1B69A7
keywords:
- Modo de exibição de buffer constante (CBV)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 33e850ba16be7a8d2621f061015d39c8b334cab2
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8692285"
---
# <a name="constant-buffer-view-cbv"></a>Modo de exibição de buffer constante (CBV)


Buffers constantes contêm dados constantes de sombreador. O valor deles é que os dados são preservados e podem ser acessados por qualquer sombreador de GPU, até que seja necessário alterar os dados.

Dados típicos para um buffer constante seria mundo, projeção e matrizes de modo de exibição, que permanecem constantes durante todo o desenho de um quadro.

O layout de buffer constante deve corresponder ao layout HLSL (consulte [Regras de Empacotamento de Variáveis Constantes](https://msdn.microsoft.com/library/windows/desktop/bb509632.aspx)).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Modos de exibição](views.md)

 

 




