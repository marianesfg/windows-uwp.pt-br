---
author: mcleanbyron
ms.assetid: 2A454057-FF14-40D2-8ED2-CEB5F27E0226
description: "Use estes métodos na API de envio da Windows Store para gerenciar envios de pacote de pré-lançamento dos aplicativos que estão registrados em sua conta do Centro de Desenvolvimento do Windows."
title: "Gerenciar envios de pacote de pré-lançamento usando a API de envio da Windows Store"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 18d28495b80101cf5cfe53869b0f5cd3d61b50c9

---

# Gerenciar envios de pacote de pré-lançamento usando a API de envio da Windows Store




Use os métodos a seguir na API de envio da Windows Store para gerenciar envios de pacote de pré-lançamento dos aplicativos que estão registrados em sua conta do Centro de Desenvolvimento do Windows. Para obter uma introdução à API de envio da Windows Store, incluindo os pré-requisitos para usar a API, consulte [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md).

>**Observação**&nbsp;&nbsp;Estes métodos só podem ser usados para contas do Centro de Desenvolvimento do Windows que receberam permissões para usar a API de envio da Windows Store. Nem todas as contas têm essa permissão habilitada. Antes de usar esses métodos para criar ou gerenciar envios de pacote de pré-lançamento, o pacote de pré-lançamento já deve existir na sua conta do Centro de Desenvolvimento. Você pode criar um pacote de pré-lançamento [usando o painel do Centro de Desenvolvimento](https://msdn.microsoft.com/windows/uwp/publish/package-flights) ou usando os métodos de API de envio da Windows Store descritos em [Gerenciar pacotes de pré-lançamento](manage-flights.md).

| Método        | URI    | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}``` | Obtém dados de um envio de pacote de pré-lançamento existente. Para obter mais informações, consulte [Obter um envio de pacote de pré-lançamento](get-a-flight-submission.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status``` | Obtém o status de um envio de pacote de pré-lançamento existente. Para obter mais informações, consulte [Obter o status de um envio de pacote de pré-lançamento](get-status-for-a-flight-submission.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/applications/{applicationId}/flights/{flightId}/submissions``` | Cria um novo envio de pacote de pré-lançamento para um aplicativo que está registrado em sua conta do Centro de Desenvolvimento do Windows. Para saber mais, veja [Criar um envio de pacote de pré-lançamento](create-a-flight-submission.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit``` | Confirma um envio de pacote de pré-lançamento novo ou atualizado para o Centro de Desenvolvimento do Windows. Para saber mais, veja [Confirmar um envio de pacote de pré-lançamento](commit-a-flight-submission.md). |
| PUT | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}``` | Atualiza um envio de pacote de pré-lançamento existente. Para saber mais, veja [Atualizar um envio de pacote de pré-lançamento](update-a-flight-submission.md). |
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}``` | Exclui um envio de pacote de pré-lançamento. Para saber mais, veja [Excluir um envio de pacote de pré-lançamento](delete-a-flight-submission.md). |

<span id="create-a-package-flight-submission">
## Criar um envio de pacote de pré-lançamento

Para criar um envio para um pacote de pré-lançamento, siga este processo.

1. Se você ainda não tiver feito isso, conclua os pré-requisitos descritos em [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md), incluindo associar um aplicativo do Azure AD à sua conta do Centro de Desenvolvimento do Windows e obter a ID e a chave do cliente. Você só precisa fazer uma vez. Depois que você tiver a ID e a chave do cliente, poderá reutilizá-las sempre que precisar criar um novo token de acesso do Azure AD.  

