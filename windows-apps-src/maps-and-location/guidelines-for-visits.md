---
author: PatrickFarley
Description: Learn how to use the powerful Visits Tracking feature for more practical location tracking.
title: Diretrizes para usar o rastreamento de visitas
ms.assetid: 0c101684-48a9-4592-9ed5-6c20f3b830f2
ms.author: pafarley
ms.date: 05/18/2017
ms.topic: article
keywords: windows 10, uwp, mapa, localização, geovisit, geovisits
ms.localizationpriority: medium
ms.openlocfilehash: 3a78b2689a10dff50696e5e65cc44f79a6f1ea7d
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6188423"
---
# <a name="guidelines-for-using-visits-tracking"></a>Diretrizes para usar o rastreamento de visitas

O recurso de visitas simplifica o processo de rastreamento de localização para torná-lo mais eficiente para os fins de praticidade de muitos aplicativos. Uma visita é definida como uma área geográfica significativa em que o usuário entra e sai. Visitas são semelhantes às [cercas geográficas](guidelines-for-geofencing.md) em que eles permitem que o aplicativo seja notificado somente quando o usuário insere ou sai de determinadas áreas de interesse, eliminando a necessidade de rastreamento de localização contínua, que pode diminuir a duração da bateria. No entanto, ao contrário de cercas geográficas, áreas de visita são dinamicamente identificadas no nível da plataforma e não precisam ser definidas explicitamente por aplicativos individuais. Além disso, a seleção de quais visitas um aplicativo irá controlar é manipulada por uma configuração de granularidade única, em vez de atribuir a locais individuais.

## <a name="preliminary-setup"></a>Configuração preliminar

Antes de continuarmos, verifique se seu aplicativo é capaz de acessar a localização do dispositivo. Você precisa declarar a funcionalidade `Location` no manifesto e chamar o método **[Geolocator.RequestAccessAsync](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator.RequestAccessAsync)** para garantir que os usuários deem ao aplicativo as permissões de localização. Consulte [Obter a localização do usuário](get-location.md) para obter mais informações sobre como fazer isso. 

Lembre-se de adicionar o namespace `Geolocation` à sua classe. Isso será necessário para que todos os trechos de código neste guia funcionem.

```csharp
using Windows.Devices.Geolocation;
```

## <a name="check-the-latest-visit"></a>Verificar a visita mais recente
A maneira mais simples de usar o recurso de rastreamento de visitas é recuperar a última alteração de estado conhecido das visitas relacionadas. Uma alteração de estado é um evento de logon de plataforma na qual o usuário entra/sai de um local significativo, há movimento significativo desde o último relatório ou a localização do usuário é perdida (consulte a enumeração **[VisitStateChange](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.visitstatechange)**). Mudanças de estado são representadas por instâncias do **[Geovisit](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geovisit)**. Para recuperar a instância do **Geovisit** para a última alteração de estado gravada, basta usar o método designado na classe **[GeovisitMonitor](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geovisitmonitor)**.

> [!NOTE]
> Verificar o último logon de visita não garante que as visitas estejam atualmente sendo controladas pelo sistema. Para acompanhar as visitas enquanto elas acontecem, você deve monitorá-las em primeiro plano ou registrar-se para rastreamento em segundo plano (consulte as seções abaixo).

```csharp
private async void GetLatestStateChange() {
    // retrieve the Geovisit instance
    Geovisit latestVisit = await GeovisitMonitor.GetLastReportAsync();

    // Using the properties of "latestVisit", parse out the time that the state 
    // change was recorded, the device's location when the change was recorded,
    // and the type of state change.
}
```

### <a name="parse-a-geovisit-instance-optional"></a>Analisar uma instância de Geovisit (opcional)
O método a seguir converte todas as informações armazenadas em uma instância **Geovisit** em uma cadeia de caracteres facilmente legível. Ele pode ser usado em qualquer um dos cenários neste guia para ajudar a fornecer comentários para visitas sendo relatadas.

