---
title: Componentes do Windows Runtime com C# e Visual Basic
description: A partir do .NET 4,5, você pode usar código gerenciado para criar seus próprios tipos de Windows Runtime, empacotados em um componente Windows Runtime.
ms.assetid: A5672966-74DF-40AB-B01E-01E3FCD0AD7A
ms.date: 12/04/2018
ms.topic: article
dev_langs:
- csharp
- vb
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c402b8e4ba98f55267a42c1bce1c16e6f090e80c
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340372"
---
# <a name="windows-runtime-components-with-c-and-visual-basic"></a>Componentes do Windows Runtime com C# e Visual Basic

Você pode usar código gerenciado para criar seus próprios tipos de Windows Runtime e empacotá-los em um componente Windows Runtime. Você pode usar seu componente em aplicativos Plataforma Universal do Windows (UWP) escritos em C++, JavaScript, Visual Basic ou. C# Este tópico descreve as regras para a criação de um componente e discute alguns aspectos do suporte do .NET ao Windows Runtime. Em geral, esse suporte foi projetado para ser transparente para o programador do .NET. No entanto, ao criar um componente a ser usado com JavaScript ou C++, você precisa estar ciente das diferenças na maneira como essas linguagens dão suporte ao Windows Runtime.

Se você estiver criando um componente para uso somente em aplicativos UWP gravados em Visual Basic ou C#, e o componente não contiver controles UWP, então, em vez de usar o modelo de biblioteca de **classes** , e não o modelo de projeto de **componente Windows Runtime** no Microsoft Visual Studio. Existem menos restrições em uma biblioteca de classes simples.

## <a name="declaring-types-in-windows-runtime-components"></a>Declarando tipos em componentes Windows Runtime

Internamente, os tipos de Windows Runtime em seu componente podem usar qualquer funcionalidade .NET que seja permitida em um aplicativo UWP. Para obter mais informações, consulte [.net para aplicativos UWP](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0).

Externamente, os membros de seus tipos podem expor apenas tipos Windows Runtime para seus parâmetros e valores de retorno. A lista a seguir descreve as limitações em tipos .NET que são expostas de um componente Windows Runtime.

