---
ms.assetid: bfabd3d5-dd56-4917-9572-f3ba0de4f8c0
title: Referência de API central do Device Portal
description: Saiba mais sobre as APIs REST centrais do Windows Device Portal que você pode usar para acessar os dados e controlar seu dispositivo de forma programática.
ms.custom: 19H1
ms.date: 04/19/2019
ms.topic: article
keywords: Windows 10, uwp, o portal do dispositivo
ms.localizationpriority: medium
ms.openlocfilehash: 910e3108009704d444fb81b195f9dd9eae3daa9d
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63798188"
---
# <a name="device-portal-core-api-reference"></a>Referência de API central do Device Portal

Toda a funcionalidade do Portal de Dispositivos é criada com base em APIs REST, que os desenvolvedores podem chamar diretamente para acessar os recursos e controlar seus dispositivos de forma programática.

## <a name="app-deployment"></a>Implantação de aplicativos

### <a name="install-an-app"></a>Instalar um aplicativo

**Solicitação**

Você pode instalar um aplicativo usando o formato de solicitação a seguir.

| Método      | URI da solicitação |
| :------     | :----- |
| POSTAR | /api/app/packagemanager/package |

**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI | Descrição |
| :------          | :------ |
| pacote   | (**necessário**) O nome do arquivo do pacote a ser instalado. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- O arquivo .appx ou. appxbundle, bem como quaisquer dependências que exigem o aplicativo. 
- O certificado usado para assinar o aplicativo, se o dispositivo for IoT ou área de trabalho do Windows. Outras plataformas não exigem o certificado. 

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

|  Código de status HTTP      | Descrição | 
| :------     | :----- |
| 200 | Implantar solicitação aceita e em processamento |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

### <a name="install-a-related-set"></a>Instalar um conjunto relacionado

**Solicitação**