```csharp
private string ParseGeovisit(Geovisit visit){
    string visitString = null;

    // Use a DateTimeFormatter object to process the timestamp. The following
    // format configuration will give an intuitive representation of the date/time
    Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatterLongTime;
    
    formatterLongTime = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter(
        "{hour.integer}:{minute.integer(2)}:{second.integer(2)}", new[] { "en-US" }, "US", 
        Windows.Globalization.CalendarIdentifiers.Gregorian, 
        Windows.Globalization.ClockIdentifiers.TwentyFourHour);
    
    // use this formatter to convert the timestamp to a string, and add to "visitString"
    visitString = formatterLongTime.Format(visit.Timestamp);

    // Next, add on the state change type value
    visitString += " " + visit.StateChange.ToString();

    // Next, add the position information (if any is provided; this will be null if 
    // the reported event was "TrackingLost")
    if (visit.Position != null) {
        visitString += " (" +
        visit.Position.Coordinate.Point.Position.Latitude.ToString() + "," +
        visit.Position.Coordinate.Point.Position.Longitude.ToString() + 
        ")";
    }

    return visitString;
}
```

## <a name="monitor-visits-in-the-foreground"></a>Monitorar visitas em primeiro plano

A classe **GeovisitMonitor** usada na seção anterior também manipula o cenário de escuta para mudanças de estado ao longo de um período de tempo. Você pode fazer isso instanciando essa classe, registrando um método de manipulador para o evento e chamando o método `Start`.

```csharp
// this GeovisitMonitor instance will belong to the class scope
GeovisitMonitor monitor;

public void RegisterForVisits() {

    // Create and initialize a new monitor instance.
    monitor = new GeovisitMonitor();
    
    // Attach a handler to receive state change notifications.
    monitor.VisitStateChanged += OnVisitStateChanged;
    
    // Calling the start method will start Visits tracking for a specified scope:
    // For higher granularity such as venue/building level changes, choose Venue.
    // For lower granularity in the range of zipcode level changes, choose City.
    monitor.Start(VisitMonitoringScope.Venue);
}
```

Neste exemplo, o método `OnVisitStateChanged` manipulará a entrada de relatórios de visita. A instância **Geovisit** correspondente é passada por meio do parâmetro do evento.

```csharp
private void OnVisitStateChanged(GeoVisitWatcher sender, GeoVisitStateChangedEventArgs args) {
    Geovisit visit = args.Visit;
    
    // Using the properties of "visit", parse out the time that the state 
    // change was recorded, the device's location when the change was recorded,
    // and the type of state change.
}
```
Quando o aplicativo tiver terminado o monitoramento de alterações de estado relacionado à visita, ele deve interromper o monitor e cancelar o registro de manipuladores de eventos. Isso também deve ser feito sempre que o aplicativo for suspenso ou fechado.

```csharp
public void UnregisterFromVisits() {
    
    // Stop the monitor to stop tracking Visits. Otherwise, tracking will
    // continue until the monitor instance is destroyed.
    monitor.Stop();
    
    // Remove the handler to stop receiving state change events.
    monitor.VisitStateChanged -= OnVisitStateChanged;
}
```

## <a name="monitor-visits-in-the-background"></a>Monitorar visitas em segundo plano

Você também pode implementar monitoramento de visita em uma tarefa em segundo plano, para que a atividade de visita possa ser manipulada no dispositivo, mesmo quando seu aplicativo não estiver aberto. Esse é o método recomendado, pois é mais versátil e eficiente com relação a energia. 

Este guia usa o modelo em [Criar e registrar uma tarefa em segundo plano fora do processo](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task), em que os arquivos do aplicativo principal live em um projeto e o arquivo de tarefa em segundo plano residem em um projeto separado na mesma solução. Se essa for sua primeira vez implementando tarefas em segundo plano, é recomendável que você siga as diretrizes principais, realizando as substituições necessárias abaixo para criar uma tarefa em segundo plano de manipulação de visita.

> [!NOTE]
> Nos trechos de código os seguir, algumas funcionalidades, como tratamento de erros e armazenamento local importantes estão ausentes por questões de simplicidade. Para uma implementação robusta de manipulação de visitas em segundo plano, consulte o [Aplicativo de amostra](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation).


Primeiro, verifique se o aplicativo declarou permissões de tarefas de segundo plano. No elemento `Application/Extensions` de seu arquivo *Package. appxmanifest*, adicione a seguinte extensão (adicionar um elemento `Extensions`, se ainda não existir).

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.VisitBackgroundTask">
    <BackgroundTasks>
        <Task Type="location" />
    </BackgroundTasks>
</Extension>
```

Em seguida, na definição da classe de tarefa em segundo plano, cole o código a seguir. O método `Run` dessa tarefa em segundo plano será aprovado de forma simples pelos detalhes do gatilho (que contêm as informações de visitas) em um método separado.

```csharp
using Windows.ApplicationModel.Background;

