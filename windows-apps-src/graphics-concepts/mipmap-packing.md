---
title: "Compactação de mapas MIP"
description: "Um número de mips (por fatia de matriz) pode ser incluído em uma quantidade de blocos, dependendo das dimensões, do formato, do número de mipmaps e das fatias de matriz de um recurso de streaming."
ms.assetid: 906C3CAC-4E84-4947-B508-06788551BE85
keywords: "Compactação de mapas MIP"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 720e8ff5fd6dee2a67bd7a1bf7de02a8f5c34583
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="mipmap-packing"></a>Compactação de mapas MIP


Um número de mips (por fatia de matriz) pode ser incluído em uma quantidade de blocos, dependendo das dimensões, do formato, do número de mipmaps e das fatias de matriz de um recurso de streaming.

Dependendo da [camada](streaming-resources-features-tiers.md) de suporte aos recursos de streaming, os mipmaps com dimensões específicas não seguem as formas de bloco padrão e são considerados como compactados em conjunto de modo opaco para o app. Níveis mais altos de suporte têm garantia mais ampla sobre quais tipos de dimensões de superfície se encaixam nas formas de bloco padrão (e, portanto, podem ser mapeados individualmente por apps).

Devido às dimensões, ao formato, ao número de mipmaps e às fatias de matriz de um recurso de streaming, a variação entre implementações pode ocorrer na quantidade M de mips (por fatia de matriz) incluída em N blocos. Ao obter as informações de agrupamento lado a lado de recursos de um dispositivo, o driver relata ao app o que são M e N (entre outros detalhes padrão sobre a superfície e que não variam de acordo com o fornecedor de hardware). O conjunto de blocos para mips compactados ainda tem 64 KB e pode ser mapeado individualmente em locais diferentes no pool de blocos.

No entanto, a forma de pixel dos blocos e como os mipmaps se encaixam no conjunto de blocos é específico de um fornecedor de hardware e muito complexo para expor. Assim, os apps devem mapear todos os blocos designados como compactados ou nenhum deles simultaneamente. Caso contrário, o comportamento para acessar o recurso de streaming é indefinido.

Para superfícies dispostas, o conjunto de mips compactado e a quantidade de blocos compactados que armazenam esses mips (M e N descrito acima) se aplica individualmente a cada fatia de matriz.

As APIs dedicadas para copiar blocos não podem acessar mips compactados. Os apps que desejam copiar dados de e para mips compactados podem fazer isso usando todas as APIS de recursos específicos sem streaming para copiar e renderizar em superfícies.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Como uma área de recurso de streaming é colocada lado a lado](how-a-streaming-resource-s-area-is-tiled.md)

 

 




