---
author: martinekuan
title: Eventos personalizados e acessadores de evento em componentes do Tempo de Execução do Windows
description: O suporte do .NET framework para componentes do Tempo de Execução do Windows facilitar declarar componentes de eventos ocultando as diferenças entre o padrão do evento da Plataforma Universal do Windows (UWP) e o padrão de evento do .NET Framework.
ms.assetid: 6A66D80A-5481-47F8-9499-42AC8FDA0EB4
---

# Eventos personalizados e acessadores de evento em componentes do Tempo de Execução do Windows


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


\[Algumas informações dizem respeito a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não dá nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.\]

O suporte do .NET framework para componentes do Tempo de Execução do Windows facilitar declarar componentes de eventos ocultando as diferenças entre o padrão do evento da Plataforma Universal do Windows (UWP) e o padrão de evento do .NET Framework. No entanto, ao declarar acessadores de eventos personalizado em um componente do Tempo de Execução do Windows, você deve seguir o padrão usado na UWP.

## Registro de eventos


Quando você se registra para manipular um evento na UWP, acessador add retorna um token. Para cancelar o registro, você passa esse token para o acessador remove. Isso significa que os acessadores add e remove de eventos da UWP têm assinaturas diferentes dos acessadores a que você está acostumado.

Felizmente, os compiladores de Visual Basic e C# simplificam esse processo: quando você declara um evento usando acessadores personalizados em um componente do Tempo de Execução do Windows, os compiladores usam automaticamente o padrão UWP. Por exemplo, você obtém um erro de compilador caso o acessador add não retorne um token. O .NET Framework fornece dois tipos para dar suporte à implementação:

-   A estrutura [EventRegistrationToken](https://msdn.microsoft.com/library/windows/apps/windows.foundation.eventregistrationtoken.aspx) representa o token.
-   A classe [EventRegistrationTokenTable&lt;T&gt;](https://msdn.microsoft.com/library/hh138412.aspx) cria tokens e mantém um mapeamento entre tokens e manipuladores de eventos. O argumento de tipo genérico é o tipo de argumento do evento. Você cria uma instância dessa classe para cada evento na primeira vez em que um manipulador de eventos é registrado para esse evento.

O código a seguir do evento NumberChanged mostra o padrão básico de eventos UWP. Neste exemplo, o construtor do objeto de argumento do evento, NumberChangedEventArgs, utiliza um único parâmetro inteiro que representa o valor numérico alterado.

> **Observação**  Esse é o mesmo padrão que os compiladores usam para eventos comuns que você declara em um componente do Tempo de Execução do Windows.

 
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

> **Importante**  Para garantir a segurança do thread, o campo que mantém a instância do evento de EventRegistrationTokenTable&lt;T&gt; deve ser um campo de nível de classe. Caso ele seja um campo de nível de classe, o método GetOrCreateEventRegistrationTokenTable garante que, quando vários threads tentam criar a tabela de tokens, todos os threads obtêm a mesma instância da tabela. Para um determinado evento, todas as chamadas para o método GetOrCreateEventRegistrationTokenTable devem usar o mesmo campo de nível de classe.

Chamar o método GetOrCreateEventRegistrationTokenTable no acessador remove e no método [RaiseEvent](https://msdn.microsoft.com/library/fwd3bwed.aspx) (o método OnRaiseEvent em C#) garante que nenhuma exceção ocorra caso esses métodos sejam chamados antes de qualquer representante do manipulador de eventos ter sido adicionado.

Entre os outros membros da classe EventRegistrationTokenTable&lt;T&gt; que são usados no padrão de evento UWP estão os seguintes:

-   O método [AddEventHandler](https://msdn.microsoft.com/library/hh138458.aspx) gera um token para o representante do manipulador de eventos, armazena o representante na tabela, adiciona-o à lista de invocações e devolve o token.
-   A sobrecarga de método [RemoveEventHandler(EventRegistrationToken)](https://msdn.microsoft.com/library/hh138425.aspx) remove o representante da tabela e da lista de invocações.

    >**Observação**  Os métodos AddEventHandler e RemoveEventHandler(EventRegistrationToken) bloqueiam a tabela para ajudar a garantir a segurança do thread.

-   A propriedade [InvocationList](https://msdn.microsoft.com/library/hh138465.aspx) retorna um representante que inclui todos os manipuladores de eventos registrados no momento para manipular o evento. Use esse representante para acionar o evento ou use os métodos da classe Delegate para invocar os manipuladores individualmente.

    >**Observação**  Recomendamos seguir o padrão mostrado no exemplo fornecido anteriormente neste artigo e copiar o representante para uma variável temporária antes de chamá-lo. Isso evita uma condição de corrida em que um thread remove o último manipulador, o que reduz o representante para nulo antes de outro thread tentar invocar o representante. Como os representantes são imutáveis, a cópia continua sendo válida.

Coloque o próprio código nos acessadores conforme apropriado. Caso a segurança do thread seja um problema, você deve fornecer o próprio bloqueio para o código.

Usuários de C#: quando você escreve acessadores de eventos personalizados no padrão de eventos da UWP, o compilador não fornece os atalhos sintáticos usuais. Ele gera erros caso você use o nome do evento no código.

Usuários do Visual Basic: no .NET Framework, um evento é apenas um representante multicast que representa todos os manipuladores de eventos registrados. Acionar o evento significa apenas invocar o representante. A sintaxe do Visual Basic normalmente oculta as interações com o representante, e o compilador copia o representante antes de invocá-lo, conforme descrito na observação sobre segurança do thread. Ao criar um evento personalizado em um componente do Tempo de Execução do Windows, você precisa lidar com o representante diretamente. Isso também significa ser possível, por exemplo, usar o método [MulticastDelegate.GetInvocationList](https://msdn.microsoft.com/library/system.multicastdelegate.getinvocationlist.aspx) para obter uma matriz que contém um representante separado para cada manipulador de eventos, caso você queira invocar os manipuladores separadamente.

## Tópicos relacionados

* [Eventos (Visual Basic)](https://msdn.microsoft.com/library/ms172877.aspx)
* [Eventos (guia de programação de C#)](https://msdn.microsoft.com/library/awbftdfh.aspx)
* [Visão geral do .NET para aplicativos da Windows Store](https://msdn.microsoft.com/library/windows/apps/xaml/br230302.aspx)
* [.NET para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx)
* [Procedimento passo a passo: criação de um componente do Tempo de Execução do Windows básico chamada dele em JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)



<!--HONumber=May16_HO2-->


