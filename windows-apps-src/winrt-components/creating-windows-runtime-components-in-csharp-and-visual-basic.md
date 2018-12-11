---
title: Criando componentes do Windows Runtime em C# e Visual Basic
description: A partir do .NET Framework 4.5, é possível usar código gerenciado para criar os próprios tipos do Windows Runtime, empacotados em um componente do Tempo de Execução do Windows.
ms.assetid: A5672966-74DF-40AB-B01E-01E3FCD0AD7A
ms.date: 12/04/2018
ms.topic: article
dev_langs:
- csharp
- vb
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b4f5a2de5c3fa5564b4e4389cfc0806fd5d2844f
ms.sourcegitcommit: 231065c899d0de285584d41e6335251e0c2c4048
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8827131"
---
# <a name="creating-windows-runtime-components-in-c-and-visual-basic"></a>Criando componentes do Windows Runtime em C# e Visual Basic
Começando com o .NET Framework 4.5, você pode usar código gerenciado para criar seus próprios tipos de tempo de execução do Windows e empacotá-las em um componente do tempo de execução do Windows. Você pode usar o componente em aplicativos da plataforma Universal do Windows (UWP) que são escritos em C++, JavaScript, Visual Basic ou c#. Este tópico descreve as regras para a criação de um componente e descreve alguns aspectos do suporte do .NET Framework para o Windows Runtime. Em geral, esse suporte foi projetado para ser transparente para o programador do .NET Framework. No entanto, ao criar um componente a ser usado com JavaScript ou C++, você precisa estar ciente das diferenças na maneira como essas linguagens dão suporte ao Windows Runtime.

Se você estiver criando um componente para uso somente em aplicativos UWP que são escritos em Visual Basic ou c#, e o componente não contenha controles UWP, em seguida, onsider usando o modelo de **Biblioteca de classes** em vez do modelo de projeto do **Componente de tempo de execução do Windows** no Microsoft Visual Studio. Existem menos restrições em uma biblioteca de classes simples.

## <a name="declaring-types-in-windows-runtime-components"></a>Declaração de tipos em componentes do Windows Runtime

Internamente, os tipos de tempo de execução do Windows no componente podem usar qualquer funcionalidade do .NET Framework permitida em um aplicativo UWP. Para obter mais informações, consulte [.NET para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/mt185501).

Externamente, os membros de seus tipos podem expor somente tipos de tempo de execução do Windows para os parâmetros e valores de retorno. A lista a seguir descreve as limitações em tipos do .NET Framework expostos em um componente do tempo de execução do Windows.

