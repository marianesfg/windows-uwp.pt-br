---
author: normesta
Description: "Este guia ajuda você a capacitar seu aplicativo para manipular dados empresariais gerenciados pela política Proteção de Informações do Windows (WIP), bem como dados pessoais."
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Criar um aplicativo capacitado que consuma dados empresariais e pessoais
translationtype: Human Translation
ms.sourcegitcommit: bf1c47e9cca45b626a45ca664bf2bb4be9c529e0
ms.openlocfilehash: 82b674c72126c66aff34b0396a2c32f88023dd25

---

# Criar um aplicativo capacitado que consuma dados empresariais e pessoais

__Observação__ A política Proteção de Informações do Windows (WIP) pode ser aplicada ao Windows 10, versão 1607.

Um aplicativo *capacitado* diferencia entre dados pessoais e empresariais e sabe o que proteger com base nas políticas Proteção de Informações do Windows (WIP) definidas pelo administrador.

Neste guia, mostraremos como criar uma. Quando você terminar, os administradores de política poderão confiar em seu aplicativo para consumir dados da organização. E os funcionários vão adorar saber que os dados pessoais deles ficarão intactos no dispositivo mesmo se eles cancelarem a inscrição do gerenciamento de dispositivo móvel (MDM) da organização ou saírem completamente da organização.

Você pode ler mais sobre WIP e aplicativos capacitados aqui: [Proteção de Informações do Windows (WIP)](wip-hub.md).

Você pode encontrar um exemplo completo [aqui](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/EnterpriseDataProtection).

Se você estiver pronto para executar cada tarefa, vamos começar.

## Primeiro, coletar o que você precisa

Você precisará do seguinte:

* Uma Máquina Virtual (VM) de teste que executa o Windows 10, versão 1607. Você depurará seu aplicativo em relação a essa VM de teste.

* Um computador de desenvolvimento com o Windows 10, versão 1607. Pode ser a VM de teste caso você tenha o Visual Studio instalado.

## Configurar o ambiente de desenvolvimento

Você executará as seguintes tarefas:

* Instalar o Assistente de desenvolvedor de instalação do WIP na VM de teste.

* Criar uma política de proteção usando o Assistente de desenvolvedor de instalação do WIP.

* Configurar um projeto do Visual Studio.

* Configurar a depuração remota.

* Adicionar namespaces aos seus arquivos de código

