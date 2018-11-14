---
author: Xansky
ms.assetid: 94B5B2E9-BAEE-4B7F-BAF1-DA4D491427D7
description: Use este método na API de compra da Microsoft Store para obter as assinaturas que um determinado usuário tem direitos de usar.
title: Obter assinaturas para um usuário
ms.author: mhopkins
ms.date: 07/10/2018
ms.topic: article
keywords: windows 10, uwp, API de compra na Microsoft Store, assinaturas
ms.localizationpriority: medium
ms.openlocfilehash: b8fe6262ca6ef52ca94ade2c56b18f5e71951f07
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6651225"
---
# <a name="get-subscriptions-for-a-user"></a>Obter assinaturas para um usuário

Use este método na API de compra da Microsoft Store para obter os complementos de assinatura que um determinado usuário tem direitos de usar.

> [!NOTE]
> Este método só pode ser usado por contas de desenvolvedores provisionadas pela Microsoft para poder criar complementos de assinatura para aplicativos da Plataforma Universal do Windows (UWP). Atualmente, os complementos de assinatura não estão disponíveis para a maioria das contas de desenvolvedores.

## <a name="prerequisites"></a>Pré-requisitos

Para usar esse método, você precisará:

* Um token de acesso do Azure AD com o URI de audiência `https://onestore.microsoft.com`.
* Uma chave de ID da Microsoft Store que representa a identidade do usuário cujas inscrições você deseja obter.

