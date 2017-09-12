---
author: PatrickFarley
ms.assetid: 41ac0142-4d86-4bb3-b580-36d0d6956091
title: "Referência de API do Device Portal para HoloLens"
description: "Saiba mais sobre as APIs REST do Windows Device Portal para Hololens que você pode usar para acessar os dados e controlar seu dispositivo de forma programática."
ms.openlocfilehash: 3c000bc19c0bd45050e5be1ca73e5dc7b73d8103
ms.sourcegitcommit: e8cc657d85566768a6efb7cd972ebf64c25e0628
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2017
---
# <a name="device-portal-api-reference-for-hololens"></a>Referência de API do Device Portal para HoloLens

Tudo no Windows Device Portal foi criado com base em APIs REST, que você pode usar para acessar os dados e controlar o dispositivo de forma programática.

## <a name="holographic-os"></a>Sistema operacional holográfico
---
### <a name="get-https-requirements-for-the-device-portal"></a>Obter requisitos de HTTPS para o Device Portal

**Solicitação**

Você pode obter os requisitos de HTTPS para o Device Portal usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/os/webmanagement/settings/https


**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="get-the-stored-interpupillary-distance-ipd"></a>Obter a DIP (distância interpupilar) armazenada

**Solicitação**

Você pode obter o valor armazenado da DIP usando o seguinte formato de solicitação: O valor é retornado em milímetros.
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/os/settings/ipd


**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="get-a-list-of-hololens-specific-etw-providers"></a>Obter uma lista de provedores ETW específicos do HoloLens

**Solicitação**

Você pode obter uma lista de provedores ETW específicos do HoloLens que não são registrados no sistema usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/os/etw/customproviders


**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="return-the-state-for-all-active-services"></a>Retornar o estado para todos os serviços de ativos

**Solicitação**

Você pode obter o estado de todos os serviços em execução no momento usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/os/services


**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="set-the-https-requirement-for-the-device-portal"></a>Defina o requisito de HTTPS para o Device Portal.

**Solicitação**

Você pode definir os requisitos de HTTPS para o Device Portal usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/holographic/management/settings/https


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
required   | (**necessário**) Determina se o HTTPS é necessário ou não para o Device Portal. Os valores possíveis são **yes**, **no** e **default**.

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="set-the-interpupillary-distance-ipd"></a>Definir a DIP (distância interpupilar)

**Solicitação**

Você pode definir a DIP armazenada usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/holographic/os/settings/ipd


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
ipd   | (**necessário**) O novo valor da DIP a ser armazenado. Esse valor deve ser em milímetros.

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
## Holographic perception
---
### <a name="accept-websocket-upgrades-and-run-a-mirage-client-that-sends-updates"></a>Aceitar atualizações Websocket e executar um cliente do Mirage que envia atualizações

**Solicitação**

Você pode aceitar atualizações Websocket e executar um cliente do Mirage que envia atualizações em 30 fps usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET/WebSocket | /api/holographic/perception/client


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
clientmode   | (**necessário**) Determina o modo de rastreamento. Um valor **active** força o modo de rastreamento visual quando ele não pode ser estabelecido passivamente.

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
## Holographic thermal
---
### <a name="get-the-thermal-stage-of-the-device"></a>Obter o estágio térmico do dispositivo

**Solicitação**

Você pode obter o estágio térmico do dispositivo usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/

**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

Os valores possíveis são indicados pela tabela a seguir.

Valor | Descrição
--- | ---
1 | Normal
2 | Quente
3 | Crítico

**Código de status**

- Códigos de status padrão.

---
## HSimulation control
---
### <a name="create-a-control-stream-or-post-data-to-a-created-stream"></a>Criar um fluxo de controle ou postar dados em um fluxo criado

**Solicitação**

Você pode criar um fluxo de controle ou postar dados em um fluxo criado usando o seguinte formato de solicitação: Espera-se que os dados postados sejam do tipo **application/octet-stream**.
 
Método      | URI da solicitação
:------     | :-----
POST | /api/holographic/simulation/control/stream


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
priority   | (**necessário na criação de um fluxo de controle**) Indica a prioridade do fluxo.
streamid   | (**necessário na postagem em um fluxo criado**) O identificador do fluxo no qual postar.

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="delete-a-control-stream"></a>Excluir um fluxo de controle

**Solicitação**

Você pode excluir um fluxo de controle usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
DELETE | /api/holographic/simulation/control/stream


**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="get-a-control-stream"></a>Obter um fluxo de controle

**Solicitação**

Você pode abrir uma conexão de soquete da Web para um fluxo de controle usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET/WebSocket | /api/holographic/simulation/control/stream


**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="get-the-simluation-mode"></a>Obter o modo de simulação

**Solicitação**

Você pode obter o modo de simulação usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/simulation/control/mode


**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="set-the-simluation-mode"></a>Definir o modo de simulação

**Solicitação**

