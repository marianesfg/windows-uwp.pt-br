---
author: mcleanbyron
ms.assetid: AC74B4FA-5554-4C03-9683-86EE48546C05
description: "Use este método na API de envio da Windows Store para confirmar um envio de complemento novo ou atualizado para o Centro de Desenvolvimento do Windows."
title: Confirmar um envio de complemento usando a API de envio da Windows Store
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: f9b9e5801f94101156850086c16311cf567b1e7d

---

# Confirmar um envio de complemento usando a API de envio da Windows Store




Use este método na API de envio da Windows Store para confirmar um envio de complemento novo ou atualizado (também conhecido como produto no aplicativo ou IAP) para o Centro de Desenvolvimento do Windows. A ação de confirmação alerta o Centro de Desenvolvimento de que os dados de envio foram carregados (incluindo todos os ícones relacionados). Em resposta, o Centro de Desenvolvimento confirma as alterações nos dados de envio para inclusão e publicação. Depois que a operação de confirmação for bem-sucedida, as alterações no envio serão mostradas no painel do Centro de Desenvolvimento.

Para obter mais informações sobre como a operação de confirmação se adapta ao processo de envio de um complemento, usando a API de envio da Windows Store, consulte [Gerenciar envios de complemento](manage-add-on-submissions.md).

## Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Windows Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.
* [Crie um envio de complemento](create-an-add-on-submission.md) e, em seguida, [atualize o envio](update-an-add-on-submission.md) com as alterações necessárias para os dados de envio.

>**Observação**&nbsp;&nbsp;Este método só pode ser usado para contas do Centro de Desenvolvimento do Windows que receberam permissões para usar a API de envio da Windows Store. Nem todas as contas têm essa permissão habilitada.

## Request

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da Solicitação                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}/commit``` |

<span/>
 

### Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |

<span/>

### Parâmetros solicitados

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | cadeia de caracteres | Obrigatório. A ID da Loja do complemento que contém o envio que você deseja confirmar. A ID da Loja está disponível no painel do Centro de Desenvolvimento, e ela está incluída nos dados de resposta de solicitações para [Obter todos os complementos](get-all-add-ons.md) e [Criar um complemento](create-an-add-on.md). |
| submissionId | cadeia de caracteres | Obrigatório. A ID do envio que você deseja confirmar. Essa ID está disponível no painel do Centro de Desenvolvimento e está incluída nos dados de resposta de solicitações para [Criar um envio de complemento](create-an-add-on-submission.md).  |

<span/>

### Corpo da solicitação

Não forneça um corpo da solicitação para esse método.

### Exemplo de solicitação

O exemplo a seguir demonstra como confirmar um envio de complemento.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621230023/commit HTTP/1.1
Authorization: Bearer <your access token>
```

## Resposta

O exemplo a seguir demonstra o corpo da resposta JSON para uma chamada bem-sucedida para esse método. Para obter mais detalhes sobre os valores no corpo da resposta, veja as seções a seguir.

```json
{
  "status": "CommitStarted"
}
```

### Corpo da resposta

| Valor      | Tipo   | Descrição                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | cadeia de caracteres  | O status do envio. Ele pode ter um dos seguintes valores: <ul><li>Nenhum</li><li>Cancelado</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publicação</li><li>Publicado</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certificação</li><li>CertificationFailed</li><li>Versão</li><li>ReleaseFailed</li></ul>  |

<span/>

## Códigos de erro

Se não for possível concluir a solicitação, a resposta conterá um dos seguintes códigos de erro HTTP.

| Código de erro |  Descrição   |
|--------|------------------|
| 400  | Os parâmetros de solicitação não são válidos. |
| 404  | O envio especificado não pôde ser encontrado. |
| 409  | O envio especificado foi encontrado, mas não pôde ser confirmado em seu estado atual ou o complemento que usa um recurso de painel do Centro de Desenvolvimento atualmente [não é compatível com a API de envio da Windows Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |

<span/>


## Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obter um envio de complemento](get-an-add-on-submission.md)
* [Criar um envio de complemento](create-an-add-on-submission.md)
* [Atualizar um envio de complemento](update-an-add-on-submission.md)
* [Excluir um envio de complemento](delete-an-add-on-submission.md)
* [Obter o status de um envio de complemento](get-status-for-an-add-on-submission.md)



<!--HONumber=Aug16_HO5-->


