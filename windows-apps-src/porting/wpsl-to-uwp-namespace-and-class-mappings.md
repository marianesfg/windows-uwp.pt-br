---
description: Este tópico fornece um mapeamento abrangente de APIs do Windows Phone Silverlight para seus equivalentes da plataforma Universal do Windows (UWP).
title: Windows Phone Silverlight para mapeamentos de namespace e classe UWP
ms.assetid: 33f06706-4790-48f3-a2e4-ebef9ddb61a4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9acd42f57117fb01eef4ba8f87d35664be21cf32
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57634971"
---
# <a name="windowsphone-silverlight-to-uwp-api-mappings"></a>Windows Phone Silverlight para mapeamentos de API de UWP


Este tópico fornece um mapeamento abrangente de APIs do Windows Phone Silverlight para seus equivalentes da plataforma Universal do Windows (UWP). Geralmente não há um mapeamento de funcionalidade um para um: qualquer plataforma pode ter mais ou menos funcionalidades que suas equivalentes em um namespace ou classe.

A tabela de mapeamento ajudará quando você estiver trabalhando em um projeto UWP e você estiver usando novamente o código-fonte de um projeto do Windows Phone Silverlight. Há diferenças nos nomes de namespaces e classes (inclusive controles de interface do usuário) entre as duas plataformas. Em muitos casos, isso é tão fácil quanto alterar um nome de namespace e, em seguida, o código será compilado. Às vezes, uma classe ou o nome da API mudou, bem como o nome do namespace. Em outras ocasiões, o mapeamento requer um pouco mais de trabalho e, em casos raros, requer uma mudança na abordagem.

**Como usar a tabela:  ** Primeiro, procure o nome da classe que você está usando. As classes estão listadas sempre que o mapeamento é mais complicado do que simplesmente alterar o nome do namespace. Caso a classe não esteja listada, o mapeamento é apenas uma alteração do namespace. Assim, encontre o nome do namespace da classe, e você encontrará o nome do namespace UWP equivalente. A classe estará nesse namespace. Caso o namespace não esteja listado é porque o nome não foi alterado.

**Observação**  Windows 10 dá suporte a muito mais do que o .NET Framework que um aplicativo do Windows Phone Store. Por exemplo, o Windows 10 tem vários System. ServiceModel. \* namespaces, bem como Sockets, NetworkInformation e System.Net.
Além disso, em um aplicativo do Windows 10, você será beneficiado com .NET Native, que uma tecnologia de compilação ahead of time que converte o MSIL em código executável nativo de máquina. Aplicativos .NET Native iniciam com mais rapidez, usam menos memória e bateria do que seus equivalentes MSIL.

