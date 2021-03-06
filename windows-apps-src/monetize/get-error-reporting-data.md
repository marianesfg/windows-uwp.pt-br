---
ms.assetid: 252C44DF-A2B8-4F4F-9D47-33E423F48584
description: Use este método na API de análise da Microsoft Store para obter dados de relatório de erros agregados para um determinado intervalo de datas e outros filtros opcionais.
title: Obter dados de relatório de erros do seu aplicativo
ms.date: 09/04/2018
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, erros
ms.localizationpriority: medium
ms.openlocfilehash: 69504fd8d670158a35e5afd5dcd035acd296f084
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590591"
---
# <a name="get-error-reporting-data-for-your-app"></a>Obter dados de relatório de erros do seu aplicativo

Use este método na API de análise da Microsoft Store para obter dados de relatório de erros agregados do seu app no formato JSON para um determinado intervalo de datas e outros filtros opcionais. Esse método só pode recuperar os erros que ocorreram nos últimos 30 dias. Essas informações também estão disponíveis na **falhas** seção o [relatório de integridade](../publish/health-report.md) no Partner Center.

Você pode recuperar informações de erro adicionais usando os métodos [obter detalhes de erro](get-details-for-an-error-in-your-app.md), [obter rastreamento de pilha](get-the-stack-trace-for-an-error-in-your-app.md) e [baixar arquivo CAB](download-the-cab-file-for-an-error-in-your-app.md).

## <a name="prerequisites"></a>Pré-requisitos


Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  
|---------------|--------|---------------|------|
| applicationId | cadeia de caracteres | A ID da Loja do aplicativo para o qual você deseja recuperar dados de relatório de erros. A ID de Store está disponível na [página de aplicativo de identidade](../publish/view-app-identity-details.md) no Partner Center. Um exemplo de ID da Loja é 9WZDNCRFJ3Q8. |  Sim  |
| startDate | date | A data de início no intervalo de datas dos dados do relatório de erros a serem recuperados. O padrão é a data atual. Se *aggregationLevel* for **day**, **week** ou **month**, esse parâmetro deverá especificar uma data no formato ```mm/dd/yyyy```. Se *aggregationLevel* for **hour**, esse parâmetro poderá especificar uma data no formato ```mm/dd/yyyy``` ou uma data e hora no formato ```yyyy-mm-dd hh:mm:ss```.<p/><p/>**Observação:**&nbsp;&nbsp;esse método só pode recuperar os erros que ocorreram nos últimos 30 dias.  |  Não  |
| endDate | date | A data de término no intervalo de datas dos dados do relatório de erros a serem recuperados. O padrão é a data atual. Se *aggregationLevel* for **day**, **week** ou **month**, esse parâmetro deverá especificar uma data no formato ```mm/dd/yyyy```. Se *aggregationLevel* for **hour**, esse parâmetro poderá especificar uma data no formato ```mm/dd/yyyy``` ou uma data e hora no formato ```yyyy-mm-dd hh:mm:ss```. |  Não  |
| top | int | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |  Não  |
| skip | int | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante. |  Não  |
| filter |cadeia de caracteres  | Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo de resposta e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*. Você pode especificar os campos a seguir do corpo da resposta:<p/><ul><li>**ApplicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**Símbolo**</li><li>**osVersion**</li><li>**osRelease**</li><li>**EventType**</li><li>**mercado**</li><li>**deviceType**</li><li>**Nome de pacote**</li><li>**PackageVersion**</li><li>**Data**</li></ul> | Não   |
| aggregationLevel | cadeia de caracteres | Especifica o intervalo de tempo para o qual recuperar dados agregados. Pode ser uma das seguintes sequências: **hour**, **day**, **week** ou **month**. Se não for especificado, o padrão será **day**. Se você especificar **week** ou **month**, os valores *failureName* e *failureHash* serão limitados a 1.000 buckets.<p/><p/>**Observação:**&nbsp;&nbsp;se você especificar **hour**, poderá recuperar dados de erro somente das últimas 72 horas. Para recuperar dados de erro anteriores às 72 horas, especifique **day** ou um dos níveis de agregação.  | Não |
| orderby | cadeia de caracteres | Uma instrução que ordena os valores dos dados de resultado. A sintaxe é *orderby=field [order],field [order],...*. O parâmetro *field* pode ser uma das seguintes cadeias de caracteres:<ul><li>**ApplicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**Símbolo**</li><li>**osVersion**</li><li>**osRelease**</li><li>**EventType**</li><li>**mercado**</li><li>**deviceType**</li><li>**Nome de pacote**</li><li>**PackageVersion**</li><li>**Data**</li></ul><p>O parâmetro *order* é opcional, e pode ser **asc** ou **desc** para especificar a ordem crescente ou decrescente de cada campo. O padrão é **asc**.</p><p>Este é um exemplo de cadeia de caracteres *orderby*: *orderby=date,market*</p> |  Não  |
| groupby | cadeia de caracteres | Uma instrução que aplica a agregação de dados apenas aos campos especificados. Você pode especificar os campos a seguir:<ul><li>**failureName**</li><li>**failureHash**</li><li>**Símbolo**</li><li>**osVersion**</li><li>**EventType**</li><li>**mercado**</li><li>**deviceType**</li><li>**Nome de pacote**</li><li>**PackageVersion**</li></ul><p>As linhas de dados retornados conterão os campos especificados no parâmetro *groupby*, bem como o seguinte:</p><ul><li>**Data**</li><li>**applicationId**</li><li>**ApplicationName**</li><li>**deviceCount**</li><li>**Eventcount/eventcount10**</li></ul><p>O parâmetro *groupby* pode ser usado com o parâmetro *aggregationLevel*. Por exemplo *&amp;groupby=failureName,market&amp;aggregationLevel=week*</p></p> |  Não  |


