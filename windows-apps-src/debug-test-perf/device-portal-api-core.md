---
ms.assetid: bfabd3d5-dd56-4917-9572-f3ba0de4f8c0
title: Referência de API central do Device Portal
description: Saiba mais sobre as APIs REST centrais do Windows Device Portal que você pode usar para acessar os dados e controlar seu dispositivo de forma programática.
---

# Referência de API central do Device Portal

Tudo no Windows Device Portal foi criado com base em APIs REST, que você pode usar para acessar os dados e controlar o dispositivo de forma programática.

## Implantação de aplicativos

---
### Instalar um aplicativo

**Solicitação**

Você pode instalar um aplicativo usando o formato de solicitação a seguir.

Método      | URI da solicitação
:------     | :-----
POST | /api/appx/packagemanager/package

**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
package   | (**necessário**) O nome do arquivo do pacote a ser instalado.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### Obter o status de instalação do aplicativo

**Solicitação**

Você pode obter o status de uma instalação de aplicativo que esteja em andamento usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/appx/packagemanager/state

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### Desinstalar um aplicativo

**Solicitação**

Você pode desinstalar um aplicativo usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
DELETE | /api/appx/packagemanager/package


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
### Obter aplicativos instalados

**Solicitação**

Você pode obter uma lista de aplicativos instalados no sistema usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/appx/packagemanager/packages


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

A resposta inclui uma lista de pacotes instalados com os detalhes associados.

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
## Gerenciador de dispositivos
---
### Obter os dispositivos instalados no computador

**Solicitação**

Você pode obter uma lista de dispositivos que estão instalados no computador usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/devicemanager/devices


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- A resposta inclui uma estrutura JSON que contém uma árvore de dispositivos hierárquica.

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* IoT

---
## Coleta de despejo
---
### Obter a lista de todos os despejos de memória para aplicativos

**Solicitação**

Você pode obter a lista de todos os despejos de memória disponíveis para todos os aplicativos de sideload usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/debug/dump/usermode/dumps


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- A resposta inclui uma lista de despejos de memória para cada aplicativo de sideload.

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT

---
### Obter configurações da coleta de despejo de memória para um aplicativo

**Solicitação**

Você pode obter as configurações da coleta de despejo de memória para um aplicativo de sideload usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/debug/dump/usermode/crashcontrol


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
packageFullname   | (**necessário**) O nome completo do pacote para o aplicativo de sideload.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT

---
### Excluir um despejo de memória para um aplicativo de sideload

**Solicitação**

Você pode excluir um despejo de memória de um aplicativo de sideload usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
DELETE | /api/debug/dump/usermode/crashdump


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
packageFullname   | (**necessário**) O nome completo do pacote para o aplicativo de sideload.
fileName   | (**necessário**) O nome do arquivo de despejo que deve ser excluído.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT

---
### Desabilitar despejos de memória para um aplicativo de sideload

**Solicitação**

Você pode desabilitar despejos de memória para um aplicativo de sideload usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
DELETE | /api/debug/dump/usermode/crashcontrol


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
packageFullname   | (**necessário**) O nome completo do pacote para o aplicativo de sideload.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT

---
### Baixar o despejo de memória para um aplicativo de sideload

**Solicitação**

Você pode baixar um despejo de memória de um aplicativo de sideload usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/debug/dump/usermode/crashdump


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
packageFullname   | (**necessário**) O nome completo do pacote para o aplicativo de sideload.
fileName   | (**necessário**) O nome do arquivo de despejo que você deseja baixar.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- A resposta inclui um arquivo de despejo. Você pode usar o WinDbg ou o Visual Studio para examinar o arquivo de despejo.

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT

---
### Habilitar despejos de memória para um aplicativo de sideload

**Solicitação**

Você pode habilitar despejos de memória para um aplicativo de sideload usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/debug/dump/usermode/crashcontrol


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
packageFullname   | (**necessário**) O nome completo do pacote para o aplicativo de sideload.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT

---
### Obter a lista de arquivos de verificação de erro

**Solicitação**

Você pode obter a lista de arquivos de minidespejo de verificação de erro usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/debug/dump/kernel/dumplist


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- A resposta inclui uma lista de nomes de arquivos de despejo e os tamanhos desses arquivos.

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
### Baixar um arquivo de despejo de verificação de erro

**Solicitação**

