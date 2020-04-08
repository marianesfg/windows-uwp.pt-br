---
Description: Saiba como conceder identidade a aplicativos de área de trabalho não empacotados para usar recursos modernos do Windows 10 nesses aplicativos.
title: Conceder identidade a aplicativos da área de trabalho não empacotados
ms.date: 02/28/2020
ms.topic: article
keywords: Windows 10, área de trabalho, pacote, identidade, MSIX, Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6b77cc7b2f39a987df4c832f7a8daeb7e2722def
ms.sourcegitcommit: f2f61a43f5bc24b829e8db679ffaca3e663c00e9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80588714"
---
# <a name="grant-identity-to-non-packaged-desktop-apps"></a>Conceder identidade a aplicativos da área de trabalho não empacotados

<!--
> [!NOTE]
> The features described in this article require Windows 10 Insider Preview Build 10.0.19000.0 or a later release.
-->

Muitos recursos de extensibilidade do Windows 10 exigem que o [identificador de pacote](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) seja usado de aplicativos da área de trabalho que não são UWP, incluindo tarefas em segundo plano, notificações, blocos dinâmicos e destinos de compartilhamento. Para esses cenários, o sistema operacional requer identidade para que possa identificar o chamador da API correspondente.

Em versões do sistema operacional antes Windows 10 Insider Preview Build 10.0.19000.0, a única maneira de conceder identidade a um aplicativo da área de trabalho era [empacotá-lo em um pacote MSIX assinado](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root). Para esses aplicativos, a identidade é especificada no manifesto do pacote e o registro de identidade é realizado pelo pipeline de implantação do MSIX com base nas informações no manifesto. Todo o conteúdo referenciado no manifesto do pacote está presente no pacote MSIX.

Começando no Windows 10 Insider Preview Build 10.0.19000.0, você pode conceder um identificador de pacote a aplicativos da área de trabalho que não são empacotados em um pacote MSIX criando e registrando um *pacote esparso* com o aplicativo. Esse suporte permite que os aplicativos da área de trabalho que ainda não são capazes de adotar o empacotamento MSIX para implantação usem recursos de extensibilidade do Windows 10 que exigem o identificador de pacote. Para obter mais informações de contexto, confira [esta postagem de blog](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97).

Para criar e registrar um pacote esparso que concede um identificador de pacote ao seu aplicativo da área de trabalho, siga estas etapas.