2. [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Você deve passar esse token de acesso aos métodos na API de envio da Windows Store. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

3. Execute o seguinte método na API de envio da Windows Store. Esse método cria um novo envio em andamento, que é uma cópia de seu último envio publicado. Para saber mais, veja [Criar um envio de pacote de pré-lançamento](create-a-flight-submission.md).

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications{applicationId}/flights/{flightId}/submissions
  ```

  O corpo da resposta contém três itens: a ID do novo envio, os dados para o novo envio (incluindo todas as listagens e informações de preço) e a URI da assinatura de acesso compartilhado (SAS) para carregar todos os pacotes para o envio. Para saber mais sobre SAS, consulte [Assinaturas de acesso compartilhado, parte 1: Noções básicas sobre o modelo SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).

4. Se você estiver adicionando novos pacotes para o envio, [preparar os pacotes](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements) e adicione-os a um arquivo ZIP.

5. Atualize os dados de envio com as alterações necessárias para o novo envio e execute o método a seguir para atualizar o envio. Para saber mais, veja [Atualizar um envio de pacote de pré-lançamento](update-a-flight-submission.md).

  ```
  PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}
  ```

  >**Observação**&nbsp;&nbsp;Se você estiver adicionando novos pacotes para o envio, atualize os dados de envio para fazer referência ao nome e caminho relativo desses arquivos no arquivo ZIP.

4. Se você estiver adicionando novos pacotes para o envio, carregue o arquivo ZIP na URI de SAS que foi fornecida no corpo da resposta do método POST chamado na etapa 2. Para obter mais informações, consulte [Assinaturas de acesso compartilhado, parte 2: Criar e usar uma SAS com o armazenamento de Blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

  O snippet de código a seguir demonstra como carregar o arquivo usando a classe [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx) na biblioteca de cliente de armazenamento do Azure para .NET.

  ```csharp
  string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";

  Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
  await blockBob.UploadFromStreamAsync(stream);
  ```

5. Confirme o envio executando o método a seguir. Isso alertará o Centro de Desenvolvimento que você terminou seu envio e que suas atualizações agora devem ser aplicadas à sua conta. Para saber mais, veja [Confirmar um envio de pacote de pré-lançamento](commit-a-flight-submission.md).

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit
  ```

6. Verifique o status de confirmação executando o método a seguir. Para obter mais informações, consulte [Obter o status de um envio de pacote de pré-lançamento](get-status-for-a-flight-submission.md).

  ```
  GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status
  ```

  Para confirmar o status de envio, examine o valor de *status* no corpo da resposta. Esse valor deve mudar de **CommitStarted** para **PreProcessing** se a solicitação for bem-sucedida ou **CommitFailed** se houver erros na solicitação. Se houver erros, o campo *statusDetails* contém mais detalhes sobre o erro.

7. Após a confirmação ser concluída, o envio será enviado para a Loja para inclusão. Você pode continuar a monitorar o progresso de envio usando o método anterior ou visitando o painel do Centro de Desenvolvimento.

## Recursos

Esses métodos usam os seguintes recursos de dados.

<span id="flight-submission-object" />
### Envio de pacote de pré-lançamento

Esse recurso representa um envio para um pacote de pré-lançamento. O exemplo a seguir demonstra o formato desse recurso.