Você pode baixar um arquivo de despejo de verificação de erro usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/debug/dump/kernel/dump


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
filename   | (**necessário**) O nome do arquivo de despejo de memória. Você pode descobrir isso usando a API para obter a lista de despejo.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- A resposta inclui o arquivo de despejo. Você pode inspecionar esse arquivo usando o WinDbg.

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
### Obter as configurações de controle de falhas de verificação de erro

**Solicitação**

Você pode obter essas configurações de controle de falhas de verificação de erro usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/debug/dump/kernel/crashcontrol


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- A resposta inclui as configurações de controle de falhas. Para saber mais sobre CrashControl, consulte o artigo [CrashControl](https://technet.microsoft.com/library/cc951703.aspx).

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
### Obter um despejo de kernel dinâmico

**Solicitação**

Você pode obter um despejo de kernel dinâmico usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/debug/dump/livekernel


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- A resposta inclui o despejo no modo kernel completo. Você pode inspecionar esse arquivo usando o WinDbg.

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
### Obter um despejo de um processo de usuário dinâmico

**Solicitação**

Você pode obter o despejo para o processo de usuário dinâmico usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/debug/dump/usermode/live


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
pid   | (**necessário**) A ID de processo exclusiva do processo no qual você está interessado.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- A resposta inclui o despejo do processo. Você pode inspecionar esse arquivo usando o WinDbg ou o Visual Studio.

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
### Definir as configurações de controle de falhas de verificação de erro

**Solicitação**

Você pode definir as configurações para coletar dados de verificação de erro usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/debug/dump/kernel/crashcontrol


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
autoreboot   | (**opcional**) True ou false. Isso indica se o sistema reinicia automaticamente após alguma falha ou bloqueio.
dumptype   | (**opcional**) O tipo de despejo. Para os valores aceitos, consulte [Enumeração de CrashDumpType](https://msdn.microsoft.com/library/azure/microsoft.azure.management.insights.models.crashdumptype.aspx).
maxdumpcount   | (**opcional**) O número máximo de despejos para salvar.
overwrite   | (**opcional**) True ou false. Isso indica a substituição ou não de despejos antigos quando o limite do contador de despejos especificado pelo *maxdumpcount* foi atingido.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
## ETW
---
### Criar uma sessão ETW em tempo real por um Websocket

**Solicitação**

Você pode criar uma sessão ETW em tempo real usando o seguinte formato de solicitação: Isso será gerenciado por um Websocket.
 
Método      | URI da solicitação
:------     | :-----
GET/WebSocket | /api/etw/session/realtime


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- A resposta inclui os eventos ETW de provedores habilitados.

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### Enumerar os provedores ETW registrados

**Solicitação**

Você pode enumerar os provedores registrados usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/etw/providers


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- A resposta inclui a lista de provedores ETW. A lista incluirá o nome amigável e o GUID para cada provedor.

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

---
## Rede
---
### Obter a configuração de IP atual

**Solicitação**

Você pode obter a configuração de IP atual usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/networking/ipconfig


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- A resposta inclui a configuração de IP

**Código de status**

A tabela a seguir mostra possíveis códigos de status adicionais que podem ser retornados como resultado dessa operação.

Código de status HTTP      | Descrição
:------     | :-----
200 | A operação foi concluída com êxito
500 | Ocorreu um erro interno de servidor

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
## Informações do sistema operacional
---
### Obter o nome do computador

**Solicitação**

Você pode obter o nome de um computador usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/os/machinename


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
### Obter as informações do sistema operacional

**Solicitação**

Você pode obter as informações do sistema operacional de um computador usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/os/info


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
### Definir o nome do computador

**Solicitação**

Você pode definir o nome de um computador usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/os/machinename


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
name | (**necessário**) O novo nome para o computador.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
## Dados de desempenho
---
### Obter a lista de processos em execução

**Solicitação**

Você pode obter a lista de processos atualmente em execução usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/resourcemanager/processes


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- A resposta inclui uma lista de processos com detalhes para cada um deles. As informações estão no formato JSON.

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### Obter as estatísticas de desempenho do sistema

**Solicitação**

Você pode obter as estatísticas de desempenho do sistema usando o seguinte formato de solicitação: Isso inclui informações como ciclos de leitura e gravação e a quantidade de memória que foi usada.
 
Método      | URI da solicitação
:------     | :-----
GET | /api/resourcemanager/systemperf


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- A resposta inclui as estatísticas de desempenho do sistema como CPU e uso de GPU, acesso à memória e acesso à rede. Essas informações estão no formato JSON.

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
## Energia
---
### Obter o estado atual da bateria

**Solicitação**

Você pode obter o estado atual da bateria usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/power/battery


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT

---
### Obter o esquema de energia ativo

**Solicitação**

Você pode obter o esquema de energia ativo usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/power/activecfg


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
### Obter o subvalor para um esquema de energia

**Solicitação**

Você pode obter o subvalor para um esquema de energia usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/power/cfg/*<power scheme path>*


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
### Obter o estado de energia do sistema

**Solicitação**

Você pode verificar o estado de energia do sistema usando o seguinte formato de solicitação: Isso permitirá que você verifique se ele está em um estado de baixo consumo de energia.
 
Método      | URI da solicitação
:------     | :-----
GET | /api/power/state


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT

---
### Obter um relatório de estudo de suspensão

**Solicitação**

Você pode obter um relatório de estudo de suspensão usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/power/sleepstudy/reports


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
FileName | (**necessário**) O nome do arquivo do relatório de estudo de suspensão que você deseja baixar.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
### Definir o esquema de energia ativo

**Solicitação**

Você pode definir o esquema de energia ativo usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/power/activecfg


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
scheme | (**necessário**) O GUID do esquema que você deseja definir como o esquema de energia ativo para o sistema.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
### Definir o subvalor para um esquema de energia

**Solicitação**

Você pode definir o subvalor para um esquema de energia usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/power/cfg/*<power scheme path>*


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
valueAC | (**necessário**) O valor a ser usado para energia CA.
valueDC | (**necessário**) O valor a ser usado para energia da bateria.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
### Enumerar os relatórios de estudo de suspensão disponíveis

**Solicitação**

Você pode enumerar os relatórios de estudo de suspensão disponíveis usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/power/sleepstudy/reports


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
### Obter a transformação de estudo de suspensão

**Solicitação**

Você pode obter o relatório de estudo de suspensão usando o seguinte formato de solicitação: Essa transformação é um XSLT que converte o relatório de estudo de suspensão em um formato XML que possa ser lido por uma pessoa.
 
Método      | URI da solicitação
:------     | :-----
GET | /api/power/sleepstudy/reports


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* IoT

---
## Controle remoto
---
### Reiniciar o computador de destino

**Solicitação**

Você pode reiniciar o computador de destino usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/control/restart


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
### Desligar o computador de destino

**Solicitação**

Você pode desligar o computador de destino usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/control/shutdown


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
## Gerenciador de tarefas
---
### Iniciar um aplicativo moderno

**Solicitação**

Você pode iniciar um aplicativo moderno usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/taskmanager/app


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
appid   | (**necessário**) O PRAID do aplicativo que você deseja iniciar. Esse valor deve ser codificado em hex64.
package   | (**necessário**) O nome completo do pacote do aplicativo que você deseja iniciar. Esse valor deve ser codificado em hex64.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
### Parar um aplicativo moderno

**Solicitação**

Você pode parar um aplicativo moderno usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
DELETE | /api/taskmanager/app


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
package   | (**necessário**) O nome completo dos pacotes do aplicativo que você deseja parar. Esse valor deve ser codificado em hex64.
forcestop   | (**opcional**) Um valor **yes** indica que o sistema deve forçar todos os processos a parar.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
## WiFi
---
### Enumerar as interfaces de rede sem fio

**Solicitação**

Você pode enumerar as interfaces de rede sem fio disponíveis usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/wifi/interfaces


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Uma lista das interfaces sem fio disponíveis com detalhes. Os detalhes incluirão itens como GUID, descrição, nome amigável e muito mais.

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
### Enumerar as redes sem fio

**Solicitação**

Você pode enumerar a lista de redes sem fio na interface especificada usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/wifi/networks


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
interface   | (**necessário**) O GUID para a interface de rede usar para procurar redes sem fio.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- A lista de redes sem fio encontradas na *interface* fornecida. Isso inclui detalhes para as redes.

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
### Conectar-se a uma rede Wi-Fi e desconectar-se dela

**Solicitação**

Você pode se conectar a uma rede Wi-Fi ou se desconectar dela usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/wifi/network


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
interface   | (**necessário**) O GUID da interface de rede que você usa para se conectar à rede.
op   | (**necessário**) Indica a ação a ser executada. Valores possíveis são connect ou disconnect.
ssid   | (**necessário se *op* = = connect**) O SSID ao qual se conectar.
key   | (**necessário se *op* == connect**) A chave compartilhada.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
### Excluir um perfil de Wi-Fi

**Solicitação**

Você pode excluir um perfil associado a uma rede em uma interface específica usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
DELETE | /api/wifi/network


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
interface   | (**necessário**) O GUID da interface de rede associada ao perfil a ser excluído.
profile   | (**necessário**) O nome do perfil a ser excluído.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* Xbox
* HoloLens
* IoT

---
## Relatório de Erros do Windows (WER)
---
### Baixar um arquivo de relatório de erros do Windows (WER)

**Solicitação**

Você pode baixar um arquivo WER usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/wer/reports/file


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
user   | (**necessário**) O nome de usuário associado ao relatório.
type   | (**necessário**) O tipo de relatório. Pode ser **queried** ou **archived**.
name   | (**necessário**) O nome do relatório.
file   | (**necessário**) O nome do arquivo a ser baixado do relatório.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT

---
### Enumerar arquivos em um relatório de erros do Windows (WER)

**Solicitação**

Você pode enumerar os arquivos em um relatório WER usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/wer/reports/files


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
user   | (**necessário**) O usuário associado ao relatório.
type   | (**necessário**) O tipo de relatório. Pode ser **queried** ou **archived**.
name   | (**necessário**) O nome do relatório.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT

---
### Listar os relatórios WER (Relatório de Erros do Windows)

**Solicitação**

Você pode obter os relatórios WER usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/wer/reports


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Área de Trabalho do Windows
* HoloLens
* IoT

---
## Windows Performance Recorder (WPR) 
---
### Inicie o rastreamento com um perfil personalizado

**Solicitação**

Você pode carregar um perfil WPR e iniciar o rastreamento com esse perfil usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/wpr/customtrace


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Um corpo http correspondente a várias partes, que contém o perfil WPR personalizado.

**Resposta**

- Retorna o status da sessão WPR.

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### Iniciar uma sessão de rastreamento de desempenho de inicialização

**Solicitação**

Você pode iniciar uma sessão de rastreamento de WPR de inicialização usando o seguinte formato de solicitação: Isso também é conhecido como uma sessão de rastreamento de desempenho.
 
Método      | URI da solicitação
:------     | :-----
POST | /api/wpr/boottrace


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
profile   | (**necessário**) Esse parâmetro é necessário no início. O nome do perfil que deve iniciar uma sessão de rastreamento de desempenho. Os perfis possíveis estão armazenados em perfprofiles/profiles.json.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- No início, essa API retorna o status da sessão WPR.

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### Parar uma sessão de rastreamento de desempenho de inicialização

**Solicitação**

Você pode parar uma sessão WPR de rastreamento de inicialização usando o seguinte formato de solicitação: Isso também é conhecido como uma sessão de rastreamento de desempenho.
 
Método      | URI da solicitação
:------     | :-----
GET | /api/wpr/boottrace


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Retorna o arquivo de rastreamento ETL.

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### Iniciar uma sessão de rastreamento de desempenho

**Solicitação**

Você pode iniciar uma sessão WPR de rastreamento usando o seguinte formato de solicitação: Isso também é conhecido como uma sessão de rastreamento de desempenho.
 
Método      | URI da solicitação
:------     | :-----
POST | /api/wpr/trace


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
profile   | (**necessário**) O nome do perfil que deve iniciar uma sessão de rastreamento de desempenho. Os perfis possíveis estão armazenados em perfprofiles/profiles.json.

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- No início, essa API retorna o status da sessão WPR.

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### Parar uma sessão de rastreamento de desempenho

**Solicitação**

Você pode parar uma sessão WPR de rastreamento usando o seguinte formato de solicitação: Isso também é conhecido como uma sessão de rastreamento de desempenho.
 
Método      | URI da solicitação
:------     | :-----
GET | /api/wpr/trace


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Retorna o arquivo de rastreamento ETL.

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT

---
### Recuperar o status de uma sessão de rastreamento

**Solicitação**

Você pode recuperar o status da sessão WPR atual usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/wpr/status


**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- O status da sessão WPR de rastreamento.

**Código de status**

- Códigos de status padrão.

**Famílias de dispositivos disponíveis**

* Windows Mobile
* Área de Trabalho do Windows
* HoloLens
* IoT


<!--HONumber=Mar16_HO5-->


