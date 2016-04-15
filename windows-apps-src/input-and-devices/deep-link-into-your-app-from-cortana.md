---
Description: Forneça links profundos do serviço de aplicativo em segundo plano na Cortana para iniciar o aplicativo em primeiro plano em um estado ou contexto específico.
title: Link profundo da Cortana para um aplicativo em segundo plano
ms.assetid: BE811A87-8821-476A-90E4-2E20D37E4043
label: Deep link to a background app
template: detail.hbs
---

# Link profundo da Cortana para um aplicativo em segundo plano


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos do Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**APIs Importantes**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**Elementos e atributos do Voice Command Definition (VCD) v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

Forneça links profundos de um aplicativo em segundo plano na **Cortana** que iniciem o aplicativo em primeiro plano em um estado ou contexto específico.

> **Observação**  
A **Cortana** e o serviço de aplicativo em segundo plano são encerrados quando o aplicativo em primeiro plano é iniciado.

Por padrão, um link profundo é exibido na tela de conclusão da **Cortana** como mostrado aqui ("Ir para AdventureWorks"), mas você pode exibir links profundos em diversas outras telas. 

![tela de conclusão do aplicativo em segundo plano da cortana](images/cortana-completion-screen-upcomingtrip-small.png)

**Pré-requisitos: **

Este tópico complementa [Interagir com um aplicativo em segundo plano na Cortana](interact-with-a-background-app-in-cortana.md). Continuamos usando um aplicativo de planejamento e gerenciamento de viagens chamado **Adventure Works** para demonstrar diversos recursos da **Cortana**.

Se você for iniciante no desenvolvimento de aplicativos UWP (Plataforma Universal do Windows), consulte estes tópicos para familiarizar-se com as tecnologias discutidas aqui.

