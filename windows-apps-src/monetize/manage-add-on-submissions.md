---
ms.assetid: 66400066-24BF-4AF2-B52A-577F5C3CA474
description: Use estes métodos na API de envio da Microsoft Store para gerenciar envios de complemento dos aplicativos que estão registrados em sua conta do Partner Center.
title: Gerenciar envios de complemento
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, envios de complemento, produto no app, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 45fc2274ac22eee4a4c249397f25c1b0405cb856
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8753225"
---
# <a name="manage-add-on-submissions"></a>Gerenciar envios de complemento

A API de envio da Microsoft Store oferece métodos que é possível usar para gerenciar envios de complemento (também conhecido como produto no aplicativo ou IAP) para os aplicativos. Para obter uma introdução à API de envio da Microsoft Store, inclusive pré-requisitos para usar a API, consulte [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md).

> [!IMPORTANT]
> Se você usar a API de envio da Microsoft Store para criar um envio para um complemento, certifique-se de fazer outras alterações no envio somente por usando a API, em vez de fazer alterações no Partner Center. Se você usar o Partner Center para alterar um envio que criou originalmente usando a API, você não poderá alterar ou confirmar esse envio usando a API. Em alguns casos, o envio pode ficar em um estado de erro em que ele não pode continuar no processo de envio. Se isso ocorrer, você deve excluir o envio e criar um novo.

<span id="methods-for-add-on-submissions" />

## <a name="methods-for-managing-add-on-submissions"></a>Métodos para gerenciar envios de complementos

Use os métodos a seguir para obter, criar, atualizar, confirmar ou excluir um envio de complemento. Antes de usar esses métodos, o complemento já deve existir na sua conta do Partner Center. Você pode criar um complemento no Partner Center, [Definindo o tipo de produto e a ID do produto](../publish/set-your-add-on-product-id.md) ou usando os métodos da API de envio Microsoft Store em descrito em [Gerenciar complementos](manage-add-ons.md).

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Método</th>
<th align="left">URI</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="get-an-add-on-submission.md">Obter um envio de complemento existente</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-an-add-on-submission.md">Obter o status de um envio de complemento existente</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions</td>
<td align="left"><a href="create-an-add-on-submission.md">Criar um novo envio de complemento</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="update-an-add-on-submission.md">Atualizar um envio de complemento existente</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-an-add-on-submission.md">Confirmar um envio de complemento novo ou atualizado</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="delete-an-add-on-submission.md">Excluir um envio de complemento</a></td>
</tr>
</tbody>
</table>

<span id="create-an-add-on-submission">

## <a name="create-an-add-on-submission"></a>Criar um envio de complemento

Para criar um envio de um complemento, siga este processo.

1. Se você ainda não tiver feito isso, conclua os pré-requisitos descritos em [criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md), incluindo associar um aplicativo Azure AD à sua conta do Partner Center e obter a ID e chave do cliente. Você só precisa fazer uma vez. Depois que você tiver a ID e a chave do cliente, poderá reutilizá-las sempre que precisar criar um novo token de acesso do Azure AD.  

