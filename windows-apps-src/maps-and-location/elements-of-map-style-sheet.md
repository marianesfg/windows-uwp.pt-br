---
author: normesta
description: As entradas e as propriedades de uma folha de estilos de mapa
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Referência da folha de estilos de mapa
ms.author: normesta
ms.date: 03/16/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, mapas, folha de estilos de mapa
ms.localizationpriority: medium
ms.openlocfilehash: 8fb80bc28900ee695ecf3b9e62b5dafc8f1a8cb3
ms.sourcegitcommit: f91aa1e402f1bc093b48a03fbae583318fc7e05d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2018
ms.locfileid: "1917769"
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

<a id="entries" />

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
| >>> areaOfInterest           | [MapElement](#mapelement) | Áreas em que há uma alta concentração de empresas ou pontos de interessante. |
| >>> cemetery                 | [MapElement](#mapelement) | Áreas de cemitérios. |
| >>> continent                | [MapElement](#mapelement) | Áreas de continentes inteiros. |
| >>> education                | [MapElement](#mapelement) |  |
| >>> indigenousPeoplesReserve | [MapElement](#mapelement) |  |
| >>> industrial               | [MapElement](#mapelement) | Áreas de terra que são usadas para fins industriais. |
| >>> island                   | [MapElement](#mapelement) | Rótulos em áreas insulares. |
| >>> medical                  | [MapElement](#mapelement) | Áreas de terra usadas para fins médicos (por exemplo: um campus de hospital). |
| >>> military                 | [MapElement](#mapelement) | Áreas de bases militares. |
| >>> nautical                 | [MapElement](#mapelement) | Áreas de terra que são usadas para fins náuticos. |
| >>> neighborhood             | [MapElement](#mapelement) | Rótulos em áreas definidas como vizinhanças. |
| >>> runway                   | [MapElement](#mapelement) | Áreas de terra que são cobertas por uma pista. |
| >>> sand                     | [MapElement](#mapelement) | Áreas arenosas, como praias. |
| >>> shoppingCenter           | [MapElement](#mapelement) | Áreas de chão alocadas para shopping centers ou outros centros comerciais. |
| >>> stadium                  | [MapElement](#mapelement) | Área de um estádio. |
| >>> underground              | [MapElement](#mapelement) | Áreas subterrâneas (por exemplo: um volume de estação de metrô). |
| >>> vegetation               | [MapElement](#mapelement) | Florestas, áreas gramadas etc. |
| >>>> forest                  | [MapElement](#mapelement) | Áreas de floresta. |
| >>>> golfCourse              | [MapElement](#mapelement) |  |
| >>>> park                    | [MapElement](#mapelement) | Áreas de parque. |
| >>>> playingField            | [MapElement](#mapelement) | Campos extraídos, como um campo de baseball ou uma quadra de tênis. |
| >>>> reserve                 | [MapElement](#mapelement) | Áreas de reservas naturais. |
| >> point                     | [PointStyle](#pointstyle) | Todos os recursos de ponto renderizados com um ícone de algum tipo. |
| >>> address                  | [PointStyle](#pointstyle) | Números de endereço. |
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
| >> transportation            | [MapElement](#mapelement) | Linhas que fazem parte da rede de transporte (por exemplo: rodovias, trens e barcas). |
| >>> road                     | [MapElement](#mapelement) | Linhas que representam todas as estradas. |
| >>>> controlledAccessHighway | [MapElement](#mapelement) |  |
| >>>>> highSpeedRamp          | [MapElement](#mapelement) | Linhas que representam vias de acesso. Essas vias de acesso normalmente aparecem ao lado de rodovias de acesso controlado. |
| >>>> highway                 | [MapElement](#mapelement) |  |
| >>>> majorRoad               | [MapElement](#mapelement) |  |
| >>>> arterialRoad            | [MapElement](#mapelement) |  |
| >>>> street                  | [MapElement](#mapelement) |  |
| >>>>> ramp                   | [MapElement](#mapelement) | Linhas que representam a entrada e saída de uma autoestrada. |
| >>>>> unpavedStreet          | [MapElement](#mapelement) |  |
| >>>> tollRoad                | [MapElement](#mapelement) | Rodovias que acarretam custos de utilização. |
| >>> railway                  | [MapElement](#mapelement) | Linhas de estrada de ferro. |
| >>> trail                    | [MapElement](#mapelement) | Percorrendo trilhas em parques ou fazendo caminhadas. |
| >>> waterRoute               | [MapElement](#mapelement) | Linhas de rota de barcas |
| >> water                     | [MapElement](#mapelement) | Qualquer coisa que se parece com água. Isso inclui oceanos e rios. |
| >>> river                    | [MapElement](#mapelement) | Rios, córregos ou outras passagens de água.  Observe que isso pode ser uma linha ou um polígono e pode se conectar a corpos de água diferentes de rios. |
| > routeMapElement            | [MapElement](#mapelement) | Todas as entradas de roteamento são feitas sob esta entrada. |
| >> routeLine                 | [MapElement](#mapelement) | O estilo para todas as linhas de rota. |
| >>> drivingRoute             | [MapElement](#mapelement) |  |
| >>> scenicRoute              | [MapElement](#mapelement) |  |
| >>> walkingRoute             | [MapElement](#mapelement) |  |
| > userMapElement             | [MapElement](#mapelement) | Todas as entradas de usuário são feitas sob esta entrada. |
| >> userBillboard             | [MapElement](#mapelement) | O estilo das instâncias padrão de [MapBillboard](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard). |
| >> userLine                  | [MapElement](#mapelement) | O estilo das instâncias padrão de [MapPolyline](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mappolyline). |
| >> userModel3D               | [MapElement3D](#mapelement3d) | O estilo das instâncias padrão de [MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d).  Isso acontece principalmente para configurar renderAsSurface. |
| >> userPoint                 | [PointStyle](#pointstyle) | O estilo das instâncias padrão de [MapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon). |


<a id="properties" />

## <a name="properties"></a>Propriedades

Esta seção descreve as propriedades que você pode usar para cada entrada.

<a id="version" />

### <a name="version-properties"></a>Propriedades de versão

| Propriedade                     | Tipo    | Descrição                                                                                                           |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------|
| version                      | String  | Versão de folha de estilos direcionada. Usado para aplicabilidade. "1.0" para padrão, "1." para atualizações adicionais de pequenos recursos. |

<a id="settings" />

### <a name="settings-properties"></a>Propriedades de configurações

| Propriedade                     | Tipo    | Descrição                                                                                                                                                                                                                 |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| atmosphereVisible            | Bool    | Um sinalizador que indica se o atmosfera aparece no controle 3D.                                                                                                                                                     |
| buildingTexturesVisible      | Bool    | Um sinalizador que indica se deseja ou não mostrar texturas em construções 3D simbólicas com texturas.                                                                                                                          |
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

<a id="mapelement" />

### <a name="mapelement-properties"></a>Propriedades de MapElement

| Propriedade                     | Tipo    | Descrição                                                                                                                       |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------------------|
| backgroundScale              | Float   | Quantidade pela qual o elemento de fundo de um ícone deve ser dimensionado.  Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| fillColor                    | Color   | A cor que é usada para polígonos de preenchimento, a tela de fundo de ícones de ponto e para o centro de linhas se elas tiverem se dividido.       |
| fontFamily                   | Cadeia de caracteres  |                                                                                                                                   |
| iconColor                    | Color   | A cor do glifo mostrado no meio de um ícone de ponto.                                                                       |
| iconScale                    | Float   | Quantidade pela qual o glifo de um ícone deve ser dimensionado.  Por exemplo, use *1* para padrão e *2* para o dobro do tamanho.              |
| labelColor                   | Color   |                                                                                                                                   |
| labelOutlineColor            | Color   |                                                                                                                                   |
| labelScale                   | Float   | O valor pelo qual os tamanhos de rótulo padrão são dimensionados. Por exemplo, use *1* para padrão e *2* para o dobro do tamanho.                  |
| labelVisible                 | Bool    |                                                                                                                                   |
| overwriteColor               | Bool    | Faz com que o valor alfa de **FillColor** substitua **StrokeColor** em vez de mesclar-se com ela.                               |
| scale                        | Float   | O valor pelo qual o tamanho inteiro do ponto é dimensionado. Por exemplo, use *1* para padrão e *2* para o dobro do tamanho.                |
| strokeColor                  | Color   | A cor a ser usada para o contorno em torno de polígonos, o contorno em torno dos ícones de ponto e a cor das linhas.                         |
| strokeWidthScale             | Float   | O valor pelo qual o traço de linhas é dimensionado. Por exemplo, use *1* para padrão e *2* para o dobro do tamanho.                  |
| visible                      | Bool    |                                                                                                                                   |

<a id="borderedmap" />

### <a name="borderedmapelement"></a>BorderedMapElement

Esse grupo de propriedades herda do grupo de propriedades [MapElement](#mapelement).

| Propriedade                     | Tipo    | Descrição                                                           |
|------------------------------|---------|-----------------------------------------------------------------------|
| borderOutlineColor           | Color   | A cor secundária a linha de revestimento da borda de um polígono preenchido. |
| borderStrokeColor            | Color   | A cor da linha principal da borda de um polígono preenchido.             |
| borderVisible                | Bool    |                                                                       |
| borderWidthScale             | Float   |                                                                       |

<a id="pointstyle" />

### <a name="pointstyle-properties"></a>Propriedades de PointStyle

Esse grupo de propriedades herda do grupo de propriedades [MapElement](#mapelement).

| Propriedade                     | Tipo    | Descrição                                                                                                                      |
|------------------------------|---------|----------------------------------------------------------------------------------------------------------------------------------|
| stemAnchorRadiusScale        | Float   | Quantidade pela qual o ponto de ancoramento de um tronco do ícone deve ser dimensionado.  Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| stemColor                    | Color   | A cor do tronco oriundo da parte inferior do ícone no modo de 3D.                                                           |
| stemHeightScale              | Float   | Quantidade pela qual o tamanho do tronco de um ícone deve ser dimensionado.  Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| stemWidthScale               | Float   | Quantidade pela qual a largura do tronco de um ícone deve ser dimensionada.  Por exemplo, use *1* para padrão e *2* para o dobro do tamanho.  |
| stemOutlineColor             | Color   | A cor do contorno do tronco oriundo da parte inferior do ícone no modo de 3D.                                        |

<a id="mapelement3d" />

### <a name="mapelement3d"></a>MapElement3D

Esse grupo de propriedades herda do grupo de propriedades [MapElement](#mapelement).

| Propriedade                     | Tipo    | Descrição                                                                                                                      |
|------------------------------|---------|----------------------------------------------------------------------------------------------------------------------------------|
| renderAsSurface              | Bool    | Um sinalizador indica que um modelo 3D deve ser renderizado como uma construção, sem esmaecimento de profundidade em relação ao piso.               |
