---
Description: Solução de eventos e erros com o Fazer um Teste da Microsoft com o visualizador de eventos.
title: Solução de problemas com Fazer um Teste da Microsoft com o visualizador de eventos.
ms.assetid: 9218e542-f520-4616-98fc-b113d5a08e0f
ms.date: 10/06/2017
ms.topic: article
keywords: windows 10, uwp, educação
ms.localizationpriority: medium
ms.openlocfilehash: 2f4bdcf45c7dd37dd540a666d99b5fa2fd2d49f8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598471"
---
# <a name="troubleshoot-microsoft-take-a-test-with-the-event-viewer"></a>Solução de problemas com Fazer um Teste da Microsoft com o visualizador de eventos

Você pode usar o Visualizador de Eventos para exibir eventos e erros com Fazer um Teste. O Fazer um Teste registra eventos quando uma solicitação de bloqueio é recebida, o registro dos dispositivos foi feito com êxito, as políticas de bloqueio foram aplicadas com êxito e muito mais.

Para habilitar a exibição de eventos no Visualizador de Eventos:
1. Abra o `Event Viewer`
2. Navegue até `Applications and Services Logs > Microsoft > Windows > Management-SecureAssessment`
3. Clique com botão direito `Operational` e selecione `Enable Log`

Para salvar os logs de eventos:
1. Clique com botão direito `Operational`
2. Clique em `Save All Events As…`
