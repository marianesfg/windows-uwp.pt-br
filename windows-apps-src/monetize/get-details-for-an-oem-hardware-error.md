---
author: mcleanbyron
ms.assetid: 8425F704-8A03-493F-A3D2-8442E85FD835
description: Use este método na API de análise da Microsoft Store para obter dados detalhados de um erro específico de hardware. Esse método destina-se apenas para OEMs.
title: Obter detalhes de um erro de hardware OEM
ms.author: mcleans
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, erros, detalhes
ms.localizationpriority: medium
ms.openlocfilehash: 429ebc5237ce35baa6f9c3f31a25d480410d9c86
ms.sourcegitcommit: 1773bec0f46906d7b4d71451ba03f47017a87fec
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/17/2018
ms.locfileid: "1663206"
---
# <a name="get-details-for-an-oem-hardware-error"></a>Obter detalhes de um erro de hardware OEM

Use este método na API de análise da Microsoft Store para obter dados detalhados de um erro específico de hardware OEM no formato JSON. Antes de usar este método, primeiro você deve usar o método [obter dados de relatório de erros do hardware OEM](get-oem-hardware-error-reporting-data.md) para recuperar a ID do erro para o qual você deseja obter informações detalhadas.

> [!NOTE]
> Esse método pode ser usado somente por contas de desenvolvedor que pertencem ao [programa do Centro de Desenvolvimento de Hardware do Windows](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard).

## <a name="prerequisites"></a>Pré-requisitos


Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.
* Obtenha a ID do erro para o qual você deseja obter informações detalhadas. Para obter essa ID, use o método [obter dados de relatório de erros de hardware OEM](get-oem-hardware-error-reporting-data.md) e o valor **failureHash** no corpo da resposta desse método.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/hardware/failuredetails``` |


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
| filter |string  | Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo de resposta e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*. Você pode especificar os campos a seguir:<p/><ul><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>moduleName</strong></li><li><strong>moduleVersion</strong></li><li><strong>modo</strong></li><li><strong>architecture</strong></li><li><strong>model</strong></li><li><strong>baseboard</strong></li><li><strong>modelFamily</strong></li><li><strong>flightRing</strong></li><li><strong>cabIdHash</strong></li><li><strong>cabType</strong></li><li><strong>cabExpirationTime</strong></li></ul> | Não   |
| orderby | string | Uma instrução que ordena os valores dos dados de resultado. A sintaxe é <em>orderby=field [order],field [order],....</em>. Você pode especificar os seguintes campos do corpo da resposta:<p/><ul><li><strong>date</strong></li><li><strong>market</strong></li><li><strong>cabIdHash</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>cabType</strong></li><li><strong>deviceType</strong></li><li><strong>moduleName</strong></li><li><strong>moduleVersion</strong></li><li><strong>osVersion</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li><li><strong>modo</strong></li><li><strong>architecture</strong></li><li><strong>model</strong></li><li><strong>baseboard</strong></li><li><strong>modelFamily</strong></li></ul><p>O parâmetro <em>order</em> é opcional, e pode ser <strong>asc</strong> ou <strong>desc</strong> para especificar a ordem crescente ou decrescente de cada campo. O padrão é <strong>asc</strong>.</p><p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,market</em></p> |  Não  |


### <a name="request-example"></a>Exemplo de solicitação

Os exemplos a seguir demonstram várias solicitações para a obtenção de dados de erros detalhados.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/hardware/failuredetails?failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/hardware/failuredetails?failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo    | Descrição    |
|------------|---------|------------|
| Valor      | array   | Uma matriz de objetos que contêm dados de erros detalhados. Para obter mais informações sobre os dados em cada objeto, consulte a tabela a seguir.          |
| @nextLink  | cadeia  | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor será retornado se o parâmetro **top** da solicitação estiver definido como 10, mas houver mais de 10 linhas de erros para a consulta. |
| TotalCount | inumber | O número total de linhas no resultado dos dados da consulta.        |


Os elementos na matriz *Value* contêm os valores a seguir.

| Valor           | Tipo    | Descrição     |
|-----------------|---------|----------------------------|
| date            | string  | A primeira data no intervalo de datas dos dados do erro. Se a solicitação especificou um único dia, o valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, o valor será a primeira data nesse intervalo de datas. |
| sellerId   | cadeia  | O valor de ID de vendedor está associado à conta de desenvolvedor (isso corresponde ao valor de **ID de vendedor** nas configurações da conta do Centro de Desenvolvimento). |
| failureName     | string  | O nome de falha, que é composto de quatro partes: uma ou mais classes de problema, um código de verificação de bug/exceção, o nome da imagem/driver em que ocorreu a falha e o nome da função associada.             |
| failureHash     | string  | O identificador exclusivo do erro.     |
| osVersion       | cadeia  | A versão de quatro partes do sistema operacional no qual o erro ocorreu.    |
| mercado          | string  | O código de país ISO 3166 do mercado do dispositivo.     |
| deviceType      | cadeia  | Uma das seguintes cadeias de caracteres que especifica o tipo de dispositivo no qual ocorreu o erro:<p/><ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Desconhecido</strong></li></ul>     |
| moduleName     | cadeia  | O nome exclusivo do módulo que está associado a esse erro.      |
| moduleVersion  | cadeia  | O versão do módulo que está associado a esse erro.   |
| architecture | cadeia |  A arquitetura do dispositivo no qual o erro ocorreu.  |
| model | cadeia | O nome do modelo do dispositivo no qual o erro ocorreu. |
| baseboard | cadeia | O nome da placa do dispositivo no qual ocorreu o erro. |
| modelFamily | cadeia | O nome da família de modelo de dispositivo no qual ocorreu o erro. |
| modo | cadeia | Esse valor é sempre *kernel*. |
| cabIdHash         | cadeia  | A ID exclusiva do arquivo CAB que está associado a esse erro.   |
| cabType         | cadeia  | O tipo do arquivo CAB.   |
| cabExpirationTime  | string  | A data e a hora em que o arquivo CAB expirou e não pôde mais ser baixado, no formato ISO 8601.   |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "date": "2017-03-14 23:51:11",
      "sellerId": "14313740",
      "mode": "Kernel",
      "failureName": "0x109_18_2_LoadImageNotify",
      "failureHash": "f060c0b6-fbe6-463f-a9f1-b7ebc1cc722f",
      "cabIdHash": "cc1797f9-b783-44d4-b0e5-46ae7ca668bc",
      "cabType": "MINI",
      "cabExpirationTime": "2017-06-12 23:51:11",
      "osVersion": "10.0.14393.10",
      "market": "US",
      "deviceType": "PC",
      "moduleName": "Unknown_Image",
      "moduleVersion": "Unknown",
      "architecture": "x64",
      "model": "101",
      "baseboard": "750-ABX",
      "modelFamily": "ContosoPad",
      "flightRing": "Windows 10 Retail"
    }]
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Obter dados de relatórios de erros de hardware OEM](get-oem-hardware-error-reporting-data.md)
* [Baixe o arquivo CAB de um erro de hardware OEM](download-the-cab-file-for-an-oem-hardware-error.md)