2. [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Você deve passar esse token de acesso aos métodos na API de envio da Microsoft Store. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

3. Execute o seguinte método na API de envio da Microsoft Store. Esse método cria um novo envio em andamento, que é uma cópia de seu último envio publicado. Para saber mais, veja [Criar um envio de complemento](create-an-add-on-submission.md).

    ```
    POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions
    ```

    O corpo da resposta contém um recurso [envio de complemento](#add-on-submission-object) que inclui a ID do novo envio, o URI da SAS (assinatura de acesso compartilhado) para o upload de todos os ícones de complemento para o envio para o armazenamento de Blobs do Azure e todos os dados do novo envio (como as listagens e as informações sobre preços).

    > [!NOTE]
    > Um URI SAS dá acesso a um recurso seguro no armazenamento do Azure sem exigir chaves de conta. Para obter informações contextuais sobre URIs SAS e o uso com o armazenamento do Blob do Azure, consulte [Assinaturas de acesso compartilhado, parte 1: Noções básicas sobre o modelo SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1) e [Assinaturas de acesso compartilhado, parte 2: Criar e usar uma SAS com o armazenamento de Blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

4. Se você estiver adicionando novos ícones para o envio, [prepare os ícones](https://msdn.microsoft.com/windows/uwp/publish/create-iap-descriptions#icon) e adicione-os a um arquivo ZIP.

5. Atualize os dados de [envio de complemento](#add-on-submission-object) com as alterações necessárias para o novo envio e execute o método a seguir para atualizar o envio. Para saber mais, veja [Atualizar um envio de complemento](update-an-add-on-submission.md).

    ```
    PUT https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}
    ```
      > [!NOTE]
      > Se você estiver adicionando novos ícones para o envio, atualize os dados de envio para fazer referência ao nome e caminho relativo desses arquivos no arquivo ZIP.

4. Se você estiver adicionando novos ícones para o envio, carregue o arquivo ZIP no [armazenamento do Blob do Azure](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage) usando o URI SAS que foi fornecido no corpo da resposta do método POST chamado anteriormente. Existem bibliotecas do Azure diferentes que é possível usar para fazer isso em uma grande variedade de plataformas, inclusive:

    * [Biblioteca de Cliente do Armazenamento do Azure para .NET](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-blobs)
    * [SDK de Armazenamento do Azure para Java](https://docs.microsoft.com/azure/storage/storage-java-how-to-use-blob-storage)
    * [SDK de Armazenamento do Azure para Python](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)

    O exemplo de código em C# a seguir demonstra como carregar um arquivo ZIP no armazenamento do Blob do Azure usando a classe [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx) na Biblioteca de Cliente do Armazenamento do Azure para .NET. Este exemplo pressupõe que o arquivo ZIP já tenha sido escrito para um objeto de fluxo.

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. Confirme o envio executando o método a seguir. Isso alertará o Partner Center que você terminou seu envio e que suas atualizações agora devem ser aplicadas à sua conta. Para saber mais, veja [Confirmar um envio de complemento](commit-an-add-on-submission.md).

    ```
    POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit
    ```

6. Verifique o status de confirmação executando o método a seguir. Para obter mais informações, consulte [Obter o status de um envio de complemento](get-status-for-an-add-on-submission.md).

    ```
    GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status
    ```

    Para confirmar o status de envio, examine o valor de *status* no corpo da resposta. Esse valor deve mudar de **CommitStarted** para **PreProcessing** se a solicitação for bem-sucedida ou **CommitFailed** se houver erros na solicitação. Se houver erros, o campo *statusDetails* contém mais detalhes sobre o erro.

7. Após a confirmação ser concluída, o envio será enviado para a Loja para inclusão. Você pode continuar a monitorar o progresso do envio usando o método anterior ou visitando o Partner Center.

<span/>

## <a name="code-examples"></a>Exemplos de código

Os artigos a seguir apresentam exemplos detalhados de código que demonstram como criar um envio de complemento em diversas linguagens de programação diferentes:

* [Exemplos de código em C#](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Exemplo de código Java](java-code-examples-for-the-windows-store-submission-api.md)
* [Exemplos de código do Python](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="storebroker-powershell-module"></a>Módulo StoreBroker do PowerShell

Como uma alternativa à chamada direta à API de envio da Microsoft Store, nós também fornecemos um módulo do PowerShell de software livre que implementa uma interface de linha de comando sobre API. Esse módulo é chamado [StoreBroker](https://aka.ms/storebroker). Você pode usar esse módulo para gerenciar seu app, versão de pré-lançamento e envios de complemento na linha de comando em vez de chamar diretamente a API de envio da Microsoft Store, ou você pode simplesmente procurar a fonte para ver mais exemplos de como chamar essa API. O módulo StoreBroker ativamente é usado dentro da Microsoft como a principal forma de muitos apps de terceiros serem enviados para a Store.

Para obter mais informações, consulte nossa [Página do StoreBroker no GitHub](https://aka.ms/storebroker).

<span/>

## <a name="data-resources"></a>Recursos de dados

Os métodos da API de envio da Microsoft Store para gerenciar envios do pacote de complemento usam os recursos de dados JSON a seguir.

<span id="add-on-submission-object" />

### <a name="add-on-submission-resource"></a>Recurso de envio de complemento

Esse recurso descreve um envio de complemento.

```json
{
  "id": "1152921504621243680",
  "contentType": "EMagazine",
  "keywords": [
    "books"
  ],
  "lifetime": "FiveDays",
  "listings": {
    "en": {
      "description": "English add-on description",
      "icon": {
        "fileName": "add-on-en-us-listing2.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (English)"
    },
    "ru": {
      "description": "Russian add-on description",
      "icon": {
        "fileName": "add-on-ru-listing.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (Russian)"
    }
  },
  "pricing": {
    "marketSpecificPricings": {
      "RU": "Tier3",
      "US": "Tier4",
    },
    "sales": [],
    "priceId": "Free",
    "isAdvancedPricingModel": true
  },
  "targetPublishDate": "2016-03-15T05:10:58.047Z",
  "targetPublishMode": "Immediate",
  "tag": "SampleTag",
  "visibility": "Public",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [
      {
        "code": "None",
        "details": "string"
      }
    ],
    "warnings": [
      {
        "code": "ListingOptOutWarning",
        "details": "You have removed listing language(s): []"
      }
    ],
    "certificationReports": [
      {
      }
    ]
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl",
  "friendlyName": "Submission 2"
}
```

Esse recurso tem os valores a seguir.

| Valor      | Tipo   | Descrição        |
|------------|--------|----------------------|
| id            | string  | A ID do envio. Essa ID está disponível nos dados de resposta para solicitações para [criar um envio de complemento](create-an-add-on-submission.md), [obter todos os complementos](get-all-add-ons.md) e [obter um complemento](get-an-add-on.md). Para um envio que foi criado no Partner Center, essa ID também está disponível na URL da página de envio no Partner Center.  |
| contentType           | string  |  O [tipo de conteúdo](../publish/enter-add-on-properties.md#content-type) fornecido no complemento. Ele pode ter um dos seguintes valores: <ul><li>NotSet</li><li>BookDownload</li><li>EMagazine</li><li>ENewspaper</li><li>MusicDownload</li><li>MusicStream</li><li>OnlineDataStorage</li><li>VideoDownload</li><li>VideoStream</li><li>Asp</li><li>OnlineDownload</li></ul> |  
| keywords           | array  | Uma matriz de cadeias de caracteres que contenham até 10 [palavras-chave](../publish/enter-add-on-properties.md#keywords) do complemento. O aplicativo pode consultar complementos usando essas palavras-chave.   |
| lifetime           | string  |  O tempo de vida do complemento. Ele pode ter um dos seguintes valores: <ul><li>Forever</li><li>OneDay</li><li>ThreeDays</li><li>FiveDays</li><li>OneWeek</li><li>TwoWeeks</li><li>OneMonth</li><li>TwoMonths</li><li>ThreeMonths</li><li>SixMonths</li><li>OneYear</li></ul> |
| listings           | object  |  Um dicionário de pares de chave e valor, em que cada chave é um código ISO 3166-1 alpha-2 de duas letras do país e cada valor é um [recurso de listagem](#listing-object) que contém informações de listagem do complemento.  |
| pricing           | objeto  | Um [recurso de preço](#pricing-object) que contém informações de preço para o complemento.   |
| targetPublishMode           | string  | O modo de publicação do envio. Ele pode ter um dos seguintes valores: <ul><li>Imediata</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | A data de publicação do envio em formato ISO 8601, se o *targetPublishMode* estiver definido como SpecificDate.  |
| tag           | string  |  Os [dados de desenvolvedor personalizados](../publish/enter-add-on-properties.md#custom-developer-data) do complemento (essas informações foram anteriormente chamadas de *marca*).   |
| visibilidade  | string  |  A visibilidade do complemento. Ele pode ter um dos seguintes valores: <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>  |
| status  | string  |  O status do envio. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>Cancelado</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publicação</li><li>Publicado</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certificação</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | objeto  |  Um [recurso de detalhes do status](#status-details-object) que contém detalhes adicionais sobre o status do envio, inclusive informações sobre eventuais erros. |
| fileUploadUrl           | string  | O URI da assinatura de acesso compartilhado (SAS) para carregar todos os pacotes para o envio. Se você estiver adicionando novos pacotes para o envio, carregue o arquivo ZIP que contém os pacotes para essa URI. Para saber mais, veja [Criar um envio de complemento](#create-an-add-on-submission).  |
| friendlyName  | string  |  O nome amigável do envio, conforme mostrado no Partner Center. Esse valor é gerado para você ao criar o envio.  |

<span id="listing-object" />

### <a name="listing-resource"></a>Recurso de listagem

Esse recurso contém [informações de listagem de um complemento](../publish/create-add-on-store-listings.md). Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição       |
|-----------------|---------|------|
|  description               |    cadeia de caracteres     |   A descrição da listagem do complemento.   |     
|  icon               |   objeto      |Um [recurso de ícone](#icon-object) que contém dados do ícone para a listagem de complemento.    |
|  title               |     cadeia de caracteres    |   O título da listagem do complemento.   |  

<span id="icon-object" />

### <a name="icon-resource"></a>Recurso de ícone

Esse recurso contém dados de ícone para a listagem de um complemento. Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição     |
|-----------------|---------|------|
|  fileName               |    string     |   O nome do arquivo de ícone no arquivo ZIP que você carregou para o envio. O ícone deve ser um arquivo .png que meça exatamente 300 x 300 pixels.   |     
|  fileStatus               |   string      |  O status do arquivo de ícone. Isso pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>PendingUpload</li><li>Carregado</li><li>PendingDelete</li></ul>   |

<span id="pricing-object" />

### <a name="pricing-resource"></a>Recurso de preço

Esse recurso contém informações de preço do complemento. Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição    |
|-----------------|---------|------|
|  marketSpecificPricings               |    object     |  Um dicionário de pares de chave e valor, onde cada chave é um código ISO 3166-1 alpha-2 de duas letras do país e cada valor é uma [faixa de preço](#price-tiers). Esses itens representam os [preços personalizados do complemento em mercados específicos](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#markets-and-custom-prices). Todos os itens nesse dicionário substituem o preço base especificado pelo valor *priceId* para o mercado especificado.     |     
|  sales               |   matriz      |  **Preterido**. Uma matriz de [recursos de venda](#sale-object) que contêm informações de venda do complemento.     |     
|  priceId               |   cadeia de caracteres      |  A [faixa de preço](#price-tiers) que especifica o [preço base](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#base-price) do complemento.    |    
|  isAdvancedPricingModel               |   booliano      |  Se for **true**, sua conta de desenvolvedor tem acesso ao conjunto expandido de faixas de preço de US$ 0,99 a US$ 1999,99. Se for **true**, sua conta de desenvolvedor tem acesso ao conjunto original de faixas de preço de US$ 0,99 a US$ 999,99. Para saber mais sobre as diferentes camadas, consulte [faixas de preço](#price-tiers).<br/><br/>**Observação**&nbsp;&nbsp;Esse campo é somente leitura.   |


<span id="sale-object" />

### <a name="sale-resource"></a>Recurso de venda

Esse recurso contém informações de venda de um complemento.

> [!IMPORTANT]
> O recurso **Venda** não tem mais suporte, e atualmente você não pode acessar nem modificar os dados de venda de um envio de complemento usando a API de envio da Microsoft Store. No futuro, atualizaremos a API de envio da Microsoft Store para apresentar uma nova maneira de acessar programaticamente as informações de vendas para envios de complemento.
>    * Depois de chamar o [método GET para obter um envio de complemento](get-an-add-on-submission.md), o valor de *sales* estará vazio. Você pode continuar a usar o Partner Center para obter os dados de venda de seu envio de complemento.
>    * Ao chamar o [método PUT para atualizar um envio de complemento](update-an-add-on-submission.md), as informações no valor de *sales* serão ignoradas. Você pode continuar a usar o Partner Center para alterar os dados de venda de seu envio de complemento.

Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição           |
|-----------------|---------|------|
|  name               |    string     |   O nome da promoção.    |     
|  basePriceId               |   string      |  A [faixa de preço](#price-tiers) a ser usada para o preço base da promoção.    |     
|  startDate               |   string      |   A data de início da promoção no formato ISO 8601.  |     
|  endDate               |   string      |  A data de término da promoção no formato ISO 8601.      |     
|  marketSpecificPricings               |   object      |   Um dicionário de pares de chave e valor, onde cada chave é um código ISO 3166-1 alpha-2 de duas letras do país e cada valor é uma [faixa de preço](#price-tiers). Esses itens representam os [preços personalizados do complemento em mercados específicos](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#markets-and-custom-pricess). Todos os itens nesse dicionário substituem o preço base especificado pelo valor *basePriceId* para o mercado especificado.    |

<span id="status-details-object" />

### <a name="status-details-resource"></a>Recurso de detalhes do status

Esse recurso contém detalhes adicionais sobre o status de um envio. Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição       |
|-----------------|---------|------|
|  errors               |    objeto     |   Uma matriz de [recursos de detalhes do status](#status-detail-object) que contêm detalhes do erro para o envio.   |     
|  warnings               |   objeto      | Uma matriz de [recursos de detalhes do status](#status-detail-object) que contêm detalhes do aviso para o envio.     |
|  certificationReports               |     objeto    |   Uma matriz de [recursos do relatório de certificação](#certification-report-object) que dão acesso aos dados do relatório de certificação para o envio. Será possível examinar esses relatórios para obter mais informações, se a certificação falhar.    |  

<span id="status-detail-object" />

### <a name="status-detail-resource"></a>Recurso de detalhes do status

Esse recurso contém informações adicionais sobre todos os erros ou avisos relacionados a um envio. Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição    |
|-----------------|---------|------|
|  code               |    cadeia de caracteres     |   Um [código de status do envio](#submission-status-code) que descreve o tipo de erro ou aviso.   |     
|  details               |     cadeia de caracteres    |  Uma mensagem com mais detalhes sobre o problema.     |

<span id="certification-report-object" />

### <a name="certification-report-resource"></a>Recurso do relatório de certificação

Esse recurso dá acesso aos dados do relatório de certificação para um envio. Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição               |
|-----------------|---------|------|
|     date            |    string     |  A data e hora que o relatório foi gerado, no formato ISO 8601.    |
|     reportUrl            |    string     |  A URL na qual você pode acessar o relatório.    |

## <a name="enums"></a>Enumerações

Esses métodos usam as enumerações a seguir.

<span id="price-tiers" />

### <a name="price-tiers"></a>Faixas de preço

Os seguintes valores representam as faixas de preço disponíveis no recurso [preço do recurso](#pricing-object) para um envio de complemento.

| Valor           | Descrição       |
|-----------------|------|
|  Base               |   A faixa de preço não está definida. Use o preço base para o complemento.      |     
|  NotAvailable              |   O complemento não está disponível na região especificada.    |     
|  Grátis              |   O complemento é gratuito.    |    
|  Faixa de*xxxx*               |   Uma cadeia de caracteres que especifica a faixa de preço do complemento, no formato **Faixa<em>xxxx</em>**. No momento, há suporte para os seguintes intervalos de faixas de preço:<br/><br/><ul><li>Se o valor *isAdvancedPricingModel* do [preço do recurso](#pricing-object) for **true**, os valores de nível de preço disponíveis para sua conta são **Tier1012** - **Tier1424**.</li><li>Se o valor *isAdvancedPricingModel* do [preço do recurso](#pricing-object) for **false**, os valores de nível de preço disponíveis para sua conta são **Tier2** - **Tier96**.</li></ul>Para ver a tabela completa de preço níveis que estão disponíveis para sua conta de desenvolvedor, incluindo os preços de mercado específicos que estão associados a cada camada, vá para a página **preço e disponibilidade** para qualquer um dos envios de seus aplicativos no Partner Center e Clique no link **Exibir tabela** na seção **mercados e preços personalizados** (para algumas contas de desenvolvedor, este link está na seção **preços** ).     |

<span id="submission-status-code" />

### <a name="submission-status-code"></a>Código de status do envio

Os seguintes valores representam o código de status de um envio.

| Valor           |  Descrição      |
|-----------------|---------------|
|  Nenhum(a)            |     Nenhum código foi especificado.         |     
|      InvalidArchive        |     O arquivo ZIP que contém o pacote não é válido ou tem um formato de arquivo não reconhecido.  |
| MissingFiles | O arquivo ZIP não tem todos os arquivos que foram listados nos seus dados de envio ou estão no local errado no arquivo. |
| PackageValidationFailed | Falha ao validar um ou mais pacotes no seu envio. |
| InvalidParameterValue | Um dos parâmetros no corpo da solicitação não é válido. |
| InvalidOperation | A operação tentada não é válida. |
| InvalidState | A operação tentada não é válida para o estado atual do pacote de pré-lançamento. |
| ResourceNotFound | O pacote de pré-lançamento especificado não foi encontrado. |
| ServiceError | Um erro de serviço interno impediu o êxito da solicitação. Repita a solicitação. |
| ListingOptOutWarning | O desenvolvedor removeu uma listagem de um envio anterior ou não incluiu informações de listagem que são compatíveis com o pacote. |
| ListingOptInWarning  | O desenvolvedor adicionou uma listagem. |
| UpdateOnlyWarning | O desenvolvedor está tentando inserir algo que só tem suporte para a atualização. |
| Outro  | O envio está em um estado não reconhecido ou não categorizado. |
| PackageValidationWarning | O processo de validação do pacote resultou em um aviso. |

<span/>

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gerenciar complementos usando a API de envio da Microsoft Store](manage-add-ons.md)
* [Envios de complemento no Partner Center](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions)
