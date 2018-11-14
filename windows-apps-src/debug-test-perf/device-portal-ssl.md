---
author: PatrickFarley
ms.assetid: e04ebe3f-479c-4b48-99d8-3dd4bb9bfaf4
title: Provisionar o Portal de Dispositivos com um certificado SSL personalizado
description: A ser definido
ms.author: pafarley
ms.date: 07/11/2017
ms.topic: article
keywords: Windows 10, uwp, portal de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: 525c64ab289d26a4835168f410ac4ba3fc14343a
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6653655"
---
# <a name="provision-device-portal-with-a-custom-ssl-certificate"></a>Provisionar o Portal de Dispositivos com um certificado SSL personalizado
No Windows 10 Creators Update, o Windows Device Portal adicionado uma maneira para os administradores do dispositivo instalar um certificado personalizado para uso na comunicação HTTPS. 

Enquanto você pode fazer isso em seu próprio computador, esse recurso se destina principalmente para as empresas que têm uma infraestrutura existente do certificado no lugar.  

Por exemplo, uma empresa pode ter uma autoridade de certificação (CA) que ele usa para assinar certificados para sites de intranet servido por HTTPS. Esse recurso na parte superior desse significa infraestrutura. 

## <a name="overview"></a>Visão geral
Por padrão, o Device Portal gera uma autoridade de certificação raiz autoassinada e, em seguida, usa isso para assinar certificados SSL para cada ponto de extremidade que esteja escutando. Isso inclui `localhost`, `127.0.0.1`, e `::1` (IPv6 localhost).

Também estão incluídos nome do host do dispositivo (por exemplo, `https://LivingRoomPC`) e cada endereço IP de conexão local atribuída ao dispositivo (até dois [IPv4, IPv6] por adaptador de rede). Você pode ver os endereços IP de conexão local para um dispositivo, observando a ferramenta de rede no Portal de dispositivos. Eles começar com `10.` ou `192.` para IPv4, ou `fe80:` para IPv6. 

Na configuração padrão, um aviso de certificado pode aparecer em seu navegador devido a autoridade de certificação raiz confiável. Especificamente, o certificado SSL fornecido pelo Portal de dispositivo é assinado por uma autoridade de certificação que o navegador ou o computador não confiável de raiz. Isso pode ser corrigido criando uma nova autoridade de certificação de raiz confiável.

## <a name="create-a-root-ca"></a>Criar uma CA raiz

Isso deve ser feito somente se sua empresa (ou home) não tem uma infraestrutura de certificado configurar e deve ser feito apenas uma vez. O script do PowerShell a seguir cria uma CA chamada _WdpTestCA.cer_raiz. Instalar esse arquivo para autoridades de certificação de raiz confiável da máquina local fará com que seu dispositivo para certificados SSL assinados por essa autoridade de certificação raiz de confiança. Você pode (e deve) instalar esse arquivo. cer em cada computador que você deseja se conectar ao Windows Device Portal.  

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

Certificados SSL tem duas funções importantes: proteger sua conexão por meio de criptografia e verificar que você está, na verdade, se comunicando com o endereço exibido na barra de navegador (Bing.com, 192.168.1.37, etc.) e não um terceiro mal-intencionado.

O script do PowerShell a seguir cria um certificado SSL para o `localhost` ponto de extremidade. Cada ponto de extremidade que escuta Device Portal precisa seu próprio certificado; Você pode substituir o `$IssuedTo` argumento no script com cada um dos pontos de extremidade diferentes para seu dispositivo: o nome do host, localhost e o endereços IP.

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

Se você tiver vários dispositivos, você pode reutilizar os localhost. pfx arquivos, mas você ainda precisará criar certificados IP endereço e nome do host para cada dispositivo separadamente.

Quando o lote de arquivos. pfx é gerado, você precisará carregá-los no Windows Device Portal. 

## <a name="provision-device-portal-with-the-certifications"></a>Provisionar o Portal de dispositivo com o certification(s)

Para cada arquivo. pfx que você criou para um dispositivo, você precisará executar o seguinte comando em um prompt de comando com privilégios elevados.

```
WebManagement.exe -SetCert <Path to .pfx file> <password for pfx> 
```

Veja a seguir uso por exemplo:
```
WebManagement.exe -SetCert localhost.pfx PickAPassword
WebManagement.exe -SetCert --1.pfx PickAPassword
WebManagement.exe -SetCert MyLivingRoomPC.pfx PickAPassword
```

Depois de instalar os certificados, basta reinicie o serviço para que as alterações entrem em vigor:

```
sc stop webmanagement
sc start webmanagement
```

> [!TIP]
> Os endereços IP podem mudar ao longo do tempo.
Várias redes de usam DHCP para fornecer endereços IP, para que dispositivos não sempre obtém o mesmo endereço IP que tinham anteriormente. Se você tiver criado um certificado para um endereço IP em um dispositivo e que o endereço do dispositivo foi alterado, Windows Device Portal irá gerar um novo certificado usando o certificado autoassinado existente, e ele irá parar de usar aquele que você criou. Isso fará com que a página de aviso de certificado apareça no navegador novamente. Por esse motivo, é recomendável se conectando seus dispositivos por meio de seus nomes de host, que pode ser definida no Portal de dispositivos. Esses permanecerá o mesmo independentemente de endereços IP.
