---
description: Use este método na API de análise da Microsoft Store para obter dados de relatório de erros agregados para um determinado intervalo de datas e outros filtros opcionais.
title: Obter dados de relatório para seu Xbox One erros jogo
ms.date: 11/06/2018
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, erros
ms.localizationpriority: medium
ms.openlocfilehash: f9ae7c75fb332e910aa1b63712cf0d230172afd3
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8750021"
---
# <a name="get-error-reporting-data-for-your-xbox-one-game"></a>Obter dados de relatório para seu Xbox One erros jogo

Use este método na API de análise da Microsoft Store para obter dados de relatório de erros agregados para seu Xbox One jogo ingerido por meio do Portal de desenvolvedor do Xbox (XDP) e está disponível no painel do Centro de parceiro de análise XDP.

Você pode recuperar informações de erro adicionais usando os métodos de [obter detalhes de um erro em seu Xbox One jogo](get-details-for-an-error-in-your-xbox-one-game.md), [obter o rastreamento de pilha de um erro em seu Xbox One jogo](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)e [baixar o arquivo CAB para um erro em seu jogo Xbox One](download-the-cab-file-for-an-error-in-your-xbox-one-game.md) .

## <a name="prerequisites"></a>Pré-requisitos


Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failurehits``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  
|---------------|--------|---------------|------|
| applicationId | string | A ID do produto do jogo Xbox One para o qual você está recuperando dados de relatório de erros. Para obter a ID do produto do jogo, navegue até seu jogo no Portal de Desenvolvedor do Xbox (XDP) e recupere a ID do produto da URL. Como alternativa, se você baixar os dados de integridade do relatório de análise do Partner Center do Windows, a ID do produto está incluída no arquivo. tsv. |  Sim  |
| startDate | date | A data de início no intervalo de datas dos dados do relatório de erros a serem recuperados. O padrão é a data atual. Se *aggregationLevel* for **day**, **week** ou **month**, esse parâmetro deverá especificar uma data no formato ```mm/dd/yyyy```. Se *aggregationLevel* for **hour**, esse parâmetro poderá especificar uma data no formato ```mm/dd/yyyy``` ou uma data e hora no formato ```yyyy-mm-dd hh:mm:ss```.  |  Não  |
| endDate | date | A data de término no intervalo de datas dos dados do relatório de erros a serem recuperados. O padrão é a data atual. Se *aggregationLevel* for **day**, **week** ou **month**, esse parâmetro deverá especificar uma data no formato ```mm/dd/yyyy```. Se *aggregationLevel* for **hour**, esse parâmetro poderá especificar uma data no formato ```mm/dd/yyyy``` ou uma data e hora no formato ```yyyy-mm-dd hh:mm:ss```. |  Não  |
| top | int | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |  Não  |
| skip | int | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante. |  Não  |
| filter |string  | Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo de resposta e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*. Você pode especificar os campos a seguir do corpo da resposta:<p/><ul><li>**applicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**osRelease**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li><li>**date**</li></ul> | Não   |
| aggregationLevel | string | Especifica o intervalo de tempo para o qual recuperar dados agregados. Pode ser uma das seguintes sequências: **hour**, **day**, **week** ou **month**. Se não for especificado, o padrão será **day**. Se você especificar **week** ou **month**, os valores *failureName* e *failureHash* serão limitados a 1.000 buckets.<p/><p/>**Observação:**&nbsp;&nbsp;se você especificar **hour**, poderá recuperar dados de erro somente das últimas 72 horas. Para recuperar dados de erro anteriores às 72 horas, especifique **day** ou um dos níveis de agregação.  | Não |
| orderby | string | Uma instrução que ordena os valores dos dados de resultado. A sintaxe é *orderby=field [order],field [order],...*. O parâmetro *field* pode ser uma das seguintes sequências:<ul><li>**applicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**osRelease**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li><li>**date**</li></ul><p>O parâmetro *order* é opcional, e pode ser **asc** ou **desc** para especificar a ordem crescente ou decrescente de cada campo. O padrão é **asc**.</p><p>Este é um exemplo de cadeia de caracteres *orderby*: *orderby=date,market*</p> |  Não  |
| groupby | string | Uma instrução que aplica a agregação de dados apenas aos campos especificados. Você pode especificar os campos a seguir:<ul><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li></ul><p>As linhas de dados retornados conterão os campos especificados no parâmetro *groupby*, bem como o seguinte:</p><ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**deviceCount**</li><li>**eventCount**</li></ul><p>O parâmetro *groupby* pode ser usado com o parâmetro *aggregationLevel*. Por exemplo *&amp;groupby=failureName,market&amp;aggregationLevel=week*</p></p> |  Não  |


### <a name="request-example"></a>Exemplo de solicitação

