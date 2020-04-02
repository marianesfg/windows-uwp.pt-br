---
title: Passo a passos de C++criar um componente de Windows Runtime do/CX e chamá-lo do JavaScript ouC#
description: Este passo a passo mostra como criar uma DLL básica do componente do Windows Runtime que pode ser chamada do JavaScript, do C# ou do Visual Basic.
ms.assetid: 764CD9C6-3565-4DFF-88D7-D92185C7E452
ms.date: 05/14/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1b4cd9bb5921209be852e183e1fa7a93ea18816a
ms.sourcegitcommit: 5618242614997045593821fdbe5ed8878fd8c01e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80578681"
---
# <a name="walkthrough-of-creating-a-ccx-windows-runtime-component-and-calling-it-from-javascript-or-c"></a>Passo a passos de C++criar um componente de Windows Runtime do/CX e chamá-lo do JavaScript ouC#

> [!NOTE]
> Este tópico existe para ajudar você na manutenção do seu aplicativo C++/CX. Recomendamos que você use [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) para novos aplicativos. C++/WinRT é uma projeção de linguagem C++17 completamente moderna e padrão para APIs do Windows Runtime (WinRT), implementada como uma biblioteca com base em cabeçalho e arquivo, projetada para fornecer acesso de primeira classe à API moderna do Windows. Para saber como criar um componente de Windows Runtime usando C++o/WinRT, consulte [criar eventos C++em/WinRT](../cpp-and-winrt-apis/author-events.md).

Este passo a passo mostra como criar uma DLL básica do componente do Windows Runtime que pode ser chamada do JavaScript, do C# ou do Visual Basic. Antes de iniciar esta explicação passo a passo, verifique se você entende conceitos como a interface binária abstrata (ABI) classes de referência e as extensões de componentes Visual C++ que facilitam o trabalho com classes de referência. Para obter mais informações, consulte [componentes de C++Windows Runtime com o/CX e a](creating-windows-runtime-components-in-cpp.md) referência de [linguagem Visual C++ (C++/CX)](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx).

## <a name="creating-the-c-component-dll"></a>Criando a DLL do componente em C++
Neste exemplo, criaremos o projeto do componente primeiro, mas você pode criar o projeto em JavaScript primeiro. A ordem não importa.

Observe que a classe principal do componente contém exemplos de definições de propriedade e de método e uma declaração de evento. Eles são fornecidos apenas para mostrar como é feito. Eles não são necessários e, neste exemplo, substituiremos todo o código gerado pelo nosso próprio código.

### <a name="to-create-the-c-component-project"></a>**Para criar o C++ projeto de componente**
1. Na barra de menus do Visual Studio, escolha **Arquivo, Novo, Projeto**.

2. Na caixa de diálogo **Novo Projeto**, no painel à esquerda, expanda **Visual C++** e selecione o nó de aplicativos Universais do Windows.

3. No painel central, selecione **Windows Runtime componente** e, em seguida, nomeie o projeto WINRT\_cpp.

4. Clique no botão **OK**.