-   [Criar seu primeiro aplicativo](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Saiba mais sobre eventos com [Visão geral de eventos e eventos roteados](https://msdn.microsoft.com/library/windows/apps/mt185584)

**Diretrizes de experiência do usuário: **

Consulte as [Diretrizes de design da Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233) para saber como integrar seu aplicativo à **Cortana** e as [Diretrizes para design de controle por voz](https://msdn.microsoft.com/library/windows/apps/dn596121) para obter dicas úteis para criar um aplicativo interessante e prático, habilitado para controle por voz.

## <span id="Overview"></span><span id="overview"></span><span id="OVERVIEW"></span>Visão geral


Os usuários podem acessar seu aplicativo por meio da **Cortana**:

-   Ativando-o como um aplicativo em primeiro plano (consulte [Ativar um aplicativo em primeiro plano com comandos de voz através da Cortana](launch-a-foreground-app-with-voice-commands-in-cortana.md)).
-   Expondo uma funcionalidade específica como um serviço de aplicativo em segundo plano (consulte [Ativar um aplicativo em segundo plano com comandos de voz através da Cortana](launch-a-background-app-with-voice-commands-in-cortana.md)).
-   Criando links profundos para páginas, conteúdo e estado ou contexto específicos.

Discutimos aqui vinculação profunda.

Os links profundos são úteis quando a Cortana e o serviço de aplicativo age como um gateway para o seu aplicativo completo (em vez de exigir que o usuário inicie o aplicativo por meio do menu Iniciar), ou para dar acesso a detalhes e funcionalidades mais avançados dentro do seu aplicativo, algo que não é possível por meio da Cortana. Os links profundos são outra maneira de aumentar a usabilidade e promover o seu aplicativo.

Existem três maneiras de fornecer links profundos:

-   Um link "Ir para &lt;aplicativo&gt;" em várias telas da **Cortana**.
-   Um link inserido em um bloco de conteúdo em diversas telas da **Cortana**.
-   Iniciando programaticamente o aplicativo em primeiro plano a partir do serviço de aplicativo em segundo plano.

## <span id="Go_to__app__deep_link"></span><span id="go_to__app__deep_link"></span><span id="GO_TO__APP__DEEP_LINK"></span>Link profundo "Ir para &lt;aplicativo&gt;"


**Cortana** exibe um link profundo "Ir para & lt; aplicativo & gt;" abaixo do cartão de conteúdo na maioria das telas.

![tela de conclusão do aplicativo em segundo plano da cortana](images/cortana-completion-screen.png)

Você pode fornecer um argumento de inicialização para esse link que abre seu aplicativo no contexto semelhante ao do serviço de aplicativo. Caso você não forneça um argumento de inicialização, o aplicativo será iniciado na tela principal.

Neste exemplo de AdventureWorksVoiceCommandService.cs do exemplo **AdventureWorks**, passamos o destino especificado para o método SendCompletionMessageForDestination, que recupera todas as viagens correspondentes e fornece um link profundo para o aplicativo.

Primeiro, criamos um [**VoiceCommandUserMessage**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandusermessage.aspx) (```userMessage```) que é falado pela **Cortana** e mostrado na tela da **Cortana**. Em seguida, é criado um objeto de lista [**VoiceCommandContentTile**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandcontenttile.aspx) para exibir a coleção de cartões de resultado na tela. 

Esses dois objetos são passados para o método [CreateResponse](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandresponse.createresponse.aspx) do objeto [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182) (```response```). Em seguida, definimos o valor da propriedade [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183) para o valor de destino no comando de voz.

Finalmente, chamamos o método [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580) do [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204).

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination specified in the voice command.</param>
private async Task SendCompletionMessageForDestination(string destination)
{
...
    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);

    var userMessage = new VoiceCommandUserMessage();
    var destinationsContentTiles = new List<VoiceCommandContentTile>();
...
    var response = VoiceCommandResponse.CreateResponse(userMessage, destinationsContentTiles);

    if (trips.Count() > 0)
    {
        response.AppLaunchArgument = destination;
    }

    await voiceServiceConnection.ReportSuccessAsync(response);
}
```


## <span id="Content_tile_deep_link"></span><span id="content_tile_deep_link"></span><span id="CONTENT_TILE_DEEP_LINK"></span>Link profundo do bloco de conteúdo


Você pode adicionar links profundos a cartões de conteúdo em diversas telas da **Cortana**.

![tela de transição do aplicativo em segundo plano da cortana ](images/cortana-backgroundapp-progress-result.png)

Assim como acontece com os links "Ir para &lt;aplicativo&gt;", você pode fornecer um argumento de inicialização para abrir o aplicativo com contexto semelhante ao do serviço de aplicativo. Caso você não forneça um argumento de inicialização, o bloco de conteúdo não se vinculará ao seu aplicativo.

Neste exemplo de AdventureWorksVoiceCommandService.cs do exemplo **AdventureWorks**, passamos o destino especificado para o método SendCompletionMessageForDestination, que recupera todas as viagens correspondentes e fornece cartões de conteúdo com links profundos para o aplicativo.

Primeiro, criamos um [**VoiceCommandUserMessage**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandusermessage.aspx) (```userMessage```) que é falado pela **Cortana** e mostrado na tela da **Cortana**. Em seguida, é criado um objeto de lista [**VoiceCommandContentTile**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandcontenttile.aspx) para exibir a coleção de cartões de resultado na tela. 

Esses dois objetos são passados para o método [CreateResponse](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandresponse.createresponse.aspx) do objeto [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182) (```response```). Em seguida, definimos o valor da propriedade [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183) para o valor de destino no comando de voz.

Finalmente, chamamos o método [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580) do [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204).
Aqui, adicionamos dois blocos de conteúdo com valores de parâmetro [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183) diferentes a uma lista [**VoiceCommandContentTile**](https://msdn.microsoft.com/library/windows/apps/dn974168) usada na chamada [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580) do objeto [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204).

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination specified in the voice command.</param>
private async Task SendCompletionMessageForDestination(string destination)
{
    // If this operation is expected to take longer than 0.5 seconds, the task must
    // supply a progress response to Cortana before starting the operation, and
    // updates must be provided at least every 5 seconds.
    string loadingTripToDestination = string.Format(
               cortanaResourceMap.GetValue("LoadingTripToDestination", cortanaContext).ValueAsString,
               destination);
    await ShowProgressScreen(loadingTripToDestination);
    Model.TripStore store = new Model.TripStore();
    await store.LoadTrips();

    // Query for the specified trip. 
    // The destination should be in the phrase list. However, there might be  
    // multiple trips to the destination. We pick the first.
    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);

    var userMessage = new VoiceCommandUserMessage();
    var destinationsContentTiles = new List<VoiceCommandContentTile>();
    if (trips.Count() == 0)
    {
        string foundNoTripToDestination = string.Format(
               cortanaResourceMap.GetValue("FoundNoTripToDestination", cortanaContext).ValueAsString,
               destination);
        userMessage.DisplayMessage = foundNoTripToDestination;
        userMessage.SpokenMessage = foundNoTripToDestination;
    }
    else
    {
        // Set plural or singular title.
        string message = "";
        if (trips.Count() > 1)
        {
            message = cortanaResourceMap.GetValue("PluralUpcomingTrips", cortanaContext).ValueAsString;
        }
        else
        {
            message = cortanaResourceMap.GetValue("SingularUpcomingTrip", cortanaContext).ValueAsString;
        }
        userMessage.DisplayMessage = message;
        userMessage.SpokenMessage = message;

        // Define a tile for each destination.
        foreach (Model.Trip trip in trips)
        {
            int i = 1;
            
            var destinationTile = new VoiceCommandContentTile();

            destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
            destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));

            destinationTile.AppLaunchArgument = trip.Destination;
            destinationTile.Title = trip.Destination;
            if (trip.StartDate != null)
            {
                destinationTile.TextLine1 = trip.StartDate.Value.ToString(dateFormatInfo.LongDatePattern);
            }
            else
            {
                destinationTile.TextLine1 = trip.Destination + " " + i;
            }

            destinationsContentTiles.Add(destinationTile);
            i++;
        }
    }

    var response = VoiceCommandResponse.CreateResponse(userMessage, destinationsContentTiles);

    if (trips.Count() > 0)
    {
        response.AppLaunchArgument = destination;
    }

    await voiceServiceConnection.ReportSuccessAsync(response);
}
```
## <span id="Programmatic_deep_link"></span><span id="programmatic_deep_link"></span><span id="PROGRAMMATIC_DEEP_LINK"></span>Link profundo programático


Você também pode iniciar programaticamente seu aplicativo com um argumento de inicialização para abrir seu aplicativo com contexto semelhante ao do serviço de aplicativo. Caso você não forneça um argumento de inicialização, o aplicativo é iniciado na tela principal.

Aqui, adicionamos um parâmetro [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183) com um valor de "Las Vegas" para um objeto [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182) usado na chamada [**RequestAppLaunchAsync**](https://msdn.microsoft.com/library/windows/apps/dn706581) do objeto [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204).

```CSharp
var userMessage = new VoiceCommandUserMessage();
userMessage.DisplayMessage = "Here are your trips.";
userMessage.SpokenMessage = 
  "You have one trip to Vegas coming up.";

response = VoiceCommandResponse.CreateResponse(userMessage);
response.AppLaunchArgument = “Las Vegas”;
await  VoiceCommandServiceConnection.RequestAppLaunchAsync(response);
```

## <span id="App_manifest"></span><span id="app_manifest"></span><span id="APP_MANIFEST"></span>Manifesto do aplicativo


Para habilitar os links profundos para o seu aplicativo, você deve declarar a extensão `windows.personalAssistantLaunch` no arquivo Package appxmanifest de seu projeto de aplicativo.

Aqui, declaramos a extensão `windows.personalAssistantLaunch` do aplicativo **Adventure Works**.

```XML
<Extensions>
  <uap:Extension Category="windows.appService" 
    EntryPoint="AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService">
    <uap:AppService Name="AdventureWorksVoiceCommandService"/>
  </uap:Extension>
  <uap:Extension Category="windows.personalAssistantLaunch"/> 
</Extensions>
```

## <span id="Protocol_contract"></span><span id="protocol_contract"></span><span id="PROTOCOL_CONTRACT"></span>Contrato de protocolo


Seu aplicativo é iniciado em primeiro plano por meio da ativação do URI (Uniform Resource Identifier) usando um contrato [**Protocol**](https://msdn.microsoft.com/library/windows/apps/br224693). Seu aplicativo deve substituir o evento [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) do aplicativo e verificar se há um **ActivationKind** de **Protocol**. Para saber mais, consulte [Manipular a ativação do URI](https://msdn.microsoft.com/library/windows/apps/mt228339).

Aqui, decodificamos o URI fornecido pelo [**ProtocolActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224742) para acessar o argumento de inicialização. Para este exemplo, o [**Uri**](https://msdn.microsoft.com/library/windows/apps/br224746) é definido como "windows.personalassistantlaunch:?LaunchContext=Las Vegas".

```CSharp
if (args.Kind == ActivationKind.Protocol)
  {
    var commandArgs = args as ProtocolActivatedEventArgs;
    Windows.Foundation.WwwFormUrlDecoder decoder = 
      new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
    var destination = decoder.GetFirstValueByName("LaunchContext");

    navigationCommand = new ViewModel.TripVoiceCommand(
      "protocolLaunch",
      "text",
      "destination",
      destination);

    navigationToPageType = typeof(View.TripDetails);

    rootFrame.Navigate(navigationToPageType, navigationCommand);

    // Ensure the current window is active.
    Window.Current.Activate();
  }
```

## <span id="related_topics"></span>Artigos relacionados


**Desenvolvedores**
* [Interações da Cortana](cortana-interactions.md)
* [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Designers**
* [Diretrizes para design da Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Diretrizes para design de controle por voz](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Exemplos**
* [Amostra de comando de voz da Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=Mar16_HO4-->


