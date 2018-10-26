---
author: msatranjr
title: Componentes do Tempo de Execução do Windows agenciados para um aplicativo UWP de sideload
description: Este documento discute um recurso direcionado a compatíveis com o Windows 10, que permite que aplicativos .NET com navegação por toque usem o código existente responsável por operações essenciais para.
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 81b3930c-6af9-406d-9d1e-8ee6a13ec38a
ms.localizationpriority: medium
ms.openlocfilehash: 3228cd80e7a9e8efb5dca1ec3a2d469e40a52c8a
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5543587"
---
# <a name="brokered-windows-runtime-components-for-a-side-loaded-uwp-app"></a>Componentes do Tempo de Execução do Windows agenciados para um aplicativo UWP de sideload

Este artigo discute um recurso direcionado a compatíveis com o Windows 10, que permite que aplicativos .NET com navegação por toque usem o código existente responsável por operações essenciais para.

## <a name="introduction"></a>Introdução

>**Observação**o código de exemplo que acompanha este documento pode ser baixado para[Visual Studio 2015 & 2017](https://aka.ms/brokeredsample). O modelo do Microsoft Visual Studio para compilar os Componentes do Tempo de Execução do Windows agenciados pode ser baixado aqui: [Modelo do Visual Studio 2015 segmentado para Aplicativos Universais do Windows para Windows 10](https://visualstudiogallery.msdn.microsoft.com/10be07b3-67ef-4e02-9243-01b78cd27935)

O Windows inclui um novo recurso chamado*Intermediado componentes do tempo de execução do Windows para aplicativos de sideload*. Usamos o termo IPC (comunicação entre processos) para descrever a capacidade de executar ativos de software de desktop existentes em um processo (componente de desktop) ao interagir com esse código em um aplicativo UWP. Este é um modelo familiar aos desenvolvedores corporativos, já que aplicativos de banco de dados e aplicativos que utilizam serviços NT no Windows compartilham uma arquitetura de vários processos semelhante.

O sideload do aplicativo é um componente essencial desse recurso.
Aplicativos específicos para empresas não têm lugar na Microsoft Store para consumidores em geral e as corporações têm exigências muito específicas em relação à segurança, privacidade, distribuição, instalação e manutenção. Dessa forma, o modelo de sideload é uma exigência daqueles que usariam esse recurso e um detalhe de implementação essencial.

Aplicativos centrados em dados são um alvo fundamental para essa arquitetura de aplicativo. Prevê-se que as regras de negócios existentes inseridas, por exemplo, no SQL Server, serão uma parte comum do componente de desktop. Isso certamente não é o único tipo de funcionalidade que pode ser oferecida pelo componente de desktop, mas uma grande parte da demanda por esse recurso está relacionada à lógica de negócios e dados existente.

Por último, dada a grande penetração do tempo de execução do .NET e da linguagem C\# no desenvolvimento das empresas, esse recurso foi desenvolvido com ênfase no uso do .NET para os lados do aplicativo UWP e do componente de desktop. Apesar de existirem outras linguagens e tempos de execução possíveis para o aplicativo UWP, o exemplo exibido apenas ilustra a C\#, e é restrito exclusivamente ao tempo de execução do .NET.

## <a name="application-components"></a>Componentes de aplicativo

>**Observação**esse recurso é exclusivamente para o uso do .NET. O aplicativo cliente e o componente da área de trabalho precisam ser criados usando-se o .NET.

**Modelo de aplicativo**

Este recurso foi criado em torno da arquitetura de aplicativo geral conhecida como MVVM (Model View View-Model). Como tal, supõe-se que o "modelo" encontra-se inteiramente no componente de desktop. Portanto, deve ser imediatamente óbvio que o componente de desktop será "sem periféricos" (ou seja, sem nenhuma interface do usuário). A exibição será inteiramente contida no aplicativo empresarial de sideload. Apesar de não haver nenhuma exigência para que esse aplicativo seja criado com o constructo de "view-model", prevemos que o uso desse padrão será comum.

**Componente de desktop**

O componente de desktop neste recurso é um novo tipo de aplicativo sendo apresentado como parte deste recurso. Esse componente da área de trabalho só pode ser escrito em C\# e deve segmentar .NET 4.6 ou superior para Windows 10. O tipo de projeto é um híbrido entre o CLR que segmenta UWP porque o formato de comunicação entre processos é composto de tipos e classes UWP, e o componente da área de trabalho tem permissão para chamar todas as partes da biblioteca de classes do tempo de execução do .NET. O impacto sobre o projeto do Visual Studio será descrito em detalhes mais tarde. Essa configuração híbrida permite marshaling de tipos UWP entre o aplicativo criado nos componentes de desktop, além de permitir que o código CLR de desktop seja chamado dentro da implementação do componente de desktop.

**Contrato**

O contrato entre o aplicativo de sideload e o componente de desktop é descrito em termos do sistema de tipo de UWP. Isso envolve declarar uma ou mais classes C\# que podem representar uma UWP. Consulte o tópico do MSDN [Criando componentes do Tempo de Execução do Windows em C\# e Visual Basic](https://msdn.microsoft.com/library/br230301.aspx) para requisito específico da criação de classe do Windows Runtime usando C\#.

>**Observação**enumerações não são compatíveis no contrato de componentes de tempo de execução do Windows entre o componente da área de trabalho e o aplicativo de sideload neste momento.

**Aplicativo de sideload**

O aplicativo de sideload é um aplicativo UWP normal em todos os aspectos, exceto um: usa sideload em vez de ser instalado através do aplicativo da Microsoft Store. A maioria dos mecanismos de instalação é idêntica: o manifesto e o empacotamento do aplicativo são semelhantes (uma adição ao manifesto é descrita em detalhes mais tarde). Depois que o sideload estiver habilitado, um script PowerShell simples poderá instalar os certificados necessários e o próprio aplicativo. Uma prática recomendada normal é que o aplicativo de sideload passe no teste de certificação WACK incluído no menu Projeto/Loja do Visual Studio

>**Observação**sideload pode ser ativado em configurações -&gt; atualização e segurança -&gt; para desenvolvedores.

Um ponto importante a salientar é que o mecanismo de Agente de aplicativo enviado como parte do Windows10 é somente de 32 bits. O componente da área de trabalho deve ser 32 bits.
Aplicativos de sideload podem ser de 64 bits (desde que haja proxies de 64 bits e 32 bits registrados), mas isso será atípico. A criação do aplicativo de sideload em C\# usando a configuração normal "neutra" e o padrão de "preferir 32 bits" naturalmente cria aplicativos de sideload de 32 bits.

**Instanciação de servidor e AppDomains**

Cada aplicativo de sideload recebe sua própria instância de um servidor de Agente de aplicativo (chamado de "várias instâncias"). O código de servidor é executado dentro de um único AppDomain. Isso permite ter várias versões de bibliotecas em instâncias separadas. Por exemplo, o aplicativo A precisa da V1.1 de um componente e o aplicativo B precisa da V2. Eles são corretamente separados, com componentes da V1.1 e da V2 em diretórios de servidor separados e apontando o aplicativo para qualquer servidor que suporte a versão correta desejada.

A implementação do código de servidor pode ser compartilhada entre várias instâncias do servidor de Agente de aplicativo, bastando apontar vários aplicativos para o mesmo diretório do servidor. Ainda haverá várias instâncias do servidor de Agente de aplicativo, mas elas estarão executando códigos idênticos. Todos os componentes de implementação usados em um único aplicativo devem estar presentes no mesmo caminho.

## <a name="defining-the-contract"></a>Definição do contrato

A primeira etapa na criação de um aplicativo usando este recurso é criar o contrato entre o aplicativo de sideload e o componente de desktop. Isso deve ser feito exclusivamente usando-se tipos do Windows Runtime.
Felizmente, eles são fáceis de declarar usando classes C\#. Porém, existem considerações de desempenho importantes durante a definição dessas conversas, algo abordado em uma seção posterior.

A sequência para definir o contrato é apresentada da seguinte maneira:

**Etapa 1:** Crie um nova biblioteca de classes no Visual Studio. Não se esqueça de criar o projeto usando o modelo da biblioteca de classes, e não o modelo do componente do Tempo de Execução do Windows

Obviamente, uma implementação vem a seguir, mas esta seção está cobrindo apenas a definição do contrato entre processos. O exemplo exibido inclui a seguinte classe (EnterpriseServer.cs), a forma inicial do que se parece com:

```csharp
namespace Fabrikam
{
    public sealed class EnterpriseServer
    {

        public ILis<String> TestMethod(String input)
        {
            throw new NotImplementedException();
        }
        
        public IAsyncOperation<int> FindElementAsync(int input)
        {
            throw new NotImplementedException();
        }
        
        public string[] RetrieveData()
        {
            throw new NotImplementedException();
        }
        
        public event EventHandler<string> PeriodicEvent;
    }
}
```

Isso define uma classe "EnterpriseServer" que pode ser instanciada pelo aplicativo de sideload. Essa classe fornece a funcionalidade prometida na RuntimeClass. A RuntimeClass pode ser usada para gerar o winmd de referência que será incluído no aplicativo de sideload.

**Etapa 2:** Edite o arquivo de projeto manualmente para alterar o tipo de saída do projeto para componente do Tempo de Execução do Windows

Para fazer isso no Visual Studio, clique com o botão direito do mouse no projeto recém-criado e selecione "Descarregar Projeto"; em seguida, clique com o botão direito do mouse novamente e selecione "Editar EnterpriseServer.csproj" para abrir o arquivo de projeto, um arquivo XML, para edição.

No arquivo aberto, procure a marca \<OutputType\> e altere o valor para "winmdobj".

**Etapa 3:** Crie uma regra de compilação que cria um arquivo de metadados do Windows de "referência" (arquivo .winmd). ou seja, não tem implementação.

**Etapa 4:** Crie uma regra de compilação que cria um arquivo de metadados do Windows de "implementação", ou seja, tem as mesmas informações de metadados, mas também inclui a implementação.

Isso será feito pelos scripts a seguir. Adicione os scripts à linha de comando de evento pós-compilação, em projeto **Propriedades** > **Compilar Eventos**.

> **Observação** o script é diferente com base na versão do Windows que você está segmentando (Windows 10) e a versão do Visual Studio em uso.

**Visual Studio 2015**
```cmd
    call "$(DevEnvDir)..\..\vc\vcvarsall.bat" x86 10.0.14393.0

    md "$(TargetDir)"\impl    md "$(TargetDir)"\reference

    erase "$(TargetDir)\impl\*.winmd"
    erase "$(TargetDir)\impl\*.pdb"
    rem erase "$(TargetDir)\reference\*.winmd"

    xcopy /y "$(TargetPath)" "$(TargetDir)impl"
    xcopy /y "$(TargetDir)*.pdb" "$(TargetDir)impl"

    winmdidl /nosystemdeclares /metadata_dir:C:\Windows\System32\Winmetadata "$(TargetPath)"

    midl /metadata_dir "%WindowsSdkDir%UnionMetadata" /iid "$(SolutionDir)BrokeredProxyStub\$(TargetName)_i.c" /env win32 /x86 /h   "$(SolutionDir)BrokeredProxyStub\$(TargetName).h" /winmd "$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(SolutionDir)BrokeredProxyStub\dlldata.c" /proxy "$(SolutionDir)BrokeredProxyStub\$(TargetName)_p.c"  "$(TargetName).idl"
    mdmerge -n 1 -i "$(ProjectDir)bin\$(ConfigurationName)" -o "$(TargetDir)reference" -metadata_dir "%WindowsSdkDir%UnionMetadata" -partial

    rem erase "$(TargetPath)"

```


**Visual Studio 2017**
```cmd
    call "$(DevEnvDir)..\..\vc\auxiliary\build\vcvarsall.bat" x86 10.0.16299.0

    md "$(TargetDir)"\impl
    md "$(TargetDir)"\reference

    erase "$(TargetDir)\impl\*.winmd"
    erase "$(TargetDir)\impl\*.pdb"
    rem erase "$(TargetDir)\reference\*.winmd"

    xcopy /y "$(TargetPath)" "$(TargetDir)impl"
    xcopy /y "$(TargetDir)*.pdb" "$(TargetDir)impl"

    winmdidl /nosystemdeclares /metadata_dir:C:\Windows\System32\Winmetadata "$(TargetPath)"

    midl /metadata_dir "%WindowsSdkDir%UnionMetadata" /iid "$(SolutionDir)BrokeredProxyStub\$(TargetName)_i.c" /env win32 /x86 /h "$(SolutionDir)BrokeredProxyStub\$(TargetName).h" /winmd "$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(SolutionDir)BrokeredProxyStub\dlldata.c" /proxy "$(SolutionDir)BrokeredProxyStub\$(TargetName)_p.c"  "$(TargetName).idl"
    mdmerge -n 1 -i "$(ProjectDir)bin\$(ConfigurationName)" -o "$(TargetDir)reference" -metadata_dir "%WindowsSdkDir%UnionMetadata" -partial

    rem erase "$(TargetPath)"
```

Uma vez a referência**winmd**é criada (na pasta "referência" na pasta de destino do projeto), ela será levada (copiada) para cada projeto de aplicativo de sideload de consumo e referenciada. Isso será descrito ainda na próxima seção. A estrutura de projeto incorporada nas regras de compilação acima garante que a implementação e o**winmd**de referênciaestejam em diretórios claramente distinguidos na hierarquia de compilação para evitar confusão.

## <a name="side-loaded-applications-in-detail"></a>Aplicativos de sideload em detalhes
Conforme mencionado anteriormente, o aplicativo de sideload é criado como qualquer outro aplicativo UWP, mas com um detalhe adicional: a declaração da disponibilidade da(s) RuntimeClass(es) no manifesto do aplicativo de sideload. Isso permite que o aplicativo simplesmente escreva algo novo para acessar a funcionalidade do componente de desktop. Uma nova entrada de manifesto na seção <Extension> descreve a RuntimeClass implementada no componente de desktop e informações sobre onde ela está localizada. Esse conteúdo da declaração no manifesto do aplicativo é o mesmo para aplicativos que segmentam o Windows 10. Por exemplo:

```XML
<Extension Category="windows.activatableClass.inProcessServer">
    <InProcessServer>
        <Path>clrhost.dll</Path>
        <ActivatableClass ActivatableClassId="Fabrikam.EnterpriseServer" ThreadingModel="both">
            <ActivatableClassAttribute Name="DesktopApplicationPath" Type="string" Value="c:\test" />
        </ActivatableClass>
    </InProcessServer>
</Extension>
```

A categoria é inProcessServer porque existem várias entradas na categoria outOfProcessServer que não são aplicáveis a essa configuração de aplicativo. Observe que o <Path> componente deve sempre conter clrhost. dll (no entanto, isso é**não**imposto e especificar um valor diferente resultará em falha formas indefinidas).

A seção <ActivatableClass> é a mesma de uma RuntimeClass verdadeiramente em processo preferida por um componente do Tempo de Execução do Windows no pacote do aplicativo. <ActivatableClassAttribute> é um novo elemento, e os atributos Name="DesktopApplicationPath" e Type="string" são obrigatórios e invariáveis. O atributo Value aponta para o local onde o winmd de implementação do componente de desktop reside (mais detalhes sobre isso na próxima seção). Cada RuntimeClass preferida pelo componente de desktop deve ter sua própria árvore de elemento <ActivatableClass>. A ActivatableClassId deve coincidir com o nome totalmente qualificado de namespace da RuntimeClass.

Como mencionado na seção "Definição do contrato", uma referência de projeto deve ser feita para o winmd de referência do componente de desktop. Normalmente, o sistema de projeto do Visual Studio cria uma estrutura de diretório de dois níveis com o mesmo nome. No exemplo, ela é EnterpriseIPCApplication\\EnterpriseIPCApplication. O **winmd**de referênciaé copiado manualmente para esse diretório de segundo nível e, em seguida, as referências de projeto caixa de diálogo é usada (clique a**Procurar...** botão) para localizar e fazer referência a esse **winmd**. Depois disso, o namespace de nível superior do componente de desktop (por exemplo, Fabrikam) deve aparecer como um nó de nível superior na parte de Referências do projeto.

>**Observação** É muito importante usar o**winmd de referência**no aplicativo de sideload. Se você transportar acidentalmente o**winmd de implementação**para o diretório do aplicativo de sideload e referenciá-lo, você provavelmente receberá um erro relacionado a "não é possível encontrar IStringable". Esse é um sinal que o errado**winmd**foi referenciado. As regras de pós-compilação no aplicativo servidor de IPC (detalhadas na próxima seção) cuidadosamente separam esses dois**winmd**em diretórios separados.

Variáveis de ambiente (especialmente %ProgramFiles%) podem ser usadas em <ActivatableClassAttribute Value="path">. Como observado anteriormente, o Agente de aplicativo só suporta 32 bits, então %ProgramFiles% será resolvido em C:\\Program Files (x86) se o aplicativo for executado em um sistema operacional de 64 bits.

## <a name="desktop-ipc-server-detail"></a>Detalhe de servidor de IPC de desktop

As duas seções anteriores descrevem a declaração da classe e a mecânica do transporte o**winmd**de referênciapara o projeto de aplicativo de sideload. A maior parte do trabalho restante no componente de desktop envolve a implementação. Como o ponto principal do componente de desktop é conseguir chamar o código de desktop (geralmente para reutilizar ativos de código existentes), o projeto deve ser configurado de maneira especial.
Normalmente, um projeto do Visual Studio usando .NET utiliza um dos dois "perfis".
Um é para o desktop (".NetFramework") e outro tem como objetivo a porção do aplicativo UWP do CLR (".NetCore"). Um componente de desktop neste recurso é um híbrido entre esses dois. Como resultado, a seção de referências é cuidadosamente construída para misturar esses dois perfis.

Um projeto normal de aplicativo UWP não contém nenhuma referência explícita de projeto, porque toda a superfície da API do Tempo de Execução do Windows está implicitamente incluída.
Normalmente, só outras referências entre projetos são feitas. No entanto, um projeto de componente de desktop tem um conjunto muito especial de referências. Ele nasce como um projeto de "Área de trabalho clássica\\Biblioteca de classe" e, portanto, é um projeto de desktop. Referências explícitas portanto, a API de tempo de execução do Windows (através de referências aos**winmd**arquivos) devem ser feitas. Adicione referências adequadas conforme mostrado abaixo.

```XML
<ItemGroup>
    <!-- These reference are added by VS automatically when you create a Class Library project-->
    <Reference Include="System" />
    <Reference Include="System.Core" />
    <Reference Include="System.Xml.Linq" />
    <Reference Include="System.Data.DataSetExtensions" />
    <Reference Include="Microsoft.CSharp" />
    <Reference Include="System.Data" />
    <Reference Include="System.Net.Http" />
<Reference Include="System.Xml" />
    <!-- These reference should be added manually by editing .csproj file-->

    <Reference Include="System.Runtime.WindowsRuntime, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089, processorArchitecture=MSIL">
      <HintPath>$(MSBuildProgramFiles32)\Microsoft SDKs\NETCoreSDK\System.Runtime.WindowsRuntime\4.0.10\lib\netcore50\System.Runtime.WindowsRuntime.dll</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\UnionMetadata\Facade\Windows.WinMD</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Foundation.FoundationContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Foundation.FoundationContract\1.0.0.0\Windows.Foundation.FoundationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Foundation.UniversalApiContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Foundation.UniversalApiContract\1.0.0.0\Windows.Foundation.UniversalApiContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.Connectivity.WwanContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.Connectivity.WwanContract\1.0.0.0\Windows.Networking.Connectivity.WwanContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.ActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ActivationCameraSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ActivationCameraSettingsContract\1.0.0.0\Windows.ApplicationModel.Activation.ActivationCameraSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ContactActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ContactActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.ContactActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract\1.0.0.0\Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Calls.LockScreenCallContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Calls.LockScreenCallContract\1.0.0.0\Windows.ApplicationModel.Calls.LockScreenCallContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Resources.Management.ResourceIndexerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Resources.Management.ResourceIndexerContract\1.0.0.0\Windows.ApplicationModel.Resources.Management.ResourceIndexerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Search.Core.SearchCoreContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Search.Core.SearchCoreContract\1.0.0.0\Windows.ApplicationModel.Search.Core.SearchCoreContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Search.SearchContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Search.SearchContract\1.0.0.0\Windows.ApplicationModel.Search.SearchContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Wallet.WalletContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Wallet.WalletContract\1.0.0.0\Windows.ApplicationModel.Wallet.WalletContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Custom.CustomDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Custom.CustomDeviceContract\1.0.0.0\Windows.Devices.Custom.CustomDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Portable.PortableDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Portable.PortableDeviceContract\1.0.0.0\Windows.Devices.Portable.PortableDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Printers.Extensions.ExtensionsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Printers.Extensions.ExtensionsContract\1.0.0.0\Windows.Devices.Printers.Extensions.ExtensionsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Printers.PrintersContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Printers.PrintersContract\1.0.0.0\Windows.Devices.Printers.PrintersContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Scanners.ScannerDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Scanners.ScannerDeviceContract\1.0.0.0\Windows.Devices.Scanners.ScannerDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Sms.LegacySmsApiContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Sms.LegacySmsApiContract\1.0.0.0\Windows.Devices.Sms.LegacySmsApiContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Gaming.Preview.GamesEnumerationContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Gaming.Preview.GamesEnumerationContract\1.0.0.0\Windows.Gaming.Preview.GamesEnumerationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract\1.0.0.0\Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Graphics.Printing3D.Printing3DContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Graphics.Printing3D.Printing3DContract\1.0.0.0\Windows.Graphics.Printing3D.Printing3DContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Management.Deployment.Preview.DeploymentPreviewContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Management.Deployment.Preview.DeploymentPreviewContract\1.0.0.0\Windows.Management.Deployment.Preview.DeploymentPreviewContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Management.Workplace.WorkplaceSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Management.Workplace.WorkplaceSettingsContract\1.0.0.0\Windows.Management.Workplace.WorkplaceSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Capture.AppCaptureContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Capture.AppCaptureContract\1.0.0.0\Windows.Media.Capture.AppCaptureContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Capture.CameraCaptureUIContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Capture.CameraCaptureUIContract\1.0.0.0\Windows.Media.Capture.CameraCaptureUIContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Devices.CallControlContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Devices.CallControlContract\1.0.0.0\Windows.Media.Devices.CallControlContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.MediaControlContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.MediaControlContract\1.0.0.0\Windows.Media.MediaControlContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Playlists.PlaylistsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Playlists.PlaylistsContract\1.0.0.0\Windows.Media.Playlists.PlaylistsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Protection.ProtectionRenewalContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Protection.ProtectionRenewalContract\1.0.0.0\Windows.Media.Protection.ProtectionRenewalContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract\1.0.0.0\Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.Sockets.ControlChannelTriggerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.Sockets.ControlChannelTriggerContract\1.0.0.0\Windows.Networking.Sockets.ControlChannelTriggerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Security.EnterpriseData.EnterpriseDataContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Security.EnterpriseData.EnterpriseDataContract\1.0.0.0\Windows.Security.EnterpriseData.EnterpriseDataContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Security.ExchangeActiveSyncProvisioning.EasContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Security.ExchangeActiveSyncProvisioning.EasContract\1.0.0.0\Windows.Security.ExchangeActiveSyncProvisioning.EasContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Services.Maps.GuidanceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Services.Maps.GuidanceContract\1.0.0.0\Windows.Services.Maps.GuidanceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Services.Maps.LocalSearchContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Services.Maps.LocalSearchContract\1.0.0.0\Windows.Services.Maps.LocalSearchContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.SystemManufacturers.SystemManufacturersContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.SystemManufacturers.SystemManufacturersContract\1.0.0.0\Windows.System.Profile.SystemManufacturers.SystemManufacturersContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.ProfileHardwareTokenContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.ProfileHardwareTokenContract\1.0.0.0\Windows.System.Profile.ProfileHardwareTokenContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.ProfileRetailInfoContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.ProfileRetailInfoContract\1.0.0.0\Windows.System.Profile.ProfileRetailInfoContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.UserProfile.UserProfileContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.UserProfile.UserProfileContract\1.0.0.0\Windows.System.UserProfile.UserProfileContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.UserProfile.UserProfileLockScreenContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.UserProfile.UserProfileLockScreenContract\1.0.0.0\Windows.System.UserProfile.UserProfileLockScreenContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.ApplicationSettings.ApplicationsSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.ApplicationSettings.ApplicationsSettingsContract\1.0.0.0\Windows.UI.ApplicationSettings.ApplicationsSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Core.AnimationMetrics.AnimationMetricsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Core.AnimationMetrics.AnimationMetricsContract\1.0.0.0\Windows.UI.Core.AnimationMetrics.AnimationMetricsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Core.CoreWindowDialogsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Core.CoreWindowDialogsContract\1.0.0.0\Windows.UI.Core.CoreWindowDialogsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Xaml.Hosting.HostingContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Xaml.Hosting.HostingContract\1.0.0.0\Windows.UI.Xaml.Hosting.HostingContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Web.Http.Diagnostics.HttpDiagnosticsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Web.Http.Diagnostics.HttpDiagnosticsContract\1.0.0.0\Windows.Web.Http.Diagnostics.HttpDiagnosticsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
</ItemGroup>
```

As referências acima são uma mistura cuidadosa de referências que são essenciais para o bom funcionamento desse servidor híbrido. O protocolo é abrir o arquivo .csproj (conforme descrito em como editar o projeto OutputType) e adicionar essas referências conforme necessário.

Depois que as referências estão configuradas corretamente, a próxima tarefa é implementar a funcionalidade do servidor. Consulte o tópico do MSDN[práticas recomendadas para interoperabilidade com componentes de tempo de execução do Windows (aplicativos UWP usando c \ # /VB/c + + e XAML)](https://msdn.microsoft.com/library/windows/apps/hh750311.aspx).
A tarefa é criar uma dll de componente do Tempo de Execução do Windows que consiga chamar o código de desktop como parte de sua implementação. O exemplo exibido inclui os principais padrões usados no Tempo de Execução do Windows:

-   Chamadas de método

-   Fontes de eventos do Tempo de Execução do Windows pelo componente de desktop

-   Operações assíncronas do Tempo de Execução do Windows

-   Retorno de matrizes de tipos básicos

**Instalar**

Para instalar o aplicativo, copie a implementação**winmd**para o diretório correto especificado no manifesto do sideload do aplicativo associado: <ActivatableClassAttribute>Value = "path". Copie também todos os arquivos de suporte associados e a dll do proxy/stub (esse último detalhe é abordado abaixo). Falha ao copiar a implementação**winmd**para o servidor local do diretório fará com que o sideload todas as chamadas do aplicativo para novo na RuntimeClass lancem um erro "classe não registrada". Não instalar o proxy/stub (ou deixar de registrar) fará com que todas as chamadas falhem sem nenhum valor de retorno. Este último erro é frequentemente**não**associado a exceções visíveis.
Se exceções forem observadas devido a esse erro de configuração, elas podem se referir à "conversão inválida".

**Considerações sobre implementação de servidores**

O servidor do Tempo de Execução do Windows de desktop pode ser pensado como com base em "tarefa" ou "trabalhador". Cada chamada para o servidor opera em um thread que não é da interface do usuário e todo o código deve ter reconhecimento de vários threads e ser seguro. Qual parte do aplicativo de sideload está chamando a funcionalidade do servidor também é importante. É fundamental evitar sempre a chamada de códigos de execução longa de qualquer thread de IU no aplicativo de sideload. Existem duas maneiras principais para realizar essa tarefa:

1.  Se for uma chamada de funcionalidade do servidor a partir de um thread de IU, sempre use um padrão assíncrono na implementação e área de superfície pública do servidor.

2.  Chame a funcionalidade do servidor a partir de um thread de segundo plano no aplicativo de sideload.

**Assíncrono do Tempo de Execução do Windows no servidor**

Por causa da natureza de processo cruzado do modelo de aplicativo, chamadas para o servidor têm mais sobrecarga do que o código que é executado exclusivamente no processo. Normalmente é seguro chamar uma propriedade simples que retorna um valor na memória porque ele executará isso de forma rápida o suficiente e o bloqueio do thread de IU não é uma preocupação. No entanto, qualquer chamada que envolva E/S de qualquer tipo (incluindo todas as recuperações de banco de dados e manipulação de arquivos) potencialmente pode bloquear o thread de IU de chamada e fazer com que o aplicativo seja finalizado devido à ausência de resposta. Além disso, chamadas de propriedade em objetos são desencorajadas nesta arquitetura de aplicativo por motivos de desempenho.
Isso é abordado de forma mais detalhada na seção a seguir.

Um servidor corretamente implementado normalmente implementará chamadas feitas diretamente a partir de threads de interface do usuário pelo padrão assíncrono do Tempo de Execução do Windows. Isso pode ser implementado seguindo este padrão. Primeiro, a declaração (novamente, a partir do exemplo exibido):

```csharp
public IAsyncOperation<int> FindElementAsync(int input)
```

Isso declara uma operação assíncrona do Tempo de Execução do Windows que retorna um número inteiro.
A implementação da operação assíncrona normalmente assume a forma:

```csharp
return Task<int>.Run( () =>
{
    int retval = ...
    // execute some potentially long-running code here 
}).AsAsyncOperation<int>();

```

>**Observação** É comum aguardar algumas outras operações potencialmente de execução longa durante a escrita da implementação. Em caso afirmativo, o**Task. Run**código precisa ser declarado:

```csharp
return Task<int>.Run(async () =>
{
    int retval = ...
    // execute some potentially long-running code here 
    await ... // some other WinRT async operation or Task
}).AsAsyncOperation<int>();
```

Os clientes desse método assíncrono podem aguardar essa operação como qualquer outra operação assíncrona do Windows Runtime.

**Chamar funcionalidade de servidor a partir de um thread de segundo plano de aplicativo**

Como é normal que o cliente e o servidor sejam escritos pela mesma organização, uma prática de programação pode ser adotada para que todas as chamadas para o servidor sejam feitas por um thread de segundo plano no aplicativo de sideload. Uma chamada direta que coleta um ou mais lotes de dados do servidor pode ser feita a partir de um thread de segundo plano. Quando os resultados estão completamente recuperados, o lote de dados que está na memória no processo do aplicativo geralmente pode ser obtido diretamente do thread de IU. Objetos C\# são naturalmente ágeis entre threads de segundo plano e threads de interface do usuário, então são especialmente úteis para esse tipo de padrão de chamada.

## <a name="creating-and-deploying-the-windows-runtime-proxy"></a>Criação e implantação do proxy do Tempo de Execução do Windows

Como a abordagem IPC envolve marshaling de interfaces do Tempo de Execução do Windows entre dois processos, um proxy e stub do Tempo de Execução do Windows registrado globalmente deve ser usado.

**Criação do proxy no Visual Studio**

O processo de criação e registro de proxies e stubs para uso dentro de um pacote de aplicativos UWP comum são descritos no tópico[Acionamento de eventos em componentes do Windows Runtime](https://msdn.microsoft.com/library/windows/apps/dn169426.aspx).
As etapas descritas nesse artigo são mais complicadas do que o processo descrito abaixo porque envolvem o registro do proxy/stub dentro do pacote de aplicativos (em vez de registrá-lo globalmente).

**Etapa 1:** Usando a solução para o projeto do componente de desktop, crie um projeto de Proxy/Stub no Visual Studio:

**Solução > Adicionar > Projeto > Visual C++ > Console Win32 Selecione a opção DLL.**

Para as etapas a seguir, presumimos que o componente de servidor é chamado**MyWinRTComponent**.

**Etapa 3:** Exclua todos os arquivos CPP/H do projeto.

**Etapa 4:** A seção anterior "Definindo o contrato" contém um comando de pós-compilação que executa**winmdidl.exe**,**midl.exe**,**mdmerge.exe**e assim por diante. Uma das saídas da etapa midl deste comando pós-compilação gera quatro saídas importantes:

a) Dlldata.c

b) Um arquivo de cabeçalho (por exemplo, MyWinRTComponent.h)

c) Um arquivo \*\_i.c (por exemplo, MyWinRTComponent\_i.c)

d) Um arquivo \*\_p.c (por exemplo, MyWinRTComponent\_p.c)