Você pode definir o modo de simulação usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/holographic/simluation/control/mode


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
mode   | (**necessário**) Indica o modo de simulação. Os valores possíveis incluem **default**, **simulation**, **remote** e **legacy**.

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
## HSimulation playback
---
### <a name="delete-a-recording"></a>Excluir uma gravação

**Solicitação**

Você pode excluir uma gravação usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
DELETE | /api/holographic/simulation/playback/file


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
recording   | (**necessário**) O nome da gravação a ser excluída.

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="get-all-recordings"></a>Obter todas as gravações

**Solicitação**

Você pode obter todas as gravações disponíveis usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/simulation/playback/files


**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="get-the-types-of-data-in-a-loaded-recording"></a>Obter os tipos de dados em uma gravação carregada

**Solicitação**

Você pode obter os tipos de dados em uma gravação carregada usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/simulation/playback/session/types


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
recording   | (**necessário**) O nome da gravação pela qual você se interessa.

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="get-all-the-loaded-recordings"></a>Obter todas as gravações carregadas

**Solicitação**

Você pode obter todas as gravações carregadas disponíveis usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/simulation/playback/session/files


**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="get-the-current-playback-state-of-a-recording"></a>Obter o estado de reprodução atual de uma gravação 

**Solicitação**

Você pode obter o estado de reprodução atual de uma gravação usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/simulation/playback/session


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
recording   | (**necessário**) O nome da gravação pela qual você se interessa.

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="load-a-recording"></a>Carregar uma gravação

**Solicitação**

Você pode carregar uma gravação usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/holographic/simulation/playback/session/file


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
recording   | (**necessário**) O nome da gravação a ser carregada.

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="pause-a-recording"></a>Pausar uma gravação

**Solicitação**

Você pode pausar uma gravação usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/holographic/simulation/playback/session/pause


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
recording   | (**necessário**) O nome da gravação a ser pausada.

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="play-a-recording"></a>Reproduzir uma gravação

**Solicitação**

Você pode reproduzir uma gravação usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/holographic/simulation/playback/session/play


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
recording   | (**necessário**) O nome da gravação a ser reproduzida.

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="stop-a-recording"></a>Parar a gravação

**Solicitação**

Você pode parar uma gravação usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/holographic/simulation/playback/session/stop


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
recording   | (**necessário**) O nome da gravação a ser parada.

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="unload-a-recording"></a>Descarregar uma gravação

**Solicitação**

Você pode descarregar uma gravação usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
DELETE | /api/holographic/simulation/playback/session/file


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
recording   | (**necessário**) O nome da gravação a ser descarregada.

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="upload-a-recording"></a>Carregar uma gravação

**Solicitação**

Você pode carregar uma gravação usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/holographic/simulation/playback/file


**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
## HSimulation recording
---
### <a name="get-the-recording-state"></a>Obter o estado de gravação

**Solicitação**

Você pode obter a estado de gravação atual usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/simulation/recording/status


**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="start-a-recording"></a>Iniciar uma gravação

**Solicitação**

Você pode iniciar uma gravação usando o seguinte formato de solicitação: Só é possível haver uma gravação ativa por vez. 
 
Método      | URI da solicitação
:------     | :-----
POST | /api/holographic/simulation/recording/start


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
head   | (**veja abaixo**) Defina esse valor como 1 para indicar que o sistema deve gravar dados da cabeça.
hands   | (**veja abaixo**) Defina esse valor como 1 para indicar que o sistema deve gravar dados das mãos.
spatialMapping   | (**veja abaixo**) Defina esse valor como 1 para indicar que o sistema deve gravar dados do mapeamento espacial.
environment   | (**veja abaixo**) Defina esse valor como 1 para indicar que o sistema deve gravar dados do ambiente.
name   | (**necessário**) O nome da gravação.
singleSpatialMappingFrame   | (**opcional**) Defina esse valor como 1 para indicar que somente um único quadro de mapeamento espacial deve ser gravado.

Para esses parâmetros, um dos seguintes parâmetros deve ser definido exatamente como 1: *head*, *hands*, *spatialMapping* ou *environment*.

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="stop-the-current-recording"></a>Parar a gravação atual

**Solicitação**

Você pode parar a gravação atual usando o seguinte formato de solicitação: A gravação será retornada como um arquivo.
 
Método      | URI da solicitação
:------     | :-----
POST | /api/holographic/simulation/recording/stop


**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
## Mixed reality capture
---
### <a name="delete-a-mixed-reality-capture-mrc-recording-from-the-device"></a>Excluir uma gravação MRC (captura de realidade mista) do dispositivo

**Solicitação**

Você pode excluir uma gravação MRC usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
DELETE | /api/holographic/mrc/file


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
filename   | (**necessário**) O nome do arquivo de vídeo a ser excluído. O nome deve ser codificado em hex64.

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="download-a-mixed-reality-capture-mrc-file"></a>Baixar um arquivo de MRC (captura de realidade mista)

**Solicitação**

