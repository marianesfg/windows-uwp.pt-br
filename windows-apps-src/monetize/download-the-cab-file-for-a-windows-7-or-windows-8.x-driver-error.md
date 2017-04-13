---
author: mcleanbyron
ms.assetid: 48D891CD-706C-4759-AB33-B0663774A829
description: "Use este método na API de análise da Windows Store para baixar o arquivo CAB para um erro de driver do Windows 7 ou Windows 8. x. Esse método destina-se apenas para IHVs."
title: Baixe o arquivo CAB para um erro de driver do Windows 7 ou Windows 8. x
ms.author: mcleans
ms.date: 03/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, API de análise da Windows Store, baixar CAB"
ms.openlocfilehash: 622ad656a64381e80549bc286a4d30490e488631
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="download-the-cab-file-for-a-windows-7-or-windows-8x-driver-error"></a>Baixe o arquivo CAB para um erro de driver do Windows 7 ou Windows 8. x

Use este método na API de análise da Windows Store para baixar o arquivo CAB associado a um erro de driver específico do Windows 7/Windows 8. x. Antes de usar esse método, primeiro você deve usar o método [obter detalhes de um erro de driver do Windows 7 ou Windows 8. x](get-details-for-a-windows-7-or-windows-8.x-driver-error.md) para recuperar a ID do arquivo CAB você deseja baixar.

Você pode obter outras informações sobre os erros de driver do Windows 7/Windows 8. x usando os métodos [obter dados do relatório de erros de drivers do Windows 7 e Windows 8. x](get-error-reporting-data-for-windows-7-and-windows-8.x-drivers.md) e [obter detalhes de um erro de driver do Windows 7 ou Windows 8. x](get-details-for-a-windows-7-or-windows-8.x-driver-error.md) na API de análise da Windows Store.

> [!NOTE]
> Esse método pode ser usado somente por contas de desenvolvedor que pertencem ao [programa do Centro de Desenvolvimento de Hardware do Windows](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard).

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Windows Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.
* Obtenha a ID do arquivo CAB que você deseja baixar. Para obter essa ID, use o método [obter detalhes de um erro de driver do Windows 7 ou Windows 8.x](get-details-for-a-windows-7-or-windows-8.x-driver-error.md) para recuperar os detalhes de um erro de driver e utilizar o valor **cabldHash** no corpo da resposta desse método.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/cabdownload``` |

<span/> 

### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |

<span/> 

### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  |
|---------------|--------|---------------|------|
| cabIdHash | cadeia | A ID exclusiva do arquivo CAB que você deseja baixar. Para obter essa ID, use o método [obter detalhes de um erro de driver do Windows 7 ou Windows 8.x](get-details-for-a-windows-7-or-windows-8.x-driver-error.md) para recuperar os detalhes de um erro específico no aplicativo e utilizar o valor **cabldHash** no corpo da resposta desse método. |  Sim  |

<span/>
 
### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como obter baixar um arquivo CAB usando esse método.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/cabdownload?cabIdHash=c1a51104-d682-4230-be14-7278b18e3555 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

Esse método retorna um código de resposta 302 (redirecionamento) e o cabeçalho **Local** na resposta é atribuído à URI de assinatura do acesso compartilhado (SAS) do arquivo CAB. O chamador é redirecionado para essa URI a fim de baixar automaticamente o arquivo CAB.

## <a name="related-topics"></a>Tópicos relacionados

* [Obter dados de relatório de erro para drivers do Windows 7 e Windows 8. x](get-error-reporting-data-for-windows-7-and-windows-8.x-drivers.md)
* [Obtenha informações sobre o erro de driver do Windows 7 ou Windows 8. x](get-details-for-a-windows-7-or-windows-8.x-driver-error.md)
