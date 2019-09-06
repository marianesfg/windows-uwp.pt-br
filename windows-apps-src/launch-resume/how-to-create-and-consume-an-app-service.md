---
title: Criar e consumir um serviço de app
description: Saiba como escrever um aplicativo UWP (Plataforma Universal do Windows) que pode fornecer serviços a outros aplicativos UWP e também como consumir esses serviços.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: comunicação de aplicativo para aplicativo, comunicação entre processos, IPC, mensagens em segundo plano, comunicação em segundo plano, aplicativo para aplicativo, serviço de aplicativo
ms.date: 01/16/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8d6edc49bc97a336b8d722c496c1980a5f9b0efb
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393555"
---
# <a name="create-and-consume-an-app-service"></a>Criar e consumir um serviço de app

Os serviços de aplicativo são aplicativos UWP que fornecem serviços a outros aplicativos UWP. Eles são análogos aos serviços da Web em um dispositivo. Um serviço de aplicativo é executado como uma tarefa em segundo plano no aplicativo host e pode fornecer seu serviço a outros aplicativos. Por exemplo, um serviço de aplicativo pode fornecer um serviço de scanner de código de barras que outros aplicativos podem usar. Ou talvez um conjunto de aplicativos Enterprise tenha um serviço de verificação ortográfica que está disponível para os outros aplicativos no pacote.  Os serviços de aplicativo permitem que você crie serviços sem interface do usuário que os aplicativos podem chamar no mesmo dispositivo e, a partir do Windows 10, versão 1607, nos dispositivos remotos.

A partir do Windows 10, versão 1607, você pode criar serviços de aplicativo que são executados no mesmo processo do aplicativo host. Este artigo se concentra na criação e no consumo de um serviço de aplicativo executado em um processo separado em segundo plano. Consulte [Converter um serviço de aplicativo para ser executado no mesmo processo de seu aplicativo host](convert-app-service-in-process.md) para obter mais detalhes sobre a execução de um serviço de aplicativo no mesmo processo do provedor.

Para obter um exemplo de código de serviço do aplicativo, consulte [Amostras de aplicativos da Plataforma Universal do Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices).

## <a name="create-a-new-app-service-provider-project"></a>Criar um novo projeto de provedor de serviços de aplicativo

Nestas instruções, criaremos tudo em uma única solução por questão de simplicidade.

