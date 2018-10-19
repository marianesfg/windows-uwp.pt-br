---
author: normesta
Description: Fix issues that prevent your desktop application from running in an MSIX container
Search.Product: eADQiWindows 10XVcnh
title: Corrigir problemas que impedem que seu aplicativo da área de trabalho seja executado em um contêiner de MSIX
ms.author: normesta
ms.date: 07/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1c8e4649fad0e467e0656415e2fd49a9fea05109
ms.sourcegitcommit: 310a4555fedd4246188a98b31f6c094abb33ec60
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/19/2018
ms.locfileid: "5129822"
---
# <a name="apply-runtime-fixes-to-an-msix-package-by-using-the-package-support-framework"></a>Aplicar correções de tempo de execução em um pacote MSIX usando a estrutura de suporte do pacote

A estrutura de suporte do pacote é um kit de código-fonte aberto que ajuda você a aplicar correções para o seu aplicativo win32 quando você não tiver acesso ao código-fonte, para que ele possa executar em um contêiner de MSIX. A estrutura de suporte do pacote ajuda a seu aplicativo siga as práticas recomendadas do ambiente de tempo de execução moderno.

Para saber mais, consulte a [Estrutura de suporte do pacote](https://docs.microsoft.com/windows/msix/package-support-framework-overview).

Este guia ajudará você a identificar problemas de compatibilidade de aplicativo e encontrar, aplicar e estender o tempo de execução correções que solucioná-los.

<a id="identify" />

## <a name="identify-packaged-application-compatibility-issues"></a>Identificar problemas de compatibilidade de aplicativo empacotado

Primeiro, crie um pacote para seu aplicativo. Em seguida, instalá-lo, executá-lo e observar seu comportamento. Você poderá receber mensagens de erro que podem ajudá-lo a identificar um problema de compatibilidade. Você também pode usar o [Monitor de processo](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) para identificar problemas.  Problemas comuns se relacionam com suposições de aplicativo sobre as permissões de caminho de programa e diretório de trabalho.

### <a name="using-process-monitor-to-identify-an-issue"></a>Usando o Monitor de processo para identificar um problema

[Monitor de processo](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) é um utilitário poderoso para observar o arquivo do aplicativo e operações de registro e seus resultados.  Isso pode ajudar você a compreender os problemas de compatibilidade de aplicativo.  Após a abertura do Monitor do processo, adicione um filtro (filtro > filtro...) para incluir apenas os eventos do executável do aplicativo.

![Filtro de aplicativo ProcMon](images/desktop-to-uwp/procmon_app_filter.png)

Será exibida uma lista de eventos. Para muitos desses eventos, o word **sucesso** aparecerá na coluna do **resultado** .

![Eventos de ProcMon](images/desktop-to-uwp/procmon_events.png)

Opcionalmente, você pode filtrar os eventos para mostrar apenas somente falhas.

![ProcMon Exclude sucesso](images/desktop-to-uwp/procmon_exclude_success.png)

Se você suspeitar que existe uma falha de acesso do sistema de arquivos, procure eventos de falha que estão sob o System32/SysWOW64 ou o caminho do arquivo de pacote. Filtros também podem ajudar aqui também. Iniciar na parte inferior desta lista e role para cima. Falhas que aparecem na parte inferior desta lista ocorreram mais recentemente. Preste atenção a maioria dos erros que contêm cadeias de caracteres como "acesso negado" e "caminho/nome não encontrado" e ignorar as coisas que não examinar suspeitas. O [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/) tem dois problemas. Você pode ver esses problemas na lista que aparece na imagem a seguir.

![Txt ProcMon](images/desktop-to-uwp/procmon_config_txt.png)

Na primeira saída aparece nesta imagem, o aplicativo está falhando ler o arquivo "Txt" que está localizado no caminho "C:\Windows\SysWOW64". É improvável que o aplicativo está tentando fazer referência a esse caminho diretamente. Provavelmente, ele está tentando ler esse arquivo usando um caminho relativo, e por padrão, "System32/SysWOW64" é o diretório de trabalho do aplicativo. Isso sugere que o aplicativo está esperando seu diretório de trabalho atual ser definidas em algum lugar no pacote. Procurando dentro de appx, podemos ver que o arquivo existe no mesmo diretório do executável.

![Txt de aplicativo](images/desktop-to-uwp/psfsampleapp_config_txt.png)

O segundo problema é exibido na imagem a seguir.

![Arquivo de log ProcMon](images/desktop-to-uwp/procmon_logfile.png)

Esse problema, o aplicativo está falhando gravar um arquivo. log em seu caminho de pacote. Isso seria sugerir que uma correção de redirecionamento de arquivo pode ajudar.

<a id="find" />

## <a name="find-a-runtime-fix"></a>Encontre uma correção de tempo de execução

O PSF contém correções de tempo de execução que você pode usar no momento, como a correção de redirecionamento de arquivo.

### <a name="file-redirection-fixup"></a>Correção de redirecionamento de arquivo

Você pode usar o [Arquivo de correção de redirecionamento](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim) para redirecionar tentativas de gravação ou leitura de dados em um diretório que não é acessível de um aplicativo que é executado em um contêiner de MSIX.

Por exemplo, se seu aplicativo grava em um arquivo de log que esteja no mesmo diretório que seus aplicativos executáveis, você pode usar a [Correção de redirecionamento de arquivo](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim) para criar esse arquivo de log em outro local, como o repositório de dados de aplicativo local.

### <a name="runtime-fixes-from-the-community"></a>Correções de tempo de execução da comunidade

Certifique-se de revisar as contribuições de comunidade para nossa página do [GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop) . É possível que outros desenvolvedores resolveram um problema semelhante ao seu e compartilharam uma correção de tempo de execução.

## <a name="apply-a-runtime-fix"></a>Aplicar uma correção de tempo de execução

Você pode aplicar uma correção de tempo de execução existentes com algumas ferramentas simples do SDK do Windows e seguindo estas etapas.

> [!div class="checklist"]
> * Crie uma pasta de layout do pacote
> * Obter os arquivos de estrutura de suporte do pacote
> * Adicioná-los ao pacote
> * Modificar o manifesto do pacote
> * Criar um arquivo de configuração

Vamos percorra cada tarefa.

### <a name="create-the-package-layout-folder"></a>Crie a pasta de layout do pacote

Se já tiver um arquivo .msix (ou. AppX), você pode descompactar seu conteúdo em uma pasta de layout que servirá como a área de transferência para seu pacote.  Você pode fazer isso de um **x64 Prompt de comando de ferramentas nativas para o VS 2017**, ou manualmente com o caminho de bin do SDK no caminho de pesquisa executável.

```
makemsix unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.msix /d PackageContents

```

Isso lhe dará algo parecido com o seguinte.

![Layout do pacote](images/desktop-to-uwp/package_contents.png)

Se você não tiver um arquivo .msix (ou. AppX) para começar, você pode criar a pasta de pacote e os arquivos do zero.

### <a name="get-the-package-support-framework-files"></a>Obter os arquivos de estrutura de suporte do pacote

Você pode obter o pacote Nuget PSF usando o Visual Studio. Você também pode obtê-lo usando a ferramenta de linha de comando do Nuget autônomo.

#### <a name="get-the-package-by-using-visual-studio"></a>Obter o pacote usando o Visual Studio

No Visual Studio, clique no nó do seu projeto ou solução e escolha um dos comandos Manage Nuget Packages.  Procurar **Microsoft.PackageSupportFramework** ou **PSF** para localizar o pacote em Nuget.org. Em seguida, instalá-lo.

#### <a name="get-the-package-by-using-the-command-line-tool"></a>Obter o pacote usando a ferramenta de linha de comando

Instalar a ferramenta de linha de comando do Nuget deste local: https://www.nuget.org/downloads. Em seguida, na linha de comando Nuget, execute este comando:

```
nuget install Microsoft.PackageSupportFramework
```

### <a name="add-the-package-support-framework-files-to-your-package"></a>Adicionar os arquivos de pacote estrutura de suporte ao pacote

Adicione as DLLs de PSF necessária de 32 bits e 64 bits e os arquivos executáveis no diretório do pacote. Use a tabela a seguir como guia. Você também vai querer incluir quaisquer correções de tempo de execução que você precisa. No nosso exemplo, precisamos a correção de tempo de execução de redirecionamento de arquivo.

| Executável do aplicativo esteja x64 | Executável do aplicativo esteja x86 |
|-------------------------------|-----------|
| [PSFLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |  [PSFLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |
| [PSFRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) | [PSFRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) |
| [PSFRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) | [PSFRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) |

O conteúdo do pacote agora deve ser semelhante isso.

![Binários de pacote](images/desktop-to-uwp/package_binaries.png)

### <a name="modify-the-package-manifest"></a>Modificar o manifesto do pacote

Abra o manifesto do pacote em um editor de texto e, em seguida, defina o `Executable` atributo do `Application` elemento com o nome do arquivo executável do iniciador PSF.  Se você souber a arquitetura do seu aplicativo de destino, selecione a versão apropriada, PSFLauncher32.exe ou PSFLauncher64.exe.  Caso contrário, PSFLauncher32.exe funcionará em todos os casos.  Veja um exemplo.

```xml
<Package ...>
  ...
  <Applications>
    <Application Id="PSFSample"
                 Executable="PSFLauncher32.exe"
                 EntryPoint="Windows.FullTrustApplication">
      ...
    </Application>
  </Applications>
</Package>
```

### <a name="create-a-configuration-file"></a>Criar um arquivo de configuração

Criar um nome de arquivo ``config.json``e salve esse arquivo para a pasta raiz do seu pacote. Modificar a ID do aplicativo declarado do arquivo config.json para apontar para o executável que você acabou de ser substituído. Usando o conhecimento que você obteve usem Monitor do processo, você pode também definir o diretório de trabalho, bem como usar a correção de redirecionamento de arquivo para redirecionar leituras e gravações para arquivos. log no diretório relativo ao pacote "PSFSampleApp".

```json
{
    "applications": [
        {
            "id": "PSFSample",
            "executable": "PSFSampleApp/PSFSample.exe",
            "workingDirectory": "PSFSampleApp/"
        }
    ],
    "processes": [
        {
            "executable": "PSFSample",
            "fixups": [
                {
                    "dll": "FileRedirectionFixup.dll",
                    "config": {
                        "redirectedPaths": {
                            "packageRelative": [
                                {
                                    "base": "PSFSampleApp/",
                                    "patterns": [
                                        ".*\\.log"
                                    ]
                                }
                            ]
                        }
                    }
                }
            ]
        }
    ]
}
```
Veja a seguir um guia para o esquema de config.json:

| Matriz | key | Valor |
|-------|-----------|-------|
| applications | id |  Use o valor da `Id` atributo do `Application` elemento no manifesto do pacote. |
| applications | executável | O caminho relativo ao pacote para o executável que você deseja iniciar. Na maioria dos casos, você pode obter esse valor no arquivo de manifesto do pacote antes de modificá-lo. É o valor do `Executable` atributo do `Application` elemento. |
| applications | workingDirectory | (Opcional) Um caminho relativo ao pacote a ser usado como o diretório de trabalho do aplicativo que é iniciado. Se você não definir esse valor, o sistema operacional usa o `System32` diretório como o diretório de trabalho do aplicativo. |
| processos | executável | Na maioria dos casos, isso será o nome do `executable` configurado acima com a extensão de arquivo e caminho removida. |
| correções | DLL | Caminho do pacote relativos a correção,.msix/.appx para carregar. |
| correções | config | (Opcional) Controla como a lista de distribuição de correção se comporta. O formato exato desse valor varia em uma base de correção por correção como cada correção pode interpretar essa "blob" quanto desejar. |

O `applications`, `processes`, e `fixups` chaves são matrizes. Isso significa que você pode usar o arquivo config.json para especificar mais de um aplicativo, processos e correção DLL.


### <a name="package-and-test-the-app"></a>Pacote e teste o aplicativo

Em seguida, crie um pacote.

```
makeappx pack /d PackageContents /p PSFSamplePackageFixup.msix
```

Em seguida, assiná-lo.

```
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.msix
```

Para obter mais informações, consulte [como criar um certificado de assinatura de pacote](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate) e [como assinar um pacote usando a signtool](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool)

Usando o PowerShell, instale o pacote.

>[!NOTE]
> Lembre-se desinstalar o pacote pela primeira vez.

```
powershell Add-MSIXPackage .\PSFSamplePackageFixup.msix
```

Execute o aplicativo e observar o comportamento com correção de tempo de execução aplicada.  Repita o diagnóstico e etapas de empacotamento conforme necessário.

### <a name="use-the-trace-fixup"></a>Usar a correção de rastreamento

Uma técnica alternativa para diagnosticar problemas de compatibilidade de aplicativo empacotado é usar a correção de rastreamento. Essa DLL está incluída com o PSF e fornece uma visão detalhada de diagnóstico do comportamento do aplicativo, semelhante ao processo de Monitor.  Ele foi projetado especialmente para revelar problemas de compatibilidade de aplicativo.  Para usar a correção de rastreamento, adicionar a DLL do pacote, adicione o fragmento a seguir ao seu config.json e empacotá e instalar seu aplicativo.

```json
{
    "dll": "TraceFixup.dll",
    "config": {
        "traceLevels": {
            "filesystem": "allFailures"
        }
    }
}
```

Por padrão, a correção de rastreamento estejam falhas que podem ser consideradas "esperadas".  Por exemplo, aplicativos podem tentar incondicionalmente excluir um arquivo sem verificar se ele já existir, ignorando o resultado. Isso tem a consequência ruim que algumas falhas inesperadas podem obter filtradas, portanto, no exemplo acima, podemos optar por receber todas as falhas de funções de sistema de arquivos. Fazemos isso porque sabemos de antes que a tentativa de ler o arquivo config txt falhar com a mensagem "arquivo não encontrado". Isso é uma falha observada com frequência e não geralmente considerada inesperado. Na prática é provavelmente melhor começar filtragem somente a falhas inesperadas e, em seguida, voltando para todas as falhas se houver um problema que ainda não pode ser identificado.

Por padrão, a saída da correção de rastreamento é enviada para o depurador anexado. Neste exemplo, nós não estão indo para anexar um depurador e usará em vez disso, o programa [DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview) da SysInternals para exibir sua saída. Depois de executar o aplicativo, podemos ver as falhas mesmo como antes, que seria direcioná-nas mesma correções de tempo de execução.

![TraceShim arquivo não encontrado](images/desktop-to-uwp/traceshim_filenotfound.png)

![TraceShim acesso negado](images/desktop-to-uwp/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>Depurar, estender ou criar uma correção de tempo de execução

Você pode usar o Visual Studio para depurar uma correção de tempo de execução, estender uma correção de tempo de execução ou criar um do zero. Você precisará fazer o seguinte seja bem-sucedido.

> [!div class="checklist"]
> * Adicione um projeto de empacotamento
> * Adicione o projeto para a correção de tempo de execução
> * Adicione um projeto que inicia o inicializador de PSF executável
> * Configurar o projeto de empacotamento

Quando terminar, a solução terá a aparência desta forma.

![Solução concluída](images/desktop-to-uwp/runtime-fix-project-structure.png)

Vamos examinar cada projeto neste exemplo.

| Projeto | Finalidade |
|-------|-----------|
| DesktopApplicationPackage | Este projeto é baseado no [projeto de empacotamento de aplicativo do Windows](desktop-to-uwp-packaging-dot-net.md) e ele produz o pacote MSIX. |
| Runtimefix | Este é um projeto C++ Dynamic-Linked biblioteca que contém uma ou mais funções de substituição que servem como a correção de tempo de execução. |
| PSFLauncher | Isso é o projeto vazio de C++. Este projeto é um lugar para coletar os arquivos de tempo de execução distribuível da estrutura de suporte ao pacote. Ele gera um arquivo executável. Esse executável é a primeira coisa que é executado quando você inicia a solução. |
| WinFormsDesktopApplication | Este projeto contém o código-fonte de um aplicativo da área de trabalho. |

Para ver um exemplo completo que contém todos esses tipos de projetos, consulte [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/).

Vamos examinar as etapas para criar e configurar cada um desses projetos em sua solução.


### <a name="create-a-package-solution"></a>Criar uma solução de pacote

Se você ainda não tiver uma solução para o seu aplicativo da área de trabalho, crie uma nova **Solução em branco** no Visual Studio.

![Solução em branco](images/desktop-to-uwp/blank-solution.png)

Você também pode adicionar qualquer projetos de aplicativo que você tem.

### <a name="add-a-packaging-project"></a>Adicione um projeto de empacotamento

Se você ainda não tiver um **Projeto de empacotamento de aplicativo do Windows**, crie um e adicioná-lo à sua solução.

![Modelo de projeto do pacote](images/desktop-to-uwp/package-project-template.png)

Para obter mais informações sobre o projeto de empacotamento de aplicativo do Windows, consulte o [pacote do seu aplicativo usando o Visual Studio](desktop-to-uwp-packaging-dot-net.md).

No **Gerenciador de soluções**, clique com botão direito do projeto de empacotamento, selecione **Editar**e, em seguida, adicioná-lo na parte inferior do arquivo de projeto:

```
<Target Name="PSFRemoveSourceProject" AfterTargets="ExpandProjectReferences" BeforeTargets="_ConvertItems">
<ItemGroup>
  <FilteredNonWapProjProjectOutput Include="@(_FilteredNonWapProjProjectOutput)">
  <SourceProject Condition="'%(_FilteredNonWapProjProjectOutput.SourceProject)'=='<your runtime fix project name goes here>'" />
  </FilteredNonWapProjProjectOutput>
  <_FilteredNonWapProjProjectOutput Remove="@(_FilteredNonWapProjProjectOutput)" />
  <_FilteredNonWapProjProjectOutput Include="@(FilteredNonWapProjProjectOutput)" />
</ItemGroup>
</Target>
```

### <a name="add-project-for-the-runtime-fix"></a>Adicione o projeto para a correção de tempo de execução

Adicione um projeto de **Biblioteca de vínculo dinâmico (DLL)** do C++ à solução.

![Biblioteca de correção de tempo de execução](images/desktop-to-uwp/runtime-fix-library.png)

Clique com botão direito do que de projeto e escolha **Propriedades**.

Nas páginas de propriedades, localize o campo **C++ idioma padrão** e, em seguida, na lista suspensa ao lado do campo, selecione o **ISO padrão c++17 (/ std:c++17 + + 17)** opção.

![ISO 17 opção](images/desktop-to-uwp/iso-option.png)

Clique com botão direito desse projeto e, em seguida, no menu de contexto, escolha a opção de **Gerenciar pacotes Nuget** . Certifique-se de que a opção de **origem do pacote** é definida como **todos os** ou **nuget.org**.

Clique no ícone de configurações em seguida esse campo.

Procure o *PSF** Nuget empacotar e, em seguida, instalá-lo para este projeto.

![pacote NuGet](images/desktop-to-uwp/psf-package.png)

Se você deseja depurar ou estender uma correção de tempo de execução existente, adicione os arquivos de correção de tempo de execução que você obteve usando as diretrizes descritas na seção [Localizar uma correção de tempo de execução](#find) deste guia.

Se você pretende criar uma correção totalmente nova, não adicione nada a este projeto ainda. Ajudaremos você a adicionar os arquivos certos para este projeto mais tarde neste guia. Por enquanto, continuaremos configurar sua solução.

### <a name="add-a-project-that-starts-the-psf-launcher-executable"></a>Adicione um projeto que inicia o inicializador de PSF executável

Adicione um projeto C++ **Projeto vazio** à solução.

![Projeto vazio](images/desktop-to-uwp/blank-app.png)

Adicione o pacote Nuget **PSF** a este projeto usando as mesmas diretrizes descritas na seção anterior.

Abra as páginas de propriedades para o projeto e, na página de configurações **gerais** , defina a propriedade de **Nome de destino** como ``PSFLauncher32`` ou ``PSFLauncher64`` dependendo da arquitetura de seu aplicativo.

![Referência de iniciador PSF](images/desktop-to-uwp/shim-exe-reference.png)

Adicione uma referência de projeto para o projeto de correção de tempo de execução em sua solução.

![referência de correção de tempo de execução](images/desktop-to-uwp/reference-fix.png)

Clique com botão direito a referência e, em seguida, na janela **Propriedades** , aplicar esses valores.

| Propriedade | Value |
|-------|-----------|
| Copiar local | True |
| Copie Assemblies Satélites Local | True |
| Saída de Assembly de referência | True |
| Dependências de biblioteca de link | False |
| Entradas de dependência de biblioteca de link | False |

### <a name="configure-the-packaging-project"></a>Configurar o projeto de empacotamento

No projeto de empacotamento, clique com botão direito na pasta **Aplicativos** e escolha **Adicionar referência**.

![Adicionar referência de projeto](images/desktop-to-uwp/add-reference-packaging-project.png)

Escolha o projeto de iniciador PSF e seu projeto de aplicativo da área de trabalho e, em seguida, escolha o botão **Okey** .

![Projeto de desktop](images/desktop-to-uwp/package-project-references.png)

>[!NOTE]
> Se você não tiver o código-fonte para seu aplicativo, basta escolha o projeto de iniciador PSF. Mostraremos a você como fazer referência do executável quando você cria um arquivo de configuração.

No nó de **aplicativos** , clique com botão direito do aplicativo inicializador PSF e escolha **Definir como ponto de entrada**.

![Definir ponto de entrada](images/desktop-to-uwp/set-startup-project.png)

Adicione um arquivo chamado ``config.json`` ao seu projeto de empacotamento, em seguida, copie e cole o seguinte texto json para o arquivo. Defina a propriedade de **Ação de pacote** de **conteúdo**.

```json
{
    "applications": [
        {
            "id": "",
            "executable": "",
            "workingDirectory": ""
        }
    ],
    "processes": [
        {
            "executable": "",
            "fixups": [
                {
                    "dll": "",
                    "config": {
                    }
                }
            ]
        }
    ]
}
```
Fornece um valor para cada chave. Use esta tabela como um guia.

| Matriz | key | Valor |
|-------|-----------|-------|
| applications | id |  Use o valor da `Id` atributo do `Application` elemento no manifesto do pacote. |
| applications | executável | O caminho relativo ao pacote para o executável que você deseja iniciar. Na maioria dos casos, você pode obter esse valor no arquivo de manifesto do pacote antes de modificá-lo. É o valor do `Executable` atributo do `Application` elemento. |
| applications | workingDirectory | (Opcional) Um caminho relativo ao pacote a ser usado como o diretório de trabalho do aplicativo que é iniciado. Se você não definir esse valor, o sistema operacional usa o `System32` diretório como o diretório de trabalho do aplicativo. |
| processos | executável | Na maioria dos casos, isso será o nome do `executable` configurado acima com a extensão de arquivo e caminho removida. |
| correções | DLL | Caminho do pacote relativos a correção DLL para carregar. |
| correções | config | (Opcional) Controla como a correção DLL se comporta. O formato exato desse valor varia em uma base de correção por correção como cada correção pode interpretar essa "blob" quanto desejar. |

Quando terminar, o ``config.json`` arquivo será algo parecido com isto.

```json
{
  "applications": [
    {
      "id": "DesktopApplication",
      "executable": "DesktopApplication/WinFormsDesktopApplication.exe",
      "workingDirectory": "WinFormsDesktopApplication"
    }
  ],
  "processes": [
    {
      "executable": ".*App.*",
      "fixups": [ { "dll": "RuntimeFix.dll" } ]
    }
  ]
}

```

>[!NOTE]
> O `applications`, `processes`, e `fixups` chaves são matrizes. Isso significa que você pode usar o arquivo config.json para especificar mais de um aplicativo, processos e correção DLL.

### <a name="debug-a-runtime-fix"></a>Depurar uma correção de tempo de execução

No Visual Studio, pressione F5 para iniciar o depurador.  A primeira coisa que inicia é o aplicativo inicializador PSF, que por sua vez, inicia o aplicativo de área de trabalho de destino.  Para depurar o aplicativo da área de trabalho de destino, você precisará manualmente anexar ao processo do aplicativo da área de trabalho, escolhendo **Depurar**->**anexar ao processo**e, em seguida, selecionando o processo do aplicativo. Para permitir a depuração de um aplicativo .NET com uma correção de tempo de execução nativo DLL, selecione os tipos de código gerenciado e nativo (depuração de modo misto).  

Depois que você configurou isso, você pode definir pontos de interrupção ao lado de linhas de código no código do aplicativo da área de trabalho e o projeto de correção de tempo de execução. Se você não tiver o código-fonte para seu aplicativo, você poderá definir pontos de interrupção apenas ao lado de linhas de código em seu projeto de correção de tempo de execução.

>[!NOTE]
> Enquanto o Visual Studio oferece o desenvolvimento mais simples e experiência de depuração, existem algumas limitações, portanto, mais adiante neste guia, vamos discutir outras técnicas de depuração que você pode aplicar.

## <a name="create-a-runtime-fix"></a>Criar uma correção de tempo de execução

Se não existe um tempo de execução corrigir o problema que você deseja resolver, você pode criar uma nova correção de tempo de execução, escrevendo funções de substituição e incluindo quaisquer dados de configuração que faz sentido. Vamos examinar cada parte.

### <a name="replacement-functions"></a>Funções de substituição

Primeiro, identifique qual função falha nas chamadas quando seu aplicativo é executado em um contêiner de MSIX. Em seguida, você pode criar funções de substituição que você gostaria que o Gerenciador de tempo de execução para chamar em vez disso. Isso lhe dá a oportunidade de substituir a implementação de uma função com comportamento em conformidade com as regras do ambiente de tempo de execução moderno.

No Visual Studio, abra o projeto de correção de tempo de execução que você criou anteriormente neste guia.

Declare o ``FIXUP_DEFINE_EXPORTS`` macro e, em seguida, adicione uma instrução include para o `fixup_framework.h` na parte superior de cada um. Arquivo CPP em que você pretende adicionar as funções de sua correção de tempo de execução.

```c++
#define FIXUP_DEFINE_EXPORTS
#include <fixup_framework.h>
```
>[!IMPORTANT]
>Verifique se o `FIXUP_DEFINE_EXPORTS` macro aparece antes da instrução de inclusão.

Criar uma função que tem a mesma assinatura da função que tem comportamento que você deseja modificar. Aqui está uma função de exemplo que substitui o `MessageBoxW` função.

```c++
auto MessageBoxWImpl = &::MessageBoxW;
int WINAPI MessageBoxWFixup(
    _In_opt_ HWND hwnd,
    _In_opt_ LPCWSTR,
    _In_opt_ LPCWSTR caption,
    _In_ UINT type)
{
    return MessageBoxWImpl(hwnd, L"SUCCESS: This worked", caption, type);
}

DECLARE_FIXUP(MessageBoxWImpl, MessageBoxWFixup);
```

A chamada para `DECLARE_FIXUP` mapeia a `MessageBoxW` função para sua nova função de substituição. Quando seu aplicativo tenta chamar o `MessageBoxW` função, ele chamará a função de substituição em vez disso.

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>Proteger contra recursiva chamadas para funções no correções de tempo de execução

Opcionalmente, você pode aplicar o `reentrancy_guard` tipo de suas funções que protegem contra recursiva chamadas para funções no correções de tempo de execução.

Por exemplo, você pode produzir uma função de substituição para o `CreateFile` função. Sua implementação pode chamar o `CopyFile` função, mas a implementação do `CopyFile` função pode chamar o `CreateFile` função. Isso pode levar a um ciclo de recursiva infinita de chamadas para o `CreateFile` função.

Para obter mais informações sobre `reentrancy_guard` consulte [authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### <a name="configuration-data"></a>Dados de configuração

Se você deseja adicionar dados de configuração para a correção de tempo de execução, considere adicioná-lo para o ``config.json``. Dessa forma, você pode usar o `FixupQueryCurrentDllConfig` facilmente analisar esses dados. Este exemplo analisa um valor booleano e cadeia de caracteres do arquivo de configuração.

```c++
if (auto configRoot = ::FixupQueryCurrentDllConfig())
{
    auto& config = configRoot->as_object();

    if (auto enabledValue = config.try_get("enabled"))
    {
        g_enabled = enabledValue->as_boolean().get();
    }

    if (auto logPathValue = config.try_get("logPath"))
    {
        g_logPath = logPathValue->as_string().wstring();
    }
}
```

## <a name="other-debugging-techniques"></a>Outras técnicas de depuração

Embora o Visual Studio oferece o desenvolvimento mais simples e a experiência de depuração, há algumas limitações.

Primeiro, depuração F5 executa o aplicativo Implantando arquivos soltos do caminho de pasta de layout do pacote, em vez de instalando a partir de um .msix / pacote. AppX.  A pasta de layout normalmente não tem as mesmas restrições de segurança como uma pasta de pacote instalado. Como resultado, talvez não seja possível reproduzir erros de negação de acesso de caminho de pacote antes da aplicação de uma correção de tempo de execução.

Para resolver esse problema, use .msix / implantação do pacote. AppX, em vez de F5 solto implantação do arquivo.  Para criar um .msix /. AppX pacotes de arquivo, use o utilitário [MakeMSIX](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) do SDK do Windows, conforme descrito acima. Ou, de dentro do Visual Studio, clique com botão direito o nó do seu projeto de aplicativo e selecione **Store**->**Criar pacotes de aplicativo**.

Outro problema com o Visual Studio é que ele não tem suporte interno para anexar a quaisquer processos filho iniciados pelo depurador.   Isso dificulta depurar lógica no caminho de inicialização do aplicativo de destino, que deve ser anexado manualmente pelo Visual Studio após a inicialização.

Para resolver esse problema, use um depurador que ofereça suporte a anexar processo filho.  Observe que geralmente não é possível anexar um depurador do just-in-time (JIT) para o aplicativo de destino.  Isso ocorre porque a maioria das técnicas JIT envolvem iniciar o depurador no lugar do aplicativo de destino, por meio de chave do registro ImageFileExecutionOptions.  Isso destrói o mecanismo detouring usado pela PSFLauncher.exe para injetar FixupRuntime.dll no aplicativo de destino.  WinDbg, incluídos nas [Ferramentas de depuração para Windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)e obtidos do [SDK do Windows](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk), processo de filho dá suporte à anexar.  Ele também agora oferece suporte diretamente [Iniciando e depuração de um aplicativo UWP](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app).

Para depurar a inicialização do aplicativo de destino como um processo filho, inicie ``WinDbg``.

```
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

No ``WinDbg`` solicitar, habilitar a depuração do filho e defina pontos de interrupção apropriados.

```
.childdbg 1
g
```
(é executada até que o aplicativo de destino é iniciado e entra no depurador)

```
sxe ld fixup.dll
g
```
(é executada até que a correção que dll é carregada)

```
bp ...
```

>[!NOTE]
> [PLMDebug](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/plmdebug) também pode ser usado para anexar um depurador a um aplicativo durante a inicialização e também está incluído nas [Ferramentas de depuração para Windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index).  No entanto, é mais complexo de usar que o suporte direto agora fornecido pelo WinDbg.

## <a name="support-and-feedback"></a>Suporte e comentários

**Encontrar respostas para suas dúvidas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

