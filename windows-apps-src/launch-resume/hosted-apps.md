---
Description: Saiba como criar um aplicativo hospedado que herda o executável, o ponto de entrada e os atributos de tempo de execução de um aplicativo host.
title: Criar aplicativos hospedados
ms.date: 04/23/2020
ms.topic: article
keywords: Windows 10, área de trabalho, pacote, identidade, MSIX, Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ed4356513e406c7c787ec111d32560ac08d293f1
ms.sourcegitcommit: f26d0b22a70b05679fc7089e11d639ba1a4a23af
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82107719"
---
# <a name="create-hosted-apps"></a>Criar aplicativos hospedados

A partir do Windows 10, versão 2004, você pode criar *aplicativos hospedados*. Um aplicativo hospedado compartilha o mesmo executável e definição como um aplicativo *host* pai, mas parece e se comporta como um aplicativo separado no sistema.

Os aplicativos hospedados são úteis para cenários em que você deseja que um componente (como um arquivo executável ou um arquivo de script) se comporte como um aplicativo autônomo do Windows 10, mas o componente requer um processo de host para ser executado. Por exemplo, um script do PowerShell ou Python poderia ser entregue como um aplicativo hospedado que exige a instalação de um host para ser executado. Um aplicativo hospedado pode ter seu próprio bloco inicial, identidade e integração profunda com recursos do Windows 10, como tarefas em segundo plano, notificações, blocos e destinos de compartilhamento.

O recurso aplicativos hospedados tem suporte de vários elementos e atributos no manifesto do pacote que permitem que um aplicativo hospedado use um executável e uma definição em um pacote de aplicativo host. Quando um usuário executa o aplicativo hospedado, o sistema operacional inicia automaticamente o host executável sob a identidade do aplicativo hospedado. O host pode, então, carregar ativos visuais, conteúdo ou chamar APIs como o aplicativo hospedado. O aplicativo hospedado Obtém a interseção de recursos declarados entre o host e o aplicativo hospedado. Isso significa que um aplicativo hospedado não pode solicitar mais recursos do que o fornecido pelo host.

## <a name="define-a-host"></a>Definir um host

O *host* é o executável principal ou o processo de tempo de execução para o aplicativo hospedado. Atualmente, os únicos hosts com suporte são aplicativos da área de trabalho (.NET ou C++/Win32) que têm a *identidade do pacote*. Não há suporte para aplicativos UWP como hosts no momento. Há várias maneiras para um aplicativo de área de trabalho ter a identidade do pacote:

