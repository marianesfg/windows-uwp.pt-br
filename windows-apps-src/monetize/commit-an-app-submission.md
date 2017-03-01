---
author: mcleanbyron
ms.assetid: 934F2DBF-2C7E-4B77-997D-17B9B0535D51
description: "Use este método na API de envio da Windows Store para confirmar um envio de app novo ou atualizado para o Centro de Desenvolvimento do Windows."
title: Confirmar um envio de app usando a API de envio da Windows Store
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, API de envio da Windows Store, confirmar envio de app
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: b22143dc9f64e1f1075b0f9a2851699ca4208673
ms.lasthandoff: 02/07/2017

---

# <a name="commit-an-app-submission-using-the-windows-store-submission-api"></a>Confirmar um envio de app usando a API de envio da Windows Store


Use este método na API de envio da Windows Store para confirmar um envio de app novo ou atualizado para o Centro de Desenvolvimento do Windows. A ação de confirmação alerta o Centro de Desenvolvimento de que os dados de envio foram carregados (incluindo todos os pacotes e as imagens relacionados). Em resposta, o Centro de Desenvolvimento confirma as alterações nos dados de envio para inclusão e publicação. Depois que a operação de confirmação for bem-sucedida, as alterações no envio serão mostradas no painel do Centro de Desenvolvimento.

Para obter mais informações sobre como a operação de confirmação se adapta ao processo de envio de um app, usando a API de envio da Windows Store, consulte [Gerenciar envios de app](manage-app-submissions.md).

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Windows Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.
* [Crie um envio de app](create-an-app-submission.md) e, em seguida, [atualize o envio](update-an-app-submission.md) com as alterações necessárias para os dados de envio.

>**Observação**&nbsp;&nbsp;Este método só pode ser usado para contas do Centro de Desenvolvimento do Windows que receberam permissões para usar a API de envio da Windows Store. Nem todas as contas têm essa permissão habilitada.

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da Solicitação                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit``` |

<span/>
 

### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |

<span/>

### <a name="request-parameters"></a>Parâmetros solicitados

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obrigatório. A ID da Loja do app que contém o envio que você deseja confirmar. Para saber mais informações sobre a ID da Loja, consulte [Exibir detalhes de identidade de app](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| submissionId | cadeia de caracteres | Obrigatório. A ID do envio que você deseja confirmar. Essa ID está disponível no painel do Centro de Desenvolvimento e está incluída nos dados de resposta de solicitações para [criar um envio de app](create-an-app-submission.md).  |

<span/>

### <a name="request-body"></a>Corpo da solicitação

Não forneça um corpo da solicitação para esse método.

### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como confirmar um envio de app.

```
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
| status           | string  | O status do envio. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>Cancelado</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publicação</li><li>Publicado</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certificação</li><li>CertificationFailed</li><li>Versão</li><li>ReleaseFailed</li></ul>  |

<span/>

## <a name="error-codes"></a>Códigos de erro

Se não for possível concluir a solicitação, a resposta conterá um dos seguintes códigos de erro HTTP.

| Código de erro |  Descrição   |
|--------|------------------|
| 400  | Os parâmetros de solicitação não são válidos. |
| 404  | O envio especificado não pôde ser encontrado. |
| 409  | O envio especificado foi encontrado, mas não pôde ser confirmado em seu estado atual ou o app que usa um recurso de painel do Centro de Desenvolvimento atualmente [não é compatível com a API de envio da Windows Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |

<span/>


## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obter um envio de app](get-an-app-submission.md)
* [Criar um envio de app](create-an-app-submission.md)
* [Atualizar um envio de app](update-an-app-submission.md)
* [Excluir um envio de app](delete-an-app-submission.md)
* [Obter o status de um envio de app](get-status-for-an-app-submission.md)

