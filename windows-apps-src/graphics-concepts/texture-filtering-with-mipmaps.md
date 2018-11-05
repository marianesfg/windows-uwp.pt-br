---
title: Filtragem de textura com mipmaps
description: Um mipmap é uma sequência de texturas, cada uma delas é uma representação em resolução mais baixa progressivamente da mesma imagem. A altura e a largura de cada imagem ou nível, no mipmap é uma potência de dois menor do que o nível anterior.
ms.assetid: 28E863A2-C776-40E4-8551-9851DF7EC93E
keywords:
- Filtragem de textura com mipmaps
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d32d5a77fe9bc840ea676c7156c1b59e498d07e1
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6051941"
---
# <a name="texture-filtering-with-mipmaps"></a>Filtragem de textura com mipmaps


Um *mipmap* é uma sequência de texturas, cada uma delas é uma representação em resolução mais baixa progressivamente da mesma imagem. A altura e a largura de cada imagem ou nível, no mipmap é uma potência de dois menor do que o nível anterior. Mipmaps não precisam ser quadrados.

Uma imagem de mipmap de alta resolução é usada para objetos que estão próximos ao usuário. Imagens de resolução mais baixa são usadas para o objeto que parece mais distante. O mipmapping melhora a qualidade das texturas renderizadas às custas do uso de mais memória.

O Direct3D representa os mipmaps como uma cadeia de superfícies anexadas. A textura de resolução mais alta fica no topo da cadeia e tem o próximo nível do mipmap como um anexo. Por sua vez, esse nível possui um anexo que é o próximo nível no mipmap e assim por diante, até o nível mais baixo de resolução do mipmap.

As ilustrações a seguir mostram um exemplo desses níveis. As texturas de bitmap representam uma placa em um contêiner em um jogo 3D em primeira pessoa. Quando criada como um mipmap, a textura de resolução mais alta é primeira do conjunto. Cada textura subsequente no conjunto de mipmaps é uma potência de 2 menor em altura e largura. Nesse caso, o mipmap de resolução máxima é de 256 pixels por 256 pixels. A próxima textura é de 128x128. A última textura na cadeia é de 64x64.

Essa placa tem uma distância máxima do qual fica visível. Se o usuário começar distante da placa, o jogo exibirá a textura menor da cadeia de mipmaps, que neste caso é a textura de 64x64.

![ilustração de uma textura de 64x64 de uma placa de perigo](images/mip1.jpg)

Conforme o ponto de vista do usuário se aproxima da placa, as texturas de maior resolução da cadeia de mipmaps são usadas progressivamente. A resolução na ilustração a seguir é de 128x128.

![ilustração de uma textura de 128x128 de uma placa de perigo](images/mip2.jpg)

A textura de resolução mais alta é usada quando o ponto de vista do usuário é a distância mínima permitida de placa, conforme mostrado na ilustração a seguir.

![ilustração de uma textura de 256x256 de uma placa de perigo](images/mip3.jpg)

Essa é uma maneira mais eficiente de simular perspectiva para texturas. Em vez de renderizar uma textura em várias resoluções, é mais rápido usar várias texturas em resoluções variadas.

O Direct3D pode avaliar qual textura de um conjunto de mipmaps tem a resolução mais próxima do resultado desejado e pode mapear pixels para seu espaço de texel. Se a resolução da imagem final é entre as resoluções de texturas no conjunto de mipmaps, o Direct3D pode examinar texels nos dois mipmaps e mesclar seus valores de cor.

Para usar mipmaps, seu aplicativo deve criar um conjunto de mipmaps. Os aplicativos aplicam mipmaps selecionando o conjunto de mipmaps como a primeira textura no conjunto de texturas atuais. Consulte [Mesclagem de texturas](texture-blending.md).

Em seguida, seu aplicativo deve definir o método de filtragem que o Direct3D usa para fazer a amostragem de texels. O método mais rápido de filtragem de mipmaps é fazer com que o Direct3D selecione o texel mais próximo. Use o valor enumerado D3DTEXF\_POINT para selecionar essa opção. O Direct3D poderá produzir resultados de filtragem melhores se seu aplicativo usar o valor enumerado D3DTEXF\_LINEAR. Isso seleciona o mipmap mais próximo e depois calcula uma média ponderada dos texels ao redor do local na textura para a qual o pixel atual é mapeado.

As texturas de mipmap são usadas em cenas 3D para reduzir o tempo necessário para renderizar uma cena. Eles também melhoram o realismo da cena. No entanto, eles geralmente exigem grandes quantidades de memória.

**Observação**  cada superfície em uma cadeia de mipmap tem dimensões são metade que da superfície anterior na cadeia. Se o mipmap de nível superior tiver dimensões de 256x128, as dimensões do mipmap do segundo nível serão de 128 x64, do terceiro nível serão de 64x32 e assim por diante, até 1x1. Você não pode solicitar um número de níveis de mipmap em Níveis que possam fazer com que a largura ou altura de qualquer mipmap da cadeia seja menor que 1. No caso simples de uma superfície de mipmap de nível superior de 4x2, o valor máximo permitido para Níveis é três. As dimensões de nível superior são de 4x2, as dimensões do segundo nível são de 2x1 e as dimensões do terceiro nível são de 1x1. Um valor maior que 3 em Níveis resulta em um valor fracionário na altura do mipmap de segundo nível e, portanto, não permitido.

 

O Direct3D pode executar automaticamente a filtragem de textura de mipmap. Os aplicativos podem percorrer manualmente uma cadeia de mipmaps para carregar dados de bitmap em cada superfície da cadeia. Esse geralmente é o único motivo para percorrer a cadeia. A geração automática de mipmaps no momento da criação da textura tira proveito da filtragem de hardware porque o mipmap reside na memória de vídeo.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Filtragem de textura](texture-filtering.md)

 

 




