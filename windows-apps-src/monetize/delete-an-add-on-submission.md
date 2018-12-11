---
ms.assetid: D677E126-C3D6-46B6-87A5-6237EBEDF1A9
description: Use este método na API de envio da Microsoft Store para excluir um envio de complemento existente.
title: Excluir um envio de complemento
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, envio de complemento, excluir, produto no aplicativo, IAP
ms.localizationpriority: medium
ms.openlocfilehash: cdcd74b86ce846f19f8a4eb912781762adb66a2d
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8925767"
---
# <a name="delete-an-add-on-submission"></a>Excluir um envio de complemento

Use este método na API de envio da Microsoft Store para excluir um envio existente do complemento (também conhecido como produto no app ou IAP).

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | string | Obrigatório. A ID da Loja do complemento que contém o envio para excluir. A ID da loja está disponível no Partner Center.  |
| submissionId | string | Obrigatório. A ID do envio para excluir. Essa ID está disponível nos dados de resposta para solicitações para [criar um envio de complemento](create-an-add-on-submission.md). Para um envio que foi criado no Partner Center, essa ID também está disponível na URL da página de envio no Partner Center.  |


### <a name="request-body"></a>Corpo da solicitação

Não forneça um corpo da solicitação para esse método.


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como excluir um envio de complemento.

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621230023 HTTP/1.1
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
| 409  | O envio especificado foi encontrado, mas não pôde ser excluído em seu estado atual ou o complemento usa um recurso do Partner Center que está [atualmente não é compatível com a API de envio da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |


## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obter um envio de complemento](get-an-add-on-submission.md)
* [Criar um envio de complemento](create-an-add-on-submission.md)
* [Confirmar um envio de complemento](commit-an-add-on-submission.md)
* [Atualizar um envio de complemento](update-an-add-on-submission.md)
* [Obter o status de um envio de complemento](get-status-for-an-add-on-submission.md)