**Etapa 5:** Adicione esses quatro arquivos gerados ao projeto "MyWinRTProxy".

**Etapa 6:** Adicionar um arquivo def ao projeto "MyWinRTProxy"**(projeto > Adicionar Novo Item > código > arquivo de definição de módulo**) e atualizar o conteúdo para ser:

LIBRARY MyWinRTComponent.Proxies.dll

EXPORTS

DllCanUnloadNow PRIVATE

DllGetClassObject PRIVATE

DllRegisterServer PRIVATE

DllUnregisterServer PRIVATE

**Etapa 7:** Abra as propriedades para o projeto "MyWinRTProxy":

**Propriedades de configuração > Geral > Nome de destino:**

MyWinRTComponent.Proxies

**C/C++ > Definições de pré-processador > Adicionar**

"WIN32;\_WINDOWS;REGISTER\_PROXY\_DLL"

**C/C++ >Cabeçalho pré-compilado: selecione "Não utilizando cabeçalho pré-compilado"**

**Vinculador > Geral > Ignorar biblioteca de importação: selecione "Sim"**

**Vinculador > Entrada >Dependências adicionais: adicionar rpcrt4.lib;runtimeobject.lib**

**Vinculador > Metadados do Windows > Gerar metadados do Windows: selecione "Não"**

