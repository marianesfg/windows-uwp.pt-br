---
author: mcleanbyron
ms.assetid: C78176D6-47BB-4C63-92F8-426719A70F04
description: "Use este método na API de envio da Windows Store para obter o status de um envio de pacote de pré-lançamento."
title: "Obter o status de um envio de pacote de pré-lançamento usando a API de envio da Windows Store"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 9c087a01d8a499f8dd31c428f0c1abdbe9b19fac

---

# Obter o status de um envio de pacote de pré-lançamento usando a API de envio da Windows Store




Use este método na API de envio da Windows Store para obter o status de um envio de pacote de pré-lançamento. Para obter mais informações sobre o processo de criação de um envio de pacote de pré-lançamento usando a API de envio da Windows Store, consulte [Gerenciar envios do pacote de pré-lançamento](manage-flight-submissions.md).

## Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Windows Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.
* Crie um envio de pacote de pré-lançamento de um aplicativo em sua conta do Centro de Desenvolvimento. Você pode fazer isso no painel do Centro de Desenvolvimento ou usando o método [criar um envio de pacote de pré-lançamento](create-a-flight-submission.md).

>**Observação**&nbsp;&nbsp;Este método só pode ser usado para contas do Centro de Desenvolvimento do Windows que receberam permissões para usar a API de envio da Windows Store. Nem todas as contas têm essa permissão habilitada.

## Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions{submissionId}/status``` |

<span/>
 

### Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |

<span/>

### Parâmetros solicitados

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obrigatório. A ID da Loja do aplicativo que contém o envio de pacote de pré-lançamento para o qual você deseja obter o status. Para obter mais informações sobre a ID da Loja, consulte [Exibir detalhes de identidade de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | string | Obrigatório. A ID do pacote de pré-lançamento que contém o envio para o qual você deseja obter o status. Essa ID está disponível no painel do Centro de Desenvolvimento, e está incluída nos dados de resposta de solicitações para [criar um pacote de pré-lançamento](create-a-flight.md) e [obter pacotes de pré-lançamento para um aplicativo](get-flights-for-an-app.md).  |
| submissionId | string | Obrigatório. A ID do envio para o qual você deseja obter o status. Essa ID está disponível no painel do Centro de Desenvolvimento e está incluída nos dados de resposta de solicitações para [criar um envio de pacote de pré-lançamento](create-a-flight-submission.md).  |

<span/>

### Corpo da solicitação

Não forneça um corpo da solicitação para esse método.

### Exemplo de solicitação

O exemplo a seguir demonstra como obter o status de um envio de pacote de pré-lançamento.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649/status HTTP/1.1
Authorization: Bearer <your access token>
```

## Resposta

O exemplo a seguir demonstra o corpo da resposta JSON para uma chamada bem-sucedida para esse método. O corpo da resposta contém informações sobre o envio especificado. Para obter mais detalhes sobre os valores no corpo da resposta, veja a seção a seguir.

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
| statusDetails           | object  |  Contém detalhes adicionais sobre o status do envio, incluindo informações sobre os erros. Para obter mais informações, consulte [Recurso de detalhes sobre o status](manage-flight-submissions.md#status-details-object). |


<span/>

## Códigos de erro

Se não foi possível concluir a solicitação, a resposta conterá um dos seguintes códigos de erro HTTP.

| Código de erro |  Descrição   |
|--------|------------------|
| 404  | O envio não foi encontrado. |
| 409  | O aplicativo usa um recurso de painel do Centro de Desenvolvimento que [atualmente não é compatível com a API de envio da Windows Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |

<span/>


## Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gerenciar envios de pacote de pré-lançamento](manage-flight-submissions.md)
* [Obter um envio de aplicativo](get-an-app-submission.md)
* [Criar um envio de aplicativo](create-an-app-submission.md)
* [Confirmar um envio de aplicativo](commit-an-app-submission.md)
* [Atualizar um envio de aplicativo](update-an-app-submission.md)
* [Excluir um envio de aplicativo](delete-an-app-submission.md)



<!--HONumber=Aug16_HO5-->


