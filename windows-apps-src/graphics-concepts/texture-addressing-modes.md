---
title: Modos de endereçamento de textura
description: Seu aplicativo Direct3D pode atribuir coordenadas de textura a qualquer vértice de qualquer primitiva.
ms.assetid: 925E8F2E-43EC-404E-8870-03E39155F697
keywords:
- Modos de endereçamento de textura
- Modo de endereçamento de textura Wrap
- Modo de endereçamento de textura Mirror
- Modo de endereçamento de textura Clamp
- Modo de endereçamento de textura Border Color
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0e817dcc92741ca2e738784f387cfe49399a108c
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6979468"
---
# <a name="texture-addressing-modes"></a>Modos de endereçamento de textura


Seu aplicativo Direct3D pode atribuir coordenadas de textura a qualquer vértice de qualquer primitiva. Normalmente, as coordenadas de textura u e v que você atribui a um vértice estão no intervalo de 0.0 a 1.0. No entanto, atribuindo coordenadas de textura fora desse intervalo, você pode criar certos efeitos especiais de texturização. .

Você controla o que o Direct3D faz com as coordenadas de textura que estão fora do intervalo \[0.0, 1.0\] definindo o modo de endereçamento de textura. Por exemplo, você pode fazer com que seu aplicativo defina o modo de endereçamento de textura de forma que uma textura seja colocada lado a lado em uma primitiva.

O Direct3D permite que os aplicativos executem o encapsulamento de textura. Consulte [Encapsulamento de textura](texture-wrapping.md).

Habilitar o encapsulamento de textura torna efetivamente as coordenadas de textura fora do intervalo \[0.0, 1.0\] inválidas, e o comportamento para rasterizar essas coordenadas de textura negligentes é indefinido nesse caso. Quando o encapsulamento de textura está habilitado, os modos de endereçamento de textura não são usados. Tome cuidado para seu aplicativo não especificar coordenadas de textura menores que 0.0 ou maiores que 1.0 quando o encapsulamento de textura estiver habilitado.

## <a name="span-idsummaryofthetextureaddressingmodesspanspan-idsummaryofthetextureaddressingmodesspanspan-idsummaryofthetextureaddressingmodesspansummary-of-the-texture-addressing-modes"></a><span id="Summary_of_the_texture_addressing_modes"></span><span id="summary_of_the_texture_addressing_modes"></span><span id="SUMMARY_OF_THE_TEXTURE_ADDRESSING_MODES"></span>Resumo dos modos de endereçamento de textura


| Modo de endereçamento de textura | Descrição                                                                                                                           |
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| Wrap                    | Repete a textura em cada junção de inteiros.                                                                                        |
| Mirror                  | Espelha a textura em cada limite de inteiro.                                                                                        |
| Clamp                   | Ancora seu coordenadas de textura ao intervalo \[0.0, 1.0\] variar; o modo Clamp aplica a textura uma vez e depois mancha a cor dos pixels da borda. |
| Border Color            | Usa uma *cor de borda* arbitrária para qualquer coordenada de textura fora do intervalo de 0.0 e 1.0.                         |

 

## <a name="span-idwraptextureaddressmodespanspan-idwraptextureaddressmodespanspan-idwraptextureaddressmodespanwrap-texture-address-mode"></a><span id="Wrap_texture_address_mode"></span><span id="wrap_texture_address_mode"></span><span id="WRAP_TEXTURE_ADDRESS_MODE"></span>Modo de endereçamento de textura Wrap


O modo de endereçamento de textura Wrap faz o Direct3D repetir a textura em cada junção de inteiros.

Suponhamos que, por exemplo, seu aplicativo crie uma primitiva quadrada e especifique as coordenadas de textura de (0.0,0.0), (0.0,3.0), (3.0,3.0) e (3.0,0.0). Definir o modo de endereçamento de textura como "Wrap" resulta na aplicação da textura três vezes nas direções u e v, conforme mostrado na ilustração a seguir.

![ilustração de uma textura de face encapsulada na direção u e na direção v](images/wrap.png)

Compare isso com o **modo de endereçamento de textura Mirror** a seguir.

## <a name="span-idmirrortextureaddressmodespanspan-idmirrortextureaddressmodespanspan-idmirrortextureaddressmodespanmirror-texture-address-mode"></a><span id="Mirror_texture_address_mode"></span><span id="mirror_texture_address_mode"></span><span id="MIRROR_TEXTURE_ADDRESS_MODE"></span>Modo de endereçamento de textura Mirror


