---
title: "Criação de pool de blocos"
description: "Os aplicativos podem criar um ou mais pools de blocos por dispositivo Direct3D. O tamanho total de cada pool de blocos é restrito ao limite de tamanho de recursos do Direct3D 11, que é aproximadamente 1/4 da RAM da GPU (unidade de processamento gráfico)."
ms.assetid: BD51EDD3-4AD3-4733-B014-DD77B9D743BB
keywords: "Criação de pool de blocos"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 315c1b2e1a2b8c89b432a89278ae1b3b240c5ad5
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="tile-pool-creation"></a>Criação de pool de blocos


Os aplicativos podem criar um ou mais pools de blocos por dispositivo Direct3D. O tamanho total de cada pool de blocos é restrito ao limite de tamanho de recursos do Direct3D 11, que é aproximadamente 1/4 da RAM da GPU (unidade de processamento gráfico).

Um pool de blocos é composto por blocos de 64 KB, mas o sistema operacional (driver de vídeo) gerencia o pool inteiro como uma ou mais alocações em segundo plano – a divisão não fica visível para os aplicativos. Os recursos de streaming definem o conteúdo apontando para blocos em um pool de blocos. O desmapeamento de um bloco de um recurso de streaming é feito apontando o bloco para **NULL**. Esses blocos desmapeados têm regras sobre o comportamento de leituras ou gravações; consulte [Controle de risco versus recursos de pool de blocos](hazard-tracking-versus-tile-pool-resources.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Mapeamentos estão em um pool de blocos](mappings-are-into-a-tile-pool.md)

 

 



