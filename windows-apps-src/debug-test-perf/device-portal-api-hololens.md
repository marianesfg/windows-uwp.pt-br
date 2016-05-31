---
author: mcleblanc
ms.assetid: 41ac0142-4d86-4bb3-b580-36d0d6956091
title: Referência de API do Device Portal para HoloLens
description: Saiba mais sobre as APIs REST do Windows Device Portal para Hololens que você pode usar para acessar os dados e controlar seu dispositivo de forma programática.
---
# Referência de API do Device Portal para HoloLens

Tudo no Windows Device Portal foi criado com base em APIs REST, que você pode usar para acessar os dados e controlar o dispositivo de forma programática.

## Sistema operacional holográfico
---
### Obter requisitos de HTTPS para o Device Portal

**Solicitação**

Você pode obter os requisitos de HTTPS para o Device Portal usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/os/webmanagement/settings/https


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

---
### Obter a DIP (distância interpupilar) armazenada

**Solicitação**

Você pode obter o valor armazenado da DIP usando o seguinte formato de solicitação: O valor é retornado em milímetros.
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/os/settings/ipd


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

---
### Obter uma lista de provedores ETW específicos do HoloLens

**Solicitação**

Você pode obter uma lista de provedores ETW específicos do HoloLens que não são registrados no sistema usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/os/etw/customproviders


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

---
### Retornar o estado para todos os serviços de ativos

**Solicitação**

Você pode obter o estado de todos os serviços em execução no momento usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/os/services


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

---
### Defina o requisito de HTTPS para o Device Portal.

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

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

---
### Definir a DIP (distância interpupilar)

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

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

---
## Percepção holográfica
---
### Aceitar atualizações Websocket e executar um cliente do Mirage que envia atualizações

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

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

---
## Térmico holográfico
---
### Obter o estágio térmico do dispositivo

**Solicitação**

Você pode obter o estágio térmico do dispositivo usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/

**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

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
## Controle de HSimulation
---
### Criar um fluxo de controle ou postar dados em um fluxo criado

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

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

---
### Excluir um fluxo de controle

**Solicitação**

Você pode excluir um fluxo de controle usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
DELETE | /api/holographic/simulation/control/stream


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

---
### Obter um fluxo de controle

**Solicitação**

Você pode abrir uma conexão de soquete da Web para um fluxo de controle usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET/WebSocket | /api/holographic/simulation/control/stream


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

---
### Obter o modo de simulação

**Solicitação**

Você pode obter o modo de simulação usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/simulation/control/mode


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

---
### Definir o modo de simulação

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

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

---
## Reprodução de HSimulation
---
### Excluir uma gravação

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

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

---
### Obter todas as gravações

**Solicitação**

Você pode obter todas as gravações disponíveis usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/simulation/playback/files


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

---
### Obter os tipos de dados em uma gravação carregada

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

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

---
### Obter todas as gravações carregadas

**Solicitação**

Você pode obter todas as gravações carregadas disponíveis usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/simulation/playback/session/files


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

---
### Obter o estado de reprodução atual de uma gravação 

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

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

---
### Carregar uma gravação

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

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

---
### Pausar uma gravação

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

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

---
### Reproduzir uma gravação

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

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

---
### Parar a gravação

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

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

---
### Descarregar uma gravação

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

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

---
### Carregar uma gravação

**Solicitação**

Você pode carregar uma gravação usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/holographic/simulation/playback/file


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

---
## Gravação de HSimulation
---
### Obter o estado de gravação

**Solicitação**

Você pode obter a estado de gravação atual usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/simulation/recording/status


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

---
### Iniciar uma gravação

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

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

---
### Parar a gravação atual

**Solicitação**

Você pode parar a gravação atual usando o seguinte formato de solicitação: A gravação será retornada como um arquivo.
 
Método      | URI da solicitação
:------     | :-----
POST | /api/holographic/simulation/recording/stop


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

---
## Captura de realidade mista
---
### Excluir uma gravação MRC (captura de realidade mista) do dispositivo

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

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

---
### Baixar um arquivo de MRC (captura de realidade mista)

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

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhum

**Código de status**

- Códigos de status padrão.

---
### Obter as configurações de MRC (captura de realidade mista)

**Solicitação**

Você pode obter as configurações de MRC usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/mrc/settings


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

---
### Obter o status da gravação de MRC (captura de realidade mista)

**Solicitação**

Você pode obter o status da gravação de MRC usando o seguinte formato de solicitação: Os valores possíveis incluem **running** e **stopped**.
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/mrc/status


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

---
### Obter a lista de arquivos de MRC (captura de realidade mista)

**Solicitação**

Você pode obter os arquivos de MRC armazenados no dispositivo usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/mrc/files


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

---
### Definir as configurações de MRC (captura de realidade mista)

**Solicitação**

Você pode definir as configurações de MRC usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/holographic/mrc/settings


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

---
### Iniciar uma gravação de MRC (captura de realidade mista)

**Solicitação**

Você pode iniciar uma gravação de MRC usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/holographic/mrc/video/control/start


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

---
### Parar a atual gravação de MRC (captura de realidade mista)

**Solicitação**

Você pode parar a atual gravação de MRC usando o seguinte formato de solicitação:
 
Método      | URI da solicitação
:------     | :-----
POST | /api/holographic/mrc/video/control/stop


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

---
### Tirar uma foto de MRC (captura de realidade mista)

**Solicitação**

Você pode tirar uma foto de MRC usando o seguinte formato de solicitação: A foto é retornada no formato JPEG.
 
Método      | URI da solicitação
:------     | :-----
GET | /api/holographic/mrc/photo


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

---
## Streaming de realidade mista
---
### Inicia um download em partes de um mp4 fragmentado

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

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

---
### Inicia um download em partes de um mp4 fragmentado

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

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

---
### Inicia um download em partes de um mp4 fragmentado

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

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.

---
### Inicia um download em partes de um mp4 fragmentado

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

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**

- Nenhuma

**Código de status**

- Códigos de status padrão.


<!--HONumber=May16_HO2-->


