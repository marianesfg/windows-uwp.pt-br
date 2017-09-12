---
author: normesta
description: As entradas e as propriedades de uma folha de estilos de mapa
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "Referência da folha de estilos de mapa"
ms.author: normesta
ms.date: 03/16/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, mapas, folha de estilos de mapa
ms.openlocfilehash: 9e2605917027d7be96ac86421e83082aa5f368f9
ms.sourcegitcommit: 23cda44f10059bcaef38ae73fd1d7c8b8330c95e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/19/2017
---
# <a name="map-style-sheet-reference"></a>Referência da folha de estilos de mapa

Você pode criar folhas de estilo de mapa usando JSON (JavaScript Object Notation).

Por exemplo, você usaria o JSON seguinte para exibir áreas aquáticas em vermelho, rótulos em verde e áreas terrestres em azul:

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000", "labelColor":"#00FF00"}}
    }
```
Você também poderia usar o JSON para remover todos os rótulos e pontos de um mapa.

```json

    {"version":"1.*", "elements":{"mapElement":{"labelVisible":false},"point":{"visible":false}}}
```

Às vezes, o valor de uma propriedade é transformado para produzir o resultado final.  Por exemplo, fillColor de vegetação tem sombreamentos ligeiramente diferentes, dependendo do tipo de entidade exibido.  Esse comportamento pode ser desativado por meio do uso do valor preciso fornecido, usando a propriedade ignoreTransform.

```json
    {"version":"1.*",
        "settings":{"shadedReliefVisible":false},
        "elements":{"vegetation":{"fillColor":{"value":"#999999","ignoreTransform":true}}}
    }