- Os campos, os parâmetros e os valores de retorno de todos os tipos públicos e membros no componente devem ser tipos do Windows Runtime. Essa restrição inclui os tipos de tempo de execução do Windows que você criar, bem como tipos fornecidos pelo Windows Runtime em si. Ele também inclui vários tipos do .NET Framework. A inclusão desses tipos faz parte do suporte do .NET Framework oferece para habilitar o uso natural do Windows Runtime em código gerenciado&mdash;seu código aparentemente usa tipos do .NET Framework familiares em vez dos tipos de tempo de execução do Windows subjacentes. Por exemplo, você pode usar tipos primitivos do .NET Framework, como **Int32** e **Double**, determinados tipos fundamentais como **DateTimeOffset** e **Uri**, e alguns tipos de interface genérica, como IEnumerable **mais usados&lt;T &gt; ** (IEnumerable (Of T) no Visual Basic) e **IDictionary&lt;TKey, TValue&gt;**. Observe que os argumentos de tipo desses tipos genéricos devem ser tipos do Windows Runtime. Isso é discutido nas seções [do Windows Runtime passando tipos para código gerenciado](#passing-windows-runtime-types-to-managed-code) e [passagem de tipos de tempo de execução do Windows gerenciados](#passing-managed-types-to-the-windows-runtime), neste tópico.

- Classes públicas e interfaces podem conter métodos, propriedades e eventos. Você pode declarar representantes para os eventos ou usar o **EventHandler&lt;T&gt; ** delegar. Uma classe pública ou interface não pode:
    - Ser genérica.
    - Implementar uma interface que não é uma interface de tempo de execução do Windows (no entanto, você pode criar suas próprias interfaces do Windows Runtime e implementá-los).
    - Derive de tipos que não estejam no Windows Runtime, como **System. Exception** e **EventArgs**.

- Todos os tipos públicos devem ter um namespace raiz correspondente ao nome do assembly, e o nome do assembly não deve começar com "Windows".

    > **Dica**. Por padrão, os projetos do Visual Studio têm nomes de namespace correspondentes ao nome do assembly. No Visual Basic, a instrução Namespace desse namespace padrão não é mostrada no código.

- As estruturas públicas não podem ter membros que não sejam campos públicos, e esses campos devem ser tipos de valor ou cadeias de caracteres.
- As classes públicas devem ser **sealed** (**NotInheritable** no Visual Basic). Se o modelo de programação exija polimorfismo, em seguida, você pode criar uma interface pública e implementar essa interface nas classes que devem ser polimórficas.

## <a name="debugging-your-component"></a>Depuração do componente

Se seu aplicativo UWP e o componente sejam compilados com código gerenciado, você pode depurar ambos ao mesmo tempo.

Quando você estiver testando o componente como parte de um aplicativo UWP usando C++, você pode depurar código gerenciado e nativo ao mesmo tempo. O padrão é somente código nativo.

## <a name="to-debug-both-native-c-code-and-managed-code"></a>Para depurar códigos em C++ nativo e gerenciado
1.  Abra o menu de atalho do projeto do Visual C++ e escolha **Propriedades**.
2.  Nas páginas de propriedades, em **Propriedades de Configuração**, escolha **Depuração**.
3.  Escolha **Tipo de Depurador** e, na caixa de listagem suspensa, altere **Native Only** para **Misto (Gerenciado e Nativo)**. Escolha **OK**.
4.  Defina pontos de interrupção nos códigos nativo e gerenciado.

Quando você estiver testando o componente como parte de um aplicativo UWP usando JavaScript, por padrão a solução está no modo de depuração de JavaScript. No Visual Studio, não é possível depurar JavaScript e código gerenciado ao mesmo tempo.

## <a name="to-debug-managed-code-instead-of-javascript"></a>Para depurar código gerenciado em vez de JavaScript
1.  Abra o menu de atalho do projeto JavaScript e escolha **Propriedades**.
2.  Nas páginas de propriedades, em **Propriedades de Configuração**, escolha **Depuração**.
3.  Escolha **Tipo de Depurador** e, na caixa de listagem suspensa, altere **Script Only** para **Managed Only**. Escolha **OK**.
4.  Defina pontos de interrupção no código gerenciado e depure como sempre.

## <a name="passing-windows-runtime-types-to-managed-code"></a>Passagem de tipos do Windows Runtime para código gerenciado
Conforme mencionado anteriormente na seção [declaração de tipos em componentes do Windows Runtime](#declaring-types-in-windows-runtime-components), determinados tipos .NET Framework podem ser exibidos nas assinaturas dos membros de classes públicas. Isso faz parte do suporte que o .NET Framework oferece para habilitar o uso natural do Windows Runtime em código gerenciado. Ele inclui tipos primitivos e algumas classes e interfaces. Quando o componente é usado em JavaScript, ou em código C++, é importante saber como os tipos do .NET Framework são exibidos para o chamador. Consulte [Passo a passo: criação de um componente simples em C# ou Visual Basic e a chamada dele em JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) para obter exemplos com JavaScript. Esta seção aborda os tipos mais usados.

No .NET Framework, tipos primitivos, como a estrutura **Int32** têm muitas propriedades e métodos úteis, como o método **TryParse** . Por outro lado, tipos primitivos e estruturas no Windows Runtime só têm campos. Quando você passa esses tipos para código gerenciado, eles aparentam ser tipos do .NET Framework, e é possível usar as propriedades e os métodos dos tipos do .NET Framework, como faria normalmente. A lista a seguir resume as substituições que são feitas automaticamente no IDE:

-   Para as primitivas do Windows Runtime, **Int32**, **Int64**, **único**, **Double**, **Boolean**, **String** (uma coleção imutável de caracteres Unicode), **Enum**, **UInt32**, **UInt64**e Guid ** **, use o tipo de mesmo nome no namespace System.
-   Para **UInt8**, use **System**.
-   Para **Char16**, use System. **Char**.
-   Para a interface **IInspectable** , use **System. Object**.

Se o c# ou Visual Basic forneça uma palavra-chave de linguagem para qualquer um desses tipos, você pode usar a palavra-chave de linguagem em vez disso.

Além de tipos primitivos, alguns tipos do Windows Runtime básicos mais usados são exibidos em código gerenciado como os equivalentes do .NET Framework. Por exemplo, suponha que seu código JavaScript usa a classe **Windows. Foundation** , e você quiser passá-lo para um método c# ou Visual Basic. O tipo equivalente em código gerenciado é a classe do .NET Framework **System. URI** , e esse é o tipo a ser usado para o parâmetro do método. É possível dizer quando um tipo do Windows Runtime é exibido como um tipo do .NET Framework, porque o IntelliSense no Visual Studio oculta o tipo do Windows Runtime quando você está escrevendo código gerenciado e apresenta o tipo .NET Framework equivalente. (Normalmente, os dois tipos têm o mesmo nome. No entanto, observe que a estrutura do **Windows** aparece em código gerenciado como **DateTimeOffset** e não como **System. DateTime**.)

Para alguns tipos de coleção mais usados, o mapeamento é entre as interfaces implementadas por um tipo do Windows Runtime e as interfaces implementadas pelo tipo .NET Framework correspondente. Assim como acontece com os tipos mencionados acima, você declara tipos de parâmetro usando o tipo .NET Framework. Isso oculta algumas diferenças entre os tipos e deixa a escrita do código .NET Framework mais natural.

A tabela a seguir lista os tipos de interface genérica mais comuns, além de outros mapeamentos de classe e interface comuns. Para obter uma lista completa dos tipos de tempo de execução do Windows que o .NET Framework mapeia, consulte [mapeamentos do .NET Framework dos tipos de tempo de execução do Windows](net-framework-mappings-of-windows-runtime-types.md).

| Windows Runtime                                  | .NET Framework                                    |
|-|-|
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

Quando um tipo implementa mais de uma interface, é possível usar qualquer uma das interfaces implementadas como um tipo de parâmetro ou um tipo de retorno de um membro. Por exemplo, você pode passar ou retornar um **dicionário&lt;int, string&gt; ** (**Dictionary (Of Integer, String)** no Visual Basic) como **IDictionary&lt;int, string&gt;**, IReadOnlyDictionary **&lt;int, string &gt; **, ou **IEnumerable&lt;System.Collections.Generic.KeyValuePair&lt;TKey, TValue&gt;**.

> [!IMPORTANT]
> JavaScript usa a primeira interface exibida na lista de interfaces implementadas por um tipo gerenciado. Por exemplo, se você retornar **dicionário&lt;int, string&gt; ** ao código JavaScript, ele será exibido como **IDictionary&lt;int, string&gt; ** independentemente de qual interface você especificar como o tipo de retorno. Isso significa que, caso a primeira interface não inclua um membro exibido em interfaces posteriores, esse membro não permanece visível para JavaScript.

No Windows Runtime **IMap&lt;K, V&gt; ** e **IMapView&lt;K, V&gt; ** são iterados usando-se IKeyValuePair. Quando você os passa para código gerenciado, eles são exibidos como **IDictionary&lt;TKey, TValue&gt; ** e **IReadOnlyDictionary&lt;TKey, TValue&gt;**, portanto, você naturalmente usa **System.Collections.Generic.KeyValuePair&lt;TKey, TValue&gt; ** para enumerá-los.

A maneira como as interfaces são exibidas em código gerenciado afeta o modo como os tipos que implementam essas interfaces são exibidos. Por exemplo, a classe **PropertySet** implementa **IMap&lt;K, V&gt;**, que é exibido em código gerenciado como **IDictionary&lt;TKey, TValue&gt;**. **PropertySet** é exibido como se tivesse implementado **IDictionary&lt;TKey, TValue&gt; ** em vez de **IMap&lt;K, V&gt;**, portanto, em código gerenciado ele aparenta ter um método **Add** , que se comporta como o método **Add** em dicionários do .NET Framework. Ele não aparenta ter um método **Insert** . Você pode ver este exemplo no tópico [passo a passo: Criando um componente simple em c# ou Visual Basic e chamada dele em JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

## <a name="passing-managed-types-to-the-windows-runtime"></a>Passagem de tipos gerenciados para o Windows Runtime

Conforme abordado na seção anterior, alguns tipos do Windows Runtime podem ser exibidos como tipos do .NET Framework nas assinaturas dos membros do componente ou nas assinaturas dos membros do Windows Runtime quando você os usa no IDE. Quando você passa tipos do .NET Framework para esses membros ou os usa como os valores de retorno dos membros do componente, eles são exibidos no código do outro lado como o tipo do Windows Runtime correspondente. Para obter exemplos dos efeitos que isso pode ter quando o componente é chamado no JavaScript, consulte a seção "Retorno de tipos gerenciados do componente" em [Passo a passo: criação de um componente simples em C# ou Visual Basic e a chamada dele em JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

## <a name="overloaded-methods"></a>Métodos sobrecarregados

No Windows Runtime, os métodos podem ser sobrecarregados. No entanto, se você declarar várias sobrecargas com o mesmo número de parâmetros, você deve aplicar o atributo [**defaultoverloadattribute**](/uwp/api/windows.foundation.metadata.defaultoverloadattribute) a apenas uma dessas sobrecargas. Essa sobrecarga é a única que é possível chamar no JavaScript. Por exemplo, no código a seguir, a sobrecarga que utiliza um **int** (**Integer** em Visual Basic) é a sobrecarga padrão.

```csharp
public string OverloadExample(string s)
{
    return s;
}

[Windows.Foundation.Metadata.DefaultOverload()]
public int OverloadExample(int x)
{
    return x;
}
```

```vb
Public Function OverloadExample(ByVal s As String) As String
    Return s
End Function

<Windows.Foundation.Metadata.DefaultOverload> _
Public Function OverloadExample(ByVal x As Integer) As Integer
    Return x
End Function
```

> [IMPORTANTES] JavaScript permite passar qualquer valor para **OverloadExample**e força o valor para o tipo exigido pelo parâmetro. Você pode chamar **OverloadExample** com "quarenta e dois", "42" ou 42.3, mas todos esses valores são passados para a sobrecarga padrão. A sobrecarga padrão no exemplo anterior retorna 0, 42 e 42, respectivamente.

Você não pode aplicar o atributo e **DefaultOverloadAttribut**para construtores. Todos os construtores em uma classe devem ter números de parâmetros diferentes.

## <a name="implementing-istringable"></a>Implementação de IStringable

A partir do Windows 8.1, o tempo de execução do Windows inclui uma interface **IStringable** cujo único método, **istringable. ToString**, oferece suporte básico de formatação comparável ao oferecido por **ToString**. Se você optar por implementar **IStringable** em um tipo gerenciado público exportado em um componente do tempo de execução do Windows, as seguintes restrições se aplicam:

-   Você pode definir a interface **IStringable** apenas em um relacionamento "classe implementa", como o código em c# a seguir:

    ```cs
    public class NewClass : IStringable
    ```

    Ou o seguinte código do Visual Basic:

    ```vb
    Public Class NewClass : Implements IStringable
    ```

-   Você não pode implementar **IStringable** em uma interface.
-   Você não pode declarar um parâmetro para ser do tipo **IStringable**.
-   **IStringable** não pode ser o tipo de retorno de um método, propriedade ou campo.
-   Você não pode ocultar a implementação **IStringable** das classes base usando uma definição de método como o seguinte:

    ```cs
    public class NewClass : IStringable
    {
       public new string ToString()
       {
          return "New ToString in NewClass";
       }
    }
    ```

    Em vez disso, a implementação de **istringable. ToString** deve sempre substituir a implementação da classe base. Você pode ocultar uma implementação de **ToString** apenas por chamá-lo em uma instância de classe fortemente tipada.

> [!NOTE]
> Em uma variedade de condições, chamadas de código nativo para um tipo gerenciado que implementa **IStringable** ou oculta a implementação de **ToString** podem produzir um comportamento inesperado.

## <a name="asynchronous-operations"></a>Operações assíncronas

Para implementar um método assíncrono no componente, adicione "Async" ao final do nome do método e retorne uma das interfaces do Windows Runtime que representam ações ou operações assíncronas: **IAsyncAction**, IAsyncActionWithProgress **&lt; TProgress&gt;**, **IAsyncOperation&lt;TResult&gt;**, ou **IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**.

Você pode usar tarefas do .NET Framework (a classe de [**tarefa**](/dotnet/api/system.threading.tasks.task) e genérico [**tarefa&lt;TResult&gt; **](/dotnet/api/system.threading.tasks.task-1) classe) para implementar o método assíncrono. Você deve retornar uma tarefa que represente uma operação contínua, como uma tarefa que é retornada de um método assíncrono escrito em c# ou Visual Basic ou uma tarefa que é retornada do método [**Run**](/dotnet/api/system.threading.tasks.task.run) . Caso use um construtor para criar a tarefa, você deve chamar o método [Task.Start](/dotnet/api/system.threading.tasks.task.start) antes de devolvê-lo.

Um método que usa `await` (`Await` no Visual Basic) requer o `async` palavra-chave (`Async` no Visual Basic). Caso você exponha um método assim em um componente do tempo de execução do Windows, aplique a `async` palavra-chave para o representante que você passa para o método **Run** .

Para ações e operações assíncronas que não dão suporte ao cancelamento ou aos relatórios de progresso, é possível usar o método de extensão [WindowsRuntimeSystemExtensions.AsAsyncAction](https://msdn.microsoft.com/library/system.windowsruntimesystemextensions.asasyncaction.aspx) ou [AsAsyncOperation&lt;TResult&gt;](https://msdn.microsoft.com/library/hh779745.aspx) para encapsular a tarefa na interface apropriada. Por exemplo, o código a seguir implementa um método assíncrono usando o **Task. Run&lt;TResult&gt; ** método para iniciar uma tarefa. O **AsAsyncOperation&lt;TResult&gt; ** método de extensão retorna a tarefa como uma operação assíncrona do tempo de execução do Windows.

```csharp
public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
{
    return Task.Run<IList<string>>(async () =>
    {
        var data = await DownloadDataAsync(id);
        return ExtractStrings(data);
    }).AsAsyncOperation();
}
```

```vb
Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
     As IAsyncOperation(Of IList(Of String))

    Return Task.Run(Of IList(Of String))(
        Async Function()
            Dim data = Await DownloadDataAsync(id)
            Return ExtractStrings(data)
        End Function).AsAsyncOperation()
End Function
```

O código JavaScript a seguir mostra como o método pode ser chamado usando um objeto [**Promise**](https://msdn.microsoft.com/library/windows/apps/br211867.aspx) . A função passada para o método then é executada quando a chamada assíncrona é concluída. O parâmetro stringList contém a lista de cadeias de caracteres que é retornada pelo método **DownloadAsStringAsync** , e a função faz o processamento necessário.

```javascript
function asyncExample(id) {

    var result = SampleComponent.Example.downloadAsStringAsync(id).then(
        function (stringList) {
            // Place code that uses the returned list of strings here.
        });
}
```

Para ações e operações assíncronas que dão suporte ao cancelamento ou aos relatórios de progresso, use a classe [**AsyncInfo**](/dotnet/api/system.runtime.interopservices.windowsruntime) para gerar uma tarefa iniciada e vincular os recursos da tarefa com o progresso e cancelamento de relatórios de progresso e cancelamento recursos da interface do Windows Runtime apropriada de relatório. Para obter um exemplo que dê suporte ao cancelamento e aos relatórios de progresso, consulte [Passo a passo: criação de um componente simples em C# ou Visual Basic e a chamada dele em JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

Observe que você pode usar os métodos da classe **AsyncInfo** mesmo que o método assíncrono não oferece suporte ao cancelamento ou aos relatórios de progresso. Se você usar uma função lambda do Visual Basic ou um método anônimo de c#, não forneça parâmetros para o token e [**IProgress&lt;T&gt; **](https://msdn.microsoft.com/library/hh138298.aspx) interface. Caso você use uma função lambda de C#, forneça um parâmetro token, mas o ignore. O exemplo anterior, que usou o AsAsyncOperation&lt;TResult&gt; método, esta aparência quando você usa o [**asyncinfo. Run&lt;TResult&gt;(Func&lt;CancellationToken, Task&lt;TResult&gt;**](https://msdn.microsoft.com/library/hh779740.aspx)) método sobrecarga em vez disso.

```csharp
public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
{
    return AsyncInfo.Run<IList<string>>(async (token) =>
    {
        var data = await DownloadDataAsync(id);
        return ExtractStrings(data);
    });
}
```

```vb
Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
    As IAsyncOperation(Of IList(Of String))

    Return AsyncInfo.Run(Of IList(Of String))(
        Async Function()
            Dim data = Await DownloadDataAsync(id)
            Return ExtractStrings(data)
        End Function)
End Function
```

Se você criar um método assíncrono que suporte ao cancelamento ou aos relatórios de progresso, considere a adição de sobrecargas que não tenham parâmetros para um token de cancelamento ou a **IProgress&lt;T&gt; ** interface.

## <a name="throwing-exceptions"></a>Acionamento de exceções

É possível acionar qualquer tipo de exceção que esteja incluído no .NET para aplicativos do Windows. Não é possível declarar os próprios tipos de exceção pública em um componente do Tempo de Execução do Windows, mas você pode declarar e acionar tipos não públicos.

Caso o componente não manipule a exceção, uma exceção correspondente é acionada no código que chamou o componente. A maneira como a exceção é exibida para o chamador depende da maneira como a linguagem de chamada dá suporte ao Windows Runtime.

-   Em JavaScript, a exceção é exibida como um objeto no qual a mensagem de exceção é substituída por um rastreamento de pilha. Ao depurar o aplicativo no Visual Studio, você pode ver o texto da mensagem original exibido na caixa de diálogo de exceção do depurador, identificado como "Informações de WinRT". Não é possível acessar o texto da mensagem original no código JavaScript.

    > **Dica**.Atualmente, o rastreamento de pilha contém o tipo de exceção gerenciada, mas não recomendamos analisar o rastreamento para identificar o tipo de exceção. Em vez disso, use um valor HRESULT conforme descrito mais à frente nesta seção.

-   Em C++, a exceção é exibida como uma exceção da plataforma. Se a propriedade HResult da exceção gerenciada pode ser mapeada para o HRESULT de uma exceção de plataforma específica, a exceção específica é usada; Caso contrário, será gerada uma exceção [**Platform:: COMException**](https://msdn.microsoft.com/library/windows/apps/xaml/hh710414.aspx) . O texto da mensagem da exceção gerenciada não está disponível para o código C++. Caso uma exceção de plataforma específica seja acionada, o texto da mensagem padrão desse tipo de exceção é exibido; do contrário, nenhum texto de mensagem é exibido. Consulte [Exceções (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699896.aspx).
-   Em C# ou Visual Basic, a exceção é uma exceção gerenciada normal.

Ao acionar uma exceção no componente, você pode facilitar para um chamador JavaScript ou C++ lidar com a exceção acionando um tipo de exceção não público cujo valor da propriedade HResult é específico do componente. O HRESULT está disponível para um chamador JavaScript por meio da propriedade de número do objeto de exceção e para um chamador C++ por meio da propriedade [**COMException:: HRESULT**](https://msdn.microsoft.com/library/windows/apps/xaml/hh710415.aspx) .

> [!NOTE]
> Use um valor negativo para o HRESULT. Um valor positivo é interpretado como êxito, e nenhuma exceção é acionada no chamador JavaScript ou C++.

## <a name="declaring-and-raising-events"></a>Declaração e acionamento de eventos

Quando você declara um tipo para manter os dados do evento, derive de Object, em vez de EventArgs, porque EventArgs não é um tipo do Windows Runtime. Use [**EventHandler&lt;TEventArgs&gt; **](https://msdn.microsoft.com/library/db0etb8x.aspx) como o tipo de evento e use o argumento do evento tipo como o argumento de tipo genérico. Acione o evento, assim como você faria em um aplicativo do .NET Framework.

Quando o componente do Tempo de Execução do Windows é usado em JavaScript ou C++, o evento segue o padrão de evento do Windows Runtime que essas linguagens esperam. Quando você usa o componente de C# ou Visual Basic, o evento é exibido como um evento do .NET Framework comum. Um exemplo é dado em [Passo a passo: criação de um componente simples em C# ou Visual Basic e a chamada dele em JavaScript]().

Caso implemente acessadores de eventos personalizado (declarar um evento com a palavra-chave **Custom**, em Visual Basic), você deve seguir o padrão de evento do Windows Runtime na implementação. Consulte [Eventos personalizados e acessadores de evento em componentes do Windows Runtime](custom-events-and-event-accessors-in-windows-runtime-components.md). Quando você manipula o evento no código C# ou Visual Basic, ele ainda parece ser um evento do .NET Framework comum.

## <a name="next-steps"></a>Próximas etapas

Depois que tiver criado um componente do Tempo de Execução do Windows para o próprio uso, talvez você ache que a funcionalidade que ele encapsula é útil para outros desenvolvedores. Você tem duas opções para empacotar um componente para distribuição a outros desenvolvedores. Consulte [Distribuição de um componente do Tempo de Execução do Windows gerenciado](https://msdn.microsoft.com/library/jj614475.aspx).

Para saber mais sobre recursos da linguagem Visual Basic e C# e o suporte do .NET Framework ao Windows Runtime, consulte a [Referência das linguagens Visual Basic e C#](https://msdn.microsoft.com/library/windows/apps/xaml/br212458.aspx).

## <a name="related-topics"></a>Tópicos relacionados
* [.NET para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/mt185501)
* [Passo a passo: Criando um componente do Tempo de Execução do Windows simples e chamando-o em JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
