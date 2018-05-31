---
author: PatrickFarley
Description: Follow these best practices for geofencing in your app.
title: Diretrizes de aplicativos com cerca geográfica
ms.assetid: F817FA55-325F-4302-81BE-37E6C7ADC281
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, mapa, localização, cerca geográfica
ms.localizationpriority: medium
ms.openlocfilehash: e781ff1fbcf5c39f03fb7b2c5f713a881b36e4b2
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
ms.locfileid: "1689492"
---
# <a name="guidelines-for-geofencing-apps"></a>Diretrizes de aplicativos com cerca geográfica




**APIs importantes**

-   [**Classe Geofence (XAML)**](https://msdn.microsoft.com/library/windows/apps/dn263587)
-   [**Classe Geolocator (XAML)**](https://msdn.microsoft.com/library/windows/apps/br225534)

Siga estas práticas recomendadas para [**cerca geográfica**](https://msdn.microsoft.com/library/windows/apps/dn263744) em seu aplicativo.

## <a name="recommendations"></a>Recomendações


-   Se seu aplicativo precisará de acesso à Internet quando ocorrer um evento de [**Cerca geográfica**](https://msdn.microsoft.com/library/windows/apps/dn263587), verifique o acesso à Internet antes de criar a cerca geográfica.
    -   Se o aplicativo atualmente não oferece acesso à Internet, você pode pedir ao usuário que se conecte à Internet antes de você configurar a cerca geográfica.
    -   Se não for possível ter acesso à Internet, evite consumir o poder necessário para as verificações de local da cerca geográfica.
-   Verifique a relevância das notificações de cerca geográfica verificando o carimbo de data/hora e o local atual quando um evento de cerca geográfica indicar mudanças em um estado de [**Entered**](https://msdn.microsoft.com/library/windows/apps/dn263660) ou de **Exited**. Consulte **Verificando o carimbo de data/hora e o local atual** a seguir para obter mais informações.
(#timestamp) abaixo para saber mais.
-   Crie exceções para gerenciar casos em que um dispositivo não pode acessar informações sobre a localização, nem notificar o usuário se necessário. As informações de localização podem não estar disponíveis porque as permissões estão desativadas, o dispositivo não contém um rádio de GPS, o sinal de GPS está bloqueado ou o sinal de Wi-Fi não está suficientemente forte.
-   Em geral, não é necessário escutar eventos de cerca geográfica no primeiro e no segundo plano ao mesmo tempo. Todavia, se seu aplicativo precisar escutar eventos de cerca geográfica tanto em primeiro quanto em segundo plano:

    -   Chame o método [**ReadReports**](https://msdn.microsoft.com/library/windows/apps/dn263633) para descobrir se ocorreu um evento.
    -   Cancele seu registro no ouvinte de eventos em primeiro plano quando seu aplicativo não estiver visível para o usuário e registre-o novamente quando ele se tornar visível novamente.

    Consulte [Event listeners em segundo e primeiro plano](#background-and-foreground-listeners) para exemplos de código e mais informações.

-   Não use mais de 1000 cercas geográficas por aplicativo. O sistema na realidade oferece suporte para milhares de cercas geográficas por aplicativo. Você pode manter um bom desempenho do aplicativo para reduzir o uso de memória do aplicativo, usando menos de 1000.
-   Não crie uma cerca geográfica com um raio menor que 50 metros. Se seu aplicativo precisar usar cercas geográficas com raios pequenos, sugira aos usuários para o utilizarem em um dispositivo com rádio GPS para garantir um melhor desempenho.

## <a name="additional-usage-guidance"></a>Diretrizes de uso adicional

### <a name="checking-the-time-stamp-and-current-location"></a>Verificando o carimbo de data/hora e a localização atual

Quando um evento indica uma alteração em um estado [**Entered**](https://msdn.microsoft.com/library/windows/apps/dn263660) ou **Exited**, verifique o carimbo de data/hora do evento e sua localização atual. Diversos fatores, como por exemplo o sistema não ter recursos suficientes para executar uma tarefa em segundo plano, o usuário não notar a notificação ou então o dispositivo estar em modo de standby (em Windows), podem afetar o momento em que o evento é processado pelo usuário. Por exemplo, a seguinte sequência pode ocorrer:

-   Seu aplicativo cria um limite geográfico e monitora os eventos que entram e saem dele.
-   O usuário move o dispositivo para dentro do limite geográfico, provocando o disparo de um evento de entrada.
-   Seu aplicativo envia uma notificação ao usuário avisando que ele está agora dentro do limite geográfico.
-   O usuário estava ocupado e não reparou na notificação antes, somente depois de 10 minutos.
-   Durante esses 10 minutos de atraso, o usuário saiu do limite geográfico.

Pelo carimbo de data/hora, você pode dizer qual ação já ocorreu. Pela localização atual, você pode ver que o usuário agora está fora do limite geográfico. Dependendo da funcionalidade de seu aplicativo, convém remover esse evento do filtro.

### <a name="background-and-foreground-listeners"></a>Ouvintes em primeiro e segundo plano

Em geral, seu aplicativo não precisa escutar eventos de [**Cerca geográfica**](https://msdn.microsoft.com/library/windows/apps/dn263587) em primeiro plano e em uma tarefa em segundo plano ao mesmo tempo. O melhor método caso você precise das duas coisas, é deixar que a tarefa em segundo plano cuide das notificações. Se você configurar ouvintes de cerca geográfica em primeiro e em segundo plano, não será possível saber qual vai ser disparado primeiro, portanto, você sempre deverá chamar o método [**ReadReports**](https://msdn.microsoft.com/library/windows/apps/dn263633) para descobrir se um evento ocorreu.

Se você configurar ouvintes de cerca geográfica em primeiro e segundo plano, cancele o registro do ouvinte de evento em primeiro plano sempre que seu aplicativo não estiver visível para o usuário e registre novamente seu aplicativo quando ele voltar a ficar visível. Veja um código de exemplo que registra o evento de visibilidade.

```csharp
    Windows.UI.Core.CoreWindow coreWindow;    

    // This needs to be set before InitializeComponent sets up event registration for app visibility
    coreWindow = CoreWindow.GetForCurrentThread();
    coreWindow.VisibilityChanged += OnVisibilityChanged;
```

```javascript
 document.addEventListener("visibilitychange", onVisibilityChanged, false);
```

Quando a visibilidade é alterada, você pode habilitar ou desabilitar os manipuladores de eventos em primeiro plano, conforme mostrado aqui.

```csharp
private void OnVisibilityChanged(CoreWindow sender, VisibilityChangedEventArgs args)
{
    // NOTE: After the app is no longer visible on the screen and before the app is suspended
    // you might want your app to use toast notification for any geofence activity.
    // By registering for VisibiltyChanged the app is notified when the app is no longer visible in the foreground.

    if (args.Visible)
    {
        // register for foreground events
        GeofenceMonitor.Current.GeofenceStateChanged += OnGeofenceStateChanged;
        GeofenceMonitor.Current.StatusChanged += OnGeofenceStatusChanged;
    }
    else
    {
        // unregister foreground events (let background capture events)
        GeofenceMonitor.Current.GeofenceStateChanged -= OnGeofenceStateChanged;
        GeofenceMonitor.Current.StatusChanged -= OnGeofenceStatusChanged;
    }
}
```

```javascript
function onVisibilityChanged() {
    // NOTE: After the app is no longer visible on the screen and before the app is suspended
    // you might want your app to use toast notification for any geofence activity.
    // By registering for VisibiltyChanged the app is notified when the app is no longer visible in the foreground.

    if (document.msVisibilityState === "visible") {
        // register for foreground events
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.addEventListener("geofencestatechanged", onGeofenceStateChanged);
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.addEventListener("statuschanged", onGeofenceStatusChanged);
    } else {
        // unregister foreground events (let background capture events)
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.removeEventListener("geofencestatechanged", onGeofenceStateChanged);
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.removeEventListener("statuschanged", onGeofenceStatusChanged);
    }
}
```

### <a name="sizing-your-geofences"></a>Dimensionando suas cercas geográficas

Enquanto o GPS mostra as informações de localização mais precisas, o limite geográfico também usa Wi-Fi ou outros sensores de localização para determinar a posição atual do usuário. Mas o uso desses outros métodos pode afetar o tamanho dos limites geográficos que você pode criar. Se o nível de precisão for baixo, criar cercas geográficas não será útil. Em geral, é recomendado que você não crie uma cerca geográfica com um raio inferior a 50 metros. Além disso, tarefas em segundo plano de cercas geográficas são executadas apenas periodicamente no Windows. Se você utilizar uma cerca geográfica pequena, há a possibilidade de perder totalmente um evento de [**Enter**](https://msdn.microsoft.com/library/windows/apps/dn263660) ou de **Exit**.

Se seu aplicativo precisar usar cercas geográficas com raios pequenos, sugira aos usuários para o utilizarem em um dispositivo com rádio GPS para garantir um melhor desempenho.

## <a name="related-topics"></a>Tópicos relacionados


* [Configurar uma cerca geográfica](https://msdn.microsoft.com/library/windows/apps/mt219702)
* [Obter a localização atual](https://msdn.microsoft.com/library/windows/apps/mt219698)
* [Amostra de localização UWP (geolocalização)](http://go.microsoft.com/fwlink/p/?linkid=533278)
 

 