Você pode instalar um [conjunto relacionado](https://blogs.msdn.microsoft.com/appinstaller/2017/05/12/tooling-to-create-a-related-set/) usando o formato de solicitação a seguir.

| Método      | URI da solicitação |
| :------     | :------ |
| POSTAR | /api/app/packagemanager/package |

**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI | Descrição |
| :------          | :------ |
| pacote   | (**necessário**) Os nomes do arquivo dos pacotes a serem instalados. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação** 
- Adicionar ".opt" aos nomes de arquivo de pacote opcional ao especificá-las como parâmetro, da seguinte forma: "foo.appx.opt" ou "bar.appxbundle.opt". 
- O arquivo .appx ou. appxbundle, bem como quaisquer dependências que exigem o aplicativo. 
- O certificado usado para assinar o aplicativo, se o dispositivo for IoT ou área de trabalho do Windows. Outras plataformas não exigem o certificado. 

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

|  Código de status HTTP      | Descrição | 
| :------     | :----- |
| 200 | Implantar solicitação aceita e em processamento |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

### <a name="register-an-app-in-a-loose-folder"></a>Registre um aplicativo em uma pasta flexível

**Solicitação**

Você pode registrar um aplicativo em uma pasta flexível usando o formato de solicitação a seguir.

| Método      | URI da solicitação |
| :------     | :----- |
| POSTAR | /api/app/packagemanager/networkapp |

**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

```json
{
    "mainpackage" :
    {
        "networkshare" : "\\some\share\path",
        "username" : "optional_username",
        "password" : "optional_password"
    }
}
```

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

|  Código de status HTTP      | Descrição | 
| :------     | :----- |
| 200 | Implantar solicitação aceita e em processamento |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

### <a name="register-a-related-set-in-loose-file-folders"></a>Registrar um conjunto relacionado em pastas de arquivo flexível

**Solicitação**

Você pode registrar um [conjunto relacionado](https://blogs.msdn.microsoft.com/appinstaller/2017/05/12/tooling-to-create-a-related-set/) em pastas flexíveis usando o formato de solicitação a seguir.

| Método      | URI da solicitação |
| :------     | :----- |
| POSTAR | /api/app/packagemanager/networkapp |

**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

```json
{
    "mainpackage" :
    {
        "networkshare" : "\\some\share\path",
        "username" : "optional_username",
        "password" : "optional_password"
    },
    "optionalpackages" :
    [
        {
            "networkshare" : "\\some\share\path2",
            "username" : "optional_username2",
            "password" : "optional_password2"
        },
        ...
    ]
}
```

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

|  Código de status HTTP      | Descrição | 
| :------     | :----- |
| 200 | Implantar solicitação aceita e em processamento |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

### <a name="get-app-installation-status"></a>Obter o status de instalação do aplicativo

**Solicitação**

Você pode obter o status de uma instalação de aplicativo que esteja em andamento usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/app/packagemanager/state |

**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

|  Código de status HTTP      | Descrição | 
| :------     | :----- |
| 200 | O resultado da última implantação |
| 204 | A instalação está em execução. |
| 404 | Nenhuma ação de instalação foi encontrada |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

### <a name="uninstall-an-app"></a>Desinstalar um aplicativo

**Solicitação**

Você pode desinstalar um aplicativo usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| DELETE | /api/app/packagemanager/package |

**Parâmetros de URI**

| Parâmetro do URI | Descrição |
| :------          | :------ |
| pacote   | (**necessário**) O PackageFullName (de GET/api/app/packagemanager/packages) do aplicativo de destino |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

|  Código de status HTTP      | Descrição | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

### <a name="get-installed-apps"></a>Obter aplicativos instalados

**Solicitação**

Você pode obter uma lista de aplicativos instalados no sistema usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/app/packagemanager/packages |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta inclui uma lista de pacotes instalados com os detalhes associados. O modelo dessa resposta é o seguinte.
```json
{"InstalledPackages": [
    {
        "Name": string,
        "PackageFamilyName": string,
        "PackageFullName": string,
        "PackageOrigin": int, (https://msdn.microsoft.com/en-us/library/windows/desktop/dn313167(v=vs.85).aspx)
        "PackageRelativeId": string,
        "Publisher": string,
        "Version": {
            "Build": int,
            "Major": int,
            "Minor": int,
            "Revision": int
     },
     "RegisteredUsers": [
     {
        "UserDisplayName": string,
        "UserSID": string
     },...
     ]
    },...
]}
```
**Código de status**

Esta API tem os códigos de status esperados a seguir.

|  Código de status HTTP      | Descrição | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

## <a name="bluetooth"></a>Bluetooth

<hr>

### <a name="get-the-bluetooth-radios-on-the-machine"></a>Ouça rádios Bluetooth no computador

**Solicitação**

Você pode obter uma lista rádios Bluetooth instaladas no computador usando o seguinte formato de solicitação: Isso pode ser atualizado para uma conexão de WebSocket, também com os mesmos dados JSON.
 
| Método        | URI da solicitação |
| :------          | :------ |
| OBTER           | /API/BT/getradios |
| GET/WebSocket | /API/BT/getradios |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta inclui uma matriz JSON das rádios Bluetooth conectadas ao dispositivo.
```json
{"BluetoothRadios" : [
    {
        "BluetoothAddress" : int64,
        "DisplayName" : string,
        "HasUnknownUsbDevice" : boolean,
        "HasProblem" : boolean,
        "ID" : string,
        "ProblemCode" : int,
        "State" : string
    },...
]}
```
**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP | Descrição |
| :------             | :------ |
| 200              | OK |
| 4XX              | Códigos de erro |
| 5XX              | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

### <a name="turn-the-bluetooth-radio-on-or-off"></a>Ativar ou desativar a rádio Bluetooth

**Solicitação**

Define uma rádio Bluetooth específica como ativada ou desativada.
 
| Método | URI da solicitação |
| :------   | :------ |
| POSTAR   | /api/bt/setradio |

**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI | Descrição |
| :------          | :------ |
| ID            | (**obrigatório**) A ID do dispositivo para rádio Bluetooth e deve ter codificação na base 64. |
| Estado         | (**necessária**) pode ser `"On"` ou `"Off"`. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP | Descrição |
| :------             | :------ |
| 200              | OK |
| 4XX              | Códigos de erro |
| 5XX              | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT

---
### <a name="get-a-list-of-paired-bluetooth-devices"></a>Obter uma lista de dispositivos Bluetooth emparelhados

**Solicitação**

Você pode obter uma lista de dispositivos Bluetooth emparelhados atualmente usando o seguinte formato de solicitação. Isso pode ser atualizado para uma conexão WebSocket com os mesmos dados JSON. Durante o tempo de vida da conexão de WebSocket, pode alterar a lista de dispositivos. Uma lista completa de dispositivos será enviada sobre a conexão de WebSocket sempre que há uma atualização.

| Método        | URI da solicitação       |
| :---          | :---              |
| OBTER           | /api/bt/getpaired |
| GET/WebSocket | /api/bt/getpaired |

**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta inclui uma matriz JSON dos dispositivos Bluetooth emparelhados no momento.
```json
{"PairedDevices": [
    {
        "Name" : string,
        "ID" : string,
        "AudioConnectionStatus" : string
    },...
]}
```
O *AudioConnectionStatus* campo estará presente se o dispositivo pode ser usado para áudio neste sistema. (As políticas e componentes opcionais podem afetar isso.) *AudioConnectionStatus* será "Conectado" ou "Desconectado".

---
### <a name="get-a-list-of-available-bluetooth-devices"></a>Obter uma lista de dispositivos Bluetooth disponíveis

**Solicitação**

Você pode obter uma lista dos dispositivos Bluetooth disponíveis para o emparelhamento usando o seguinte formato de solicitação. Isso pode ser atualizado para uma conexão WebSocket com os mesmos dados JSON. Durante o tempo de vida da conexão de WebSocket, pode alterar a lista de dispositivos. Uma lista completa de dispositivos será enviada sobre a conexão de WebSocket sempre que há uma atualização.

| Método        | URI da solicitação          |
| :---          | :---                 |
| OBTER           | /api/bt/getavailable |
| GET/WebSocket | /api/bt/getavailable |

**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta inclui uma matriz JSON dos dispositivos Bluetooth que estão atualmente disponíveis para o emparelhamento.
```json
{"AvailableDevices": [
    {
        "Name" : string,
        "ID" : string
    },...
]}
```

---
### <a name="connect-a-bluetooth-device"></a>Conectar um dispositivo Bluetooth

**Solicitação**

Se conectará ao dispositivo se o dispositivo pode ser usado para áudio neste sistema. (As políticas e componentes opcionais podem afetar isso.)

| Método       | URI da solicitação           |
| :---         | :---                  |
| POSTAR         | /api/bt/connectdevice |

**Parâmetros de URI**

| Parâmetro do URI | Descrição |
| :---          | :--- |
| ID            | (**necessária**) a ID de ponto de extremidade de associação para o dispositivo Bluetooth e deve ser codificada em Base64. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP | Descrição |
| :---             | :--- |
| 200              | OK |
| 4XX              | Códigos de erro |
| 5XX              | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT


---
### <a name="disconnect-a-bluetooth-device"></a>Desconectar um dispositivo Bluetooth

**Solicitação**

Desconecta o dispositivo se o dispositivo pode ser usado para áudio neste sistema. (As políticas e componentes opcionais podem afetar isso.)

| Método       | URI da solicitação              |
| :---         | :---                     |
| POSTAR         | /api/bt/disconnectdevice |

**Parâmetros de URI**

| Parâmetro do URI | Descrição |
| :---          | :--- |
| ID            | (**necessária**) a ID de ponto de extremidade de associação para o dispositivo Bluetooth e deve ser codificada em Base64. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP | Descrição |
| :---             | :--- |
| 200              | OK |
| 4XX              | Códigos de erro |
| 5XX              | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT

---
## <a name="device-manager"></a>Gerenciador de dispositivos
<hr>

### <a name="get-the-installed-devices-on-the-machine"></a>Obter os dispositivos instalados no computador

**Solicitação**

Você pode obter uma lista de dispositivos que estão instalados no computador usando o seguinte formato de solicitação:

| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/devicemanager/devices |

**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta inclui uma matriz JSON dos dispositivos conectados ao dispositivo.
```json
{"DeviceList": [
    {
        "Class": string,
        "Description": string,
        "ID": string,
        "Manufacturer": string,
        "ParentID": string,
        "ProblemCode": int,
        "StatusCode": int
    },...
]}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

|  Código de status HTTP      | Descrição | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* IoT

<hr>

### <a name="get-data-on-connected-usb-deviceshubs"></a>Obter dados em dispositivos USB/Hubs conectados

**Solicitação**

Você pode obter uma lista de descritores USB para os dispositivos USB e Hubs usando o seguinte formato de solicitação:

| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /ext/Devices/usbdevices |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta é o JSON que inclui DeviceID para o dispositivo USB, juntamente com os descritores de USB e as informações de porta para hubs.
```json
{
    "DeviceList": [
        {
        "ID": string,
        "ParentID": string, // Will equal an "ID" within the list, or be blank
        "Description": string, // optional
        "Manufacturer": string, // optional
        "ProblemCode": int, // optional
        "StatusCode": int // optional
        },
        ...
    ]
}
```

**Retornar dados de exemplo**
```json
{
    "DeviceList": [{
        "ID": "System",
        "ParentID": ""
    }, {
        "Class": "USB",
        "Description": "Texas Instruments USB 3.0 xHCI Host Controller",
        "ID": "PCI\\VEN_104C&DEV_8241&SUBSYS_1589103C&REV_02\\4&37085792&0&00E7",
        "Manufacturer": "Texas Instruments",
        "ParentID": "System",
        "ProblemCode": 0,
        "StatusCode": 25174026
    }, {
        "Class": "USB",
        "Description": "USB Composite Device",
        "DeviceDriverKey": "{36fc9e60-c465-11cf-8056-444553540000}\\0016",
        "ID": "USB\\VID_045E&PID_00DB\\8&2994096B&0&1",
        "Manufacturer": "(Standard USB Host Controller)",
        "ParentID": "USB\\VID_0557&PID_8021\\7&2E9A8711&0&4",
        "ProblemCode": 0,
        "StatusCode": 25182218
    }]
}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

|  Código de status HTTP      | Descrição | 
| :------     | :----- |
|  200 | OK | 
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

<hr>

## <a name="dump-collection"></a>Coleta de despejo

<hr>

### <a name="get-the-list-of-all-crash-dumps-for-apps"></a>Obter a lista de todos os despejos de memória para aplicativos

**Solicitação**

Você pode obter a lista de todos os despejos de memória disponíveis para todos os aplicativos de sideload usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/debug/dump/usermode/dumps |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta inclui uma lista de despejos de memória para cada aplicativo de sideload.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

|  Código de status HTTP      | Descrição | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile (no Programa Windows Insider)
* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

### <a name="get-the-crash-dump-collection-settings-for-an-app"></a>Obter configurações da coleta de despejo de memória para um app

**Solicitação**

Você pode obter as configurações da coleta de despejo de memória para um aplicativo de sideload usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/debug/dump/usermode/crashcontrol |


**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI | Descrição |
| :------          | :------ |
| packageFullname   | (**necessário**) O nome completo do pacote para o aplicativo de sideload. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta tem o formato a seguir.
```json
{"CrashDumpEnabled": bool}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

|  Código de status HTTP      | Descrição | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile (no Programa Windows Insider)
* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

### <a name="delete-a-crash-dump-for-a-sideloaded-app"></a>Excluir um despejo de memória para um app de sideload

**Solicitação**

Você pode excluir um despejo de memória de um aplicativo de sideload usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| DELETE | /api/debug/dump/usermode/crashdump |


**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI | Descrição |
| :---          | :--- |
| packageFullname   | (**necessário**) O nome completo do pacote para o aplicativo de sideload. |
| fileName   | (**necessário**) O nome do arquivo de despejo que deve ser excluído. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

|  Código de status HTTP      | Descrição | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile (no Programa Windows Insider)
* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

### <a name="disable-crash-dumps-for-a-sideloaded-app"></a>Desabilitar despejos de memória para um app de sideload

**Solicitação**

Você pode desabilitar despejos de memória para um aplicativo de sideload usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| DELETE | /api/debug/dump/usermode/crashcontrol |


**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI | Descrição |
| :---          | :--- |
| packageFullname   | (**necessário**) O nome completo do pacote para o aplicativo de sideload. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

|  Código de status HTTP      | Descrição | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile (no Programa Windows Insider)
* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

### <a name="download-the-crash-dump-for-a-sideloaded-app"></a>Baixar o despejo de memória para um app de sideload

**Solicitação**

Você pode baixar um despejo de memória de um aplicativo de sideload usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/debug/dump/usermode/crashdump |


**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI | Descrição |
| :------          | :------ |
| packageFullname   | (**necessário**) O nome completo do pacote para o aplicativo de sideload. |
| fileName   | (**necessário**) O nome do arquivo de despejo que você deseja baixar. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta inclui um arquivo de despejo. Você pode usar o WinDbg ou o Visual Studio para examinar o arquivo de despejo.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
|  200 | OK | 
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile (no Programa Windows Insider)
* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

### <a name="enable-crash-dumps-for-a-sideloaded-app"></a>Habilitar despejos de memória para um app de sideload

**Solicitação**

Você pode habilitar despejos de memória para um aplicativo de sideload usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| POSTAR | /api/debug/dump/usermode/crashcontrol |


**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI | Descrição |
| :---          | :--- |
| packageFullname   | (**necessário**) O nome completo do pacote para o aplicativo de sideload. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

|  Código de status HTTP      | Descrição | 
| :------     | :----- |
|  200 | OK | 

**Famílias de dispositivos disponíveis**

* Windows Mobile (no Programa Windows Insider)
* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

### <a name="get-the-list-of-bugcheck-files"></a>Obter a lista de arquivos de verificação de erro

**Solicitação**

Você pode obter a lista de arquivos de minidespejo de verificação de erro usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/debug/dump/kernel/dumplist |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta inclui uma lista de nomes de arquivos de despejo e os tamanhos desses arquivos. Essa lista estará no formato a seguir. 
```json
{"DumpFiles": [
    {
        "FileName": string,
        "FileSize": int
    },...
]}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

|  Código de status HTTP      | Descrição | 
| :------     | :----- |
|  200 | OK | 

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

<hr>

### <a name="download-a-bugcheck-dump-file"></a>Baixar um arquivo de despejo de verificação de erro

**Solicitação**

Você pode baixar um arquivo de despejo de verificação de erro usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/debug/dump/kernel/dump |


**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI | Descrição |
| :------          | :------ |
| filename   | (**necessário**) O nome do arquivo de despejo de memória. Você pode descobrir isso usando a API para obter a lista de despejo. |


**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta inclui o arquivo de despejo. Você pode inspecionar esse arquivo usando o WinDbg.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

|  Código de status HTTP      | Descrição | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

<hr>

### <a name="get-the-bugcheck-crash-control-settings"></a>Obter as configurações de controle de falhas de verificação de erro

**Solicitação**

Você pode obter essas configurações de controle de falhas de verificação de erro usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/debug/dump/kernel/crashcontrol |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta inclui as configurações de controle de falhas. Para saber mais sobre CrashControl, consulte o artigo [CrashControl](https://technet.microsoft.com/library/cc951703.aspx). O modelo da resposta é como está a seguir.
```json
{
    "autoreboot": bool (0 or 1),
    "dumptype": int (0 to 4),
    "maxdumpcount": int,
    "overwrite": bool (0 or 1)
}
```

**Tipos de despejo**

0: Desabilitada

1: Despejo de memória completo (coleta de toda a memória em uso)

2: Despejo de memória do kernel (ignora a memória do modo de usuário)

3: Minidespejo kernel limitado

**Código de status**

Esta API tem os códigos de status esperados a seguir.

|  Código de status HTTP      | Descrição | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

<hr>

### <a name="get-a-live-kernel-dump"></a>Obter um despejo de kernel dinâmico

**Solicitação**

Você pode obter um despejo de kernel dinâmico usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/debug/dump/livekernel |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta inclui o despejo no modo kernel completo. Você pode inspecionar esse arquivo usando o WinDbg.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

|  Código de status HTTP      | Descrição | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

<hr>

### <a name="get-a-dump-from-a-live-user-process"></a>Obter um despejo de um processo de usuário dinâmico

**Solicitação**

Você pode obter o despejo para o processo de usuário dinâmico usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/debug/dump/usermode/live |


**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI | Descrição |
| :------          | :------ |
| pid   | (**necessário**) A ID de processo exclusiva do processo no qual você está interessado. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta inclui o despejo do processo. Você pode inspecionar esse arquivo usando o WinDbg ou o Visual Studio.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

|  Código de status HTTP      | Descrição | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

<hr>

### <a name="set-the-bugcheck-crash-control-settings"></a>Definir as configurações de controle de falhas de verificação de erro

**Solicitação**

Você pode definir as configurações para coletar dados de verificação de erro usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| POSTAR | /api/debug/dump/kernel/crashcontrol |


**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI | Descrição |
| :---          | :--- |
| autoreboot   | (**opcional**) True ou false. Isso indica se o sistema reinicia automaticamente após alguma falha ou bloqueio. |
| dumptype   | (**opcional**) O tipo de despejo. Para os valores aceitos, consulte [Enumeração de CrashDumpType](https://msdn.microsoft.com/library/azure/microsoft.azure.management.insights.models.crashdumptype.aspx).|
| maxdumpcount   | (**opcional**) O número máximo de despejos para salvar. |
| overwrite   | (**opcional**) True ou false. Isso indica a substituição ou não de despejos antigos quando o limite do contador de despejos especificado pelo *maxdumpcount* foi atingido. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

|  Código de status HTTP      | Descrição | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

<hr>

## <a name="etw"></a>ETW

<hr>

### <a name="create-a-realtime-etw-session-over-a-websocket"></a>Criar uma sessão ETW em tempo real por um Websocket

**Solicitação**

Você pode criar uma sessão ETW em tempo real usando o seguinte formato de solicitação: Isso será gerenciado por um Websocket.  Os eventos ETW são enviados em lote no servidor e enviados para o cliente uma vez por segundo. 
 
| Método      | URI da solicitação |
| :------     | :----- |
| GET/WebSocket | /api/etw/session/realtime |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta inclui os eventos ETW de provedores habilitados.  Veja comandos do ETW WebSocket a seguir. 

**Código de status**

Esta API tem os códigos de status esperados a seguir.

|  Código de status HTTP      | Descrição | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

### <a name="etw-websocket-commands"></a>Comandos do ETW WebSocket
Esses comandos são enviados do cliente para o servidor.

| Comando | Descrição |
| :----- | :----- |
| provider *{guid}* enable *{level}* | Habilita o provedor marcado por *{guid}* (sem colchetes) no nível especificado. *{level}* é um **int** de 1 (menos detalhes) a 5 (detalhado). |
| provider *{guid}* disable | Desabilita o provedor marcado por *{guid}* (sem colchetes). |

Estas respostas são enviadas do servidor para o cliente. Isso é enviado como texto e você obtém o formato a seguir analisando o JSON.
```json
{
    "Events":[
        {
            "Timestamp": int,
            "ProviderName": string,
            "ID": int, 
            "TaskName": string,
            "Keyword": int,
            "Level": int,
            payload objects...
        },...
    ],
    "Frequency": int
}
```

Objetos de carga são pares de chave-valor extras (string:string) fornecidos no evento ETW original.

Exemplo:
```json
{
    "ID" : 42, 
    "Keyword" : 9223372036854775824, 
    "Level" : 4, 
    "Message" : "UDPv4: 412 bytes transmitted from 10.81.128.148:510 to 132.215.243.34:510. ",
    "PID" : "1218", 
    "ProviderName" : "Microsoft-Windows-Kernel-Network", 
    "TaskName" : "KERNEL_NETWORK_TASK_UDPIP", 
    "Timestamp" : 131039401761757686, 
    "connid" : "0", 
    "daddr" : "132.245.243.34", 
    "dport" : "500", 
    "saddr" : "10.82.128.118", 
    "seqnum" : "0", 
    "size" : "412", 
    "sport" : "500"
}
```

<hr>

### <a name="enumerate-the-registered-etw-providers"></a>Enumerar os provedores ETW registrados

**Solicitação**

Você pode enumerar os provedores registrados usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/etw/providers |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta inclui a lista de provedores ETW. A lista incluirá o nome amigável e o GUID de cada provedor no formato a seguir.
```json
{"Providers": [
    {
        "GUID": string, (GUID)
        "Name": string
    },...
]}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

|  Código de status HTTP      | Descrição | 
| :------     | :----- |
|  200 | OK | 

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

### <a name="enumerate-the-custom-etw-providers-exposed-by-the-platform"></a>Enumere os provedores ETW personalizados expostos pela plataforma.

**Solicitação**

Você pode enumerar os provedores registrados usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/etw/customproviders |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

200 OK. A resposta inclui a lista de provedores ETW. A lista incluirá o nome amigável e o GUID para cada provedor.

```json
{"Providers": [
    {
        "GUID": string, (GUID)
        "Name": string
    },...
]}
```

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

## <a name="location"></a>Location

<hr>

### <a name="get-location-override-mode"></a>Obter modo de substituição de local

**Solicitação**

Você pode obter o status da substituição de pilha de locais do dispositivo usando o formato de solicitação a seguir. O modo de desenvolvedor deve estar ativado para que esta chamada seja bem-sucedida.
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /ext/location/override |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta inclui o estado de substituição do dispositivo no formato a seguir. 

```json
{"Override" : bool}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
|  200 | OK | 
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

### <a name="set-location-override-mode"></a>Definir modo de substituição de local

**Solicitação**

Você pode definir o status da substituição de pilha de locais do dispositivo usando o formato de solicitação a seguir. Quando habilitada, a pilha de locais permite a injeção de posição. O modo de desenvolvedor deve estar ativado para que esta chamada seja bem-sucedida.

| Método      | URI da solicitação |
| :------     | :----- |
| PUT | /ext/location/override |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

```json
{"Override" : bool}
```

**Resposta**

A resposta inclui o estado de substituição para o qual o dispositivo foi definido no formato a seguir. 

```json
{"Override" : bool}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

### <a name="get-the-injected-position"></a>Obtenha a posição injetada

**Solicitação**

Você pode obter o local injetado (falsificado) do dispositivo usando o formato de solicitação a seguir. Um local injetado deve ser definido; caso contrário, um erro será lançado.
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /ext/location/position |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta inclui os valores atuais de longitude e latitude injetados no formato a seguir. 

```json
{
    "Latitude" : double,
    "Longitude" : double
}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

|  Código de status HTTP      | Descrição | 
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

### <a name="set-the-injected-position"></a>Definir a posição injetada

**Solicitação**

Você pode definir o local injetado (falsificado) do dispositivo usando o formato de solicitação a seguir. O modo de substituição de local deve ser habilitado primeiro no dispositivo, e o local definido deve ser um local válido, caso contrário, um erro será lançado.

| Método      | URI da solicitação |
| :------     | :----- |
| PUT | /ext/location/override |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

```json
{
    "Latitude" : double,
    "Longitude" : double
}
```

**Resposta**

A resposta inclui o local definido no formato a seguir. 

```json
{
    "Latitude" : double,
    "Longitude" : double
}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

## <a name="os-information"></a>Informações do sistema operacional

<hr>

### <a name="get-the-machine-name"></a>Obter o nome do computador

**Solicitação**

Você pode obter o nome de um computador usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/os/machinename |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta inclui o nome do computador no formato a seguir. 

```json
{"ComputerName": string}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

### <a name="get-the-operating-system-information"></a>Obter as informações do sistema operacional

**Solicitação**

Você pode obter as informações do sistema operacional de um computador usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/os/info |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta inclui as informações do sistema operacional no formato a seguir.

```json
{
    "ComputerName": string,
    "OsEdition": string,
    "OsEditionId": int,
    "OsVersion": string,
    "Platform": string
}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

### <a name="get-the-device-family"></a>Obter a família de dispositivos 

**Solicitação**

Você pode obter a família de dispositivos (Xbox, telefone, área de trabalho etc.) usando o seguinte formato de solicitação.
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/os/devicefamily |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta inclui a família de dispositivos (SKU - Desktop, Xbox etc.).

```json
{
   "DeviceType" : string
}
```

DeviceType aparecerá como "Windows.Xbox", "Windows.Desktop" etc. 

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

### <a name="set-the-machine-name"></a>Definir o nome do computador

**Solicitação**

Você pode definir o nome de um computador usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| POSTAR | /api/os/machinename |


**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI | Descrição |
| :------          | :------ |
| name | (**necessário**) O novo nome para o computador. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

## <a name="user-information"></a>Informações do usuário

<hr>

### <a name="get-the-active-user"></a>Obter o usuário ativo

**Solicitação**

Você pode obter o nome do usuário ativo no dispositivo usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/users/activeuser |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta inclui as informações do usuário no formato a seguir. 

Em caso de sucesso: 
```json
{
    "UserDisplayName" : string, 
    "UserSID" : string
}
```
Em caso de falha:
```json
{
    "Code" : int, 
    "CodeText" : string, 
    "Reason" : string, 
    "Success" : bool
}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

## <a name="performance-data"></a>Dados de desempenho

<hr>

### <a name="get-the-list-of-running-processes"></a>Obter a lista de processos em execução

**Solicitação**

Você pode obter a lista de processos atualmente em execução usando o seguinte formato de solicitação:  isso pode ser atualizado para uma conexão WebSocket também, com os mesmos dados JSON sendo enviados ao cliente uma vez por segundo. 
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/resourcemanager/processes |
| GET/WebSocket | /api/resourcemanager/processes |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta inclui uma lista de processos com detalhes para cada um deles. As informações estão no formato JSON e têm o modelo a seguir.
```json
{"Processes": [
    {
        "CPUUsage": float,
        "ImageName": string,
        "PageFileUsage": long,
        "PrivateWorkingSet": long,
        "ProcessId": int,
        "SessionId": int,
        "UserName": string,
        "VirtualSize": long,
        "WorkingSetSize": long
    },...
]}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

### <a name="get-the-system-performance-statistics"></a>Obter as estatísticas de desempenho do sistema

**Solicitação**

Você pode obter as estatísticas de desempenho do sistema usando o seguinte formato de solicitação: Isso inclui informações como ciclos de leitura e gravação e a quantidade de memória que foi usada.
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/resourcemanager/systemperf |
| GET/WebSocket | /api/resourcemanager/systemperf |

Isso também pode ser atualizado para uma conexão WebSocket.  Isso fornece os mesmos dados JSON a seguir uma vez por segundo. 

**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta inclui as estatísticas de desempenho do sistema como CPU e uso de GPU, acesso à memória e acesso à rede. Essas informações estão no formato JSON e têm o modelo a seguir.
```json
{
    "AvailablePages": int,
    "CommitLimit": int,
    "CommittedPages": int,
    "CpuLoad": int,
    "IOOtherSpeed": int,
    "IOReadSpeed": int,
    "IOWriteSpeed": int,
    "NonPagedPoolPages": int,
    "PageSize": int,
    "PagedPoolPages": int,
    "TotalInstalledInKb": int,
    "TotalPages": int,
    "GPUData": 
    {
        "AvailableAdapters": [{ (One per detected adapter)
            "DedicatedMemory": int,
            "DedicatedMemoryUsed": int,
            "Description": string,
            "SystemMemory": int,
            "SystemMemoryUsed": int,
            "EnginesUtilization": [ float,... (One per detected engine)]
        },...
    ]},
    "NetworkingData": {
        "NetworkInBytes": int,
        "NetworkOutBytes": int
    }
}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

## <a name="power"></a>Potência

<hr>

### <a name="get-the-current-battery-state"></a>Obter o estado atual da bateria

**Solicitação**

Você pode obter o estado atual da bateria usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/power/battery |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

As informações do estado atual da bateria são retornadas usando o formato a seguir.
```json
{
    "AcOnline": int (0 | 1),
    "BatteryPresent": int (0 | 1),
    "Charging": int (0 | 1),
    "DefaultAlert1": int,
    "DefaultAlert2": int,
    "EstimatedTime": int,
    "MaximumCapacity": int,
    "RemainingCapacity": int
}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

### <a name="get-the-active-power-scheme"></a>Obter o esquema de energia ativo

**Solicitação**

Você pode obter o esquema de energia ativo usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/power/activecfg |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

O esquema de energia ativo tem o formato a seguir.
```json
{"ActivePowerScheme": string (guid of scheme)}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

<hr>

### <a name="get-the-sub-value-for-a-power-scheme"></a>Obter o subvalor para um esquema de energia

**Solicitação**

Você pode obter o subvalor para um esquema de energia usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/power/cfg/*<power scheme path>* |

Opções:
- SCHEME_CURRENT

**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

Uma listagem completa de estados de energia disponíveis por aplicativo e as configurações para sinalizar vários estados de energia como bateria crítica e baixa. 

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

<hr>

### <a name="get-the-power-state-of-the-system"></a>Obter o estado de energia do sistema

**Solicitação**

Você pode verificar o estado de energia do sistema usando o seguinte formato de solicitação: Isso permitirá que você verifique se ele está em um estado de baixo consumo de energia.
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/power/state |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

As informações de estado de energia têm o modelo a seguir.
```json
{"LowPowerState" : false, "LowPowerStateAvailable" : true }
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

### <a name="set-the-active-power-scheme"></a>Definir o esquema de energia ativo

**Solicitação**

Você pode definir o esquema de energia ativo usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| POSTAR | /api/power/activecfg |


**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI | Descrição |
| :---          | :--- |
| scheme | (**necessário**) O GUID do esquema que você deseja definir como o esquema de energia ativo para o sistema. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

<hr>

### <a name="set-the-sub-value-for-a-power-scheme"></a>Definir o subvalor para um esquema de energia

**Solicitação**

Você pode definir o subvalor para um esquema de energia usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| POSTAR | /api/power/cfg/*<power scheme path>* |


**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI | Descrição |
| :------          | :------ |
| valueAC | (**necessário**) O valor a ser usado para energia CA. |
| valueDC | (**necessário**) O valor a ser usado para energia da bateria. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

<hr>

### <a name="get-a-sleep-study-report"></a>Obter um relatório de estudo de suspensão

**Solicitação**

| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/power/sleepstudy/report |

Você pode obter um relatório de estudo de suspensão usando o seguinte formato de solicitação:

**Parâmetros de URI**
| Parâmetro do URI | Descrição |
| :------          | :------ |
| nome_de_arquivo | (**necessário**) O nome completo do arquivo que você deseja baixar. Esse valor deve ser codificado em hex64. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta é um arquivo que contém o estudo de suspensão. 

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

<hr>

### <a name="enumerate-the-available-sleep-study-reports"></a>Enumerar os relatórios de estudo de suspensão disponíveis

**Solicitação**

Você pode enumerar os relatórios de estudo de suspensão disponíveis usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/power/sleepstudy/reports |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A lista de relatórios disponíveis tem o modelo a seguir.

```json
{"Reports": [
    {
        "FileName": string
    },...
]}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

<hr>

### <a name="get-the-sleep-study-transform"></a>Obter a transformação de estudo de suspensão

**Solicitação**

Você pode obter o relatório de estudo de suspensão usando o seguinte formato de solicitação: Essa transformação é um XSLT que converte o relatório de estudo de suspensão em um formato XML que possa ser lido por uma pessoa.
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/power/sleepstudy/transform |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta contém a transformação do estudo de suspensão.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

<hr>

## <a name="remote-control"></a>Controle remoto

<hr>

### <a name="restart-the-target-computer"></a>Reiniciar o computador de destino

**Solicitação**

Você pode reiniciar o computador de destino usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| POSTAR | /api/control/restart |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

### <a name="shut-down-the-target-computer"></a>Desligar o computador de destino

**Solicitação**

Você pode desligar o computador de destino usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| POSTAR | /api/control/shutdown |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

## <a name="task-manager"></a>Gerenciador de tarefas

<hr>

### <a name="start-a-modern-app"></a>Iniciar um aplicativo moderno

**Solicitação**

Você pode iniciar um aplicativo moderno usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| POSTAR | /api/taskmanager/app |


**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI | Descrição |
| :---          | :--- |
| appid   | (**necessário**) O PRAID do aplicativo que você deseja iniciar. Esse valor deve ser codificado em hex64. |
| pacote   | (**necessário**) O nome completo do pacote do aplicativo que você deseja iniciar. Esse valor deve ser codificado em hex64. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

### <a name="stop-a-modern-app"></a>Parar um aplicativo moderno

**Solicitação**

Você pode parar um aplicativo moderno usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| DELETE | /api/taskmanager/app |


**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI | Descrição |
| :---          | :--- |
| pacote   | (**necessário**) O nome completo dos pacotes do aplicativo que você deseja parar. Esse valor deve ser codificado em hex64. |
| forcestop   | (**opcional**) Um valor **yes** indica que o sistema deve forçar todos os processos a parar. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

### <a name="kill-process-by-pid"></a>Interromper o processo por PID

**Solicitação**

Você pode interromper um processo usando o formato de solicitação a seguir.
 
| Método      | URI da solicitação |
| :------     | :----- |
| DELETE | /api/taskmanager/process |


**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI | Descrição |
| :------          | :------ |
| pid   | (**necessário**) A ID de processo exclusiva do processo a ser interrompido. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

## <a name="networking"></a>Rede

<hr>

### <a name="get-the-current-ip-configuration"></a>Obter a configuração de IP atual

**Solicitação**

Você pode obter a configuração de IP atual usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/networking/ipconfig |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A resposta inclui a configuração de IP no modelo a seguir.

```json
{"Adapters": [
    {
        "Description": string,
        "HardwareAddress": string,
        "Index": int,
        "Name": string,
        "Type": string,
        "DHCP": {
            "LeaseExpires": int, (timestamp)
            "LeaseObtained": int, (timestamp)
            "Address": {
                "IpAddress": string,
                "Mask": string
            }
        },
        "WINS": {(WINS is optional)
            "Primary": {
                "IpAddress": string,
                "Mask": string
            },
            "Secondary": {
                "IpAddress": string,
                "Mask": string
            }
        },
        "Gateways": [{ (always 1+)
            "IpAddress": "10.82.128.1",
            "Mask": "255.255.255.255"
            },...
        ],
        "IpAddresses": [{ (always 1+)
            "IpAddress": "10.82.128.148",
            "Mask": "255.255.255.0"
            },...
        ]
    },...
]}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

### <a name="set-a-static-ip-address-ipv4-configuration"></a>Definir um endereço IP estático (configuração de IPV4)

**Solicitação**

Define a configuração de IPV4 com o IP estático e o DNS. Se não for especificado um endereço IP estático, ele habilitará DHCP. Se for especificado um endereço IP estático, em seguida, DNS deve ser especificado também.
 
| Método      | URI da solicitação |
| :------     | :----- |
| PUT | /api/networking/ipv4config |


**Parâmetros de URI**

| Parâmetro do URI | Descrição |
| :---          | :--- |
| AdapterName | (**necessária**) a GUID da interface de rede. |
| IPAddress | O endereço IP estático para definir. |
| SubnetMask | (**necessária** se *IPAddress* não for nulo) a máscara de sub-rede estático. |
| DefaultGateway | (**necessária** se *IPAddress* não for nulo) no gateway padrão estático. |
| PrimaryDNS | (**necessária** se *IPAddress* não for nulo) de DNS primário estático para definir. |
| SecondayDNS | (**necessária** se *PrimaryDNS* não for nulo) DNS secundário estático para definir. |

Para maior clareza, para definir uma interface para DHCP, serializar apenas o `AdapterName` durante a transmissão:

```json
{
    "AdapterName":"{82F86C1B-2BAE-41E3-B08D-786CA44FEED7}"
}
```

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

### <a name="enumerate-wireless-network-interfaces"></a>Enumerar as interfaces de rede sem fio

**Solicitação**

Você pode enumerar as interfaces de rede sem fio disponíveis usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/wifi/interfaces |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

Uma lista das interfaces sem fio disponíveis com detalhes no formato a seguir.

```json 
{"Interfaces": [{
    "Description": string,
    "GUID": string (guid with curly brackets),
    "Index": int,
    "ProfilesList": [
        {
            "GroupPolicyProfile": bool,
            "Name": string, (Network currently connected to)
            "PerUserProfile": bool
        },...
    ]
    }
]}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

### <a name="enumerate-wireless-networks"></a>Enumerar as redes sem fio

**Solicitação**

Você pode enumerar a lista de redes sem fio na interface especificada usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/wifi/networks |


**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI | Descrição |
| :------          | :------ |
| interface   | (**necessário**) O GUID da interface de rede a ser usado para procurar redes sem fio, sem colchetes. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A lista de redes sem fio encontradas na *interface* fornecida. Isso inclui detalhes das redes no formato a seguir.

```json
{"AvailableNetworks": [
    {
        "AlreadyConnected": bool,
        "AuthenticationAlgorithm": string, (WPA2, etc)
        "Channel": int,
        "CipherAlgorithm": string, (e.g. AES)
        "Connectable": int, (0 | 1)
        "InfrastructureType": string,
        "ProfileAvailable": bool,
        "ProfileName": string,
        "SSID": string,
        "SecurityEnabled": int, (0 | 1)
        "SignalQuality": int,
        "BSSID": [int,...],
        "PhysicalTypes": [string,...]
    },...
]}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

### <a name="connect-and-disconnect-to-a-wi-fi-network"></a>Conectar-se a uma rede Wi-Fi e desconectar-se dela

**Solicitação**

Você pode se conectar a uma rede Wi-Fi ou se desconectar dela usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| POSTAR | /api/wifi/network |


**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI | Descrição |
| :------          | :------ |
| interface   | (**necessário**) O GUID da interface de rede que você usa para se conectar à rede. |
| op   | (**necessário**) Indica a ação a ser executada. Valores possíveis são connect ou disconnect.|
| ssid   | (**necessário se *op* = = connect**) O SSID ao qual se conectar. |
| key   | (**necessário se *op* = = connect e se a rede exigir autenticação**) A chave compartilhada. |
| createprofile | (**necessário**) Crie um perfil de rede no dispositivo.  Isso fará o dispositivo se conectar automaticamente à rede no futuro. Isso pode ser **sim** ou **não**. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

### <a name="delete-a-wi-fi-profile"></a>Excluir um perfil de Wi-Fi

**Solicitação**

Você pode excluir um perfil associado a uma rede em uma interface específica usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| DELETE | /api/wifi/profile |


**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI | Descrição |
| :------          | :------ |
| interface   | (**necessário**) O GUID da interface de rede associada ao perfil a ser excluído. |
| perfil   | (**necessário**) O nome do perfil a ser excluído. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

## <a name="windows-error-reporting-wer"></a>Relatório de Erros do Windows (WER)

<hr>

### <a name="download-a-windows-error-reporting-wer-file"></a>Baixar um arquivo de relatório de erros do Windows (WER)

**Solicitação**

Você pode baixar um arquivo relacionado a WER usando o formato de solicitação a seguir:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/wer/report/file |


**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI | Descrição |
| :------          | :------ |
| usuário   | (**necessário**) O nome de usuário associado ao relatório. |
| type   | (**necessário**) O tipo de relatório. Pode ser **queried** ou **archived**. |
| name   | (**necessário**) O nome do relatório. Isso deve ser codificado em base64. |
| arquivo   | (**necessário**) O nome do arquivo a ser baixado do relatório. Isso deve ser codificado em base64. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

- A resposta contém o arquivo solicitado. 

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

### <a name="enumerate-files-in-a-windows-error-reporting-wer-report"></a>Enumerar arquivos em um relatório de erros do Windows (WER)

**Solicitação**

Você pode enumerar os arquivos em um relatório WER usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/wer/report/files |


**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI | Descrição |
| :------          | :------ |
| usuário   | (**necessário**) O usuário associado ao relatório. |
| type   | (**necessário**) O tipo de relatório. Pode ser **queried** ou **archived**. |
| name   | (**necessário**) O nome do relatório. Isso deve ser codificado em base64. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

```json
{"Files": [
    {
        "Name": string, (Filename, not base64 encoded)
        "Size": int (bytes)
    },...
]}
```

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

### <a name="list-the-windows-error-reporting-wer-reports"></a>Listar os relatórios WER (Relatório de Erros do Windows)

**Solicitação**

Você pode obter os relatórios WER usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/wer/reports |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

Os relatórios WER no formato a seguir.

```json
{"WerReports": [
    {
        "User": string,
        "Reports": [
            {
                "CreationTime": int,
                "Name": string, (not base64 encoded)
                "Type": string ("Queue" or "Archive")
            },
    },...
]}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

## <a name="windows-performance-recorder-wpr"></a>Windows Performance Recorder (WPR) 

<hr>

### <a name="start-tracing-with-a-custom-profile"></a>Inicie o rastreamento com um perfil personalizado

**Solicitação**

Você pode carregar um perfil WPR e iniciar o rastreamento com esse perfil usando o seguinte formato de solicitação:  Somente um rastreamento pode ser executado por vez. O perfil não permanecerá no dispositivo. 
 
| Método      | URI da solicitação |
| :------     | :----- |
| POSTAR | /api/wpr/customtrace |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Um corpo http correspondente a várias partes, que contém o perfil WPR personalizado.

**Resposta**

O status da sessão WPR no formato a seguir.

```json
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal or boot)
}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

### <a name="start-a-boot-performance-tracing-session"></a>Iniciar uma sessão de rastreamento de desempenho de inicialização

**Solicitação**

Você pode iniciar uma sessão de rastreamento de WPR de inicialização usando o seguinte formato de solicitação: Isso também é conhecido como uma sessão de rastreamento de desempenho.
 
| Método      | URI da solicitação |
| :------     | :----- |
| POSTAR | /api/wpr/boottrace |


**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI | Descrição |
| :------          | :------ |
| perfil   | (**necessário**) Esse parâmetro é necessário no início. O nome do perfil que deve iniciar uma sessão de rastreamento de desempenho. Os perfis possíveis estão armazenados em perfprofiles/profiles.json. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

No início, essa API retorna o status da sessão WPR no formato a seguir.

```json
{
    "SessionType": string, (Running or Idle) 
    "State": string (boot)
}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

### <a name="stop-a-boot-performance-tracing-session"></a>Parar uma sessão de rastreamento de desempenho de inicialização

**Solicitação**

Você pode parar uma sessão WPR de rastreamento de inicialização usando o seguinte formato de solicitação: Isso também é conhecido como uma sessão de rastreamento de desempenho.
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/wpr/boottrace |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

-  Nenhum.  **Observação:** Essa é uma operação de longa execução.  Ela retornará quando ETL terminar de gravar em disco.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

### <a name="start-a-performance-tracing-session"></a>Iniciar uma sessão de rastreamento de desempenho

**Solicitação**

Você pode iniciar uma sessão WPR de rastreamento usando o seguinte formato de solicitação: Isso também é conhecido como uma sessão de rastreamento de desempenho.  Somente um rastreamento pode ser executado por vez. 
 
| Método      | URI da solicitação |
| :------     | :----- |
| POSTAR | /api/wpr/trace |


**Parâmetros de URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI | Descrição |
| :------          | :------ |
| perfil   | (**necessário**) O nome do perfil que deve iniciar uma sessão de rastreamento de desempenho. Os perfis possíveis estão armazenados em perfprofiles/profiles.json. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

No início, essa API retorna o status da sessão WPR no formato a seguir.

```json
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal)
}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

### <a name="stop-a-performance-tracing-session"></a>Parar uma sessão de rastreamento de desempenho

**Solicitação**

Você pode parar uma sessão WPR de rastreamento usando o seguinte formato de solicitação: Isso também é conhecido como uma sessão de rastreamento de desempenho.
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/wpr/trace |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

- Nenhum.  **Observação:** Essa é uma operação de longa execução.  Ela retornará quando ETL terminar de gravar em disco.  

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

### <a name="retrieve-the-status-of-a-tracing-session"></a>Recuperar o status de uma sessão de rastreamento

**Solicitação**

Você pode recuperar o status da sessão WPR atual usando o seguinte formato de solicitação:
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/wpr/status |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

O status da sessão de rastreamento de WPR no formato a seguir.

```json
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal or boot)
}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

### <a name="list-completed-tracing-sessions-etls"></a>Listar sessões de rastreamento concluídas (ETLs)

**Solicitação**

Você pode obter uma lista dos rastreamentos de ETL no dispositivo usando o seguinte formato de solicitação: 

| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/wpr/tracefiles |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

A listagem de sessões de rastreamento concluídas é fornecida no formato a seguir.

```json
{"Items": [{
    "CurrentDir": string (filepath),
    "DateCreated": int (File CreationTime),
    "FileSize": int (bytes),
    "Id": string (filename),
    "Name": string (filename),
    "SubPath": string (filepath),
    "Type": int
}]}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

### <a name="download-a-tracing-session-etl"></a>Baixar uma sessão de rastreamento (ETL)

**Solicitação**

Você pode baixar um arquivo de rastreamento (rastreamento de inicialização ou rastreamento de modo de usuário) usando o seguinte formato de solicitação. 

| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/wpr/tracefile |


**Parâmetros de URI**

Você pode especificar o seguinte parâmetro adicional no URI da solicitação:

| Parâmetro do URI | Descrição |
| :------          | :------ |
| filename   | (**obrigatório**) O nome do rastreamento de ETL a ser baixado.  Podem ser encontrados em /api/wpr/tracefiles |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

- Retorna o arquivo de rastreamento ETL.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

### <a name="delete-a-tracing-session-etl"></a>Excluir uma sessão de rastreamento (ETL)

**Solicitação**

Você pode excluir um arquivo de rastreamento (rastreamento de inicialização ou rastreamento de modo de usuário) usando o seguinte formato de solicitação. 

| Método      | URI da solicitação |
| :------     | :----- |
| DELETE | /api/wpr/tracefile |


**Parâmetros de URI**

Você pode especificar o seguinte parâmetro adicional no URI da solicitação:

| Parâmetro do URI | Descrição |
| :------          | :------ |
| filename   | (**obrigatório**) O nome do rastreamento de ETL a ser excluído.  Podem ser encontrados em /api/wpr/tracefiles |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

- Retorna o arquivo de rastreamento ETL.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

<hr>

## <a name="dns-sd-tags"></a>Marcas de DNS-SD 

<hr>

### <a name="view-tags"></a>Exibir Marcas

**Solicitação**

Veja as marcas atualmente aplicadas para o dispositivo.  Elas são anunciadas por meio de registros DNS-SD TXT na chave T.  
 
| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/dns-sd/tags |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta** as marcas atualmente aplicadas no formato a seguir. 
```json
 {
    "tags": [
        "tag1", 
        "tag2", 
        ...
     ]
}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 5XX | Erro do Servidor |


**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

### <a name="delete-tags"></a>Excluir Marcas

**Solicitação**

Exclua todas as marcas atualmente anunciadas pelo DNS-SD.   
 
| Método      | URI da solicitação |
| :------     | :----- |
| DELETE | /api/dns-sd/tags |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**
 - Nenhuma

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 5XX | Erro do Servidor |


**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

<hr>

### <a name="delete-tag"></a>Excluir Marca

**Solicitação**

Exclua uma marca atualmente anunciada pelo DNS-SD.   
 
| Método      | URI da solicitação |
| :------     | :----- |
| DELETE | /api/dns-sd/tag |


**Parâmetros de URI**

| Parâmetro do URI | Descrição |
| :------     | :----- |
| tagValue | (**obrigatório**) A marca a ser removida. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**
 - Nenhuma

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |


**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT
 
<hr>

### <a name="add-a-tag"></a>Adicionar uma Marca

**Solicitação**

Adicione uma marca do anúncio DNS-SD.   
 
| Método      | URI da solicitação |
| :------     | :----- |
| POSTAR | /api/dns-sd/tag |


**Parâmetros de URI**

| Parâmetro do URI | Descrição |
| :------     | :----- |
| tagValue | (**obrigatório**) A marca a ser adicionada. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**
 - Nenhuma

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 401 | Espaço de marca excedente.  Resultado de quando a marca proposta é muito longa para o registro de serviço DNS-SD resultante. |


**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

## <a name="app-file-explorer"></a>Aplicativo Explorador de Arquivos

<hr>

### <a name="get-known-folders"></a>Obter pastas conhecidas

**Solicitação**

Obtenha uma lista de pastas de nível superior acessíveis.

| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/filesystem/apps/knownfolders |


**Parâmetros de URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta** As pastas disponíveis no formato a seguir. 
```json
 {"KnownFolders": [
    "folder0",
    "folder1",...
]}
```
**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | Implantar solicitação aceita e em processamento |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |


**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* Xbox
* IoT

<hr>

### <a name="get-files"></a>Obter arquivos

**Solicitação**

Obtenha uma lista de arquivos em uma pasta.

| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/filesystem/apps/files |


**Parâmetros de URI**

| Parâmetro do URI | Descrição |
| :------     | :----- |
| knownfolderid | (**obrigatório**) O diretório de nível superior onde você deseja a lista de arquivos. Use **LocalAppData** para acessar aplicativos de sideload. |
| packagefullname | (**obrigatório se *knownfolderid* == LocalAppData**) O nome completo do pacote do aplicativo em que você está interessado. |
| path | (**opcional**) O subdiretório dentro da pasta ou do pacote especificados acima. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta** As pastas disponíveis no formato a seguir. 
```json
{"Items": [
    {
        "CurrentDir": string (folder under the requested known folder),
        "DateCreated": int,
        "FileSize": int (bytes),
        "Id": string,
        "Name": string,
        "SubPath": string (present if this item is a folder, this is the name of the folder),
        "Type": int
    },...
]}
```
**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* Xbox
* IoT

<hr>

### <a name="download-a-file"></a>Baixar um arquivo

**Solicitação**

Obtenha um arquivo de uma pasta conhecida ou appLocalData.

| Método      | URI da solicitação |
| :------     | :----- |
| OBTER | /api/filesystem/apps/file |

**Parâmetros de URI**

| Parâmetro do URI | Descrição |
| :------     | :----- |
| knownfolderid | (**obrigatório**) O diretório de nível superior onde você deseja baixar arquivos. Use **LocalAppData** para acessar aplicativos de sideload. |
| filename | (**necessário**) O nome do arquivo que está sendo baixado. |
| packagefullname | (**necessário se *knownfolderid* == LocalAppData**) O nome completo do pacote em que você está interessado. |
| path | (**opcional**) O subdiretório dentro da pasta ou do pacote especificados acima. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- O arquivo solicitado, se presente

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | O arquivo solicitado |
| 404 | Arquivo não encontrado |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* Xbox
* IoT

<hr>

### <a name="rename-a-file"></a>Renomear um arquivo

**Solicitação**

Renomeie um arquivo em uma pasta.

| Método      | URI da solicitação |
| :------     | :----- |
| POSTAR | /api/filesystem/apps/rename |


**Parâmetros de URI**

| Parâmetro do URI | Descrição |
| :------     | :----- |
| knownfolderid | (**necessário**) O diretório de nível superior onde o arquivo está localizado. Use **LocalAppData** para acessar aplicativos de sideload. |
| filename | (**necessário**) O nome original do arquivo que está sendo renomeado. |
| newfilename | (**necessário**) O novo nome do arquivo.|
| packagefullname | (**obrigatório se *knownfolderid* == LocalAppData**) O nome completo do pacote do aplicativo em que você está interessado. |
| path | (**opcional**) O subdiretório dentro da pasta ou do pacote especificados acima. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

- Nenhuma

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |. O arquivo será excluído
| 404 | Arquivo não encontrado |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* Xbox
* IoT

<hr>

### <a name="delete-a-file"></a>Excluir um arquivo

**Solicitação**

Exclua um arquivo em uma pasta.

| Método      | URI da solicitação |
| :------     | :----- |
| DELETE | /api/filesystem/apps/file |

**Parâmetros de URI**

| Parâmetro do URI | Descrição |
| :------     | :----- |
| knownfolderid | (**obrigatório**) O diretório de nível superior onde você deseja excluir arquivos. Use **LocalAppData** para acessar aplicativos de sideload. |
| filename | (**necessário**) O nome do arquivo que está sendo excluído. |
| packagefullname | (**obrigatório se *knownfolderid* == LocalAppData**) O nome completo do pacote do aplicativo em que você está interessado. |
| path | (**opcional**) O subdiretório dentro da pasta ou do pacote especificados acima. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

- Nenhuma 

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |. O arquivo será excluído |
| 404 | Arquivo não encontrado |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* Xbox
* IoT

<hr>

### <a name="upload-a-file"></a>Carregar um arquivo

**Solicitação**

Carregue um arquivo em uma pasta.  Isso sobrescreverá um arquivo existente com o mesmo nome, mas não criará novas pastas. 

| Método      | URI da solicitação |
| :------     | :----- |
| POSTAR | /api/filesystem/apps/file |

**Parâmetros de URI**

| Parâmetro do URI | Descrição |
| :------     | :----- |
| knownfolderid | (**obrigatório**) O diretório de nível superior onde você deseja carregar arquivos. Use **LocalAppData** para acessar aplicativos de sideload. |
| packagefullname | (**obrigatório se *knownfolderid* == LocalAppData**) O nome completo do pacote do aplicativo em que você está interessado. |
| path | (**opcional**) O subdiretório dentro da pasta ou do pacote especificados acima. |

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

| Código de status HTTP      | Descrição |
| :------     | :----- |
| 200 | OK |. O arquivo é carregado |
| 4XX | Códigos de erro |
| 5XX | Códigos de erro |

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* Xbox
* IoT
