---
title: Configurar compilações automáticas para seu aplicativo UWP
description: Como configurar compilações automáticas para produzir pacotes de sideload e/ou armazenamento da Loja.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: 5837674f2cb20710a59eeac0af59498bf28b197e
ms.sourcegitcommit: a86d0bd1c2f67e5986cac88a98ad4f9e667cfec5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68229393"
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

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>Adicionar seu certificado de projeto para a biblioteca de arquivos seguros

Você deve evitar o envio de certificados ao seu repositório, se possível, e ignora git-los por padrão. Para gerenciar o tratamento seguro de arquivos confidenciais, como certificados, DevOps do Azure dá suporte a [proteger arquivos](https://docs.microsoft.com/azure/devops/pipelines/library/secure-files?view=azure-devops).

Para carregar um certificado para a compilação automatizada:

1. Em Pipelines do Azure, expanda **Pipelines** no painel de navegação e clique **biblioteca**.
2. Clique o **proteger arquivos** guia e, em seguida, clique em **+ arquivo seguro**.

    ![como carregar um arquivo seguro](images/secure-file1.png)

3. Navegue até o arquivo de certificado e clique em **Okey**.
4. Depois de carregar o certificado, selecione-o para exibir suas propriedades. Sob **permissões de Pipeline**, habilite a **autorizar para uso em todos os pipelines** ativar/desativar.

    ![como carregar um arquivo seguro](images/secure-file2.png)

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
| UapAppxPackageBuildMode | SideloadOnly | Gera o **Test** pasta para o sideload apenas. |
| AppxPackageSigningEnabled | true | Habilita a assinatura do pacote. |
| PackageCertificateThumbprint | Impressão digital do certificado | Esse valor **deve** corresponda a impressão digital no certificado de assinatura, ou ser uma cadeia de caracteres vazia. |
| Valor PackageCertificateKeyFile | Path | O caminho para o certificado a ser usado. Isso é recuperado dos metadados de arquivo seguro. |

### <a name="configure-the-build"></a>Configure a compilação

Se você quiser criar uma solução usando a linha de comando ou por meio de qualquer outro sistema de compilação, execute o MSBuild com estes argumentos.

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

### <a name="configure-package-signing"></a>Configurar a assinatura de pacote

Para assinar o pacote MSIX (ou APPX) pipeline precisa recuperar o certificado de autenticação. Para fazer isso, adicione uma tarefa de DownloadSecureFile antes da tarefa VSBuild.
Isso lhe dará acesso para o certificado de autenticação por meio de ```signingCert```.

```yml
- task: DownloadSecureFile@1
  name: signingCert
  displayName: 'Download CA certificate'
  inputs:
    secureFile: '[Your_Pfx].pfx'
```

Em seguida, atualize a tarefa VSBuild para referenciar o certificado de autenticação:

```yml
- task: VSBuild@1
  inputs:
    platform: 'x86'
    solution: '$(solution)'
    configuration: '$(buildConfiguration)'
    msbuildArgs: '/p:AppxBundlePlatforms="$(buildPlatform)" 
                  /p:AppxPackageDir="$(appxPackageDir)" 
                  /p:AppxBundle=Always 
                  /p:UapAppxPackageBuildMode=StoreUpload 
                  /p:AppxPackageSigningEnabled=true
                  /p:PackageCertificateThumbprint="" 
                  /p:PackageCertificateKeyFile="$(signingCert.secureFilePath)"'
```

> [!NOTE]
> O argumento de PackageCertificateThumbprint intencionalmente é definido como uma cadeia de caracteres vazia como uma precaução. Se a impressão digital é definida no projeto, mas não coincide com o certificado de autenticação, a compilação falhará com o erro: `Certificate does not match supplied signing thumbprint`.

### <a name="review-parameters"></a>Parâmetros de revisão

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

|**Projeto**|**Propriedades**|
|-------|----------|
|Aplicativo|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

Em seguida, remova o `AppxBundle` argumento de MSBuild da etapa de compilação.

## <a name="related-topics"></a>Tópicos relacionados

- [Compilar seu aplicativo .NET para Windows](https://docs.microsoft.com/vsts/build-release/get-started/dot-net)
- [Empacotando aplicativos da UWP](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)
- [Aplicativos LOB de sideload no Windows 10](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10)
- [Criar um certificado de assinatura do pacote](https://docs.microsoft.com/windows/uwp/packaging/create-certificate-package-signing)
