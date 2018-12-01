---
ms.assetid: 96C090C1-88F8-42E7-AED1-AFA9031E952B
description: Use este método na API de envio da Microsoft Store para excluir um envio de aplicativo existente.
title: Excluir um envio de app
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, envio de aplicativo, excluir
ms.localizationpriority: medium
ms.openlocfilehash: 5d582f79d57fb8b3648d8c872f700d998a2fec1a
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8346354"
---
# <a name="delete-an-app-submission"></a>Excluir um envio de app

Use este método na API de envio da Microsoft Store para excluir um envio de aplicativo existente.

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obrigatório. A ID da Loja do app que contém o envio para excluir. Para saber mais informações sobre a ID da Loja, consulte [Exibir detalhes de identidade de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| submissionId | string | Obrigatório. A ID do envio para excluir. Essa ID está disponível nos dados de resposta para solicitações para [criar um envio de aplicativo](create-an-app-submission.md). Para um envio que foi criado no Partner Center, essa ID também está disponível na URL da página de envio no Partner Center.  |


### <a name="request-body"></a>Corpo da solicitação

Não forneça um corpo da solicitação para esse método.


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como excluir um envio de aplicativo.

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243610 HTTP/1.1
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
| 409  | O envio especificado foi encontrado, mas não pôde ser excluído em seu estado atual ou o aplicativo usa um recurso do Partner Center que está [atualmente não é compatível com a API de envio da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |


## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obter um envio de aplicativo](get-an-app-submission.md)
* [Criar um envio de aplicativo](create-an-app-submission.md)
* [Confirmar um envio de aplicativo](commit-an-app-submission.md)
* [Atualizar um envio de aplicativo](update-an-app-submission.md)
* [Obter o status de um envio de aplicativo](get-status-for-an-app-submission.md)
