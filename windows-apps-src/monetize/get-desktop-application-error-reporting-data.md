---
description: Use este método na API de análise da Microsoft Store para obter dados de relatório de erros agregados de um aplicativo da área de trabalho por um determinado intervalo de datas e outros filtros opcionais.
title: Obter dados de relatório de erros do seu aplicativo da área de trabalho
ms.date: 09/04/2018
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, erros, aplicativo da área de trabalho
ms.localizationpriority: medium
ms.openlocfilehash: 0a628df7f557070a077cd0c4ec328bc49ffaf5bd
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372111"
---
# <a name="get-error-reporting-data-for-your-desktop-application"></a>Obter dados de relatório de erros do seu aplicativo da área de trabalho

Use este método na API de análise da Microsoft Store para obter os dados de relatório de erros agregados para um aplicativo da área de trabalho que você adicionou ao [programa do Aplicativo da Área de Trabalho do Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program). Esse método só pode recuperar os erros que ocorreram nos últimos 30 dias. Essas informações também estão disponíveis na [relatório de integridade](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program) para aplicativos da área de trabalho no Partner Center.

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                          |
|--------|----------------------------------------------------------------------|
| OBTER    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  
|---------------|--------|---------------|------|
| applicationId | cadeia de caracteres | A ID de produto do aplicativo da área de trabalho para o qual você deseja recuperar dados de relatório de erros. Para obter a ID do produto de um aplicativo da área de trabalho, abra qualquer [relatório de análise para seu aplicativo da área de trabalho no Partner Center](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program) (como o **relatório de integridade**) e recupere a ID do produto da URL. |  Sim  |
| startDate | date | A data de início no intervalo de datas dos dados do relatório de erros a serem recuperados, no formato ```mm/dd/yyyy```. O padrão é a data atual.<p/><p/>**Observação:** &nbsp;&nbsp;esse método só pode recuperar os erros que ocorreram nos últimos 30 dias.  |  Não  |
| endDate | date | A data de término no intervalo de datas dos dados do relatório de erros a serem recuperados, no formato ```mm/dd/yyyy```. O padrão é a data atual.   |  Não  |
| top | int | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |  Não  |
| skip | int | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante. |  Não  |
| filter |cadeia de caracteres  | Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo de resposta e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*. Você pode especificar os campos a seguir do corpo da resposta:<p/><ul><li><strong>fileName</strong></li><li><strong>applicationVersion</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osBuild</strong></li><li><strong>osRelease</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>productName</strong></li><li><strong>date</strong></li></ul> | Não   |
| aggregationLevel | cadeia de caracteres | Especifica o intervalo de tempo para o qual recuperar dados agregados. Pode ser uma das seguintes cadeias de caracteres: **day**, **week** ou **month**. Se não for especificado, o padrão será **day**. Se você especificar **week** ou **month**, os valores *failureName* e *failureHash* serão limitados a 1.000 buckets.<p/>  | Não |
| orderby | cadeia de caracteres | Uma instrução que ordena os valores dos dados de resultado. A sintaxe é *orderby=field [order],field [order],...* . O parâmetro *field* pode ser uma das seguintes cadeias de caracteres:<ul><li><strong>fileName</strong></li><li><strong>applicationVersion</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osBuild</strong></li><li><strong>osRelease</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>productName</strong></li><li><strong>date</strong></li></ul>O parâmetro *order* é opcional, e pode ser **asc** ou **desc** para especificar a ordem crescente ou decrescente de cada campo. O padrão é **asc**.</p><p>Este é um exemplo de cadeia de caracteres *orderby*: *orderby=date,market*</p> |  Não  |
| groupby | cadeia de caracteres | Uma instrução que aplica a agregação de dados apenas aos campos especificados. Você pode especificar os campos a seguir:<ul><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li></ul><p>As linhas de dados retornados conterão os campos especificados no parâmetro *groupby*, bem como o seguinte:</p><ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**eventCount**</li></ul><p>O parâmetro *groupby* pode ser usado com o parâmetro *aggregationLevel*. Por exemplo *&amp;groupby=failureName,market&amp;aggregationLevel=week*</p></p> |  Não  |


### <a name="request-example"></a>Exemplo de solicitação

