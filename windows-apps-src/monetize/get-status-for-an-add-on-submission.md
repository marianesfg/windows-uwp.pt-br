---
author: mcleanbyron
ms.assetid: 55315F38-6EC5-4889-A14E-7D8EC282FE98
description: "Use este método na API de envio da Windows Store para obter o status de um envio de complemento."
title: Obter o status de um envio de complemento usando a API de envio da Windows Store
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: e5be4ee466c204255a074adae4d150860286ba0a

---

# Obter o status de um envio de complemento usando a API de envio da Windows Store




Use este método na API de envio da Windows Store para obter o status de um envio de complemento (também conhecido como produto no aplicativo ou IAP) existente. Para obter mais informações sobre o processo de criação de um envio de complemento, usando a API de envio da Windows Store, consulte [Gerenciar envios de complemento](manage-add-on-submissions.md).

## Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Windows Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.
* Crie um envio de complemento para um aplicativo em sua conta do Centro de Desenvolvimento. Você pode fazer isso no painel do Centro de Desenvolvimento ou usando o método [Criar um envio de complemento](create-an-add-on-submission.md).

>**Observação**&nbsp;&nbsp;Este método só pode ser usado para contas do Centro de Desenvolvimento do Windows que receberam permissões para usar a API de envio da Windows Store. Nem todas as contas têm essa permissão habilitada.

## Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}/status``` |

<span/>
 

### Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |

<span/>

### Parâmetros solicitados

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | string | Obrigatório. A ID da Loja do complemento que contém o envio para o qual você deseja obter o status. A ID da Loja está disponível no painel do Centro de Desenvolvimento.  |
| submissionId | string | Obrigatório. A ID do envio para o qual você deseja obter o status. Essa ID está disponível no painel do Centro de Desenvolvimento e está incluída nos dados de resposta de solicitações para [Criar um envio de complemento](create-an-add-on-submission.md).  |

<span/>

### Corpo da solicitação

Não forneça um corpo da solicitação para esse método.

### Exemplo de solicitação

O exemplo a seguir demonstra como obter o status de um envio de complemento.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621243680/status HTTP/1.1
Authorization: Bearer <your access token>
```

## Resposta

O exemplo a seguir demonstra o corpo da resposta JSON para uma chamada bem-sucedida para esse método. O corpo da resposta contém informações sobre o envio especificado. Para obter mais detalhes sobre os valores no corpo da resposta, veja as seções a seguir.

```json
{
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
}
```

### Corpo da resposta

| Valor      | Tipo   | Descrição                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | string  | O status do envio. Ele pode ter um dos seguintes valores: <ul><li>Nenhum</li><li>Cancelado</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publicação</li><li>Publicado</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certificação</li><li>CertificationFailed</li><li>Versão</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | object  |  Contém detalhes adicionais sobre o status do envio, incluindo informações sobre os erros. Para obter mais informações, consulte [Recurso de detalhes sobre o status](manage-add-on-submissions.md#status-details-object). |

<span/>

## Códigos de erro

Se não foi possível concluir a solicitação, a resposta conterá um dos seguintes códigos de erro HTTP.

| Código de erro |  Descrição   |
|--------|------------------|
| 404  | O envio não foi encontrado. |
| 409  | O complemento usa um recurso de painel do Centro de Desenvolvimento que [atualmente não é compatível com a API de envio da Windows Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |

<span/>


## Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obter um envio de complemento](get-an-add-on-submission.md)
* [Criar um envio de complemento](create-an-add-on-submission.md)
* [Confirmar um envio de complemento](commit-an-add-on-submission.md)
* [Atualizar um envio de complemento](update-an-add-on-submission.md)
* [Excluir um envio de complemento](delete-an-add-on-submission.md)



<!--HONumber=Aug16_HO5-->

