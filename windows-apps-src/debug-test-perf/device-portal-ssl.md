---
ms.assetid: e04ebe3f-479c-4b48-99d8-3dd4bb9bfaf4
title: Provisionar o Portal de Dispositivos com um certificado SSL personalizado
description: A ser definido
ms.date: 4/8/2019
ms.topic: article
keywords: Windows 10, uwp, o portal do dispositivo
ms.localizationpriority: medium
ms.openlocfilehash: cbe813a58124b1cd80f352ae11e9dcff59b21da4
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244332"
---
# <a name="provision-device-portal-with-a-custom-ssl-certificate"></a>Provisionar o Portal de Dispositivos com um certificado SSL personalizado

Na Atualização do Windows 10 para Criadores, o Windows Device Portal adicionou uma maneira de os administradores de dispositivo para instalar um certificado personalizado para uso na comunicação HTTPS.

Embora seja possível fazer isso em seu próprio computador, esse recurso destina-se principalmente a empresas que têm uma infraestrutura de certificado em vigor.  

Por exemplo, uma empresa pode ter uma autoridade de certificação (CA) para assinar certificados de sites de intranet por HTTPS. Além disso, esse recurso funciona sobre essa infraestrutura.

## <a name="overview"></a>Visão geral

Por padrão, o Device Portal gera uma autoridade de certificação raiz autoassinada e, em seguida, a utiliza para assinar certificados SSL para cada ponto de extremidade no qual esteja realizando a escuta. Isso inclui `localhost`, `127.0.0.1` e `::1` (IPv6 localhost).

Também estão incluídos o nome de host do dispositivo (por exemplo, `https://LivingRoomPC`) e o endereço IP de link local atribuído ao dispositivo (até dois [IPv4, IPv6] por adaptador de rede).
Você pode ver os endereços IP de link local de um dispositivo analisando a ferramenta de rede no Device Portal. Eles começarão com `10.` ou `192.` para IPv4 ou `fe80:` para IPv6.

Na configuração padrão, um aviso de certificado pode aparecer no navegador devido à autoridade de certificação raiz não confiável. Especificamente, o certificado SSL fornecido pelo Device Portal é assinado por uma autoridade de certificação raiz na qual o navegador ou o computador não confie. Isso pode ser corrigido por meio da criação de uma nova autoridade de certificação raiz confiável.

## <a name="create-a-root-ca"></a>Criar uma autoridade de certificação raiz

Isso só deverá ser feito uma vez e se sua empresa (ou casa) não tiver uma configuração de infraestrutura de certificado. O script de PowerShell a seguir cria uma autoridade de certificação raiz chamada _WdpTestCA.cer_. A instalação desse arquivo nas autoridades de certificação raiz confiáveis do computador local fará com que o dispositivo confie nos certificados SSL assinados por essa autoridade de certificação raiz. Você pode (e deve) instalar esse arquivo. cer em cada computador que você deseja conectar ao Windows Device Portal.  

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

## <a name="create-an-ssl-certificate-with-the-root-ca"></a>Criar um certificado SSL com a autoridade de certificação raiz

Os certificados SSL têm duas funções essenciais: proteger sua conexão por meio de criptografia e verificar se é você que está se comunicando com o endereço exibido na barra do navegador (Bing.com, 192.168.1.37 etc.), e não um terceiro mal-intencionado.

O script de PowerShell a seguir cria um certificado SSL para o ponto de extremidade `localhost`. Cada ponto de extremidade no qual o Device Portal realiza a escuta precisa ter seu próprio certificado; você pode substituir o argumento `$IssuedTo` no script por cada um dos pontos de extremidade do seu dispositivo: nome do host, localhost e os endereços IP.

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

Quando o pacote de arquivos .pfx for gerado, você precisará carregá-los no Windows Device Portal.

## <a name="provision-device-portal-with-the-certifications"></a>Provisionar Device Portal com as certificações

Para cada arquivo .pfx que você criou para um dispositivo, será necessário executar o seguinte comando em um prompt de comandos com privilégios elevados.

```cmd
WebManagement.exe -SetCert <Path to .pfx file> <password for pfx>
```

Veja a seguir o uso do exemplo:

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
Muitas redes usam DHCP para atribuir endereços IP; portanto, nem sempre os dispositivos terão o mesmo endereço IP que tinham antes. Se você tiver criado um certificado para um endereço IP em um dispositivo e se o endereço desse dispositivo tiver sido alterado, o Windows Device Portal gerará um novo certificado usando o certificado autoassinado existente e deixará de usar o que você criou. Isso fará com que a página de aviso de certificado apareça no navegador novamente. Por esse motivo, recomendamos que você se conecte aos seus dispositivos por meio dos nomes de host, que você pode definir no Device Portal. Eles permanecerão inalterados, independentemente dos endereços IP.
