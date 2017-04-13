---
title: "Exibições de textura"
description: "No Direct3D, os recursos de textura são acessados com uma exibição, que é um mecanismo para interpretação de um recurso na memória pelo hardware."
ms.assetid: 18DABFCE-8A36-4C4E-B08E-10428B05D701
keywords: "Exibições de textura"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 70b7b5da92f5be038fd1eb16ca27875704410449
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="texture-views"></a>Exibições de textura


No Direct3D, os recursos de textura são acessados com uma exibição, que é um mecanismo para interpretação de um recurso na memória pelo hardware. Uma exibição permite que um estágio de pipeline específico acesse somente os [sub-recursos](resource-types.md) de que precisa, na representação desejado pelo aplicativo.

Uma exibição dá suporte à noção de um recurso sem especificação de tipo. Um recurso sem especificação de tipo é um recurso criado com um tamanho específico, mas não é um tipo de dados específico. Os dados são interpretados dinamicamente quando são associados ao pipeline.

A ilustração a seguir mostra um exemplo de associação de uma matriz de texturas 2D a 6 texturas como um recurso sombreador, criando uma exibição de recurso sombreador para isso. Em seguida, o recurso é tratado como uma matriz de texturas. (Observação: um sub-recurso não pode ser associado como entrada e saída ao pipeline simultaneamente.)

![ilustração de uma matriz com seis texturas](images/d3d10-cube-texture-faces.png)

Ao usar uma matriz de texturas 2D como destino de renderização, o recurso pode ser exibido como uma matriz de texturas 2D (6 neste exemplo) com níveis de mipmap (3 neste exemplo).

Crie um objeto de exibição para um destino de renderização chamando CreateRenderTargetView. Em seguida, chame OMSetRenderTargets para definir a exibição do destino de renderização para o pipeline. Renderize os destinos de renderização chamando Draw e usando RenderTargetArrayIndex para indexar à textura adequada na matriz. Você pode usar um sub-recurso (uma combinação de nível de mipmap e índice de matriz) para associar a qualquer matriz de sub-recursos. Você pode associar o segundo nível de mipmap e só atualizar esse nível de mipmap específico se quiser, como na ilustração a seguir.

![ilustração de associação somente ao segundo nível de mipmap de uma matriz de texturas](images/d3d10-cube-texture-faces-subresource.png)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Recursos](resources.md)

 

 




