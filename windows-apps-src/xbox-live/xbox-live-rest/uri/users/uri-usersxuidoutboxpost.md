---
title: POST (/users/xuid({xuid})/outbox)
assetID: de991d88-efe0-04f2-f6b2-0bc3e68bfd46
permalink: en-us/docs/xboxlive/rest/uri-usersxuidoutboxpost.html
description: " POST (/users/xuid({xuid})/outbox)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2b225e8441fee3d499f172e2e5701096cdbc161a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594281"
---
# <a name="post-usersxuidxuidoutbox"></a>POST (/users/xuid({xuid})/outbox)
Envia uma mensagem especificada para uma lista de destinatários.
O domínio para esses URIs é `msg.xboxlive.com`.

  * [Comentários](#ID4EV)
  * [Parâmetros de URI](#ID4EAB)
  * [Autorização](#ID4ENB)
  * [Efeito das configurações de privacidade no recurso](#ID4EYB)
  * [Corpo da solicitação](#ID4E3F)
  * [Códigos de status HTTP](#ID4ETCAC)
  * [Corpo da resposta](#ID4E1EAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Comentários

O tipo de conteúdo só dá suporte a essa API é "application/json", que é necessário nos cabeçalhos HTTP de cada chamada.

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- |
| xuid | inteiro sem sinal de 64 bits | O Xbox usuário ID (XUID) do player que está fazendo a solicitação. |

<a id="ID4ENB"></a>


## <a name="authorization"></a>Autorização

Você deve ter sua própria declaração de usuário e uma assinatura válida do gold para enviar uma mensagem do usuário.

<a id="ID4EYB"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>Efeito das configurações de privacidade no recurso

Enviar com êxito uma mensagem do usuário para um player, se esse jogador é um amigo ou não, resulta em um código de resultado de 200. No entanto, se você enviar uma mensagem para alguém que bloqueou a, o destinatário não receberá a mensagem, e você não receberá nenhuma indicação de que sua mensagem não foi bem-sucedida.

Também há limites no quantas mensagens podem ser enviadas por dia e para o número de amigos e não-amigos, da seguinte maneira.

   * 20 estranhos por mensagem
   * 200 estranhos por 24 horas
   * total de 250 mensagens por 24 horas
   * total de 2500 destinatários por 24 horas

| Usuário solicitante| Configuração de privacidade do usuário de destino| Comportamento|
| --- | --- | --- | --- | --- | --- |
| Me| -| Conforme descrito.|
| Friend| Todas as pessoas| 200 OK|
| Friend| somente os amigos| 200 OK|
| Friend| bloqueado| 200 OK|
| usuário não friend| Todas as pessoas| 200 OK|
| usuário não friend| somente os amigos| 200 OK|
| usuário não friend| bloqueado| 200 OK|
| site de terceiros| Todas as pessoas| 200 OK|
| site de terceiros| somente os amigos| 200 OK|
| site de terceiros| bloqueado| 200 OK|

<a id="ID4E3F"></a>


## <a name="request-body"></a>Corpo da solicitação

| Propriedade| Tipo| Comprimento máximo| Consumidores| Comentários|
| --- | --- | --- | --- | --- |
| header| Cabeçalho|  | Todas| Cabeçalho de mensagem do usuário|
| messageText| cadeia de caracteres| 250| Todas as plataformas, exceto o Windows 8| Texto da mensagem de usuário (UTF-8)|

#### <a name="header"></a>Cabeçalho

| Propriedade| Tipo| Comprimento máximo| Consumidores| Comentários|
| --- | --- | --- | --- | --- |
| destinatários| User[]| 20| Todas| Lista de destinatários da mensagem|

#### <a name="user"></a>Usuário

| Propriedade| Tipo| Comprimento máximo| Consumidores| Comentários|
| --- | --- | --- | --- | --- |
| xuid| ULong|  | Todas| XUID do destinatário. Não usado se o nome de jogador é enviado.|
| gamertag| cadeia de caracteres| 15| Todas| Gamertag do destinatário. Não é usado se XUID for enviada.|

#### <a name="sample-request-body"></a>Exemplo de corpo de solicitação 

```cpp
{
          "header":
          {
            "recipients":
            [{"gamertag":"GoTeamEmily"},
            {"gamertag":"Longstreet360"}]
          },
          "messageText":"random user text"
        }

```


<a id="ID4ETCAC"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP

O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).

| Código| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| Êxito.|
| 400| Lista de destinatários está vazia ou excede o comprimento máximo; ou gamertag e XUID foram especificados; ou messageText é muito longo.|
| 403| XUID não pode ser convertido.|
| 404| Nome de jogador é inválido ou o usuário não pode ser encontrado.|
| 409| Usuário atingiu o limite diário imposto pelo sistema.|
| 500| Erro geral do lado do servidor.|

<a id="ID4E1EAC"></a>


## <a name="response-body"></a>Corpo da resposta

Não há objetos são enviados no corpo da resposta.

<a id="ID4EJFAC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4ELFAC"></a>


##### <a name="parent"></a>Parent  

[/users/xuid({xuid})/outbox](uri-usersxuidoutbox.md)


<a id="ID4EZFAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>Referência [códigos de status HTTP padrão](../../additional/httpstatuscodes.md)
