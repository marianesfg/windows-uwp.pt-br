---
author: laurenhughes
title: Instalar um conjunto relacionado usando um arquivo do Instalador de Aplicativo
description: Nesta seção, analisaremos as etapas que você precisa executar para permitir a instalação de um conjunto relacionado por meio do Instalador de Aplicativo. Também explicaremos as etapas para construir um arquivo *.appinstaller que definirão o conjunto relacionado.
ms.author: lahugh
ms.date: 1/4/2018
ms.topic: article
keywords: windows 10, uwp, instalador de aplicativo, AppInstaller, sideload, conjunto relacionado, pacotes opcionais
ms.localizationpriority: medium
ms.openlocfilehash: 4caf4333bb3d442779aedac2028b0996cbd17645
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/22/2018
ms.locfileid: "7576460"
---
# <a name="install-a-related-set-using-an-app-installer-file"></a>Instalar um conjunto relacionado usando um arquivo do Instalador de Aplicativo

Se você estiver apenas começando com pacotes opcionais UWP ou conjuntos relacionados, os artigos a seguir são bons recursos para começar. 

1.  [Estender seu aplicativo usando pacotes opcionais](https://blogs.msdn.microsoft.com/appinstaller/2017/04/05/uwpoptionalpackages/)
2.  [Criar seu primeiro pacote opcional](https://blogs.msdn.microsoft.com/appinstaller/2017/05/09/build-your-first-optional-package/)
3.  [Código de carregamento de um pacote opcional](https://blogs.msdn.microsoft.com/appinstaller/2017/05/11/loading-code-from-an-optional-package/)
4.  [Ferramentas para criar um conjunto relacionado](https://blogs.msdn.microsoft.com/appinstaller/2017/05/12/tooling-to-create-a-related-set/)
5.  [Criação de pacotes opcionais e conjunto relacionado](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)

Com a atualização do Windows 10 Fall Creators Update, conjuntos relacionados agora podem ser instalados por meio do Instalador de Aplicativo. Isso permite a distribuição e a implantação de pacotes de aplicativos do conjunto relacionado aos usuários. 

## <a name="how-to-install-a-related-set"></a>Como instalar um conjunto relacionado 
Um conjunto relacionado não é uma entidade, mas em vez disso, uma combinação de um pacote principal e pacotes opcionais. Para poder instalar um conjunto relacionado como uma entidade, nós devemos ser capazes de especificar o pacote principal e o pacote opcional como um. Para fazer isso, precisamos criar um arquivo XML com uma extensão ".appinstaller" para definir um conjunto relacionado. O Instalador de Aplicativo consome o arquivo *.appinstaller e permite que o usuário instale todos os pacotes definidos com um único clique. 

Antes de entrarmos em mais detalhes, veja a seguir um arquivo de *.appinstaller de exemplo completo:

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

</AppInstaller>
```

Durante a implantação, o arquivo do Instalador de Aplicativo é validado contra os pacotes de aplicativos referenciados no elemento `Uri`. Assim, o `Name`, `Publisher` e `Version` devem corresponder ao elemento [Pacote/Identidade](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) no manifesto do conjunto de aplicativo. 

## <a name="how-to-create-an-app-installer-file"></a>Como criar um arquivo de Instalador de Aplicativo 

Para distribuir seu conjunto relacionado como uma entidade, você deve criar um arquivo do Instalador de Aplicativo que contém os elementos que são exigidos pelo [Esquema appinstaller](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file).

### <a name="step-1-create-the-appinstaller-file"></a>Etapa 1: Criar o arquivo *.appinstaller
Usando um editor de texto, crie um arquivo (que irá conter XML) e nomeie-o &lt;filename&gt;.appinstaller. 

### <a name="step-2-add-the-basic-template"></a>Etapa 2: Adicionar o modelo básico
O modelo básico inclui as informações de arquivo do Instalador de Aplicativo. 
```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
</AppInstaller>
```

### <a name="step-3-add-the-main-package-information"></a>Etapa 3: Adicionar as informações de pacote principal 
Se o pacote do aplicativo principal for um arquivo. appxbundle ou .msixbundle, em seguida, use o `<MainBundle>` mostrado abaixo. Se o pacote do aplicativo principal for um arquivo. AppX ou .msix, use `<MainPackage>` em vez de `<MainBundle>` no trecho. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />

</AppInstaller>
```
As informações no atributo `<MainBundle>` ou `<MainPackage>` devem corresponder ao elemento [Pacote/Identidade](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) no manifesto do lote de aplicativo ou manifesto do conjunto de aplicativo, respectivamente. 

### <a name="step-4-add-the-optional-packages"></a>Etapa 4: Adicionar os pacotes opcionais 
Semelhante ao atributo de conjunto de aplicativo principal, se o pacote opcional puder ser um conjunto de aplicativo ou um lote do aplicativo, o elemento filho dentro do atributo `<OptionalPackages>` deve ser `<Package>`ou `<Bundle>`, respectivamente. As informações nos elementos filho do pacote devem corresponder ao elemento identidade no manifesto do pacote ou lote. 

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

</AppInstaller>
```

### <a name="step-5-add-dependencies"></a>Etapa 5: Adicionar dependências 
No elemento de dependências, você pode especificar os pacotes de estrutura necessários para o pacote principal ou para os pacotes opcionais. 

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x86"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

    <Dependencies>
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x86" Uri="http://foobarbaz.com/fwkx86.appx" />
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x64" Uri="http://foobarbaz.com/fwkx64.appx" />
    </Dependencies>

</AppInstaller>
```

### <a name="step-6-add-update-setting"></a>Etapa 6: Adicionar a configuração de atualização 
O arquivo do Instalador de Aplicativo também pode especificar a configuração de atualização para que os conjuntos relacionados possam ser atualizados automaticamente quando um arquivo mais recente do Instalador de Aplicativo for publicado. **<UpdateSettings>** é um elemento opcional. Dentro de **<UpdateSettings>**, a opção OnLaunch especifica que as verificações de atualização devem ser feitas na inicialização do aplicativo e HoursBetweenUpdateChecks = "12" especifica que uma verificação de atualização deve ser feita a cada 12 horas. Se HoursBetweenUpdateChecks não for especificado, o intervalo padrão usado para verificar se há atualizações será de 24 horas.
``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x86"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

    <Dependencies>
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x86" Uri="http://foobarbaz.com/fwkx86.appx" />
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x64" Uri="http://foobarbaz.com/fwkx64.appx" />
    </Dependencies>
    
    <UpdateSettings>
        <OnLaunch HoursBetweenUpdateChecks="12" />
    </UpdateSettings>

</AppInstaller>
```

Para todos os detalhes sobre o esquema XML, consulte [Referência de arquivo do Instalador de Aplicativo](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file).

> [!NOTE]
> 
> O tipo de arquivo do Instalador de Aplicativo é uma novidade no Windows 10 Fall Creators Update. Não há nenhum suporte para a implantação de aplicativos UWP usando um arquivo do Instalador de Aplicativo em versões anteriores do Windows 10.
> Também deve ser observado que o elemento **HoursBetweenUpdateChecks** é novo na próxima atualização importante do Windows 10.
