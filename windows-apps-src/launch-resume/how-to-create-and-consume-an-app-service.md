---
title: Criar e consumir um serviço de app
description: Saiba como escrever um aplicativo UWP (Plataforma Universal do Windows) que pode fornecer serviços a outros aplicativos UWP e também como consumir esses serviços.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: comunicação, a comunicação entre processos, IPC, mensagens, o serviço de aplicativo em segundo plano comunicação, aplicativo, em segundo plano
ms.date: 1/16/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5239bff53bb0e5383bce28b4d781a0ab6a41c3af
ms.sourcegitcommit: cfdc854fede8e586202523cdb59d3d0a2f5b4b36
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/17/2019
ms.locfileid: "9013955"
---
# <a name="create-and-consume-an-app-service"></a>Criar e consumir um serviço de app

Os serviços de aplicativo são aplicativos UWP que fornecem serviços a outros aplicativos UWP. Eles são análogos aos serviços da Web em um dispositivo. Um serviço de aplicativo é executado como uma tarefa em segundo plano no aplicativo host e pode fornecer seu serviço a outros aplicativos. Por exemplo, um serviço de aplicativo pode fornecer um serviço de scanner de código de barras que outros aplicativos podem usar. Ou talvez um conjunto de aplicativos Enterprise tenha um serviço de verificação ortográfica que está disponível para os outros aplicativos no pacote.  Os serviços de aplicativo permitem que você crie serviços sem interface do usuário que os aplicativos podem chamar no mesmo dispositivo e, a partir do Windows 10, versão 1607, nos dispositivos remotos.

A partir do Windows 10, versão 1607, você pode criar serviços de aplicativo que são executados no mesmo processo do aplicativo host. Este artigo se concentra na criação e no consumo de um serviço de aplicativo executado em um processo separado em segundo plano. Consulte [Converter um serviço de aplicativo para ser executado no mesmo processo de seu aplicativo host](convert-app-service-in-process.md) para obter mais detalhes sobre a execução de um serviço de aplicativo no mesmo processo do provedor.

Para obter um exemplo de código de serviço do aplicativo, consulte [Amostras de aplicativos da Plataforma Universal do Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices).

## <a name="create-a-new-app-service-provider-project"></a>Criar um novo projeto de provedor de serviços de aplicativo

Nestas instruções, criaremos tudo em uma única solução por questão de simplicidade.

1. No Visual Studio 2015 ou posterior, crie um novo projeto de aplicativo UWP e chame-o de **AppServiceProvider**.
    1. Selecione o **arquivo gt _ gt _ projeto...** 
    2. Na caixa de diálogo **Novo projeto** , selecione **gt _ Visual c# gt _ de instalado o aplicativo em branco (Universal Windows)**. Esse será o aplicativo que disponibilizará o serviço de aplicativo para outros aplicativos UWP.
    3. Nomeie o projeto **AppServiceProvider**, escolha um local para ele e clique em **Okey**.

2. Quando for solicitado a selecionar um **destino** e a **versão mínima** do projeto, selecione pelo menos **10.0.14393**. Se você quiser usar o novo atributo **SupportsMultipleInstances** , você deve usar o Visual Studio 2017 e o destino **10.0.15063** (**Windows 10 para criadores**) ou superior.

<span id="appxmanifest"/>

## <a name="add-an-app-service-extension-to-packageappxmanifest"></a>Adicionar uma extensão de serviço de aplicativo ao Package. appxmanifest

No projeto **AppServiceProvider** , abra o arquivo **Package. appxmanifest** em um editor de texto: 

1. Clique com botão direito-lo no **Gerenciador de soluções**. 
2. Selecione **Abrir com**. 
3. Selecione **Editor XML (texto)**. 

Adicione o seguinte `AppService` extensão dentro do `<Application>` elemento. Este exemplo anuncia o serviço `com.microsoft.inventory` e é o que identifica esse aplicativo como um provedor de serviços de aplicativo. O serviço real será implementado como uma tarefa em segundo plano. O projeto do serviço de aplicativo expõe o serviço a outros aplicativos. Recomendamos o uso de um estilo de nome de domínio reverso para o nome do serviço.

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