```

Este tópico mostra as entradas JSON e as [propriedades](#properties) que você pode usar para personalizar a aparência dos seus mapas.

<span id="entries" />
## <a name="entries"></a>Entradas
Esta tabela usa os caracteres ">" para representar os níveis na hierarquia da entrada.   

| Nome                         | Grupo de propriedades            | Descrição    |
|------------------------------|---------------------------|----------------|
| version                      | [Version](#version)       | A versão de folha de estilos que você deseja usar. |
| settings                     | [Settings](#settings)     | As configurações que se aplicam à folha de estilos inteira. |
| mapElement                   | [MapElement](#mapelement) | A entrada pai para todas as entradas do mapa. |
| > baseMapElement             | [MapElement](#mapelement) | A entrada pai para todas as entradas que não foram feitas por usuário. |
| >> area                      | [MapElement](#mapelement) | Áreas de uso de terra (não deve ser confundido com a entrada de estrutura). |
| >>> airport                  | [MapElement](#mapelement) | Áreas que abrangem um aeroporto. |
| >>> cemetery                 | [MapElement](#mapelement) | Áreas de cemitérios. |
| >>> continent                | [MapElement](#mapelement) | Áreas de continentes inteiros. |
| >>> education                | [MapElement](#mapelement) |  |
| >>> indigenousPeoplesReserve | [MapElement](#mapelement) |  |
| >>> island                   | [MapElement](#mapelement) | Rótulos em áreas insulares. |
| >>> medical                  | [MapElement](#mapelement) | Áreas de terra usadas para fins médicos (por exemplo: um campus de hospital). |
| >>> military                 | [MapElement](#mapelement) | Áreas de bases militares. |
| >>> nautical                 | [MapElement](#mapelement) | Áreas de terra que são usadas para fins náuticos. |
| >>> neighborhood             | [MapElement](#mapelement) | Rótulos em áreas definidas como vizinhanças. |
| >>> runway                   | [MapElement](#mapelement) | Áreas de terra que são cobertas por uma pista. |
| >>> sand                     | [MapElement](#mapelement) | Áreas arenosas, como praias. |
| >>> shoppingCenter           | [MapElement](#mapelement) | Áreas de chão alocadas para shopping centers ou outros centros comerciais. |
| >>> stadium                  | [MapElement](#mapelement) | Área de um estádio. |
| >>> vegetation               | [MapElement](#mapelement) | Florestas, áreas gramadas etc. |
| >>>> forest                  | [MapElement](#mapelement) | Áreas de floresta. |
| >>>> golfCourse              | [MapElement](#mapelement) |  |
| >>>> park                    | [MapElement](#mapelement) | Áreas de parque. |
| >>>> playingField            | [MapElement](#mapelement) | Campos extraídos, como um campo de baseball ou uma quadra de tênis. |
| >>>> reserve                 | [MapElement](#mapelement) | Áreas de reservas naturais. |
| >> point                     | [PointStyle](#pointstyle) | Todos os recursos de ponto renderizados com um ícone de algum tipo. |
| >>> naturalPoint             | [PointStyle](#pointstyle) |  |
| >>>> peak                    | [PointStyle](#pointstyle) | Ícones que representam picos de montanha. |
| >>>>> volcanicPeak           | [PointStyle](#pointstyle) | Ícones que representam picos de vulcão. |
| >>>> waterPoint              | [PointStyle](#pointstyle) | Ícones que representam os locais de recursos hídricos, como uma cachoeira. |
| >>> pointOfInterest          | [PointStyle](#pointstyle) | Restaurantes, hospitais, escolas, marinas, áreas de esqui etc. |
| >>>> business                | [PointStyle](#pointstyle) | Restaurantes, hospitais, escolas etc. |
| >>>>> foodPoint              | [PointStyle](#pointstyle) | Restaurantes, cafés etc. |
| >>> populatedPlace           | [PointStyle](#pointstyle) | Ícones que representam o tamanho do local habitado (por exemplo: uma cidade ou vilarejo). |
| >>>> capital                 | [PointStyle](#pointstyle) | Ícones que representam a capital de um lugar habitado. |
| >>>>> adminDistrictCapital   | [PointStyle](#pointstyle) | Ícones que representam a capital de um estado ou província. |
| >>>>> countryRegionCapital   | [PointStyle](#pointstyle) | Ícones que representam a capital de um país ou região. |
| >>> roadShield               | [PointStyle](#pointstyle) | Sinais que representam o nome compacto de uma estrada. (Por exemplo: I-5). Use apenas os valores de paleta se você definir a propriedade **ImageFamily** da entrada de configurações com um valor *Palette* |
| >>> roadExit                 | [PointStyle](#pointstyle) | Ícones que representam saídas, normalmente de uma autoestrada de acesso controlado. |
| >>> transit                  | [PointStyle](#pointstyle) | Ícones que representam rodoviárias, estações de trem, aeroportos etc. |
| >> political                 | [BorderedMapElement](#borderedmapelement) | Regiões políticas, como países, regiões e estados. |
| >>> countryRegion            | [BorderedMapElement](#borderedmapelement) |  |
| >>> adminDistrict            | [BorderedMapElement](#borderedmapelement) | Admin1, estados, províncias etc. |
| >>> district                 | [BorderedMapElement](#borderedmapelement) | Admin2, regiões etc. |
| >> structure                 | [MapElement](#mapelement) | Edifícios e outras estruturas de construção semelhantes. |
| >>> building                 | [MapElement](#mapelement) |  |
| >>>> educationBuilding       | [MapElement](#mapelement) |  |
| >>>> medicalBuilding         | [MapElement](#mapelement) |  |
| >>>> transitBuilding         | [MapElement](#mapelement) |  |
| >> transportation            | [TwoToneLineStyle](#twotonelinestyle) | Linhas que fazem parte da rede de transporte (por exemplo: rodovias, trens e barcas). |
| >>> road                     | [TwoToneLineStyle](#twotonelinestyle) | Linhas que representam todas as estradas. |
| >>>> controlledAccessHighway | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>>> highSpeedRamp          | [TwoToneLineStyle](#twotonelinestyle) | Linhas que representam vias de acesso. Essas vias de acesso normalmente aparecem ao lado de rodovias de acesso controlado. |
| >>>> highway                 | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>> majorRoad               | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>> arterialRoad            | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>> street                  | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>>> ramp                   | [TwoToneLineStyle](#twotonelinestyle) | Linhas que representam a entrada e saída de uma autoestrada. |
| >>>>> unpavedStreet          | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>> tollRoad                | [TwoToneLineStyle](#twotonelinestyle) | Rodovias que acarretam custos de utilização. |
| >>> railway                  | [TwoToneLineStyle](#twotonelinestyle) | Linhas de estrada de ferro. |
| >>> trail                    | [TwoToneLineStyle](#twotonelinestyle) | Percorrendo trilhas em parques ou fazendo caminhadas. |
| >>> waterRoute               | [TwoToneLineStyle](#twotonelinestyle) | Linhas de rota de barcas |
| >> water                     | [MapElement](#mapelement) | Qualquer coisa que se parece com água. Isso inclui oceanos e rios. |
| >>> river                    | [MapElement](#mapelement) | Rios, córregos ou outras passagens de água.  Observe que isso pode ser uma linha ou um polígono e pode se conectar a corpos de água diferentes de rios. |
| > routeMapElement            | [MapElement](#mapelement) | Todas as entradas de roteamento são feitas sob esta entrada. |
| >> routeLine                 | [TwoToneLineStyle](#twotonelinestyle) | O estilo para todas as linhas de rota. |
| >>> walkingRoute             | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>> drivingRoute             | [TwoToneLineStyle](#twotonelinestyle) |  |
| > userMapElement             | [MapElement](#mapelement) | Todas as entradas de usuário são feitas sob esta entrada. |
| >> userPoint                 | [PointStyle](#pointstyle) | O estilo dos pontos de usuário padrão. |
| >> userLine                  | [MapElement](#mapelement) | O estilo das linhas de usuário padrão. |

<span id="properties" />
## <a name="properties"></a>Propriedades

Esta seção descreve as propriedades que você pode usar para cada entrada.

<span id="version" />
### <a name="version-properties"></a>Propriedades de versão

| Propriedade                     | Tipo    | Descrição                                                                                                           |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------|
| version                      | String  | Versão de folha de estilos direcionada. Usado para aplicabilidade. "1.0" para padrão, "1." para atualizações adicionais de pequenos recursos. |

<span id="settings" />
### <a name="settings-properties"></a>Propriedades de configurações

| Propriedade                     | Tipo    | Descrição                                                                                                                                                                                                                 |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| atmosphereVisible            | Bool    | Um sinalizador que indica se o atmosfera aparece no controle 3D.                                                                                                                                                     |
| fogColor                     | Color   | O valor de cor ARGB do nevoeiro a distância que aparece no controle 3D.                                                                                                                                                    |
| glowColor                    | Color   | O valor de cor ARGB que pode ser aplicado para rotular o brilho e o brilho do ícone.                                                                                                                                                     |
| imageFamily                  | String  | O nome da imagem definida para usar para esse estilo. Defina esse valor como *padrão* para sinais que uso corrigido cores que são baseados na entrada do mundo real. Defina esse valor como *Palette* para sinais que usem uma paleta de cores configuráveis. |
| landColor                    | Color   | O valor de cor ARGB da land terra antes que algo seja desenhado naquela terra.                                                                                                                                                     |
| logosVisible                 | Bool    | Um sinalizador que indica se os itens com uma propriedade **Organization** devem desenhar os Logotipos apropriados ou usar um ícone genérico.                                                                                         |
| officialColorVisible         | Bool    | Um sinalizador que indica se os itens que possuem uma propriedade de cor oficial (como linhas de trânsito na China) devem desenhar essa cor. Por exemplo, desative esse valor para um mapa em preto e branco.                               |
| rasterRegionsVisible         | Bool    | Um sinalizador que indica se é possível ou não desenhar regiões de rasterização onde não renderizamos por vetores (por exemplo: Japão e Coreia).                                                                                                |
| shadedReliefVisible          | Bool    | Um sinalizador que indica se é possível ou não desenhar o sombreamento de elevações no mapa.                                                                                                                                                  |
| shadedReliefDarkColor        | Color   | A cor do lado escuro do baixo relevo sombreado.  O canal alfa representa o máximo alfa. value.                                                                                                                            |
| shadedReliefLightColor       | Color   | A cor do lado claro do baixo relevo sombreado.  O canal alfa representa o máximo alfa. value.                                                                                                                           |
| spaceColor                   | Color   | O valor de cor ARGB para a área ao redor do mapa.                                                                                                                                                                               |
| useDefaultImageColors        | Bool    | Um sinalizador que indica se as cores originais no SVG devem ser usadas em vez de procurar a entrada da paleta para cores em uma imagem.                                                                                |

<span id="mapelement" />
### <a name="mapelement-properties"></a>Propriedades de MapElement

| Propriedade                     | Tipo    | Descrição                                                                                                                 |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------------|
| fillColor                    | Color   | A cor que é usada para polígonos de preenchimento, a tela de fundo de ícones de ponto e para o centro de linhas se elas tiverem se dividido. |
| fontFamily                   | String  |                                                                                                                             |
| labelColor                   | Color   |                                                                                                                             |
| labelOutlineColor            | Color   |                                                                                                                             |
| labelScale                   | Float   | O valor pelo qual os tamanhos de rótulo padrão são dimensionados. Por exemplo, use *1* para padrão e *2* para o dobro do tamanho.            |
| labelVisible                 | Bool    |                                                                                                                             |
| strokeColor                  | Color   | A cor a ser usada para o contorno em torno de polígonos, o contorno em torno dos ícones de ponto e a cor das linhas.                   |
| strokeWidthScale             | Float   | O valor pelo qual o traço de linhas é dimensionado. Por exemplo, use *1* para padrão e *2* para o dobro do tamanho.            |
| visible                      | Bool    |                                                                                                                             |

<span id="borderedmap" />
### <a name="borderedmapelement"></a>BorderedMapElement

Esse grupo de propriedades herda do grupo de propriedades [MapElement](#mapelement).

| Propriedade                     | Tipo    | Descrição                                                           |
|------------------------------|---------|-----------------------------------------------------------------------|
| borderOutlineColor           | Color   | A cor secundária a linha de revestimento da borda de um polígono preenchido. |
| borderStrokeColor            | Color   | A cor da linha principal da borda de um polígono preenchido.             |
| borderVisible                | Bool    |                                                                       |
| borderWidthScale             | Float   |                                                                       |

<span id="pointstyle" />
### <a name="pointstyle-properties"></a>Propriedades de PointStyle

Esse grupo de propriedades herda do grupo de propriedades [MapElement](#mapelement).

| Propriedade                     | Tipo    | Descrição                                                                                                        |
|------------------------------|---------|--------------------------------------------------------------------------------------------------------------------|
| iconColor                    | Color   | A cor do glifo mostrado no meio de um ícone de ponto.                                                        |
| scale                        | Float   | O valor pelo qual o tamanho inteiro do ponto é dimensionado. Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| stemColor                    | Color   | A cor do tronco oriundo da parte inferior do ícone no modo de 3D.                                             |
| stemOutlineColor             | Color   | A cor do contorno do tronco oriundo da parte inferior do ícone no modo de 3D.                          |

<span id="twotonelinestyle" />
### <a name="twotonelinestyle-properties"></a>Propriedades de TwoToneLineStyle

Esse grupo de propriedades herda do grupo de propriedades [MapElement](#mapelement).

| Propriedade                     | Tipo    | Descrição                                                                                          |
|------------------------------|---------|------------------------------------------------------------------------------------------------------|
| overwriteColor               | Bool    |  Faz com que o valor alfa de **FillColor** substitua **StrokeColor** em vez de mesclar-se com ela. |
