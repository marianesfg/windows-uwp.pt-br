---
author: mcleanbyron
ms.assetid: C7428551-4B31-4259-93CD-EE229007C4B8
description: "Use estes métodos na API de envio da Windows Store para gerenciar envios dos apps que estão registrados em sua conta do Centro de Desenvolvimento do Windows."
title: Gerenciar envios de app usando a API de envio da Windows Store
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, API de envios da Windows Store, envios de app
translationtype: Human Translation
ms.sourcegitcommit: e5d9d3e08aaae7e349f7aaf23f6683e2ce9a4f88
ms.openlocfilehash: 21a421b057a55120865c01cc3dffb80318ab38ed
ms.lasthandoff: 02/08/2017

---

# <a name="manage-app-submissions-using-the-windows-store-submission-api"></a>Gerenciar envios de app usando a API de envio da Windows Store

A API de envio da Windows Store oferece métodos que é possível usar para gerenciar envios dos apps, inclusive distribuições de pacote graduais. Para obter uma introdução à API de envio da Windows Store, inclusive pré-requisitos para usar a API, consulte [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md).

>**Observação**&nbsp;&nbsp;Estes métodos podem ser usados somente para contas do Centro de Desenvolvimento do Windows com permissão para usar a API de envio da Windows Store. Essa permissão está sendo habilitada para contas de desenvolvedor em estágios, e nem todas as contas têm essa permissão habilitado no momento. Para solicitar acesso anterior, fazer logon no painel do Centro de Desenvolvimento, clique em **Comentários** na parte inferior do painel, selecione **API de envio** para a área de comentários e envie sua solicitação. Você receberá um email quando essa permissão for habilitada em sua conta.

>**Importante**&nbsp;&nbsp;Se você usar a API de envio da Windows Store a fim de criar um envio para um app, certifique-se de fazer outras alterações no envio somente usando a API, em vez do painel do Centro de Desenvolvimento. Se você usar o painel para alterar um envio criado originalmente usando a API, não será possível alterar ou confirmar esse envio com a API. Em alguns casos, o envio pode ficar em um estado de erro, no qual não é possível continuar o processo de envio. Se isso ocorrer, você deve excluir o envio e criar um novo.

<span id="methods-for-app-submissions" />
## <a name="methods-for-managing-app-submissions"></a>Métodos para gerenciar envios de apps