**Instalar o Assistente de desenvolvedor de instalação do WIP na VM de teste**

 Use essa ferramenta para configurar uma política de Proteção de Informações do Windows na VM de teste.

 Baixe a ferramenta aqui: [Assistente de desenvolvedor de instalação do WIP](https://www.microsoft.com/store/p/wip-setup-developer-assistant/9nblggh526jf).

**Criar uma política de proteção**

Defina sua política adicionando informações a cada seção do Assistente de desenvolvedor de instalação do WIP. Escolha o ícone de ajuda ao lado de qualquer configuração para saber mais sobre como usá-lo.

Para obter diretrizes gerais sobre como usar essa ferramenta, consulte a seção Notas de versão na página de download do aplicativo.

**Configurar um projeto do Visual Studio**

1. No computador de desenvolvimento, abra seu projeto.

2. Adicione uma referência às extensões de desktop e móveis à Plataforma Universal do Windows (UWP).

    ![Adicionar extensões UWP](images/extensions.png)

3. Adicione esses recursos ao seu arquivo de manifesto do pacote:

    ```xml
       <Capability Name="privateNetworkClientServer" />
       <rescap:Capability Name="enterpriseDataPolicy"/>
    ```
   >*Leitura Opcional*: o prefixo "rescap" significa *Recurso Restrito*. Consulte [Funcionalidades especiais e restritas](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations).

4. Adicione este namespace ao seu arquivo de manifesto do pacote:

    ```xml
      xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    ```
5. Adicione o prefixo de namespace ao elemento ``<ignorableNamespaces>`` do seu arquivo de manifesto do pacote.

    ```xml
        <IgnorableNamespaces="uap mp rescap">
    ```

    Dessa forma, se seu aplicativo for executado em uma versão do sistema operacional Windows que não dá suporte a funcionalidades restritas, o Windows irá ignorar a funcionalidade ``enterpriseDataPolicy``.

**Configurar a depuração remota**

Instale as Ferramentas Remotas para Visual Studio na VM de teste somente se você estiver desenvolvendo seu aplicativo em um computador que não seja a VM. Em seguida, no computador de desenvolvimento, inicie o depurador remoto e veja se seu aplicativo é executado na VM de teste.

Consulte [Instruções para computador remoto](https://msdn.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#remote-pc-instructions).

**Adicionar esses namespaces aos seus arquivos de código**

Adicione estas instruções "using" à parte superior de seus arquivos de código (os trechos de código neste guia as usa):

```csharp
using System.Threading.Tasks;
using Windows.Security.EnterpriseData;
using Windows.ApplicationModel.DataTransfer;
using Windows.Web.Http;
using Windows.Storage.Streams;
using Windows.Web.Http.Filters;
using Windows.Storage;
using Windows.Foundation.Metadata;
using Windows.UI.Xaml.Controls;
using Windows.Data.Xml.Dom;
```

## Determinar se o sistema operacional que executa seu aplicativo dá suporte a WIP

Use a função [**IsApiContractPresent**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.foundation.metadata.apiinformation.isapicontractpresent.aspx) para determinar isso.

```csharp
bool wipSupported = ApiInformation.IsApiContractPresent("Windows.Security.EnterpriseData.EnterpriseDataContract", 3);

if (wipSupported)
{
    // WIP is supported on the platform
}
else
{
    // WIP is not supported on the platform
}
```

A Proteção de Informações do Windows é compatível com o Windows 10, versão 1607.

## Ler dados empresariais

Arquivo, pontos de extremidade de rede, dados de área de transferência e dados que você aceita de um contrato de compartilhamento todos têm uma ID de empresa.

Para ler dados de qualquer uma dessas origens, seu aplicativo precisará confirmar se a ID da empresa é gerenciada pela política.

Vamos começar com arquivos.

### Ler dados de um arquivo

**Etapa 1: Obter o identificador de arquivo**

```csharp
    Windows.Storage.StorageFolder storageFolder =
        Windows.Storage.ApplicationData.Current.LocalFolder;

    Windows.Storage.StorageFile file =
        await storageFolder.GetFileAsync(fileName);
```

**Etapa 2: Determinar se o seu aplicativo pode abrir o arquivo**

Determine se o arquivo está protegido. Se estiver, seu aplicativo poderá abrir esse arquivo se estas duas condições forem verdadeiras:

* A identidade do arquivo é gerenciada pela política.
* Seu aplicativo está na lista de permissões da política.

Se alguma dessas condições não for verdadeira, [**ProtectionPolicyManager.IsIdentityManaged** ](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged.aspx) retornará **false**, e você não poderá abrir esse arquivo.

```csharp
FileProtectionInfo protectionInfo = await FileProtectionManager.GetProtectionInfoAsync(file);

if (protectionInfo.Status == FileProtectionStatus.Protected)
{
    if (!ProtectionPolicyManager.IsIdentityManaged(protectionInfo.Identity))
    {
        return false;
    }
}
else if (protectionInfo.Status == FileProtectionStatus.Revoked)
{
    // Code goes here to handle this situation. Perhaps, show UI
    // saying that the user's data has been revoked.
}
```
> **APIs** <br>
[FileProtectionManager.GetProtectionInfoAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionmanager.getprotectioninfoasync.aspx)<br>
[FileProtectionInfo](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectioninfo.aspx)<br>
[FileProtectionStatus](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionstatus.aspx)<br>
[ProtectionPolicyManager.IsIdentityManaged](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged.aspx)

**Etapa 3: Ler o arquivo em um fluxo ou buffer**

*Ler o arquivo em um fluxo*

```csharp
var stream = await file.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
```

*Ler o arquivo em um buffer*

```csharp
var buffer = await Windows.Storage.FileIO.ReadBufferAsync(file);
```

### Ler dados de um ponto de extremidade de rede

Crie um contexto de thread protegido para ler de um ponto de extremidade empresarial.

**Etapa 1: Obter a identidade do ponto de extremidade de rede**

```csharp
Uri resourceURI = new Uri("http://contoso.com/stockData.xml");

Windows.Networking.HostName hostName =
    new Windows.Networking.HostName(resourceURI.Host);

string identity = await ProtectionPolicyManager.
    GetPrimaryManagedIdentityForNetworkEndpointAsync(hostName);
```

Se o ponto de extremidade não for gerenciado por política, você receberá novamente uma cadeia de caracteres vazia.

> **APIs** <br>
[ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getprimarymanagedidentityfornetworkendpointasync.aspx)


**Etapa 2: Criar um contexto de thread protegido**

Se o ponto de extremidade for gerenciado pela política, crie um contexto de thread protegido. Isso marca qualquer conexão de rede estabelecida no mesmo thread à identidade.

Ele também fornece acesso aos recursos de rede corporativa que são gerenciados por essa política.

```csharp
HttpClient client = null;

if (!string.IsNullOrEmpty(identity))
{
    using (ThreadNetworkContext threadNetworkContext =
            ProtectionPolicyManager.CreateCurrentThreadNetworkContext(identity))
    {
        client = new HttpClient();

        // Add code here to get data from the endpoint.
    }
}
```
Este exemplo inclui chamadas de soquete em um bloco ``using``. Se você não fizer esse procedimento, certifique-se de fechar o contexto de thread depois de recuperar seu recurso. Consulte [ThreadNetworkContext.Close](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.threadnetworkcontext.close.aspx).

Não crie nenhum arquivo pessoal nesse thread protegido porque esses arquivos serão criptografados automaticamente.

O método [**ProtectionPolicyManager.CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext.aspx) retorna um objeto [**ThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.threadnetworkcontext.aspx), esteja o ponto de extremidade sendo gerenciado ou não pela política. Se seu aplicativo manipula recursos pessoais e empresariais, chame [**ProtectionPolicyManager.CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext.aspx) para todas as identidades.  Depois de obter o recurso, descarte a ThreadNetworkContext para limpar todas as marcas de identidade do thread atual.

> **APIs** <br>
[ProtectionPolicyManager.GetForCurrentView](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview.aspx)<br>
[ProtectionPolicyManager.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.aspx)<br>
[ProtectionPolicyManager.CreateCurrentThreadNetworkContext](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext.aspx)

**Etapa 3: Ler o recurso em um buffer**

```csharp
IBuffer data = await client.GetBufferAsync(resourceURI);
```


**Tratar redirecionamentos de página**

Às vezes, um servidor Web redirecionará o tráfego para uma versão mais atual de um recurso.

Para lidar com isso, faça solicitações até que o status de resposta de sua solicitação tenha um valor de **OK**.

Em seguida, use o URI dessa resposta para obter a identidade do ponto de extremidade. Veja uma das maneiras de fazer isso:

```csharp
public static async Task<IBuffer> getDataFromNetworkResource(Uri resourceURI)
{
    bool finalURL = false;

    HttpResponseMessage response = null;

    while (!finalURL)
    {

        Windows.Networking.HostName hostName =
            new Windows.Networking.HostName(resourceURI.Host);

        string identity = await ProtectionPolicyManager.
            GetPrimaryManagedIdentityForNetworkEndpointAsync(hostName);

        HttpClient client = null;

        HttpBaseProtocolFilter filter = new HttpBaseProtocolFilter();
        filter.AllowAutoRedirect = false;

        if (!string.IsNullOrEmpty(m_EnterpriseId))
        {
            using (ThreadNetworkContext threadNetworkContext =
                    ProtectionPolicyManager.CreateCurrentThreadNetworkContext(identity))
            {
                client = new HttpClient(filter);
            }
        }
        else
        {
            client = new HttpClient(filter);
        }
        HttpRequestMessage message = new HttpRequestMessage(HttpMethod.Get, resourceURI);
        response = await client.SendRequestAsync(message);

        if (response.StatusCode == HttpStatusCode.MultipleChoices &&
            response.StatusCode == HttpStatusCode.MovedPermanently &&
            response.StatusCode == HttpStatusCode.Found &&
            response.StatusCode == HttpStatusCode.SeeOther &&
            response.StatusCode == HttpStatusCode.NotModified &&
            response.StatusCode == HttpStatusCode.UseProxy &&
            response.StatusCode == HttpStatusCode.TemporaryRedirect &&
            response.StatusCode == HttpStatusCode.PermanentRedirect)
        {
            resourceURI = message.RequestUri;
        }
        else
        {
            finalURL = true;
        }
    }

    IBuffer data = await response.Content.ReadAsBufferAsync();

    return data;

}
```

> **APIs** <br>
[ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getprimarymanagedidentityfornetworkendpointasync.aspx)<br>
[ProtectionPolicyManager.CreateCurrentThreadNetworkContext](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext.aspx)<br>
[ProtectionPolicyManager.GetForCurrentView](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview.aspx)<br>
[ProtectionPolicyManager.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.aspx)

### Ler os dados da área de transferência

**Obter permissão para usar dados da área de transferência**

Para obter dados da área de transferência, peça permissão ao Windows. Use [**DataPackageView.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706645.aspx) para fazer isso.

```csharp
public static async Task PasteText(TextBox textBox)
{
    DataPackageView dataPackageView = Clipboard.GetContent();

    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        ProtectionPolicyEvaluationResult result = await dataPackageView.RequestAccessAsync();

        if (result == ProtectionPolicyEvaluationResult..Allowed)
        {
            string contentsOfClipboard = await dataPackageView.GetTextAsync();
            textBox.Text = contentsOfClipboard;
        }
    }
}
```

> **APIs** <br>
[DataPackageView.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/dn706645.aspx)

**Ocultar ou desabilitar os recursos que usam dados da área de transferência**

Determine se a exibição atual tem permissão para acessar dados que estão na área de transferência.

Se isso não tiver, você poderá desabilitar ou ocultar os controles que permitem aos usuários colar informações da área de transferência ou visualizar seu conteúdo.

```csharp
private bool IsClipboardAllowedAsync()
{
    ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult = ProtectionPolicyEvaluationResult.Blocked;

    DataPackageView dataPackageView = Clipboard.GetContent();

    if (dataPackageView.Contains(StandardDataFormats.Text))

        protectionPolicyEvaluationResult =
            ProtectionPolicyManager.CheckAccess(dataPackageView.Properties.EnterpriseId,
                ProtectionPolicyManager.GetForCurrentView().Identity);

    return (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed |
        protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.ConsentRequired);
}
```

> **APIs** <br>
[ProtectionPolicyEvaluationResult](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicyevaluationresult.aspx)<br>
[ProtectionPolicyManager.GetForCurrentView](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview.aspx)<br>
[ProtectionPolicyManager.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.aspx)

**Impedir que os usuários sejam avisados com uma caixa de diálogo de consentimento**

Um novo documento não é *pessoal* nem *empresarial*. É apenas novo. Se um usuário colar os dados empresariais nele, o Windows aplicará a política e o usuário será avisado com uma caixa de diálogo de consentimento. Esse código impede que isso ocorra. Essa tarefa não pretende ajudar a proteger os dados. É mais sobre como impedir que os usuários recebam a caixa de diálogo de consentimento em casos em que seu aplicativo cria um novo item.

```csharp
private async void PasteText(bool isNewEmptyDocument)
{
    DataPackageView dataPackageView = Clipboard.GetContent();

    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        if (!string.IsNullOrEmpty(dataPackageView.Properties.EnterpriseId))
        {
            if (isNewEmptyDocument)
            {
                ProtectionPolicyManager.TryApplyProcessUIPolicy(dataPackageView.Properties.EnterpriseId);
                string contentsOfClipboard = contentsOfClipboard = await dataPackageView.GetTextAsync();
                // add this string to the new item or document here.          

            }
            else
            {
                ProtectionPolicyEvaluationResult result = await dataPackageView.RequestAccessAsync();

                if (result == ProtectionPolicyEvaluationResult.Allowed)
                {
                    string contentsOfClipboard = contentsOfClipboard = await dataPackageView.GetTextAsync();
                    // add this string to the new item or document here.
                }
            }
        }
    }
}
```

> **APIs** <br>
[DataPackageView.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/dn706645.aspx)<br>
[ProtectionPolicyEvaluationResult](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicyevaluationresult.aspx)<br>
[ProtectionPolicyManager.TryApplyProcessUIPolicy](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.tryapplyprocessuipolicy.aspx)


### Ler dados de um contrato de compartilhamento

Quando os funcionários escolhem seu aplicativo para compartilhar suas informações, seu aplicativo abre um novo item que contém esse conteúdo.

Como mencionamos anteriormente, um novo item não é *pessoal* nem *empresarial*. É apenas novo. Se seu código adicionar conteúdo empresarial ao item, o Windows aplicará a política e o usuário será avisado com uma caixa de diálogo de consentimento. Esse código impede que isso ocorra.

```csharp
protected override async void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    bool isNewEmptyDocument = true;
    string identity = "corp.microsoft.com";

    ShareOperation shareOperation = args.ShareOperation;
    if (shareOperation.Data.Contains(StandardDataFormats.Text))
    {
        if (!string.IsNullOrEmpty(shareOperation.Data.Properties.EnterpriseId))
        {
            if (isNewEmptyDocument)
                // If this is a new and empty document, and we're allowed to access
                // the data, then we can avoid popping the consent dialog
                ProtectionPolicyManager.TryApplyProcessUIPolicy(shareOperation.Data.Properties.EnterpriseId);
            else
            {
                // In this case, we can't optimize the workflow, so we just
                // request consent from the user in this case.

                ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult = await shareOperation.Data.RequestAccessAsync();

                if (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed)
                {
                    string text = await shareOperation.Data.GetTextAsync();

                    // Do something with that text.
                }
            }
        }
        else
        {
            // If the data has no enterprise identity, then we already have access.
            string text = await shareOperation.Data.GetTextAsync();

            // Do something with that text.
        }

    }

}
```

> **APIs** <br>
[ProtectionPolicyManager.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/dn705789.aspx)<br>
[ProtectionPolicyEvaluationResult](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicyevaluationresult.aspx)<br>
[ProtectionPolicyManager.TryApplyProcessUIPolicy](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.tryapplyprocessuipolicy.aspx)

## Proteger dados empresariais

Protege dados empresariais que deixam o aplicativo. Os dados deixam seu aplicativo quando você os mostra em uma página, os salva em um arquivo ou um ponto de extremidade de rede ou por meio de um contrato de compartilhamento.

### <a id="display-data"></a>Proteger dados que aparecem nas páginas

Quando você mostra dados em uma página, informe ao Windows qual é o tipo dos dados (pessoais ou empresariais). Para fazer isso, *marque* a exibição atual do aplicativo ou todo o processo do aplicativo.

Quando você marca a exibição ou o processo, o Windows impõe a política nele. Isso ajuda a evitar a perda de dados resultante de ações que seu aplicativo não controla. Por exemplo, em um computador, um usuário pode usar CTRL-V para copiar informações corporativas de uma exibição e, em seguida, colá-las em outro aplicativo. O Windows protege contra isso. O Windows também ajuda a impor contratos de compartilhamento.

**Marcar a exibição atual do aplicativo**

Faça isso se seu aplicativo tiver várias exibições entre as quais algumas consomem dados empresariais e outras, dados pessoais.

```csharp

// tag as enterprise data. "identity" the string that contains the enterprise ID.
// You'd get that from a file, network endpoint, or clipboard data package.
ProtectionPolicyManager.GetForCurrentView().Identity = identity;

// tag as personal data.
ProtectionPolicyManager.GetForCurrentView().Identity = String.Empty();
```

> **APIs** <br>
[ProtectionPolicyManager.GetForCurrentView](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview.aspx)<br>
[ProtectionPolicyManager.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.aspx)

**Marcar o processo**

Faça isso se todas as exibições do aplicativo trabalharem apenas com um tipo de dados (pessoais ou empresariais).

Isso evita a necessidade de gerenciar exibições marcadas de forma independente.

```csharp


// tag as enterprise data. "identity" the string that contains the enterprise ID.
// You'd get that from a file, network endpoint, or clipboard data package.
bool result =
            ProtectionPolicyManager.TryApplyProcessUIPolicy(identity);

// tag as personal data.
bool result =
            ProtectionPolicyManager.TryApplyProcessUIPolicy(String.Empty());
```

> **APIs** <br>
[ProtectionPolicyManager.TryApplyProcessUIPolicy](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.tryapplyprocessuipolicy.aspx)

### Proteger dados em um arquivo

Crie um arquivo protegido e, em seguida, grave-o.

**Etapa 1: Determinar se o seu aplicativo pode criar um arquivo empresarial**

Seu aplicativo pode criar um arquivo empresarial se a cadeia de caracteres de identidade for gerenciada por política e seu aplicativo estiver na lista de permissões da política.

```csharp
  if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;
```

> **APIs** <br>
[ProtectionPolicyManager.IsIdentityManaged](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged.aspx)


**Etapa 2: Criar o arquivo e protegê-lo para a identidade**

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
StorageFile storageFile = await storageFolder.CreateFileAsync("sample.txt",
    CreationCollisionOption.ReplaceExisting);

FileProtectionInfo fileProtectionInfo =
    await FileProtectionManager.ProtectAsync(storageFile, identity);
```

> **APIs** <br>
[FileProtectionManager.ProtectAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionmanager.protectasync.aspx)

**Etapa 3: Gravar esse fluxo ou buffer no arquivo**

*Gravar um fluxo*

```csharp
    if (fileProtectionInfo.Identity == identity &&
        fileProtectionInfo.Status == FileProtectionStatus.Protected)
    {
        var stream = await storageFile.OpenAsync(FileAccessMode.ReadWrite);

        using (var outputStream = stream.GetOutputStreamAt(0))
        {
            using (var dataWriter = new DataWriter(outputStream))
            {
                dataWriter.WriteString(enterpriseData);
            }
        }

    }
```

*Gravar um buffer*

```csharp
     if (fileProtectionInfo.Identity == identity &&
         fileProtectionInfo.Status == FileProtectionStatus.Protected)
     {
         var buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
             enterpriseData, Windows.Security.Cryptography.BinaryStringEncoding.Utf8);

         await FileIO.WriteBufferAsync(storageFile, buffer);

      }
```

> **APIs** <br>
[FileProtectionInfo](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectioninfo.aspx)<br>
[FileProtectionStatus](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionstatus.aspx)<br>



### Proteger dados em um arquivo como um processo em segundo plano

Esse código pode ser executado enquanto a tela do dispositivo está bloqueada. Se o administrador configurou uma política "Proteção de dados sob bloqueio" (DPL), o Windows removerá as chaves de criptografia necessárias para acessar recursos protegidos na memória do dispositivo. Isso impede vazamentos de dados se o dispositivo for perdido. O mesmo recurso também remove chaves associadas a arquivos protegidos quando seus identificadores são fechados.

Você precisará usar uma abordagem que mantém o identificador de arquivo aberto ao criar um arquivo.  

**Etapa 1: Determinar se você pode criar um arquivo empresarial**

Você poderá criar um arquivo empresarial se a identidade que estiver usando for gerenciada por política e seu aplicativo estiver na lista de permissões da política.

```csharp
if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;
```

> **APIs** <br>
[ProtectionPolicyManager.IsIdentityManaged](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged.aspx)

**Etapa 2: Criar um arquivo e protegê-lo para a identidade**

O [**FileProtectionManager.CreateProtectedAndOpenAsync**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionmanager.createprotectedandopenasync.aspx) cria um arquivo protegido e mantém o identificador de arquivo aberto enquanto você grava nele.

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;

ProtectedFileCreateResult protectedFileCreateResult =
    await FileProtectionManager.CreateProtectedAndOpenAsync(storageFolder,
        "sample.txt", identity, CreationCollisionOption.ReplaceExisting);
```

> **APIs** <br>
[FileProtectionManager.CreateProtectedAndOpenAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionmanager.createprotectedandopenasync.aspx)

**Etapa 3: Gravar um fluxo ou um buffer no arquivo**

Este exemplo grava um fluxo em um arquivo.

```csharp
if (protectedFileCreateResult.ProtectionInfo.Identity == identity &&
    protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.Protected)
{
    IOutputStream outputStream =
        protectedFileCreateResult.Stream.GetOutputStreamAt(0);

    using (DataWriter writer = new DataWriter(outputStream))
    {
        writer.WriteString(enterpriseData);
        await writer.StoreAsync();
        await writer.FlushAsync();
    }

    outputStream.Dispose();
}
else if (protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.AccessSuspended)
{
    // Perform any special processing for the access suspended case.
}

```

> **APIs** <br>
[ProtectedFileCreateResult.ProtectionInfo](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectedfilecreateresult.protectioninfo.aspx)<br>
[FileProtectionStatus](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionstatus.aspx)<br>
[ProtectedFileCreateResult.Stream](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectedfilecreateresult.stream.aspx)<br>

### Proteger parte de um arquivo

Na maioria dos casos, é melhor armazenar dados pessoais e empresariais separadamente, mas você pode armazená-los no mesmo arquivo, se quiser. Por exemplo, o Microsoft Outlook pode armazenar emails empresariais juntamente com emails pessoais em um único arquivo morto.

Criptografe os dados empresariais, mas não todo o arquivo. Dessa forma, os usuários poderão continuar a usar esse arquivo mesmo se cancelarem o registro do MDM ou seus direitos de acesso a dados empresariais forem revogados. Além disso, seu aplicativo deve manter o controle de quais dados ele criptografa para que saiba quais dados proteger ao ler o arquivo na memória.

**Etapa 1: Adicionar dados empresariais a um fluxo criptografado ou buffer**

```csharp
string enterpriseDataString = "<employees><employee><name>Bill</name><social>xxx-xxx-xxxx</social></employee></employees>";

var enterpriseData= Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
        enterpriseDataString, Windows.Security.Cryptography.BinaryStringEncoding.Utf8);

BufferProtectUnprotectResult result =
   await DataProtectionManager.ProtectAsync(enterpriseData, identity);

enterpriseData= result.Buffer;
```

> **APIs** <br>
[DataProtectionManager.ProtectAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.dataprotectionmanager.protectasync.aspx)<br>
[BufferProtectUnprotectResult.buffer](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.bufferprotectunprotectresult.buffer.aspx)


**Etapa 2: Adicionar dados pessoais a um fluxo não criptografado ou buffer**

```csharp
string personalDataString = "<recipies><recipe><name>BillsCupCakes</name><cooktime>30</cooktime></recipe></recipies>";

var personalData = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
    personalDataString, Windows.Security.Cryptography.BinaryStringEncoding.Utf8);
```

**Etapa 3: Gravar fluxos ou buffers em um arquivo**

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;

StorageFile storageFile = await storageFolder.CreateFileAsync("data.xml",
    CreationCollisionOption.ReplaceExisting);

 // Write both buffers to the file and save the file.

var stream = await storageFile.OpenAsync(FileAccessMode.ReadWrite);

using (var outputStream = stream.GetOutputStreamAt(0))
{
    using (var dataWriter = new DataWriter(outputStream))
    {
        dataWriter.WriteBuffer(enterpriseData);
        dataWriter.WriteBuffer(personalData);

        await dataWriter.StoreAsync();
        await outputStream.FlushAsync();
    }
}
```

**Etapa 4: Manter controle da localização dos dados empresariais no arquivo**

É responsabilidade do seu aplicativo manter o controle dos dados que são empresariais nesse arquivo.

Você pode armazenar essas informações em uma propriedade associada ao arquivo, em um banco de dados ou em algum texto de cabeçalho no arquivo.

Neste exemplo, essas informações são salvas em um arquivo XML separado.

```csharp
StorageFile metaDataFile = await storageFolder.CreateFileAsync("metadata.xml",
   CreationCollisionOption.ReplaceExisting);

await Windows.Storage.FileIO.WriteTextAsync
    (metaDataFile, "<EnterpriseDataMarker start='0' end='" + enterpriseData.Length.ToString() +
    "'></EnterpriseDataMarker>");
```

### Ler a parte protegida de um arquivo

Veja a seguir como você lerá os dados empresariais fora desse arquivo.

**Etapa 1: Obter a posição de seus dados empresariais no arquivo**

```csharp
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;

 Windows.Storage.StorageFile metaDataFile =
   await storageFolder.GetFileAsync("metadata.xml");

string metaData = await Windows.Storage.FileIO.ReadTextAsync(metaDataFile);

XmlDocument doc = new XmlDocument();

doc.LoadXml(metaData);

uint startPosition =
    Convert.ToUInt16((doc.FirstChild.Attributes.GetNamedItem("start")).InnerText);

uint endPosition =
    Convert.ToUInt16((doc.FirstChild.Attributes.GetNamedItem("end")).InnerText);
```

**Etapa 2: Abrir o arquivo de dados e certificar-se de que ele não esteja protegido**

```csharp
Windows.Storage.StorageFile dataFile =
    await storageFolder.GetFileAsync("data.xml");

FileProtectionInfo protectionInfo =
    await FileProtectionManager.GetProtectionInfoAsync(dataFile);

if (protectionInfo.Status == FileProtectionStatus.Protected)
    return false;
```

> **APIs** <br>
[FileProtectionManager.GetProtectionInfoAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionmanager.getprotectioninfoasync.aspx)<br>
[FileProtectionInfo](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectioninfo.aspx)<br>
[FileProtectionStatus](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionstatus.aspx)<br>

**Etapa 3: Ler dados empresariais do arquivo**

```csharp
var stream = await dataFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);

