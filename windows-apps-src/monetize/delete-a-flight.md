---
author: mcleanbyron
ms.assetid: AD80F9B3-CED0-40BD-A199-AB81CDAE466C
description: Use este método na API de envio da Microsoft Store para excluir um pacote de pré-lançamento para um aplicativo que está registrado à sua conta do Centro de Desenvolvimento do Windows.
title: Excluir um pacote de pré-lançamento
ms.author: mcleans
ms.date: 04/17/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, API de envio da Microsoft Store, excluir versão de pré-lançamento
ms.localizationpriority: medium
ms.openlocfilehash: d907d87929a64178b3ebd3d169e30b04f6577eb4
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1815501"
---
# <a name="delete-a-package-flight"></a>Excluir um pacote de pré-lançamento

Use este método na API de envio da Microsoft Store para excluir um pacote de pré-lançamento para um aplicativo que está registrado à sua conta do Centro de Desenvolvimento do Windows.


## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obrigatório. A ID da Loja do aplicativo que contém o pacote de pré-lançamento que você deseja excluir. A ID da Loja do aplicativo está disponível no painel do Centro de Desenvolvimento.  |
| flightId | string | Obrigatório. A ID do pacote de pré-lançamento para excluir. Essa ID está disponível nos dados de resposta de solicitações para [criar um pacote de pré-lançamento](create-a-flight.md) e [obter pacotes de pré-lançamento para um aplicativo](get-flights-for-an-app.md). Para uma versão de pré-lançamento criada no painel do Centro de Desenvolvimento, essa ID também está disponível na URL da página de pré-lançamento no painel.  |


### <a name="request-body"></a>Corpo da solicitação

Não forneça um corpo da solicitação para esse método.


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como excluir um pacote de pré-lançamento.

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

Se for bem-sucedida, esse método retorna um corpo de resposta vazia.

## <a name="error-codes"></a>Códigos de erro

Se não for possível concluir a solicitação, a resposta conterá um dos seguintes códigos de erro HTTP.

| Código de erro |  Descrição                                                                                                                                                                           |
|--------|------------------|
| 400  | Os parâmetros de solicitação não são válidos. |
| 404  | O pacote de pré-lançamento especificado não foi encontrado.  |
| 409  | A pacote de pré-lançamento do pacote especificada foi encontrada, mas não pôde ser excluída em seu estado atual ou o aplicativo que usa um recurso de painel do Centro de Desenvolvimento atualmente [não é compatível com a API de envio da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Criar um pacote de pré-lançamento](create-a-flight.md)
* [Obter um pacote de pré-lançamento](get-a-flight.md)