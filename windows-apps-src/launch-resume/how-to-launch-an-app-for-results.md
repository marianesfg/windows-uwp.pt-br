---
author: TylerMSFT
title: Iniciar um aplicativo para obter resultados
description: Saiba como iniciar um app a partir de outro app e trocar dados entre os dois. Isso é chamado de "iniciar" um aplicativo para obter resultados.
ms.assetid: AFC53D75-B3DD-4FF6-9FC0-9335242EE327
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0fbbe1978cc59afcc7d681331dadc9a06e3eb2d0
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6976348"
---
# <a name="launch-an-app-for-results"></a>Iniciar um aplicativo para obter resultados




**APIs importantes**

-   [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686)
-   [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)

Saiba como iniciar um aplicativo a partir de outro aplicativo e trocar dados entre os dois. Isso é chamado de *iniciar um aplicativo para obter resultados*. O exemplo mostra como usar [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686) para iniciar um aplicativo para obter resultados.

Novas APIs no Windows 10 de comunicação de aplicativo para aplicativo certifique para Windows aplicativos (e aplicativos Web do Windows) iniciar um aplicativo e troca de dados e arquivos. Isso permite que você crie soluções diversas de vários aplicativos. Usando essas novas APIs, tarefas complexas que exigiriam que o usuário usasse vários aplicativos podem agora ser executadas perfeitamente. Por exemplo, seu aplicativo pode iniciar um aplicativo de rede social para selecionar um contato ou iniciar um aplicativo de check-out para concluir um processo de pagamento.

O aplicativo que você iniciará para obter resultados será chamado de aplicativo iniciado. O aplicativo que inicia o aplicativo será referenciado como aplicativo de chamada. Para este exemplo, você escreverá o aplicativo de chamada e o aplicativo iniciado.

## <a name="step-1-register-the-protocol-to-be-handled-in-the-app-that-youll-launch-for-results"></a>Etapa 1: Registrar o protocolo que será manipulado no aplicativo que você iniciará para obter resultados


No arquivo Package.appxmanifest do aplicativo iniciado, adicione uma extensão do protocolo à seção **&lt;Application&gt;**. O exemplo aqui usa um protocolo fictício chamado **test-app2app**.

O atributo **ReturnResults** na extensão do protocolo aceita um destes valores:

