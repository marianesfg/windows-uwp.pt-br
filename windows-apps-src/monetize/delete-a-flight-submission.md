---
author: mcleanbyron
ms.assetid: 1A69A388-B1CC-4D2C-886B-EA07E6E60252
description: "Use este método na API de envio da Windows Store para excluir um envio de pacote de pré-lançamento existente do pacote."
title: "Excluir um envio de pacote de pré-lançamento"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, API de envio da Windows Store, envio de versão de pré-lançamento, excluir, pacote de pré-lançamento"
ms.openlocfilehash: 1b69fb04742e30a80b258613385b40ce8999306c
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="delete-a-package-flight-submission"></a>Excluir um envio de pacote de pré-lançamento




Use este método na API de envio da Windows Store para excluir um envio de pacote de pré-lançamento existente do pacote.

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Windows Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

>**Observação**&nbsp;&nbsp;Este método só pode ser usado para contas do Centro de Desenvolvimento do Windows que receberam permissões para usar a API de envio da Windows Store. Nem todas as contas têm essa permissão habilitada.

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/flights/{flightId}/submissions/{submissionId}``` |

<span/>
 

### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |

<span/>

### <a name="request-parameters"></a>Parâmetros solicitados

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obrigatório. A ID da Loja do aplicativo que contém o envio do pacote de pré-lançamento que você deseja excluir. Para saber mais informações sobre a ID da Loja, consulte [Exibir detalhes de identidade de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | string | Obrigatório. A ID do pacote de pré-lançamento que contém o envio para excluir. Esse ID está disponível no painel do Centro de Desenvolvimento e está incluído nos dados de resposta para solicitações para [criar um pacote de pré-lançamento](create-a-flight.md) e [obter pacotes de pré-lançamento para um aplicativo](get-flights-for-an-app.md).  |
| submissionId | string | Obrigatório. A ID do envio para excluir. Esse ID está disponível no painel do Centro de Desenvolvimento e está incluído nos dados de resposta para solicitações para [criar um envio de pacote de pré-lançamento do pacote](create-a-flight-submission.md).  |

<span/>

### <a name="request-body"></a>Corpo da solicitação

Não forneça um corpo da solicitação para esse método.

<span/>

### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como excluir um envio para um pacote de pré-lançamento.

```
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
| 409  | O envio especificado foi encontrado, mas não pôde ser excluído em seu estado atual ou o aplicativo que usa um recurso de painel do Centro de Desenvolvimento atualmente [não é compatível com a API de envio da Windows Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |

<span/>

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gerenciar envios de pacote de pré-lançamento](manage-flight-submissions.md)
* [Obter um envio de pacote de pré-lançamento](get-a-flight-submission.md)
* [Criar um envio de pacote de pré-lançamento](create-a-flight-submission.md)
* [Confirmar um envio de pacote de pré-lançamento](commit-a-flight-submission.md)
* [Atualizar um envio de pacote de pré-lançamento](update-a-flight-submission.md)
* [Obter o status de um envio de pacote de pré-lançamento](get-status-for-a-flight-submission.md)
