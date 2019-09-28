---
title: Eventos personalizados e acessadores de evento em componentes do Tempo de Execução do Windows
description: O suporte do .NET para componentes de Windows Runtime facilita a declaração de componentes de eventos, ocultando as diferenças entre o padrão de evento Plataforma Universal do Windows (UWP) e o padrão de evento .NET.
ms.assetid: 6A66D80A-5481-47F8-9499-42AC8FDA0EB4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1891a89d83ea8a193d5c0fcedf939101323981e6
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340539"
---
# <a name="custom-events-and-event-accessors-in-windows-runtime-components"></a>Eventos personalizados e acessadores de evento em componentes do Tempo de Execução do Windows

O suporte do .NET para componentes de Windows Runtime facilita a declaração de componentes de eventos, ocultando as diferenças entre o padrão de evento Plataforma Universal do Windows (UWP) e o padrão de evento .NET. No entanto, ao declarar acessadores de eventos personalizados em um componente Windows Runtime, você deve seguir o padrão usado no UWP.

## <a name="registering-events"></a>Registro de eventos

Quando você se registra para manipular um evento na UWP, acessador add retorna um token. Para cancelar o registro, você passa esse token para o acessador remove. Isso significa que os acessadores add e remove de eventos da UWP têm assinaturas diferentes dos acessadores a que você está acostumado.

Felizmente, o Visual Basic e C# os compiladores simplificam esse processo: Quando você declara um evento com acessadores personalizados em um componente Windows Runtime, os compiladores usam automaticamente o padrão UWP. Por exemplo, você obtém um erro de compilador caso o acessador add não retorne um token. O .NET fornece dois tipos para dar suporte à implementação:

