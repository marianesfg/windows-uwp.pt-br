---
author: mcleanbyron
ms.assetid: F94AF8F6-0742-4A3F-938E-177472F96C00
description: "Use este método na API de envio da Windows Store para confirmar um envio de pacote de pré-lançamento novo ou atualizado para o Centro de Desenvolvimento do Windows."
title: "Confirmar um envio do pacote de pré-lançamento usando a API de envio da Windows Store"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: a9ea8de7b92b254c7bb8d63a5a3ea41afdd2d966

---

# Confirmar um envio do pacote de pré-lançamento usando a API de envio da Windows Store




Use este método na API de envio da Windows Store para confirmar um envio de pacote de pré-lançamento novo ou atualizado para o Centro de Desenvolvimento do Windows. A ação de confirmação alerta o Centro de Desenvolvimento de que os dados de envio foram carregados (incluindo todos os pacotes relacionados). Em resposta, o Centro de Desenvolvimento confirma as alterações nos dados de envio para inclusão e publicação. Depois que a operação de confirmação for bem-sucedida, as alterações no envio serão mostradas no painel do Centro de Desenvolvimento.

Para obter mais informações sobre como a operação de confirmação se encaixa no processo de criação de um envio de pacote de pré-lançamento usando a API de envio da Windows Store, consulte [Gerenciar envios de pacote de pré-lançamento](manage-flight-submissions.md).

## Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Windows Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.
* [Crie um envio de pacote de pré-lançamento](create-a-flight-submission.md) e, em seguida, [atualize o envio](update-a-flight-submission.md) com as alterações necessárias para os dados de envio.

>**Observação**&nbsp;&nbsp;Este método só pode ser usado para contas do Centro de Desenvolvimento do Windows que receberam permissões para usar a API de envio da Windows Store. Nem todas as contas têm essa permissão habilitada.

## Request

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da Solicitação                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit``` |

<span/>
 

### Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |

<span/>

### Parâmetros solicitados

| Name        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | cadeia de caracteres | Obrigatório. A ID da Loja do aplicativo que contém o envio do pacote de pré-lançamento que você deseja confirmar. A ID da Loja do aplicativo está disponível no painel do Centro de Desenvolvimento.  |
| flightId | cadeia de caracteres | Obrigatório. A ID do pacote de pré-lançamento que contém o envio para confirmar. Essa ID está disponível no painel do Centro de Desenvolvimento, e está incluída nos dados de resposta de solicitações para [criar um pacote de pré-lançamento](create-a-flight.md) e [obter pacotes de pré-lançamento para um aplicativo](get-flights-for-an-app.md).  |
| submissionId | cadeia de caracteres | Obrigatório. A ID do envio para confirmar. Essa ID está disponível no painel do Centro de Desenvolvimento e está incluída nos dados de resposta de solicitações para [criar um envio de pacote de pré-lançamento](create-a-flight-submission.md).  |

<span/>

### Corpo da solicitação

Não forneça um corpo da solicitação para esse método.

### Exemplo de solicitação

O exemplo a seguir demonstra como confirmar o envio de um pacote de pré-lançamento.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649/commit HTTP/1.1
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
| 409  | O envio especificado foi encontrado, mas não pôde ser confirmado em seu estado atual ou o aplicativo que usa um recurso de painel do Centro de Desenvolvimento atualmente [não é compatível com a API de envio da Windows Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |

<span/>


## Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gerenciar envios de pacote de pré-lançamento](manage-flight-submissions.md)
* [Obter um envio de pacote de pré-lançamento](get-a-flight-submission.md)
* [Criar um envio de pacote de pré-lançamento](create-a-flight-submission.md)
* [Atualizar um envio de pacote de pré-lançamento](update-a-flight-submission.md)
* [Excluir um envio de pacote de pré-lançamento](delete-a-flight-submission.md)
* [Obter o status de um envio de pacote de pré-lançamento](get-status-for-a-flight-submission.md)



<!--HONumber=Aug16_HO5-->

