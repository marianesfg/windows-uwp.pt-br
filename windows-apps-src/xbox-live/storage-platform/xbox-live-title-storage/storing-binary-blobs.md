---
title: Armazenando um blob binário
description: Saiba como armazenar um blob binário no armazenamento de título do Xbox Live.
ms.assetid: a0da36ef-5a5a-466d-80a8-e055ba7e4cdc
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, armazenamento de título
ms.localizationpriority: medium
ms.openlocfilehash: 70e620f2e992dfdd240f71b5c73165f13d4ef5cb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660091"
---
# <a name="storing-a-binary-blob-in-xbox-live-title-storage"></a>Armazenando um blob binário no armazenamento de título do Xbox Live

1.  Enviar uma solicitação usando o abaixo do método para enviar os dados para o armazenamento do título.

        PUT https://titlestorage.xboxlive.com/trustedplatform/users/xuid(1245111)/scids/{scid}/data/lastturn.bin,binary              
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Content-Length: 40
        Connection: Keep-Alive


-   O usuário deve ser na sessão para atualizá-lo.

-   STSTokenString é um espaço reservado para fins de brevidade e deve ser substituído com o token retornado pela solicitação de autenticação.

2.  Envie os dados binários. Uma vez que os dados serão transferidos por meio de HTTP, os dados devem ser restrito ao conjunto de caracteres aceitável. Informações como dados de áudio ou de imagem devem ser codificadas. Você pode selecionar qualquer método de codificação que gera a caracteres compatíveis do HTTP.
d.
```
  01EAEFBAD05903A4
  1EA2311656677DFF
  CF00
```

#### <a name="reference"></a>Referência

**/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}**