stream.Seek(startPosition);

Windows.Storage.Streams.Buffer tempBuffer = new Windows.Storage.Streams.Buffer(50000);

IBuffer enterpriseData = await stream.ReadAsync(tempBuffer, endPosition, InputStreamOptions.None);
```

**Etapa 4: Descriptografar o buffer que contém dados empresariais**

```csharp
DataProtectionInfo dataProtectionInfo =
   await DataProtectionManager.GetProtectionInfoAsync(enterpriseData);

if (dataProtectionInfo.Status == DataProtectionStatus.Protected)
{
    BufferProtectUnprotectResult result = await DataProtectionManager.UnprotectAsync(enterpriseData);
    enterpriseData = result.Buffer;
}
else if (dataProtectionInfo.Status == DataProtectionStatus.Revoked)
{
    // Code goes here to handle this situation. Perhaps, show UI
    // saying that the user's data has been revoked.
}

```

> **APIs** <br>
[DataProtectionInfo](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.dataprotectioninfo.aspx)<br>
[DataProtectionManager.GetProtectionInfoAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.dataprotectionmanager.getstreamprotectioninfoasync.aspx)<br>


### Proteger dados em uma pasta

Você pode criar uma pasta e protegê-la. Dessa forma todos os itens que você adicionar à pasta serão automaticamente protegidos.

```csharp
private async Task<bool> CreateANewFolderAndProtectItAsync(string folderName, string identity)
{
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
    StorageFolder newStorageFolder =
        await storageFolder.CreateFolderAsync(folderName);

    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.ProtectAsync(newStorageFolder, identity);

    if (fileProtectionInfo.Identity != identity ||
        fileProtectionInfo.Status != FileProtectionStatus.Protected)
    {
        // Protection failed.
        return false;
    }
    return true;
}
```

Certifique-se de que a pasta esteja vazia antes protegê-la. Você não pode proteger uma pasta que já contenha itens.

> **APIs** <br>
[ProtectionPolicyManager.IsIdentityManaged](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged.aspx)<br>
[FileProtectionManager.ProtectAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionmanager.protectasync.aspx)<br>
[FileProtectionInfo.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectioninfo.identity.aspx)<br>
[FileProtectionInfo.Status](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectioninfo.status.aspx)


### Proteger dados para um ponto de extremidade de rede

Crie um contexto de thread protegido para enviar esses dados a um ponto de extremidade empresarial.  

**Etapa 1: Obter a identidade do ponto de extremidade de rede**

```csharp
Windows.Networking.HostName hostName =
    new Windows.Networking.HostName(resourceURI.Host);

