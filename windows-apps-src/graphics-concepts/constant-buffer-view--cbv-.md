---
title: "Modo de exibição de buffer constante (CBV)"
description: "Buffers constantes contêm dados constantes de sombreador. O valor deles é que os dados são preservados e podem ser acessados por qualquer sombreador de GPU, até que seja necessário alterar os dados."
ms.assetid: 99AEC6B0-A43B-4B61-8C3A-ECC8DE1B69A7
keywords: "Modo de exibição de buffer constante (CBV)"
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9e26a446bdd1e5a692e826d2c0ba303688bbd3d7
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/22/2017
---
# <a name="constant-buffer-view-cbv"></a>Modo de exibição de buffer constante (CBV)


Buffers constantes contêm dados constantes de sombreador. O valor deles é que os dados são preservados e podem ser acessados por qualquer sombreador de GPU, até que seja necessário alterar os dados.

Dados típicos para um buffer constante seria mundo, projeção e matrizes de modo de exibição, que permanecem constantes durante todo o desenho de um quadro.

O layout de buffer constante deve corresponder ao layout HLSL (consulte [Regras de Empacotamento de Variáveis Constantes](https://msdn.microsoft.com/library/windows/desktop/bb509632.aspx)).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Modos de exibição](views.md)

 

 




