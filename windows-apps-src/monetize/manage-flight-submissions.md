---
author: Xansky
ms.assetid: 2A454057-FF14-40D2-8ED2-CEB5F27E0226
description: Use estes métodos na API de envio da Microsoft Store para gerenciar envios de versão de pré-lançamento do pacote de aplicativos que estão registrados em sua conta do Partner Center.
title: Gerenciar envios de pacote de pré-lançamento
ms.author: mhopkins
ms.date: 04/16/2018
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, envios de versão de pré-lançamento
ms.localizationpriority: medium
ms.openlocfilehash: 5f2a643aa80a59dd64ec1e7b829c02470aaed8bd
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7438861"
---
# <a name="manage-package-flight-submissions"></a>Gerenciar envios de pacote de pré-lançamento

A API de envio da Microsoft Store oferece métodos que é possível usar para gerenciar envios de pacote de pré-lançamento dos aplicativos, inclusive distribuições de pacote graduais. Para obter uma introdução à API de envio da Microsoft Store, inclusive pré-requisitos para usar a API, consulte [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md).

> [!IMPORTANT]
> Se você usar a API de envio da Microsoft Store para criar um envio para um pacote de pré-lançamento, certifique-se de fazer outras alterações no envio somente usando a API, em vez do Partner Center. Se você usar o painel para alterar um envio que criou originalmente usando a API, você não poderá alterar ou confirmar esse envio usando a API. Em alguns casos, o envio pode ficar em um estado de erro em que ele não pode continuar no processo de envio. Se isso ocorrer, você deve excluir o envio e criar um novo.

<span id="methods-for-package-flight-submissions" />

## <a name="methods-for-managing-package-flight-submissions"></a>Métodos para gerenciar envios do pacote de pré-lançamento

