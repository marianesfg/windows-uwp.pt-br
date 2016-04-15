---
title: Criar e consumir um serviço de aplicativo
description: Saiba como escrever um aplicativo UWP (Plataforma Universal do Windows) que pode fornecer serviços a outros aplicativos UWP e também como consumir esses serviços.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
---

# Criar e consumir um serviço de aplicativo


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos do Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Saiba como escrever um aplicativo UWP (Plataforma Universal do Windows) que pode fornecer serviços a outros aplicativos UWP e também como consumir esses serviços.

## Criar um novo projeto de provedor de serviços de aplicativo


Nestas instruções, criaremos tudo em uma única solução por questão de simplicidade.

-   No Microsoft Visual Studio 2015, crie um novo projeto de aplicativo UWP e chame-o de AppServiceProvider. (Na caixa de diálogo **Novo Projeto**, selecione **Modelos &gt; Outros Idiomas &gt; Visual C# &gt; Windows &gt; Universal do Windows &gt; Aplicativo em Branco (Universal do Windows)**). Esse será o aplicativo que fornecerá o serviço de aplicativo.

## Adicionar uma extensão de serviço de aplicativo ao arquivo package.appxmanifest


No arquivo Package.appxmanifest do projeto AppServiceProvider, adicione a extensão AppService a seguir ao elemento **&lt;Application&gt;**. Este exemplo anuncia o serviço `com.Microsoft.Inventory` e é o que identifica esse aplicativo como um provedor de serviços de aplicativo. O serviço real será implementado como uma tarefa em segundo plano. O aplicativo de serviço de aplicativo expõe o serviço a outros aplicativos. Recomendamos usar um estilo de nome de domínio reverso para o nome do serviço.

``` syntax
... 
<Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="AppServiceProvider.App">
      <Extensions>
        <uap:Extension Category="windows.appService" EntryPoint="MyAppService.Inventory">
          <uap:AppService Name="com.microsoft.inventory"/>
        </uap:Extension>
      </Extensions>
      ...
    </Application>
</Applications>
```

O atributo **Category** identifica o aplicativo como um provedor de serviços de aplicativo.

O atributo **EntryPoint** identifica a classe que implementa o serviço, que implementaremos a seguir.

## Criar o serviço de aplicativo


1.  Um serviço de aplicativo é implementado como uma tarefa em segundo plano. Isso permite que um aplicativo em primeiro plano invoque um serviço de aplicativo em outro aplicativo para realizar tarefas em segundo plano. Adicione um novo projeto do componente do Windows Runtime à solução (**Arquivo &gt; Adicionar &gt; Novo Projeto**) chamado MyAppService. (Na caixa de diálogo **Adicionar Novo Projeto**, escolha **Instalado &gt; Outros Idiomas &gt; Visual C# &gt; Windows &gt; Universal do Windows &gt; Componente do Windows Runtime (Universal do Windows)**
2.  No projeto AppServiceProvider, adicione uma referência ao projeto MyAppService.
3.  No projeto MyappService, adicione as seguintes instruções **using** à parte superior de Class1.cs:
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  Substitua o código stub de **Class1** por uma nova classe de tarefa em segundo plano chamada **Inventory**:

    ```cs
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn&#39;t terminated.
            taskInstance.Canceled += OnTaskCanceled; // Associate a cancellation handler with the background task.

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
        }

        private void OnTaskCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            if (this.backgroundTaskDeferral != null)
            {
                // Complete the service deferral.
                this.backgroundTaskDeferral.Complete();
            }
        }
    }
    ```

    Essa classe é onde o serviço de aplicativo fará seu trabalho.

    **Run()** é chamado quando a tarefa em segundo plano é criada. Como as tarefas em segundo plano são encerradas após a conclusão de **Run**, o código é adiado para que a tarefa em segundo plano fique ativa para atender às solicitações.

    **OnTaskCanceled()** é chamado quando a tarefa é cancelada. A tarefa é cancelada quando o aplicativo cliente descarta [**AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704), o aplicativo cliente é suspenso, o sistema operacional é desligado ou suspenso, ou o sistema operacional está sem recursos para executar a tarefa.

## Escrever o código para o serviço de aplicativo


**OnRequestedReceived()** é para onde o código do serviço de aplicativo vai. Substitua o stub **OnRequestedReceived()** em Class1.cs de MyAppService pelo código deste exemplo. Esse código obtém um índice para um item de estoque e passa-o, juntamente com uma sequência de comando, ao serviço para recuperar o nome e o preço do item de estoque especificado. O código de tratamento de erro foi removido por questão de brevidade.

```cs
private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below to respond to the message
    // and we don&#39;t want this call to get cancelled while we are waiting.
    var messageDeferral = args.GetDeferral();

    ValueSet message = args.Request.Message;
    ValueSet returnData = new ValueSet();

    string command = message["Command"] as string;
    int? inventoryIndex = message["ID"] as int?;

    if ( inventoryIndex.HasValue &amp;&amp;
         inventoryIndex.Value >= 0 &amp;&amp;
         inventoryIndex.Value < inventoryItems.GetLength(0))
    {
        switch (command)
        {
            case "Price":
            {
                returnData.Add("Result", inventoryPrices[inventoryIndex.Value]);
                returnData.Add("Status", "OK");
                break;
            }

            case "Item":
            {
                returnData.Add("Result", inventoryItems[inventoryIndex.Value]);
                returnData.Add("Status", "OK");
                break;
            }

            default:
            {
                returnData.Add("Status", "Fail: unknown command");
                break;
            }
        }
    }
    else
    {
        returnData.Add("Status", "Fail: Index out of range");
    }

    await args.Request.SendResponseAsync(returnData); // Return the data to the caller.
    messageDeferral.Complete(); // Complete the deferral so that the platform knows that we&#39;re done responding to the app service call.
}
```

Observe que **OnRequestedReceived()** é **async** porque fazemos uma chamada de método que pode ser aguardado para [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) neste exemplo.

Um adiamento é obtido para que o serviço possa usar métodos **async** no manipulador OnRequestReceived. Isso garante que a chamada para OnRequestReceived não seja concluída até que o processamento da mensagem seja concluído. [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) é usado para enviar um resposta junto com a conclusão. **SendResponseAsync** não sinaliza a conclusão da chamada. É a conclusão do adiamento que sinaliza para [**SendMessageAsync**](https://msdn.microsoft.com/library/windows/apps/dn921712) que OnRequestReceived foi concluído.

Os serviços de aplicativo usam [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) para trocar informações. O tamanho dos dados que você pode passar é limitado apenas pelos recursos do sistema. Não há chaves predefinidas para uso em **ValueSet**. Você deve determinar quais valores de chave serão usados para definir o protocolo do serviço de aplicativo. O chamador deve ser escrito considerando esse protocolo. Neste exemplo, escolhemos uma chave chamada "Command" que contém um valor que indica se desejamos que o serviço de aplicativo forneça o nome do item de estoque ou seu preço. O índice do nome do estoque é armazenado na chave "ID". O valor retornado é armazenado na chave "Result".

Uma enumeração [**AppServiceClosedStatus**](https://msdn.microsoft.com/library/windows/apps/dn921703) é retornada ao chamador para indicar se a chamada para o serviço de aplicativo obteve êxito ou não. Um exemplo de como a chamada para o serviço de aplicativo pode falhar é se o sistema operacional anular o ponto de extremidade do serviço, os recursos forem excedidos etc. Você pode retornar informações de erro adicionais por meio de [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131). Neste exemplo, usamos uma chave chamada "Status" para retornar informações de erro mais detalhadas para o chamador.

A chamada para [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) retorna [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) ao chamador.

## Implantar o aplicativo de serviço e obter o nome da família de pacotes


O aplicativo de provedor de serviços de aplicativo deve ser implantado antes de chamá-lo de um cliente. Você também precisará do nome da família de pacotes do aplicativo de serviço de aplicativo para chamá-lo.

-   Uma maneira de obter o nome da família de pacotes do aplicativo de serviço de aplicativo é chamar [**Windows.ApplicationModel.Package.Current.Id.FamilyName**](https://msdn.microsoft.com/library/windows/apps/br224670) a partir do projeto **AppServiceProvider** (por exemplo, de `public App()` em App.xaml.cs) e anotar o resultado. Para executar o AppServiceProvider no Microsoft Visual Studio, defina-o como o projeto de inicialização na janela do Gerenciador de Soluções e execute o projeto.
-   Outra maneira de obter o nome da família de pacotes é implantar a solução (**Compilar &gt; Implantar solução**) e anotar o nome completo do pacote na janela de saída (**Exibir &gt; Saída**). Você deve remover as informações de plataforma da cadeia de caracteres na janela de saída para derivar o nome do pacote. Por exemplo, se o nome completo do pacote reportado na janela de saída for "9fe3058b-3de0-4e05-bea7-84a06f0ee4f0\_1.0.0.0\_x86\_\_yd7nk54bq29ra", você deve extrair "1.0.0.0\_x86\_\_" deixando "9fe3058b-3de0-4e05-bea7-84a06f0ee4f0\_yd7nk54bq29ra" como o nome da família de pacotes.

## Escrever um cliente para chamar o serviço de aplicativo


1.  Adicione um novo projeto em branco do aplicativo Universal do Windows à solução (**Arquivo &gt; Adicionar &gt; Novo Projeto**) chamado ClientApp. (Na caixa de diálogo **Adicionar Novo Projeto**, escolha **Instalado &gt; Outros idiomas &gt; Visual C# &gt; Windows &gt; Universal do Windows &gt; Aplicativo em Branco (Universal do Windows)**).
2.  No projeto ClientApp, adicione a seguinte instrução **using** à parte superior de MainPage.xaml.cs:
    ```cs
    >using Windows.ApplicationModel.AppService;
    ```

3.  Adicione uma caixa de texto e um botão a MainPage.xaml.
4.  Adicione um manipulador de clique de botão e a palavra-chave **async** à assinatura do manipulador do botão.
5.  Substitua o stub do manipulador de clique de botão pelo código a seguir. Certifique-se de incluir a declaração de campo `inventoryService`.

   ```cs
   private AppServiceConnection inventoryService;
    private async void button_Click(object sender, RoutedEventArgs e)
    {
        // Add the connection.
        if (this.inventoryService == null)
        {
            this.inventoryService = new AppServiceConnection();

            // Here, we use the app service name defined in the app service provider&#39;s Package.appxmanifest file in the &lt;Extension&gt; section. 
            this.inventoryService.AppServiceName = "com.microsoft.inventory";

            // Use Windows.ApplicationModel.Package.Current.Id.FamilyName within the app service provider to get this value.
            this.inventoryService.PackageFamilyName = "replace with the package family name";

            var status = await this.inventoryService.OpenAsync();
            if (status != AppServiceConnectionStatus.Success)
            {
                button.Content = "Failed to connect";
                return;
            }
        }

        // Call the service.
        int idx = int.Parse(textBox.Text);
        var message = new ValueSet();
        message.Add("Command", "Item");
        message.Add("ID", idx);
        AppServiceResponse response = await this.inventoryService.SendMessageAsync(message);
        string result = "";

        if (response.Status == AppServiceResponseStatus.Success)
        {
            // Get the data  that the service sent  to us.
            if (response.Message["Status"] as string == "OK")
            {
                result = response.Message["Result"] as string;
            }
        }

        message.Clear();
        message.Add("Command", "Price");
        message.Add("ID", idx);
        response = await this.inventoryService.SendMessageAsync(message);

        if (response.Status == AppServiceResponseStatus.Success)
        {
            // Get the data that the service sent to us.
            if (response.Message["Status"] as string == "OK")
            {
                result += " : Price = " + response.Message["Result"] as string;
            }
        }

        button.Content = result;
    }
    ```

    Replace the package family name in the line `this.inventoryService.PackageFamilyName = "replace with the package family name";` with the package family name of the **AppServiceProvider** project that you obtained in \[Step 5: Deploy the service app and get the package family name\].

    The code first establishes a connection with the app service. The connection will remain open until you dispose **this.inventoryService**. The app service name must match the **AppService Name** attribute that you added to the AppServiceProvider project's Package.appxmanifest file. In this example, it is `<uap:AppService Name="com.microsoft.inventory"/>`.

    A [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) named **message** is created to specify the command that we want to send to the app service. The example app service expects a command to indicate which of two actions to take. We get the index from the textbox in the ClientApp, and then call the service with the "Item" command to get the description of the item. Then, we make the call with the "Price" command to get the item's price. The button text is set to the result.

    Because [**AppServiceResponseStatus**](https://msdn.microsoft.com/library/windows/apps/dn921724) only indicates whether the operating system was able to connect the call to the app service, we check the "Status" key in the [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) we receive from the app service to ensure that it was able to fulfill the request.

6.  In Visual Studio, set the ClientApp project to be the startup project in the Solution Explorer window and run the solution. Enter the number 1 into the text box and click the button. You should get "Chair : Price = 88.99" back from the service.

    ![sample app displaying chair price=88.99](images/appserviceclientapp.png)

If the app service call fails, check the following in the ClientApp:

1.  Verify that the package family name assigned to the inventory service connection matches the package family name of the AppServiceProvider app. See: **button\_Click()**`this.inventoryService.PackageFamilyName = "...";`).
2.  In **button\_Click()**, verify that the app service name that is assigned to the inventory service connection matches the app service name in the AppServiceProvider's Package.appxmanifest file. See: `this.inventoryService.AppServiceName = "com.microsoft.inventory";`.
3.  Ensure that the AppServiceProvider app has been deployed (In the Solution Explorer, right-click the solution and choose **Deploy**).

## Debug the app service


1.  Ensure that the entire solution is deployed before debugging because the app service provider app must be deployed before the service can be called. (In Visual Studio, **Build &gt; Deploy Solution**).
2.  In the Solution Explorer, right-click the AppServiceProvider project and choose **Properties**. From the **Debug** tab, change the **Start action** to **Do not launch, but debug my code when it starts**.
3.  In the MyAppService project, in the Class1.cs file, set a breakpoint in OnRequestReceived().
4.  Set the AppServiceProvider project to be the startup project and press F5.
5.  Start ClientApp from the Start menu (not from Visual Studio).
6.  Enter the number 1 into the text box and press the button. The debugger will stop in the app service call on the breakpoint in your app service.

## Debug the client


1.  Follow the instructions in the preceding step to debug the app service.
2.  Launch ClientApp from the Start menu.
3.  Attach the debugger to the ClientApp.exe process (not the ApplicationFrameHost.exe process). (In Visual Studio, choose **Debug &gt; Attach to Process...**.)
4.  In the ClientApp project, set a breakpoint in **button\_Click()**.
5.  The breakpoints in both the client and the app service will now be hit when you enter the number 1 into the text box of the ClientApp and click the button.

## Remarks


This example provides a simple introduction to creating an app service and calling it from another app. The key things to note are the creation of a background task to host the app service, the addition of the windows.appservice extension to the app service provider app's Package.appxmanifest file, obtaining the package family name of the app service provider app so that we can connect to it from the client app, and using [**Windows.ApplicationModel.AppService.AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704) to call the service.

## Full code for MyAppService


```cs
using System;
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
using Windows.Foundation.Collections;

namespace MyAppService
{
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn&#39;t terminated.
            taskInstance.Canceled += OnTaskCanceled; // Associate a cancellation handler with the background task.

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // Get a deferral because we use an awaitable API below to respond to the message
            // and we don&#39;t want this call to get cancelled while we are waiting.
            var messageDeferral = args.GetDeferral();

            ValueSet message = args.Request.Message;
            ValueSet returnData = new ValueSet();

            string command = message["Command"] as string;
            int? inventoryIndex = message["ID"] as int?;

            if (inventoryIndex.HasValue &amp;&amp;
                 inventoryIndex.Value >= 0 &amp;&amp;
                 inventoryIndex.Value < inventoryItems.GetLength(0))
            {
                switch (command)
                {
                    case "Price":
                        {
                            returnData.Add("Result", inventoryPrices[inventoryIndex.Value]);
                            returnData.Add("Status", "OK");
                            break;
                        }

                    case "Item":
                        {
                            returnData.Add("Result", inventoryItems[inventoryIndex.Value]);
                            returnData.Add("Status", "OK");
                            break;
                        }

                    default:
                        {
                            returnData.Add("Status", "Fail: unknown command");
                            break;
                        }
                }
            }
            else
            {
                returnData.Add("Status", "Fail: Index out of range");
            }

            await args.Request.SendResponseAsync(returnData); // Return the data to the caller.
            messageDeferral.Complete(); // Complete the deferral so that the platform knows that we&#39;re done responding to the app service call.
        }


        private void OnTaskCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            if (this.backgroundTaskDeferral != null)
            {
                // Complete the service deferral.
                this.backgroundTaskDeferral.Complete();
            }
        }
    }
}
```

## Tópicos relacionados


* [Oferecer suporte a tarefas em segundo plano em seu aplicativo](support-your-app-with-background-tasks.md)

 

 





<!--HONumber=Mar16_HO1-->


