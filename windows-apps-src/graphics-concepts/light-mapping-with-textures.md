---
title: Mapeamento de luzes com texturas
description: "Um mapa de luz é uma textura ou um grupo de texturas que contém informações sobre a iluminação em uma cena 3D."
ms.assetid: 5C7518D2-AC92-4A97-B7AF-4469D213D7BD
keywords:
- Mapeamento de luzes com texturas
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 1154be19719cc3101c114c0bd700fb735824bcda
ms.lasthandoff: 02/07/2017

---

# <a name="light-mapping-with-textures"></a>Mapeamento de luzes com texturas


Um mapa de luz é uma textura ou um grupo de texturas que contém informações sobre a iluminação em uma cena 3D. Os mapas de luz mapeiam áreas de luz e sombra em primitivos. A passagem múltipla e a mesclagem de textura múltipla permitem que o app renderize cenas com uma aparência mais realista em comparação às técnicas de sombreamento.

Para um app renderizar uma cena 3D de forma realista, ele deve considerar o efeito que as fontes de luz tem sobre a aparência da cena. Embora as técnicas como sombreamento simples e de Gouraud sejam ferramentas valiosas nesse sentido, elas podem ser insuficientes para suas necessidades. O Direct3D oferece suporte à passagem múltipla e à mesclagem de textura múltipla. Esses recursos permitem que o app renderize cenas com uma aparência mais realista do que as cenas renderizadas somente com técnicas de sombreamento. Ao aplicar um ou mais mapas de luz, o app pode mapear áreas de luz e sombra nos primitivos.

Um mapa de luz é uma textura ou um grupo de texturas que contém informações sobre a iluminação em uma cena 3D. Você pode armazenar as informações de iluminação nos valores de alfa do mapa de luz, nos valores de cor ou em ambos.

Se você implementar o mapeamento de luz usando a mesclagem de textura de passagem múltipla, o app deve renderizar o mapa de luz nos primitivos na primeira passagem. Ele deve usar uma segunda passada para renderizar a textura base. A exceção é o mapeamento de luz especular. Nesse caso, renderize a textura base primeiro; em seguida, adicione o mapa de luz.

A mistura de textura múltipla permite que o app renderize o mapa de luz e a textura base em uma passagem. Se o hardware do usuário fornece mistura de textura múltipla, o app deve aproveitar isso ao executar o mapeamento de luz. Isso melhora significativamente o desempenho do app.

Com mapas de luz, um app Direct3D pode conseguir uma variedade de efeitos de iluminação ao renderizar primitivos. Ele pode mapear não apenas as luzes monocromáticas e coloridas em uma cena, mas também pode adicionar detalhes como realces especulares e iluminação difusa.

As informações sobre o uso da mistura de textura do Direct3D para executar o mapeamento de luz são apresentadas nos tópicos a seguir.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>Nesta seção


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tópico</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Mapas de luzes monocromáticas](monochrome-light-maps.md)</p></td>
<td align="left"><p>O mapeamento de luzes monocromáticas permite que os adaptadores mais antigos misturem a textura de passagem múltipla, quando uma placa de aceleradora 3D mais antiga não oferece suporte à mesclagem de textura com o valor de alfa do pixel de destino.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Mapas de luzes coloridas](color-light-maps.md)</p></td>
<td align="left"><p>Um mapa de luz colorido usa os dados RGB no mapa de luz para obter as informações de iluminação. Um app normalmente renderiza cenas 3D de forma mais realista ao usar mapas de luz coloridos.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Mapas de luzes especulares](specular-light-maps.md)</p></td>
<td align="left"><p>Quando iluminados por uma fonte de luz, os objetos brilhantes que usam materiais altamente refletores recebem realces especulares. Às vezes, você pode obter realces mais precisos aplicando mapas de luz especulares aos primitivos, em vez de usar os realces especulares produzido pelo módulo de iluminação.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Mapas de luzes difusas](diffuse-light-maps.md)</p></td>
<td align="left"><p>As superfícies foscas têm reflexão de luz difusa. O brilho da luz difusa depende da distância da fonte de luz e do ângulo entre a superfície normal e o vetor de direção da fonte de luz. Os mapas de luz de textura podem simular iluminação difusa complexa.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Texturas](textures.md)

 

 





