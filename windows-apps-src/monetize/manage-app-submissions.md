---
author: mcleanbyron
ms.assetid: C7428551-4B31-4259-93CD-EE229007C4B8
description: "Use estes métodos na API de envio da Windows Store para gerenciar envios dos aplicativos que estão registrados em sua conta do Centro de Desenvolvimento do Windows."
title: Gerenciar envios de aplicativo usando a API de envio da Windows Store
translationtype: Human Translation
ms.sourcegitcommit: 020c8b3f4d9785842bbe127dd391d92af0962117
ms.openlocfilehash: ef7727befa20606800fb9747f9402be9be5cc9e4

---

# <a name="manage-app-submissions-using-the-windows-store-submission-api"></a>Gerenciar envios de aplicativo usando a API de envio da Windows Store

A API de envio da Windows Store oferece métodos que é possível usar para gerenciar envios dos aplicativos, inclusive distribuições de pacote graduais. Para obter uma introdução à API de envio da Windows Store, inclusive pré-requisitos para usar a API, consulte [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md).

>**Observação**&nbsp;&nbsp;Estes métodos só podem ser usados para contas do Centro de Desenvolvimento do Windows que tenham recebido permissão para usar a API de envio da Windows Store. Nem todas as contas têm essa permissão habilitada.

<span id="methods-for-app-submissions" />
## <a name="methods-for-managing-app-submissions"></a>Métodos para gerenciar envios de aplicativos

