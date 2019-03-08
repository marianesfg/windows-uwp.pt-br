---
title: Enumeração GameClipState
assetID: 97fe5c1e-f7b5-537e-69eb-8284b69cd3e1
permalink: en-us/docs/xboxlive/rest/gvr-enum-gameclipstate.html
description: " Enumeração GameClipState"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f7b20224eeab1b98c7c80f0e4b551420b5a15e7d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630711"
---
# <a name="gameclipstate-enumeration"></a>Enumeração GameClipState
Fornece detalhes sobre a enumeração GameClipState. 
<a id="ID4ET"></a>

 
## <a name="gameclipstate"></a>GameClipState
 
| <b>Enumerator</b>| <b>Descrição</b>| 
| --- | --- | 
| Nenhuma | Estado do serviço de clipe de jogo é desconhecido ou não foi definida.| 
| PendingUpload | Serviço de clipe de jogo está aguardando o carregamento de ativos.| 
| PendingDelete | Clipe de jogo está na fila para exclusão. (Efetivamente significa que ele é "excluído").| 
| Processados | Clipe de jogo terminou todo o processamento.| 
| Processando| Clipe de jogo está sendo processado (codificação, miniaturas, etc.).| 
| Publicação| Ativos de jogo clip estão sendo publicados.| 
| Publicado| Ativos de clipe de jogo são publicados – este estado indica que ele está configurado para exibir.| 
| Sinalizados| Clipe de jogo foi sinalizado para imposição.| 
| Proibido| Clipe de jogo foi proibido, mas ainda não foram excluído.| 
| Uploaded| Clipe de jogo concluiu o carregamento.| 
| Excluído| Clipe de jogo foi excluído.| 
| Erro| Clipe de jogo está em um erro de estado e inutilizável.| 
  