## <a name="to-add-an-activatable-class-to-the-component"></a>**Para adicionar uma classe ativável ao componente**
Uma classe ativável é aquela que código cliente pode criar usando uma expressão **new** (**New** em Visual Basic ou **ref new** em C++). Em seu componente, declare-a como **classe ref pública selado**. Na verdade, os arquivos Class1.h e .cpp já têm uma classe de referência. É possível alterar o nome, mas neste exemplo usaremos o nome padrão – Class1. Você pode definir classes de referência adicionais ou classes regulares em seu componente, caso sejam necessárias. Para obter mais informações sobre classes ref, consulte [Sistema de tipos (C++/CX)](https://docs.microsoft.com/cpp/cppcx/type-system-c-cx).

Adicione essas \#diretivas de inclusão a Class1. h:

```cpp
#include <collection.h>
#include <ppl.h>
#include <amp.h>
#include <amp_math.h>
```

collection.h é o arquivo de cabeçalho para classes concretas C++, como a classe Platform::Collections::Vector e a classe Platform::Collections::Map, que implementam interfaces de linguagem neutra definidas pelo Windows Runtime. Os cabeçalhos amp são usados para executar computações na GPU. Eles não têm equivalentes de Windows Runtime, e tudo bem quanto a isso porque eles são privados. Em geral, por questões de desempenho você deve usar o código C++ ISO e as bibliotecas padrão internamente dentro do componente; é apenas a interface de Windows Runtime que deve ser expressada em tipos de Windows Runtime.

## <a name="to-add-a-delegate-at-namespace-scope"></a>Para adicionar um delegado no escopo do namespace
Um delegado é uma construção que define os parâmetros e o tipo de retorno para métodos. Um evento é uma instância de um tipo específico de delegado, e qualquer método manipulador de eventos que assinar o evento deverá ter a assinatura que é especificada no delegado. O código a seguir define um tipo de representante que utiliza um int e retorna void. Em seguida, o código declara um evento público desse tipo; isso permite que o código do cliente forneça métodos invocados quando o evento é acionado.

{6&gt;Adicione a declaração de delegado a seguir no escopo do namespace em Class1.h, exatamente antes da declaração Class1.&lt;6}

```cpp
public delegate void PrimeFoundHandler(int result);
```

Se o código não for alinhado corretamente quando você o colar no Visual Studio, basta pressionar Ctrl+K+D para corrigir o recuo do arquivo inteiro.

## <a name="to-add-the-public-members"></a>Para adicionar os membros públicos
A classe expõe três métodos públicos e um evento público. O primeiro método é síncrono porque sempre é executado muito rápido. Como os outros dois métodos podem levar algum tempo, eles são assíncronos, de modo que não bloqueiam o thread de interface do usuário. Esses métodos retornam IAsyncOperationWithProgress e IAsyncActionWithProgress. O primeiro define um método assíncrono que retorna um resultado, e o segundo define um método assíncrono que retorna void. Essas interfaces também permitem que o código cliente receba atualizações durante o progresso da operação.

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
## <a name="to-add-the-private-members"></a>Para adicionar os membros particulares
A classe contém três membros particulares: dois métodos auxiliares para os cálculos numéricos e um objeto CoreDispatcher que é usado para realizar marshaling das invocações de eventos dos threads de trabalho para o thread de interface do usuário.

```cpp
private:
        bool is_prime(int n);
        Windows::UI::Core::CoreDispatcher^ m_dispatcher;
```

### <a name="to-add-the-header-and-namespace-directives"></a>Para adicionar as diretivas de cabeçalho e namespace
1. Em Class1.cpp, adicione estas diretivas #include:

```cpp
#include <ppltasks.h>
#include <concurrent_vector.h>
```

2. Agora adicione estas instruções using para extrair os namespaces necessários:

```cpp
using namespace concurrency;
using namespace Platform::Collections;
using namespace Windows::Foundation::Collections;
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
```

## <a name="to-add-the-implementation-for-computeresult"></a>Para adicionar a implementação para ComputeResult
Em Class1.cpp, adicione a implementação do método a seguir. Esse método é executado de modo síncrono no thread de chamada, mas é muito rápido porque usa C++ AMP para paralelizar a computação na GPU. Para obter mais informações, consulte Visão geral de C++ AMP. Os resultados são anexados a um tipo concreto Platform::Collections::Vector<T>, que é implicitamente convertido em um Windows::Foundation::Collections::IVector<T> quando ele é retornado.

```cpp
//Public API
IVector<double>^ Class1::ComputeResult(double input)
{
    // Implement your function in ISO C++ or
    // call into your C++ lib or DLL here. This example uses AMP.
    float numbers[] = { 1.0, 10.0, 60.0, 100.0, 600.0, 10000.0 };
    array_view<float, 1> logs(6, numbers);

    // See http://msdn.microsoft.com/library/hh305254.aspx
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
## <a name="to-add-the-implementation-for-getprimesordered-and-its-helper-method"></a>Para adicionar a implementação para GetPrimesOrdered e seu método auxiliar
Em Class1.cpp, adicione as implementações de GetPrimesOrdered e o método auxiliar is_prime. GetPrimesOrdered usa uma classe concurrent_vector e um loop de função parallel_for para dividir o trabalho e usar os recursos máximos do computador no qual o programa está em execução para produzir resultados. Depois que os resultados são calculados, armazenados e classificados, eles são adicionados a um Platform::Collections::Vector<T> e retornados como Windows::Foundation::Collections::IVector<T> ao código do cliente.

Observe o código do relator de progresso, que permite que o cliente vincule uma barra de progresso ou outra interface de usuário para mostrar ao usuário quanto tempo a operação ainda vai demorar. O relatório de progresso tem um custo. Um evento deve ser acionado no lado do componente e manipulado no thread de interface do usuário, e o valor de progresso deve ser armazenado em cada iteração. Uma forma de minimizar o custo é limitar a frequência com que um evento de progresso é acionado. Se o custo ainda for proibitivo, ou se você não puder estimar a duração da operação, considere usar um anel de progresso, que mostra que uma operação está em andamento mas não mostra o tempo restante até a conclusão.

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

## <a name="to-add-the-implementation-for-getprimesunordered"></a>Para adicionar a implementação para GetPrimesUnordered
A última etapa para criar o componente C++ é adicionar a implementação do GetPrimesUnordered em Class1.cpp. Esse método retorna cada resultado como ele é encontrado, sem esperar até que todos os resultados sejam localizados. Cada resultado é retornado no manipulador de eventos e exibido na interface do usuário em tempo real. Novamente, observe que é usado um relator de progresso. Esse método também usa o método auxiliar is_prime.

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

## <a name="creating-a-javascript-client-app-visual-studio-2017"></a>Criando um aplicativo cliente JavaScript (Visual Studio 2017)

Se você quiser criar um C# cliente, poderá ignorar esta seção.

> [!NOTE]
> Não há suporte para projetos Plataforma Universal do Windows (UWP) no Visual Studio 2019. Consulte [JavaScript e TypeScript no Visual Studio 2019](/visualstudio/javascript/javascript-in-vs-2019?view=vs-2019#projects). Para acompanhar esta seção, recomendamos que você use o Visual Studio 2017. Consulte [JavaScript no Visual Studio 2017](/visualstudio/javascript/javascript-in-vs-2017).

### <a name="to-create-a-javascript-project"></a>Para criar um projeto em JavaScript
1. No Gerenciador de Soluções (no Visual Studio 2017; consulte a **Observação** acima), abra o menu de atalho para o nó da solução e escolha **Adicionar, novo projeto**.

2. Expanda JavaScript (ele pode estar aninhado em **Outros Idiomas**) e escolha **Aplicativo em Branco (Universal do Windows)** .

3. Aceite o nome padrão&mdash;App1&mdash;escolhendo o botão **OK** .

4. Abra o menu de atalho do nó do projeto App1 e escolha **Definir como Projeto de Inicialização**.

5. Adicione uma referência de projeto a WinRT_CPP:

6. Abra o menu de atalho do nó Referências e escolha **Adicionar Referência**.

7. No painel à esquerda da caixa de diálogo Gerenciador de Referências, selecione **Projetos** e **Solução**.

8. No painel central, selecione WinRT_CPP e escolha o botão **OK**

## <a name="to-add-the-html-that-invokes-the-javascript-event-handlers"></a>Para adicionar o HTML que invoca manipuladores de eventos JavaScript
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

## <a name="to-add-the-javascript-event-handlers-that-call-into-the-component-dll"></a>Para adicionar manipuladores de eventos JavaScript que chamam a DLL do componente
Adicione as seguintes funções ao final do arquivo default.js. Essas funções são chamadas quando os botões na página principal são escolhidos. Observe como o JavaScript ativa a classe C++ e, em seguida, chama seus métodos e usa os valores de retorno para preencher os rótulos HTML.

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

Adicione um código para incluir os ouvintes de eventos, substituindo a chamada existente para WinJS.UI.processAll em app.onactivated em default.js com o código a seguir que implementa o registro de evento em um bloco then. Para obter uma explicação detalhada disso, consulte [criar um aplicativo "Olá, mundo" (js)](/windows/uwp/get-started/create-a-hello-world-app-js-uwp).

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

{1&gt;Pressione F5 para executar o aplicativo.&lt;1}

## <a name="creating-a-c-client-app"></a>Criando um aplicativo cliente C#

### <a name="to-create-a-c-project"></a>Para criar um projeto C#
1. No Gerenciador de Soluções, abra o menu de atalho do nó Solution e escolha **Adicionar, Novo Projeto**.

2. Expanda Visual C# (ele pode estar aninhado em **Outros Idiomas**), selecione **Windows** e **Universal** no painel à esquerda e selecione **Aplicativo em Branco** no painel intermediário.

3. Nomeie esse aplicativo CS_Client e escolha o botão **OK**.

4. Abra o menu de atalho do nó do projeto CS_Client e escolha **Definir como Projeto de Inicialização**.

5. Adicione uma referência de projeto a WinRT_CPP:

   - Abra o menu de atalho do nó **Referências** e escolha **Adicionar Referência**.

   - No painel à esquerda da caixa de diálogo **Gerenciador de Referências**, selecione **Projetos** e **Solução**.

   - No painel central, selecione WinRT_CPP e escolha o botão **OK**.

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

## <a name="to-add-the-event-handlers-for-the-buttons"></a>Para adicionar manipuladores de eventos para os botões
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

{1&gt;Adicione o manipulador de eventos para o resultado ordenado:&lt;1}

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

{1&gt;Adicione o manipulador de eventos para o resultado não ordenado e para o botão que limpa os resultados, de modo que você possa executar o código novamente.&lt;1}

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
Selecione o projeto C# ou JavaScript como projeto de inicialização abrindo o menu de atalho do nó do projeto no Gerenciador de Soluções e escolhendo **Definir como Projeto de Inicialização**. Em seguida, pressione F5 para executar com depuração, ou Ctrl+F5 para executar sem depuração.

## <a name="inspecting-your-component-in-object-browser-optional"></a>Inspecionando seu componente no Pesquisador de Objetos (opcional)
No Pesquisador de Objetos, é possível inspecionar todos os tipos de Windows Runtime definidos em arquivos .winmd. Isso inclui os tipos nos namespaces Platform e padrão. No entanto, como os tipos no namespace Platform::Collections são definidos no arquivo de cabeçalho collections.h, e não em um arquivo winmd, eles não são exibidos no Pesquisador de Objetos.

### <a name="to-inspect-a-component"></a>**Para inspecionar um componente**
1. Na barra de menus, escolha **Exibir, Pesquisador de Objetos** (Ctrl+Alt+J).

2. No painel esquerdo do pesquisador de objetos, expanda o nó\_CPP do WinRT para mostrar os tipos e métodos que são definidos em seu componente.

## <a name="debugging-tips"></a>Dicas de depuração
{1&gt;Para obter uma melhor experiência de depuração, baixe os símbolos de depuração dos servidores públicos de símbolos da Microsoft:&lt;1}

### <a name="to-download-debugging-symbols"></a>**Para baixar símbolos de depuração**
1. Na barra de menus, escolha **Ferramentas, Opções**.

2. Na caixa de diálogo **Opções**, expanda **Depuração** e selecione **Símbolos**.

3. Selecione **Servidores de Símbolos Microsoft** e escolha o botão **OK**.

Pode levar algum tempo para baixar os símbolos na primeira vez. Para melhorar o desempenho na próxima vez que você pressionar F5, especifique um diretório local no qual armazenar os símbolos em cache.

Quando você depura uma solução JavaScript que tem uma DLL de componente, pode definir o depurador para percorrer o script ou o código nativo no componente, mas não ambos ao mesmo tempo. Para alterar a configuração, abra o menu de atalho do nó do projeto JavaScript no Gerenciador de Soluções e escolha **Propriedades, Depuração, Tipo de Depurador**.

Certifique-se de selecionar recursos apropriados no designer de pacote. Você pode abrir o designer de pacote abrindo o arquivo Package.appxmanifest. Por exemplo, caso você esteja tentando acessar arquivos programaticamente na pasta Imagens, não se esqueça de marcar a caixa de seleção **Biblioteca de Imagens** no painel **Recursos** do designer de pacotes.

Se o seu código JavaScript não reconhecer as propriedades públicas nem os métodos do componente, verifique se você está usando a concatenação com maiúsculas e minúsculas em JavaScript. Por exemplo, o método `ComputeResult` C++ deve ser referenciado como `computeResult` em JavaScript.

Caso remova um projeto de componente do Windows Runtime C++ de uma solução, você também deve remover manualmente a referência do projeto JavaScript. Caso contrário, as operações de depuração e build subsequentes não serão executadas. Se necessário, você poderá adicionar uma referência a assembly à DLL.

## <a name="related-topics"></a>Tópicos relacionados
* [Componentes do Windows Runtime com C++/CX](creating-windows-runtime-components-in-cpp.md)