Para obter mais informações, consulte [Gerenciar direitos a produtos de um serviço](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                            |
|--------|--------------------------------------------------------|
| POST   | ```https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/query``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho         | Tipo   | Descrição      |
|----------------|--------|-------------------|
| Autorização  | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;.                           |
| Host           | cadeia | Deve ser definido como o valor **purchase.mp.microsoft.com**.                                            |
| Content-Length | número | O comprimento do corpo da solicitação.                                                                       |
| Content-Type   | string | Especifica o tipo de solicitação e resposta. Atualmente, o único valor com suporte é **application/json**. |


### <a name="request-body"></a>Corpo da solicitação

| Parâmetro      | Tipo   | Descrição     | Necessário |
|----------------|--------|-----------------|----------|
| b2bKey         | string | A [chave de ID da Microsoft Store](view-and-grant-products-from-a-service.md#step-4) que representa a identidade do usuário cujas inscrições você deseja obter.  | Sim      |
| continuationToken |  cadeia     |  Se o usuário tiver direitos sobre múltiplas assinaturas, o corpo da resposta retornará um token de continuação quando o limite da página for atingido. Forneça esse token de continuação em chamadas subsequentes para recuperar produtos restantes.    | Não      |
| pageSize       | cadeia | O número máximo de assinaturas para retornar uma resposta. O padrão é 25.     |  Não      |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como usar esse método para obter os complementos de assinatura que um determinado usuário tenha direitos a serem usados. Substitua o valor *b2bKey* pela [chave ID da Microsoft Store](view-and-grant-products-from-a-service.md#step-4) que representa a identidade do usuário cujas assinaturas você deseja obter.

```json
POST https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/query HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
Host: https://purchase.mp.microsoft.com

{
  "b2bKey":  "eyJ0eXAiOiJ..."
}
```


## <a name="response"></a>Resposta

Este método retorna um corpo de resposta JSON que contém uma coleção de objetos de dados que descrevem os complementos de assinatura que o usuário tem direitos a serem usados. O exemplo a seguir demonstra o corpo de resposta para um usuário que possui um direito para uma assinatura.

```json
{
  "items": [
    {
      "autoRenew":true,
      "beneficiary":"pub:gFVuEBiZHPXonkYvtdOi+tLE2h4g2Ss0ZId0RQOwzDg=",
      "expirationTime":"2017-06-11T03:07:49.2552941+00:00",
      "id":"mdr:0:bc0cb6960acd4515a0e1d638192d77b7:77d5ebee-0310-4d23-b204-83e8613baaac",
      "lastModified":"2017-01-08T21:07:51.1459644+00:00",
      "market":"US",
      "productId":"9NBLGGH52Q8X",
      "skuId":"0024",
      "startTime":"2017-01-10T21:07:49.2552941+00:00",
      "recurrenceState":"Active"
    }
  ]
}
```


### <a name="response-body"></a>Corpo da resposta

O corpo de resposta contém os seguintes dados.

| Valor        | Tipo   | Descrição            |
|---------------|--------|---------------------|
| itens | matriz | Uma série de objetos que contêm dados sobre cada complemento de inscrição que o usuário especificado tem o direito de usar. Para obter mais informações sobre os dados em cada objeto, consulte a tabela a seguir.  |


Cada objeto na matriz *itens* contém os seguintes valores.

| Valor        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------|
| Renovar automaticamente | Booliano |  Indica se a assinatura está configurada para renovar automaticamente no final do período de inscrição atual.   |
| beneficiário | cadeia |  A ID do beneficiário do direito associado a esta subscrição.   |
| expirationTime | cadeia | A data e a hora em que a assinatura expirará, no formato ISO 8601. Este campo só está disponível quando a assinatura está em certos estados. O tempo de expiração geralmente indica quando o estado atual expira. Por exemplo, para uma assinatura ativa, a data de validade indica quando ocorrerá a próxima renovação automática.    |
| expirationTimeWithGrace | string | A data e hora que a assinatura expirará incluindo o período de cortesia, no formato ISO 8601. Esse valor indica quando o usuário perde o acesso à assinatura depois que a assinatura não pôde ser renovada automaticamente.    |
| id | cadeia |  A ID da assinatura. Use esse valor para indicar qual delas você deseja modificar ao chamar o método [alterar o estado de cobrança de uma assinatura para um usuário](change-the-billing-state-of-a-subscription-for-a-user.md).    |
| isTrial | Booliano |  Indica se a assinatura é uma versão de avaliação.     |
| lastModified | cadeia |  A data e hora em que a assinatura foi modificada pela última vez, no formato ISO 8601.      |
| mercado | cadeia | O código do país (no formato ISO-3166-1 alpha-2 de duas letras) no qual o usuário adquiriu a assinatura.      |
| productId | cadeia |  A [ID da Store](in-app-purchases-and-trials.md#store-ids) para o [produto](in-app-purchases-and-trials.md#products-skus-and-availabilities) que representa o complemento de assinatura no catálogo da Microsoft Store. Um exemplo de ID da Store para um produto é 9NBLGGH42CFD.     |
| skuId | cadeia |  A [ID da Store](in-app-purchases-and-trials.md#store-ids) para o [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) que representa o complemento de assinatura no catálogo da Microsoft Store. Um exemplo de ID da Store para uma SKU é 0010.    |
| startTime | cadeia |  A data e a hora de início da assinatura, no formato ISO 8601.     |
| recurrenceState | cadeia  |  Um dos seguintes valores:<ul><li>**Nenhum**:&nbsp;&nbsp;isso indica uma assinatura permanente.</li><li>**Ativa**:&nbsp;&nbsp;a assinatura está ativa e o usuário está qualificado para usar os serviços.</li><li>**Inativa**:&nbsp;&nbsp;a assinatura passou da data de expiração, e o usuário desativou a opção de renovação automática da assinatura.</li><li>**Cancelada**:&nbsp;&nbsp;a assinatura foi encerrada propositadamente antes da data de expiração, com ou sem um reembolso.</li><li>**Pagamento atrasado**:&nbsp;&nbsp;a assinatura está com *pagamento atrasado* (ou seja, assinatura está prestes a expirar e a Microsoft está tentando adquirir fundos para renovar automaticamente a assinatura).</li><li>**Falha**:&nbsp;&nbsp;o período de pagamento acabou e a assinatura não pôde ser renovada após várias tentativas.</li></ul><p>**Observação:**</p><ul><li>**Inativa**/**Canceleda**/**Falha** são estados terminais. Quando uma inscrição entra em um desses estados, o usuário deve recomprar a assinatura para ativá-la novamente. O usuário não tem direito a usar os serviços nesses estados.</li><li>Quando uma assinatura for **Cancelada**, o expirationTime será atualizado com a data e hora de cancelamento.</li><li>A ID da assinatura permanecerá a mesmo durante toda a sua vida. Ela não irá mudar se a opção de renovação automática for ativada ou desativada. Se um usuário recomprar uma inscrição depois de chegar a um estado terminal, será criada uma nova ID de inscrição.</li><li>O ID de uma assinatura deve ser usado para executar qualquer operação em uma assinatura individual.</li><li>Quando um usuário recompra uma assinatura depois de cancelar ou descontinuar, se você consultar os resultados para o usuário, você receberá duas entradas: uma com a ID da assinatura antiga em um estado terminal e uma com a nova ID da inscrição em um estado ativo.</li><li>É sempre uma boa prática verificar os tempos de recorrência e expiração, uma vez que as atualizações do estado de recorrência podem ser atrasadas em poucos minutos (ou ocasionalmente horas).       |
| cancellationDate | cadeia   |  A data e hora que é assinatura do usuário foi cancelada, sem formato ISO 8601.     |


## <a name="related-topics"></a>Tópicos relacionados

* [Gerenciar direitos a produtos de um serviço](view-and-grant-products-from-a-service.md)
* [Alterar o estado de cobrança de uma assinatura para um usuário](change-the-billing-state-of-a-subscription-for-a-user.md)
* [Consulta por produtos](query-for-products.md)
* [Declarar produtos consumíveis como providenciados](report-consumable-products-as-fulfilled.md)
* [Renovar uma chave de ID da Microsoft Store](renew-a-windows-store-id-key.md)
