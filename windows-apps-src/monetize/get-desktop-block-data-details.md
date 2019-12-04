---
description: Use este URI REST para obter dados de detalhes de bloco para um aplicativo de área de trabalho durante um determinado intervalo de datas e outros filtros opcionais.
title: Obter detalhes do bloco de atualização para seu aplicativo
ms.date: 07/11/2018
ms.topic: article
keywords: Windows 10, blocos de aplicativos de desktop, programa de aplicativos para área de trabalho do Windows
localizationpriority: medium
ms.openlocfilehash: b30594ff3943d1ed9183190a0b5a43824816c080
ms.sourcegitcommit: ae9c1646398bb5a4a888437628eca09ae06e6076
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74735111"
---
# <a name="get-upgrade-block-details-for-your-desktop-application"></a>Obter detalhes do bloqueio de atualização para seu aplicativo da área de trabalho

Use este URI REST para obter detalhes de dispositivos Windows 10 nos quais um executável específico em seu aplicativo de área de trabalho está bloqueando a execução de uma atualização do Windows 10. Você pode usar esse URI somente para aplicativos de área de trabalho que você adicionou ao [programa de aplicativo da área de trabalho do Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program). Essas informações também estão disponíveis no [relatório de blocos de aplicativos](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report) para aplicativos de área de trabalho no Partner Center.

Esse URI é semelhante a [obter blocos de atualização para seu aplicativo de área de trabalho](get-desktop-block-data.md), mas retorna informações de bloqueio de dispositivo para um executável específico em seu aplicativo de área de trabalho.

