---
title: Criar um certificado para a assinatura de pacote
description: Criar e exportar um certificado para a assinatura de pacote de apps com as ferramentas do PowerShell.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 7bc2006f-fc5a-4ff6-b573-60933882caf8
ms.localizationpriority: medium
ms.openlocfilehash: 1476410c96900eff7ba4b8d0ad34c9d7b5599434
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372730"
---
# <a name="create-a-certificate-for-package-signing"></a>Criar um certificado para a assinatura de pacote


Este artigo explica como criar e exportar um certificado para a assinatura de pacote de apps usando as ferramentas do PowerShell. É recomendável que você use o Visual Studio para [Empacotar apps UWP](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps), mas você ainda pode empacotar um app da Loja pronto manualmente se você não usou o Visual Studio para desenvolver seu app.

> [!IMPORTANT] 
> Se você usou o Visual Studio para desenvolver seu app, é recomendável que você use o Assistente do Visual Studio para importar um certificado e assinar seu pacote de app. Para obter mais informações, consulte [Empacotar um aplicativo UWP com Visual Studio](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps).

## <a name="prerequisites"></a>Pré-requisitos

- **Um aplicativo compactado ou descompactado**  
Um app que contém um arquivo AppxManifest.xml. Você precisará fazer referência ao arquivo de manifesto ao criar o certificado que será usado para assinar o pacote do app final. Para obter detalhes sobre como empacotar manualmente um app, consulte [Criar um pacote de apps com a ferramenta MakeAppx.exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool).

- **Cmdlets de infraestrutura de chave pública (PKI)**  
Você precisa de cmdlets de PKI para criar e exportar o certificado de assinatura. Para obter mais informações, consulte [Cmdlets da Infraestrutura de Chave Pública](https://docs.microsoft.com/powershell/module/pkiclient/).

## <a name="create-a-self-signed-certificate"></a>Criar um certificado autoassinado

Um certificado autoassinado é útil para testar seu aplicativo antes que você está pronto para publicá-lo para a Store. Siga as etapas descritas nesta seção para criar um certificado autoassinado.

> [!NOTE]
> Os certificados autoassinados são estritamente para teste. Quando estiver pronto para publicar seu aplicativo para o armazenamento ou de outros locais, alterne o certificado para a fonte com boa reputação. Falha ao fazer isso pode resultar na incapacidade do aplicativo instalado por seus clientes.

### <a name="determine-the-subject-of-your-packaged-app"></a>Determine o assunto do seu app empacotado  

Usar um certificado para assinar o pacote de apps, o "Assunto" no certificado **deve** corresponder à seção "Fornecedor" no manifesto do app.

Por exemplo, a seção "Identidade" no arquivo de AppxManifest.xml do seu app deve ser algo parecido com isso:

```xml
  <Identity Name="Contoso.AssetTracker" 
    Version="1.0.0.0" 
    Publisher="CN=Contoso Software, O=Contoso Corporation, C=US"/>
```

O "Fornecedor" nesse caso é "CN=Contoso Software, O=Contoso Corporation, C=US" que precisa ser usado para criar o certificado.

### <a name="use-new-selfsignedcertificate-to-create-a-certificate"></a>Use **New-SelfSignedCertificate** para criar um certificado

Use o cmdlet **New-SelfSignedCertificate** do PowerShell para criar um certificado autoassinado. **New-SelfSignedCertificate** tem vários parâmetros para personalização, mas para fins deste artigo, vamos nos concentrar sobre como criar um certificado simples que funcionará com **SignTool**. Para obter mais exemplos e usos deste cmdlet, consulte [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/New-SelfSignedCertificate).

Com base no arquivo AppxManifest.xml do exemplo anterior, você deve usar a sintaxe a seguir para criar um certificado. Em um prompt do PowerShell elevado:

```powershell
New-SelfSignedCertificate -Type Custom -Subject "CN=Contoso Software, O=Contoso Corporation, C=US" -KeyUsage DigitalSignature -FriendlyName "Your friendly name goes here" -CertStoreLocation "Cert:\LocalMachine\My" -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.3", "2.5.29.19={text}")
```

Observe os seguintes detalhes sobre alguns dos parâmetros:

- **KeyUsage**: Esse parâmetro define que o certificado pode ser usado. Para um certificado autoassinado, esse parâmetro deve ser definido como **DigitalSignature**.

- **TextExtension**: Esse parâmetro inclui configurações para as seguintes extensões:

  - Uso estendido de chave (EKU): Essa extensão indica fins adicionais para o qual a chave pública certificada pode ser usada. Para obter um certificado autoassinado, esse parâmetro deve incluir a cadeia de caracteres de extensão **"2.5.29.37={text}1.3.6.1.5.5.7.3.3"** , que indica que o certificado deve ser usado para assinatura de código.

  - Restrições básicas: Essa extensão indica se é ou não o certificado de uma autoridade de certificação (CA). Para obter um certificado autoassinado, esse parâmetro deve incluir a cadeia de caracteres de extensão **"2.5.29.19={text}"** , que indica que o certificado é uma entidade final (não uma autoridade de certificação).

Depois de executar esse comando, o certificado será adicionado ao repositório de certificados local, conforme especificado no parâmetro "-CertStoreLocation". O resultado do comando também produzirá a impressão digital do certificado.  

Você pode exibir o certificado em uma janela do PowerShell usando os seguintes comandos:

```powershell
Set-Location Cert:\LocalMachine\My
Get-ChildItem | Format-Table Subject, FriendlyName, Thumbprint
```

Isso exibirá todos os certificados no repositório local.

## <a name="export-a-certificate"></a>Exportar um certificado 

Para exportar o certificado no repositório local para um arquivo de troca de informações pessoais (PFX), use o cmdlet **Export-PfxCertificate**.

Ao usar **Export-PfxCertificate**, você deve criar e usar uma senha ou usar o parâmetro "-ProtectTo" para especificar quais usuários ou grupos podem acessar o arquivo sem uma senha. Observe que um erro será exibido se você não usar o parâmetro "-Password" ou "-ProtectTo".

### <a name="password-usage"></a>Uso de senhas

```powershell
$pwd = ConvertTo-SecureString -String <Your Password> -Force -AsPlainText 
Export-PfxCertificate -cert "Cert:\LocalMachine\My\<Certificate Thumbprint>" -FilePath <FilePath>.pfx -Password $pwd
```

### <a name="protectto-usage"></a>Uso de ProtectTo

```powershell
Export-PfxCertificate -cert Cert:\LocalMachine\My\<Certificate Thumbprint> -FilePath <FilePath>.pfx -ProtectTo <Username or group name>
```

Depois de criar e exportar o certificado, você está pronto para assinar seu pacote de apps com **SignTool**. Para a próxima etapa no processo de empacotamento manual, consulte [Assinar um pacote de apps usando a SignTool](https://docs.microsoft.com/windows/uwp/packaging/sign-app-package-using-signtool).

## <a name="security-considerations"></a>Considerações sobre segurança

Ao adicionar um certificado em [repositórios de certificados do computador local](https://docs.microsoft.com/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores), você afetará a confiança do certificado de todos os usuários no computador. É recomendável que você remova esses certificados quando eles não são mais necessários para evitar que eles sejam usados para comprometer a confiança do sistema.