Você pode baixar um arquivo de MRC do dispositivo usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/mrc/file


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
filename   | (**necessário**) O nome do arquivo de vídeo que você deseja obter. O nome deve ser codificado em hex64.
op   | (**opcional**) Defina esse valor como **stream** se você deseja baixar um fluxo.

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="get-the-mixed-reality-capture-mrc-settings"></a>Obter as configurações de MRC (captura de realidade mista)

**Solicitação**

Você pode obter as configurações de MRC usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/mrc/settings


**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="get-the-status-of-the-mixed-reality-capture-mrc-recording"></a>Obter o status da gravação de MRC (captura de realidade mista)

**Solicitação**

Você pode obter o status da gravação de MRC usando o seguinte formato de solicitação: Os valores possíveis incluem **running** e **stopped**.
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/mrc/status


**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="get-the-list-of-mixed-reality-capture-mrc-files"></a>Obter a lista de arquivos de MRC (captura de realidade mista)

**Solicitação**

Você pode obter os arquivos de MRC armazenados no dispositivo usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/mrc/files


**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="set-the-mixed-reality-capture-mrc-settings"></a>Definir as configurações de MRC (captura de realidade mista)

**Solicitação**

Você pode definir as configurações de MRC usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/holographic/mrc/settings


**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="starts-a-mixed-reality-capture-mrc-recording"></a>Iniciar uma gravação de MRC (captura de realidade mista)

**Solicitação**

Você pode iniciar uma gravação de MRC usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/holographic/mrc/video/control/start


**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="stop-the-current-mixed-reality-capture-mrc-recording"></a>Parar a atual gravação de MRC (captura de realidade mista)

**Solicitação**

Você pode parar a atual gravação de MRC usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/holographic/mrc/video/control/stop


**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="take-a-mixed-reality-capture-mrc-photo"></a>Tirar uma foto de MRC (captura de realidade mista)

**Solicitação**

Você pode tirar uma foto de MRC usando o seguinte formato de solicitação: A foto é retornada no formato JPEG.
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/mrc/photo


**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
## Mixed reality streaming
---
### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>Inicia um download em partes de um mp4 fragmentado

**Solicitação**

Você pode iniciar um download em partes de um mp4 fragmentado usando o seguinte formato de solicitação: Esta API usa a qualidade padrão.
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/stream/live.mp4


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
pv   | (**opcional**) Indica se deve ser capturada a câmera PV. Deve ser **true** ou **false**.
holo   | (**opcional**) Indica se devem ser capturados hologramas. Deve ser **true** ou **false**.
mic   | (**opcional**) Indica se deve ser capturado o microfone. Deve ser **true** ou **false**.
loopback   | (**opcional**) Indica se deve ser capturado o áudio do aplicativo. Deve ser **true** ou **false**.

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>Inicia um download em partes de um mp4 fragmentado

**Solicitação**

Você pode iniciar um download em partes de um mp4 fragmentado usando o seguinte formato de solicitação: Esta API usa a alta qualidade.
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/stream/live_high.mp4


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
pv   | (**opcional**) Indica se deve ser capturada a câmera PV. Deve ser **true** ou **false**.
holo   | (**opcional**) Indica se devem ser capturados hologramas. Deve ser **true** ou **false**.
mic   | (**opcional**) Indica se deve ser capturado o microfone. Deve ser **true** ou **false**.
loopback   | (**opcional**) Indica se deve ser capturado o áudio do aplicativo. Deve ser **true** ou **false**.

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>Inicia um download em partes de um mp4 fragmentado

**Solicitação**

Você pode iniciar um download em partes de um mp4 fragmentado usando o seguinte formato de solicitação: Esta API usa a baixa qualidade.
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/stream/live_low.mp4


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
pv   | (**opcional**) Indica se deve ser capturada a câmera PV. Deve ser **true** ou **false**.
holo   | (**opcional**) Indica se devem ser capturados hologramas. Deve ser **true** ou **false**.
mic   | (**opcional**) Indica se deve ser capturado o microfone. Deve ser **true** ou **false**.
loopback   | (**opcional**) Indica se deve ser capturado o áudio do aplicativo. Deve ser **true** ou **false**.

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.

---
### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>Inicia um download em partes de um mp4 fragmentado

**Solicitação**

Você pode iniciar um download em partes de um mp4 fragmentado usando o seguinte formato de solicitação: Esta API usa a qualidade média.
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/stream/live_med.mp4


**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

Parâmetro do URI | Descrição
:---          | :---
pv   | (**opcional**) Indica se deve ser capturada a câmera PV. Deve ser **true** ou **false**.
holo   | (**opcional**) Indica se devem ser capturados hologramas. Deve ser **true** ou **false**.
mic   | (**opcional**) Indica se deve ser capturado o microfone. Deve ser **true** ou **false**.
loopback   | (**opcional**) Indica se deve ser capturado o áudio do aplicativo. Deve ser **true** ou **false**.

**Cabeçalhos de solicitação**

- Nenhum(a)

**Corpo da solicitação**

- Nenhum(a)

**Resposta**

- Nenhum(a)

**Código de status**

- Códigos de status padrão.