Use os métodos a seguir para obter, criar, atualizar, confirmar ou excluir um envio de pacote de pré-lançamento. Antes de usar esses métodos, o pacote de pré-lançamento já deve existir no Partner Center. Você pode criar um pacote de versão de pré-lançamento [no Partner Center](https://msdn.microsoft.com/windows/uwp/publish/package-flights) ou usando os métodos API de envio da Microsoft Store descritos em [Gerenciar pacote de pré-lançamento](manage-flights.md).

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="get-a-flight-submission.md">Obter um envio de pacote de pré-lançamento existente</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-a-flight-submission.md">Obter o status de um envio de pacote de pré-lançamento existente</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions</td>
<td align="left"><a href="create-a-flight-submission.md">Criar um novo envio de pacote de pré-lançamento</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="update-a-flight-submission.md">Atualizar um envio de pacote de pré-lançamento existente</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-a-flight-submission.md">Confirmar um envio de pacote de pré-lançamento novo ou atualizado</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="delete-a-flight-submission.md">Excluir um envio de pacote de pré-lançamento</a></td>
</tr>
</tbody>
</table>

<span id="create-a-package-flight-submission">

## <a name="create-a-package-flight-submission"></a>Criar um envio de pacote de pré-lançamento

Para criar um envio para um pacote de pré-lançamento, siga este processo.

1. Se você ainda não tiver feito isso, conclua os pré-requisitos descritos em [criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md), incluindo associar um aplicativo Azure AD à sua conta do Partner Center e obter a ID e chave do cliente. Você só precisa fazer uma vez. Depois que você tiver a ID e a chave do cliente, poderá reutilizá-las sempre que precisar criar um novo token de acesso do Azure AD.  

2. [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Você deve passar esse token de acesso aos métodos na API de envio da Microsoft Store. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

3. [Criar um envio de pacote de pré-lançamento](create-a-flight-submission.md) executando o seguinte método na API de envio da Microsoft Store. Esse método cria um novo envio em andamento, que é uma cópia de seu último envio publicado.

    ```
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications{applicationId}/flights/{flightId}/submissions
    ```

    O corpo da resposta contém um recurso [envio de versão de pré-lançamento](#flight-submission-object) que inclui a ID do novo envio, o URI da SAS (assinatura de acesso compartilhado) para o upload de todos os pacotes para o envio para o armazenamento de Blobs do Azure e os dados do novo envio (inclusive todas as listagens e informações de preço).

    > [!NOTE]
    > Um URI SAS dá acesso a um recurso seguro no armazenamento do Azure sem exigir chaves de conta. Para obter informações contextuais sobre URIs SAS e o uso com o armazenamento do Blob do Azure, consulte [Assinaturas de acesso compartilhado, parte 1: Noções básicas sobre o modelo SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1) e [Assinaturas de acesso compartilhado, parte 2: Criar e usar uma SAS com o armazenamento de Blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

4. Se você estiver adicionando novos pacotes para o envio, [prepare os pacotes](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements) e adicione-os a um arquivo ZIP.

5. Revise os dados de [envio de versão de pré-lançamento](#flight-submission-object) com as alterações necessárias para o novo envio e execute o método a seguir para [atualizar o envio de pacote de pré-lançamento](update-a-flight-submission.md).

    ```
    PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}
    ```
      > [!NOTE]
      > Se você estiver adicionando novos pacotes para o envio, atualize os dados de envio para fazer referência ao nome e caminho relativo desses arquivos no arquivo ZIP.

4. Se você estiver adicionando novos pacotes para o envio, carregue o arquivo ZIP no [armazenamento do Blob do Azure](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage) usando o URI SAS que foi fornecido no corpo da resposta do método POST chamado anteriormente. Existem bibliotecas do Azure diferentes que é possível usar para fazer isso em uma grande variedade de plataformas, inclusive:

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

5. [Confirme o envio do pacote de pré-lançamento](commit-a-flight-submission.md) realizando o método a seguir. Isso alertará o Partner Center que você terminou seu envio e que suas atualizações agora devem ser aplicadas à sua conta.

    ```
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit
    ```

6. Verifique o status de confirmação executando o método a seguir para [obter o status de envio de pacote de pré-lançamento](get-status-for-a-flight-submission.md).

    ```
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status
    ```

    Para confirmar o status de envio, examine o valor de *status* no corpo da resposta. Esse valor deve mudar de **CommitStarted** para **PreProcessing** se a solicitação for bem-sucedida ou **CommitFailed** se houver erros na solicitação. Se houver erros, o campo *statusDetails* contém mais detalhes sobre o erro.

7. Após a confirmação ser concluída, o envio será enviado para a Loja para inclusão. Você pode continuar a monitorar o progresso do envio usando o método anterior ou visitando o Partner Center.

<span/>

## <a name="code-examples"></a>Exemplos de código

Os artigos a seguir dão exemplos de código detalhados que demonstram como criar um envio de pacote de pré-lançamento em diversas linguagens de programação diferentes:

* [Exemplos de código em C#](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Exemplo de código Java](java-code-examples-for-the-windows-store-submission-api.md)
* [Exemplos de código do Python](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="storebroker-powershell-module"></a>Módulo StoreBroker do PowerShell

Como uma alternativa à chamada direta à API de envio da Microsoft Store, nós também fornecemos um módulo do PowerShell de software livre que implementa uma interface de linha de comando sobre API. Esse módulo é chamado [StoreBroker](https://aka.ms/storebroker). Você pode usar esse módulo para gerenciar seu app, versão de pré-lançamento e envios de complemento na linha de comando em vez de chamar diretamente a API de envio da Microsoft Store, ou você pode simplesmente procurar a fonte para ver mais exemplos de como chamar essa API. O módulo StoreBroker ativamente é usado dentro da Microsoft como a principal forma de muitos apps de terceiros serem enviados para a Store.

Para obter mais informações, consulte nossa [Página do StoreBroker no GitHub](https://aka.ms/storebroker).

<span id="manage-gradual-package-rollout">

## <a name="manage-a-gradual-package-rollout-for-a-package-flight-submission"></a>Gerenciar uma distribuição de pacote gradual para um envio de pacote de pré-lançamento

Você pode distribuir gradualmente os pacotes atualizados em um envio de pacote de pré-lançamento para um percentual de clientes do seu aplicativo no Windows 10. Isso permite que você monitore comentários e dados de análise dos pacotes específicos para verificar se a atualização é necessária antes de implantá-la mais amplamente. Você pode alterar a porcentagem de distribuição (ou parar a atualização) para um envio publicado sem precisar criar um novo envio. Para obter mais detalhes, incluindo instruções sobre como habilitar e gerenciar uma distribuição de pacote gradual no Partner Center, consulte [Este artigo](../publish/gradual-package-rollout.md).

Para habilitar programaticamente uma distribuição de pacote gradual para um envio de pacote de pré-lançamento, siga esse processo usando métodos na API de envio da Microsoft Store:

  1. [Criar um envio de pacote de pré-lançamento](create-a-flight-submission.md) ou [obter um envio de pacote de pré-lançamento](get-a-flight-submission.md).
  2. Nos dados de resposta, localize o recurso [packageRollout](#package-rollout-object), defina o campo *isPackageRollout* como verdadeiro e defina o campo *packageRolloutPercentage* para o percentual de clientes do seu aplicativo que devem receber os pacotes atualizados.
  3. Passe os dados de envio do pacote de pré-lançamento atualizados para o método [atualizar um envio de pacote de pré-lançamento](update-a-flight-submission.md).

Depois que uma distribuição de pacote gradual for habilitada para um envio de pacote de pré-lançamento, será possível usar os métodos a seguir para obter, atualizar, interromper ou finalizar programaticamente a distribuição gradual.

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/packagerollout</td>
<td align="left"><a href="get-package-rollout-info-for-a-flight-submission.md">Obter as informações de distribuição gradual para um envio de pacote de pré-lançamento</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/updatepackagerolloutpercentage</td>
<td align="left"><a href="update-the-package-rollout-percentage-for-a-flight-submission.md">Atualizar a porcentagem de distribuição gradual para um envio de pacote de pré-lançamento</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/haltpackagerollout</td>
<td align="left"><a href="halt-the-package-rollout-for-a-flight-submission.md">Interromper a distribuição gradual para um envio de pacote de pré-lançamento</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/finalizepackagerollout</td>
<td align="left"><a href="finalize-the-package-rollout-for-a-flight-submission.md">Finalizar a distribuição gradual para um envio de pacote de pré-lançamento</a></td>
</tr>
</tbody>
</table>

<span/>

## <a name="data-resources"></a>Recursos de dados

Os métodos da API de envio da Microsoft Store para gerenciar envios do pacote de pré-lançamento usam os recursos de dados JSON a seguir.

<span id="flight-submission-object" />

### <a name="flight-submission-resource"></a>Recurso de envio da versão de pré-lançamento

Esse recurso descreve um envio do pacote de pré-lançamento.

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
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/8b389577-5d5e-4cbe-a744-1ff2e97a9eb8?sv=2014-02-14&sr=b&sig=wgMCQPjPDkuuxNLkeG35rfHaMToebCxBNMPw7WABdXU%3D&se=2016-06-17T21:29:44Z&sp=rwl",
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

Esse recurso tem os valores a seguir.

| Valor      | Tipo   | Descrição              |
|------------|--------|------------------------------|
| id            | string  | A ID do envio.  |
| flightId           | string  |  A ID do pacote de pré-lançamento ao qual o envio está associado.  |  
| status           | string  | O status do envio. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>Cancelado</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publicação</li><li>Publicado</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certificação</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | objeto  |  Um [recurso de detalhes do status](#status-details-object) que contém detalhes adicionais sobre o status do envio, inclusive informações sobre eventuais erros.  |
| flightPackages           | matriz  | Contém [recursos do pacote da versão de pré-lançamento](#flight-package-object) que fornecem detalhes sobre cada pacote no envio.   |
| packageDeliveryOptions    | objeto  | Um [recurso de opções de entrega do pacote](#package-delivery-options-object) que contém configurações da distribuição de pacote gradual e da atualização obrigatória para o envio.   |
| fileUploadUrl           | string  | O URI da assinatura de acesso compartilhado (SAS) para carregar todos os pacotes para o envio. Se você estiver adicionando novos pacotes para o envio, carregue o arquivo ZIP que contém os pacotes para essa URI. Para saber mais, veja [Criar um envio de pacote de pré-lançamento](#create-a-package-flight-submission).  |
| targetPublishMode           | string  | O modo de publicação do envio. Ele pode ter um dos seguintes valores: <ul><li>Imediata</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | A data de publicação do envio em formato ISO 8601, se o *targetPublishMode* estiver definido como SpecificDate.  |
| notesForCertification           | string  |  Fornece informações adicionais para os testadores de certificação, como credenciais da conta de teste e as etapas para acessar e confirmar recursos. Para obter mais informações, consulte [Notas para certificação](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification). |

<span id="status-details-object" />

### <a name="status-details-resource"></a>Recurso de detalhes do status

Esse recurso contém detalhes adicionais sobre o status de um envio. Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição                   |
|-----------------|---------|------|
|  errors               |    objeto     |   Uma matriz de [recursos de detalhes do status](#status-detail-object) que contêm detalhes do erro para o envio.   |     
|  warnings               |   objeto      | Uma matriz de [recursos de detalhes do status](#status-detail-object) que contêm detalhes do aviso para o envio.     |
|  certificationReports               |     objeto    |   Uma matriz de [recursos do relatório de certificação](#certification-report-object) que dão acesso aos dados do relatório de certificação para o envio. Será possível examinar esses relatórios para obter mais informações, se a certificação falhar.    |  


<span id="status-detail-object" />

### <a name="status-detail-resource"></a>Recurso de detalhes do status

Esse recurso contém informações adicionais sobre todos os erros ou avisos relacionados a um envio. Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição       |
|-----------------|---------|------|
|  code               |    cadeia de caracteres     |   Um [código de status do envio](#submission-status-code) que descreve o tipo de erro ou aviso. |  
|  details               |     cadeia de caracteres    |  Uma mensagem com mais detalhes sobre o problema.     |


<span id="certification-report-object" />

### <a name="certification-report-resource"></a>Recurso do relatório de certificação

Esse recurso dá acesso aos dados do relatório de certificação para um envio. Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição         |
|-----------------|---------|------|
|     date            |    string     |  A data e hora que o relatório foi gerado, no formato ISO 8601.    |
|     reportUrl            |    string     |  A URL na qual é possível acessar o relatório.    |


<span id="flight-package-object" />

### <a name="flight-package-resource"></a>Recurso do pacote da versão de pré-lançamento

Esse recurso fornece detalhes sobre um pacote em um envio.

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

Esse recurso tem os valores a seguir.

> [!NOTE]
> Durante a chamada do método [atualizar um envio de pacote de pré-lançamento](update-a-flight-submission.md) somente os valores *fileName*, *fileStatus*, *minimumDirectXVersion*, e *minimumSystemRam* desse objeto são necessários no corpo da solicitação. Os outros valores são preenchidos pelo Partner Center.

| Valor           | Tipo    | Descrição              |
|-----------------|---------|------|
| fileName   |   string      |  O nome do pacote.    |  
| fileStatus    | string    |  O status do pacote. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>PendingUpload</li><li>Carregado</li><li>PendingDelete</li></ul>    |  
| id    |  string   |  Uma ID que identifica exclusivamente o pacote. Esse valor é usado por Partner Center.   |     
| version    |  string   |  A versão do pacote do aplicativo. Para obter mais informações, consulte [Numeração de versão do pacote](https://msdn.microsoft.com/windows/uwp/publish/package-version-numbering).   |   
| architecture    |  string   |  A arquitetura do pacote de aplicativo (por exemplo, ARM).   |     
| languages    | array    |  Uma matriz de códigos de idioma para os idiomas com suporte do aplicativo. Para obter mais informações, consulte [Idiomas suportados](https://msdn.microsoft.com/windows/uwp/publish/supported-languages).    |     
| capabilities    |  array   |  Uma matriz de recursos necessários pelo pacote. Para obter mais informações sobre recursos, consulte [Declarações de recursos de aplicativos](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations).   |     
| minimumDirectXVersion    |  string   |  A versão mínima do DirectX que é compatível com o pacote do aplicativo. Isso pode ser definido apenas para aplicativos que visam o Windows 8.x; isso é ignorado para aplicativos destinados a outras versões. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | string    |  A RAM mínima necessária para o pacote do aplicativo. Isso pode ser definido apenas para aplicativos que visam o Windows 8.x; isso é ignorado para aplicativos destinados a outras versões. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>Memory2GB</li></ul>   |    


<span id="package-delivery-options-object" />

### <a name="package-delivery-options-resource"></a>Recurso de opções de entrega do pacote

Esse recurso contém configurações da distribuição de pacote gradual e da atualização obrigatória para o envio.

```json
{
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
}
```

Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição        |
|-----------------|---------|------|
| packageRollout   |   objeto      |   Um [recurso de distribuição de pacote](#package-rollout-object) que contém configurações da distribuição de pacote gradual para o envio.    |  
| isMandatoryUpdate    | booliano    |  Indica se você deseja tratar os pacotes nesse envio como obrigatórios para instalar automaticamente atualizações de aplicativo. Para obter mais informações sobre pacotes obrigatórios para instalar automaticamente as atualizações de aplicativos, consulte [Baixar e instalar atualizações de pacote para seu app](../packaging/self-install-package-updates.md).    |  
| mandatoryUpdateEffectiveDate    |  date   |  A data e hora em que os pacotes nesse envio se tornam obrigatórios, em formato ISO 8601 e fuso horário UTC.   |        

<span id="package-rollout-object" />

### <a name="package-rollout-resource"></a>Recurso de distribuição de pacote

Esse recurso contém [configurações de distribuição de pacote](#manage-gradual-package-rollout) gradual para o envio. Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição        |
|-----------------|---------|------|
| isPackageRollout   |   booliano      |  Indica se a distribuição de pacote gradual está habilitada para o envio.    |  
| packageRolloutPercentage    | flutuante    |  O percentual de usuários que receberão os pacotes na distribuição gradual.    |  
| packageRolloutStatus    |  string   |  Uma das seguintes sequências que indicam o status da distribuição de pacote gradual: <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  string   |  A ID da submissão que será recebido por clientes que não recebem os pacotes de lançamento gradual.   |          

> [!NOTE]
> Os valores *packageRolloutStatus* e *fallbackSubmissionId* são atribuídos pelo Centro de parceiros e não devem ser definidos pelo desenvolvedor. Se você incluir esses valores no corpo da solicitação, esses valores serão ignorados.

<span/>

## <a name="enums"></a>Enumerações

Esses métodos usam as enumerações a seguir.

<span id="submission-status-code" />

### <a name="submission-status-code"></a>Código de status do envio

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

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gerenciar pacotes de pré-lançamento usando a API de envio da Microsoft Store](manage-flights.md)
* [Obter um envio de pacote de pré-lançamento](get-a-flight-submission.md)
* [Criar um envio de pacote de pré-lançamento](create-a-flight-submission.md)
* [Atualizar um envio de pacote de pré-lançamento](update-a-flight-submission.md)
* [Confirmar um envio de pacote de pré-lançamento](commit-a-flight-submission.md)
* [Excluir um envio de pacote de pré-lançamento](delete-a-flight-submission.md)
* [Obter o status de um envio de pacote de pré-lançamento](get-status-for-a-flight-submission.md)