## <a name="prerequisites"></a>Pré-requisitos


Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockdetails```|


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Digite   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | sequência | Obrigatório. O token de acesso do AD do Azure no formulário **portador** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros de solicitação

| Parâmetro        | Digite   |  Descrição      |  Obrigatório  
|---------------|--------|---------------|------|
| applicationId | sequência | A ID do produto do aplicativo de área de trabalho para o qual você deseja recuperar dados de bloqueio. Para obter a ID do produto de um aplicativo de área de trabalho, abra qualquer [relatório de análise para seu aplicativo de área de trabalho no Partner Center](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program) (como o **relatório de blocos**) e recupere a ID do produto da URL. |  Sim  |
| fileName | sequência | O nome do executável bloqueado |
| startDate | date | A data de início no intervalo de datas dos dados de bloco a serem recuperados. O padrão é 90 dias antes da data atual. |  Não  |
| endDate | date | A data de término no intervalo de datas dos dados de bloco a serem recuperados. O padrão é a data atual. |  Não  |
| top | int | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |  Não  |
| skip | int | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante. |  Não  |
| filter | sequência  | Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo de resposta e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Os valores de sequência devem estar entre aspas simples no parâmetro *filter*. Você pode especificar os campos a seguir do corpo da resposta:<p/><ul><li><strong>applicationVersion</strong></li><li><strong>arquitectura</strong></li><li><strong>blockType</strong></li><li><strong>deviceType</strong></li><li><strong>anunciar</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>NomeDoProduto</strong></li><li><strong>targetOs</strong></li></ul> | Não   |
| orderby | sequência | Uma instrução que ordena os valores de dados de resultado para cada bloco. A sintaxe é <em>orderby=field [order],field [order],...</em>. O parâmetro <em>field</em> pode ser um dos seguintes campos do corpo da resposta:<p/><ul><li><strong>applicationVersion</strong></li><li><strong>arquitectura</strong></li><li><strong>blockType</strong></li><li><strong>date</strong><li><strong>deviceType</strong></li><li><strong>anunciar</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>NomeDoProduto</strong></li><li><strong>targetOs</strong></li><li><strong>deviceCount</strong></li></ul><p>O parâmetro <em>order</em> é opcional e pode ser <strong>asc</strong> ou <strong>desc</strong> para especificar a ordem crescente ou decrescente de cada campo. O padrão é <strong>asc</strong>.</p><p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,market</em></p> |  Não  |
| groupby | sequência | Uma instrução que aplica a agregação de dados apenas aos campos especificados. Você pode especificar os campos a seguir do corpo da resposta:<p/><ul><li><strong>applicationVersion</strong></li><li><strong>arquitectura</strong></li><li><strong>blockType</strong></li><li><strong>deviceType</strong></li><li><strong>anunciar</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>targetOs</strong></li></ul><p>As linhas de dados retornados conterão os campos especificados no parâmetro <em>groupby</em>, bem como o seguinte:</p><ul><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>NomeDoProduto</strong></li><li><strong>deviceCount</strong></li></ul></p> |  Não  |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra várias solicitações para obter dados de bloco de aplicativo de área de trabalho. Substitua o valor *ApplicationId* pela ID do produto do aplicativo da área de trabalho.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockdetails?applicationId=10238467886765136388&fileName=contoso.exe&startDate=2018-05-01&endDate=2018-06-07&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockdetails?applicationId=10238467886765136388&fileName=contoso.exe&startDate=2018-05-01&endDate=2018-06-07&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Valor      | Digite   | Descrição                  |
|------------|--------|-------------------------------------------------------|
| Valor      | matriz  | Uma matriz de objetos que contêm dados de bloco agregados. Para obter mais informações sobre os dados em cada objeto, consulte a tabela a seguir. |
| @nextLink  | sequência | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor será retornado se o parâmetro **superior** da solicitação for definido como 10000, mas houver mais de 10000 linhas de dados de bloco para a consulta. |
| TotalCount | int    | O número total de linhas no resultado dos dados da consulta. |


Os elementos na matriz *Value* contêm os valores a seguir.

| Valor               | Digite   | Descrição                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | sequência | A ID do produto do aplicativo de área de trabalho para o qual você recuperou dados de bloqueio. |
| date                | sequência | A data associada ao valor de ocorrências de bloqueio. |
| productName         | sequência | O nome de exibição do aplicativo da área de trabalho como derivado dos metadados de seus executáveis associados. |
| fileName            | sequência | O executável que foi bloqueado. |
| applicationVersion  | sequência | A versão do executável do aplicativo que foi bloqueado. |
| osVersion           | sequência | Uma das seguintes cadeias de caracteres que especifica a versão do sistema operacional em que o aplicativo da área de trabalho está em execução no momento:<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Conhecidos</strong></li></ul>   |
| osRelease           | sequência | Uma das cadeias de caracteres a seguir que especifica a versão do sistema operacional ou o anel de vôo (como uma subpopulação na versão do sistema operacional) na qual o aplicativo da área de trabalho está em execução no momento.<p/><p>No Windows 10:</p><ul><li><strong>Versão 1507</strong></li><li><strong>Versão 1511</strong></li><li><strong>Versão 1607</strong></li><li><strong>Versão 1703</strong></li><li><strong>Versão 1709</strong></li><li><strong>Versão prévia</strong></li><li><strong>Insider rápido</strong></li><li><strong>Insider lento</strong></li></ul><p/><p>Para o Windows Server 1709:</p><ul><li><strong>RTM</strong></li></ul><p>Para o Windows Server 2016:</p><ul><li><strong>Versão 1607</strong></li></ul><p>No Windows 8.1:</p><ul><li><strong>Atualização 1</strong></li></ul><p>No Windows 7:</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Se a versão do sistema operacional ou anel de liberação de versão de pré-lançamento for desconhecida, este campo terá o valor <strong>Desconhecido</strong>.</p> |
| market              | sequência | O código do país ISO 3166 do mercado no qual o aplicativo da área de trabalho está bloqueado. |
| deviceType          | sequência | Uma das seguintes cadeias de caracteres que especifica o tipo de dispositivo no qual o aplicativo de área de trabalho está bloqueado:<p/><ul><li><strong>PROTECÇÃO</strong></li><li><strong>Servidor</strong></li><li><strong>PC</strong></li><li><strong>Conhecidos</strong></li></ul> |
| blockType            | sequência | Uma das seguintes cadeias de caracteres que especifica o tipo de bloco encontrado no dispositivo:<p/><ul><li><strong>Potencial SEDIMENT</strong></li><li><strong>SEDIMENT temporário</strong></li><li><strong>Notificação de tempo de execução</strong></li></ul><p/><p/>Para obter mais informações sobre esses tipos de bloco e o que eles significam para desenvolvedores e usuários, consulte a descrição do [relatório de blocos de aplicativos](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report). |
| architecture        | sequência | A arquitetura do dispositivo no qual o bloco existe: <p/><ul><li><strong>ARM64</strong></li><li><strong>X86</strong></li></ul> |
| targetOs            | sequência | Uma das seguintes cadeias de caracteres que especifica a versão do sistema operacional Windows 10 na qual o aplicativo da área de trabalho está impedido de ser executado: <p/><ul><li><strong>Versão 1709</strong></li><li><strong>Versão 1803</strong></li></ul> |
| deviceCount         | number | O número de dispositivos distintos que têm blocos no nível de agregação especificado. |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
     "applicationId": "10238467886765136388",
     "date": "2018-06-03",
     "productName": "Contoso Demo",
     "fileName": "contosodemo.exe",
     "applicationVersion": "2.2.2.0",
     "osVersion": "Windows 8.1",
     "osRelease": "Update 1",
     "market": "ZA",
     "deviceType": "All",
     "blockType": "Runtime Notification",
     "architecture": "X86",
     "targetOs": "RS4",
     "deviceCount": 120
    }
  ],
  "@nextLink": "desktop/blockdetails?applicationId=123456789&startDate=2018-01-01&endDate=2018-02-01&top=10000&skip=10000&groupby=applicationVersion,deviceType,osVersion,osRelease",
  "TotalCount": 23012
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Programa de aplicativos da área de trabalho do Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)
* [Obter blocos de atualização para seu aplicativo de desktop](get-desktop-block-data.md)
