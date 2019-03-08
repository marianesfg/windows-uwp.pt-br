---
title: Presença avançada Atualizando cadeias de caracteres
description: Saiba como atualizar uma cadeia de caracteres de presença de Rich do Xbox Live.
ms.assetid: eb2bb82e-8730-4d74-9b33-95d133360e44
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox, recursos avançados de presença
ms.localizationpriority: medium
ms.openlocfilehash: ac4549301c60eafb935dab0ac9c5b5028452edfb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610391"
---
# <a name="rich-presence-updating-strings"></a>Presença avançada Atualizando cadeias de caracteres

Para atualizar a cadeia de caracteres de presença avançada em seu título, você pode chamar o URI de título de gravação com os parâmetros apropriados em um objeto JSON. Essa chamada restful também é encapsulada por APIs de serviços do Xbox. Ver **Microsoft.Xbox.Services.Presence Namespace** para obter informações sobre a API relacionada.

O URI terá esta aparência:

          POST /users/xuid({xuid})/devices/current/titles/current

A seguir estão apenas os campos para a configuração de cadeias de caracteres de presença avançada. Há outros campos opcionais relacionados na presença de gravação para um título não listado aqui.

## <a name="titlerequest-object"></a>Objeto TitleRequest

Propriedade | Tipo | Req seria | Descrição
---|---|---|---
Atividade|ActivityRequest|N|Registro que descreve as informações no título (presença avançada e mídia info, se disponível)

## <a name="activityrequest-object"></a>Objeto ActivityRequest

Propriedade | Tipo | Req seria | Descrição
---|---|---|---
richPresence|RichPresenceRequest|N|O nome amigável da cadeia de caracteres presença avançada que deve ser usada.

## <a name="richpresencerequest-object"></a>Objeto RichPresenceRequest

Propriedade | Tipo | Req seria | Descrição
---|---|---|---
Id|String|Y|O nome amigável da cadeia de caracteres presença avançada que deve ser usada
scid|String|Y|Scid que nos informa onde as cadeias de caracteres de presença avançada são definidas.

Por exemplo, se eu quisesse atualizar a presença avançada para o usuário cuja xuid for 12345, minha chamada seria semelhante ao seguinte:

          POST /users/xuid(12345)/devices/current/titles/current


Com o seguinte corpo JSON:

```json
          {
            activity:
            {
              richPresence:
              {
                id:"playingMap",
                scid:"0000-0000-0000-0000-01010101"
              }
            }
          }
```

Usando o wrapper de API, isso seria uma chamada para **PresenceService.SetPresenceAsync método**

Se você está mantendo a plataforma de dados atualizados, em seguida, você não precisa redefinir a cadeia de caracteres de presença avançada sempre que os dados para preencher as alterações em branco. No exemplo acima, nós sabemos que você deseja usar o map atual. Presença procurará os dados na plataforma de dados quando um usuário tenta ler a cadeia de caracteres para preencher o valor atual. Assim, mesmo que o jogador está alternando de mapa para ao mapa do mapa, você não precisa redefinir a cadeia de caracteres de presença avançada em seu jogo, desde que você está enviando os eventos apropriados para a plataforma de dados. Tenha em mente que pode levar alguns segundos para os dados para localizar sua maneira por meio da plataforma de dados.

Em seguida, quando alguém tenta ler presença avançada de usuário do 12345, o serviço examinar quais localidade está sendo solicitada e a cadeia de caracteres de formato adequadamente antes de retornar.

Nesse caso, digamos que um usuário deseja ler a cadeia de caracteres de en-US. Ler recursos avançados de presença funciona da seguinte maneira (para obter mais informações sobre essa chamada, consulte **GET (/users/xuid({xuid}))**

          GET /users/xuid(12345)?level=all

O wrapper de API para isso é **PresenceService.GetPresenceAsync método**

O que está acontecendo aqui é que você está pedindo para o PresenceRecord do usuário, cujo xuid for 12345. E você está solicitando que o nível de detalhe seja "tudo". Se "all" não foi especificado, a presença avançada não seriam retornada.

E ele retornaria o seguinte na resposta JSON:

```json
          {
            xuid:"12345",
            state:"online",
            devices:
            [
              {
                type:"D",
                titles:
                [
                  {
                    id:"12345",
                    name:"Buckets are Awesome",
                    lastModified:"2012-09-17T07:15:23.4930000",
                    placement: "full",
                    state:"active",
                    activity:
                    {
                      richPresence:"Playing on map:Mountains"
                    }
                  }
                ]
              }
            ]
          }
```