Use os métodos a seguir para obter, criar, atualizar, confirmar ou excluir um envio de app. Antes de usar esses métodos, o app já deve existir na sua conta do Centro de Desenvolvimento e você deve primeiro criar um envio para o app no painel. Para obter mais informações, consulte os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites).

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
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}```</td>
<td align="left">[Obter um envio de app existente](get-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status```</td>
<td align="left">[Obter o status de um envio de app existente](get-status-for-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions```</td>
<td align="left">[Criar um novo envio de app](create-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}```</td>
<td align="left">[Atualizar um envio de app existente](update-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit```</td>
<td align="left">[Confirmar um envio de app novo ou atualizado](commit-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}```</td>
<td align="left">[Excluir um envio de app](delete-an-app-submission.md)</td>
</tr>
</tbody>
</table>

<span id="create-an-app-submission">
### Criar um envio de app

Para criar um envio de um app, siga este processo.

1. Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Windows Store.

  >**Observação**&nbsp;&nbsp;Verifique se o app já tem pelo menos um envio concluído com as informações de [classificações etárias](https://msdn.microsoft.com/windows/uwp/publish/age-ratings) preenchidas.

2. [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Você deve passar esse token de acesso aos métodos na API de envio da Windows Store. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

3. [Criar um envio de app](create-an-app-submission.md) executando o seguinte método na API de envio da Windows Store. Esse método cria um novo envio em andamento, que é uma cópia de seu último envio publicado.

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions
  ```

  O corpo da resposta contém três itens: a ID do novo envio, os dados do novo envio (inclusive todas as listagens e informações de preço) e o URI da Assinatura de Acesso Compartilhado (SAS) para carregar todos os pacotes de apps e imagens de lista para o envio ao armazenamento do Blob do Azure.

  >**Observação**&nbsp;&nbsp;Um URI SAS dá acesso a um recurso seguro no armazenamento do Azure sem exigir chaves de conta. Para obter informações contextuais sobre URIs SAS e o uso com o armazenamento do Blob do Azure, consulte [Assinaturas de acesso compartilhado, parte 1: Noções básicas sobre o modelo SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1) e [Assinaturas de acesso compartilhado, parte 2: Criar e usar uma SAS com o armazenamento de Blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

4. Se você estiver adicionando novos pacotes ou imagens para o envio, [prepare os pacotes de apps](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements) e [prepare as imagens e as capturas de tela do app](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images). Adicione todos esses arquivos a um arquivo ZIP.

5. Revise os dados de envio com as alterações necessárias para o novo envio e execute o método a seguir para [atualizar o envio de app](update-an-app-submission.md).

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}
  ```

  <span/>
  >**Observação**&nbsp;&nbsp;Se você estiver adicionando novos pacotes ou imagens para o envio, atualize os dados de envio para fazer referência ao nome e caminho relativo desses arquivos no arquivo ZIP.

4. Se você estiver adicionando novos pacotes ou imagens para o envio, carregue o arquivo ZIP no [armazenamento do Blob do Azure](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage) usando o URI SAS que foi fornecido no corpo da resposta do método POST chamado anteriormente. Existem bibliotecas do Azure diferentes que é possível usar para fazer isso em uma grande variedade de plataformas, inclusive:

  * [Biblioteca de Cliente do Armazenamento do Azure para .NET](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-blobs)
  * [SDK de Armazenamento do Azure para Java](https://docs.microsoft.com/azure/storage/storage-java-how-to-use-blob-storage)
  * [SDK de Armazenamento do Azure para Python](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)

  <span/>

  O exemplo de código em C# a seguir demonstra como carregar um arquivo ZIP no armazenamento do Blob do Azure usando a classe [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx) na Biblioteca de Cliente do Armazenamento do Azure para .NET. Este exemplo pressupõe que o arquivo ZIP já tenha sido escrito para um objeto de fluxo.

  > [!div class="tabbedCodeSnippets"]
  ```csharp
  string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
  Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
  await blockBob.UploadFromStreamAsync(stream);
  ```

5. [Confirme o envio de app](commit-an-app-submission.md) executando o método a seguir. Isso alertará o Centro de Desenvolvimento que você terminou seu envio e que suas atualizações agora devem ser aplicadas à sua conta.

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit
  ```

6. Verifique o status de confirmação executando o método a seguir para [obter o status de envio de app](get-status-for-an-app-submission.md).

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status
  ```

  Para confirmar o status de envio, examine o valor de *status* no corpo da resposta. Esse valor deve mudar de **CommitStarted** para **PreProcessing** se a solicitação for bem-sucedida ou **CommitFailed** se houver erros na solicitação. Se houver erros, o campo *statusDetails* contém mais detalhes sobre o erro.

7. Após a confirmação ser concluída, o envio será enviado para a Loja para inclusão. Você pode continuar monitorando o progresso do envio usando o método anterior ou visitando o painel do Centro de Desenvolvimento.

<span/>
### <a name="code-examples-for-managing-app-submissions"></a>Exemplos de código para gerenciar envios de app

Os artigos a seguir apresentam exemplos detalhados de código que demonstram como criar um envio de app em diversas linguagens de programação diferentes:

* [Exemplos de código em C#](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Exemplos de código em Java](java-code-examples-for-the-windows-store-submission-api.md)
* [Exemplos de código em Python](python-code-examples-for-the-windows-store-submission-api.md)

>**Observação**&nbsp;&nbsp;Além dos exemplos de código listados acima, também fornecemos um módulo de PowerShell de código-fonte aberto que implementa uma interface de linha de comando sobre a API de envio da Windows Store. Esse módulo é chamado de [StoreBroker](https://aka.ms/storebroker). Você pode usar esse módulo para gerenciar seu app, a versão de pré-lançamento e os envios de complemento na linha de comando em vez de chamar diretamente o envio pela API da Windows Store, ou você pode procurar a fonte para ver mais exemplos de como chamá-la. O módulo StoreBroker é usado ativamente na Microsoft como o modo principal de enviar diversos apps primários para a loja. Para obter mais informações, consulte nossa [Página do StoreBroker no GitHub](https://aka.ms/storebroker).

<span id="manage-gradual-package-rollout">
## <a name="methods-for-managing-a-gradual-package-rollout"></a>Métodos para gerenciar uma distribuição de pacote gradual

É possível distribuir gradualmente os pacotes atualizados em um envio de app para um percentual de clientes do app no Windows 10. Isso permite que você monitore comentários e dados de análise dos pacotes específicos para verificar se a atualização é necessária antes de implantá-la mais amplamente. Você pode alterar a porcentagem de distribuição (ou parar a atualização) para um envio publicado sem precisar criar um novo envio. Para obter mais detalhes, inclusive instruções sobre como habilitar e gerenciar uma distribuição de pacote gradual no painel do Centro de Desenvolvimento, consulte [este artigo](../publish/gradual-package-rollout.md).

Para habilitar programaticamente uma distribuição de pacote gradual para um envio de app, siga esse processo usando métodos na API de envio da Windows Store:

  1. [Crie um envio de app](create-an-app-submission.md) ou [obtenha um envio de app existente](get-an-app-submission.md).
  2. Nos dados de resposta, localize o recurso [packageRollout](#package-rollout-object), defina o campo *isPackageRollout* como **true** e o campo *packageRolloutPercentage* como o percentual de clientes do app que devem receber os pacotes atualizados.
  3. Passe os dados de envio de app atualizados para o método [atualizar um envio de app](update-an-app-submission.md).

Depois que uma distribuição de pacote gradual for habilitada para um envio de app, será possível usar os métodos a seguir para obter, atualizar, interromper ou finalizar programaticamente a distribuição gradual.

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
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/packagerollout```</td>
<td align="left">[Obter as informações de distribuição gradual para um envio de app](get-package-rollout-info-for-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/updatepackagerolloutpercentage```</td>
<td align="left">[Atualizar o percentual de distribuição gradual para um envio de app](update-the-package-rollout-percentage-for-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout```</td>
<td align="left">[Interromper a distribuição gradual para um envio de app](halt-the-package-rollout-for-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/finalizepackagerollout```</td>
<td align="left">[Finalizar a distribuição gradual para um envio de app](finalize-the-package-rollout-for-an-app-submission.md)</td>
</tr>
</tbody>
</table>

<span/>
## Recursos de dados Os métodos da API de envio da Windows Store para gerenciar envios do pacote de app usam os recursos de dados JSON a seguir.

<span id="app-submission-object" />
### Recurso de envio de app

Esse recurso descreve um envio de app.

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2",
    "isAdvancedPricingModel": "true"
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "ApiTestApp For Devbox"
      },
      "platformOverrides": {}
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
  "hasExternalInAppProducts": false,
  "meetAccessibilityGuidelines": true,
  "notesForCertification": "",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/387a9ea8-a412-43a9-8fb3-a38d03eb483d?sv=2014-02-14&sr=b&sig=sdd12JmoaT6BhvC%2BZUrwRweA%2Fkvj%2BEBCY09C2SZZowg%3D&se=2016-06-17T18:32:26Z&sp=rwl",
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "Uploaded",
      "id": "1152921504620138797",
      "version": "1.0.0.0",
      "architecture": "ARM",
      "languages": [
        "en-US"
      ],
      "capabilities": [
        "ID_RESOLUTION_HD720P",
        "ID_RESOLUTION_WVGA",
        "ID_RESOLUTION_WXGA"
      ],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None",
      "targetDeviceFamilies": [
        "Windows.Mobile min version 10.0.10240.0"
      ]
    }
  ],
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "friendlyName": "Submission 2"
}
```

Esse recurso tem os valores a seguir.

| Valor      | Tipo   | Descrição      |
|------------|--------|-------------------|
| id            | string  | A ID do envio.  |
| applicationCategory           | string  |   Uma cadeia de caracteres que especifica a [categoria e/ou subcategoria](https://msdn.microsoft.com/windows/uwp/publish/category-and-subcategory-table) para o app. Categorias e subcategorias são combinadas em uma única cadeia de caracteres com o caractere de sublinhado '_', como **BooksAndReference_EReader**.      |  
| pricing           |  objeto  | Um [recurso de preço](#pricing-object) que contém informações de preço para o app.        |   
| visibilidade           |  cadeia de caracteres  |  A visibilidade do app. Isso pode ter um dos seguintes valores: <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>       |   
| targetPublishMode           | string  | O modo de publicação do envio. Ele pode ter um dos seguintes valores: <ul><li>Imediata</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | A data de publicação do envio em formato ISO 8601, se o *targetPublishMode* estiver definido como SpecificDate.  |  
| listings           |   objeto  |  Um dicionário de pares de chave e valor, em que cada chave é um código de país e cada valor é um [recurso de listagem](#listing-object) que contém informações de listagem do app.       |   
| hardwarePreferences           |  array  |   Uma matriz de cadeias de caracteres que definem as [preferências de hardware](https://msdn.microsoft.com/windows/uwp/publish/enter-app-properties#hardware_preferences) do app. Ele pode ter um dos seguintes valores: <ul><li>Touch</li><li>Teclado</li><li>Mouse</li><li>Câmera</li><li>NfcHce</li><li>Nfc</li><li>BluetoothLE</li><li>Telefonia</li></ul>     |   
| automaticBackupEnabled           |  booliano  |   Indica se o Windows pode incluir dados do app em backups automáticos no OneDrive. Para obter mais informações, consulte [Declarações de app](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).   |   
| canInstallOnRemovableMedia           |  booliano  |   Indica se os clientes podem instalar o app em armazenamento removível. Para obter mais informações, consulte [Declarações de app](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).     |   
| isGameDvrEnabled           |  booliano |   Indica se o DVR de jogos está habilitado para o app.    |   
| hasExternalInAppProducts           |     booliano          |   Indica se o app permite que os usuários façam compras fora do sistema de comércio da Windows Store. Para obter mais informações, consulte [Declarações de app](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).     |   
| meetAccessibilityGuidelines           |    booliano           |  Indica se o app foi testado para atender às diretrizes de acessibilidade. Para obter mais informações, consulte [Declarações de app](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).      |   
| notesForCertification           |  string  |   Contém [observações de certificação](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification) do app.    |    
| status           |   string  |  O status do envio. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>Cancelado</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publicação</li><li>Publicado</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certificação</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>      |    
| statusDetails           |   objeto  | Um [recurso de detalhes do status](#status-details-object) que contém detalhes adicionais sobre o status do envio, inclusive informações sobre eventuais erros.       |    
| fileUploadUrl           |   cadeia de caracteres  | O URI da assinatura de acesso compartilhado (SAS) para carregar todos os pacotes para o envio. Se você estiver adicionando novos pacotes ou imagens para o envio, carregue o arquivo ZIP que contém os pacotes e imagens para essa URI. Para obter mais informações, consulte [Criar um envio de app](#create-an-app-submission).       |    
| applicationPackages           |   matriz  | Uma matriz de [recursos do pacote de apps](#application-package-object) que dão detalhes sobre cada pacote no envio. |    
| packageDeliveryOptions    | objeto  | Um [recurso de opções de entrega do pacote](#package-delivery-options-object) que contém configurações da distribuição de pacote gradual e da atualização obrigatória para o envio.  |
| enterpriseLicensing           |  cadeia de caracteres  |  Um dos [valores de licenciamento empresarial](#enterprise-licensing) que indicam o comportamento de licenciamento empresarial para o app.  |    
| allowMicrosftDecideAppAvailabilityToFutureDeviceFamilies           |  booliano   |  Indica se a Microsoft tem permissão para [disponibilizar o app para as futuras famílias de dispositivos Windows 10](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families).    |    
| allowTargetFutureDeviceFamilies           | object   |  Um dicionário de pares de chave e valor, onde cada chave é uma [família de dispositivos Windows 10](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families) e cada valor é um valor booliano que indica se seu app tem permissão para segmentar a família de dispositivos especificadas.     |    
| friendlyName           |   cadeia  |  O nome amigável do envio, conforme mostrado no painel do Centro de Desenvolvimento. Esse valor é gerado para você ao criar o envio.       |  


<span id="listing-object" />
### <a name="listing-resource"></a>Recurso de listagem

Esse recurso contém informações de listagem de um app. Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição                  |
|-----------------|---------|------|
|  baseListing               |   object      |  As informações de [listagem básica](#base-listing-object) do app, que definem o padrão de informações de listagem para todas as plataformas.   |     
|  platformOverrides               | object |   Um dicionário de pares de chave e valor, em que cada chave é a cadeia de caracteres que identifica uma plataforma cujas informações de listagem devem ser substituídas e cada valor é um recurso de [listagem básica](#base-listing-object) (contendo apenas os valores da descrição ao título) que especifica as informações de listagem a serem substituídas para a plataforma especificada. As chaves podem ter os seguintes valores: <ul><li>Unknown</li><li>Windows80</li><li>Windows81</li><li>WindowsPhone71</li><li>WindowsPhone80</li><li>WindowsPhone81</li></ul>     |      |     

<span id="base-listing-object" />
### <a name="base-listing-resource"></a>Recurso de listagem base

Esse recurso contém informações de listagem base de um app. Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição       |
|-----------------|---------|------|
|  copyrightAndTrademarkInfo                |   string      |  Informações opcionais de [direitos autorais e/ou marca comercial](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#copyright-and-trademark-info).  |
|  keywords                |  array       |  Uma matriz de [palavra-chave](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#keywords) para ajudar seu app a aparecer nos resultados de pesquisa.    |
|  licenseTerms                |    string     | Os [termos de licença](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#additional-license-terms) opcionais do seu app.     |
|  privacyPolicy                |   string      |   A URL para a [política de privacidade](../publish/create-app-store-listings.md#privacy-policy) do seu app.    |
|  supportContact                |   string      |  A URL ou endereço de email para as [informações de contato de suporte](../publish/create-app-store-listings.md#support-contact-info) do seu app.     |
|  websiteUrl                |   string      |  A URL da [página da Web](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#website) do seu app.    |    
|  description               |    string     |   A [descrição](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#description) dos detalhes do app.   |     
|  features               |    array     |  Uma matriz de até 20 cadeias de caracteres que lista os [recursos](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#app-features) do seu app.     |
|  releaseNotes               |  string       |  As [notas de versão](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#release-notes) do app.    |
|  images               |   matriz      |  Uma matriz de recursos de [imagem e ícone](#image-object) para a listagem do app.  |
|  recommendedHardware               |   matriz      |  Uma matriz de até 11 cadeias de caracteres que lista as [configurações de hardware recomendadas](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#recommended-hardware) para o app.     |
|  title               |     cadeia de caracteres    |   O título da listagem do app.   |  

<span id="image-object" />
### <a name="image-resource"></a>Recurso de imagem

Esse recurso contém dados de imagem e ícone para uma listagem do app. Para obter mais informações sobre imagens e ícones para detalhes, consulte [Capturas de tela e imagens do app](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images). Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição           |
|-----------------|---------|------|
|  fileName               |    string     |   O nome do arquivo de imagem no arquivo ZIP que você carregou para o envio.    |     
|  fileStatus               |   string      |  O status do arquivo de imagem. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>PendingUpload</li><li>Carregado</li><li>PendingDelete</li></ul>   |
|  id  |  string  | A ID da imagem, conforme especificado pelo Centro de Desenvolvimento.  |
|  description  |  string  | A descrição da imagem.  |
|  imageType  |  string  | Uma das seguintes cadeias de caracteres que indica o tipo da imagem: <ul><li>Unknown</li><li>Screenshot</li><li>PromotionalArtwork414X180</li><li>PromotionalArtwork846X468</li><li>PromotionalArtwork558X756</li><li>PromotionalArtwork414X468</li><li>PromotionalArtwork558X558</li><li>PromotionalArtwork2400X1200</li><li>Ícone</li><li>WideIcon358X173</li><li>BackgroundImage1000X800</li><li>SquareIcon358X358</li><li>MobileScreenshot</li><li>XboxScreenshot</li><li>SurfaceHubScreenshot</li><li>HoloLensScreenshot</li></ul>      |


<span id="pricing-object" />
### <a name="pricing-resource"></a>Recurso de preço

Esse recurso contém informações de preço do app. Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição        |
|-----------------|---------|------|
|  trialPeriod               |    string     |  Uma cadeia de caracteres que especifica o período de avaliação do app. Ele pode ter um dos seguintes valores: <ul><li>NoFreeTrial</li><li>OneDay</li><li>TrialNeverExpires</li><li>SevenDays</li><li>FifteenDays</li><li>ThirtyDays</li></ul>    |
|  marketSpecificPricings               |    object     |  Um dicionário de pares de chave e valor, onde cada chave é um código ISO 3166-1 alpha-2 de duas letras do país e cada valor é uma [faixa de preço](#price-tiers). Esses itens representam os [preços personalizados do app em mercados específicos](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices). Todos os itens nesse dicionário substituem o preço base especificado pelo valor *priceId* para o mercado especificado.      |     
|  sales               |   matriz      |  **Preterido**. Uma matriz de [recursos de venda](#sale-object) que contêm informações de venda do app.   |     
|  priceId               |   cadeia      |  A [faixa de preço](#price-tiers) que especifica o [preço base](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#base-price) do app.   |     
|  isAdvancedPricingModel               |   booliano      |  Se for **true**, sua conta de desenvolvedor tem acesso ao conjunto expandido de faixas de preço de US$ 0,99 a US$ 1999,99. Se for **true**, sua conta de desenvolvedor tem acesso ao conjunto original de faixas de preço de US$ 0,99 a US$ 999,99. Para saber mais sobre as diferentes camadas, consulte [faixas de preço](#price-tiers).<br/><br/>**Observação**&nbsp;&nbsp;Esse campo é somente leitura.   |


<span id="sale-object" />
### <a name="sale-resource"></a>Recurso de venda

Esse recurso contém informações de venda de um app.

>**Importante**&nbsp;&nbsp;O recurso **Promoção** não tem mais suporte, e atualmente você não pode acessar nem modificar os dados de venda de um envio de app usando a API de envio da Windows Store:

   > * Depois de chamar o [método GET para obter um envio de app](get-an-app-submission.md), o valor de *sales* estará vazio. Você pode continuar a usar o painel do Centro de Desenvolvimento para obter os dados de venda de seu envio de app.
   > * Ao chamar o [método PUT para atualizar um envio de app](update-an-app-submission.md), as informações no valor de *sales* serão ignoradas. Você pode continuar a usar o painel do Centro de Desenvolvimento para modificar os dados de venda de seu envio de app.

> No futuro, atualizaremos a API de envio da Windows Store para apresentar uma nova maneira de acessar programaticamente as informações de vendas para envios de app.

Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição    |
|-----------------|---------|------|
|  name               |    string     |   O nome da promoção.    |     
|  basePriceId               |   string      |  A [faixa de preço](#price-tiers) a ser usada para o preço base da promoção.    |     
|  startDate               |   string      |   A data de início da promoção no formato ISO 8601.  |     
|  endDate               |   string      |  A data de término da promoção no formato ISO 8601.      |     
|  marketSpecificPricings               |   object      |   Um dicionário de pares de chave e valor, onde cada chave é um código ISO 3166-1 alpha-2 de duas letras do país e cada valor é uma [faixa de preço](#price-tiers). Esses itens representam os [preços personalizados do app em mercados específicos](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices). Todos os itens nesse dicionário substituem o preço base especificado pelo valor *basePriceId* para o mercado especificado.    |


<span id="status-details-object" />
### <a name="status-details-resource"></a>Recurso de detalhes do status

Esse recurso contém detalhes adicionais sobre o status de um envio. Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição         |
|-----------------|---------|------|
|  errors               |    objeto     |   Uma matriz de [recursos de detalhes do status](#status-detail-object) que contêm detalhes do erro para o envio.    |     
|  warnings               |   objeto      | Uma matriz de [recursos de detalhes do status](#status-detail-object) que contêm detalhes do aviso para o envio.      |
|  certificationReports               |     objeto    |   Uma matriz de [recursos do relatório de certificação](#certification-report-object) que dão acesso aos dados do relatório de certificação para o envio. Será possível examinar esses relatórios para obter mais informações, se a certificação falhar.   |  


<span id="status-detail-object" />
### <a name="status-detail-resource"></a>Recurso de detalhes do status

Esse recurso contém informações adicionais sobre todos os erros ou avisos relacionados a um envio. Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição        |
|-----------------|---------|------|
|  code               |    cadeia de caracteres     |   Um [código de status do envio](#submission-status-code) que descreve o tipo de erro ou aviso.   |     
|  details               |     cadeia de caracteres    |  Uma mensagem com mais detalhes sobre o problema.     |


<span id="application-package-object" />
### <a name="application-package-resource"></a>Recurso do pacote de apps

Esse recurso contém detalhes sobre um pacote de apps para o envio.

```json
{
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "Uploaded",
      "id": "1152921504620138797",
      "version": "1.0.0.0",
      "architecture": "ARM",
      "languages": [
        "en-US"
      ],
      "capabilities": [
        "ID_RESOLUTION_HD720P",
        "ID_RESOLUTION_WVGA",
        "ID_RESOLUTION_WXGA"
      ],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None",
      "targetDeviceFamilies": [
        "Windows.Mobile min version 10.0.10240.0"
      ]
    }
  ],
}
```

Esse recurso tem os valores a seguir.  

>**Observação**&nbsp;&nbsp;Durante a chamada do método [atualizar um envio de app](update-an-app-submission.md), apenas os valores *fileName*, *fileStatus*, *minimumDirectXVersion* e *minimumSystemRam* desse objeto são necessários no corpo da solicitação. Os outros valores são preenchidos pelo Centro de Desenvolvimento.

| Valor           | Tipo    | Descrição                   |
|-----------------|---------|------|
| fileName   |   string      |  O nome do pacote.    |  
| fileStatus    | string    |  O status do pacote. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>PendingUpload</li><li>Carregado</li><li>PendingDelete</li></ul>    |  
| id    |  string   |  Uma ID que identifica exclusivamente o pacote. Esse valor é usado pelo Centro de Desenvolvimento.   |     
| version    |  string   |  A versão do pacote do app. Para obter mais informações, consulte [Numeração de versão do pacote](https://msdn.microsoft.com/windows/uwp/publish/package-version-numbering).   |   
| architecture    |  string   |  A arquitetura do pacote (por exemplo, ARM).   |     
| languages    | array    |  Uma matriz de códigos de idioma para os idiomas com suporte do app. Para obter mais informações, consulte [Idiomas suportados](https://msdn.microsoft.com/windows/uwp/publish/supported-languages).    |     
| capabilities    |  array   |  Uma matriz de recursos necessários pelo pacote. Para obter mais informações sobre recursos, consulte [Declarações de recursos de apps](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations).   |     
| minimumDirectXVersion    |  string   |  A versão mínima do DirectX que é compatível com o pacote do app. Isso pode ser definido apenas para apps que visam o Windows 8.x; isso é ignorado para apps destinados a outras versões. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | string    |  A RAM mínima necessária para o pacote do app. Isso pode ser definido apenas para apps que visam o Windows 8.x; isso é ignorado para apps destinados a outras versões. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>Memory2GB</li></ul>   |       
| targetDeviceFamilies    | array    |  Uma matriz de cadeias de caracteres que representam as famílias de dispositivos que o pacote segmenta. Esse valor é usado somente para pacotes destinados ao Windows 10; para pacotes destinados a versões anteriores, esse valor tem o valor **Nenhum**. As seguintes strings de família de dispositivos são atualmente suportadas para pacotes do Windows 10, onde *{0}* é uma string de versão do Windows 10, como 10.0.10240.0, 10.0.10586.0 ou 10.0.14393.0: <ul><li>Versão mínima do Windows.Universal *{0}*</li><li>Versão mínima do Windows.Desktop *{0}*</li><li>Versão mínima do Windows.Mobile *{0}*</li><li>Versão mínima do Windows.Xbox *{0}*</li><li>Versão mínima de Windows.Holographic *{0}*</li></ul>   |    

<span/>

<span id="certification-report-object" />
### <a name="certification-report-resource"></a>Recurso do relatório de certificação

Esse recurso dá acesso aos dados do relatório de certificação para um envio. Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição             |
|-----------------|---------|------|
|     date            |    string     |  A data e hora em que o relatório foi gerado, no formato ISO 8601.    |
|     reportUrl            |    string     |  A URL na qual é possível acessar o relatório.    |


<span id="package-delivery-options-object" />
### <a name="package-delivery-options-resource"></a>Recurso de opções de entrega do pacote

Esse recurso contém configurações da distribuição de pacote gradual e da atualização obrigatória para o envio.

```json
{
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0,
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
| packageRollout   |   objeto      |  Um [recurso de distribuição de pacote](#package-rollout-object) que contém configurações da distribuição de pacote gradual para o envio.   |  
| isMandatoryUpdate    | booliano    |  Indica se você deseja tratar os pacotes nesse envio como obrigatórios para instalar automaticamente atualizações de app. Para obter mais informações sobre pacotes obrigatórios para instalar automaticamente as atualizações de apps, consulte [Baixar e instalar atualizações de pacote para seu app](../packaging/self-install-package-updates.md).    |  
| mandatoryUpdateEffectiveDate    |  date   |  A data e hora em que os pacotes nesse envio se tornam obrigatórios, em formato ISO 8601 e fuso horário UTC.   |        

<span id="package-rollout-object" />
### <a name="package-rollout-resource"></a>Recurso de distribuição de pacote

Esse recurso contém [configurações de distribuição de pacote](#manage-gradual-package-rollout) gradual para o envio. Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição        |
|-----------------|---------|------|
| isPackageRollout   |   booliano      |  Indica se a distribuição de pacote gradual está habilitada para o envio.    |  
| packageRolloutPercentage    | flutuante    |  O percentual de usuários que receberão os pacotes na distribuição gradual.    |  
| packageRolloutStatus    |  string   |  Uma das seguintes cadeias que indicam o status da distribuição de pacote gradual: <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  string   |  A ID do envio que será recebida por clientes que não recebem os pacotes de distribuição gradual.   |          

<span/>

## <a name="enums"></a>Enumerações

Esses métodos usam as enumerações a seguir.

<span id="price-tiers" />
### <a name="price-tiers"></a>Faixas de preço

Os seguintes valores representam as faixas de preço disponíveis no recurso [preço do recurso](#pricing-object) para um envio de app.

| Valor           | Descrição        |
|-----------------|------|
|  Base               |   A faixa de preço não está definida. Use o preço base para o app.      |     
|  NotAvailable              |   O app não está disponível na região especificada.    |     
|  Grátis              |   O app é gratuito.    |    
|  Faixa de*xxxx*               |   Uma cadeia de caracteres que especifica a faixa de preço do app, no formato **Faixa<em>xxxx</em>**. No momento, há suporte para os seguintes intervalos de faixas de preço:<br/><br/><ul><li>Se o valor *isAdvancedPricingModel* do [preço do recurso](#pricing-object) for **true**, os valores de nível de preço disponíveis para sua conta são **Tier1012** - **Tier1424**.</li><li>Se o valor *isAdvancedPricingModel* do [preço do recurso](#pricing-object) for **false**, os valores de nível de preço disponíveis para sua conta são **Tier2** - **Tier96**.</li></ul>Para visualizar a tabela completa de faixas de preço disponíveis para a conta de desenvolvedor, incluindo os preços específicos do mercado associados a cada camada, acesse a página **Preço e disponibilidade** de qualquer um dos envios de app no painel do Centro de Desenvolvimento e clique no link **tabela de modo de exibição** na seção **Mercados e preços personalizados** (para algumas contas de desenvolvedor, este link está na seção **Preço**).    |


<span id="enterprise-licensing" />
### <a name="enterprise-licensing-values"></a>Valores de licenciamento da empresa

Os seguintes valores representam o comportamento de licenciamento para empresa do app. Para obter mais informações sobre essas opções, consulte [Opções de licenciamento organizacional](https://msdn.microsoft.com/windows/uwp/publish/organizational-licensing).

| Valor           |  Descrição      |
|-----------------|---------------|
| Nenhum(a)            |     Não disponibilize seu app para empresas com licenciamento por volume gerenciado pela Loja (online).         |     
| Online        |     Disponibilize seu app para empresas com licenciamento por volume gerenciado pela Loja (online).  |
| OnlineAndOffline | Disponibilize seu app para as empresas com licenciamento por volume gerenciado pela Loja (online) e disponibilize seu app a empresas por meio de licenciamento desconectado (offline). |


<span id="submission-status-code" />
### <a name="submission-status-code"></a>Código de status do envio

Os seguintes valores representam o código de status de um envio.

| Valor           |  Descrição      |
|-----------------|---------------|
| Nenhum(a)            |     Nenhum código foi especificado.         |     
| InvalidArchive        |     O arquivo ZIP que contém o pacote não é válido ou tem um formato de arquivo não reconhecido.  |
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

* [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obter dados de app usando a API de envio da Windows Store](get-app-data.md)
* [Envios de app no painel do Centro de Desenvolvimento](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)

