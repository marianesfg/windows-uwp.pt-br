---
description: As entradas e as propriedades de uma folha de estilos de mapa
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Referência da folha de estilos de mapa
ms.date: 03/19/2017
ms.topic: article
keywords: windows 10, uwp, mapas, folha de estilos de mapa
ms.localizationpriority: medium
ms.openlocfilehash: 5f59775de8d86b5a0bae77d8c84e08e0328896f4
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63816682"
---
# <a name="map-style-sheet-reference"></a>Referência da folha de estilos de mapa

Usam tecnologias de mapeamento do Microsoft _folhas de estilo do mapa_ para definir a aparência de mapas.  Uma folha de estilos de mapa é definida usando a notação JSON (JavaScript Object) e pode ser usada em várias maneiras incluindo em um aplicativo Windows Store [MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol) por meio de [MapStyleSheet.ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_) método.

Folhas de estilos podem ser criadas interativamente usando o [Editor de folha de estilo de mapa](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) aplicativo.

O JSON a seguir pode ser usado para fazer com que as áreas de água aparecem em vermelho, água rótulos exibidos em verde e áreas de terra aparecem em azul:

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

Este tópico mostra as entradas JSON e as [propriedades](#properties) que você pode usar para personalizar a aparência dos seus mapas.  Essas propriedades também podem ser aplicadas a elementos do mapa de usuário por meio de [MapStyleSheetEntry](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.mapstylesheetentry) propriedade.

<a id="entries" />

## <a name="entries"></a>Entradas
Esta tabela usa os caracteres ">" para representar os níveis na hierarquia da entrada.  Ele também mostra quais versões do Windows dão suporte a cada entrada e que ignorá-lo.

| Version | Nome de versão do Windows |
|---------|----------------------|
|  1703   | Atualização dos criadores      |
|  1709   | Atualização do Fall Creators |
|  1803   | Atualização de abril de 2018    |
|  1809   | Atualização de outubro de 2018  |

| Nome                         | Grupo de propriedades            | 1703 | 1709 | 1803 | 1809 | Descrição    |
|------------------------------|---------------------------|------|------|------|------|----------------|
| version                      | [Version](#version)       |  ✔   |  ✔   |  ✔   |  ✔   | A versão de folha de estilos que você deseja usar. |
| configurações                     | [Configurações](#settings)     |  ✔   |  ✔   |  ✔   |  ✔   | As configurações que se aplicam à folha de estilos inteira. |
| mapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | A entrada pai para todas as entradas do mapa. |
| > baseMapElement             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | A entrada pai para todas as entradas que não foram feitas por usuário. |
| >> area                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Usam áreas que descreve a Terra.  Eles não devem para ser confundido com os prédios físicos que estão sob a entrada de estrutura. |
| >>> airport                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que englobam aeroportos. |
| >>> areaOfInterest           | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Áreas em que há uma alta concentração de empresas ou pontos de interessante. |
| >>> cemetery                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que englobam cemeteries. |
| >>> continent                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Rótulos de área continente. |
| >>> education                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que englobam escolas e outros recursos educacionais. |
| >>> indigenousPeoplesReserve | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Reserva de áreas que englobam peoples nativos. |
| >>> industrial               | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Áreas que são usadas para fins de industriais. |
| >>> island                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Rótulos de área de ilha. |
| >>> medical                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que são usadas para fins de médicos (por exemplo: um campus hospital). |
| >>> military                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abrangem a militares bases ou tem a militares usa. |
| >>> nautical                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que são usadas para fins relacionados náuticos. |
| >>> neighborhood             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Rótulos de área de vizinhança. |
| >>> runway                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que é usado como uma pista de avião. |
| >>> sand                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas arenosas, como praias. |
| >>> shoppingCenter           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas de chão alocadas para shopping centers ou outros centros comerciais. |
| >>> stadium                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que englobam Estádio. |
| >>> underground              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Áreas subterrâneas (por exemplo: um volume de estação de metrô). |
| >>> vegetation               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Florestas, áreas gramadas etc. |
| >>>> forest                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas de floresta. |
| >>>> golfCourse              | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que englobam Golfe cursos. |
| >>>> park                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que englobam parques. |
| >>>> playingField            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Campos extraídos, como um campo de baseball ou uma quadra de tênis. |
| >>>> reserve                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que englobam a natureza de reserva. |
| >> point                     | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Todos os recursos de ponto são desenhados com um ícone de algum tipo. |
| >>> address                  | [PointStyle](#pointstyle) |      |      |  ✔   |  ✔   | Rótulos de números de endereço. |
| >>> naturalPoint             | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam recursos naturais. |
| >>>> peak                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam picos de montanha. |
| >>>>> volcanicPeak           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam picos de vulcão. |
| >>>> waterPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam os locais de recursos hídricos, como uma cachoeira. |
| >>> pointOfInterest          | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam qualquer local interessante. |
| >>>> business                | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam qualquer local de negócios. |
| >>>>> attractionPoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Ícones que representam atrações turísticas como museus, zoológicos, etc. |
| >>>>> communityPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Ícones que representam os locais de uso geral para a comunidade. |
| >>>>> educationPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Ícones que representam as escolas e outra educação relacionadas a locais. |
| >>>>> entertainmentPoint     | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Ícones que representam os locais de entretenimento, como teatros, cinemas, etc. |
| >>>>> essentialServicePoint  | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Ícones que representam os serviços essenciais como estacionamento, bancos, gás, etc. |
| >>>>> foodPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam os restaurantes, cafés, etc. |
| >>>>> lodgingPoint           | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Ícones que representam os hotéis e outras empresas de hospedagem. |
| >>>>> realEstatePoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Ícones que representam as empresas de imóveis. |
| >>>>> shoppingPoint          | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Ícones que representam os hotéis e outras empresas de hospedagem. |
| >>> populatedPlace           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam o tamanho do local habitado (por exemplo: uma cidade ou vilarejo). |
| >>>> capital                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam a capital de um lugar habitado. |
| >>>>> adminDistrictCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam a capital de um estado ou província. |
| >>>>> countryRegionCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam a capital de um país ou região. |
| >>> roadShield               | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Sinais que representam o nome compacto de uma estrada. (Por exemplo: I-5). Use apenas os valores de paleta se você definir a propriedade **ImageFamily** da entrada de configurações com um valor *Palette* |
| >>> roadExit                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam saídas, normalmente de uma autoestrada de acesso controlado. |
| >>> transit                  | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam rodoviárias, estações de trem, aeroportos etc. |
| >> political                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Regiões políticas, como países, regiões e estados. |
| >>> countryRegion            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bordas de região de país e rótulos. |
| >>> adminDistrict            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin1, estados, províncias, etc., bordas e rótulos. |
| >>> district                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin2, municípios, etc., bordas e rótulos. |
| >> structure                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Edifícios e outras estruturas de construção semelhantes. |
| >>> building                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Prédios. |
| >>>> educationBuilding       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Usado para educação de prédios. |
| >>>> medicalBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Usado para fins de médicos, como hospitais de prédios. |
| >>>> transitBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Usado para o trânsito, como aeroportos de prédios. |
| >> transportation            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que fazem parte da rede de transporte (por exemplo: rodovias, trens e barcas). |
| >>> road                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam todas as estradas. |
| >>>> controlledAccessHighway | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam rodovias grande acesso controlado. |
| >>>>> highSpeedRamp          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam Rampas de alta velocidade que normalmente se conectam ao controlado rodovias de acesso. |
| >>>> highway                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam vias expressas. |
| >>>> majorRoad               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam as principais estradas. |
| >>>> arterialRoad            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam arterial estradas. |
| >>>> street                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam ruas. |
| >>>>> ramp                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam Rampas que normalmente se conectam ao vias expressas. |
| >>>>> unpavedStreet          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam ruas unpaved. |
| >>>> tollRoad                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam as estradas que custam dinheiro para usar. |
| >>> railway                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas de estrada de ferro. |
| >>> trail                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Percorrendo trilhas em parques ou fazendo caminhadas. |
| >>> rolante                  | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Rolante com privilégios elevados. |
| >>> waterRoute               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas de rota de barcas |
| >> water                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Qualquer coisa que se parece com água. Isso inclui oceanos e rios. |
| >>> river                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Rios, córregos ou outras passagens de água.  Observe que isso pode ser uma linha ou um polígono e pode se conectar a corpos de água diferentes de rios. |
| > routeMapElement            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Todas as entradas relacionadas roteamentos. |
| >> routeLine                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Rotear linha as entradas relacionadas. |
| >>> drivingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam rotas de condução. |
| >>> scenicRoute              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Linhas que representam rotas scenic de condução. |
| >>> walkingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam a movimentação de rotas. |
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
| fogColor                     | Cor   |  ✔   |  ✔   |  ✔   |  ✔   | O valor de cor ARGB do nevoeiro a distância que aparece no controle 3D. |
| glowColor                    | Cor   |  ✔   |  ✔   |  ✔   |  ✔   | O valor de cor ARGB que pode ser aplicado para rotular o brilho e o brilho do ícone. |
| imageFamily                  | String  |  ✔   |  ✔   |  ✔   |  ✔   | O nome da imagem definida para usar para esse estilo. Defina esse valor como *padrão* para sinais que uso corrigido cores que são baseados na entrada do mundo real. Defina esse valor como *Palette* para sinais que usem uma paleta de cores configuráveis. |
| landColor                    | Cor   |  ✔   |  ✔   |  ✔   |  ✔   | O valor de cor ARGB da land terra antes que algo seja desenhado naquela terra. |
| logosVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Um sinalizador que indica se os itens com uma propriedade **Organization** devem desenhar os Logotipos apropriados ou usar um ícone genérico. |
| officialColorVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Um sinalizador que indica se os itens que possuem uma propriedade de cor oficial (como linhas de trânsito na China) devem desenhar essa cor. Por exemplo, desative esse valor para um mapa em preto e branco. |
| rasterRegionsVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Um sinalizador que indica se deve ou não desenhar regiões de varredura em que eles têm a melhor representação de vetores (Japão e Coreia). |
| shadedReliefVisible          | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Um sinalizador que indica se é possível ou não desenhar o sombreamento de elevações no mapa. |
| shadedReliefDarkColor        | Cor   |  ✔   |  ✔   |  ✔   |  ✔   | A cor do lado escuro do baixo relevo sombreado.  Canal alfa representa o máximo valor alfa. |
| shadedReliefLightColor       | Cor   |  ✔   |  ✔   |  ✔   |  ✔   | A cor do lado claro do baixo relevo sombreado.  Canal alfa representa o máximo valor alfa. |
| shadowColor                  | Cor   |      |      |      |  ✔   | A cor da sombra atrás de ícones que usam sombras. |
| spaceColor                   | Cor   |  ✔   |  ✔   |  ✔   |  ✔   | O valor de cor ARGB para a área ao redor do mapa. |
| useDefaultImageColors        | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Um sinalizador que indica se as cores originais em SVG devem ser usado em vez de aparência a entrada da paleta de cores em uma imagem. |

<a id="mapelement" />

### <a name="mapelement-properties"></a>Propriedades de MapElement

| Propriedade                     | Tipo    | 1703 | 1709 | 1803 | 1809 | Descrição |
|------------------------------|---------|------|------|------|------|-------------|
| backgroundScale              | Float   |  ✔   |  ✔   |  ✔   |  ✔   | Quantidade pela qual o elemento de fundo de um ícone deve ser dimensionado.  Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| fillColor                    | Cor   |  ✔   |  ✔   |  ✔   |  ✔   | A cor que é usada para polígonos de preenchimento, a tela de fundo de ícones de ponto e para o centro de linhas se elas tiverem se dividido. |
| fontFamily                   | String  |  ✔   |  ✔   |  ✔   |  ✔   |  |
| iconColor                    | Cor   |  ✔   |  ✔   |  ✔   |  ✔   | A cor do glifo mostrado no meio de um ícone de ponto. |
| iconScale                    | Float   |      |  ✔   |  ✔   |  ✔   | Quantidade pela qual o glifo de um ícone deve ser dimensionado.  Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| labelColor                   | Cor   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelOutlineColor            | Cor   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelScale                   | Float   |  ✔   |  ✔   |  ✔   |  ✔   | O valor pelo qual os tamanhos de rótulo padrão são dimensionados. Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| labelVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  |
| overwriteColor               | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Faz com que o valor alfa de **FillColor** substitua **StrokeColor** em vez de mesclar-se com ela. |
| scale                        | Float   |  ✔   |  ✔   |  ✔   |  ✔   | O valor pelo qual o tamanho inteiro do ponto é dimensionado. Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| strokeColor                  | Cor   |  ✔   |  ✔   |  ✔   |  ✔   | A cor a ser usada para o contorno em torno de polígonos, o contorno em torno dos ícones de ponto e a cor das linhas. |
| strokeWidthScale             | Float   |  ✔   |  ✔   |  ✔   |  ✔   | O valor pelo qual o traço de linhas é dimensionado. Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| visible                      | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  |

<a id="borderedmap" />

### <a name="borderedmapelement"></a>BorderedMapElement

Esse grupo de propriedades herda do grupo de propriedades [MapElement](#mapelement).

| Propriedade                     | Tipo    | 1703 | 1709 | 1803 | 1809 | Descrição |
|------------------------------|---------|------|------|------|------|-------------|
| borderOutlineColor           | Cor   |  ✔   |  ✔   |  ✔   |  ✔   | A cor secundária a linha de revestimento da borda de um polígono preenchido. |
| borderStrokeColor            | Cor   |  ✔   |  ✔   |  ✔   |  ✔   | A cor da linha principal da borda de um polígono preenchido. |
| borderVisible                | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  |
| borderWidthScale             | Float   |  ✔   |  ✔   |  ✔   |  ✔   | A quantidade pela qual o traço de bordas são dimensionados. Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |

<a id="pointstyle" />

### <a name="pointstyle-properties"></a>Propriedades de PointStyle

Esse grupo de propriedades herda do grupo de propriedades [MapElement](#mapelement).

| Propriedade                     | Tipo    | 1703 | 1709 | 1803 | 1809 | Descrição |
|------------------------------|---------|------|------|------|------|-------------|
| plano de fundo de forma             | Float   |      |      |      |  ✔   | Forma de usar como plano de fundo do ícone, substituindo qualquer forma que exista nela. |
| stemAnchorRadiusScale        | Float   |      |      |  ✔   |  ✔   | Quantidade pela qual o ponto de ancoramento de um tronco do ícone deve ser dimensionado.  Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| stemColor                    | Cor   |  ✔   |  ✔   |  ✔   |  ✔   | A cor do tronco oriundo da parte inferior do ícone no modo de 3D. |
| stemHeightScale              | Float   |      |      |  ✔   |  ✔   | Quantidade pela qual o tamanho do tronco de um ícone deve ser dimensionado.  Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| stemOutlineColor             | Cor   |  ✔   |  ✔   |  ✔   |  ✔   | A cor do contorno do tronco oriundo da parte inferior do ícone no modo de 3D. |
| stemWidthScale               | Float   |  ✔   |  ✔   |  ✔   |  ✔   | Quantidade pela qual a largura do tronco de um ícone deve ser dimensionada.  Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |

<a id="mapelement3d" />

### <a name="mapelement3d"></a>MapElement3D

Esse grupo de propriedades herda do grupo de propriedades [MapElement](#mapelement).

| Propriedade                     | Tipo    | 1703 | 1709 | 1803 | 1809 | Descrição |
|------------------------------|---------|------|------|------|------|------------|
| renderAsSurface              | Bool    |      |      |  ✔   |  ✔   | Um sinalizador indica que um modelo 3D deve ser renderizado como uma construção, sem esmaecimento de profundidade em relação ao piso. |
