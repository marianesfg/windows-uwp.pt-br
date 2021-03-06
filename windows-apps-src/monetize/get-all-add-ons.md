---
ms.assetid: 7B6A99C6-AC86-41A1-85D0-3EB39A7211B6
description: Use esse método na API de envio a Microsoft Store para recuperar todos os dados de complemento para todos os aplicativos que são registrados em sua conta no Partner Center.
title: Obter todos os complementos
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, complementos, produtos no aplicativo, IAPs
ms.localizationpriority: medium
ms.openlocfilehash: 84b55ea8bc62955e3556fcff4e8d608738eb59ce
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334424"
---
# <a name="get-all-add-ons"></a>Obter todos os complementos

Use esse método na API de envio a Microsoft Store para recuperar dados para todos os complementos para todos os aplicativos que são registrados em sua conta no Partner Center.

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| OBTER    | `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

Todos os parâmetros de solicitação são opcionais para esse método. Se você chamar esse método sem parâmetros, a resposta conterá dados para todos os complementos de todos os aplicativos que estão registrados em sua conta.

|  Parâmetro  |  Tipo  |  Descrição  |  Obrigatório  |
|------|------|------|------|
|  top  |  int  |  O número de itens a serem retornados na solicitação (ou seja, o número de complementos a serem retornados). Se sua conta tiver mais complementos que o valor especificado na consulta, o corpo da resposta incluirá um caminho relativo do URI que você pode acrescentar ao URI do método para solicitar a próxima página de dados.  |  Não  |
|  skip  |  int  |  O número de itens a serem ignorados na consulta antes de retornar os itens restantes. Use este parâmetro para percorrer conjuntos de dados. Por exemplo, top=10 e skip=0 recuperam os itens de 1 a 10, top=10 e skip=10 recuperam os itens de 11 a 20 e assim por diante.  |  Não  |


### <a name="request-body"></a>Corpo da solicitação

Não forneça um corpo da solicitação para esse método.

### <a name="request-examples"></a>Exemplos de solicitação

O exemplo a seguir demonstra como recuperar todos os dados de complemento de todos os aplicativos que estão registrados em sua conta.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts HTTP/1.1
Authorization: Bearer <your access token>
```

O exemplo a seguir demonstra como recuperar somente os 10 primeiros complementos.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

O exemplo a seguir demonstra o corpo da resposta JSON retornado por uma solicitação bem-sucedida para os 5 primeiros complementos que são registrados para uma conta de desenvolvedor com 1072 complementos no total. Para abreviar, este exemplo mostra apenas os dados dos dois primeiros complementos retornados pela solicitação. Para obter mais detalhes sobre os valores no corpo da resposta, veja a seção a seguir.

```json
{
  "@nextLink": "inappproducts/?skip=5&top=5",
  "value": [
    {
      "applications": {
        "value": [
          {
            "id": "9NBLGGH4R315",
            "resourceLocation": "applications/9NBLGGH4R315"
          }
        ],
        "totalCount": 1
      },
      "id": "9NBLGGH4TNMP",
      "productId": "a8b8310b-fa8d-4da0-aca0-577ef6dce4dd",
      "productType": "Consumable",
      "pendingInAppProductSubmission": {
        "id": "1152921504621243619",
        "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
      },
      "lastPublishedInAppProductSubmission": {
        "id": "1152921504621243705",
        "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243705"
      }
    },
    {
      "applications": {
        "value": [
          {
            "id": "9NBLGGH4R315",
            "resourceLocation": "applications/9NBLGGH4R315"
          }
        ],
        "totalCount": 1
      },
      "id": "9NBLGGH4TNMN",
      "productId": "6a3c9788-a350-448a-bd32-16160a13018a",
      "productType": "Consumable",
      "pendingInAppProductSubmission": {
        "id": "1152921504621243538",
        "resourceLocation": "inappproducts/9NBLGGH4TNMN/submissions/1152921504621243538"
      },
      "lastPublishedInAppProductSubmission": {
        "id": "1152921504621243106",
        "resourceLocation": "inappproducts/9NBLGGH4TNMN/submissions/1152921504621243106"
      }
    },

  // Other add-ons omitted for brevity...
  ],
  "totalCount": 1072
}
```

### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo   | Descrição                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| @nextLink  | cadeia de caracteres | Se houver páginas adicionais de dados, essa cadeia de caracteres terá um caminho relativo que você pode acrescentar ao URI básico da solicitação `https://manage.devcenter.microsoft.com/v1.0/my/` para solicitar a próxima página de dados. Por exemplo, se o parâmetro *top* do corpo da solicitação inicial for definido como 10, mas houver 100 complementos registrados em sua conta, o corpo da resposta incluirá um valor @nextLink de `inappproducts?skip=10&top=10`, o que indica que você pode chamar `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts?skip=10&top=10` para solicitar os próximos 10 complementos. |
| value            | matriz  |  Uma matriz que contém objetos que fornecem informações sobre cada complemento. Para saber mais, veja [recurso de complemento](manage-add-ons.md#add-on-object).   |
| totalCount   | int  | O número de objetos de aplicativo na matriz *value* do corpo da resposta.     |


## <a name="error-codes"></a>Códigos de erro

Se não for possível concluir a solicitação, a resposta conterá um dos seguintes códigos de erro HTTP.

| Código de erro |  Descrição   |
|--------|------------------|
| 404  | Nenhum complemento foi encontrado. |
| 409  | Os aplicativos ou complementos usam recursos Partner Center [atualmente não tem suporte da API de envio a Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando os serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gerenciar envios de complemento](manage-add-on-submissions.md)
* [Adquirir um complemento](get-an-add-on.md)
* [Criar um complemento](create-an-add-on.md)
* [Excluir um complemento](delete-an-add-on.md)
