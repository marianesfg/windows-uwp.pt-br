---
ms.assetid: e04ebe3f-479c-4b48-99d8-3dd4bb9bfaf4
title: Provisionar o Portal de Dispositivos com um certificado SSL personalizado
description: Saiba como provisionar o Portal de Dispositivos do Windows com um certificado personalizado para uso em comunicações HTTPS.
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp, device portal
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: ce4e45bc23f4efb636618bb4891b9d6e9d207490
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "63774146"
---
# <a name="provision-device-portal-with-a-custom-ssl-certificate"></a>Provisionar o Portal de Dispositivos com um certificado SSL personalizado

Na Atualização do Windows 10 para Criadores, o Portal de Dispositivos do Windows adicionou uma maneira para os administradores de dispositivo instalarem um certificado personalizado para uso na comunicação HTTPS.

Embora seja possível fazer isso em seu próprio computador, esse recurso destina-se principalmente a empresas que têm uma infraestrutura de certificado em vigor.  

Por exemplo, uma empresa pode ter uma AC (autoridade de certificação) para assinar certificados de sites de intranet por HTTPS. Além disso, esse recurso funciona sobre essa infraestrutura.

## <a name="overview"></a>Visão geral

Por padrão, o Portal de Dispositivos gera uma AC raiz autoassinada, depois a utiliza para assinar certificados SSL em cada ponto de extremidade que ele esteja escutando. Isso inclui `localhost`, `127.0.0.1` e `::1` (localhost IPv6).

Também estão incluídos o nome de host do dispositivo (por exemplo, `https://LivingRoomPC`) e o endereço IP de link local atribuído ao dispositivo (até dois [IPv4, IPv6] por adaptador de rede).
Você pode ver os endereços IP de link local de um dispositivo analisando a ferramenta de Rede no Portal de Dispositivos. Eles começarão com `10.` ou `192.` para IPv4 ou `fe80:` para IPv6.

Na configuração padrão, um aviso de certificado pode aparecer no navegador devido à AC raiz não confiável. Especificamente, o certificado SSL fornecido pelo Portal de Dispositivos é assinado por uma AC raiz na qual o navegador ou computador não confia. Isso pode ser corrigido por meio da criação de uma nova AC raiz confiável.

## <a name="create-a-root-ca"></a>Criar uma AC raiz

Isso só deverá ser feito uma vez e se sua empresa (ou casa) não tiver uma configuração de infraestrutura de certificado. O script do PowerShell a seguir cria uma AC raiz chamada _WdpTestCA.cer_. A instalação desse arquivo nas autoridades de certificação raiz confiáveis do computador local fará com que o dispositivo confie nos certificados SSL assinados por essa AC raiz. Você pode (e deve) instalar esse arquivo .cer em cada computador que você deseja conectar ao Portal de Dispositivos do Windows.  

```PowerShell
$CN = "PickAName"
$OutputPath = "c:\temp\"

# Create root certificate authority
$FilePath = $OutputPath + "WdpTestCA.cer"
$Subject =  "CN="+$CN
$rootCA = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -Subject $Subject -HashAlgorithm "SHA512" -KeyUsage CertSign,CRLSign
$rootCAFile = Export-Certificate -Cert $rootCA -FilePath $FilePath
```

Depois que isso for criado, use o arquivo _WdpTestCA.cer_ para assinar certificados SSL.

## <a name="create-an-ssl-certificate-with-the-root-ca"></a>Criar um certificado SSL com a AC raiz

Os certificados SSL têm duas funções essenciais: proteger sua conexão por meio de criptografia e verificar se é você que está se comunicando com o endereço exibido na barra do navegador (Bing.com, 192.168.1.37 etc.), e não um terceiro mal-intencionado.

O script de PowerShell a seguir cria um certificado SSL para o ponto de extremidade `localhost`. Cada ponto de extremidade que o Portal de Dispositivos escuta precisa ter seu próprio certificado; você pode substituir o argumento `$IssuedTo` no script por cada um dos pontos de extremidade de seu dispositivo: nome do host, localhost e os endereços IP.

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

Se você tiver vários dispositivos, poderá reutilizar os arquivos .pfx do localhost, mas ainda precisará criar certificados de endereço IP e nome de host para cada dispositivo separadamente.

Quando o pacote de arquivos .pfx for gerado, você precisará carregá-lo no Portal de Dispositivos do Windows.

## <a name="provision-device-portal-with-the-certifications"></a>Provisionar Portal de Dispositivos com as certificações

Para cada arquivo .pfx que você criou para um dispositivo, será necessário executar o seguinte comando em um prompt de comandos com privilégios elevados.

```cmd
WebManagement.exe -SetCert <Path to .pfx file> <password for pfx>
```

Confira o seguinte exemplo de uso:

```cmd
WebManagement.exe -SetCert localhost.pfx PickAPassword
WebManagement.exe -SetCert --1.pfx PickAPassword
WebManagement.exe -SetCert MyLivingRoomPC.pfx PickAPassword
```

Após instalar os certificados, basta reiniciar o serviço para que as alterações entrem em vigor:

```cmd
sc stop webmanagement
sc start webmanagement
```

> [!TIP]
> Os endereços IP podem mudar ao longo do tempo.
Muitas redes usam DHCP para atribuir endereços IP, portanto, nem sempre os dispositivos terão o mesmo endereço IP que tinham antes. Se você tiver criado um certificado para um endereço IP em um dispositivo, e o endereço desse dispositivo tiver sido alterado, o Portal de Dispositivos do Windows gerará um novo certificado usando o certificado autoassinado existente e deixará de usar o que você criou. Isso fará com que a página de aviso de certificado apareça no navegador novamente. Por esse motivo, recomendamos que você se conecte aos seus dispositivos por meio dos nomes de host, que você pode definir no Portal de Dispositivos. Eles permanecerão inalterados, independentemente dos endereços IP.