- Os campos, os parâmetros e os valores de retorno de todos os tipos públicos e membros no componente devem ser tipos do Windows Runtime. Essa restrição inclui os tipos de Windows Runtime que você cria, bem como os tipos que são fornecidos pelo Windows Runtime em si. Ele também inclui vários tipos .NET. A inclusão desses tipos faz parte do suporte que o .NET fornece para habilitar o uso natural do Windows Runtime em código gerenciado&mdash;seu código parece usar tipos .NET conhecidos em vez dos tipos de Windows Runtime subjacentes. Por exemplo, você pode usar tipos primitivos do .NET, como **Int32** e **Double**, certos tipos fundamentais, como **DateTimeOffset** e **URI**, e alguns tipos de interface genérica comumente usados como **IEnumerable&lt;t&gt;** (ienumerable (Of t) em Visual Basic) e **IDictionary&lt;TKey, TValue&gt;** . Observe que os argumentos de tipo desses tipos genéricos devem ser tipos Windows Runtime. Isso é discutido nas seções que [passam tipos de Windows Runtime para código gerenciado](#passing-windows-runtime-types-to-managed-code) e [passando tipos gerenciados para o Windows Runtime](#passing-managed-types-to-the-windows-runtime), mais adiante neste tópico.

- Classes públicas e interfaces podem conter métodos, propriedades e eventos. Você pode declarar delegados para seus eventos ou usar o delegado **EventHandler&lt;t&gt;** . Uma classe ou interface pública não pode:
    - Ser genérica.
    - Implementar uma interface que não seja uma interface Windows Runtime (no entanto, você pode criar suas próprias interfaces de Windows Runtime e implementá-las).
    - Derive de tipos que não estão no Windows Runtime, como **System. Exception** e **System. EventArgs**.

- Todos os tipos públicos devem ter um namespace raiz correspondente ao nome do assembly, e o nome do assembly não deve começar com "Windows".

    > **Dica**. Por padrão, os projetos do Visual Studio têm nomes de namespace que correspondem ao nome do assembly. No Visual Basic, a instrução Namespace desse namespace padrão não é mostrada no código.

- As estruturas públicas não podem ter membros que não sejam campos públicos, e esses campos devem ser tipos de valor ou cadeias de caracteres.
- As classes públicas devem ser **sealed** (**NotInheritable** no Visual Basic). Se seu modelo de programação exigir polimorfismo, você poderá criar uma interface pública e implementar essa interface nas classes que devem ser polimórficas.

## <a name="debugging-your-component"></a>Depuração do componente

Se seu aplicativo UWP e seu componente forem criados com código gerenciado, você poderá depurá-los ao mesmo tempo.

Quando você estiver testando seu componente como parte de um aplicativo UWP C++usando, poderá depurar código gerenciado e nativo ao mesmo tempo. O padrão é somente código nativo.

## <a name="to-debug-both-native-c-code-and-managed-code"></a>Para depurar códigos em C++ nativo e gerenciado
1.  Abra o menu de atalho do projeto do Visual C++ e escolha **Propriedades**.
2.  Nas páginas de propriedades, em **Propriedades de Configuração**, escolha **Depuração**.
3.  Escolha **Tipo de Depurador** e, na caixa de listagem suspensa, altere **Native Only** para **Misto (Gerenciado e Nativo)** . Escolha **OK**.
4.  Defina pontos de interrupção nos códigos nativo e gerenciado.

Quando você estiver testando seu componente como parte de um aplicativo UWP usando JavaScript, por padrão, a solução estará no modo de depuração do JavaScript. No Visual Studio, não é possível depurar JavaScript e código gerenciado ao mesmo tempo.

## <a name="to-debug-managed-code-instead-of-javascript"></a>Para depurar código gerenciado em vez de JavaScript
1.  Abra o menu de atalho do projeto JavaScript e escolha **Propriedades**.
2.  Nas páginas de propriedades, em **Propriedades de Configuração**, escolha **Depuração**.
3.  Escolha **Tipo de Depurador** e, na caixa de listagem suspensa, altere **Script Only** para **Managed Only**. Escolha **OK**.
4.  Defina pontos de interrupção no código gerenciado e depure como sempre.

## <a name="passing-windows-runtime-types-to-managed-code"></a>Passagem de tipos do Windows Runtime para código gerenciado
Conforme mencionado anteriormente na seção [declarando tipos em componentes Windows Runtime](#declaring-types-in-windows-runtime-components), determinados tipos .net podem aparecer nas assinaturas de membros de classes públicas. Isso faz parte do suporte que o .NET fornece para habilitar o uso natural do Windows Runtime em código gerenciado. Ele inclui tipos primitivos e algumas classes e interfaces. Quando o componente é usado do JavaScript ou do C++ código, é importante saber como seus tipos .NET aparecem para o chamador. Consulte [a explicação sobre como C# criar um componente ou Visual Basic Windows Runtime e chamá-lo do JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) para obter exemplos com JavaScript. Esta seção aborda os tipos mais usados.

No .NET, tipos primitivos como a estrutura **Int32** têm muitas propriedades e métodos úteis, como o método **TryParse** . Por outro lado, tipos primitivos e estruturas no Windows Runtime só têm campos. Quando você passa esses tipos para código gerenciado, eles parecem ser tipos .NET, e você pode usar as propriedades e métodos de tipos .NET como faria normalmente. A lista a seguir resume as substituições que são feitas automaticamente no IDE:

-   Para os primitivos de Windows Runtime **Int32**, **Int64**, **single**, **Double**, **Boolean**, **String** (uma coleção imutável de caracteres Unicode), **enum**, **UInt32**, **UInt64**e **GUID**, use o tipo de mesmo nome no namespace do sistema.
-   Para **uint8**, use **System. byte**.
-   Para **Char16**, use **System. Char**.
-   Para a interface **IInspectable** , use **System. Object**.

Se C# ou Visual Basic fornecer uma palavra-chave Language para qualquer um desses tipos, você poderá usar a palavra-chave Language em vez disso.

Além dos tipos primitivos, alguns tipos de Windows Runtime básicos, comumente usados, aparecem no código gerenciado como seus equivalentes em .NET. Por exemplo, suponha que seu código JavaScript use a classe **Windows. Foundation. URI** e você queira passá-la para um C# método ou Visual Basic. O tipo equivalente no código gerenciado é a classe **System. URI** do .net, e esse é o tipo a ser usado para o parâmetro do método. Você pode saber quando um tipo de Windows Runtime aparece como um tipo .NET, porque o IntelliSense no Visual Studio oculta o tipo de Windows Runtime quando você está escrevendo código gerenciado e apresenta o tipo .NET equivalente. (Normalmente, os dois tipos têm o mesmo nome. No entanto, observe que a estrutura **Windows. Foundation. DateTime** aparece em código gerenciado como **System. DateTimeOffset** e não como **System. DateTime**.)

Para alguns tipos de coleção usados com frequência, o mapeamento é entre as interfaces que são implementadas por um tipo de Windows Runtime e as interfaces implementadas pelo tipo .NET correspondente. Assim como nos tipos mencionados acima, você declara os tipos de parâmetro usando o tipo .NET. Isso oculta algumas diferenças entre os tipos e torna a escrita de código .NET mais natural.

A tabela a seguir lista os tipos de interface genérica mais comuns, além de outros mapeamentos de classe e interface comuns. Para obter uma lista completa de tipos de Windows Runtime que o .NET mapeia, consulte [mapeamentos do .net de tipos de Windows Runtime](net-framework-mappings-of-windows-runtime-types.md).

| Windows Runtime                                  | .NET                                    |
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

Quando um tipo implementa mais de uma interface, é possível usar qualquer uma das interfaces implementadas como um tipo de parâmetro ou um tipo de retorno de um membro. Por exemplo, você pode passar ou retornar um **dicionário&lt;int, Cadeia de caracteres&gt;** (**dicionário (Of Integer, string)** em Visual Basic) como **IDictionary&lt;int, String&gt;** , **IReadOnlyDictionary&lt;int, String&gt;** ou **IEnumerable&lt;System. Collections. Generic. KeyValuePair&lt;TKey, TValue&gt;&gt;** .

> [!IMPORTANT]
> O JavaScript usa a interface que aparece primeiro na lista de interfaces implementadas por um tipo gerenciado. Por exemplo, se você retornar **Dictionary&lt;int, string&gt;** ao código JavaScript, ele aparecerá como **IDictionary&lt;int, String&gt;** não importa qual interface você especifica como o tipo de retorno. Isso significa que, se a primeira interface não incluir um membro exibido em interfaces posteriores, esse membro não permanecerá visível para JavaScript.

No Windows Runtime, o **IMap&lt;k, V&gt;** e **IMapView&lt;K, v&gt;** são iterados usando IKeyValuePair. Ao passá-los para o código gerenciado, eles aparecem como **IDictionary&lt;TKey, TValue&gt;** e **IReadOnlyDictionary&lt;tkey, TValue&gt;** , naturalmente, você usa **System. Collections. Generic. KeyValuePair&lt;TKey, TValue&gt;** para enumerá-los.

A maneira como as interfaces são exibidas em código gerenciado afeta o modo como os tipos que implementam essas interfaces são exibidos. Por exemplo, a classe **PropertySet** implementa o **IMap&lt;K, V&gt;** , que aparece em código gerenciado como **IDictionary&lt;TKey, TValue&gt;** . **PropertySet** aparece como se ele implementasse **IDictionary&lt;TKey, TValue&gt;** em vez de **IMap&lt;K, V&gt;** , portanto, em código gerenciado, parece ter um método **Add** , que se comporta como o método **Add** em dicionários .net. Parece que não há um método **Insert** . Você pode ver este exemplo no tópico [Walkthrough, criando C# um ou Visual Basic Windows Runtime componente e chamando-o a partir do JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

## <a name="passing-managed-types-to-the-windows-runtime"></a>Passagem de tipos gerenciados para o Windows Runtime

Conforme discutido na seção anterior, alguns tipos de Windows Runtime podem aparecer como tipos .NET nas assinaturas dos membros do componente ou nas assinaturas de membros de Windows Runtime ao usá-los no IDE. Quando você passa os tipos do .NET para esses membros ou os usa como os valores de retorno dos membros do componente, eles aparecem para o código no outro lado como o tipo de Windows Runtime correspondente. Para obter exemplos dos efeitos que isso pode ter quando seu componente é chamado a partir do JavaScript, consulte a seção "retornando tipos gerenciados do seu componente" em [explicação sobre como criar C# um ou Visual Basic Windows Runtime componente e chamá-lo do JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

## <a name="overloaded-methods"></a>Métodos sobrecarregados

No Windows Runtime, os métodos podem ser sobrecarregados. No entanto, se você declarar várias sobrecargas com o mesmo número de parâmetros, deverá aplicar o atributo [**Windows. Foundation. Metadata. Defaultoverloadattribute**](/uwp/api/windows.foundation.metadata.defaultoverloadattribute) a apenas uma dessas sobrecargas. Essa sobrecarga é a única que é possível chamar no JavaScript. Por exemplo, no código a seguir, a sobrecarga que utiliza um **int** (**Integer** em Visual Basic) é a sobrecarga padrão.

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

> FUNDAMENTAL O JavaScript permite passar qualquer valor para **OverloadExample**e impõe o valor para o tipo exigido pelo parâmetro. Você pode chamar **OverloadExample** com "42", "42" ou 42,3, mas todos esses valores são passados para a sobrecarga padrão. A sobrecarga padrão no exemplo anterior retorna 0, 42 e 42, respectivamente.

Não é possível aplicar o atributo **DefaultOverloadAttribut**e aos construtores. Todos os construtores em uma classe devem ter números de parâmetros diferentes.

## <a name="implementing-istringable"></a>Implementação de IStringable

A partir do Windows 8.1, o Windows Runtime inclui uma interface **isastringable** cujo único método, **isastringable. ToString**, fornece suporte de formatação básica comparável ao fornecido pelo **Object. ToString**. Se você optar por implementar **isastringable** em um tipo gerenciado público que é exportado em um componente Windows Runtime, as seguintes restrições se aplicarão:

-   Você pode definir a interface **isastringable** somente em uma relação de "classe implementa", como o código a seguir C#em:

    ```cs
    public class NewClass : IStringable
    ```

    Ou o seguinte código do Visual Basic:

    ```vb
    Public Class NewClass : Implements IStringable
    ```

-   Não é possível implementar **isastringable** em uma interface.
-   Você não pode declarar um parâmetro para ser do tipo **isastringable**.
-   **Isastringable** não pode ser o tipo de retorno de um método, propriedade ou campo.
-   Não é possível ocultar sua implementação **isastringable** de classes base usando uma definição de método como a seguinte:

    ```cs
    public class NewClass : IStringable
    {
       public new string ToString()
       {
          return "New ToString in NewClass";
       }
    }
    ```

    Em vez disso, a implementação **isastringable. ToString** sempre deve substituir a implementação da classe base. Você pode ocultar uma implementação **ToString** apenas invocando-a em uma instância de classe com rigidez de tipos.

> [!NOTE]
> Sob uma variedade de condições, o chama de código nativo para um tipo gerenciado que implementa **isastringable** ou oculta sua implementação **ToString** pode produzir um comportamento inesperado.

## <a name="asynchronous-operations"></a>Operações assíncronas

Para implementar um método assíncrono em seu componente, adicione "Async" ao final do nome do método e retorne uma das interfaces de Windows Runtime que representam ações ou operações assíncronas: **IAsyncAction**, **IAsyncActionWithProgress&lt;TProgress&gt;** , **IAsyncOperation&lt;TResult&gt;** ou **IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** .

Você pode usar tarefas do .NET (a classe [**Task**](/dotnet/api/system.threading.tasks.task) e a tarefa genérica&lt;a classe [**TResult&gt;** ](/dotnet/api/system.threading.tasks.task-1) ) para implementar o método assíncrono. Você deve retornar uma tarefa que representa uma operação em andamento, como uma tarefa retornada por um método assíncrono gravado no C# ou Visual Basic, ou uma tarefa retornada pelo método [**Task. Run**](/dotnet/api/system.threading.tasks.task.run) . Caso use um construtor para criar a tarefa, você deve chamar o método [Task.Start](/dotnet/api/system.threading.tasks.task.start) antes de devolvê-lo.

Um método que usa `await` (`Await` em Visual Basic) requer a palavra-chave `async` (`Async` em Visual Basic). Se você expor tal método de um componente Windows Runtime, aplique a palavra-chave `async` ao delegado que você passa para o método **Run** .

Para ações e operações assíncronas que não dão suporte ao cancelamento ou aos relatórios de progresso, é possível usar o método de extensão [WindowsRuntimeSystemExtensions.AsAsyncAction](https://docs.microsoft.com/dotnet/api/system) ou [AsAsyncOperation&lt;TResult&gt;](https://docs.microsoft.com/dotnet/api/system) para encapsular a tarefa na interface apropriada. Por exemplo, o código a seguir implementa um método assíncrono usando a **tarefa. execute&lt;** método de&gt;de TResult para iniciar uma tarefa. O método de extensão **asasyncoperation&lt;TResult&gt;** retorna a tarefa como uma Windows Runtime operação assíncrona.

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

O código JavaScript a seguir mostra como o método pode ser chamado usando um objeto [**WinJS. Promise**](https://docs.microsoft.com/previous-versions/windows/apps/br211867(v=win.10)) . A função passada para o método then é executada quando a chamada assíncrona é concluída. O parâmetro StringList contém a lista de cadeias de caracteres que é retornada pelo método **DownloadAsStringAsync** e a função faz qualquer processamento necessário.

```javascript
function asyncExample(id) {

    var result = SampleComponent.Example.downloadAsStringAsync(id).then(
        function (stringList) {
            // Place code that uses the returned list of strings here.
        });
}
```

Para ações assíncronas e operações que dão suporte ao cancelamento ou relatório de andamento, use a classe [**AsyncInfo**](/dotnet/api/system.runtime.interopservices.windowsruntime) para gerar uma tarefa iniciada e para conectar os recursos de cancelamento e relatório de andamento da tarefa com os recursos de relatório de cancelamento e progresso da interface de Windows Runtime apropriada. Para obter um exemplo que ofereça suporte ao cancelamento e ao relatório de progresso, consulte [passo a passos de criando um ou Visual Basic componente Windows Runtime e chamando-o C# do JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

Observe que você pode usar os métodos da classe **AsyncInfo** mesmo que seu método assíncrono não ofereça suporte a relatórios de cancelamento ou progresso. Se você usar uma função lambda Visual Basic ou um C# método anônimo, não forneça parâmetros para o token e a interface [**IProgress&lt;t&gt;** ](https://docs.microsoft.com/dotnet/api/system.iprogress-1) . Caso você use uma função lambda de C#, forneça um parâmetro token, mas o ignore. O exemplo anterior, que usou o método asasyncoperation&lt;TResult&gt;, tem a seguinte aparência quando você usa o AsyncInfo. Execute a sobrecarga do método [ **&lt;tresult&gt;(Func&lt;CancellationToken, Task&lt;TResult&gt;&gt;** ](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime)) em vez disso.

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

Se você criar um método assíncrono que opcionalmente dá suporte ao cancelamento ou ao relatório de andamento, considere adicionar sobrecargas que não têm parâmetros para um token de cancelamento ou a interface **IProgress&lt;t&gt;** .

## <a name="throwing-exceptions"></a>Acionamento de exceções

É possível acionar qualquer tipo de exceção que esteja incluído no .NET para aplicativos do Windows. Não é possível declarar os próprios tipos de exceção pública em um componente do Windows Runtime, mas você pode declarar e acionar tipos não públicos.

Caso o componente não manipule a exceção, uma exceção correspondente é acionada no código que chamou o componente. A maneira como a exceção é exibida para o chamador depende da maneira como a linguagem de chamada dá suporte ao Windows Runtime.

-   Em JavaScript, a exceção é exibida como um objeto no qual a mensagem de exceção é substituída por um rastreamento de pilha. Ao depurar o aplicativo no Visual Studio, você pode ver o texto da mensagem original exibido na caixa de diálogo de exceção do depurador, identificado como "Informações de WinRT". Não é possível acessar o texto da mensagem original no código JavaScript.

    > **Dica**. Atualmente, o rastreamento de pilha contém o tipo de exceção gerenciada, mas não recomendamos a análise do rastreamento para identificar o tipo de exceção. Em vez disso, use um valor HRESULT conforme descrito mais à frente nesta seção.

-   Em C++, a exceção é exibida como uma exceção da plataforma. Se a propriedade HResult da exceção gerenciada puder ser mapeada para o HRESULT de uma exceção de plataforma específica, a exceção específica será usada; caso contrário, uma exceção [**Platform:: COMException**](https://docs.microsoft.com/cpp/cppcx/platform-comexception-class) será lançada. O texto da mensagem da exceção gerenciada não está disponível para o código C++. Caso uma exceção de plataforma específica seja acionada, o texto da mensagem padrão desse tipo de exceção é exibido; do contrário, nenhum texto de mensagem é exibido. Consulte [Exceções (C++/CX)](https://docs.microsoft.com/cpp/cppcx/exceptions-c-cx).
-   Em C# ou Visual Basic, a exceção é uma exceção gerenciada normal.

Ao acionar uma exceção no componente, você pode facilitar para um chamador JavaScript ou C++ lidar com a exceção acionando um tipo de exceção não público cujo valor da propriedade HResult é específico do componente. O HRESULT está disponível para um chamador do JavaScript por meio da propriedade Number do objeto de exceção e C++ para um chamador por meio da propriedade [**COMException:: HRESULT**](https://docs.microsoft.com/cpp/cppcx/platform-comexception-class#hresult) .

> [!NOTE]
> Use um valor negativo para o HRESULT. Um valor positivo é interpretado como êxito, e nenhuma exceção é acionada no chamador JavaScript ou C++.

## <a name="declaring-and-raising-events"></a>Declaração e acionamento de eventos

Quando você declara um tipo para manter os dados do evento, derive de Object, em vez de EventArgs, porque EventArgs não é um tipo do Windows Runtime. Use [**EventHandler&lt;TEventArgs&gt;** ](https://docs.microsoft.com/dotnet/api/system.eventhandler-1) como o tipo do evento e use o tipo de argumento de evento como o argumento de tipo genérico. Gere o evento exatamente como você faria em um aplicativo .NET.

Quando o componente do Tempo de Execução do Windows é usado em JavaScript ou C++, o evento segue o padrão de evento do Windows Runtime que essas linguagens esperam. Quando você usa o componente do C# ou Visual Basic, o evento aparece como um evento comum do .net. Um exemplo é fornecido em [passo a passos de C# criação de um componente do ou Visual Basic Windows Runtime e sua chamada a partir do JavaScript](/windows/uwp/winrt-components/walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript).

Caso implemente acessadores de eventos personalizado (declarar um evento com a palavra-chave **Custom**, em Visual Basic), você deve seguir o padrão de evento do Windows Runtime na implementação. Consulte [eventos personalizados e acessadores de eventos em componentes do Windows Runtime](custom-events-and-event-accessors-in-windows-runtime-components.md). Observe que quando você manipula o evento do C# ou Visual Basic código, ele ainda parece ser um evento comum do .net.

## <a name="next-steps"></a>Próximas etapas

Depois que tiver criado um componente do Windows Runtime para o próprio uso, talvez você ache que a funcionalidade que ele encapsula é útil para outros desenvolvedores. Você tem duas opções para empacotar um componente para distribuição a outros desenvolvedores. Consulte [Distribuição de um componente do Windows Runtime gerenciado](https://docs.microsoft.com/previous-versions/windows/apps/jj614475(v=vs.140)).

Para obter mais informações sobre os C# recursos de Visual Basic e linguagem e o suporte do .net para o Windows Runtime, consulte [referência de C# linguagem e Visual Basic](https://docs.microsoft.com/visualstudio/welcome-to-visual-studio-2015?view=vs-2015).

## <a name="related-topics"></a>Tópicos relacionados
* [.NET para aplicativos UWP](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)
* [Passo a passo para criar um componente do Windows Runtime em C# ou Visual Basic e chamá-lo do JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