-   **optional** — O aplicativo pode ser iniciado para obter resultados usando o método [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686) ou para não obter resultados usando [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476). Ao usar **optional**, o aplicativo iniciado deve determinar se ele foi iniciado para obter resultados. Ele pode fazer isso verificando o argumento do evento [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330). Se a propriedade [**IActivatedEventArgs.Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) do argumento retornar [**ActivationKind.ProtocolForResults**](https://msdn.microsoft.com/library/windows/apps/br224693), ou se o tipo do argumento do evento for [**ProtocolActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224742), o aplicativo foi iniciado por meio de **LaunchUriForResultsAsync**.
-   **always** — O aplicativo pode ser iniciado somente para obter resultados. Ou seja, ele pode responder somente a [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686).
-   **none** — O aplicativo não pode ser iniciado para obter resultados; ele pode responder somente a [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476).

Neste exemplo de extensão de protocolo, o aplicativo pode ser iniciado somente para obter resultados. Isso simplifica a lógica do método **OnActivated**, discutido a seguir, porque só será necessário lidar com o caso "iniciado para obter resultados" e não com as outras maneiras de ativação do aplicativo.

```xml
<Applications>
   <Application ...>

     <Extensions>
       <uap:Extension Category="windows.protocol">
         <uap:Protocol Name="test-app2app" ReturnResults="always">
           <uap:DisplayName>Test app-2-app</uap:DisplayName>
         </uap:Protocol>
       </uap:Extension>
     </Extensions>

   </Application>
</Applications>
```

## <a name="step-2-override-applicationonactivated-in-the-app-that-youll-launch-for-results"></a>Etapa 2: substituir Application.OnActivated no aplicativo que será iniciado para obter resultados


Se esse método ainda não existir no aplicativo iniciado, crie-o dentro da classe `App` definida em App.xaml.cs.

Em um aplicativo que permite selecionar seus amigos em uma rede social, essa função pode estar onde você abre a página do seletor de pessoas. No próximo exemplo, uma página denominada **LaunchedForResultsPage** é exibida quando o aplicativo é ativado para obter resultados. Verifique se a instrução **using** está incluída na parte superior do arquivo.

```cs
using Windows.ApplicationModel.Activation;
...
protected override void OnActivated(IActivatedEventArgs args)
{
    // Window management
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame == null)
    {
        rootFrame = new Frame();
        Window.Current.Content = rootFrame;
    }

    // Code specific to launch for results
    var protocolForResultsArgs = (ProtocolForResultsActivatedEventArgs)args;
    // Open the page that we created to handle activation for results.
    rootFrame.Navigate(typeof(LaunchedForResultsPage), protocolForResultsArgs);

    // Ensure the current window is active.
    Window.Current.Activate();
}
```

Como a extensão de protocolo no arquivo Package.appxmanifest especifica **ReturnResults** como **always**, o código que acabamos de mostrar pode converter `args` diretamente no [**ProtocolForResultsActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn906905) com confiança de que somente **ProtocolForResultsActivatedEventArgs** será enviada a **OnActivated** para esse aplicativo. Caso seu aplicativo possa ser ativado de outras formas além de iniciar para obter resultados, você pode verificar se a propriedade [**IActivatedEventArgs.Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) retorna [**ActivationKind.ProtocolForResults**](https://msdn.microsoft.com/library/windows/apps/br224693) para determinar se o aplicativo foi iniciado para obter resultados.

## <a name="step-3-add-a-protocolforresultsoperation-field-to-the-app-you-launch-for-results"></a>Etapa 3: Adicionar um campo ProtocolForResultsOperation ao aplicativo iniciado para obter resultados


```cs
private Windows.System.ProtocolForResultsOperation _operation = null;
```

Você usará o campo [**ProtocolForResultsOperation**](https://msdn.microsoft.com/library/windows/apps/dn906913) para sinalizar quando o aplicativo iniciado estiver pronto para retornar o resultado para o aplicativo de chamada. Neste exemplo, o campo é adicionado à classe **LaunchedForResultsPage** porque a operação de início para obter resultados será concluída nessa página e precisaremos acessá-la.

## <a name="step-4-override-onnavigatedto-in-the-app-you-launch-for-results"></a>Etapa 4: Substituir OnNavigatedTo() no aplicativo iniciado para obter resultados


Substitua o método [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) na página que será exibida quando o aplicativo for iniciado para obter resultados. Se esse método ainda não existir, crie-o dentro da classe para a página definida em &lt;pagename&gt;.xaml.cs. Verifique se a seguinte instrução **using** está incluída na parte superior do arquivo:

```cs
using Windows.ApplicationModel.Activation
```

O objeto [**NavigationEventArgs**](https://msdn.microsoft.com/library/windows/apps/br243285) no método [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) contém os dados passados do aplicativo de chamada. Os dados não podem exceder 100 KB e são armazenados em um objeto [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131).

Neste código de exemplo, o aplicativo iniciado espera que os dados enviados do aplicativo de chamada estejam em um [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) sob uma chave denominada **TestData**, porque o aplicativo de chamada do exemplo está codificado para enviar isso.

```cs
using Windows.ApplicationModel.Activation;
...
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    var protocolForResultsArgs = e.Parameter as ProtocolForResultsActivatedEventArgs;
    // Set the ProtocolForResultsOperation field.
    _operation = protocolForResultsArgs.ProtocolForResultsOperation;

    if (protocolForResultsArgs.Data.ContainsKey("TestData"))
    {
        string dataFromCaller = protocolForResultsArgs.Data["TestData"] as string;
    }
}
...
private Windows.System.ProtocolForResultsOperation _operation = null;
```

## <a name="step-5-write-code-to-return-data-to-the-calling-app"></a>Etapa 5: Escrever o código para retornar dados para o aplicativo chamador


No aplicativo iniciado, use [**ProtocolForResultsOperation**](https://msdn.microsoft.com/library/windows/apps/dn906913) para retornar dados para o aplicativo de chamada. Neste código de exemplo, um objeto [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) é criado com o valor a ser retornado ao aplicativo de chamada. O campo **ProtocolForResultsOperation** é usado para enviar o valor para o aplicativo de chamada.

```cs
    ValueSet result = new ValueSet();
    result["ReturnedData"] = "The returned result";
    _operation.ReportCompleted(result);
```

## <a name="step-6-write-code-to-launch-the-app-for-results-and-get-the-returned-data"></a>Etapa 6: Escrever o código para iniciar o aplicativo para obter resultados e os dados retornados


Inicie o aplicativo a partir de um método assíncrono no aplicativo de chamada, conforme mostrado no código de exemplo. Observe as instruções **using** que são necessárias para a compilação do código:

```cs
using System.Threading.Tasks;
using Windows.System;
...

async Task<string> LaunchAppForResults()
{
    var testAppUri = new Uri("test-app2app:"); // The protocol handled by the launched app
    var options = new LauncherOptions();
    options.TargetApplicationPackageFamilyName = "67d987e1-e842-4229-9f7c-98cf13b5da45_yd7nk54bq29ra";

    var inputData = new ValueSet();
    inputData["TestData"] = "Test data";

    string theResult = "";
    LaunchUriResult result = await Windows.System.Launcher.LaunchUriForResultsAsync(testAppUri, options, inputData);
    if (result.Status == LaunchUriStatus.Success &&
        result.Result != null &&
        result.Result.ContainsKey("ReturnedData"))
    {
        ValueSet theValues = result.Result;
        theResult = theValues["ReturnedData"] as string;
    }
    return theResult;
}
```

Neste exemplo, um [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) que contém a chave **TestData** é passado para o aplicativo iniciado. O aplicativo iniciado cria um **ValueSet** com uma chave denominada **ReturnedData** que contém o resultado retornado para o chamador.

Você deve criar e implantar o aplicativo que será iniciado para obter resultados antes de executar o aplicativo de chamada. Caso contrário, [**LaunchUriResult.Status**](https://msdn.microsoft.com/library/windows/apps/dn906892) reportará **LaunchUriStatus.AppUnavailable**.

O nome da família do aplicativo iniciado será necessário ao definir [**TargetApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/dn893511). Para obter o nome da família, faça a seguinte chamada a partir do aplicativo iniciado:

```cs
string familyName = Windows.ApplicationModel.Package.Current.Id.FamilyName;
```

## <a name="remarks"></a>Comentários


O exemplo nestas instruções fornece uma introdução "hello world" para iniciar um aplicativo para obter resultados. Os principais aspectos a serem observados são que a nova API [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686) permite iniciar um aplicativo de maneira assíncrona e se comunicar por meio da classe [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131). A transmissão de dados por meio de **ValueSet** é limitada a 100 KB. Se você precisar passar maiores quantidades de dados, poderá compartilhar arquivos usando a classe [**SharedStorageAccessManager**](https://msdn.microsoft.com/library/windows/apps/dn889985) para criar tokens de arquivos que poderão ser passados entre aplicativos. Por exemplo, dado um **ValueSet** denominado `inputData`, você pode armazenar o token em um arquivo que você quer compartilhar com o aplicativo iniciado:

```cs
inputData["ImageFileToken"] = SharedStorageAccessManager.AddFile(myFile);
```

Em seguida, passá-lo para o aplicativo iniciado por meio de **LaunchUriForResultsAsync**.

## <a name="related-topics"></a>Tópicos relacionados


* [**LaunchUri**](https://msdn.microsoft.com/library/windows/apps/hh701476)
* [**LaunchUriForResultsAsync**](https://msdn.microsoft.com/library/windows/apps/dn956686)
* [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)

 

 
