---
title: Disposição da textura
description: O encapsulamento de textura muda a maneira básica com que o Direct3D rasteriza polígonos texturizados usando as coordenadas de textura especificadas para cada vértice.
ms.assetid: C28FB369-9A91-4D57-A96D-4A5D36484B35
keywords:
- Disposição da textura
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6618b7573be7cd39f703299b9418d1575297120e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622621"
---
# <a name="texture-wrapping"></a>Disposição da textura


O encapsulamento de textura muda a maneira básica com que o Direct3D rasteriza polígonos texturizados usando as coordenadas de textura especificadas para cada vértice. Durante a rasterização de um polígono, o sistema faz a interpolação entre as coordenadas de textura em cada um dos vértices do polígono para determinar os texels que devem ser usados para cada pixel do polígono. Normalmente, o sistema trata a textura como um plano 2D, interpolando novos texels pegando a rota mais curta do ponto A de uma textura ao ponto B. Se o ponto A representar a posição u, v (0.8, 0.1) e o ponto B estiver em (0.1,0.1), a linha de interpolação terá a aparência do diagrama a seguir.

![diagrama de uma linha de interpolação entre dois pontos](images/interp1.png)

Observe que a distância mais curta entre A e B nesta ilustração passa basicamente pelo meio da textura. Habilitar o encapsulamento das coordenadas de textura u ou textura v muda a forma como o Direct3D identifica a rota mais curta entre as coordenadas de textura na direção u e na direção v. Por definição, o encapsulamento de textura faz com que o rasterizador pegue a rota mais curta entre os conjuntos de coordenadas de textura, presumindo que 0.0 e 1.0 sejam coincidentes. A última é a parte complicada: Você pode imaginar a textura Habilitar quebra automática em uma direção faz com que o sistema tratar uma textura, como se ela foi encapsulada ao redor de um cilindro. Por exemplo, considere o diagrama a seguir.

![diagrama de uma textura e dois pontos encapsulados em um cilindro](images/interp2.png)

A ilustração anterior mostra como o encapsulamento na direção u afeta a forma como o sistema interpola as coordenadas de textura. Usando os mesmos pontos do exemplo para texturas normais, ou não encapsuladas, você pode ver que a rota mais curta entre os pontos A e B não está mais no meio da textura; agora está na borda em que 0.0 e 1.0 coexistem. O encapsulamento na direção v é similar, exceto que ele encapsula a textura em torno de um cilindro disposto de lado. O encapsulamento na direção u e na direção v é mais complexo. Nessa situação, você pode imaginar a textura como uma tora ou rosca.

A aplicação prática mais comum do encapsulamento de textura é executar o mapeamento do ambiente. Geralmente, um objeto texturizado com um mapa do ambiente parece muito refletivo, mostrando uma imagem espelhada dos arredores do objeto na cena. Para o âmbito desta discussão, imagine uma sala com quatro paredes, cada uma delas pintada com uma letra R, G, B, Y e as cores correspondentes: vermelho, verde, azul e amarelo. O mapa do ambiente para essa sala simples pode se parecer com a ilustração a seguir.

![ilustração de faixas verticais de vermelho, verde, azul e amarelo](images/envmap.png)

Imagine que o teto da sala seja sustentado por um pilar quadrilátero perfeitamente refletivo. Mapear a textura do mapa do ambiente para o pilar é simples; fazer o pilar parecer que está refletindo as letras e as cores não é tão fácil. O diagrama a seguir mostra um esboço do pilar com as coordenadas de textura aplicáveis listadas próximo aos vértices superiores. A fenda onde o encapsulamento cruzará as bordas da textura é mostrada com uma linha pontilhada.

![diagrama de um retângulo com uma linha pontilhada bifurcada](images/seam.png)

Com o encapsulado habilitado na direção u, o pilar texturizado mostra as cores e os símbolos do mapa do ambiente adequadamente e, na fenda na frente da textura, o rasterizador escolhe corretamente a rota mais curta entre as coordenadas de textura, presumindo que as coordenadas u 0.0 e 1.0 compartilhem o mesmo local. O pilar texturizado se parece com a ilustração a seguir.

![ilustração de um pilar que consiste em quadrantes nas cores vermelho, verde, azul e amarelo](images/tex-seam.png)

Se o encapsulamento de textura não estiver habilitado, o rasterizador não fará a interpolação na direção necessária para gerar uma imagem refletida verossímil. Em vez disso, a área na frente do pilar contém uma versão compactada horizontalmente dos texels entre as coordenadas u 0.175 e 0.875, pois passam pelo centro da textura. O efeito de encapsulamento está arruinado.

Não confunda encapsulamento de textura com os modos de endereçamento de textura de nomes similares. O encapsulamento de textura é executado antes do endereçamento de textura. Certifique-se de que a textura quebra automática de dados não contém qualquer coordenadas de textura fora do intervalo dos \[0,0, 1,0\] porque isso produzirá resultados indefinidos. Para obter mais informações sobre o endereçamento de textura, consulte [Modos de endereçamento de textura](texture-addressing-modes.md).

## <a name="span-iddisplacementmapwrappingspanspan-iddisplacementmapwrappingspanspan-iddisplacementmapwrappingspandisplacement-map-wrapping"></a><span id="Displacement_Map_Wrapping"></span><span id="displacement_map_wrapping"></span><span id="DISPLACEMENT_MAP_WRAPPING"></span>Mapa de mesclagem de encapsulamento


Os mapas de deslocamento são interpolados pelo mecanismo de mosaico. Como o modo de encapsulamento não pode ser especificado para o mecanismo de mosaico, o encapsulamento de textura não pode ser executado com mapas de deslocamento. Um aplicativo é capaz de usar um conjunto de vértices que força a interpolação para encapsular em qualquer direção. O aplicativo também pode especificar a interpolação para ser feita como uma simples interpolação linear.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Texturas](textures.md)

 

 




