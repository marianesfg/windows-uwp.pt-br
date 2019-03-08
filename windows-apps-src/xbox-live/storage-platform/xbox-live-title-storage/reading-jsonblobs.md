---
title: Ler um blob JSON
description: Saiba como ler um blob JSON no armazenamento de título do Xbox Live.
ms.assetid: 3697af16-d054-4835-af7f-7fee8c628345
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 30d2b9f6539e73df1c5ea5ae18b3d1a6ca061d60
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613481"
---
# <a name="reading-a-json-blob-in-xbox-live-title-storage"></a>Ler um blob JSON no armazenamento de título do Xbox Live

1.  Enviar uma solicitação usando o *obter* método para ler os dados do armazenamento do título. Este exemplo usa o armazenamento de título global.

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/surprise.json,json
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive

-   O usuário deve ser na sessão para atualizá-lo.

-   STSTokenString é um espaço reservado para fins de brevidade e deve ser substituído com o token retornado pela solicitação de autenticação.

#### <a name="reference"></a>Referência

**/global/scids/{scid}/data/{pathAndFileName},{type}**