1. No Visual Studio 2015 ou posterior, crie um novo projeto de aplicativo UWP e nomeie-o **AppServiceProvider**.
    1. Selecione **arquivo > novo projeto de >...** 
    2. Na caixa de diálogo **criar um novo projeto** , selecione **aplicativo em branco (universal do C#Windows)** . Esse será o aplicativo que disponibilizará o serviço de aplicativo para outros aplicativos UWP.
    3. Clique em **Avançar**e nomeie o projeto **AppServiceProvider**, escolha um local para ele e, em seguida, clique em **criar**.

2. Quando solicitado a selecionar um **destino** e uma **versão mínima** para o projeto, selecione pelo menos **10.0.14393**. Se você quiser usar o novo atributo **SupportsMultipleInstances** , deverá usar o visual Studio 2017 ou o visual Studio 2019 e o Target **10.0.15063** (**atualização do Windows 10 para criadores**) ou superior.

<span id="appxmanifest"/>

## <a name="add-an-app-service-extension-to-packageappxmanifest"></a>Adicionar uma extensão do serviço de aplicativo ao Package. appxmanifest

No projeto **AppServiceProvider** , abra o arquivo **Package. appxmanifest** em um editor de texto: 

1. Clique com o botão direito do mouse no **Gerenciador de soluções**. 
2. Selecione **abrir com**. 
3. Selecione **Editor de XML (texto)** . 

Adicione a seguinte `AppService` extensão dentro do `<Application>` elemento. Este exemplo anuncia o serviço `com.microsoft.inventory` e é o que identifica esse aplicativo como um provedor de serviços de aplicativo. O serviço real será implementado como uma tarefa em segundo plano. O projeto do serviço de aplicativo expõe o serviço a outros aplicativos. Recomendamos usar um estilo de nome de domínio reverso para o nome do serviço.

Observe que o prefixo de namespace `xmlns:uap4` e o atributo `uap4:SupportsMultipleInstances` só são válidos se você estiver visando a versão 10.0.15063 ou superior do SDK do Windows. Você pode removê-los tranquilamente se estiver visando versões mais antigas do SDK.

``` xml
<Package
    ...
    xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    ...
    <Applications>
        <Application Id="AppServiceProvider.App"
          Executable="$targetnametoken$.exe"
          EntryPoint="AppServiceProvider.App">
          ...
          <Extensions>
            <uap:Extension Category="windows.appService" EntryPoint="MyAppService.Inventory">
              <uap3:AppService Name="com.microsoft.inventory" uap4:SupportsMultipleInstances="true"/>
            </uap:Extension>
          </Extensions>
          ...
        </Application>
    </Applications>
```

O `Category` atributo identifica esse aplicativo como um provedor de serviço de aplicativo.

O `EntryPoint` atributo identifica a classe qualificada de namespace que implementa o serviço, que implementaremos em seguida.

O `SupportsMultipleInstances` atributo indica que sempre que o serviço de aplicativo é chamado, ele deve ser executado em um novo processo. Isso não é necessário, mas está disponível para você se você precisar dessa funcionalidade e estiver direcionando o SDK do 10.0.15063 (**atualização do Windows 10 para criadores**) ou superior. Ele também deve ser precedido pelo namespace `uap4`.

## <a name="create-the-app-service"></a>Criar o serviço de aplicativo

1.  Um serviço de aplicativo pode ser implementado como uma tarefa em segundo plano. Isso permite que um aplicativo em primeiro plano invoque um serviço de aplicativo em outro aplicativo. Para criar um serviço de aplicativo como uma tarefa em segundo plano, adicione um novo projeto de componente Windows Runtime à solução (**arquivo &gt; adicionar &gt; novo projeto**) chamado **MyAppService**. Na caixa de diálogo **Adicionar novo projeto** , escolha **instalado > componente C# do Visual > Windows Runtime (universal do Windows)** .
2.  No projeto **AppServiceProvider** , adicione uma referência projeto-para-projeto ao novo projeto **MyAppService** (na **Gerenciador de soluções**, clique com o botão direito do mouse no projeto **AppServiceProvider** > **Adicionar**  >  > **Solução**de**projetos** > dereferência,selecioneMyAppServiceok). >  Esta etapa é fundamental porque, se você não adicionar a referência, o serviço de aplicativo não se conectará em tempo de execução.
3.  No projeto **MyAppService** , adicione as seguintes instruções **using** à parte superior de **Class1.cs**:
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  Renomeie **Class1.cs** como **Inventory.cs**e substitua o código de stub para **Class1** por uma nova classe de tarefa em segundo plano chamada **Inventory**:

    ```cs
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Get a deferral so that the service isn't terminated.
            this.backgroundTaskDeferral = taskInstance.GetDeferral();

            // Associate a cancellation handler with the background task.
            taskInstance.Canceled += OnTaskCanceled;

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // This function is called when the app service receives a request.
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

    **Executar** é chamado quando a tarefa em segundo plano é criada. Como as tarefas em segundo plano são encerradas após a conclusão de **Run**, o código é adiado para que a tarefa em segundo plano fique ativa para atender às solicitações. Um serviço de aplicativo que é implementado como uma tarefa em segundo plano permanecerá ativo por cerca de 30 segundos depois de receber uma chamada, a menos que seja chamado novamente dentro dessa janela de tempo ou que um adiamento seja retirado. Se o serviço de aplicativo for implementado no mesmo processo que o chamador, o tempo de vida do serviço de aplicativo estará vinculado ao tempo de vida do chamador.

    O tempo de vida do serviço de aplicativo depende do chamador:

    * Se o chamador estiver em primeiro plano, o tempo de vida do serviço de aplicativo será o mesmo que o chamador.
    * Se o chamador estiver em segundo plano, o serviço de aplicativo obterá 30 segundos para ser executado. Um adiamento fornece 5 segundos adicionais, uma vez.

    **OnTaskCanceled** é chamado quando a tarefa é cancelada. A tarefa é cancelada quando o aplicativo cliente descarta o [AppServiceConnection](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection), o aplicativo cliente é suspenso, o sistema operacional é desligado ou suspenso, ou o sistema operacional fica sem recursos para executar a tarefa.

## <a name="write-the-code-for-the-app-service"></a>Escrever o código para o serviço de aplicativo

**OnRequestReceived** é onde o código para o serviço de aplicativo vai. Substitua o **OnRequestReceived** do stub em **Inventory.cs** do **MyAppService**pelo código deste exemplo. Esse código obtém um índice para um item de estoque e passa-o, juntamente com uma sequência de comando, ao serviço para recuperar o nome e o preço do item de estoque especificado. Para seus próprios projetos, adicione o código de tratamento de erro.

```cs
private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below to respond to the message
    // and we don't want this call to get canceled while we are waiting.
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

    try
    {
        // Return the data to the caller.
        await args.Request.SendResponseAsync(returnData);
    }
    catch (Exception e)
    {
        // Your exception handling code here.
    }
    finally
    {
        // Complete the deferral so that the platform knows that we're done responding to the app service call.
        // Note for error handling: this must be called even if SendResponseAsync() throws an exception.
        messageDeferral.Complete();
    }
}
```

Observe que **OnRequestReceived** é **Async** porque podemos fazer uma chamada de método awaitable para [SendResponseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) neste exemplo.

Um adiamento é usado para que o serviço possa usar métodos **assíncronos** no manipulador **OnRequestReceived** . Isso garante que a chamada para **OnRequestReceived** não seja concluída até que o processamento da mensagem seja concluído.  [SendResponseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) envia o resultado para o chamador. **SendResponseAsync** não sinaliza a conclusão da chamada. É a conclusão do adiamento que sinaliza para [SendMessageAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.sendmessageasync) que o **OnRequestReceived** foi concluído. A chamada para **SendResponseAsync** é encapsulada em um bloco try/finally porque você deve concluir o adiamento mesmo se o **SendResponseAsync** lançar uma exceção.

Os serviços de aplicativos usam objetos [ValueSet](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet) para trocar informações. O tamanho dos dados que você pode passar é limitado apenas pelos recursos do sistema. Não há chaves predefinidas para uso em **ValueSet**. Você deve determinar quais valores de chave serão usados para definir o protocolo do serviço de aplicativo. O chamador deve ser escrito considerando esse protocolo. Neste exemplo, escolhemos uma chave chamada `Command` que contém um valor que indica se desejamos que o serviço de aplicativo forneça o nome do item de estoque ou seu preço. O índice do nome do estoque é armazenado na chave `ID`. O valor retornado é armazenado na chave `Result`.

Uma enumeração [AppServiceClosedStatus](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService.AppServiceClosedStatus) é retornada ao chamador para indicar se a chamada para o serviço de aplicativo foi bem-sucedida ou falhou. Um exemplo de como a chamada para o serviço de aplicativo pode falhar é se o sistema operacional anular o ponto de extremidade de serviço porque os recursos foram excedidos. Você pode retornar informações de erro adicionais por meio do [valorset](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet). Neste exemplo, usamos uma chave chamada `Status` para retornar informações de erro mais detalhadas para o chamador.

A chamada para [SendResponseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) retorna o [valorset](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet) para o chamador.

## <a name="deploy-the-service-app-and-get-the-package-family-name"></a>Implantar o aplicativo de serviço e obter o nome da família de pacotes

O provedor de serviço de aplicativo deve ser implantado antes que você possa chamá-lo de um cliente. Você pode implantá-lo selecionando **compilar > implantar solução** no Visual Studio.

Você também precisará do nome da família de pacotes do provedor de serviços de aplicativo para chamá-lo. Você pode obtê-lo abrindo o arquivo **Package. appxmanifest** do projeto **AppServiceProvider** na exibição do designer (clique duas vezes nele no **Gerenciador de soluções**). Selecione a guia **empacotamento** , copie o valor ao lado de **nome da família de pacotes**e cole-o em algum lugar como o bloco de notas por enquanto.

## <a name="write-a-client-to-call-the-app-service"></a>Escrever um cliente para chamar o serviço de aplicativo

1.  Adicione um novo projeto em branco do aplicativo Universal do Windows à solução com **Arquivo &gt; Adicionar &gt; Novo Projeto**. Na caixa de diálogo **Adicionar novo projeto** , escolha **instalado > aplicativo C# Visual > em branco (universal do Windows)** e nomeie-o **ClientApp**.

2.  No projeto **ClientApp** , adicione a seguinte instrução **using** à parte superior de **MainPage.XAML.cs**:
    ```cs
    using Windows.ApplicationModel.AppService;
    ```

3.  Adicione uma caixa de texto chamada **TextBox** e um botão para **MainPage. XAML**.

4.  Adicione um manipulador de clique de botão ao botão chamado **Button_Click**e adicione a palavra-chave **Async** à assinatura do manipulador de botão.

5. Substitua o stub do manipulador de clique de botão pelo código a seguir. Certifique-se de incluir a declaração de campo `inventoryService`.
    ```cs
   private AppServiceConnection inventoryService;

   private async void button_Click(object sender, RoutedEventArgs e)
   {
       // Add the connection.
       if (this.inventoryService == null)
       {
           this.inventoryService = new AppServiceConnection();

           // Here, we use the app service name defined in the app service 
           // provider's Package.appxmanifest file in the <Extension> section.
           this.inventoryService.AppServiceName = "com.microsoft.inventory";

           // Use Windows.ApplicationModel.Package.Current.Id.FamilyName 
           // within the app service provider to get this value.
           this.inventoryService.PackageFamilyName = "Replace with the package family name";

           var status = await this.inventoryService.OpenAsync();

           if (status != AppServiceConnectionStatus.Success)
           {
               textBox.Text= "Failed to connect";
               this.inventoryService = null;
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
           // Get the data  that the service sent to us.
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

       textBox.Text = result;
   }
   ```
    
    Substitua o nome da família de pacotes na linha `this.inventoryService.PackageFamilyName = "Replace with the package family name";` pelo nome da família de pacotes do projeto **AppServiceProvider** que você obteve acima em [Implantar o aplicativo de serviço e obter o nome da família de pacotes](#deploy-the-service-app-and-get-the-package-family-name).

    > [!NOTE]
    > Certifique-se de colar o literal de cadeia de caracteres, em vez de colocá-lo em uma variável. Ele não funcionará se você usar uma variável.

    O código primeiro estabelece uma conexão com o serviço de aplicativo. A conexão permanecerá aberta até que você descarte `this.inventoryService`. O nome do serviço de aplicativo deve `AppService` corresponder ao `Name` atributo do elemento que você adicionou ao arquivo **Package. appxmanifest** do projeto **AppServiceProvider** . Neste exemplo, é `<uap3:AppService Name="com.microsoft.inventory"/>`.

    Um [valor](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet) nomeado chamado `message` é criado para especificar o comando que queremos enviar ao serviço de aplicativo. O serviço de aplicativo de exemplo espera que um comando indique qual das duas ações deve ser tomada. Obtemos o índice da caixa de texto no aplicativo cliente e, em seguida, chamamos o serviço com `Item` o comando para obter a descrição do item. Em seguida, fazemos a chamada com o comando `Price` para obter o preço do item. O texto do botão é definido como o resultado.

    Como [AppServiceResponseStatus](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService.AppServiceResponseStatus) apenas indica se o sistema operacional foi capaz de conectar a chamada ao serviço de aplicativo, verificamos a `Status` chave no [valor](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet) que recebemos do serviço de aplicativo para garantir que ele pudesse atender ao Quest.

6. Defina o projeto **ClientApp** como o projeto de inicialização (clique com o botão direito do mouse no **Gerenciador de soluções** > **definir como projeto de inicialização**) e execute a solução. Insira o número 1 na caixa de texto e clique no botão. Você deve obter a "cadeira: Price = 88,99 "de volta do serviço.

    ![exemplo de aplicativo exibindo o preço da cadeira=88.99](images/appserviceclientapp.png)

Se a chamada do serviço de aplicativo falhar, verifique o seguinte no projeto **ClientApp** :

1.  Verifique se o nome da família de pacotes atribuído à conexão do serviço de inventário corresponde ao nome da família de pacotes do aplicativo **AppServiceProvider** . Consulte a linha no **botão\_clique** em `this.inventoryService.PackageFamilyName = "...";`com.
2.  Em **clique\_no botão**, verifique se o nome do serviço de aplicativo atribuído à conexão do serviço de inventário corresponde ao nome do serviço de aplicativo no arquivo **Package. appxmanifest** do **AppServiceProvider**. Veja: `this.inventoryService.AppServiceName = "com.microsoft.inventory";`.
3.  Verifique se o aplicativo **AppServiceProvider** foi implantado. (No **Gerenciador de soluções**, clique com o botão direito do mouse na solução e escolha **implantar solução**).

## <a name="debug-the-app-service"></a>Depurar o serviço de aplicativo

1.  Certifique-se de que a solução esteja implantada antes da depuração, pois o aplicativo do provedor de serviços de aplicativo deve estar implantado para que o serviço possa ser chamado. (No Visual Studio, **Compilar &gt; Implantar Solução**).
2.  Na **Gerenciador de soluções**, clique com o botão direito do mouse no projeto **AppServiceProvider** e escolha **Propriedades**. Na guia **Depurar**, altere **Iniciar ação** para **Não iniciar, mas sim depurar meu código quando iniciar**. (Observe que, se você estivesse usando o C++ para implementar o provedor de serviços de aplicativo, na guia **Depuração** você alteraria **Iniciar aplicativo** para **Não**).
3.  No projeto **MyAppService** , no arquivo **Inventory.cs** , defina um ponto de interrupção em **OnRequestReceived**.
4.  Defina o projeto **AppServiceProvider** como o projeto de inicialização e pressione **F5**.
5.  Inicie o **ClientApp** no menu iniciar (não no Visual Studio).
6.  Insira o número 1 na caixa de texto e pressione o botão. O depurador será interrompido na chamada de serviço de aplicativo no ponto de interrupção no serviço de aplicativo.

## <a name="debug-the-client"></a>Depurar o cliente

1.  Siga as instruções na etapa anterior para depurar o cliente que chama o serviço de aplicativo.
2.  Inicie o **ClientApp** no menu iniciar.
3.  Anexe o depurador ao processo **ClientApp. exe** (não o processo **ApplicationFrameHost. exe** ). (No Visual Studio, escolha **Depurar &gt; Anexar ao Processo...** .)
4.  No projeto **ClientApp** , defina um ponto de interrupção **no\_clique do botão**.
5.  Os pontos de interrupção no cliente e no serviço de aplicativo agora serão atingidos quando você inserir o número 1 na caixa de texto de **ClientApp** e clicar no botão.

## <a name="general-app-service-troubleshooting"></a>Solução de problemas gerais do serviço de aplicativo

Se você encontrar um status de **AppUnavailable** depois de tentar se conectar a um serviço de aplicativo, verifique o seguinte:

- Verifique se o projeto de provedor de serviços de aplicativo e o projeto de serviço de aplicativo foram implantados. Ambos devem ser implantados antes de executar o cliente, caso contrário, o cliente não terá nada a que se conectar. Você pode implantar do Visual Studio usando **Build** > **Implantar solução**.
- No **Gerenciador de soluções**, verifique se o projeto do provedor de serviço de aplicativo tem uma referência projeto-para-projeto para o projeto que implementa o serviço de aplicativo.
- Verifique se a `<Extensions>` entrada e seus elementos filho foram adicionados ao arquivo **Package. appxmanifest** pertencente ao projeto de provedor do serviço de aplicativo, conforme especificado acima em [Adicionar uma extensão de serviço de aplicativo a Package. appxmanifest](#appxmanifest).
- Verifique se a cadeia de caracteres [AppServiceConnection. AppServiceName](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.appservicename) no cliente que chama o provedor de serviço de `<uap3:AppService Name="..." />` aplicativo corresponde à especificada no arquivo **Package. appxmanifest** do projeto do provedor do serviço de aplicativo.
- Verifique se [AppServiceConnection. PackageFamilyName](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.packagefamilyname) corresponde ao nome da família de pacotes do componente do provedor de serviço de aplicativo, conforme especificado acima em [Adicionar uma extensão do serviço de aplicativo ao Package. appxmanifest](#appxmanifest)
- Para serviços de aplicativo fora de processo, como aquele neste exemplo, valide que o `EntryPoint` especificado `<uap:Extension ...>` no elemento do arquivo **Package. appxmanifest** do seu projeto de provedor de serviço de aplicativo corresponde ao namespace e ao nome de classe do público classe que implementa [IBackgroundTask](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask) em seu projeto do serviço de aplicativo.

### <a name="troubleshoot-debugging"></a>Solucionar problemas de depuração

Se o depurador não parar em pontos de interrupção no provedor de serviços de aplicativo ou nos projetos de serviços de aplicativo, verifique o seguinte:

- Verifique se o projeto de provedor de serviços de aplicativo e o projeto de serviço de aplicativo foram implantados. Ambos devem ser implantados antes de executar o cliente. Você pode implantá-los do Visual Studio usando **Build** > **Implantar solução**.
- Verifique se o projeto que você deseja depurar está definido como o projeto de inicialização e se as propriedades de depuração desse projeto estão definidas para não executar o projeto quando **F5** é pressionado. Clique com o botão direito do mouse no projeto, clique em **Propriedades** e, em seguida, **Depurar** (ou **Depuração** em C++). Em C#, altere **Iniciar ação** para **Não iniciar, mas sim depurar meu código quando iniciar**. Em C++, defina **Iniciar aplicativo** como **Não**.

## <a name="remarks"></a>Comentários

Este exemplo fornece uma introdução para criar um serviço de aplicativo que seja executado como uma tarefa em segundo plano e chamá-lo a partir de outro aplicativo. Os principais itens a serem observados são:

* Crie uma tarefa em segundo plano para hospedar o serviço de aplicativo.
* Adicione a `windows.appService` extensão ao arquivo **Package. appxmanifest** do provedor do serviço de aplicativo.
* Obtenha o nome da família de pacotes do provedor de serviços de aplicativo para que possamos se conectar a ele por meio do aplicativo cliente.
* Adicione uma referência projeto-para-projeto do projeto do provedor do serviço de aplicativo ao projeto do serviço de aplicativo.
* Use [Windows. ApplicationModel. AppService. AppServiceConnection](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection) para chamar o serviço.

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
            // Get a deferral so that the service isn't terminated.
            this.backgroundTaskDeferral = taskInstance.GetDeferral();

            // Associate a cancellation handler with the background task.
            taskInstance.Canceled += OnTaskCanceled;

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // Get a deferral because we use an awaitable API below to respond to the message
            // and we don't want this call to get canceled while we are waiting.
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

            // Return the data to the caller.
            await args.Request.SendResponseAsync(returnData);

            // Complete the deferral so that the platform knows that we're done responding to the app service call.
            // Note for error handling: this must be called even if SendResponseAsync() throws an exception.
            messageDeferral.Complete();
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

* [Converter um serviço de aplicativo para ser executado no mesmo processo de seu aplicativo host](convert-app-service-in-process.md)
* [Oferecer suporte a tarefas em segundo plano em seu aplicativo](support-your-app-with-background-tasks.md)
* [Exemplo de código do serviçoC#de C++aplicativo (, e VB)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)