O `SupportsMultipleInstances` atributo indica que cada vez que o serviço de aplicativo é chamado que ele deve ser executado em um novo processo. Isso não é obrigatório, mas está disponível caso você precise dessa funcionalidade e esteja segmentando o 10.0.15063 SDK (**Windows 10 para criadores**) ou superior. Ele também deve ser precedido pelo namespace `uap4`.

## <a name="create-the-app-service"></a>Criar o serviço de aplicativo

1.  Um serviço de aplicativo pode ser implementado como uma tarefa em segundo plano. Isso permite que um aplicativo em primeiro plano invoque um serviço de aplicativo em outro aplicativo. Para criar um serviço de aplicativo como uma tarefa em segundo plano, adicione um novo projeto de componente de tempo de execução do Windows à solução (**arquivo &gt; adicionar &gt; novo projeto**) chamada **MyAppService**. Na caixa de diálogo **Adicionar novo projeto** , escolha **gt _ Visual c# gt _ de instalado o componente de tempo de execução do Windows (Windows Universal)**.
2.  No projeto **AppServiceProvider** , adicione uma referência de projeto a projeto ao novo projeto **MyAppService** (no **Gerenciador de soluções**, clique com botão direito no de projeto gt **AppServiceProvider** _ **Adicionar**  >  ** Referência** > **projetos** > **soluções**, selecione **MyAppService** > **Okey**). Esta etapa é fundamental porque, se você não adicionar a referência, o serviço de aplicativo não se conectará em tempo de execução.
3.  No projeto **MyAppService** , adicione as seguintes instruções **using** para a parte superior de **Class1.cs**:
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  Renomeie **Class1.cs** para **Inventory.cs**e substitua o código stub para **Class1** com uma nova classe de tarefa em segundo plano chamada **inventário**:

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

    **Executar** é chamado quando a tarefa em segundo plano é criada. Como as tarefas em segundo plano são encerradas após a conclusão de **Run**, o código é adiado para que a tarefa em segundo plano fique ativa para atender às solicitações. Um serviço de aplicativo implementado como uma tarefa em segundo plano permanecerá ativo por cerca de 30 segundos após receber uma chamada, a menos que seja chamado novamente dentro daquele período ou que ocorra um adiamento. Se o serviço de aplicativo for implementado no mesmo processo que o chamador, o tempo de vida do serviço de aplicativo é vinculado ao tempo de vida do chamador.

    O tempo de vida do serviço de aplicativo depende do chamador:

    * Se o chamador estiver em primeiro plano, o tempo de vida do serviço de aplicativo é o mesmo que o chamador.
    * Se o chamador estiver em segundo plano, o serviço de aplicativo recebe 30 segundos para ser executado. Um adiamento fornece 5 segundos adicionais, uma vez.

    **OnTaskCanceled** é chamado quando a tarefa é cancelada. A tarefa é cancelada quando o aplicativo cliente descarta o [AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/dn921704), o aplicativo cliente é suspenso, o sistema operacional é desligado ou suspenso ou executa o sistema operacional está sem recursos para executar a tarefa.

## <a name="write-the-code-for-the-app-service"></a>Escrever o código para o serviço de aplicativo

**OnRequestReceived** é onde o código para o serviço de aplicativo vai. Substitua o stub **OnRequestReceived** em **MyAppService**do **Inventory.cs** com o código deste exemplo. Esse código obtém um índice para um item de estoque e passa-o, juntamente com uma sequência de comando, ao serviço para recuperar o nome e o preço do item de estoque especificado. Para seus próprios projetos, adicione o código de tratamento de erro.

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

