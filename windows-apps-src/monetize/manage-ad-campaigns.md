---
author: Xansky
ms.assetid: 7b07a6ca-4be1-497c-a901-0a2da3762555
description: Use este método nas promoções da API da Microsoft Store para criar, editar e obter campanhas publicitárias promocionais.
title: Gerenciar campanhas publicitárias
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, API de promoções da Microsoft Store, campanhas publicitárias
ms.localizationpriority: medium
ms.openlocfilehash: 6c86c0d5d1a10442c7addeed11cdbfc37846f337
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6972154"
---
# <a name="manage-ad-campaigns"></a>Gerenciar campanhas publicitárias

Use esses métodos na [API de promoções da Microsoft Store](run-ad-campaigns-using-windows-store-services.md) para criar, editar e obter campanhas publicitárias promocionais para seu app. Cada campanha criada usando esse método pode ser associada a apenas um app.

>**Observação**&nbsp;&nbsp;você também pode criar e gerenciar as campanhas publicitárias usando o Partner Center e campanhas que você criar programaticamente podem ser acessadas no Partner Center. Para obter mais informações sobre como gerenciar as campanhas publicitárias no Partner Center, consulte [criar uma campanha publicitária para seu aplicativo](../publish/create-an-ad-campaign-for-your-app.md).

