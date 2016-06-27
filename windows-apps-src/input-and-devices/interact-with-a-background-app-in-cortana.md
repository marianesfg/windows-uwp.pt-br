---
author: Karl-Bridge-Microsoft
Description: "Saiba como um usuário pode interagir com um aplicativo em segundo plano usando a voz e a tela da Cortana durante a execução de um comando de voz."
title: Interagir com um aplicativo em segundo plano
ms.assetid: 6C60F03C-A242-435D-96BB-736892CC1CA6
label: Interact with a background app
template: detail.hbs
ms.sourcegitcommit: 7d9f5eff0f6561b18024658fe99d1e11bbe3309f
ms.openlocfilehash: 675553f5c3954597982360900e965b2a756d7f63

---

# Interagir com um aplicativo em segundo plano na Cortana

Habilite a interação do usuário com um aplicativo em segundo plano, por meio de entrada de controle por voz e de texto na tela da **Cortana** durante e execução de um comando de voz.



**APIs importantes**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**Elementos e atributos de Voice Command Definition (VCD) v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)


A Cortana dá suporte a um fluxo de trabalho completo de curva a curva com seu aplicativo. Esse fluxo de trabalho é definido por seu aplicativo e pode dar suporte a funcionalidades, como: 

-   Conclusão bem-sucedida
-   Entrega
-   Progresso
-   Confirmação
-   Desambiguação
-   Erro

**Pré-requisitos:**

Este tópico complementa [Iniciar um aplicativo em segundo plano com comandos de voz na Cortana](launch-a-background-app-with-voice-commands-in-cortana.md). Continuaremos demonstrando recursos com um aplicativo de planejamento e gerenciamento de viagens chamado **Adventure Works**.

Se você for iniciante no desenvolvimento de aplicativos da Plataforma Universal do Windows (UWP), consulte estes tópicos para familiarizar-se com as tecnologias discutidas aqui.

