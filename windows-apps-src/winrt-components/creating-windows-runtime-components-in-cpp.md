---
title: Windows Runtime componentes com C++o/CX
description: Este tópico mostra como usar C++o/CX para criar um componente&mdash;Windows Runtime um componente que possa ser chamado de um aplicativo universal do Windows criado usando qualquer linguagem de Windows Runtime.
ms.assetid: F7E06AA2-DCEC-427E-BD5D-9CA2A0ED2612
ms.date: 05/14/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e6cec29594405bb9a77a9b16805d2ac5a73c13cf
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393736"
---
# <a name="windows-runtime-components-with-ccx"></a>Windows Runtime componentes com C++o/CX

> [!NOTE]
> Este tópico existe para ajudar você na manutenção do seu aplicativo C++/CX. Recomendamos que você use [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) para novos aplicativos. C++/WinRT é uma projeção de linguagem C++17 completamente moderna e padrão para APIs do Windows Runtime (WinRT), implementada como uma biblioteca com base em cabeçalho e arquivo, projetada para fornecer acesso de primeira classe à API moderna do Windows. Para saber como criar um componente de Windows Runtime usando C++o/WinRT, consulte [criar eventos C++em/WinRT](../cpp-and-winrt-apis/author-events.md).

C++Este tópico mostra como usar o/CX para criar um componente&mdash;Windows Runtime um componente que possa ser chamado de um aplicativo universal do Windows criado usando qualquer linguagem de Windows Runtime ( C++C#, Visual Basic, ou JavaScript).

Há vários motivos para criar um componente de Windows Runtime.
- Obter a vantagem em termos de desempenho do C++ em operações complexas ou intensivas.
- Reutilizar um código que já tenha sido escrito e testado.

Quando você compila uma solução que contém um projeto JavaScript ou .NET e um projeto de componente do Tempo de Execução do Windows, os arquivos de projeto JavaScript e a DLL compilada são mesclados em um único pacote, que é possível depurar localmente no simulador ou remotamente em um dispositivo vinculado. Também é possível distribuir apenas o projeto de componente como um SDK de extensão. Para obter mais informações, consulte [Criação de um Software Development Kit](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015).

Em geral, ao codificar seu C++componente/CX, use a biblioteca regular C++ e os tipos internos, exceto no limite da Abi (interface binária abstrata) em que você está passando dados de e para o código em outro pacote de winmd. Lá, use Windows Runtime tipos e a sintaxe especial que C++o/CX dá suporte para criar e manipular esses tipos. Além disso, no código C++/CX, use tipos como delegate e Event para implementar eventos que podem ser gerados do componente e tratados em JavaScript, Visual Basic, C++ou. C# Para obter mais informações sobre C++a sintaxe do/CX, consulte [referência deC++linguagem Visual C++ (/CX)](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx).

## <a name="casing-and-naming-rules"></a>Uso de maiúsculas e regras de nomenclatura

### <a name="javascript"></a>JavaScript
JavaScript diferencia maiúsculas de minúsculas. Portanto, você deve seguir estas convenções do uso de maiúsculas:

-   Ao fazer referência a namespaces e classes C++, siga o uso de maiúsculas no lado de C++.
-   Ao chamar métodos, siga o uso de maiúsculas camel mesmo que o nome do método esteja em maiúsculas no lado de C++. Por exemplo, um método C++ GetDate() deve ser chamado em JavaScript como getDate().
-   Um nome de classe ativável e o nome de namespace não podem conter caracteres UNICODE.

### <a name="net"></a>.NET
As linguagens .NET seguem as regras do uso de maiúsculas normal.

## <a name="instantiating-the-object"></a>Criação de instância do objeto
Somente os tipos do Windows Runtime podem cruzar o limite ABI. O compilador acionará um erro se o componente tiver um tipo como std::wstring como um tipo de retorno ou parâmetro em um método público. Os tipos internos de extensões de componente do Visual C++ (C++/CX) incluem os escalares usuais, como int e double, além dos equivalentes typedef int32, float64, etc. Para obter mais informações, consulte [Sistema de tipos (C++/CX)](https://docs.microsoft.com/cpp/cppcx/type-system-c-cx).

```cpp
// ref class definition in C++
public ref class SampleRefClass sealed
{
    // Class members...

    // #include <valarray>
public:
    double LogCalc(double input)
    {
        // Use C++ standard library as usual.
        return std::log(input);
    }

};
```

```javascript
//Instantiation in JavaScript (requires "Add reference > Project reference")
var nativeObject = new CppComponent.SampleRefClass();
```

```csharp
//Call a method and display result in a XAML TextBlock
var num = nativeObject.LogCalc(21.5);
ResultText.Text = num.ToString();
```

## <a name="ccx-built-in-types-library-types-and-windows-runtime-types"></a>C++Tipos internos de/CX, tipos de biblioteca e tipos de Windows Runtime
Uma classe ativável (também conhecida como uma classe ref) é aquela cuja instância é criada com base em outra linguagem, como JavaScript, C# ou Visual Basic. Para ser consumível em outra linguagem, um componente deve conter pelo menos uma classe ativável.

Um componente do Tempo de Execução do Windows pode conter várias classes ativáveis públicas, bem como classes adicionais que são conhecidas apenas internamente para o componente. Aplique o atributo [WebHostHidden](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.webhosthiddenattribute) a C++tipos de/CX que não devem ser visíveis para JavaScript.

Todas as classes públicas devem residir no mesmo namespace raiz com o mesmo nome do arquivo de metadados do componente. Por exemplo, uma classe denominada A.B.C.MyClass poderá ser instanciada somente se for definida em um arquivo de metadados denominado A.winmd ou A.B.winmd ou A.B.C.winmd. O nome da DLL não precisa coincidir com o nome do arquivo .winmd.

O código do cliente cria uma instância do componente usando a palavra-chave **new** (**New** em Visual Basic) assim como acontece com qualquer classe.

Uma classe ativável deve ser declarada como **classe ref pública selado**. A palavra-chave **ref class** informa ao compilador para criar a classe como um tipo compatível do Windows Runtime e a palavra-chave sealed especifica que a classe não pode ser herdada. O Windows Runtime não dá suporte no momento a um modelo de herança generalizado; um modelo de herança limitado dá suporte à criação de controles XAML personalizados. Para obter mais informações, consulte [Classes e estruturas de referência (C++/CX)](https://docs.microsoft.com/cpp/cppcx/ref-classes-and-structs-c-cx).

Para C++o/CX, todos os primitivos numéricos são definidos no namespace padrão. O namespace de [plataforma](https://docs.microsoft.com/cpp/cppcx/platform-namespace-c-cx) C++ contém classes/CX que são específicas para o sistema de tipo de Windows Runtime. Isso inclui as classes [Platform::String](https://docs.microsoft.com/cpp/cppcx/platform-string-class) e [Platform::Object](https://docs.microsoft.com/cpp/cppcx/platform-object-class). Os tipos de coleção concretos como as classes [Platform::Collections::Map](https://docs.microsoft.com/cpp/cppcx/platform-collections-map-class) e [Platform::Collections::Vector](https://docs.microsoft.com/cpp/cppcx/platform-collections-vector-class) são definidos no namespace [Platform::Collections](https://docs.microsoft.com/cpp/cppcx/platform-collections-namespace). As interfaces públicas que esses tipos implementam são definidas no [Namespace Windows::Foundation::Collections (C++/CX)](https://docs.microsoft.com/cpp/cppcx/windows-foundation-collections-namespace-c-cx). São esses tipos de interface consumidos por JavaScript, C# e Visual Basic. Para obter mais informações, consulte [Sistema de tipos (C++/CX)](https://docs.microsoft.com/cpp/cppcx/type-system-c-cx).

## <a name="method-that-returns-a-value-of-built-in-type"></a>Método que retorna um valor de tipo interno
```cpp
    // #include <valarray>
public:
    double LogCalc(double input)
    {
        // Use C++ standard library as usual.
        return std::log(input);
    }
```

```javascript
//Call a method
var nativeObject = new CppComponent.SampleRefClass;
var num = nativeObject.logCalc(21.5);
document.getElementById('P2').innerHTML = num;
```

## <a name="method-that-returns-a-custom-value-struct"></a>Método que retorna uma estrutura de valor personalizada
```cpp
namespace CppComponent
{
    // Custom struct
    public value struct PlayerData
    {
        Platform::String^ Name;
        int Number;
        double ScoringAverage;
    };

    public ref class Player sealed
    {
    private:
        PlayerData m_player;
    public:
        property PlayerData PlayerStats
        {
            PlayerData get(){ return m_player; }
            void set(PlayerData data) {m_player = data;}
        }
    };
}
```

Para passar structs de valor definido pelo usuário pela ABI, defina um objeto JavaScript que tenha os mesmos membros que o struct de valor definido em C++/CX. Em seguida, você pode passar esse objeto como um argumento C++para um método/CX para que o objeto seja convertido implicitamente C++no tipo/CX.

```javascript
// Get and set the value struct
function GetAndSetPlayerData() {
    // Create an object to pass to C++
    var myData =
        { name: "Bob Homer", number: 12, scoringAverage: .357 };
    var nativeObject = new CppComponent.Player();
    nativeObject.playerStats = myData;

    // Retrieve C++ value struct into new JavaScript object
    var myData2 = nativeObject.playerStats;
    document.getElementById('P3').innerHTML = myData.name + " , " + myData.number + " , " + myData.scoringAverage.toPrecision(3);
}
```

Outra abordagem é definir uma classe que implemente IPropertySet (não mostrado).

Nas linguagens do .NET, basta criar uma variável do tipo definido no componente C++/CX.

```csharp
private void GetAndSetPlayerData()
{
    // Create a ref class
    var player = new CppComponent.Player();

    // Create a variable of a value struct
    // type that is defined in C++
    CppComponent.PlayerData myPlayer;
    myPlayer.Name = "Babe Ruth";
    myPlayer.Number = 12;
    myPlayer.ScoringAverage = .398;

    // Set the property
    player.PlayerStats = myPlayer;

    // Get the property and store it in a new variable
    CppComponent.PlayerData myPlayer2 = player.PlayerStats;
    ResultText.Text += myPlayer.Name + " , " + myPlayer.Number.ToString() +
        " , " + myPlayer.ScoringAverage.ToString();
}
```

## <a name="overloaded-methods"></a>Métodos Sobrecarregados
Uma C++classe ref pública/CX pode conter métodos sobrecarregados, mas o JavaScript tem capacidade limitada de diferenciar métodos sobrecarregados. Por exemplo, ele pode informar a diferença entre essas assinaturas:

```cpp
public ref class NumberClass sealed
{
public:
    int GetNumber(int i);
    int GetNumber(int i, Platform::String^ str);
    double GetNumber(int i, MyData^ d);
};
```

Mas ele não pode determinar a diferença entre elas:

```cpp
int GetNumber(int i);
double GetNumber(double d);
```

Em casos ambíguos, é possível garantir que o JavaScript sempre chame uma sobrecarga específica aplicando o atributo [Windows::Foundation::Metadata::DefaultOverload](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.defaultoverloadattribute) à assinatura do método no arquivo de cabeçalho.

Esse JavaScript sempre chama a sobrecarga atribuída:

```javascript
var nativeObject = new CppComponent.NumberClass();
var num = nativeObject.getNumber(9);
document.getElementById('P4').innerHTML = num;
```

## <a name="net"></a>.NET
As linguagens .NET reconhecem sobrecargas em uma classe de C++referência/CX, assim como em qualquer classe .net.

## <a name="datetime"></a>DateTime
No Windows Runtime, um objeto [Windows::Foundation::DateTime](https://docs.microsoft.com/uwp/api/windows.foundation.datetime) é apenas um inteiro assinado de 64 bits que representa o número de intervalos de 100 nanossegundos antes ou depois de 1° de janeiro de 1601. Não há método em um objeto Windows:Foundation::DateTime. Em vez disso, cada linguagem projeta o DateTime na forma que é nativa a esse idioma: o objeto Date em JavaScript e os tipos System. DateTime e System. DateTimeOffset no .NET.

```cpp
public  ref class MyDateClass sealed
{
public:
    property Windows::Foundation::DateTime TimeStamp;
    void SetTime(Windows::Foundation::DateTime dt)
    {
        auto cal = ref new Windows::Globalization::Calendar();
        cal->SetDateTime(dt);
        TimeStamp = cal->GetDateTime(); // or TimeStamp = dt;
    }
};
```

Quando você passa um valor DateTime de C++/CX para JavaScript, o JavaScript o aceita como um objeto Date e o exibe por padrão como uma cadeia de caracteres de data de forma longa.

```javascript
function SetAndGetDate() {
    var nativeObject = new CppComponent.MyDateClass();

    var myDate = new Date(1956, 4, 21);
    nativeObject.setTime(myDate);

    var myDate2 = nativeObject.timeStamp;

    //prints long form date string
    document.getElementById('P5').innerHTML = myDate2;

}
```

Quando uma linguagem .NET passa um System. DateTime para um C++componente/CX, o método o aceita como um Windows:: Foundation::D atetime. Quando o componente passa um Windows:: Foundation::D ateTime para um método .NET, o método de estrutura o aceita como um DateTimeOffset.

```csharp
private void DateTimeExample()
{
    // Pass a System.DateTime to a C++ method
    // that takes a Windows::Foundation::DateTime
    DateTime dt = DateTime.Now;
    var nativeObject = new CppComponent.MyDateClass();
    nativeObject.SetTime(dt);

    // Retrieve a Windows::Foundation::DateTime as a
    // System.DateTimeOffset
    DateTimeOffset myDate = nativeObject.TimeStamp;

    // Print the long-form date string
    ResultText.Text += myDate.ToString();
}
```

## <a name="collections-and-arrays"></a>Coleções e matrizes
Coleções sempre passam o limite da ABI como identificadores para tipos do Windows Runtime, como Windows::Foundation::Collections::IVector^ e Windows::Foundation::Collections::IMap^. Por exemplo, caso você retorne um identificador para um Platform::Collections::Map, ele é convertido implicitamente em Windows::Foundation::Collections::IMap^. As interfaces de coleção são definidas em um namespace separado das classes C++/CX que fornecem as implementações concretas. As linguagens JavaScript e .NET consomem as interfaces. Para obter mais informações, consulte [Coleções (C++/CX)](https://docs.microsoft.com/cpp/cppcx/collections-c-cx) e [Array e WriteOnlyArray (C++/CX)](https://docs.microsoft.com/cpp/cppcx/array-and-writeonlyarray-c-cx).

## <a name="passing-ivector"></a>Passagem de IVector
```cpp
// Windows::Foundation::Collections::IVector across the ABI.
//#include <algorithm>
//#include <collection.h>
Windows::Foundation::Collections::IVector<int>^ SortVector(Windows::Foundation::Collections::IVector<int>^ vec)
{
    std::sort(begin(vec), end(vec));
    return vec;
}
```

```javascript
var nativeObject = new CppComponent.CollectionExample();
// Call the method to sort an integer array
var inVector = [14, 12, 45, 89, 23];
var outVector = nativeObject.sortVector(inVector);
var result = "Sorted vector to array:";
for (var i = 0; i < outVector.length; i++)
{
    outVector[i];
    result += outVector[i].toString() + ",";
}
document.getElementById('P6').innerHTML = result;
```

As linguagens .NET consideram IVector&lt;T&gt; como IList&lt;T&gt;.

```csharp
private void SortListItems()
{
    IList<int> myList = new List<int>();
    myList.Add(5);
    myList.Add(9);
    myList.Add(17);
    myList.Add(2);

    var nativeObject = new CppComponent.CollectionExample();
    IList<int> mySortedList = nativeObject.SortVector(myList);

    foreach (var item in mySortedList)
    {
        ResultText.Text += " " + item.ToString();
    }
}
```

## <a name="passing-imap"></a>Passagem de IMap
```cpp
// #include <map>
//#include <collection.h>
Windows::Foundation::Collections::IMap<int, Platform::String^> ^GetMap(void)
{    
    Windows::Foundation::Collections::IMap<int, Platform::String^> ^ret =
        ref new Platform::Collections::Map<int, Platform::String^>;
    ret->Insert(1, "One ");
    ret->Insert(2, "Two ");
    ret->Insert(3, "Three ");
    ret->Insert(4, "Four ");
    ret->Insert(5, "Five ");
    return ret;
}
```

```javascript
// Call the method to get the map
var outputMap = nativeObject.getMap();
var mStr = "Map result:" + outputMap.lookup(1) + outputMap.lookup(2)
    + outputMap.lookup(3) + outputMap.lookup(4) + outputMap.lookup(5);
document.getElementById('P7').innerHTML = mStr;
```

As linguagens .NET consideram IMap e IDictionary&lt;K, V&gt;.

```csharp
private void GetDictionary()
{
    var nativeObject = new CppComponent.CollectionExample();
    IDictionary<int, string> d = nativeObject.GetMap();
    ResultText.Text += d[2].ToString();
}
```

## <a name="properties"></a>Properties
Uma classe ref pública em C++extensões de componente/CX expõe membros de dados públicos como propriedades, usando a palavra-chave Property. O conceito é idêntico às propriedades do .NET. Uma propriedade trivial se parece com um membro de dados porque a funcionalidade é implícita. Uma propriedade não trivial tem acessadores get e set explícitos e uma variável privada nomeada que é o "armazenamento de backup" para o valor. Neste exemplo, a variável \_de membro particular propertyAValue é o repositório de backup da propriedadeA. Uma propriedade pode acionar um evento quando o valor é alterado e um aplicativo cliente pode se registrar para receber esse evento.

```cpp
//Properties
public delegate void PropertyChangedHandler(Platform::Object^ sender, int arg);
public ref class PropertyExample  sealed
{
public:
    PropertyExample(){}

    // Event that is fired when PropertyA changes
    event PropertyChangedHandler^ PropertyChangedEvent;

    // Property that has custom setter/getter
    property int PropertyA
    {
        int get() { return m_propertyAValue; }
        void set(int propertyAValue)
        {
            if (propertyAValue != m_propertyAValue)
            {
                m_propertyAValue = propertyAValue;
                // Fire event. (See event example below.)
                PropertyChangedEvent(this, propertyAValue);
            }
        }
    }

    // Trivial get/set property that has a compiler-generated backing store.
    property Platform::String^ PropertyB;

private:
    // Backing store for propertyA.
    int m_propertyAValue;
};
```

```javascript
var nativeObject = new CppComponent.PropertyExample();
var propValue = nativeObject.propertyA;
document.getElementById('P8').innerHTML = propValue;

//Set the string property
nativeObject.propertyB = "What is the meaning of the universe?";
document.getElementById('P9').innerHTML += nativeObject.propertyB;
```

As linguagens do .NET acessam C++Propriedades em um objeto nativo do/CX, assim como fariam em um objeto .net.

```csharp
private void GetAProperty()
{
    // Get the value of the integer property
    // Instantiate the C++ object
    var obj = new CppComponent.PropertyExample();

    // Get an integer property
    var propValue = obj.PropertyA;
    ResultText.Text += propValue.ToString();

    // Set a string property
    obj.PropertyB = " What is the meaning of the universe?";
    ResultText.Text += obj.PropertyB;

}
```

## <a name="delegates-and-events"></a>Representantes e eventos
Um representante é um tipo do Windows Runtime que representa um objeto de função. É possível usar representantes em relação a eventos, retornos de chamada e chamadas de método assíncrono para especificar uma ação a ser realizada posteriormente. Assim como um objeto de função, o representante fornece segurança de tipo, permitindo que o compilador verifique o tipo de retorno e os tipos de parâmetro da função. A declaração de um representante se parece com uma assinatura de função, a implementação é semelhante a uma definição de classe e a invocação se parece com uma chamada de função.

## <a name="adding-an-event-listener"></a>Adição de um ouvinte de eventos
É possível pode usar a palavra-chave event para declarar um membro público de um tipo representante especificado. O código do cliente se inscreve no evento usando os mecanismos padrão fornecidos na linguagem específica.

```cpp
public:
    event SomeHandler^ someEvent;
```

Este exemplo usa o mesmo código C++ da seção de propriedades anterior.

```javascript
function Button_Click() {
    var nativeObj = new CppComponent.PropertyExample();
    // Define an event handler method
    var singlecasthandler = function (ev) {
        document.getElementById('P10').innerHTML = "The button was clicked and the value is " + ev;
    };

    // Subscribe to the event
    nativeObj.onpropertychangedevent = singlecasthandler;

    // Set the value of the property and fire the event
    var propValue = 21;
    nativeObj.propertyA = 2 * propValue;

}
```

Nas linguagens .NET, assinar um evento em um C++ componente é o mesmo que assinar um evento em uma classe .net:

```csharp
//Subscribe to event and call method that causes it to be fired.
private void TestMethod()
{
    var objWithEvent = new CppComponent.PropertyExample();
    objWithEvent.PropertyChangedEvent += objWithEvent_PropertyChangedEvent;

    objWithEvent.PropertyA = 42;
}

//Event handler method
private void objWithEvent_PropertyChangedEvent(object __param0, int __param1)
{
    ResultText.Text = "the event was fired and the result is " +
         __param1.ToString();
}
```

## <a name="adding-multiple-event-listeners-for-one-event"></a>Adição de vários ouvintes de eventos para um evento
JavaScript tem um método addEventListener que permite que vários manipuladores se inscrevam em um único evento.

```cpp
public delegate void SomeHandler(Platform::String^ str);

public ref class LangSample sealed
{
public:
    event SomeHandler^ someEvent;
    property Platform::String^ PropertyA;

    // Method that fires an event
    void FireEvent(Platform::String^ str)
    {
        someEvent(Platform::String::Concat(str, PropertyA->ToString()));
    }
    //...
};
```

```javascript
// Add two event handlers
var multicast1 = function (ev) {
    document.getElementById('P11').innerHTML = "Handler 1: " + ev.target;
};
var multicast2 = function (ev) {
    document.getElementById('P12').innerHTML = "Handler 2: " + ev.target;
};

var nativeObject = new CppComponent.LangSample();
//Subscribe to the same event
nativeObject.addEventListener("someevent", multicast1);
nativeObject.addEventListener("someevent", multicast2);

nativeObject.propertyA = "42";

// This method should fire an event
nativeObject.fireEvent("The answer is ");
```

Em C#, qualquer número de manipuladores de eventos pode se inscrever no evento usando o operador += conforme mostrado no exemplo anterior.

## <a name="enums"></a>Enums
Um Windows Runtime enum em C++/CX é declarado usando enum de classe pública; Ele é semelhante a uma enumeração com escopo definido C++no padrão.

```cpp
public enum class Direction {North, South, East, West};

public ref class EnumExampleClass sealed
{
public:
    property Direction CurrentDirection
    {
        Direction  get(){return m_direction; }
    }

private:
    Direction m_direction;
};
```

Os valores de enumeração são C++passados entre/CX e JavaScript como inteiros. Opcionalmente, você pode declarar um objeto JavaScript que contém os mesmos valores nomeados C++que o enum/CX e usá-lo da seguinte maneira.

```javascript
var Direction = { 0: "North", 1: "South", 2: "East", 3: "West" };
//. . .

var nativeObject = new CppComponent.EnumExampleClass();
var curDirection = nativeObject.currentDirection;
document.getElementById('P13').innerHTML =
Direction[curDirection];
```

C# e Visual Basic têm suporte de linguagem para enumerações. Esses idiomas veem uma C++ classe de enumeração pública assim como veremos um enum .net.

## <a name="asynchronous-methods"></a>Métodos assíncronos
Para consumir métodos assíncronos expostos por outros objetos do Windows Runtime, use a [Classe task (Tempo de Execução de Simultaneidade)](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class). Para obter mais informações, consulte [Paralelismo de tarefas (Tempo de Execução de Simultaneidade)](https://docs.microsoft.com/cpp/parallel/concrt/task-parallelism-concurrency-runtime).

Para implementar métodos assíncronos C++em/CX, use a função [Create\_Async](https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace-functions?view=vs-2017) definida em ppltasks. h. Para obter mais informações, consulte [criando operações assíncronas em C++/CX para aplicativos UWP](https://docs.microsoft.com/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps). Para obter um exemplo, consulte [a explicação sobre C++como criar um componente de Windows Runtime do/CX e chamá C#-lo do JavaScript ou ](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md). As linguagens .NET C++consomem os métodos assíncronos do/CX da mesma forma que fariam com qualquer método assíncrono definido no .net.

## <a name="exceptions"></a>Exceções
É possível acionar qualquer tipo de exceção definido pelo Windows Runtime. Não é possível derivar tipos personalizados de qualquer tipo de exceção do Windows Runtime. No entanto, é possível acionar COMException e fornecer um HRESULT personalizado que pode ser acessado pelo código que captura a exceção. Não existe uma maneira de especificar uma mensagem personalizada em um COMException.

## <a name="debugging-tips"></a>Dicas de depuração
Ao depurar uma solução JavaScript com uma DLL de componente, você pode definir o depurador para habilitar a passagem pelo script ou a passagem pelo código nativo no componente, mas não ambos ao mesmo tempo. Para alterar a configuração, selecione o nó do projeto JavaScript no Gerenciador de Soluções e escolha Propriedades, Depuração, Tipo de Depurador.

Não se esqueça de selecionar os recursos apropriados no designer do pacote. Por exemplo, caso você esteja tentando abrir um arquivo de imagem na biblioteca de imagens do usuário usando as APIs do Windows Runtime, assegure-se de marcar a caixa de seleção Biblioteca de Imagens no painel Funcionalidades do designer de manifesto.

Caso o código JavaScript aparentemente não reconheça as propriedades públicas ou os métodos no componente, assegure-se de que, no JavaScript, você esteja seguindo o uso de maiúsculas camel. Por exemplo, o método C++LogCalc/CX deve ser referenciado como LogCalc em JavaScript.

Se você remover um C++projeto de componente do Windows Runtime/CX de uma solução, também deverá remover manualmente a referência do projeto do projeto JavaScript. Deixar de fazer isso impede operações de depuração ou compilação subsequentes. Caso necessário, é possível adicionar uma referência de assembly à DLL.

## <a name="related-topics"></a>Tópicos relacionados
* [Passo a passos de C++criar um componente de Windows Runtime do/CX e chamá-lo do JavaScript ouC#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md)
