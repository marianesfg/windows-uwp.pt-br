---
title: Visão geral de mapas e local
description: Esta seção explica como é possível exibir mapas, usar serviços de mapa, encontrar o local e configurar uma cerca geográfica no aplicativo. Esta seção também mostra como iniciar o aplicativo Mapas do Windows em um mapa, rota ou conjunto de trajetos passo a passo específico.
ms.assetid: F4C1F094-CF46-4B15-9D80-C1A26A314521
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, mapa, local, serviços de mapa
ms.localizationpriority: medium
ms.openlocfilehash: 7f39e365cd15a10f775a89cf32747b4bf0bbd87a
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75685060"
---
# <a name="maps-and-location-overview"></a>Visão geral de mapas e local




Esta seção explica como é possível exibir mapas, usar serviços de mapa, encontrar o local e configurar uma cerca geográfica no aplicativo. Esta seção também mostra como iniciar o aplicativo Mapas do Windows em um mapa, rota ou conjunto de trajetos passo a passo específico.

> [!TIP]
> Para saber mais sobre como usar mapas e local no aplicativo, baixe as seguintes amostras do [repositório Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples) no GitHub:
-   [Amostra de mapa da UWP (Plataforma Universal do Windows)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
-   [Amostra de geolocalização da UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)

 

## <a name="display-maps"></a>Exibir mapas


Exiba mapas com modos de exibição 2D, 3D ou Streetside no seu aplicativo usando APIs do namespace [**Windows.UI.Xaml.Controls.Maps**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps). Você pode marcar POIs (pontos de interesse) no mapa usando pinos, imagens, formas ou elementos de interface do usuário XAML. Também é possível sobrepor imagens lado a lado ou substituir completamente as imagens do mapa.

| Tópico | Descrição |
|-------|-------------|
| [Solicitar uma chave de autenticação de mapas](authentication-key.md) | O aplicativo precisa ser autenticado para que possa usar o [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) e os serviços de mapa no namespace [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps). Para autenticar o aplicativo, você precisa especificar uma chave de autenticação de mapas. Este artigo descreve como solicitar uma chave de autenticação de mapas na [Central de desenvolvedores do Bing Mapas](https://www.bingmapsportal.com/) e adicioná-la ao aplicativo. |
| [Exibir mapas com modos de exibição 2D, 3D e Streetside](display-maps.md) | Exiba mapas personalizáveis no seu aplicativo usando a classe [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl). Este tópico também apresenta modos de exibição 3D e Streetside. |
| [Exibir POIs (pontos de interesse) em um mapa](display-poi.md) | Adicione POIs (pontos de interesse) a um mapa usando pinos, imagens, formas e elementos de interface do usuário XAML. |
| [Sobrepor imagens lado a lado em um mapa](overlay-tiled-images.md) | Sobreponha imagens em blocos de terceiros ou personalizados em um mapa usando fontes de blocos. Use fontes de blocos para sobrepor informações especializadas, como dados de previsão do tempo, dados de população ou dados sísmicos; ou use fontes de blocos para substituir por completo o mapa padrão. |



## <a name="access-map-services"></a>Serviços de mapa de acesso

Adicione rotas, trajetos e funcionalidades de geocódigo ao aplicativo usando APIs do namespace [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps).

| Tópico | Descrição |
|-----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Solicitar uma chave de autenticação de mapas](authentication-key.md) | O aplicativo precisa ser autenticado para que possa usar o [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) e os serviços de mapa no namespace [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps). Para autenticar o aplicativo, você precisa especificar uma chave de autenticação de mapas. Este artigo descreve como solicitar uma chave de autenticação de mapas na [Central de desenvolvedores do Bing Mapas](https://www.bingmapsportal.com/) e adicioná-la ao aplicativo. |
| [Exibir POIs (pontos de interesse) em um mapa](display-poi.md) | Adicione POIs (pontos de interesse) a um mapa usando pinos, imagens, formas e elementos de interface do usuário XAML. |
| [Exibir rotas e trajeto](routes-and-directions.md) | Solicite rotas e trajetos e exiba-os no aplicativo. |
| [Executar geocodificação e geocodificação reversa](geocoding.md) | Converta endereços em localizações geográficas (geocódigo) e converta localizações geográficas em endereços (geocódigo reverso) chamando os métodos da classe [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) no namespace [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps). |
| [Localize e baixe pacotes de mapas para uso offline](https://docs.microsoft.com/uwp/api/windows.services.maps.offlinemaps)| No passado, o aplicativo precisava direcionar os usuários para o aplicativo de configurações para baixar Mapas offline. Agora, você pode usar classes no namespace [Windows.Services.Maps.OfflineMaps](https://docs.microsoft.com/uwp/api/windows.services.maps.offlinemaps) para localizar os pacotes baixados em uma determinada área (com base em [Geopoint](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geopoint), [GeoboundingBox](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geoboundingbox) etc.). <br> Você pode também verificar e escutar o status de download de pacotes de mapa, bem como iniciar um download sem exigir que o usuário saia do aplicativo. <br> Você encontrará exemplos de como fazer isso no conteúdo de referência e a [amostra de mapa da UWP (Plataforma Universal do Windows)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl).

## <a name="get-the-users-location"></a>Obter a localização do usuário

Obtenha o local atual do usuário e seja notificado quando o local mudar no aplicativo usando APIs do namespace [**Windows.Devices.Geolocation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation). Esses membros da API também são usados com frequência em parâmetros das APIs de mapas. As APIs do namespace [**Windows.Devices.Geolocation.Geofencing**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geofencing) notificam o aplicativo quando o usuário insere ou sai de uma cerca geográfica (uma área geográfica predefinida).

| Tópico | Descrição |
|-------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Solicitar uma chave de autenticação de mapas](authentication-key.md) | O aplicativo precisa ser autenticado para que possa usar o [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) e os serviços de mapa no namespace [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps). Para autenticar o aplicativo, você precisa especificar uma chave de autenticação de mapas. Este artigo descreve como solicitar uma chave de autenticação de mapas na [Central de desenvolvedores do Bing Mapas](https://www.bingmapsportal.com/) e adicioná-la ao aplicativo. |
| [Diretrizes de design para aplicativos com detecção de localização](guidelines-and-checklist-for-detecting-location.md) | Diretrizes de desempenho para aplicativos que exigirem acesso à localização de um usuário. |
| [Obter a localização do usuário](get-location.md) | Obtenha acesso à localização do usuário e recupere-o. | 
| [Diretrizes para usar o acompanhamento de visitas](guidelines-for-visits.md) | Saiba como usar o recurso de acompanhamento de visitas potente para acompanhamento de localização mais prático. |
| [Diretrizes de design para cerca geográfica](guidelines-for-geofencing.md) | Diretrizes de desempenho para aplicativos que utilizam o recurso cerca geográfica. |
| [Configurar uma cerca geográfica](set-up-a-geofence.md) | Configure uma cerca geográfica no aplicativo e saiba como manipular notificações em primeiro e segundo planos. |

## <a name="launch-the-windows-maps-app"></a>Iniciar o aplicativo Mapas do Windows

O aplicativo pode iniciar o aplicativo Mapas do Windows conforme mostrado aqui para exibir mapas específicos e trajetos curva a curva. Em vez de oferecer funcionalidade de mapa diretamente no próprio aplicativo, leve em consideração usar o aplicativo Mapas do Windows para fornecer essa funcionalidade. Para saber mais, consulte [Iniciar o aplicativo Mapas do Windows](https://docs.microsoft.com/windows/uwp/launch-resume/launch-maps-app).

![um exemplo do aplicativo Mapas do Windows.](images/mapnyc.png)

## <a name="related-topics"></a>Tópicos relacionados

* [Amostra de mapa UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [Amostra de geolocalização da UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
* [Central de Desenvolvedores do Bing Mapas](https://www.bingmapsportal.com/)
* [Obter a localização atual](get-location.md)
* [Diretrizes de design para aplicativos com detecção de localização](guidelines-and-checklist-for-detecting-location.md)
* [Diretrizes de design para mapas](controls-map.md)
* [Diretrizes de design para aplicativos com detecção de privacidade](https://docs.microsoft.com/windows/uwp/security/index)
* [Vídeo da Build 2015: Aproveitando mapas e localização em telefones, tablets e computadores em seus aplicativos do Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Exemplo de aplicativo de tráfego UWP](https://github.com/Microsoft/Windows-appsample-trafficapp)
