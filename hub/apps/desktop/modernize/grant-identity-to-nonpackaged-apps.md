---
Description: Saiba como conceder identidade a aplicativos de área de trabalho não empacotados para que você possa usar os recursos modernos do Windows 10 nesses aplicativos.
title: Conceder identidade a aplicativos da área de trabalho não empacotados
ms.date: 02/28/2020
ms.topic: article
keywords: Windows 10, área de trabalho, pacote, identidade, MSIX, Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ae05a00cac19fdd349aa48160b88cde6b84e26b0
ms.sourcegitcommit: 620e4a51e2486ec2cb7190176b3d9bf3d7b5b6af
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2020
ms.locfileid: "78222022"
---
# <a name="grant-identity-to-non-packaged-desktop-apps"></a>Conceder identidade a aplicativos da área de trabalho não empacotados

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

Por exemplo, se você tiver seu aplicativo de área de trabalho não empacotado existente que instala o executável do aplicativo e outro conteúdo em C:\Program Files\MyDesktopApp\, você pode criar um pacote esparso que inclui o elemento **\<\>do AllowExternalContent** no manifesto. Durante o processo de instalação para seu aplicativo ou na primeira vez que seus aplicativos, você pode instalar o pacote esparso e declarar C:\Program Files\MyDesktopApp\ como o local externo que seu aplicativo usará.

## <a name="create-a-package-manifest-for-the-sparse-package"></a>Criar um manifesto de pacote para o pacote esparso

Antes de criar um pacote esparso, você deve primeiro criar um [manifesto de pacote](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest) (um arquivo chamado AppxManifest. xml) que declara os metadados de identidade do pacote para seu aplicativo de área de trabalho e outros detalhes necessários. A maneira mais fácil de criar um manifesto de pacote para o pacote esparso é usar o exemplo abaixo e personalizá-lo para seu aplicativo usando a [referência de esquema](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root).

Verifique se o manifesto do pacote inclui estes itens:

* Um [ **\<identidade\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) elemento que descreve os atributos de identidade para seu aplicativo de área de trabalho.
* Um elemento **\<AllowExternalContent\>** no elemento [ **\>de propriedades de\<** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties) . Esse elemento deve ser atribuído ao valor `true`, que permite que o manifesto do pacote faça referência ao conteúdo fora do pacote, em um local específico no disco. Em uma etapa posterior, você especificará o caminho do local externo ao registrar seu pacote esparso a partir do código que é executado em seu instalador ou seu aplicativo. Qualquer conteúdo que você referencia no manifesto que não está localizado no pacote em si deve ser instalado no local externo.
* O atributo **MinVersion** do elemento [ **\<TargetDeviceFamily\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) deve ser definido como `10.0.19000.0` ou uma versão posterior.
* Os atributos **TrustLevel = mediumIL** e **RuntimeBehavior = Win32App** do elemento [ **\<Application\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) declaram que o aplicativo da área de trabalho associado ao pacote esparso será executado de forma semelhante a um aplicativo de área de trabalho não empacotado padrão, sem a virtualização do registro e do sistema de arquivos e outras alterações de tempo de execução.

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

Você também deve incluir um [manifesto do aplicativo lado a lado](https://docs.microsoft.com/windows/win32/sbscs/application-manifests) com seu aplicativo de área de trabalho e incluir um elemento [&lt;msix&gt;](https://docs.microsoft.com/windows/win32/sbscs/application-manifests#msix) com atributos que declaram os atributos de identidade do seu aplicativo. Os valores desses atributos são usados pelo sistema operacional para determinar a identidade do aplicativo quando o executável é iniciado.

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

* Os atributos **PackageName** e **Publisher** devem corresponder aos atributos **Name** e **Publisher** no elemento [ **\<Identity\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) no manifesto do pacote, respectivamente.
* O atributo **ApplicationId** deve corresponder ao atributo **Id** do elemento [ **\<Application\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) no manifesto do pacote.

O manifesto do aplicativo lado a lado deve existir no mesmo diretório que o arquivo executável para seu aplicativo de área de trabalho e, por convenção, ele deve ter o mesmo nome que o arquivo executável do seu aplicativo com a extensão `.manifest` acrescentada a ele. Por exemplo, se o nome do executável do seu aplicativo for `ContosoPhotoStore`, o nome de arquivo do manifesto do aplicativo deverá ser `ContosoPhotoStore.exe.manifest`.

## <a name="register-your-sparse-package-at-run-time"></a>Registrar seu pacote esparso em tempo de execução

Para conceder identidade de pacote ao seu aplicativo de área de trabalho, seu aplicativo deve registrar o pacote esparso usando a classe [PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) . Você pode adicionar código ao seu aplicativo para registrar o pacote esparso quando seu aplicativo é executado pela primeira vez ou você pode executar o código para registrar o pacote enquanto seu aplicativo de área de trabalho está instalado (por exemplo, se você estiver usando o MSI para instalar seu aplicativo de área de trabalho , você pode executar esse código de uma ação personalizada.

O exemplo a seguir demonstra como registrar um pacote esparso. Esse código cria um objeto **Addpackageoptions** que contém o caminho para o local externo onde o manifesto do pacote pode fazer referência a conteúdo fora do pacote. Em seguida, o código passa esse objeto para o método **PackageManager. AddPackageByUriAsync** para registrar o pacote esparso. Esse método também recebe o local do pacote esparso assinado como um URI. Para obter um exemplo mais completo, consulte o arquivo de código `StartUp.cs` no [exemplo](#sample)relacionado.

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

Para um aplicativo de exemplo totalmente funcional que demonstra como conceder a identidade do pacote a um aplicativo de desktop usando um pacote esparso, consulte [https://github.com/microsoft/AppModelSamples/tree/master/Samples/SparsePackages](https://github.com/microsoft/AppModelSamples/tree/master/Samples/SparsePackages). Mais informações sobre como criar e executar o exemplo são fornecidas nesta [postagem de blog](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97).

Este exemplo inclui o seguinte:

* O código-fonte de um aplicativo do WPF chamado PhotoStoreDemo. Durante a inicialização, o aplicativo verifica se ele está em execução com a identidade. Se não estiver em execução com a identidade, ele registrará o pacote esparso e reiniciará o aplicativo. Consulte `StartUp.cs` para o código que executa essas etapas.
* Um manifesto de aplicativo lado a lado chamado `PhotoStoreDemo.exe.manifest`.
* Um manifesto de pacote chamado `AppxManifest.xml`.
