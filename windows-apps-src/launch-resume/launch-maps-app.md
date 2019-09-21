---
title: Iniciar o app Mapas do Windows
description: Saiba como iniciar o app Mapas do Windows a partir de seu app.
ms.assetid: E363490A-C886-4D92-9A64-52E3C24F1D98
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3c85eaacd62de9a2efe380197ba467c5009cd0c5
ms.sourcegitcommit: f0588a086cf2499968bf03b10c6bce5f518e90cb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2019
ms.locfileid: "68757433"
---
# <a name="launch-the-windows-maps-app"></a>Iniciar o app Mapas do Windows




Saiba como iniciar o app Mapas do Windows a partir de seu app. Este tópico descreve o **BingMaps:** , **MS-drive-to:** , **MS-Oriente-to:** e **MS-Settings:** Esquemas de Uniform Resource Identifier (URI). Use esses esquemas de URI para iniciar o aplicativo Mapas do Windows para ver mapas, trajetos e resultados de pesquisa específicos ou para baixar mapas offline de Mapas do Windows no aplicativo Configurações.

**Dica** Para saber mais sobre como iniciar o aplicativo Mapas do Windows a partir de seu aplicativo, baixe a [amostra de mapas da Plataforma Universal do Windows (UWP)](https://go.microsoft.com/fwlink/p/?LinkId=619977) do [repositório de amostras universais do Windows](https://go.microsoft.com/fwlink/p/?LinkId=619979) no GitHub.

## <a name="introducing-uris"></a>Apresentando URIs

Os esquemas de URI permitem que você abra aplicativos clicando em hiperlinks (ou programaticamente, em seu aplicativo). Assim como você pode iniciar um novo email usando **mailto:** ou abrir um navegador da Web usando **http:** , é possível abrir o aplicativo Mapas do Windows usando **bingmaps:** , **ms-drive-to:** e **ms-walk-to:** .

-   O **BingMaps:** O URI fornece mapas para localizações, resultados de pesquisa, direções e tráfego.
-   O **MS-drive-to:** O URI fornece instruções de condução ativadas de acordo com o seu local atual.
-   O **MS-Walk-to:** O URI fornece instruções de movimentação ativadas por ativação do seu local atual.

Por exemplo, o URI a seguir abre o aplicativo Mapas do Windows e exibe um mapa centralizado na cidade de Nova York.

```xml
<bingmaps:?cp=40.726966~-74.006076>
```

![um mapa centralizado na cidade de Nova York.](images/mapnyc.png)

Esta é uma descrição do esquema de URI:

**BingMaps:? consulta**

Neste esquema de URI, *query* é uma série de pares de parâmetro nome/valor:

**& param1 = value1 & param2 = value2...**

Para obter uma lista completa dos parâmetros disponíveis, consulte a referência de parâmetro [bingmaps:](#bingmaps-param-reference), [ms-drive-to:](#ms-drive-to-param-reference)e [ms-walk-to:](#ms-walk-to-param-reference). Também há exemplos mais adiante neste tópico.

## <a name="launch-a-uri-from-your-app"></a>Iniciar um URI a partir de seu aplicativo


Para iniciar o aplicativo do Windows Maps do seu aplicativo, chame o método [**LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) com um **BingMaps:** , **MS-drive-to:** ou **MS-Walk-to:** URI. O exemplo a seguir inicia o mesmo URI do exemplo anterior. Para saber mais sobre como iniciar aplicativos via URI, consulte [Iniciar o aplicativo padrão para um URI](launch-default-app.md).

```cs
// Center on New York City
var uriNewYork = new Uri(@"bingmaps:?cp=40.726966~-74.006076");

// Launch the Windows Maps app
var launcherOptions = new Windows.System.LauncherOptions();
launcherOptions.TargetApplicationPackageFamilyName = "Microsoft.WindowsMaps_8wekyb3d8bbwe";
var success = await Windows.System.Launcher.LaunchUriAsync(uriNewYork, launcherOptions);
```

Neste exemplo, a classe [**LauncherOptions**](https://docs.microsoft.com/uwp/api/Windows.System.LauncherOptions) é usada para ajudar a garantir que o aplicativo Mapas do Windows seja iniciado.

## <a name="display-known-locations"></a>Exibir locais conhecidos

Há várias opções para controlar qual parte do mapa será mostrada. Você pode usar o parâmetro *cp* (ponto central) com os parâmetros *rad* (raio) ou *lvl* (nível de zoom) para mostrar um local e escolher a proximidade do zoom na exibição. Quando você usa o parâmetro *cp*, também pode especificar *hdg* (título) e *pit* (rotação) para controlar a direção do olhar. Outro método é usar o parâmetro *bb* (caixa delimitadora) para fornecer o máximo de coordenadas sul, leste, norte e oeste da área que você deseja mostrar.

Para controlar o tipo de exibição, use os parâmetros *sty* (estilo) e *ss* (Streetside). O parâmetro *sty* permite que você alterne entre os modos de exibição de estrada e vista aérea. O parâmetro *ss* coloca o mapa em um modo de exibição Streetside. Para obter mais informações sobre esses e outros parâmetros, consulte a [referência de parâmetro bingmaps:](#bingmaps-param-reference).


| URI de Exemplo                                                                 | Resultados                                                                                                                                                                                        |
|----------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bingmaps:?                                                                 | Abre o aplicativo Mapas.                                                                                                                                                                            |
| bingmaps:?cp=40.726966~-74.006076                                          | Exibe um mapa centralizado na cidade de Nova York.                                                                                                                                                    |
| bingmaps:?cp=40.726966~-74.006076&lvl=10                                   | Exibe um mapa centralizado na cidade de Nova York com um nível de zoom 10.                                                                                                                            |
| BingMaps:? BB = 39.719\_-74.52 ~ 41.71\_-73,5                                   | Exibe um mapa da cidade de Nova York, que é a área específica no argumento **bb**.                                                                                                           |
| BingMaps:? BB = 39.719\_-74.52 ~ 41.71\_-73.5 & CP = 47 ~-122                        | Exibe um mapa da cidade de Nova York, que é a área especifica no argumento de caixa delimitadora. O ponto central de Seattle especificado no argumento **cp** é ignorado porque *bb* é especificado. |
| BingMaps:? Collection = Point. 36.116584\_-115,176753\_Caesars% 20Palace & lvl = 16 | Exibe um mapa com um ponto denominado Caesars Palace (em Las Vegas) e define o nível de zoom para 16.                                                                                                 |
| BingMaps:? Collection = Point. 40.726966\_-74, 6076\_alguns% 255FBusiness        | Exibe um mapa com um ponto chamado algum\_negócio (em Las Vegas).                                                                                                                               |
| bingmaps:?cp=40.726966~-74.006076&trfc=1&sty=a                             | Exibe um mapa da cidade de Nova York com tráfego e estilo de mapa aéreo.                                                                                                                          |
| bingmaps:?cp=47.6204~-122.3491&sty=3d                                      | Mostra a exibição 3D do Space Needle.                                                                                                                                                        |
| bingmaps:?cp=47.6204~-122.3491&sty=3d&rad=200&pit=75&hdg=165               | Mostra a exibição 3D do Space Needle com um raio de 200 m, uma rotação sobre o eixo x de 75 graus e um título de 165 graus.                                                                             |
| bingmaps:?cp=47.6204~-122.3491&ss=1                                        | Mostra a exibição Streetside do Space Needle.                                                                                                                                                |


## <a name="display-search-results"></a>Exibir resultados de pesquisa

Ao pesquisar por locais usando o parâmetro *q*, recomendamos o uso de termos específicos e dos parâmetros *cp*, *bb* ou *where* para especificar um local de pesquisa. Se você não especificar um local de pesquisa e a localização atual do usuário não estiver disponível, a pesquisa poderá não retornar resultados significativos. Os resultados da pesquisa são exibidos no modo de exibição de mapa mais adequado. Para obter mais informações sobre esses e outros parâmetros, consulte a [referência de parâmetro bingmaps:](#bingmaps-param-reference).


| URI de Exemplo                                                    | Resultados                                                                            |
|---------------------------------------------------------------|------------------------------------------------------------------------------------|
| bingmaps:?q=1600%20Pennsylvania%20Ave,%20Washington,%20DC     | Exibe um mapa e pesquisa o endereço da Casa Branca em Washington, D.C. |
| bingmaps:?q=coffee&where=Seattle                              | Pesquisa por café em Seattle.                                                    |
| bingmaps:?cp=40.726966~-74.006076&where=New%20York            | Pesquisa por Nova York próximo ao ponto central especificado.                             |
| BingMaps:? BB = 39.719\_-74.52 ~ 41.71\_-73.5 & q = pizza              | Pesquisa por pizza na caixa delimitadora especificada (ou seja, na cidade de Nova York).      |

 
## <a name="display-multiple-points"></a>Exibir vários pontos


Use o parâmetro *collection* para mostrar um conjunto personalizado de pontos no mapa. Se houver mais de um ponto, será exibida uma lista de pontos. Pode haver até 25 pontos em uma coleção, e eles são listados na ordem fornecida. A coleção tem precedência sobre solicitações de pesquisa e trajetos. Para obter mais informações sobre esse e outros parâmetros, consulte a [referência de parâmetro bingmaps:](#bingmaps-param-reference).

| URI de Exemplo | Resultados                                                                                                                   |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| BingMaps:? Collection = Point. 36.116584\_-115,176753\_Caesars% 20Palace                                                                                                | Pesquisa por Caesar's Palace em Las Vegas e exibe os resultados em um mapa na melhor exibição de mapa.                         |
| BingMaps:? Collection = Point. 36.116584\_-115,176753\_Caesars% 20Palace & lvl = 16                                                                                         | Exibe um pino denominado Caesars Palace em Las Vegas e define o zoom para nível 16.                                               |
| BingMaps:? Collection = Point. 36.116584\_-115,176753\_Caesars% 20Palace ~ Point. 36.113126\_-115,175188\_o% 20Bellagio & lvl = 16 & CP = 36.114902 ~-115,176669                   | Exibe um pino denominado Caesars Palace e um pino denominado The Bellagio em Las Vegas e define o zoom para nível 16.              |
| BingMaps:? Collection = Point. 40.726966\_-74, 6076\_falso% 255FBusiness% 255Fwith% 255FUnderscore                                                                        | Exibe Nova York com uma anotação chamada empresa\_\_falsa com\_sublinhado.                                                  |
| BingMaps:? Collection = Name. Hotel% 20List ~ Point. 36.116584\_-115,176753\_Caesars% 20Palace ~ Point. 36.113126\_-115,175188\_o% 20Bellagio & lvl = 16 & CP = 36.114902 ~-115,176669 | Exibe uma lista denominada Lista de Hotéis e dois pinos para Caesars Palace e The Bellagio em Las Vegas, e define o zoom para nível 16. |

 

## <a name="display-directions-and-traffic"></a>Exibir trajeto e tráfego


Você pode exibir o trajeto entre dois pontos usando o parâmetro *rtp*. Esses pontos podem ser endereços ou coordenadas de latitude e longitude. Use o parâmetro *trfc* para mostrar informações do tráfego. Para especificar o tipo de trajeto: de carro, a pé ou trânsito, use o parâmetro *mode*. Se *mode* não for especificado, o trajeto será fornecido usando o modo de transporte preferido do usuário. Para obter mais informações sobre esses e outros parâmetros, consulte a [referência de parâmetro bingmaps:](#bingmaps-param-reference).

![exemplo de trajeto](images/windowsmapgcdirections.png)

| URI de Exemplo                                                                                                              | Resultados                                                                                                                                                         |
|-------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| BingMaps:? RTP = pos. 44.9160\_-110.4158 ~ pos. 45.0475\_-109,4187                                                             | Exibe um mapa com trajeto ponto a ponto. Como *mode* não é especificado, o trajeto será fornecido usando o modo de preferência de transporte do usuário. |
| bingmaps:?cp=43.0332~-87.9167&trfc=1                                                                                    | Exibe um mapa centralizado na cidade de Milwaukee, WI, com tráfego.                                                                                                        |
| BingMaps:? RTP = ADR. One Microsoft Way, Redmond, WA 98052 ~ pos. 39.0731\_-108,7238                                           | Exibe um mapa com o trajeto do endereço especificado para o local especificado.                                                                            |
| BingMaps:? RTP = ADR. 1% 20Microsoft% 20Way,% 20Redmond,% 20WA,% 2098052 ~ pos. 36.1223\_-111,9495\_Grand% 20Canyon% 20northern% 20rim | Exibe o trajeto de 1 Microsoft Way, Redmond, WA, 98052 até a borda norte do Grand Canyon.                                                                |
| bingmaps:?rtp=adr.Davenport, CA~adr.Yosemite Village                                                                    | Exibe um mapa com trajetos para dirigir com endereço especificado para o ponto de referência especificado.                                                                   |
| bingmaps:?rtp=adr.Mountain%20View,%20CA~adr.San%20Francisco%20International%20Airport,%20CA&mode=d                      | Exibe com trajetos de carro de Mountain View, CA para o San Francisco International Airport, CA.                                                                  |
| bingmaps:?rtp=adr.Mountain%20View,%20CA~adr.San%20Francisco%20International%20Airport,%20CA&mode=w                      | Exibe trajetos a pé de Mountain View, CA para o San Francisco International Airport, CA.                                                                  |
| bingmaps:?rtp=adr.Mountain%20View,%20CA~adr.San%20Francisco%20International%20Airport,%20CA&mode=t                      | Exibe o trajeto de transporte público de Mountain View, CA para o San Francisco International Airport, CA.                                                                  |

## <a name="display-turn-by-turn-directions"></a>Exibir trajeto curva a curva


O **MS-drive-to:** e **MS-Walk-to:** Os esquemas de URI permitem que você inicie diretamente em uma exibição ativa por ativação de uma rota. Esses esquemas de URI só podem fornecer o trajeto da localização atual do usuário. Se você precisar fornecer instruções entre pontos que não incluem o local atual do usuário, use o **BingMaps:** Esquema de URI, conforme descrito na seção anterior. Para saber mais sobre esses esquemas de URI, consulte as referências de parâmetro [ms-drive-to:](#ms-drive-to-param-reference) e [ms-walk-to:](#ms-walk-to-param-reference).

> **Importante**  Quando o **MS-drive-to:** ou **MS-Walk-to:** Os esquemas de URI são iniciados, o aplicativo Maps verificará se o dispositivo já teve uma correção de local de GPS. Em caso afirmativo, o aplicativo Mapas continuará com o trajeto curva a curva. Caso contrário, o aplicativo exibirá a visão geral da rota, conforme descrito em [Exibir trajeto e tráfego](#display-directions-and-traffic).

![um exemplo de trajeto curva a curva](images/windowsmapsappdirections.png)

| URI de Exemplo                                                                                                | Resultados                                                                                       |
|-----------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| ms-drive-to:?destination.latitude=47.680504&destination.longitude=-122.328262&destination.name=Green Lake | Exibe um mapa com um trajeto de carro curva a curva para o Green Lake a partir de sua localização atual. |
| ms-walk-to:?destination.latitude=47.680504&destination.longitude=-122.328262&destination.name=Green Lake  | Exibe um mapa com um trajeto a pé curva a curva para o Green Lake a partir de sua localização atual. |


## <a name="download-offline-maps"></a>Baixar mapas offline

As **configurações MS-Settings:** O esquema de URI permite que você inicie diretamente em uma página específica no aplicativo de configurações. Enquanto o **MS-Settings:** O esquema de URI não é iniciado no aplicativo Maps, ele permite que você inicie diretamente na página mapas offline no aplicativo Configurações e exibe uma caixa de diálogo de confirmação para baixar os mapas offline usados pelo aplicativo Maps. O esquema de URI aceita um ponto especificado por uma latitude e longitude e determina automaticamente se há mapas offline disponíveis para uma região que contém esse ponto.  Se a latitude e longitude informadas estiverem em várias regiões de download, a caixa de diálogo de confirmação permitirá que o usuário escolha qual essas regiões baixar. Se mapas offline não estiverem disponíveis para uma região que contém esse ponto, a página Mapas offline no aplicativo Configurações será exibida com uma caixa de diálogo de erro.

| URI de Exemplo  | Resultados |
|-------------|---------|
| ms-settings:maps-downloadmaps?latlong=47.6,-122.3 | Abre o aplicativo Configurações na página Mapas Offline com uma caixa de diálogo de confirmação exibida para baixar mapas para a região que contém o ponto de latitude e longitude especificado. |

<span id="bingmaps-param-reference"/>

## <a name="bingmaps-parameter-reference"></a>Referência de parâmetro bingmaps:

A sintaxe de cada parâmetro nesta tabela é mostrada com a metalinguagem Augmented Backus–Naur Form (ABNF).

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Meter</th>
<th align="left">Definição</th>
<th align="left">Exemplo e definição ABNF</th>
<th align="left">Detalhes</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><b>cp</b></p></td>
<td align="left"><p>Ponto central</p></td>
<td align="left"><p>cp = "cp=" cpval</p>
<p>cpval = degreeslat "~" degreeslon</p>
<p>degreeslat = ["-"] 1*3DIGIT ["." 1*7DIGIT]</p>
<p>degreeslon = ["-"] 1*2DIGIT ["." 1*7DIGIT]</p>
<p>Exemplo:</p>
<p>cp=40.726966~-74.006076</p></td>
<td align="left"><p>Os dois valores devem ser expressos em graus decimais e separados por um til (<b>~</b>).</p>
<p>Os valores válidos de longitude estão entre -180 e +180, inclusive.</p>
<p>Os valores válidos de latitude estão entre -90 e +90, inclusive.</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>b</b></p></td>
<td align="left"><p>Caixa delimitadora</p></td>
<td align="left"><p>bb = "bb=" southlatitude " _" westlongitude "~" northlatitude "_ " eastlongitude</p>
<p>southlatitude = degreeslat</p>
<p>northlatitude = degreeslat</p>
<p>westlongitude = degreeslon</p>
<p>eastlongitude = degreeslon</p>
<p>degreeslat = ["-"] 13DIGIT ["." 17DIGIT]</p>
<p>degreeslon = ["-"] 12DIGIT ["." 17DIGIT]</p>
<p>Exemplo:</p>
<p>bb=39.719_-74.52~41.71_-73.5</p></td>
<td align="left"><p>Uma área retangular que especifica a caixa delimitadora expressa em graus decimais, usando um til (<b>~</b>) para separar o canto inferior esquerdo do canto superior direito. A latitude e a longitude para cada um são separadas por um sublinhado (<b>_</b>).</p>
<p>Os valores válidos de longitude estão entre -180 e +180, inclusive.</p>
<p>Os valores válidos de latitude estão entre -90 e +90, inclusive.</p><p>Os parâmetros cp e lvl são ignorados quando uma caixa delimitadora é fornecida.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><b>where</b></p></td>
<td align="left"><p>Location</p></td>
<td align="left"><p>where = "where=" whereval</p>
<p>whereval = 1 *( ALPHA / DIGIT / "-" / "." / "_" / pct-encoded / "!" / "$" / "'" / "(" / ")" / "* " / "+" / "," / ";" / ":" / "@" / "/" / "?")</p>
<p>Exemplo:</p>
<p>where=1600%20Pennsylvania%20Ave,%20Washington,%20DC</p></td>
<td align="left"><p>Termo de pesquisa de um local, ponto de referência ou lugar específico.</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>perguntas</b></p></td>
<td align="left"><p>Termo da consulta</p></td>
<td align="left"><p>q = "q="</p>
<p>whereval</p>
<p>Exemplo:</p>
<p>q=mexican%20restaurants</p></td>
<td align="left"><p>Termo de pesquisa para a categoria de empresas ou negócios locais.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><b>lvl</b></p></td>
<td align="left"><p>Nível de zoom</p></td>
<td align="left"><p>lvl = "lvl =" 1<i>2DIGIT ["." 1</i>2DIGIT]</p>
<p>Exemplo:</p>
<p>lvl=10.50</p></td>
<td align="left"><p>Define o nível de zoom do modo de exibição de mapa. Os valores válidos são de 1 a 20, onde 1 é totalmente sem zoom.</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>sty</b></p></td>
<td align="left"><p>Estilo</p></td>
<td align="left"><p>sty = "sty=" ("a" / "r"/"3d")</p>
<p>Exemplo:</p>
<p>sty=a</p></td>
<td align="left"><p>Define o estilo de mapa. Os valores válidos para esse parâmetro incluem:</p>
<ul>
<li><b>a</b>: Exibir uma exibição aérea do mapa.</li>
<li><b>r</b>: Exibir uma exibição de estrada do mapa.</li>
<li><b>3D</b>: Exibir uma exibição 3D do mapa. Use em conjunto com o parâmetro <b>cp</b> e, opcionalmente, com o parâmetro <b>rad</b>.</li>
</ul>
<p>No Windows 10, os estilos de vista aérea e exibição 3D são os mesmos.</p>
<div class="alert">
<b>Observação</b>a  omissão do parâmetro <b>sty</b> produz os mesmos resultados que sty = r.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><b>Rad</b></p></td>
<td align="left"><p>Radius</p></td>
<td align="left"><p>rad = "rad=" 1*8DIGIT</p>
<p>Exemplo:</p>
<p>rad=1000</p></td>
<td align="left"><p>Uma área circular que especifica o modo de exibição de mapa desejado. O valor do raio é medido em metros.</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>poço</b></p></td>
<td align="left"><p>Rotação sobre o eixo x</p></td>
<td align="left"><p>pit = "pit=" pitch</p>
<p>Exemplo:</p>
<p>pit=60</p></td>
<td align="left"><p>Indica o ângulo no qual o mapa é exibido, onde 90 está voltado para o horizonte (máximo) e 0 está voltado diretamente para baixo (mínimo).</p><p>Os valores válidos de rotação sobre o eixo x estão entre 0 e 90, inclusive.</td>
</tr>
<tr class="odd">
<td align="left"><p><b>hdg</b></p></td>
<td align="left"><p>Direcionamento</p></td>
<td align="left"><p>hdg = "hdg=" heading</p>
<p>Exemplo:</p>
<p>hdg=180</p></td>
<td align="left"><p>Indica a direção que o mapa está tomando em graus, onde 0 ou 360 = Norte, 90 = Leste, 180 = Sul e 270 = Oeste.</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>II</b></p></td>
<td align="left"><p>Streetside</p></td>
<td align="left"><p>ss = "ss=" BIT</p>
<p>Exemplo:</p>
<p>ss=1</p></td>
<td align="left"><p>Indica as imagens em nível de rua que serão mostradas quando <code>ss=1</code>. Omitir o parâmetro <b>ss</b> produz os mesmos resultados que <code>ss=0</code>. Use em conjunto com o parâmetro <b>cp</b> para especificar o local do modo de exibição em nível de rua.</p>
<div class="alert">
<b>Observação</b>:  as imagens de nível de rua não estão disponíveis em todas as regiões.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><b>tRFC</b></p></td>
<td align="left"><p>Tráfego</p></td>
<td align="left"><p>trfc = "trfc=" BIT</p>
<p>Exemplo:</p>
<p>trfc=1</p></td>
<td align="left"><p>Especifica se as informações de trânsito estão incluídas no mapa. Omitir o parâmetro trfc produz os mesmos resultados que <code>trfc=0</code>.</p>
<div class="alert">
<b></b>Observação  os dados de tráfego não estão disponíveis em todas as regiões.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p><b>RTP</b></p></td>
<td align="left"><p>Rota</p></td>
<td align="left"><p>rtp = "rtp=" (waypoint "~" [waypoint]) / ("~" waypoint)</p>
<p>waypoint = ("pos." point ) / ("adr." whereval)</p>
<p>point = "point." pointval ["_" title]</p>
<p>pointval = degreeslat "" degreeslon</p>
<p>degreeslat = ["-"] 13DIGIT ["." 17DIGIT]</p>
<p>degreeslon = ["-"] 12DIGIT ["." 17DIGIT]</p>
<p>title = whereval</p>
<p>whereval = 1( ALPHA / DIGIT / "-" / "." / "_" / pct-encoded / "!" / "$" / "'" / "(" / ")" / "" / "+" / "," / ";" / ":" / "@" / "/" / "?")</p>


<p>Exemplos:</p>
<p>rtp=adr.Mountain%20View,%20CA~adr.SFO</p>
<p>rtp=adr.One%20Microsoft%20Way,%20Redmond,%20WA~pos.45.23423_-122.1232 _My%20Picnic%20Spot</p></td>
<td align="left"><p>Define o início e o fim de uma rota a ser desenhada no mapa, separados por um til (<b>~</b>). Cada um dos pontos intermediários é definido por uma posição usando-se a latitude, a longitude e um título opcional ou um identificador de endereço.</p>
<p>Uma rota completa contém exatamente dois pontos intermediários. Por exemplo, uma rota com dois pontos intermediários é definida por <code>rtp="A"~"B"</code>.</p>
<p>Também é aceitável especificar uma rota incompleta. Por exemplo, você pode definir apenas o início de uma rota com <code>rtp="A"~</code>. Nesse caso, a entrada de trajeto é exibida com o ponto intermediário fornecido no campo <b>De</b> e o campo <b>Para</b> tem foco.</p>
<p>Se apenas o final de uma rota for especificado, como com <code>rtp=~"B"</code>, o painel do trajeto é exibido com o ponto intermediário fornecido no campo <b>Para</b>. Se uma localização atual precisa estiver disponível, a localização atual será previamente preenchida no campo <b>De</b> com foco.</p>
<p>Nenhuma linha de rota é desenhada quando uma rota incompleta é determinada.</p>
<p>Use em conjunto com o parâmetro <b>mode</b> para especificar o modo de transporte (de carro, transporte público ou a pé). Se <b>mode</b> não for especificado, o trajeto será fornecido usando o modo de preferência de transporte do usuário.</p>
<div class="alert">
<b>Observe</b>que  um título pode ser usado para um local se o local for especificado pelo valor do parâmetro <b>pos</b> . Em vez de mostrar a latitude e a longitude, o título será exibido.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><b>mode</b></p></td>
<td align="left"><p>Modo de transporte</p></td>
<td align="left"><p>mode = "mode=" ("d" / "t" / "w")</p>
<p>Exemplo:</p>
<p>mode=d</p></td>
<td align="left"><p>Define o modo de transporte. Os valores válidos para esse parâmetro incluem:</p>
<ul>
<li><b>d</b>: Exibe a visão geral da rota para direções de direção</li>
<li><b>t</b>: Exibe a visão geral da rota para direções de trânsito</li>
<li><b>w</b>: Exibe a visão geral da rota para percorrer as direções</li>
</ul>
<p>Use em conjunto com o parâmetro <b>rtp</b> para o trajeto de transporte. Se <b>mode</b> não for especificado, o trajeto será fornecido usando o modo de preferência de transporte do usuário. Um <b>modo</b> pode ser fornecido sem nenhum parâmetro de rota para inserir entradas de trajeto para o modo em questão do local atual.</p></td>
</tr>

<tr class="even">
<td align="left"><p><b>Cole</b></p></td>
<td align="left"><p>Collection</p></td>
<td align="left"><p>collection = "collection="(name"~"/)point["~"point]</p>
<p>name = "name." whereval </p>
<p>whereval = 1( ALPHA / DIGIT / "-" / "." / "_" / pct-encoded / "!" / "$" / "'" / "(" / ")" / "" / "+" / "," / ";" / ":" / "@" / "/" / "?") </p>
<p>point = "point." pointval ["_" title] </p>
<p>pointval = degreeslat "" degreeslon </p>
<p>degreeslat = ["-"] 13DIGIT ["." 17DIGIT] </p>
<p>degreeslon = ["-"] 12DIGIT ["." 17DIGIT] </p>
<p>title = whereval</p>


<p>Exemplo:</p>
<p>collection=name.My%20Trip%20Stops~point.36.116584_-115.176753_Las%20Vegas~point.37.8268_-122.4798_Golden%20Gate%20Bridge</p></td>
<td align="left"><p>Coleção de pontos a ser adicionada ao mapa e à lista. A coleção de pontos pode ser nomeada usando o parâmetro name. Um ponto é especificado usando latitude, longitude e título opcional.</p>
<p>Separe o nome e vários pontos com tils (<b>~</b>).</p>
<p>Se o item que você especificar contiver um til, verifique se o til está codificado como <code>%7E</code>. Se não estiver acompanhada dos parâmetros de Ponto central e de Nível de zoom, a coleção fornecerá o melhor modo de exibição de mapa.</p>

<p><b>Importante</b> Se o item que você especificar contiver um sublinhado, verifique se o sublinhado está codificado duplamente como %255F.</p></td>
</tr>
</tbody>
</table>

  
<span id="ms-drive-to-param-reference"/>

## <a name="ms-drive-to-parameter-reference"></a>Referência de parâmetro ms-drive-to:


O URI para iniciar uma solicitação de trajetos de automóvel curva a curva não precisa ser codificado e tem o seguinte formato.

> **Observação**  Você não especifica o ponto de partida nesse esquema de URI. O ponto de partida é sempre presumido como sendo a localização atual. Se você precisa especificar um ponto de partida que não seja a localização atual, consulte [Exibir trajeto e tráfego](#display-directions-and-traffic).

 

| Parâmetro | Definição | Exemplo | Detalhes |
|------------|-----------|---------|---------|
| **destino. latitude** | Latitude do destino | Exemplo: destination.latitude=47.6451413797194 | A latitude do destino. Os valores válidos de latitude estão entre -90 e +90, inclusive. |
| **destino. longitude** | Longitude do destino | Exemplo: destination.longitude=-122.141964733601 | A longitude do destino. Os valores válidos de longitude estão entre -180 e +180, inclusive. |
| **destination.name** | Nome do destino | Exemplo: destination.name=Redmond, WA | O nome do destino. Você não precisa codificar o valor de **destination.name**. |

 
<span id="ms-walk-to-param-reference"/>

## <a name="ms-walk-to-parameter-reference"></a>Referência de parâmetro ms-walk-to:


O URI para iniciar uma solicitação de trajetos a pé curva a curva não precisa ser codificado e tem o seguinte formato.

> **Observação**  Você não especifica o ponto de partida nesse esquema de URI. O ponto de partida é sempre presumido como sendo a localização atual. Se você precisa especificar um ponto de partida que não seja a localização atual, consulte [Exibir trajeto e tráfego](#display-directions-and-traffic).
 

| Parâmetro | Definição | Exemplo | Detalhes |
|-----------|------------|---------|----------|
| **destino. latitude** | Latitude do destino | Exemplo: destination.latitude=47.6451413797194 | A latitude do destino. Os valores válidos de latitude estão entre -90 e +90, inclusive. |
| **destino. longitude** | Longitude do destino | Exemplo: destination.longitude=-122.141964733601 | A longitude do destino. Os valores válidos de longitude estão entre -180 e +180, inclusive. |
| **destination.name** | Nome do destino | Exemplo: destination.name=Redmond, WA | O nome do destino. Você não precisa codificar o valor de **destination.name**. |

## <a name="ms-settings-parameter-reference"></a>Referência de parâmetro ms-settings:

A sintaxe para os parâmetros específicos do aplicativo do Maps para as **configurações do MS:** O esquema de URI está definido abaixo. **Maps-downloadmaps** é especificado junto com a **MS-Settings:** URI na forma de **MS-Settings: Maps-downloadmaps?** para indicar a página de configurações de mapas offline. 

| Parâmetro | Definição | Exemplo | Detalhes |
|-----------|------------|---------|----------|
| **LatLong** | Ponto que define a região de mapa offline. | Exemplo: latlong=47.6,-122.3 | O geopoint é especificado por latitude e longitude separadas por vírgula. Os valores válidos de latitude estão entre -90 e +90, inclusive. Os valores válidos de longitude estão entre -180 e +180, inclusive. |
