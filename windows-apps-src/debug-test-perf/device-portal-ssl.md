---
author: PatrickFarley
ms.assetid: e04ebe3f-479c-4b48-99d8-3dd4bb9bfaf4
title: Provisionar o Portal de Dispositivos com um certificado SSL personalizado
description: A ser definido
ms.author: pafarley
ms.date: 07/11/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, portal de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: 1192c200cd42ab28cc7e763c06fd8a5638aa3400
ms.sourcegitcommit: 310a4555fedd4246188a98b31f6c094abb33ec60
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/19/2018
ms.locfileid: "5128514"
---
# <a name="provision-device-portal-with-a-custom-ssl-certificate"></a>Provisionar o Portal de Dispositivos com um certificado SSL personalizado
Na atualização de criadores do Windows 10, o Windows Device Portal adicionada uma maneira para os administradores do dispositivo instalar um certificado personalizado para uso na comunicação HTTPS. 

Enquanto você pode fazer isso em seu próprio computador, esse recurso se destina principalmente para as empresas que têm uma infraestrutura existente do certificado no lugar.  

Por exemplo, uma empresa pode ter uma autoridade de certificação (CA) que ele usa para assinar certificados para sites da intranet servido por HTTPS. Esse recurso na parte superior desse significa infraestrutura. 

## <a name="overview"></a>Visão geral
Por padrão, Device Portal gera uma autoridade de certificação raiz autoassinada e, em seguida, usa isso para assinar certificados SSL para cada ponto de extremidade que esteja escutando. Isso inclui `localhost`, `127.0.0.1`, e `::1` (IPv6 localhost).

Há também nome de host do dispositivo (por exemplo, `https://LivingRoomPC`) e o endereço IP cada conexão local atribuída ao dispositivo (até duas [IPv4, IPv6] por adaptador de rede). Você pode ver os endereços IP de conexão local para um dispositivo, observando a ferramenta de rede no Portal de dispositivos. Eles começar com `10.` ou `192.` para IPv4, ou `fe80:` para IPv6. 

Na configuração padrão, um aviso de certificado pode aparecer em seu navegador devido a autoridade de certificação raiz confiável. Especificamente, o certificado SSL fornecido pelo Portal de dispositivos é assinado por uma autoridade de certificação que o navegador ou o computador não confiável de raiz. Isso pode ser corrigido criando uma nova autoridade de certificação de raiz confiável.

## <a name="create-a-root-ca"></a>Criar uma CA raiz

Isso deve ser feito apenas se a sua empresa (ou home) não tiver uma infraestrutura de certificado configurar e deve ser feito apenas uma vez. O script do PowerShell a seguir cria uma CA chamada _WdpTestCA.cer_raiz. Instalando esse arquivo para autoridades de certificação de raiz confiáveis do computador local fará com que seu dispositivo para confiar em certificados SSL assinados por essa autoridade de certificação raiz. Você pode (e deve) instalar esse arquivo. cer em cada computador que você deseja se conectar ao Windows Device Portal.  

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

Certificados SSL tem duas funções essenciais: proteger sua conexão por meio de criptografia e verificar que você está, na verdade, se comunicando com o endereço exibido na barra de navegador (Bing.com, 192.168.1.37, etc.) e não um terceiro mal-intencionado.

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

Se você tiver vários dispositivos, você pode reutilizar os localhost. pfx arquivos, mas você ainda precisará criar certificados de nome de host e endereço IP para cada dispositivo separadamente.

Quando o lote de arquivos. pfx é gerado, você precisará carregá-los no Windows Device Portal. 

## <a name="provision-device-portal-with-the-certifications"></a>Provisionar o Portal de dispositivos com o certification(s)

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
Várias redes de usam o DHCP para distribuir endereços IP, portanto, dispositivos não receber sempre o mesmo endereço IP que tinham anteriormente. Se você tiver criado um certificado para um endereço IP em um dispositivo e que o endereço do dispositivo foi alterado, Windows Device Portal irá gerar um novo certificado usando o certificado autoassinado existente, e ele irá parar de usar aquele que você criou. Isso fará com que a página de aviso de certificado apareça no navegador novamente. Por esse motivo, é recomendável se conectando seus dispositivos por meio de seus nomes de host, que pode ser definida no Portal de dispositivos. Esses permanecerá o mesmo independentemente de endereços IP.