Use os métodos a seguir para obter, criar, atualizar, confirmar ou excluir um envio de aplicativo.

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
<td align="left">[Obter um envio de aplicativo existente](get-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status```</td>
<td align="left">[Obter o status de um envio de aplicativo existente](get-status-for-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions```</td>
<td align="left">[Criar um novo envio de aplicativo](create-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}```</td>
<td align="left">[Atualizar um envio de aplicativo existente](update-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit```</td>
<td align="left">[Confirmar um envio de aplicativo novo ou atualizado](commit-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}```</td>
<td align="left">[Excluir um envio de aplicativo](delete-an-app-submission.md)</td>
</tr>
</tbody>
</table>

<span id="create-an-app-submission">
### Criar um envio de aplicativo

Para criar um envio de um aplicativo, siga este processo.

1. Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Windows Store.

  >**Observação**&nbsp;&nbsp;Verifique se o aplicativo já tem pelo menos um envio concluído com as informações de [classificações etárias](https://msdn.microsoft.com/windows/uwp/publish/age-ratings) preenchidas.

2. [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Você deve passar esse token de acesso aos métodos na API de envio da Windows Store. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

3. [Criar um envio de aplicativo](create-an-app-submission.md) executando o seguinte método na API de envio da Windows Store. Esse método cria um novo envio em andamento, que é uma cópia de seu último envio publicado.

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions
  ```

  O corpo da resposta contém três itens: a ID do novo envio, os dados do novo envio (inclusive todas as listagens e informações de preço) e o URI da Assinatura de Acesso Compartilhado (SAS) para carregar todos os pacotes de aplicativos e imagens de lista para o envio ao armazenamento do Blob do Azure.

  >**Observação**&nbsp;&nbsp;Um URI SAS dá acesso a um recurso seguro no armazenamento do Azure sem exigir chaves de conta. Para obter informações contextuais sobre URIs SAS e o uso com o armazenamento do Blob do Azure, consulte [Assinaturas de acesso compartilhado, parte 1: Noções básicas sobre o modelo SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1) e [Assinaturas de acesso compartilhado, parte 2: Criar e usar uma SAS com o armazenamento de Blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

4. Se você estiver adicionando novos pacotes ou imagens para o envio, [prepare os pacotes de aplicativos](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements) e [prepare as imagens e as capturas de tela do aplicativo](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images). Adicione todos esses arquivos a um arquivo ZIP.

5. Revise os dados de envio com as alterações necessárias para o novo envio e execute o método a seguir para [atualizar o envio de aplicativo](update-an-app-submission.md).

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

5. [Confirme o envio de aplicativo](commit-an-app-submission.md) executando o método a seguir. Isso alertará o Centro de Desenvolvimento que você terminou seu envio e que suas atualizações agora devem ser aplicadas à sua conta.

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit
  ```

6. Verifique o status de confirmação executando o método a seguir para [obter o status de envio de aplicativo](get-status-for-an-app-submission.md).

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status
  ```

  Para confirmar o status de envio, examine o valor de *status* no corpo da resposta. Esse valor deve mudar de **CommitStarted** para **PreProcessing** se a solicitação for bem-sucedida ou **CommitFailed** se houver erros na solicitação. Se houver erros, o campo *statusDetails* contém mais detalhes sobre o erro.

7. Após a confirmação ser concluída, o envio será enviado para a Loja para inclusão. Você pode continuar monitorando o progresso do envio usando o método anterior ou visitando o painel do Centro de Desenvolvimento.

<span/>
### <a name="code-examples-for-managing-app-submissions"></a>Exemplos de código para gerenciar envios de aplicativo

Os artigos a seguir apresentam exemplos detalhados de código que demonstram como criar um envio de aplicativo em diversas linguagens de programação diferentes:

* [Exemplos de código em C#](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Exemplos de código em Java](java-code-examples-for-the-windows-store-submission-api.md)
* [Exemplos de código em Python](python-code-examples-for-the-windows-store-submission-api.md)

<span id="manage-gradual-package-rollout">
## <a name="methods-for-managing-a-gradual-package-rollout"></a>Métodos para gerenciar uma distribuição de pacote gradual

É possível distribuir gradualmente os pacotes atualizados em um envio de aplicativo para um percentual de clientes do aplicativo no Windows 10. Isso permite que você monitore comentários e dados de análise dos pacotes específicos para verificar se a atualização é necessária antes de implantá-la mais amplamente. Você pode alterar a porcentagem de distribuição (ou parar a atualização) para um envio publicado sem precisar criar um novo envio. Para obter mais detalhes, inclusive instruções sobre como habilitar e gerenciar uma distribuição de pacote gradual no painel do Centro de Desenvolvimento, consulte [este artigo](../publish/gradual-package-rollout.md).

Para habilitar programaticamente uma distribuição de pacote gradual para um envio de aplicativo, siga esse processo usando métodos na API de envio da Windows Store:

  1. [Crie um envio de aplicativo](create-an-app-submission.md) ou [obtenha um envio de aplicativo existente](get-an-app-submission.md).
  2. Nos dados de resposta, localize o recurso [packageRollout](#package-rollout-object), defina o campo *isPackageRollout* como **true** e o campo *packageRolloutPercentage* como o percentual de clientes do aplicativo que devem receber os pacotes atualizados.
  3. Passe os dados de envio de aplicativo atualizados para o método [atualizar um envio de aplicativo](update-an-app-submission.md).

Depois que uma distribuição de pacote gradual for habilitada para um envio de aplicativo, será possível usar os métodos a seguir para obter, atualizar, interromper ou finalizar programaticamente a distribuição gradual.

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
<td align="left">[Obter as informações de distribuição gradual para um envio de aplicativo](get-package-rollout-info-for-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/updatepackagerolloutpercentage```</td>
<td align="left">[Atualizar o percentual de distribuição gradual para um envio de aplicativo](update-the-package-rollout-percentage-for-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout```</td>
<td align="left">[Interromper a distribuição gradual para um envio de aplicativo](halt-the-package-rollout-for-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/finalizepackagerollout```</td>
<td align="left">[Finalizar a distribuição gradual para um envio de aplicativo](finalize-the-package-rollout-for-an-app-submission.md)</td>
</tr>
</tbody>
</table>

<span/>
## Recursos de dados Os métodos da API de envio da Windows Store para gerenciar envios do pacote de aplicativo usam os recursos de dados JSON a seguir.

<span id="app-submission-object" />
### Recurso de envio de aplicativo

Esse recurso descreve um envio de aplicativo.

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2"
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
| applicationCategory           | string  |   Uma cadeia de caracteres que especifica a [categoria e/ou subcategoria](https://msdn.microsoft.com/windows/uwp/publish/category-and-subcategory-table) para o aplicativo. Categorias e subcategorias são combinadas em uma única cadeia de caracteres com o caractere de sublinhado '_', como **BooksAndReference_EReader**.      |  
| pricing           |  objeto  | Um [recurso de preço](#pricing-object) que contém informações de preço para o aplicativo.        |   
| visibilidade           |  cadeia de caracteres  |  A visibilidade do aplicativo. Isso pode ter um dos seguintes valores: <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>       |   
| targetPublishMode           | string  | O modo de publicação do envio. Ele pode ter um dos seguintes valores: <ul><li>Imediata</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | A data de publicação do envio em formato ISO 8601, se o *targetPublishMode* estiver definido como SpecificDate.  |  
| listings           |   objeto  |  Um dicionário de pares de chave e valor, em que cada chave é um código de país e cada valor é um [recurso de listagem](#listing-object) que contém informações de listagem do aplicativo.       |   
| hardwarePreferences           |  array  |   Uma matriz de cadeias de caracteres que definem as [preferências de hardware](https://msdn.microsoft.com/windows/uwp/publish/enter-app-properties#hardware_preferences) do aplicativo. Ele pode ter um dos seguintes valores: <ul><li>Touch</li><li>Teclado</li><li>Mouse</li><li>Câmera</li><li>NfcHce</li><li>Nfc</li><li>BluetoothLE</li><li>Telefonia</li></ul>     |   
| automaticBackupEnabled           |  booliano  |   Indica se o Windows pode incluir dados do aplicativo em backups automáticos no OneDrive. Para obter mais informações, consulte [Declarações de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).   |   
| canInstallOnRemovableMedia           |  booliano  |   Indica se os clientes podem instalar o aplicativo em armazenamento removível. Para obter mais informações, consulte [Declarações de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).     |   
| isGameDvrEnabled           |  booliano |   Indica se o DVR de jogos está habilitado para o aplicativo.    |   
| hasExternalInAppProducts           |     booliano          |   Indica se o aplicativo permite que os usuários façam compras fora do sistema de comércio da Windows Store. Para obter mais informações, consulte [Declarações de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).     |   
| meetAccessibilityGuidelines           |    booliano           |  Indica se o aplicativo foi testado para atender às diretrizes de acessibilidade. Para obter mais informações, consulte [Declarações de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).      |   
| notesForCertification           |  string  |   Contém [observações de certificação](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification) do aplicativo.    |    
| status           |   string  |  O status do envio. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>Cancelado</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publicação</li><li>Publicado</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certificação</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>      |    
| statusDetails           |   objeto  | Um [recurso de detalhes do status](#status-details-object) que contém detalhes adicionais sobre o status do envio, inclusive informações sobre eventuais erros.       |    
| fileUploadUrl           |   cadeia de caracteres  | O URI da assinatura de acesso compartilhado (SAS) para carregar todos os pacotes para o envio. Se você estiver adicionando novos pacotes ou imagens para o envio, carregue o arquivo ZIP que contém os pacotes e imagens para essa URI. Para obter mais informações, consulte [Criar um envio de aplicativo](#create-an-app-submission).       |    
| applicationPackages           |   matriz  | Uma matriz de [recursos do pacote de aplicativos](#application-package-object) que dão detalhes sobre cada pacote no envio. |    
| packageDeliveryOptions    | objeto  | Um [recurso de opções de entrega do pacote](#package-delivery-options-object) que contém configurações da distribuição de pacote gradual e da atualização obrigatória para o envio.  |
| enterpriseLicensing           |  cadeia de caracteres  |  Um dos [valores de licenciamento empresarial](#enterprise-licensing) que indicam o comportamento de licenciamento empresarial para o aplicativo.  |    
| allowMicrosftDecideAppAvailabilityToFutureDeviceFamilies           |  booliano   |  Indica se a Microsoft tem permissão para [disponibilizar o aplicativo para as futuras famílias de dispositivos Windows 10](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families).    |    
| allowTargetFutureDeviceFamilies           | object   |  Um dicionário de pares de chave e valor, onde cada chave é uma [família de dispositivos Windows 10](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families) e cada valor é um valor booliano que indica se seu aplicativo tem permissão para segmentar a família de dispositivos especificadas.     |    
| friendlyName           |   string  |  O nome amigável do aplicativo, usado para fins de exibição.       |  


<span id="listing-object" />
### <a name="listing-resource"></a>Recurso de listagem

Esse recurso contém informações de listagem de um aplicativo. Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição                  |
|-----------------|---------|------|
|  baseListing               |   object      |  As informações de [listagem básica](#base-listing-object) do aplicativo, que definem o padrão de informações de listagem para todas as plataformas.   |     
|  platformOverrides               | object |   Um dicionário de pares de chave e valor, em que cada chave é a cadeia de caracteres que identifica uma plataforma cujas informações de listagem devem ser substituídas e cada valor é um recurso de [listagem básica](#base-listing-object) (contendo apenas os valores da descrição ao título) que especifica as informações de listagem a serem substituídas para a plataforma especificada. As chaves podem ter os seguintes valores: <ul><li>Unknown</li><li>Windows80</li><li>Windows81</li><li>WindowsPhone71</li><li>WindowsPhone80</li><li>WindowsPhone81</li></ul>     |      |     

<span id="base-listing-object" />
### <a name="base-listing-resource"></a>Recurso de listagem base

Esse recurso contém informações de listagem base de um aplicativo. Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição       |
|-----------------|---------|------|
|  copyrightAndTrademarkInfo                |   string      |  Informações opcionais de [direitos autorais e/ou marca comercial](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#copyright-and-trademark-info).  |
|  keywords                |  array       |  Uma matriz de [palavra-chave](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#keywords) para ajudar seu aplicativo a aparecer nos resultados de pesquisa.    |
|  licenseTerms                |    string     | Os [termos de licença](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#additional-license-terms) opcionais do seu aplicativo.     |
|  privacyPolicy                |   string      |   A URL para a [política de privacidade](https://msdn.microsoft.com/windows/uwp/publish/privacy-policy) do seu aplicativo.    |
|  supportContact                |   string      |  A URL ou endereço de email para as [informações de contato de suporte](https://msdn.microsoft.com/windows/uwp/publish/support-contact-info) do seu aplicativo.     |
|  websiteUrl                |   string      |  A URL da [página da Web](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#website) do seu aplicativo.    |    
|  description               |    string     |   A [descrição](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#description) dos detalhes do aplicativo.   |     
|  features               |    array     |  Uma matriz de até 20 cadeias de caracteres que lista os [recursos](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#app-features) do seu aplicativo.     |
|  releaseNotes               |  string       |  As [notas de versão](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#release-notes) do aplicativo.    |
|  images               |   matriz      |  Uma matriz de recursos de [imagem e ícone](#image-object) para a listagem do aplicativo.  |
|  recommendedHardware               |   matriz      |  Uma matriz de até 11 cadeias de caracteres que lista as [configurações de hardware recomendadas](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#recommended-hardware) para o aplicativo.     |
|  title               |     cadeia de caracteres    |   O título da listagem do aplicativo.   |  

<span id="image-object" />
### <a name="image-resource"></a>Recurso de imagem

Esse recurso contém dados de imagem e ícone para uma listagem do aplicativo. Para obter mais informações sobre imagens e ícones para detalhes, consulte [Capturas de tela e imagens do aplicativo](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images). Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição           |
|-----------------|---------|------|
|  fileName               |    string     |   O nome do arquivo de imagem no arquivo ZIP que você carregou para o envio.    |     
|  fileStatus               |   string      |  O status do arquivo de imagem. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>PendingUpload</li><li>Carregado</li><li>PendingDelete</li></ul>   |
|  id  |  string  | A ID da imagem, conforme especificado pelo Centro de Desenvolvimento.  |
|  description  |  string  | A descrição da imagem.  |
|  imageType  |  string  | Uma das seguintes cadeias de caracteres que indica o tipo da imagem: <ul><li>Unknown</li><li>Screenshot</li><li>PromotionalArtwork414X180</li><li>PromotionalArtwork846X468</li><li>PromotionalArtwork558X756</li><li>PromotionalArtwork414X468</li><li>PromotionalArtwork558X558</li><li>PromotionalArtwork2400X1200</li><li>Ícone</li><li>WideIcon358X173</li><li>BackgroundImage1000X800</li><li>SquareIcon358X358</li><li>MobileScreenshot</li><li>XboxScreenshot</li><li>SurfaceHubScreenshot</li><li>HoloLensScreenshot</li></ul>      |


<span id="pricing-object" />
### <a name="pricing-resource"></a>Recurso de preço

Esse recurso contém informações de preço do aplicativo. Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição        |
|-----------------|---------|------|
|  trialPeriod               |    string     |  Uma cadeia de caracteres que especifica o período de avaliação do aplicativo. Ele pode ter um dos seguintes valores: <ul><li>NoFreeTrial</li><li>OneDay</li><li>TrialNeverExpires</li><li>SevenDays</li><li>FifteenDays</li><li>ThirtyDays</li></ul>    |
|  marketSpecificPricings               |    object     |  Um dicionário de pares de chave e valor, onde cada chave é um código ISO 3166-1 alpha-2 de duas letras do país e cada valor é uma [faixa de preço](#price-tiers). Esses itens representam os [preços personalizados do aplicativo em mercados específicos](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices). Todos os itens nesse dicionário substituem o preço base especificado pelo valor *priceId* para o mercado especificado.      |     
|  sales               |   matriz      |  **Preterido**. Uma matriz de [recursos de venda](#sale-object) que contêm informações de venda do aplicativo.   |     
|  priceId               |   cadeia de caracteres      |  A [faixa de preço](#price-tiers) que especifica o [preço base](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#base-price) do aplicativo.   |


<span id="sale-object" />
### <a name="sale-resource"></a>Recurso de venda

Esse recurso contém informações de venda de um aplicativo.

>**Importante**&nbsp;&nbsp;O recurso **Promoção** não tem mais suporte, e atualmente você não pode acessar nem modificar os dados de venda de um envio de aplicativo usando a API de envio da Windows Store:

   > * Depois de chamar o [método GET para obter um envio de aplicativo](get-an-app-submission.md), o valor de *sales* estará vazio. Você pode continuar a usar o painel do Centro de Desenvolvimento para obter os dados de venda de seu envio de aplicativo.
   > * Ao chamar o [método PUT para atualizar um envio de aplicativo](update-an-app-submission.md), as informações no valor de *sales* serão ignoradas. Você pode continuar a usar o painel do Centro de Desenvolvimento para modificar os dados de venda de seu envio de aplicativo.

> No futuro, atualizaremos a API de envio da Windows Store para apresentar uma nova maneira de acessar programaticamente as informações de vendas para envios de aplicativo.

Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição    |
|-----------------|---------|------|
|  name               |    string     |   O nome da promoção.    |     
|  basePriceId               |   string      |  A [faixa de preço](#price-tiers) a ser usada para o preço base da promoção.    |     
|  startDate               |   string      |   A data de início da promoção no formato ISO 8601.  |     
|  endDate               |   string      |  A data de término da promoção no formato ISO 8601.      |     
|  marketSpecificPricings               |   object      |   Um dicionário de pares de chave e valor, onde cada chave é um código ISO 3166-1 alpha-2 de duas letras do país e cada valor é uma [faixa de preço](#price-tiers). Esses itens representam os [preços personalizados do aplicativo em mercados específicos](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices). Todos os itens nesse dicionário substituem o preço base especificado pelo valor *basePriceId* para o mercado especificado.    |


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
### <a name="application-package-resource"></a>Recurso do pacote de aplicativos

Esse recurso contém detalhes sobre um pacote de aplicativos para o envio.

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

>**Observação**&nbsp;&nbsp;Durante a chamada do método [atualizar um envio de aplicativo](update-an-app-submission.md), apenas os valores *fileName*, *fileStatus*, *minimumDirectXVersion* e *minimumSystemRam* desse objeto são necessários no corpo da solicitação. Os outros valores são preenchidos pelo Centro de Desenvolvimento.

| Valor           | Tipo    | Descrição                   |
|-----------------|---------|------|
| fileName   |   string      |  O nome do pacote.    |  
| fileStatus    | string    |  O status do pacote. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>PendingUpload</li><li>Carregado</li><li>PendingDelete</li></ul>    |  
| id    |  string   |  Uma ID que identifica exclusivamente o pacote. Esse valor é usado pelo Centro de Desenvolvimento.   |     
| version    |  string   |  A versão do pacote do aplicativo. Para obter mais informações, consulte [Numeração de versão do pacote](https://msdn.microsoft.com/windows/uwp/publish/package-version-numbering).   |   
| architecture    |  string   |  A arquitetura do pacote (por exemplo, ARM).   |     
| languages    | array    |  Uma matriz de códigos de idioma para os idiomas com suporte do aplicativo. Para obter mais informações, consulte [Idiomas suportados](https://msdn.microsoft.com/windows/uwp/publish/supported-languages).    |     
| capabilities    |  array   |  Uma matriz de recursos necessários pelo pacote. Para obter mais informações sobre recursos, consulte [Declarações de recursos de aplicativos](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations).   |     
| minimumDirectXVersion    |  string   |  A versão mínima do DirectX que é compatível com o pacote do aplicativo. Isso pode ser definido apenas para aplicativos que visam o Windows 8.x; isso é ignorado para aplicativos destinados a outras versões. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | string    |  A RAM mínima necessária para o pacote do aplicativo. Isso pode ser definido apenas para aplicativos que visam o Windows 8.x; isso é ignorado para aplicativos destinados a outras versões. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>Memory2GB</li></ul>   |       
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
| fallbackSubmissionId    |  string   |  A ID do envio que será recebida por clientes que não recebem os pacotes de distribuição gradual.   |          

<span/>

## <a name="enums"></a>Enumerações

Esses métodos usam as enumerações a seguir.

<span id="price-tiers" />
### <a name="price-tiers"></a>Faixas de preço

Os seguintes valores representam as faixas de preço disponíveis para um envio de aplicativo.

| Valor           | Descrição        |
|-----------------|------|
|  Base               |   A faixa de preço não está definida. Use o preço base para o aplicativo.      |     
|  NotAvailable              |   O aplicativo não está disponível na região especificada.    |     
|  Grátis              |   O aplicativo é gratuito.    |    
|  Tier2 até Tier194               |   Tier2 representa a faixa de preço 0,99 USD. Cada nível adicional representa incrementos adicionais (1,29 USD, 1,49 USD, 1,99 USD e assim por diante).    |


<span id="enterprise-licensing" />
### <a name="enterprise-licensing-values"></a>Valores de licenciamento da empresa

Os seguintes valores representam o comportamento de licenciamento para empresa do aplicativo. Para obter mais informações sobre essas opções, consulte [Opções de licenciamento organizacional](https://msdn.microsoft.com/windows/uwp/publish/organizational-licensing).

| Valor           |  Descrição      |
|-----------------|---------------|
| Nenhum(a)            |     Não disponibilize seu aplicativo para empresas com licenciamento por volume gerenciado pela Loja (online).         |     
| Online        |     Disponibilize seu aplicativo para empresas com licenciamento por volume gerenciado pela Loja (online).  |
| OnlineAndOffline | Disponibilize seu aplicativo para as empresas com licenciamento por volume gerenciado pela Loja (online) e disponibilize seu aplicativo a empresas por meio de licenciamento desconectado (offline). |


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
* [Obter dados de aplicativo usando a API de envio da Windows Store](get-app-data.md)
* [Envios de aplicativo no painel do Centro de Desenvolvimento](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)



<!--HONumber=Dec16_HO3-->


