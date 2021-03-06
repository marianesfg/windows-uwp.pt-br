---
ms.assetid: 8C1E9E36-13AF-4386-9D0F-F9CB320F02F5
description: Use esse método na API de envio a Microsoft Store para criar um voo de pacote para um aplicativo que é registrado em sua conta no Partner Center.
title: Criar um pacote de pré-lançamento
ms.date: 04/16/2018
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, criar versão de pré-lançamento
ms.localizationpriority: medium
ms.openlocfilehash: 0c71dfc05bf2f283652087620848396b731871cd
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371960"
---
# <a name="create-a-package-flight"></a>Criar um pacote de pré-lançamento

Use esse método na API de envio a Microsoft Store para criar um voo de pacote para um aplicativo que é registrado em sua conta no Partner Center.

> [!NOTE]
> Este método cria um pacote de pré-lançamento sem nenhum envio. Para criar um envio para o pacote de pré-lançamento, consulte os métodos em [Gerenciar envios do pacote de pré-lançamento](manage-flight-submissions.md).

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| POSTAR    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parâmetros de solicitação

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | cadeia de caracteres | Obrigatório. A ID da Loja do aplicativo para o qual você deseja criar um pacote de pré-lançamento. Para saber mais informações sobre a ID da Loja, consulte [Exibir detalhes de identidade de aplicativo](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details).  |


### <a name="request-body"></a>Corpo da solicitação

O corpo da solicitação tem os parâmetros a seguir.

|  Parâmetro  |  Tipo  |  Descrição  |  Obrigatório  |
|------|------|------|------|
|  friendlyName  |  cadeia de caracteres  |  O nome do pacote de pré-lançamento, conforme especificado pelo desenvolvedor.  |  Não  |
|  groupIds  |  array  |  Uma matriz de cadeias de caracteres que contêm as IDs dos grupos de versão de pré-lançamento que estão associados ao pacote de pré-lançamento. Para saber mais sobre grupos de versão de pré-lançamento, consulte [Pacotes de pré-lançamento](https://docs.microsoft.com/windows/uwp/publish/package-flights).  |  Não  |
|  rankHigherThan  |  cadeia de caracteres  |  O nome amigável do pacote de pré-lançamento que ficou imediatamente abaixo do pacote de pré-lançamento atual. Se você não definir este parâmetro, o novo pacote de pré-lançamento terá a classificação mais alta de todos os pacotes de pré-lançamento. Para saber mais sobre a classificação de grupos de versão de pré-lançamento, consulte [Pacotes de pré-lançamento](https://docs.microsoft.com/windows/uwp/publish/package-flights).    |  Não  |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como criar um novo pacote de pré-lançamento para um aplicativo que tem a ID da Loja 9WZDNCRD911W.

```syntax
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q...
Content-Type: application/json
{
  "friendlyName": "myflight",
  "groupIds": [
    0
  ],
  "rankHigherThan": null
}

```

## <a name="response"></a>Resposta

O exemplo a seguir demonstra o corpo da resposta JSON para uma chamada bem-sucedida para esse método. Para obter mais detalhes sobre os valores no corpo da resposta, veja as seções a seguir.

```json
{
  "flightId": "43e448df-97c9-4a43-a0bc-2a445e736bcd",
  "friendlyName": "myflight",
  "groupIds": [
    "0"
  ],
  "rankHigherThan": "671c2857-725e-4faf-9e9e-ea1191ef879c"
}
```

### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo   | Descrição                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | cadeia de caracteres  | A ID do pacote de pré-lançamento. Esse valor é fornecido pelo Centro de parceiros.  |
| friendlyName           | cadeia de caracteres  | O nome do pacote de pré-lançamento, conforme especificado na solicitação.   |  
| groupIds           | array  | Uma matriz de cadeias de caracteres que contêm os IDs dos grupos de versão de pré-lançamento que estão associados ao pacote de pré-lançamento, conforme especificado na solicitação. Para saber mais sobre grupos de versão de pré-lançamento, consulte [Pacotes de pré-lançamento](https://docs.microsoft.com/windows/uwp/publish/package-flights).   |
| rankHigherThan           | cadeia de caracteres  | O nome amigável do pacote de pré-lançamento que foi classificado imediatamente abaixo do pacote de pré-lançamento atual, conforme especificado na solicitação. Para saber mais sobre a classificação de grupos de versão de pré-lançamento, consulte [Pacotes de pré-lançamento](https://docs.microsoft.com/windows/uwp/publish/package-flights).  |


## <a name="error-codes"></a>Códigos de erro

Se não for possível concluir a solicitação, a resposta conterá um dos seguintes códigos de erro HTTP.

| Código de erro |  Descrição   |
|--------|------------------|
| 400  | A solicitação é inválida. |
| 409  | O voo de pacote não pôde ser criado devido a seu estado atual ou o aplicativo usa um recurso do Partner Center que está [atualmente não tem suporte da API de envio a Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando os serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtenha um voo de pacote](get-a-flight.md)
* [Excluir um voo de pacote](delete-a-flight.md)