O modo de endereço de textura Mirror faz com que o Direct3D espelhe a textura em cada limite de inteiro.

Suponhamos que, por exemplo, seu aplicativo crie uma primitiva quadrada e especifique as coordenadas de textura de (0.0,0.0), (0.0,3.0), (3.0,3.0) e (3.0,0.0). Definir o modo de endereçamento de textura como "Mirror" faz com que a textura seja aplicada três vezes nas direções u e v. As linhas e colunas alternadas em que ele é aplicado são uma imagem espelhada da linha ou coluna anterior, conforme mostrado na ilustração a seguir.

![ilustração de imagens espelhadas em uma grade 3x3](images/mirror.png)

Compare isso com o **modo de endereçamento de textura Wrap** anterior.

## <a name="span-idclamptextureaddressmodespanspan-idclamptextureaddressmodespanspan-idclamptextureaddressmodespanclamp-texture-address-mode"></a><span id="Clamp_texture_address_mode"></span><span id="clamp_texture_address_mode"></span><span id="CLAMP_TEXTURE_ADDRESS_MODE"></span>Modo de endereçamento de textura Clamp


O modo de endereçamento de textura Clamp faz com que o Direct3D ancore as coordenadas de textura ao intervalo \[0.0, 1.0\]; o modo Clamp aplica a textura uma vez e depois mancha a cor dos pixels da borda.

Suponhamos que seu aplicativo crie uma primitiva quadrada e atribua coordenadas de textura de (0.0,0.0), (0.0,3.0), (3.0,3.0) e (3.0,0.0) aos vértices da primitiva. Definir o modo de endereçamento de textura como "Clamp" faz com que a textura seja aplicada uma vez. As cores dos pixels na parte superior das colunas e no final das linhas são estendidas até a parte superior e à direita da primitiva respectivamente.

A ilustração a seguir mostra uma textura ancorada.

![ilustração de uma textura e uma textura ancorada](images/clamp.png)

## <a name="span-idbordercolortextureaddressmodespanspan-idbordercolortextureaddressmodespanspan-idbordercolortextureaddressmodespanborder-color-texture-address-mode"></a><span id="Border_Color_texture_address_mode"></span><span id="border_color_texture_address_mode"></span><span id="BORDER_COLOR_TEXTURE_ADDRESS_MODE"></span>Modo de endereçamento de textura Border Color


O modo de endereçamento de textura Border Color faz com que o Direct3D use uma cor arbitrária, conhecida como a *cor da borda*, para qualquer coordenada de textura fora do intervalo de 0.0 a 1.0.

Na ilustração a seguir, o aplicativo especifica que a textura deve ser aplicada à primitiva usando uma borda vermelha.

![ilustração de uma textura e uma textura com uma borda vermelha](images/border.png)

## <a name="span-iddevicelimitationsspanspan-iddevicelimitationsspanspan-iddevicelimitationsspandevice-limitations"></a><span id="Device_Limitations"></span><span id="device_limitations"></span><span id="DEVICE_LIMITATIONS"></span>Limitações do dispositivo


Embora o sistema geralmente permita coordenadas de textura fora do intervalo de 0.0 e 1.0, as limitações do hardware geralmente ditam até que ponto as coordenadas de textura podem se afastar desse intervalo. O dispositivo de renderização comunica o limite para o intervalo completo de coordenadas de textura permitido pelo dispositivo quando você recupera as funcionalidades do dispositivo.

Por exemplo, se esse valor for 128, as coordenadas de textura de entrada deverão ser mantidas no intervalo de -128.0 a +128.0. Passar vértices com coordenadas de textura fora desse intervalo será inválido. A mesma restrição se aplica às coordenadas de textura geradas como resultado da geração automática de coordenadas de textura e das transformações de coordenadas de textura.

As limitações de repetição de textura podem depender do tamanho da textura indexada pelas coordenadas de textura. Nesse caso, supondo que a dimensão da textura seja 32 e o intervalo de coordenadas de textura permitido pelo dispositivo seja 512, o intervalo de coordenadas de textura válido real seria, portanto, 512/32 = 16. Então, as coordenadas de textura para esse dispositivo devem estar dentro do intervalo de -16.0 a +16.0.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Texturas](textures.md)

 

 