**Etapa 8:** Crie o projeto "MyWinRTProxy".

**Implantação do proxy**

O proxy deve ser registrado globalmente. A maneira mais simples de fazer isso é ter sua chamada de processo de instalação DllRegisterServer na dll do proxy. Observe que, como o recurso só oferece suporte a servidores criados para x86 (ou seja, sem suporte a 64 bits), a configuração mais simples é usar um servidor de 32 bits, um proxy de 32 bits e um aplicativo de sideload de 32 bits. O proxy normalmente fica junto com a implementação**winmd**para o componente da área de trabalho.

Uma etapa de configuração adicional deve ser realizada. Para que o processo de sideload carregue e execute o proxy, o diretório deve ser marcado como "ler/executar" para ALL_APPLICATION_PACKAGES. Isso é feito por meio do**icacls.exe**ferramenta de linha de comando. Esse comando deve ser executado no diretório onde o**winmd**de implementaçãoe a dll do proxy/stub reside:

*icacls. /T /grant \*S-1-15-2-1:RX*

## <a name="patterns-and-performance"></a>Padrões e desempenho

É muito importante que o desempenho do transporte de processo cruzado seja cuidadosamente monitorado. Uma chamada de processo cruzado exige pelo menos duas vezes mais do que uma chamada no processo. A criação de conversas "tagarelas" de processo cruzado ou a realização de transferências repetidas de objetos grandes, como imagens bitmap, podem causar desempenho inesperado e indesejável do aplicativo.

