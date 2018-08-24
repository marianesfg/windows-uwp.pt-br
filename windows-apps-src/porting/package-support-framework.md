---
author: normesta
Description: Fix issues that prevent your desktop application from running in an MSIX container
Search.Product: eADQiWindows 10XVcnh
title: Corrigir problemas que impedem o seu aplicativo da área de trabalho sejam executados em um contêiner MSIX
ms.author: normesta
ms.date: 07/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 46d5705233af9e8254b9ac89a2d6e9891e90701f
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/24/2018
ms.locfileid: "2841358"
---
# <a name="apply-runtime-fixes-to-an-msix-package-by-using-the-package-support-framework"></a>Aplicar correções de tempo de execução em um pacote MSIX usando a estrutura de suporte do pacote

A estrutura de suporte do pacote é um kit de código aberto ajudá-lo a aplicar correções para seu aplicativo win32 quando você não tem acesso ao código-fonte, para que ele possa ser executado em um recipiente MSIX. A estrutura de suporte do pacote de ajuda seu aplicativo que seguem as práticas recomendadas do ambiente de tempo de execução moderno.

Para criar a estrutura de suporte do pacote, podemos aproveitado a tecnologia de [desvios](https://www.microsoft.com/en-us/research/project/detours) que é uma estrutura de código-fonte aberto desenvolvida pelo Microsoft Research (MSR) e ajuda com o redirecionamento de API e enganchando.

Essa estrutura é o código-fonte aberto, leve, e você pode usá-lo para resolver problemas de aplicativo rapidamente. Ele também oferece a oportunidade para consultar a comunidade em todo o mundo e criar na parte superior os investimentos de outras pessoas.

## <a name="a-quick-look-inside-of-the-package-support-framework"></a>Uma rápida visualização dentro a estrutura de suporte do pacote

A estrutura de suporte do pacote contém um arquivo executável, um gerente de tempo de execução DLL e um conjunto de correções de tempo de execução.

![Estrutura de suporte do pacote](images/desktop-to-uwp/package-support-framework.png)

Veja como funciona. Você vai criar um arquivo de configuração que especifica o fix(s) que você deseja aplicar ao seu aplicativo. Em seguida, você vai modificar seu pacote para apontar para o arquivo executável do iniciador de correção.

Quando os usuários iniciam seu aplicativo, o iniciador de correção é o executável da primeiro que é executado. Lê o seu arquivo de configuração e insere um fix(s) o tempo de execução e o Gerenciador de tempo de execução DLL no processo do aplicativo.

![Pacote suporte Framework DLL injeção](images/desktop-to-uwp/package-support-framework-2.png)

O Gerenciador de tempo de execução se aplica a correção quando for necessária, o aplicativo seja executado dentro de um contêiner MSIX.

Este guia o ajudará a identificar problemas de compatibilidade de aplicativo e localizar, aplicar e estender o runtime correções que resolvê-los.

<a id="identify" />

## <a name="identify-packaged-application-compatibility-issues"></a>Identificar problemas de compatibilidade de aplicativo de pacote

Primeiro, crie um pacote para seu aplicativo. Em seguida, instalá-lo, executá-lo e observar seu comportamento. Você poderá receber mensagens de erro que podem ajudá-lo a identificar um problema de compatibilidade. Você também pode usar o [Monitor de processo](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) para identificar problemas.  Problemas comuns se relacionam com as suposições de aplicativos sobre as permissões de caminho de diretório e o programa de trabalho.

### <a name="using-process-monitor-to-identify-an-issue"></a>Usando o Monitor de processo para identificar um problema

[Monitor de processo](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) é um utilitário poderoso para observando o arquivo de um aplicativo e as operações do registro e seus resultados.  Isso pode ajudar você a compreender os problemas de compatibilidade de aplicativo.  Depois de abrir o Monitor de processo, adicionar um filtro (filtro > filtro …) para incluir somente os eventos o executável do aplicativo.

![Filtro de App ProcMon](images/desktop-to-uwp/procmon_app_filter.png)

Aparecerá uma lista de eventos. Para muitos desses eventos, a palavra **sucesso** será exibida na coluna **Result** .

![Eventos de ProcMon](images/desktop-to-uwp/procmon_events.png)

Opcionalmente, você pode filtrar eventos para mostrar apenas somente falhas.

![Excluir ProcMon sucesso](images/desktop-to-uwp/procmon_exclude_success.png)

Se você suspeitar de uma falha de acesso do sistema de arquivos, procure falhas de eventos que estão sob o System32/SysWOW64 ou o caminho do arquivo de pacote. Filtros também podem ajudar aqui, muito. Iniciar na parte inferior desta lista e role para cima. Falhas que aparecem na parte inferior desta lista ocorreram mais recentemente. Preste atenção a maioria dos erros que contêm as cadeias de caracteres, como "acesso negado" e "nome do caminho não encontrado" e ignorar coisas que não parecem suspeitas. O [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/) tem dois problemas. Você pode ver esses problemas na lista que aparece na imagem a seguir.

![Txt ProcMon](images/desktop-to-uwp/procmon_config_txt.png)

Na primeira edição exibida nesta imagem, o aplicativo está falhando ao ler o arquivo "Txt" que está localizado no caminho "C:\Windows\SysWOW64". É improvável que o aplicativo está tentando fazer referência a esse caminho diretamente. Provavelmente, ele está tentando ler de arquivo usando um caminho relativo e por padrão, "System32/SysWOW64" é o diretório de trabalho do aplicativo. Isso sugere que o aplicativo está esperando sua pasta de trabalho atual seja definida como em algum lugar no pacote. Procurando dentro do appx, podemos ver o arquivo existe no mesmo diretório que o arquivo executável.

![App txt](images/desktop-to-uwp/psfsampleapp_config_txt.png)

O segundo problema aparece na imagem a seguir.

![Arquivo de log ProcMon](images/desktop-to-uwp/procmon_logfile.png)

Nesta edição, o aplicativo está falhando gravar um arquivo de log em seu caminho de pacote. Isso seria sugerir que pode ajudar a uma correção de redirecionamento de arquivo.

<a id="find" />

## <a name="find-a-runtime-fix"></a>Encontre uma correção de tempo de execução

O PSF contém correções de tempo de execução que você pode usar conveniente, como a correção de redirecionamento de arquivo.

### <a name="file-redirection-shim"></a>Correção de redirecionamento de arquivo

Você pode usar o [Arquivo de correção de redirecionamento](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim) para redirecionar as tentativas de gravar ou ler dados em um diretório que não está acessível a partir de um aplicativo que é executado em um contêiner MSIX.

Por exemplo, se seu aplicativo grava um arquivo de log que esteja no mesmo diretório que seus aplicativos executáveis, você pode usar o [Arquivo de correção de redirecionamento](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim) para criar esse arquivo de log em outro local, como o armazenamento de dados de aplicativo local.

### <a name="runtime-fixes-from-the-community"></a>Correções de tempo de execução da comunidade

Certifique-se de revisar as contribuições da comunidade à nossa página [GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop) . É possível que outros desenvolvedores têm resolve um problema semelhante ao seu e compartilharam uma correção de tempo de execução.

## <a name="apply-a-runtime-fix"></a>Aplicar uma correção de tempo de execução

Você pode aplicar uma correção de tempo de execução existente com algumas ferramentas simples do SDK do Windows e seguindo estas etapas.

> [!div class="checklist"]
> * Crie uma pasta de layout de pacote
> * Obter os arquivos de estrutura de suporte do pacote
> * Adicioná-los ao seu pacote
> * Modificar o manifesto do pacote
> * Criar um arquivo de configuração

Vamos analisar cada tarefa.

### <a name="create-the-package-layout-folder"></a>Criar a pasta de layout de pacote

Se já tiver um arquivo de .appx, será possível descompactar seu conteúdo em uma pasta de layout que servirá como a área de transferência para o pacote.  Você pode fazer isso de uma **x64 Prompt de comando ferramentas nativas para 2017 VERSUS**, ou manualmente com o caminho de bin SDK no caminho de pesquisa executável.

```
makeappx unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.appx /d PackageContents

```

Isso fornecerá a você algo parecido com o seguinte.

![Layout do pacote](images/desktop-to-uwp/package_contents.png)

Se você não possui um arquivo .appx começar com, você pode criar a pasta de pacote e os arquivos do zero.

### <a name="get-the-package-support-framework-files"></a>Obter os arquivos de estrutura de suporte do pacote

Você pode obter o pacote PSF Nuget usando Visual Studio. Você também pode obtê-lo usando a ferramenta de linha de comando do Nuget autônomo.

#### <a name="get-the-package-by-using-visual-studio"></a>Obter o pacote usando o Visual Studio

No Visual Studio, o nó de solução ou projeto do mouse em e selecione um dos comandos Manage Nuget Packages.  Procurar **Microsoft.PackageSupportFramework** ou **PSF** localizar o pacote em Nuget.org. Em seguida, instalá-lo.

#### <a name="get-the-package-by-using-the-command-line-tool"></a>Obtenha o pacote usando a ferramenta de linha de comando

Instalar a ferramenta de linha de comando do Nuget deste local: https://www.nuget.org/downloads. Em seguida, na linha de comando Nuget, execute este comando:

```
nuget install Microsoft.PackageSupportFramework
```

### <a name="add-the-package-support-framework-files-to-your-package"></a>Adicione os arquivos de estrutura de suporte do pacote ao seu pacote

Adicione as DLLs de PSF de 32 bits e 64 bits necessário e os arquivos executáveis para o diretório de pacote. Use a tabela a seguir como guia. Você também desejará incluir quaisquer correções de tempo de execução que você precisa. No nosso exemplo, precisamos a correção de tempo de execução de redirecionamento de arquivo.

| Aplicativo executável é x64 | Aplicativo executável é x86 |
|-------------------------------|-----------|
| [ShimLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |  [ShimLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |
| [ShimRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) | [ShimRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) |
| [ShimRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) | [ShimRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) |

O conteúdo do pacote agora deve ser semelhante isso.

![Pacote de binários](images/desktop-to-uwp/package_binaries.png)

### <a name="modify-the-package-manifest"></a>Modificar o manifesto do pacote

Abra seu manifesto do pacote em um editor de texto e, em seguida, defina o `Executable` atributo do `Application` elemento com o nome do arquivo executável correção iniciador.  Se você souber a arquitetura de seu aplicativo de destino, selecione a versão apropriada, ShimLauncher32.exe ou ShimLauncher64.exe.  Caso contrário, ShimLauncher32.exe funcionará em todos os casos.  Veja um exemplo.

```xml
<Package ...>
  ...
  <Applications>
    <Application Id="PSFSample"
                 Executable="ShimLauncher32.exe"
                 EntryPoint="Windows.FullTrustApplication">
      ...
    </Application>
  </Applications>
</Package>
```

### <a name="create-a-configuration-file"></a>Criar um arquivo de configuração

Criar um nome de arquivo ``config.json``e salve-o para a pasta raiz do seu pacote. Modifica a ID de aplicativo declarado do arquivo config.json para apontar para o executável que você acabou de ser substituído. Usando o conhecimento que você obteve do usando o Monitor de processo, você pode também definir o diretório de trabalho, bem como usar a correção de redirecionamento de arquivo para redirecionar leituras/gravações para arquivos. log no diretório "PSFSampleApp" pacote relativa.

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
            "shims": [
                {
                    "dll": "FileRedirectionShim.dll",
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
A seguir está um guia para o esquema de config.json:

| Matriz | key | Valor |
|-------|-----------|-------|
| applications | id |  Use o valor do `Id` atributo do `Application` elemento no manifesto do pacote. |
| applications | executável | O caminho relativo do pacote para o executável que você deseja iniciar. Na maioria dos casos, você pode obter esse valor do seu arquivo de manifesto de pacote, antes de modificá-lo. É o valor do `Executable` atributo do `Application` elemento. |
| applications | workingDirectory | (Opcional) Um caminho relativo ao pacote a ser usada como a pasta de trabalho do aplicativo que inicia. Se você não definir esse valor, o sistema operacional usa o `System32` diretório como o diretório de trabalho do aplicativo. |
| processos | executável | Na maioria dos casos, esse será o nome do `executable` configurado acima com a extensão de arquivo e caminho removida. |
| correções | DLL | Caminho relativo do pacote para a correção .appx para carregar. |
| correções | config | (Opcional) Controla como o dl correção se comporta. O formato exato desse valor varia em uma base de correção pela correção conforme cada correção pode interpretar essa "blob" quanto desejar. |

O `applications`, `processes`, e `shims` chaves são matrizes. Isso significa que você pode usar o arquivo config.json para especificar mais de um aplicativo, processo e DLL corretiva.


### <a name="package-and-test-the-app"></a>O pacote e o aplicativo de teste

Em seguida, crie um pacote.

```
makeappx pack /d PackageContents /p PSFSamplePackageFixup.appx
```

Em seguida, assiná-lo.

```
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.appx
```

Para obter mais informações, consulte [como criar um pacote de certificado de assinatura](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate) e [como assinar um pacote usando signtool](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool)

Usando o PowerShell, instale o pacote.

>[!NOTE]
> Lembre-se primeiro, desinstalar o pacote.

```
powershell Add-AppxPackage .\PSFSamplePackageFixup.appx
```

Executar o aplicativo e observar o comportamento com aplicadas de correção de tempo de execução.  Repita as etapas de empacotamento conforme necessário e de diagnóstico.

### <a name="use-the-trace-shim"></a>Use a correção de rastreamento

Uma técnica alternativa para diagnosticar problemas de compatibilidade de aplicativo de pacote é usar a correção de rastreamento. Esta DLL está incluído com o PSF e fornece uma visão detalhada de diagnóstico de comportamento do aplicativo, semelhante ao processo de Monitor.  Ele é especialmente projetado para revelar problemas de compatibilidade de aplicativo.  Para usar a correção de rastreamento, adicionar a DLL para o pacote, adicionar o fragmento a seguir ao seu config.json e então empacotar e instalar um aplicativo.

```json
{
    "dll": "TraceShim.dll",
    "config": {
        "traceLevels": {
            "filesystem": "allFailures"
        }
    }
}
```

Por padrão, a correção de rastreamento filtra os falhas que podem ser consideradas "esperadas".  Por exemplo, aplicativos podem tentar fazer incondicionalmente excluir um arquivo sem verificar se ele já existir, ignorando o resultado. Isso tem a consequência uma pena que algumas falhas inesperadas podem obter filtradas, no exemplo acima, podemos aceitar para receber todas as falhas de funções de sistema de arquivos. Fazemos isso porque sabemos de antes que a tentativa de ler o arquivo txt falhar com a mensagem "arquivo não encontrado". Esta é uma falha que é frequentemente observada e geralmente não considerada inesperada. Na prática é melhor provavelmente começam com filtragem somente a falhas inesperadas e, em seguida, voltando para todas as falhas se não houver um problema que ainda não puder ser identificado.

Por padrão, a saída de correção de rastreamento é enviada para o depurador anexado. Nesse exemplo, não estão indo para anexar um depurador e usará, em vez disso, o programa [DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview) da SysInternals para exibir o arquivo de saída. Depois de executar o aplicativo, podemos ver as falhas mesmas como antes, qual seria apontem conosco para as mesmas correções de tempo de execução.

![TraceShim arquivo não encontrado](images/desktop-to-uwp/traceshim_filenotfound.png)

![TraceShim acesso negado](images/desktop-to-uwp/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>Depurar, estender ou criar uma correção de tempo de execução

Você pode usar o Visual Studio para depurar uma correção de tempo de execução, estender uma correção de tempo de execução ou criar uma desde o início. Você precisará fazer essas coisas seja bem-sucedido.

> [!div class="checklist"]
> * Adicione um projeto de empacotamento
> * Adicionar o projeto para a correção de tempo de execução
> * Adicionar um projeto que inicia o iniciador de correção executável
> * Configurar o projeto de empacotamento

Quando terminar, sua solução terá uma aparência semelhante a esta.

![Solução concluída](images/desktop-to-uwp/runtime-fix-project-structure.png)

Vamos examinar cada projeto neste exemplo.

| Projeto | Finalidade |
|-------|-----------|
| DesktopApplicationPackage | Esse projeto baseia-se no [projeto de empacotamento de aplicativos do Windows](desktop-to-uwp-packaging-dot-net.md) e ele emite o pacote do MSIX. |
| Runtimefix | Esse é um projeto de biblioteca de Dynamic-Linked C++ que contém uma ou mais funções de substituição que servem como a correção de tempo de execução. |
| ShimLauncher | Esse é o projeto vazio de C++. Esse projeto é um lugar para coletar os arquivos de tempo de execução distribuível do pacote de suporte Framework. Ele emite um arquivo executável. Esse executável é a primeira coisa que é executado quando você inicia a solução. |
| WinFormsDesktopApplication | Esse projeto contém o código-fonte de um aplicativo de área de trabalho. |

Para ver um exemplo completo que contém todos esses tipos de projetos, consulte [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/).

Vamos examinar as etapas para criar e configurar cada um desses projetos em sua solução.


### <a name="create-a-package-solution"></a>Criar uma solução de pacote

Se você ainda não tiver uma solução para o seu aplicativo de área de trabalho, crie uma nova **Solução em branco** no Visual Studio.

![Solução em branco](images/desktop-to-uwp/blank-solution.png)

Convém também adicionar projetos qualquer aplicativo que você tem.

### <a name="add-a-packaging-project"></a>Adicione um projeto de empacotamento

Se você ainda não tiver um **Projeto de empacotamento de aplicativo do Windows**, crie um e adicioná-lo à sua solução.

![Modelo de projeto de pacote](images/desktop-to-uwp/package-project-template.png)

Para obter mais informações sobre o projeto de empacotamento de aplicativo do Windows, consulte [pacote seu aplicativo usando o Visual Studio](desktop-to-uwp-packaging-dot-net.md).

No **Solution Explorer**, clique com botão direito no projeto de empacotamento, selecione **Editar**e, em seguida, adicione-à parte inferior do arquivo de projeto:

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

### <a name="add-project-for-the-runtime-fix"></a>Adicionar o projeto para a correção de tempo de execução

Adicione um projeto de **Biblioteca de vínculo dinâmico (DLL)** do C++ para a solução.

![Biblioteca de correção de tempo de execução](images/desktop-to-uwp/runtime-fix-library.png)

Botão direito do mouse que project e escolha **Propriedades**.

Nas páginas de propriedade, encontre o campo **Padrão de idioma do C++** e na lista suspensa ao lado do campo, selecione o **padrão de 17 ISO C + + (/ std:c + + 17)** opção.

![ISO 17 opção](images/desktop-to-uwp/iso-option.png)

Clique com botão direito desse projeto e, no menu de contexto, escolha a opção **Manage Nuget Packages** . Certifique-se de que a opção de **origem de pacote** é definida como **tudo** ou **nuget.org**.

Clique no ícone configurações Avançar nesse campo.

Procure o *PSF** Nuget empacotar e instalá-lo para esse projeto.

![pacote do NuGet](images/desktop-to-uwp/psf-package.png)

Se desejar depurá-lo ou estender uma correção existente do tempo de execução, adicione os arquivos de correção de runtime obtido usando as orientações descritas na seção [Localizar uma correção de tempo de execução](#find) deste guia.

Se você pretende criar uma nova correção, não adicione nada a este projeto ainda. Podemos vai ajudá-lo a adicionar os arquivos direita a este projeto neste manual. Agora, podemos vai continuar a configurar sua solução.

### <a name="add-a-project-that-starts-the-shim-launcher-executable"></a>Adicionar um projeto que inicia o iniciador de correção executável

Adicione um projeto de C++ **Projeto vazio** e a solução.

![Projeto vazio](images/desktop-to-uwp/blank-app.png)

Adicione o pacote de Nuget **PSF** a este projeto usando a mesma orientações descritas na seção anterior.

Abrir as páginas de propriedades para o projeto e na página Configurações **gerais** , defina a propriedade **Name de destino** ``ShimLauncher32`` ou ``ShimLauncher64`` dependendo da arquitetura de seu aplicativo.

![referência do iniciador de correção](images/desktop-to-uwp/shim-exe-reference.png)

Adicione uma referência de projeto para o projeto de correção de tempo de execução em sua solução.

![referência de correção de tempo de execução](images/desktop-to-uwp/reference-fix.png)

A referência do mouse em e, em seguida, na janela **Propriedades** , aplicar esses valores.

| Propriedade | Value |
|-------|-----------|-------|
| Copiar local | True |
| Copie os Assemblies de satélite Local | True |
| Saída de Assembly de referência | True |
| Dependências de biblioteca de link | False |
| Entradas de dependência de biblioteca de link | False |

### <a name="configure-the-packaging-project"></a>Configurar o projeto de empacotamento

No projeto de empacotamento, clique com botão direito na pasta **Aplicativos** e escolha **Adicionar referência**.

![Adicionar referência de projeto](images/desktop-to-uwp/add-reference-packaging-project.png)

Escolha o projeto do iniciador de correção e seu projeto de aplicativo de área de trabalho e escolha o botão **Okey** .

![Projeto de desktop](images/desktop-to-uwp/package-project-references.png)

>[!NOTE]
> Se você não tiver o código-fonte ao seu aplicativo, escolha apenas projeto correção iniciador. Mostraremos a você como fazer referência a seu executável quando você cria um arquivo de configuração.

No nó **aplicativos** , clique no aplicativo iniciador de correção e escolha **Definir como ponto de entrada**.

![Definir ponto de entrada](images/desktop-to-uwp/set-startup-project.png)

Adicionar um arquivo chamado ``config.json`` ao seu projeto de empacotamento, em seguida, copie e cole o seguinte texto json para o arquivo. Defina a propriedade **Action do pacote** para o **conteúdo**.

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
            "shims": [
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
| applications | id |  Use o valor do `Id` atributo do `Application` elemento no manifesto do pacote. |
| applications | executável | O caminho relativo do pacote para o executável que você deseja iniciar. Na maioria dos casos, você pode obter esse valor do seu arquivo de manifesto de pacote, antes de modificá-lo. É o valor do `Executable` atributo do `Application` elemento. |
| applications | workingDirectory | (Opcional) Um caminho relativo ao pacote a ser usada como a pasta de trabalho do aplicativo que inicia. Se você não definir esse valor, o sistema operacional usa o `System32` diretório como o diretório de trabalho do aplicativo. |
| processos | executável | Na maioria dos casos, esse será o nome do `executable` configurado acima com a extensão de arquivo e caminho removida. |
| correções | DLL | Caminho relativo do pacote para a correção de DLL para carregar. |
| correções | config | (Opcional) Controla como o dl correção se comporta. O formato exato desse valor varia em uma base de correção pela correção conforme cada correção pode interpretar essa "blob" quanto desejar. |

Quando terminar, sua ``config.json`` arquivo terá uma aparência semelhante isso.

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
      "shims": [ { "dll": "RuntimeFix.dll" } ]
    }
  ]
}

```

>[!NOTE]
> O `applications`, `processes`, e `shims` chaves são matrizes. Isso significa que você pode usar o arquivo config.json para especificar mais de um aplicativo, processo e DLL corretiva.

### <a name="debug-a-runtime-fix"></a>Depurar uma correção de tempo de execução

No Visual Studio, pressione F5 para iniciar o depurador.  A primeira coisa que inicia é o aplicativo de iniciador correção, que por sua vez, inicia o aplicativo de destino da área de trabalho.  Para depurar o aplicativo de destino da área de trabalho, você precisará anexar manualmente ao processo do aplicativo da área de trabalho, escolhendo **Depurar**->**Attach to Process**e selecionando o processo do aplicativo. Para permitir a depuração de um aplicativo do .NET com uma correção runtime nativo DLL, selecione os tipos de código gerenciado e nativo (depuração de modo misto).  

Depois que você tiver configurado isso, você pode definir pontos de quebra ao lado de linhas de código no código do aplicativo de área de trabalho e o projeto de correção de tempo de execução. Se você não tiver o código-fonte ao seu aplicativo, você poderá definir pontos de quebra ao lado de linhas de código em seu projeto de correção de tempo de execução.

>[!NOTE]
> Enquanto o Visual Studio oferece a você o desenvolvimento mais simples e experiência de depuração, há algumas limitações, isso mais tarde neste guia, abordaremos outras que você pode aplicar técnicas de depuração.

## <a name="create-a-runtime-fix"></a>Criar uma correção de tempo de execução

Se não há ainda um tempo de execução corrigir o problema que você deseja resolver, você pode criar uma nova correção de tempo de execução, gravando funções de substituição e incluindo quaisquer dados de configuração que faz sentido. Vamos examinar cada parte.

### <a name="replacement-functions"></a>Funções de substituição

Primeiro, identifique qual função chamadas falham quando seu aplicativo é executado em um contêiner MSIX. Em seguida, você pode criar funções de substituição que você gostaria que o Gerenciador de tempo de execução para chamar em vez disso. Isso lhe dá a oportunidade de substituir a implementação de uma função com comportamento que está em conformidade com as regras do ambiente de tempo de execução moderno.

No Visual Studio, abra o projeto de correção de tempo de execução que você criou anteriormente neste guia.

Declarar o ``SHIM_DEFINE_EXPORTS`` macro e adicione uma instrução include para o `shim_framework.h` na parte superior de cada uma delas. Arquivo CPP onde você pretende adicionar as funções de sua correção de tempo de execução.

```c++
#define SHIM_DEFINE_EXPORTS
#include <shim_framework.h>
```
>[!IMPORTANT]
>Certifique-se de que o `SHIM_DEFINE_EXPORTS` macro aparece antes da instrução include.

Criar uma função que possui a mesma assinatura da função que tem comportamento que você deseja modificar. Aqui está uma função de exemplo que substitui o `MessageBoxW` função.

```c++
auto MessageBoxWImpl = &::MessageBoxW;
int WINAPI MessageBoxWShim(
    _In_opt_ HWND hwnd,
    _In_opt_ LPCWSTR,
    _In_opt_ LPCWSTR caption,
    _In_ UINT type)
{
    return MessageBoxWImpl(hwnd, L"SUCCESS: This worked", caption, type);
}

DECLARE_SHIM(MessageBoxWImpl, MessageBoxWShim);
```

A chamada para `DECLARE_SHIM` mapeia o `MessageBoxW` função para sua nova função de substituição. Quando seu aplicativo tenta chamar o `MessageBoxW` função, ele chamará a função de substituição em vez disso.

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>Protege contra chamadas recursivas às funções em tempo de execução correções

Opcionalmente, você pode aplicar o `reentrancy_guard` tipo de suas funções de proteger contra chamadas recursivas às funções em correções de tempo de execução.

Por exemplo, você pode produzir uma função de substituição para o `CreateFile` função. Sua implementação chamaria o `CopyFile` função, mas a implementação do `CopyFile` função pode chamar o `CreateFile` função. Isso pode levar a um ciclo recursiva infinita de chamadas para o `CreateFile` função.

Para obter mais informações sobre `reentrancy_guard` consulte [authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### <a name="configuration-data"></a>Dados de configuração

Se você deseja adicionar dados de configuração para a correção de tempo de execução, considere a possibilidade de adicioná-lo para o ``config.json``. Dessa forma, você pode usar o `ShimQueryCurrentDllConfig` facilmente analisar dados. Este exemplo analisa um valor boolean e a cadeia de caracteres do arquivo de configuração.

```c++
if (auto configRoot = ::ShimQueryCurrentDllConfig())
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

Enquanto o Visual Studio dá a você o desenvolvimento mais simples e a experiência de depuração, há algumas limitações.

Em primeiro lugar, a depuração F5 executa o aplicativo arquivos afastados do caminho de pasta de layout do pacote de implantação, ao invés de instalação a partir de um pacote de .appx.  A pasta de layout normalmente não tem as mesmas restrições de segurança como uma pasta de pacote instalado. Consequentemente, talvez não seja possível reproduzir erros de negação de acesso de caminho de pacote antes de aplicar uma correção de tempo de execução.

Para resolver esse problema, use a implantação do pacote de .appx em vez de implantação do arquivo afastado F5.  Para criar um arquivo de pacote .appx, use o utilitário de [MakeAppx](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) do SDK do Windows, conforme descrito acima. Ou, de dentro do Visual Studio, o nó do seu projeto de aplicativo do mouse em e selecione **repositório**->**Criar pacotes do App**.

Outro problema com o Visual Studio é que ele não tem suporte interno para anexar a qualquer processos filho iniciados pelo depurador.   Isso dificulta depurar lógica no caminho de inicialização do aplicativo de destino, que deve ser anexado manualmente pelo Visual Studio depois do lançamento.

Para resolver esse problema, use um depurador que ofereça suporte a anexar processo filho.  Observe que geralmente não é possível anexar um depurador do just-in-time (JIT) para o aplicativo de destino.  Isso ocorre porque a maioria das técnicas JIT envolvem iniciando o depurador no lugar do aplicativo de destino, por meio da chave do registro ImageFileExecutionOptions.  Isso anula o mecanismo de detouring usado pelo ShimLauncher.exe inserir ShimRuntime.dll no aplicativo de destino.  WinDbg, incluído nas [Ferramentas de depuração para Windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)e obtidas do [SDK do Windows](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk), que suporta filho processo anexar.  Ele também suporta diretamente [Lançar e depurando um aplicativo UWP](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app).

Para depurar a inicialização do aplicativo de destino como um processo de filho, inicie ``WinDbg``.

```
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

No ``WinDbg`` Avisar, habilitar filho depuração e definir pontos de interrupção apropriados.

```
.childdbg 1
g
```
(executar até que o aplicativo de destino inicia e entra no depurador)

```
sxe ld fixup.dll
g
```
(executa até a correção de que dll é carregada)

```
bp ...
```

>[!NOTE]
> [PLMDebug](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/plmdebug) também pode ser usado para anexar um depurador a um aplicativo ao ser iniciado e também pode ser incluído nas [Ferramentas de depuração para Windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index).  No entanto, é mais complexo de usar do que o suporte direto agora fornecido pelo WinDbg.

## <a name="support-and-feedback"></a>Suporte e comentários

**Encontrar respostas para suas dúvidas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).
