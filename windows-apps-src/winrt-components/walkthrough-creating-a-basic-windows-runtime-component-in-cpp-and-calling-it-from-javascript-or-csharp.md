---
author: msatranjr
title: Criar um componente do Windows Runtime em C++/CX e chamando-o por JavaScript ou C#
description: Este procedimento passo a passo mostra como criar uma DLL de componente do Tempo de Execução do Windows básica que pode ser chamada em JavaScript, C# ou Visual Basic.
ms.assetid: 764CD9C6-3565-4DFF-88D7-D92185C7E452
ms.author: misatran
ms.date: 05/14/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 79c2bd63c326b90b0b5d6e5007c4610f22bff670
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/24/2018
ms.locfileid: "2830475"
---
# <a name="walkthrough-creating-a-windows-runtime-component-in-ccx-and-calling-it-from-javascript-or-c"></a>Procedimento passo a passo: criando um componente do Windows Runtime em C++/CX e chamando-o em JavaScript ou C#
> [!NOTE]
> Este tópico existe para ajudar você na manutenção do seu aplicativo C++/CX. Recomendamos que você use [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) para novos aplicativos. C++/WinRT é uma projeção de linguagem C++17 completamente moderna e padrão para APIs do Windows Runtime (WinRT), implementada como uma biblioteca com base em cabeçalho e arquivo, projetada para fornecer acesso de primeira classe à API moderna do Windows. Para saber como criar um componente de tempo de execução do Windows usando C + + / WinRT, consulte [originar eventos no C + + / WinRT](../cpp-and-winrt-apis/author-events.md).

Este passo a passo mostra como criar uma DLL de componente do Windows Runtime básico que pode ser chamada do JavaScript, C# ou Visual Basic. Antes de começar este procedimento passo a passo, assegure-se de que você compreendeu conceitos como a Abstract Binary Interface (ABI), as classes ref e as extensões de componente do Visual C++ que facilitam o trabalho com classes ref. Para obter mais informações, consulte [Criação de componentes do Tempo de Execução do Windows em C++](creating-windows-runtime-components-in-cpp.md) e [Referência da linguagem Visual C++ (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx).

## <a name="creating-the-c-component-dll"></a>Criação da DLL do componente C++
Neste exemplo, criamos o projeto de componente primeiro, mas você pode criar o projeto JavaScript primeiro. A ordem não importa.

A classe principal do componente contém exemplos de definições de propriedade e método, além de uma declaração de evento. Eles são fornecidos apenas para mostrar como isso é feito. Eles não são necessários e, neste exemplo, substituiremos todos os códigos gerados pelo próprio código.

## **<a name="to-create-the-c-component-project"></a>Para criar o projeto de componente C++**
Na barra de menus do Visual Studio, escolha **Arquivo, Novo, Projeto**.

Na caixa de diálogo **Novo Projeto**, no painel à esquerda, expanda **Visual C++** e selecione o nó de aplicativos Universais do Windows.

No painel central, selecione **Componente do Tempo de Execução do Windows** e nomeie o projeto WinRT\_CPP.

Escolha o botão **OK**.

