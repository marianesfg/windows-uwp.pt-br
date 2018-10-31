---
author: normesta
title: Visão geral de mapas e local
description: Esta seção explica como é possível exibir mapas, usar serviços de mapa, encontrar o local e configurar uma cerca geográfica no aplicativo. Esta seção também mostra como iniciar o app Mapas do Windows em um mapa específico, rota ou um conjunto de trajetos passo a passo.
ms.assetid: F4C1F094-CF46-4B15-9D80-C1A26A314521
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, mapa, local, serviços de mapa
ms.localizationpriority: medium
ms.openlocfilehash: 17d123b440b6ec7892c84a9a6bca9177799ad0fb
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5827659"
---
# <a name="maps-and-location-overview"></a>Visão geral de mapas e local




Esta seção explica como é possível exibir mapas, usar serviços de mapa, encontrar o local e configurar uma cerca geográfica no aplicativo. Esta seção também mostra como iniciar o aplicativo Mapas do Windows em um mapa específico, rota ou um conjunto de trajetos passo a passo.

> [!TIP]
> Para saber mais sobre como usar mapas e localização em seu aplicativo, baixe os exemplos a seguir do [repositório Windows-universal-samples](http://go.microsoft.com/fwlink/p/?LinkId=619979) no GitHub:
-   [Amostra de mapa da UWP (Plataforma Universal do Windows)](http://go.microsoft.com/fwlink/p/?LinkId=619977)
-   [Amostra de geolocalização da UWP](http://go.microsoft.com/fwlink/p/?linkid=533278)

 

## <a name="display-maps"></a>Exibir mapas


Exiba mapas com modos de exibição 2D, 3D ou Streetside no seu aplicativo usando APIs do namespace [**Windows.UI.Xaml.Controls.Maps**](https://msdn.microsoft.com/library/windows/apps/dn610751). Você pode marcar pontos de interesse (POI) no mapa usando pinos, imagens, formas ou elementos de interface do usuário XAML. Também é possível sobrepor imagens lado a lado ou substituir as imagens do mapa juntas.

| Tópico | Descrição |
|-------|-------------|
| [Solicitar uma chave de autenticação de mapas](authentication-key.md) | O aplicativo precisa ser autenticado para poder usar o [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) e os serviços de mapa no namespace [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979). Para autenticar o aplicativo, você deve especificar uma chave de autenticação de mapas. Este artigo descreve como solicitar uma chave de autenticação de mapas na [central de desenvolvedores do Bing Mapas](https://www.bingmapsportal.com/) e adicioná-la ao aplicativo. |
| [Exibir mapas com modos de exibição 2D, 3D e Streetside](display-maps.md) | Exiba mapas personalizáveis no seu aplicativo usando a classe [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004). Este tópico também apresenta modos de exibição 3D e Streetside. |
| [Exibir pontos de interesse (POI) em um mapa](display-poi.md) | Adicione pontos de interesse (POI) a um mapa usando pinos, imagens, formas e elementos de interface do usuário XAML. |
| [Sobrepor imagens lado a lado em um mapa](overlay-tiled-images.md) | Sobreponha imagens em blocos de terceiros ou personalizados em um mapa usando fontes de blocos. Use fontes de blocos para sobrepor informações especializadas, como dados de previsão do tempo, dados de população ou dados sísmicos; ou use fontes de blocos para substituir por completo o mapa padrão. |



## <a name="access-map-services"></a>Serviços de mapa de acesso

Adicione rotas, trajeto e funcionalidades de geocódigo ao aplicativo usando APIs do namespace [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979).

| Tópico | Descrição |
|-----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Solicitar uma chave de autenticação de mapas](authentication-key.md) | O aplicativo precisa ser autenticado para poder usar o [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) e os serviços de mapa no namespace [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979). Para autenticar o aplicativo, você deve especificar uma chave de autenticação de mapas. Este artigo descreve como solicitar uma chave de autenticação de mapas na [central de desenvolvedores do Bing Mapas](https://www.bingmapsportal.com/) e adicioná-la ao aplicativo. |
| [Exibir pontos de interesse (POI) em um mapa](display-poi.md) | Adicione pontos de interesse (POI) a um mapa usando pinos, imagens, formas e elementos de interface do usuário XAML. |
| [Exibir rotas e trajeto](routes-and-directions.md) | Solicite rotas e trajeto e os exiba no aplicativo. |
| [Executar geocodificação e geocodificação reversa](geocoding.md) | Converta endereços em localizações geográficas (geocódigo) e converta localizações geográficas em endereços (geocódigo reverso) chamando os métodos da classe [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) no namespace [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979). |
| [Localizar e baixar pacotes de mapas para uso offline](https://docs.microsoft.com/uwp/api/windows.services.maps.offlinemaps)| No passado, o aplicativo tinha que direcionar os usuários para o aplicativo configurações para baixar mapas offline. Agora, você pode usar classes no namespace [offlinemaps](https://docs.microsoft.com/en-us/uwp/api/windows.services.maps.offlinemaps) para encontrar os pacotes baixados em uma determinada área (com base em um [Geopoint](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geopoint), [GeoboundingBox](https://docs.microsoft.com/en-us/uwp/api/windows.devices.geolocation.geoboundingbox), etc.). <br> Você pode também verificar e escutar o status baixado de pacotes de mapas, bem como iniciar um download sem exigir que o usuário deixar seu aplicativo. <br> Você encontrará exemplos de como fazer isso no conteúdo de referência e a [amostra de mapa da plataforma Universal do Windows (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977).

## <a name="get-the-users-location"></a>Obter a localização do usuário

Obtenha o local atual e seja notificado quando o local mudar no aplicativo usando APIs do namespace [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603). Esses membros da API também são usados com frequência em parâmetros das APIs de mapas. As APIs do namespace [**Windows.Devices.Geolocation.Geofencing**](https://msdn.microsoft.com/library/windows/apps/dn263744) notificam o aplicativo quando o usuário insere ou sai de uma cerca geográfica (uma área geográfica predefinida).

| Tópico | Descrição |
|-------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Solicitar uma chave de autenticação de mapas](authentication-key.md) | O aplicativo precisa ser autenticado para poder usar o [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) e os serviços de mapa no namespace [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979). Para autenticar o aplicativo, você deve especificar uma chave de autenticação de mapas. Este artigo descreve como solicitar uma chave de autenticação de mapas na [central de desenvolvedores do Bing Mapas](https://www.bingmapsportal.com/) e adicioná-la ao aplicativo. |
| [Diretrizes de design para aplicativos com detecção de localização](guidelines-and-checklist-for-detecting-location.md) | Diretrizes de desempenho para aplicativos que exigirem acesso à localização de um usuário. |
| [Obter a localização do usuário](get-location.md) | Obtenha acesso à localização do usuário e recupere-o. | 
| [Diretrizes para usar o rastreamento de visitas](guidelines-for-visits.md) | Saiba como usar o recurso de rastreamento de visitas potente para rastreamento de localização mais prático. |
| [Diretrizes de design para delimitação geográfica](guidelines-for-geofencing.md) | Diretrizes de desempenho para aplicativos que utilizam o recurso cerca geográfica. |
| [Configurar uma cerca geográfica](set-up-a-geofence.md) | Configure uma cerca geográfica no aplicativo e saiba como manipular notificações em primeiro e segundo planos. |

## <a name="launch-the-windows-maps-app"></a>Iniciar o aplicativo Mapas do Windows

O aplicativo pode iniciar o aplicativo Mapas do Windows conforme mostrado aqui para exibir mapas específicos e trajetos curva a curva. Em vez de oferecer funcionalidade de mapa diretamente no próprio aplicativo, leve em consideração usar o aplicativo Mapas do Windows para fornecer essa funcionalidade. Para obter mais informações, consulte [Iniciar o aplicativo Mapas do Windows](https://msdn.microsoft.com/library/windows/apps/mt228341).

![um exemplo do aplicativo Mapas do Windows.](images/mapnyc.png)

## <a name="related-topics"></a>Tópicos relacionados

* [Amostra de mapa UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Amostra de geolocalização da UWP](http://go.microsoft.com/fwlink/p/?linkid=533278)
* [Central de desenvolvedores do Bing Mapas](https://www.bingmapsportal.com/)
* [Obter a localização atual](get-location.md)
* [Diretrizes de design para aplicativos com detecção de localização](guidelines-and-checklist-for-detecting-location.md)
* [Diretrizes de design para mapas](controls-map.md)
* [Diretrizes de design para aplicativos com detecção de privacidade](https://msdn.microsoft.com/library/windows/apps/hh768223)
* [Vídeo de compilação 2015: aproveitando mapas e localização em telefone, Tablet e computador em seus aplicativos do Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Exemplo do aplicativo de tráfego UWP](http://go.microsoft.com/fwlink/p/?LinkId=619982)