Aqui está uma lista não exaustiva das coisas a serem consideradas:

-   Chamadas de método síncrono do thread de IU do aplicativo para o servidor sempre devem ser evitadas. Chame o método de um thread de segundo plano no aplicativo e, em seguida, use o CoreWindowDispatcher para levar os resultados para o thread de IU, se necessário.

-   A chamada de operações assíncronas de um thread de IU do aplicativo é segura, mas considere os problemas de desempenho discutidos abaixo.

-   A transferência em massa dos resultados reduz a quantidade de conversas do processo cruzado. Isso normalmente é realizado pelo uso do constructo de matriz do Windows Runtime.

-   Retornando*lista<T>* onde*T*é um objeto de um fetch de propriedade ou operação assíncrona, fará com que muitos quantidade de conversas de processo cruzado. Por exemplo, suponha que você retorne um*lista&lt;pessoas&gt;* objetos. Cada passe de iteração será uma chamada de processo cruzado. Cada*pessoas*objeto retornado é representado por um proxy e cada chamada para um método ou propriedade nesse objeto individual resultará em uma chamada de processo cruzado. Portanto, um "inocente",*lista&lt;pessoas&gt;* objeto onde*contagem*é grande fará com que um grande número de chamadas lentas. Um melhor desempenho resulta da transferência em massa de estruturas do conteúdo em uma matriz. Por exemplo:

