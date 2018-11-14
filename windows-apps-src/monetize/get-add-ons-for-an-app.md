---
author: Xansky
ms.assetid: E59FB6FE-5318-46DF-B050-73F599C3972A
description: Use este método na API de envio da Microsoft Store para recuperar informações sobre as compras no aplicativo para um aplicativo que está registrado para o Partner Center.
title: Obter complementos para um app
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, complementos, produtos no aplicativo, IAPs
ms.localizationpriority: medium
ms.openlocfilehash: 3689a4fe16d016bb23bb7141630fd1f6a7b83142
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6653864"
---
# <a name="get-add-ons-for-an-app"></a>Obter complementos para um aplicativo

Use este método na API de envio da Microsoft Store para listar os complementos para um aplicativo que está registrado à sua conta do Partner Center.

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados


|  Nome  |  Tipo  |  Descrição  |  Obrigatório  |
|------|------|------|------|
|  applicationId  |  string  |  A ID da Loja do aplicativo para o qual você deseja recuperar os complementos. Para obter mais informações sobre a ID da Loja, consulte [Exibir detalhes de identidade de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |  Sim  |
|  top  |  int  |  O número de itens a serem retornados na solicitação (ou seja, o número de complementos a serem retornados). Se o aplicativo tiver mais complementos que o valor especificado na consulta, o corpo da resposta incluirá um caminho relativo do URI que você pode acrescentar ao URI do método para solicitar a próxima página de dados.  |  Não  |
|  skip |  int  | O número de itens a serem ignorados na consulta antes de retornar os itens restantes. Use este parâmetro para percorrer conjuntos de dados. Por exemplo, top=10 e skip=0 recuperam os itens de 1 a 10, top=10 e skip=10 recuperam os itens de 11 a 20 e assim por diante.   |  Não  |


### <a name="request-body"></a>Corpo da solicitação

Não forneça um corpo da solicitação para esse método.

### <a name="request-examples"></a>Exemplos de solicitação

O exemplo a seguir demonstra como listar todos os complementos de um aplicativo.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listinappproducts HTTP/1.1
Authorization: Bearer <your access token>
```

O exemplo a seguir demonstra como listar os 10 primeiros complementos de um aplicativo.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listinappproducts?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

O exemplo a seguir demonstra o corpo da resposta JSON retornado por uma solicitação bem-sucedida para os 10 primeiros complementos de um aplicativo com 53 complementos no total. Para abreviar, este exemplo mostra apenas os dados dos três primeiros complementos retornados pela solicitação. Para obter mais detalhes sobre os valores no corpo da resposta, veja a seção a seguir.

```json
{
  "@nextLink": "applications/9NBLGGH4R315/listinappproducts/?skip=10&top=10",
  "value": [
    {
      "inAppProductId": "9NBLGGH4TNMP"
    },
    {
      "inAppProductId": "9NBLGGH4TNMN"
    },
    {
      "inAppProductId": "9NBLGGH4TNNR"
    },
    // Next 7 add-ons  are omitted for brevity ...
  ],
  "totalCount": 53
}
```

### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo   | Descrição                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| @nextLink  | cadeia | Se houver páginas adicionais de dados, essa cadeia de caracteres terá um caminho relativo que você pode acrescentar ao URI básico da solicitação ```https://manage.devcenter.microsoft.com/v1.0/my/``` para solicitar a próxima página de dados. Por exemplo, se o parâmetro *top* do corpo da solicitação inicial for definido como 10, mas houver 50 complementos para o aplicativo, o corpo da resposta incluirá um valor @nextLink de ```applications/{applicationid}/listinappproducts/?skip=10&top=10```, o que indica que você pode chamar ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/listinappproducts/?skip=10&top=10``` para solicitar os próximos 10 complementos. |
| value      | array  | Uma matriz de objetos que listam a ID da Loja de cada complemento para o aplicativo especificado. Para obter mais informações sobre os dados em cada objeto, consulte [recurso do complemento](get-app-data.md#add-on-object).                                                                                                                           |
| totalCount | int    | O número total de linhas no resultado dos dados da consulta (ou seja, o número total de complementos do aplicativo especificado).    |


## <a name="error-codes"></a>Códigos de erro

Se não for possível concluir a solicitação, a resposta conterá um dos seguintes códigos de erro HTTP.

| Código de erro |  Descrição   |
|--------|------------------|
| 404  | Nenhum complemento foi encontrado. |
| 409  | Os complementos usam recursos do Partner Center que estão [atualmente não é compatível com a API de envio da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obter todos os apps](get-all-apps.md)
* [Obter um aplicativo](get-an-app.md)
* [Obter pacotes de pré-lançamento de um aplicativo](get-flights-for-an-app.md)
