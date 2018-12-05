---
ms.assetid: F37C2CEC-9ED1-4F9E-883D-9FBB082504D4
description: Use este método na API de compra da Microsoft Store para alterar o estado de cobrança de uma assinatura para um usuário.
title: Alterar o estado de cobrança de uma assinatura para um usuário
ms.date: 08/01/2018
ms.topic: article
keywords: windows 10, uwp, API de compra na Microsoft Store, assinaturas
ms.localizationpriority: medium
ms.openlocfilehash: 9e4cf27331a218c0c0ef06ee1a80c141b889504a
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8750124"
---
# <a name="change-the-billing-state-of-a-subscription-for-a-user"></a>Alterar o estado de cobrança de uma assinatura para um usuário

Use este método na API de compra da Microsoft Store para alterar o estado de cobrança de um complemento de assinatura para um determinado usuário. Você pode cancelar, estender, reembolsar ou desabilitar a renovação automática de uma assinatura.

> [!NOTE]
> Este método só pode ser usado por contas de desenvolvedores provisionadas pela Microsoft para poder criar complementos de assinatura para aplicativos da Plataforma Universal do Windows (UWP). Atualmente, os complementos de assinatura não estão disponíveis para a maioria das contas de desenvolvedores.

## <a name="prerequisites"></a>Pré-requisitos

Para usar esse método, você precisará:

* Um token de acesso do Azure AD com o URI de audiência `https://onestore.microsoft.com`.
* Uma chave de ID da Microsoft Store que representa a identidade do usuário que tem direito à assinatura que deseja alterar.

