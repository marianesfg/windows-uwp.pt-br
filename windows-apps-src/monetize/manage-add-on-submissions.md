---
author: mcleanbyron
ms.assetid: 66400066-24BF-4AF2-B52A-577F5C3CA474
description: "Use estes métodos na API de envio da Windows Store para gerenciar envios de complemento dos aplicativos que estão registrados em sua conta do Centro de Desenvolvimento do Windows."
title: Gerenciar envios de complemento usando a API de envio da Windows Store
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 52e589c90a8d78905a9617dc2802d76a2f0f0360

---

# Gerenciar envios de complemento usando a API de envio da Windows Store



Use os métodos a seguir na API de envio da Windows Store para gerenciar envios de complemento (também conhecidos como produto no aplicativo ou IAP) de aplicativos que estão registrados na sua conta do Centro de Desenvolvimento do Windows. Para obter uma introdução à API de envio da Windows Store, incluindo os pré-requisitos para usar a API, consulte [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md).

>**Observação**&nbsp;&nbsp;Estes métodos só podem ser usados para contas do Centro de Desenvolvimento do Windows que receberam permissões para usar a API de envio da Windows Store. Nem todas as contas têm essa permissão habilitada. Antes de usar esses métodos para criar ou gerenciar envios de um complemento, o complemento já deve existir na sua conta do Centro de Desenvolvimento. Você pode criar um complemento [usando o painel do Centro de Desenvolvimento](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions) ou usando os métodos da API de envio da Windows Store descritos em [Gerenciar complementos](manage-add-ons.md).

| Método        | URI    | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}``` | Obtém dados para um envio de complemento existente. Para saber mais, veja [Obter um envio de complemento](get-an-add-on-submission.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status``` | Obtém o status de um envio de complemento existente. Para obter mais informações, consulte [Obter o status de um envio de complemento](get-status-for-an-add-on-submission.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions``` | Cria um novo envio de complemento para um aplicativo que está registrado em sua conta do Centro de Desenvolvimento do Windows. Para saber mais, veja [Criar um envio de complemento](create-an-add-on-submission.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit``` | Confirma um envio de complemento novo ou atualizado para o Centro de Desenvolvimento do Windows. Para saber mais, veja [Confirmar um envio de complemento](commit-an-add-on-submission.md). |
| PUT | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}``` | Atualiza um envio de complemento existente. Para saber mais, veja [Atualizar um envio de complemento](update-an-add-on-submission.md). |
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}``` | Exclui um envio de complemento. Para obter mais informações, consulte [Excluir um envio de complemento](delete-an-add-on-submission.md). |

<span id="create-an-add-on-submission">
## Criar um envio de complemento

Para criar um envio de um complemento, siga este processo.

1. Se você ainda não tiver feito isso, conclua os pré-requisitos descritos em [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md), incluindo associar um aplicativo do Azure AD à sua conta do Centro de Desenvolvimento do Windows e obter a ID e a chave do cliente. Você só precisa fazer uma vez. Depois que você tiver a ID e a chave do cliente, poderá reutilizá-las sempre que precisar criar um novo token de acesso do Azure AD.  

2. [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Você deve passar esse token de acesso aos métodos na API de envio da Windows Store. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

3. Execute o seguinte método na API de envio da Windows Store. Esse método cria um novo envio em andamento, que é uma cópia de seu último envio publicado. Para saber mais, veja [Criar um envio de complemento](create-an-add-on-submission.md).

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions
  ```

  O corpo da resposta contém três itens: a ID do novo envio, os dados para o novo envio (incluindo todas as listagens e informações de preço) e a URI da assinatura de acesso compartilhado (SAS) para carregar todos os ícones de complemento para o envio. Para saber mais sobre SAS, consulte [Assinaturas de acesso compartilhado, parte 1: Noções básicas sobre o modelo SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).

4. Se você estiver adicionando novos ícones para o envio, [preparar os ícones](https://msdn.microsoft.com/windows/uwp/publish/create-iap-descriptions#icon) e adicione-os a um arquivo ZIP.

5. Atualize os dados de envio com as alterações necessárias para o novo envio e execute o método a seguir para atualizar o envio. Para saber mais, veja [Atualizar um envio de complemento](update-an-add-on-submission.md).

  ```
  PUT https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}
  ```

  >**Observação**&nbsp;&nbsp;Se você estiver adicionando novos ícones para o envio, atualize os dados de envio para fazer referência ao nome e caminho relativo desses arquivos no arquivo ZIP.

4. Se você estiver adicionando novos ícones para o envio, carregue o arquivo ZIP na URI de SAS que foi fornecida no corpo da resposta do método POST chamado na etapa 2. Para obter mais informações, consulte [Assinaturas de acesso compartilhado, parte 2: Criar e usar uma SAS com o armazenamento de Blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

  O snippet de código a seguir demonstra como carregar o arquivo usando a classe [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx) na biblioteca de cliente de armazenamento do Azure para .NET.

  ```csharp
  string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";

  Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
  await blockBob.UploadFromStreamAsync(stream);
  ```

5. Confirme o envio executando o método a seguir. Isso alertará o Centro de Desenvolvimento que você terminou seu envio e que suas atualizações agora devem ser aplicadas à sua conta. Para saber mais, veja [Confirmar um envio de complemento](commit-an-add-on-submission.md).

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit
  ```