Os exemplos a seguir demonstram várias solicitações para a obtenção de dados de relatório de erros. Substitua o valor de *applicationId* pela ID do produto de seu aplicativo da área de trabalho.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits?applicationId=10238467886765136388&startDate=1/1/2018&endDate=2/1/2018&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits?applicationId=10238467886765136388&startDate=8/1/2017&endDate=8/31/2017&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo    | Descrição     |
|------------|---------|--------------|
| Valor      | matriz   | Uma matriz de objetos que contêm dados de relatório de erros agregados. Para saber mais sobre os dados em cada objeto, consulte a seção de [valores de erros](#error-values) a seguir.     |
| @nextLink  | cadeia de caracteres  | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor será retornado se o parâmetro **top** da solicitação estiver definido como 10.000, mas houver mais de 10.000 linhas de erros para a consulta. |
| TotalCount | número inteiro | O número total de linhas no resultado dos dados da consulta.     |


### <a name="error-values"></a>Valores de erros

Os elementos na matriz *Value* contêm os valores a seguir.

| Valor           | Tipo    | Descrição        |
|-----------------|---------|---------------------|
| date            | cadeia de caracteres  | A primeira data no intervalo de datas dos dados do erro no formato ```yyyy-mm-dd```. Se a solicitação especificar um único dia, o valor será essa data. Se a solicitação especificar um intervalo de datas maior, um mês ou outro intervalo de datas, o valor será a primeira data nesse intervalo de datas. Para solicitações que especificam um valor de *aggregationLevel* de **hora**, esse valor também inclui um valor de hora no formato ```hh:mm:ss```.  |
| applicationId   | cadeia de caracteres  | A ID de produto do aplicativo da área de trabalho para o qual você recuperou dados de erro.    |
| productName | cadeia de caracteres  | O nome de exibição do aplicativo da área de trabalho como derivado dos metadados de seus executáveis associados.   |
| appName | cadeia de caracteres  |  A ser definido  |
| fileName | cadeia de caracteres  | O nome do arquivo executável do aplicativo da área de trabalho.   |
| failureName     | cadeia de caracteres  | O nome de falha, que é composto de quatro partes: uma ou mais classes de problema, um código de verificação de bug/exceção, o nome da imagem em que ocorreu a falha e o nome da função associada.  |
| failureHash     | cadeia de caracteres  | O identificador exclusivo do erro.   |
| symbol          | cadeia de caracteres  | O símbolo atribuído a esse erro. |
| osBuild       | cadeia de caracteres  | O número de build de quatro partes do sistema operacional no qual o erro ocorreu.  |
| osVersion       | cadeia de caracteres  | Uma das seguintes sequências que especifica a versão do sistema operacional no qual o aplicativo da área de trabalho está instalado:<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Desconhecido</strong></li></ul>   |   
| osRelease | cadeia de caracteres  | Uma das sequências a seguir que especifica a versão do sistema operacional ou anel de liberação de versões de pré-lançamento (como uma subpopulação na versão do sistema operacional) no qual o erro ocorreu.<p/><p>No Windows 10:</p><ul><li><strong>Versão 1507</strong></li><li><strong>Versão 1511</strong></li><li><strong>Versão 1607</strong></li><li><strong>Versão 1703</strong></li><li><strong>Versão 1709</strong></li><li><strong>Versão 1803</strong></li><li><strong>Release Preview</strong></li><li><strong>Insider Fast</strong></li><li><strong>Insider lento</strong></li></ul><p/><p>Para o Windows Server 1709:</p><ul><li><strong>RTM</strong></li></ul><p>Para o Windows Server 2016:</p><ul><li><strong>Versão 1607</strong></li></ul><p>No Windows 8.1:</p><ul><li><strong>Atualização 1</strong></li></ul><p>No Windows 7:</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Se a versão do sistema operacional ou anel de liberação de versão de pré-lançamento for desconhecida, este campo terá o valor <strong>Desconhecido</strong>.</p> |
| eventType       | cadeia de caracteres  | Uma das seguintes sequências que indica o tipo da evento de erro:<ul><li>**crash**</li><li>**hang**</li><li>**memory**</li><li>**jse**</li></ul>       |
| market          | cadeia de caracteres  | O código de país ISO 3166 do mercado do dispositivo.   |
| deviceType      | cadeia de caracteres  | Uma das seguintes cadeias de caracteres que especifica o tipo de dispositivo no qual ocorreu o erro:<p/><ul><li><strong>PC</strong></li><li><strong>Servidor</strong></li><li><strong>Tablet</strong></li><li><strong>Desconhecido</strong></li></ul>    |
| applicationVersion     | cadeia de caracteres  |   A versão do executável do aplicativo em que ocorreu o erro.    |
| eventCount      | number | O número de eventos que são atribuídos a esse erro para o nível de agregação especificado.      |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "date": "2018-02-01",
      "applicationId": "10238467886765136388",
      "productName": "Contoso Demo",
      "appName": "Contoso Demo",
      "fileName": "contosodemo.exe",
      "failureName": "SVCHOSTGROUP_localservice_IN_PAGE_ERROR_c0000006_hardware_disk!Unknown",
      "failureHash": "11242ef3-ebd8-d525-838d-b5497b225695",
      "symbol": "hardware_disk!Unknown",
      "osBuild": "10.0.15063.850",
      "osVersion": "Windows 10",
      "osRelease": "Version 1703",
      "eventType": "crash",
      "market": "US",
      "deviceType": "PC",
      "applicationVersion": "2.2.2.0",
      "eventCount": 0.0012422360248447205
    }
  ],
  "@nextLink": "desktop/failurehits?applicationId=10238467886765136388&aggregationLevel=week&startDate=2018/02/01&endDate2018/02/08&top=1&skip=1",
  "TotalCount": 21
}

```

## <a name="related-topics"></a>Tópicos relacionados

* [Relatório de integridade](../publish/health-report.md)
* [Dados de análise de acesso usando os serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter detalhes sobre um erro em seu aplicativo da área de trabalho](get-details-for-an-error-in-your-desktop-application.md)
* [Obter o rastreamento de pilha para um erro em seu aplicativo da área de trabalho](get-the-stack-trace-for-an-error-in-your-desktop-application.md)
* [Baixe o arquivo CAB para um erro em seu aplicativo da área de trabalho](download-the-cab-file-for-an-error-in-your-desktop-application.md)
