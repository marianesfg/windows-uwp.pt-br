---
author: msatranjr
title: Criando componentes do Windows Runtime em C# e Visual Basic
description: "A partir do .NET Framework 4.5, é possível usar código gerenciado para criar os próprios tipos do Windows Runtime, empacotados em um componente do Tempo de Execução do Windows."
ms.assetid: A5672966-74DF-40AB-B01E-01E3FCD0AD7A
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: a5fd89757817c06be55020d74e677369725af5a2
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="creating-windows-runtime-components-in-c-and-visual-basic"></a>Criando componentes do Windows Runtime em C# e Visual Basic


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

A partir do .NET Framework 4.5, é possível usar código gerenciado para criar os próprios tipos do Windows Runtime, empacotados em um componente do Tempo de Execução do Windows. É possível usar o componente em aplicativos da Plataforma Universal do Windows (UWP) com C++, JavaScript, Visual Basic ou C#. Este artigo descreve as regras para a criação de um componente e descreve alguns aspectos do suporte do .NET Framework para o Windows Runtime. Em geral, esse suporte foi projetado para ser transparente para o programador do .NET Framework. No entanto, ao criar um componente a ser usado com JavaScript ou C++, você precisa estar ciente das diferenças na maneira como essas linguagens dão suporte ao Windows Runtime.

Caso você esteja criando um componente a ser usado apenas em aplicativos UWP com Visual Basic ou C# e o componente não contenha controles UWP, leve em consideração usar o modelo **Biblioteca de Classes**, em vez do modelo **Componente do Tempo de Execução do Windows**. Existem menos restrições em uma biblioteca de classes simples.

Este artigo contém as seguintes seções:

## <a name="declaring-types-in-windows-runtime-components"></a>Declaração de tipos em componentes do Windows Runtime


Internamente, os tipos do Windows Runtime no componente podem usar qualquer funcionalidade do .NET Framework permitida em um aplicativo Universal do Windows. (Confira a visão geral [.NET para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx) para obter mais informações.) Externamente, os membros de seus tipos podem expor somente tipos do Windows Runtime para os parâmetros e valores de retorno. A lista a seguir descreve as limitações em tipos do .NET Framework expostos em componentes do Windows Runtime.

-   Os campos, os parâmetros e os valores de retorno de todos os tipos públicos e membros no componente devem ser tipos do Windows Runtime.

    Essa restrição inclui os tipos do Windows Runtime que você cria, bem como os tipos fornecidos pelo Windows Runtime propriamente dito. Ele também inclui vários tipos do .NET Framework. A inclusão desses tipos faz parte do suporte que o .NET Framework fornece para habilitar o uso natural do Windows Runtime em código gerenciado: o código aparentemente usa tipos do .NET Framework familiares, em vez dos tipos do Windows Runtime subjacentes. Por exemplo, é possível usar tipos primitivos do .NET Framework, como Int32 e Double, determinados tipos fundamentais como DateTimeOffset e Uri, e alguns tipos de interface genérica mais usados, como IEnumerable&lt;T&gt; (IEnumerable(Of T) em Visual Basic) e IDictionary&lt;TKey,TValue&gt;. (Observe que os argumentos de tipo desses tipos genéricos devem ser tipos do Windows Runtime.) Isso é discutido nas seções Passagem de tipos do Windows Runtime para código gerenciado e Passagem de tipos gerenciados para o Windows Runtime, mais adiante neste artigo.

-   Classes públicas e interfaces podem conter métodos, propriedades e eventos. É possível declarar representantes para os eventos ou usar o representante EventHandler&lt;T&gt;. Uma classe pública ou interface não pode:

    -   Ser genérica.
    -   Implementar uma interface que não seja uma interface do Windows Runtime. (No entanto, é possível criar as próprias interfaces do Windows Runtime e implementá-las.)
    -   Derivar de tipos que não estejam no Windows Runtime, como System.Exception e System.EventArgs.