6. Verifique o status de confirmação executando o método a seguir. Para obter mais informações, consulte [Obter o status de um envio de complemento](get-status-for-an-add-on-submission.md).

  ```
  GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status
  ```

  Para confirmar o status de envio, examine o valor de *status* no corpo da resposta. Esse valor deve mudar de **CommitStarted** para **PreProcessing** se a solicitação for bem-sucedida ou **CommitFailed** se houver erros na solicitação. Se houver erros, o campo *statusDetails* contém mais detalhes sobre o erro.

7. Após a confirmação ser concluída, o envio será enviado para a Loja para inclusão. Você pode continuar a monitorar o progresso de envio usando o método anterior ou visitando o painel do Centro de Desenvolvimento.

## Recursos

Esses métodos usam os recursos a seguir para formatar dados.

<span id="add-on-submission-object" />
### Envio de complemento

Esse recurso representa um envio para um complemento. O exemplo a seguir demonstra o formato desse recurso.

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
    "sales": [
      {
         "name": "Sale1",
         "basePriceId": "Free",
         "startDate": "2016-05-21T18:40:11.7369008Z",
         "endDate": "2016-05-22T18:40:11.7369008Z",
         "marketSpecificPricings": {
            "RU": "NotAvailable"
         }
      }
    ],
    "priceId": "Free"
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

Este recurso tem os seguintes valores.

