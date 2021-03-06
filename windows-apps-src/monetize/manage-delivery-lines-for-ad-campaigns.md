---
ms.assetid: dc632a4c-ce48-400b-8e6e-1dddbd13afff
description: Use este método na API de promoções da Microsoft Store para gerenciar linhas de entrega de campanhas publicitárias promocionais.
title: Gerenciar linhas de entrega
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, API de promoções da Microsoft Store, campanhas publicitárias
ms.localizationpriority: medium
ms.openlocfilehash: 363f7034d7e353d9ee110637971e7b848dbca1bb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57625631"
---
# <a name="manage-delivery-lines"></a>Gerenciar linhas de entrega

Use esses métodos na API de promoções da Microsoft Store a fim de criar uma ou mais *linhas de entrega* para comprar inventário e disponibilizar seus anúncios de uma campanha publicitária promocional. Para cada linha de entrega, você pode definir direcionamento, o preço da oferta e decidir quanto deseja gastar ao definir um orçamento e vincular aos criativos que deseja usar.

Para saber mais sobre a relação entre linhas de entrega e campanhas publicitárias, perfis de direcionamento e criativos, consulte [Veicular campanhas publicitárias usando serviços da Microsoft Store](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

>**Observação**&nbsp;&nbsp;antes de você pode criar linhas de entrega para campanhas publicitárias usando essa API com êxito, você deve primeiro [criar uma campanha de anúncio pago usando o **campanhas publicitárias** página Partner Center](../publish/create-an-ad-campaign-for-your-app.md), e você deve adicionar pelo menos um instrumento de pagamento nesta página. Depois que você fizer isso, é possível criar linhas de entrega faturáveis para campanhas publicitárias usando essa API. Campanhas de anúncios que você cria usando a API cobrará automaticamente o instrumento de pagamento padrão escolhido na **campanhas publicitárias** página no Partner Center.

## <a name="prerequisites"></a>Pré-requisitos

Para usar esses métodos, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](run-ad-campaigns-using-windows-store-services.md#prerequisites) da API de promoções da Microsoft Store.

  > [!NOTE]
  > Como parte dos pré-requisitos, certifique-se que você [criar a campanha de anúncio pago pelo menos um no Partner Center](../publish/create-an-ad-campaign-for-your-app.md) e que você adicione pelo menos um instrumento de pagamento para a campanha de anúncio no Partner Center. Linhas de entrega que você cria usando essa API serão cobrada automaticamente o instrumento de pagamento padrão escolhido na **campanhas publicitárias** página no Partner Center.

* [Obtenha um token de acesso do Azure AD](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usar no cabeçalho da solicitação desses métodos. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

## <a name="request"></a>Solicitação

Esses métodos têm os seguintes URIs.

| Tipo de método | URI da solicitação         |  Descrição  |
|--------|---------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line``` |  Cria uma nova linha de entrega.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  Edita a linha de entrega especificada por *lineId*.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  Obtém a linha de entrega especificada por *lineId*.  |


### <a name="header"></a>Cabeçalho

| Cabeçalho        | Tipo   | Descrição         |
|---------------|--------|---------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |
| ID de rastreamento   | GUID   | Opcional. Uma ID que rastreia o fluxo de chamada.                                  |


### <a name="request-body"></a>Corpo da solicitação

Os métodos POST e PUT exigem um corpo da solicitação JSON com os campos obrigatórios de um objeto [Linha de entrega](#line) e quaisquer campos adicionais que você deseja definir ou alterar.


### <a name="request-examples"></a>Exemplos de solicitação

O exemplo a seguir demonstra como chamar o método POST para criar uma linha de entrega.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/line HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign - Paid Line",
    "configuredStatus": "Active",
    "startDateTime": "2017-01-19T12:09:34Z",
    "endDateTime": "2017-01-31T23:59:59Z",
    "bidAmount": 0.4,
    "dailyBudget": 20,
    "targetProfileId": {
        "id": 310021746
    },
    "creatives": [
        {
            "id": 106851
        }
    ],
    "campaignId": 31043481,
    "minMinutesPerImp ": 1
}
```

O exemplo a seguir demonstra como chamar o método GET para recuperar uma linha de entrega.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/31019990  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

Esses métodos retornam um corpo de resposta JSON com um objeto de [Linha de entrega](#line) que contém informações sobre a linha de entrega criada, atualizada ou recuperada. O exemplo a seguir demonstra um corpo de resposta para esses métodos.

```json
{
    "Data": {
        "id": 31043476,
        "name": "Contoso App Campaign - Paid Line",
        "configuredStatus": "Active",
        "effectiveStatus": "Active",
        "effectiveStatusReasons": [
            "{\"ValidationStatusReasons\":null}"
        ],
        "startDateTime": "2017-01-19T12:09:34Z",
        "endDateTime": "2017-01-31T23:59:59Z",
        "createdDateTime": "2017-01-17T10:28:34Z",
        "bidType": "CPM",
        "bidAmount": 0.4,
        "dailyBudget": 20,
        "targetProfileId": {
            "id": 310021746
        },
        "creatives": [
            {
                "id": 106126
            }
        ],
        "campaignId": 31043481,
        "minMinutesPerImp ": 1,
        "pacingType ": "SpendEvenly",
        "currencyId ": 732
    }
}

```


<span id="line"/>

## <a name="delivery-line-object"></a>Objeto da linha de entrega

O corpo da solicitação e resposta desses métodos contêm os campos a seguir. Essa tabela mostra quais campos são somente de leitura (ou seja, eles não podem ser alterados no método PUT) e quais são necessários no corpo da solicitação para os métodos POST ou PUT.

| Campo        | Tipo   |  Descrição      |  Somente leitura  | Padrão  | Obrigatório para POST/PUT |   
|--------------|--------|---------------|------|-------------|------------|
|  id   |  número inteiro   |  A ID da linha de entrega.     |   Sim    |      |  Não      |    
|  name   |  cadeia de caracteres   |   O nome da linha de entrega.    |    Não   |      |  POST     |     
|  configuredStatus   |  cadeia de caracteres   |  Um dos seguintes valores que especifica o status da linha de entrega especificada pelo desenvolvedor: <ul><li>**Active Directory**</li><li>**inativo**</li></ul>     |  Não     |      |   POST    |       
|  effectiveStatus   |  cadeia de caracteres   |   Um dos seguintes valores que especifica o status eficaz da linha de entrega com base na validação do sistema: <ul><li>**Active Directory**</li><li>**inativo**</li><li>**Processamento**</li><li>**Falha**</li></ul>    |    Sim   |      |  Não      |      
|  effectiveStatusReasons   |  matriz   |  Um ou mais dos seguintes valores que especificam o motivo para o status eficaz da linha de entrega: <ul><li>**AdCreativesInactive**</li><li>**ValidationFailed**</li></ul>      |  Sim     |     |    Não    |           
|  startDatetime   |  cadeia de caracteres   |  A data e hora de início para a linha de entrega no formato ISO 8601. Esse valor não pode ser alterado se já estiver no passado.     |    Não   |      |    POST, PUT     |
|  endDatetime   |  cadeia de caracteres   |  A data e hora de término para a linha de entrega no formato ISO 8601. Esse valor não pode ser alterado se já estiver no passado.     |   Não    |      |  POST, PUT     |
|  createdDatetime   |  cadeia de caracteres   |  A data e hora de criação da linha de entrega no formato ISO 8601.      |    Sim   |      |  Não      |
|  bidType   |  cadeia de caracteres   |  Um valor que especifica o tipo de lance da linha de entrega. Atualmente, o único valor com suporte é **CPM**.      |    Não   |  CPM    |  Não     |
|  bidAmount   |  decimal   |  A quantidade do lance a ser usada para qualquer solicitação de anúncio pedida.      |    Não   |  O valor médio de CPM com base nos mercados de destino (esse valor é revisado periodicamente).    |    Não    |  
|  dailyBudget   |  decimal   |  O orçamento diário da linha de entrega. É necessário definir *dailyBudget* ou *lifetimeBudget*.      |    Não   |      |   POST, PUT (se *lifetimeBudget* não estiver definido)       |
|  lifetimeBudget   |  decimal   |   O orçamento vitalício da linha de entrega. lifetimeBudget* ou *dailyBudget* devem ser definidos.      |    Não   |      |   POST, PUT (se *dailyBudget* não estiver definido)    |
|  targetingProfileId   |  objeto   |  No objeto que identifica o [perfil de direcionamento](manage-targeting-profiles-for-ad-campaigns.md#targeting-profile) que descreve os usuários, as regiões geográficas e os tipos de inventário que você deseja destinar essa linha de entrega. Esse objeto consiste em um único campo de *id* que especifica a ID do perfil de direcionamento.     |    Não   |      |  Não      |  
|  criativos   |  matriz   |  Um ou mais objetos que representam os [criativos](manage-creatives-for-ad-campaigns.md#creative) associados à linha de entrega. Cada objeto nesse campo consiste em um único campo *id* que especifica a ID de um criativo.      |    Não   |      |   Não     |  
|  campaignId   |  número inteiro   |  A ID da campanha publicitária pai.      |    Não   |      |   Não     |  
|  minMinutesPerImp   |  número inteiro   |  Especifica o intervalo de tempo mínimo (em minutos) entre duas impressões mostrado para o mesmo usuário dessa linha de entrega.      |    Não   |  4000    |  Não      |  
|  pacingType   |  cadeia de caracteres   |  Um dos seguintes valores que especificam o tipo de ritmo: <ul><li>**SpendEvenly**</li><li>**SpendAsFastAsPossible**</li></ul>      |    Não   |  SpendEvenly    |  Não      |
|  currencyId   |  número inteiro   |  A ID da moeda da campanha.      |    Sim   |  A moeda da conta de desenvolvedor (você não precisa especificar esse campo nas chamadas POST ou PUT)    |   Não     |      |


## <a name="related-topics"></a>Tópicos relacionados

* [Executar campanhas publicitárias usando serviços da Microsoft Store](run-ad-campaigns-using-windows-store-services.md)
* [Gerenciar campanhas publicitárias](manage-ad-campaigns.md)
* [Gerenciar perfis de direcionamento para campanhas publicitárias](manage-targeting-profiles-for-ad-campaigns.md)
* [Gerenciar criativos para campanhas publicitárias](manage-creatives-for-ad-campaigns.md)
* [Obter dados de desempenho de campanha de anúncio](get-ad-campaign-performance-data.md)
