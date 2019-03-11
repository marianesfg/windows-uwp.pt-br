---
title: Ler um blob de configuração
description: Saiba como ler um blob de configuração no armazenamento de título do Xbox Live.
ms.assetid: ee62d221-69b9-4f52-9b5d-5a44d04de548
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 71607f05f07ee4a98f73a19c28c85dc325041a10
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599391"
---
# <a name="reading-a-configuration-blob-in-xbox-live-title-storage"></a>Ler um blob de configuração no armazenamento de título do Xbox Live

*Blobs de configuração* são arquivos que contêm dados de jogos no armazenamento de título Xbox Live. Os dados são objetos JSON que incluem os nós virtuais que podem ser filtrados antes de serem entregues ao jogo. Para obter mais informações sobre blobs de configuração, consulte **URIs de armazenamento do título**.

### <a name="to-read-a-configuration-blob"></a>Ler um blob de configuração

1.  Enviar uma solicitação usando o abaixo do método para ler os dados do armazenamento do título.

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/config.json,config              
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive


-   O usuário deve ser na sessão para atualizá-lo.
-   STSTokenString é um espaço reservado para fins de brevidade e deve ser substituído com o token retornado pela solicitação de autenticação.

#### <a name="reference"></a>Referência

**/global/scids/{scid}/data/{pathAndFileName},{type}**
**GET (/global/scids/{scid}/data/{pathAndFileName},{type})**