1. [Criar um manifesto do pacote para o pacote esparso](#create-a-package-manifest-for-the-sparse-package)
2. [Compilar e assinar o pacote esparso](#build-and-sign-the-sparse-package)
3. [Adicionar os metadados do identificador de pacote ao manifesto do aplicativo da área de trabalho](#add-the-package-identity-metadata-to-your-desktop-application-manifest)
4. [Registrar o pacote esparso no runtime](#register-your-sparse-package-at-run-time)

## <a name="important-concepts"></a>Conceitos importantes

Os recursos a seguir permitem que aplicativos da área de trabalho não empacotados adquiram um identificador de pacote.

### <a name="sparse-packages"></a>Pacotes esparsos

Um *pacote esparso* contém um manifesto do pacote, mas nenhum outro conteúdo nem binários de aplicativo. O manifesto de um pacote esparso pode referenciar arquivos fora do pacote em uma localização externa predeterminada. Isso permite que aplicativos que ainda não são capazes de adotar o empacotamento MSIX para o aplicativo inteiro adquiram o identificador de pacote, conforme exigido por alguns recursos de extensibilidade do Windows 10.

> [!NOTE]
> Um aplicativo da área de trabalho que usa um pacote esparso não recebe alguns dos benefícios de ser totalmente implantado por um pacote MSIX. Esses benefícios incluem proteção contra adulterações, instalação em uma localização bloqueada e gerenciamento completo pelo sistema operacional na implantação, no runtime e na desinstalação.

### <a name="package-external-location"></a>Localização externa do pacote

Para dar suporte a pacotes esparsos, o esquema de manifesto do pacote agora dá suporte a um elemento **\<AllowExternalContent\>** opcional sob o elemento [ **\<Properties\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties). Isso permite que o manifesto do pacote faça referência ao conteúdo fora do pacote, em uma localização específica no disco.

Por exemplo, se tiver seu aplicativo da área de trabalho não empacotado existente que instala o executável do aplicativo e outro conteúdo em C:\Arquivos de Programas\MyDesktopApp\,, você poderá criar um pacote esparso que inclui o elemento **\<AllowExternalContent\>** no manifesto. Durante o processo de instalação do aplicativo ou na primeira vez em que ele é usado, você pode instalar o pacote esparso e declarar C:\Arquivos de Programas\MyDesktopApp\ como a localização externa que o aplicativo usará.

## <a name="create-a-package-manifest-for-the-sparse-package"></a>Criar um manifesto do pacote para o pacote esparso

Antes de compilar um pacote esparso, primeiro você precisa criar um [manifesto do pacote](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest) (um arquivo chamado AppxManifest.xml) que declara os metadados do identificador de pacote para seu aplicativo da área de trabalho e outros detalhes necessários. A maneira mais fácil de criar um manifesto do pacote para o pacote esparso é usar o exemplo abaixo e personalizá-lo para seu aplicativo usando a [referência de esquema](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root).

Verifique se o manifesto do pacote inclui estes itens:

* Um elemento [ **\<Identity\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) que descreve os atributos de identidade de seu aplicativo da área de trabalho.
* Um elemento **\<AllowExternalContent\>** sob o elemento [ **\<Properties\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties). Esse elemento deve receber o valor `true`, que permite que o manifesto do pacote faça referência ao conteúdo fora do pacote, em uma localização específica no disco. Em uma etapa posterior, você especificará o caminho da localização externa ao registrar o pacote esparso no código que é executado em seu instalador ou aplicativo. Qualquer conteúdo que você referencia no manifesto que não está localizado no pacote em si deve ser instalado na localização externa.
* O atributo **MinVersion** do elemento [ **\<TargetDeviceFamily\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) deve ser definido como `10.0.19000.0` ou uma versão posterior.
* Os atributos **TrustLevel=mediumIL** e **RuntimeBehavior=Win32App** do elemento [ **\<Application\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) declaram que o aplicativo da área de trabalho associado ao pacote esparso será executado de maneira semelhante a um aplicativo da área de trabalho não empacotado padrão, sem a virtualização do registro e do sistema de arquivos e outras alterações de runtime.

O exemplo a seguir mostra o conteúdo completo de um manifesto do pacote esparso (AppxManifest.xml). Esse manifesto inclui uma extensão `windows.sharetarget`, que requer o identificador de pacote.

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package 
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10"
  IgnorableNamespaces="uap uap2 uap3 rescap desktop uap10">
  <Identity Name="ContosoPhotoStore" ProcessorArchitecture="x64" Publisher="CN=Contoso" Version="1.0.0.0" />
  <Properties>
    <DisplayName>ContosoPhotoStore</DisplayName>
    <PublisherDisplayName>Contoso</PublisherDisplayName>
    <Logo>Assets\storelogo.png</Logo>
    <uap10:AllowExternalContent>true</uap10:AllowExternalContent>
  </Properties>
  <Resources>
    <Resource Language="en-us" />
  </Resources>
  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.19000.0" MaxVersionTested="10.0.19000.0" />
  </Dependencies>
  <Capabilities>
    <rescap:Capability Name="runFullTrust" />
    <rescap:Capability Name="unvirtualizedResources"/>
  </Capabilities>
  <Applications>
    <Application Id="ContosoPhotoStore" Executable="ContosoPhotoStore.exe" uap10:TrustLevel="mediumIL" uap10:RuntimeBehavior="win32App"> 
      <uap:VisualElements AppListEntry="none" DisplayName="Contoso PhotoStore" Description="Demonstrate photo app" BackgroundColor="transparent" Square150x150Logo="Assets\Square150x150Logo.png" Square44x44Logo="Assets\Square44x44Logo.png">
        <uap:DefaultTile Wide310x150Logo="Assets\Wide310x150Logo.png" Square310x310Logo="Assets\LargeTile.png" Square71x71Logo="Assets\SmallTile.png"></uap:DefaultTile>
        <uap:SplashScreen Image="Assets\SplashScreen.png" />
      </uap:VisualElements>
      <Extensions>
        <uap:Extension Category="windows.shareTarget">
          <uap:ShareTarget Description="Send to ContosoPhotoStore">
            <uap:SupportedFileTypes>
              <uap:FileType>.jpg</uap:FileType>
              <uap:FileType>.png</uap:FileType>
              <uap:FileType>.gif</uap:FileType>
            </uap:SupportedFileTypes>
            <uap:DataFormat>StorageItems</uap:DataFormat>
            <uap:DataFormat>Bitmap</uap:DataFormat>
          </uap:ShareTarget>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

## <a name="build-and-sign-the-sparse-package"></a>Compilar e assinar o pacote esparso

Depois de criar o manifesto do pacote, compile o pacote esparso usando a [ferramenta MakeAppx.exe](https://docs.microsoft.com/windows/msix/package/create-app-package-with-makeappx-tool) no SDK do Windows. Como o pacote esparso não contém os arquivos referenciados no manifesto, você precisa especificar a opção `/nv`, que ignora a validação semântica do pacote.

O exemplo a seguir demonstra como criar um pacote esparso na linha de comando.  

```Console
MakeAppx.exe pack /d <path to directory that contains manifest> /p <output path>\MyPackage.msix /nv
```

Antes que o pacote esparso possa ser instalado com êxito em um computador de destino, você precisa conectá-lo com um certificado confiável no computador de destino. Você pode criar um certificado autoassinado para fins de desenvolvimento e assinar o pacote esparso usando [SignTool](https://docs.microsoft.com/windows/msix/package/sign-app-package-using-signtool), que está disponível no SDK do Windows.

O exemplo a seguir demonstra como assinar um pacote esparso na linha de comando.

```Console
SignTool.exe sign /fd SHA256 /a /f <path to certificate>\MyCertificate.pfx /p <certificate password> <path to sparse package>\MyPackage.msix
```

### <a name="add-the-package-identity-metadata-to-your-desktop-application-manifest"></a>Adicionar os metadados do identificador de pacote ao manifesto do aplicativo da área de trabalho

Você também precisa incluir um [manifesto do aplicativo lado a lado](https://docs.microsoft.com/windows/win32/sbscs/application-manifests) com seu aplicativo da área de trabalho e incluir um elemento [&lt;msix&gt;](https://docs.microsoft.com/windows/win32/sbscs/application-manifests#msix) com atributos que declaram os atributos de identidade do aplicativo. Os valores desses atributos são usados pelo sistema operacional para determinar a identidade do aplicativo quando o executável é iniciado.

O exemplo a seguir mostra um manifesto do aplicativo lado a lado com um elemento **\<msix\>** .

```xml
<?xml version="1.0" encoding="utf-8"?>
<assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
  <assemblyIdentity version="1.0.0.0" name="Contoso.PhotoStoreApp"/>
  <msix xmlns="urn:schemas-microsoft-com:msix.v1"
          publisher="CN=Contoso"
          packageName="ContosoPhotoStore"
          applicationId="ContosoPhotoStore"
        />
</assembly>
```

Os atributos do elemento **\<msix\>** devem corresponder a esses valores no manifesto do pacote para seu pacote esparso:

* Os atributos **packageName** e **publisher** devem corresponder aos atributos **Name** e **Publisher** no elemento [ **\<Identity\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) do manifesto do pacote, respectivamente.
* O atributo **applicationId** deve corresponder ao atributo **Id** do elemento [ **\<aplicativo\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) do manifesto do pacote.

O manifesto do aplicativo lado a lado deve existir no mesmo diretório que o arquivo executável de seu aplicativo da área de trabalho e, por convenção, deve ter o mesmo nome que o arquivo executável do aplicativo, com a extensão `.manifest` acrescentada a ele. Por exemplo, se o nome do executável do aplicativo for `ContosoPhotoStore`, o nome de arquivo do manifesto deverá ser `ContosoPhotoStore.exe.manifest`.

## <a name="register-your-sparse-package-at-run-time"></a>Registrar o pacote esparso no runtime

Para conceder o identificador de pacote ao seu aplicativo da área de trabalho, o aplicativo deve registrar o pacote esparso usando a classe [PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager). Você pode adicionar código ao aplicativo para registrar o pacote esparso quando o aplicativo é executado pela primeira vez ou pode executar código para registrar o pacote enquanto seu aplicativo da área de trabalho é instalado (por exemplo, se estiver usando o MSI para instalar o aplicativo da área de trabalho, você poderá executar esse código de uma ação personalizada).

O exemplo a seguir demonstra como registrar um pacote esparso. Esse código cria um objeto **AddPackageOptions** que contém o caminho para a localização externa em que o manifesto do pacote pode fazer referência ao conteúdo fora do pacote. Em seguida, o código passa esse objeto para o método **PackageManager.AddPackageByUriAsync** para registrar o pacote esparso. Esse método também recebe a localização do pacote esparso assinado como um URI. Para obter um exemplo mais completo, confira o arquivo de código `StartUp.cs` no [exemplo](#sample) relacionado.

```csharp
private static bool registerSparsePackage(string externalLocation, string sparsePkgPath)
{
    bool registration = false;
    try
    {
        Uri externalUri = new Uri(externalLocation);
        Uri packageUri = new Uri(sparsePkgPath);

        Console.WriteLine("exe Location {0}", externalLocation);
        Console.WriteLine("msix Address {0}", sparsePkgPath);

        Console.WriteLine("  exe Uri {0}", externalUri);
        Console.WriteLine("  msix Uri {0}", packageUri);

        PackageManager packageManager = new PackageManager();

        // Declare use of an external location
        var options = new AddPackageOptions();
        options.ExternalLocationUri = externalUri;

        Windows.Foundation.IAsyncOperationWithProgress<DeploymentResult, DeploymentProgress> deploymentOperation = packageManager.AddPackageByUriAsync(packageUri, options);

        // Other progress and error-handling code omitted for brevity...
    }
}
```

## <a name="sample"></a>Exemplo

Para um aplicativo de exemplo totalmente funcional que demonstra como conceder um identificador de pacote a um aplicativo da área de trabalho usando um pacote esparso, confira [https://github.com/microsoft/AppModelSamples/tree/master/Samples/SparsePackages](https://github.com/microsoft/AppModelSamples/tree/master/Samples/SparsePackages). Mais informações sobre como compilar e executar o exemplo são fornecidas [nesta postagem de blog](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97).

Esse exemplo inclui o seguinte:

* O código-fonte de um aplicativo do WPF chamado PhotoStoreDemo. Durante a inicialização, o aplicativo verifica se está em execução com uma identidade. Se não estiver em execução com uma identidade, ele registrará o pacote esparso e reiniciará o aplicativo. Confira `StartUp.cs` para ver o código que executa essas etapas.
* Um manifesto do aplicativo lado a lado chamado `PhotoStoreDemo.exe.manifest`.
* Um manifesto do pacote chamado `AppxManifest.xml`.
