---
description: Use este método na API de análise da Microsoft Store para baixar o arquivo CAB para um erro em seu jogo Xbox One.
title: Baixar o arquivo CAB para um erro em seu jogo Xbox One
ms.date: 11/06/2018
ms.topic: article
keywords: windows 10, uwp, API de análise da Microsoft Store, baixar CAB
ms.localizationpriority: medium
ms.openlocfilehash: 736219533a254e6380c10600e97f707f15e37de6
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "7985223"
---
# <a name="download-the-cab-file-for-an-error-in-your-xbox-one-game"></a>Baixar o arquivo CAB para um erro em seu jogo Xbox One

Use este método na API de análise da Microsoft Store para baixar o arquivo CAB que está associado um erro específico em seu jogo Xbox One que foi inserido por meio do Portal de desenvolvedor do Xbox (XDP) e está disponível no painel do Centro de parceiro de análise XDP. Esse método pode apenas baixar o arquivo CAB para um erro que ocorreu nos últimos 30 dias.

Antes de usar esse método, primeiro você deve usar o método [obter detalhes de um erro em seu jogo Xbox One](get-details-for-an-error-in-your-xbox-one-game.md) para recuperar a ID do arquivo CAB que você deseja baixar.

## <a name="prerequisites"></a>Pré-requisitos


Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.
* Obtenha a ID do arquivo CAB que você deseja baixar. Para obter essa ID, use o método [obter detalhes de um erro em seu Xbox One jogo](get-details-for-an-error-in-your-xbox-one-game.md) para recuperar os detalhes de um erro específico em seu aplicativo e use o valor de **cabId** no corpo da resposta desse método.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/cabdownload``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  |
|---------------|--------|---------------|------|
| applicationId | string | A ID do produto do jogo Xbox One para o qual você está baixando o arquivo CAB. Para obter a ID do produto do jogo, navegue até seu jogo no Portal de Desenvolvedor do Xbox (XDP) e recupere a ID do produto da URL. Como alternativa, se você baixar os dados de integridade do relatório de análise do Partner Center do Windows, a ID do produto estará incluída no arquivo. tsv. |  Sim  |
| cabId | string | A ID exclusiva do arquivo CAB que você deseja baixar. Para obter essa ID, use o método [obter detalhes de um erro em seu Xbox One jogo](get-details-for-an-error-in-your-xbox-one-game.md) para recuperar os detalhes de um erro específico em seu aplicativo e use o valor de **cabId** no corpo da resposta desse método. |  Sim  |

 
### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como obter baixar um arquivo CAB usando esse método. Substitua os parâmetros *applicationId* e *cabId* pelos valores apropriados para seu aplicativo.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/cabdownload?applicationId=BRRT4NJ9B3D1&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

Esse método retorna um código de resposta 302 (redirecionamento) e o cabeçalho **Local** na resposta é atribuído à URI de assinatura do acesso compartilhado (SAS) do arquivo CAB. O chamador é redirecionado para essa URI a fim de baixar automaticamente o arquivo CAB.

## <a name="related-topics"></a>Tópicos relacionados

* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter dados de relatório para seu Xbox One erros jogo](get-error-reporting-data-for-your-xbox-one-game.md)
* [Obter detalhes de um erro em seu Xbox One jogo](get-details-for-an-error-in-your-xbox-one-game.md)
* [Obter o rastreamento de pilha de um erro em seu Xbox One jogo](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)
