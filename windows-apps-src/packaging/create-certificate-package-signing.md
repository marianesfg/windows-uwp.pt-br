---
author: laurenhughes
title: Criar um certificado para a assinatura de pacote
description: Criar e exportar um certificado para a assinatura de pacote de apps com as ferramentas do PowerShell.
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 7bc2006f-fc5a-4ff6-b573-60933882caf8
ms.localizationpriority: medium
ms.openlocfilehash: 419fa90dfd21c42d256aeea8848862b8c5eb3628
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6979488"
---
# <a name="create-a-certificate-for-package-signing"></a>Criar um certificado para a assinatura de pacote


Este artigo explica como criar e exportar um certificado para a assinatura de pacote de apps usando as ferramentas do PowerShell. É recomendável que você use o Visual Studio para [Empacotar apps UWP](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps), mas você ainda pode empacotar um app da Loja pronto manualmente se você não usou o Visual Studio para desenvolver seu app.

> [!IMPORTANT] 
> Se você usou o Visual Studio para desenvolver seu app, é recomendável que você use o Assistente do Visual Studio para importar um certificado e assinar seu pacote de app. Para obter mais informações, consulte [Empacotar um aplicativo UWP com Visual Studio](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps).

## <a name="prerequisites"></a>Pré-requisitos

- **Um app empacotado ou não empacotado**  
Um app que contém um arquivo AppxManifest.xml. Você precisará fazer referência ao arquivo de manifesto ao criar o certificado que será usado para assinar o pacote do app final. Para obter detalhes sobre como empacotar manualmente um app, consulte [Criar um pacote de apps com a ferramenta MakeAppx.exe](https://msdn.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool).

- **Cmdlets da Infraestrutura de Chave Pública (PKI)**  
Você precisa de cmdlets de PKI para criar e exportar o certificado de assinatura. Para obter mais informações, consulte [Cmdlets da Infraestrutura de Chave Pública](https://docs.microsoft.com/powershell/module/pkiclient/).

## <a name="create-a-self-signed-certificate"></a>Criar um certificado autoassinado

Um certificado autoassinado é útil para testar seu app antes de você esteja pronto para publicá-lo na loja. Siga as etapas descritas nesta seção para criar um certificado autoassinado.

### <a name="determine-the-subject-of-your-packaged-app"></a>Determine o assunto do seu app empacotado  

Usar um certificado para assinar o pacote de apps, o "Assunto" no certificado **deve** corresponder à seção "Fornecedor" no manifesto do app.

Por exemplo, a seção "Identidade" no arquivo de AppxManifest.xml do seu app deve ser algo parecido com isso:
```
  <Identity Name="Contoso.AssetTracker" 
    Version="1.0.0.0" 
    Publisher="CN=Contoso Software, O=Contoso Corporation, C=US"/>
```

O "Fornecedor" nesse caso é "CN=Contoso Software, O=Contoso Corporation, C=US" que precisa ser usado para criar o certificado. 

### <a name="use-new-selfsignedcertificate-to-create-a-certificate"></a>Use **New-SelfSignedCertificate** para criar um certificado
Use o cmdlet **New-SelfSignedCertificate** do PowerShell para criar um certificado autoassinado. **New-SelfSignedCertificate** tem vários parâmetros para personalização, mas para fins deste artigo, vamos nos concentrar sobre como criar um certificado simples que funcionará com **SignTool**. Para obter mais exemplos e usos deste cmdlet, consulte [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/New-SelfSignedCertificate).

Com base no arquivo AppxManifest.xml do exemplo anterior, você deve usar a sintaxe a seguir para criar um certificado. Em um prompt do PowerShell elevado:
```
New-SelfSignedCertificate -Type Custom -Subject "CN=Contoso Software, O=Contoso Corporation, C=US" -KeyUsage DigitalSignature -FriendlyName <Your Friendly Name> -CertStoreLocation "Cert:\LocalMachine\My"
```

Depois de executar esse comando, o certificado será adicionado ao repositório de certificados local, conforme especificado no parâmetro "-CertStoreLocation". O resultado do comando também produzirá a impressão digital do certificado.  

**Observação**  
Você pode exibir o certificado em uma janela do PowerShell usando os seguintes comandos:
```
Set-Location Cert:\LocalMachine\My
Get-ChildItem | Format-Table Subject, FriendlyName, Thumbprint
```
Isso exibirá todos os certificados no repositório local.

## <a name="export-a-certificate"></a>Exportar um certificado 

Para exportar o certificado no repositório local para um arquivo de troca de informações pessoais (PFX), use o cmdlet **Export-PfxCertificate**.

Ao usar **Export-PfxCertificate**, você deve criar e usar uma senha ou usar o parâmetro "-ProtectTo" para especificar quais usuários ou grupos podem acessar o arquivo sem uma senha. Observe que um erro será exibido se você não usar o parâmetro "-Password" ou "-ProtectTo".

- **Uso de senhas**
```
$pwd = ConvertTo-SecureString -String <Your Password> -Force -AsPlainText 
Export-PfxCertificate -cert "Cert:\LocalMachine\My\<Certificate Thumbprint>" -FilePath <FilePath>.pfx -Password $pwd
```

- **Uso de ProtectTo**
```
Export-PfxCertificate -cert Cert:\LocalMachine\My\<Certificate Thumbprint> -FilePath <FilePath>.pfx -ProtectTo <Username or group name>
```

Depois de criar e exportar o certificado, você está pronto para assinar seu pacote de apps com **SignTool**. Para a próxima etapa no processo de empacotamento manual, consulte [Assinar um pacote de apps usando a SignTool](https://msdn.microsoft.com/windows/uwp/packaging/sign-app-package-using-signtool).

## <a name="security-considerations"></a>Considerações sobre segurança 
Ao adicionar um certificado em [repositórios de certificados do computador local](https://msdn.microsoft.com/windows/hardware/drivers/install/local-machine-and-current-user-certificate-stores), você afetará a confiança do certificado de todos os usuários no computador. É recomendável que você remova esses certificados quando eles não são mais necessários para evitar que eles sejam usados para comprometer a confiança do sistema.
