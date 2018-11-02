---
author: msatranjr
title: Criando um componente do Tempo de Execução do Windows simples e chamando-o em JavaScript
description: Este passo a passo mostra como é possível usar o .NET Framework com Visual Basic ou C# para criar os próprios tipos de Windows Runtime, empacotados em um componente do Tempo de Execução do Windows, e como chamar o componente no aplicativo Universal do Windows em JavaScript para Windows.
ms.assetid: 1565D86C-BF89-4EF3-81FE-35367DB8D671
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 49b9fe0833151155b11b7d7b796e395bb6a2ca7f
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5974723"
---
# <a name="walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript"></a>Procedimento passo a passo: criando um componente simples do Tempo de Execução do Windows e chamando-o em JavaScript




Este passo a passo mostra como é possível usar o .NET Framework com Visual Basic ou C# para criar os próprios tipos de Windows Runtime, empacotados em um componente do Tempo de Execução do Windows, e como chamar o componente no aplicativo Universal do Windows em JavaScript para Windows.

O Visual Studio facilita adicionar um componente do Tempo de Execução do Windows escrito em C# ou Visual Basic ao aplicativo e criar tipos de Tempo de Execução do Windows que é possível chamar em JavaScript. Internamente, os tipos de Tempo de Execução do Windows podem usar qualquer funcionalidade do .NET Framework permitida em um aplicativo Universal do Windows. (Para obter mais informações, consulte [Criando componentes do tempo de execução do Windows em c# e Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) e [.NET para visão geral de aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx).) Externamente, os membros do seu tipo podem expor somente tipos de tempo de execução do Windows para os parâmetros e valores de retorno. Quando você compila a solução, o Visual Studio compila o projeto do componente do Tempo de Execução do Windows do .NET Framework e executa uma etapa de compilação que cria um arquivo de metadados do Windows (. winmd). Trata-se do componente do Tempo de Execução do Windows, que inclui o Visual Studio no aplicativo.

