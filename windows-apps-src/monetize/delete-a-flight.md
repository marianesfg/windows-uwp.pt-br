---
author: mcleanbyron
ms.assetid: AD80F9B3-CED0-40BD-A199-AB81CDAE466C
description: "Use este método na API de envio da Windows Store para excluir um pacote de pré-lançamento para um aplicativo que está registrado à sua conta do Centro de Desenvolvimento do Windows."
title: "Exclua um pacote de pré-lançamento usando a API de envio da Windows Store"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: eae25f9a19523b928e30baaf0ffe0eec6e779ed2

---

# Exclua um pacote de pré-lançamento usando a API de envio da Windows Store




Use este método na API de envio da Windows Store para excluir um pacote de pré-lançamento para um aplicativo que está registrado à sua conta do Centro de Desenvolvimento do Windows.


## Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para o envio da API da Windows Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

>**Observação**&nbsp;&nbsp;Este método só pode ser usado para contas do Centro de Desenvolvimento do Windows as quais foram dadas permissões para usar a API de envio da Windows Store. Nem todas as contas têm essa permissão habilitada.

## Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e de cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` |

<span/>
 

### Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |

<span/>

### Parâmetros solicitados

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obrigatório. A ID da Loja do aplicativo que contém o pacote de pré-lançamento que você deseja excluir. A ID da Loja do aplicativo está disponível no painel do Centro de Desenvolvimento.  |
| flightId | string | Obrigatório. A ID do pacote de pré-lançamento para excluir. Esse ID está disponível no painel do Centro de Desenvolvimento, e está incluído nos dados de resposta para solicitações para [criar um pacote de pré-lançamento](create-a-flight.md) e [obter pacotes de pré-lançamento para um aplicativo](get-flights-for-an-app.md).  |

<span/>

### Corpo da solicitação

Não fornece um corpo da solicitação para esse método.

<span/>

### Exemplo de solicitação

O exemplo a seguir demonstra como excluir um pacote de pré-lançamento.

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd HTTP/1.1
Authorization: Bearer <your access token>
```

## Resposta

Se for bem-sucedida, esse método retorna um corpo de resposta vazia.

## Códigos de erro

Se a solicitação não pode ser concluída com êxito, a resposta conterá um dos seguintes códigos de erro HTTP.

| Código de erro |  Descrição                                                                                                                                                                           |
|--------|------------------|
| 400  | Os parâmetros de solicitação não são válidos. |
| 404  | A pacote de pré-lançamento do pacote especificado não pôde ser encontrado.  |
| 409  | A pacote de pré-lançamento do pacote especificada foi encontrada, mas não pôde ser excluída em seu estado atual ou o aplicativo que usa um recurso de painel do Centro de Desenvolvimento atualmente [não é compatível com a API de envio da Windows Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

<span/>

## Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Criar um pacote de pré-lançamento](create-a-flight.md)
* [Obter um pacote de pré-lançamento](get-a-flight.md)



<!--HONumber=Aug16_HO5-->


