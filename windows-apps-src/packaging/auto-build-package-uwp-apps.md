---
author: laurenhughes
title: Configurar compilações automáticas para seu aplicativo UWP
description: Como configurar compilações automáticas para produzir pacotes de sideload e/ou armazenamento da Loja.
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: 7492f9d4fc2111880f27dcb6a48eff3ad0ccd315
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4432592"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>Configurar compilações automáticas para seu app UWP

Você pode usar o Visual Studio Team Services (VSTS) para criar compilações automáticas para projetos UWP. Neste artigo, vamos analisar algumas maneiras diferentes de fazer isso.  Também mostraremos como executar essas tarefas usando a linha de comando para que você possa integrar com outros sistemas de compilação como o AppVeyor. 

## <a name="select-the-right-type-of-build-agent"></a>Selecione o tipo certo de agente de compilação

Escolha o tipo de agente de compilação que você deseja que o VSTS use ao executar o processo de compilação. Um agente de compilação hospedado é implantado com as ferramentas e sdks mais comuns, e funciona para a maioria dos cenários. Consulte o artigo [Software no servidor de compilação hospedado](https://www.visualstudio.com/docs/build/admin/agents/hosted-pool#software). No entanto, você pode criar um agente de compilação personalizado se precisar de mais controle sobre as etapas de criação. Você pode usar a tabela a seguir para ajudá-lo a tomar essa decisão.

| **Cenário** | **Agente personalizado** | **Agente de compilação hospedado** |
|-------------|----------------|----------------------|
| Compilação UWP básica (incluindo o .NET Native)| :white_check_mark: | :white_check_mark: |
| Gerar pacotes para sideload| :white_check_mark: | :white_check_mark: |
| Gerar pacotes para envio à Loja| :white_check_mark: | :white_check_mark: |
| Usar certificados personalizados| :white_check_mark: | |
| Criar direcionamento para um SDK do Windows personalizado| :white_check_mark: |  |
| Executar testes de unidade| :white_check_mark: |  |
| Usar compilações incrementais| :white_check_mark: |  |

#### <a name="create-a-custom-build-agent-optional"></a>Criar um agente de compilação personalizado (opcional)

Se você optar por criar um agente de compilação personalizado, precisará das ferramentas da Plataforma Universal do Windows. Essas ferramentas fazem parte do Visual Studio. Você pode usar a edição da comunidade do Visual Studio.

Para saber mais, consulte [Implantar um agente no Windows](https://www.visualstudio.com/docs/build/admin/agents/v2-windows). 

Para executar testes de unidade UWP, você precisará fazer o seguinte: 
- Implantar e iniciar seu aplicativo 
- Executar o agente VSTS no modo interativo 
- Configurar seu agente para logon automático após uma reinicialização 

## <a name="set-up-an-automated-build"></a>Configurar uma compilação automática
Vamos começar com a definição padrão de compilação de UWP que está disponível no VSTS e, em seguida, mostraremos como configurar essa definição para que você possa realizar tarefas de compilação mais avançadas.

**Adicionar o certificado do seu projeto a um repositório de código-fonte**

O VSTS funciona com TFS e GIT com base em repositórios de código.  
Se você usar um repositório de Git, adicione o arquivo de certificado do seu projeto ao repositório para que o agente de compilação possa assinar o pacote do aplicativo. Se você não fizer isso, o repositório de Git ignorará o arquivo de certificado. Para adicionar o arquivo de certificado ao seu repositório, clique com o botão direito no arquivo de certificado no Gerenciador de soluções e, em seguida, no menu de atalho, escolha o comando Adicionar Arquivo Ignorado ao Controle de Origem. 

![como incluir um certificado](images/building-screen1.png)

Falaremos sobre [gerenciamento avançado de certificados](#certificates-best-practices) mais adiante neste guia. 

Para criar sua primeira definição de compilação no VSTS, navegue até a guia Compilações e, em seguida, selecione o botão +.

![criar definição de compilação](images/building-screen2.png)

Na lista de modelos de definição de compilação, escolha o modelo *Plataforma Universal do Windows*.

![selecionar a compilação de UWP](images/building-screen3.png)

Essa definição de compilação contém as seguintes tarefas de compilação:

- Restauração do NuGet **\*.sln
- Compilar solução **\*.sln
- Publicar símbolos
- Publicar artefato: soltar

#### <a name="configure-the-nuget-restore-build-task"></a>Configurar a tarefa de compilação de restauração do NuGet

Essa tarefa restaura os pacotes NuGet que são definidos em seu projeto. Alguns pacotes requerem uma versão personalizada do NuGet.exe. Se você estiver usando um pacote que precisa de uma, consulte essa versão NuGet.exe em seu repositório e faça referência a ela na propriedade avançada *Caminho até NuGet.exe*.

![definição de compilação padrão](images/building-screen4.png)

#### <a name="configure-the-build-solution-build-task"></a>Configurar a tarefa de compilação Compilar solução

Essa tarefa compila qualquer solução que esteja na pasta de trabalho para binários e produz o arquivo de pacote de aplicativo de saída. Essa tarefa usa argumentos MSbuild.  Você precisará especificar o valor desses argumentos. Use a tabela a seguir como guia. 

|**Argumento MSBuild**|**Valor**|**Descrição**|
|--------------------|---------|---------------|
|AppxPackageDir|$(Build.ArtifactStagingDirectory)\AppxPackages|Define a pasta para armazenar os artefatos gerados.|
|AppxBundlePlatforms|$(Build.BuildPlatform)|Permite que você defina as plataformas a serem incluídas no pacote.|
|AppxBundle|Sempre|Cria um appxbundle com os arquivos appx para a plataforma especificada.|
|**UapAppxPackageBuildMode**|StoreUpload|Define o tipo de pacote do aplicativo a ser gerado. (Não incluído por padrão)|


Se você quiser desenvolver sua solução usando a linha de comando ou usando qualquer outro sistema de compilação, execute msbuild com esses argumentos.

```
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"  
/p:UapAppxPackageBuildMode=StoreUpload 
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

Os parâmetros definidos com a sintaxe $() são variáveis definidas na definição da compilação e mudarão em outros sistemas de compilação.

![variáveis padrão](images/building-screen5.png)

Para ver todas as variáveis predefinidas, consulte [Usar variáveis de compilação](https://www.visualstudio.com/docs/build/define/variables).

#### <a name="configure-the-publish-artifact-build-task"></a>Configurar a tarefa de compilação Publicar artefato 
Essa tarefa armazena os artefatos gerados no VSTS. Você pode vê-los na guia Artefatos da página de resultados da compilação. O VSTS usa a pasta `$(Build.ArtifactStagingDirectory)\AppxPackages` que definimos anteriormente.

![artefatos](images/building-screen6.png)

Como definimos a propriedade `UapAppxPackageBuildMode` para `StoreUpload`, a pasta de artefatos inclui o pacote recomendado para envio à Loja (.appxupload). Observe que você também pode enviar um pacote de Apps normal (.appx/.msix) ou um lote de aplicativo (.appxbundle/.msixbundle) para a loja. Para os fins deste artigo, vamos usar o arquivo .appxupload.


>[!NOTE]
> Por padrão, o agente de VSTS mantém os últimos pacotes de aplicativo gerados. Se você quiser armazenar somente os artefatos da compilação atual, configure a compilação para limpar o diretório de binários. Para fazer isso, adicione uma variável chamada `Build.Clean` e defina-a como o valor `all`. Para saber mais, consulte [Especificar o repositório](https://www.visualstudio.com/docs/build/define/repository#how-can-i-clean-the-repository-in-a-different-way).

#### <a name="the-types-of-automated-builds"></a>Os tipos de compilações automáticas
Em seguida, você usará sua definição de compilação para criar uma compilação automática. A tabela a seguir descreve cada tipo de compilação automática que você pode criar. 

|**Tipo de compilação**|**Artefato**|**Frequência recomendada**|**Descrição**|
|-----------------|------------|-------------------------|---------------|
|Integração contínua|Criar Log, Resultados de Teste|Cada confirmação|Esse tipo de compilação é rápido e executado várias vezes ao dia.|
|Compilação de implantação contínua para sideload|Pacotes de Implantação|Diário |Esse tipo de compilação pode incluir testes de unidade, mas demora um pouco mais. Ele permite o teste manual e você pode integrá-lo a outras ferramentas como o HockeyApp.|
|Compilação de implantação contínua que envia um pacote para a Loja|Pacotes de Publicação|Sob demanda|Esse tipo de compilação cria um pacote que você pode publicar na Loja.|

Vejamos como configurar cada um.


## <a name="set-up-a-continuous-integration-ci-build"></a>Configurar uma compilação de integração contínua (CI) 
Esse tipo de compilação ajuda você a diagnosticar problemas relacionados a código rapidamente. Normalmente, elas são executadas apenas para uma plataforma e não precisam ser processadas pela cadeia de ferramentas do .NET Native. Além disso, com compilações CI, você pode executar testes de unidade que produzem um relatório de resultados de teste.  

Se você quiser executar testes de unidade UWP como parte de sua compilação CI, precisará usar um agente de compilação personalizado em vez do agente de compilação hospedado.

>[!NOTE]
> Se você incluir mais de um aplicativo na mesma solução, poderá receber um erro. Consulte o seguinte tópico de ajuda para resolver esse erro: [Corrigir erros que aparecem quando você inclui mais de um aplicativo na mesma solução](#bundle-errors). 


### <a name="configure-a-ci-build-definition"></a>Configurar uma definição de compilação CI
Use o modelo UWP padrão para criar uma definição de compilação. Em seguida, configure o gatilho a ser executado em cada check-in.  

![gatilho CI](images/building-screen7.png)

Como a compilação CI não será implantada em usuários, é uma boa ideia manter números diferentes de controle de versão para evitar confusão com compilações de CD. Por exemplo:
`$(BuildDefinitionName)_0.0.$(DayOfYear)$(Rev:.r)`


#### <a name="configure-a-custom-build-agent-for-unit-testing"></a>Configurar um agente de compilação personalizado para testes de unidade

1. Habilite o modo de desenvolvedor em seu computador. Consulte [Habilitar seu dispositivo para desenvolvimento](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) para obter mais informações. 
2. Ative o serviço a ser executado como um processo interativo. Para saber mais, consulte [Implantar um agente no Windows](https://docs.microsoft.com/vsts/build-release/actions/agents/v2-windows). 
3. Implante o certificado de autenticação para o agente.

Para implantar um certificado de autenticação, clique duas vezes no arquivo `.cer`, escolha **Computador local** e, depois, **Repositório de pessoas confiáveis**.

<span id="uwp-unit-tests" />

### <a name="configure-the-build-definition-to-run-uwp-unit-tests"></a>Configurar a definição de compilação para executar testes de unidade UWP
Para executar um teste de unidade, use a etapa de compilação Teste do Visual Studio.


![adicionar testes de unidade](images/building-screen8.png)

Os testes de unidade UWP são executados no contexto de um arquivo appxrecipe determinado, de modo que você não pode usar o pacote gerado. Além disso, você precisará especificar o caminho para um arquivo appxrecipe concreto de plataforma. Por exemplo:

```
$(Build.ArtifactStagingDirectory)\AppxPackages\MyUWPApp.UnitTest\x86\MyUWPApp.UnitTest_$(AppxVersion)_x86.appxrecipe
```

Para que os testes sejam executados, um parâmetro de console precisará ser adicionado a vstest.console.exe. Esse parâmetro pode ser fornecido por meio de: **Opções de execução => Outras opções de console**. Adicione o seguinte parâmetro: 

```
/framework:FrameworkUap10
```

>[!NOTE]
> Use o seguinte comando para executar os testes de unidade localmente na linha de comando:
`"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\CommonExtensions\Microsoft\TestWindow\vstest.console.exe"`

#### <a name="access-test-results"></a>Acessar resultados de teste
No VSTS, a página de resumo de compilação mostra os resultados de teste para cada compilação que executa os testes de unidade. Aqui, você pode abrir a página **Resultados de teste** para ver mais detalhes sobre os resultados do teste. 

![resultados do teste](images/building-screen9.png)

#### <a name="improve-the-speed-of-a-ci-build"></a>Aumentar a velocidade de uma compilação CI
Se você quiser usar sua compilação CI apenas para monitorar a qualidade dos check-ins, reduza os tempos de compilação.

#### <a name="to-improve-the-speed-of-a-ci-build"></a>Para aumentar a velocidade de uma compilação CI
1.  Compile apenas para uma plataforma.
2.  Edite a variável BuildPlatform para usar somente x86. ![config ci](images/building-screen10.png) 
3.  Na etapa de compilação, adicione /p:AppxBundle=Never à propriedade Argumentos de MSBuild e, em seguida, defina a propriedade Plataforma. ![configurar a plataforma](images/building-screen11.png)
4.  No projeto de teste de unidade, desabilite .NET Native. 

Para fazer isso, abra o arquivo de projeto e, nas propriedades do projeto, defina a propriedade `UseDotNetNativeToolchain` como `false`.

Usar a cadeia de ferramentas do .NET Native é uma parte importante do fluxo de trabalho e ainda deve ser usada para testar compilações. 

<span id="bundle-errors" />

#### <a name="address-errors-that-appear-when-you-bundle-more-than-one-app-in-the-same-solution"></a>Corrigir erros que aparecem quando você inclui mais de um aplicativo na mesma solução 
Se você adicionar mais de um projeto UWP à sua solução e, em seguida, tentar criar um pacote, poderá receber um erro como este: 

```
MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle
```

Esse erro é exibido porque, no nível da solução, não está claro qual aplicativo deve aparecer no pacote. Para resolver esse problema, abra cada arquivo de projeto e adicione as seguintes propriedades ao final do primeiro elemento `<PropertyGroup>`:

|**Projeto**|**Propriedades**|
|-------|----------|
|Aplicativo|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

Em seguida, remova o argumento `AppxBundle` msbuild da etapa de compilação.

## <a name="set-up-a-continuous-deployment-build-for-sideloading"></a>Configurar uma compilação de implantação contínua para sideload
Quando esse tipo de compilação é concluído, os usuários podem baixar o arquivo de lote de aplicativo da seção artefatos da página de resultados da compilação. Se quiser fazer o teste beta do aplicativo pela criação de uma distribuição mais completa, você pode usar o serviço HockeyApp. Esse serviço oferece recursos avançados para testes beta, análise do usuário e diagnóstico de falhas.

### <a name="applying-version-numbers-to-your-builds"></a>Aplicação de números de versão a compilações

O arquivo de manifesto contém o número de versão do aplicativo.  Atualize o arquivo de manifesto no seu repositório de controle de origem para alterar o número de versão. Outra maneira de atualizar o número da versão do seu aplicativo é usar o número de compilação que é gerado pelo VSTS e, em seguida, modificar o manifesto do aplicativo antes de compilar o aplicativo. Não basta confirmar essas alterações no repositório de código-fonte.

Você precisará definir o formato de número de compilação do controle de versão na definição da compilação e, em seguida, usar o número de versão resultante para atualizar o AppxManifest e, opcionalmente, os arquivos AssemblyInfo.cs antes de compilar.

Defina o formato de número de compilação na guia *Geral* de sua definição de compilação.

![versão de compilação](images/building-screen12.png) 

Por exemplo, se você definir o formato de número de compilação como o seguinte valor:
``` 
$(BuildDefinitionName)_1.1.$(DayOfYear)$(Rev:r).0 
```

O VSTS gera um número de versão como:
```
CI_MyUWPApp_1.1.2501.0
```

>[!NOTE]
>A Store exigirá que o último número na versão seja 0.

Para que você possa extrair o número de versão e aplicá-lo ao manifesto e/ou arquivos `AssemblyInfo`, use um script do PowerShell personalizado (disponível [aqui](https://go.microsoft.com/fwlink/?prd=12560&pver=14&plcid=0x409&clcid=0x9&ar=DevCenter&sar=docs)). Esse script lê o número de versão da variável de ambiente `BUILD_BUILDNUMBER` e, em seguida, modifica os arquivos AssemblyInfo e AppxManifest. Adicione esse script ao seu repositório de origem e, em seguida, configure uma tarefa de compilação do PowerShell, conforme mostrado aqui:


![versão de atualização](images/building-screen13.png) 

A variável `$(AppxVersion)` contém o número de versão. Você pode usar esse número em outras etapas de compilação. 


#### <a name="optional-integrate-with-hockeyapp"></a>Opcional: integrar a HockeyApp
Primeiro, instale a extensão [HockeyApp](https://marketplace.visualstudio.com/items?itemName=ms.hockeyapp) do Visual Studio. Você precisará instalar essa extensão como um administrador do VSTS. 

![hockey app](images/building-screen14.png) 

Em seguida, configure a conexão do HockeyApp usando este guia: [Como usar o HockeyApp com o Visual Studio Team Services (VSTS) ou o Team Foundation Server (TFS).](https://support.hockeyapp.net/kb/third-party-bug-trackers-services-and-webhooks/how-to-use-hockeyapp-with-visual-studio-team-services-vsts-or-team-foundation-server-tfs) Você pode usar sua conta da Microsoft, conta de mídia social ou apenas um endereço de email para configurar sua conta do HockeyApp. O plano gratuito vem com dois aplicativos, um proprietário e nenhuma restrição de dados.

Em seguida, você pode criar um aplicativo HockeyApp manualmente, ou carregar um arquivo de pacote de aplicativo existente. Para saber mais, consulte [Como criar um novo aplicativo](https://support.hockeyapp.net/kb/app-management-2/how-to-create-a-new-app).  

Para usar um arquivo de pacote de aplicativo existente, adicionar uma etapa de compilação e defina o parâmetro de caminho do arquivo binário da etapa de compilação. 

![configurar o hockey app](images/building-screen15.png) 

Para definir esse parâmetro, combine o nome do aplicativo, a variável AppxVersion e as plataformas com suporte em uma string como esta:

``` 
$(Build.ArtifactStagingDirectory)\AppxPackages\MyUWPApp_$(AppxVersion)_Test\MyUWPApp_$(AppxVersion)_x86_x64_ARM.appxbundle
```

Embora a tarefa HockeyApp permita que você especifique o caminho para o arquivo de símbolos, é uma prática recomendada incluir os símbolos com o pacote.

## <a name="set-up-a-continuous-deployment-build-that-submits-a-package-to-the-store"></a>Configurar uma compilação de implantação contínua que envia um pacote para a Store 

Para gerar pacotes de envio da Loja, associe seu app à Loja usando o Assistente de Associação à Loja no Visual Studio.

![associar à Store](images/building-screen16.png) 

O Assistente de Associação à Store gera um arquivo chamado Package.StoreAssociation.xml que contém as informações de associação à Store. Se você armazenar seu código-fonte em um repositório público, como o GitHub, esse arquivo irá conter todos os nomes de aplicativo reservados para essa conta. Você pode excluir esse arquivo antes de torná-lo público.

Se você não tiver acesso à conta do Centro de Desenvolvimento que foi usada para publicar o aplicativo, siga as instruções neste documento: [Criando um aplicativo para terceiros? Como empacotar seu aplicativo da Store.](https://blogs.windows.com/buildingapps/2015/12/15/building-an-app-for-a-3rd-party-how-to-package-their-store-app/#e35YzR5aRG6uaBqK.97) 

Em seguida, você precisa verificar se a etapa de compilação inclui o seguinte parâmetro:

```
/p:UapAppxPackageBuildMode=StoreUpload 
```

Isso irá gerar um arquivo de upload que pode ser enviado para a loja.


#### <a name="configure-automatic-store-submission"></a>Configurar o envio automático à Store

Use a extensão do Visual Studio Team Services para a Microsoft Store para integrar-se à API da Store e envie seu pacote de apps para a Store.

Você precisa conectar sua conta do Centro de Desenvolvimento ao Azure Active Directory (AD) e, em seguida, criar um aplicativo no AD para autenticar as solicitações. Você pode seguir as orientações na página de extensão para fazer isso. 

Depois que você tiver configurado a extensão, você pode adicionar a tarefa de compilação e configure-a com sua ID do aplicativo e o local do arquivo de upload.

![configurar o Centro de Desenvolvimento](images/building-screen17.png) 

Onde o valor do parâmetro `Package File` será:

```
$(Build.ArtifactStagingDirectory)\
AppxPackages\MyUWPApp__$(AppxVersion)_x86_x64_ARM_bundle.appxupload
```

Você precisa ativar manualmente essa compilação. Você pode usá-la para atualizar os apps existentes, mas não pode usá-la para seu primeiro envio à Store. Para saber mais, consulte [Criar e gerenciar envios à Store usando serviços da Microsoft Store](https://msdn.microsoft.com/windows/uwp/monetize/create-and-manage-submissions-using-windows-store-services).

## <a name="best-practices"></a>Práticas recomendadas

<span id="sideloading-best-practices"/>

### <a name="best-practices-for-sideloading-apps"></a>Práticas recomendadas para aplicativos de sideload

Se você quiser distribuir seu app sem publicá-lo na Loja, faça o sideload do seu aplicativo diretamente em dispositivos, contanto que os dispositivos confiem no certificado que foi usado para assinar o pacote do aplicativo. 

Use o script do PowerShell `Add-AppDevPackage.ps1` para instalar aplicativos. Esse script será adicionar o certificado à seção certificação confiável para o computador local e, em seguida, irá instalar ou atualizar o arquivo de pacote do aplicativo.

#### <a name="sideloading-your-app-with-the-windows-10-anniversary-update"></a>Sideload de seu aplicativo com a Atualização de Aniversário do Windows 10
Na atualização de aniversário do Windows 10, você pode duas vezes no arquivo de pacote do aplicativo e instalar seu aplicativo escolhendo o botão instalar em uma caixa de diálogo. 

![sideload em rs1](images/building-screen18.png) 

>[!NOTE]
> Esse método não instala o certificado nem as dependências associadas.

Se você quiser distribuir seus pacotes de aplicativo do Windows em um site como VSTS ou HockeyApp, você precisará adicionar o site à lista de sites confiáveis no navegador. Caso contrário, o Windows marca o arquivo como bloqueado. 

<span id="certificates-best-practices"/>

### <a name="best-practices-for-signing-certificates"></a>Práticas recomendadas para certificados de assinatura 
O Visual Studio gera um certificado para cada projeto. Isso dificulta manter uma lista gerenciada de certificados válidos. Se você pretende criar vários aplicativos, pode criar um único certificado para assinar todos os seus aplicativos. Em seguida, cada dispositivo que confia no certificado será capaz de fazer o sideload de qualquer um dos seus aplicativos sem precisar instalar outro certificado. Para saber mais, consulte [Criar um certificado para assinatura de pacote](https://docs.microsoft.com/windows/uwp/packaging/create-certificate-package-signing).


#### <a name="create-a-signing-certificate"></a>Criar um certificado de assinatura
Use a ferramenta [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/ff548309.aspx) para criar um certificado.

O exemplo a seguir cria um certificado usando a ferramenta MakeCert.exe.

```
MakeCert /n publisherName /r /h 0 /eku "1.3.6.1.5.5.7.3.3,1.3.6.1.4.1.311.10.3.13" /e expirationDate /sv MyKey.pvk MyKey.cer
```

Em seguida, você pode usar a ferramenta Pvk2Pfx para gerar um arquivo PFX que contém a chave privada protegida com uma senha.

Forneça esses certificados para cada função de computador:

|**Máquina**|**Utilização**|**Certificado**|**Repositório de Certificados**|
|-----------|---------|---------------|---------------------|
|Máquina de desenvolvedor/compilação|Assinar compilações|MyCert.PFX|Usuário atual/pessoal|
|Máquina de desenvolvedor/compilação|Executar|MyCert.cer|Máquina local/pessoas confiáveis|
|Usuário|Executar|MyCert.cer|Máquina local/pessoas confiáveis|

>Observação: você também pode usar um certificado corporativo que já é confiável para seus usuários.

#### <a name="sign-your-uwp-app"></a>Assinar seu aplicativo UWP
O Visual Studio e o MSBuild oferecem diversas opções para gerenciar o certificado que você usa para assinar o aplicativo:

Uma opção é incluir o certificado com a chave privada (normalmente na forma de um arquivo .PFX) em sua solução e fazer referência a pfx no arquivo do projeto. Você pode gerenciar isso usando a guia Pacote do editor de manifesto.


![criar certificado](images/building-screen19.png) 

Outra opção é instalar o certificado na máquina de compilação (Usuário atual/pessoal) e, em seguida, usar a opção Escolher no Repositório de Certificados. Isso especifica a impressão digital do certificado no arquivo de projeto para que o certificado deva ser instalado em todas as máquinas que serão usadas para compilar o projeto.

#### <a name="trust-the-signing-certificate-in-the-target-devices"></a>Confiar no certificado de assinatura em dispositivos de destino
Um dispositivo de destino precisa confiar no certificado para que o aplicativo possa ser instalado nele. 

Registre a chave pública do certificado no local Pessoas confiáveis ou Raiz de confiança no repositório de certificados do computador local.

A maneira mais rápida de registrar o certificado é clicar duas vezes no arquivo .cer e seguir as etapas no assistente para salvar o certificado em **Computador local** e repositório de **pessoas confiáveis**.

## <a name="related-topics"></a>Tópicos relacionados
* [Criar seu aplicativo .NET para Windows](https://www.visualstudio.com/docs/build/get-started/dot-net) 
* [Empacotando aplicativos UWP](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)
* [Sideload de aplicativos LOB no Windows 10](https://technet.microsoft.com/itpro/windows/deploy/sideload-apps-in-windows-10)
* [Criar um certificado para a assinatura de pacote](https://docs.microsoft.com/windows/uwp/packaging/create-certificate-package-signing)
