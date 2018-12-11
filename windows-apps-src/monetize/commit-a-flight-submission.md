---
ms.assetid: F94AF8F6-0742-4A3F-938E-177472F96C00
description: Use este método na API de envio da Microsoft Store para confirmar um envio de versão de pré-lançamento do pacote novo ou atualizado para o Partner Center.
title: Confirmar um envio de pacote de pré-lançamento
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, confirmar envio de versão de pré-lançamento
ms.localizationpriority: medium
ms.openlocfilehash: 820e10695cce2d6242a51b0017d2fe3981cf77b1
ms.sourcegitcommit: 231065c899d0de285584d41e6335251e0c2c4048
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8828794"
---
# <a name="commit-a-package-flight-submission"></a>Confirmar um envio de pacote de pré-lançamento

Use este método na API de envio da Microsoft Store para confirmar um envio de versão de pré-lançamento do pacote novo ou atualizado para o Partner Center. Confirmação ação alertas Partner Center que os dados de envio foram carregados (incluindo todos os pacotes relacionados). Em resposta, o Partner Center confirma as alterações nos dados de envio para inclusão e publicação. Depois que a operação de confirmação for bem-sucedida, as alterações no envio são mostradas no Partner Center.

Para obter mais informações sobre como a operação de confirmação se encaixa no processo de criação de um envio de pacote de pré-lançamento usando a API de envio da Microsoft Store, consulte [Gerenciar envios de pacote de pré-lançamento](manage-flight-submissions.md).

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.
* [Crie um envio de pacote de pré-lançamento](create-a-flight-submission.md) e, em seguida, [atualize o envio](update-a-flight-submission.md) com as alterações necessárias para os dados de envio.

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obrigatório. A ID da Loja do aplicativo que contém o envio do pacote de pré-lançamento que você deseja confirmar. A ID da loja para o aplicativo está disponível no Partner Center.  |
| flightId | string | Obrigatório. A ID do pacote de pré-lançamento que contém o envio para confirmar. Essa ID está disponível nos dados de resposta de solicitações para [criar um pacote de pré-lançamento](create-a-flight.md) e [obter pacotes de pré-lançamento para um app](get-flights-for-an-app.md). Para uma versão de pré-lançamento que foi criado no Partner Center, essa ID também está disponível na URL da página de pré-lançamento no Partner Center.  |
| submissionId | string | Obrigatório. A ID do envio para confirmar. Esse ID está disponível nos dados de resposta para solicitações para [criar um envio de pacote de pré-lançamento](create-a-flight-submission.md). Para um envio que foi criado no Partner Center, essa ID também está disponível na URL da página de envio no Partner Center.  |


### <a name="request-body"></a>Corpo da solicitação

Não forneça um corpo da solicitação para esse método.

### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como confirmar o envio de um pacote de pré-lançamento.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649/commit HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

O exemplo a seguir demonstra o corpo da resposta JSON para uma chamada bem-sucedida para esse método. Para obter mais detalhes sobre os valores no corpo da resposta, veja as seções a seguir.

```json
{
  "status": "CommitStarted"
}
```

### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo   | Descrição                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | string  | O status do envio. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>Cancelado</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publicação</li><li>Publicado</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certificação</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>  |


## <a name="error-codes"></a>Códigos de erro

Se não for possível concluir a solicitação, a resposta conterá um dos seguintes códigos de erro HTTP.

| Código de erro |  Descrição   |
|--------|------------------|
| 400  | Os parâmetros de solicitação não são válidos. |
| 404  | O envio especificado não pôde ser encontrado. |
| 409  | O envio especificado foi encontrado, mas não pôde ser confirmado em seu estado atual ou o aplicativo usa um recurso do Partner Center que está [atualmente não é compatível com a API de envio da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |


## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gerenciar envios de pacote de pré-lançamento](manage-flight-submissions.md)
* [Obter um envio de pacote de pré-lançamento](get-a-flight-submission.md)
* [Criar um envio de pacote de pré-lançamento](create-a-flight-submission.md)
* [Atualizar um envio de pacote de pré-lançamento](update-a-flight-submission.md)
* [Excluir um envio de pacote de pré-lançamento](delete-a-flight-submission.md)
* [Obter o status de um envio de pacote de pré-lançamento](get-status-for-a-flight-submission.md)
