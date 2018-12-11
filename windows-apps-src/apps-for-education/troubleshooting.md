---
Description: Troubleshoot Microsoft Take a Test events and errors with the event viewer.
title: Solução de problemas com Fazer um Teste da Microsoft com o visualizador de eventos.
ms.assetid: 9218e542-f520-4616-98fc-b113d5a08e0f
ms.date: 10/06/2017
ms.topic: article
keywords: Windows 10, uwp, educação
ms.localizationpriority: medium
ms.openlocfilehash: 2f4bdcf45c7dd37dd540a666d99b5fa2fd2d49f8
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8947056"
---
# <a name="troubleshoot-microsoft-take-a-test-with-the-event-viewer"></a>Solução de problemas com Fazer um Teste da Microsoft com o visualizador de eventos

Você pode usar o Visualizador de Eventos para exibir eventos e erros com Fazer um Teste. O Fazer um Teste registra eventos quando uma solicitação de bloqueio é recebida, o registro dos dispositivos foi feito com êxito, as políticas de bloqueio foram aplicadas com êxito e muito mais.

Para habilitar a exibição de eventos no Visualizador de Eventos:
1. Abra o `Event Viewer`
2. Navegue até `Applications and Services Logs > Microsoft > Windows > Management-SecureAssessment`
3. Clique com o botão direito do mouse em `Operational` e selecione `Enable Log`

Para salvar os logs de eventos:
1. Clique com o botão direito do mouse `Operational`
2. Clique em `Save All Events As…`