-   Todos os tipos públicos devem ter um namespace raiz correspondente ao nome do assembly, e o nome do assembly não deve começar com "Windows".

    > **Dica**  Por padrão, os projetos do Visual Studio têm nomes de namespace correspondentes ao nome do assembly. No Visual Basic, a instrução Namespace desse namespace padrão não é mostrada no código.

-   As estruturas públicas não podem ter membros que não sejam campos públicos, e esses campos devem ser tipos de valor ou cadeias de caracteres.
-   As classes públicas devem ser **sealed** (**NotInheritable** no Visual Basic). Caso o modelo de programação exija polimorfismo, é possível criar uma interface pública e implementar essa interface nas classes que devem ser polimórficas.

## <a name="debugging-your-component"></a>Depuração do componente


Caso o aplicativo Universal do Windows e o componente sejam compilados com código gerenciado, é possível depurá-los ao mesmo tempo.

Ao testar o componente como parte de um aplicativo Universal do Windows em C++, você pode depurar código gerenciado e nativo ao mesmo tempo. O padrão é somente código nativo.

## **<a name="to-debug-both-native-c-code-and-managed-code"></a>Para depurar códigos em C++ nativo e gerenciado**

1.  Abra o menu de atalho do projeto do Visual C++ e escolha **Propriedades**.
2.  Nas páginas de propriedades, em **Propriedades de Configuração**, escolha **Depuração**.
3.  Escolha **Tipo de Depurador** e, na caixa de listagem suspensa, altere **Native Only** para **Misto (Gerenciado e Nativo)**. Escolha **OK**.
4.  Defina pontos de interrupção nos códigos nativo e gerenciado.

Quando você está testando o componente como parte de um aplicativo Universal do Windows usando JavaScript, por padrão, a solução está no modo de depuração de JavaScript. No Visual Studio, não é possível depurar JavaScript e código gerenciado ao mesmo tempo.

## **<a name="to-debug-managed-code-instead-of-javascript"></a>Para depurar código gerenciado em vez de JavaScript**

1.  Abra o menu de atalho do projeto JavaScript e escolha **Propriedades**.
2.  Nas páginas de propriedades, em **Propriedades de Configuração**, escolha **Depuração**.
3.  Escolha **Tipo de Depurador** e, na caixa de listagem suspensa, altere **Script Only** para **Managed Only**. Escolha **OK**.
4.  Defina pontos de interrupção no código gerenciado e depure como sempre.

## <a name="passing-windows-runtime-types-to-managed-code"></a>Passagem de tipos do Windows Runtime para código gerenciado


