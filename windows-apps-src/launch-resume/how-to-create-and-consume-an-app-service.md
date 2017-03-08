---
author: TylerMSFT
title: "Criar e consumir um serviço de app"
description: "Saiba como escrever um aplicativo UWP (Plataforma Universal do Windows) que pode fornecer serviços a outros aplicativos UWP e também como consumir esses serviços."
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: "comunicação entre apps, comunicação entre processos, IPC, mensagens em segundo plano, comunicação em segundo plano, app para app"
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 3dcf6a8191553deac5821346718a202bc362c7ff
ms.lasthandoff: 02/07/2017

---

# <a name="create-and-consume-an-app-service"></a>Criar e consumir um serviço de app


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Saiba como escrever um aplicativo UWP (Plataforma Universal do Windows) que pode fornecer serviços a outros aplicativos UWP e também como consumir esses serviços.

A partir do Windows 10, versão 1607, você pode criar serviços de aplicativo que são executados no mesmo processo do aplicativo host. Este artigo se concentra na criação de serviços de aplicativo que são executados em um processo separado em segundo plano. Consulte [Converter um serviço de app para ser executado no mesmo processo de seu app host](convert-app-service-in-process.md) para obter mais detalhes sobre os serviços de aplicativo que são executados no mesmo processo do provedor.

## <a name="create-a-new-app-service-provider-project"></a>Criar um novo projeto de provedor de serviços de aplicativo

Nestas instruções, criaremos tudo em uma única solução por questão de simplicidade.

