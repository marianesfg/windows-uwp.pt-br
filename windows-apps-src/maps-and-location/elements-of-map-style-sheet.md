---
description: As entradas e as propriedades de uma folha de estilos de mapa
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Referência da folha de estilos de mapa
ms.date: 03/16/2017
ms.topic: article
keywords: windows 10, uwp, mapas, folha de estilos de mapa
ms.localizationpriority: medium
ms.openlocfilehash: 9ecf83b25a8f448cb0ac22bbad0aa2d4f687c721
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8202784"
---
# <a name="map-style-sheet-reference"></a>Referência da folha de estilos de mapa

Tecnologias de mapeamento da Microsoft usam _folhas de estilo de mapa_ para definir a aparência de mapas.  Uma folha de estilos de mapa é definida usando JavaScript Object Notation (JSON) e pode ser usada em várias maneiras incluindo em de um aplicativo Windows Store [MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol) por meio do método [MapStyleSheet.ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_) .

Folhas de estilo podem ser criadas interativamente usando o aplicativo [Editor de folha de estilo de mapa](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) .

O JSON a seguir pode ser usado para tornar áreas de água aparecem em vermelho, os rótulos de água aparecem em verde e áreas de terra aparecem em azul:

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

Este JSON pode ser usado para remover todos os rótulos e pontos de um mapa.

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