namespace Tasks {
    
    public sealed class VisitBackgroundTask : IBackgroundTask {
        
        public void Run(IBackgroundTaskInstance taskInstance) {
            
            // get a deferral
            BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
            
            // this task's trigger will be a Geovisit trigger
            GeovisitTriggerDetails triggerDetails = taskInstance.TriggerDetails as GeovisitTriggerDetails;

            // Handle Visit reports
            GetVisitReports(triggerDetails);         

            finally {
                deferral.Complete();
            }
        }        
    }
}
```

Defina o método `GetVisitReports` em algum lugar dessa mesma classe.

```csharp
private void GetVisitReports(GeovisitTriggerDetails triggerDetails) {

    // Read reports from the triggerDetails. This populates the "reports" variable 
    // with all of the Geovisit instances that have been logged since the previous
    // report reading.
    IReadOnlyList<Geovisit> reports = triggerDetails.ReadReports();

    foreach (Geovisit report in reports) {
        // Using the properties of "visit", parse out the time that the state 
        // change was recorded, the device's location when the change was recorded,
        // and the type of state change.
    }

    // Note: depending on the intent of the app, you many wish to store the
    // reports in the app's local storage so they can be retrieved the next time 
    // the app is opened in the foreground.
}
```

Em seguida, no projeto principal do seu aplicativo, você precisará realizar o registro da tarefa em segundo plano. Cria um método de registro que pode ser chamado por alguma ação do usuário ou é chamado cada vez que a classe é ativada.

```csharp
// a reference to this registration should be declared at the class level
private IBackgroundTaskRegistration visitTask = null;

// The app must call this method at some point to allow future use of 
// the background task. 
private async void RegisterBackgroundTask(object sender, RoutedEventArgs e) {
    
    string taskName = "MyVisitTask";
    string taskEntryPoint = "Tasks.VisitBackgroundTask";

    // First check whether the task in question is already registered
    foreach (var task in BackgroundTaskRegistration.AllTasks) {
        if (task.Value.Name == taskName) {
            // if a task is found with the name of this app's background task, then
            // return and do not attempt to register this task
            return;
        }
    }
    
    // Attempt to register the background task.
    try {
        // Get permission for a background task from the user. If the user has 
        // already responded once, this does nothing and the user must manually 
        // update their preference via Settings.
        BackgroundAccessStatus backgroundAccessStatus = await BackgroundExecutionManager.RequestAccessAsync();

        switch (backgroundAccessStatus) {
            case BackgroundAccessStatus.AlwaysAllowed:
            case BackgroundAccessStatus.AllowedSubjectToSystemPolicy:
                // BackgroundTask is allowed
                break;

            default:
                // notify user that background tasks are disabled for this app
                //...
                break;
        }

        // Create a new background task builder
        BackgroundTaskBuilder visitTaskBuilder = new BackgroundTaskBuilder();

        visitTaskBuilder.Name = exampleTaskName;
        visitTaskBuilder.TaskEntryPoint = taskEntryPoint;

        // Create a new Visit trigger
        var trigger = new GeovisitTrigger();

        // Set the desired monitoring scope.
        // For higher granularity such as venue/building level changes, choose Venue.
        // For lower granularity in the range of zipcode level changes, choose City. 
        trigger.MonitoringScope = VisitMonitoringScope.Venue; 

        // Associate the trigger with the background task builder
        visitTaskBuilder.SetTrigger(trigger);

        // Register the background task
        visitTask = visitTaskBuilder.Register();      
    }
    catch (Exception ex) {
        // notify user that the task failed to register, using ex.ToString()
    }
}
```

Isso estabelece que uma classe de tarefa em segundo plano chamada `VisitBackgroundTask` no namespace `Tasks` fará algo com o tipo de disparador `location`. 

Seu aplicativo deve ser capaz de registrar a tarefa em segundo plano de manipulação de visitas, e essa tarefa deve ser ativada sempre que o dispositivo fizer uma alteração de estado relacionada à visita. Você precisará preencher a lógica em sua classe de tarefa em segundo plano para determinar o que fazer com essa informação de alteração de estado.

## <a name="related-topics"></a>Tópicos relacionados
* [Criar e registrar uma tarefa em segundo plano fora do processo](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
* [Obter a localização do usuário](get-location.md)
* [Namespace Windows.Devices.Geolocation](https://docs.microsoft.com/uwp/api/windows.devices.geolocation)