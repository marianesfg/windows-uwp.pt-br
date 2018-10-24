---
author: Xansky
description: Use este método na API de análise da Microsoft Store para obter os dados de funil de aquisição de um aplicativo durante um determinado intervalo de datas e outros filtros opcionais.
title: Obter dados de funil de aquisição do app
ms.author: mhopkins
ms.date: 08/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, aquisição, funil
ms.localizationpriority: medium
ms.openlocfilehash: 362bcc956fa5945f9685aac7d6351b9fda7690de
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/23/2018
ms.locfileid: "5443132"
---
# <a name="get-app-acquisition-funnel-data"></a>Obter dados de funil de aquisição do app

Use este método na API de análise da Microsoft Store para obter os dados de funil de aquisição de um aplicativo durante um determinado intervalo de datas e outros filtros opcionais. Essas informações também estão disponíveis no [Relatório de aquisições](../publish/acquisitions-report.md#acquisition-funnel) no painel do Centro de Desenvolvimento do Windows.

## <a name="prerequisites"></a>Pré-requisitos


Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  
|---------------|--------|---------------|------|
| applicationId | string | A [ID da Loja](in-app-purchases-and-trials.md#store-ids) do aplicativo para o qual você deseja recuperar dados de funil de aquisição. Uma ID da Loja de exemplo é 9WZDNCRFJ3Q8. |  Sim  |
| startDate | date | A data de início no intervalo de datas de dados de funil de aquisição a serem recuperados. O padrão é a data atual. |  Não  |
| endDate | date | A data final no intervalo de datas de dados de funil de aquisição a serem recuperados. O padrão é a data atual. |  Não  |
| filter | string  | Uma ou mais instruções que filtram as linhas na resposta. Para saber mais, consulte a seção [campos de filtro](#filter-fields) a seguir. | Não   |

 
### <a name="filter-fields"></a>Campos de filtro

O parâmetro *filter* da solicitação contém uma ou mais instruções que filtram as linhas da resposta. Cada instrução contém um campo e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**.

Há suporte para os campos de filtro a seguir. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*.

| Campos        |  Descrição        |
|---------------|-----------------|
| campaignId | A cadeia de caracteres de ID para uma [campanha de promoção de apps personalizados](../publish/create-a-custom-app-promotion-campaign.md) associada à aquisição. |
| market | Uma cadeia de caracteres que contém o código de país ISO 3166 do mercado onde ocorreu a aquisição. |
| deviceType | Uma das seguintes cadeias de caracteres que especifica o tipo de dispositivo no qual ocorreu a aquisição:<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Desconhecido</strong></li></ul> |
| ageGroup | Uma das cadeias de caracteres a seguir que especifica a faixa etária do usuário que concluiu a aquisição:<ul><li><strong>0 a 17</strong></li><li><strong>18 a 24</strong></li><li><strong>25 a 34</strong></li><li><strong>35 a 49</strong></li><li><strong>50 ou mais</strong></li><li><strong>Desconhecido</strong></li></ul> |
| gender | Uma das cadeias de caracteres a seguir que especifica o gênero do usuário que concluiu a aquisição:<ul><li><strong>M</strong></li><li><strong>F</strong></li><li><strong>Desconhecido</strong></li></ul> |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra várias solicitações de obtenção de dados de funil de aquisição para um app. Substitua o valor de *applicationId* pela ID da Loja do seu app.

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
| Valor      | array  | Uma matriz de objetos que contém dados de funil de aquisição para o app. Para saber mais sobre os dados em cada objeto, consulte a seção de [valores de funil](#funnel-values) abaixo.                  |
| TotalCount | int    | O número total de objetos da matriz *Valor*.        |


### <a name="funnel-values"></a>Valores de funil

Os objetos na matriz *Valor* contêm os valores a seguir.

| Valor               | Tipo   | Descrição                           |
|---------------------|--------|-------------------------------------------|
| MetricType                | string | Uma das cadeias de caracteres a seguir que especifica o [tipo de dados de funil](../publish/acquisitions-report.md#acquisition-funnel) incluído nesse objeto:<ul><li><strong>PageView</strong></li><li><strong>Aquisição</strong></li><li><strong>Instalar</strong></li><li><strong>Utilização</strong></li></ul> |
| UserCount       | string | O número de usuários que executou a etapa de funil especificada pelo valor *MetricType*.             |


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
* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter aquisições de app](get-app-acquisitions.md)