| Windows Phone Silverlight | Tempo de execução do Windows |
| ------------------------- | --------------- |
| Publicidade | |
| Classe **Microsoft.Advertising.Mobile.UI.AdControl** | Classe [AdControl](https://msdn.microsoft.com/library/advertising-windows-sdk-api-reference-adcontrol.aspx) |
| Alarmes, lembretes e agentes de segundo plano | |
| Classe **Microsoft.Phone.BackgroundAgent** | [**BackgroundTaskBuilder** ](https://msdn.microsoft.com/library/windows/apps/br224768) classe |
| Namespace **Microsoft.Phone.Scheduler** | [**Windows.ApplicationModel.Background** ](https://msdn.microsoft.com/library/windows/apps/br224847) namespace |
| Classe **Microsoft.Phone.Scheduler.Alarm** | [**BackgroundTaskBuilder** ](https://msdn.microsoft.com/library/windows/apps/br224768) e [ **ToastNotificationManager** ](https://msdn.microsoft.com/library/windows/apps/br208642) classes |
| Classes **Microsoft.Phone.Scheduler.PeriodicTask**, **ScheduledAction**, **ScheduledActionService**, **ScheduledTask** , **ScheduledTaskAgent** | [**BackgroundTaskBuilder** ](https://msdn.microsoft.com/library/windows/apps/br224768) classe |
| Classe **Microsoft.Phone.Scheduler.Reminder** | [**BackgroundTaskBuilder** ](https://msdn.microsoft.com/library/windows/apps/br224768) e [ **ToastNotificationManager** ](https://msdn.microsoft.com/library/windows/apps/br208642) classes |
| Classe **Microsoft.Phone.PictureDecoder** | [**{5&gt;{6&gt;BitmapDecoder&lt;6}&lt;5** ](https://msdn.microsoft.com/library/windows/apps/br226176) classe |
| Namespace **Microsoft.Phone.BackgroundAudio** | [**Windows.Media.Playback** ](https://msdn.microsoft.com/library/windows/apps/dn640562) namespace |
| Namespace **Microsoft.Phone.BackgroundTransfer** | [**Windows.Networking.BackgroundTransfer** ](https://msdn.microsoft.com/library/windows/apps/br207242) namespace |
| Modelo de aplicativo e ambiente | |
| Classe **System.AppDomain** | Nenhuma equivalência direta. Veja as classes [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324), [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016) |
| Classe **System.Environment** | Nenhuma equivalência direta |
| Classe **System.ComponentModel.Annotations**  | Nenhuma equivalência direta |
| Classe **System.ComponentModel.BackgroundWorker** | [**ThreadPool** ](https://msdn.microsoft.com/library/windows/apps/br229621) classe |
| Classe **System.ComponentModel.DesignerProperties** | [**DesignMode** ](https://msdn.microsoft.com/library/windows/apps/br224664) classe |
| **Classes System.Threading.Thread**, **System.Threading.ThreadPool** | [**ThreadPool** ](https://msdn.microsoft.com/library/windows/apps/br229621) classe |
| (ST = **System.Threading**) <br/> Método **ST.Thread.MemoryBarrier** | (ST = **System.Threading**) <br/> Método **ST.Interlocked.MemoryBarrier** |
| (ST = **System.Threading**) <br/> Propriedade **ST.Thread.ManagedThreadId** | (S = **System**) <br/> Propriedade **S.Environment.ManagedThreadId** |
| Classe **System.Threading.Timer** | [**ThreadPoolTimer** ](https://msdn.microsoft.com/library/windows/apps/br230587) classe |
| (SWT = **System.Windows.Threading**) <br/> Classe **SWT.Dispatcher** | [**CoreDispatcher** ](https://msdn.microsoft.com/library/windows/apps/br208211) classe |
| (SWT = **System.Windows.Threading**) <br/> Classe **SWT.DispatcherTimer** | [**DispatcherTimer** ](https://msdn.microsoft.com/library/windows/apps/br244250) classe |
| Blend for Visual Studio | |
| (MEDC = **Microsoft.Expression.Drawing.Core**) <br/> Classe **MEDC.GeometryHelper** | Nenhuma equivalência direta |
| Namespace **Microsoft.Expression.Interactivity** | Namespace [Microsoft.Xaml.Interactivity](https://go.microsoft.com/fwlink/p/?LinkId=328776) |
| Namespace **Microsoft.Expression.Interactivity.Core** | Namespace [Microsoft.Xaml.Interactions.Core](https://go.microsoft.com/fwlink/p/?LinkId=328773) |
| (MEIC = **Microsoft.Expression.Interactivity.Core**) <br/> Classe **MEIC.ExtendedVisualStateManager** | Nenhuma equivalência direta |
| Namespace **Microsoft.Expression.Interactivity.Input** | Nenhuma equivalência direta |
| Namespace **Microsoft.Expression.Interactivity.Media** | Namespace [Microsoft.Xaml.Interactions.Media](https://go.microsoft.com/fwlink/p/?LinkId=328775) |
| Namespace **Microsoft.Expression.Shapes** | Nenhuma equivalência direta |
| (MI = **Microsoft.Internal**) <br/> Interface **MI.IManagedFrameworkInternalHelper** | Nenhuma equivalência direta |
| Dados de contato e do calendário | |
| Namespace **Microsoft.Phone.UserData** | [**ApplicationModel**](https://msdn.microsoft.com/library/windows/apps/br225002), [ **Windows.ApplicationModel.Appointments** ](https://msdn.microsoft.com/library/windows/apps/dn263359) namespaces |
| (MPU = **Microsoft.Phone.UserData**) <br/> Classes **MPU.Account**, **ContactAddress**, **ContactCompanyInformation**, **ContactEmailAddress**, **ContactPhoneNumber** | [**Entre em contato com** ](https://msdn.microsoft.com/library/windows/apps/br224849) classe |
| (MPU = **Microsoft.Phone.UserData**) <br/> Classe **MPU.Appointments** | [**AppointmentCalendar** ](https://msdn.microsoft.com/library/windows/apps/dn596134) classe |
| (MPU = **Microsoft.Phone.UserData**) <br/> Classe **MPU.Contacts** | [**ContactStore** ](https://msdn.microsoft.com/library/windows/apps/dn624859) classe |
| Controles e infraestrutura de interface do usuário | |
| Classe **ControlTiltEffect.TiltEffect** | As animações da biblioteca de animação do Windows Runtime são incorporadas os estilos padrão dos controles comuns. Consulte [Animação](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Namespace **Microsoft.Phone.Controls** | [**Windows** ](https://msdn.microsoft.com/library/windows/apps/br227716) namespace |
| (MPC = **Microsoft.Phone.Controls**) <br/> Classe **MPC.ContextMenu** | [**PopupMenu** ](https://msdn.microsoft.com/library/windows/apps/br208693) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.DatePickerPage** | [**DatePickerFlyout** ](https://msdn.microsoft.com/library/windows/apps/dn625013) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.GestureListener** | [**GestureRecognizer** ](https://msdn.microsoft.com/library/windows/apps/br241937) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.LongListSelector** | [**SemanticZoom** ](https://msdn.microsoft.com/library/windows/apps/hh702601) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.ObscuredEventArgs** | [**SystemProtection**](https://msdn.microsoft.com/library/windows/apps/jj585394), [ **WindowActivatedEventArgs** ](https://msdn.microsoft.com/library/windows/apps/br208377) classes |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.Panorama** | [**Hub** ](https://msdn.microsoft.com/library/windows/apps/dn251843) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationFrame**,<br/>(SWN = **System.Windows.Navigation**) <br/>Classes **SWN.NavigationService** | [**Quadro** ](https://msdn.microsoft.com/library/windows/apps/br242682) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.PhoneApplicationPage** | [**Página** ](https://msdn.microsoft.com/library/windows/apps/br227503) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.TiltEffect** | [**PointerDownThemeAnimation** ](https://msdn.microsoft.com/library/windows/apps/hh969164) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.TimePickerPage** | [**TimePickerFlyout** ](https://msdn.microsoft.com/library/windows/apps/dn608313) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.WebBrowser** | [**WebView** ](https://msdn.microsoft.com/library/windows/apps/br227702) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.WebBrowserExtensions** | Nenhuma equivalência direta |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.WrapPanel** | Não há equivalente direto para fins de layout geral. [**ItemsWrapGrid** ](https://msdn.microsoft.com/library/windows/apps/dn298849) e [ **WrapGrid** ](https://msdn.microsoft.com/library/windows/apps/br227717) podem ser usados no modelo de painel de itens de um controle de itens. |
| (MPD = **Microsoft.Phone.Data**) <br/>Namespace **MPD.Linq** | Nenhuma equivalência direta |
| (MPD = **Microsoft.Phone.Data**) <br/>Namespace **MPD.Linq.Mapping** | Nenhuma equivalência direta |
| Namespace **Microsoft.Phone.Globalization** | Nenhuma equivalência direta |
| (MPI = **Microsoft.Phone.Info**) <br/>Classes **MPI.DeviceExtendedProperties**, **DeviceStatus** | [**EasClientDeviceInformation**](https://msdn.microsoft.com/library/windows/apps/hh701390), [ **MemoryManager** ](https://msdn.microsoft.com/library/windows/apps/dn633831) classes. Para obter mais detalhes, consulte [Status do dispositivo](wpsl-to-uwp-input-and-sensors.md). |
| (MPI = **Microsoft.Phone.Info**) <br/>Classe **MPI.MediaCapabilities** | Nenhuma equivalência direta |
| (MPI = **Microsoft.Phone.Info**) <br/>Classe **MPI.UserExtendedProperties** | [**AdvertisingManager** ](https://msdn.microsoft.com/library/windows/apps/dn363391) classe |
| Namespace **System.Windows** | [**Windows.UI.Xaml** ](https://msdn.microsoft.com/library/windows/apps/br209045) namespace |
| Namespace **System.Windows.Automation** | [**Windows.UI.Xaml.Automation** ](https://msdn.microsoft.com/library/windows/apps/br209179) namespace |
| Namespaces **System.Windows.Controls**, **System.Windows.Input** | [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383), [ **Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084), [ **Windows** ](https://msdn.microsoft.com/library/windows/apps/br227716) namespaces |
| Classes **System.Windows.Controls.DrawingSurface**, **DrawingSurfaceBackgroundGrid** | [**SwapChainPanel** ](https://msdn.microsoft.com/library/windows/apps/dn252834) classe |
| Classe **System.Windows.Controls.RichTextBox** | [**RichEditBox** ](https://msdn.microsoft.com/library/windows/apps/br227548) classe |
| Classe **System.Windows.Controls.WrapPanel** | Não há equivalente direto para fins de layout geral. [**ItemsWrapGrid** ](https://msdn.microsoft.com/library/windows/apps/dn298849) e [ **WrapGrid** ](https://msdn.microsoft.com/library/windows/apps/br227717) podem ser usados no modelo de painel de itens de um controle de itens. |
| Namespace **System.Windows.Controls.Primitives** | [**Primitives** ](https://msdn.microsoft.com/library/windows/apps/br209818) namespace |
| Namespace **System.Windows.Controls.Shapes** | [**Windows.UI.Xaml.Controls.Shapes** ](/uwp/api/Windows.UI.Xaml.Shapes) namespace |
| Namespace **System.Windows.Data** | [**Windows.UI.Xaml.Data** ](https://msdn.microsoft.com/library/windows/apps/br209917) namespace |
| Namespace **System.Windows.Documents** | [**Windows.UI.Xaml.Documents** ](https://msdn.microsoft.com/library/windows/apps/br209984) namespace |
| Namespace **System.Windows.Ink** | Nenhuma equivalência direta |
| Namespace **System.Windows.Markup** | [**Windows.UI.Xaml.Markup** ](https://msdn.microsoft.com/library/windows/apps/br228046) namespace | 
| Namespace **System.Windows.Navigation** | [**Windows.UI.Xaml.Navigation** ](https://msdn.microsoft.com/library/windows/apps/br243300) namespace |
| Evento **System.Windows.UIElement.Tap**, **EventHandler&lt;GestureEventArgs&gt;** delegado | [**Tocado** ](https://msdn.microsoft.com/library/windows/apps/br208985) evento [ **TappedEventHandler** ](https://msdn.microsoft.com/library/windows/apps/br227993) delegar |
| Dados e serviços |  |
| Classe **System.Data.Linq.DataContext** | Nenhuma equivalência direta |
| Classe **System.Data.Linq.Mapping.ColumnAttribute** | Nenhuma equivalência direta |
| Classe **System.Data.Linq.SqlClient.SqlHelpers** | Nenhuma equivalência direta |
| Dispositivos | |
| Namespaces **Microsoft.Devices**, **Microsoft.Devices.Sensors** | [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459), [ **Windows.Devices.Enumeration.Pnp**](https://msdn.microsoft.com/library/windows/apps/br225517), [ **Windows.Devices.Input** ](https://msdn.microsoft.com/library/windows/apps/br225648), [ **Windows.Devices.Sensors** ](https://msdn.microsoft.com/library/windows/apps/br206408) namespaces |
| Classes **Microsoft.Devices.Camera**, **Microsoft.Devices.PhotoCamera** | [**MediaCapture** ](https://msdn.microsoft.com/library/windows/apps/br241124) classe. Além disso, a classe [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) class (somente Windows). |
| Classe **Microsoft.Devices.CameraButtons** | [**HardwareButtons** ](https://msdn.microsoft.com/library/windows/apps/jj207557) classe |
| Classe **Microsoft.Devices.CameraVideoBrushExtensions** | [**CaptureElement** ](https://msdn.microsoft.com/library/windows/apps/br209278) classe |
| Classe **Microsoft.Devices.Environment** | Nenhuma equivalência direta. Como alternativa, use a compilação condicional e defina um símbolo personalizado. Ou você pode ser capaz de criar uma solução alternativa usando a propriedade [IsAttached](https://msdn.microsoft.com/library/e299w87h.aspx). |
| Classe **Microsoft.Devices.MediaHistory** | Nenhuma equivalência direta |
| Classe **Microsoft.Devices.VibrateController** | [**VibrationDevice** ](https://msdn.microsoft.com/library/windows/apps/jj207230) classe |
| Classe **Microsoft.Devices.Radio.FMRadio** | Nenhuma equivalência direta |
| Classes **Microsoft.Devices.Sensors.Accelerometer**, **Compass** | No namespace [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/br206408) |
| Classe **Microsoft.Devices.Sensors.Gyroscope** | [**Girômetro** ](https://msdn.microsoft.com/library/windows/apps/br225718) classe |
| Classe **Microsoft.Devices.Sensors.Motion** | [**Inclinômetro** ](https://msdn.microsoft.com/library/windows/apps/br225766) classe |
| Globalização | |
| Namespace **System.Globalization** | [**Windows.Globalization** ](https://msdn.microsoft.com/library/windows/apps/br206813) namespace |
| (ST = **System.Threading**) <br/> Propriedade **ST.Thread.CurrentCulture** | (SG = **System.Globalization**) <br/> Propriedade **S.CultureInfo.CurrentCulture** |
| (ST = **System.Threading**) <br/> Propriedade **ST.Thread.CurrentUICulture** | (SG = **System.Globalization**) <br/> Propriedade **S.CultureInfo.CurrentUICulture** |
| Elementos gráficos e animação | |
| **XNA. \***  namespaces, [biblioteca de classes do XNA Framework](https://go.microsoft.com/fwlink/p/?LinkId=263769), [biblioteca de classe do Pipeline de conteúdo](https://go.microsoft.com/fwlink/p/?LinkId=263770) | Nenhuma equivalência direta. Em geral, use [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) com C++. Consulte [Desenvolvendo jogos](https://msdn.microsoft.com/library/windows/apps/hh452744) e [Interoperabilidade entre DirectX e XAML](https://msdn.microsoft.com/library/windows/apps/hh825871). |
| Classe **Microsoft.Xna.Framework.Audio.Microphone** | [**MediaCapture** ](https://msdn.microsoft.com/library/windows/apps/br241124) classe |
| Classe **Microsoft.Xna.Framework.Audio.SoundEffect** | [**MediaElement** ](https://msdn.microsoft.com/library/windows/apps/br242926) classe |
| Namespace **Microsoft.Xna.Framework.GamerServices** | (WPS = **Windows.Phone.System**) <br/> [**WPS. UserProfile.GameServices.Core** ](https://msdn.microsoft.com/library/windows/apps/jj207609) namespace |
| Classe **Microsoft.Xna.Framework.GamerServices.Guide** | Nenhuma equivalência direta |
| Classe **Microsoft.Xna.Framework.Input.GamePad** | [**HardwareButtons** ](https://msdn.microsoft.com/library/windows/apps/jj207557) classe |
| Classe **Microsoft.Xna.Framework.Input.Touch.TouchPanel** | [**GestureRecognizer** ](https://msdn.microsoft.com/library/windows/apps/br241937) classe |
| (MXFM = **Microsoft.Xna.Framework.Media**) <br/> Classes **MXFM.MediaLibrary**, **MXFM.PhoneExtensions.MediaLibraryExtensions** | [**KnownFolders** ](https://msdn.microsoft.com/library/windows/apps/br227151) classe |
| Classe **Microsoft.Xna.Framework.Media.MediaQueue** | [**SystemMediaTransportControls** ](https://msdn.microsoft.com/library/windows/apps/dn278677) classe |
| Classe **Microsoft.Xna.Framework.Media.Playlist** | [**BackgroundMediaPlayer** ](https://msdn.microsoft.com/library/windows/apps/dn652527) classe |
| Namespace **System.Windows.Media** | [**UI** ](/uwp/api/Windows.UI.Xaml.Media) namespace |
| Classe **System.Windows.Media.RadialGradientBrush** | Nenhuma equivalência direta. Consulte [Mídia e elementos gráficos](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Namespace **System.Windows.Media.Animation** | [**Animation** ](https://msdn.microsoft.com/library/windows/apps/br243232) namespace |
| Namespace **System.Windows.Media.Effects** | Nenhuma equivalência direta |
| Namespace **System.Windows.Media.Imaging** | [**Imaging** ](https://msdn.microsoft.com/library/windows/apps/br243258) namespace |
| Namespace **System.Windows.Media.Media3D** | [**Windows.UI.Xaml.Media.Media3D** ](https://msdn.microsoft.com/library/windows/apps/br243274) namespace |
| Namespace **System.Windows.Shapes** | [**Windows.UI.Xaml.Shapes** ](/uwp/api/Windows.UI.Xaml.Shapes) namespace |
| Iniciadores e seletores | |
| Classes **Microsoft.Phone.Tasks.AddressChooserTask**, **EmailAddressChooserTask**, **PhoneNumberChooserTask** | [**ContactPicker** ](https://msdn.microsoft.com/library/windows/apps/br224913) classe |
| Classes **Microsoft.Phone.Tasks.AddWalletItemTask**, **AddWalletItemResult** | [**Windows.ApplicationModel.Wallet** ](https://msdn.microsoft.com/library/windows/apps/dn631399) namespace |
| Classes **Microsoft.Phone.Tasks.BingMapsDirectionsTask**, **BingMapsTask** | Nenhuma equivalência direta |
| Classe **Microsoft.Phone.Tasks.CameraCaptureTask** | [**MediaCapture** ](https://msdn.microsoft.com/library/windows/apps/br241124) classe. Além disso, a classe [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) class (somente Windows). |
| **Microsoft.Phone.Tasks.MarketplaceDetailTask** | [**CurrentApp** ](https://msdn.microsoft.com/library/windows/apps/hh779765) classe ([**RequestAppPurchaseAsync** ](https://msdn.microsoft.com/library/windows/apps/hh967813) método) |
| Classes **Microsoft.Phone.Tasks.ConnectionSettingsTask**, **MarketplaceHubTask**, **MarketplaceReviewTask**, **MarketplaceSearchTask**, **MediaPlayerLauncher**, **SearchTask**, **SmsComposeTask**, **WebBrowserTask** | [**Inicializador** ](https://msdn.microsoft.com/library/windows/apps/br241801) classe |
| Classe **Microsoft.Phone.Tasks.EmailComposeTask** | [**EmailMessage** ](https://msdn.microsoft.com/library/windows/apps/dn631270) classe |
| Classe **Microsoft.Phone.Tasks.GameInviteTask** | Nenhuma equivalência direta |
| Classes **Microsoft.Phone.Tasks.MapDownloaderTask**, **MapsDirectionsTask**, **MapsTask**, **MapUpdaterTask** | Nenhuma equivalência direta |
| Classe **Microsoft.Phone.Tasks.PhoneCallTask** | [**PhoneCallManager** ](https://msdn.microsoft.com/library/windows/apps/dn624832) classe |
| Classe **Microsoft.Phone.Tasks.PhotoChooserTask** | [**FileOpenPicker** ](https://msdn.microsoft.com/library/windows/apps/br207847) classe |
| Classe **Microsoft.Phone.Tasks.SaveAppointmentTask** | [**AppointmentManager** ](https://msdn.microsoft.com/library/windows/apps/dn297254) classe |
| Classes **Microsoft.Phone.Tasks.SaveContactTask**, **SaveEmailAddressTask**, **SavePhoneNumberTask** | [**StoredContact** ](https://msdn.microsoft.com/library/windows/apps/jj207727) classe (apenas no Windows Phone) | 
| Classe **Microsoft.Phone.Tasks.SaveRingtoneTask** | Nenhuma equivalência direta |
| Classes **Microsoft.Phone.Tasks.ShareLinkTask**, **ShareMediaTask**, **ShareStatusTask** | [**DataPackage** ](https://msdn.microsoft.com/library/windows/apps/br205873) classe |
| Location | |
| Namespace **System.Device.Location** | [**Windows.Devices.Geolocation** ](https://msdn.microsoft.com/library/windows/apps/br225603) namespace |
| Classe **System.Device.GeoCoordinateWatcher** | [**Geolocator** ](https://msdn.microsoft.com/library/windows/apps/br225534) classe |
| Mapas | |
| Namespaces **Microsoft.Phone.Maps** | [**Windows.Services.Maps** ](https://msdn.microsoft.com/library/windows/apps/dn636979) namespace |
| Namespace **Microsoft.Phone.Maps.Controls** | [**Windows.UI.Xaml.Controls.Maps** ](https://msdn.microsoft.com/library/windows/apps/dn610751) namespace |
| Classe **Microsoft.Phone.Maps.Controls.Map** | [**MapControl** ](https://msdn.microsoft.com/library/windows/apps/dn637004) classe |
| Namespace **Microsoft.Phone.Maps.Services** | [**Windows.Services.Maps** ](https://msdn.microsoft.com/library/windows/apps/dn636979) namespace |
| Classes **Microsoft.Phone.Maps.Services.GeocodeQuery**, **ReverseGeocodeQuery** | [**MapLocationFinder** ](https://msdn.microsoft.com/library/windows/apps/dn627550) classe |
| Classe **System.Device.Location.GeoCoordinate** | [**Geopoint** ](https://msdn.microsoft.com/library/windows/apps/dn263675) classe |
| Classe **Microsoft.Phone.Maps.Services.Route** | [**MapRoute** ](https://msdn.microsoft.com/library/windows/apps/dn636937) classe |
| Classe **Microsoft.Phone.Maps.Services.RouteQuery** | [**MapRouteFinder** ](https://msdn.microsoft.com/library/windows/apps/dn636938) classe |
| Monetização | |
| Namespace **Microsoft.Phone.Marketplace** | [**Windows.ApplicationModel.Store** ](https://msdn.microsoft.com/library/windows/apps/br225197) namespace |
| Media | |
| Namespace **Microsoft.Phone.Media** | [**MediaElement** ](https://msdn.microsoft.com/library/windows/apps/br242926) classe |
| Rede | |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.DeviceNetworkInformation** | [**Nome do host**](https://msdn.microsoft.com/library/windows/apps/br207113), [ **NetworkInformation** ](https://msdn.microsoft.com/library/windows/apps/br207293) classes |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.NetworkInterface** | [**NetworkInformation** ](https://msdn.microsoft.com/library/windows/apps/br207293) classe |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.NetworkInterfaceInfo** | [**ConnectionProfile** ](https://msdn.microsoft.com/library/windows/apps/br207249) classe |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.NetworkInterfaceList** | [**NetworkInformation** ](https://msdn.microsoft.com/library/windows/apps/br207293) classe |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.SocketExtensions** | Nenhuma equivalência direta |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.WebRequestExtensions** | Nenhuma equivalência direta |
| Namespace **Microsoft.Phone.Networking.Voip** | Nenhuma equivalência direta |
| Classe **System.Net.CookieCollection** | Ainda há suporte, mas algumas propriedades estão ausentes (por exemplo, IsReadOnly) |
| Classe **System.Net.DownloadProgressChangedEventArgs**, e classes semelhantes relacionadas a **System.Net.WebClient** | [**HttpClient** ](https://msdn.microsoft.com/library/windows/apps/dn298639) classe (ou [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Derivar de [System.Net.Http.StreamContent](https://msdn.microsoft.com/library/system.net.http.streamcontent.aspx) para medir o progresso. |
| Classes **System.Net.DnsEndPoint**, **IPAddress** | Essas classes ainda têm suporte, mas algumas propriedades estão ausentes. Como alternativa, porte para a classe [**HostName**](https://msdn.microsoft.com/library/windows/apps/br207113). |
| Classe **System.Net.HttpUtility** | [**HtmlFormatHelper** ](https://msdn.microsoft.com/library/windows/apps/hh738437) classe |
| Classe **System.Net.HttpWebRequest** | Suporte parcial, mas a alternativa recomendada e prospectiva é a classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Essas APIs usam [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx) para representar uma solicitação HTTP. |
| Classe **System.Net.HttpWebResponse** | Ainda compatível, mas use Dispose() em vez de Close(). Porém, a alternativa futura, recomendada, é a classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Essas APIs usam [System.Net.Http.HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx) para representar uma resposta HTTP. |
| (SNN = **System.Net.NetworkInformation**) <br/> Classe **SNN.NetworkChange** | Ainda há suporte, exceto para o construtor. |
| Classe **System.Net.OpenReadCompletedEventArgs** e classes semelhantes relacionadas a **System.Net.WebClient** | [**HttpClient** ](https://msdn.microsoft.com/library/windows/apps/dn298639) classe (ou [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| Classe **System.Net.Sockets.Socket** | Ainda compatível, mas use Dispose() em vez de Close(). Como alternativa, porte para a classe [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882). |
| Classe **System.Net.Sockets.SocketException** | Ainda há suporte, mas use a propriedade SocketErrorCode em vez de ErrorCode. |
| Classes **System.Net.Sockets.UdpAnySourceMulticastClient**, **UdpSingleSourceMulticastClient** | [**DatagramSocket** ](https://msdn.microsoft.com/library/windows/apps/br241319) classe |
| Classe **System.Net.UploadProgressChangedEventArgs**, e classes semelhantes relacionadas a **System.Net.WebClient** | [**HttpClient** ](https://msdn.microsoft.com/library/windows/apps/dn298639) classe (ou [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| Classe **System.Net.WebClient** | [**HttpClient** ](https://msdn.microsoft.com/library/windows/apps/dn298639) classe (ou [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| Classe **System.Net.WebRequest** | Suporte parcial (um conjunto de propriedades diferente), mas a alternativa recomendada e prospectiva é a classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Essas APIs usam [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx) para representar uma solicitação HTTP. |
| Classe **System.Net.WebResponse** | Ainda compatível, mas use Dispose() em vez de Close(). Porém, a alternativa futura, recomendada, é a classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Essas APIs usam [System.Net.Http.HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx) para representar uma resposta HTTP. |
| (SN = **System.Net**) <br/> Classe **SN.WriteStreamClosedEventArgs** | [**HttpClient** ](https://msdn.microsoft.com/library/windows/apps/dn298639) classe (ou [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| (SN = **System.Net**) <br/> Classe **SN.WriteStreamClosedEventHandler** | [**HttpClient** ](https://msdn.microsoft.com/library/windows/apps/dn298639) classe (ou [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| Classe **System.UriFormatException** | Classe **System.FormatException** |
| Notificações | |
| Namespace MPN = **Microsoft.Phone.Notification** | [**Windows**](https://msdn.microsoft.com/library/windows/apps/br208661), [ **Windows.Networking.PushNotifications** ](https://msdn.microsoft.com/library/windows/apps/br241307) namespaces |
| MPN = **Microsoft.Phone.Notification** <br/> Classe **MPN.HttpNotification** | [**TileNotification** ](https://msdn.microsoft.com/library/windows/apps/br208616) classe |
| MPN = **Microsoft.Phone.Notification** <br/> Classe **MPN.HttpNotificationChannel** | [**PushNotificationChannel** ](https://msdn.microsoft.com/library/windows/apps/br241283) classe |
| Programação | |
| Namespace **System** | [**Foundation** ](https://msdn.microsoft.com/library/windows/apps/br226021) namespace |
| Classes **System.Diagnostics.StackFrame**, **StackTrace** | Nenhuma equivalência direta |
| Namespace **System.Diagnostics** | [**Windows** ](https://msdn.microsoft.com/library/windows/apps/br206677) namespace |
| Interface **System.ICloneable** | Um método personalizado que retorna o tipo adequado. |
| Classe **System.Reflection.Emit.ILGenerator** | Nenhuma equivalência direta |
| Extensões reativas | |
| Namespace **Microsoft.Phone.Reactive** | Nenhuma equivalência direta |
| Reflexão | |
| Classe **System.Type** | Classe **System.Reflection.TypeInfo**. Consulte [Reflexão no .NET Framework para aplicativos UWP](https://msdn.microsoft.com/library/hh535795.aspx). |
| Recursos | |
| Classe **System.Resources.ResourceManager** | (WA = **Windows.ApplicationModel**)<br/>[**WA. Resources.Core** ](https://msdn.microsoft.com/library/windows/apps/br225039) e [ **WA. Recursos** ](https://msdn.microsoft.com/library/windows/apps/br206022) namespaces, [ **ResourceManager** ](https://msdn.microsoft.com/library/windows/apps/br206078) classe. Consulte [Criando e recuperando recursos em aplicativos do Windows Runtime](https://msdn.microsoft.com/library/windows/apps/xaml/hh694557.aspx). |
| Elemento seguro | |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> Classes **MPS.SecureElementChannel**, **MPS.SecureElementSession** | [**SmartCardConnection** ](https://msdn.microsoft.com/library/windows/apps/dn608002) classe |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> Classe **MPS.SecureElementReader** | [**Leitora de cartão inteligente** ](https://msdn.microsoft.com/library/windows/apps/dn263857) classe |
| Segurança | |
| (SSC = **System.Security.Cryptography**) <br/> Classes **SSC.Aes**, **SSC.RSA** | [**CryptographicEngine** ](https://msdn.microsoft.com/library/windows/apps/br241490) classe |
| (SSC = **System.Security.Cryptography**) <br/> Classes **SSC.HMACSHA256**, **SSC.SHA256** | [**HashAlgorithmProvider** ](https://msdn.microsoft.com/library/windows/apps/br241511) classe |
| (SSC = **System.Security.Cryptography**) <br/> Classe **SSC.ProtectedData** | [**DataProtectionProvider** ](https://msdn.microsoft.com/library/windows/apps/br241559) classe |
| (SSC = **System.Security.Cryptography**) <br/> Classe **SSC.RandomNumberGenerator** | [**CryptographicBuffer** ](https://msdn.microsoft.com/library/windows/apps/br227092) classe |
| (SSC = **System.Security.Cryptography**) <br/> Classe **SSC.X509Certificates.X509Certificate** | [**CertificateEnrollmentManager** ](https://msdn.microsoft.com/library/windows/apps/br212075) classe |
| Shell | |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ApplicationBar** | [**CommandBar** ](https://msdn.microsoft.com/library/windows/apps/dn279427) classe |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ApplicationBarIconButton** | [**AppBarButton** ](https://msdn.microsoft.com/library/windows/apps/dn279244) classe (quando usado dentro de [ **PrimaryCommands** ](https://msdn.microsoft.com/library/windows/apps/dn279430) propriedade) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ApplicationBarMenuItem** | [**AppBarButton** ](https://msdn.microsoft.com/library/windows/apps/dn279244) classe (quando usado dentro de [ **SecondaryCommands** ](https://msdn.microsoft.com/library/windows/apps/dn279434) propriedade) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classes **MPSh.CycleTileData**, **MPSh.FlipTileData**, **MPSh.IconicTileData**, **MPSh.ShellTileData**, **MPSh.StandardTileData** | [**TileTemplateType** ](https://msdn.microsoft.com/library/windows/apps/br208621) classe |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.PhoneApplicationService** | [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016), [ **DisplayRequest** ](https://msdn.microsoft.com/library/windows/apps/br241816) classes |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ProgressIndicator** | [**StatusBarProgressIndicator** ](https://msdn.microsoft.com/library/windows/apps/dn633865) classe |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ShellTile** | [**SecondaryTile** ](https://msdn.microsoft.com/library/windows/apps/br242183) classe |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ShellTileSchedule** | [**TileUpdater** ](https://msdn.microsoft.com/library/windows/apps/br208628) classe |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ShellToast** | [**ToastNotificationManager** ](https://msdn.microsoft.com/library/windows/apps/br208642) classe |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.SystemTray** | [**StatusBar** ](https://msdn.microsoft.com/library/windows/apps/dn633864) classe |
| Armazenamento e E/S | |
| Classes **Microsoft.Phone.Storage.ExternalStorage**, **ExternalStorageDevice**, **ExternalStorageFile**, **ExternalStorageFolder** | [**KnownFolders** ](https://msdn.microsoft.com/library/windows/apps/br227151) classe |
| Namespace **System.IO** | [**Storage**](https://msdn.microsoft.com/library/windows/apps/br227346), [ **Windows.Storage.Streams** ](https://msdn.microsoft.com/library/windows/apps/br241791) namespaces |
| Classe **System.IO.Directory** | [**StorageFolder** ](https://msdn.microsoft.com/library/windows/apps/br227230) classe |
| Classe **System.IO.File** | [**StorageFile** ](https://msdn.microsoft.com/library/windows/apps/br227171) e [ **PathIO** ](https://msdn.microsoft.com/library/windows/apps/hh701663) classes
| (SII = **System.IO.IsolatedStorage**) <br/> Classe **SII.IsolatedStorageFile** |[**ApplicationData.LocalFolder** ](https://msdn.microsoft.com/library/windows/apps/br241621) propriedade |
| (SII = **System.IO.IsolatedStorage**) <br/> Classe **SII.IsolatedStorageSettings** | [**ApplicationData.LocalSettings** ](https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.localsettings.aspx) propriedade |
| Classe **System.IO.Stream** | Ainda há suporte, mas use ReadAsync() e WriteAsync() em vez de BeginRead()/EndRead() e BeginWrite()/EndWrite(). |
| Carteira | |
| Namespace **Microsoft.Phone.Wallet** | [**Windows.ApplicationModel.Wallet** ](https://msdn.microsoft.com/library/windows/apps/dn631399) namespace |
| Xml | |
| (SX = **System.Xml**) | Método **SX.XmlConvert.ToDateTime** |
| (SX = **System.Xml**) | Método **SX.XmlConvert.ToDateTimeOffset** |


O próximo tópico é [Portando o projeto](wpsl-to-uwp-porting-to-a-uwp-project.md).