```csharp
struct PersonStruct
{
    String LastName;
    String FirstName;
    int Age;
   // etc.
}
```

Em seguida, retorne*Personstruct \ [\]* em vez de*lista&lt;PersonObject&gt;*.
Isso leva todos os dados por um "salto" de processo cruzado

Tal como acontece com todas as considerações de desempenho, medição e testes são fundamentais. O ideal é que uma telemetria seja inserida nas diversas operações para determinar quanto tempo elas levam. É importante medir em uma ampla gama: por exemplo, quanto tempo realmente leva para consumir todas as*pessoas*objetos para uma consulta particular no aplicativo de sideload?

Outra técnica é o teste de carga variável. Isso pode ser feito colocando ganchos de teste de desempenho no aplicativo, que introduzem cargas de atraso variáveis no processamento do servidor. Isso pode simular vários tipos de carga e a reação do aplicativo para diferentes desempenhos do servidor.
O exemplo ilustra como colocar atrasos no código usando técnicas assíncronas adequadas. A quantidade exata de atraso para injetar e o intervalo aleatório para colocar nessa carga artificial variam de acordo com o projeto do aplicativo e o ambiente esperado no qual o aplicativo será executado.

## <a name="development-process"></a>Processo de desenvolvimento

Quando você faz alterações no servidor, é necessário certificar-se de que todas as instâncias em execução anteriormente já não estejam em execução. O COM eventualmente elimina o processo, mas o temporizador de encerramento leva mais tempo do que é eficaz para o desenvolvimento iterativo. Dessa forma, eliminar uma instância em execução anteriormente é uma etapa normal durante o desenvolvimento. Isso exige que o desenvolvedor mantenha o controle de qual instância dllhost está hospedando o servidor.

