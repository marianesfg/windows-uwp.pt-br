---
author: mcleanbyron
ms.assetid: 3D6EE7D7-7D75-499D-AA7A-55DA1C485BA6
description: Use este método na API de análise da Microsoft Store a fim de baixar o arquivo CAB para um erro de driver do Windows 10. Esse método destina-se apenas para IHVs.
title: Baixe o arquivo CAB para um erro de driver do Windows 10
ms.author: mcleans
ms.date: 03/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, API de análise da Microsoft Store, baixar CAB
ms.localizationpriority: medium
ms.openlocfilehash: 98d83dd6bbaeb67f4601315dbb92b1d2cf8dfd23
ms.sourcegitcommit: 1773bec0f46906d7b4d71451ba03f47017a87fec
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/17/2018
ms.locfileid: "1664506"
---
# <a name="download-the-cab-file-for-a-windows-10-driver-error"></a>Baixe o arquivo CAB para um erro de driver do Windows 10

Use este método na API de análise da Microsoft Store a fim de baixar o arquivo CAB associado a um erro de driver específico do Windows 10. Antes de usar esse método, primeiro você deve usar o método [obter detalhes de um erro de driver do Windows 10](get-details-for-a-windows-10-driver-error.md) para recuperar a ID do arquivo CAB você deseja baixar.

Você pode obter outras informações sobre erros de hardware OEM usando os métodos [obter dados de relatório de erro do Windows 10](get-error-reporting-data-for-windows-10-drivers.md) e [obter detalhes de um erro de driver do Windows 10](get-details-for-a-windows-10-driver-error.md) na API de análise da Microsoft Store.

> [!NOTE]
> Esse método pode ser usado somente por contas de desenvolvedor que pertencem ao [programa do Centro de Desenvolvimento de Hardware do Windows](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard).

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.
* Obtenha a ID do arquivo CAB que você deseja baixar. Para obter essa ID, use o método [obter detalhes de um erro de driver do Windows 10](get-details-for-a-windows-10-driver-error.md) para recuperar os detalhes de um erro de driver e utilizar o valor **cabldHash** no corpo da resposta desse método.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/driver/cabdownload``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  |
|---------------|--------|---------------|------|
| applicationId | cadeia | O valor da ID do produto do driver para o qual você deseja recuperar dados de relatório de erros. |  Sim  |
| cabIdHash | cadeia | A ID exclusiva do arquivo CAB que você deseja baixar. Para obter essa ID, use o método [obter detalhes de um erro de driver do Windows 10](get-details-for-a-windows-10-driver-error.md) para recuperar os detalhes de um erro específico no seu aplicativo e utilizar o valor **cabldHash** no corpo da resposta desse método. |  Sim  |

 
### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como obter baixar um arquivo CAB usando esse método.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/driver/cabdownload?applicationId=18430592857500421&cabIdHash=c1a51104-d682-4230-be14-7278b18e3555 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

Esse método retorna um código de resposta 302 (redirecionamento) e o cabeçalho **Local** na resposta é atribuído à URI de assinatura do acesso compartilhado (SAS) do arquivo CAB. O chamador é redirecionado para essa URI a fim de baixar automaticamente o arquivo CAB.

## <a name="related-topics"></a>Tópicos relacionados

* [Obter dados de relatório de erro para drivers do Windows 10](get-error-reporting-data-for-windows-10-drivers.md)
* [Obter detalhes sobre um erro de driver do Windows 10](get-details-for-a-windows-10-driver-error.md)
