---
author: awkoren
Description: "Este guia explica como configurar sua solução do Visual Studio para editar, depurar e empacotar seu aplicativo convertido usando a Ponte de Desktop."
Search.Product: eADQiWindows 10XVcnh
title: "Guia de empacotamento de Ponte de Desktop para aplicativos de área de trabalho .NET com o Visual Studio"
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 8aa68312d6ce81c809c79ddcafe7732944a628be
ms.lasthandoff: 02/08/2017

---

# <a name="desktop-bridge-packaging-guide-for-net-desktop-apps-with-visual-studio"></a>Guia de empacotamento de Ponte de Desktop para aplicativos de área de trabalho .NET com o Visual Studio

A Atualização de Aniversário do Windows 10 permite que os desenvolvedores usem o Ponte de Desktop para empacotar aplicativos Win32 existentes usando o novo modelo de pacote (.appx), que permite publicar na loja ou sideload com facilidade. Este guia explica como configurar a solução do Visual Studio para editar, depurar e empacotar o aplicativo. 

Para começar, preencha o formulário em [Trazer os aplicativos e jogos existentes para a Windows Store usando a Ponte de Desktop](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge). A Microsoft entrará em contato com você para iniciar o processo de integração. Depois de sua conta ser aprovada para enviar aplicativos de Ponte de Desktop, siga as instruções neste documento para preparar o pacote appxupload para carregamento. 

