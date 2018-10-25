---
author: Xansky
description: Use este método na API de análise da Microsoft Store para obter dados de análise do Xbox Live.
title: Obter dados de análise do Xbox Live
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, análise do Xbox Live
ms.localizationpriority: medium
ms.openlocfilehash: 68bc3389276acc910272d57e9cc859a7e8ffd6c7
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5514950"
---
# <a name="get-xbox-live-analytics-data"></a>Obter dados de análise do Xbox Live

Use este método na API de análise da Microsoft Store para obter os últimos 30 dias de dados analíticos gerais para os clientes jogando seu [jogo habilitado para Xbox Live](../xbox-live/index.md), incluindo os dados de uso de acessórios do dispositivo, tipo de conexão de internet, distribuição de pontuação do jogador, estatísticas de jogo e amigos. Essas informações também estão disponíveis no [Relatório de análise do Xbox](../publish/xbox-analytics-report.md) no painel do Centro de Desenvolvimento do Windows.

> [!IMPORTANT]
> Esse método oferece suporte somente a jogos para Xbox ou que usam os serviços do Xbox Live. Esses jogos devem passar pelo [processo de aprovação de conceito](../gaming/concept-approval.md), que inclui jogos publicados por [parceiros da Microsoft](../xbox-live/developer-program-overview.md#microsoft-partners) e jogos enviados por meio do programa [ID@Xbox](../xbox-live/developer-program-overview.md#id). Esse método não oferece suporte no momento para jogos publicados pelo [Programa de Criadores do Xbox Live](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md).

Os dados de análise adicionais para jogos habilitados para Xbox Live estão disponíveis por meio dos seguintes métodos:
* [Obter dados de conquistas do Xbox Live](get-xbox-live-achievements-data.md)
* [Obter dados de integridade do Xbox Live](get-xbox-live-health-data.md)
* [Obter dados do Hub de Jogos Xbox Live](get-xbox-live-game-hub-data.md)
* [Obter dados de clube do Xbox Live](get-xbox-live-club-data.md)
* [Obter dados de multijogador do Xbox Live](get-xbox-live-multiplayer-data.md)
* [Obter dados de uso simultâneo do Xbox Live](get-xbox-live-concurrent-usage-data.md)

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  
|---------------|--------|---------------|------|
| applicationId | string | A [ID da Store](in-app-purchases-and-trials.md#store-ids) do jogo para o qual você deseja recuperar os dados de análise gerais do Xbox Live.  |  Sim  |
| metricType | string | Uma sequência que especifica o tipo de dados de análise do Xbox Live para recuperar. Para este método, especifique o valor **productvalues**.  |  Sim  |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra uma solicitação de obtenção de dados de análise gerais para os clientes jogando seu jogo habilitado para Xbox Live. Substitua o valor de *applicationId* pela ID da Store do seu jogo.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=productvalues HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

Este método retorna uma matriz *Value* que contém os objetos a seguir.

| Objeto      | Descrição                  |
|-------------|---------------------------------------------------|
| ProductData   |   Contém um objeto [DeviceProperties](#deviceproperties) e um objeto [UserProperties](#userproperties) que contêm os últimos 30 dias de dados de análise de dispositivo e usuário para o seu jogo.    |  
| XboxwideData   |  Contém um objeto [DeviceProperties](#deviceproperties) e um objeto [UserProperties](#userproperties) que contêm os últimos 30 dias de dados de análise médios de dispositivo e usuário para todos os clientes do Xbox Live, como porcentagens. Esses dados são incluídos para fins de comparação com os dados para o seu jogo.   |                                           


### <a name="deviceproperties"></a>DeviceProperties

Esse recurso contém dados de uso do dispositivo para os dados de uso médio do dispositivo ou de jogo para todos os clientes do Xbox Live durante os últimos 30 dias.

| Valor           | Tipo    | Descrição        |
|-----------------|---------|------|
|  applicationId               |    string     |  A [ID da Store](in-app-purchases-and-trials.md#store-ids) do jogo para o qual você recuperou dados de análise.   |
|  connectionTypeDistribution               |    array     |   Contém objetos que indicam quantos clientes usam uma conexão de Internet com fio contra uma conexão de Internet sem fio no Xbox. Cada objeto tem dois campos string: <ul><li>**conType**: especifica o tipo de conexão.</li><li>**deviceCount**: no objeto **ProductData**, este campo especifica o número de clientes do seu jogo que usam o tipo de conexão. No objeto **XboxwideData**, este campo especifica o percentual de todos os clientes do Xbox Live que usam o tipo de conexão.</li></ul>   |     
|  deviceCount               |   string      |  No objeto **ProductData**, este campo especifica o número de dispositivos de cliente no qual seu jogo foi reproduzido durante os últimos 30 dias. No objeto **XboxwideData**, este campo sempre será 1, indicando uma porcentagem inicial de 100% para dados para todos os clientes do Xbox Live.   |     
|  eliteControllerPresentDeviceCount               |   string      |  No objeto **ProductData**, este campo especifica o número de clientes do seu jogo que usam o Controle Xbox Elite Wireless. No objeto **XboxwideData**, esse campo especifica o percentual de todos os clientes do Xbox Live que usam o Controle Xbox Elite Wireless.  |     
|  externalDrivePresentDeviceCount               |   string      |  No objeto **ProductData**, este campo especifica o número de clientes do seu jogo que usam um disco rígido externo no Xbox. No objeto **XboxwideData**, esse campo especifica o percentual de todos os clientes do Xbox Live que usam um disco rígido externo no Xbox.  |


### <a name="userproperties"></a>UserProperties

Este recurso contém dados de usuário para os dados de uso médio ou do jogo para todos os clientes do Xbox Live durante os últimos 30 dias.

| Valor           | Tipo    | Descrição        |
|-----------------|---------|------|
|  applicationId               |    string     |   A [ID da Store](in-app-purchases-and-trials.md#store-ids) do jogo para o qual você recuperou dados de análise.  |
|  userCount               |    string     |   No objeto **ProductData**, esse campo especifica o número de clientes que reproduziram seu jogo durante os últimos 30 dias. No objeto **XboxwideData**, este campo sempre será 1, indicando uma porcentagem inicial de 100% para dados para todos os clientes do Xbox Live.   |     
|  dvrUsageCounts               |   array      |  Contém objetos que indicam quantos clientes usaram o DVR de jogos para gravar e ler um jogo. Cada objeto tem dois campos string: <ul><li>**dvrName**: especifica o recurso do DVR de jogos usado. Os valores possíveis são **gameClipUploads**, **gameClipViews**, **screenshotUploads** e **screenshotViews**.</li><li>**userCount**: no objeto **ProductData**, este campo especifica o número de clientes do seu jogo que usaram o recurso especificado do DVR de jogos. No objeto **XboxwideData**, este campo especifica o percentual de todos os clientes do Xbox Live que usaram o recurso especificado do DVR de jogos.</li></ul>   |     
|  followerCountPercentiles               |   array      |  Contém objetos que fornecem detalhes sobre o número de seguidores para os clientes. Cada objeto tem dois campos string: <ul><li>**percentage**: no momento, esse valor sempre será 50, indicando que os dados de seguidores são fornecidos como um valor médio.</li><li>**value**: no objeto **ProductData**, este campo especifica o número médio de seguidores para clientes do seu jogo. No objeto **XboxwideData**, este campo especifica o número médio de seguidores para todos os clientes do Xbox Live.</li></ul>  |   
|  friendCountPercentiles               |   array      |  Contém objetos que fornecem detalhes sobre o número de amigos para os clientes. Cada objeto tem dois campos string: <ul><li>**percentage**: no momento, esse valor sempre será 50, indicando que os dados de amigos são fornecidos como um valor médio.</li><li>**value**: no objeto **ProductData**, este campo especifica o número médio de amigos para clientes do seu jogo. No objeto **XboxwideData**, este campo especifica o número médio de amigos para todos os clientes do Xbox Live.</li></ul>  |     
|  gamerScoreRangeDistribution               |   array      |  Contém objetos que fornecem detalhes sobre a distribuição de pontuação do jogador para os clientes. Cada objeto tem dois campos string: <ul><li>**scoreRange**: o intervalo de pontuação do jogador para o qual o campo a seguir fornece dados de uso. Por exemplo, **10K-25K**.</li><li>**userCount**: no objeto **ProductData**, este campo especifica o número de clientes do seu jogo com uma pontuação do jogador no intervalo especificado para todos os jogos que foram jogados. No objeto **XboxwideData**, este campo especifica a porcentagem de todos os clientes do Xbox Live com uma pontuação do jogador no intervalo especificado para todos os jogos que foram jogados.</li></ul>  |
|  titleGamerScoreRangeDistribution               |   array      |  Contém objetos que fornecem detalhes sobre a distribuição de pontuação do jogador para seu jogo. Cada objeto tem dois campos string: <ul><li>**scoreRange**: o intervalo de pontuação do jogador para o qual o campo a seguir fornece dados de uso. Por exemplo: **100-200**.</li><li>**userCount**: no objeto **ProductData**, este campo especifica o número de clientes do seu jogo com uma pontuação do jogador no intervalo especificado para seu jogo. No objeto **XboxwideData**, este campo especifica a porcentagem de todos os clientes do Xbox Live com uma pontuação do jogador no intervalo especificado para seu jogo.</li></ul>   |
|  socialUsageCounts               |   array      |  Contém objetos que fornecem detalhes sobre o uso social para os clientes. Cada objeto tem dois campos string: <ul><li>**scName**: o tipo de uso social. Por exemplo, **gameInvites** e **textMessages**.</li><li>**userCount**: no objeto **ProductData**, este campo especifica o número de clientes do seu jogo que participaram do tipo de uso social especificado. No objeto **XboxwideData**, este campo especifica a porcentagem de todos os clientes do Xbox Live que participaram do tipo de uso social especificado.</li></ul>   |
|  streamingUsageCounts               |   array      |  Contém objetos que fornecem detalhes sobre o uso de streaming para os clientes. Cada objeto tem dois campos string: <ul><li>**stName**: o tipo de plataforma de streaming. Por exemplo, **youtubeUsage**, **twitchUsage** e **mixerUsage**.</li><li>**userCount**: no objeto **ProductData**, este campo especifica o número de clientes do seu jogo que usaram a plataforma de streaming especificada. No objeto **XboxwideData**, este campo especifica o percentual de todos os clientes do Xbox Live que usaram a plataforma de streaming especificada.</li></ul>  |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "ProductData": {
        "DeviceProperties": [
          {
            "applicationId": "9NBLGGGZ5QDR",
            "connectionTypeDistribution": [
              {
                "conType": "WIRED",
                "deviceCount": "43806"
              },
              {
                "conType": "WIRELESS",
                "deviceCount": "104035"
              }
            ],
            "deviceCount": "148063",
            "eliteControllerPresentDeviceCount": "10615",
            "externalDrivePresentDeviceCount": "46388"
          }
        ],
        "UserProperties": [
          {
            "applicationId": "9NBLGGGZ5QDR",
            "userCount": "142345",
            "dvrUsageCounts": [
              {
                "dvrName": "gameClipUploads",
                "userCount": "31264"
              },
              {
                "dvrName": "gameClipViews",
                "userCount": "52236"
              },
              {
                "dvrName": "screenshotUploads",
                "userCount": "27051"
              },
              {
                "dvrName": "screenshotViews",
                "userCount": "45640"
              }
            ],
            "followerCountPercentiles": [
              {
                "percentage": "50",
                "value": "11"
              }
            ],
            "friendCountPercentiles": [
              {
                "percentage": "50",
                "value": "11"
              }
            ],
            "gamerScoreRangeDistribution": [
              {
                "scoreRange": "10K-25K",
                "userCount": "30015"
              },
              {
                "scoreRange": "25K-50K",
                "userCount": "20495"
              },
              {
                "scoreRange": "3K-10K",
                "userCount": "32438"
              },
              {
                "scoreRange": "50K-100K",
                "userCount": "10608"
              },
              {
                "scoreRange": "<3K",
                "userCount": "45726"
              },
              {
                "scoreRange": ">100K",
                "userCount": "3063"
              }
            ],
            "titleGamerScoreRangeDistribution": [
              {
                "scoreRange": "400-600",
                "userCount": "133875"
              },
              {
                "scoreRange": "800-1000",
                "userCount": "45960"
              },
              {
                "scoreRange": "<100",
                "userCount": "269137"
              },
              {
                "scoreRange": "≥1K",
                "userCount": "11634"
              },
              {
                "scoreRange": "100-200",
                "userCount": "334471"
              },
              {
                "scoreRange": "600-800",
                "userCount": "123044"
              },
              {
                "scoreRange": "200-400",
                "userCount": "396725"
              }
            ],
            "socialUsageCounts": [
              {
                "scName": "gameInvites",
                "userCount": "82390"
              },
              {
                "scName": "textMessages",
                "userCount": "91880"
              },
              {
                "scName": "partySessionCount",
                "userCount": "68129"
              }
            ],
            "streamingUsageCounts": [
              {
                "stName": "youtubeUsage",
                "userCount": "74092"
              },
              {
                "stName": "twitchUsage",
                "userCount": "13401"
              }
              {
                "stName": "mixerUsage",
                "userCount": "22907"
              }
            ]
          }
        ]
      },
      "XboxwideData": {
        "DeviceProperties": [
          {
            "applicationId": "XBOXWIDE",
            "connectionTypeDistribution": [
              {
                "conType": "WIRED",
                "deviceCount": "0.213677732584786"
              },
              {
                "conType": "WIRELESS",
                "deviceCount": "0.786322267415214"
              }
            ],
            "deviceCount": "1",
            "eliteControllerPresentDeviceCount": "0.0476609278128012",
            "externalDrivePresentDeviceCount": "0.173747147416134"
          }
        ],
        "UserProperties": [
          {
            "applicationId": "XBOXWIDE",
            "userCount": "1",
            "dvrUsageCounts": [
              {
                "dvrName": "gameClipUploads",
                "userCount": "0.173210623993245"
              },
              {
                "dvrName": "gameClipViews",
                "userCount": "0.202104713778096"
              },
              {
                "dvrName": "screenshotUploads",
                "userCount": "0.136682414274251"
              },
              {
                "dvrName": "screenshotViews",
                "userCount": "0.158057895120314"
              }
            ],
            "followerCountPercentiles": [
              {
                "percentage": "50",
                "value": "5"
              }
            ],
            "friendCountPercentiles": [
              {
                "percentage": "50",
                "value": "5"
              }
            ],
            "gamerScoreRangeDistribution": [
              {
                "scoreRange": "10K-25K",
                "userCount": "0.134709282586519"
              },
              {
                "scoreRange": "25K-50K",
                "userCount": "0.0549468789343825"
              },
              {
                "scoreRange": "50K-100K",
                "userCount": "0.017301313342277"
              },
              {
                "scoreRange": "3K-10K",
                "userCount": "0.216512780268453"
              },
              {
                "scoreRange": "<3K",
                "userCount": "0.573515440094644"
              },
              {
                "scoreRange": ">100K",
                "userCount": "0.00301430477372488"
              }
            ],
            "titleGamerScoreRangeDistribution": [
              {
                "scoreRange": "100-200",
                "userCount": "0.178055695637076"
              },
              {
                "scoreRange": "200-400",
                "userCount": "0.173283639825241"
              },
              {
                "scoreRange": "400-600",
                "userCount": "0.0986865193958259"
              },
              {
                "scoreRange": "600-800",
                "userCount": "0.0506375775462092"
              },
              {
                "scoreRange": "800-1000",
                "userCount": "0.0232398822856435"
              },
              {
                "scoreRange": "<100",
                "userCount": "0.456443551582991"
              },
              {
                "scoreRange": "≥1K",
                "userCount": "0.0196531337270126"
              }
            ],
            "socialUsageCounts": [
              {
                "scName": "gameInvites",
                "userCount": "0.460375855738335"
              },
              {
                "scName": "textMessages",
                "userCount": "0.429256324070832"
              },
              {
                "scName": "partySessionCount",
                "userCount": "0.378446577751268"
              },
              {
                "scName": "gamehubViews",
                "userCount": "0.000197115778147329"
              }
            ],
            "streamingUsageCounts": [
              {
                "stName": "youtubeUsage",
                "userCount": "0.330320919178683"
              },
              {
                "stName": "twitchUsage",
                "userCount": "0.040666241835399"
              }
              {
                "stName": "mixerUsage",
                "userCount": "0.140193816053558"
              }
            ]
          }
        ]
      }
    }
  ],
  "@nextLink": null,
  "TotalCount": 4
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter dados de conquistas do Xbox Live](get-xbox-live-achievements-data.md)
* [Obter dados de integridade do Xbox Live](get-xbox-live-health-data.md)
* [Obter dados do hub de jogos Xbox Live](get-xbox-live-game-hub-data.md)
* [Obter dados de clube do Xbox Live](get-xbox-live-club-data.md)
* [Obter dados de multijogador do Xbox Live](get-xbox-live-multiplayer-data.md)
* [Obter dados de uso simultâneo do Xbox Live](get-xbox-live-concurrent-usage-data.md)
