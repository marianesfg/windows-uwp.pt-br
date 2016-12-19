---
author: dbirtolo
ms.assetid: bfabd3d5-dd56-4917-9572-f3ba0de4f8c0
title: "Referência de API central do Device Portal"
description: "Saiba mais sobre as APIs REST centrais do Windows Device Portal que você pode usar para acessar os dados e controlar seu dispositivo de forma programática."
translationtype: Human Translation
ms.sourcegitcommit: b4222774dc4b0f9cdcac871311f5ead69c1e70a9
ms.openlocfilehash: 3bacb9ac42e157afaed5e9e0e6438654db03ff28

---

# <a name="device-portal-core-api-reference"></a>Referência de API central do Device Portal

Tudo no Windows Device Portal foi criado com base em APIs REST, que você pode usar para acessar os dados e controlar o dispositivo de forma programática.

## <a name="app-deployment"></a>Implantação de aplicativos

---
### <a name="install-an-app"></a>Instalar um aplicativo

**Solicitação**

Você pode instalar um aplicativo usando o formato de solicitação a seguir.

Método      | URI da solicitação
:------     | :-----
POST | /api/app/packagemanager/package
<br />
**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
package   | (**necessário**) O nome do arquivo do pacote a ser instalado.
<br />
**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- O arquivo .appx ou. appxbundle, bem como quaisquer dependências que exigem o aplicativo. 
- O certificado usado para assinar o aplicativo, se o dispositivo for IoT ou área de trabalho do Windows. Outras plataformas não exigem o certificado. 

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | Implantar solicitação aceita e em processamento
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
### <a name="get-app-installation-status"></a>Obter o status de instalação do aplicativo

**Solicitação**

Você pode obter o status de uma instalação de aplicativo que esteja em andamento usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/app/packagemanager/state
<br />
**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | O resultado da última implantação
204 | A instalação está em execução.
404 | Nenhuma ação de instalação foi encontrada
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
### <a name="uninstall-an-app"></a>Desinstalar um aplicativo

**Solicitação**

Você pode desinstalar um aplicativo usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
DELETE | /api/app/packagemanager/package
<br />

**Parâmetros do URI**

Parâmetro do URI | Descrição
:---          | :---
package   | (**necessário**) O PackageFullName (de GET/api/app/packagemanager/packages) do aplicativo de destino

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
### <a name="get-installed-apps"></a>Obter aplicativos instalados

**Solicitação**

Você pode obter uma lista de aplicativos instalados no sistema usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/app/packagemanager/packages
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

A resposta inclui uma lista de pacotes instalados com os detalhes associados. O modelo dessa resposta é o seguinte.
```
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

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
## <a name="device-manager"></a>Gerenciador de dispositivos
---
### <a name="get-the-installed-devices-on-the-machine"></a>Obter os dispositivos instalados no computador

**Solicitação**

Você pode obter uma lista de dispositivos que estão instalados no computador usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/devicemanager/devices
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

A resposta inclui uma matriz JSON dos dispositivos conectados ao dispositivo.
``` 
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

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* IoT

---
## <a name="dump-collection"></a>Coleta de despejo
---
### <a name="get-the-list-of-all-crash-dumps-for-apps"></a>Obter a lista de todos os despejos de memória para aplicativos

**Solicitação**

Você pode obter a lista de todos os despejos de memória disponíveis para todos os aplicativos de sideload usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/debug/dump/usermode/dumps
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

A resposta inclui uma lista de despejos de memória para cada aplicativo de sideload.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile (no Programa Windows Insider)
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### <a name="get-the-crash-dump-collection-settings-for-an-app"></a>Obter configurações da coleta de despejo de memória para um app

**Solicitação**

Você pode obter as configurações da coleta de despejo de memória para um aplicativo de sideload usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/debug/dump/usermode/crashcontrol
<br />

**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
packageFullname   | (**necessário**) O nome completo do pacote para o aplicativo de sideload.
<br />
**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

