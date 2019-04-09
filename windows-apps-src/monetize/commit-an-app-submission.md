---
ms.assetid: 934F2DBF-2C7E-4B77-997D-17B9B0535D51
description: Use esse método na API de envio a Microsoft Store para confirmar o envio de um aplicativo novo ou atualizado Partner Center.
title: Confirmar um envio de aplicativo
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, confirmar envio de aplicativo
ms.localizationpriority: medium
ms.openlocfilehash: 9e44f5672c817f9e1ab00df341a2fd78b23f2944
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334144"
---
# <a name="commit-an-app-submission"></a>Confirmar um envio de aplicativo

Use esse método na API de envio a Microsoft Store para confirmar o envio de um aplicativo novo ou atualizado Partner Center. Confirmar ação alertas Partner Center que os dados de envio foi carregados (incluindo todos os pacotes relacionados e imagens). Em resposta, o Partner Center confirma as alterações nos dados de envio para inclusão e publicação. Depois que a operação de confirmação for bem-sucedida, as alterações para o envio são mostradas no Partner Center.

Para obter mais informações sobre como a operação de confirmação se adapta ao processo de envio de um aplicativo, usando a API de envio da Microsoft Store, consulte [Gerenciar envios de aplicativo](manage-app-submissions.md).

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.
* [Crie um envio de aplicativo](create-an-app-submission.md) e, em seguida, [atualize o envio](update-an-app-submission.md) com as alterações necessárias para os dados de envio.

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| POSTAR    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | cadeia de caracteres | Obrigatório. A ID da Loja do aplicativo que contém o envio que você deseja confirmar. Para obter mais informações sobre a ID da Loja, consulte [Exibir detalhes de identidade do aplicativo](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| submissionId | cadeia de caracteres | Obrigatório. A ID do envio que você deseja confirmar. Esse ID está disponível nos dados de resposta para solicitações para [criar um envio de aplicativo](create-an-app-submission.md). Para um envio que foi criado no Partner Center, essa ID também está disponível na URL para a página de envio no Partner Center.  |

### <a name="request-body"></a>Corpo da solicitação

Não forneça um corpo da solicitação para esse método.

### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como confirmar um envio de aplicativo.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243610/commit HTTP/1.1
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
| status           | cadeia de caracteres  | O status do envio. Isso pode ter um dos seguintes valores: <ul><li>Nenhuma</li><li>Cancelado</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publicação</li><li>Publicado</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certificação</li><li>CertificationFailed</li><li>Versão</li><li>ReleaseFailed</li></ul>  |

## <a name="error-codes"></a>Códigos de erro

Se não for possível concluir a solicitação, a resposta conterá um dos seguintes códigos de erro HTTP.

| Código de erro |  Descrição   |
|--------|------------------|
| 400  | Os parâmetros de solicitação não são válidos. |
| 404  | O envio especificado não pôde ser encontrado. |
| 409  | O envio especificado foi encontrado, mas não pôde ser confirmada em seu estado atual ou o aplicativo usa um recurso do Partner Center que está [atualmente não tem suporte da API de envio a Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando os serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtenha um envio de aplicativo](get-an-app-submission.md)
* [Criar um envio de aplicativo](create-an-app-submission.md)
* [Atualizar um envio de aplicativo](update-an-app-submission.md)
* [Excluir um envio de aplicativo](delete-an-app-submission.md)
* [Obter o status de envio de um aplicativo](get-status-for-an-app-submission.md)