-   No Microsoft Visual Studio 2015, crie um novo projeto de aplicativo UWP e chame-o de AppServiceProvider. (Na caixa de diálogo **Novo Projeto**, selecione **Modelos &gt; Outros Idiomas &gt; Visual C# &gt; Windows &gt; Universal do Windows &gt; Aplicativo em Branco (Universal do Windows)**). Esse será o aplicativo que fornecerá o serviço de aplicativo.

## <a name="add-an-app-service-extension-to-packageappxmanifest"></a>Adicionar uma extensão de serviço de aplicativo ao arquivo package.appxmanifest

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

## <a name="create-the-app-service"></a>Criar o serviço de aplicativo

1.  Um serviço de aplicativo é implementado como uma tarefa em segundo plano. Isso permite que um aplicativo em primeiro plano invoque um serviço de aplicativo em outro aplicativo para realizar tarefas em segundo plano. Adicione um novo projeto do componente do Tempo de Execução do Windows à solução (**Arquivo &gt; Adicionar &gt; Novo Projeto**) chamado MyAppService. (Na caixa de diálogo **Adicionar Novo Projeto**, escolha **Instalado &gt; Outros Idiomas &gt; Visual C# &gt; Windows &gt; Universal do Windows &gt; Componente do Tempo de Execução do Windows (Universal do Windows)**
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
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn't terminated.
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

## <a name="write-the-code-for-the-app-service"></a>Escrever o código para o serviço de aplicativo

**OnRequestedReceived()** é para onde o código do serviço de aplicativo vai. Substitua o stub **OnRequestedReceived()** em Class1.cs de MyAppService pelo código deste exemplo. Esse código obtém um índice para um item de estoque e passa-o, juntamente com uma sequência de comando, ao serviço para recuperar o nome e o preço do item de estoque especificado. O código de tratamento de erro foi removido por questão de brevidade.

```cs
private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below to respond to the message
    // and we don't want this call to get cancelled while we are waiting.
    var messageDeferral = args.GetDeferral();

    ValueSet message = args.Request.Message;
    ValueSet returnData = new ValueSet();

    string command = message["Command"] as string;
    int? inventoryIndex = message["ID"] as int?;

    if ( inventoryIndex.HasValue &&
         inventoryIndex.Value >= 0 &&
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
    messageDeferral.Complete(); // Complete the deferral so that the platform knows that we're done responding to the app service call.
}
```

Observe que **OnRequestedReceived()** é **async** porque fazemos uma chamada de método que pode ser aguardado para [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) neste exemplo.

Um adiamento é obtido para que o serviço possa usar métodos **async** no manipulador OnRequestReceived. Isso garante que a chamada para OnRequestReceived não seja concluída até que o processamento da mensagem seja concluído. [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) é usado para enviar um resposta junto com a conclusão. **SendResponseAsync** não sinaliza a conclusão da chamada. É a conclusão do adiamento que sinaliza para [**SendMessageAsync**](https://msdn.microsoft.com/library/windows/apps/dn921712) que OnRequestReceived foi concluído.

Os serviços de aplicativo usam [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) para trocar informações. O tamanho dos dados que você pode passar é limitado apenas pelos recursos do sistema. Não há chaves predefinidas para uso em **ValueSet**. Você deve determinar quais valores de chave serão usados para definir o protocolo do serviço de aplicativo. O chamador deve ser escrito considerando esse protocolo. Neste exemplo, escolhemos uma chave chamada "Command" que contém um valor que indica se desejamos que o serviço de aplicativo forneça o nome do item de estoque ou seu preço. O índice do nome do estoque é armazenado na chave "ID". O valor retornado é armazenado na chave "Result".

Uma enumeração [**AppServiceClosedStatus**](https://msdn.microsoft.com/library/windows/apps/dn921703) é retornada ao chamador para indicar se a chamada para o serviço de aplicativo obteve êxito ou não. Um exemplo de como a chamada para o serviço de aplicativo pode falhar é se o sistema operacional anular o ponto de extremidade do serviço, os recursos forem excedidos etc. Você pode retornar informações de erro adicionais por meio de [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131). Neste exemplo, usamos uma chave chamada "Status" para retornar informações de erro mais detalhadas para o chamador.

A chamada para [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) retorna [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) ao chamador.

## <a name="deploy-the-service-app-and-get-the-package-family-name"></a>Implantar o aplicativo de serviço e obter o nome da família de pacotes

O aplicativo de provedor de serviços de aplicativo deve ser implantado antes de chamá-lo de um cliente. Você também precisará do nome da família de pacotes do aplicativo de serviço de aplicativo para chamá-lo.

-   Uma maneira de obter o nome da família de pacotes do aplicativo de serviço de aplicativo é chamar [**Windows.ApplicationModel.Package.Current.Id.FamilyName**](https://msdn.microsoft.com/library/windows/apps/br224670) a partir do projeto **AppServiceProvider** (por exemplo, de `public App()` em App.xaml.cs) e anotar o resultado. Para executar o AppServiceProvider no Microsoft Visual Studio, defina-o como o projeto de inicialização na janela do Gerenciador de Soluções e execute o projeto.
-   Outra maneira de obter o nome da família de pacotes é implantar a solução (**Compilar &gt; Implantar solução**) e anotar o nome completo do pacote na janela de saída (**Exibir &gt; Saída**). Você deve remover as informações de plataforma da cadeia de caracteres na janela de saída para derivar o nome do pacote. Por exemplo, se o nome completo do pacote reportado na janela de saída for "9fe3058b-3de0-4e05-bea7-84a06f0ee4f0\_1.0.0.0\_x86\_\_yd7nk54bq29ra", você deve extrair "1.0.0.0\_x86\_\_" deixando "9fe3058b-3de0-4e05-bea7-84a06f0ee4f0\_yd7nk54bq29ra" como o nome da família de pacotes.

## <a name="write-a-client-to-call-the-app-service"></a>Escrever um cliente para chamar o serviço de aplicativo

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

            // Here, we use the app service name defined in the app service provider's Package.appxmanifest file in the <Extension> section.
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

    Substitua o nome da família de pacotes na linha `this.inventoryService.PackageFamilyName = "replace with the package family name";` pelo nome de família de pacotes do projeto **AppServiceProvider** que você obteve em \[Etapa 5: Implantar o aplicativo de serviço e obter o nome da família de pacotes\].

    O código primeiro estabelece uma conexão com o serviço de aplicativo. A conexão permanecerá aberta até que você descarte **this.inventoryService**. O nome de serviço do aplicativo deve coincidir com o atributo **AppService Name** que você adicionou ao arquivo Package.appxmanifest do projeto AppServiceProvider. Neste exemplo, é `<uap:AppService Name="com.microsoft.inventory"/>`.

    Um [ **ValueSet** ](https://msdn.microsoft.com/library/windows/apps/dn636131) denominado **message** é criado para especificar o comando que queremos enviar para o serviço de aplicativo. O serviço de aplicativo de exemplo espera que um comando indique qual das duas ações deve ser tomada. Podemos obter o índice da caixa de texto no ClientApp e depois chamar o serviço com o comando "Item" para obter a descrição do item. Em seguida, podemos fazer a chamada com o comando "Price" para obter o preço do item. O texto do botão é definido como o resultado.

    Como [**AppServiceResponseStatus**](https://msdn.microsoft.com/library/windows/apps/dn921724) apenas indica se o sistema operacional conseguiu conectar a chamada ao serviço de aplicativo, verificamos a tecla "Status" no [ **ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) recebida do serviço de aplicativo para garantir que ele conseguiu atender à solicitação.

6.  No Visual Studio, defina o projeto ClientApp como o projeto de inicialização na janela do Gerenciador de Soluções e execute a solução. Insira o número 1 na caixa de texto e clique no botão. Você deve obter "Chair : Price = 88.99" de volta do serviço.

    ![exemplo de aplicativo exibindo o preço da cadeira=88.99](images/appserviceclientapp.png)

Se a chamada de serviço de aplicativo falhar, verifique o seguinte no ClientApp:

1.  Verifique se o nome da família de pacotes atribuído à conexão de serviço de estoque corresponde ao nome da família de pacotes do aplicativo AppServiceProvider. Consulte: **button\_Click()**`this.inventoryService.PackageFamilyName = "...";`).
2.  Em **button\_Click()**, verifique se o nome do serviço de aplicativo atribuído à conexão do serviço de estoque corresponde ao nome do serviço de aplicativo no arquivo Package.appxmanifest do AppServiceProvider. Veja: `this.inventoryService.AppServiceName = "com.microsoft.inventory";`.
3.  Certifique-se de que o aplicativo AppServiceProvider foi implantado (No Gerenciador de Soluções, clique com o botão direito do mouse na solução e escolha **Implantar**).

## <a name="debug-the-app-service"></a>Depurar o serviço de aplicativo


1.  Certifique-se de que toda a solução esteja implantada antes da depuração, pois o aplicativo do provedor de serviços de aplicativo deve estar implantado para que o serviço possa ser chamado. (No Visual Studio, **Compilar &gt; Implantar Solução**).
2.  No Gerenciador de Soluções, clique com o botão direito do mouse no projeto AppServiceProvider e escolha **Propriedades**. Na guia **Depurar**, altere **Iniciar ação** para **Não iniciar, mas sim depurar meu código quando iniciar**.
3.  No projeto MyAppService, no arquivo Class1.cs, defina um ponto de interrupção em OnRequestReceived().
4.  Defina o projeto AppServiceProvider como o projeto de inicialização e pressione F5.
5.  Inicie ClientApp no menu Iniciar (não no Visual Studio).
6.  Insira o número 1 na caixa de texto e pressione o botão. O depurador será interrompido na chamada de serviço de aplicativo no ponto de interrupção no serviço de aplicativo.

## <a name="debug-the-client"></a>Depurar o cliente

1.  Siga as instruções na etapa anterior para depurar o serviço de aplicativo.
2.  Inicie ClientApp no menu Iniciar.
3.  Anexe o depurador ao processo ClientApp.exe (não ao processo ApplicationFrameHost.exe). (No Visual Studio, escolha **Depurar &gt; Anexar ao Processo...**.)
4.  No projeto ClientApp, defina um ponto de interrupção em **button\_Click()**.
5.  Os pontos de interrupção no cliente e no serviço de aplicativo agora serão atingidos quando você inserir o número 1 na caixa de texto do ClientApp e clicar no botão.

## <a name="remarks"></a>Comentários

Este exemplo fornece uma introdução simples para criar um serviço de aplicativo e chamá-lo a partir de outro aplicativo. Os principais itens a serem observados são a criação de uma tarefa em segundo plano para hospedar o serviço de aplicativo, a adição da extensão windows.appservice ao arquivo Package.appxmanifest do aplicativo de provedor de serviços de aplicativo, obtendo o nome da família de pacotes do aplicativo do provedor de serviços de aplicativo para que possamos conectar a ele a partir do aplicativo cliente, e usando [**Windows.ApplicationModel.AppService.AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704) para chamar o serviço.

## <a name="full-code-for-myappservice"></a>Código completo para MyAppService

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
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn't terminated.
            taskInstance.Canceled += OnTaskCanceled; // Associate a cancellation handler with the background task.

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // Get a deferral because we use an awaitable API below to respond to the message
            // and we don't want this call to get cancelled while we are waiting.
            var messageDeferral = args.GetDeferral();

            ValueSet message = args.Request.Message;
            ValueSet returnData = new ValueSet();

            string command = message["Command"] as string;
            int? inventoryIndex = message["ID"] as int?;

            if (inventoryIndex.HasValue &&
                 inventoryIndex.Value >= 0 &&
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
            messageDeferral.Complete(); // Complete the deferral so that the platform knows that we're done responding to the app service call.
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

## <a name="related-topics"></a>Tópicos relacionados

* [Converter um serviço de app para ser executado no mesmo processo de seu app host](convert-app-service-in-process.md)
* [Dar suporte a seu app com tarefas em segundo plano](support-your-app-with-background-tasks.md)