string identity = await ProtectionPolicyManager.
    GetPrimaryManagedIdentityForNetworkEndpointAsync(hostName);
```

> **APIs** <br>
[ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getprimarymanagedidentityfornetworkendpointasync.aspx)

**Etapa 2: Criar um contexto de thread protegido e enviar dados para o ponto de extremidade de rede**

```csharp
HttpClient client = null;

if (!string.IsNullOrEmpty(m_EnterpriseId))
{
    ProtectionPolicyManager.GetForCurrentView().Identity = identity;

    using (ThreadNetworkContext threadNetworkContext =
            ProtectionPolicyManager.CreateCurrentThreadNetworkContext(identity))
    {
        client = new HttpClient();
        HttpRequestMessage message = new HttpRequestMessage(HttpMethod.Put, resourceURI);
        message.Content = new HttpStreamContent(dataToWrite);

        HttpResponseMessage response = await client.SendRequestAsync(message);

        if (response.StatusCode == HttpStatusCode.Ok)
            return true;
        else
            return false;
    }
}
else
{
    return false;
}
```

> **APIs** <br>
[ProtectionPolicyManager.GetForCurrentView](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview.aspx)<br>
[ProtectionPolicyManager.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.aspx)<br>
[ProtectionPolicyManager.CreateCurrentThreadNetworkContext](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext.aspx)

### Proteger dados que seu aplicativo compartilha por meio de um contrato de compartilhamento

Se você quiser que os usuários compartilhem conteúdo do seu aplicativo, precisará implementar um contrato de compartilhamento e manipular o evento [**DataTransferManager.DataRequested**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.datarequested).

No manipulador de eventos, defina o contexto de identidade empresarial no pacote de dados.

```csharp
private void OnShareSourceOperation(object sender, RoutedEventArgs e)
{
    // Register the current page as a share source (or you could do this earlier in your app).
    DataTransferManager.GetForCurrentView().DataRequested += OnDataRequested;
    DataTransferManager.ShowShareUI();
}

