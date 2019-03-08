---
title: Armazenando um blob JSON
description: Saiba como armazenar um blob JSON no armazenamento de título do Xbox Live.
ms.assetid: f424aca1-e671-4e31-84c6-098fda4a9558
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, armazenamento de título
ms.localizationpriority: medium
ms.openlocfilehash: dabcd0634bb20bc6b82ae302c77338b5bb726a63
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595061"
---
# <a name="storing-a-json-blob-in-xbox-live-title-storage"></a>Armazenando um blob JSON no armazenamento de título do Xbox Live

1.  Enviar uma solicitação usando o *colocar* método para enviar os dados para o armazenamento do título.

        PUT https://titlestorage.xboxlive.com/json/users/xuid(1245111)/scids/{scid}/data/{pathAndFileName},json
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Content-Length: 240
        Connection: Keep-Alive



-   O usuário deve ser na sessão para atualizá-lo.

-   STSTokenString é um espaço reservado para fins de brevidade e deve ser substituído com o token retornado pela solicitação de autenticação.

2.  Envie um objeto JSON.

        {
            "startlevel":"1",
            "expression":"smile"
        }

#### <a name="reference"></a>Referência

**/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json)**