Para obter mais informações, consulte [Gerenciar direitos a produtos de um serviço](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                            |
|--------|--------------------------------------------------------|
| POST   | ```https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/{recurrenceId}/change``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho         | Tipo   | Descrição   |
|----------------|--------|-------------|
| Autorização  | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;.                           |
| Host           | cadeia | Deve ser definido como o valor **purchase.mp.microsoft.com**.                                            |
| Content-Length | número | O comprimento do corpo da solicitação.                                                                       |
| Content-Type   | string | Especifica o tipo de solicitação e resposta. Atualmente, o único valor com suporte é **application/json**. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Nome         | Tipo  | Descrição   |  Necessário  |
|----------------|--------|-------------|-----------|
| recurrenceId | cadeia | A ID da assinatura que deseja alterar. Para obter essa ID, chame o método [obter assinaturas para um usuário](get-subscriptions-for-a-user.md) , identifique a entrada de corpo de resposta que representa o complemento de assinatura que você deseja alterar e use o valor do campo **id** para a entrada.     | Sim      |


### <a name="request-body"></a>Corpo da solicitação

| Campo      | Tipo   | Descrição   | Necessário |
|----------------|--------|---------------|----------|
| b2bKey         | cadeia | A [chave de ID da Microsoft Store](view-and-grant-products-from-a-service.md#step-4) que representa a identidade do usuário cuja assinatura deseja alterar.     | Sim      |
| changeType     | cadeia |  Uma das seguintes strings que identifica o tipo de alteração que você quer fazer:<ul><li>**Cancelar**: cancela a assinatura.</li><li>**Estender**: estende a assinatura. Se você especificar esse valor, você também deve incluir o parâmetro *extensionTimeInDays*.</li><li>**Reembolsar**: reembolsa a assinatura para o cliente.</li><li>**ToggleAutoRenew**: desabilita a renovação automática da assinatura. Se a renovação automática estiver desativada atualmente para a assinatura, esse valor não faz nada.</li></ul>   | Sim      |
| extensionTimeInDays  | cadeia  | Se o parâmetro *changeType* tiver o valor **Estender**, esse parâmetro especifica o número de dias para estender a assinatura. |  Sim, se *changeType* tiver o valor **estender**; caso contrário, não.  ||


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como usar esse método para estender o período de inscrição por 5 dias. Substitua o valor *b2bKey* pela [chave ID da Microsoft Store](view-and-grant-products-from-a-service.md#step-4) que representa a identidade do usuário cuja assinatura você deseja alterar.

```json
POST https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/mdr:0:bc0cb6960acd4515a0e1d638192d77b7:77d5ebee-0310-4d23-b204-83e8613baaac/change HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
Host: https://purchase.mp.microsoft.com

{
  "b2bKey":  "eyJ0eXAiOiJ...",
  "changeType": "Extend",
  "extensionTimeInDays": "5"
}
```


## <a name="response"></a>Resposta

Esse método retorna um corpo de resposta JSON que contém informações sobre o complemento de assinatura que foi modificado, incluindo os campos que foram modificados. O exemplo a seguir demonstra um corpo de resposta para esse método.

```json
{
  "items": [
    {
      "autoRenew":true,
      "beneficiary":"pub:gFVuEBiZHPXonkYvtdOi+tLE2h4g2Ss0ZId0RQOwzDg=",
      "expirationTime":"2017-06-16T03:07:49.2552941+00:00",
      "id":"mdr:0:bc0cb6960acd4515a0e1d638192d77b7:77d5ebee-0310-4d23-b204-83e8613baaac",
      "lastModified":"2017-01-10T21:08:13.1459644+00:00",
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
| productId | cadeia | A [ID da Store](in-app-purchases-and-trials.md#store-ids) para o [produto](in-app-purchases-and-trials.md#products-skus-and-availabilities) que representa o complemento de assinatura no catálogo da Microsoft Store. Um exemplo de ID da Store para um produto é 9NBLGGH42CFD.     |
| skuId | cadeia |  A [ID da Store](in-app-purchases-and-trials.md#store-ids) para o [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) que representa o complemento de assinatura no catálogo da Microsoft Store. Um exemplo de ID da Store para uma SKU é 0010.    |
| startTime | cadeia |  A data e a hora de início da assinatura, no formato ISO 8601.     |
| recurrenceState | cadeia  |  Um dos seguintes valores:<ul><li>**Nenhum**:&nbsp;&nbsp;isso indica uma assinatura permanente.</li><li>**Ativa**:&nbsp;&nbsp;a assinatura está ativa e o usuário está qualificado para usar os serviços.</li><li>**Inativa**:&nbsp;&nbsp;a assinatura passou da data de expiração, e o usuário desativou a opção de renovação automática da assinatura.</li><li>**Cancelada**:&nbsp;&nbsp;a assinatura foi encerrada propositadamente antes da data de expiração, com ou sem um reembolso.</li><li>**Pagamento atrasado**:&nbsp;&nbsp;a assinatura está com *pagamento atrasado* (ou seja, assinatura está prestes a expirar e a Microsoft está tentando adquirir fundos para renovar automaticamente a assinatura).</li><li>**Falha**:&nbsp;&nbsp;o período de pagamento acabou e a assinatura não pôde ser renovada após várias tentativas.</li></ul><p>**Observação:**</p><ul><li>**Inativa**/**Canceleda**/**Falha** são estados terminais. Quando uma inscrição entra em um desses estados, o usuário deve recomprar a assinatura para ativá-la novamente. O usuário não tem direito a usar os serviços nesses estados.</li><li>Quando uma assinatura for **Cancelada**, o expirationTime será atualizado com a data e hora de cancelamento.</li><li>A ID da assinatura permanecerá a mesmo durante toda a sua vida. Ela não irá mudar se a opção de renovação automática for ativada ou desativada. Se um usuário recomprar uma inscrição depois de chegar a um estado terminal, será criada uma nova ID de inscrição.</li><li>O ID de uma assinatura deve ser usado para executar qualquer operação em uma assinatura individual.</li><li>Quando um usuário recompra uma assinatura depois de cancelar ou descontinuar, se você consultar os resultados para o usuário, você receberá duas entradas: uma com a ID da assinatura antiga em um estado terminal e uma com a nova ID da inscrição em um estado ativo.</li><li>É sempre uma boa prática verificar os tempos de recorrência e expiração, uma vez que as atualizações do estado de recorrência podem ser atrasadas em poucos minutos (ou ocasionalmente horas).       |
| cancellationDate | cadeia   |  A data e hora que é assinatura do usuário foi cancelada, sem formato ISO 8601.     |


## <a name="related-topics"></a>Tópicos relacionados


* [Gerenciar direitos a produtos de um serviço](view-and-grant-products-from-a-service.md)
* [Obter assinaturas para um usuário](get-subscriptions-for-a-user.md)
* [Consulta por produtos](query-for-products.md)
* [Declarar produtos consumíveis como providenciados](report-consumable-products-as-fulfilled.md)
* [Renovar uma chave de ID da Microsoft Store](renew-a-windows-store-id-key.md)
