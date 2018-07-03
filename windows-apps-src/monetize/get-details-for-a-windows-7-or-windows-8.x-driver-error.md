---
author: mcleanbyron
ms.assetid: 2FBA0B73-17C6-4F25-A79D-63F2F262491A
description: Use este método na API de análise da Microsoft Store para obter dados detalhados de um erro de driver do Windows 7 ou Windows 8.x. Esse método destina-se apenas para IHVs.
title: Obtenha informações sobre o erro de driver do Windows 7 ou Windows 8. x
ms.author: mcleans
ms.date: 06/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, erros, detalhes
ms.localizationpriority: medium
ms.openlocfilehash: aa3eaf0915ba7e26d2b27b4f21df95fae8a5c0e1
ms.sourcegitcommit: cd91724c9b81c836af4773df8cd78e9f808a0bb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2018
ms.locfileid: "1989480"
---
# <a name="get-details-for-a-windows-7-or-windows-8x-driver-error"></a>Obtenha informações sobre o erro de driver do Windows 7 ou Windows 8. x

Use este método na API de análise da Microsoft Store para obter dados detalhados de um erro de driver específico do Windows 7/Windows 8.x no formato JSON. Antes de usar este método, primeiro você deve usar o método [obter dados de relatório de erros de drivers do Windows 7 e Windows 8.x](get-error-reporting-data-for-windows-7-and-windows-8.x-drivers.md) para recuperar a ID do erro para o qual você deseja obter informações detalhadas.

> [!NOTE]
> Esse método pode ser usado somente por contas de desenvolvedor que pertencem ao [programa do Centro de Desenvolvimento de Hardware do Windows](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard).

## <a name="prerequisites"></a>Pré-requisitos


Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.
* Obtenha a ID do erro para o qual você deseja obter informações detalhadas. Para obter essa ID, use o método [obter dados de relatório de erros de drivers do Windows 7 e Windows 8.x](get-error-reporting-data-for-windows-7-and-windows-8.x-drivers.md) e o valor **failureHash** no corpo da resposta desse método.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/failuredetails``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Necessário  
|---------------|--------|---------------|------|
| failureHash | string | A ID exclusiva do erro para o qual você deseja obter informações detalhadas. Para obter esse valor para o erro no qual você está interessado, use o método [obter dados de relatório de erros de hardware OEM](get-oem-hardware-error-reporting-data.md) e o valor **failureHash** no corpo da resposta desse método. |  Sim  |
| startDate | date | A data de início no intervalo de datas dos dados de erros detalhados a serem recuperados. O padrão é 30 dias antes da data atual. |  Não  |
| endDate | date | A data de término no intervalo de datas dos dados de erros detalhados a serem recuperados. O padrão é a data atual. |  Não  |
| top | int | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |  Não  |
| skip | int | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10 e skip=0 recuperam as primeiras 10 linhas de dados, top=10 e skip=10 recuperam as próximas 10 linhas de dados e assim por diante. |  Não  |
| filter | string  | Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo de resposta e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*. Você pode especificar os campos a seguir do corpo da resposta:<p/><ul><li><strong>date</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osName</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li><li><strong>cabType</strong></li><li><strong>cabExpirationTime</strong></li></ul> | Não   |
| orderby | string | Uma instrução que ordena os valores dos dados de resultado. A sintaxe é <em>orderby=field [order],field [order],....</em>. Você pode especificar os seguintes campos do corpo da resposta:<p/><ul><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>cabType</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osName</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li></ul><p>O parâmetro <em>order</em> é opcional, e pode ser <strong>asc</strong> ou <strong>desc</strong> para especificar a ordem crescente ou decrescente de cada campo. O padrão é <strong>asc</strong>.</p><p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,market</em></p> |  Não  |


### <a name="request-example"></a>Exemplo de solicitação