Conforme mencionado anteriormente na seção Declaração de tipos em componentes do Windows Runtime, determinados tipos .NET Framework podem ser exibidos nas assinaturas de membros de classes públicas. Isso faz parte do suporte que o .NET Framework oferece para habilitar o uso natural do Windows Runtime em código gerenciado. Ele inclui tipos primitivos e algumas classes e interfaces. Quando o componente é usado em JavaScript ou em código C++, é importante saber como os tipos do .NET Framework são exibidos para o chamador. Consulte [Passo a passo: criação de um componente simples em C# ou Visual Basic e a chamada dele em JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) para obter exemplos com JavaScript. Esta seção aborda os tipos mais usados.

No .NET Framework, tipos primitivos como a estrutura Int32 têm muitas propriedades e métodos úteis, como o método TryParse. Por outro lado, tipos primitivos e estruturas no Windows Runtime só têm campos. Quando você passa esses tipos para código gerenciado, eles aparentam ser tipos do .NET Framework, e é possível usar as propriedades e os métodos dos tipos do .NET Framework, como faria normalmente. A lista a seguir resume as substituições que são feitas automaticamente no IDE:

-   Para as primitivas do Windows Runtime, Int32, Int64, Single, Double, Boolean, String (uma coleção imutável de caracteres Unicode), Enum, UInt32, UInt64 e Guid, use o tipo de mesmo nome no namespace System.
-   Para UInt8, use System.Byte.
-   Para Char16, use System.Char.
-   Para a interface IInspectable, use System.Object.

Caso C# ou Visual Basic forneça uma palavra-chave de linguagem para qualquer um desses tipos, você pode usar a palavra-chave de linguagem em seu lugar.

Além de tipos primitivos, alguns tipos do Windows Runtime básicos mais usados são exibidos em código gerenciado como os equivalentes do .NET Framework. Por exemplo, suponhamos que o código JavaScript use a classe Windows.Foundation.Uri e você queira passá-lo para um método C# ou Visual Basic. O tipo equivalente em código gerenciado é a classe System.Uri do .NET Framework, e esse é o tipo a ser usado para o parâmetro do método. É possível dizer quando um tipo do Windows Runtime é exibido como um tipo do .NET Framework, porque o IntelliSense no Visual Studio oculta o tipo do Windows Runtime quando você está escrevendo código gerenciado e apresenta o tipo .NET Framework equivalente. (Normalmente, os dois tipos têm o mesmo nome. No entanto, observe que a estrutura Windows.Foundation.DateTime é exibida em código gerenciado como DateTimeOffset, e não como System. DateTime.)

Para alguns tipos de coleção mais usados, o mapeamento é entre as interfaces implementadas por um tipo do Windows Runtime e as interfaces implementadas pelo tipo .NET Framework correspondente. Assim como acontece com os tipos mencionados acima, você declara tipos de parâmetro usando o tipo .NET Framework. Isso oculta algumas diferenças entre os tipos e deixa a escrita do código .NET Framework mais natural. A tabela a seguir lista os tipos de interface genérica mais comuns, além de outros mapeamentos de classe e interface comuns. Para obter uma lista completa dos tipos do Windows Runtime que o .NET Framework mapeia, consulte os mapeamentos do .NET Framework dos tipos do Windows Runtime.

| Windows Runtime                                  | .NET Framework                                    |
|--------------------------------------------------|---------------------------------------------------|
| IIterable&lt;T&gt;                               | IEnumerable&lt;T&gt;                              |
| IVector&lt;T&gt;                                 | IList&lt;T&gt;                                    |
| IVectorView&lt;T&gt;                             | IReadOnlyList&lt;T&gt;                            |
| IMap&lt;K, V&gt;                                 | IDictionary&lt;TKey, TValue&gt;                   |
| IMapView&lt;K, V&gt;                             | IReadOnlyDictionary&lt;TKey, TValue&gt;           |
| IKeyValuePair&lt;K, V&gt;                        | KeyValuePair&lt;TKey, TValue&gt;                  |
| IBindableIterable                                | IEnumerable                                       |
| IBindableVector                                  | IList                                             |
| Windows.UI.Xaml.Data.INotifyPropertyChanged      | System.ComponentModel.INotifyPropertyChanged      |
| Windows.UI.Xaml.Data.PropertyChangedEventHandler | System.ComponentModel.PropertyChangedEventHandler |
| Windows.UI.Xaml.Data.PropertyChangedEventArgs    | System.ComponentModel.PropertyChangedEventArgs    |

 

Quando um tipo implementa mais de uma interface, é possível usar qualquer uma das interfaces implementadas como um tipo de parâmetro ou um tipo de retorno de um membro. Por exemplo, é possível passar ou retornar um Dictionary&lt;int, string&gt; (Dictionary(Of Integer, String) em Visual Basic) como IDictionary&lt;int, string&gt;, IReadOnlyDictionary&lt;int, string&gt; ou IEnumerable&lt;System.Collections.Generic.KeyValuePair&lt;TKey, TValue&gt;&gt;.

**Importante**  O JavaScript usa a primeira interface exibida na lista de interfaces implementadas por um tipo gerenciado. Por exemplo, se você retornar Dictionary&lt;int, string&gt; ao código JavaScript, ele será exibido como IDictionary&lt;int, string&gt;, independentemente de qual interface você especificar como o tipo de retorno. Isso significa que, caso a primeira interface não inclua um membro exibido em interfaces posteriores, esse membro não permanece visível para JavaScript.

No Windows Runtime, IMap&lt;K, V&gt; e IMapView&lt;K, V&gt; são iterados usando-se IKeyValuePair. Quando você os passa para código gerenciado, eles são exibidos como IDictionary&lt;TKey, TValue&gt; e IReadOnlyDictionary&lt;TKey, TValue&gt;, logo, você naturalmente usa System.Collections.Generic.KeyValuePair&lt;TKey, TValue&gt; para enumerá-los.

A maneira como as interfaces são exibidas em código gerenciado afeta o modo como os tipos que implementam essas interfaces são exibidos. Por exemplo, a classe PropertySet implementa IMap&lt;K, V&gt;, que é exibido em código gerenciado como IDictionary&lt;TKey, TValue&gt;. PropertySet é exibido como se tivesse implementado IDictionary&lt;TKey, TValue&gt; em vez de IMap&lt;K, V&gt;, logo, em código gerenciado ele aparenta ter um método Add, que se comporta como o método Add em dicionários do .NET Framework. Ele não aparenta ter um método Insert. É possível consultar esse exemplo no artigo [Passo a passo: criação de um componente simples em C# ou Visual Basic e a chamada dele em JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

## <a name="passing-managed-types-to-the-windows-runtime"></a>Passagem de tipos gerenciados para o Windows Runtime


Conforme abordado na seção anterior, alguns tipos do Windows Runtime podem ser exibidos como tipos do .NET Framework nas assinaturas dos membros do componente ou nas assinaturas dos membros do Windows Runtime quando você os usa no IDE. Quando você passa tipos do .NET Framework para esses membros ou os usa como os valores de retorno dos membros do componente, eles são exibidos no código do outro lado como o tipo do Windows Runtime correspondente. Para obter exemplos dos efeitos que isso pode ter quando o componente é chamado no JavaScript, consulte a seção "Retorno de tipos gerenciados do componente" em [Passo a passo: criação de um componente simples em C# ou Visual Basic e a chamada dele em JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

## <a name="overloaded-methods"></a>Métodos sobrecarregados


No Windows Runtime, os métodos podem ser sobrecarregados. No entanto, se declarar várias sobrecargas com o mesmo número de parâmetros, você deverá aplicar o atributo [Windows.Foundation.Metadata.DefaultOverloadAttribute](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.defaultoverloadattribute.aspx) a apenas uma dessas sobrecargas. Essa sobrecarga é a única que é possível chamar no JavaScript. Por exemplo, no código a seguir, a sobrecarga que utiliza um **int** (**Integer** em Visual Basic) é a sobrecarga padrão.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public string OverloadExample(string s)
> {
>     return s;
> }
> [Windows.Foundation.Metadata.DefaultOverload()]
> public int OverloadExample(int x)
> {
>     return x;
> }
> ```
> ```vb
> Public Function OverloadExample(ByVal s As String) As String
>     Return s
> End Function
> <Windows.Foundation.Metadata.DefaultOverload> _
> Public Function OverloadExample(ByVal x As Integer) As Integer
>     Return x
> End Function
> ```

 **Cuidado**  O JavaScript permite passar qualquer valor para OverloadExample e força o valor para o tipo exigido pelo parâmetro. É possível chamar OverloadExample com "quarenta e dois", "42" ou 42.3, mas todos esses valores são passados para a sobrecarga padrão. A sobrecarga padrão no exemplo anterior retorna 0, 42 e 42, respectivamente.

Não é possível aplicar o atributo DefaultOverloadAttribute a construtores. Todos os construtores em uma classe devem ter números de parâmetros diferentes.

## <a name="implementing-istringable"></a>Implementação de IStringable


Desde o Windows 8.1, o Windows Runtime inclui uma interface IStringable cujo único método, IStringable.ToString, oferece suporte básico de formatação comparável ao oferecido por Object.ToString. Caso você opte por implementar IStringable em um tipo gerenciado público exportado em um componente do Tempo de Execução do Windows, as seguintes restrições se aplicam:

-   Só é possível definir a interface IStringable em um relacionamento "classe implementa", como o seguinte código em C#:

    ```cs
    public class NewClass : IStringable
    ```

    Ou o seguinte código do Visual Basic:

    ```vb
    Public Class NewClass : Implements IStringable
    ```

-   Não é possível implementar IStringable em uma interface.
-   Não é possível declarar um parâmetro para ser do tipo IStringable.
-   IStringable não pode ser o tipo de retorno de um método, propriedade ou campo.
-   Você não pode ocultar a implementação IStringable das classes base usando uma definição de método como a seguinte:

    ```cs
    public class NewClass : IStringable
    {
       public new string ToString()
       {
          return "New ToString in NewClass";
       }
    }
    ```

    Em vez disso, a implementação de IStringable.ToString deve sempre substituir a implementação da classe base. Você só pode ocultar uma implementação de ToString invocando-a em uma instância de classe fortemente tipada.

Em várias condições, chamadas do código nativo para um tipo gerenciado que implementa IStringable ou oculta a implementação de ToString podem produzir um comportamento inesperado.

## <a name="asynchronous-operations"></a>Operações assíncronas


Para implementar um método assíncrono no componente, adicione "Async" ao final do nome do método e retorne uma das interfaces do Windows Runtime que representam ações ou operações assíncronas: IAsyncAction, IAsyncActionWithProgress&lt;TProgress&gt;, IAsyncOperation&lt;TResult&gt; ou IAsyncOperationWithProgress&lt;TResult, TProgress&gt;.

É possível usar tarefas do .NET Framework (a classe [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) e a classe[Task&lt;TResult&gt;](https://msdn.microsoft.com/library/dd321424.aspx) genérica) para implementar o método assíncrono. Você deve retornar uma tarefa que represente uma operação contínua, como uma tarefa retornada de um método assíncrono escrito em C# ou Visual Basic, ou uma tarefa retornada do método [Task.Run](https://msdn.microsoft.com/library/system.threading.tasks.task.run.aspx). Caso use um construtor para criar a tarefa, você deve chamar o método [Task.Start](https://msdn.microsoft.com/library/system.threading.tasks.task.start.aspx) antes de devolvê-lo.

Um método que usa await (Await em Visual Basic) requer a palavra-chave **async** (**Async** em Visual Basic). Caso você exponha um método assim em um componente do Tempo de Execução do Windows, aplique a palavra-chave **async** ao representante que você passa para o método Run.

Para ações e operações assíncronas que não dão suporte ao cancelamento ou aos relatórios de progresso, é possível usar o método de extensão [WindowsRuntimeSystemExtensions.AsAsyncAction](https://msdn.microsoft.com/library/system.windowsruntimesystemextensions.asasyncaction.aspx) ou [AsAsyncOperation&lt;TResult&gt;](https://msdn.microsoft.com/library/hh779745.aspx) para encapsular a tarefa na interface apropriada. Por exemplo, o código a seguir implementa um método assíncrono usando o método Task.Run&lt;TResult&gt; para iniciar uma tarefa. O método de extensão AsAsyncOperation&lt;TResult&gt; retorna a tarefa como uma operação assíncrona do Windows Runtime.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
> {
>     return Task.Run<IList<string>>(async () =>
>     {
>         var data = await DownloadDataAsync(id);
>         return ExtractStrings(data);
>     }).AsAsyncOperation();
> }
> ```
> ```vb
> Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
>      As IAsyncOperation(Of IList(Of String))
>
>     Return Task.Run(Of IList(Of String))(
>         Async Function()
>             Dim data = Await DownloadDataAsync(id)
>             Return ExtractStrings(data)
>         End Function).AsAsyncOperation()
> End Function
> ```

O código JavaScript a seguir mostra como o método pode ser chamado usando-se um objeto [WinJS.Promise](https://msdn.microsoft.com/library/windows/apps/br211867.aspx). A função passada para o método then é executada quando a chamada assíncrona é concluída. O parâmetro stringList contém a lista de cadeias de caracteres retornada pelo método DownloadAsStringAsync, e a função faz o processamento necessário.

```javascript
function asyncExample(id) {

    var result = SampleComponent.Example.downloadAsStringAsync(id).then(
        function (stringList) {
            // Place code that uses the returned list of strings here.
        });
}
```

Para ações e operações assíncronas que dão suporte ao cancelamento ou aos relatórios de progresso, use a classe [AsyncInfo](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.asyncinfo.aspx) para gerar uma tarefa iniciada e vincular os recursos de cancelamento e relatórios de progresso da tarefa aos recursos de cancelamento e relatórios de progresso da interface do Windows Runtime. Para obter um exemplo que dê suporte ao cancelamento e aos relatórios de progresso, consulte [Passo a passo: criação de um componente simples em C# ou Visual Basic e a chamada dele em JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

É possível usar os métodos da classe AsyncInfo mesmo que o método assíncrono não dê suporte ao cancelamento ou aos relatórios de progresso. Caso você use uma função lambda do Visual Basic ou um método anônimo de C#, não forneça parâmetros para o token e a interface [IProgress&lt;T&gt;](https://msdn.microsoft.com/library/hh138298.aspx). Caso você use uma função lambda de C#, forneça um parâmetro token, mas o ignore. O exemplo anterior, que usou o método AsAsyncOperation&lt;TResult&gt;, tem esta aparência quando você usa a sobrecarga de método [AsyncInfo.Run&lt;TResult&gt;(Func&lt;CancellationToken, Task&lt;TResult&gt;&gt;](https://msdn.microsoft.com/library/hh779740.aspx)) em seu lugar:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
> {
>     return AsyncInfo.Run<IList<string>>(async (token) =>
>     {
>         var data = await DownloadDataAsync(id);
>         return ExtractStrings(data);
>     });
> }
> ```
> ```vb
> Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
>     As IAsyncOperation(Of IList(Of String))
>
>     Return AsyncInfo.Run(Of IList(Of String))(
>         Async Function()
>             Dim data = Await DownloadDataAsync(id)
>             Return ExtractStrings(data)
>         End Function)
> End Function
> ```

Caso você crie um método assíncrono que possa dar suporte ao cancelamento ou aos relatórios de progresso, leve em consideração a adição de sobrecargas que não tenham parâmetros para um token de cancelamento ou a interface IProgress&lt;T&gt;.

## <a name="throwing-exceptions"></a>Acionamento de exceções


É possível acionar qualquer tipo de exceção que esteja incluído no .NET para aplicativos do Windows. Não é possível declarar os próprios tipos de exceção pública em um componente do Tempo de Execução do Windows, mas você pode declarar e acionar tipos não públicos.

Caso o componente não manipule a exceção, uma exceção correspondente é acionada no código que chamou o componente. A maneira como a exceção é exibida para o chamador depende da maneira como a linguagem de chamada dá suporte ao Windows Runtime.

-   Em JavaScript, a exceção é exibida como um objeto no qual a mensagem de exceção é substituída por um rastreamento de pilha. Ao depurar o aplicativo no Visual Studio, você pode ver o texto da mensagem original exibido na caixa de diálogo de exceção do depurador, identificado como "Informações de WinRT". Não é possível acessar o texto da mensagem original no código JavaScript.

    > **Dica**  Atualmente, o rastreamento de pilha contém o tipo de exceção gerenciado, mas não recomendamos analisar o rastreamento para identificar o tipo de exceção. Em vez disso, use um valor HRESULT conforme descrito mais à frente nesta seção.

-   Em C++, a exceção é exibida como uma exceção da plataforma. Caso a propriedade HResult da exceção gerenciada possa ser mapeada para o HRESULT de uma exceção de plataforma específica, a exceção específica é usada; do contrário, uma exceção [Platform::COMException](https://msdn.microsoft.com/library/windows/apps/xaml/hh710414.aspx) é acionada. O texto da mensagem da exceção gerenciada não está disponível para o código C++. Caso uma exceção de plataforma específica seja acionada, o texto da mensagem padrão desse tipo de exceção é exibido; do contrário, nenhum texto de mensagem é exibido. Consulte [Exceções (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699896.aspx).
-   Em C# ou Visual Basic, a exceção é uma exceção gerenciada normal.

Ao acionar uma exceção no componente, você pode facilitar para um chamador JavaScript ou C++ lidar com a exceção acionando um tipo de exceção não público cujo valor da propriedade HResult é específico do componente. O HRESULT está disponível para o chamador JavaScript por meio da propriedade de número do objeto de exceção e para um chamador C++ por meio da propriedade [COMException::HResult](https://msdn.microsoft.com/library/windows/apps/xaml/hh710415.aspx).

> **Observação**  Use um valor negativo para o HRESULT. Um valor positivo é interpretado como êxito, e nenhuma exceção é acionada no chamador JavaScript ou C++.

## <a name="declaring-and-raising-events"></a>Declaração e acionamento de eventos

Quando você declara um tipo para manter os dados do evento, derive de Object, em vez de EventArgs, porque EventArgs não é um tipo do Windows Runtime. Use [EventHandler&lt;TEventArgs&gt;](https://msdn.microsoft.com/library/db0etb8x.aspx) como o tipo do evento e use o tipo de argumento do evento como o argumento de tipo genérico. Acione o evento, assim como você faria em um aplicativo do .NET Framework.

Quando o componente do Tempo de Execução do Windows é usado em JavaScript ou C++, o evento segue o padrão de evento do Windows Runtime que essas linguagens esperam. Quando você usa o componente de C# ou Visual Basic, o evento é exibido como um evento do .NET Framework comum. Um exemplo é dado em [Passo a passo: criação de um componente simples em C# ou Visual Basic e a chamada dele em JavaScript]().

Caso implemente acessadores de eventos personalizado (declarar um evento com a palavra-chave **Custom**, em Visual Basic), você deve seguir o padrão de evento do Windows Runtime na implementação. Consulte [Eventos personalizados e acessadores de evento em componentes do Windows Runtime](custom-events-and-event-accessors-in-windows-runtime-components.md). Quando você manipula o evento no código C# ou Visual Basic, ele ainda parece ser um evento do .NET Framework comum.

## <a name="next-steps"></a>Próximas etapas


Depois que tiver criado um componente do Tempo de Execução do Windows para o próprio uso, talvez você ache que a funcionalidade que ele encapsula é útil para outros desenvolvedores. Você tem duas opções para empacotar um componente para distribuição a outros desenvolvedores. Consulte [Distribuição de um componente do Tempo de Execução do Windows gerenciado](https://msdn.microsoft.com/library/jj614475.aspx).

Para saber mais sobre recursos da linguagem Visual Basic e C# e o suporte do .NET Framework ao Windows Runtime, consulte a [Referência das linguagens Visual Basic e C#](https://msdn.microsoft.com/library/windows/apps/xaml/br212458.aspx).

## <a name="related-topics"></a>Tópicos relacionados

* [Visão geral do .NET para aplicativos da Windows Store](https://msdn.microsoft.com/library/windows/apps/xaml/br230302.aspx)
* [.NET para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx)
* [Passo a passo: Criando um componente do Tempo de Execução do Windows simples e chamando-o em JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
