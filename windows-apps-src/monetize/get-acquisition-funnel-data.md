---
description: Use este método na API de análise da Microsoft Store para obter os dados de funil de aquisição de um aplicativo durante um determinado intervalo de datas e outros filtros opcionais.
title: Obter dados de funil de aquisição do app
ms.date: 08/04/2017
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, aquisição, funil
ms.localizationpriority: medium
ms.openlocfilehash: d9ccbb081ef6f39ad795105ee2449de4d8442ab3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646191"
---
# <a name="get-app-acquisition-funnel-data"></a>Obter dados de funil de aquisição do app

Use este método na API de análise da Microsoft Store para obter os dados de funil de aquisição de um aplicativo durante um determinado intervalo de datas e outros filtros opcionais. Essas informações também estão disponíveis na [relatório de aquisições](../publish/acquisitions-report.md#acquisition-funnel) no Partner Center.

## <a name="prerequisites"></a>Pré-requisitos


Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  
|---------------|--------|---------------|------|
| applicationId | cadeia de caracteres | A [ID da Loja](in-app-purchases-and-trials.md#store-ids) do aplicativo para o qual você deseja recuperar dados de funil de aquisição. Um exemplo de ID da Loja é 9WZDNCRFJ3Q8. |  Sim  |
| startDate | date | A data de início no intervalo de datas de dados de funil de aquisição a serem recuperados. O padrão é a data atual. |  Não  |
| endDate | date | A data final no intervalo de datas de dados de funil de aquisição a serem recuperados. O padrão é a data atual. |  Não  |
| filter | cadeia de caracteres  | Uma ou mais instruções que filtram as linhas na resposta. Para saber mais, consulte a seção [campos de filtro](#filter-fields) a seguir. | Não   |

 
### <a name="filter-fields"></a>Campos de filtro

O parâmetro *filter* da solicitação contém uma ou mais instruções que filtram as linhas da resposta. Cada instrução contém um campo e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**.

Há suporte para os campos de filtro a seguir. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*.

| Campos        |  Descrição        |
|---------------|-----------------|
| campaignId | A cadeia de caracteres de ID para uma [campanha de promoção de apps personalizados](../publish/create-a-custom-app-promotion-campaign.md) associada à aquisição. |
| market | Uma cadeia de caracteres que contém o código de país ISO 3166 do mercado onde ocorreu a aquisição. |
| deviceType | Uma das seguintes cadeias de caracteres que especifica o tipo de dispositivo no qual ocorreu a aquisição:<ul><li><strong>PC</strong></li><li><strong>Telefone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Desconhecido</strong></li></ul> |
| ageGroup | Uma das cadeias de caracteres a seguir que especifica a faixa etária do usuário que concluiu a aquisição:<ul><li><strong>0 – 17</strong></li><li><strong>18 a 24</strong></li><li><strong>25 – 34</strong></li><li><strong>35 e 49</strong></li><li><strong>50 ou acima</strong></li><li><strong>Desconhecido</strong></li></ul> |
| gender | Uma das cadeias de caracteres a seguir que especifica o gênero do usuário que concluiu a aquisição:<ul><li><strong>M</strong></li><li><strong>F</strong></li><li><strong>Desconhecido</strong></li></ul> |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra várias solicitações de obtenção de dados de funil de aquisição para um app. Substitua o valor de *applicationId* pela ID da Loja de seu app.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel?applicationId=9NBLGGGZ5QDR&startDate=8/1/2016&endDate=8/31/2016&filter=market eq 'US' and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo   | Descrição                  |
|------------|--------|-------------------------------------------------------|
| Valor      | matriz  | Uma matriz de objetos que contém dados de funil de aquisição para o app. Para saber mais sobre os dados em cada objeto, consulte a seção de [valores de funil](#funnel-values) abaixo.                  |
| TotalCount | int    | O número total de objetos da matriz *Valor*.        |


### <a name="funnel-values"></a>Valores de funil

Os objetos na matriz *Valor* contêm os valores a seguir.

| Valor               | Tipo   | Descrição                           |
|---------------------|--------|-------------------------------------------|
| MetricType                | cadeia de caracteres | Uma das cadeias de caracteres a seguir que especifica o [tipo de dados de funil](../publish/acquisitions-report.md#acquisition-funnel) incluído nesse objeto:<ul><li><strong>PageView</strong></li><li><strong>Aquisição</strong></li><li><strong>Instalar</strong></li><li><strong>Uso</strong></li></ul> |
| UserCount       | cadeia de caracteres | O número de usuários que executou a etapa de funil especificada pelo valor *MetricType*.             |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "MetricType": "PageView",
      "UserCount": 100
    },
    {
      "MetricType": "Acquisition",
      "UserCount": 80
    },
    {
      "MetricType": "Install",
      "UserCount": 50
    },
    {
      "MetricType": "Usage",
      "UserCount": 10
    }
  ],
  "TotalCount": 4
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Relatório de aquisições](../publish/acquisitions-report.md)
* [Dados de análise de acesso usando os serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter aquisições de aplicativo](get-app-acquisitions.md)
