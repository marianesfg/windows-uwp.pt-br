---
description: As entradas e as propriedades de uma folha de estilos de mapa
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Referência da folha de estilos de mapa
ms.date: 03/19/2017
ms.topic: article
keywords: windows 10, uwp, mapas, folha de estilos de mapa
ms.localizationpriority: medium
ms.openlocfilehash: b2e6e57721a5667a9ca38b21eee2a618353cd30b
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218556"
---
# <a name="map-style-sheet-reference"></a>Referência da folha de estilos de mapa

As tecnologias de mapeamento da Microsoft usam _folhas de estilo de mapa_ para definir a aparência dos mapas.  Uma folha de estilos de mapa é definida usando JavaScript Object Notation (JSON) e pode ser usada de várias maneiras, incluindo em uma [MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol) do aplicativo da Windows Store por meio do método [MapStyleSheet. ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_) .

As folhas de estilo podem ser criadas interativamente usando o aplicativo [Editor de folha de estilo de mapa](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) .

O JSON a seguir pode ser usado para fazer com que as áreas de água apareçam em vermelho, os rótulos de água aparecem em verde e as áreas de aterrissagem aparecem em azul:

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

Esse JSON pode ser usado para remover todos os rótulos e pontos de um mapa.

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

Este tópico mostra as entradas JSON e as [propriedades](#properties) que você pode usar para personalizar a aparência dos seus mapas.  Essas propriedades também podem ser aplicadas a elementos de mapa do usuário por meio da propriedade [MapStyleSheetEntry](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.mapstylesheetentry) .

<a id="entries" />

## <a name="entries"></a>Entradas
Esta tabela usa os caracteres ">" para representar os níveis na hierarquia da entrada.  Ele também mostra quais versões do Windows oferecem suporte a cada entrada e que a ignora.

| {1&gt;Version&lt;1} | Nome da versão do Windows |
|---------|----------------------|
|  1703   | Atualização dos criadores      |
|  1709   | Fall Creators Update |
|  1803   | Atualização de abril de 2018    |
|  1809   | Atualização de outubro de 2018  |
|  1903   | Atualização 2019 de maio      |

| {1&gt;Nome&lt;1}                               | Grupo de propriedades            | 1703 | 1709 | 1803 | 1809 | 1903 | Descrição    |
|------------------------------------|---------------------------|------|------|------|------|------|----------------|
| version                            | [Versão](#version)       |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | A versão de folha de estilos que você deseja usar. |
| configurações                           | [Configurações](#settings)     |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | As configurações que se aplicam à folha de estilos inteira. |
| mapElement                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | A entrada pai para todas as entradas do mapa. |
| > baseMapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | A entrada pai para todas as entradas que não foram feitas por usuário. |
| >> area                            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que descrevem o uso do solo.  Eles não devem ser confundidos com os prédios físicos que estão sob a entrada da estrutura. |
| >>> airport                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abrangem aeroportos. |
| >>> areaOfInterest                 | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | Áreas em que há uma alta concentração de empresas ou pontos de interessante. |
| >>> cemetery                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abrangem Cemeteries. |
| >>> continent                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Rótulos de área de continente. |
| >>> education                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abrangem escolas e outras instalações educacionais. |
| >>> indigenousPeoplesReserve       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que englobam as pessoas nativos reserva. |
| >>> industrial                     | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que são usadas para fins industriais. |
| >>> island                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Rótulos da área da ilha. |
| >>> medical                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que são usadas para fins médicos (por exemplo: um campus de hospital). |
| >>> military                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abrangem bases militares ou têm usos militares. |
| >>> nautical                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que são usadas para fins relacionados a náuticas. |
| >>> neighborhood                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Rótulos da área de vizinhança. |
| >>> runway                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas usadas como uma pista de avião. |
| >>> sand                           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas arenosas, como praias. |
| >>> shoppingCenter                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas de chão alocadas para shopping centers ou outros centros comerciais. |
| >>> stadium                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abrangem estádios. |
| >>> underground                    | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | Áreas subterrâneas (por exemplo: um volume de estação de metrô). |
| >>> vegetation                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Florestas, áreas gramadas etc. |
| >>>> forest                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas de floresta. |
| >>>> golfCourse                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abrangem cursos de golfe. |
| >>>> park                          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abrangem os parques. |
| >>>> playingField                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Campos extraídos, como um campo de baseball ou uma quadra de tênis. |
| >>>> reserve                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abrangem reservas de natureza. |
| > > frozenWater                     | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Água congelada, como Glacier. |
| >> point                           | [Ponto de extremidade](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Todos os recursos de ponto que são desenhados com um ícone de algum tipo. |
| >>> address                        | [Ponto de extremidade](#pointstyle) |      |      |  ✔   |  ✔   |  ✔   | Rótulos de números de endereço. |
| >>> naturalPoint                   | [Ponto de extremidade](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam recursos naturais. |
| >>>> peak                          | [Ponto de extremidade](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam picos de montanha. |
| >>>>> volcanicPeak                 | [Ponto de extremidade](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam picos de vulcão. |
| >>>> waterPoint                    | [Ponto de extremidade](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam os locais de recursos hídricos, como uma cachoeira. |
| >>> pointOfInterest                | [Ponto de extremidade](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam qualquer local interessante. |
| >>>> business                      | [Ponto de extremidade](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam qualquer local de negócios. |
| > > > > > attractionPoint              | [Ponto de extremidade](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam Tourist Attractions como museus, zoológicos, etc. |
| > > > > > > amusementPlacePoint         | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de local do diversão. |
| > > > > > > aquariumPoint               | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de aquário. |
| > > > > > > artGalleryPoint             | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone da Galeria de arte. |
| > > > > > > campPoint                   | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de Camp. |
| > > > > > > fishingPoint                | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de pesca. |
| > > > > > > gardenPoint                 | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de jardim. |
| > > > > > > hikingPoint                 | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de caminhada. |
| > > > > > > libraryPoint                | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de biblioteca. |
| > > > > > > museumPoint                 | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de museu. |
| > > > > > > naturalPlacePoint           | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de local natural. |
| > > > > > > parkPoint                   | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de estacionamento. |
| > > > > > > restAreaPoint               | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone da área de REST. |
| > > > > > > touristAttractionPoint      | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de atração Tourist. |
| > > > > > > zooPoint                    | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de Zoo. |
| > > > > > communityPoint               | [Ponto de extremidade](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam locais de uso geral para a Comunidade. |
| > > > > > > conventionCenterPoint       | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone da central de convenções. |
| > > > > > > financialPoint              | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone financeiro. |
| > > > > > > governmentPoint             | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone do governo. |
| > > > > > > informationTechnologyPoint  | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de tecnologia da informação. |
| > > > > > > palacePoint                 | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de Palace. |
| > > > > > > pollingStationPoint         | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de estação de sondagem. |
| > > > > > > researchPoint               | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de pesquisa. |
| > > > > > educationPoint               | [Ponto de extremidade](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam escolas e outros locais relacionados à educação. |
| > > > > > entertainmentPoint           | [Ponto de extremidade](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam locais de entretenimento, como teatros, cinemas, etc. |
| > > > > > > artsPoint                   | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de artes. |
| > > > > > > bowlingPoint                | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de boliche. |
| > > > > > > casinoPoint                 | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de casino. |
| > > > > > > golfCoursePoint             | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de curso de golfe. |
| > > > > > > gymPoint                    | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de Gym. |
| > > > > > > marinaPoint                 | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de Marina. |
| > > > > > > movieTheaterPoint           | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de cinema do filme. |
| > > > > > > nightClubPoint              | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone do clube da noite. |
| > > > > > > recreationPoint             | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de recreação. |
| > > > > > > skatingPoint                | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de patinação. |
| > > > > > > skiAreaPoint                | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone da área de esqui. |
| > > > > > > stadiumPoint                | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de pool de nada. |
| > > > > > > swimmingPoolPoint           | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de pool de nada. |
| > > > > > > theaterPoint                | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de teatro. |
| > > > > > > wineryPoint                 | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone Coho. |
| > > > > > essentialServicePoint        | [Ponto de extremidade](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam serviços essenciais, como estacionamento, bancos, gás, etc. |
| > > > > > > aTMPoint                    | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de ATM. |
| > > > > > > automobileRentalPoint       | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de aluguel de automóvel. |
| > > > > > > automobileRepairPoint       | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de reparo do automóvel. |
| > > > > > > bankPoint                   | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de banco. |
| > > > > > > clinicPoint                 | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone da clínica. |
| > > > > > > electricChargingStationPoint| [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de estação de carregamento elétrica. |
| > > > > > > fireStationPoint            | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de FireStation. |
| > > > > > > gasStationPoint             | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de GasStation. |
| > > > > > > groceryPoint                | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de supermercado. |
| > > > > > > hospitalPoint               | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone do hospital. |
| > > > > > > laundryPoint                | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de lavanderia. |
| > > > > > > liquorAndWineStorePoint     | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Bebidas e ícone de loja de vinhos. |
| > > > > > > mailPoint                   | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de email. |
| > > > > > > marketPoint                 | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de mercado. |
| > > > > > > parkingPoint                | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de estacionamento. |
| > > > > > > petsPoint                   | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de animais de estimação. |
| > > > > > > pharmacyPoint               | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de farmácia. |
| > > > > > > policePoint                 | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de polícia. |
| > > > > > > postalServicePoint          | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de serviço postal. |
| > > > > > > professionalPoint           | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de serviço profissional. |
| > > > > > > toiletPoint                 | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de vaso sanitário. |
| > > > > > > veterinarianPoint           | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de veterinarian. |
| >>>>> foodPoint                    | [Ponto de extremidade](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam restaurantes, cafés, etc. |
| > > > > > > barAndGrillPoint            | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de barra e grelha. |
| > > > > > > barPoint                    | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de barra. |
| > > > > > > breweryPoint                | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de Brewery. |
| > > > > > > cafePoint                   | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de Cafe. |
| > > > > > > iceCreamShopPoint           | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de loja de sorvetes. |
| > > > > > > restaurantPoint             | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de restaurante. |
| > > > > > > teaShopPoint                | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de TeaShop. |
| > > > > > lodgingPoint                 | [Ponto de extremidade](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam Hotéis e outros negócios de alojamento. |
| > > > > > > gotelPoint                  | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de Hotel. |
| > > > > > realEstatePoint              | [Ponto de extremidade](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam empresas imobiliárias. |
| > > > > > shoppingPoint                | [Ponto de extremidade](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam Hotéis e outros negócios de alojamento. |
| > > > > > > automobileDealerPoint       | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de revendedor de automóvel. |
| > > > > > > beautyAndSpaPoint           | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de beleza e Spa. |
| > > > > > > bookstorePoint              | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de livraria. |
| > > > > > > departmentStorePoint        | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de loja do departamento. |
| > > > > > > electronicsShopPoint        | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de loja eletrônica. |
| > > > > > > golfShopPoint               | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de loja de golfe. |
| > > > > > > homeApplianceShopPoint      | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de loja de dispositivos domésticos. |
| > > > > > > mallPoint                   | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de shopping. |
| > > > > > > phoneShopPoint              | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícone de loja de telefone. |
| >>> populatedPlace                 | [Ponto de extremidade](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam o tamanho do local habitado (por exemplo: uma cidade ou vilarejo). |
| >>>> capital                       | [Ponto de extremidade](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam a capital de um lugar habitado. |
| >>>>> adminDistrictCapital         | [Ponto de extremidade](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam a capital de um estado ou província. |
| > > > > > > majorAdminDistrictCapital   | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícones que representam a capitalização principal de um Estado ou província. |
| > > > > > > minorAdminDistrictCapital   | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícones que representam a capital secundária de um Estado ou província. |
| >>>>> countryRegionCapital         | [Ponto de extremidade](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam a capital de um país ou região. |
| > > > > majorPopulatedPlace           | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícones que representam o tamanho do local principal preenchido. |
| > > > > minorPopulatedPlace           | [Ponto de extremidade](#pointstyle) |      |      |      |      |  ✔   | Ícones que representam o tamanho do local pequeno preenchido. |
| >>> roadShield                     | [Ponto de extremidade](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Sinais que representam o nome compacto de uma estrada. (Por exemplo: I-5). Use apenas os valores de paleta se você definir a propriedade **ImageFamily** da entrada de configurações com um valor *Palette* |
| >>> roadExit                       | [Ponto de extremidade](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam saídas, normalmente de uma autoestrada de acesso controlado. |
| >>> transit                        | [Ponto de extremidade](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Ícones que representam rodoviárias, estações de trem, aeroportos etc. |
| >> political                       | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Regiões políticas, como países, regiões e estados. |
| >>> countryRegion                  | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Rótulos e bordas da região do país. |
| >>> adminDistrict                  | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Admin1, Estados, províncias, etc., bordas e rótulos. |
| >>> district                       | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Admin2, municípios, etc., bordas e rótulos. |
| >> structure                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Edifícios e outras estruturas de construção semelhantes. |
| >>> building                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Escritórios. |
| >>>> educationBuilding             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Edifícios usados para educação. |
| >>>> medicalBuilding               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Edifícios usados para fins médicos, como hospitais. |
| >>>> transitBuilding               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Edifícios usados para trânsito como aeroportos. |
| >> transportation                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que fazem parte da rede de transporte (por exemplo: rodovias, trens e barcas). |
| >>> road                           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam todas as estradas. |
| >>>> controlledAccessHighway       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam rodovias de acesso controlado e grandes. |
| >>>>> highSpeedRamp                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam rampas de alta velocidade que normalmente se conectam a rodovias de acesso controlado. |
| >>>> highway                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam rodovias. |
| >>>> majorRoad                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam as principais estradas. |
| >>>> arterialRoad                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam estradas arterials. |
| >>>> street                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam ruas. |
| >>>>> ramp                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam rampas que normalmente se conectam a rodovias. |
| >>>>> unpavedStreet                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam ruas unpaved. |
| >>>> tollRoad                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam as estradas que custam dinheiro para uso. |
| >>> railway                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Linhas de estrada de ferro. |
| >>> trail                          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Percorrendo trilhas em parques ou fazendo caminhadas. |
| > > > Walkway                        | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | Walkway elevado. |
| >>> waterRoute                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Linhas de rota de barcas |
| >> water                           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Qualquer coisa que se parece com água. Isso inclui oceanos e rios. |
| >>> river                          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Rios, córregos ou outras passagens de água.  Observe que isso pode ser uma linha ou um polígono e pode se conectar a corpos de água diferentes de rios. |
| > routeMapElement                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Todas as entradas relacionadas ao roteamento. |
| >> routeLine                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Entradas relacionadas à linha de rota. |
| >>> drivingRoute                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam rotas de direcionamento. |
| >>> scenicRoute                    | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam rotas de condução de deslumbrante. |
| >>> walkingRoute                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Linhas que representam rotas de movimentação. |
| > userMapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Todas as entradas de usuário. |
| >> userBillboard                   | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | O estilo das instâncias padrão de [MapBillboard](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard). |
| >> userLine                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | O estilo das instâncias padrão de [MapPolyline](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mappolyline). |
| >> userModel3D                     | [MapElement3D](#mapelement3d) |      |  ✔   |  ✔   |  ✔   |  ✔   | O estilo das instâncias padrão de [MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d).  Isso acontece principalmente para configurar renderAsSurface. |
| >> userPoint                       | [Ponto de extremidade](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | O estilo das instâncias padrão de [MapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon). |

<a id="properties" />

## <a name="properties"></a>{1&gt;Propriedades&lt;1}

Esta seção descreve as propriedades que você pode usar para cada entrada.

<a id="version" />

### <a name="version-properties"></a>Propriedades de versão

| Propriedade                     | Tipo    | Descrição                                                                                                           |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------|
| version                      | String  | Versão de folha de estilos direcionada. Usado para aplicabilidade. "1.0" para padrão, "1." para atualizações adicionais de pequenos recursos. |

<a id="settings" />

### <a name="settings-properties"></a>Propriedades de configurações

| Propriedade                     | Tipo    | 1703 | 1709 | 1803 | 1809 | 1903 | Descrição |
|------------------------------|---------|------|------|------|------|------|-------------|
| atmosphereVisible            | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Um sinalizador que indica se o atmosfera aparece no controle 3D. |
| buildingTexturesVisible      | Bool    |      |      |  ✔   |  ✔   |  ✔   | Um sinalizador que indica se deseja ou não mostrar texturas em construções 3D simbólicas com texturas. |
| fogColor                     | Cor   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | O valor de cor ARGB do nevoeiro a distância que aparece no controle 3D. |
| glowColor                    | Cor   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | O valor de cor ARGB que pode ser aplicado para rotular o brilho e o brilho do ícone. |
| imageFamily                  | String  |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | O nome da imagem definida para usar para esse estilo. Defina esse valor como *padrão* para sinais que uso corrigido cores que são baseados na entrada do mundo real. Defina esse valor como *Palette* para sinais que usem uma paleta de cores configuráveis. |
| landColor                    | Cor   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | O valor de cor ARGB da land terra antes que algo seja desenhado naquela terra. |
| logosVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Um sinalizador que indica se os itens com uma propriedade **Organization** devem desenhar os Logotipos apropriados ou usar um ícone genérico. |
| officialColorVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Um sinalizador que indica se os itens que possuem uma propriedade de cor oficial (como linhas de trânsito na China) devem desenhar essa cor. Por exemplo, desative esse valor para um mapa em preto e branco. |
| rasterRegionsVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Um sinalizador que indica se deve ou não desenhar regiões rasterizadoras onde elas têm uma representação melhor do que os vetores (Japão e Coreia). |
| shadedReliefVisible          | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Um sinalizador que indica se é possível ou não desenhar o sombreamento de elevações no mapa. |
| shadowColor                  | Cor   |      |      |      |  ✔   |  ✔   | A cor da sombra por trás dos ícones que usam sombras. |
| spaceColor                   | Cor   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | O valor de cor ARGB para a área ao redor do mapa. |
| terrainFlat                  | Bool    |      |      |      |      |      | Um sinalizador que indica se o terreno deve ser simples (desabilitado) no mapa. |
| useDefaultImageColors        | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Um sinalizador que indica se as cores originais no SVG devem ser usadas em vez de Pesquisar a entrada da paleta em busca de cores em uma imagem. |

<a id="mapelement" />

### <a name="mapelement-properties"></a>Propriedades de MapElement

| Propriedade                     | Tipo    | 1703 | 1709 | 1803 | 1809 | 1903 | Descrição |
|------------------------------|---------|------|------|------|------|------|-------------|
| backgroundScale              | Float   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Quantidade pela qual o elemento de fundo de um ícone deve ser dimensionado.  Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| fillColor                    | Cor   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | A cor que é usada para polígonos de preenchimento, a tela de fundo de ícones de ponto e para o centro de linhas se elas tiverem se dividido. |
| fontFamily                   | String  |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| fontWeight                   | String  |      |      |      |      |  ✔   | A densidade de uma face de tipos, em termos da claridade ou da pesada dos traços. "**Light**", "**normal**", "**seminegrito**" e "**Bold**" podem ser definidos |
| iconColor                    | Cor   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | A cor do glifo mostrado no meio de um ícone de ponto. |
| iconScale                    | Float   |      |  ✔   |  ✔   |  ✔   |  ✔   | Quantidade pela qual o glifo de um ícone deve ser dimensionado.  Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| labelColor                   | Cor   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelOutlineColor            | Cor   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelScale                   | Float   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | O valor pelo qual os tamanhos de rótulo padrão são dimensionados. Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| labelVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| overwriteColor               | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Faz com que o valor alfa de **FillColor** substitua **StrokeColor** em vez de mesclar-se com ela. |
| dimensionar                        | Float   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | O valor pelo qual o tamanho inteiro do ponto é dimensionado. Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| strokeColor                  | Cor   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | A cor a ser usada para o contorno em torno de polígonos, o contorno em torno dos ícones de ponto e a cor das linhas. |
| strokeWidthScale             | Float   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | O valor pelo qual o traço de linhas é dimensionado. Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| visible                      | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |

<a id="borderedmap" />

### <a name="borderedmapelement"></a>BorderedMapElement

Esse grupo de propriedades herda do grupo de propriedades [MapElement](#mapelement).

| Propriedade                     | Tipo    | 1703 | 1709 | 1803 | 1809 | 1903 | Descrição |
|------------------------------|---------|------|------|------|------|------|-------------|
| borderOutlineColor           | Cor   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | A cor secundária a linha de revestimento da borda de um polígono preenchido. |
| borderStrokeColor            | Cor   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | A cor da linha principal da borda de um polígono preenchido. |
| borderVisible                | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| borderWidthScale             | Float   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | A quantidade pela qual o traço de bordas é dimensionado. Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |

<a id="pointstyle" />

### <a name="pointstyle-properties"></a>Propriedades de PointStyle

Esse grupo de propriedades herda do grupo de propriedades [MapElement](#mapelement).

| Propriedade                     | Tipo    | 1703 | 1709 | 1803 | 1809 | 1903 | Descrição |
|------------------------------|---------|------|------|------|------|------|-------------|
| shadowVisible                | Bool    |      |      |      |      |  ✔   | O sinalizador que indica se a sombra do ícone deve estar visível ou não |
| forma-plano de fundo             | String  |      |      |      |      |  ✔   | Forma a ser usada como plano de fundo do ícone – substituindo qualquer forma existente. |
| Ícone de forma                   | String  |      |      |      |      |  ✔   | Forma a ser usada como o glifo de primeiro plano do ícone – substituindo qualquer forma existente. |
| stemAnchorRadiusScale        | Float   |      |      |  ✔   |  ✔   |  ✔   | Quantidade pela qual o ponto de ancoramento de um tronco do ícone deve ser dimensionado.  Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| stemColor                    | Cor   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | A cor do tronco oriundo da parte inferior do ícone no modo de 3D. |
| stemHeightScale              | Float   |      |      |  ✔   |  ✔   |  ✔   | Quantidade pela qual o tamanho do tronco de um ícone deve ser dimensionado.  Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |
| stemOutlineColor             | Cor   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | A cor do contorno do tronco oriundo da parte inferior do ícone no modo de 3D. |
| stemWidthScale               | Float   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Quantidade pela qual a largura do tronco de um ícone deve ser dimensionada.  Por exemplo, use *1* para padrão e *2* para o dobro do tamanho. |

<a id="mapelement3d" />

### <a name="mapelement3d"></a>MapElement3D

Esse grupo de propriedades herda do grupo de propriedades [MapElement](#mapelement).

| Propriedade                     | Tipo    | 1703 | 1709 | 1803 | 1809 | 1903 | Descrição |
|------------------------------|---------|------|------|------|------|------|------------|
| renderAsSurface              | Bool    |      |      |  ✔   |  ✔   |  ✔   | Um sinalizador indica que um modelo 3D deve ser renderizado como uma construção, sem esmaecimento de profundidade em relação ao piso. |
