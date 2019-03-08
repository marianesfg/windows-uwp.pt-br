---
description: Use esse URI de REST para obter dados de agregação de instalação para um aplicativo da área de trabalho durante um determinado intervalo de datas e outros filtros opcionais.
title: Obter instalações de aplicativo da área de trabalho
ms.date: 03/01/2018
ms.topic: article
keywords: Windows 10, instala o aplicativo da área de trabalho, programa de aplicativo de área de trabalho do Windows
localizationpriority: medium
ms.openlocfilehash: f9beb73dafa96c489b8a4c9dddecf59d8a047109
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612971"
---
# <a name="get-desktop-application-installs"></a>Obter instalações de aplicativo da área de trabalho


Usar esse URI de REST para obter dados de agregação de instalação no formato JSON para um aplicativo da área de trabalho que você adicionou para a [programa de aplicativo de área de trabalho do Windows](https://msdn.microsoft.com/library/windows/desktop/mt826504). Esse URI permite que você obtenha dados de instalação durante um determinado intervalo de datas e outros filtros opcionais. Essas informações também estão disponíveis na [instala o relatório](https://msdn.microsoft.com/library/windows/desktop/mt826504) para aplicativos da área de trabalho no Partner Center.

## <a name="prerequisites"></a>Pré-requisitos


Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/installbasedaily```|


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  
|---------------|--------|---------------|------|
| applicationId | cadeia de caracteres | A ID do aplicativo da área de trabalho para o qual você deseja recuperar os dados de instalação do produto. Para obter a ID do produto de um aplicativo da área de trabalho, abra qualquer [relatório de análise para seu aplicativo da área de trabalho no Partner Center](https://msdn.microsoft.com/library/windows/desktop/mt826504) (como o **instala relatório**) e recupere a ID do produto da URL. |  Sim  |
| startDate | date | A data de início no intervalo de datas dos dados de instalação a serem recuperados. O padrão é de 90 dias antes da data atual. |  Não  |
| endDate | date | A data final no intervalo de datas de dados de instalação a serem recuperados. O padrão é a data atual. |  Não  |
| top | int | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |  Não  |
| skip | int | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante. |  Não  |
| filter | cadeia de caracteres  | Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo de resposta e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*. Você pode especificar os campos a seguir do corpo da resposta:<p/><ul><li><strong>applicationVersion</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li></ul> | Não   |
| orderby | cadeia de caracteres | Uma instrução que classifica os valores dos dados resultantes de cada instalação. A sintaxe é <em>orderby=field [order],field [order],...</em>. O parâmetro <em>field</em> pode ser um dos seguintes campos do corpo da resposta:<p/><ul><li><strong>productName</strong></li><li><strong>date</strong><li><strong>applicationVersion</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>installBase</strong></li></ul><p>O parâmetro <em>order</em> é opcional, e pode ser <strong>asc</strong> ou <strong>desc</strong> para especificar a ordem crescente ou decrescente de cada campo. O padrão é <strong>asc</strong>.</p><p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,market</em></p> |  Não  |
| groupby | cadeia de caracteres | Uma instrução que aplica a agregação de dados apenas aos campos especificados. Você pode especificar os campos a seguir do corpo da resposta:<p/><ul><li><strong>applicationVersion</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li></ul><p>As linhas de dados retornados conterão os campos especificados no parâmetro <em>groupby</em>, bem como o seguinte:</p><ul><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>productName</strong></li><li><strong>installBase</strong></li></ul></p> |  Não  |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como instalar os dados de várias solicitações para obter o aplicativo da área de trabalho. Substitua os *applicationId* valor com a ID de produto para o seu aplicativo da área de trabalho.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/installbasedaily?applicationId=1234567890&startDate=2018-01-01&endDate=2018-02-01&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/installbasedaily?applicationId=1234567890&startDate=2018-01-01&endDate=2018-02-01&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo   | Descrição                  |
|------------|--------|-------------------------------------------------------|
| Valor      | matriz  | Uma matriz de objetos que contêm dados de instalação agregados. Para obter mais informações sobre os dados em cada objeto, consulte a tabela a seguir. |
| @nextLink  | cadeia de caracteres | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor será retornado se o parâmetro **top** da solicitação estiver definido como 10.000, mas houver mais de 10.000 linhas de dados de instalação para a consulta. |
| TotalCount | int    | O número total de linhas no resultado dos dados da consulta. |


Os elementos na matriz *Value* contêm os valores a seguir.

| Valor               | Tipo   | Descrição                           |
|---------------------|--------|-------------------------------------------|
| date                | cadeia de caracteres | A data associada com o valor de base de instalação. |
| applicationId       | cadeia de caracteres | A ID do produto do aplicativo da área de trabalho para o qual você recuperou instalar os dados. |
| productName         | cadeia de caracteres | O nome de exibição do aplicativo da área de trabalho como derivado dos metadados de seus executáveis associados. |
| applicationVersion  | cadeia de caracteres | A versão do executável do aplicativo que foi instalado. |
| deviceType          | cadeia de caracteres | Uma das seguintes cadeias de caracteres que especifica o tipo de dispositivo no qual o aplicativo de desktop é instalado:<p/><ul><li><strong>PC</strong></li><li><strong>Servidor</strong></li><li><strong>Tablet</strong></li><li><strong>Desconhecido</strong></li></ul> |
| market              | cadeia de caracteres | O código do país ISO 3166 de mercado no qual o aplicativo de área de trabalho está instalado. |
| osVersion           | cadeia de caracteres | Uma das seguintes sequências que especifica a versão do sistema operacional no qual o aplicativo da área de trabalho está instalado:<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Desconhecido</strong></li></ul>   |
| osRelease           | cadeia de caracteres | Uma das sequências a seguir que especifica a versão do sistema operacional ou anel de liberação de versões de pré-lançamento (como uma subpopulação na versão do sistema operacional) no qual o aplicativo da área de trabalho está instalado.<p/><p>No Windows 10:</p><ul><li><strong>Versão 1507</strong></li><li><strong>Versão 1511</strong></li><li><strong>Versão 1607</strong></li><li><strong>Versão 1703</strong></li><li><strong>Versão 1709</strong></li><li><strong>Release Preview</strong></li><li><strong>Insider Fast</strong></li><li><strong>Insider lento</strong></li></ul><p/><p>Para o Windows Server 1709:</p><ul><li><strong>RTM</strong></li></ul><p>Para o Windows Server 2016:</p><ul><li><strong>Versão 1607</strong></li></ul><p>No Windows 8.1:</p><ul><li><strong>Atualização 1</strong></li></ul><p>No Windows 7:</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Se a versão do sistema operacional ou anel de liberação de versão de pré-lançamento for desconhecida, este campo terá o valor <strong>Desconhecido</strong>.</p> |
| installBase         | number | O número de dispositivos distintos que tinha o produto instalado no nível de agregação especificada. |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "date": "2018-01-24",
      "applicationId": "123456789",
      "productName": "Contoso Demo",
      "applicationVersion": "1.0.0.0",
      "deviceType": "PC",
      "market": "All",
      "osVersion": "Windows 10",
      "osRelease": "Version 1709",
      "installBase": 348218.0
    }
  ],
  "@nextLink": "desktop/installbasedaily?applicationId=123456789&startDate=2018-01-01&endDate=2018-02-01&top=10000&skip=10000&groupby=applicationVersion,deviceType,osVersion,osRelease",
  "TotalCount": 23012
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Programa de aplicativo de área de trabalho do Windows](https://msdn.microsoft.com/library/windows/desktop/mt826504)
