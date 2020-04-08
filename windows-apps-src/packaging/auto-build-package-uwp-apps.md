---
title: Configurar compilações automáticas para seu aplicativo UWP
description: Como configurar builds automáticos para produzir pacotes de sideload e/ou para a Store.
ms.date: 07/17/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: 70415c9f3d58625cfdc651ec67c8a9f37c23cffa
ms.sourcegitcommit: 3e7a4f7605dfb4e87bac2d10b6d64f8b35229546
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/08/2020
ms.locfileid: "77089492"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>Configurar compilações automáticas para seu aplicativo UWP

Você pode usar o Azure Pipelines para criar builds automatizados para seus projetos da UWP. Neste artigo, vamos analisar diferentes maneiras de fazer isso. Também mostraremos como realizar essas tarefas usando a linha de comando para que seja possível fazer a integração com qualquer sistema de build.

## <a name="create-a-new-azure-pipeline"></a>Crie um novo pipeline do Azure

Comece [inscrevendo-se no Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up) caso ainda não tenha feito isso.

Em seguida, crie um pipeline que possa ser usado para criar seu código-fonte. Para obter um tutorial sobre como criar um pipeline para criar um repositório GitHub, consulte [Crie seu primeiro pipeline](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml). O Azure Pipelines é compatível com os tipos de repositório listados [neste artigo](https://docs.microsoft.com/azure/devops/pipelines/repos).

## <a name="set-up-an-automated-build"></a>Configurar uma compilação automática

Vamos começar com a definição de build padrão da UWP disponível no Azure DevOps e, em seguida, vamos mostrar como configurar o pipeline.

Na lista de modelos de definição de compilação, escolha o modelo **Plataforma Universal do Windows**.

![Selecione o modelo da UWP](images/select-yaml-template.png)

Este modelo inclui a configuração básica para criar seu projeto da UWP:

```yml
trigger:
- master

pool:
  vmImage: 'windows-latest'

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

O modelo padrão tenta assinar o pacote com o certificado especificado no arquivo .csproj. Se quiser assinar o pacote durante o build, você precisará ter acesso à chave privada. Caso contrário, você pode desabilitar a assinatura adicionando o parâmetro `/p:AppxPackageSigningEnabled=false` à seção `msbuildArgs` no arquivo YAML.

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>Adicione o certificado do projeto à biblioteca Arquivos seguros

Se possível, você deve evitar enviar certificados para o seu repositório e o git os ignorará por padrão. Para gerenciar o tratamento seguro de arquivos confidenciais como certificados, o Azure DevOps oferece suporte ao recurso [arquivos seguros](https://docs.microsoft.com/azure/devops/pipelines/library/secure-files?view=azure-devops).

Para carregar um certificado em seu build automatizado:

1. No Azure Pipelines, expanda **Pipelines** no painel de navegação e clique em **Biblioteca**.
2. Clique na guia **Arquivos seguros** e clique em **+ Arquivo seguro**.

    ![como carregar um arquivo seguro](images/secure-file1.png)

3. Navegue até o arquivo de certificado e clique em **OK**.
4. Depois de carregar o certificado, selecione-o para visualizar as propriedades dele. Em **Permissões de pipeline**, habilite a opção **Autorizar para uso em todos os pipelines**.

    ![como carregar um arquivo seguro](images/secure-file2.png)

5. Se a chave privada no certificado tiver uma senha, recomendamos que você a armazene no [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) e a vincule a um [grupo de variáveis](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups). Você pode usar a variável para acessar a senha pelo pipeline. Observe que uma senha é compatível apenas com a chave privada. No momento, não é possível usar um arquivo de certificado que seja, em si, protegido por senha.

> [!NOTE]
> Começando no Visual Studio 2019, o certificado temporário deixou de ser gerado nos projetos da UWP. Para criar ou exportar certificados, use os cmdlets do PowerShell descritos [neste artigo](/windows/msix/package/create-certificate-package-signing).

## <a name="configure-the-build-solution-build-task"></a>Configurar a tarefa de compilação Compilar solução

Essa tarefa compila qualquer solução na pasta de trabalho para binários e produz o arquivo do pacote do aplicativo de saída. Essa tarefa usa argumentos MSbuild. Você precisará especificar o valor desses argumentos. Use a tabela a seguir como guia.

|**Argumento MSBuild**|**Valor**|**Descrição**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | Define a pasta para armazenar os artefatos gerados. |
| AppxBundlePlatforms | $(Build.BuildPlatform) | Permite definir quais plataformas incluir no pacote. |
| AppxBundle | Sempre | Cria um .msixbundle/.appxbundle com os arquivos .msix/.appx da plataforma especificada. |
| UapAppxPackageBuildMode | StoreUpload | Gera o arquivo .msixupload/.appxupload e a pasta **_Test** para sideload. |
| UapAppxPackageBuildMode | CI | Gera somente o arquivo .msixupload/.appxupload. |
| UapAppxPackageBuildMode | SideloadOnly | Gera a pasta **_Test** apenas para sideload. |
| AppxPackageSigningEnabled | verdadeiro | Habilita a assinatura do pacote. |
| PackageCertificateThumbprint | Impressão digital do certificado | Esse valor **deve** corresponder à impressão digital no certificado de autenticação ou ser uma cadeia de caracteres vazia. |
| PackageCertificateKeyFile | Caminho | O caminho para o certificado a ser usado. Isso é recuperado pelos metadados do arquivo seguro. |
| PackageCertificatePassword | Senha | A senha da chave privada no certificado. Recomendamos que você armazene a senha no [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) e vincule-a ao [grupo de variáveis](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups). Você pode passar a variável para este argumento. |

### <a name="configure-the-build"></a>Configurar o build

Se quiser desenvolver sua solução usando a linha de comando ou qualquer outro sistema de build, execute MSBuild com esses argumentos.

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

### <a name="configure-package-signing"></a>Configure a autenticação do pacote

Para autenticar o pacote MSIX (ou .appx), o pipeline precisa recuperar o certificado de autenticação. Para isso, adicione uma tarefa DownloadSecureFile antes da tarefa VSBuild.
Isso possibilitará o acesso ao certificado de autenticação por ```signingCert```.

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
> O argumento PackageCertificateThumbprint é definido intencionalmente como uma cadeia de caracteres vazia por precaução. Caso a impressão digital esteja definida no projeto, mas não corresponda ao certificado de autenticação, o build falhará com o erro: `Certificate does not match supplied signing thumbprint`.

### <a name="review-parameters"></a>Revise os parâmetros

Os parâmetros definidos com a sintaxe `$()` são variáveis ajustadas na definição do build e serão alterados em outros sistemas de build.

![variáveis padrão](images/building-screen5.png)

Para ver todas as variáveis predefinidas, consulte [Variáveis de build predefinidas](https://docs.microsoft.com/azure/devops/pipelines/build/variables).

## <a name="configure-the-publish-build-artifacts-task"></a>Configure a tarefa Publicar Artefatos de Build

O pipeline da UWP padrão não salva os artefatos gerados. Para adicionar recursos de publicação à sua definição YAML, adicione as tarefas a seguir.

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

Você pode ver os artefatos gerados na opção **Artefatos** da página de resultados do build.

![artefatos](images/building-screen6.png)

Como definimos o argumento `UapAppxPackageBuildMode` como `StoreUpload`, a pasta de artefatos inclui o pacote para envio à Store (.msixupload/.appxupload). Observe que você também pode enviar o pacote do aplicativo comum (.appx) ou um lote de aplicativo (.msixbundle/.appxbundle/) à Store. Para este artigo, usaremos o arquivo .appxupload.

## <a name="address-bundle-errors"></a>Tratamento de erros de pacote

Se você adicionar mais de um projeto da UWP à solução e, em seguida, tentar criar um pacote, poderá receber um erro como este.

  `MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle`

Esse erro é exibido porque, no nível da solução, não está claro qual aplicativo deve aparecer no pacote. Para resolver esse problema, abra cada arquivo de projeto e adicione as propriedades a seguir ao final do primeiro elemento `<PropertyGroup>`.

|**Projeto**|**Propriedades**|
|-------|----------|
|Aplicativo|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

Em seguida, remova o argumento de MSBuild `AppxBundle` da etapa de build.

## <a name="related-topics"></a>Tópicos relacionados

- [Criar seu aplicativo .NET para Windows](https://docs.microsoft.com/vsts/build-release/get-started/dot-net)
- [Empacotando aplicativos UWP](/windows/msix/package/packaging-uwp-apps)
- [Sideload de aplicativos LOB no Windows 10](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10)
- [Criar um certificado para a assinatura de pacote](/windows/msix/package/create-certificate-package-signing)