## **<a name="to-add-an-activatable-class-to-the-component"></a>Para adicionar uma classe ativável ao componente**
Uma classe ativável é aquela que código cliente pode criar usando uma expressão **new** (**New** em Visual Basic ou **ref new** em C++). Em seu componente, declare-a como **classe ref pública selado**. Na verdade, os arquivos Class1.h e .cpp já têm uma classe ref. É possível alterar o nome, mas neste exemplo usaremos o nome padrão – Class1. É possível definir classes ref adicionais ou classes regulares no componente caso elas sejam necessárias. Para obter mais informações sobre classes ref, consulte [Sistema de tipos (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822.aspx).

Adicione estas diretivas \#include a Class1.h:

```cpp
#include <collection.h>
#include <ppl.h>
#include <amp.h>
#include <amp_math.h>
```

collection.h é o arquivo de cabeçalho para classes concretas C++, como a classe Platform::Collections::Vector e a classe Platform::Collections::Map, que implementam interfaces de linguagem neutra definidas pelo Windows Runtime. Os cabeçalhos amp são usados para executar computações na GPU. Eles não têm equivalentes de Tempo de Execução do Windows, e tudo bem quanto a isso porque eles são privados. Em geral, por questões de desempenho você deve usar o código C++ ISO e as bibliotecas padrão internamente dentro do componente; é apenas a interface de Tempo de Execução do Windows que deve ser expressada em tipos de Tempo de Execução do Windows.

## <a name="to-add-a-delegate-at-namespace-scope"></a>Para adicionar um representante no escopo de namespace
Representante é um constructo que define os parâmetros e o tipo de retorno para métodos. Evento é uma instância de um determinado tipo de representante, e qualquer método de manipulador de eventos que assine o evento deve ter a assinatura especificada no representante. O código a seguir define um tipo de representante que utiliza um int e retorna void. Em seguida, o código declara um evento público desse tipo; isso permite que o código do cliente forneça métodos invocados quando o evento é acionado.

Adicione a declaração do representante a seguir no escopo de namespace em Class1.h, pouco antes da declaração de Class1.

```cpp
public delegate void PrimeFoundHandler(int result);
```

Caso o código não se alinhe corretamente quando você o cola no Visual Studio, basta pressionar Ctrl+K+D para corrigir o recuo de todo o arquivo.

## <a name="to-add-the-public-members"></a>Para adicionar membros públicos
A classe expõe três métodos públicos e um evento público. O primeiro método é síncrono porque ele sempre é executado muito rapidamente. Como os outros dois métodos podem levar algum tempo, eles são assíncronos, de maneira que eles não bloqueiam o thread de interface do usuário. Esses métodos retornam IAsyncOperationWithProgress e IAsyncActionWithProgress. O primeiro define um método async que retorna um resultado e o último define um método async que retorna void. Essas interfaces também permitem que o código do cliente receba atualizações sobre o progresso da operação.

```cpp
public:

        // Synchronous method.
        Windows::Foundation::Collections::IVector<double>^  ComputeResult(double input);

        // Asynchronous methods
        Windows::Foundation::IAsyncOperationWithProgress<Windows::Foundation::Collections::IVector<int>^, double>^
            GetPrimesOrdered(int first, int last);
        Windows::Foundation::IAsyncActionWithProgress<double>^ GetPrimesUnordered(int first, int last);

        // Event whose type is a delegate "class"
        event PrimeFoundHandler^ primeFoundEvent;

```
## <a name="to-add-the-private-members"></a>Para adicionar os membros privados
A classe contém três membros privados: dois métodos auxiliares para os cálculos numéricos e um objeto CoreDispatcher usado para realizar marshaling das invocações de evento em threads de trabalho de volta para o thread de interface do usuário.

```cpp
private:
        bool is_prime(int n);
        Windows::UI::Core::CoreDispatcher^ m_dispatcher;
```

## <a name="to-add-the-header-and-namespace-directives"></a>Para adicionar as diretivas de cabeçalho e namespace
Em Class1.cpp, adicione estas diretivas #include:

```cpp
#include <ppltasks.h>
#include <concurrent_vector.h>
```

Agora adicione estas instruções using para extrair os namespaces necessários:

```cpp
using namespace concurrency;
using namespace Platform::Collections;
using namespace Windows::Foundation::Collections;
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
```

## <a name="to-add-the-implementation-for-computeresult"></a>Para adicionar a implementação de ComputeResult
Em Class1.cpp, adicione a implementação do método a seguir. Esse método é executado de maneira síncrona no thread de chamada, mas é muito rápido porque usa AMP C++ para paralelizar a computação na GPU. Para obter mais informações, consulte Visão geral de AMP C++. Os resultados são anexados a um tipo concreto Platform::Collections::Vector<T>, que é implicitamente convertido em um Windows::Foundation::Collections::IVector<T> quando ele é retornado.

```cpp
//Public API
IVector<double>^ Class1::ComputeResult(double input)
{
    // Implement your function in ISO C++ or
    // call into your C++ lib or DLL here. This example uses AMP.
    float numbers[] = { 1.0, 10.0, 60.0, 100.0, 600.0, 10000.0 };
    array_view<float, 1> logs(6, numbers);

    // See http://msdn.microsoft.com/en-us/library/hh305254.aspx
    parallel_for_each(
        logs.extent,
        [=] (index<1> idx) restrict(amp)
    {
        logs[idx] = concurrency::fast_math::log10(logs[idx]);
    }
    );

    // Return a Windows Runtime-compatible type across the ABI
    auto res = ref new Vector<double>();
    int len = safe_cast<int>(logs.extent.size());
    for(int i = 0; i < len; i++)
    {      
        res->Append(logs[i]);
    }

    // res is implicitly cast to IVector<double>
    return res;
}
```
## <a name="to-add-the-implementation-for-getprimesordered-and-its-helper-method"></a>Para adicionar a implementação de GetPrimesOrdered e o método auxiliar
Em Class1.cpp, adicione as implementações de GetPrimesOrdered e o método auxiliar is_prime. GetPrimesOrdered usa uma classe concurrent_vector e um loop de função parallel_for para dividir o trabalho e usar os recursos máximos do computador no qual o programa está em execução para produzir resultados. Depois que os resultados são calculados, armazenados e classificados, eles são adicionados a um Platform::Collections::Vector<T> e retornados como Windows::Foundation::Collections::IVector<T> ao código do cliente.

Observe o código do gerador de relatórios de progresso, que permite que o cliente vincule uma barra de progresso ou outra interface do usuário para mostrar ao usuário quanto mais a operação irá demorar. Relatórios de progresso têm um custo. Um evento deve ser acionado no lado do componente e manipulado no thread da interface do usuário, e o valor do progresso deve ser armazenado em cada iteração. Uma maneira de minimizar o custo é limitando a frequência na qual um evento de progresso é acionado. Caso o custo ainda assim seja proibitivo ou caso você não consiga estimar a duração da operação, leve em consideração usar um anel de progresso, que mostra que uma operação está em andamento, mas não mostra o tempo restante até a conclusão.

```cpp
// Determines whether the input value is prime.
bool Class1::is_prime(int n)
{
    if (n < 2)
        return false;
    for (int i = 2; i < n; ++i)
    {
        if ((n % i) == 0)
            return false;
    }
    return true;
}

// This method computes all primes, orders them, then returns the ordered results.
IAsyncOperationWithProgress<IVector<int>^, double>^ Class1::GetPrimesOrdered(int first, int last)
{
    return create_async([this, first, last]
    (progress_reporter<double> reporter) -> IVector<int>^ {
        // Ensure that the input values are in range.
        if (first < 0 || last < 0) {
            throw ref new InvalidArgumentException();
        }
        // Perform the computation in parallel.
        concurrent_vector<int> primes;
        long operation = 0;
        long range = last - first + 1;
        double lastPercent = 0.0;

        parallel_for(first, last + 1, [this, &primes, &operation,
            range, &lastPercent, reporter](int n) {

                // Increment and store the number of times the parallel
                // loop has been called on all threads combined. There
                // is a performance cost to maintaining a count, and
                // passing the delegate back to the UI thread, but it's
                // necessary if we want to display a determinate progress
                // bar that goes from 0 to 100%. We can avoid the cost by
                // setting the ProgressBar IsDeterminate property to false
                // or by using a ProgressRing.
                if(InterlockedIncrement(&operation) % 100 == 0)
                {
                    reporter.report(100.0 * operation / range);
                }

                // If the value is prime, add it to the local vector.
                if (is_prime(n)) {
                    primes.push_back(n);
                }
        });

        // Sort the results.
        std::sort(begin(primes), end(primes), std::less<int>());        
        reporter.report(100.0);

        // Copy the results to a Vector object, which is
        // implicitly converted to the IVector return type. IVector
        // makes collections of data available to other
        // Windows Runtime components.
        return ref new Vector<int>(primes.begin(), primes.end());
    });
}
```

## <a name="to-add-the-implementation-for-getprimesunordered"></a>Para adicionar a implementação de GetPrimesUnordered
A última etapa para criar o componente C++ é adicionar a implementação do GetPrimesUnordered em Class1.cpp. Esse método retorna cada resultado como encontrado, sem esperar todos os resultados serem encontrados. Cada resultado é retornado no manipulador de eventos e exibido na interface do usuário em tempo real. Mais uma vez, observe que um gerador de relatórios de progresso é usado. Esse método também usa o método auxiliar is_prime.

```cpp
// This method returns no value. Instead, it fires an event each time a
// prime is found, and passes the prime through the event.
// It also passes progress info.
IAsyncActionWithProgress<double>^ Class1::GetPrimesUnordered(int first, int last)
{

    auto window = Windows::UI::Core::CoreWindow::GetForCurrentThread();
    m_dispatcher = window->Dispatcher;


    return create_async([this, first, last](progress_reporter<double> reporter) {

        // Ensure that the input values are in range.
        if (first < 0 || last < 0) {
            throw ref new InvalidArgumentException();
        }

        // In this particular example, we don't actually use this to store
        // results since we pass results one at a time directly back to
        // UI as they are found. However, we have to provide this variable
        // as a parameter to parallel_for.
        concurrent_vector<int> primes;
        long operation = 0;
        long range = last - first + 1;
        double lastPercent = 0.0;

        // Perform the computation in parallel.
        parallel_for(first, last + 1,
            [this, &primes, &operation, range, &lastPercent, reporter](int n)
        {
            // Store the number of times the parallel loop has been called  
            // on all threads combined. See comment in previous method.
            if(InterlockedIncrement(&operation) % 100 == 0)
            {
                reporter.report(100.0 * operation / range);
            }

            // If the value is prime, pass it immediately to the UI thread.
            if (is_prime(n))
            {                
                // Since this code is probably running on a worker
                // thread, and we are passing the data back to the
                // UI thread, we have to use a CoreDispatcher object.
                m_dispatcher->RunAsync( CoreDispatcherPriority::Normal,
                    ref new DispatchedHandler([this, n, operation, range]()
                {
                    this->primeFoundEvent(n);

                }, Platform::CallbackContext::Any));

            }
        });
        reporter.report(100.0);
    });
}
```

## <a name="creating-a-javascript-client-app"></a>Criação de um aplicativo cliente JavaScript
Caso queira criar um cliente em C#, você pode ignorar esta seção.

## <a name="to-create-a-javascript-project"></a>Para criar um projeto JavaScript
No Gerenciador de Soluções, abra o menu de atalho do nó Solução e escolha **Adicionar, Novo Projeto**.

Expanda JavaScript (ele pode estar aninhado em **Outros Idiomas**) e escolha **Aplicativo em Branco (Universal do Windows)**.

Aceite o nome padrão – App1 – escolhendo o botão **OK**.

Abra o menu de atalho do nó do projeto App1 e escolha **Definir como Projeto de Inicialização**.

Adicione uma referência de projeto para WinRT_CPP:

Abra o menu de atalho do nó Referências e escolha **Adicionar Referência**.

No painel à esquerda da caixa de diálogo Gerenciador de Referências, selecione **Projetos** e **Solução**.

No painel central, selecione WinRT_CPP e escolha o botão **OK**

## <a name="to-add-the-html-that-invokes-the-javascript-event-handlers"></a>Para adicionar o HTML que invoca os manipuladores de eventos JavaScript
Cole este HTML no nó <body> da página default.html:

```HTML
<div id="LogButtonDiv">
     <button id="logButton">Logarithms using AMP</button>
 </div>
 <div id="LogResultDiv">
     <p id="logResult"></p>
 </div>
 <div id="OrderedPrimeButtonDiv">
     <button id="orderedPrimeButton">Primes using parallel_for with sort</button>
 </div>
 <div id="OrderedPrimeProgress">
     <progress id="OrderedPrimesProgressBar" value="0" max="100"></progress>
 </div>
 <div id="OrderedPrimeResultDiv">
     <p id="orderedPrimes">
         Primes found (ordered):
     </p>
 </div>
 <div id="UnorderedPrimeButtonDiv">
     <button id="ButtonUnordered">Primes returned as they are produced.</button>
 </div>
 <div id="UnorderedPrimeDiv">
     <progress id="UnorderedPrimesProgressBar" value="0" max="100"></progress>
 </div>
 <div id="UnorderedPrime">
     <p id="unorderedPrimes">
         Primes found (unordered):
     </p>
 </div>
 <div id="ClearDiv">
     <button id="Button_Clear">Clear</button>
 </div>
```

## <a name="to-add-styles"></a>Para adicionar estilos
Em default.css, remova o estilo do corpo e adicione estes estilos:

```css
#LogButtonDiv {
border: orange solid 1px;
-ms-grid-row: 1; /* default is 1 */
-ms-grid-column: 1; /* default is 1 */
}
#LogResultDiv {
background: black;
border: red solid 1px;
-ms-grid-row: 1;
-ms-grid-column: 2;
}
#UnorderedPrimeButtonDiv, #OrderedPrimeButtonDiv {
border: orange solid 1px;
-ms-grid-row: 2;   
-ms-grid-column:1;
}
#UnorderedPrimeProgress, #OrderedPrimeProgress {
border: red solid 1px;
-ms-grid-column-span: 2;
height: 40px;
}
#UnorderedPrimeResult, #OrderedPrimeResult {
border: red solid 1px;
font-size:smaller;
-ms-grid-row: 2;
-ms-grid-column: 3;
-ms-overflow-style:scrollbar;
}
```

## <a name="to-add-the-javascript-event-handlers-that-call-into-the-component-dll"></a>Para adicionar os manipuladores de eventos JavaScript que chamam a DLL do componente
Adicione as funções a seguir ao final do arquivo default.js. Essas funções são chamadas quando os botões na página principal são escolhidos. Observe como JavaScript ativa a classe C++ e, em seguida, chama os métodos e usa os valores de retorno para popular os rótulos HTML.

```JavaScript
var nativeObject = new WinRT_CPP.Class1();

function LogButton_Click() {

    var val = nativeObject.computeResult(0);
    var result = "";

    for (i = 0; i < val.length; i++) {
        result += val[i] + "<br/>";
    }

    document.getElementById('logResult').innerHTML = result;
}

function ButtonOrdered_Click() {
    document.getElementById('orderedPrimes').innerHTML = "Primes found (ordered): ";

    nativeObject.getPrimesOrdered(2, 10000).then(
        function (v) {
            for (var i = 0; i < v.length; i++)
                document.getElementById('orderedPrimes').innerHTML += v[i] + " ";
        },
        function (error) {
            document.getElementById('orderedPrimes').innerHTML += " " + error.description;
        },
        function (p) {
            var progressBar = document.getElementById("OrderedPrimesProgressBar");
            progressBar.value = p;
        });
}

function ButtonUnordered_Click() {
    document.getElementById('unorderedPrimes').innerHTML = "Primes found (unordered): ";
    nativeObject.onprimefoundevent = handler_unordered;

    nativeObject.getPrimesUnordered(2, 10000).then(
        function () { },
        function (error) {
            document.getElementById("unorderedPrimes").innerHTML += " " + error.description;
        },
        function (p) {
            var progressBar = document.getElementById("UnorderedPrimesProgressBar");
            progressBar.value = p;
        });
}

var handler_unordered = function (n) {
    document.getElementById('unorderedPrimes').innerHTML += n.target.toString() + " ";
};

function ButtonClear_Click() {

    document.getElementById('logResult').innerHTML = "";
    document.getElementById("unorderedPrimes").innerHTML = "";
    document.getElementById('orderedPrimes').innerHTML = "";
    document.getElementById("UnorderedPrimesProgressBar").value = 0;
    document.getElementById("OrderedPrimesProgressBar").value = 0;
}
```

Adicione um código para incluir os ouvintes de eventos, substituindo a chamada existente para WinJS.UI.processAll em app.onactivated em default.js com o código a seguir que implementa o registro de evento em um bloco then. Para obter uma explicação detalhada sobre isso, consulte Criar um aplicativo "Hello World" (JS).

```JavaScript
args.setPromise(WinJS.UI.processAll().then( function completed() {
    var logButton = document.getElementById("logButton");
    logButton.addEventListener("click", LogButton_Click, false);
    var orderedPrimeButton = document.getElementById("orderedPrimeButton");
    orderedPrimeButton.addEventListener("click", ButtonOrdered_Click, false);
    var buttonUnordered = document.getElementById("ButtonUnordered");
    buttonUnordered.addEventListener("click", ButtonUnordered_Click, false);
    var buttonClear = document.getElementById("Button_Clear");
    buttonClear.addEventListener("click", ButtonClear_Click, false);
}));
```

Pressione F5 para executar o aplicativo.

## <a name="creating-a-c-client-app"></a>Criação de um aplicativo cliente do C#

## <a name="to-create-a-c-project"></a>Para criar um projeto C#
No Gerenciador de Soluções, abra o menu de atalho do nó Solution e escolha **Adicionar, Novo Projeto**.

Expanda Visual C# (ele pode estar aninhado em **Outros Idiomas**), selecione **Windows** e **Universal** no painel à esquerda e selecione **Aplicativo em Branco** no painel intermediário.

Nomeie esse aplicativo CS_Client e escolha o botão **OK**.

Abra o menu de atalho do nó do projeto CS_Client e escolha **Definir como Projeto de Inicialização**.

Adicione uma referência de projeto para WinRT_CPP:

Abra o menu de atalho do nó **Referências** e escolha **Adicionar Referência**.

No painel à esquerda da caixa de diálogo **Gerenciador de Referências**, selecione **Projetos** e **Solução**.

No painel central, selecione WinRT_CPP e escolha o botão **OK**.

## <a name="to-add-the-xaml-that-defines-the-user-interface"></a>Para adicionar o XAML que define a interface do usuário
Copie o código a seguir para o elemento Grid em MainPage.xaml.

```xaml
<ScrollViewer>
            <StackPanel Width="1400">

                <Button x:Name="Button1" Width="340" Height="50"  Margin="0,20,20,20" Content="Synchronous Logarithm Calculation" FontSize="16" Click="Button1_Click_1"/>
                <TextBlock x:Name="Result1" Height="100" FontSize="14"></TextBlock>
            <Button x:Name="PrimesOrderedButton" Content="Prime Numbers Ordered" FontSize="16" Width="340" Height="50" Margin="0,20,20,20" Click="PrimesOrderedButton_Click_1"></Button>
            <ProgressBar x:Name="PrimesOrderedProgress" IsIndeterminate="false" Height="40"></ProgressBar>
                <TextBlock x:Name="PrimesOrderedResult" MinHeight="100" FontSize="10" TextWrapping="Wrap"></TextBlock>
            <Button x:Name="PrimesUnOrderedButton" Width="340" Height="50" Margin="0,20,20,20" Click="PrimesUnOrderedButton_Click_1" Content="Prime Numbers Unordered" FontSize="16"></Button>
            <ProgressBar x:Name="PrimesUnOrderedProgress" IsIndeterminate="false" Height="40" ></ProgressBar>
            <TextBlock x:Name="PrimesUnOrderedResult" MinHeight="100" FontSize="10" TextWrapping="Wrap"></TextBlock>

            <Button x:Name="Clear_Button" Content="Clear" HorizontalAlignment="Left" Margin="0,20,20,20" VerticalAlignment="Top" Width="341" Click="Clear_Button_Click" FontSize="16"/>
        </StackPanel>
</ScrollViewer>
```

## <a name="to-add-the-event-handlers-for-the-buttons"></a>Para adicionar os manipuladores de eventos para os botões
No Gerenciador de Soluções, abra MainPage.xaml.cs. (O arquivo pode estar aninhado em MainPage.xaml.) Adicione uma diretiva using para System.Text o manipulador de eventos para o cálculo de logaritmo na classe MainPage.

```csharp
private void Button1_Click_1(object sender, RoutedEventArgs e)
{
    // Create the object
    var nativeObject = new WinRT_CPP.Class1();

    // Call the synchronous method. val is an IList that
    // contains the results.
    var val = nativeObject.ComputeResult(0);
    StringBuilder result = new StringBuilder();
    foreach (var v in val)
    {
        result.Append(v).Append(System.Environment.NewLine);
    }
    this.Result1.Text = result.ToString();
}
```

Adicione o manipulador de eventos para o resultado ordenado:

```csharp
async private void PrimesOrderedButton_Click_1(object sender, RoutedEventArgs e)
{
    var nativeObject = new WinRT_CPP.Class1();

    StringBuilder sb = new StringBuilder();
    sb.Append("Primes found (ordered): ");

    PrimesOrderedResult.Text = sb.ToString();

    // Call the asynchronous method
    var asyncOp = nativeObject.GetPrimesOrdered(2, 100000);

    // Before awaiting, provide a lambda or named method
    // to handle the Progress event that is fired at regular
    // intervals by the asyncOp object. This handler updates
    // the progress bar in the UI.
    asyncOp.Progress = (asyncInfo, progress) =>
        {
            PrimesOrderedProgress.Value = progress;
        };

    // Wait for the operation to complete
    var asyncResult = await asyncOp;

    // Convert the results to strings
    foreach (var result in asyncResult)
    {
        sb.Append(result).Append(" ");
    }

    // Display the results
    PrimesOrderedResult.Text = sb.ToString();
}
```

Adicione o manipulador de eventos para o resultado não ordenado e para o botão que limpa os resultados de maneira que seja possível reexecutar o código.

```csharp
private void PrimesUnOrderedButton_Click_1(object sender, RoutedEventArgs e)
{
    var nativeObject = new WinRT_CPP.Class1();

    StringBuilder sb = new StringBuilder();
    sb.Append("Primes found (unordered): ");
    PrimesUnOrderedResult.Text = sb.ToString();

    // primeFoundEvent is a user-defined event in nativeObject
    // It passes the results back to this thread as they are produced
    // and the event handler that we define here immediately displays them.
    nativeObject.primeFoundEvent += (n) =>
    {
        sb.Append(n.ToString()).Append(" ");
        PrimesUnOrderedResult.Text = sb.ToString();
    };

    // Call the async method.
    var asyncResult = nativeObject.GetPrimesUnordered(2, 100000);

    // Provide a handler for the Progress event that the asyncResult
    // object fires at regular intervals. This handler updates the progress bar.
    asyncResult.Progress += (asyncInfo, progress) =>
        {
            PrimesUnOrderedProgress.Value = progress;
        };
}

private void Clear_Button_Click(object sender, RoutedEventArgs e)
{
    PrimesOrderedProgress.Value = 0;
    PrimesUnOrderedProgress.Value = 0;
    PrimesUnOrderedResult.Text = "";
    PrimesOrderedResult.Text = "";
    Result1.Text = "";
}
```

## <a name="running-the-app"></a>Execução do aplicativo
Selecione o projeto C# ou JavaScript como projeto de inicialização abrindo o menu de atalho do nó do projeto no Gerenciador de Soluções e escolhendo **Definir como Projeto de Inicialização**. Em seguira, pressione F5 para executar com depuração ou Ctrl+F5 para executar sem depuração.

## <a name="inspecting-your-component-in-object-browser-optional"></a>Inspeção do componente no Pesquisador de Objetos (opcional)
No Pesquisador de Objetos, é possível inspecionar todos os tipos de Tempo de Execução do Windows definidos em arquivos .winmd. Isso inclui os tipos nos namespaces Platform e padrão. No entanto, como os tipos no namespace Platform::Collections são definidos no arquivo de cabeçalho collections.h, e não em um arquivo winmd, eles não são exibidos no Pesquisador de Objetos.

## **<a name="to-inspect-a-component"></a>Para inspecionar um componente**
Na barra de menus, escolha **Exibir, Pesquisador de Objetos** (Ctrl+Alt+J).

No painel à esquerda do Pesquisador de Objetos, expanda o nó WinRT\_CPP para mostrar os tipos e os métodos definidos no componente.

## <a name="debugging-tips"></a>Dicas de depuração
Para obter uma experiência de depuração melhor, baixe os símbolos de depuração dos servidores de símbolos Microsoft públicos:

## **<a name="to-download-debugging-symbols"></a>Para baixar símbolos de depuração**
Na barra de menus, escolha **Ferramentas, Opções**.

Na caixa de diálogo **Opções**, expanda **Depuração** e selecione **Símbolos**.

Selecione **Servidores de Símbolos Microsoft** e escolha o botão **OK**.

Pode levar algum tempo para baixar os símbolos pela primeira vez. Para aumentar o desempenho na próxima vez em que você pressionar F5, especifique um diretório local no qual armazenar em cache os símbolos.

Ao depurar uma solução JavaScript com uma DLL de componente, você pode definir o depurador para habilitar a passagem pelo script ou a passagem pelo código nativo no componente, mas não ambos ao mesmo tempo. Para alterar a configuração, abra o menu de atalho do nó do projeto JavaScript no Gerenciador de Soluções e escolha **Propriedades, Depuração, Tipo de Depurador**.

Não se esqueça de selecionar os recursos apropriados no designer do pacote. Você pode abrir o designer de pacote abrindo o arquivo Package.appxmanifest. Por exemplo, caso você esteja tentando acessar arquivos programaticamente na pasta Imagens, não se esqueça de marcar a caixa de seleção **Biblioteca de Imagens** no painel **Recursos** do designer de pacotes.

Caso o código JavaScript não reconheça as propriedades públicas ou os métodos no componente, assegure-se de que, no JavaScript, você esteja seguindo o uso de maiúsculas camel. Por exemplo, o método `ComputeResult` C++ deve ser referenciado como `computeResult` em JavaScript.

Caso remova um projeto de componente do Tempo de Execução do Windows C++ de uma solução, você também deve remover manualmente a referência do projeto JavaScript. Deixar de fazer isso impede operações de depuração ou compilação subsequentes. Caso necessário, é possível adicionar uma referência de assembly à DLL.

## <a name="related-topics"></a>Tópicos relacionados
* [Criando componentes do Windows Runtime em C++/CX](creating-windows-runtime-components-in-cpp.md)
