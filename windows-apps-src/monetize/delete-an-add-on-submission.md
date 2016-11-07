---
author: mcleanbyron
ms.assetid: D677E126-C3D6-46B6-87A5-6237EBEDF1A9
description: "Use este método na API de envio da Windows Store para excluir um envio de complemento existente."
title: Excluir um envio de complemento usando a API de envio da Windows Store
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: b0068876803ea62dbf1d5329ddda19912a4f94d7

---

# Excluir um envio de complemento usando a API de envio da Windows Store




Use este método na API de envio da Windows Store para excluir um envio existente do complemento (também conhecido como produto no app ou IAP).

## Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para o envio da API da Windows Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

>**Observação**&nbsp;&nbsp;Este método só pode ser usado para contas do Centro de Desenvolvimento do Windows as quais foram dadas permissões para usar a API de envio da Windows Store. Nem todas as contas têm essa permissão habilitada.

## Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e de cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}``` |

<span/>
 

### Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |

<span/>

### Parâmetros solicitados

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | string | Obrigatório. A ID da Loja do complemento que contém o envio para excluir. A ID da Loja está disponível no painel do Centro de Desenvolvimento.  |
| submissionId | string | Obrigatório. A ID do envio para excluir. Esse ID está disponível no painel do Centro de Desenvolvimento e está incluído nos dados de resposta para solicitações para [Criar um envio de complemento](create-an-add-on-submission.md).  |

<span/>

### Corpo da solicitação

Não fornece um corpo da solicitação para esse método.

<span/>

### Exemplo de solicitação

O exemplo a seguir demonstra como excluir um envio de complemento.

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621230023 HTTP/1.1
Authorization: Bearer <your access token>
```

## Resposta

Se for bem-sucedida, esse método retorna um corpo de resposta vazia.

## Códigos de erro

Se a solicitação não pode ser concluída com êxito, a resposta conterá um dos seguintes códigos de erro HTTP.

| Código de erro |  Descrição   |
|--------|------------------|
| 400  | Os parâmetros de solicitação não são válidos. |
| 404  | O envio especificado não pôde ser encontrado. |
| 409  | O envio especificado foi encontrado, mas não pôde ser excluído em seu estado atual ou o complemento que usa um recurso de painel do Centro de Desenvolvimento atualmente [não é compatível com a API de envio da Windows Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |

<span/>

## Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obter um envio de complemento](get-an-add-on-submission.md)
* [Criar um envio de complemento](create-an-add-on-submission.md)
* [Confirmar um envio de complemento](commit-an-add-on-submission.md)
* [Atualizar um envio de complemento](update-an-add-on-submission.md)
* [Obter o status de um envio de complemento](get-status-for-an-add-on-submission.md)



<!--HONumber=Aug16_HO5-->