Os exemplos a seguir demonstram várias solicitações para a obtenção de dados de erros detalhados.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/failuredetails?failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/failuredetails?failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo    | Descrição    |
|------------|---------|------------|
| Valor      | array   | Uma matriz de objetos que contêm dados de erros detalhados. Para obter mais informações sobre os dados em cada objeto, consulte a tabela a seguir.          |
| @nextLink  | cadeia  | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor será retornado se o parâmetro **top** da solicitação estiver definido como 10, mas houver mais de 10 linhas de erros para a consulta. |
| TotalCount | número inteiro | O número total de linhas no resultado dos dados da consulta.        |


Os elementos na matriz *Value* contêm os valores a seguir.

| Valor           | Tipo    | Descrição     |
|-----------------|---------|----------------------------|
| date            | string  | A primeira data no intervalo de datas dos dados do erro. Se a solicitação especificou um único dia, o valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, o valor será a primeira data nesse intervalo de datas. |
| sellerId   | cadeia  | O valor de ID de vendedor está associado à conta de desenvolvedor (isso corresponde ao valor de **ID de vendedor** nas configurações da conta do Centro de Desenvolvimento). |
| failureName     | string  | O nome de falha, que é composto de quatro partes: uma ou mais classes de problema, um código de verificação de bug/exceção, o nome da imagem/driver em que ocorreu a falha e o nome da função associada.           |
| failureHash     | string  | O identificador exclusivo do erro.     |
| symbol     | string  | O símbolo atribuído a esse erro.     |
| osVersion       | cadeia  | A versão de quatro partes do sistema operacional no qual o erro ocorreu.    |
| osName       | cadeia  | O nome do sistema operacional no qual o erro ocorreu.  |
| eventType       | cadeia  | O tipo do erro que ocorreu.      |
| market          | string  | O código de país ISO 3166 do mercado do dispositivo.     |
| deviceType      | string  | O tipo de dispositivo no qual o erro ocorreu.     |
| driverName     | cadeia  | O nome exclusivo do driver associado a esse erro.      |
| driverVersion  | cadeia  | O versão do driver associado a esse erro.   |
| architecture | cadeia |  A arquitetura do dispositivo no qual o erro ocorreu.  |
| oemName | cadeia | O nome do OEM do dispositivo no qual o erro ocorreu. |
| oemModel | cadeia | O nome do modelo do dispositivo no qual o erro ocorreu. |
| flightRing | cadeia | O nome de versão de pré-lançamento do sistema operacional no qual o erro ocorreu. |
| clientDeviceId | cadeia | A ID do dispositivo no qual o erro ocorreu. |
| cabIdHash         | string  | A ID exclusiva do arquivo CAB que está associado a esse erro.   |
| cabType         | cadeia  | O tipo do arquivo CAB.   |
| cabExpirationTime  | string  | A data e a hora em que o arquivo CAB expirou e não pôde mais ser baixado, no formato ISO 8601.   |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "sellerId": "14313740",
      "architecture": "x64",
      "cabExpirationTime": "2017-06-12 23:51:11",
      "cabIdHash": "cc1797f9-b783-44d4-b0e5-46ae7ca668bc",
      "cabType": "MINI",
      "clientDeviceId": null,
      "date": "2017-03-14 23:51:11",
      "deviceType": "PC",
      "driverName": "fastboot.sys",
      "driverVersion": "2.1.1.0",
      "failureHash": "f060c0b6-fbe6-463f-a9f1-b7ebc1cc722f",
      "failureName": "0x109_18_2_LoadImageNotify",
      "market": "US",
      "oemModel": "C-122",
      "oemName": "Contoso",
      "osName": "Windows 8.1",
      "osVersion": "6.3.9600.9600"
    }]
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Obter dados de relatório de erro para drivers do Windows 7 e Windows 8. x](get-error-reporting-data-for-windows-7-and-windows-8.x-drivers.md)
* [Baixe o arquivo CAB para um erro de driver do Windows 7 ou Windows 8. x](download-the-cab-file-for-a-windows-7-or-windows-8.x-driver-error.md)