-   A estrutura [EventRegistrationToken](https://docs.microsoft.com/uwp/api/windows.foundation.eventregistrationtoken) representa o token.
-   A classe [EventRegistrationTokenTable&lt;T&gt;](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1) cria tokens e mantém um mapeamento entre tokens e manipuladores de eventos. O argumento de tipo genérico é o tipo de argumento do evento. Você cria uma instância dessa classe para cada evento na primeira vez em que um manipulador de eventos é registrado para esse evento.

O código a seguir do evento NumberChanged mostra o padrão básico de eventos UWP. Neste exemplo, o construtor do objeto de argumento do evento, NumberChangedEventArgs, utiliza um único parâmetro inteiro que representa o valor numérico alterado.

> **Observe**que   This é o mesmo padrão que os compiladores usam para eventos comuns que você declara em um componente Windows Runtime.

 
> [!div class="tabbedCodeSnippets"]
> ```csharp
> private EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>     m_NumberChangedTokenTable = null;
>
> public event EventHandler<NumberChangedEventArgs> NumberChanged
> {
>     add
>     {
>         return EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>             .GetOrCreateEventRegistrationTokenTable(ref m_NumberChangedTokenTable)
>             .AddEventHandler(value);
>     }
>     remove
>     {
>         EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>             .GetOrCreateEventRegistrationTokenTable(ref m_NumberChangedTokenTable)
>             .RemoveEventHandler(value);
>     }
> }
>
> internal void OnNumberChanged(int newValue)
> {
>     EventHandler<NumberChangedEventArgs> temp =
>         EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>         .GetOrCreateEventRegistrationTokenTable(ref m_NumberChangedTokenTable)
>         .InvocationList;
>     if (temp != null)
>     {
>         temp(this, new NumberChangedEventArgs(newValue));
>     }
> }
> ```
> ```vb
> Private m_NumberChangedTokenTable As  _
>     EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs))
>
> Public Custom Event NumberChanged As EventHandler(Of NumberChangedEventArgs)
>
>     AddHandler(ByVal handler As EventHandler(Of NumberChangedEventArgs))
>         Return EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs)).
>             GetOrCreateEventRegistrationTokenTable(m_NumberChangedTokenTable).
>             AddEventHandler(handler)
>     End AddHandler
>
>     RemoveHandler(ByVal token As EventRegistrationToken)
>         EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs)).
>             GetOrCreateEventRegistrationTokenTable(m_NumberChangedTokenTable).
>             RemoveEventHandler(token)
>     End RemoveHandler
>
>     RaiseEvent(ByVal sender As Class1, ByVal args As NumberChangedEventArgs)
>         Dim temp As EventHandler(Of NumberChangedEventArgs) = _
>             EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs)).
>             GetOrCreateEventRegistrationTokenTable(m_NumberChangedTokenTable).
>             InvocationList
>         If temp IsNot Nothing Then
>             temp(sender, args)
>         End If
>     End RaiseEvent
> End Event
> ```

O método estático (compartilhado em Visual Basic) GetOrCreateEventRegistrationTokenTable cria a instância do evento do objeto EventRegistrationTokenTable&lt;T&gt; lentamente. Passe o campo de nível de classe que armazenará a instância da tabela de tokens para esse método. Caso o campo esteja vazio, o método cria a tabela, armazena uma referência à tabela no campo e retorna uma referência para a tabela. Caso o campo já contenha uma referência à tabela de tokens, o método retorna apenas essa referência.

> **Importante**  to garantir a segurança do thread, o campo que mantém a instância do evento de EventRegistrationTokenTable @ No__t-2T @ no__t-3 deve ser um campo de nível de classe. Caso ele seja um campo de nível de classe, o método GetOrCreateEventRegistrationTokenTable garante que, quando vários threads tentam criar a tabela de tokens, todos os threads obtêm a mesma instância da tabela. Para um determinado evento, todas as chamadas para o método GetOrCreateEventRegistrationTokenTable devem usar o mesmo campo de nível de classe.

Chamar o método GetOrCreateEventRegistrationTokenTable no acessador remove e no método [RaiseEvent](https://docs.microsoft.com/dotnet/articles/visual-basic/language-reference/statements/raiseevent-statement) (o método OnRaiseEvent em C#) garante que nenhuma exceção ocorra caso esses métodos sejam chamados antes de qualquer representante do manipulador de eventos ter sido adicionado.

Entre os outros membros da classe EventRegistrationTokenTable&lt;T&gt; que são usados no padrão de evento UWP estão os seguintes:

-   O método [AddEventHandler](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1.addeventhandler#System_Runtime_InteropServices_WindowsRuntime_EventRegistrationTokenTable_1_AddEventHandler__0_) gera um token para o representante do manipulador de eventos, armazena o representante na tabela, adiciona-o à lista de invocações e devolve o token.
-   A sobrecarga de método [RemoveEventHandler(EventRegistrationToken)](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1.removeeventhandler#System_Runtime_InteropServices_WindowsRuntime_EventRegistrationTokenTable_1_RemoveEventHandler_System_Runtime_InteropServices_WindowsRuntime_EventRegistrationToken_) remove o representante da tabela e da lista de invocações.

    >**Observe**que os métodos   a AddEventHandler e RemoveEventHandler (EventRegistrationToken) bloqueiam a tabela para ajudar a garantir a segurança do thread.

-   A propriedade [InvocationList](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1.invocationlist#System_Runtime_InteropServices_WindowsRuntime_EventRegistrationTokenTable_1_InvocationList) retorna um representante que inclui todos os manipuladores de eventos registrados no momento para manipular o evento. Use esse representante para acionar o evento ou use os métodos da classe Delegate para invocar os manipuladores individualmente.

    >**Observe**  we recomendamos que você siga o padrão mostrado no exemplo fornecido anteriormente neste artigo e copie o delegado para uma variável temporária antes de chamá-lo. Isso evita uma condição de corrida em que um thread remove o último manipulador, o que reduz o representante para nulo antes de outro thread tentar invocar o representante. Como os representantes são imutáveis, a cópia continua sendo válida.

Coloque o próprio código nos acessadores conforme apropriado. Caso a segurança do thread seja um problema, você deve fornecer o próprio bloqueio para o código.

C#podem Quando você escreve acessadores de evento personalizados no padrão de evento UWP, o compilador não fornece os atalhos sintáticas usuais. Ele gera erros caso você use o nome do evento no código.

Visual Basic usuários: No .NET, um evento é apenas um delegado de multicast que representa todos os manipuladores de eventos registrados. Acionar o evento significa apenas invocar o representante. A sintaxe do Visual Basic normalmente oculta as interações com o representante, e o compilador copia o representante antes de invocá-lo, conforme descrito na observação sobre segurança do thread. Ao criar um evento personalizado em um componente Windows Runtime, você precisa lidar diretamente com o delegado. Isso também significa ser possível, por exemplo, usar o método [MulticastDelegate.GetInvocationList](https://docs.microsoft.com/dotnet/api/system.multicastdelegate.getinvocationlist#System_MulticastDelegate_GetInvocationList) para obter uma matriz que contém um representante separado para cada manipulador de eventos, caso você queira invocar os manipuladores separadamente.

## <a name="related-topics"></a>Tópicos relacionados

* [Eventos (Visual Basic)](https://docs.microsoft.com/dotnet/articles/visual-basic/programming-guide/language-features/events/index)
* [Eventos (C# guia de programação)](https://docs.microsoft.com/dotnet/articles/csharp/programming-guide/events/index)
* [Visão geral dos aplicativos .NET para UWP](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140))
* [.NET para aplicativos UWP](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)
* [Passo a passo para criar um componente do Windows Runtime em C# ou Visual Basic e chamá-lo do JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