O processo do servidor pode ser encontrado e eliminado usando o Gerenciador de Tarefas ou outros aplicativos de terceiros. A ferramenta de linha de comando**TaskList.exe**também está incluída e tem uma sintaxe flexível, por exemplo:

  
 | **Comando** | **Ação** |
 | ------------| ---------- |
 | tasklist | Lista todos os processos em execução na ordem aproximada de tempo de criação, com os processos criados mais recentemente próximo ao final. |
 | tasklist /FI "IMAGENAME eq dllhost.exe" /M | Lista as informações sobre todas as instâncias dllhost.exe. A opção /M lista os módulos que elas carregaram. |
 | tasklist /FI "PID eq 12564" /M | Você pode usar esta opção para consultar a dllhost.exe se você sabe o seu PID. |

Lista de módulos para um servidor do agente deve listar*clrhost. dll*em sua lista de módulos carregados.

## <a name="resources"></a>Recursos

-   [Modelos de projeto de componente WinRT intermediado para Windows 10 e VS 2015](https://visualstudiogallery.msdn.microsoft.com/10be07b3-67ef-4e02-9243-01b78cd27935)

-   [Componente de exemplo do WinRT intermediado para NorthwindRT](http://go.microsoft.com/fwlink/p/?LinkID=397349)

-   [Como entregar aplicativos fidedignos e confiáveis da Microsoft Store](http://go.microsoft.com/fwlink/p/?LinkID=393644)

-   [Contratos e extensões de aplicativos (aplicativos da Windows Store)](https://msdn.microsoft.com/library/windows/apps/hh464906.aspx)

-   [Como fazer sideload de aplicativos no Windows 10](https://msdn.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#GroupPolicy)

-   [Implantação de aplicativos UWP para empresas](http://go.microsoft.com/fwlink/p/?LinkID=264770)