Este tópico mostra as entradas JSON e as [propriedades](#properties) que você pode usar para personalizar a aparência dos seus mapas.  Essas propriedades também podem ser aplicadas aos elementos de mapa do usuário por meio da propriedade [MapStyleSheetEntry](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.mapstylesheetentry) .

<a id="entries" />

## <a name="entries"></a>Entradas
Esta tabela usa os caracteres ">" para representar os níveis na hierarquia da entrada.  Ele também mostra quais versões do Windows oferecem suporte a cada entrada e que ignorá-la.

| Version | Nome de versão do Windows |
|---------|----------------------|
|  1703   | Atualização dos criadores      |
|  1709   | Atualização dos criadores do segundo semestre |
|  1803   | Atualização de abril de 2018    |
|  1809   | Atualização de outubro de 2018  |

| Nome                         | Grupo de propriedades            | 1703 | 1709 | 1803 | 1809 | Descrição    |
|------------------------------|---------------------------|------|------|------|------|----------------|
| version                      | [Version](#version)       |  ✔   |  ✔   |  ✔   |  ✔   | A versão de folha de estilos que você deseja usar. |
| settings                     | [Settings](#settings)     |  ✔   |  ✔   |  ✔   |  ✔   | As configurações que se aplicam à folha de estilos inteira. |
| mapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | A entrada pai para todas as entradas do mapa. |
| > baseMapElement             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | A entrada pai para todas as entradas que não foram feitas por usuário. |
| >> area                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Usam áreas descrevendo Terra.  Eles não devem para ser confundido com os edifícios físicos que estão sob a entrada de estrutura. |
| >>> airport                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abrangem aeroporto. |
| >>> areaOfInterest           | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Áreas em que há uma alta concentração de empresas ou pontos de interessante. |
| >>> cemetery                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abrangem cemitérios. |
| >>> continent                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Rótulos de área continente. |
| >>> education                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abrangem escolas e outros recursos educacionais. |
| >>> indigenousPeoplesReserve | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abrangem popular Indígena reserva. |
| >>> industrial               | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Áreas que são usadas para fins industriais. |
| >>> island                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Rótulos de área de ilha. |
| >>> medical                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que são usadas para fins médicos (por exemplo: um campus de hospital). |
| >>> military                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abrangem bases militares ou têm usos militares. |
| >>> nautical                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que são usadas para fins náuticos relacionados. |
| >>> neighborhood             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Rótulos de área de bairro. |
| >>> runway                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que é usado como uma pista avião. |
| >>> sand                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas arenosas, como praias. |
| >>> shoppingCenter           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas de chão alocadas para shopping centers ou outros centros comerciais. |
| >>> stadium                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abrangem Estádio. |
| >>> underground              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Áreas subterrâneas (por exemplo: um volume de estação de metrô). |
| >>> vegetation               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Florestas, áreas gramadas etc. |
| >>>> forest                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas de floresta. |
| >>>> golfCourse              | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abrangem Golfe cursos. |
| >>>> park                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abrangem parques. |
| >>>> playingField            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Campos extraídos, como um campo de baseball ou uma quadra de tênis. |
| >>>> reserve                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abrangem a natureza reserva. |
| >> point                     | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Todos os recursos que são desenhados com um ícone de algum tipo de ponto. |
| >>> address                  | [PointStyle](#pointstyle) |      |      |  ✔   |  ✔   | Rótulos de números de endereço. |
| >>> naturalPoint             | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam recursos naturais. |
| >>>> peak                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam picos de montanha. |
| >>>>> volcanicPeak           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam picos de vulcão. |
| >>>> waterPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam os locais de recursos hídricos, como uma cachoeira. |
| >>> pointOfInterest          | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam qualquer local interessante. |
| >>>> business                | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam qualquer locaiton de negócios. |
| >>>>> attractionPoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Ícones que representam atrações turísticas como museus, zoos, etc. |
| >>>>> communityPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Ícones que representam os locais de uso geral para a comunidade. |
| >>>>> educationPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Ícones que representam escolas e outro education relacionados locais. |
| >>>>> entertainmentPoint     | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Ícones que representam entretenimento como teatros, cinemas, etc. |
| >>>>> essentialServicePoint  | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Ícones que representam serviços essenciais como vaga, bancos, pedal, etc. |
| >>>>> foodPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam restaurantes, cafés, etc. |
| >>>>> lodgingPoint           | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Ícones que representam hotéis e outras empresas de hospedagem. |
| >>>>> realEstatePoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Ícones que representam as empresas imóveis. |
| >>>>> shoppingPoint          | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Ícones que representam hotéis e outras empresas de hospedagem. |
| >>> populatedPlace           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam o tamanho do local habitado (por exemplo: uma cidade ou vilarejo). |
| >>>> capital                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam a capital de um lugar habitado. |
| >>>>> adminDistrictCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam a capital de um estado ou província. |
| >>>>> countryRegionCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam a capital de um país ou região. |
| >>> roadShield               | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Sinais que representam o nome compacto de uma estrada. (Por exemplo: I-5). Use apenas os valores de paleta se você definir a propriedade **ImageFamily** da entrada de configurações com um valor *Palette* |
| >>> roadExit                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam saídas, normalmente de uma autoestrada de acesso controlado. |
| >>> transit                  | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam rodoviárias, estações de trem, aeroportos etc. |
| >> political                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Regiões políticas, como países, regiões e estados. |
| >>> countryRegion            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bordas de região do país e rótulos. |
| >>> adminDistrict            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin1, estados, províncias, etc., bordas e rótulos. |
| >>> district                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin2, regiões, etc., bordas e rótulos. |
| >> structure                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Edifícios e outras estruturas de construção semelhantes. |
| >>> building                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Edifícios. |
| >>>> educationBuilding       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Edifícios usados para educação. |
| >>>> medicalBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Edifícios usados para fins médicos, como hospitais. |
| >>>> transitBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Edifícios usados para trânsito, como aeroportos. |
| >> transportation            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que fazem parte da rede de transporte (por exemplo: rodovias, trens e barcas). |
| >>> road                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam todas as estradas. |
| >>>> controlledAccessHighway | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam grandes, controlado rodovias de acesso. |
| >>>>> highSpeedRamp          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam vias de acesso de alta velocidade que normalmente se conectam à controlada rodovias de acesso. |
| >>>> highway                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam rodovias. |
| >>>> majorRoad               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam estradas principais. |
| >>>> arterialRoad            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam arterial estradas. |
| >>>> street                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam ruas. |
| >>>>> ramp                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam vias de acesso que normalmente se conectam à rodovias. |
| >>>>> unpavedStreet          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam ruas unpaved. |
| >>>> tollRoad                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam rodovias que acarretam usar. |
| >>> railway                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas de estrada de ferro. |
| >>> trail                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Percorrendo trilhas em parques ou fazendo caminhadas. |
| >>> rolante                  | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Rolante com privilégios elevados. |
| >>> waterRoute               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas de rota de barcas |
| >> water                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Qualquer coisa que se parece com água. Isso inclui oceanos e rios. |
| >>> river                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Rios, córregos ou outras passagens de água.  Observe que isso pode ser uma linha ou um polígono e pode se conectar a corpos de água diferentes de rios. |
| > routeMapElement            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Todas as entradas roteamento relacionadas. |
| >> routeLine                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linha de rota entradas relacionadas. |
| >>> drivingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam as rotas de carro. |
| >>> scenicRoute              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Linhas que representam as rotas de carro estradas. |
| >>> walkingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam a pé rotas. |
| > userMapElement             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Todas as entradas do usuário. |
| >> userBillboard             | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | O estilo das instâncias padrão de [MapBillboard](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard). |
| >> userLine                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | O estilo das instâncias padrão de [MapPolyline](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mappolyline). |
| >> userModel3D               | [MapElement3D](#mapelement3d) |      |  ✔   |  ✔   |  ✔   | O estilo das instâncias padrão de [MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d).  Isso acontece principalmente para configurar renderAsSurface. |
| >> userPoint                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | O estilo das instâncias padrão de [MapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon). |

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

| Propriedade                     | Tipo    | 1703 | 1709 | 1803 | 1809 | Descrição |
|------------------------------|---------|------|------|------|------|-------------|
| atmosphereVisible            | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Um sinalizador que indica se o atmosfera aparece no controle 3D. |
| buildingTexturesVisible      | Bool    |      |      |  ✔   |  ✔   | Um sinalizador que indica se deseja ou não mostrar texturas em construções 3D simbólicas com texturas. |
| fogColor                     | Color   |  ✔   |  ✔   |  ✔   |  ✔   | O valor de cor ARGB do nevoeiro a distância que aparece no controle 3D. |
| glowColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   | O valor de cor ARGB que pode ser aplicado para rotular o brilho e o brilho do ícone. |
| imageFamily                  | String  |  ✔   |  ✔   |  ✔   |  ✔   | O nome da imagem definida para usar para esse estilo. Defina esse valor como *padrão* para sinais que uso corrigido cores que são baseados na entrada do mundo real. Defina esse valor como *Palette* para sinais que usem uma paleta de cores configuráveis. |
| landColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   | O valor de cor ARGB da land terra antes que algo seja desenhado naquela terra. |
| logosVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Um sinalizador que indica se os itens com uma propriedade **Organization** devem desenhar os Logotipos apropriados ou usar um ícone genérico. |
| officialColorVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Um sinalizador que indica se os itens que possuem uma propriedade de cor oficial (como linhas de trânsito na China) devem desenhar essa cor. Por exemplo, desative esse valor para um mapa em preto e branco. |
| rasterRegionsVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Um sinalizador que indica se deseja ou não desenhar regiões de rasterização onde eles têm uma representação melhor que vetores (Japão e Coreia). |
| shadedReliefVisible          | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Um sinalizador que indica se é possível ou não desenhar o sombreamento de elevações no mapa. |
| shadedReliefDarkColor        | Color   |  ✔   |  ✔   |  ✔   |  ✔   | A cor do lado escuro do baixo relevo sombreado.  O canal alfa representa o valor máximo alfa. |
| shadedReliefLightColor       | Color   |  ✔   |  ✔   |  ✔   |  ✔   | A cor do lado claro do baixo relevo sombreado.  O canal alfa representa o valor máximo alfa. |
| shadowColor                  | Cor   |      |      |      |  ✔️   | A cor da sombra por trás de ícones que usam sombras. |
| spaceColor                   | Color   |  ✔   |  ✔   |  ✔   |  ✔   | O valor de cor ARGB para a área ao redor do mapa. |
| useDefaultImageColors        | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Um sinalizador que indica se as cores originais no SVG devem ser usados em vez de com a entrada de paleta de cores em uma imagem. |

<a id="mapelement" />

### <a name="mapelement-properties"></a>Propriedades de MapElement

| Propriedade                     | Tipo    | 1703 | 1709 | 1803 | 1809 | Descrição |
|------------------------------|---------|------|------|------|------|-------------|
| backgroundScale              | Float   |  ✔   |  ✔   |  ✔   |  ✔   | Quantidade pela qual o elemento de fundo de um ícone deve ser dimensionado.  Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| fillColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   | A cor que é usada para polígonos de preenchimento, a tela de fundo de ícones de ponto e para o centro de linhas se elas tiverem se dividido. |
| fontFamily                   | Cadeia de caracteres  |  ✔   |  ✔   |  ✔   |  ✔   |  |
| iconColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   | A cor do glifo mostrado no meio de um ícone de ponto. |
| iconScale                    | Float   |      |  ✔   |  ✔   |  ✔   | Quantidade pela qual o glifo de um ícone deve ser dimensionado.  Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| labelColor                   | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelOutlineColor            | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelScale                   | Float   |  ✔   |  ✔   |  ✔   |  ✔   | O valor pelo qual os tamanhos de rótulo padrão são dimensionados. Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| labelVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  |
| overwriteColor               | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Faz com que o valor alfa de **FillColor** substitua **StrokeColor** em vez de mesclar-se com ela. |
| scale                        | Float   |  ✔   |  ✔   |  ✔   |  ✔   | O valor pelo qual o tamanho inteiro do ponto é dimensionado. Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| strokeColor                  | Color   |  ✔   |  ✔   |  ✔   |  ✔   | A cor a ser usada para o contorno em torno de polígonos, o contorno em torno dos ícones de ponto e a cor das linhas. |
| strokeWidthScale             | Float   |  ✔   |  ✔   |  ✔   |  ✔   | O valor pelo qual o traço de linhas é dimensionado. Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| visible                      | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  |

<a id="borderedmap" />

### <a name="borderedmapelement"></a>BorderedMapElement

Esse grupo de propriedades herda do grupo de propriedades [MapElement](#mapelement).

| Propriedade                     | Tipo    | 1703 | 1709 | 1803 | 1809 | Descrição |
|------------------------------|---------|------|------|------|------|-------------|
| borderOutlineColor           | Color   |  ✔   |  ✔   |  ✔   |  ✔   | A cor secundária a linha de revestimento da borda de um polígono preenchido. |
| borderStrokeColor            | Color   |  ✔   |  ✔   |  ✔   |  ✔   | A cor da linha principal da borda de um polígono preenchido. |
| borderVisible                | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  |
| borderWidthScale             | Float   |  ✔   |  ✔   |  ✔   |  ✔   | O valor pelo qual o traço de bordas são dimensionados. Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |

<a id="pointstyle" />

### <a name="pointstyle-properties"></a>Propriedades de PointStyle

Esse grupo de propriedades herda do grupo de propriedades [MapElement](#mapelement).

| Propriedade                     | Tipo    | 1703 | 1709 | 1803 | 1809 | Descrição |
|------------------------------|---------|------|------|------|------|-------------|
| plano de fundo de forma             | Float   |      |      |      |  ✔️   | Forma a ser usada como o plano de fundo do ícone, substituindo qualquer forma nesse local. |
| stemAnchorRadiusScale        | Float   |      |      |  ✔   |  ✔   | Quantidade pela qual o ponto de ancoramento de um tronco do ícone deve ser dimensionado.  Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| stemColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   | A cor do tronco oriundo da parte inferior do ícone no modo de 3D. |
| stemHeightScale              | Float   |      |      |  ✔   |  ✔   | Quantidade pela qual o tamanho do tronco de um ícone deve ser dimensionado.  Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| stemOutlineColor             | Color   |  ✔   |  ✔   |  ✔   |  ✔   | A cor do contorno do tronco oriundo da parte inferior do ícone no modo de 3D. |
| stemWidthScale               | Float   |  ✔   |  ✔   |  ✔   |  ✔   | Quantidade pela qual a largura do tronco de um ícone deve ser dimensionada.  Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |

<a id="mapelement3d" />

### <a name="mapelement3d"></a>MapElement3D

Esse grupo de propriedades herda do grupo de propriedades [MapElement](#mapelement).

| Propriedade                     | Tipo    | 1703 | 1709 | 1803 | 1809 | Descrição |
|------------------------------|---------|------|------|------|------|------------|
| renderAsSurface              | Bool    |      |      |  ✔   |  ✔   | Um sinalizador indica que um modelo 3D deve ser renderizado como uma construção, sem esmaecimento de profundidade em relação ao piso. |