A resposta tem o formato a seguir.
```
{"CrashDumpEnabled": bool}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile (no Programa Windows Insider)
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### <a name="delete-a-crash-dump-for-a-sideloaded-app"></a>Excluir um despejo de memória para um app de sideload

**Solicitação**

Você pode excluir um despejo de memória de um aplicativo de sideload usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
DELETE | /api/debug/dump/usermode/crashdump
<br />

**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
packageFullname   | (**necessário**) O nome completo do pacote para o aplicativo de sideload.
fileName   | (**necessário**) O nome do arquivo de despejo que deve ser excluído.
<br />
**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile (no Programa Windows Insider)
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### <a name="disable-crash-dumps-for-a-sideloaded-app"></a>Desabilitar despejos de memória para um app de sideload

**Solicitação**

Você pode desabilitar despejos de memória para um aplicativo de sideload usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
DELETE | /api/debug/dump/usermode/crashcontrol

<br />
**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
packageFullname   | (**necessário**) O nome completo do pacote para o aplicativo de sideload.
<br />
**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile (no Programa Windows Insider)
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### <a name="download-the-crash-dump-for-a-sideloaded-app"></a>Baixar o despejo de memória para um app de sideload

**Solicitação**

Você pode baixar um despejo de memória de um aplicativo de sideload usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/debug/dump/usermode/crashdump
<br />

**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
packageFullname   | (**necessário**) O nome completo do pacote para o aplicativo de sideload.
fileName   | (**necessário**) O nome do arquivo de despejo que você deseja baixar.
<br />
**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

A resposta inclui um arquivo de despejo. Você pode usar o WinDbg ou o Visual Studio para examinar o arquivo de despejo.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile (no Programa Windows Insider)
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### <a name="enable-crash-dumps-for-a-sideloaded-app"></a>Habilitar despejos de memória para um app de sideload

**Solicitação**

Você pode habilitar despejos de memória para um aplicativo de sideload usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/debug/dump/usermode/crashcontrol
<br />

**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
packageFullname   | (**necessário**) O nome completo do pacote para o aplicativo de sideload.
<br />
**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile (no Programa Windows Insider)
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### <a name="get-the-list-of-bugcheck-files"></a>Obter a lista de arquivos de verificação de erro

**Solicitação**

Você pode obter a lista de arquivos de minidespejo de verificação de erro usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/debug/dump/kernel/dumplist
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

A resposta inclui uma lista de nomes de arquivos de despejo e os tamanhos desses arquivos. Essa lista estará no formato a seguir. 
```
{"DumpFiles": [
    {
        "FileName": string,
        "FileSize": int
    },...
]}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
<br />
**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
### <a name="download-a-bugcheck-dump-file"></a>Baixar um arquivo de despejo de verificação de erro

**Solicitação**

Você pode baixar um arquivo de despejo de verificação de erro usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/debug/dump/kernel/dump
<br />

**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
filename   | (**necessário**) O nome do arquivo de despejo de memória. Você pode descobrir isso usando a API para obter a lista de despejo.
<br />
**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

A resposta inclui o arquivo de despejo. Você pode inspecionar esse arquivo usando o WinDbg.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
### <a name="get-the-bugcheck-crash-control-settings"></a>Obter as configurações de controle de falhas de verificação de erro

**Solicitação**

Você pode obter essas configurações de controle de falhas de verificação de erro usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/debug/dump/kernel/crashcontrol

<br />
**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

