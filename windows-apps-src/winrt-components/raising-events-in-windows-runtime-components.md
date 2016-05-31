---
author: martinekuan
title: Acionando eventos em componentes do Tempo de Execução do Windows
ms.assetid: 3F7744E8-8A3C-4203-A1CE-B18584E89000
description: 
---

# Acionando eventos em componentes do Tempo de Execução do Windows


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


\[Algumas informações dizem respeito a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não dá nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.\]

Caso o componente do Tempo de Execução do Windows acione um evento de um tipo representante definido pelo usuário em um thread em segundo plano (thread de trabalho) e você deseje que o JavaScript seja capaz de receber o evento, é possível implementar e/ou acioná-lo destas formas:

-   (Opção 1) Acione o evento por meio do [Windows.UI.Core.CoreDispatcher](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.aspx) para realizar marshaling do evento para o contexto do thread JavaScript. Embora normalmente essa seja a melhor opção, em alguns cenários ela talvez não ofereça o desempenho mais rápido.
-   (Opção 2) Usar [Windows.Foundation.EventHandler](https://msdn.microsoft.com/library/windows/apps/br206577.aspx)&lt;Object&gt;, mas perder informações sobre o tipo (mas perder as informações sobre o tipo de evento). Caso a Opção 1 não seja viável ou o desempenho não seja adequado, essa é uma boa segunda opção caso a perda de informações sobre o tipo seja aceitável.
-   (Opção 3) Crie os próprios proxy e stub do componente. Essa opção é mais difícil de implementar, mas preserva informações sobre o tipo e pode fornecer um desempenho melhor em comparação com a Opção 1 em cenários exigentes.

Se você simplesmente acionar um evento em um thread em segundo plano sem usar uma dessas opções, um cliente JavaScript não receberá o evento.

## Segundo plano


Todos os componentes do Tempo de Execução do Windows e aplicativos são fundamentalmente objetos COM, independentemente da linguagem que você usa para criá-los. Na API do Windows, a maioria dos componentes é de objetos COM Agile que podem se comunicar igualmente bem com objetos no thread em segundo plano e no thread da interface do usuário. Caso um objeto COM não possa ser Agile, isso requer que objetos auxiliares conhecidos como proxies e stubs se comuniquem com outros objetos COM em todo o limite de thread em segundo plano do thread de interface do usuário. (Em termos de COM, isso é conhecido como comunicação entre apartments de thread.)

A maioria dos objetos na API do Windows é Agile ou tem proxies e stubs integrados. No entanto, proxies e stubs não podem ser criados para tipos genéricos, como Windows.Foundation.[TypedEventHandler&lt;TSender, TResult&gt;](https://msdn.microsoft.com/library/windows/apps/br225997.aspx) porque eles só serão tipos completos quando você fornecer o argumento de tipo. É apenas com clientes JavaScript que a falta de proxies ou stubs se torna um problema, mas caso queira que o componente seja utilizável em JavaScript, bem como em C++ ou em uma linguagem .NET, você deve usar uma das três opções a seguir.

## (Opção 1) Acionar o evento por meio de CoreDispatcher


Você pode enviar eventos de qualquer tipo de representante definido pelo usuário usando o [Windows.UI.Core.CoreDispatcher](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.aspx), e o JavaScript poderá recebê-los. Caso você não tenha certeza de qual opção usar, tente esta primeiro. Caso a latência entre o acionamento do evento e a manipulação do evento se torne um problema, tente uma das outras opções.

O exemplo a seguir mostra como usar o CoreDispatcher para acionar um evento fortemente tipado. O argumento do tipo é Toast, e não Object.

```csharp
public event EventHandler<Toast> ToastCompletedEvent;
private void OnToastCompleted(Toast args)
{
    var completedEvent = ToastCompletedEvent;
    if (completedEvent != null)
    {
        completedEvent(this, args);
    }
}

public void MakeToastWithDispatcher(string message)
{
    Toast toast = new Toast(message);
    // Assume you have a CoreDispatcher at class scope.
    // Initialize it here, then use it from the background thread.
    var window = Windows.UI.Core.CoreWindow.GetForCurrentThread();
    m_dispatcher = window.Dispatcher;

    Task.Run( () =>
    {
        if (ToastCompletedEvent != null)
        {
            m_dispatcher.RunAsync(CoreDispatcherPriority.Normal,
            new DispatchedHandler(() =>
            {
                this.OnToastCompleted(toast);
            })); // end m_dispatcher.RunAsync
         }
     }); // end Task.Run
}
```

## (Opção 2) Usar EventHandler &lt;Object&gt;, mas perder informações sobre o tipo


Outra maneira de enviar um evento de um thread em segundo plano é usar [Windows.Foundation.EventHandler](https://msdn.microsoft.com/library/windows/apps/br206577.aspx)&lt;Object&gt; como o tipo do evento. O Windows oferece essa instanciação concreta do tipo genérico e fornece um proxy e um stub para ele. A desvantagem é que as informações de tipo dos argumentos de evento e remetente são perdidas. Os clientes C++ e .NET devem saber pela documentação para qual tipo reconverter quando o evento é recebido. Os clientes JavaScript não precisam das informações sobre o tipo original. Eles encontram as propriedades arg, com base nos nomes nos metadados.

Este exemplo mostra como usar Windows.Foundation.EventHandler&lt;Object&gt; em C#:

```csharp
public sealed Class1
{
// Declare the event
public event EventHandler<Object> ToastCompletedEvent;

    // Raise the event
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message);
        // Fire the event from a background thread to allow this thread to continue 
        Task.Run(() =>
        {
            if (ToastCompletedEvent != null)
            {
                OnToastCompleted(toast);
            }
        });
    }

    private void OnToastCompleted(Toast args)
    {
        var completedEvent = ToastCompletedEvent;
        if (completedEvent != null)
        {
            completedEvent(this, args);
        }
    }
}
```

Você consome esse evento no lado do JavaScript desta forma:

```javascript
toastCompletedEventHandler: function (event) {
   var toastType = event.toast.toastType;
   document.getElementById("toasterOutput").innerHTML = "<p>Made " + toastType + " toast</p>";
}
```

## (Opção 3) Criar os próprios proxy e stub


Para ganhos de desempenho potenciais em tipos de eventos definidos pelo usuário que tenham informações de tipo totalmente preservadas, você precisa criar os próprios objetos de proxy e stub e incorporá-los ao pacote do aplicativo. Normalmente, você só precisa usar essa opção em situações raras nas quais nenhuma das outras duas opções são adequadas. Além disso, não há garantia de que essa opção fornecerá desempenho melhor do que as outras duas opções. O desempenho real depende de muitos fatores. Use o criador de perfil do Visual Studio ou outras ferramentas de criação de perfil para avaliar o desempenho real no aplicativo e determinar se o evento é, na verdade, um afunilamento.

O restante deste artigo mostra como usar C# para criar um componente do Tempo de Execução do Windows básico e usar C++ para criar uma DLL para o proxy e o stub que permitirão que o JavaScript consuma um evento Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt; acionado pelo componente em uma operação assíncrona. (Também é possível usar C++ ou Visual Basic para criar o componente. As etapas relacionadas à criação de proxies e stubs são as mesmas.) Este procedimento passo a passo se baseia na criação de uma amostra de componente no processo de Windows Runtime (C++/CX) e ajuda a explicar as finalidades.

Este procedimento passo a passo tem estas partes:

-   Aqui, você criará duas classes de Tempo de Execução do Windows básicas. Uma classe expõe um evento do tipo [Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt;](https://msdn.microsoft.com/library/windows/apps/br225997.aspx) e a outra classe é o tipo retornado para o JavaScript como o argumento de TValue. Essas classes não podem se comunicar com JavaScript até você concluir as etapas posteriores.
-   Este aplicativo ativa o objeto de classe principal, chama um método e manipula um evento acionado pelo componente do Tempo de Execução do Windows.
-   Elas são exigidas pelas ferramentas para gerar as classes de proxy e stub.
-   Em seguida, você usa o arquivo IDL para gerar o código-fonte C para o proxy e o stub.
-   Registre os objetos proxy-stub de maneira que o tempo de execução COM possa encontrá-los e faça referência à DLL proxy-stub no projeto do aplicativo.

## Para criar o componente do Tempo de Execução do Windows

1.  No Visual Studio, na barra de menus, escolha **Arquivo &gt; Novo Projeto**. Na caixa de diálogo **Novo Projeto**, expanda **JavaScript &gt; Universal Windows** e selecione **Aplicativo em Branco**. Nomeie o projeto ToasterApplication e escolha o botão **OK**.
2.  Adicione um componente do Tempo de Execução do Windows C# à solução: no Gerenciador de Soluções, abra o menu de atalho da solução e escolha **Adicionar&gt; Novo Projeto**. Expanda **Visual C# &gt; Windows Store** e selecione **Componente do Tempo de Execução do Windows**. Nomeie o projeto ToasterComponent e escolha o botão **OK**. ToasterComponent será o namespace raiz dos componentes que você criará em etapas posteriores.

    No Gerenciador de Soluções, abra o menu de atalho da solução e escolha **Propriedades**. Na caixa de diálogo **Páginas de Propriedades**, selecione **Propriedades de Configuração** no painel esquerdo e, na parte superior da caixa de diálogo, defina **Configuração** como **Depurar** e **Plataforma** como x86, x64 ou ARM. Escolha o botão **OK**.

    > **Importante**  Plataforma = nenhuma CPU funcionará porque ela não é válida para a DLL Win32 de código nativo que você adicionará à solução mais tarde.

3.  No Gerenciador de Soluções, renomeie class1.cs para ToasterComponent.cs de maneira que ele corresponda ao nome do projeto. O Visual Studio renomeia automaticamente a classe no arquivo para coincidir com o novo nome de arquivo.
4.  No arquivo .cs, adicione uma diretiva using para o namespace Windows.Foundation a fim de colocar TypedEventHandler em escopo.
5.  Quando você precisa de proxies e stubs, o componente deve usar interfaces para expor os membros públicos. Em ToasterComponent.cs, defina uma interface para o notificador do sistema (toaster) e outra para a notificação (Toast) que o notificador produz.

    > **Observação**  Em C#, você pode ignorar essa etapa. Em vez disso, crie primeiro uma classe e, em seguida, abra o menu de atalho e escolha **Refatorar &gt; Extrair Interface**. No código gerado, dê manualmente às interfaces acessibilidade pública.

    ```csharp
    public interface IToaster
        {
            void MakeToast(String message);
            event TypedEventHandler<Toaster, Toast> ToastCompletedEvent;

        }
        public interface IToast
        {
            String ToastType { get; }
        }
    ```

    A interface IToast tem uma cadeia de caracteres que pode ser recuperada para descrever o tipo de notificação do sistema. A interface IToaster tem um método para criar a notificação do sistema e um evento para indicar que a notificação do sistema foi feita. Como retorna a parte específica (ou seja, tipo) da notificação do sistema, esse evento é conhecido como um evento tipado.

6.  Em seguida, precisamos de classes que implementem essas interfaces e sejam públicas e seladas de maneira que permaneçam acessíveis no aplicativo JavaScript que você programará depois.

    ```csharp
    public sealed class Toast : IToast
        {
            private string _toastType;

            public string ToastType
            {
                get
                {
                    return _toastType;
                }
            }
            internal Toast(String toastType)
            {
                _toastType = toastType;
            }

        }
        public sealed class Toaster : IToaster
        {
            public event TypedEventHandler<Toaster, Toast> ToastCompletedEvent;

            private void OnToastCompleted(Toast args)
            {
                var completedEvent = ToastCompletedEvent;
                if (completedEvent != null)
                {
                    completedEvent(this, args);
                }
            }

            public void MakeToast(string message)
            {
                Toast toast = new Toast(message);
                // Fire the event from a thread-pool thread to enable this thread to continue 
                Windows.System.Threading.ThreadPool.RunAsync(
                (IAsyncAction action) =>
                {
                    if (ToastCompletedEvent != null)
                    {
                        OnToastCompleted(toast);
                    }
                });
           }
        }
    ``` 

    No código anterior, criamos a notificação do sistema e giramos um item de trabalho do pool de threads para acionar a notificação. Embora o IDE possa sugerir que você aplique a palavra-chave await à chamada assíncrona, isso não é necessário neste caso porque o método não faz nenhum trabalho que dependa dos resultados da operação.

    > **Observação**  A chamada assíncrona no código anterior usa ThreadPool.RunAsync exclusivamente para demonstrar uma forma simples de acionar o evento em um thread em segundo plano. Você pode escrever esse método em particular, conforme mostrado no exemplo a seguir, e ele funcionaria bem porque o agendador de tarefas .NET realiza marshaling automaticamente de chamadas async/await de volta para o thread da interface do usuário.
  
    ````csharp
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message)
        await Task.Delay(new Random().Next(1000));
        OnToastCompleted(toast);
    }
    ```

    If you build the project now, it should build cleanly.

## To program the JavaScript app

1.  Now we can add a button to the JavaScript app to cause it to use the class we just defined to make toast. Before we do this, we must add a reference to the ToasterComponent project we just created. In Solution Explorer, open the shortcut menu for the ToasterApplication project, choose **Add &gt; References**, and then choose the **Add New Reference** button. In the Add Reference dialog box, in the left pane under Solution, select the component project, and then in the middle pane, select ToasterComponent. Choose the **OK** button.
2.  In Solution Explorer, open the shortcut menu for the ToasterApplication project and then choose **Set as Startup Project**.
3.  At the end of the default.js file, add a namespace to contain the functions to call the component and be called back by it. The namespace will have two functions, one to make toast and one to handle the toast-complete event. The implementation of makeToast creates a Toaster object, registers the event handler, and makes the toast. So far, the event handler doesn’t do much, as shown here:

    ```javascript
    WinJS.Namespace.define("ToasterApplication"), {
       makeToast: function () {

          var toaster = new ToasterComponent.Toaster();
          //toaster.addEventListener("ontoastcompletedevent", ToasterApplication.toastCompletedEventHandler);
          toaster.ontoastcompletedevent = ToasterApplication.toastCompletedEventHandler;
          toaster.makeToast("Peanut Butter");
       },

       toastCompletedEventHandler: function(event) {
           // The sender of the event (the delegate's first type parameter)
           // is mapped to event.target. The second argument of the delegate
           // is contained in event, which means in this case event is a 
           // Toast class, with a toastType string.
           var toastType = event.toastType;

           document.getElementById('toastOutput').innerHTML = "<p>Made " + toastType + " toast</p>";
        },
    });
    ```

4.  The makeToast function must be hooked up to a button. Update default.html to include a button and some space to output the result of making toast:

    ```html
    <body>
        <h1>Click the button to make toast</h1>
        <button onclick="ToasterApplication.makeToast()">Make Toast!</button>
        <div id="toasterOutput">
            <p>No Toast Yet...</p>
        </div>
    </body>
    ```

    If we weren’t using a TypedEventHandler, we would now be able to run the app on the local machine and click the button to make toast. But in our app, nothing happens. To find out why, let’s debug the managed code that fires the ToastCompletedEvent. Stop the project, and then on the menu bar, choose **Debug &gt; Toaster Application properties**. Change **Debugger Type** to **Managed Only**. Again on the menu bar, choose **Debug &gt; Exceptions**, and then select **Common Language Runtime Exceptions**.

    Now run the app and click the make-toast button. The debugger catches an invalid cast exception. Although it’s not obvious from its message, this exception is occurring because proxies are missing for that interface.

    ![missing proxy](./images/debuggererrormissingproxy.png)

    The first step in creating a proxy and stub for a component is to add a unique ID or GUID to the interfaces. However, the GUID format to use differs depending on whether you're coding in C#, Visual Basic, or another .NET language, or in C++.

## To generate GUIDs for the component's interfaces (C# and other .NET languages)

1.  On the menu bar, choose Tools &gt; Create GUID. In the dialog box, select 5. \[Guid(“xxxxxxxx-xxxx...xxxx)\]. Choose the New GUID button and then choose the Copy button.

    ![guid generator tool](./images/guidgeneratortool.png)

2.  Go back to the interface definition, and then paste the new GUID just before the IToaster interface, as shown in the following example. (Don't use the GUID in the example. Every unique interface should have its own GUID.)

    ```cpp
    [Guid("FC198F74-A808-4E2A-9255-264746965B9F")]
        public interface IToaster...
    ```

3.  Add a using directive for the System.Runtime.InteropServices namespace.
4.  Repeat these steps for the IToast interface.

## To generate GUIDs for the component's interfaces (C++)

1.  On the menu bar, choose Tools &gt; Create GUID. In the dialog box, select 3. static const struct GUID = {...}. Choose the New GUID button and then choose the Copy button.

2.  Paste the GUID just before the IToaster interface definition. After you paste, the GUID should resemble the following example. (Don't use the GUID in the example. Every unique interface should have its own GUID.)

    ```cpp 
      <Extensions> <!--Use your own GUIDs!!!-->
        <Extension Category="windows.activatableClass.proxyStub">
          <ProxyStub ClassId="1ecafeff-1ee1-504a-9af5-a68c6fb2b47d">
            <Path>Proxies.dll</Path>
            <Interface Name="IToast" InterfaceId="F8D30778-9EAF-409C-BCCD-C8B24442B09B"/>
            <Interface Name="IToaster"  InterfaceId="E976784C-AADE-4EA4-A4C0-B0C2FD1307C3"/>  
            <Interface Name="ITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast" InterfaceId="1ecafeff-1ee1-504a-9af5-a68c6fb2b47d"/>
          </ProxyStub>      
        </Extension>
      </Extensions>
    ```

    Paste the Extensions XML node as a direct child of the Package node, and a peer of, for example, the Resources node.

2.  Before moving on, it’s important to ensure that:

    -   The ProxyStub ClassId is set to the first GUID in the ToasterComponent\_i.c file. Use the first GUID that's defined in this file for the classId. (This might be the same as the GUID for ITypedEventHandler2.)
    -   The Path is the package relative path of the proxy binary. (In this walkthrough, proxies.dll is in the same folder as ToasterApplication.winmd.)
    -   The GUIDs are in the correct format. (This is easy to get wrong.)
    -   The interface IDs in the manifest match the IIDs in ToasterComponent\_i.c file.
    -   The interface names are unique in the manifest. Because these are not used by the system, you can choose the values. It is a good practice to choose interface names that clearly match interfaces that you have defined. For generated interfaces, the names should be indicative of the generated interfaces. You can use the ToasterComponent\_i.c file to help you generate interface names.

3.  If you try to run the solution now, you will get an error that proxies.dll is not part of the payload. Open the shortcut menu for the **References** folder in the ToasterApplication project and then choose **Add Reference**. Select the check box next to the Proxies project. Also, make sure that the check box next to ToasterComponent is also selected. Choose the **OK** button.

    The project should now build. Run the project and verify that you can make toast.

## Related topics

* [Creating Windows Runtime Components in C++](creating-windows-runtime-components-in-cpp.md)



<!--HONumber=May16_HO2-->


