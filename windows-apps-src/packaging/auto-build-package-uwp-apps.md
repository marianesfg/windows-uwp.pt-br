---
title: Configurar compilações automáticas para seu aplicativo UWP
description: Como configurar compilações automáticas para produzir pacotes de sideload e/ou armazenamento da Loja.
ms.date: 07/17/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: de623240e275dda5b6fc4df9afee31e1adf9fd4f
ms.sourcegitcommit: 04683376dbdbff987601f546f058748442170068
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68340854"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>Configurar compilações automáticas para seu aplicativo UWP

Você pode usar Azure Pipelines para criar compilações automatizadas para projetos UWP. Neste artigo, veremos diferentes maneiras de fazer isso. Também mostraremos como executar essas tarefas usando a linha de comando para que você possa se integrar a qualquer outro sistema de compilação.

## <a name="create-a-new-azure-pipeline"></a>Criar um novo pipeline do Azure

Comece inscrevendo-se [para Azure pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up) se ainda não tiver feito isso.

Em seguida, crie um pipeline que você pode usar para criar seu código-fonte. Para obter um tutorial sobre como criar um pipeline para criar um repositório GitHub, consulte [criar seu primeiro pipeline](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml). Azure Pipelines dá suporte aos tipos de repositório listados [neste artigo](https://docs.microsoft.com/azure/devops/pipelines/repos).

## <a name="set-up-an-automated-build"></a>Configurar uma compilação automática

Vamos começar com a definição de compilação padrão do UWP que está disponível no Azure dev Ops e, em seguida, mostrar como configurar o pipeline.

Na lista de modelos de definição de compilação, escolha o modelo **Plataforma Universal do Windows**.

![Selecionar o modelo UWP](images/select-yaml-template.png)

Este modelo inclui a configuração básica para criar seu projeto UWP:

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

O modelo padrão tenta assinar o pacote com o certificado especificado no arquivo. csproj. Se você quiser assinar o pacote durante a compilação, deverá ter acesso à chave privada. Caso contrário, você pode desabilitar a assinatura adicionando o `/p:AppxPackageSigningEnabled=false` parâmetro `msbuildArgs` à seção no arquivo YAML.

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>Adicionar o certificado do projeto à biblioteca de arquivos seguros

Você deve evitar o envio de certificados para seu repositório, se possível, e o Git os ignora por padrão. Para gerenciar o tratamento seguro de arquivos confidenciais, como certificados, o Azure DevOps dá suporte ao recurso [arquivos seguros](https://docs.microsoft.com/azure/devops/pipelines/library/secure-files?view=azure-devops) .

Para carregar um certificado para a compilação automatizada:

1. Em Azure Pipelines, expanda **pipelines** no painel de navegação e clique em **biblioteca**.
2. Clique na guia **arquivos seguros** e, em seguida, clique em **+ arquivo seguro**.

    ![como carregar um arquivo seguro](images/secure-file1.png)

3. Navegue até o arquivo de certificado e clique em **OK**.
4. Depois de carregar o certificado, selecione-o para exibir suas propriedades. Em **permissões de pipeline**, habilite a alternância **autorizar para uso em todos os pipelines** .

    ![como carregar um arquivo seguro](images/secure-file2.png)

5. Se o certificado tiver uma senha, recomendamos que você armazene sua senha no [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) e, em seguida, vincule a senha a um [grupo de variáveis](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups). Você pode usar a variável para acessar a senha do pipeline.

> [!NOTE]
> A partir do Visual Studio 2019, um certificado temporário não é mais gerado em projetos UWP. Para criar ou exportar certificados, use os cmdlets do PowerShell descritos neste [artigo](create-certificate-package-signing.md).

## <a name="configure-the-build-solution-build-task"></a>Configurar a tarefa de compilação Compilar solução

Esta tarefa compila qualquer solução que esteja na pasta de trabalho para binários e produz o arquivo de pacote do aplicativo de saída. Essa tarefa usa argumentos do MSBuild. Você precisará especificar o valor desses argumentos. Use a tabela a seguir como guia.

|**Argumento do MSBuild**|**Valor**|**Descrição**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | Define a pasta para armazenar os artefatos gerados. |
| AppxBundlePlatforms | $(Build.BuildPlatform) | Permite que você defina as plataformas a serem incluídas no pacote. |
| AppxBundle | Sempre | Cria um. msixbundle/. appxbundle com os arquivos. msix/. Appx para a plataforma especificada. |
| UapAppxPackageBuildMode | StoreUpload | Gera o arquivo. msixupload/. appxupload e a pasta **_Test** para Sideload. |
| UapAppxPackageBuildMode | CI | Gera somente o arquivo. msixupload/. appxupload. |
| UapAppxPackageBuildMode | SideloadOnly | Gera a pasta **_Test** somente para Sideload. |
| AppxPackageSigningEnabled | true | Habilita a assinatura de pacote. |
| PackageCertificateThumbprint | Impressão digital do certificado | Esse valor **deve** corresponder à impressão digital no certificado de autenticação ou ser uma cadeia de caracteres vazia. |
| PackageCertificateKeyFile | Path | O caminho para o certificado a ser usado. Isso é recuperado dos metadados de arquivo seguro. |
| PackageCertificatePassword | Senha | A senha do certificado. É recomendável que você armazene sua senha no [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) e vincule a senha ao [grupo de variáveis](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups). Você pode passar a variável para esse argumento. |

### <a name="configure-the-build"></a>Configurar a compilação

Se você quiser compilar sua solução usando a linha de comando ou usando qualquer outro sistema de compilação, execute o MSBuild com esses argumentos.

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

### <a name="configure-package-signing"></a>Configurar assinatura de pacote

Para assinar o pacote MSIX (ou APPX), o pipeline precisa recuperar o certificado de autenticação. Para fazer isso, adicione uma tarefa DownloadSecureFile antes da tarefa VSBuild.
Isso fornecerá acesso ao certificado de autenticação via ```signingCert```.

```yml
- task: DownloadSecureFile@1
  name: signingCert
  displayName: 'Download CA certificate'
  inputs:
    secureFile: '[Your_Pfx].pfx'
```

Em seguida, atualize a tarefa VSBuild para fazer referência ao certificado de autenticação:

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
> O argumento PackageCertificateThumbprint é intencionalmente definido como uma cadeia de caracteres vazia como precaução. Se a impressão digital estiver definida no projeto, mas não corresponder ao certificado de autenticação, a compilação falhará com o erro `Certificate does not match supplied signing thumbprint`:.

### <a name="review-parameters"></a>Examinar parâmetros

Os parâmetros definidos com a `$()` sintaxe são variáveis definidas na definição da compilação e serão alterados em outros sistemas de compilação.

![variáveis padrão](images/building-screen5.png)

Para exibir todas as variáveis predefinidas, consulte [variáveis de compilação](https://docs.microsoft.com/azure/devops/pipelines/build/variables)predefinidas.

## <a name="configure-the-publish-build-artifacts-task"></a>Configurar a tarefa publicar artefatos de compilação

O pipeline UWP padrão não salva os artefatos gerados. Para adicionar os recursos de publicação à definição do YAML, adicione as seguintes tarefas.

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

Você pode ver os artefatos gerados na  opção artefatos da página compilar resultados.

![artifacts](images/building-screen6.png)

Como definimos o `UapAppxPackageBuildMode` argumento como `StoreUpload`, a pasta artefatos inclui o pacote para envio para o repositório (. msixupload/. appxupload). Observe que você também pode enviar uma pacote de aplicativo regular (. msix/. AppX) ou um pacote de aplicativo (. msixbundle/. appxbundle/) para o repositório. Para os fins deste artigo, vamos usar o arquivo .appxupload.

## <a name="address-bundle-errors"></a>Erros de pacote de endereços

Se você adicionar mais de um projeto UWP à sua solução e, em seguida, tentar criar um pacote, você poderá receber um erro como este.

  `MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle`

Esse erro é exibido porque, no nível da solução, não está claro qual aplicativo deve aparecer no pacote. Para resolver esse problema, abra cada arquivo de projeto e adicione as propriedades a seguir ao final do primeiro `<PropertyGroup>` elemento.

|**Projeto**|**Propriedades**|
|-------|----------|
|Aplicativo|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

Em seguida, remova `AppxBundle` o argumento MSBuild da etapa de compilação.

## <a name="related-topics"></a>Tópicos relacionados

- [Crie seu aplicativo .NET para Windows](https://docs.microsoft.com/vsts/build-release/get-started/dot-net)
- [Empacotando aplicativos UWP](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)
- [Aplicativos LOB Sideload no Windows 10](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10)
- [Criar um certificado para assinatura de pacote](https://docs.microsoft.com/windows/uwp/packaging/create-certificate-package-signing)
