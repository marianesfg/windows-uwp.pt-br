---
ms.assetid: 95CF7F3D-9E3A-40AC-A083-D8A375272181
title: Práticas recomendadas para usar o pool de threads
description: Este tópico descreve práticas recomendadas para trabalhar com o pool de threads.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, thread, pool de threads
ms.localizationpriority: medium
ms.openlocfilehash: a498f685e7a810d19e2f1eb63ae112dd02587b84
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370690"
---
# <a name="best-practices-for-using-the-thread-pool"></a>Práticas recomendadas para usar o pool de threads

Este tópico descreve práticas recomendadas para trabalhar com o pool de threads.

## <a name="dos"></a>Fazer


-   Use o pool de threads para realizar trabalho em paralelo em seu aplicativo.

-   Use os itens de trabalho para realizar tarefas estendidas sem bloquear o thread de interface do usuário.

-   Crie itens de trabalho que têm vida útil curta e são independentes. Os itens de trabalho são executados assincronamente e podem ser enviados para o pool em qualquer ordem da fila.

-   Envie atualizações para o thread de interface do usuário com o [**Windows.UI.Core.CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher).

-   Use [**ThreadPoolTimer.CreateTimer**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createtimer), em vez da função **Sleep**.

-   Use o pool de threads em vez de criar seu próprio sistema de gerenciamento de threads. O pool de threads é executado a nível do SO com funcionalidade avançada e é otimizado para escalar dinamicamente de acordo com os recursos e a atividade do dispositivo dentro do processo e ao longo do sistema.

-   Em C++, verifique se os representantes de item de trabalho usam o modelo de threading ágil (representantes de C++ são ágeis por padrão).

-   Use itens de trabalho pré-alocados quando não puder tolerar uma falha de alocação de recursos em tempo de uso.

## <a name="donts"></a>O que não fazer


-   Não crie temporizadores periódicos com um valor de *período* de &lt;1 milissegundo (incluindo 0). Isso fará com que o item de trabalho se comporte como um temporizador de disparo único.

-   Não envie itens de trabalho periódicos que levem mais tempo para concluir do que a quantidade de tempo que você especificou no parâmetro *período*.

-   Não tente enviar atualizações de IU (além de notificações do sistema e notificações) a partir de um item de trabalho despachado de uma tarefa em segundo plano. Em vez disso, use progresso de tarefa em segundo plano e manipuladores de conclusão - por exemplo, [**IBackgroundTaskInstance.Progress**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.progress).

-   Fique atento quando for usar manipuladores de item de trabalho que usem a palavra-chave **async**, pois o item de trabalho do pool de threads pode ser definido para o estado completo antes de todo o código no manipulador ter sido executado. Códigos que seguem uma palavra-chave **await** dentro do manipulador podem ser executados após o item de trabalho ter sido definido para o estado completo.

-   Não tente executar um item de trabalho pré-alocado mais de uma vez sem reiniciá-lo. [Criar um item de trabalho periódico](create-a-periodic-work-item.md)

## <a name="related-topics"></a>Tópicos relacionados


* [Criar um item de trabalho periódico](create-a-periodic-work-item.md)
* [Enviar um item de trabalho ao pool de threads](submit-a-work-item-to-the-thread-pool.md)
* [Usar um temporizador para enviar um item de trabalho](use-a-timer-to-submit-a-work-item.md)
