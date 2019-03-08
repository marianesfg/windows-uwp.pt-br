---
title: Ler um blob binário
description: Saiba mais sobre como ler um blob binário no armazenamento de título do Xbox Live.
ms.assetid: 9b8e0c35-0cea-4491-bf30-22fad224f11b
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, armazenamento de título
ms.localizationpriority: medium
ms.openlocfilehash: e2a0be72142a3e11c7c680cfc998287f396c2afc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641651"
---
# <a name="reading-a-binary-blob-in-xbox-live-title-storage"></a>Ler um blob binário no armazenamento de título do Xbox Live

1.  Enviar uma solicitação usando o *obter* método para ler os dados do armazenamento do título. Este exemplo usa o armazenamento de título global.

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/userinfo.bin,binary
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive



-   O usuário deve ser na sessão para atualizá-lo.

-   STSTokenString é um espaço reservado para fins de brevidade e deve ser substituído com o token retornado pela solicitação de autenticação.

#### <a name="reference"></a>Referência

**/global/scids/{scid}/data/{pathAndFileName},{type}**