| Valor      | Tipo   | Descrição                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | string  | A ID do envio.  |
| contentType           | string  |  O [tipo de conteúdo](https://msdn.microsoft.com/windows/uwp/publish/enter-iap-properties#content-type) fornecido no complemento. Ele pode ter um dos seguintes valores: <ul><li>NotSet</li><li>BookDownload</li><li>EMagazine</li><li>ENewspaper</li><li>MusicDownload</li><li>MusicStream</li><li>OnlineDataStorage</li><li>VideoDownload</li><li>VideoStream</li><li>Asp</li><li>OnlineDownload</li></ul> |  
| keywords           | array  | Uma matriz de cadeias de caracteres que contenham até 10 [palavras-chave](../publish/enter-iap-properties.md#keywords) do complemento. O aplicativo pode consultar complementos usando essas palavras-chave.   |
| lifetime           | string  |  O tempo de vida do complemento. Ele pode ter um dos seguintes valores: <ul><li>Forever</li><li>OneDay</li><li>ThreeDays</li><li>FiveDays</li><li>OneWeek</li><li>TwoWeeks</li><li>OneMonth</li><li>TwoMonths</li><li>ThreeMonths</li><li>SixMonths</li><li>OneYear</li></ul> |
| listings           | object  |  Um dicionário de pares de chave e valor, onde cada chave é um código ISO 3166-1 alpha-2 de duas letras do país e cada valor é um objeto de [Recurso de listagem](#listing-object) que contém as informações de detalhes do complemento.  |
| pricing           | object  | Um objeto que contém as informações de preços do complemento. Para saber mais, veja a seção [Preços de recurso](#pricing-object) abaixo.  |
| targetPublishMode           | string  | O modo de publicação do envio. Ele pode ter um dos seguintes valores: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | A data de publicação do envio em formato ISO 8601, se o *targetPublishMode* estiver definido como SpecificDate.  |
| tag           | string  |  A [marca](../publish/enter-iap-properties.md#tag) do complemento.   |
| visibility  | string  |  A visibilidade do complemento. Ele pode ter um dos seguintes valores: <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>  |
| status  | string  |  O status do envio. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>Cancelado</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publicação</li><li>Publicado</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certificação</li><li>CertificationFailed</li><li>Versão</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | object  |  Contém detalhes adicionais sobre o status do envio, incluindo informações sobre os erros. Para saber mais, consulte a seção [Detalhes de status](#status-details-object) a seguir. |
| fileUploadUrl           | string  | A URI da assinatura de acesso compartilhado (SAS) para carregar todos os pacotes para o envio. Se você estiver adicionando novos pacotes para o envio, carregue o arquivo ZIP que contém os pacotes para essa URI. Para saber mais, veja [Criar um envio de complemento](#create-an-add-on-submission).  |
| friendlyName  | string  |  O nome amigável do complemento, usado para propósitos de exibição.  |

<span id="listing-object" />
### Listagem

Esse recurso contém informações de detalhes de um complemento. Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  description               |    string     |   A descrição da listagem do complemento.   |     
|  icon               |   object      |  Contém dados do ícone para a listagem do complemento. Para saber mais, consulte a seção [Ícone](#icon-object) a seguir.   |
|  title               |     string    |   O título da listagem do complemento.   |  

<span id="icon-object" />
### Ícone

Esse recurso contém dados de ícone para a listagem de um complemento. Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  fileName               |    string     |   O nome do arquivo de ícone no arquivo ZIP que você carregou para o envio.    |     
|  fileStatus               |   string      |  O status do arquivo de ícone. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>PendingUpload</li><li>Carregado</li><li>PendingDelete</li></ul>   |

<span id="pricing-object" />
### Preço

Esse recurso contém informações de preço do complemento. Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  marketSpecificPricings               |    object     |  Um dicionário de pares de chave e valor, onde cada chave é um código ISO 3166-1 alpha-2 de duas letras do país e cada valor é uma [faixa de preço](#price-tiers). Esses itens representam os [preços personalizados do complemento em mercados específicos](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#markets-and-custom-prices). Todos os itens nesse dicionário substituem o preço base especificado pelo valor de *priceId* para o mercado especificado.     |     
|  sales               |   array      |  Uma matriz de objetos que contêm informações de promoções do complemento. Para saber mais, consulte a seção [Promoção](#sale-object) a seguir.    |     
|  priceId               |   string      |  A [faixa de preço](#price-tier) que especifica o [preço base](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#base-price) do complemento.    |


<span id="sale-object" />
### Promoção

Esse recursos contém informações de promoção de um complemento. Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  name               |    string     |   O nome da promoção.    |     
|  basePriceId               |   string      |  A [faixa de preço](#price-tiers) a ser usada para o preço base da promoção.    |     
|  startDate               |   string      |   A data de início da promoção no formato ISO 8601.  |     
|  endDate               |   string      |  A data de término da promoção no formato ISO 8601.      |     
|  marketSpecificPricings               |   object      |   Um dicionário de pares de chave e valor, onde cada chave é um código ISO 3166-1 alpha-2 de duas letras do país e cada valor é uma [faixa de preço](#price-tiers). Esses itens representam os [preços personalizados do complemento em mercados específicos](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#markets-and-custom-pricess). Todos os itens nesse dicionário substituem o preço base especificado pelo valor de *basePriceId* para o mercado especificado.    |



<span id="status-details-object" />
### Detalhes de status

Esse recurso contém detalhes adicionais sobre o status de um envio. Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  errors               |    object     |   Uma matriz de objetos que contêm os detalhes de erro do envio. Para saber mais, consulte a seção [Detalhes de status](#status-detail-object) a seguir.   |     
|  warnings               |   object      | Uma matriz de objetos que contêm os detalhes de aviso do envio. Para saber mais, consulte a seção [Detalhes de status](#status-detail-object) a seguir.     |
|  certificationReports               |     object    |   Uma matriz de objetos que fornecem acesso aos dados do relatório de certificação para o envio. Se a certificação falhar, você poderá examinar esses relatórios para obter mais informações. Para saber mais, consulte a seção [Relatório de certificação](#certification-report-object) a seguir.   |  


<span id="status-detail-object" />
### Detalhes de status

Esse recurso contém informações adicionais sobre erros ou avisos relatados para um envio. Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  code               |    string     |   Uma cadeia de caracteres que descreve o tipo de erro ou aviso. Para obter mais informações, consulte a seção [Código de status de envio](#submission-status-code) abaixo.   |     
|  details               |     string    |  Uma mensagem com mais detalhes sobre o problema.     |


<span id="certification-report-object" />
### Relatório de certificação

Esse recurso fornece acesso aos dados do relatório de certificação para um envio. Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição                                                                                                                                                                                                                          |
|-----------------|---------|------|
|     date            |    string     |  A data e hora em que o relatório foi gerado, no formato ISO 8601.    |
|     reportUrl            |    string     |  A URL na qual você pode acessar o relatório.    |



## Enumerações

Esses métodos usam as enumerações a seguir.


<span id="price-tiers" />
### Faixas de preço

Os seguintes valores representam as faixas de preço disponíveis para um envio de complemento.

| Valor           | Descrição                                                                                                                                                                                                                          |
|-----------------|------|
|  Base               |   A faixa de preço não está definida. Use o preço base para o complemento.      |     
|  NotAvailable              |   O complemento não está disponível na região especificada.    |     
|  Grátis              |   O complemento é gratuito.    |    
|  Tier2 até Tier194               |   Tier2 representa a faixa de preço 0,99 USD. Cada nível adicional representa incrementos adicionais (1,29 USD, 1,49 USD, 1,99 USD e assim por diante).    |


<span id="submission-status-code" />
### Código de status do envio

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

## Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gerenciar complementos usando a API de envio da Windows Store](manage-add-ons.md)
* [Envios de complemento no painel do Centro de Desenvolvimento](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions)



<!--HONumber=Aug16_HO5-->


