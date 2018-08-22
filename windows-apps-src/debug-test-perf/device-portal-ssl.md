---
author: PatrickFarley
ms.assetid: e04ebe3f-479c-4b48-99d8-3dd4bb9bfaf4
title: Portal de dispositivo de provisão com um certificado SSL personalizado
description: A ser definido
ms.author: pafarley
ms.date: 07/11/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, portal de dispositivo
ms.localizationpriority: medium
ms.openlocfilehash: 1192c200cd42ab28cc7e763c06fd8a5638aa3400
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2018
ms.locfileid: "2786694"
---
# <a name="provision-device-portal-with-a-custom-ssl-certificate"></a>Portal de dispositivo de provisão com um certificado SSL personalizado
No Windows 10 criadores Update, Portal de dispositivo do Windows adicionado uma maneira dos administradores do dispositivo instalar um certificado personalizado para uso na comunicação HTTPS. 

Enquanto você pode fazer isso em seu próprio PC, esse recurso se destina principalmente para as empresas que adotar uma infra-estrutura de certificado existente.  

Por exemplo, uma empresa pode ter uma autoridade de certificação (CA) que ele usa para assinar certificados para sites de intranet servidas via HTTPS. Esse recurso na parte superior desse significa infraestrutura. 

## <a name="overview"></a>Visão geral
Por padrão, Portal de dispositivo gera uma autoridade de certificação de raiz auto-assinado e, em seguida, usa que para assinar certificados SSL para cada ponto de extremidade que ele está ouvindo. Isso inclui `localhost`, `127.0.0.1`, e `::1` (localhost IPv6).

Também são incluídos hostname do dispositivo (por exemplo, `https://LivingRoomPC`) e o endereço IP cada link local atribuído ao dispositivo (até dois [IPv4, IPv6] por adaptador de rede). Você pode ver os endereços IP de link local para um dispositivo examinando a ferramenta de rede no Portal do dispositivo. Eles começará com `10.` ou `192.` para IPv4, ou `fe80:` para IPv6. 

Na configuração padrão, pode aparecer um aviso de certificado no seu navegador, devido à autoridade de certificação raiz confiável. Especificamente, o cert SSL fornecido pelo Portal de dispositivo é assinado por uma autoridade de certificação que o navegador ou o PC não confia raiz. Isso pode ser resolvido criando uma nova autoridade de certificação de raiz confiável.

## <a name="create-a-root-ca"></a>Criar uma autoridade de certificação raiz

Isso deve ser feito apenas se sua empresa (ou residência) não tiver uma infra-estrutura de certificado configurada e deve ser feito apenas uma vez. O seguinte script do PowerShell cria uma chamada _WdpTestCA.cer_de CA raiz. Instalar este arquivo para autoridades de certificação de raiz confiáveis da máquina local fará com que seu dispositivo confiar em certificados SSL que sejam assinados por essa autoridade de certificação raiz. Você pode (e deve) instalar esse arquivo. cer em cada PC que você deseja se conectar ao Portal de dispositivo do Windows.  

```PowerShell
$CN = "PickAName"
$OutputPath = "c:\temp\"

# Create root certificate authority
$FilePath = $OutputPath + "WdpTestCA.cer"
$Subject =  "CN="+$CN
$rootCA = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -Subject $Subject -HashAlgorithm "SHA512" -KeyUsage CertSign,CRLSign
$rootCAFile = Export-Certificate -Cert $rootCA -FilePath $FilePath
```

Depois que isso é criado, você pode usar o arquivo _WdpTestCA.cer_ para assinar os certificados SSL. 

## <a name="create-an-ssl-certificate-with-the-root-ca"></a>Criar um certificado SSL com a CA raiz

Certificados SSL tenham duas funções críticas: Protegendo sua conexão por meio de criptografia e verificando se você estiver realmente se comunicando com o endereço exibido na barra de navegador (Bing.com, 192.168.1.37, etc.) e não um terceiro mal-intencionado.

O seguinte script do PowerShell cria um certificado SSL para o `localhost` ponto de extremidade. Cada ponto de extremidade que escuta o Portal do dispositivo precisa seu próprio certificado; Você pode substituir a `$IssuedTo` argumento no script com cada um dos pontos de extremidade diferentes para seu dispositivo: o nome do host, localhost e o endereços IP.

```PowerShell
$IssuedTo = "localhost"
$Password = "PickAPassword"
$OutputPath = "c:\temp\"
$rootCA = Import-Certificate -FilePath C:\temp\WdpTestCA.cer -CertStoreLocation Cert:\CurrentUser\My\

# Create SSL cert signed by certificate authority
$IssuedToClean = $IssuedTo.Replace(":", "-").Replace(" ", "_")
$FilePath = $OutputPath + $IssuedToClean + ".pfx"
$Subject = "CN="+$IssuedTo
$cert = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -Subject $Subject -DnsName $IssuedTo -Signer $rootCA -HashAlgorithm "SHA512"
$certFile = Export-PfxCertificate -cert $cert -FilePath $FilePath -Password (ConvertTo-SecureString -String $Password -Force -AsPlainText)
```

Se você tiver vários dispositivos, você pode reutilizar os arquivos do localhost. pfx, mas você ainda precisará criar certificados de endereço e o nome de host IP para cada dispositivo separadamente.

Quando o conjunto de arquivos. pfx é gerado, você precisará carregá-los no Portal de dispositivo do Windows. 

## <a name="provision-device-portal-with-the-certifications"></a>Portal de dispositivo de provisão com o certification(s)

Para cada arquivo. pfx que você criou para um dispositivo, você precisará executar o seguinte comando em um prompt de comando elevado.

```
WebManagement.exe -SetCert <Path to .pfx file> <password for pfx> 
```

Veja abaixo uso por exemplo:
```
WebManagement.exe -SetCert localhost.pfx PickAPassword
WebManagement.exe -SetCert --1.pfx PickAPassword
WebManagement.exe -SetCert MyLivingRoomPC.pfx PickAPassword
```

Depois de instalar os certificados, simplesmente reinicie o serviço de forma que as alterações entrem em vigor:

```
sc stop webmanagement
sc start webmanagement
```

> [!TIP]
> Os endereços IP podem mudar ao longo do tempo.
Muitas redes utilizar DHCP para distribuir endereços IP, para que dispositivos não sempre obtém o mesmo endereço IP que tinham anteriormente. Se você criou um certificado para um endereço IP em um dispositivo e que o endereço do dispositivo foi alterada, Portal de dispositivo do Windows irá gerar um novo certificado usando o cert autoassinado existente, e ele irá parar de usar um que é criado. Isso fará com que a página de aviso cert apareça no seu navegador novamente. Por esse motivo, é recomendável conectar-se a seus dispositivos por meio de seus nomes de host, que pode ser definida no Portal do dispositivo. Estes serão permanecem os mesmos independentemente de endereços IP.
