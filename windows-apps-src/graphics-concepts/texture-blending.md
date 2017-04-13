---
title: Mesclagem de texturas
description: "O Direct3D pode mesclar até oito texturas em primitivas em uma única passagem."
ms.assetid: 9AD388FA-B2B9-44A9-B73E-EDBD7357ACFB
keywords: Mesclagem de texturas
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 1fed6a38121402e1aa0273b9186a9eab80fdf4ea
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="texture-blending"></a>Mesclagem de texturas


O Direct3D pode mesclar até oito texturas em primitivas em uma única passagem. O uso da mesclagem de várias texturas pode aumentar profundamente a taxa de quadros de um aplicativo Direct3D. Um aplicativo emprega a mesclagem de várias texturas para aplicar texturas, sombras, iluminação especular, iluminação difusa e outros efeitos especiais em uma única passagem.

Para usar a mesclagem de texturas, seu aplicativo deve verificar primeiro se o hardware do usuário tem suporte para isso.

## <a name="span-idtexture-stages-and-the-texture-blending-cascadespanspan-idtexture-stages-and-the-texture-blending-cascadespanspan-idtexture-stages-and-the-texture-blending-cascadespantexture-stages-and-the-texture-blending-cascade"></a><span id="Texture-Stages-and-the-Texture-Blending-Cascade"></span><span id="texture-stages-and-the-texture-blending-cascade"></span><span id="TEXTURE-STAGES-AND-THE-TEXTURE-BLENDING-CASCADE"></span>Os estágios de textura e a mesclagem de texturas em cascata


O Direct3D dá suporte para a mesclagem de várias texturas em uma única passagem por meio do uso de estágios de textura. Um estágio de textura pega dois argumentos e executa uma operação de mesclagem neles, passando o resultado para processamento adicional ou para rasterização. Você pode visualizar um estágio de textura conforme mostrado no diagrama a seguir.

![diagrama de um estágio de textura](images/texstg.png)

Como mostra o diagrama anterior, os estágios de textura combinam dois argumentos usando um operador especificado. As operações comuns incluem modulação simples ou adição dos componentes de cor ou alfa dos argumentos, mas há suporte para dezenas de operações. Os argumentos para um estágio podem ser uma textura associada, a cor iterada ou o alfa (iterado durante o sombreamento Gouraud), cor ou alfa arbitrário ou o resultado do estágio de textura anterior.

**Observação**   O Direct3D diferencia a mesclagem de cores da mesclagem alfa. Os aplicativos definem as operações e os argumentos de mesclagem de cor e alfa individualmente, e os resultados dessas configurações são independentes uns dos outros.

 

A combinação de argumentos e operações usados por vários estágios da mesclagem definem uma linguagem simples de mesclagem baseada em fluxo. Os resultados de um estágio fluem para outro estágio, e desse estágio para o próximo e assim por diante. O conceito da fluência de resultados de estágio para estágio para, finalmente, serem rasterizados em um polígono geralmente é chamado de mesclagem de texturas em cascata. O diagrama a seguir mostra como estágios de textura individuais compõem a mesclagem de texturas em cascata.

![diagrama dos estágios de textura na mesclagem de texturas em cascata](images/tcascade.png)

Cada estágio em um dispositivo tem um índice baseado em zero. O Direct3D permite até oito estágios de mesclagem, embora você deva sempre verificar as funcionalidades do dispositivo para determinar quantos estágios o hardware atual aceita. O primeiro estágio de mesclagem está no índice 0, o segundo é no 1 e assim por diante, até o índice 7. O sistema mescla os estágios aumentando a ordem de indexação.

Use apenas o número de estágios de que você precisa; os estágios de mesclagem não utilizados são desabilitados por padrão. Portanto, se seu aplicativo usar apenas os dois primeiros estágios, ele só precisará definir as operações e os argumentos para os estágios de 0 e 1. O sistema mescla os dois estágios e ignora os estágios desabilitados.

Se seu aplicativo usa um número de estágios variado para diferentes situações – como quatro estágios para alguns objetos e apenas dois para outros – você não precisa explicitamente desabilitar todos os estágios usados anteriormente. Uma opção é desabilitar a operação de cor para o primeiro estágio não utilizado. Assim, todos os estágios com um índice mais alto não serão aplicados. Outra opção é desabilitar o mapeamento de texturas por completo definindo a operação de cor para o primeiro estágio de textura (estágio 0) para um estado desabilitado.

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
<td align="left"><p>[Estágios de mesclagem](blending-stages.md)</p></td>
<td align="left"><p>Um estágio de mesclagem é um conjunto de operações de textura e seus argumentos que definem como as texturas são mescladas.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Mistura de textura com passagem múltipla](multipass-texture-blending.md)</p></td>
<td align="left"><p>Os aplicativos Direct3D podem conseguir vários efeitos especiais aplicando várias texturas a uma primitiva ao longo de várias passagens de renderização. O termo comum para isso é <em>mesclagem de texturas de passagem múltipla</em>. Um uso comum de mesclagem de texturas de passagem múltipla é emular os efeitos de iluminação complexa e sombrear modelos aplicando várias cores de várias texturas diferentes. Uma dessas aplicações é chamada de <em>mapeamento suave</em>.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Texturas](textures.md)

 

 




