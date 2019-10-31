---
Description: Saiba como conceder identidade a aplicativos de área de trabalho não empacotados para que você possa usar os recursos modernos do Windows 10 nesses aplicativos.
title: Conceder identidade a aplicativos de área de trabalho não empacotados
ms.date: 10/25/2019
ms.topic: article
keywords: Windows 10, área de trabalho, pacote, identidade, MSIX, Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f355bba3087f58ed20800052371804048bc0006c
ms.sourcegitcommit: d7eccdb27c22bccac65bd014e62b6572a6b44602
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73145611"
---
# <a name="grant-identity-to-non-packaged-desktop-apps"></a>Conceder identidade a aplicativos de área de trabalho não empacotados

<!--
> [!NOTE]
> The features described in this article require Windows 10 Insider Preview Build 10.0.19000.0 or a later release.
-->

Muitos recursos de extensibilidade do Windows 10 exigem que a [identidade do pacote](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) seja usada de aplicativos de área de trabalho não UWP, incluindo tarefas em segundo plano, notificações, blocos dinâmicos e destinos de compartilhamento. Para esses cenários, o sistema operacional requer identidade para que possa identificar o chamador da API correspondente.

Em versões do so antes do Build 10.0.19000.0 do Windows 10 Insider Preview, a única maneira de conceder identidade a um aplicativo de desktop é [empacotá-lo em um pacote MSIX assinado](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root). Para esses aplicativos, a identidade é especificada no manifesto do pacote e o registro de identidade é tratado pelo pipeline de implantação do MSIX com base nas informações no manifesto. Todo o conteúdo referenciado no manifesto do pacote está presente no pacote MSIX.

A partir do Build 10.0.19000.0 do Windows 10 Insider Preview, você pode conceder identidade de pacote a aplicativos de área de trabalho que não são empacotados em um pacote MSIX criando e registrando um *pacote esparso* com seu aplicativo. Esse suporte permite que os aplicativos da área de trabalho que ainda não são capazes de adotar o empacotamento MSIX para implantação usem recursos de extensibilidade do Windows 10 que exigem a identidade do pacote. Para obter mais informações básicas, consulte [esta postagem no blog](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97).

Para criar e registrar um pacote esparso que conceda identidade de pacote ao seu aplicativo de área de trabalho, siga estas etapas.