* A maneira mais comum de conceder identidade de pacote a um aplicativo de desktop é [empacotá-lo em um pacote MSIX](https://docs.microsoft.com/windows/msix).
* Em alguns casos, você também pode optar por conceder a identidade do pacote criando um [pacote esparso](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps). Essa opção será útil se você não puder adotar o empacotamento MSIX para implantar seu aplicativo de área de trabalho.

O host é declarado em seu manifesto de pacote pela extensão [**uap10: HostRuntime**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-hostruntime) . Essa extensão tem um atributo de **ID** que deve ser atribuído a um valor que também é referenciado pelo manifesto do pacote para o aplicativo hospedado. Quando o aplicativo hospedado é ativado, o host é iniciado sob a identidade do aplicativo hospedado e pode carregar conteúdo ou binários do pacote do aplicativo hospedado.

O exemplo a seguir demonstra como definir um host em um manifesto de pacote. A extensão **uap10: HostRuntime** é de todo o pacote e, portanto, é declarada como um filho do elemento [**Package**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-package) .

``` xml
<Package xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10">

  <Extensions>
    <uap10:Extension Category="windows.hostRuntime"  
        Executable="PyScriptEngine\PyScriptEngine.exe"  
        uap10:RuntimeBehavior="packagedClassicApp"  
        uap10:TrustLevel="mediumIL">
      <uap10:HostRuntime Id="PythonHost" />
    </uap10:Extension>
  </Extensions>

</Package>
```

Anote esses detalhes importantes sobre os elementos a seguir.

| Elemento              | Detalhes |
|----------------------|-------|
| [**uap10:Extension**](https://docs.microsoft.com/wp/schemas/appxpackage/uapmanifestschema/element-uap10-extension) | A `windows.hostRuntime` categoria declara uma extensão em todo o pacote que define as informações de tempo de execução a serem usadas ao ativar um aplicativo hospedado. Um aplicativo hospedado será executado com as definições declaradas na extensão. Ao usar o aplicativo host declarado no exemplo anterior, um aplicativo hospedado será executado como o executável **PyScriptEngine. exe** no nível de confiança **mediumIL** .<br/><br/>Os atributos **executável**, **uap10: RuntimeBehavior**e **uap10: TrustLevel** especificam o nome do binário do processo de host no pacote e como os aplicativos hospedados serão executados. Por exemplo, um aplicativo hospedado usando os atributos no exemplo anterior será executado como o executável PyScriptEngine. exe no nível de confiança mediumIL. |
| [**uap10:HostRuntime**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-hostruntime) | O atributo **ID** declara o identificador exclusivo desse aplicativo host específico no pacote. Um pacote pode ter vários aplicativos de host, e cada um deve ter um elemento **uap10: HostRuntime** com uma **ID**exclusiva.

## <a name="declare-a-hosted-app"></a>Declarar um aplicativo hospedado

Um *aplicativo hospedado* declara uma dependência de pacote em um *host*. O aplicativo hospedado utiliza a ID do host (ou seja, o atributo **ID** da extensão **uap10: HostRuntime** no pacote do host) para ativação em vez de especificar um ponto de entrada executável em seu próprio pacote. O aplicativo hospedado normalmente contém conteúdo, ativos visuais, scripts ou binários que podem ser acessados pelo host. O valor de [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) no pacote do aplicativo hospedado deve ter como destino o mesmo valor que o host.

Pacotes de aplicativos hospedados podem ser assinados ou não assinados:

* Os pacotes assinados podem conter arquivos executáveis. Isso é útil em cenários que têm um mecanismo de extensão binária, que permite que o host carregue uma DLL ou um componente registrado no pacote do aplicativo hospedado.
* Pacotes não assinados só podem conter arquivos não executáveis. Isso é útil em cenários em que o host só precisa carregar imagens, ativos e conteúdo ou arquivos de script. Os pacotes não assinados devem incluir um `OID` valor especial em seu elemento de [**identidade**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) ou não poderão ser registrados. Isso impede que os pacotes não assinados entrem em conflito ou falsifiquem a identidade de um pacote assinado.

Para definir um aplicativo hospedado, declare os seguintes itens no manifesto do pacote:

* O elemento [**uap10: HostRuntimeDependency**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-hostruntimedependency) . Este é um filho do elemento [Dependencies](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-dependencies) .
* O atributo **uap10: hostid** do elemento de [**aplicativo**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) (para um aplicativo) ou elemento de [**extensão**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) (para uma extensão ativável).

O exemplo a seguir demonstra as seções relevantes de um manifesto de pacote para um aplicativo hospedado não assinado.

``` xml
<Package xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10">

  <Identity Name="NumberGuesserManifest"
    Publisher="CN=AppModelSamples, OID.2.25.311729368913984317654407730594956997722=1"
    Version="1.0.0.0" />

  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.19041.0" MaxVersionTested="10.0.19041.0" />
    <uap10:HostRuntimeDependency Name="PyScriptEnginePackage" Publisher="CN=AppModelSamples" MinVersion="1.0.0.0"/>
  </Dependencies>

  <Applications>
    <Application Id="NumberGuesserApp"  
      uap10:HostId="PythonHost"  
      uap10:Parameters="-Script &quot;NumberGuesser.py&quot;">
    </Application>
  </Applications>

</Package>
```

Anote esses detalhes importantes sobre os elementos a seguir.

| Elemento              | Detalhes |
|----------------------|-------|
| [**Identidade**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) | Como o pacote do aplicativo hospedado neste exemplo não está assinado, o atributo do **Publicador** deve incluir `OID.2.25.311729368913984317654407730594956997722=1` a cadeia de caracteres. Isso garante que o pacote não assinado não possa falsificar a identidade de um pacote assinado. |
| [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) | O atributo **MinVersion** deve especificar 10.0.19041.0 ou uma versão do sistema operacional posterior. |
| [**uap10:HostRuntimeDependency**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-hostruntimedependency)  | Esse elemento de elemento declara uma dependência no pacote do aplicativo host. Isso consiste no **nome** e no **Editor** do pacote do host, e a **MinVersion** depende. Esses valores podem ser encontrados no elemento [Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) no pacote do host. |
| [**Aplicativo**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) | O atributo **uap10: hostid** expressa a dependência no host. O pacote do aplicativo hospedado deve declarar esse atributo em vez dos atributos de **ponto de entrada** e **executáveis** usuais para um elemento [**Application**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) ou [**Extension**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) . Como resultado, o aplicativo hospedado herda os atributos **Executable**, **EntryPoint** e Runtime do host com o valor de **hostid** correspondente.<br/><br/>O atributo **uap10: Parameters** especifica parâmetros que são passados para a função de ponto de entrada do executável do host. Como o host precisa saber o que fazer com esses parâmetros, há um contrato implícito entre o host e o aplicativo hospedado. |

## <a name="register-an-unsigned-hosted-app-package-at-run-time"></a>Registrar um pacote de aplicativo hospedado não assinado em tempo de execução

Um benefício da extensão **uap10: HostRuntime** é que ele permite que um host gere dinamicamente um pacote de aplicativo hospedado em tempo de execução e registre-o usando a API do [**PackageManager**](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) , sem a necessidade de conectá-lo. Isso permite que um host gere dinamicamente o conteúdo e o manifesto para o pacote do aplicativo hospedado e, em seguida, registre-o.

Use os métodos a seguir da classe [**PackageManager**](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) para registrar um pacote de aplicativo hospedado não assinado. Esses métodos estão disponíveis a partir do Windows 10, versão 2004.

* **AddPackageByUriAsync**: registra um pacote MSIX não assinado usando a propriedade **AllowUnsigned** do parâmetro *Options* .
* **RegisterPackageByUriAsync**: executa um registro de arquivo de manifesto de pacote flexível. Se o pacote for assinado, a pasta que contém o manifesto deverá incluir um [arquivo. P7X](https://docs.microsoft.com/windows/msix/overview#inside-an-msix-package) e um catálogo. Se não for assinado, a propriedade **AllowUnsigned** do parâmetro *Options* deverá ser definida.

### <a name="requirements-for-unsigned-hosted-apps"></a>Requisitos para aplicativos hospedados não assinados

* Os elementos [**Application**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) ou [**Extension**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) no manifesto do pacote não podem conter dados de ativação, como os atributos **Executable**, **EntryPoint**ou **TrustLevel** . Em vez disso, esses elementos podem conter apenas um atributo **uap10: hostid** que expressa a dependência no host e um atributo **Uap10: Parameters** .
* O pacote deve ser um pacote principal. Ele não pode ser um pacote, um pacote de estrutura, um recurso ou um pacotes opcionais.

### <a name="requirements-for-a-host-that-installs-and-registers-an-unsigned-hosted-app-package"></a>Requisitos para um host que instala e registra um pacote de aplicativo hospedado não assinado

* O host deve ter a [identidade do pacote](#define-a-host).
* O host deve ter a **packageManagement** [capacidade restrita](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#restricted-capabilities)de packageManagement.
    ```xml
    <rescap:Capability Name="packageManagement" />
    ```

<!--
* If the host runs in app container (for example, it is a UWP app), it must also have the unsigned package management [custom capability](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#custom-capabilities) and a [Signed Custom Capability Descriptor (SCCD) file](https://docs.microsoft.com/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#preparing-the-signed-custom-capability-descriptor-sccd-file).
    ```xml
    <uap4:CustomCapability Name="Microsoft.unsignedPackageManagement_cw5n1h2txyewy" />
    ```
-->

## <a name="sample"></a>Amostra

Para um aplicativo de exemplo totalmente funcional que se declara como um host e, em seguida, registra dinamicamente um pacote de aplicativo hospedado em tempo de execução, consulte o [exemplo de aplicativo hospedado](https://github.com/microsoft/AppModelSamples/tree/master/Samples/HostedApps).

### <a name="the-host"></a>O host

O host é denominado **PyScriptEngine**. Este é um wrapper escrito em C# que executa scripts Python. Quando executado com o `-Register` parâmetro, o mecanismo de script instala um aplicativo hospedado que contém um script Python. Quando um usuário tenta iniciar o aplicativo hospedado recentemente instalado, o host é iniciado e executa o script Python **NumberGuesser** .

O manifesto do pacote para o aplicativo host (o arquivo Package. appxmanifest na pasta PyScriptEnginePackage) contém uma extensão **uap10: HostRuntime** que declara o aplicativo como um host com a ID **PythonHost** e o executável **PyScriptEngine. exe**.  

> [!NOTE]
> Neste exemplo, o manifesto do pacote é chamado Package. appxmanifest e faz parte de um [projeto de empacotamento de aplicativos do Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net). Quando esse projeto é compilado, ele [gera um manifesto chamado AppxManifest. xml](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/generate-package-manifest) e cria o pacote MSIX para o aplicativo host.

### <a name="the-hosted-app"></a>O aplicativo hospedado

O aplicativo hospedado consiste em um script Python e em artefatos de pacote, como o manifesto do pacote. Ele não contém nenhum arquivo PE.

O manifesto do pacote para o aplicativo hospedado (o arquivo NumberGuesser/AppxManifest. xml) contém os seguintes itens:

* O atributo **Publisher** do elemento [**Identity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) contém o `OID.2.25.311729368913984317654407730594956997722=1` identificador, que é necessário para um pacote não assinado.
* O atributo **uap10: hostid** do elemento [**Application**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) identifica **PythonHost** como seu host.

### <a name="run-the-sample"></a>Execute o exemplo

O exemplo requer a versão 10.0.19041.0 ou posterior do Windows 10 e o SDK do Windows.

1. Baixe o [exemplo](https://aka.ms/hostedappsample) em uma pasta em seu computador de desenvolvimento.
2. Abra a solução PyScriptEngine. sln no Visual Studio e defina o projeto **PyScriptEnginePackage** como o projeto de inicialização.
3. Compile o projeto **PyScriptEnginePackage** .
4. Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto **PyScriptEnginePackage** e escolha **implantar**.
5. Abra uma janela de prompt de comando no diretório em que você copiou os arquivos de exemplo e execute o comando a seguir para registrar o aplicativo **NumberGuesser** de exemplo (o aplicativo hospedado). Altere `D:\repos\HostedApps` para o caminho em que você copiou os arquivos de exemplo.

    ```CMD
    D:\repos\HostedApps>pyscriptengine -Register D:\repos\HostedApps\NumberGuesser\AppxManifest.xml
    ```

    > [!NOTE]
    > Você pode executar `pyscriptengine` na linha de comando porque o host no exemplo declara um [**AppExecutionAlias**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias).

6. Abra o menu **Iniciar** e clique em **NumberGuesser** para executar o aplicativo hospedado.
