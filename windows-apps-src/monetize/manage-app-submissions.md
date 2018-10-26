---
author: Xansky
ms.assetid: C7428551-4B31-4259-93CD-EE229007C4B8
description: Use estes métodos na API de envio da Microsoft Store para gerenciar envios dos apps que estão registrados em sua conta do Centro de Desenvolvimento do Windows.
title: Gerenciar envios de aplicativo
ms.author: mhopkins
ms.date: 04/30/2018
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, envios de aplicativo
ms.localizationpriority: medium
ms.openlocfilehash: b042e3cc3e92bce7a895e717c6bc20c2e11d1677
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5571996"
---
# <a name="manage-app-submissions"></a>Gerenciar envios de aplicativo

A API de envio da Microsoft Store oferece métodos que é possível usar para gerenciar envios dos aplicativos, inclusive distribuições de pacote graduais. Para obter uma introdução à API de envio da Microsoft Store, inclusive pré-requisitos para usar a API, consulte [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md).

> [!IMPORTANT]
> Se você usar essa API de envio da Microsoft Store para criar um envio para um aplicativo, certifique-se de fazer outras alterações no envio somente usando a API, em vez do painel do Centro de Desenvolvimento. Se você usar o painel para alterar um envio que criou originalmente usando a API, você não poderá alterar ou confirmar esse envio usando a API. Em alguns casos, o envio pode ficar em um estado de erro em que ele não pode continuar no processo de envio. Se isso ocorrer, você deve excluir o envio e criar um novo.

> [!IMPORTANT]
> Você não pode usar essa API para publicar os envios de [compras de volume por meio da Microsoft Store para Empresas e da Microsoft Store para Educação](../publish/organizational-licensing.md) ou publicar os envios de [aplicativos LOB](../publish/distribute-lob-apps-to-enterprises.md) diretamente para empresas. Para esses dois cenários, você deve usar o painel do Centro de Desenvolvimento do Windows para publicar o envio.


<span id="methods-for-app-submissions" />

## <a name="methods-for-managing-app-submissions"></a>Métodos para gerenciar envios de aplicativo

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="get-an-app-submission.md">Obter um envio de aplicativo existente</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-an-app-submission.md">Obter o status de um envio de aplicativo existente</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions</td>
<td align="left"><a href="create-an-app-submission.md">Criar um novo envio de aplicativo</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="update-an-app-submission.md">Atualizar um envio de aplicativo existente</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-an-app-submission.md">Confirmar um envio de aplicativo novo ou atualizado</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="delete-an-app-submission.md">Excluir um envio de aplicativo</a></td>
</tr>
</tbody>
</table>

<span id="create-an-app-submission">

### <a name="create-an-app-submission"></a>Criar um envio de aplicativo

Para criar um envio de um aplicativo, siga este processo.

1. Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Microsoft Store.
    > [!NOTE]
    > Verifique se o aplicativo já tem pelo menos um envio concluído com as informações de [classificações etárias](https://msdn.microsoft.com/windows/uwp/publish/age-ratings) preenchidas.

2. [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Você deve passar esse token de acesso aos métodos na API de envio da Microsoft Store. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

3. [Criar um envio de aplicativo](create-an-app-submission.md) executando o seguinte método na API de envio da Microsoft Store. Esse método cria um novo envio em andamento, que é uma cópia de seu último envio publicado.

    ```
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions
    ```

    O corpo da resposta contém um recurso [envio de complemento](#app-submission-object) que inclui a ID do novo envio, o URI da SAS (assinatura de acesso compartilhado) para o upload de todos os arquivos relacionados para o envio para o armazenamento de Blobs do Azure (como pacotes de aplicativo, imagens de listagem e arquivos de trailer) e todos os dados do novo envio (como as listagens e as informações sobre preços).

    > [!NOTE]
    > Um URI SAS dá acesso a um recurso seguro no armazenamento do Azure sem exigir chaves de conta. Para obter informações contextuais sobre URIs SAS e o uso com o armazenamento do Blob do Azure, consulte [Assinaturas de acesso compartilhado, parte 1: Noções básicas sobre o modelo SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1) e [Assinaturas de acesso compartilhado, parte 2: Criar e usar uma SAS com o armazenamento de Blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

4. Se você estiver adicionando novos pacotes, imagens de listagem ou arquivos de trailer para o envio, [prepare os pacotes de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements) e [prepare as imagens, os trailers e as capturas de tela do app](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images). Adicione todos esses arquivos a um arquivo ZIP.

5. Revise os dados de [envio de aplicativo](#app-submission-object) com as alterações necessárias para o novo envio e execute o método a seguir para [atualizar o envio de aplicativo](update-an-app-submission.md).

    ```
    PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}
    ```
      > [!NOTE]
      > Se você estiver adicionando novos arquivos para o envio, atualize os dados de envio para fazer referência ao nome e caminho relativo desses arquivos no arquivo ZIP.

4. Se você estiver adicionando novos pacotes, imagens de listagem ou arquivos de trailer para o envio, carregue o arquivo ZIP no [armazenamento do Blob do Azure](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage) usando o URI SAS que foi fornecido no corpo da resposta do método POST chamado anteriormente. Existem bibliotecas do Azure diferentes que é possível usar para fazer isso em uma grande variedade de plataformas, inclusive:

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

5. [Confirme o envio de aplicativo](commit-an-app-submission.md) executando o método a seguir. Isso alertará o Centro de Desenvolvimento que você terminou seu envio e que suas atualizações agora devem ser aplicadas à sua conta.

    ```
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit
    ```

6. Verifique o status de confirmação executando o método a seguir para [obter o status de envio de aplicativo](get-status-for-an-app-submission.md).

    ```
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status
    ```

    Para confirmar o status de envio, examine o valor de *status* no corpo da resposta. Esse valor deve mudar de **CommitStarted** para **PreProcessing** se a solicitação for bem-sucedida ou **CommitFailed** se houver erros na solicitação. Se houver erros, o campo *statusDetails* contém mais detalhes sobre o erro.

7. Após a confirmação ser concluída, o envio será enviado para a Loja para inclusão. Você pode continuar monitorando o progresso do envio usando o método anterior ou visitando o painel do Centro de Desenvolvimento.

<span id="manage-gradual-package-rollout">

## <a name="methods-for-managing-a-gradual-package-rollout"></a>Métodos para gerenciar uma distribuição de pacote gradual

É possível distribuir gradualmente os pacotes atualizados em um envio de aplicativo para um percentual de clientes do aplicativo no Windows 10. Isso permite que você monitore comentários e dados de análise dos pacotes específicos para verificar se a atualização é necessária antes de implantá-la mais amplamente. Você pode alterar a porcentagem de distribuição (ou parar a atualização) para um envio publicado sem precisar criar um novo envio. Para obter mais detalhes, inclusive instruções sobre como habilitar e gerenciar uma distribuição de pacote gradual no painel do Centro de Desenvolvimento, consulte [este artigo](../publish/gradual-package-rollout.md).

Para habilitar programaticamente uma distribuição de pacote gradual para um envio de aplicativo, siga esse processo usando métodos na API de envio da Microsoft Store:

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/packagerollout</td>
<td align="left"><a href="get-package-rollout-info-for-an-app-submission.md">Obter as informações de distribuição gradual para um envio de aplicativo</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/updatepackagerolloutpercentage</td>
<td align="left"><a href="update-the-package-rollout-percentage-for-an-app-submission.md">Atualizar o percentual de distribuição gradual para um envio de aplicativo</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout</td>
<td align="left"><a href="halt-the-package-rollout-for-an-app-submission.md">Interromper a distribuição gradual para um envio de aplicativo</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/finalizepackagerollout</td>
<td align="left"><a href="finalize-the-package-rollout-for-an-app-submission.md">Finalizar a distribuição gradual para um envio de aplicativo</a></td>
</tr>
</tbody>
</table>


## <a name="code-examples-for-managing-app-submissions"></a>Exemplos de código para gerenciar envios de aplicativo

Os artigos a seguir apresentam exemplos detalhados de código que demonstram como criar um envio de aplicativo em diversas linguagens de programação diferentes:

* [Exemplo de C#: envios de apps, complementos e versões de pré-lançamento](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Exemplo de C#: envio de aplicativo com opções de jogo e trailers](csharp-code-examples-for-submissions-game-options-and-trailers.md)
* [Exemplo de Java: envios de apps, complementos e versões de pré-lançamento](java-code-examples-for-the-windows-store-submission-api.md)
* [Exemplo de Java: envio de aplicativo com opções de jogo e trailers](java-code-examples-for-submissions-game-options-and-trailers.md)
* [Exemplo de Python: envios para apps, complementos e versões de pré-lançamento](python-code-examples-for-the-windows-store-submission-api.md)
* [Exemplo de Python: envio de aplicativo com opções e trailers de jogos](python-code-examples-for-submissions-game-options-and-trailers.md)

## <a name="storebroker-powershell-module"></a>Módulo StoreBroker do PowerShell

Como uma alternativa à chamada direta à API de envio da Microsoft Store, nós também fornecemos um módulo do PowerShell de software livre que implementa uma interface de linha de comando sobre API. Esse módulo é chamado [StoreBroker](https://aka.ms/storebroker). Você pode usar esse módulo para gerenciar seu app, versão de pré-lançamento e envios de complemento na linha de comando em vez de chamar diretamente a API de envio da Microsoft Store, ou você pode simplesmente procurar a fonte para ver mais exemplos de como chamar essa API. O módulo StoreBroker ativamente é usado dentro da Microsoft como a principal forma de muitos apps de terceiros serem enviados para a Store.

Para obter mais informações, consulte nossa [Página do StoreBroker no GitHub](https://aka.ms/storebroker).

<span/>

## <a name="data-resources"></a>Recursos de dados
Os métodos da API de envio da Microsoft Store para gerenciar envios de aplicativo usam os recursos de dados JSON a seguir.

<span id="app-submission-object" />

### <a name="app-submission-resource"></a>Recurso de envio de aplicativo

Esse recurso descreve um envio de aplicativo.

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2",
    "isAdvancedPricingModel": true
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [
          "epub"
        ],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [
          "Free ebook reader"
        ],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "description": "Main page",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "Contoso ebook reader"
      },
      "platformOverrides": {
        "Windows81": {
          "description": "Ebook reader for Windows 8.1"
        }
      }
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
  "gamingOptions": [],
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
        "packageRolloutPercentage": 0.0,
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
  "friendlyName": "Submission 2",
  "trailers": []
}
```

Esse recurso tem os valores a seguir.

| Valor      | Tipo   | Descrição      |
|------------|--------|-------------------|
| id            | string  | A ID do envio. Essa ID está disponível nos dados de resposta para solicitações para [criar um envio de aplicativo](create-an-app-submission.md), [obter todos os apps](get-all-apps.md) e [obter um app](get-an-app.md). Para um envio criado no painel do Centro de Desenvolvimento, essa ID também está disponível na URL da página de envio no painel.  |
| applicationCategory           | string  |   Uma cadeia de caracteres que especifica a [categoria e/ou subcategoria](https://msdn.microsoft.com/windows/uwp/publish/category-and-subcategory-table) para o aplicativo. Categorias e subcategorias são combinadas em uma única cadeia de caracteres com o caractere de sublinhado '_', como **BooksAndReference_EReader**.      |  
| pricing           |  objeto  | Um [recurso de preço](#pricing-object) que contém informações de preço para o aplicativo.        |   
| visibilidade           |  string  |  A visibilidade do aplicativo. Ele pode ter um dos seguintes valores: <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>       |   
| targetPublishMode           | string  | O modo de publicação do envio. Ele pode ter um dos seguintes valores: <ul><li>Imediata</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | A data de publicação do envio em formato ISO 8601, se o *targetPublishMode* estiver definido como SpecificDate.  |  
| listings           |   objeto  |  Um dicionário de pares de chave e valor, em que cada chave é um código de país e cada valor é um [recurso de listagem](#listing-object) que contém informações de listagem do aplicativo.       |   
| hardwarePreferences           |  array  |   Uma matriz de cadeias de caracteres que definem as [preferências de hardware](https://msdn.microsoft.com/windows/uwp/publish/enter-app-properties#hardware_preferences) do aplicativo. Ele pode ter um dos seguintes valores: <ul><li>Touch</li><li>Teclado</li><li>Mouse</li><li>Câmera</li><li>NfcHce</li><li>Nfc</li><li>BluetoothLE</li><li>Telefonia</li></ul>     |   
| automaticBackupEnabled           |  booliano  |   Indica se o Windows pode incluir dados do aplicativo em backups automáticos no OneDrive. Para obter mais informações, consulte [Declarações de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).   |   
| canInstallOnRemovableMedia           |  booliano  |   Indica se os clientes podem instalar o aplicativo em armazenamento removível. Para obter mais informações, consulte [Declarações de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).     |   
| isGameDvrEnabled           |  booliano |   Indica se o DVR de jogos está habilitado para o aplicativo.    |   
| gamingOptions           |  array |   Uma matriz que contém um [recurso de opções de jogos](#gaming-options-object) que define as configurações relacionadas a jogos para o app.     |   
| hasExternalInAppProducts           |     boolean          |   Indica se o aplicativo permite que os usuários façam compras fora do sistema de comércio da Microsoft Store. Para obter mais informações, consulte [Declarações de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).     |   
| meetAccessibilityGuidelines           |    booliano           |  Indica se o aplicativo foi testado para atender às diretrizes de acessibilidade. Para obter mais informações, consulte [Declarações de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).      |   
| notesForCertification           |  string  |   Contém [observações de certificação](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification) do aplicativo.    |    
| status           |   string  |  O status do envio. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>Cancelado</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publicação</li><li>Publicado</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certificação</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>      |    
| statusDetails           |   objeto  | Um [recurso de detalhes do status](#status-details-object) que contém detalhes adicionais sobre o status do envio, inclusive informações sobre eventuais erros.       |    
| fileUploadUrl           |   string  | O URI da assinatura de acesso compartilhado (SAS) para carregar todos os pacotes para o envio. Se você estiver adicionando novos pacotes, imagens de listagem ou arquivos de trailer para o envio, carregue o arquivo ZIP que contém os pacotes e imagens para este URI. Para obter mais informações, consulte [Criar um envio de aplicativo](#create-an-app-submission).       |    
| applicationPackages           |   matriz  | Uma matriz de [recursos do pacote de aplicativos](#application-package-object) que dão detalhes sobre cada pacote no envio. |    
| packageDeliveryOptions    | objeto  | Um [recurso de opções de entrega do pacote](#package-delivery-options-object) que contém configurações da distribuição de pacote gradual e da atualização obrigatória para o envio.  |
| enterpriseLicensing           |  string  |  Um dos [valores de licenciamento empresarial](#enterprise-licensing) que indicam o comportamento de licenciamento empresarial para o aplicativo.  |    
| allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies           |  boolean   |  Indica se a Microsoft tem permissão para [disponibilizar o aplicativo para as futuras famílias de dispositivos Windows 10](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families).    |    
| allowTargetFutureDeviceFamilies           | object   |  Um dicionário de pares de chave e valor, onde cada chave é uma [família de dispositivos Windows 10](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families) e cada valor é um valor booliano que indica se seu aplicativo tem permissão para segmentar a família de dispositivos especificadas.     |    
| friendlyName           |   cadeia  |  O nome amigável do envio, conforme mostrado no painel do Centro de Desenvolvimento. Esse valor é gerado para você ao criar o envio.       |  
| trailers           |  array |   Uma matriz que contém até 15 [recursos de trailer](#trailer-object) que representam trailers de vídeo para a listagem de apps.<br/><br/>   |  


<span id="pricing-object" />

### <a name="pricing-resource"></a>Recurso de preço

Esse recurso contém informações de preço do aplicativo. Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição        |
|-----------------|---------|------|
|  trialPeriod               |    string     |  Uma cadeia de caracteres que especifica o período de avaliação do aplicativo. Ele pode ter um dos seguintes valores: <ul><li>NoFreeTrial</li><li>OneDay</li><li>TrialNeverExpires</li><li>SevenDays</li><li>FifteenDays</li><li>ThirtyDays</li></ul>    |
|  marketSpecificPricings               |    object     |  Um dicionário de pares de chave e valor, onde cada chave é um código ISO 3166-1 alpha-2 de duas letras do país e cada valor é uma [faixa de preço](#price-tiers). Esses itens representam os [preços personalizados do aplicativo em mercados específicos](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices). Todos os itens nesse dicionário substituem o preço base especificado pelo valor *priceId* para o mercado especificado.      |     
|  sales               |   matriz      |  **Preterido**. Uma matriz de [recursos de venda](#sale-object) que contêm informações de venda do aplicativo.   |     
|  priceId               |   cadeia de caracteres      |  A [faixa de preço](#price-tiers) que especifica o [preço base](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#base-price) do app.   |     
|  isAdvancedPricingModel               |   booliano      |  Se for **true**, sua conta de desenvolvedor tem acesso ao conjunto expandido de faixas de preço de US$ 0,99 a US$ 1999,99. Se for **true**, sua conta de desenvolvedor tem acesso ao conjunto original de faixas de preço de US$ 0,99 a US$ 999,99. Para saber mais sobre as diferentes camadas, consulte [faixas de preço](#price-tiers).<br/><br/>**Observação**&nbsp;&nbsp;Esse campo é somente leitura.   |


<span id="sale-object" />

### <a name="sale-resource"></a>Recurso de venda

Esse recurso contém informações de venda de um aplicativo.

> [!IMPORTANT]
> O recurso **Venda** não tem mais suporte, e atualmente você não pode acessar nem modificar os dados de venda de um envio de aplicativo usando a API de envio da Microsoft Store. No futuro, atualizaremos a API de envio da Microsoft Store para apresentar uma nova maneira de acessar programaticamente as informações de vendas para envios de aplicativo.
>    * Depois de chamar o [método GET para obter um envio de aplicativo](get-an-app-submission.md), o valor de *sales* estará vazio. Você pode continuar a usar o painel do Centro de Desenvolvimento para obter os dados de venda de seu envio de aplicativo.
>    * Ao chamar o [método PUT para atualizar um envio de aplicativo](update-an-app-submission.md), as informações no valor de *sales* serão ignoradas. Você pode continuar a usar o painel do Centro de Desenvolvimento para modificar os dados de venda de seu envio de aplicativo.

Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição    |
|-----------------|---------|------|
|  name               |    string     |   O nome da promoção.    |     
|  basePriceId               |   string      |  A [faixa de preço](#price-tiers) a ser usada para o preço base da promoção.    |     
|  startDate               |   string      |   A data de início da promoção no formato ISO 8601.  |     
|  endDate               |   string      |  A data de término da promoção no formato ISO 8601.      |     
|  marketSpecificPricings               |   object      |   Um dicionário de pares de chave e valor, onde cada chave é um código ISO 3166-1 alpha-2 de duas letras do país e cada valor é uma [faixa de preço](#price-tiers). Esses itens representam os [preços personalizados do aplicativo em mercados específicos](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices). Todos os itens nesse dicionário substituem o preço base especificado pelo valor *basePriceId* para o mercado especificado.    |


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
|  privacyPolicy                |   string      |   Este valor está obsoleto. Para definir ou alterar a URL da política de privacidade para o seu app, faça isso na página [Propriedades](../publish/enter-app-properties.md#privacy-policy-url) no painel do Centro de Desenvolvimento. Você pode omitir esse valor de suas chamadas para a API de envio. Se você definir esse valor, ele será ignorado.       |
|  supportContact                |   string      |  Este valor está obsoleto. Para definir ou alterar a URL do contato de suporte ou o email para o seu app, faça isso na página [Propriedades](../publish/enter-app-properties.md#support-contact-info) no painel do Centro de Desenvolvimento. Você pode omitir esse valor de suas chamadas para a API de envio. Se você definir esse valor, ele será ignorado.        |
|  websiteUrl                |   string      |  Este valor está obsoleto. Para definir ou alterar a URL da página da Web para o seu app, faça isso na página [Propriedades](../publish/enter-app-properties.md#website) no painel do Centro de Desenvolvimento. Você pode omitir esse valor de suas chamadas para a API de envio. Se você definir esse valor, ele será ignorado.      |    
|  descrição               |    string     |   A [descrição](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#description) dos detalhes do aplicativo.   |     
|  features               |    array     |  Uma matriz de até 20 cadeias de caracteres que lista os [recursos](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#app-features) do seu aplicativo.     |
|  releaseNotes               |  string       |  As [notas de versão](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#release-notes) do aplicativo.    |
|  images               |   matriz      |  Uma matriz de recursos de [imagem e ícone](#image-object) para a listagem do aplicativo.  |
|  recommendedHardware               |   matriz      |  Uma matriz de até 11 cadeias de caracteres que lista as [configurações de hardware recomendadas](../publish/create-app-store-listings.md#additional-information) para o aplicativo.     |
|  minimumHardware               |     string    |  Uma matriz de até 11 cadeias de caracteres que lista as [configurações de hardware mínimas](../publish/create-app-store-listings.md#additional-information) para o app.    |  
|  title               |     string    |   O título da listagem do aplicativo.   |  
|  shortDescription               |     string    |  Usado somente para jogos. Essa descrição é exibida na seção **Informações** do Hub de Jogos no Xbox One e ajuda os clientes a entender mais sobre o seu jogo.   |  
|  shortTitle               |     string    |  Uma versão mais curta do nome do seu produto. Se fornecido, esse nome mais curto pode aparecer em vários lugares no Xbox One (durante a instalação, em Conquistas etc.) no lugar do título completo do seu produto.    |  
|  sortTitle               |     string    |   Se seu produto puder ser colocado em ordem alfabética de maneiras diferentes, você poderá inserir outra versão aqui. Isso pode ajudar os clientes a encontrar o produto mais rapidamente ao pesquisar.    |  
|  voiceTitle               |     string    |   Um nome alternativo para seu produto que, se fornecido, pode ser usado na experiência de áudio no Xbox One ao usar o Kinect ou um headset.    |  
|  devStudio               |     string    |   Especifica esse valor se você quiser incluir um campo **Desenvolvido por** na listagem. (O campo **Publicado por** listará o nome de exibição do publicador associado à conta, independentemente de você fornecer ou não um valor para o campo *devStudio*).    |  

<span id="image-object" />

### <a name="image-resource"></a>Recurso de imagem

Esse recurso contém dados de imagem e ícone para uma listagem do aplicativo. Para obter mais informações sobre imagens e ícones para uma listagem de app, consulte [Capturas de tela e imagens do app](../publish/app-screenshots-and-images.md). Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição           |
|-----------------|---------|------|
|  fileName               |    string     |   O nome do arquivo de imagem no arquivo ZIP que você carregou para o envio.    |     
|  fileStatus               |   string      |  O status do arquivo de imagem. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>PendingUpload</li><li>Carregado</li><li>PendingDelete</li></ul>   |
|  id  |  string  | A ID da imagem. Esse valor é fornecido pelo Centro de Desenvolvimento.  |
|  descrição  |  string  | A descrição da imagem.  |
|  imageType  |  string  | Indica o tipo da imagem. Há suporte para as seguintes cadeias de caracteres. <p/>[Imagens de captura de tela](../publish/app-screenshots-and-images.md#screenshots): <ul><li>Captura de tela (use esse valor para a captura de tela da área de trabalho)</li><li>MobileScreenshot</li><li>XboxScreenshot</li><li>SurfaceHubScreenshot</li><li>HoloLensScreenshot</li></ul><p/>[Logotipos da Loja](../publish/app-screenshots-and-images.md#store-logos):<ul><li>StoreLogo9x16 </li><li>StoreLogoSquare</li><li>Ícone (use esse valor para o logotipo 1:1 de 300 x 300 pixels)</li></ul><p/>[Imagens promocionais](../publish/app-screenshots-and-images.md#promotional-images): <ul><li>PromotionalArt16x9</li><li>PromotionalArtwork2400X1200</li></ul><p/>[Imagens do Xbox](../publish/app-screenshots-and-images.md#xbox-images): <ul><li>XboxBrandedKeyArt</li><li>XboxTitledHeroArt</li><li>XboxFeaturedPromotionalArt</li></ul><p/>[Imagens promocionais opcionais](../publish/app-screenshots-and-images.md#optional-promotional-images): <ul><li>SquareIcon358X358</li><li>BackgroundImage1000X800</li><li>PromotionalArtwork414X180</li></ul><p/> <!-- The following strings are also recognized for this field, but they correspond to image types that are no longer for listings in the Store.<ul><li>PromotionalArtwork846X468</li><li>PromotionalArtwork558X756</li><li>PromotionalArtwork414X468</li><li>PromotionalArtwork558X558</li><li>WideIcon358X173</li><li>Unknown</li></ul> -->   |


<span id="gaming-options-object" />

### <a name="gaming-options-resource"></a>Recurso de opções de jogo

Esse recurso contém configurações relacionadas a jogo para o app. Os valores nesse recurso correspondem às [configurações de jogo](../publish/enter-app-properties.md#game-settings) para envios no painel do Centro de Desenvolvimento.

```json
{
  "gamingOptions": [
    {
      "genres": [
        "Games_ActionAndAdventure",
        "Games_Casino"
      ],
      "isLocalMultiplayer": true,
      "isLocalCooperative": true,
      "isOnlineMultiplayer": false,
      "isOnlineCooperative": false,
      "localMultiplayerMinPlayers": 2,
      "localMultiplayerMaxPlayers": 12,
      "localCooperativeMinPlayers": 2,
      "localCooperativeMaxPlayers": 12,
      "isBroadcastingPrivilegeGranted": true,
      "isCrossPlayEnabled": false,
      "kinectDataForExternal": "Enabled"
    }
  ],
}
```

Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição        |
|-----------------|---------|------|
|  gêneros               |    array     |  Uma matriz de uma ou mais das seguintes cadeias de caracteres que descrevem os gêneros do jogo: <ul><li>Games_ActionAndAdventure</li><li>Games_CardAndBoard</li><li>Games_Casino</li><li>Games_Educational</li><li>Games_FamilyAndKids</li><li>Games_Fighting</li><li>Games_Music</li><li>Games_Platformer</li><li>Games_PuzzleAndTrivia</li><li>Games_RacingAndFlying</li><li>Games_RolePlaying</li><li>Games_Shooter</li><li>Games_Simulation</li><li>Games_Sports</li><li>Games_Strategy</li><li>Games_Word</li></ul>    |
|  isLocalMultiplayer               |    booliano     |  Indica se o jogo dá suporte a multijogador local.      |     
|  isLocalCooperative               |   booliano      |  Indica se o jogo dá suporte a cooperação local.    |     
|  isOnlineMultiplayer               |   booliano      |  Indica se o jogo dá suporte a multijogador online.    |     
|  isOnlineCooperative               |   booliano      |  Indica se o jogo dá suporte a cooperação online.    |     
|  localMultiplayerMinPlayers               |   int      |   Especifica o número mínimo de jogadores a que o jogo dá suporte para multijogador local.   |     
|  localMultiplayerMaxPlayers               |   int      |   Especifica o número máximo de jogadores a que o jogo dá suporte para multijogador local.  |     
|  localCooperativeMinPlayers               |   int      |   Especifica o número mínimo de jogadores a que o jogo dá suporte para cooperação local.  |     
|  localCooperativeMaxPlayers               |   int      |   Especifica o número máximo de jogadores a que o jogo dá suporte para cooperação local.  |     
|  isBroadcastingPrivilegeGranted               |   booliano      |  Indica se o jogo dá suporte a difusão.   |     
|  isCrossPlayEnabled               |   booliano      |   Indica se o jogo oferece suporte a sessões multijogador entre jogadores no Xbox e em computadores Windows 10.  |     
|  kinectDataForExternal               |   string      |  Um dos seguintes valores de cadeia de caracteres que indica se o jogo pode coletar dados do Kinect e enviá-los a serviços externos: <ul><li>NotSet</li><li>Desconhecido</li><li>Habilitado</li><li>Desabilitado</li></ul>   |

> [!NOTE]
> O recurso *gamingOptions* foi adicionado em maio de 2017, depois que a API de envio da Microsoft Store foi lançada pela primeira vez para desenvolvedores. Se você tiver criado um envio para um app por meio da API de envio antes da introdução desse recurso e se esse envio ainda estiver em andamento, esse recurso será nulo para envios para o app até que você confirme com êxito o envio ou o exclua. Se o recurso *gamingOptions* não estiver disponível para envios para um app, o campo *hasAdvancedListingPermission* do [recurso Application](get-app-data.md#application_object) retornado pelo método [obter um app](get-an-app.md) será falso.

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

> [!NOTE]
> Durante a chamada do método [atualizar um envio de aplicativo](update-an-app-submission.md), somente os valores *fileName*, *fileStatus*, *minimumDirectXVersion*, e *minimumSystemRam* desse objeto são necessários no corpo da solicitação. Os outros valores são preenchidos pelo Centro de Desenvolvimento.

| Valor           | Tipo    | Descrição                   |
|-----------------|---------|------|
| fileName   |   string      |  O nome do pacote.    |  
| fileStatus    | string    |  O status do pacote. Ele pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>PendingUpload</li><li>Carregado</li><li>PendingDelete</li></ul>    |  
| id    |  string   |  Uma ID que identifica exclusivamente o pacote. Esse valor é fornecido pelo Centro de Desenvolvimento.   |     
| versão    |  string   |  A versão do pacote do aplicativo. Para obter mais informações, consulte [Numeração de versão do pacote](https://msdn.microsoft.com/windows/uwp/publish/package-version-numbering).   |   
| architecture    |  string   |  A arquitetura do pacote (por exemplo, ARM).   |     
| languages    | array    |  Uma matriz de códigos de idioma para os idiomas com suporte do aplicativo. Para obter mais informações, consulte [Idiomas com suporte](https://msdn.microsoft.com/windows/uwp/publish/supported-languages).    |     
| recursos    |  array   |  Uma matriz de recursos necessários pelo pacote. Para obter mais informações sobre recursos, consulte [Declarações de recursos de aplicativos](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations).   |     
| minimumDirectXVersion    |  string   |  A versão mínima do DirectX que é compatível com o pacote do aplicativo. Isso pode ser definido apenas para apps destinados ao Windows 8.x. Para aplicativos destinados a outras versões do sistema operacional, esse valor deve estar presente ao chamar o método [atualizar um envio de aplicativo](update-an-app-submission.md), mas o valor especificado será ignorado. Isso pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | string    |  A RAM mínima necessária para o pacote do aplicativo. Isso pode ser definido apenas para apps destinados ao Windows 8.x. Para aplicativos destinados a outras versões do sistema operacional, esse valor deve estar presente ao chamar o método [atualizar um envio de aplicativo](update-an-app-submission.md), mas o valor especificado será ignorado. Isso pode ter um dos seguintes valores: <ul><li>Nenhum(a)</li><li>Memory2GB</li></ul>   |       
| targetDeviceFamilies    | array    |  Uma matriz de cadeias de caracteres que representam as famílias de dispositivos que o pacote segmenta. Esse valor é usado somente para pacotes destinados ao Windows 10; para pacotes destinados a versões anteriores, esse valor tem o valor **Nenhum**. As seguintes sequências de família de dispositivos atualmente têm suporte para pacotes do Windows 10, onde *{0}* é uma sequência de versão do Windows 10, como 10.0.10240.0, 10.0.10586.0 ou 10.0.14393.0: <ul><li>Versão mínima do Windows.Universal *{0}*</li><li>Versão mínima do Windows.Desktop *{0}*</li><li>Versão mínima do Windows.Mobile *{0}*</li><li>Versão mínima do Windows.Xbox *{0}*</li><li>Versão mínima de Windows.Holographic *{0}*</li></ul>   |    

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
| fallbackSubmissionId    |  string   |  A ID da submissão que será recebido por clientes que não recebem os pacotes de lançamento gradual.   |          

> [!NOTE]
> Os valores *packageRolloutStatus* e *fallbackSubmissionId* são atribuídos pelo Centro de Desenvolvimento e não devem ser definidos pelo desenvolvedor. Se você incluir esses valores no corpo da solicitação, esses valores serão ignorados.

<span id="trailer-object" />

### <a name="trailers-resource"></a>Recurso de trailers

Esse recurso representa um trailer de vídeo para a listagem do app. Os valores nesse recurso correspondem às opções de [trailer](../publish/app-screenshots-and-images.md#trailers) para envios no painel do Centro de Desenvolvimento.

Você pode adicionar até 15 recursos de trailer à matriz *trailers* em um [recurso de envio de aplicativo](#app-submission-object). Para carregar arquivos de vídeo do trailer e imagens em miniatura para um envio, adicione esses arquivos ao mesmo arquivo ZIP que contém os pacotes e as imagens de listagem para o envio e então carregue esse arquivo ZIP no URI da SAS (assinatura de acesso compartilhado) do envio. Para obter mais informações sobre o upload do arquivo ZIP no URI da SAS, consulte [Criar um envio de aplicativo](#create-an-app-submission).

```json
{
  "trailers": [
    {
      "id": "1158943556954955699",
      "videoFileName": "Trailers\\ContosoGameTrailer.mp4",
      "videoFileId": "1159761554639123258",
      "trailerAssets": {
        "en-us": {
          "title": "Contoso Game",
          "imageList": [
            {
              "fileName": "Images\\ContosoGame-Thumbnail.png",
              "id": "1155546904097346923",
              "description": "This is a still image from the video."
            }
          ]
        }
      }
    }
  ]
}
```

Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição        |
|-----------------|---------|------|
|  id               |    string     |   A ID do trailer. Esse valor é fornecido pelo Centro de Desenvolvimento.   |
|  videoFileName               |    string     |    O nome do arquivo de vídeo de trailer no arquivo ZIP que contém arquivos para o envio.    |     
|  videoFileId               |   string      |  A ID do arquivo de vídeo de trailer. Esse valor é fornecido pelo Centro de Desenvolvimento.   |     
|  trailerAssets               |   object      |  Um dicionário de pares de chave e valor, onde cada chave é um código de idioma e cada valor é um [recurso de ativos de trailer](#trailer-assets-object) que contém outros ativos específicos da localidade para o trailer. Para saber mais sobre os códigos de idioma com suporte, consulte [Idiomas com suporte](https://msdn.microsoft.com/windows/uwp/publish/supported-languages).    |     

> [!NOTE]
> O recurso *trailers* foi adicionado em maio de 2017, depois que a API de envio da Microsoft Store foi lançada pela primeira vez para desenvolvedores. Se você tiver criado um envio para um app por meio da API de envio antes da introdução desse recurso e se esse envio ainda estiver em andamento, esse recurso será nulo para envios para o app até que você confirme com êxito o envio ou o exclua. Se o recurso *trailers* não estiver disponível para envios para um app, o campo *hasAdvancedListingPermission* do [recurso Application](get-app-data.md#application_object) retornado pelo método [obter um app](get-an-app.md) será falso.

<span id="trailer-assets-object" />

### <a name="trailer-assets-resource"></a>Recurso de ativos de trailer

Esse recurso contém outros ativos específicos da localidade para um trailer definido em um [recurso de trailer](#trailer-object). Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição        |
|-----------------|---------|------|
| title   |   string      |  O título localizado do trailer. O título é exibido quando o usuário reproduz o trailer em modo de tela inteira.     |  
| imageList    | array    |   Uma matriz que contém um recurso de [imagem](#image-for-trailer-object) que fornece a imagem em miniatura do trailer. Você só pode incluir um recurso de [imagem](#image-for-trailer-object) nessa matriz.  |   


<span id="image-for-trailer-object" />

### <a name="image-resource-for-a-trailer"></a>Recurso de imagem (para um trailer)

Esse recurso descreve a imagem em miniatura para um trailer. Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição           |
|-----------------|---------|------|
|  fileName               |    string     |   O nome do arquivo de imagem em miniatura no arquivo ZIP que você carregou para o envio.    |     
|  id  |  string  | A ID da imagem em miniatura. Esse valor é fornecido pelo Centro de Desenvolvimento.  |
|  description  |  string  | A descrição da imagem em miniatura. Esse valor é composto somente por metadados e não é exibido para os usuários.   |

<span/>

## <a name="enums"></a>Enumerações

Esses métodos usam as enumerações a seguir.

<span id="price-tiers" />

### <a name="price-tiers"></a>Faixas de preço

Os seguintes valores representam as faixas de preço disponíveis no recurso [preço do recurso](#pricing-object) para um envio de app.

| Valor           | Descrição        |
|-----------------|------|
|  Base               |   A faixa de preço não está definida. Use o preço base para o aplicativo.      |     
|  NotAvailable              |   O aplicativo não está disponível na região especificada.    |     
|  Grátis              |   O app é gratuito.    |    
|  Faixa de*xxxx*               |   Uma cadeia de caracteres que especifica a faixa de preço do app, no formato **Faixa<em>xxxx</em>**. No momento, há suporte para os seguintes intervalos de faixas de preço:<br/><br/><ul><li>Se o valor *isAdvancedPricingModel* do [preço do recurso](#pricing-object) for **true**, os valores de nível de preço disponíveis para sua conta são **Tier1012** - **Tier1424**.</li><li>Se o valor *isAdvancedPricingModel* do [preço do recurso](#pricing-object) for **false**, os valores de nível de preço disponíveis para sua conta são **Tier2** - **Tier96**.</li></ul>Para visualizar a tabela completa de faixas de preço disponíveis para a conta de desenvolvedor, incluindo os preços específicos do mercado associados a cada camada, acesse a página **Preço e disponibilidade** de qualquer um dos envios de app no painel do Centro de Desenvolvimento e clique no link **tabela de modo de exibição** na seção **Mercados e preços personalizados** (para algumas contas de desenvolvedor, este link está na seção **Preço**).    |


<span id="enterprise-licensing" />

### <a name="enterprise-licensing-values"></a>Valores de licenciamento da empresa

Os seguintes valores representam o comportamento de licenciamento organizacional do app. Para obter mais informações sobre essas opções, consulte [Opções de licenciamento organizacional](https://msdn.microsoft.com/windows/uwp/publish/organizational-licensing).

> [!NOTE]
> Embora você possa configurar as opções de licenciamento organizacional para um envio de aplicativo por meio da API de envio, você não pode usar essa API para publicar os envios de [compras de volume por meio da Microsoft Store para Empresas e da Microsoft Store para Educação](../publish/organizational-licensing.md). Para publicar envios para a Microsoft Store para Empresas e a Microsoft Store para Educação, você deve usar o painel do Centro de Desenvolvimento do Windows.


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

* [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obter dados de app usando a API de envio da Microsoft Store](get-app-data.md)
* [Envios de aplicativo no painel do Centro de Desenvolvimento](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)