Observe que **OnRequestReceived** **async** porque fazemos um método que pode ser aguardado para [SendResponseAsync](https://msdn.microsoft.com/library/windows/apps/dn921722) neste exemplo.

Um adiamento é obtido para que o serviço possa usar métodos **assíncronos** no manipulador **OnRequestReceived** . Isso garante que a chamada para **OnRequestReceived** não seja concluída até que o processamento da mensagem seja concluído.  [SendResponseAsync](https://msdn.microsoft.com/library/windows/apps/dn921722) envia o resultado para o chamador. **SendResponseAsync** não sinaliza a conclusão da chamada. É a conclusão do adiamento que sinaliza para [SendMessageAsync](https://msdn.microsoft.com/library/windows/apps/dn921712) que **OnRequestReceived** foi concluído. A chamada para **SendResponseAsync** é encapsulada em um bloco try/finally porque você deve concluir o adiamento mesmo se **SendResponseAsync** lança uma exceção.

Serviços de aplicativo usam objetos [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) para trocar informações. O tamanho dos dados que você pode passar é limitado apenas pelos recursos do sistema. Não há chaves predefinidas para uso em **ValueSet**. Você deve determinar quais valores de chave serão usados para definir o protocolo do serviço de aplicativo. O chamador deve ser escrito considerando esse protocolo. Neste exemplo, escolhemos uma chave chamada `Command` que contém um valor que indica se desejamos que o serviço de aplicativo forneça o nome do item de estoque ou seu preço. O índice do nome do estoque é armazenado na chave `ID`. O valor retornado é armazenado na chave `Result`.

Uma enumeração [AppServiceClosedStatus](https://msdn.microsoft.com/library/windows/apps/dn921703) é retornada ao chamador para indicar se a chamada para o serviço de aplicativo obteve êxito ou não. Um exemplo de como a chamada para o serviço de aplicativo pode falhar é se o sistema operacional anular o ponto de extremidade de serviço porque os recursos foram excedidos. Você pode retornar informações de erro adicionais por meio de [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131). Neste exemplo, usamos uma chave chamada `Status` para retornar informações de erro mais detalhadas para o chamador.

A chamada para [SendResponseAsync](https://msdn.microsoft.com/library/windows/apps/dn921722) retorna [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) ao chamador.

## <a name="deploy-the-service-app-and-get-the-package-family-name"></a>Implantar o aplicativo de serviço e obter o nome da família de pacotes

O provedor de serviços de aplicativo deve ser implantado antes de chamá-lo de um cliente. Você pode implantá-lo selecionando **Compilar gt _ implantar solução** no Visual Studio.

Você também precisará do nome da família de provedor de serviços de aplicativo para chamá-lo. Você pode obtê-lo, abrindo o arquivo de **Package. appxmanifest** do projeto **AppServiceProvider** no modo de exibição designer (clique duas vezes no **Gerenciador de soluções**). Selecione a guia de **empacotamento** , copie o valor ao lado do **nome da família**e cole-o em algum lugar como o bloco de notas por enquanto.

## <a name="write-a-client-to-call-the-app-service"></a>Escrever um cliente para chamar o serviço de aplicativo

1.  Adicione um novo projeto em branco do aplicativo Universal do Windows à solução com **Arquivo &gt; Adicionar &gt; Novo Projeto**. Na caixa de diálogo **Adicionar novo projeto** , escolha **gt _ Visual c# gt _ de instalado o aplicativo em branco (Universal Windows)** e nomeie-o **ClientApp**.

2.  No projeto **ClientApp** , adicione a seguinte instrução **usando** à parte superior de **MainPage.xaml.cs**:
    ```cs
    using Windows.ApplicationModel.AppService;
    ```

3.  Adicione uma caixa de texto chamado **textBox** e um botão para **MainPage. XAML**.

4.  Adicione um botão de manipulador de clique de botão chamado **botão_clicar**e adicione a palavra-chave **async** a assinatura do manipulador do botão.

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
    > Certifique-se de colar no literal de cadeia de caracteres, em vez de colocá-lo em uma variável. Ele não funcionará se você usar uma variável.

    Primeiramente, o código estabelece uma conexão com o serviço de aplicativo. A conexão permanecerá aberta até que você descarte `this.inventoryService`. O nome do serviço de aplicativo deve coincidir com o `AppService` do elemento `Name` atributo que você adicionou ao arquivo de **Package. appxmanifest** do projeto **AppServiceProvider** . Neste exemplo, é `<uap3:AppService Name="com.microsoft.inventory"/>`.

    [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) chamado `message` é criado para especificar o comando que queremos enviar para o serviço de aplicativo. O serviço de aplicativo de exemplo espera que um comando indique qual das duas ações deve ser tomada. Obtemos o índice da caixa de texto no aplicativo cliente e, em seguida, chamar o serviço com o `Item` comando para obter a descrição do item. Em seguida, fazemos a chamada com o comando `Price` para obter o preço do item. O texto do botão é definido como o resultado.

    Como [AppServiceResponseStatus](https://msdn.microsoft.com/library/windows/apps/dn921724) apenas indica se o sistema operacional conseguiu conectar a chamada ao serviço de aplicativo, verificamos a tecla `Status` no [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) que recebemos do serviço de aplicativo para garantir que ele conseguiu atender à solicitação.

6. Defina o projeto **ClientApp** como o projeto de inicialização (clique com botão direito-lo no **Gerenciador de soluções** > **Definir como projeto de inicialização**) e execute a solução. Insira o número 1 na caixa de texto e clique no botão. Você deve obter "Chair : Price = 88.99" de volta do serviço.

    ![exemplo de aplicativo exibindo o preço da cadeira=88.99](images/appserviceclientapp.png)

Se a chamada de serviço de aplicativo falhar, verifique o seguinte no projeto **ClientApp** :

1.  Verifique se o nome da família atribuído para a conexão de serviço de estoque corresponde o nome da família do aplicativo **AppServiceProvider** . Consulte a linha na **button\_Click** com `this.inventoryService.PackageFamilyName = "...";`.
2.  Na **button\_Click**, verifique se que o nome do serviço de aplicativo que é atribuído para a conexão de serviço de estoque corresponde ao nome do serviço de aplicativo no arquivo de **Package. appxmanifest** do **AppServiceProvider**. Veja: `this.inventoryService.AppServiceName = "com.microsoft.inventory";`.
3.  Certifique-se de que o aplicativo **AppServiceProvider** foi implantado. (No **Gerenciador de soluções**, clique com botão direito a solução e escolha **Implantar solução**).

## <a name="debug-the-app-service"></a>Depurar o serviço de aplicativo

1.  Certifique-se de que a solução esteja implantada antes da depuração, pois o aplicativo do provedor de serviços de aplicativo deve estar implantado para que o serviço possa ser chamado. (No Visual Studio, **Compilar &gt; Implantar Solução**).
2.  No **Gerenciador de soluções**, clique com botão direito no projeto **AppServiceProvider** e escolha **Propriedades**. Na guia **Depurar**, altere **Iniciar ação** para **Não iniciar, mas sim depurar meu código quando iniciar**. (Observe que, se você estivesse usando o C++ para implementar o provedor de serviços de aplicativo, na guia **Depuração** você alteraria **Iniciar aplicativo** para **Não**).
3.  No projeto **MyAppService** , no arquivo **Inventory.cs** , defina um ponto de interrupção em **OnRequestReceived**.
4.  Defina o projeto **AppServiceProvider** como o projeto de inicialização e pressione **F5**.
5.  Inicie **ClientApp** no menu Iniciar (não no Visual Studio).
6.  Insira o número 1 na caixa de texto e pressione o botão. O depurador será interrompido na chamada de serviço de aplicativo no ponto de interrupção no serviço de aplicativo.

## <a name="debug-the-client"></a>Depurar o cliente

1.  Siga as instruções na etapa anterior para depurar o cliente que chama o serviço de aplicativo.
2.  Inicie **ClientApp** no menu Iniciar.
3.  Anexe o depurador ao processo **ClientApp.exe** (não o processo de **ApplicationFrameHost.exe** ). (No Visual Studio, escolha **Depurar &gt; Anexar ao Processo...**.)
4.  No projeto **ClientApp** , defina um ponto de interrupção em **button\_Click**.
5.  Os pontos de interrupção no cliente e o serviço de aplicativo agora serão atingidos quando você inserir o número 1 na caixa de texto do **ClientApp** e clicar no botão.

## <a name="general-app-service-troubleshooting"></a>Solução de problemas gerais do serviço de aplicativo

Se você encontrar um status **AppUnavailable** depois de tentar se conectar a um serviço de aplicativo, verifique o seguinte:

- Verifique se o projeto de provedor de serviços de aplicativo e o projeto de serviço de aplicativo foram implantados. Ambos devem ser implantados antes de executar o cliente, caso contrário, o cliente não terá nada a que se conectar. Você pode implantar do Visual Studio usando **Build** > **Implantar solução**.
- No **Gerenciador de soluções**, certifique-se de que seu projeto de provedor de serviços de aplicativo tem uma referência de projeto a projeto ao projeto que implementa o serviço de aplicativo.
- Verifique o `<Extensions>` entrada e seus elementos filho foram adicionados ao arquivo **Package. appxmanifest** pertencente ao projeto de provedor de serviço aplicativo conforme especificado acima em [Adicionar uma extensão de serviço de aplicativo para Package. appxmanifest](#appxmanifest).
- Certifique-se de que a cadeia de caracteres [AppServiceConnection.AppServiceName](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.appservicename) no cliente que chama o provedor de serviços de aplicativo corresponde a `<uap3:AppService Name="..." />` especificado no arquivo de **Package. appxmanifest** do projeto de aplicativo serviço provedor.
- Certifique-se de que o [AppServiceConnection.PackageFamilyName](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.packagefamilyname) corresponde ao nome de família do pacote do componente de provedor de serviço do aplicativo conforme especificado acima em [Adicionar uma extensão de serviço de aplicativo para Package. appxmanifest](#appxmanifest)
- Para serviços fora do aplicativo, como o mostrado neste exemplo, confirme que o `EntryPoint` especificado no `<uap:Extension ...>` elemento do arquivo **Package. appxmanifest** de seu aplicativo serviço do projeto de provedor corresponda ao namespace e nome da classe do público de classe que implementa [IBackgroundTask](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask) em seu projeto de serviço de aplicativo.

### <a name="troubleshoot-debugging"></a>Solucionar problemas de depuração

Se o depurador não parar em pontos de interrupção no provedor de serviços de aplicativo ou nos projetos de serviços de aplicativo, verifique o seguinte:

- Verifique se o projeto de provedor de serviços de aplicativo e o projeto de serviço de aplicativo foram implantados. Ambos devem ser implantados antes de executar o cliente. Você pode implantá-los do Visual Studio usando **Build** > **Implantar solução**.
- Certifique-se de que o projeto que você deseja depurar esteja definido como o projeto de inicialização e que as propriedades de depuração para o projeto são configuradas para não executar o projeto quando **F5** for pressionado. Clique com o botão direito do mouse no projeto, clique em **Propriedades** e, em seguida, **Depurar** (ou **Depuração** em C++). Em C#, altere **Iniciar ação** para **Não iniciar, mas sim depurar meu código quando iniciar**. Em C++, defina **Iniciar aplicativo** como **Não**.

## <a name="remarks"></a>Comentários

Este exemplo fornece uma introdução para criar um serviço de aplicativo que seja executado como uma tarefa em segundo plano e chamá-lo a partir de outro aplicativo. Os principais aspectos a serem observados são:

* Crie uma tarefa em segundo plano para hospedar o serviço de aplicativo.
* Adicione o `windows.appService` extensão do arquivo de **Package. appxmanifest** do provedor de serviço de aplicativo.
* Obtenha o nome da família do provedor de serviços de aplicativo para que podemos nos conectar a ele do aplicativo cliente.
* Adicione uma referência de projeto a projeto do projeto de provedor de serviços de aplicativo ao projeto de serviço de aplicativo.
* Use [Windows.ApplicationModel.AppService.AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/dn921704) para chamar o serviço.

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

* [Converter um serviço de app para ser executado no mesmo processo de seu app host](convert-app-service-in-process.md)
* [Dar suporte a seu aplicativo com tarefas em segundo plano](support-your-app-with-background-tasks.md)
* [Exemplo de código de serviço de aplicativo (C#, C++ e VB)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)