> Quer comentar sobre os problemas encontrados ao percorrer o Ponte de Desktop? O melhor lugar para fazer sugestões para o recurso é a [Plataforma de desenvolvimento do Windows para UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial). Para perguntas e relatórios de bugs, vá para os [Fóruns de desenvolvimento de aplicativos universais do Windows](https://social.msdn.microsoft.com/Forums/home?forum=wpdevelop).

## <a name="default-universal-windows-platform-packages"></a>Pacotes-padrão da Plataforma Universal do Windows

O Visual Studio permite que você gere a depuração e libere pacotes que podem ser distribuídos usando a Windows Store ou o sideload de aplicativos. Para facilitar a criação do pacote, o Visual Studio o ajuda a criar um arquivo appxupload pronto para ser enviado para a loja. Para obter mais informações, consulte [Empacotando aplicativos UWP](..\packaging\packaging-uwp-apps.md).

## <a name="desktop-bridge-packages"></a>Pacotes de Ponte de Desktop

O [Ponte de Desktop](desktop-to-uwp-root.md) permite configurações diferentes para integrar binários Win32 no pacote do aplicativo (appx). Nós podemos pensar na progressão pela Ponte de Desktop como um percurso com quatro etapas principais. 

- **Etapa 1: Converter**: pacotes existentes binários Win32 com nenhuma ou mínima alteração de código.
- **Etapa 2: Aprimorar**: inclui alguns recursos básicos de UWP (como um bloco dinâmico) no aplicativo existente referenciando Windows.winmd do código existente Win32.
- **Etapa 3 - Estender**: inclui funcionalidades UWP avançadas (como tarefas em segundo plano) no aplicativo existente. Se seus componentes UWP e Win32 forem criados com linguagens gerenciadas (como C# ou VB.Net), o pacote resultante terá binários mistos que precisarão ser processados cuidadosamente para garantir o processamento .NET Native correto. 
- **Etapa 4 - Migrar**: você migrou sua interface do usuário para as modernas XAML e C# / VB.NET, mas ainda tem código herdado Win32. O ponto de entrada é agora um executável .NET UWP, mas o pacote ainda tem binários que usam algumas APIs do Win32.

A tabela a seguir resume algumas das diferenças para seu aplicativo em cada uma das quatro etapas. 

| Etapa | Binários | EntryPoint | .NET Native | Depuração F5 |
|---|---|---|---|---|
| 1 (Converter) | Win32 | Win32 | N/C | Extensão VS |
| 2 (Aprimorar) | Refs WinMD | Win32 | N/C | Extensão VS |
| 3 (Estender) | Win32 + CoreCLR (*) | Win32 | Pelo usuário (**) | Extensão VS |
| 4 (Migrar)    | CoreCLR (*) + Win32 | UWP | Pelo usuário (**) | VS |
| 5 (UWP) | CoreCLR | UWP |Pela loja | VS |

(*) [CoreCLR](https://github.com/dotnet/coreclr) refere-se ao tempo de execução do .NET Core do qual dependem os componentes UWP escritos em uma linguagem gerenciada (C# / VB.NET). Esses componentes também exigirão processamento .NET Native.

(**) Nas etapas 3 e 4, o usuário deve processar os assemblies do CoreCLR para produzir os binários nativos do .NET e os símbolos correspondentes antes de publicar na loja.

## <a name="configure-your-visual-studio-solution"></a>Configurar o projeto na Solução do Visual Studio

O Visual Studio inclui as ferramentas necessárias para configurar seu pacote de aplicativo, como o editor de manifesto e o Assistente para Criação de Pacote. Para usar essas ferramentas, você precisa de um projeto UWP que atuará como contêiner de appx para seu aplicativo. Embora você possa usar qualquer projeto UWP (incluindo C#, VB.NET, C++ ou JavaScript), existem alguns problemas conhecidos com projetos C#, VB.NET e C++ (consulte a seção [Problemas conhecidos](#known-issues-anchor) mais adiante neste documento), portanto, vamos usar JavaScript para este exemplo. 

Se quiser depurar seu aplicativo no contexto do modelo de aplicativo appx, você precisará adicionar outro projeto que permita a depuração de appx F5. Para obter mais informações, consulte a seção [Depurar seu aplicativo de Ponte de Desktop](#debugging-anchor).

Vamos começar o percurso com a etapa 1.

### <a name="step-1-convert"></a>Etapa 1: Converter

Esta etapa mostra como criar um aplicativo de Ponte de Desktop a partir de um projeto existente do Win32. Neste exemplo, usaremos um projeto básico WinForms que executa operações de leitura e gravação no registro.

![](images/desktop-to-uwp/net-1.png)

#### <a name="add-the-uwp-project"></a>Adicionar o projeto UWP 

Para criar o pacote de Ponte de Desktop, adicione um projeto JavaScript UWP à mesma solução.

> Observação: embora estejamos usando um modelo de JavaScript UWP, não vamos escrever qualquer código JavaScript. Estamos usando o projeto só como uma ferramenta.

![](images/desktop-to-uwp/net-2.png)

#### <a name="add-the-win32-binaries-to-the-win32-folder"></a>Adicione os binários Win32 na pasta win32

Todos os binários Win32 serão armazenados em seu projeto UWP em uma pasta chamada win32 (embora esse nome exato não seja necessário; você pode usar qualquer nome que desejar).

Se estiver usando o Visual Studio, você pode automatizar o projeto para copiar os arquivos após cada compilação, melhorando o fluxo de trabalho de desenvolvimento. Edite o arquivo do projeto (.csproj neste exemplo) para incluir um destino de AfterBuild que irá copiar todos os arquivos de saída Win32 para a pasta win32 no projeto UWP da seguinte maneira: 

```xml
  <Target Name="AfterBuild">
    <PropertyGroup>
      <TargetUWP>..\MyDesktopApp.Package\win32\</TargetUWP>
    </PropertyGroup>
     <ItemGroup>
       <Win32Binaries Include="$(TargetDir)\*" />
     </ItemGroup>
    <Copy SourceFiles="@(Win32Binaries)" DestinationFolder="$(TargetUWP)" />
  </Target>
```

Se estiver usando outra ferramenta para produzir os binários Win32, basta copiar todos os arquivos necessários no tempo de execução para a pasta win32. 

#### <a name="edit-the-app-manifest-to-enable-the-desktop-bridge-extensions"></a>Editar o manifesto do aplicativo para habilitar as extensões da Ponte de Desktop

Esse modelo inclui um package.appxmanifest que você pode usar para adicionar as extensões da Ponte de Desktop. Para editar esse arquivo, clique com botão direito e selecione "Exibir código", e adicione ou modifique esses itens: 

- `<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10" xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10" xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities" IgnorableNamespaces="uap rescap">`

- `<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.0" MaxVersionTested="10.0.14393.0" />`

- `<rescap:Capability Name="runFullTrust" />`

- `<Application Id="MyDesktopAppStep1" Executable="win32\MyDesktopApp.exe" EntryPoint="Windows.FullTrustApplication">`

Confira aqui um exemplo completo do arquivo de manifesto: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
        xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"

        xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
        xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
        IgnorableNamespaces="uap rescap mp">
  <Identity Name="MyDesktopAppStep1"
            ProcessorArchitecture="x64"
            Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US"
            Version="1.0.0.5" />
  <mp:PhoneIdentity PhoneProductId="6f6600a4-6da1-4d91-b493-35808d01f8de" PhonePublisherId="00000000-0000-0000-0000-000000000000" />
  <Properties>
    <DisplayName>MyDesktopAppStep1</DisplayName>
    <PublisherDisplayName>CN=Test</PublisherDisplayName>
    <Logo>Assets\SampleAppx.150x150.png</Logo>
  </Properties>
  <Resources>
    <Resource Language="en-us" />
  </Resources>
  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" 
                        MinVersion="10.0.14393.0" 
                        MaxVersionTested="10.0.14393.0" />
  </Dependencies>
  <Capabilities>
    <rescap:Capability Name="runFullTrust" />
  </Capabilities>
  <Applications>
    <Application Id="MyDesktopAppStep1" 
                 Executable="win32\MyDesktopApp.exe" 
                 EntryPoint="Windows.FullTrustApplication">
      <uap:VisualElements DisplayName="MyDesktopAppStep1" 
                          Description="MyDesktopAppStep1" 
                          BackgroundColor="#777777" 
                          Square150x150Logo="Assets\SampleAppx.150x150.png" 
                          Square44x44Logo="Assets\SampleAppx.44x44.png">
      </uap:VisualElements>
    </Application>
  </Applications>
</Package>
```

#### <a name="configure-the-win32-binaries"></a>Configurar os binários Win32

Para incluir no pacote de saída os binários necessários para seu aplicativo, selecione cada arquivo no Visual Studio. Defina as propriedades como "Conteúdo" e o comportamento de compilação como "Copiar se mais recente". 

![](images/desktop-to-uwp/net-3.png)

Se não quiser enviar os arquivos binários para seu repositório de código fonte, você pode usar o arquivo .gitignore para excluir todos os arquivos na pasta win32. 

#### <a name="optional-use-wildcards-to-specify-the-files-in-your-win32-folder"></a>Opcional: Usar curingas para especificar os arquivos em sua pasta win32

Se seu aplicativo Win32 precisa de vários arquivos, você pode editar o arquivo do projeto para especificar um curinga para especificar quais arquivos devem ser marcados como "Conteúdo" com base em uma expressão curinga. Você precisa abrir o arquivo jsproj com um editor de texto e incluir os arquivos necessários conforme mostrado abaixo:

```xml
<Content Include="win32\*.dll">
  <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</Content>
<Content Include="win32\*.exe">
  <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</Content>
<Content Include="win32\*.config">
  <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</Content>
<Content Include="win32\*.pdb">
  <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</Content>
```

### <a name="step-2-enhance"></a>Etapa 2: Aprimorar

Se quiser chamar as APIs UWP disponíveis do seu código Win32, você precisa adicionar uma referência ao `\Program Files (x86)\Windows Kits\10\UnionMetadata\Windows.winmd`. A lista completa de APIs UWP disponíveis para seu aplicativo está relacionada no artigo [APIs UWP compatíveis para aplicativos convertidos com a Ponte de Desktop](desktop-to-uwp-supported-api.md).  

Como esse arquivo não é necessário no Windows 10, você não precisa distribuí-lo. Nas propriedades da referência, defina a propriedade "Cópia Local" como false.

![](images/desktop-to-uwp/net-4.png)

Para adicionar os binários Win32, use as mesmas instruções da etapa 1. 

### <a name="step-3-extend"></a>Etapa 3: Estender 

Neste exemplo, vamos um aplicativo Win32 com uma tarefa em segundo plano. Para isso é preciso registrar a tarefa em segundo plano no package. appxmanifest do aplicativo UWP e adicionar uma referência ao projeto implementando a tarefa em segundo plano, conforme mostrado abaixo.

```xml
<Extensions>
  <Extension Category="windows.backgroundTasks" 
              EntryPoint="BackgroundTasks.MyBackgroundTask">
    <BackgroundTasks>
      <Task Type="timer" />
    </BackgroundTasks>
  </Extension>
</Extensions>
```

Se a tarefa em segundo plano for implementada com C# ou VB.NET, a saída resultante conterá binários CoreCLR que precisam ser processados pela cadeia de ferramentas .NET Native antes de serem enviados à loja, conforme descrito nas etapas 3 e 4. Crie o appxupload com binários mistos.

### <a name="step-4-migrate"></a>Etapa 4: Migrar

Esse cenário já tem um ponto de entrada de C# UWP, portanto, não é necessário adicionar um outro projeto UWP. No entanto, é preciso seguir as etapas descritas na etapa 1 para incluir e configurar os binários Win32.

Para executar o processo Win32, use as APIs [**FullTrustProcessLauncher**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.FullTrustProcessLauncher). Você precisará adicionar a extensão da área de trabalho e a funcionalidade *fullTrustProcess* ao manifesto do aplicativo para usar as APIs, desta forma: 

```xml
..
xmlns:desktop=http://schemas.microsoft.com/appx/manifest/desktop/windows10
..
<desktop:Extension Category="windows.fullTrustProcess" 
                    Executable="win32\MyDesktopApp.exe" />
```

## <a name="generate-packages-for-your-desktop-bridge-app"></a>Gerar pacotes para seu aplicativo da Ponte de Desktop

Depois de seguir as instruções acima, você deve estar pronto para gerar seus pacotes usando o Visual Studio conforme descrito em [Empacotando aplicativos UWP](..\packaging\packaging-uwp-apps.md). 

### <a name="steps-1-and-2-create-appxupload-with-win32-binaries"></a>Etapas 1 e 2: Criar o appxupload com binários Win32

Para enviar pacotes com a funcionalidade *fullTrust*, você precisa gerar um arquivo appxupload que inclua os símbolos para cada plataforma em um arquivo appxsym e um lote que contenha os pacotes da plataforma appx.

Nas etapas 1 e 2, o pacote não contém nenhum binário CoreCLR, e por isso você não precisa se preocupar com qual plataforma escolher. Selecione "Neutro" e "Lançar (qualquer CPU)", como mostrado na figura a seguir.

![](images/desktop-to-uwp/net-5.png)

Após você selecionar a opção de "Gerar pacotes de loja", o assistente vai gerar o arquivo appxupload pronto para ser enviado à loja.

### <a name="step-3-and-4-create-appxupload-with-mixed-binaries"></a>Etapas 3 e 4: Criar o appxupload com binários mistos

Você também deve compilar para lançamento e, nesse caso, é obrigatório especificar quais são as plataformas desejadas porque isso é necessário para que o .NET Native produza os binários nativos para cada plataforma.

![](images/desktop-to-uwp/net-6.png)

Para criar o novo arquivo appxupload, criaremos um novo arquivo zip para incluir os appxsym e appxbundle gerados da pasta _Test.

Crie um novo arquivo zip que contenha os arquivos appxbundle e appxsym e, em seguida, renomeie a extensão para appxupload.

![](images/desktop-to-uwp/net-7.png)

<span id="debugging-anchor" />
## <a name="debugging-your-desktop-bridge-app"></a>Depurar o aplicativo da Ponte de Desktop

Embora você possa iniciar seus projetos do Visual Studio sem depuração (Ctrl + F5), há um problema conhecido em que o Visual Studio não é capaz de anexar automaticamente ao processo em execução. No entanto, você pode anexar mais tarde usando um dos métodos a seguir:

### <a name="attach-to-the-running-app"></a>Anexar ao aplicativo em execução

#### <a name="attach-to-an-existing-process"></a>Anexar a um processo existente

Após ter iniciado com êxito seu aplicativo usando Ctrl + F5, você pode anexar ao processo Win32. No entanto, você não poderá depurar módulos .NET Native. 

![](images/desktop-to-uwp/net-8.png)

#### <a name="attach-to-an-installed-app"></a>Anexar a um aplicativo instalado

Você também anexar a um pacote Appx existente, usando a opção Depurar -> Outros Destinos de Depuração -> Depurar Pacote do Aplicativo Instalado.

![](images/desktop-to-uwp/net-9.png)

Nessa opção, você pode selecionar sua máquina local ou se conectar a um dispositivo remoto.

![](images/desktop-to-uwp/net-10.png)

Usando essa opção, você deve conseguir depurar o código .NET Native.

### <a name="use-visual-studio-extension-to-debug-your-desktop-bridge-app"></a>Usar a extensão do Visual Studio para depurar seu aplicativo da Ponte de Desktop 

Se preferir executar a depuração de seu aplicativo usando F5, você precisa instalar a extensão do Visual Studio 2017 [Projeto de depuração da Ponte de Desktop](https://marketplace.visualstudio.com/items?itemName=VisualStudioProductTeam.DesktoptoUWPPackagingProject) da galeria do Visual Studio.

Este projeto permite que você depure qualquer aplicativo Win32 que esteja sendo migrado para UWP usando o Visual Studio (conforme descrito neste documento) ou usando o Desktop App Converter.

#### <a name="add-the-debugging-project-to-your-solution"></a>Adicionar o projeto de depuração à sua solução

Para começar, adicione um novo projeto de depuração da Ponte de Desktop à sua solução.

![](images/desktop-to-uwp/net-11.png)

Para configurar esse projeto, você precisa definir a propriedade PackageLayout na janela Propriedades para cada configuração/plataforma que deseja usar para depuração.
Para configurar para Debug/x86, vamos definir a propriedade de layout para a pasta de bin\x86\debug do projeto UWP usando um caminho relativo: `..\MyDesktopApp.Package\bin\x86\Debug`. 

![](images/desktop-to-uwp/net-12.png)

E editar o arquivo AppXFileLayout.xml para especificar o ponto de entrada:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project ToolsVersion="14.0" 
         xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <MyProjectOutputPath>$(PackageLayout)</MyProjectOutputPath>
  </PropertyGroup>
  <ItemGroup>
    <LayoutFile Include="$(MyProjectOutputPath)\win32\MyDesktopApp.exe">
      <PackagePath>$(PackageLayout)\win32\MyDesktopApp.exe</PackagePath>
    </LayoutFile>
  </ItemGroup>
</Project>
```

Por fim, você deve configurar suas dependências de solução para garantir que os projetos sejam compilados na ordem correta. 

Como exemplo, vamos analisar a solução criada para a etapa 3.

![](images/desktop-to-uwp/net-13.png)

Para configurar a ordem de compilação, você pode usar a configuração de dependências do projeto. Clique com botão direito na sua solução e selecione a opção Dependências do projeto. Após configurar as dependências certas, você pode validar a ordem de compilação como mostrado a seguir (para a etapa 3):

![](images/desktop-to-uwp/net-14.png)

<span id="known-issues-anchor" />
## <a name="known-issues-with-cvbnet-and-c-uwp-projects"></a>Problemas conhecidos com projetos C# / VB.NET e C++ UWP

Se preferir usar um projeto C# para empacotar o aplicativo, você precisa estar ciente dos seguintes problemas conhecidos. 

- **Compilar o aplicativo nos resultados de depuração no erro: Microsoft.Net. CoreRuntime.targets(235,5): erro: Não há suporte para aplicativos com arquivos executáveis do ponto de entrada personalizada. Verifique o atributo Executável do elemento do aplicativo no manifesto do pacote.** Como alternativa, use o modo Versão.

- **Binários Win32 armazenados na pasta raiz do projeto UWP são removidos em Versão**. Se você não usar uma pasta para armazenar os binários Win32, o compilador .NET Native vai removê-los do pacote final, resultando em um erro de validação do manifesto, pois o ponto de entrada do executável não pode ser encontrado.