> **Observação**.NET Framework mapeia automaticamente alguns tipos do .NET Framework mais usados, como tipos de dados primitivos e tipos de coleção, para seus equivalentes de tempo de execução do Windows. Esses tipos do .NET Framework podem ser usados na interface pública de um componente do Tempo de Execução do Windows e serão exibidos para usuários do componente como os tipos de Tempo de Execução do Windows correspondentes. Consulte [Criação de componentes do Tempo de Execução do Windows em C# ou Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md).

Este passo a passo ilustra as tarefas a seguir. Depois que tiver concluído a primeira seção, que configura o aplicativo do Windows com JavaScript, você poderá concluir as seções restantes em qualquer ordem.

## <a name="prerequisites"></a>Pré-requisitos:

-   Windows 10
-   Microsoft Visual Studio2015 ou Microsoft Visual Studio Community2015

## <a name="creating-a-simple-windows-runtime-class"></a>Criação de uma classe de Tempo de Execução do Windows simples


Esta seção cria um aplicativo Universal do Windows compilado para Windows usando JavaScript e adiciona um projeto de componente do Tempo de Execução do Windows em Visual Basic ou C#. Ela mostra como definir um tipo de Tempo de Execução do Windows gerenciado, criar uma instância do tipo em JavaScript e chamar membros estáticos e de instância. A exibição visual do aplicativo de exemplo é deliberadamente fosca para manter o foco no componente. Sinta-se à vontade para deixá-la mais bonita.

1.  No Visual Studio, crie um novo projeto JavaScript: na barra de menus, escolha **Arquivo, Novo, Projeto**. Na seção **Modelos Instalados** da caixa de diálogo **Novo Projeto**, escolha **JavaScript**, **Windows** e **Universal**. (Se o Windows não estiver disponível, certifique se você está usando o Windows 8 ou posterior). Escolha o modelo **Aplicativo em Branco**e digite SampleApp para o nome do projeto.
2.  Crie o projeto de componente: no Gerenciador de Soluções, abra o menu de atalho da solução SampleApp, escolha **Adicionar** e **Novo Projeto** para adicionar um novo projeto em C# ou Visual Basic à solução. Na seção **Modelos Instalados** da caixa de diálogo **Adicionar novo projeto**, escolha **Visual Basic** ou **Visual C#**, escolha **Windows** e **Universal**. Escolha o modelo **Componente do Tempo de Execução do Windows** e insira **SampleComponent** para o nome do projeto.
3.  Altere o nome da classe para **Exemplo**. Observe que, por padrão, a classe é marcada como **public sealed** (**Public NotInheritable** no Visual Basic). Todas as classes do Windows Runtime que você expõe no componente devem ser seladas.
4.  Adicione dois membros simples à classe, um método **static** (método **Shared** em Visual Basic) e uma propriedade de instância:

    > [!div class="tabbedCodeSnippets"]
    > ```csharp
    > namespace SampleComponent
    > {
    >     public sealed class Example
    >     {
    >         public static string GetAnswer()
    >         {
    >             return "The answer is 42.";
    >         }
    >
    >         public int SampleProperty { get; set; }
    >     }
    > }
    > ```
    > ```vb
    > Public NotInheritable Class Example
    >     Public Shared Function GetAnswer() As String
    >         Return "The answer is 42."
    >     End Function
    >
    >     Public Property SampleProperty As Integer
    > End Class
    > ```

5.  Opcional: para habilitar o IntelliSense para os membros recém-adicionados, no Gerenciador de Soluções, abra o menu de atalho do projeto SampleComponent e escolha **Compilar**.
6.  No Gerenciador de Soluções, no projeto do JavaScript, abra o menu de atalho de **Referências** e escolha **Adicionar Referência** para abrir o **Gerenciador de Referências**. Escolha **Projetos** e **Solução**. Marque a caixa de seleção do projeto SampleComponent e escolha **OK** para adicionar uma referência.

## <a name="call-the-component-from-javascript"></a>Chamar o componente em JavaScript


Para usar o tipo de Tempo de Execução do Windows em JavaScript, adicione o código a seguir na função anônima no arquivo default.js (na pasta js do projeto) que é fornecido pelo modelo do Visual Studio. Ele deve ficar depois do manipulador de eventos app.oncheckpoint e antes da chamada para app.start.

```javascript
var ex;

function basics1() {
   document.getElementById('output').innerHTML =
        SampleComponent.Example.getAnswer();

    ex = new SampleComponent.Example();

   document.getElementById('output').innerHTML += "<br/>" +
       ex.sampleProperty;

}

function basics2() {
    ex.sampleProperty += 1;
    document.getElementById('output').innerHTML += "<br/>" +
        ex.sampleProperty;
}
```

A primeira letra do nome de cada membro é alterada de maiúsculas para minúsculas. Essa transformação faz parte do suporte que o JavaScript oferece para habilitar o uso natural do Tempo de Execução do Windows. Namespaces e nomes de classe estão em Pascal. Os nomes de membro estão em camel, exceto nomes de evento, que estão todos em minúsculas. Consulte [Como usar o Tempo de Execução do Windows em JavaScript](https://msdn.microsoft.com/library/hh710230.aspx). As regras de uso de maiúsculas camel podem ser confusas. Uma série de letras maiúsculas iniciais normalmente é exibida em minúsculas, mas caso três letras maiúsculas sejam seguidas de uma letra minúscula, somente as duas primeiras letras são exibidas em minúsculas: por exemplo, um membro chamado IDStringKind é exibido como idStringKind. No Visual Studio, é possível compilar o projeto do componente do Tempo de Execução do Windows e usar IntelliSense no projeto de JavaScript para saber o uso de maiúsculas correto.

De maneira semelhante, o .NET Framework dá suporte para habilitar o uso natural do Tempo de Execução do Windows em código gerenciado. Isso é discutido nas seções subsequentes deste artigo e nos artigos [Criando componentes do tempo de execução do Windows em c# e Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) e [Suporte do .NET Framework para aplicativos UWP e do Windows Runtime](https://msdn.microsoft.com/library/hh694558.aspx).

## <a name="create-a-simple-user-interface"></a>Criar uma interface do usuário simples


No projeto JavaScript, abra o arquivo default.html e atualize o corpo conforme mostrado no código a seguir. Esse código inclui o conjunto completo de controles para o aplicativo de exemplo e especifica os nomes de função para os eventos de clique.

> **Observação**ao executar o aplicativo pela primeira vez, somente os botões Basics1 e Basics2 têm suporte.

```html
<body>
            <div id="buttons">
            <button id="button1" >Basics 1</button>
            <button id="button2" >Basics 2</button>

            <button id="runtimeButton1">Runtime 1</button>
            <button id="runtimeButton2">Runtime 2</button>

            <button id="returnsButton1">Returns 1</button>
            <button id="returnsButton2">Returns 2</button>

            <button id="events1Button">Events 1</button>

            <button id="btnAsync">Async</button>
            <button id="btnCancel" disabled="disabled">Cancel Async</button>
            <progress id="primeProg" value="25" max="100" style="color: yellow;"></progress>
        </div>
        <div id="output">
        </div>
</body>
```

No projeto JavaScript, na pasta css, abra default.css. Modifique a seção body conforme mostrado e adicione estilos para controlar o layout dos botões e o posicionamento do texto de saída.

```css
body
{
    -ms-grid-columns: 1fr;
    -ms-grid-rows: 1fr 14fr;
    display: -ms-grid;
}

#buttons {
    -ms-grid-rows: 1fr;
    -ms-grid-columns: auto;
    -ms-grid-row-align: start;
}
#output {
    -ms-grid-row: 2;
    -ms-grid-column: 1;
}
```

Agora adicione o código de registro do ouvinte de eventos, incluindo uma cláusula then à chamada processAll em app.onactivated em default.js. Substitua a linha de código existente que chama setPromise e a altere para seguinte código:

```javascript
args.setPromise(WinJS.UI.processAll().then(function () {
    var button1 = document.getElementById("button1");
    button1.addEventListener("click", basics1, false);
    var button2 = document.getElementById("button2");
    button2.addEventListener("click", basics2, false);
}));
```

Essa é uma maneira melhor de adicionar eventos a controles HTML do que adicionar um manipulador de eventos click diretamente no HTML. Consulte [Criar um aplicativo "Hello World" (JS)](https://msdn.microsoft.com/library/windows/apps/mt280216).

## <a name="build-and-run-the-app"></a>Compilar e executar o aplicativo


Antes de compilar, altere a plataforma de destino de todos os projetos para ARM, x64 ou x86, conforme apropriado para o computador.

Para compilar e executar a solução, escolha a tecla F5. (Caso você receba uma mensagem de erro de tempo de execução informando que SampleComponent está indefinido, a referência para o projeto de biblioteca de classes não foi encontrada.)

O Visual Studio primeiro compila a biblioteca de classes e, em seguida, executa uma tarefa MSBuild que executa [Winmdexp.exe (Ferramenta de Exportação de Metadados do Tempo de Execução do Windows)](https://msdn.microsoft.com/library/hh925576.aspx) para criar o componente do Tempo de Execução do Windows. O componente está incluído em um arquivo .winmd que contém o código gerenciado e os metadados do Windows que descrevem o código. WinMdExp.exe gera mensagens de erro de compilação quando você escreve um código que é inválido em um componente do Tempo de Execução do Windows e as mensagens de erro são exibidas no Visual Studio IDE. O Visual Studio adiciona o componente ao pacote do aplicativo (arquivo .appx) para o aplicativo Universal do Windows e gera o manifesto apropriado.

Escolha o botão Básico 1 para atribuir o valor de retorno do método GetAnswer estático à área de saída, criar uma instância da classe Example e exibir o valor da propriedade SampleProperty na área de saída. A saída é mostrada aqui:

``` syntax
"The answer is 42."
0
```

Escolha o botão Básico 2 para incrementar o valor da propriedade SampleProperty e exibir o novo valor na área de saída. Tipos primitivos, como cadeias de caracteres e números, podem ser usados como tipos de parâmetro e tipos de retorno, além de poder ser passados entre o código gerenciado e o JavaScript. Como números em JavaScript são armazenados em formato de ponto flutuante de precisão dupla, eles são convertidos em tipos numéricos do .NET Framework.

> **Observação**por padrão, você pode definir pontos de interrupção apenas no código JavaScript. Para depurar o código Visual Basic ou C#, consulte Criação de componentes do Tempo de Execução do Windows em C# ou Visual Basic.

 

Para interromper a depuração e fechar o aplicativo, alterne do aplicativo para o Visual Studio e escolha Shift+F5.

## <a name="using-the-windows-runtime-from-javascript-and-managed-code"></a>Como usar o Tempo de Execução do Windows em JavaScript e código gerenciado


O Tempo de Execução do Windows pode ser chamado no JavaScript ou no código gerenciado. Os objetos do Tempo de Execução do Windows podem ser passados para a frente e para trás entre os dois, e os eventos podem ser manipulados de ambos os lados. No entanto, as formas como você usa os tipos de Tempo de Execução do Windows nos dois ambientes diferem em alguns detalhes, porque o JavaScript e o .NET Framework dão suporte ao Tempo de Execução do Windows de maneira diferente. O exemplo a seguir demonstra essas diferenças usando a classe [Windows.Foundation.Collections.PropertySet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.propertyset.aspx). Neste exemplo, você cria uma instância da coleção PropertySet em código gerenciado e registra um manipulador de eventos para controlar alterações na coleção. Em seguida, você adiciona código JavaScript que obtém a coleção, registra o próprio manipulador de eventos e usa a coleção. Por fim, você adiciona um método que faz alterações na coleção do código gerenciado e mostra JavaScript manipulando uma exceção gerenciada.

> **Importante**neste exemplo, o evento está sendo acionado no thread da interface do usuário. Se disparar o evento em um thread em segundo plano, por exemplo, em uma chamada assíncrona, você precisará fazer um trabalho extra para JavaScript para manipular o evento. Para obter mais informações, consulte [Acionamento de eventos em componentes do Tempo de Execução do Windows](raising-events-in-windows-runtime-components.md).

 

No projeto SampleComponent, adicione uma nova classe **public sealed** (classe **Public NotInheritable** no Visual Basic) chamada PropertySetStats. A classe encapsula uma coleção PropertySet e manuseia o evento MapChanged. O manipulador de eventos controla o número de alterações de cada tipo que ocorrem, e o método DisplayStats produz um relatório formatado em HTML. Observe a instrução **using** adicional (instrução **Imports** no Visual Basic); tome cuidado para adicioná-la às instruções **using** existentes, em vez de substituí-las.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> using Windows.Foundation.Collections;
>
> namespace SampleComponent
> {
>     public sealed class PropertySetStats
>     {
>         private PropertySet _ps;
>         public PropertySetStats()
>         {
>             _ps = new PropertySet();
>             _ps.MapChanged += this.MapChangedHandler;
>         }
>
>         public PropertySet PropertySet { get { return _ps; } }
>
>         int[] counts = { 0, 0, 0, 0 };
>         private void MapChangedHandler(IObservableMap<string, object> sender,
>             IMapChangedEventArgs<string> args)
>         {
>             counts[(int)args.CollectionChange] += 1;
>         }
>
>         public string DisplayStats()
>         {
>             StringBuilder report = new StringBuilder("<br/>Number of changes:<ul>");
>             for (int i = 0; i < counts.Length; i++)
>             {
>                 report.Append("<li>" + (CollectionChange)i + ": " + counts[i] + "</li>");
>             }
>             return report.ToString() + "</ul>";
>         }
>     }
> }
> ```
> ```vb
> Imports System.Text
>
> Public NotInheritable Class PropertySetStats
>     Private _ps As PropertySet
>     Public Sub New()
>         _ps = New PropertySet()
>         AddHandler _ps.MapChanged, AddressOf Me.MapChangedHandler
>     End Sub
>
>     Public ReadOnly Property PropertySet As PropertySet
>         Get
>             Return _ps
>         End Get
>     End Property
>
>     Dim counts() As Integer = {0, 0, 0, 0}
>     Private Sub MapChangedHandler(ByVal sender As IObservableMap(Of String, Object),
>         ByVal args As IMapChangedEventArgs(Of String))
>
>         counts(CInt(args.CollectionChange)) += 1
>     End Sub
>
>     Public Function DisplayStats() As String
>         Dim report As New StringBuilder("<br/>Number of changes:<ul>")
>         For i As Integer = 0 To counts.Length - 1
>             report.Append("<li>" & CType(i, CollectionChange).ToString() &
>                           ": " & counts(i) & "</li>")
>         Next
>         Return report.ToString() & "</ul>"
>     End Function
> End Class
> ```

O manipulador de eventos segue o padrão de evento familiar do .NET Framework, exceto que o remetente do evento (nesse caso, o objeto PropertySet) é convertido para o IObservableMap&lt;cadeia de caracteres, objeto&gt; (IObservableMap (Of String, Object) na interface Visual Basic), que é uma instanciação da interface do Windows Runtime [IObservableMap&lt;K, V&gt;](https://msdn.microsoft.com/library/windows/apps/br226050.aspx). (Você pode converter o remetente para seu tipo se necessário). Além disso, os argumentos do evento são apresentados como uma interface em vez de um objeto.

No arquivo default.js, adicione a função Runtime1 conforme mostrado. Esse código cria um objeto PropertySetStats, obtém a coleção PropertySet e adiciona o próprio manipulador de eventos, a função onMapChanged, para manipular o evento MapChanged. Após as alterações feitas na coleção, runtime1 chama o método DisplayStats para mostrar um resumo dos tipos de alteração.

```javascript
var propertysetstats;

function runtime1() {
    document.getElementById('output').innerHTML = "";

    propertysetstats = new SampleComponent.PropertySetStats();
    var propertyset = propertysetstats.propertySet;

    propertyset.addEventListener("mapchanged", onMapChanged);

    propertyset.insert("FirstProperty", "First property value");
    propertyset.insert("SuperfluousProperty", "Unnecessary property value");
    propertyset.insert("AnotherProperty", "A property value");

    propertyset.insert("SuperfluousProperty", "Altered property value")
    propertyset.remove("SuperfluousProperty");

    document.getElementById('output').innerHTML +=
        propertysetstats.displayStats();
}

function onMapChanged(change) {
    var result
    switch (change.collectionChange) {
        case Windows.Foundation.Collections.CollectionChange.reset:
            result = "All properties cleared";
            break;
        case Windows.Foundation.Collections.CollectionChange.itemInserted:
            result = "Inserted " + change.key + ": '" +
                change.target.lookup(change.key) + "'";
            break;
        case Windows.Foundation.Collections.CollectionChange.itemRemoved:
            result = "Removed " + change.key;
            break;
        case Windows.Foundation.Collections.CollectionChange.itemChanged:
            result = "Changed " + change.key + " to '" +
                change.target.lookup(change.key) + "'";
            break;
        default:
            break;
     }

     document.getElementById('output').innerHTML +=
         "<br/>" + result;
}
```

A maneira como você manipula eventos de Tempo de Execução do Windows em JavaScript é muito diferente da maneira como os manipula no código do .NET Framework. O manipulador de eventos JavaScript utiliza apenas um argumento. Quando você exibe esse objeto no depurador do Visual Studio, a primeira propriedade é o remetente. Os membros da interface de argumento do evento também são exibidos diretamente nesse objeto.

Para executar o aplicativo, escolha a tecla F5. Caso a classe não seja selada, você receber a mensagem de erro, "Exporting unsealed type 'SampleComponent.Example' is not currently supported. Please mark it as sealed".

Escolha o botão **Tempo de Execução 1**. O manipulador de eventos exibe alterações à medida que elementos são adicionados ou alterados e, ao final, o método DisplayStats é chamado para produzir um resumo de contagens. Para interromper a depuração e fechar o aplicativo, retorne ao Visual Studio e escolha Shift+F5.

Para adicionar mais dois itens à coleção PropertySet do código gerenciado, adicione o seguinte código à classe PropertySetStats:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public void AddMore()
> {
>     _ps.Add("NewProperty", "New property value");
>     _ps.Add("AnotherProperty", "A property value");
> }
> ```
> ```vb
> Public Sub AddMore()
>     _ps.Add("NewProperty", "New property value")
>     _ps.Add("AnotherProperty", "A property value")
> End Sub
> ```

Esse código realça outra diferença na maneira como você usa tipos de Tempo de Execução do Windows nos dois ambientes. Se digitar esse código, você notará que o IntelliSense não mostra o método insert usado no código JavaScript. Em vez disso, ele mostra o método Add normalmente visto em coleções no .NET Framework. Isso porque algumas interfaces de coleção mais usadas têm nomes diferentes, mas funcionalidade semelhante no Tempo de Execução do Windows e no .NET Framework. Quando você usa essas interfaces em código gerenciado, elas são exibidas como os equivalentes do .NET Framework. Isso é abordado em [Criação de componentes do Tempo de Execução do Windows em C# ou Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md). Quando você usa as mesmas interfaces em JavaScript, a única alteração em relação ao Tempo de Execução do Windows é que letras maiúsculas no início dos nomes de membro se tornam minúsculas.

Por fim, para chamar o método AddMore com o tratamento de exceções, adicione a função runtime2 a default.js.

```javascript
function runtime2() {
   try {
      propertysetstats.addMore();
    }
   catch(ex) {
       document.getElementById('output').innerHTML +=
          "<br/><b>" + ex + "<br/>";
   }

   document.getElementById('output').innerHTML +=
       propertysetstats.displayStats();
}
```

Adicione o código de registro de manipulador de eventos da mesma maneira como você fez anteriormente.

```javascript
var runtimeButton1 = document.getElementById("runtimeButton1");
runtimeButton1.addEventListener("click", runtime1, false);
var runtimeButton2 = document.getElementById("runtimeButton2");
runtimeButton2.addEventListener("click", runtime2, false);
```

Para executar o aplicativo, escolha a tecla F5. Escolha **Tempo de Execução 1** e **Tempo de Execução 2**. O manipulador de eventos JavaScript relata a primeira alteração feita na coleção. A segunda alteração, porém, tem uma chave duplicada. Os usuários de dicionários do .NET Framework esperam que o método Add lance uma exceção, e é isso o que acontece. O JavaScript manipula a exceção do .NET Framework.

> **Observação**não é possível exibir a mensagem da exceção no código JavaScript. O texto da mensagem é substituído por um rastreamento de pilha. Para obter mais informações, consulte "Lançamento de exceções" em Criação de componentes do Tempo de Execução do Windows em C# ou Visual Basic.

Por outro lado, quando JavaScript chamou o método insert usando uma chave duplicada, o valor do item foi alterado. Essa diferença no comportamento acontece por causa das diferentes maneiras que o JavaScript e o .NET Framework dão suporte ao Tempo de Execução do Windows, conforme explicado em [Criação de componentes do Tempo de Execução do Windows em C# ou Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md).

## <a name="returning-managed-types-from-your-component"></a>Retorno de tipos gerenciados do componente


Conforme abordado anteriormente, é possível passar tipos de Tempo de Execução do Windows nativos para trás e para frente livremente entre o código JavaScript e o código C# ou Visual Basic. Na maioria das vezes, os nomes de tipo e membro serão iguais em ambos os casos (exceto pelos nomes de membro começarem com letras minúsculas em JavaScript). No entanto, na seção anterior, a classe PropertySet aparenta ter membros diferentes em código gerenciado. (Por exemplo, em JavaScript é chamado o método insert, e no código do .NET Framework você chamou o método Add). Esta seção explora a maneira como essas diferenças afetam os tipos do .NET Framework passados para JavaScript.

Além de retornar tipos de Tempo de Execução do Windows que você criou no componente ou passou para o componente em JavaScript, é possível retornar um tipo gerenciado, criado em código gerenciado, para JavaScript como se fosse o tipo de Tempo de Execução do Windows correspondente. Mesmo no primeiro exemplo simples de uma classe de tempo de execução, os parâmetros e os tipos de retorno dos membros eram tipos primitivos Visual Basic ou C#, que são tipos do .NET Framework. Para demonstrar isso para coleções, adicione o seguinte código à classe Example para criar um método que retorna um dicionário genérico de cadeias de caracteres, indexados por inteiros:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static IDictionary<int, string> GetMapOfNames()
> {
>     Dictionary<int, string> retval = new Dictionary<int, string>();
>     retval.Add(1, "one");
>     retval.Add(2, "two");
>     retval.Add(3, "three");
>     retval.Add(42, "forty-two");
>     retval.Add(100, "one hundred");
>     return retval;
> }
> ```
> ```vb
> Public Shared Function GetMapOfNames() As IDictionary(Of Integer, String)
>     Dim retval As New Dictionary(Of Integer, String)
>     retval.Add(1, "one")
>     retval.Add(2, "two")
>     retval.Add(3, "three")
>     retval.Add(42, "forty-two")
>     retval.Add(100, "one hundred")
>     Return retval
> End Function
> ```

Observe que o dicionário deve ser retornado como uma interface implementada por [Dictionary&lt;TKey, TValue&gt;](https://msdn.microsoft.com/library/xfhwa508.aspx) e mapeada para uma interface do Windows Runtime. Nesse caso, a interface é IDictionary&lt;int, string&gt; (IDictionary(Of Integer, String) no Visual Basic). Quando o tipo de Windows Runtime IMap&lt;int, string&gt; é passado para o código gerenciado, ele é exibido como IDictionary&lt;int, string&gt;, e o inverso é verdadeiro quando o tipo gerenciado é passado para JavaScript.

**Importante**quando um tipo gerenciado implementa várias interfaces, JavaScript usa a primeira interface exibida na lista. Por exemplo, se você retornar Dictionary&lt;int, string&gt; ao código JavaScript, ele será exibido como IDictionary&lt;int, string&gt;, independentemente de qual interface você especificar como o tipo de retorno. Isso significa que, se a primeira interface não incluir um membro exibido em interfaces posteriores, esse membro não permanecerá visível para JavaScript.

 

Para testar o novo método e usas o dicionário, adicione as funções returns1 e returns2 a default. js:

```javascript
var names;

function returns1() {
    names = SampleComponent.Example.getMapOfNames();
    document.getElementById('output').innerHTML = showMap(names);
}

var ct = 7;

function returns2() {
    if (!names.hasKey(17)) {
        names.insert(43, "forty-three");
        names.insert(17, "seventeen");
    }
    else {
        var err = names.insert("7", ct++);
        names.insert("forty", "forty");
    }
    document.getElementById('output').innerHTML = showMap(names);
}

function showMap(map) {
    var item = map.first();
    var retval = "<ul>";

    for (var i = 0, len = map.size; i < len; i++) {
        retval += "<li>" + item.current.key + ": " + item.current.value + "</li>";
        item.moveNext();
    }
    return retval + "</ul>";
}
```

Adicione o código de registro do evento ao mesmo bloco como o outro código de registro do evento:

```javascript
var returnsButton1 = document.getElementById("returnsButton1");
returnsButton1.addEventListener("click", returns1, false);
var returnsButton2 = document.getElementById("returnsButton2");
returnsButton2.addEventListener("click", returns2, false);
```

Existem algumas coisas interessantes a serem observadas sobre esse código JavaScript. Primeiro, ele inclui uma função showMap para exibir o conteúdo do dicionário em HTML. No código de showMap, observe o padrão de iteração. No .NET Framework, não há nenhum método First na interface IDictionary genérica, e o tamanho é retornado por uma propriedade Count, e não um método Size. Para JavaScript, IDictionary&lt;int, string&gt; parece ser o tipo de Windows Runtime IMap&lt;int, string&gt;. (Consulte a interface [IMap&lt;K,V&gt;](https://msdn.microsoft.com/library/windows/apps/br226042.aspx).)

Na função returns2, assim como em exemplos anteriores, JavaScript chama o método Insert (inserir em JavaScript) para adicionar itens ao dicionário.

Para executar o aplicativo, escolha a tecla F5. Para criar e exibir o conteúdo inicial do dicionário, escolha o botão **Retorna 1**. Para adicionar mais duas entradas ao dicionário, escolha o botão **Retorna 2**. Observe que as entradas são exibidas na ordem de inserção, como você esperaria de Dictionary&lt;TKey, TValue&gt;. Caso queira que elas sejam classificadas, você pode retornar um SortedDictionary&lt;int, string&gt; de GetMapOfNames. (A classe PropertySet usada em exemplos anteriores tem uma organização interna diferente de Dictionary&lt;TKey, TValue&gt;.)

Obviamente, JavaScript não é uma linguagem fortemente tipada, logo, usar coleções genéricas fortemente tipadas pode levar a alguns resultados surpreendentes. Escolha o botão **Retorna 2** novamente. O JavaScript força obrigatoriamente o "7" para um 7 numérico, e o 7 numérico que é armazenado em ct para uma cadeia de caracteres. E ele força a cadeia de caracteres "quarenta" para zero. Mas isso é apenas o começo. Escolha o botão **Retorna 2** mais algumas vezes. Em código gerenciado, o método Add geraria exceções de chave duplicada, mesmo se os valores fossem convertidos nos tipos corretos. Por outro lado, o método Insert atualiza o valor associado a uma chave existente e retorna um valor booliano que indica se uma nova chave foi adicionada ao dicionário. É por isso que o valor associado à chave 7 continua mudando.

Outro comportamento inesperado: caso passe uma variável JavaScript não atribuída como um argumento de cadeia de caracteres, o que você obtém é a cadeia de caracteres "undefined". Resumindo, tome cuidado ao passar os tipos de coleção do .NET Framework para o código JavaScript.

> **Observação**se você tiver grandes quantidades de texto a ser concatenado, você pode fazer isso com mais eficiência movendo o código para um método do .NET Framework e usando a classe StringBuilder, conforme mostrado na função showMap.

Embora não possa expor os próprios tipos genéricos de um componente do Tempo de Execução do Windows, você poderá retornar coleções genéricas do .NET Framework para classes do Windows Runtime usando um código como o seguinte:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static object GetListOfThis(object obj)
> {
>     Type target = obj.GetType();
>     return Activator.CreateInstance(typeof(List<>).MakeGenericType(target));
> }
> ```
> ```vb
> Public Shared Function GetListOfThis(obj As Object) As Object
>     Dim target As Type = obj.GetType()
>     Return Activator.CreateInstance(GetType(List(Of )).MakeGenericType(target))
> End Function
> ```

List&lt;T&gt; implementa IList&lt;T&gt;, exibido como o tipo de Windows Runtime IVector&lt;T&gt; em JavaScript.

## <a name="declaring-events"></a>Declaração de eventos


Você pode declarar eventos usando o padrão de eventos do .NET Framework ou outros padrões usados pelo Windows Runtime. O .NET Framework dá suporte à equivalência entre o representante System.EventHandler&lt;TEventArgs&gt; e o representante EventHandler&lt;T&gt; do Windows Runtime, logo, usar EventHandler&lt;TEventArgs&gt; é uma boa maneira de implementar o padrão do .NET Framework. Para saber como isso funciona, adicione o seguinte par de classes ao projeto SampleComponent:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> namespace SampleComponent
> {
>     public sealed class Eventful
>     {
>         public event EventHandler<TestEventArgs> Test;
>         public void OnTest(string msg, long number)
>         {
>             EventHandler<TestEventArgs> temp = Test;
>             if (temp != null)
>             {
>                 temp(this, new TestEventArgs()
>                 {
>                     Value1 = msg,
>                     Value2 = number
>                 });
>             }
>         }
>     }
>
>     public sealed class TestEventArgs
>     {
>         public string Value1 { get; set; }
>         public long Value2 { get; set; }
>     }
> }
> ```
> ```vb
> Public NotInheritable Class Eventful
>     Public Event Test As EventHandler(Of TestEventArgs)
>     Public Sub OnTest(ByVal msg As String, ByVal number As Long)
>         RaiseEvent Test(Me, New TestEventArgs() With {
>                             .Value1 = msg,
>                             .Value2 = number
>                             })
>     End Sub
> End Class
>
> Public NotInheritable Class TestEventArgs
>     Public Property Value1 As String
>     Public Property Value2 As Long
> End Class
> ```

Quando você expõe um evento em Tempo de Execução do Windows, a classe de argumento do evento é herdada de System.Object. Ele não herda de System.EventArgs, como aconteceria no .NET Framework, porque EventArgs não é um tipo de Windows Runtime.

Caso declare acessadores de eventos personalizados para o evento (palavra-chave **Custom** no Visual Basic), você deve usar o padrão de evento de Windows Runtime. Consulte [Eventos personalizados e acessadores de evento em componentes do Tempo de Execução do Windows](custom-events-and-event-accessors-in-windows-runtime-components.md).

Para manipular o evento Test, adicione a função events1 a default. js. A função events1 cria uma função do manipulador de eventos para o evento Test e invoca imediatamente o método OnTest para acionar o evento. Se colocar um ponto de interrupção no corpo do manipulador de eventos, você poderá ver que o objeto passado para o parâmetro único inclui o objeto de origem e ambos os membros de TestEventArgs.

```javascript
var ev;

function events1() {
   ev = new SampleComponent.Eventful();
   ev.addEventListener("test", function (e) {
       document.getElementById('output').innerHTML = e.value1;
       document.getElementById('output').innerHTML += "<br/>" + e.value2;
   });
   ev.onTest("Number of feet in a mile:", 5280);
}
```

Adicione o código de registro do evento ao mesmo bloco como o outro código de registro do evento:

```javascript
var events1Button = document.getElementById("events1Button");
events1Button.addEventListener("click", events1, false);
```

## <a name="exposing-asynchronous-operations"></a>Exposição de operações assíncronas


O .NET Framework tem um conjunto avançado de ferramentas para processamento assíncrono e processamento em paralelo, com base na tarefa e nas classes genéricas [Task&lt;TResult&gt;](https://msdn.microsoft.com/library/dd321424.aspx). Para expor um processamento assíncrono baseado em tarefa em um componente do Tempo de Execução do Windows, use as interfaces do Windows Runtime [IAsyncAction](https://msdn.microsoft.com/library/br205781.aspx), [IAsyncActionWithProgress&lt;TProgress&gt;](https://msdn.microsoft.com/library/br205784.aspx), [IAsyncOperation&lt;TResult&gt;](https://msdn.microsoft.com/library/br205802.aspx) e [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](https://msdn.microsoft.com/library/br205807.aspx). (No Windows Runtime, as operações retornam resultados, mas as ações, não.)

Esta seção demonstra uma operação assíncrona cancelável que relata progresso e retorna resultados. O método GetPrimesInRangeAsync usa a classe [AsyncInfo](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.asyncinfo.aspx) para gerar uma tarefa e conectar os recursos de relatório de progresso e cancelamento com um objeto WinJS.Promise. Comece adicionando o método GetPrimesInRangeAsync à classe de exemplo:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> using System.Runtime.InteropServices.WindowsRuntime;
> using Windows.Foundation;
>
> public static IAsyncOperationWithProgress<IList<long>, double>
> GetPrimesInRangeAsync(long start, long count)
> {
>     if (start < 2 || count < 1) throw new ArgumentException();
>
>     return AsyncInfo.Run<IList<long>, double>((token, progress) =>
>
>         Task.Run<IList<long>>(() =>
>         {
>             List<long> primes = new List<long>();
>             double onePercent = count / 100;
>             long ctProgress = 0;
>             double nextProgress = onePercent;
>
>             for (long candidate = start; candidate < start + count; candidate++)
>             {
>                 ctProgress += 1;
>                 if (ctProgress >= nextProgress)
>                 {
>                     progress.Report(ctProgress / onePercent);
>                     nextProgress += onePercent;
>                 }
>                 bool isPrime = true;
>                 for (long i = 2, limit = (long)Math.Sqrt(candidate); i <= limit; i++)
>                 {
>                     if (candidate % i == 0)
>                     {
>                         isPrime = false;
>                         break;
>                     }
>                 }
>                 if (isPrime) primes.Add(candidate);
>
>                 token.ThrowIfCancellationRequested();
>             }
>             progress.Report(100.0);
>             return primes;
>         }, token)
>     );
> }
> ```
> ```vb
> Imports System.Runtime.InteropServices.WindowsRuntime
>
> Public Shared Function GetPrimesInRangeAsync(ByVal start As Long, ByVal count As Long)
> As IAsyncOperationWithProgress(Of IList(Of Long), Double)
>
>     If (start < 2 Or count < 1) Then Throw New ArgumentException()
>
>     Return AsyncInfo.Run(Of IList(Of Long), Double)( _
>         Function(token, prog)
>             Return Task.Run(Of IList(Of Long))( _
>                 Function()
>                     Dim primes As New List(Of Long)
>                     Dim onePercent As Long = count / 100
>                     Dim ctProgress As Long = 0
>                     Dim nextProgress As Long = onePercent
>
>                     For candidate As Long = start To start + count - 1
>                         ctProgress += 1
>
>                         If ctProgress >= nextProgress Then
>                             prog.Report(ctProgress / onePercent)
>                             nextProgress += onePercent
>                         End If
>
>                         Dim isPrime As Boolean = True
>                         For i As Long = 2 To CLng(Math.Sqrt(candidate))
>                             If (candidate Mod i) = 0 Then
>                                 isPrime = False
>                                 Exit For
>                             End If
>                         Next
>
>                         If isPrime Then primes.Add(candidate)
>
>                         token.ThrowIfCancellationRequested()
>                     Next
>                     prog.Report(100.0)
>                     Return primes
>                 End Function, token)
>         End Function)
> End Function
> ```

GetPrimesInRangeAsync é um localizador de números primos muito simples por design. O foco aqui está na implementação de uma operação assíncrona, logo, a simplicidade é importante e uma implementação lenta é uma vantagem quando estamos demonstrando o cancelamento. GetPrimesInRangeAsync encontra primes por força bruta: ele divide um candidato por todos os inteiros menores que ou iguais à raiz quadrada, em vez de usar apenas os números principais. Como passar por este código:

-   Antes de iniciar uma operação assíncrona, realize atividades de manutenção, como validar parâmetros e gerar exceções para uma entrada inválida.
-   A chave para essa implementação é o método [AsyncInfo.Run&lt;TResult, TProgress&gt;(Func&lt;CancellationToken, IProgress&lt;TProgress&gt;, Task&lt;TResult&gt;](https://msdn.microsoft.com/library/hh779740.aspx)&gt;) e o representante que é o único parâmetro do método. O representante deve aceitar um token de cancelamento e uma interface de relatório de progresso, além de precisar retornar a uma tarefa iniciada que usa esses parâmetros. Quando JavaScript chama o método GetPrimesInRangeAsync, ocorrem as seguintes etapas (não necessariamente na ordem indicada aqui):

    -   O objeto [WinJS.Promise](https://msdn.microsoft.com/library/windows/apps/br211867.aspx) fornece funções para processar os resultados retornados, reagir ao cancelamento e manipular os relatórios de progresso.
    -   O método AsyncInfo.Run cria uma fonte de cancelamento e um objeto que implementa a interface IProgress&lt;T&gt;. Para o representante, ele passa um token [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) da fonte de cancelamento e da interface [IProgress&lt;T&gt;](https://msdn.microsoft.com/library/hh138298.aspx).

        > **Observação**se o objeto Promise não forneça uma função para reagir ao cancelamento, asyncinfo. Run ainda passará um token cancelável e cancelamento ainda poderá ocorrer. Caso o objeto Promise não forneça uma função para manipular atualizações de progresso, AsyncInfo.Run ainda fornecerá um objeto que implementa IProgress&lt;T&gt;, mas os relatórios serão ignorados.

    -   O representante usa o método [Task.Run&lt;TResult&gt;(Func&lt;TResult&gt;, CancellationToken](https://msdn.microsoft.com/library/hh160376.aspx)) para criar uma tarefa iniciada que usa o token e a interface de progresso. O representante da tarefa iniciada é fornecido por uma função lambda que calcula o resultado desejado. Mais sobre isso daqui a pouco.
    -   O método AsyncInfo.Run cria um objeto que implementa a interface [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](https://msdn.microsoft.com/library/windows/apps/br206594.aspx), conecta o mecanismo de cancelamento do Windows Runtime com a fonte do token e conecta a função de relatório de progresso do objeto Promise com a interface IProgress&lt;T&gt;.
    -   A interface IAsyncOperationWithProgress&lt;TResult, TProgress&gt; é retornada para JavaScript.

-   A função lambda que é representada pela tarefa iniciada não utiliza argumentos. Como é uma função lambda, ela tem acesso ao token e à interface IProgress. Sempre que um número de candidato é avaliado, a função lambda:

    -   Verifica se o próximo ponto porcentual de progresso foi atingido. Em caso afirmativo, a função lambda chamará o método IProgress&lt;T&gt;.Report, e a porcentagem será passada para a função que o objeto Promise especificou para relatar o progresso.
    -   Usa o token de cancelamento para lançar uma exceção caso a operação tenha sido cancelada. Se o método [IAsyncInfo.Cancel](https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncinfo.cancel.aspx) (que a interface IAsyncOperationWithProgress&lt;TResult, TProgress&gt; herda) tiver sido chamado, a conexão que o método AsyncInfo.Run configurou garantirá que o token de cancelamento seja notificado.
-   Quando a função lambda retorna a lista de números principais, a lista é passada para a função que o objeto WinJS.Promise especificou para processar os resultados.

Para criar a promessa JavaScript e configurar o mecanismo de cancelamento, adicione as funções asyncRun e asyncCancel a default.js.

```javascript
var resultAsync;
function asyncRun() {
    document.getElementById('output').innerHTML = "Retrieving prime numbers.";
    btnAsync.disabled = "disabled";
    btnCancel.disabled = "";

    resultAsync = SampleComponent.Example.getPrimesInRangeAsync(10000000000001, 2500).then(
        function (primes) {
            for (i = 0; i < primes.length; i++)
                document.getElementById('output').innerHTML += " " + primes[i];

            btnCancel.disabled = "disabled";
            btnAsync.disabled = "";
        },
        function () {
            document.getElementById('output').innerHTML += " -- getPrimesInRangeAsync was canceled. -- ";

            btnCancel.disabled = "disabled";
            btnAsync.disabled = "";
        },
        function (prog) {
            document.getElementById('primeProg').value = prog;
        }
    );
}

function asyncCancel() {    
    resultAsync.cancel();
}
```

Não esqueça o código de registro do evento, o mesmo que você criou anteriormente.

```javascript
var btnAsync = document.getElementById("btnAsync");
btnAsync.addEventListener("click", asyncRun, false);
var btnCancel = document.getElementById("btnCancel");
btnCancel.addEventListener("click", asyncCancel, false);
```

Chamando o método GetPrimesInRangeAsync assíncrono, a função asyncRun cria um objeto WinJS.Promise. O método then do objeto tem três funções que processam os resultados retornados, reagem a erros (inclusive o cancelamento) e manipulam relatórios de progresso. Neste exemplo, os resultados retornados são impressos na área de saída. Cancelamento ou conclusão redefine os botões iniciam e cancelam a operação. Relatório de progresso atualiza o controle de progresso.

A função asyncCancel simplesmente chama o método cancel do objeto WinJS.Promise.

Para executar o aplicativo, escolha a tecla F5. Para iniciar a operação assíncrona, escolha o botão **Assíncrono**. O que acontece em seguida depende da velocidade do computador. Caso a barra de progresso seja concluída antes de você sequer piscar, aumente o tamanho do número inicial que é passado para GetPrimesInRangeAsync por um ou mais fatores de dez. Você pode ajustar a duração da operação aumentando ou diminuindo a contagem de números para testar, mas adicionar zeros no meio do número inicial terá um impacto maior. Para cancelar a operação, escolha o botão **Cancel Async**.

## <a name="related-topics"></a>Tópicos relacionados

* [.NET para visão geral de aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/xaml/br230302.aspx)
* [.NET para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx)
* [Passo a passo: Criando um componente do Tempo de Execução do Windows simples e chamando-o em JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
