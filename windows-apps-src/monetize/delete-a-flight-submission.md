---
ms.assetid: 1A69A388-B1CC-4D2C-886B-EA07E6E60252
description: Use este método na API de envio da Microsoft Store para excluir um envio de pacote de pré-lançamento existente do pacote.
title: Excluir um envio de pacote de pré-lançamento
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, envio de versão de pré-lançamento, excluir, pacote de pré-lançamento
ms.localizationpriority: medium
ms.openlocfilehash: 80bb195f98487fbbf31f64b24dc8eef9ed089813
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372477"
---
# <a name="delete-a-package-flight-submission"></a>Excluir um envio de pacote de pré-lançamento

Use este método na API de envio da Microsoft Store para excluir um envio de pacote de pré-lançamento existente do pacote.

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/flights/{flightId}/submissions/{submissionId}` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | cadeia de caracteres | Obrigatório. A ID da Loja do aplicativo que contém o envio do pacote de pré-lançamento que você deseja excluir. Para obter mais informações sobre a ID da Loja, consulte [Exibir detalhes de identidade do aplicativo](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | cadeia de caracteres | Obrigatório. A ID do pacote de pré-lançamento que contém o envio para excluir. Essa ID está disponível nos dados de resposta de solicitações para [criar um pacote de pré-lançamento](create-a-flight.md) e [obter pacotes de pré-lançamento para um aplicativo](get-flights-for-an-app.md). Para um voo que foi criado no Partner Center, essa ID também está disponível na URL para a página de voo no Partner Center.  |
| submissionId | cadeia de caracteres | Obrigatório. A ID do envio para excluir. Esse ID está disponível nos dados de resposta para solicitações para [criar um envio de pacote de pré-lançamento](create-a-flight-submission.md). Para um envio que foi criado no Partner Center, essa ID também está disponível na URL para a página de envio no Partner Center.  |


### <a name="request-body"></a>Corpo da solicitação

Não forneça um corpo da solicitação para esse método.


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como excluir um envio para um pacote de pré-lançamento.

```json
DELETE https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

Se for bem-sucedida, esse método retorna um corpo de resposta vazia.

## <a name="error-codes"></a>Códigos de erro

Se não for possível concluir a solicitação, a resposta conterá um dos seguintes códigos de erro HTTP.

| Código de erro |  Descrição   |
|--------|------------------|
| 400  | Os parâmetros de solicitação não são válidos. |
| 404  | O envio especificado não pôde ser encontrado. |
| 409  | O envio especificado foi encontrado, mas não pôde ser excluído em seu estado atual ou o aplicativo usa um recurso do Partner Center que está [atualmente não tem suporte da API de envio a Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |


## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando os serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gerenciar envios de voo do pacote](manage-flight-submissions.md)
* [Obter o envio de voo de um pacote](get-a-flight-submission.md)
* [Criar um envio de voo do pacote](create-a-flight-submission.md)
* [O envio de voo de um pacote de confirmação](commit-a-flight-submission.md)
* [O envio de voo de um pacote de atualização](update-a-flight-submission.md)
* [Obter o status de envio de voo de um pacote](get-status-for-a-flight-submission.md)