1. [Criar um manifesto de pacote para o pacote esparso](#create-a-package-manifest-for-the-sparse-package)
2. [Compilar e assinar o pacote esparso](#build-and-sign-the-sparse-package)
3. [Adicionar os metadados de identidade do pacote ao manifesto do aplicativo da área de trabalho](#add-the-package-identity-metadata-to-your-desktop-application-manifest)
4. [Registrar seu pacote esparso em tempo de execução](#register-your-sparse-package-at-run-time)

## <a name="important-concepts"></a>Conceitos importantes

Os recursos a seguir permitem que aplicativos de área de trabalho não empacotados adquiram a identidade do pacote.

### <a name="sparse-packages"></a>Pacotes esparsos

Um *pacote esparso* contém um manifesto de pacote, mas nenhum outro conteúdo e binários de aplicativo. O manifesto de um pacote esparso pode referenciar arquivos fora do pacote em um local externo predeterminado. Isso permite que os aplicativos que ainda não são capazes de adotar o empacotamento MSIX para o aplicativo inteiro adquirirem a identidade do pacote, conforme exigido por alguns recursos de extensibilidade do Windows 10.

> [!NOTE]
> Um aplicativo de área de trabalho que usa um pacote esparso não recebe alguns benefícios de ser totalmente implantado por meio de um pacote MSIX. Esses benefícios incluem proteção contra adulteração, instalação em um local bloqueado e gerenciamento completo pelo sistema operacional em implantação, tempo de execução e desinstalação.

### <a name="package-external-location"></a>Local externo do pacote

Para dar suporte a pacotes esparsos, o esquema de manifesto de pacote agora dá suporte a um elemento opcional **\<AllowExternalContent\>** no elemento [ **\>Propriedades de\<** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties) . Isso permite que o manifesto do pacote faça referência ao conteúdo fora do pacote, em um local específico no disco.

Por exemplo, se você tiver seu aplicativo de área de trabalho não empacotado existente que instala o executável do aplicativo e outro conteúdo em C:\Program Files\MyDesktopApp\, você pode criar um pacote esparso que inclui o **\<AllowExternalContent\>** no manifesto. Durante o processo de instalação para seu aplicativo ou na primeira vez que seus aplicativos, você pode instalar o pacote esparso e declarar C:\Program Files\MyDesktopApp\ como o local externo que seu aplicativo usará.

## <a name="create-a-package-manifest-for-the-sparse-package"></a>Criar um manifesto de pacote para o pacote esparso

Antes de criar um pacote esparso, você deve primeiro criar um [manifesto de pacote](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest) (um arquivo chamado AppxManifest. xml) que declara os metadados de identidade do pacote para seu aplicativo de área de trabalho e outros detalhes necessários. A maneira mais fácil de criar um manifesto de pacote para o pacote esparso é usar o exemplo abaixo e personalizá-lo para seu aplicativo usando a [referência de esquema](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root).

Verifique se o manifesto do pacote inclui estes itens:

* Um [ **\<identidade\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) elemento que descreve os atributos de identidade para seu aplicativo de área de trabalho.
* Um elemento **\<AllowExternalContent\>** no elemento [ **\>de propriedades de\<** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties) . Esse elemento deve ser atribuído ao valor `true`, que permite que o manifesto do pacote faça referência ao conteúdo fora do pacote, em um local específico no disco. Em uma etapa posterior, você especificará o caminho do local externo ao registrar seu pacote esparso a partir do código que é executado em seu instalador ou seu aplicativo. Qualquer conteúdo que você referencia no manifesto que não está localizado no pacote em si deve ser instalado no local externo.
* O atributo **MinVersion** do elemento [ **\<TargetDeviceFamily\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) deve ser definido como `10.0.19000.0` ou uma versão posterior.
* Os atributos **TrustLevel = mediumIL** e **RuntimeBehavior = Win32App** do elemento [ **\<Application\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) declaram que o aplicativo de desktop associado ao pacote esparso será executado de forma semelhante a uma área de trabalho desempacotada padrão aplicativo, sem o registro e a virtualização do sistema de arquivos e outras alterações de tempo de execução.

O exemplo a seguir mostra o conteúdo completo de um manifesto de pacote esparso (AppxManifest. xml). Esse manifesto inclui uma extensão `windows.sharetarget`, que requer a identidade do pacote.

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

Depois de criar o manifesto do pacote, compile o pacote esparso usando a [ferramenta MakeAppx. exe](https://docs.microsoft.com/windows/msix/package/create-app-package-with-makeappx-tool) no SDK do Windows. Como o pacote esparso não contém os arquivos referenciados no manifesto, você deve especificar a opção `/nv`, que ignora a validação semântica para o pacote.

O exemplo a seguir demonstra como criar um pacote esparso a partir da linha de comando.  

```Console
MakeAppx.exe  pack  /d  <path to directory that contains manifest>  /p  <output path>\MyPackage.msix  /nv
```

Antes que o pacote esparso possa ser instalado com êxito em um computador de destino, você deve conectá-lo com um certificado confiável no computador de destino. Você pode criar um novo certificado autoassinado para fins de desenvolvimento e assinar seu pacote esparso usando [SignTool](https://docs.microsoft.com/windows/msix/package/sign-app-package-using-signtool), que está disponível no SDK do Windows.

O exemplo a seguir demonstra como assinar um pacote esparso da linha de comando.

```Console
SignTool.exe sign /fd SHA256 /a /f <path to certificate>\MyCertificate.pfx  /p <certificate password>  <path to sparse package>\MyPackage.msix
```

### <a name="add-the-package-identity-metadata-to-your-desktop-application-manifest"></a>Adicionar os metadados de identidade do pacote ao manifesto do aplicativo da área de trabalho

You must also include a [side-by-side application manifest](https://docs.microsoft.com/windows/win32/sbscs/application-manifests) with your desktop app and include an **\<msix\>** element with attributes that declare the identity attributes of your app. The values of these attributes are used by the OS to determine your app's identity when the executable is launched.

The following example shows a side-by-side application manifest with an **\<msix\>** element.

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

The attributes of the **\<msix\>** element must match these values in the package manifest for your sparse package:

* The **packageName** and **publisher** attributes must match the **Name** and **Publisher** attributes in the [ **\<Identity\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) element in your package manifest, respectively.
* The **applicationId** attribute must match the **Id** attribute of the [ **\<Application\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) element in your package manifest.

The side-by-side application manifest must exist in the same directory as the executable file for your desktop app, and by convention it should have the same name as your app's executable file with the `.manifest` extension appended to it. For example, if your app's executable name is `ContosoPhotoStore`, the application manifest filename should be `ContosoPhotoStore.exe.manifest`.

## <a name="register-your-sparse-package-at-run-time"></a>Register your sparse package at run time

To grant package identity to your desktop app, your app must register the sparse package by using the [PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) class. You can add code to your app to register the sparse package when your app is run for the first time, or you can run code to register the package while your desktop app is installed (for example, if you're using MSI to install your desktop app, you can run this code from a custom action).

The following example demonstrates how to register a sparse package. This code creates an **AddPackageOptions** object that contains the path to the external location where your package manifest can reference content outside the package. Then, the code passes this object to the **PackageManager.AddPackageByUriAsync**  method to register the sparse package. This method also receives the location of your signed sparse package as a URI. For a more complete example, see the `StartUp.cs` code file in the related [sample](#sample).

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

For a fully functional sample app that demonstrates how to grant package identity to a desktop app using a sparse package, see [https://aka.ms/sparsepkgsample](https://aka.ms/sparsepkgsample). More information about building and running the sample is provided in [this blog post](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97).

This sample includes the following:

* The source code for a WPF app named PhotoStoreDemo. During startup, the app checks to see whether it is running with identity. If it is not running with identity, it registers the sparse package and then restarts the app. See `StartUp.cs` for the code that performs these steps.
* A side-by-side application manifest named `PhotoStoreDemo.exe.manifest`.
* A package manifest named `AppxManifest.xml`.
