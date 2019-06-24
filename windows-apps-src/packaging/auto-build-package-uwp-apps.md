---
title: Configurar compilações automáticas para seu aplicativo UWP
description: Como configurar compilações automáticas para produzir pacotes de sideload e/ou armazenamento da Loja.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: 61525e2a4a088e37184bb93526722e0bf23fbd56
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319817"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>Configurar compilações automáticas para seu aplicativo UWP

Você pode usar Pipelines do Azure para criar Compilações automáticas para projetos UWP. Neste artigo, vamos examinar formas diferentes de fazer isso. Também mostraremos como executar essas tarefas usando a linha de comando para que você pode integrar com qualquer outro sistema de compilação.

## <a name="create-a-new-azure-pipeline"></a>Criar um novo Pipeline do Azure

Comece [inscrição Pipelines do Azure](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up) se você ainda não fez isso.

Em seguida, crie um pipeline que você pode usar para compilar seu código-fonte. Para obter um tutorial sobre como criar um pipeline para criar um repositório do GitHub, consulte [criar seu primeiro pipeline](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml). Pipelines do Azure dá suporte a tipos de repositório listados [neste artigo](https://docs.microsoft.com/azure/devops/pipelines/repos).

## <a name="set-up-an-automated-build"></a>Configurar uma compilação automática

Vamos começar com o padrão UWP que está disponível no Azure Dev Ops definição de compilação e, em seguida, mostrar como configurar o pipeline.

Na lista de modelos de definição de compilação, escolha o modelo **Plataforma Universal do Windows**.

![Selecione o modelo UWP](images/select-yaml-template.png)

Esse modelo inclui a configuração básica para compilar seu projeto UWP:

```yml
trigger:
- master

pool:
  vmImage: 'VS2017-Win2016'

variables:
  solution: '**/*.sln'
  buildPlatform: 'x86|x64|ARM'
  buildConfiguration: 'Release'
  appxPackageDir: '$(build.artifactStagingDirectory)\AppxPackages\\'

steps:
- task: NuGetToolInstaller@0

- task: NuGetCommand@2
  inputs:
    restoreSolution: '$(solution)'

- task: VSBuild@1
  inputs:
    platform: 'x86'
    solution: '$(solution)'
    configuration: '$(buildConfiguration)'
    msbuildArgs: '/p:AppxBundlePlatforms="$(buildPlatform)" /p:AppxPackageDir="$(appxPackageDir)" /p:AppxBundle=Always /p:UapAppxPackageBuildMode=StoreUpload'

```

O modelo padrão tenta assinar o pacote com o certificado especificado no arquivo. csproj. Se você deseja assinar seu pacote durante a compilação, você deve ter acesso à chave privada. Caso contrário, você pode desabilitar a assinatura, adicionando o parâmetro `/p:AppxPackageSigningEnabled=false` para o `msbuildArgs` seção no arquivo YAML.

## <a name="add-your-project-certificate-to-a-repository"></a>Adicionar seu certificado de projeto para um repositório

Pipelines funciona com repositórios do TFVC e Git de repositórios do Azure. Se você usar um repositório de Git, adicione o arquivo de certificado do seu projeto ao repositório para que o agente de compilação possa assinar o pacote do aplicativo. Se você não fizer isso, o repositório de Git ignorará o arquivo de certificado. Para adicionar o arquivo de certificado em seu repositório, clique no arquivo de certificado no **Gerenciador de soluções**e, em seguida, no menu de atalho, escolha o **Adicionar arquivo ignorado ao controle do código-fonte** comando.

![como incluir um certificado](images/building-screen1.png)

## <a name="configure-the-build-solution-build-task"></a>Configurar a tarefa de compilação Compilar solução

Essa tarefa é compilado em qualquer solução que está na pasta de trabalho para binários e produz o arquivo de pacote de aplicativo de saída.
Essa tarefa usa argumentos de MSBuild. Você precisará especificar o valor desses argumentos. Use a tabela a seguir como guia.

|**Argumento de MSBuild**|**Valor**|**Descrição**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | Define a pasta para armazenar os artefatos gerados. |
| AppxBundlePlatforms | $(Build.BuildPlatform) | Permite que você defina as plataformas para incluir no pacote. |
| AppxBundle | Sempre | Cria um.msixbundle/.appxbundle com os arquivos de.msix/.appx para a plataforma especificada. |
| UapAppxPackageBuildMode | StoreUpload | Gera o arquivo.msixupload/.appxupload e o **Test** pasta para o sideload. |
| UapAppxPackageBuildMode | CI | Gera o arquivo.msixupload/.appxupload apenas. |
| UapAppxPackageBuildMode | SideloadOnly | Gera o **Test** pasta para o sideload apenas |

Se você quiser criar uma solução usando a linha de comando ou por meio de qualquer outro sistema de compilação, execute o MSBuild com estes argumentos.

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

Os parâmetros definidos com o `$()` sintaxe são variáveis definidas na definição de compilação, e será compilado de alteração em outros sistemas.

![variáveis padrão](images/building-screen5.png)

Para exibir todas as variáveis predefinidas, consulte [variáveis de compilação predefinida](https://docs.microsoft.com/azure/devops/pipelines/build/variables).

## <a name="configure-the-publish-build-artifacts-task"></a>Configurar a tarefa publicar artefatos de compilação

O pipeline UWP padrão não salva os artefatos gerados. Para adicionar os recursos de publicação para sua definição YAML, adicione as seguintes tarefas.

```yml
- task: CopyFiles@2
  displayName: 'Copy Files to: $(build.artifactstagingdirectory)'
  inputs:
    SourceFolder: '$(system.defaultworkingdirectory)'
    Contents: '**\bin\$(BuildConfiguration)\**'
    TargetFolder: '$(build.artifactstagingdirectory)'

- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact: drop'
  inputs:
    PathtoPublish: '$(build.artifactstagingdirectory)'
```

Você pode ver os artefatos gerados na **artefatos** página resultados da opção de compilação.

![artifacts](images/building-screen6.png)

Porque definimos os `UapAppxPackageBuildMode` argumento para `StoreUpload`, a pasta de artefatos inclui o pacote para o envio para a Store (.msixupload/.appxupload). Observe que você também pode enviar um pacote de aplicativo regular (.msix/.appx) ou um pacote de aplicativos (.msixbundle/.appxbundle/) para a Store. Para os fins deste artigo, vamos usar o arquivo .appxupload.

## <a name="address-bundle-errors"></a>Erros de pacote de endereço

Se você adicionar mais de um projeto UWP à sua solução e, em seguida, tente criar um pacote, você poderá receber um erro como este.

  `MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle`

Esse erro é exibido porque, no nível da solução, não está claro qual aplicativo deve aparecer no pacote. Para resolver esse problema, abra cada arquivo de projeto e adicione as seguintes propriedades no final da primeira `<PropertyGroup>` elemento.

|**Project**|**Propriedades**|
|-------|----------|
|Aplicativo|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

Em seguida, remova o `AppxBundle` argumento de MSBuild da etapa de compilação.

## <a name="related-topics"></a>Tópicos relacionados

- [Compilar seu aplicativo .NET para Windows](https://docs.microsoft.com/vsts/build-release/get-started/dot-net)
- [Empacotando aplicativos da UWP](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)
- [Aplicativos LOB de sideload no Windows 10](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10)
- [Criar um certificado de assinatura do pacote](https://docs.microsoft.com/windows/uwp/packaging/create-certificate-package-signing)