-   [Criar seu primeiro aplicativo](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Saiba mais sobre eventos com [Visão geral de eventos e eventos roteados](https://msdn.microsoft.com/library/windows/apps/mt185584)

**Diretrizes de experiência do usuário:  **

Consulte as [Diretrizes de design da Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233), para obter informações sobre como integrar seu aplicativo à **Cortana** e as [Diretrizes de design de controle por voz](https://msdn.microsoft.com/library/windows/apps/dn596121) para obter dicas úteis para criar um aplicativo interessante e prático, habilitado para controle por voz.

## <span id="Feedback_strings"></span><span id="feedback_strings"></span><span id="FEEDBACK_STRINGS"></span>Cadeias de caracteres de comentários

Componha as cadeias de caracteres de comentários que são exibidas e faladas pela **Cortana**.

O artigo [Diretrizes de design da Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233) fornece recomendações sobre como compor cadeias de caracteres para a **Cortana**.

## <span id="Feedback_strings"></span><span id="feedback_strings"></span><span id="FEEDBACK_STRINGS"></span>Cadeias de caracteres de comentários

Os cartões de conteúdo podem fornecer contexto adicional para o usuário e ajudar você a manter as cadeias de caracteres de comentários concisas.

A **Cortana** dá suporte aos seguintes modelos de cartões de conteúdo (é possível usar somente um modelo na tela de conclusão):

    -   Title only
    -   Title with up to three lines of text
    -   Title with image
    -   Title with image and up to three lines of text

A imagem pode ser:

    -   68w x 68h
    -   68w x 92h
    -   280w x 140h

Você também pode permitir que os usuários iniciem seu aplicativo em primeiro plano clicando em um cartão ou link de texto para seu aplicativo.

## <span id="Completion_screen"></span><span id="completion_screen"></span><span id="COMPLETION_SCREEN"></span>Tela de conclusão

Uma tela de conclusão fornece ao usuário informações sobre a tarefa de comando de voz concluída.

As tarefas que levam menos de 500 milissegundos para seu aplicativo responder e não requerem informações adicionais do usuário são concluídas sem interação adicional com a **Cortana**. A Cortana simplesmente exibe a tela de conclusão.

Aqui, nós usamos o aplicativo **Adventure Works** para mostrar a tela de conclusão de uma solicitação de comando de voz para exibir as próximas viagens para Londres. 

![tela de conclusão do aplicativo em segundo plano da cortana](images/cortana-completion-screen-upcomingtrip-small.png)

O comando de voz é definido em AdventureWorksCommands.xml:
```
<Command Name="whenIsTripToDestination">
  <Example> When is my trip to Las Vegas?</Example>
  <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
  <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
  <Feedback> Looking for trip to {destination}</Feedback>
  <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
</Command>
```

O AdventureWorksVoiceCommandService.cs contém o método de mensagem de conclusão:

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

## <span id="Hand-off_screen"></span><span id="hand-off_screen"></span><span id="HAND-OFF_SCREEN"></span>Tela de transição

Depois que um comando de voz é reconhecido, a **Cortana** deve chamar ReportSuccessAsync e apresentar comentários em aproximadamente 500 ms. Se o serviço de aplicativo não puder concluir a ação especificada pelo comando de voz em 500 ms, a **Cortana** apresentará uma tela de entrega até que seu aplicativo chame ReportSuccessAsync ou por até cinco segundos.

Se o serviço de aplicativo não chamar ReportSuccessAsync, ou qualquer outro método VoiceCommandServiceConnection, o usuário receberá uma mensagem de erro, e o serviço de aplicativo será cancelado.

Este é um exemplo de uma tela de entrega do aplicativo **Adventure Works**. Neste exemplo, um usuário consultou a **Cortana** em relação a viagens futuras. A tela de entrega inclui uma mensagem personalizada com o nome do serviço de aplicativo, um ícone e uma cadeia de caracteres de **Comentários**. 

[!NOTE] Você pode declarar uma cadeia de caracteres de **Comentários** no arquivo VCD. Essa cadeia de caracteres não afeta o texto da interface do usuário exibido na tela da Cortana, afeta somente o texto falado pela **Cortana**.

![tela de entrega do aplicativo em segundo plano da cortana](images/cortana-backgroundapp-progress-result.png)


## <span id="Progress_screen"></span><span id="progress_screen"></span><span id="PROGRESS_SCREEN"></span>Tela de progresso


Se o serviço de aplicativo demorar mais de 500 ms para chamar ReportSuccessAsync, a **Cortana** fornecerá ao usuário uma tela de progresso. O ícone do aplicativo é exibido e você deve fornecer as cadeias de caracteres de progresso da GUI e da TTS para indicar que a tarefa está sendo manipulada.

A **Cortana** mostra uma tela de progresso por no máximo 5 segundos. Depois de 5 segundos, a **Cortana** apresenta ao usuário uma mensagem de erro e o serviço de aplicativo é fechado. Se o serviço de aplicativo precisar de mais que cinco segundos para concluir a ação, ele poderá continuar atualizando a **Cortana** com telas de progresso.

Este é um exemplo de uma tela de progresso do aplicativo **Adventure Works**. Neste exemplo, um usuário cancelou uma viagem para Las Vegas. A tela de progresso inclui uma mensagem personalizada para a ação, um ícone e um bloco de conteúdo com informações sobre a viagem que está sendo cancelada.

![tela de progresso do aplicativo em segundo plano da cortana ](images/cortana-progress-screen.png)

O AdventureWorksVoiceCommandService.cs contém o seguinte método de mensagem de progresso que chama [**ReportProgressAsync**](https://msdn.microsoft.com/library/windows/apps/dn706579) para mostrar a tela de progresso na **Cortana**.


```    CSharp
/// <summary>
/// Show a progress screen. These should be posted at least every 5 seconds for a 
/// long-running operation.
/// </summary>
/// <param name="message">The message to display, relating to the task being performed.</param>
/// <returns></returns>
private async Task ShowProgressScreen(string message)
{
    var userProgressMessage = new VoiceCommandUserMessage();
    userProgressMessage.DisplayMessage = userProgressMessage.SpokenMessage = message;

    VoiceCommandResponse response = VoiceCommandResponse.CreateResponse(userProgressMessage);
    await voiceServiceConnection.ReportProgressAsync(response);
}
```

## <span id="Confirmation_screen"></span><span id="confirmation_screen"></span><span id="CONFIRMATION_SCREEN"></span>Tela de confirmação


Quando uma ação especificada por um comando de voz é irreversível, tem um impacto significativo ou a confiança no reconhecimento não for alta, o serviço de um aplicativo pode solicitar uma confirmação.

Este é um exemplo de uma tela de confirmação do aplicativo **Adventure Works**. Neste exemplo, um usuário instruiu o serviço de aplicativo para cancelar uma viagem para Las Vegas usando a **Cortana**. O serviço de aplicativo forneceu à **Cortana** uma tela de confirmação que solicita ao usuário uma resposta sim ou não antes de cancelar a viagem.

Se o usuário disser algo diferente de "Sim" ou "Não", a **Cortana** não poderá determinar a resposta à pergunta. Nesse caso, a **Cortana** faz uma pergunta semelhante ao usuário, fornecida pelo serviço de aplicativo.

Na segunda solicitação, se o usuário ainda não responder "Sim" ou "Não", a **Cortana** fará a mesma pergunta predeterminada ao usuário pela terceira vez, com um pedido de desculpas. Se o usuário ainda não responder "Sim" ou "Não", a **Cortana** não ouvirá mais a entrada de voz e solicitará que o usuário toque em um dos botões.

A tela de confirmação inclui uma mensagem personalizada para a ação, um ícone e um bloco de conteúdo com informações sobre a viagem que está sendo cancelada.

![tela de confirmação do aplicativo em segundo plano da cortana](images/cortana-confirmation-screen.png)

O AdventureWorksVoiceCommandService.cs contém o seguinte método de cancelamento de viagem que chama [**RequestConfirmationAsync**](https://msdn.microsoft.com/library/windows/apps/dn706582) para exibir uma tela de confirmação na **Cortana**.

```    CSharp
/// <summary>
/// Handle the Trip Cancellation task. This task demonstrates how to prompt a user
/// for confirmation of an operation, show users a progress screen while performing
/// a long-running task, and show a completion screen.
/// </summary>
/// <param name="destination">The name of a destination.</param>
/// <returns></returns>
private async Task SendCompletionMessageForCancellation(string destination)
{
    // Begin loading data to search for the target store. 
    // Consider inserting a progress screen here, in order to prevent Cortana from timing out. 
    string progressScreenString = string.Format(
        cortanaResourceMap.GetValue("ProgressLookingForTripToDest", cortanaContext).ValueAsString,
        destination);
    await ShowProgressScreen(progressScreenString);

    Model.TripStore store = new Model.TripStore();
    await store.LoadTrips();

    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);
    Model.Trip trip = null;
    if (trips.Count() > 1)
    {
        // If there is more than one trip, provide a disambiguation screen.
        // However, if a significant number of items are returned, you might want to 
        // just display a link to your app and provide a deeper search experience.
        string disambiguationDestinationString = string.Format(
            cortanaResourceMap.GetValue("DisambiguationWhichTripToDest", cortanaContext).ValueAsString,
            destination);
        string disambiguationRepeatString = cortanaResourceMap.GetValue("DisambiguationRepeat", cortanaContext).ValueAsString;
        trip = await DisambiguateTrips(trips, disambiguationDestinationString, disambiguationRepeatString);
    }
    else
    {
        trip = trips.FirstOrDefault();
    }

    var userPrompt = new VoiceCommandUserMessage();
    
    VoiceCommandResponse response;
    if (trip == null)
    {
        var userMessage = new VoiceCommandUserMessage();
        string noSuchTripToDestination = string.Format(
            cortanaResourceMap.GetValue("NoSuchTripToDestination", cortanaContext).ValueAsString,
            destination);
        userMessage.DisplayMessage = userMessage.SpokenMessage = noSuchTripToDestination;

        response = VoiceCommandResponse.CreateResponse(userMessage);
        await voiceServiceConnection.ReportSuccessAsync(response);
    }
    else
    {
        // Prompt the user for confirmation that this is the correct trip to cancel.
        string cancelTripToDestination = string.Format(
            cortanaResourceMap.GetValue("CancelTripToDestination", cortanaContext).ValueAsString,
            destination);
        userPrompt.DisplayMessage = userPrompt.SpokenMessage = cancelTripToDestination;
        var userReprompt = new VoiceCommandUserMessage();
        string confirmCancelTripToDestination = string.Format(
            cortanaResourceMap.GetValue("ConfirmCancelTripToDestination", cortanaContext).ValueAsString,
            destination);
        userReprompt.DisplayMessage = userReprompt.SpokenMessage = confirmCancelTripToDestination;
        
        response = VoiceCommandResponse.CreateResponseForPrompt(userPrompt, userReprompt);

        var voiceCommandConfirmation = await voiceServiceConnection.RequestConfirmationAsync(response);

        // If RequestConfirmationAsync returns null, Cortana has likely been dismissed.
        if (voiceCommandConfirmation != null)
        {
            if (voiceCommandConfirmation.Confirmed == true)
            {
                string cancellingTripToDestination = string.Format(
               cortanaResourceMap.GetValue("CancellingTripToDestination", cortanaContext).ValueAsString,
               destination);
                await ShowProgressScreen(cancellingTripToDestination);

                // Perform the operation to remove the trip from app data. 
                // As the background task runs within the app package of the installed app,
                // we can access local files belonging to the app without issue.
                await store.DeleteTrip(trip);

                // Provide a completion message to the user.
                var userMessage = new VoiceCommandUserMessage();
                string cancelledTripToDestination = string.Format(
                    cortanaResourceMap.GetValue("CancelledTripToDestination", cortanaContext).ValueAsString,
                    destination);
                userMessage.DisplayMessage = userMessage.SpokenMessage = cancelledTripToDestination;
                response = VoiceCommandResponse.CreateResponse(userMessage);
                await voiceServiceConnection.ReportSuccessAsync(response);
            }
            else
            {
                // Confirm no action for the user.
                var userMessage = new VoiceCommandUserMessage();
                string keepingTripToDestination = string.Format(
                    cortanaResourceMap.GetValue("KeepingTripToDestination", cortanaContext).ValueAsString,
                    destination);
                userMessage.DisplayMessage = userMessage.SpokenMessage = keepingTripToDestination;

                response = VoiceCommandResponse.CreateResponse(userMessage);
                await voiceServiceConnection.ReportSuccessAsync(response);
            }
        }
    }
}
```

## <span id="Disambiguation_screen"></span><span id="disambiguation_screen"></span><span id="DISAMBIGUATION_SCREEN"></span>Tela de diferenciação


Quando uma ação especificada por um comando de voz tem mais de um resultado possível, um serviço de aplicativo pode solicitar mais informações do usuário.

Este é um exemplo de uma tela de desambiguidade do aplicativo **Adventure Works**. Neste exemplo, um usuário instruiu o serviço de aplicativo para cancelar uma viagem para Las Vegas usando a **Cortana**. No entanto, o usuário tem duas viagens para Las Vegas em datas diferentes, e o serviço de aplicativo não pode concluir a ação sem que o usuário selecione a viagem pretendida.

O serviço de aplicativo fornece à **Cortana** uma tela de desambiguação que solicita que o usuário faça a seleção em uma lista de viagens correspondentes para fazer o cancelamento.

Nesse caso, a **Cortana** faz uma pergunta semelhante ao usuário, fornecida pelo serviço de aplicativo.

Na segunda solicitação, se o usuário ainda não disser algo que possa ser usado para identificar a seleção, a **Cortana** fará a mesma pergunta predeterminada ao usuário pela terceira vez, com um pedido de desculpas. Se o usuário ainda não disser algo que possa ser usado para identificar a seleção, a **Cortana** não ouvirá mais a entrada de voz e solicitará que o usuário toque em um dos botões.

A tela de desambiguação inclui uma mensagem personalizada para a ação, um ícone e um bloco de conteúdo com informações sobre a viagem que está sendo cancelada.

![tela de desambiguação do aplicativo em segundo plano da cortana ](images/cortana-disambiguation-screen.png)

O AdventureWorksVoiceCommandService.cs contém o seguinte método de cancelamento de viagem que chama [**RequestDisambiguationAsync**](https://msdn.microsoft.com/library/windows/apps/dn706583) para mostrar a tela de desambiguação na **Cortana**.

```csharp
/// <summary>
/// Provide the user with a way to identify which trip to cancel. 
/// </summary>
/// <param name="trips">The set of trips</param>
/// <param name="disambiguationMessage">The initial disambiguation message</param>
/// <param name="secondDisambiguationMessage">Repeat prompt retry message</param>
private async Task<Model.Trip> DisambiguateTrips(IEnumerable<Model.Trip> trips, string disambiguationMessage, string secondDisambiguationMessage)
{
    // Create the first prompt message.
    var userPrompt = new VoiceCommandUserMessage();
    userPrompt.DisplayMessage =
        userPrompt.SpokenMessage = disambiguationMessage;

    // Create a re-prompt message if the user responds with an out-of-grammar response.
    var userReprompt = new VoiceCommandUserMessage();
    userReprompt.DisplayMessage =
        userReprompt.SpokenMessage = secondDisambiguationMessage;

    // Create card for each item. 
    var destinationContentTiles = new List<VoiceCommandContentTile>();
    int i = 1;
    foreach (Model.Trip trip in trips)
    {
        var destinationTile = new VoiceCommandContentTile();

        destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
        destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));
        
        // The AppContext can be any arbitrary object.
        destinationTile.AppContext = trip;
        string dateFormat = "";
        if (trip.StartDate != null)
        {
            dateFormat = trip.StartDate.Value.ToString(dateFormatInfo.LongDatePattern);
        }
        else
        {
            // The app allows a trip to have no date.
            // However, the choices must be unique so they can be distinguished.
            // Here, we add a number to identify them.
            dateFormat = string.Format("{0}", i);
        } 

        destinationTile.Title = trip.Destination + " " + dateFormat;
        destinationTile.TextLine1 = trip.Description;

        destinationContentTiles.Add(destinationTile);
        i++;
    }

    // Cortana handles re-prompting if no valid response.
    var response = VoiceCommandResponse.CreateResponseForPrompt(userPrompt, userReprompt, destinationContentTiles);

    // If cortana is dismissed in this operation, null is returned.
    var voiceCommandDisambiguationResult = await
        voiceServiceConnection.RequestDisambiguationAsync(response);
    if (voiceCommandDisambiguationResult != null)
    {
        return (Model.Trip)voiceCommandDisambiguationResult.SelectedItem.AppContext;
    }

    return null;
}
```

## <span id="Error_screen"></span><span id="error_screen"></span><span id="ERROR_SCREEN"></span>Tela de erro


Quando não é possível concluir uma ação especificada por um comando de voz, o serviço de aplicativo pode fornecer uma tela de erro.

Este é um exemplo de uma tela de erro do aplicativo **Adventure Works**. Neste exemplo, um usuário instruiu o serviço de aplicativo para cancelar uma viagem para Las Vegas usando a **Cortana**. No entanto, o usuário não tem viagens agendadas para Las Vegas.

O serviço de aplicativo fornece à **Cortana** uma tela de erro que inclui uma mensagem personalizada para a ação, um ícone e a mensagem de erro específica.

Chame [**ReportFailureAsync**](https://msdn.microsoft.com/library/windows/apps/dn706578) para mostrar a tela de erro na **Cortana**.

```csharp
var userMessage = new VoiceCommandUserMessage();
    userMessage.DisplayMessage = userMessage.SpokenMessage = 
      "Sorry, you don't have any trips to Las Vegas";
                
    var response = VoiceCommandResponse.CreateResponse(userMessage);

    response.AppLaunchArgument = "showUpcomingTrips";
    await voiceServiceConnection.ReportFailureAsync(response);
```

## <span id="related_topics"></span>Artigos relacionados


**Desenvolvedores**
* [Interações da Cortana](cortana-interactions.md)
* [**Elementos e atributos VCD v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Designers**
* [Diretrizes de design da Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Diretrizes para design de controle por voz](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Exemplos**
* [Amostra de comando de voz da Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 







<!--HONumber=Jun16_HO3-->