A resposta inclui as configurações de controle de falhas. Para saber mais sobre CrashControl, consulte o artigo [CrashControl](https://technet.microsoft.com/library/cc951703.aspx). O modelo da resposta é como está a seguir.
```
{
    "autoreboot": bool (0 or 1),
    "dumptype": int (0 to 4),
    "maxdumpcount": int,
    "overwrite": bool (0 or 1)
}
```

**Tipos de despejo**

0: desabilitado

1: despejo de memória completo (coleta toda a memória em uso)

2: despejo de memória do kernel (ignora a memória do modo de usuário)

3: minidespejo de kernel limitado

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
### <a name="get-a-live-kernel-dump"></a>Obter um despejo de kernel dinâmico

**Solicitação**

Você pode obter um despejo de kernel dinâmico usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/debug/dump/livekernel
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

A resposta inclui o despejo no modo kernel completo. Você pode inspecionar esse arquivo usando o WinDbg.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
### <a name="get-a-dump-from-a-live-user-process"></a>Obter um despejo de um processo de usuário dinâmico

**Solicitação**

Você pode obter o despejo para o processo de usuário dinâmico usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/debug/dump/usermode/live
<br />

**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
pid   | (**necessário**) A ID de processo exclusiva do processo no qual você está interessado.
<br />
**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

A resposta inclui o despejo do processo. Você pode inspecionar esse arquivo usando o WinDbg ou o Visual Studio.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
### <a name="set-the-bugcheck-crash-control-settings"></a>Definir as configurações de controle de falhas de verificação de erro

**Solicitação**

Você pode definir as configurações para coletar dados de verificação de erro usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/debug/dump/kernel/crashcontrol
<br />

**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
autoreboot   | (**opcional**) True ou false. Isso indica se o sistema reinicia automaticamente após alguma falha ou bloqueio.
dumptype   | (**opcional**) O tipo de despejo. Para os valores aceitos, consulte [Enumeração de CrashDumpType](https://msdn.microsoft.com/library/azure/microsoft.azure.management.insights.models.crashdumptype.aspx).
maxdumpcount   | (**opcional**) O número máximo de despejos para salvar.
overwrite   | (**opcional**) True ou false. Isso indica a substituição ou não de despejos antigos quando o limite do contador de despejos especificado pelo *maxdumpcount* foi atingido.
<br />
**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
## <a name="etw"></a>ETW
---
### <a name="create-a-realtime-etw-session-over-a-websocket"></a>Criar uma sessão ETW em tempo real por um Websocket

**Solicitação**

Você pode criar uma sessão ETW em tempo real usando o seguinte formato de solicitação: Isso será gerenciado por um Websocket.  Os eventos ETW são enviados em lote no servidor e enviados para o cliente uma vez por segundo. 
 
Método      | URI da solicitação
:------     | :-----
GET/WebSocket | /api/etw/session/realtime
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

A resposta inclui os eventos ETW de provedores habilitados.  Veja comandos do ETW WebSocket a seguir. 

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

### <a name="etw-websocket-commands"></a>Comandos do ETW WebSocket
Esses comandos são enviados do cliente para o servidor.

Comando | Descrição
:----- | :-----
provider *{guid}* enable *{level}* | Habilita o provedor marcado por *{guid}* (sem colchetes) no nível especificado. *{level}* é um **int** de 1 (menos detalhes) a 5 (detalhado).
provider *{guid}* disable | Desabilita o provedor marcado por *{guid}* (sem colchetes).

Estas respostas são enviadas do servidor para o cliente. Isso é enviado como texto e você obtém o formato a seguir analisando o JSON.
```
{
    "Events":[
        {
            "Timestamp": int,
            "Provider": string,
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
```
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

---
### <a name="enumerate-the-registered-etw-providers"></a>Enumerar os provedores ETW registrados

**Solicitação**

Você pode enumerar os provedores registrados usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/etw/providers
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

A resposta inclui a lista de provedores ETW. A lista incluirá o nome amigável e o GUID de cada provedor no formato a seguir.
```
{"Providers": [
    {
        "GUID": string, (GUID)
        "Name": string
    },...
]}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### <a name="enumerate-the-custom-etw-providers-exposed-by-the-platform"></a>Enumere os provedores ETW personalizados expostos pela plataforma.

**Solicitação**

Você pode enumerar os provedores registrados usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/etw/customproviders
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

200 OK. A resposta inclui a lista de provedores ETW. A lista incluirá o nome amigável e o GUID para cada provedor.

```
{"Providers": [
    {
        "GUID": string, (GUID)
        "Name": string
    },...
]}
```

**Código de status**

- Códigos de status padrão.
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

---
## <a name="os-information"></a>Informações do sistema operacional
---
### <a name="get-the-machine-name"></a>Obter o nome do computador

**Solicitação**

Você pode obter o nome de um computador usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/os/machinename
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

A resposta inclui o nome do computador no formato a seguir. 

```
{"ComputerName": string}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
### <a name="get-the-operating-system-information"></a>Obter as informações do sistema operacional

**Solicitação**

Você pode obter as informações do sistema operacional de um computador usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/os/info
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

A resposta inclui as informações do sistema operacional no formato a seguir.

```
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

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
### <a name="get-the-device-family"></a>Obter a família de dispositivos 

**Solicitação**

Você pode obter a família de dispositivos (Xbox, telefone, área de trabalho etc.) usando o seguinte formato de solicitação.
 
Método      | URI da solicitação
:------     | :-----
GET | /api/os/devicefamily
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

A resposta inclui a família de dispositivos (SKU - Desktop, Xbox etc.).

```
{
   "DeviceType" : string
}
```

DeviceType aparecerá como "Windows.Xbox", "Windows.Desktop" etc. 

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
### <a name="set-the-machine-name"></a>Definir o nome do computador

**Solicitação**

Você pode definir o nome de um computador usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/os/machinename
<br />

**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
name | (**necessário**) O novo nome para o computador.
<br />
**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
## <a name="performance-data"></a>Dados de desempenho
---
### <a name="get-the-list-of-running-processes"></a>Obter a lista de processos em execução

**Solicitação**

Você pode obter a lista de processos atualmente em execução usando o seguinte formato de solicitação:  isso pode ser atualizado para uma conexão WebSocket também, com os mesmos dados JSON sendo enviados ao cliente uma vez por segundo. 
 
Método      | URI da solicitação
:------     | :-----
GET | /api/resourcemanager/processes
GET/WebSocket | /api/resourcemanager/processes
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

A resposta inclui uma lista de processos com detalhes para cada um deles. As informações estão no formato JSON e têm o modelo a seguir.
```
{"Processes": [
    {
        "CPUUsage": int,
        "ImageName": string,
        "PageFileUsage": int,
        "PrivateWorkingSet": int,
        "ProcessId": int,
        "SessionId": int,
        "UserName": string,
        "VirtualSize": int,
        "WorkingSetSize": int
    },...
]}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### <a name="get-the-system-performance-statistics"></a>Obter as estatísticas de desempenho do sistema

**Solicitação**

Você pode obter as estatísticas de desempenho do sistema usando o seguinte formato de solicitação: Isso inclui informações como ciclos de leitura e gravação e a quantidade de memória que foi usada.
 
Método      | URI da solicitação
:------     | :-----
GET | /api/resourcemanager/systemperf
GET/WebSocket | /api/resourcemanager/systemperf
<br />
Isso também pode ser atualizado para uma conexão WebSocket.  Isso fornece os mesmos dados JSON a seguir uma vez por segundo. 

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

A resposta inclui as estatísticas de desempenho do sistema como CPU e uso de GPU, acesso à memória e acesso à rede. Essas informações estão no formato JSON e têm o modelo a seguir.
```
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

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
## <a name="power"></a>Energia
---
### <a name="get-the-current-battery-state"></a>Obter o estado atual da bateria

**Solicitação**

Você pode obter o estado atual da bateria usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/power/battery
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

As informações do estado atual da bateria são retornadas usando o formato a seguir.
```
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

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### <a name="get-the-active-power-scheme"></a>Obter o esquema de energia ativo

**Solicitação**

Você pode obter o esquema de energia ativo usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/power/activecfg
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

O esquema de energia ativo tem o formato a seguir.
```
{"ActivePowerScheme": string (guid of scheme)}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
### <a name="get-the-sub-value-for-a-power-scheme"></a>Obter o subvalor para um esquema de energia

**Solicitação**

Você pode obter o subvalor para um esquema de energia usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/power/cfg/*<power scheme path>*
<br />
Opções:
- SCHEME_CURRENT

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

Uma listagem completa de estados de energia disponíveis por aplicativo e as configurações para sinalizar vários estados de energia como bateria crítica e baixa. 

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
### <a name="get-the-power-state-of-the-system"></a>Obter o estado de energia do sistema

**Solicitação**

Você pode verificar o estado de energia do sistema usando o seguinte formato de solicitação: Isso permitirá que você verifique se ele está em um estado de baixo consumo de energia.
 
Método      | URI da solicitação
:------     | :-----
GET | /api/power/state
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

As informações de estado de energia têm o modelo a seguir.
```
{"LowPowerStateAvailable": bool}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT

---
### <a name="set-the-active-power-scheme"></a>Definir o esquema de energia ativo

**Solicitação**

Você pode definir o esquema de energia ativo usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/power/activecfg
<br />

**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
scheme | (**necessário**) O GUID do esquema que você deseja definir como o esquema de energia ativo para o sistema.
<br />
**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
### <a name="set-the-sub-value-for-a-power-scheme"></a>Definir o subvalor para um esquema de energia

**Solicitação**

Você pode definir o subvalor para um esquema de energia usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/power/cfg/*<power scheme path>*
<br />

**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
valueAC | (**necessário**) O valor a ser usado para energia CA.
valueDC | (**necessário**) O valor a ser usado para energia da bateria.
<br />
**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
<br />
**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
### <a name="get-a-sleep-study-report"></a>Obter um relatório de estudo de suspensão

**Solicitação**

Método      | URI da solicitação
:------     | :-----
GET | /api/power/sleepstudy/report
<br />
Você pode obter um relatório de estudo de suspensão usando o seguinte formato de solicitação:

**Parâmetros do URI**
Parâmetro do URI | Descrição
:---          | :---
FileName | (**necessário**) O nome completo do arquivo que você deseja baixar. Esse valor deve ser codificado em hex64.
<br />
**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

A resposta é um arquivo que contém o estudo de suspensão. 

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
### <a name="enumerate-the-available-sleep-study-reports"></a>Enumerar os relatórios de estudo de suspensão disponíveis

**Solicitação**

Você pode enumerar os relatórios de estudo de suspensão disponíveis usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/power/sleepstudy/reports
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

A lista de relatórios disponíveis tem o modelo a seguir.

```
{"Reports": [
    {
        "FileName": string
    },...
]}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
### <a name="get-the-sleep-study-transform"></a>Obter a transformação de estudo de suspensão

**Solicitação**

Você pode obter o relatório de estudo de suspensão usando o seguinte formato de solicitação: Essa transformação é um XSLT que converte o relatório de estudo de suspensão em um formato XML que possa ser lido por uma pessoa.
 
Método      | URI da solicitação
:------     | :-----
GET | /api/power/sleepstudy/transform
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

A resposta contém a transformação do estudo de suspensão.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
## <a name="remote-control"></a>Controle remoto
---
### <a name="restart-the-target-computer"></a>Reiniciar o computador de destino

**Solicitação**

Você pode reiniciar o computador de destino usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/control/restart
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
### <a name="shut-down-the-target-computer"></a>Desligar o computador de destino

**Solicitação**

Você pode desligar o computador de destino usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/control/shutdown
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
## <a name="task-manager"></a>Gerenciador de tarefas
---
### <a name="start-a-modern-app"></a>Iniciar um aplicativo moderno

**Solicitação**

Você pode iniciar um aplicativo moderno usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/taskmanager/app
<br />

**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
appid   | (**necessário**) O PRAID do aplicativo que você deseja iniciar. Esse valor deve ser codificado em hex64.
package   | (**necessário**) O nome completo do pacote do aplicativo que você deseja iniciar. Esse valor deve ser codificado em hex64.
<br />
**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
### <a name="stop-a-modern-app"></a>Parar um aplicativo moderno

**Solicitação**

Você pode parar um aplicativo moderno usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
DELETE | /api/taskmanager/app
<br />

**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
package   | (**necessário**) O nome completo dos pacotes do aplicativo que você deseja parar. Esse valor deve ser codificado em hex64.
forcestop   | (**opcional**) Um valor **yes** indica que o sistema deve forçar todos os processos a parar.
<br />
**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
## <a name="networking"></a>Rede
---
### <a name="get-the-current-ip-configuration"></a>Obter a configuração de IP atual

**Solicitação**

Você pode obter a configuração de IP atual usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/networking/ipconfig
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

A resposta inclui a configuração de IP no modelo a seguir.

```
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

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

--
### <a name="enumerate-wireless-network-interfaces"></a>Enumerar as interfaces de rede sem fio

**Solicitação**

Você pode enumerar as interfaces de rede sem fio disponíveis usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/wifi/interfaces
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

Uma lista das interfaces sem fio disponíveis com detalhes no formato a seguir.

``` 
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

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
### <a name="enumerate-wireless-networks"></a>Enumerar as redes sem fio

**Solicitação**

Você pode enumerar a lista de redes sem fio na interface especificada usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/wifi/networks
<br />

**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
interface   | (**necessário**) O GUID da interface de rede a ser usado para procurar redes sem fio, sem colchetes. 
<br />
**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

A lista de redes sem fio encontradas na *interface* fornecida. Isso inclui detalhes das redes no formato a seguir.

```
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

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
### <a name="connect-and-disconnect-to-a-wi-fi-network"></a>Conectar-se a uma rede Wi-Fi e desconectar-se dela

**Solicitação**

Você pode se conectar a uma rede Wi-Fi ou se desconectar dela usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/wifi/network
<br />

**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
interface   | (**necessário**) O GUID da interface de rede que você usa para se conectar à rede.
op   | (**necessário**) Indica a ação a ser executada. Valores possíveis são connect ou disconnect.
ssid   | (**necessário se *op* == connect**) O SSID ao qual se conectar.
chave   | (**necessário se *op* = = conectar e rede requer autenticação* *) A chave compartilhada.
createprofile | (**necessário**) Crie um perfil de rede no dispositivo.  Isso fará o dispositivo se conectar automaticamente à rede no futuro. Isso pode ser **sim** ou **não**. 

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
### <a name="delete-a-wi-fi-profile"></a>Excluir um perfil de Wi-Fi

**Solicitação**

Você pode excluir um perfil associado a uma rede em uma interface específica usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
DELETE | /api/wifi/network
<br />

**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
interface   | (**necessário**) O GUID da interface de rede associada ao perfil a ser excluído.
profile   | (**necessário**) O nome do perfil a ser excluído.
<br />
**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
## <a name="windows-error-reporting-wer"></a>Relatório de Erros do Windows (WER)
---
### <a name="download-a-windows-error-reporting-wer-file"></a>Baixar um arquivo de relatório de erros do Windows (WER)

**Solicitação**

Você pode baixar um arquivo relacionado a WER usando o formato de solicitação a seguir:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/wer/report/file
<br />

**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
user   | (**necessário**) O nome de usuário associado ao relatório.
type   | (**necessário**) O tipo de relatório. Pode ser **queried** ou **archived**.
name   | (**necessário**) O nome do relatório. Isso deve ser codificado em base64. 
file   | (**necessário**) O nome do arquivo a ser baixado do relatório. Isso deve ser codificado em base64. 
<br />
**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- A resposta contém o arquivo solicitado. 

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT

---
### <a name="enumerate-files-in-a-windows-error-reporting-wer-report"></a>Enumerar arquivos em um relatório de erros do Windows (WER)

**Solicitação**

Você pode enumerar os arquivos em um relatório WER usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/wer/report/files
<br />

**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
user   | (**necessário**) O usuário associado ao relatório.
type   | (**necessário**) O tipo de relatório. Pode ser **queried** ou **archived**.
name   | (**necessário**) O nome do relatório. Isso deve ser codificado em base64. 
<br />
**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

```
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

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT

---
### <a name="list-the-windows-error-reporting-wer-reports"></a>Listar os relatórios WER (Relatório de Erros do Windows)

**Solicitação**

Você pode obter os relatórios WER usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/wer/reports
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

Os relatórios WER no formato a seguir.

```
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

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT

---
## <a name="windows-performance-recorder-wpr"></a>Windows Performance Recorder (WPR) 
---
### <a name="start-tracing-with-a-custom-profile"></a>Inicie o rastreamento com um perfil personalizado

**Solicitação**

Você pode carregar um perfil WPR e iniciar o rastreamento com esse perfil usando o seguinte formato de solicitação:  Somente um rastreamento pode ser executado por vez. O perfil não permanecerá no dispositivo. 
 
Método      | URI da solicitação
:------     | :-----
POST | /api/wpr/customtrace
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Um corpo http correspondente a várias partes, que contém o perfil WPR personalizado.

**Resposta**

O status da sessão WPR no formato a seguir.

```
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal or boot)
}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### <a name="start-a-boot-performance-tracing-session"></a>Iniciar uma sessão de rastreamento de desempenho de inicialização

**Solicitação**

Você pode iniciar uma sessão de rastreamento de WPR de inicialização usando o seguinte formato de solicitação: Isso também é conhecido como uma sessão de rastreamento de desempenho.
 
Método      | URI da solicitação
:------     | :-----
POST | /api/wpr/boottrace
<br />

**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
profile   | (**necessário**) Esse parâmetro é necessário no início. O nome do perfil que deve iniciar uma sessão de rastreamento de desempenho. Os perfis possíveis estão armazenados em perfprofiles/profiles.json.
<br />
**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

No início, essa API retorna o status da sessão WPR no formato a seguir.

```
{
    "SessionType": string, (Running or Idle) 
    "State": string (boot)
}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### <a name="stop-a-boot-performance-tracing-session"></a>Parar uma sessão de rastreamento de desempenho de inicialização

**Solicitação**

Você pode parar uma sessão WPR de rastreamento de inicialização usando o seguinte formato de solicitação: Isso também é conhecido como uma sessão de rastreamento de desempenho.
 
Método      | URI da solicitação
:------     | :-----
GET | /api/wpr/boottrace
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

-  Nenhuma.  **Observação:** é uma operação de longa execução.  Ela retornará quando ETL terminar de gravar em disco.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### <a name="start-a-performance-tracing-session"></a>Iniciar uma sessão de rastreamento de desempenho

**Solicitação**

Você pode iniciar uma sessão WPR de rastreamento usando o seguinte formato de solicitação: Isso também é conhecido como uma sessão de rastreamento de desempenho.  Somente um rastreamento pode ser executado por vez. 
 
Método      | URI da solicitação
:------     | :-----
POST | /api/wpr/trace
<br />

**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
profile   | (**necessário**) O nome do perfil que deve iniciar uma sessão de rastreamento de desempenho. Os perfis possíveis estão armazenados em perfprofiles/profiles.json.
<br />
**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

No início, essa API retorna o status da sessão WPR no formato a seguir.

```
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal)
}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### <a name="stop-a-performance-tracing-session"></a>Parar uma sessão de rastreamento de desempenho

**Solicitação**

Você pode parar uma sessão WPR de rastreamento usando o seguinte formato de solicitação: Isso também é conhecido como uma sessão de rastreamento de desempenho.
 
Método      | URI da solicitação
:------     | :-----
GET | /api/wpr/trace
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma.  **Observação:** é uma operação de longa execução.  Ela retornará quando ETL terminar de gravar em disco.  

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### <a name="retrieve-the-status-of-a-tracing-session"></a>Recuperar o status de uma sessão de rastreamento

**Solicitação**

Você pode recuperar o status da sessão WPR atual usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/wpr/status
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

O status da sessão de rastreamento de WPR no formato a seguir.

```
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal or boot)
}
```

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### <a name="list-completed-tracing-sessions-etls"></a>Listar sessões de rastreamento concluídas (ETLs)

**Solicitação**

Você pode obter uma lista dos rastreamentos de ETL no dispositivo usando o seguinte formato de solicitação: 

Método      | URI da solicitação
:------     | :-----
GET | /api/wpr/tracefiles
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

A listagem de sessões de rastreamento concluídas é fornecida no formato a seguir.

```
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

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### <a name="download-a-tracing-session-etl"></a>Baixar uma sessão de rastreamento (ETL)

**Solicitação**

Você pode baixar um arquivo de rastreamento (rastreamento de inicialização ou rastreamento de modo de usuário) usando o seguinte formato de solicitação. 

Método      | URI da solicitação
:------     | :-----
GET | /api/wpr/tracefile
<br />

**Parâmetros do URI**

Você pode especificar o seguinte parâmetro adicional no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
filename   | (**obrigatório**) O nome do rastreamento de ETL a ser baixado.  Podem ser encontrados em /api/wpr/tracefiles

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Retorna o arquivo de rastreamento ETL.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### <a name="delete-a-tracing-session-etl"></a>Excluir uma sessão de rastreamento (ETL)

**Solicitação**

Você pode excluir um arquivo de rastreamento (rastreamento de inicialização ou rastreamento de modo de usuário) usando o seguinte formato de solicitação. 

Método      | URI da solicitação
:------     | :-----
DELETE | /api/wpr/tracefile
<br />

**Parâmetros do URI**

Você pode especificar o seguinte parâmetro adicional no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
filename   | (**obrigatório**) O nome do rastreamento de ETL a ser excluído.  Podem ser encontrados em /api/wpr/tracefiles

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Retorna o arquivo de rastreamento ETL.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

---
## <a name="dns-sd-tags"></a>Marcas de DNS-SD 
---
### <a name="view-tags"></a>Exibir Marcas

**Solicitação**

Veja as marcas atualmente aplicadas para o dispositivo.  Elas são anunciadas por meio de registros DNS-SD TXT na chave T.  
 
Método      | URI da Solicitação
:------     | :-----
GET | /api/dns-sd/tags
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta** As tags atualmente aplicadas no formato a seguir. 
```
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

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
5XX | Erro do Servidor 

<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
### <a name="delete-tags"></a>Excluir Marcas

**Solicitação**

Exclua todas as marcas atualmente anunciadas pelo DNS-SD.   
 
Método      | URI da Solicitação
:------     | :-----
DELETE | /api/dns-sd/tags
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**
 - Nenhuma

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
5XX | Erro do Servidor 

<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
### <a name="delete-tag"></a>Excluir Marca

**Solicitação**

Exclua uma marca atualmente anunciada pelo DNS-SD.   
 
Método      | URI da Solicitação
:------     | :-----
DELETE | /api/dns-sd/tag
<br />

**Parâmetros do URI**

Parâmetro do URI | Descrição
:------     | :-----
tagValue | (**obrigatório**) A marca a ser removida.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**
 - Nenhuma

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK

<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT
 
---
### <a name="add-a-tag"></a>Adicionar uma Marca

**Solicitação**

Adicione uma marca do anúncio DNS-SD.   
 
Método      | URI da Solicitação
:------     | :-----
POST | /api/dns-sd/tag
<br />

**Parâmetros do URI**

Parâmetro do URI | Descrição
:------     | :-----
tagValue | (**obrigatório**) A marca a ser adicionada.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**
 - Nenhum

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
401 | Espaço de marca excedente.  Resultado de quando a marca proposta é muito longa para o registro de serviço DNS-SD resultante.  

<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

## <a name="app-file-explorer"></a>Aplicativo Explorador de Arquivos

---
### <a name="get-known-folders"></a>Obter pastas conhecidas

**Solicitação**

Obtenha uma lista de pastas de nível superior acessíveis.

Método      | URI da Solicitação
:------     | :-----
GET | /api/filesystem/apps/knownfolders
<br />

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta** As pastas disponíveis no formato a seguir. 
```
 {"KnownFolders": [
    "folder0",
    "folder1",...
]}
```
**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | Implantar solicitação aceita e em processamento
4XX | Códigos de erro
5XX | Códigos de erro
<br />

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* Xbox
* IoT

---
### <a name="get-files"></a>Obter arquivos

**Solicitação**

Obtenha uma lista de arquivos em uma pasta.

Método      | URI da Solicitação
:------     | :-----
GET | /api/filesystem/apps/files
<br />

**Parâmetros do URI**

Parâmetro do URI | Descrição
:------     | :-----
knownfolderid | (**obrigatório**) O diretório de nível superior onde você deseja a lista de arquivos. Use **LocalAppData** para acessar aplicativos de sideload. 
packagefullname | (**obrigatório se *knownfolderid* == LocalAppData**) O nome completo do pacote do aplicativo em que você está interessado. 
path | (**opcional**) O subdiretório dentro da pasta ou do pacote especificados acima. 

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta** As pastas disponíveis no formato a seguir. 
```
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

Código de status HTTP      | Descrição
:------     | :-----
200 | OK
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* Xbox
* IoT

---
### <a name="download-a-file"></a>Baixar um arquivo

**Solicitação**

Obtenha um arquivo de uma pasta conhecida ou appLocalData.

Método      | URI da solicitação
:------     | :-----
GET | /api/filesystem/apps/file

**Parâmetros do URI**

Parâmetro do URI | Descrição
:------     | :-----
knownfolderid | (**obrigatório**) O diretório de nível superior onde você deseja baixar arquivos. Use **LocalAppData** para acessar aplicativos de sideload. 
filename | (**necessário**) O nome do arquivo que está sendo baixado. 
packagefullname | (**obrigatório se *knownfolderid* == LocalAppData**) O nome completo do pacote em que você está interessado. 
path | (**opcional**) O subdiretório dentro da pasta ou do pacote especificados acima.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- O arquivo solicitado, se presente

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | O arquivo solicitado
404 | Arquivo não encontrado
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* Xbox
* IoT

---
### <a name="rename-a-file"></a>Renomear um arquivo

**Solicitação**

Renomeie um arquivo em uma pasta.

Método      | URI da Solicitação
:------     | :-----
POST | /api/filesystem/apps/rename

<br />
**Parâmetros do URI**

Parâmetro do URI | Descrição
:------     | :-----
knownfolderid | (**necessário**) O diretório de nível superior onde o arquivo está localizado. Use **LocalAppData** para acessar aplicativos de sideload. 
filename | (**necessário**) O nome original do arquivo que está sendo renomeado. 
newfilename | (**necessário**) O novo nome do arquivo.
packagefullname | (**obrigatório se *knownfolderid* == LocalAppData**) O nome completo do pacote do aplicativo em que você está interessado. 
path | (**opcional**) O subdiretório dentro da pasta ou do pacote especificados acima. 

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK. O arquivo será excluído
404 | Arquivo não encontrado
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* Xbox
* IoT

---
### <a name="delete-a-file"></a>Excluir um arquivo

**Solicitação**

Exclua um arquivo em uma pasta.

Método      | URI da Solicitação
:------     | :-----
DELETE | /api/filesystem/apps/file
<br />
**Parâmetros do URI**

Parâmetro do URI | Descrição
:------     | :-----
knownfolderid | (**obrigatório**) O diretório de nível superior onde você deseja excluir arquivos. Use **LocalAppData** para acessar aplicativos de sideload. 
filename | (**necessário**) O nome do arquivo que está sendo excluído. 
packagefullname | (**obrigatório se *knownfolderid* == LocalAppData**) O nome completo do pacote do aplicativo em que você está interessado. 
path | (**opcional**) O subdiretório dentro da pasta ou do pacote especificados acima.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma 

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK. O arquivo será excluído
404 | Arquivo não encontrado
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* Xbox
* IoT

---
### <a name="upload-a-file"></a>Carregar um arquivo

**Solicitação**

Carregue um arquivo em uma pasta.  Isso sobrescreverá um arquivo existente com o mesmo nome, mas não criará novas pastas. 

Método      | URI da Solicitação
:------     | :-----
POST | /api/filesystem/apps/file
<br />
**Parâmetros do URI**

Parâmetro do URI | Descrição
:------     | :-----
knownfolderid | (**obrigatório**) O diretório de nível superior onde você deseja carregar arquivos. Use **LocalAppData** para acessar aplicativos de sideload.
packagefullname | (**obrigatório se *knownfolderid* == LocalAppData**) O nome completo do pacote do aplicativo em que você está interessado. 
path | (**opcional**) O subdiretório dentro da pasta ou do pacote especificados acima.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | OK. O arquivo é carregado
4XX | Códigos de erro
5XX | Códigos de erro
<br />
**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* Xbox
* IoT


<!--HONumber=Dec16_HO1-->


