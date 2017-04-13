---
author: mcleanbyron
ms.assetid: 87708690-079A-443D-807E-D2BF9F614DDF
description: "Use este método na API de envio da Windows Store para obter dados para um pacote de pré-lançamento de um aplicativo que está registrado na sua conta do Centro de Desenvolvimento do Windows."
title: "Obter um pacote de pré-lançamento"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, API de envio da Windows Store, versão de pré-lançamento, pacote de pré-lançamento"
ms.openlocfilehash: 921d33c033704de711112c30feae7dbbfebc732b
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="get-a-package-flight"></a>Obter um pacote de pré-lançamento




Use este método na API de envio da Windows Store para obter dados para um pacote de pré-lançamento de um aplicativo que está registrado na sua conta do Centro de Desenvolvimento do Windows.

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Windows Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

>**Observação**&nbsp;&nbsp;Este método só pode ser usado para contas do Centro de Desenvolvimento do Windows que receberam permissões para usar a API de envio da Windows Store. Nem todas as contas têm essa permissão habilitada.

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` |

<span/>
 

### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |

<span/>

### <a name="request-parameters"></a>Parâmetros solicitados


| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obrigatório. A ID da Loja do aplicativo que contém o pacote de pré-lançamento que você deseja obter. A ID da Loja do aplicativo está disponível no painel do Centro de Desenvolvimento.  |
| flightId | string | Obrigatório. A ID do pacote de pré-lançamento a ser obtida. Essa ID está disponível no painel do Centro de Desenvolvimento, e está incluída nos dados de resposta de solicitações para [criar um pacote de pré-lançamento](create-a-flight.md) e [obter pacotes de pré-lançamento para um aplicativo](get-flights-for-an-app.md).  |

<span/>

### <a name="request-body"></a>Corpo da solicitação

Não forneça um corpo da solicitação para esse método.

<span/>

### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como recuperar informações sobre um pacote de pré-lançamento com a ID 43e448df-97c9-4a43-a0bc-2a445e736bcd para um aplicativo com o valor de ID da Loja 9WZDNCRD91MD.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

O exemplo a seguir demonstra o corpo da resposta JSON para uma chamada bem-sucedida para esse método. Para obter mais detalhes sobre os valores no corpo da resposta, veja as seções a seguir.

```json
{
  "flightId": "43e448df-97c9-4a43-a0bc-2a445e736bcd",
  "friendlyName": "myflight",
  "lastPublishedFlightSubmission": {
    "id": "1152921504621086517",
    "resourceLocation": "flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621086517"
  },
  "pendingFlightSubmission": {
    "id": "115292150462124364",
    "resourceLocation": "flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243647"
  },
  "groupIds": [
    "0"
  ],
  "rankHigherThan": "671c2857-725e-4faf-9e9e-ea1191ef879c"
}
```

### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo   | Descrição                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | string  | A ID do pacote de pré-lançamento. Esse valor é fornecido pelo Centro de Desenvolvimento.  |
| friendlyName           | string  | O nome do pacote de pré-lançamento, conforme especificado pelo desenvolvedor.   |  
| lastPublishedFlightSubmission       | object | Um objeto que fornece informações sobre o último envio publicado do pacote de pré-lançamento. Para saber mais, consulte a seção [Objeto de envio](#submission_object) a seguir.  |
| pendingFlightSubmission        | object  |  Um objeto que fornece informações sobre o envio pendente atual do pacote de pré-lançamento. Para saber mais, consulte a seção [Objeto de envio](#submission_object) a seguir.  |   
| groupIds           | array  | Uma matriz de cadeias de caracteres que contêm as IDs dos grupos de versão de pré-lançamento que estão associados ao pacote de pré-lançamento. Para saber mais sobre grupos de versão de pré-lançamento, consulte [Pacotes de pré-lançamento](https://msdn.microsoft.com/windows/uwp/publish/package-flights).   |
| rankHigherThan           | string  | O nome amigável do pacote de pré-lançamento que ficou imediatamente abaixo do pacote de pré-lançamento atual. Para saber mais sobre a classificação de grupos de versão de pré-lançamento, consulte [Pacotes de pré-lançamento](https://msdn.microsoft.com/windows/uwp/publish/package-flights).  |

<span id="submission_object" />
### <a name="submission-object"></a>Objeto de envio

Os valores *lastPublishedFlightSubmission* e *pendingFlightSubmission* no corpo da resposta contêm objetos que fornecem informações do recurso sobre um envio para o pacote de pré-lançamento. Esses objetos possuem os seguintes valores.

| Valor           | Tipo    | Descrição                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | string  | A ID do envio.    |
| resourceLocation   | string  | Um caminho relativo que você pode acrescentar ao URI básico da solicitação ```https://manage.devcenter.microsoft.com/v1.0/my/``` para recuperar os dados completos do envio.                                                                                                                                               |
 
<span/>

## <a name="error-codes"></a>Códigos de erro

Se não for possível concluir a solicitação, a resposta conterá um dos seguintes códigos de erro HTTP.

| Código de erro |  Descrição     |
|--------|---------------------  |
| 400  | A solicitação é inválida. |
| 404  | O pacote de pré-lançamento especificado não foi encontrado.   |   
| 409  | O aplicativo usa um recurso de painel do Centro de Desenvolvimento que [atualmente não é compatível com a API de envio da Windows Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |                                                                                                 

<span/>

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Criar um pacote de pré-lançamento](create-a-flight.md)
* [Excluir um pacote de pré-lançamento](delete-a-flight.md)