Quando você usa esses métodos para criar ou atualizar uma campanha, normalmente também chama um ou mais dos seguintes métodos para gerenciar *linhas de entrega*, *perfis de direcionamento* e *criativos* associados à campanha. Para saber mais sobre a relação entre campanhas, linhas de entrega, perfis de direcionamento e criativos, consulte [Executar campanhas publicitárias usando serviços da Microsoft Store](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

* [Gerenciar linhas de entrega de campanhas publicitárias](manage-delivery-lines-for-ad-campaigns.md)
* [Gerenciar perfis de direcionamento de campanhas publicitárias](manage-targeting-profiles-for-ad-campaigns.md)
* [Gerenciar criativos de campanhas publicitárias](manage-creatives-for-ad-campaigns.md)

## <a name="prerequisites"></a>Pré-requisitos

Para usar esses métodos, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](run-ad-campaigns-using-windows-store-services.md#prerequisites) da API de promoções da Microsoft Store.

  >**Observação**&nbsp;&nbsp;como parte dos pré-requisitos, certifique-se que você [criar pelo menos uma campanha publicitária no Partner Center](../publish/create-an-ad-campaign-for-your-app.md) e que você adicionar pelo menos um método de pagamento para a campanha publicitária no Partner Center. Linhas de entrega de campanhas publicitárias criadas usando essa API cobram automaticamente o instrumento de pagamento padrão escolhido na página de **campanhas publicitárias** no Partner Center.

* [Obtenha um token de acesso do Azure AD](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usar no cabeçalho da solicitação desses métodos. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.


## <a name="request"></a>Solicitação

Esses métodos têm os seguintes URIs.

| Tipo de método | URI da solicitação                                                      |  Descrição  |
|--------|------------------------------------------------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign``` |  Cria uma nova campanha publicitária.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/{campaignId}``` |  Edita a campanha publicitária especificada pelo *campaignId*.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/{campaignId}``` |  Obtém a campanha publicitária especificada pelo *campaignId*.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign``` |  Consultas as campanhas publicitárias. Consulte a seção [Parâmetros](#parameters) para obter os parâmetros de consulta suportados.  |


### <a name="header"></a>Cabeçalho

| Cabeçalho        | Tipo   | Descrição         |
|---------------|--------|---------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |
| ID de rastreamento   | GUID   | Opcional. Uma ID que rastreia o fluxo de chamada.                                  |


<span id="parameters"/> 

### <a name="parameters"></a>Parâmetros

O método GET de consulta para campanhas publicitárias suporta os seguintes parâmetros de consulta opcionais.

| Nome        | Tipo   |  Descrição      |    
|-------------|--------|---------------|------|
| skip  |  int   | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer conjuntos de dados. Por exemplo, fetch=10 e skip=0 recuperam as primeiras 10 linhas de dados, top=10 e skip=10 recuperam as próximas 10 linhas de dados e assim por diante.    |       
| fetch  |  int   | O número de linhas de dados a serem retornadas na solicitação.    |       
| campaignSetSortColumn  |  cadeia   | Organiza os objetos [Campanha](#campaign) no corpo da resposta de acordo com o campo especificado. A sintaxe é <em>CampaignSetSortColumn=campo</em>, onde o parâmetro <em>campo</em> pode ser uma das seguintes cadeias de caracteres:</p><ul><li><strong>id</strong></li><li><strong>createdDateTime</strong></li></ul><p>O padrão é **createdDateTime**.     |     
| isDescending  |  Booliano   | Classifica os objetos [Campanha](#campaign) no corpo da resposta em ordem crescente ou decrescente.   |         
| storeProductId  |  cadeia   | Use esse valor para retornar apenas as campanhas publicitárias associadas ao app com a [ID da loja](in-app-purchases-and-trials.md#store-ids) especificada. Um exemplo de ID da loja para um produto é 9nblggh42cfd.   |         
| rótulo  |  cadeia   | Use esse valor para retornar apenas as campanhas publicitárias que incluem o *rótulo* especificado no objeto [Campanha](#campaign).    |       |    


### <a name="request-body"></a>Corpo da solicitação

Os métodos POST e PUT exigem um corpo da solicitação JSON com os campos obrigatórios de um objeto [Campanha](#campaign) e quaisquer campos adicionais que você deseja definir ou alterar.


### <a name="request-examples"></a>Exemplos de solicitação

O exemplo a seguir demonstra como chamar o método POST para criar uma campanha publicitária.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign",
    "storeProductId": "9nblggh42cfd",
    "configuredStatus": "Active",
    "objective": "DriveInstalls",
    "type": "Community"
}
```

O exemplo a seguir demonstra como chamar o método GET para recuperar uma campanha publicitária específica.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/31043481  HTTP/1.1
Authorization: Bearer <your access token>
```

O exemplo a seguir demonstra como chamar o método GET para consultar um conjunto de campanhas publicitárias, classificadas por data de criação.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign?storeProductId=9nblggh42cfd&fetch=100&skip=0&campaignSetSortColumn=createdDateTime HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>Resposta

Esses métodos retornam um corpo de resposta JSON com um ou mais objetos [Campanha](#campaign), dependendo do método chamado. O exemplo a seguir demonstra um corpo de resposta para o método GET de uma campanha específica.

```json
{
    "Data": {
        "id": 31043481,
        "name": "Contoso App Campaign",
        "createdDate": "2017-01-17T10:12:15Z",
        "storeProductId": "9nblggh42cfd",
        "configuredStatus": "Active",
        "effectiveStatus": "Active",
        "effectiveStatusReasons": [
            "{\"ValidationStatusReasons\":null}"
        ],
        "labels": [],
        "objective": "DriveInstalls",
        "type": "Paid",
        "lines": [
            {
                "id": 31043476,
                "name": "Contoso App Campaign - Paid Line"
            }
        ]
    }
}
```


<span id="campaign"/>

## <a name="campaign-object"></a>Objeto da campanha

O corpo da solicitação e resposta desses métodos contêm os campos a seguir. Essa tabela mostra quais campos são somente de leitura (ou seja, eles não podem ser alterados no método PUT) e quais são necessários no corpo da solicitação para o método POST.

| Campo        | Tipo   |  Descrição      |  Somente leitura  | Padrão  | Obrigatório para POST |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  número inteiro   |  A ID da campanha publicitária.     |   Sim    |      |  Não     |       
|  nome   |  string   |   O nome da campanha publicitária.    |    Não   |      |  Sim     |       
|  configuredStatus   |  cadeia   |  Um dos seguintes valores que especifica o status da campanha publicitária especificada pelo desenvolvedor: <ul><li>**Ativo**</li><li>**Inactive**</li></ul>     |  Não     |  Ativo    |   Sim    |       
|  effectiveStatus   |  cadeia   |   Um dos seguintes valores que especifica o status eficaz da campanha publicitária com base na validação do sistema: <ul><li>**Ativo**</li><li>**Inactive**</li><li>**Processando**</li></ul>    |    Sim   |      |   Não      |       
|  effectiveStatusReasons   |  matriz   |  Um ou mais dos seguintes valores que especificam o motivo para o status da campanha publicitária eficaz: <ul><li>**AdCreativesInactive**</li><li>**BillingFailed**</li><li>**AdLinesInactive**</li><li>**ValidationFailed**</li><li>**Falha**</li></ul>      |  Sim     |     |    Não     |       
|  storeProductId   |  cadeia   |  A [ID da loja](in-app-purchases-and-trials.md#store-ids) do app ao qual a campanha publicitária está associada. Um exemplo de ID da loja para um produto é 9nblggh42cfd.     |   Sim    |      |  Sim     |       
|  rótulos   |  matriz   |   Uma ou mais cadeias de caracteres que representam rótulos personalizados para a campanha. Esses rótulos ser usado para pesquisa e marcação de campanhas.    |   Não    |  null    |    Não     |       
|  tipo   | cadeia    |  Um dos seguintes valores que especifica o tipo de campanha: <ul><li>**Pagos**</li><li>**Doméstica**</li><li>**Comunidade**</li></ul>      |   Sim    |      |   Sim    |       
|  objetivo   |  cadeia   |  Um dos seguintes valores que especifica o objetivo da campanha: <ul><li>**DriveInstall**</li><li>**DriveReengagement**</li><li>**DriveInAppPurchase**</li></ul>     |   Não    |  DriveInstall    |   Sim    |       
|  linhas   |  matriz   |   Um ou mais objetos que identificam as [linhas de entrega](manage-delivery-lines-for-ad-campaigns.md#line) associadas à campanha publicitária. Cada objeto nesse campo consiste em um campo de *id* e *nome* que especifica a ID e o nome da linha de entrega.     |   Não    |      |    Não     |       
|  createdDate   |  cadeia   |  A data e hora em que a campanha publicitária foi criada, no formato ISO 8601.     |  Sim     |      |     Não    |       |


## <a name="related-topics"></a>Tópicos relacionados

* [Veicular campanhas publicitárias usando os serviços da Microsoft Store](run-ad-campaigns-using-windows-store-services.md)
* [Gerenciar linhas de entrega de campanhas publicitárias](manage-delivery-lines-for-ad-campaigns.md)
* [Gerenciar perfis de direcionamento de campanhas publicitárias](manage-targeting-profiles-for-ad-campaigns.md)
* [Gerenciar criativos de campanhas publicitárias](manage-creatives-for-ad-campaigns.md)
* [Obter dados de desempenho da campanha publicitária](get-ad-campaign-performance-data.md)
