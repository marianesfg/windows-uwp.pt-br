---
author: mcleanbyron
description: "Use esse método na API de envio da Windows Store a fim de parar a distribuição de pacote para um envio de aplicativo."
title: "Parar a distribuição de pacote para um envio de aplicativo usando a API de envio da Windows Store"
translationtype: Human Translation
ms.sourcegitcommit: 9b76a11adfab838b21713cb384cdf31eada3286e
ms.openlocfilehash: b1828b1add220ae7eecf72c406fe5250dbe873de

---

# Parar a distribuição de pacote para um envio de aplicativo usando a API de envio da Windows Store


Use esse método na API de envio da Windows Store a fim de [parar a distribuição de pacote](../publish/gradual-package-rollout.md#completing-the-rollout) para um envio de aplicativo. Para obter mais informações sobre o processo de criação de um envio de aplicativo, usando a API de envio da Windows Store, consulte [Gerenciar envios de aplicativo](manage-app-submissions.md).


## Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Windows Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.
* Crie um envio de aplicativo para um aplicativo em sua conta do Centro de Desenvolvimento. Você pode fazer isso no painel do Centro de Desenvolvimento ou usando o método [criar um envio de aplicativo](create-an-app-submission.md).
* Habilite uma distribuição de pacote gradual para o envio. Você pode fazer isso no [painel do Centro de Desenvolvimento](../publish/gradual-package-rollout.md) ou [usando a API de envio da Windows Store](manage-app-submissions.md#manage-gradual-package-rollout).

>**Observação**&nbsp;&nbsp;Este método só pode ser usado para contas do Centro de Desenvolvimento do Windows que receberam permissões para usar a API de envio da Windows Store. Nem todas as contas têm essa permissão habilitada.

## Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições dos parâmetros do cabeçalho e da solicitação.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout``` |

<span/>
 

### Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |

<span/>

### Parâmetros solicitados

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Necessário. A ID da Loja do aplicativo que contém o envio com a porcentagem da distribuição de pacote que você deseja parar. Para obter mais informações sobre a ID da Loja, consulte [Exibir detalhes de identidade de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| submissionId | string | Necessário. A ID do envio com a distribuição de pacote que você deseja parar. Essa ID está disponível no painel do Centro de Desenvolvimento e está incluída nos dados de resposta de solicitações para [criar um envio de aplicativo](create-an-app-submission.md).  |

<span/>

### Corpo da solicitação

Não forneça um corpo da solicitação para esse método.

### Exemplo de solicitação

O exemplo a seguir demonstra como parar a distribuição de pacote para um envio de aplicativo.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243680/haltpackagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## Resposta

O exemplo a seguir demonstra o corpo da resposta JSON para uma chamada bem-sucedida para esse método. Para obter mais detalhes sobre os valores no corpo da resposta, consulte [Recurso da distribuição de pacote](manage-app-submissions.md#package-rollout-object).

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 0,
    "packageRolloutStatus": "PackageRolloutStopped",
    "fallbackSubmissionId": "1212922684621243058"
}
```

## Códigos de erro

Se a solicitação não puder ser concluída com êxito, a resposta conterá um dos códigos de erro HTTP a seguir.

| Código de erro |  Descrição   |
|--------|------------------|
| 404  | O envio não foi encontrado. |
| 409  | Este código indica um dos seguintes erros:<br/><br/><ul><li>O envio não está em um estado válido para a operação de distribuição gradual (antes de chamar esse método, o envio deve ser publicado, e o valor [packageRolloutStatus](manage-app-submissions.md#package-rollout-object) deve ser definido como **PackageRolloutInProgress**).</li><li>O envio não pertence ao aplicativo especificado.</li><li>O aplicativo usa um recurso de painel do Centro de Desenvolvimento que [atualmente não é compatível com a API de envio da Windows Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).</li></ul> |   

<span/>


## Tópicos relacionados

* [Distribuição de pacote gradual](../publish/gradual-package-rollout.md)
* [Gerenciar envios de aplicativo usando a API de envio da Windows Store](manage-app-submissions.md)
* [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md)



<!--HONumber=Nov16_HO1-->


