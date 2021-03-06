---
ms.assetid: 2BCFF687-DC12-49CA-97E4-ACEC72BFCD9B
description: Use esse método na API de envio a Microsoft Store para recuperar informações sobre todos os aplicativos que são registrados em sua conta no Partner Center.
title: Obter todos os apps
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, aplicativos
ms.localizationpriority: medium
ms.openlocfilehash: 267e1d4de3917ae332cdfe15309f3871ef7b6647
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641981"
---
# <a name="get-all-apps"></a>Obter todos os apps


Use esse método na API de envio a Microsoft Store para recuperar dados para os aplicativos que são registrados em sua conta no Partner Center.

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

Todos os parâmetros de solicitação são opcionais para esse método. Se você chamar esse método sem parâmetros, a resposta contém os dados para os primeiros 10 aplicativos que são registrados em sua conta.

|  Parâmetro  |  Tipo  |  Descrição  |  Obrigatório  |
|------|------|------|------|
|  top  |  int  |  O número de itens a serem retornados na solicitação (ou seja, o número de aplicativos a serem retornados). Se sua conta tiver mais aplicativos que o valor especificado na consulta, o corpo da resposta incluirá um caminho relativo do URI que você pode acrescentar ao URI do método para solicitar a próxima página de dados.  |  Não  |
|  skip  |  int  |  O número de itens a serem ignorados na consulta antes de retornar os itens restantes. Use este parâmetro para percorrer conjuntos de dados. Por exemplo, top=10 e skip=0 recuperam os itens de 1 a 10, top=10 e skip=10 recuperam os itens de 11 a 20 e assim por diante.  |  Não  |


### <a name="request-body"></a>Corpo da solicitação

Não forneça um corpo da solicitação para esse método.

### <a name="request-examples"></a>Exemplos de solicitação

O exemplo a seguir demonstra como recuperar os 10 primeiros aplicativos que estão registrados em sua conta.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications HTTP/1.1
Authorization: Bearer <your access token>
```

O exemplo a seguir demonstra como recuperar informações sobre todos os aplicativos que estão registrados em sua conta. Primeiro, obtenha os 10 principais aplicativos:

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

Em seguida, chamar recursivamente `GET https://manage.devcenter.microsoft.com/v1.0/my/{@nextLink}` até que `{@nextlink}` é nulo ou não existe na resposta. Por exemplo:

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=10&top=10 HTTP/1.1
Authorization: Bearer <your access token>
```
  
```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=20&top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=30&top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

Se você já souber o número total de aplicativos que você tem em sua conta, você pode simplesmente passar esse número na **superior** para obter informações sobre todos os seus aplicativos.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?top=23 HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>Resposta

O exemplo a seguir demonstra o corpo da resposta JSON retornado por uma solicitação bem-sucedida para os 10 primeiros aplicativos que são registrados para uma conta de desenvolvedor com 21 aplicativos no total. Para abreviar, este exemplo mostra apenas os dados dos dois primeiros aplicativos retornados pela solicitação. Para obter mais detalhes sobre os valores no corpo da resposta, veja a seção a seguir.

```json
{
  "@nextLink": "applications?skip=10&top=10",
  "value": [
    {
      "id": "9NBLGGH4R315",
      "primaryName": "Contoso sample app",
      "packageFamilyName": "5224ContosoDeveloper.ContosoSampleApp_ng6try80pwt52",
      "packageIdentityName": "5224ContosoDeveloper.ContosoSampleApp",
      "publisherName": "CN=…",
      "firstPublishedDate": "2016-03-11T01:32:11.0747851Z",
      "pendingApplicationSubmission": {
        "id": "1152921504621134883",
        "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621134883"
      }
    },
    {
      "id": "9NBLGGH29DM8",
      "primaryName": "Contoso sample app 2",
      "packageFamilyName": "5224ContosoDeveloper.ContosoSampleApp2_ng6try80pwt52",
      "packageIdentityName": "5224ContosoDeveloper.ContosoSampleApp2",
      "publisherName": "CN=…",
      "firstPublishedDate": "2016-03-12T01:49:11.0747851Z",
      "lastPublishedApplicationSubmission": {
        "id": "1152921504621225621",
        "resourceLocation": "applications/9NBLGGH29DM8/submissions/1152921504621225621"
      }
      // Next 8 apps are omitted for brevity ...
    }
  ],
  "totalCount": 21
}
```

### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo   | Descrição                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| value      | matriz  | Uma matriz de objetos que contêm informações sobre cada aplicativo que está registrado em sua conta. Para obter mais informações sobre os dados em cada objeto, consulte [Recurso do aplicativo](get-app-data.md#application_object).                                                                                                                           |
| @nextLink  | cadeia de caracteres | Se houver páginas adicionais de dados, essa cadeia de caracteres terá um caminho relativo que você pode acrescentar ao URI básico da solicitação `https://manage.devcenter.microsoft.com/v1.0/my/` para solicitar a próxima página de dados. Por exemplo, se o parâmetro *top* do corpo da solicitação inicial for definido como 10, mas houver 20 aplicativos registrados em sua conta, o corpo da resposta incluirá um valor @nextLink de `applications?skip=10&top=10`, o que indica que você pode chamar `https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=10&top=10` para solicitar os próximos 10 aplicativos. |
| totalCount | int    | O número total de linhas no resultado dos dados da consulta (ou seja, o número total de aplicativos que estão registrados em sua conta).                                                |


## <a name="error-codes"></a>Códigos de erro

Se não for possível concluir a solicitação, a resposta conterá um dos seguintes códigos de erro HTTP.

| Código de erro |  Descrição   |
|--------|------------------|
| 404  | Nenhum aplicativo foi encontrado. |
| 409  | Os aplicativos usam recursos Partner Center [atualmente não tem suporte da API de envio a Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando os serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obter um aplicativo](get-an-app.md)
* [Obter voos de pacote para um aplicativo](get-flights-for-an-app.md)
* [Obter complementos para um aplicativo](get-add-ons-for-an-app.md)