```json
{
  "id": "1152921504621243649",
  "flightId": "cd2e368a-0da5-4026-9f34-0e7934bc6f23",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/8b389577-5d5e-4cbe-a744-1ff2e97a9eb8?sv=2014-02-14&sr=b&sig=wgMCQPjPDkuuxNLkeG35rfHaMToebCxBNMPw7WABdXU%3D&se=2016-06-17T21:29:44Z&sp=rwl",
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

Este recurso tem os seguintes valores.

| Valor      | Tipo   | Descrição                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | string  | A ID do envio.  |
| flightId           | string  |  A ID do pacote de pré-lançamento ao qual o envio está associado.  |  
| status           | string  | O status do envio. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>Cancelado</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publicação</li><li>Publicado</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certificação</li><li>CertificationFailed</li><li>Versão</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | object  |  Contém detalhes adicionais sobre o status do envio, incluindo informações sobre os erros. Para saber mais, consulte a seção [Detalhes de status](#status-details-object) a seguir. |
| flightPackages           | array  | Contém objetos que fornecem detalhes sobre cada pacote no envio. Para saber mais, veja a seção [Pacote de versão de pré-lançamento](#flight-package-object) abaixo.  |
| fileUploadUrl           | string  | A URI da assinatura de acesso compartilhado (SAS) para carregar todos os pacotes para o envio. Se você estiver adicionando novos pacotes para o envio, carregue o arquivo ZIP que contém os pacotes para essa URI. Para saber mais, veja [Criar um envio de pacote de pré-lançamento](#create-a-package-flight-submission).  |
| targetPublishMode           | string  | O modo de publicação do envio. Ele pode ter um dos seguintes valores: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | A data de publicação do envio em formato ISO 8601, se o *targetPublishMode* estiver definido como SpecificDate.  |
| notesForCertification           | string  |  Fornece informações adicionais para os testadores de certificação, como credenciais da conta de teste e as etapas para acessar e confirmar recursos. Para obter mais informações, consulte [Notas para certificação](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification). |

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


<span id="flight-package-object" />
### Pacote de pré-lançamento

Esse recurso fornece detalhes sobre um pacote em um envio. O exemplo a seguir demonstra o formato desse recurso.

```json
{
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
}
```

Este recurso tem os seguintes valores.

>**Observação**&nbsp;&nbsp;Durante a chamada do método para [atualizar um envio de pacote de pré-lançamento](update-a-flight-submission.md), somente os valores *fileName*, *fileStatus*, *minimumDirectXVersion* e *minimumSystemRam* desse objeto são necessários no corpo da solicitação. Os outros valores são preenchidos pelo Centro de Desenvolvimento.

| Valor           | Tipo    | Descrição                                                                                                                                                                                                                          |
|-----------------|---------|------|
| fileName   |   string      |  O nome do pacote.    |  
| fileStatus    | string    |  O status do pacote. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>PendingUpload</li><li>Carregado</li><li>PendingDelete</li></ul>    |  
| id    |  string   |  Uma ID que identifica exclusivamente o pacote. Esse valor é usado pelo Centro de Desenvolvimento.   |     
| version    |  string   |  A versão do pacote do aplicativo. Para obter mais informações, consulte [Numeração de versão do pacote](https://msdn.microsoft.com/windows/uwp/publish/package-version-numbering).   |   
| architecture    |  string   |  A arquitetura do pacote de aplicativo (por exemplo, ARM).   |     
| languages    | array    |  Uma matriz de códigos de idioma para os idiomas com suporte do aplicativo. Para obter mais informações, consulte [Idiomas suportados](https://msdn.microsoft.com/windows/uwp/publish/supported-languages).    |     
| capabilities    |  array   |  Uma matriz de recursos necessários pelo pacote. Para obter mais informações sobre recursos, consulte [Declarações de recursos de aplicativos](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations).   |     
| minimumDirectXVersion    |  string   |  A versão mínima do DirectX que é compatível com o pacote do aplicativo. Isso pode ser definido apenas para aplicativos que visam o Windows 8.x; isso é ignorado para aplicativos destinados a outras versões. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | string    |  A RAM mínima necessária para o pacote do aplicativo. Isso pode ser definido apenas para aplicativos que visam o Windows 8.x; isso é ignorado para aplicativos destinados a outras versões. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>Memory2GB</li></ul>   |    

<span/>

## Enumerações

Esses métodos usam as enumerações a seguir.

<span id="submission-status-code" />
### Código de status do envio

Os códigos a seguir representam o status de um envio.

| Código           |  Descrição      |
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
* [Gerenciar pacotes de pré-lançamento usando a API de envio da Windows Store](manage-flights.md)
* [Obter um envio de pacote de pré-lançamento](get-a-flight-submission.md)
* [Criar um envio de pacote de pré-lançamento](create-a-flight-submission.md)
* [Atualizar um envio de pacote de pré-lançamento](update-a-flight-submission.md)
* [Confirmar um envio de pacote de pré-lançamento](commit-a-flight-submission.md)
* [Excluir um envio de pacote de pré-lançamento](delete-a-flight-submission.md)
* [Obter o status de um envio de pacote de pré-lançamento](get-status-for-a-flight-submission.md)



<!--HONumber=Aug16_HO5-->


