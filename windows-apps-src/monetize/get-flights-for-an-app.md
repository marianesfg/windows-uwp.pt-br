---
ms.assetid: B0AD0B8E-867E-4403-9CF6-43C81F3C30CA
description: Use esse método na API de envio a Microsoft Store para recuperar informações de voo do pacote para um aplicativo que é registrado em sua conta no Partner Center.
title: Obter pacotes de pré-lançamento de um app
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, versões de pré-lançamento, pacotes de pré-lançamento
ms.localizationpriority: medium
ms.openlocfilehash: 66e64f2c499835a345bb9563fd005b86a926a4d2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372010"
---
# <a name="get-package-flights-for-an-app"></a>Obter pacotes de pré-lançamento de um app

Use esse método na API de envio a Microsoft Store para listar os voos de pacote para um aplicativo que é registrado em sua conta no Partner Center. Para obter mais informações sobre pacotes de pré-lançamento, consulte [Pacotes de pré-lançamento](https://docs.microsoft.com/windows/uwp/publish/package-flights).

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| OBTER    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

|  Nome  |  Tipo  |  Descrição  |  Obrigatório  |
|------|------|------|------|
|  applicationId  |  cadeia de caracteres  |  A ID da Loja do aplicativo para o qual você deseja recuperar os pacotes de pré-lançamento. Para obter mais informações sobre a ID da Loja, consulte [Exibir detalhes de identidade do aplicativo](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details).  |  Sim  |
|  top  |  int  |  O número de itens a serem retornados na solicitação (ou seja, o número de pacotes de pré-lançamento a serem retornados). Se a conta tiver mais pacotes de pré-lançamento que o valor especificado na consulta, o corpo da resposta incluirá um caminho relativo do URI que você pode acrescentar ao URI do método para solicitar a próxima página de dados.  |  Não  |
|  skip  |  int  |  O número de itens a serem ignorados na consulta antes de retornar os itens restantes. Use este parâmetro para percorrer conjuntos de dados. Por exemplo, top=10 e skip=0 recuperam os itens de 1 a 10, top=10 e skip=10 recuperam os itens de 11 a 20 e assim por diante.  |  Não  |


### <a name="request-body"></a>Corpo da solicitação

Não forneça um corpo da solicitação para esse método.

### <a name="request-examples"></a>Exemplos de solicitação

O exemplo a seguir demonstra como listar todos os pacotes de pré-lançamento de um aplicativo.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listflights HTTP/1.1
Authorization: Bearer <your access token>
```

O exemplo a seguir demonstra como listar o primeiro pacote de pré-lançamento de um aplicativo.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listflights?top=1 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

O exemplo a seguir demonstra o corpo da resposta JSON retornado por uma solicitação bem-sucedida para o primeiro pacote de pré-lançamento de um aplicativo com três pacotes de pré-lançamento no total. Para obter mais detalhes sobre os valores no corpo da resposta, veja a seção a seguir.

```json
{
  "value": [
    {
      "flightId": "7bfc11d5-f710-47c5-8a98-e04bb5aad310",
      "friendlyName": "myflight",
      "lastPublishedFlightSubmission": {
        "id": "1152921504621086517",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621086517"
      },
      "pendingFlightSubmission": {
        "id": "1152921504621215786",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621215786"
      },
      "groupIds": [
        "1152921504606962205"
      ],
      "rankHigherThan": "Non-flighted submission"
    }
  ],
  "totalCount": 3
}
```

### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo   | Descrição       |
|------------|--------|---------------------|
| @nextLink  | cadeia de caracteres | Se houver páginas adicionais de dados, essa cadeia de caracteres terá um caminho relativo que você pode acrescentar ao URI básico da solicitação `https://manage.devcenter.microsoft.com/v1.0/my/` para solicitar a próxima página de dados. Por exemplo, se o parâmetro *top* do corpo da solicitação inicial for definido como 2, mas houver 4 pacotes de pré-lançamento para o aplicativo, o corpo da resposta incluirá um@nextLink valor `applications/{applicationid}/listflights/?skip=2&top=2`, o que indica que você pode chamar `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/listflights/?skip=2&top=2`  para solicitar os próximos 2 pacotes de pré-lançamento. |
| value      | matriz  | Uma matriz de objetos que fornecem informações sobre pacotes de pré-lançamento para o aplicativo especificado. Para obter mais informações sobre os dados em cada objeto, consulte [Recurso da versão de pré-lançamento](get-app-data.md#flight-object).               |
| totalCount | int    | O número total de linhas no resultado dos dados da consulta (ou seja, o número total de pacotes de pré-lançamento do aplicativo especificado).   |


## <a name="error-codes"></a>Códigos de erro

Se não for possível concluir a solicitação, a resposta conterá um dos seguintes códigos de erro HTTP.

| Código de erro |  Descrição   |
|--------|------------------|
| 404  | Nenhum pacote de pré-lançamento foi encontrado. |
| 409  | O aplicativo usa um recurso do Partner Center que está [atualmente não tem suporte da API de envio a Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando os serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obter todos os aplicativos](get-all-apps.md)
* [Obter um aplicativo](get-an-app.md)
* [Obter complementos para um aplicativo](get-add-ons-for-an-app.md)
