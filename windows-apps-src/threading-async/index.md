---
ms.assetid: beac6333-655a-4bcf-9caf-bba15f715ea5
title: Programação threading e assíncrona
description: A programação threading e assíncrona permite que seu app realize o trabalho de forma assíncrona em threads paralelos.
ms.date: 05/14/2018
ms.topic: article
keywords: windows 10, uwp, assíncrono, threads, threading
ms.localizationpriority: medium
ms.openlocfilehash: 22c151b90be30b39da7decd9a0ce3109e29b7fb7
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7835511"
---
# <a name="threading-and-async-programming"></a>Programação threading e assíncrona
A programação threading e assíncrona permite que seu aplicativo realize o trabalho de forma assíncrona em threads paralelos.

O aplicativo pode usar o pool de threads para realizar o trabalho de forma assíncrona em threads paralelos. O pool de threads gerencia um conjunto de threads e usa uma fila para atribuir itens de trabalho a threads à medida que eles se tornam disponíveis. O pool de threads é semelhante aos padrões de programação assíncrona disponíveis no Windows Runtime porque ele pode ser usado para realizar o trabalho estendido sem bloquear a interface do usuário, mas o pool de threads oferece mais controle do que os padrões de programação assíncrona e você pode usá-lo para concluir vários itens de trabalho em paralelo. Você pode usar o pool de threads para:

-   Enviar itens de trabalho, controlar sua prioridade e cancelar itens de trabalho.

-   Programar itens de trabalho usando timers e temporizadores periódicos.

-   Reserve recursos para itens de trabalho crítico.

-   Os itens de trabalho são executados em resposta a eventos nomeados e semáforos.

O pool de threads é mais eficiente no gerenciamento de threads porque reduz a sobrecarga de criação e destruição de threads. Isso significa que ele tem acesso para otimizar os threads em vários núcleos de CPU e pode equilibrar os recursos de threads entre os aplicativos e quando as tarefas em segundo plano estão sendo executadas. Usar o pool de threads interno é conveniente porque você se concentra na escrita de código que realiza uma tarefa em vez da mecânica de gerenciamento de threads.

| Tópico                                                                                                          | Descrição                         |
|----------------------------------------------------------------------------------------------------------------|-------------------------------------|
| [Programação assíncrona (aplicativos UWP)](asynchronous-programming-universal-windows-platform-apps.md)              | Este tópico descreve a programação assíncrona na plataforma Universal do Windows (UWP) e sua representação em c#, Microsoft Visual Basic.NET VisualC + + extensões de componente (C++ c++ /CX) e JavaScript. |
| [Programação assíncrona em C++/CX (aplicativos UWP)](asynchronous-programming-in-cpp-universal-windows-platform-apps.md)| Este artigo descreve a maneira recomendada de consumir métodos assíncronos em C++/CX usando a classe <code>task</code><code>concurrency</code> que é definida no namespace  em ppltasks.h. |
| [Práticas recomendadas para usar o pool de threads](best-practices-for-using-the-thread-pool.md)                         | Este tópico descreve as práticas recomendadas para trabalhar com o pool de threads. |
| [Chamar APIs assíncronas no Visual Basic ou C#](call-asynchronous-apis-in-csharp-or-visual-basic.md)             | A Plataforma Universal do Windows (UWP) inclui muitas APIs assíncronas para garantir que o seu aplicativo permaneça responsivo ao executar trabalhos demorados. Este tópico descreve como usar métodos assíncronos da UWP em C# ou Microsoft Visual Basic. |
| [Criar um item de trabalho periódico](create-a-periodic-work-item.md)                                                   | Saiba como criar um item de trabalho periódico que se repete periodicamente. |
| [Enviar um item de trabalho ao pool de threads](submit-a-work-item-to-the-thread-pool.md)                               | Aprenda a trabalhar em um thread separado enviando um item de trabalho ao pool de threads. |
| [Usar um temporizador para enviar um item de trabalho](use-a-timer-to-submit-a-work-item.md)                                       | Saiba como criar um item de trabalho que seja executado após um temporizador. |
