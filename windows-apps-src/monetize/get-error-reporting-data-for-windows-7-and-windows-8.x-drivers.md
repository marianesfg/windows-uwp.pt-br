---
author: mcleanbyron
ms.assetid: 2F30E68B-B643-4387-9430-793D08AAF0E7
description: "Use este método na API de análise da Windows Store para obter dados de relatório de erros agregados dos drivers do Windows 7 e Windows 8.x para um determinado intervalo de datas e outros filtros opcionais. Esse método destina-se apenas para IHVs."
title: "Obter dados de relatório de erro para drivers do Windows 7 e Windows 8. x"
ms.author: mcleans
ms.date: 03/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, serviços da Loja, API de análise da Windows Store, erros"
ms.openlocfilehash: bac99a46ccf890437216c60eaf406299e9172d30
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="get-error-reporting-data-for-windows-7-and-windows-8x-drivers"></a>Obter dados de relatório de erro para drivers do Windows 7 e Windows 8. x

Use este método na API de análise da Windows Store para obter dados de relatório de erros agregados dos drivers do Windows 7/Windows 8.x para um determinado intervalo de datas e outros filtros opcionais. Você pode recuperar mais informações de erro usando o método [obter detalhes de um erro de driver do Windows 7 ou Windows 8.x](get-details-for-a-windows-7-or-windows-8.x-driver-error.md).

> [!NOTE]
> Esse método pode ser usado somente por contas de desenvolvedor que pertencem ao [programa do Centro de Desenvolvimento de Hardware do Windows](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard).

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Windows Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/failurehits``` |

<span/> 

### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |

<span/> 

### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Necessário  
|---------------|--------|---------------|------|
| startDate | date | A data de início no intervalo de datas dos dados do relatório de erros a serem recuperados. O padrão é a data atual. |  Não  |
| endDate | date | A data de término no intervalo de datas dos dados do relatório de erros a serem recuperados. O padrão é a data atual. |  Não  |
| top | int | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |  Não  |
| skip | int | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante. |  Não  |
| filter | string  | Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo de resposta e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*. Você pode especificar os campos a seguir do corpo da resposta:<p/><ul><li><strong>date</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li></ul> | Não   |
| aggregationLevel | string | Especifica o intervalo de tempo para o qual recuperar dados agregados. Pode ser uma das seguintes cadeias de caracteres: <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Se não for especificado, o padrão será <strong>day</strong>. Se você especificar <strong>week</strong> ou <strong>month</strong>, os valores <em>failureName</em> e <em>failureHash</em> serão limitados a 1.000 buckets. | Não |
| orderby | string | Uma instrução que ordena os valores dos dados de resultado. A sintaxe é <em>orderby=field [order],field [order],...</em>. Você pode especificar os campos a seguir do corpo da resposta:<p/><ul><li><strong>date</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osName</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li></ul><p>O parâmetro <em>order</em> é opcional, e pode ser <strong>asc</strong> ou <strong>desc</strong> para especificar a ordem crescente ou decrescente de cada campo. O padrão é <strong>asc</strong>.</p><p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,market</em></p> |  Não  |
| groupby | string | Uma instrução que aplica a agregação de dados apenas aos campos especificados. Você pode especificar os campos a seguir do corpo da resposta:<p/><ul><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li></ul><p>As linhas de dados retornados conterão os campos especificados no parâmetro <em>groupby</em>, bem como o seguinte:</p><ul><li><strong>date</strong></li><li><strong>eventCount</strong></li></ul><p>O parâmetro <em>groupby</em> pode ser usado com o parâmetro <em>aggregationLevel</em>. Por exemplo <em>&amp;groupby=failureName,market&amp;aggregationLevel=week</em></p></p> |  Não  |

<span/>


### <a name="request-example"></a>Exemplo de solicitação

Os exemplos a seguir demonstram várias solicitações para a obtenção de dados de relatório de erros de hardware OEM.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/failurehits?startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/failurehits?startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo    | Descrição     |
|------------|---------|--------------|
| Valor      | array   | Uma matriz de objetos que contêm dados de relatório de erros agregados. Para obter mais informações sobre os dados em cada objeto, consulte a tabela a seguir.     |
| @nextLink  | cadeia  | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor será retornado se o parâmetro **top** da solicitação estiver definido como 10.000, mas houver mais de 10.000 linhas de erros para a consulta. |
| TotalCount | inumber | O número total de linhas no resultado dos dados da consulta.     |

<span/>

Os elementos na matriz *Value* contêm os valores a seguir.

| Valor           | Tipo    | Descrição        |
|-----------------|---------|---------------------|
| date            | string  | A primeira data no intervalo de datas dos dados do erro. Se a solicitação especificou um único dia, o valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, o valor será a primeira data nesse intervalo de datas. |
| sellerId   | cadeia  | O valor de ID de vendedor está associado à conta de desenvolvedor (isso corresponde ao valor de **ID de vendedor** nas configurações da conta do Centro de Desenvolvimento). |
| failureName     | string  | O nome do erro.  |
| failureHash     | string  | O identificador exclusivo do erro.   |
| symbol          | string  | O símbolo atribuído a esse erro. |
| osVersion       | cadeia  | A versão de quatro partes do sistema operacional no qual o erro ocorreu.  |
| osName       | cadeia  | O nome do sistema operacional no qual o erro ocorreu.  |
| eventType       | cadeia  | O tipo do erro que ocorreu.      |
| market          | string  | O código de país ISO 3166 do mercado do dispositivo.   |
| deviceType      | string  | O tipo de dispositivo no qual o erro ocorreu.    |
| driverName     | cadeia  | O nome exclusivo do driver associado a esse erro.      |
| driverVersion  | cadeia  | O versão do driver associado a esse erro.   |
| architecture | cadeia |  A arquitetura do dispositivo no qual o erro ocorreu.  |
| oemName | cadeia | O nome do OEM do dispositivo no qual o erro ocorreu. |
| oemModel | cadeia | O nome do modelo do dispositivo no qual o erro ocorreu. |
| flightRing | cadeia | O nome de versão de pré-lançamento do sistema operacional no qual o erro ocorreu. |
| eventCount      | inumber | O número de eventos que são atribuídos a esse erro para o nível de agregação especificado.      |

<span/> 

### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "date": "2017-02-26",
      "sellerId":"14313740",
      "driverName": "fastboot.sys",
      "osVersion": "6.3.9600.9600",
      "osName": "Windows 8.1",
      "flightRing": "Unknown",
      "oemName": "Contoso",
      "oemModel": "C-122",
      "market": "US",
      "symbol": "fastboot",
      "failureName": "AV_fastboot!unknown_function",
      "failureHash": "f060c0b6-fbe6-463f-a9f1-b7ebc1cc722f",
      "architecture": "x64",
      "driverVersion": "2.1.1.0",
      "deviceType": "Unknown",
      "eventType": "System crash",
      "deviceCount": 1.0,
      "eventCount": 1.0
    }]
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Obtenha informações sobre o erro de driver do Windows 7 ou Windows 8. x](get-details-for-a-windows-7-or-windows-8.x-driver-error.md)
* [Baixe o arquivo CAB para um erro de driver do Windows 7 ou Windows 8. x](download-the-cab-file-for-a-windows-7-or-windows-8.x-driver-error.md)