Os exemplos a seguir demonstram várias solicitações de obtenção de dados de relatório de erro de jogo Xbox One. Substitua o valor *applicationId* com a ID do produto para o seu jogo.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failurehits?applicationId=BRRT4NJ9B3D1&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failurehits?applicationId=BRRT4NJ9B3D1&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'Console' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo    | Descrição     |
|------------|---------|--------------|
| Valor      | array   | Uma matriz de objetos que contêm dados de relatório de erros agregados. Para saber mais sobre os dados em cada objeto, consulte a seção de [valores de erros](#error-values) a seguir.     |
| @nextLink  | cadeia  | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor será retornado se o parâmetro **top** da solicitação estiver definido como 10.000, mas houver mais de 10.000 linhas de erros para a consulta. |
| TotalCount | número inteiro | O número total de linhas no resultado dos dados da consulta.     |


### <a name="error-values"></a>Valores de erros

Os elementos na matriz *Value* contêm os valores a seguir.

| Valor           | Tipo    | Descrição        |
|-----------------|---------|---------------------|
| date            | string  | A primeira data no intervalo de datas dos dados do erro no formato ```yyyy-mm-dd```. Se a solicitação especificar um único dia, o valor será essa data. Se a solicitação especificar um intervalo de datas maior, um mês ou outro intervalo de datas, o valor será a primeira data nesse intervalo de datas. Para solicitações que especificam um valor de **hora**de *aggregationLevel* , esse valor também inclui um valor de hora no formato ```hh:mm:ss``` no fuso horário local no qual ocorreu o erro.  |
| applicationId   | string  | A ID do produto do jogo Xbox One para o qual você deseja recuperar dados de erros.   |
| applicationName | string  | O nome de exibição do jogo.   |
| failureName     | string  | O nome de falha, que é composto de quatro partes: uma ou mais classes de problema, um código de verificação de bug/exceção, o nome da imagem em que ocorreu a falha e o nome da função associada.  |
| failureHash     | string  | O identificador exclusivo do erro.   |
| symbol          | string  | O símbolo atribuído a esse erro. |
| osVersion       | string  | A versão do sistema operacional no qual ocorreu o erro. Isso é sempre o valor **Windows 10**.  |
| osRelease       | string  |  Uma das seguintes cadeias de caracteres que especifica a versão do sistema operacional Windows 10 ou anel de liberação (como uma subpopulação na versão do sistema operacional) no qual ocorreu o erro.<p/><ul><li><strong>Versão1507</strong></li><li><strong>Versão1511</strong></li><li><strong>Versão1607</strong></li><li><strong>Versão1703</strong></li><li><strong>Versão1709</strong></li><li><strong>Versão1803</strong></li><li><strong>Versão prévia de lançamento</strong></li><li><strong>Participante do Programa Windows Insider - Modo Rápido</strong></li><li><strong>Participante do Programa Windows Insider - Modo Lento</strong></li></ul><p>Se a versão do sistema operacional ou anel de liberação de versão de pré-lançamento for desconhecida, este campo terá o valor <strong>Desconhecido</strong>.</p>    |
| eventType       | string  | Uma das seguintes cadeias de caracteres:<ul><li>**falha**</li><li>**travar**</li><li>**Falha de memória**</li></ul>      |
| market          | string  | O código de país ISO 3166 do mercado do dispositivo.   |
| deviceType      | string  | O tipo de dispositivo no qual o erro ocorreu. Isso é sempre o valor do **Console**.    |
| packageName     | string  | O pacote exclusivo nome do jogo que está associado esse erro.      |
| packageVersion  | string  | A versão do pacote do jogo que está associado esse erro.   |
| deviceCount     | número inteiro | O número de dispositivos exclusivos que correspondem a esse erro para o nível de agregação especificado.  |
| eventCount      | número inteiro | O número de eventos que são atribuídos a esse erro para o nível de agregação especificado.      |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "date": "2017-03-09",
      "applicationId": "BRRT4NJ9B3D1",
      "applicationName": "Contoso Sports",
      "failureName": "APPLICATION_FAULT_8013150a_StoreWrapper.ni.DLL!70475e55",
      "failureHash": "5a6b2170-1661-ed47-24d7-230fed0077af",
      "symbol": "storewrapper_ni!70475e55",
      "osVersion": "Windows 10",
      "osRelease": "Version 1803",
      "eventType": "crash",
      "market": "US",
      "deviceType": "Console",
      "packageName": "",
      "packageVersion": "0.0.0.0",
      "deviceCount": 0.0,
      "eventCount": 1.0
    }
  ],
  "@nextLink": "failurehits?applicationId=BRRT4NJ9B3D1&aggregationLevel=week&startDate=2017/03/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 21
}

```

## <a name="related-topics"></a>Tópicos relacionados

* [Obter detalhes de um erro em seu Xbox One jogo](get-details-for-an-error-in-your-xbox-one-game.md)
* [Obter o rastreamento de pilha de um erro em seu Xbox One jogo](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)
* [Baixar o arquivo CAB para um erro em seu jogo Xbox One](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)
* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