### <a name="request-example"></a>Exemplo de solicitação

Os exemplos a seguir demonstram várias solicitações para a obtenção de dados de relatório de erros. Substitua o valor de *applicationId* pela ID da Loja de seu app.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'phone' HTTP/1.1
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
| applicationId   | cadeia de caracteres  | A ID da Loja do app para o qual você deseja recuperar dados de erros.   |
| applicationName | cadeia de caracteres  | O nome de exibição do app.   |
| failureName     | cadeia de caracteres  | O nome de falha, que é composto de quatro partes: uma ou mais classes de problema, um código de verificação de bug/exceção, o nome da imagem em que ocorreu a falha e o nome da função associada.  |
| failureHash     | cadeia de caracteres  | O identificador exclusivo do erro.   |
| symbol          | cadeia de caracteres  | O símbolo atribuído a esse erro. |
| osVersion       | cadeia de caracteres  | Uma das seguintes sequências que especifica a versão do sistema operacional no qual ocorreu o erro:<ul><li>**Windows Phone 7.5**</li><li>**Windows Phone 8**</li><li>**Windows Phone 8.1**</li><li>**Windows Phone 10**</li><li>**Windows 8**</li><li>**Windows 8.1**</li><li>**Windows 10**</li><li>**Desconhecido**</li></ul>  |
| osRelease       | cadeia de caracteres  |  Uma das sequências a seguir que especifica a versão do sistema operacional ou anel de liberação de versões de pré-lançamento (como uma subpopulação na versão do sistema operacional) no qual o erro ocorreu.<p/><p>No Windows 10:</p><ul><li><strong>Versão 1507</strong></li><li><strong>Versão 1511</strong></li><li><strong>Versão 1607</strong></li><li><strong>Versão 1703</strong></li><li><strong>Versão 1709</strong></li><li><strong>Versão 1803</strong></li><li><strong>Release Preview</strong></li><li><strong>Insider Fast</strong></li><li><strong>Insider lento</strong></li></ul><p/><p>Para o Windows Server 1709:</p><ul><li><strong>RTM</strong></li></ul><p>Para o Windows Server 2016:</p><ul><li><strong>Versão 1607</strong></li></ul><p>No Windows 8.1:</p><ul><li><strong>Atualização 1</strong></li></ul><p>No Windows 7:</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Se a versão do sistema operacional ou anel de liberação de versão de pré-lançamento for desconhecida, este campo terá o valor <strong>Desconhecido</strong>.</p>    |
| eventType       | cadeia de caracteres  | Uma das seguintes cadeias de caracteres:<ul><li>**Falha**</li><li>**parar de responder**</li><li>**Memória**</li><li>**jse**</li></ul>      |
| market          | cadeia de caracteres  | O código de país ISO 3166 do mercado do dispositivo.   |
| deviceType      | cadeia de caracteres  | Uma das seguintes sequências que indica o tipo de dispositivo no qual ocorreu o erro:<ul><li>**PC**</li><li>**Telefone**</li><li>**Console**</li><li>**IoT**</li><li>**Holographic**</li><li>**Desconhecido**</li></ul>    |
| packageName     | cadeia de caracteres  | O nome exclusivo do pacote do aplicativo que está associado a esse erro.      |
| packageVersion  | cadeia de caracteres  | O versão do pacote do aplicativo que está associado a esse erro.   |
| deviceCount     | number | O número de dispositivos exclusivos que correspondem a esse erro para o nível de agregação especificado.  |
| eventCount      | number | O número de eventos que são atribuídos a esse erro para o nível de agregação especificado.      |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "date": "2017-03-09",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "failureName": "APPLICATION_FAULT_8013150a_StoreWrapper.ni.DLL!70475e55",
      "failureHash": "5a6b2170-1661-ed47-24d7-230fed0077af",
      "symbol": "storewrapper_ni!70475e55",
      "osVersion": "Windows 10",
      "osRelease": "Version 1507",
      "eventType": "crash",
      "market": "US",
      "deviceType": "PC",
      "packageName": "",
      "packageVersion": "0.0.0.0",
      "deviceCount": 0.0,
      "eventCount": 1.0
    }
  ],
  "@nextLink": "failurehits?applicationId=9NBLGGGZ5QDR&aggregationLevel=week&startDate=2017/03/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 21
}

```

## <a name="related-topics"></a>Tópicos relacionados

* [Relatório de integridade](../publish/health-report.md)
* [Obter detalhes sobre um erro em seu aplicativo](get-details-for-an-error-in-your-app.md)
* [Obter o rastreamento de pilha para um erro em seu aplicativo](get-the-stack-trace-for-an-error-in-your-app.md)
* [Baixe o arquivo CAB para um erro em seu aplicativo](download-the-cab-file-for-an-error-in-your-app.md)
* [Dados de análise de acesso usando os serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter aquisições de aplicativo](get-app-acquisitions.md)
* [Obter aquisições de complemento](get-in-app-acquisitions.md)
* [Obtenha as classificações do aplicativo](get-app-ratings.md)
* [Obter revisões de aplicativo](get-app-reviews.md)
