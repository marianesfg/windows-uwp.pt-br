---
author: PatrickFarley
title: Configurar uma cerca geográfica
description: Configure uma cerca geográfica no aplicativo e saiba como manipular notificações em primeiro e segundo planos.
ms.assetid: A3A46E03-0751-4DBD-A2A1-2323DB09BDBA
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, mapa, localização, cerca geográfica, notificações
ms.localizationpriority: medium
ms.openlocfilehash: 02baf078d127f516d57e947145ec639df5ba891b
ms.sourcegitcommit: 11edca90aaf7856c762e68903483079d30ad3877
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2018
ms.locfileid: "1594774"
---
# <a name="set-up-a-geofence"></a>Configurar uma cerca geográfica




Configure uma [**cerca geográfica**](https://msdn.microsoft.com/library/windows/apps/dn263587) no aplicativo e saiba como manipular notificações em primeiro e segundo planos.

**Dica** Para saber mais sobre como acessar o local no aplicativo, baixe a amostra a seguir no [Repositório Windows-universal-samples](http://go.microsoft.com/fwlink/p/?LinkId=619979) no GitHub.

-   [Amostra de mapa da UWP (Plataforma Universal do Windows)](http://go.microsoft.com/fwlink/p/?LinkId=619977)

## <a name="enable-the-location-capability"></a>Habilitar a funcionalidade de local


1.  Em **Gerenciador de Soluções**, clique duas vezes em **package.appxmanifest** e selecione a guia **Recursos**.
2.  Na lista **Recursos**, marque **Local**. Isso adiciona a funcionalidade `Location` do dispositivo ao arquivo de manifesto do pacote.

```xml
  <Capabilities>
    <!-- DeviceCapability elements must follow Capability elements (if present) -->
    <DeviceCapability Name="location"/>
  </Capabilities>
```

## <a name="set-up-a-geofence"></a>Configurar uma cerca geográfica


### <a name="step-1-request-access-to-the-users-location"></a>Etapa 1: Solicitar acesso ao local do usuário

**Importante** Você deverá solicitar acesso ao local do usuário usando o método [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) antes de tentar acessar o local do usuário. É necessário chamar o método **RequestAccessAsync** no thread da interface do usuário e o aplicativo deve estar em segundo plano. O aplicativo não será capaz de acessar informações de local do usuário até o usuário conceder permissão ao aplicativo.

```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();
```

O método [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) solicita ao usuário permissão para acessar o local. O usuário é solicitado apenas uma vez (por aplicativo). Após a primeira vez em que a permissão é concedida ou negada, esse método deixa de solicitar a permissão ao usuário. Para ajudar o usuário a alterar as permissões de localização depois que elas tiverem sido solicitadas, é recomendável fornecer um link para as configurações de localização, conforme demonstrado mais adiante neste tópico.

### <a name="step-2-register-for-changes-in-geofence-state-and-location-permissions"></a>Etapa 2: Registrar-se para alterações em permissões de localização e estado de cerca geográfica

Neste exemplo, uma instrução **switch** é usada com **accessStatus** (do exemplo anterior) para funcionar somente quando o acesso ao local do usuário for permitido. Caso o acesso ao local do usuário seja permitido, o código acessa as cercas geográficas atuais, registra alterações de estado da cerca geográfica e registra alterações em permissões de localização.

**Dica** Ao usar uma cerca geográfica, o monitor é alterado em permissões de local usando o evento [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/dn263646) na classe GeofenceMonitor em vez do evento StatusChanged da classe Geolocator. Um [**GeofenceMonitorStatus**](https://msdn.microsoft.com/library/windows/apps/dn263599) de **Disabled** é equivalente a um [**PositionStatus**](https://msdn.microsoft.com/library/windows/apps/br225599) desabilitado – os dois indicam que o aplicativo não tem permissão para acessar o local do usuário.

```csharp
switch (accessStatus)
{
    case GeolocationAccessStatus.Allowed:
        geofences = GeofenceMonitor.Current.Geofences;

        FillRegisteredGeofenceListBoxWithExistingGeofences();
        FillEventListBoxWithExistingEvents();

        // Register for state change events.
        GeofenceMonitor.Current.GeofenceStateChanged += OnGeofenceStateChanged;
        GeofenceMonitor.Current.StatusChanged += OnGeofenceStatusChanged;
        break;


    case GeolocationAccessStatus.Denied:
        _rootPage.NotifyUser("Access denied.", NotifyType.ErrorMessage);
        break;

    case GeolocationAccessStatus.Unspecified:
        _rootPage.NotifyUser("Unspecified error.", NotifyType.ErrorMessage);
        break;
}
```

Em seguida, ao sair de seu aplicativo em primeiro plano, cancele o registro dos ouvintes de eventos.

```csharp
protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    GeofenceMonitor.Current.GeofenceStateChanged -= OnGeofenceStateChanged;
    GeofenceMonitor.Current.StatusChanged -= OnGeofenceStatusChanged;

    base.OnNavigatingFrom(e);
}
```

### <a name="step-3-create-the-geofence"></a>Etapa 3: Criar a cerca geográfica

Agora você está pronto para definir e configurar um objeto [**Geofence**](https://msdn.microsoft.com/library/windows/apps/dn263587). Há várias sobrecargas do construtor diferentes para se escolher, dependendo de suas necessidades. No construtor de cerca geográfica mais básico, especifique somente [**Id**](https://msdn.microsoft.com/library/windows/apps/dn263724) e [**Geoshape**](https://msdn.microsoft.com/library/windows/apps/dn263718) conforme mostrado aqui.

```csharp
// Set the fence ID.
string fenceId = "fence1";

// Define the fence location and radius.
BasicGeoposition position;
position.Latitude = 47.6510;
position.Longitude = -122.3473;
position.Altitude = 0.0;
double radius = 10; // in meters

// Set a circular region for the geofence.
Geocircle geocircle = new Geocircle(position, radius);

// Create the geofence.
Geofence geofence = new Geofence(fenceId, geocircle);

```

Você pode ajustar ainda mais sua cerca geográfica usando um dos outros construtores. No próximo exemplo, o construtor de cerca geográfica especifica estes parâmetros adicionais:

-   [**MonitoredStates**](https://msdn.microsoft.com/library/windows/apps/dn263728) - Indica os eventos de cerca geográfica para os quais você deseja receber notificações: entrada na região definida, saída da região definida ou remoção da cerca geográfica.
-   [**SingleUse**](https://msdn.microsoft.com/library/windows/apps/dn263732) - Remove a cerca geográfica quando todos os estados que estão sendo monitorados na cerca geográfica são cumpridos.
-   [**DwellTime**](https://msdn.microsoft.com/library/windows/apps/dn263703) - Indica por quanto tempo o usuário deve permanecer dentro ou fora da área definida para que os eventos de entrada/saída sejam disparados.
-   [**StartTime**](https://msdn.microsoft.com/library/windows/apps/dn263735) - Indica quando começar a monitorar a cerca geográfica.
-   [**Duration**](https://msdn.microsoft.com/library/windows/apps/dn263697) - Indica o período durante o qual monitorar a cerca geográfica.

```csharp
// Set the fence ID.
string fenceId = "fence2";

// Define the fence location and radius.
BasicGeoposition position;
position.Latitude = 47.6510;
position.Longitude = -122.3473;
position.Altitude = 0.0;
double radius = 10; // in meters

// Set the circular region for geofence.
Geocircle geocircle = new Geocircle(position, radius);

// Remove the geofence after the first trigger.
bool singleUse = true;

// Set the monitored states.
MonitoredGeofenceStates monitoredStates =
                MonitoredGeofenceStates.Entered |
                MonitoredGeofenceStates.Exited |
                MonitoredGeofenceStates.Removed;

// Set how long you need to be in geofence for the enter event to fire.
TimeSpan dwellTime = TimeSpan.FromMinutes(5);

// Set how long the geofence should be active.
TimeSpan duration = TimeSpan.FromDays(1);

// Set up the start time of the geofence.
DateTimeOffset startTime = DateTime.Now;

// Create the geofence.
Geofence geofence = new Geofence(fenceId, geocircle, monitoredStates, singleUse, dwellTime, startTime, duration);
```

Depois de criar, lembre-se de registrar sua nova [**Cerca geográfica**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence) no monitor.

```csharp
// Register the geofence
try {
   GeofenceMonitor.Current.Geofences.Add(geofence);
} catch {
   // Handle failure to add geofence
}
```

### <a name="step-4-handle-changes-in-location-permissions"></a>Etapa 4: Manipular alterações em permissões de localização

O objeto [**GeofenceMonitor**](https://msdn.microsoft.com/library/windows/apps/dn263595) dispara o evento [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/dn263646) para indicar que as configurações de localização do usuário mudaram. Esse evento transmite o status correspondente por meio da propriedade **sender.Status** do argumento (do tipo [**GeofenceMonitorStatus**](https://msdn.microsoft.com/library/windows/apps/dn263599)). Observe que esse método não é chamado no thread de interface do usuário, e o objeto [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) invoca as alterações de interface do usuário.

```csharp
using Windows.UI.Core;
...
public async void OnGeofenceStatusChanged(GeofenceMonitor sender, object e)
{
   await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
   {
    // Show the location setting message only if the status is disabled.
    LocationDisabledMessage.Visibility = Visibility.Collapsed;

    switch (sender.Status)
    {
     case GeofenceMonitorStatus.Ready:
      _rootPage.NotifyUser("The monitor is ready and active.", NotifyType.StatusMessage);
      break;

     case GeofenceMonitorStatus.Initializing:
      _rootPage.NotifyUser("The monitor is in the process of initializing.", NotifyType.StatusMessage);
      break;

     case GeofenceMonitorStatus.NoData:
      _rootPage.NotifyUser("There is no data on the status of the monitor.", NotifyType.ErrorMessage);
      break;

     case GeofenceMonitorStatus.Disabled:
      _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);

      // Show the message to the user to go to the location settings.
      LocationDisabledMessage.Visibility = Visibility.Visible;
      break;

     case GeofenceMonitorStatus.NotInitialized:
      _rootPage.NotifyUser("The geofence monitor has not been initialized.", NotifyType.StatusMessage);
      break;

     case GeofenceMonitorStatus.NotAvailable:
      _rootPage.NotifyUser("The geofence monitor is not available.", NotifyType.ErrorMessage);
      break;

     default:
      ScenarioOutput_Status.Text = "Unknown";
      _rootPage.NotifyUser(string.Empty, NotifyType.StatusMessage);
      break;
    }
   });
}
```

## <a name="set-up-foreground-notifications"></a>Configurar notificações em primeiro plano


Após a criação das cercas geográficas, você deve adicionar a lógica para manipular o que acontece quando um evento de cerca geográfica ocorre. Dependendo dos [**MonitoredStates**](https://msdn.microsoft.com/library/windows/apps/dn263728) configurados, você pode receber um evento quando:

-   O usuário entra em uma região de interesse.
-   O usuário sai de uma região de interesse.
-   A cerca geográfica expirou ou foi removida. Observe que um aplicativo em segundo plano não é ativado para um evento de remoção.

Você pode escutar eventos diretamente de seu aplicativo em execução ou registrar uma tarefa em segundo plano, para que você receba uma notificação em segundo plano quando ocorrer um evento.

### <a name="step-1-register-for-geofence-state-change-events"></a>Etapa 1: Registrar eventos de alteração de estado de cerca geográfica

Para seu aplicativo receber uma notificação em primeiro plano sobre a alteração de estado de uma cerca geográfica, registre um manipulador de eventos. Normalmente, isso é configurado quando você cria a cerca geográfica.

```csharp
private void Initialize()
{
    // Other initialization logic

    GeofenceMonitor.Current.GeofenceStateChanged += OnGeofenceStateChanged;
}

```

### <a name="step-2-implement-the-geofence-event-handler"></a>Etapa 2: Implementar o manipulador de eventos de cerca geográfica

A próxima etapa é implementar os manipuladores de eventos. A ação executada aqui depende do motivo pelo qual seu aplicativo usa a cerca geográfica.

```csharp
public async void OnGeofenceStateChanged(GeofenceMonitor sender, object e)
{
    var reports = sender.ReadReports();

    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        foreach (GeofenceStateChangeReport report in reports)
        {
            GeofenceState state = report.NewState;

            Geofence geofence = report.Geofence;

            if (state == GeofenceState.Removed)
            {
                // Remove the geofence from the geofences collection.
                GeofenceMonitor.Current.Geofences.Remove(geofence);
            }
            else if (state == GeofenceState.Entered)
            {
                // Your app takes action based on the entered event.

                // NOTE: You might want to write your app to take a particular
                // action based on whether the app has internet connectivity.

            }
            else if (state == GeofenceState.Exited)
            {
                // Your app takes action based on the exited event.

                // NOTE: You might want to write your app to take a particular
                // action based on whether the app has internet connectivity.

            }
        }
    });
}



```

## <a name="set-up-background-notifications"></a>Configurar notificações em segundo plano


Após a criação das cercas geográficas, você deve adicionar a lógica para manipular o que acontece quando um evento de cerca geográfica ocorre. Dependendo dos [**MonitoredStates**](https://msdn.microsoft.com/library/windows/apps/dn263728) configurados, você pode receber um evento quando:

-   O usuário entra em uma região de interesse.
-   O usuário sai de uma região de interesse.
-   A cerca geográfica expirou ou foi removida. Observe que um aplicativo em segundo plano não é ativado para um evento de remoção.

Para escutar um evento de cerca geográfica em segundo plano

-   Declare a tarefa em segundo plano no manifesto do aplicativo.
-   Registre a tarefa em segundo plano no aplicativo. Se seu aplicativo necessitar de acesso à Internet, digamos para acesso a um serviço em nuvem, você pode definir um sinalizador para isso quando o evento for disparado. Você pode também definir um sinalizador para verificar se o usuário está presente quando o evento for disparado, para assegurar que ele receba a notificação.
-   Enquanto seu aplicativo estiver sendo executado em primeiro plano, peça ao usuário para conceder permissões de localização ao aplicativo.

### <a name="step-1-register-for-geofence-state-change-events"></a>Etapa 1: Registrar eventos de alteração de estado de cerca geográfica

No manifesto do aplicativo, na guia **Declarações**, adicione uma declaração para uma tarefa em segundo plano de localização. Para isso:

-   Adicione uma declaração do tipo **Tarefas em Segundo Plano**.
-   Defina um tipo de tarefa de propriedade de **Localização**.
-   Defina um ponto de entrada em seu aplicativo para chamar quando um evento for disparado.

### <a name="step-2-register-the-background-task"></a>Etapa 2: Registrar a tarefa em segundo plano

O código nesta etapa registra a tarefa em segundo plano de cerca geográfica. Lembre-se de que, quando a cerca geográfica foi criada, nós verificamos as permissões de localização.

```csharp
async private void RegisterBackgroundTask(object sender, RoutedEventArgs e)
{
    // Get permission for a background task from the user. If the user has already answered once,
    // this does nothing and the user must manually update their preference via PC Settings.
    BackgroundAccessStatus backgroundAccessStatus = await BackgroundExecutionManager.RequestAccessAsync();

    // Regardless of the answer, register the background task. Note that the user can use
    // the Settings app to prevent your app from running background tasks.
    // Create a new background task builder.
    BackgroundTaskBuilder geofenceTaskBuilder = new BackgroundTaskBuilder();

    geofenceTaskBuilder.Name = SampleBackgroundTaskName;
    geofenceTaskBuilder.TaskEntryPoint = SampleBackgroundTaskEntryPoint;

    // Create a new location trigger.
    var trigger = new LocationTrigger(LocationTriggerType.Geofence);

    // Associate the location trigger with the background task builder.
    geofenceTaskBuilder.SetTrigger(trigger);

    // If it is important that there is user presence and/or
    // internet connection when OnCompleted is called
    // the following could be called before calling Register().
    // SystemCondition condition = new SystemCondition(SystemConditionType.UserPresent | SystemConditionType.InternetAvailable);
    // geofenceTaskBuilder.AddCondition(condition);

    // Register the background task.
    geofenceTask = geofenceTaskBuilder.Register();

    // Associate an event handler with the new background task.
    geofenceTask.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);

    BackgroundTaskState.RegisterBackgroundTask(BackgroundTaskState.LocationTriggerBackgroundTaskName);

    switch (backgroundAccessStatus)
    {
    case BackgroundAccessStatus.Unspecified:
    case BackgroundAccessStatus.Denied:
        rootPage.NotifyUser("This app is not allowed to run in the background.", NotifyType.ErrorMessage);
        break;

    }
}


```

### <a name="step-3-handling-the-background-notification"></a>Etapa 3: Manipulação da notificação em segundo plano

A ação executada para notificar o usuário depende do que o aplicativo faz, mas é possível exibir uma notificação do sistema, reproduzir um som de áudio ou atualizar um bloco dinâmico. O código nesta etapa manipula a notificação.

```csharp
async private void OnCompleted(IBackgroundTaskRegistration sender, BackgroundTaskCompletedEventArgs e)
{
    if (sender != null)
    {
        // Update the UI with progress reported by the background task.
        await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            try
            {
                // If the background task threw an exception, display the exception in
                // the error text box.
                e.CheckResult();

                // Update the UI with the completion status of the background task.
                // The Run method of the background task sets the LocalSettings.
                var settings = ApplicationData.Current.LocalSettings;

                // Get the status.
                if (settings.Values.ContainsKey("Status"))
                {
                    rootPage.NotifyUser(settings.Values["Status"].ToString(), NotifyType.StatusMessage);
                }

                // Do your app work here.

            }
            catch (Exception ex)
            {
                // The background task had an error.
                rootPage.NotifyUser(ex.ToString(), NotifyType.ErrorMessage);
            }
        });
    }
}


```

## <a name="change-the-privacy-settings"></a>Alterar as configurações de privacidade


Se as configurações de privacidade da localização não permitirem que seu aplicativo acesse a localização do usuário, é recomendável fornecer um link conveniente para as **configurações de privacidade de localização** no aplicativo **Configurações**. Neste exemplo, um controle Hyperlink é usado navegar até o URI `ms-settings:privacy-location`.

```xml
<!--Set Visibility to Visible when access to the user's location is denied. -->  
<TextBlock x:Name="LocationDisabledMessage" FontStyle="Italic"
                 Visibility="Collapsed" Margin="0,15,0,0" TextWrapping="Wrap" >
          <Run Text="This app is not able to access Location. Go to " />
              <Hyperlink NavigateUri="ms-settings:privacy-location">
                  <Run Text="Settings" />
              </Hyperlink>
          <Run Text=" to check the location privacy settings."/>
</TextBlock>
```

Como alternativa, seu aplicativo pode chamar o método [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) para iniciar o aplicativo **Configurações** do código. Para obter mais informações, consulte [Iniciar o aplicativo Configurações do Windows](https://msdn.microsoft.com/library/windows/apps/mt228342).

```csharp
using Windows.System;
...
bool result = await Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-location"));
```

## <a name="test-and-debug-your-app"></a>Testar e depurar o aplicativo


O teste e a depuração de aplicativos com localização geográfica podem ser um desafio, pois esses aplicativos dependem da localização de um dispositivo. Aqui, destacamos vários métodos para testar cercas geográficas de primeiro e segundo plano.

**Para depurar um aplicativo com cerca geográfica**

1.  Mova o dispositivo fisicamente para novas localizações.
2.  Teste a entrada em uma cerca geográfica criando de uma região de cerca geográfica que inclua a sua localização física atual. Dessa forma, você já estará dentro da cerca geográfica, e o evento de entrada em cerca geográfica será disparado imediatamente.
3.  Use o emulador do Microsoft Visual Studio para simular localizações para o dispositivo.

### <a name="test-and-debug-a-geofencing-app-that-is-running-in-the-foreground"></a>Testar e depurar um aplicativo com cerca geográfica em execução em primeiro plano

**Para testar seu aplicativo com cerca geográfica em execução no primeiro plano**

1.  Compile seu aplicativo no Visual Studio.
2.  Inicie o aplicativo no emulador do Visual Studio.
3.  Use estas ferramentas para simular várias localizações dentro e fora da sua região de cerca geográfica. Espere o tempo suficiente após o período especificado pela propriedade [**DwellTime**](https://msdn.microsoft.com/library/windows/apps/dn263703) para disparar o evento. Observe que você deve aceitar o prompt para habilitar permissões de localização para o aplicativo. Para saber mais sobre como simular localizações, consulte [Definir a localização geográfica simulada do dispositivo](http://go.microsoft.com/fwlink/p/?LinkID=325245).
4.  Você também pode usar o emulador para estimar se o tamanho das cercas e duração dos testes aproximadamente necessitam ser detectados em diferentes velocidades.

### <a name="test-and-debug-a-geofencing-app-that-is-running-in-the-background"></a>Testar e depurar um aplicativo com cerca geográfica em execução em segundo plano

**Para testar seu aplicativo com cerca geográfica em execução em segundo plano**

1.  Compile seu aplicativo no Visual Studio. O aplicativo deve definir o tipo de tarefa em segundo plano de **Localização**.
2.  Implante o aplicativo localmente primeiro.
3.  Feche seu aplicativo que está em execução localmente.
4.  Inicie o aplicativo no emulador do Visual Studio. Observe que a simulação de cerca geográfica em segundo plano tem suporte apenas em um aplicativo de cada vez dentro do emulador. Não inicie vários aplicativos com cerca geográfica dentro do emulador.
5.  No emulador, simule várias localizações dentro e fora da sua região de cerca geográfica. Espere o tempo suficiente após o [**DwellTime**](https://msdn.microsoft.com/library/windows/apps/dn263703) para disparar o evento. Observe que você deve aceitar o prompt para habilitar permissões de localização para o aplicativo.
6.  Use o Visual Studio para disparar a tarefa de segundo plano de localização. Para obter mais informações sobre como disparar tarefas em segundo plano no Visual Studio, consulte [Como disparar tarefas em segundo plano](http://go.microsoft.com/fwlink/p/?LinkID=325378).

## <a name="troubleshoot-your-app"></a>Solucionar problemas do aplicativo


Para que o aplicativo possa acessar a localização, é necessário habilitar **Localização** no dispositivo. No aplicativo **Configurações**, verifique se as seguintes **configurações de privacidade de localização** estão ativadas:

-   **Localização deste dispositivo...** está **ativada** (não se aplica ao Windows 10 Mobile)
-   A configuração de serviços de localização, **Localização**, está **ativada**
-   Em **Escolher aplicativos que podem usar sua localização**, seu aplicativo está definido como **ativado**

## <a name="related-topics"></a>Tópicos relacionados

* [Amostra de geolocalização da UWP](http://go.microsoft.com/fwlink/p/?linkid=533278)
* [Diretrizes de design para cerca geográfica](https://msdn.microsoft.com/library/windows/apps/dn631756)
* [Diretrizes de design para aplicativos com detecção de localização](https://msdn.microsoft.com/library/windows/apps/hh465148)