private void OnDataRequested(DataTransferManager sender, DataRequestedEventArgs args)
{
    if (!string.IsNullOrEmpty(this.shareSourceContent))
    {
        var protectionPolicyManager = ProtectionPolicyManager.GetForCurrentView();
        DataPackage requestData = args.Request.Data;
        requestData.Properties.Title = this.shareSourceTitle;
        requestData.Properties.EnterpriseId = protectionPolicyManager.Identity;
        requestData.SetText(this.shareSourceContent);
    }
}
```

> **APIs** <br>
[ProtectionPolicyManager.GetForCurrentView](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview.aspx)<br>
[ProtectionPolicyManager.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.aspx)


### Proteger arquivos que você copia em outro local

```csharp
private async void CopyProtectionFromOneFileToAnother
    (StorageFile sourceStorageFile, StorageFile targetStorageFile)
{
    bool copyResult = await
        FileProtectionManager.CopyProtectionAsync(sourceStorageFile, targetStorageFile);

    if (!copyResult)
    {
        // Copying failed. To diagnose, you could check the file's status.
        // (call FileProtectionManager.GetProtectionInfoAsync and
        // check FileProtectionInfo.Status).
    }
}
```

> **APIs** <br>
[FileProtectionManager.CopyProtectionAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.fileprotectionmanager.copyprotectionasync.aspx)<br>


### Proteger dados corporativos quando a tela do dispositivo está bloqueada

Remova todos os dados confidenciais na memória quando o dispositivo estiver bloqueado. Quando o usuário desbloqueia o dispositivo, seu aplicativo pode adicionar novamente esses dados com segurança.

Manipule o evento [**ProtectionPolicyManager.ProtectedAccessSuspending**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.protectedaccesssuspending.aspx) para que seu aplicativo saiba quando a tela estiver bloqueada. Esse evento será acionado somente se o administrador configurar uma proteção de dados seguros sob a política de bloqueio. O Windows remove temporariamente as chaves de proteção de dados provisionadas no dispositivo. O Windows remove essas chaves para garantir que não haja acesso não autorizado a dados criptografados enquanto o dispositivo está bloqueado e não está em poder de seu proprietário.  

Manipule o evento [**ProtectionPolicyManager.ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.protectedaccessresumed.aspx) para que seu aplicativo saiba quando a tela está desbloqueada. Esse evento é acionado, independentemente de o administrador configurar ou não uma proteção de dados seguros sob política de bloqueio.

#### Remover dados confidenciais na memória quando a tela está bloqueada

Proteja dados confidenciais e feche todos os fluxos de arquivo que seu aplicativo tiver aberto em arquivos protegidos para ajudar a garantir que o sistema não armazena em cache dados confidenciais na memória.

Este exemplo salva o conteúdo de um bloco de texto em um buffer criptografado e remove o conteúdo desse bloco de texto.

```csharp
private async void ProtectionPolicyManager_ProtectedAccessSuspending(object sender, ProtectedAccessSuspendingEventArgs e)
{
    Deferral deferral = e.GetDeferral();

    if (ProtectionPolicyManager.GetForCurrentView().Identity != String.Empty)
    {
        IBuffer documentBodyBuffer = CryptographicBuffer.ConvertStringToBinary
           (documentTextBlock.Text, BinaryStringEncoding.Utf8);

        BufferProtectUnprotectResult result = await DataProtectionManager.ProtectAsync
            (documentBodyBuffer, ProtectionPolicyManager.GetForCurrentView().Identity);

        if (result.ProtectionInfo.Status == DataProtectionStatus.Protected)
        {
            this.protectedDocumentBuffer = result.Buffer;
            documentTextBlock.Text = null;
        }
    }

    // Close any open streams that you are actively working with
    // to make sure that we have no unprotected content in memory.

    // Optionally, code goes here to use e.Deadline to determine whether we have more
    // than 15 seconds left before the suspension deadline. If we do then process any
    // messages queued up for sending while we are still able to access them.

    deferral.Complete();
}
```

> **APIs** <br>
[ProtectionPolicyManager.ProtectedAccessSuspending](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.protectedaccesssuspending.aspx)<br>
[ProtectionPolicyManager.GetForCurrentView](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview.aspx)<br>
[ProtectionPolicyManager.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.aspx)</br>
[DataProtectionManager.ProtectAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.dataprotectionmanager.protectasync.aspx)<br>
[BufferProtectUnprotectResult.buffer](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.bufferprotectunprotectresult.buffer.aspx)<br>
[ProtectedAccessSuspendingEventArgs.GetDeferral](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectedaccesssuspendingeventargs.getdeferral.aspx)<br>
[Deferral.Complete](https://msdn.microsoft.com/library/windows/apps/windows.foundation.deferral.complete.aspx)<br>

#### Adicionar dados confidenciais novamente quando o dispositivo for desbloqueado

[**ProtectionPolicyManager.ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.protectedaccessresumed.aspx) é acionado quando o dispositivo é desbloqueado e as chaves estão disponíveis novamente no dispositivo.

[**ProtectedAccessResumedEventArgs.Identities**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectedaccessresumedeventargs.identities.aspx) será uma coleção vazia se o administrador não tiver configurado uma proteção de dados seguros sob a política de bloqueio.

Este exemplo faz o inverso do exemplo anterior. Descriptografa o buffer, adiciona as informações desse buffer de volta para a caixa de texto e, em seguida, descarta o buffer.

```csharp
private async void ProtectionPolicyManager_ProtectedAccessResumed(object sender, ProtectedAccessResumedEventArgs e)
{
    if (ProtectionPolicyManager.GetForCurrentView().Identity != String.Empty)
    {
        BufferProtectUnprotectResult result = await DataProtectionManager.UnprotectAsync
            (this.protectedDocumentBuffer);

        if (result.ProtectionInfo.Status == DataProtectionStatus.Unprotected)
        {
            // Restore the unprotected version.
            documentTextBlock.Text = CryptographicBuffer.ConvertBinaryToString
                (BinaryStringEncoding.Utf8, result.Buffer);
            this.protectedDocumentBuffer = null;
        }
    }

}
```

> **APIs** <br>
[ProtectionPolicyManager.ProtectedAccessResumed](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.protectedaccessresumed.aspx)<br>
[ProtectionPolicyManager.GetForCurrentView](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview.aspx)<br>
[ProtectionPolicyManager.Identity](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.aspx)</br>
[DataProtectionManager.UnprotectAsync](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.dataprotectionmanager.unprotectasync.aspx)<br>
[BufferProtectUnprotectResult.Status](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.bufferprotectunprotectresult.aspx)<br>

## Manipular dados empresariais quando o conteúdo protegido for revogado

Se você deseja que seu aplicativo seja notificado quando a inscrição do dispositivo é cancelada do MDM ou quando o administrador de política revoga explicitamente o acesso aos dados empresariais, manipule o evento [**ProtectionPolicyManager_ProtectedContentRevoked**](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.protectedcontentrevoked.aspx).

Este exemplo determina se os dados em uma caixa de correio empresarial para um aplicativo de email foram revogados.

```csharp
private string mailIdentity = "contoso.com";

void MailAppSetup()
{
    ProtectionPolicyManager.ProtectedContentRevoked += ProtectionPolicyManager_ProtectedContentRevoked;
    // Code goes here to set up mailbox for 'mailIdentity'.
}

private void ProtectionPolicyManager_ProtectedContentRevoked(object sender, ProtectedContentRevokedEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.mailIdentity))
    {
        // This event is not for our identity.
        return;
    }

    // Code goes here to delete any metadata associated with 'mailIdentity'.
}
```

> **APIs** <br>
[ProtectionPolicyManager_ProtectedContentRevoked](https://msdn.microsoft.com/library/windows/apps/windows.security.enterprisedata.protectionpolicymanager.protectedcontentrevoked.aspx)<br>

## Tópicos relacionados

[Exemplo de Proteção de Informações do Windows (WIP)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)
 

 



<!--HONumber=Nov16_HO1-->


