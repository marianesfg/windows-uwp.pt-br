---
title: Obter a localização do usuário
description: Encontre a localização do usuário e responda a alterações na localização. O acesso à localização do usuário é gerenciado por configurações de privacidade no aplicativo Configurações. Este tópico também mostra como verificar se o aplicativo tem permissão para acessar a localização do usuário.
ms.assetid: 24DC9A41-8CC1-48B0-BC6D-24BF571AFCC8
---

# Obter a localização do usuário


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos do Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Encontre a localização do usuário e responda a alterações na localização. O acesso à localização do usuário é gerenciado por configurações de privacidade no aplicativo Configurações. Este tópico também mostra como verificar se o aplicativo tem permissão para acessar a localização do usuário.

**Dica** Para saber mais sobre como acessar a localização do usuário em seu aplicativo, baixe a seguinte amostra do [repositório Windows-universal-samples](http://go.microsoft.com/fwlink/p/?LinkId=619979) no GitHub.

-   [Exemplo de mapa da Plataforma Universal do Windows (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977)

## Habilitar a funcionalidade de localização


1.  No **Gerenciador de Soluções**, clique duas vezes sobre **package.appxmanifest** e selecione a guia **Funcionalidades**.
2.  Na lista **Funcionalidades**, selecione a guia **Funcionalidades**. Isso adiciona a funcionalidade `Location` do dispositivo ao arquivo de manifesto do pacote.

```XML
  <Capabilities>
    <!-- DeviceCapability elements must follow Capability elements (if present) -->
    <DeviceCapability Name="location"/>
  </Capabilities>
```

## Obter a localização atual


Esta seção descreve como detectar a localização geográfica do usuário usando APIs no namespace [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603).

### Etapa 1: solicite acesso à localização do usuário

**Importante** Você deve solicitar acesso à localização do usuário usando o método [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) antes de tentar acessar a localização do usuário. Você deve chamar o método **RequestAccessAsync** do thread de interface do usuário e seu aplicativo deve estar em segundo plano. Seu aplicativo não poderá acessar as informações de localização do usuário até que o usuário conceda permissão a seu aplicativo.

```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();
```

O método [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) solicita que o usuário permita o acesso a sua localização. O usuário é solicitado apenas uma vez (por aplicativo). Após a primeira vez em que a permissão é concedida ou negada, esse método deixa de solicitar a permissão ao usuário. Para ajudar o usuário a alterar a s permissões de localização depois que ela tiver sido solicitada, recomendamos que você forneça um link para as configurações de localização, como demonstrado posteriormente neste tópico.

### Etapa 2: obtenha a localização do usuário e registre as alterações nas permissões de localização

O método [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) realiza uma leitura única da localização atual. Aqui, uma instrução **switch** é usada com **accessStatus** (do exemplo anterior) para atuar somente quando o acesso à localização do usuário é permitido. Se o acesso à localização do usuário for permitido, o código cria um objeto [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534), registra as alterações nas permissões de localização e solicita a localização do usuário.

```csharp
switch (accessStatus)
{
    case GeolocationAccessStatus.Allowed:
        _rootPage.NotifyUser("Waiting for update...", NotifyType.StatusMessage);

        // If DesiredAccuracy or DesiredAccuracyInMeters are not set (or value is 0), DesiredAccuracy.Default is used.
        Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = _desireAccuracyInMetersValue };

        // Subscribe to the StatusChanged event to get updates of location status changes.
        _geolocator.StatusChanged += OnStatusChanged;
                        
        // Carry out the operation.
        Geoposition pos = await geolocator.GetGeopositionAsync();

        UpdateLocationData(pos);
        _rootPage.NotifyUser("Location updated.", NotifyType.StatusMessage);
        break;

    case GeolocationAccessStatus.Denied:
        _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);
        LocationDisabledMessage.Visibility = Visibility.Visible;
        UpdateLocationData(null);
        break;

    case GeolocationAccessStatus.Unspecified:
        _rootPage.NotifyUser("Unspecified error.", NotifyType.ErrorMessage);
        UpdateLocationData(null);
        break;
}
```

### Etapa 3: lide com as alterações nas permissões de localização

O objeto [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) aciona o evento [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) para indicar que as configurações de localização do usuário foram alteradas. Esse evento transmite o status correspondente por meio da propriedade **Status** do argumento (do tipo [**PositionStatus**](https://msdn.microsoft.com/library/windows/apps/br225599)). Observe que esse método não é chamado a partir do thread de interface do usuário, e que o objeto [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) invoca as alterações na interface do usuário.

```csharp
using Windows.UI.Core;
...
async private void OnStatusChanged(Geolocator sender, StatusChangedEventArgs e)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        // Show the location setting message only if status is disabled.
        LocationDisabledMessage.Visibility = Visibility.Collapsed;

        switch (e.Status)
        {
            case PositionStatus.Ready:
                // Location platform is providing valid data.
                ScenarioOutput_Status.Text = "Ready";
                _rootPage.NotifyUser("Location platform is ready.", NotifyType.StatusMessage);
                break;

            case PositionStatus.Initializing:
                // Location platform is attempting to acquire a fix. 
                ScenarioOutput_Status.Text = "Initializing";
                _rootPage.NotifyUser("Location platform is attempting to obtain a position.", NotifyType.StatusMessage);
                break;

            case PositionStatus.NoData:
                // Location platform could not obtain location data.
                ScenarioOutput_Status.Text = "No data";
                _rootPage.NotifyUser("Not able to determine the location.", NotifyType.ErrorMessage);
                break;

            case PositionStatus.Disabled:
                // The permission to access location data is denied by the user or other policies.
                ScenarioOutput_Status.Text = "Disabled";
                _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);

                // Show message to the user to go to location settings.
                LocationDisabledMessage.Visibility = Visibility.Visible;

                // Clear any cached location data.
                UpdateLocationData(null);
                break;

            case PositionStatus.NotInitialized:
                // The location platform is not initialized. This indicates that the application 
                // has not made a request for location data.
                ScenarioOutput_Status.Text = "Not initialized";
                _rootPage.NotifyUser("No request for location is made yet.", NotifyType.StatusMessage);
                break;

            case PositionStatus.NotAvailable:
                // The location platform is not available on this version of the OS.
                ScenarioOutput_Status.Text = "Not available";
                _rootPage.NotifyUser("Location is not available on this version of the OS.", NotifyType.ErrorMessage);
                break;

            default:
                ScenarioOutput_Status.Text = "Unknown";
                _rootPage.NotifyUser(string.Empty, NotifyType.StatusMessage);
                break;
        }
    });
}
```

## Responder a atualizações de localização


Esta seção descreve como usar o evento [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) para receber atualizações sobre a localização do usuário durante um intervalo de tempo. Como o usuário pode revogar o acesso a localização a qualquer momento, é importante chamar [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) e usar o evento [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) como mostrados na seção anterior.

Esta seção presume que você já habilitou os recursos de localização e chamou [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) a partir do thread de interface do usuário em seu aplicativo em segundo plano.

### Etapa 1: defina o intervalo de relatório e registre as atualizações de localização

Neste exemplo, uma instrução **switch** é usada com **accessStatus** (do exemplo anterior) para atuar somente quando o acesso à localização do usuário é permitido. Se o acesso à localização do usuário for permitido, o código cria um objeto [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534), especifica o tipo de rastreamento e registra as atualizações de localização.

O objeto [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) pode acionar o evento [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) com base em uma alteração na posição (rastreamento com base em distância) ou uma alteração em um período (rastreamento periódico).

-   Para o rastreamento com base em distância, configure a propriedade [**MovementThreshold**](https://msdn.microsoft.com/library/windows/apps/br225539).
-   Para o rastreamento periódico, configure a propriedade [**ReportInterval**](https://msdn.microsoft.com/library/windows/apps/br225541).

Se nenhuma das propriedades for definida, será retornada uma posição a cada segundo (equivalente a `ReportInterval = 1000`). Aqui, é usado um intervalo de relatório de dois segundos (`ReportInterval = 2000`).
```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();

switch (accessStatus)
{
    case GeolocationAccessStatus.Allowed:
        // Create Geolocator and define perodic-based tracking (2 second interval).
        _geolocator = new Geolocator { ReportInterval = 2000 };

        // Subscribe to the PositionChanged event to get location updates.
        _geolocator.PositionChanged += OnPositionChanged;

        // Subscribe to StatusChanged event to get updates of location status changes.
        _geolocator.StatusChanged += OnStatusChanged;
                    
        _rootPage.NotifyUser("Waiting for update...", NotifyType.StatusMessage);
        LocationDisabledMessage.Visibility = Visibility.Collapsed;
        StartTrackingButton.IsEnabled = false;
        StopTrackingButton.IsEnabled = true;
        break;

    case GeolocationAccessStatus.Denied:
        _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);
        LocationDisabledMessage.Visibility = Visibility.Visible;
        break;

    case GeolocationAccessStatus.Unspecified:
        _rootPage.NotifyUser("Unspecificed error!", NotifyType.ErrorMessage);
        LocationDisabledMessage.Visibility = Visibility.Collapsed;
        break;
}
```

### Etapa 2: lide com as atualizações de localização

O objeto [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) aciona o evento [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) para indicar que a localização do usuário mudou ou que o tempo passou, dependendo de como você o configurou. Esse evento transmite a localização correspondente por meio da propriedade **Position** do argumento (do tipo [**Geoposition**](https://msdn.microsoft.com/library/windows/apps/br225543)). Neste exemplo, o método não é chamado a partir do thread de interface do usuário, e que o objeto [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) invoca as alterações na interface do usuário.

```csharp
using Windows.UI.Core;
...
async private void OnPositionChanged(Geolocator sender, PositionChangedEventArgs e)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        _rootPage.NotifyUser("Location updated.", NotifyType.StatusMessage);
        UpdateLocationData(e.Position);
    });
}
```

## Altere configurações de privacidade de localização


Se as configurações de privacidade de localização não permitirem que seu aplicativo acesse a localização do usuário, recomendamos que você forneça um link para as **configurações de privacidade de localização** no aplicativo **Configurações**. Neste exemplo, um controle de hiperlink é usado para navegar até o URI `ms-settings:privacy-location`.

```xaml
<!--Set Visibility to Visible when access to location is denied -->  
<TextBlock x:Name="LocationDisabledMessage" FontStyle="Italic" 
                 Visibility="Collapsed" Margin="0,15,0,0" TextWrapping="Wrap" >
          <Run Text="This app is not able to access Location. Go to " />
              <Hyperlink NavigateUri="ms-settings:privacy-location">
                  <Run Text="Settings" />
              </Hyperlink>
          <Run Text=" to check the location privacy settings."/>
</TextBlock>
```

Opcionalmente, seu aplicativo pode chamar o método [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) para inicializar o aplicativo **Configurações** a partir do código. Para obter mais informações, consulte [Iniciar o aplicativo Configurações do Windows](https://msdn.microsoft.com/library/windows/apps/mt228342).

```csharp
using Windows.System;
...
bool result = await Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-location"));
```

## Solucionar problemas do aplicativo


Para que o aplicativo possa acessar a localização do usuário, **Localização** deve estar habilitado no dispositivo. No aplicativo **Configurações**, verifique se as seguintes **configurações de privacidade de localização** estão ativadas:

-   **Localização deste dispositivo...** está **ativada** (não se aplica ao Windows 10 Mobile)
-   A configuração de serviços de localização, **Localização**, está **ativada**
-   Em **Escolher aplicativos que podem usar sua localização**, seu aplicativo está definido como **ativado**

## Tópicos relacionados

* [Amostra de geolocalização do UWP](http://go.microsoft.com/fwlink/p/?linkid=533278)
* [Diretrizes de design para cerca geográfica](https://msdn.microsoft.com/library/windows/apps/dn631756)
* [Diretrizes de design para aplicativos com detecção de localização](https://msdn.microsoft.com/library/windows/apps/hh465148)




<!--HONumber=Mar16_HO5-->


