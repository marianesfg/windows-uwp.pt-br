---
author: stevewhims
description: Este tópico fornece um mapeamento abrangente WindowsPhone das APIs do Silverlight para seus equivalentes da plataforma Universal do Windows (UWP).
title: WindowsPhone Silverlight para mapeamentos de namespace e classe UWP
ms.assetid: 33f06706-4790-48f3-a2e4-ebef9ddb61a4
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 54118b41fc1f3036dddba9a0cfb8ecd860c1e233
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6036936"
---
# <a name="windowsphone-silverlight-to-uwp-api-mappings"></a>WindowsPhone Silverlight para mapeamentos de API UWP


Este tópico fornece um mapeamento abrangente WindowsPhone das APIs do Silverlight para seus equivalentes da plataforma Universal do Windows (UWP). Geralmente não há um mapeamento de funcionalidade um para um: qualquer plataforma pode ter mais ou menos funcionalidades que suas equivalentes em um namespace ou classe.

A tabela de mapeamento ajudará você quando você estiver trabalhando em um projeto UWP e estiver reutilizando o código-fonte de um projeto do WindowsPhone Silverlight. Há diferenças nos nomes de namespaces e classes (inclusive controles de interface do usuário) entre as duas plataformas. Em muitos casos, isso é tão fácil quanto alterar um nome de namespace e, em seguida, o código será compilado. Às vezes, uma classe ou o nome da API mudou, bem como o nome do namespace. Em outras ocasiões, o mapeamento requer um pouco mais de trabalho e, em casos raros, requer uma mudança na abordagem.

**Como usar a tabela:** Primeiro, procure o nome da classe que você está usando. As classes estão listadas sempre que o mapeamento é mais complicado do que simplesmente alterar o nome do namespace. Caso a classe não esteja listada, o mapeamento é apenas uma alteração do namespace. Assim, encontre o nome do namespace da classe, e você encontrará o nome do namespace UWP equivalente. A classe estará nesse namespace. Caso o namespace não esteja listado é porque o nome não foi alterado.

**Observação**Windows 10 dá suporte a muito mais do .NET Framework do que um aplicativo da loja do Windows Phone. Por exemplo, o Windows 10 tem diversos namespaces System.ServiceModel.\*, bem como System.Net, System.NET. NetworkInformation e ServiceModel.
Além disso, em um aplicativo do Windows 10, você aproveitará .NET Native, que uma tecnologia de compilação ahead-of-time que converte MSIL em código de máquina executável nativamente. Aplicativos .NET Native iniciam com mais rapidez, usam menos memória e bateria do que seus equivalentes MSIL.

| WindowsPhone Silverlight | Windows Runtime |
| ------------------------- | --------------- |
| Publicidade | |
| Classe **Microsoft.Advertising.Mobile.UI.AdControl** | Classe [AdControl](http://msdn.microsoft.com/library/advertising-windows-sdk-api-reference-adcontrol.aspx) |
| Alarmes, lembretes e agentes de segundo plano | |
| Classe **Microsoft.Phone.BackgroundAgent** | Classe [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) |
| Namespace **Microsoft.Phone.Scheduler** | Namespace [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847) |
| Classe **Microsoft.Phone.Scheduler.Alarm** | Classes [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) e [**ToastNotificationManager**](https://msdn.microsoft.com/library/windows/apps/br208642) |
| Classes **Microsoft.Phone.Scheduler.PeriodicTask**, **ScheduledAction**, **ScheduledActionService**, **ScheduledTask**, **ScheduledTaskAgent** | Classe [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) |
| Classe **Microsoft.Phone.Scheduler.Reminder** | Classes [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) e [**ToastNotificationManager**](https://msdn.microsoft.com/library/windows/apps/br208642) |
| Classe **Microsoft.Phone.PictureDecoder** | Classe [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) |
| Namespace **Microsoft.Phone.BackgroundAudio** | Namespace [**Windows.Media.Playback**](https://msdn.microsoft.com/library/windows/apps/dn640562) |
| Namespace **Microsoft.Phone.BackgroundTransfer** | Namespace [**Windows.Networking.BackgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) |
| Modelo de aplicativo e ambiente | |
| Classe **System.AppDomain** | Nenhuma equivalência direta. Veja as classes [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324), [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016) |
| Classe **System.Environment** | Nenhuma equivalência direta |
| Classe **System.ComponentModel.Annotations**  | Nenhuma equivalência direta |
| Classe **System.ComponentModel.BackgroundWorker** | Classe [**ThreadPool**](https://msdn.microsoft.com/library/windows/apps/br229621) |
| Classe **System.ComponentModel.DesignerProperties** | Classe [**DesignMode**](https://msdn.microsoft.com/library/windows/apps/br224664) |
| Classes **System.Threading.Thread**, **System.Threading.ThreadPool** | Classe [**ThreadPool**](https://msdn.microsoft.com/library/windows/apps/br229621) |
| (ST = **System.Threading**) <br/> Método **ST.Thread.MemoryBarrier** | (ST = **System.Threading**) <br/> Método **ST.Interlocked.MemoryBarrier** |
| (ST = **System.Threading**) <br/> Propriedade **ST.Thread.ManagedThreadId** | (S = **System**) <br/> Propriedade **S.Environment.ManagedThreadId** |
| Classe **System.Threading.Timer** | Classe [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/br230587) |
| (SWT = **System.Windows.Threading**) <br/> Classe **SWT.Dispatcher** | Classe [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) |
| (SWT = **System.Windows.Threading**) <br/> Classe **SWT.DispatcherTimer** | Classe [**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250) |
| Blend for Visual Studio | |
| (MEDC = **Microsoft.Expression.Drawing.Core**) <br/> Classe **MEDC.GeometryHelper** | Nenhuma equivalência direta |
| Namespace **Microsoft.Expression.Interactivity** | Namespace [Microsoft.Xaml.Interactivity](http://go.microsoft.com/fwlink/p/?LinkId=328776) |
| Namespace **Microsoft.Expression.Interactivity.Core** | Namespace [Microsoft.Xaml.Interactions.Core](http://go.microsoft.com/fwlink/p/?LinkId=328773) |
| (MEIC = **Microsoft.Expression.Interactivity.Core**) <br/> Classe **MEIC.ExtendedVisualStateManager** | Nenhuma equivalência direta |
| Namespace **Microsoft.Expression.Interactivity.Input** | Nenhuma equivalência direta |
| Namespace **Microsoft.Expression.Interactivity.Media** | Namespace [Microsoft.Xaml.Interactions.Media](http://go.microsoft.com/fwlink/p/?LinkId=328775) |
| Namespace **Microsoft.Expression.Shapes** | Nenhuma equivalência direta |
| (MI = **Microsoft.Internal**) <br/> Interface **MI.IManagedFrameworkInternalHelper** | Nenhuma equivalência direta |
| Dados de contato e do calendário | |
| Namespace **Microsoft.Phone.UserData** | Namespaces [**Windows.ApplicationModel.Contacts**](https://msdn.microsoft.com/library/windows/apps/br225002), [**Windows.ApplicationModel.Appointments**](https://msdn.microsoft.com/library/windows/apps/dn263359) |
| (MPU = **Microsoft.Phone.UserData**) <br/> Classes **MPU.Account**, **ContactAddress**, **ContactCompanyInformation**, **ContactEmailAddress**, **ContactPhoneNumber** | Classe [**Contact**](https://msdn.microsoft.com/library/windows/apps/br224849) |
| (MPU = **Microsoft.Phone.UserData**) <br/> Classe **MPU.Appointments** | Classe [**AppointmentCalendar**](https://msdn.microsoft.com/library/windows/apps/dn596134) |
| (MPU = **Microsoft.Phone.UserData**) <br/> Classe **MPU.Contacts** | Classe [**ContactStore**](https://msdn.microsoft.com/library/windows/apps/dn624859) |
| Controles e infraestrutura de interface do usuário | |
| Classe **ControlTiltEffect.TiltEffect** | As animações da biblioteca de animação do Windows Runtime são incorporadas os estilos padrão dos controles comuns. Consulte [Animação](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Namespace **Microsoft.Phone.Controls** | Namespace [**Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227716) |
| (MPC = **Microsoft.Phone.Controls**) <br/> Classe **MPC.ContextMenu** | Classe [**PopupMenu**](https://msdn.microsoft.com/library/windows/apps/br208693) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.DatePickerPage** | Classe [**DatePickerFlyout**](https://msdn.microsoft.com/library/windows/apps/dn625013) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.GestureListener** | Classe [**GestureRecognizer**](https://msdn.microsoft.com/library/windows/apps/br241937) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.LongListSelector** | Classe [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.ObscuredEventArgs** | Classes [**SystemProtection**](https://msdn.microsoft.com/library/windows/apps/jj585394), [**WindowActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br208377) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.Panorama** | Classe [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843) |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationFrame**,<br/>(SWN = **System.Windows.Navigation**) <br/>Classes **SWN.NavigationService** | Classe [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.PhoneApplicationPage** | Classe [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.TiltEffect** | Classe [**PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969164) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.TimePickerPage** | Classe [**TimePickerFlyout**](https://msdn.microsoft.com/library/windows/apps/dn608313) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.WebBrowser** | Classe [**WebView**](https://msdn.microsoft.com/library/windows/apps/br227702) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.WebBrowserExtensions** | Nenhuma equivalência direta |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.WrapPanel** | Não há equivalente direto para fins de layout geral. [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/dn298849) e [**WrapGrid**](https://msdn.microsoft.com/library/windows/apps/br227717) podem ser usados no modelo de painel de itens de um controle de itens. |
| (MPD = **Microsoft.Phone.Data**) <br/>Namespace **MPD.Linq** | Nenhuma equivalência direta |
| (MPD = **Microsoft.Phone.Data**) <br/>Namespace **MPD.Linq.Mapping** | Nenhuma equivalência direta |
| Namespace **Microsoft.Phone.Globalization** | Nenhuma equivalência direta |
| (MPI = **Microsoft.Phone.Info**) <br/>Classes **MPI.DeviceExtendedProperties**, **DeviceStatus** | Classes [**EasClientDeviceInformation**](https://msdn.microsoft.com/library/windows/apps/hh701390), [**MemoryManager**](https://msdn.microsoft.com/library/windows/apps/dn633831). Para obter mais detalhes, consulte [Status do dispositivo](wpsl-to-uwp-input-and-sensors.md). |
| (MPI = **Microsoft.Phone.Info**) <br/>Classe **MPI.MediaCapabilities** | Nenhuma equivalência direta |
| (MPI = **Microsoft.Phone.Info**) <br/>Classe **MPI.UserExtendedProperties** | Classe [**AdvertisingManager**](https://msdn.microsoft.com/library/windows/apps/dn363391) |
| Namespace **System.Windows** | Namespace [**Windows.UI.Xaml**](https://msdn.microsoft.com/library/windows/apps/br209045) |
| Namespace **System.Windows.Automation** | Namespace [**Windows.UI.Xaml.Automation**](https://msdn.microsoft.com/library/windows/apps/br209179) |
| Namespaces **System.Windows.Controls**, **System.Windows.Input** | Namespaces [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383), [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084), [**Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227716) |
| Classes **System.Windows.Controls.DrawingSurface**, **DrawingSurfaceBackgroundGrid** | Classe [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) |
| Classe **System.Windows.Controls.RichTextBox** | Classe [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/br227548) |
| Classe **System.Windows.Controls.WrapPanel** | Não há equivalente direto para fins de layout geral. [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/dn298849) e [**WrapGrid**](https://msdn.microsoft.com/library/windows/apps/br227717) podem ser usados no modelo de painel de itens de um controle de itens. |
| Namespace **System.Windows.Controls.Primitives** | Namespace [**Windows.UI.Xaml.Controls.Primitives**](https://msdn.microsoft.com/library/windows/apps/br209818) |
| Namespace **System.Windows.Controls.Shapes** | Namespace [**Windows.UI.Xaml.Controls.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) |
| Namespace **System.Windows.Data** | Namespace [**Windows.UI.Xaml.Data**](https://msdn.microsoft.com/library/windows/apps/br209917) |
| Namespace **System.Windows.Documents** | Namespace [**Windows.UI.Xaml.Documents**](https://msdn.microsoft.com/library/windows/apps/br209984) |
| Namespace **System.Windows.Ink** | Nenhuma equivalência direta |
| Namespace **System.Windows.Markup** | Namespace [**Windows.UI.Xaml.Markup**](https://msdn.microsoft.com/library/windows/apps/br228046) | 
| Namespace **System.Windows.Navigation** | Namespace [**Windows.UI.Xaml.Navigation**](https://msdn.microsoft.com/library/windows/apps/br243300) |
| Evento **System.Windows.UIElement.Tap**, representante **EventHandler&lt;GestureEventArgs&gt;** | Evento [**Tapped**](https://msdn.microsoft.com/library/windows/apps/br208985), representante[**TappedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br227993) |
| Dados e serviços |  |
| Classe **System.Data.Linq.DataContext** | Nenhuma equivalência direta |
| Classe **System.Data.Linq.Mapping.ColumnAttribute** | Nenhuma equivalência direta |
| Classe **System.Data.Linq.SqlClient.SqlHelpers** | Nenhuma equivalência direta |
| Dispositivos | |
| Namespaces **Microsoft.Devices**, **Microsoft.Devices.Sensors** | Namespaces [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459), [**Windows.Devices.Enumeration.Pnp**](https://msdn.microsoft.com/library/windows/apps/br225517), [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648), [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/br206408) |
| Classes **Microsoft.Devices.Camera**, **Microsoft.Devices.PhotoCamera** | Classe [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124). Além disso, a classe [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) class (somente Windows). |
| Classe **Microsoft.Devices.CameraButtons** | Classe [**HardwareButtons**](https://msdn.microsoft.com/library/windows/apps/jj207557) |
| Classe **Microsoft.Devices.CameraVideoBrushExtensions** | Classe [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) |
| Classe **Microsoft.Devices.Environment** | Nenhuma equivalência direta. Como alternativa, use a compilação condicional e defina um símbolo personalizado. Ou você pode ser capaz de criar uma solução alternativa usando a propriedade [IsAttached](http://msdn.microsoft.com/library/e299w87h.aspx). |
| Classe **Microsoft.Devices.MediaHistory** | Nenhuma equivalência direta |
| Classe **Microsoft.Devices.VibrateController** | Classe [**VibrationDevice**](https://msdn.microsoft.com/library/windows/apps/jj207230) |
| Classe **Microsoft.Devices.Radio.FMRadio** | Nenhuma equivalência direta |
| Classes **Microsoft.Devices.Sensors.Accelerometer**, **Compass** | No namespace [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/br206408) |
| Classe **Microsoft.Devices.Sensors.Gyroscope** | Classe [**Gyrometer**](https://msdn.microsoft.com/library/windows/apps/br225718) |
| Classe **Microsoft.Devices.Sensors.Motion** | Classe [**Inclinometer**](https://msdn.microsoft.com/library/windows/apps/br225766) |
| Globalização | |
| Namespace **System.Globalization** | Namespace [**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) |
| (ST = **System.Threading**) <br/> Propriedade **ST.Thread.CurrentCulture** | (SG = **System.Globalization**) <br/> Propriedade **S.CultureInfo.CurrentCulture** |
| (ST = **System.Threading**) <br/> Propriedade **ST.Thread.CurrentUICulture** | (SG = **System.Globalization**) <br/> Propriedade **S.CultureInfo.CurrentUICulture** |
| Elementos gráficos e animação | |
| Namespaces **Microsoft.Xna.Framework.\***, [XNA Framework Class Library](http://go.microsoft.com/fwlink/p/?LinkId=263769), [Content Pipeline Class Library](http://go.microsoft.com/fwlink/p/?LinkId=263770) | Nenhuma equivalência direta. Em geral, use [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) com C++. Consulte [Desenvolvendo jogos](https://msdn.microsoft.com/library/windows/apps/hh452744) e [Interoperabilidade entre DirectX e XAML](https://msdn.microsoft.com/library/windows/apps/hh825871). |
| Classe **Microsoft.Xna.Framework.Audio.Microphone** | Classe [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) |
| Classe **Microsoft.Xna.Framework.Audio.SoundEffect** | Classe [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) |
| Namespace **Microsoft.Xna.Framework.GamerServices** | (WPS = **Windows.Phone.System**) <br/> Namespace [**WPS.UserProfile.GameServices.Core**](https://msdn.microsoft.com/library/windows/apps/jj207609) |
| Classe **Microsoft.Xna.Framework.GamerServices.Guide** | Nenhuma equivalência direta |
| Classe **Microsoft.Xna.Framework.Input.GamePad** | Classe [**HardwareButtons**](https://msdn.microsoft.com/library/windows/apps/jj207557) |
| Classe **Microsoft.Xna.Framework.Input.Touch.TouchPanel** | Classe [**GestureRecognizer**](https://msdn.microsoft.com/library/windows/apps/br241937) |
| (MXFM = **Microsoft.Xna.Framework.Media**) <br/> Classes **MXFM.MediaLibrary**, **MXFM.PhoneExtensions.MediaLibraryExtensions** | Classe [**KnownFolders**](https://msdn.microsoft.com/library/windows/apps/br227151) |
| Classe **Microsoft.Xna.Framework.Media.MediaQueue** | Classe [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) |
| Classe **Microsoft.Xna.Framework.Media.Playlist** | Classe [**BackgroundMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/dn652527) |
| Namespace **System.Windows.Media** | Namespace [**Windows.UI.Xaml.Media**](/uwp/api/Windows.UI.Xaml.Media) |
| Classe **System.Windows.Media.RadialGradientBrush** | Nenhuma equivalência direta. Consulte [Mídia e elementos gráficos](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Namespace **System.Windows.Media.Animation** | Namespace [**Windows.UI.Xaml.Media.Animation**](https://msdn.microsoft.com/library/windows/apps/br243232) |
| Namespace **System.Windows.Media.Effects** | Nenhuma equivalência direta |
| Namespace **System.Windows.Media.Imaging** | Namespace [**Windows.UI.Xaml.Media.Imaging**](https://msdn.microsoft.com/library/windows/apps/br243258) |
| Namespace **System.Windows.Media.Media3D** | Namespace [**Windows.UI.Xaml.Media.Media3D**](https://msdn.microsoft.com/library/windows/apps/br243274) |
| Namespace **System.Windows.Shapes** | Namespace [**Windows.UI.Xaml.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) |
| Iniciadores e seletores | |
| Classes **Microsoft.Phone.Tasks.AddressChooserTask**, **EmailAddressChooserTask**, **PhoneNumberChooserTask** | Classe [**ContactPicker**](https://msdn.microsoft.com/library/windows/apps/br224913) |
| Classes **Microsoft.Phone.Tasks.AddWalletItemTask**, **AddWalletItemResult** | Namespace [**Windows.ApplicationModel.Wallet**](https://msdn.microsoft.com/library/windows/apps/dn631399) |
| Classes **Microsoft.Phone.Tasks.BingMapsDirectionsTask**, **BingMapsTask** | Nenhuma equivalência direta |
| Classe **Microsoft.Phone.Tasks.CameraCaptureTask** | Classe [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124). Além disso, a classe [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) class (somente Windows). |
| **Microsoft.Phone.Tasks.MarketplaceDetailTask** | Classe [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) (método [**RequestAppPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/hh967813)) |
| Classes **Microsoft.Phone.Tasks.ConnectionSettingsTask**, **MarketplaceHubTask**, **MarketplaceReviewTask**, **MarketplaceSearchTask**, **MediaPlayerLauncher**, **SearchTask**, **SmsComposeTask**, **WebBrowserTask** | Classe [**Launcher**](https://msdn.microsoft.com/library/windows/apps/br241801) |
| Classe **Microsoft.Phone.Tasks.EmailComposeTask** | Classe [**EmailMessage**](https://msdn.microsoft.com/library/windows/apps/dn631270) |
| Classe **Microsoft.Phone.Tasks.GameInviteTask** | Nenhuma equivalência direta |
| Classes **Microsoft.Phone.Tasks.MapDownloaderTask**, **MapsDirectionsTask**, **MapsTask**, **MapUpdaterTask** | Nenhuma equivalência direta |
| Classe **Microsoft.Phone.Tasks.PhoneCallTask** | Classe [**PhoneCallManager**](https://msdn.microsoft.com/library/windows/apps/dn624832) |
| Classe **Microsoft.Phone.Tasks.PhotoChooserTask** | Classe [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) |
| Classe **Microsoft.Phone.Tasks.SaveAppointmentTask** | Classe [**AppointmentManager**](https://msdn.microsoft.com/library/windows/apps/dn297254) |
| Classes **Microsoft.Phone.Tasks.SaveContactTask**, **SaveEmailAddressTask**, **SavePhoneNumberTask** | Classe [**StoredContact**](https://msdn.microsoft.com/library/windows/apps/jj207727) (somente Windows Phone) | 
| Classe **Microsoft.Phone.Tasks.SaveRingtoneTask** | Nenhuma equivalência direta |
| Classes **Microsoft.Phone.Tasks.ShareLinkTask**, **ShareMediaTask**, **ShareStatusTask** | Classe [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/br205873) |
| Localização | |
| Namespace **System.Device.Location** | Namespace [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603) |
| Classe **System.Device.GeoCoordinateWatcher** | Classe [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) |
| Mapas | |
| Namespaces **Microsoft.Phone.Maps** | Namespace [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) |
| Namespace **Microsoft.Phone.Maps.Controls** | Namespace [**Windows.UI.Xaml.Controls.Maps**](https://msdn.microsoft.com/library/windows/apps/dn610751) |
| Classe **Microsoft.Phone.Maps.Controls.Map** | Classe [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) |
| Namespace **Microsoft.Phone.Maps.Services** | Namespace [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) |
| Classes **Microsoft.Phone.Maps.Services.GeocodeQuery**, **ReverseGeocodeQuery** | Classe [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) |
| Classe **System.Device.Location.GeoCoordinate** | Classe [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675) |
| Classe **Microsoft.Phone.Maps.Services.Route** | Classe [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) |
| Classe **Microsoft.Phone.Maps.Services.RouteQuery** | Classe [**MapRouteFinder**](https://msdn.microsoft.com/library/windows/apps/dn636938) |
| Monetização | |
| Namespace **Microsoft.Phone.Marketplace** | Namespace [**Windows.ApplicationModel.Store**](https://msdn.microsoft.com/library/windows/apps/br225197) |
| Mídia | |
| Namespace **Microsoft.Phone.Media** | Classe [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) |
| Rede | |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.DeviceNetworkInformation** | Classes [**Hostname**](https://msdn.microsoft.com/library/windows/apps/br207113), [**NetworkInformation**](https://msdn.microsoft.com/library/windows/apps/br207293) |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.NetworkInterface** | Classe [**NetworkInformation**](https://msdn.microsoft.com/library/windows/apps/br207293) |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.NetworkInterfaceInfo** | Classe [**ConnectionProfile**](https://msdn.microsoft.com/library/windows/apps/br207249) |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.NetworkInterfaceList** | Classe [**NetworkInformation**](https://msdn.microsoft.com/library/windows/apps/br207293) |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.SocketExtensions** | Nenhuma equivalência direta |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.WebRequestExtensions** | Nenhuma equivalência direta |
| Namespace **Microsoft.Phone.Networking.Voip** | Nenhuma equivalência direta |
| Classe **System.Net.CookieCollection** | Ainda há suporte, mas algumas propriedades estão ausentes (por exemplo, IsReadOnly) |
| Classe **System.Net.DownloadProgressChangedEventArgs** e classes semelhantes relacionadas a **System.Net.WebClient** | Classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Derivar de [System.Net.Http.StreamContent](https://msdn.microsoft.com/library/system.net.http.streamcontent.aspx) para medir o progresso. |
| Classes **System.Net.DnsEndPoint**, **IPAddress** | Essas classes ainda têm suporte, mas algumas propriedades estão ausentes. Como alternativa, porte para a classe [**HostName**](https://msdn.microsoft.com/library/windows/apps/br207113). |
| Classe **System.Net.HttpUtility** | Classe [**HtmlFormatHelper**](https://msdn.microsoft.com/library/windows/apps/hh738437) |
| Classe **System.Net.HttpWebRequest** | Suporte parcial, mas a alternativa recomendada e prospectiva é a classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Essas APIs usam [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx) para representar uma solicitação HTTP. |
| Classe **System.Net.HttpWebResponse** | Ainda compatível, mas use Dispose() em vez de Close(). Porém, a alternativa futura, recomendada, é a classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Essas APIs usam [System.Net.Http.HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx) para representar uma resposta HTTP. |
| (SNN = **System.Net.NetworkInformation**) <br/> Classe **SNN.NetworkChange** | Ainda há suporte, exceto para o construtor. |
| Classe **System.Net.OpenReadCompletedEventArgs** e classes semelhantes relacionadas a **System.Net.WebClient** | Classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| Classe **System.Net.Sockets.Socket** | Ainda compatível, mas use Dispose() em vez de Close(). Como alternativa, porte para a classe [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882). |
| Classe **System.Net.Sockets.SocketException** | Ainda há suporte, mas use a propriedade SocketErrorCode em vez de ErrorCode. |
| Classes **System.Net.Sockets.UdpAnySourceMulticastClient**, **UdpSingleSourceMulticastClient** | Classe [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) |
| Classe **System.Net.UploadProgressChangedEventArgs** e classes semelhantes relacionadas a **System.Net.WebClient** | Classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| Classe **System.Net.WebClient** | Classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| Classe **System.Net.WebRequest** | Suporte parcial (um conjunto de propriedades diferente), mas a alternativa recomendada e prospectiva é a classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Essas APIs usam [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx) para representar uma solicitação HTTP. |
| Classe **System.Net.WebResponse** | Ainda compatível, mas use Dispose() em vez de Close(). Porém, a alternativa futura, recomendada, é a classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Essas APIs usam [System.Net.Http.HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx) para representar uma resposta HTTP. |
| (SN = **System.Net**) <br/> Classe **SN.WriteStreamClosedEventArgs** | Classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| (SN = **System.Net**) <br/> Classe **SN.WriteStreamClosedEventHandler** | Classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| Classe **System.UriFormatException** | Classe **System.FormatException** |
| Notificações | |
| Namespace MPN = **Microsoft.Phone.Notification** | Namespaces [**Windows.UI.Notifications**](https://msdn.microsoft.com/library/windows/apps/br208661), [**Windows.Networking.PushNotifications**](https://msdn.microsoft.com/library/windows/apps/br241307) |
| MPN = **Microsoft.Phone.Notification** <br/> Classe **MPN.HttpNotification** | Classe [**TileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616) |
| MPN = **Microsoft.Phone.Notification** <br/> Classe **MPN.HttpNotificationChannel** | Classe [**PushNotificationChannel**](https://msdn.microsoft.com/library/windows/apps/br241283) |
| Programação | |
| Namespace **System** | Namespace [**Windows.Foundation**](https://msdn.microsoft.com/library/windows/apps/br226021) |
| Classes **System.Diagnostics.StackFrame**, **StackTrace** | Nenhuma equivalência direta |
| Namespace **System.Diagnostics** | Namespace [**Windows.Foundation.Diagnostics**](https://msdn.microsoft.com/library/windows/apps/br206677) |
| Interface **System.ICloneable** | Um método personalizado que retorna o tipo adequado. |
| Classe **System.Reflection.Emit.ILGenerator** | Nenhuma equivalência direta |
| Extensões reativas | |
| Namespace **Microsoft.Phone.Reactive** | Nenhuma equivalência direta |
| Reflexão | |
| Classe **System.Type** | Classe **System.Reflection.TypeInfo**. Consulte [Reflexão no .NET Framework para aplicativos UWP](https://msdn.microsoft.com/library/hh535795.aspx). |
| Recursos | |
| Classe **System.Resources.ResourceManager** | (WA = **Windows.ApplicationModel**)<br/>Namespaces [**WA.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039) e [**WA.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022), classe [**ResourceManager**](https://msdn.microsoft.com/library/windows/apps/br206078). Consulte [Criando e recuperando recursos em aplicativos do Windows Runtime](https://msdn.microsoft.com/library/windows/apps/xaml/hh694557.aspx). |
| Elemento seguro | |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> Classes **MPS.SecureElementChannel**, **MPS.SecureElementSession** | Classe [**SmartCardConnection**](https://msdn.microsoft.com/library/windows/apps/dn608002) |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> Classe **MPS.SecureElementReader** | Classe [**SmartCardReader**](https://msdn.microsoft.com/library/windows/apps/dn263857) |
| Segurança | |
| (SSC = **System.Security.Cryptography**) <br/> Classes **SSC.Aes**, **SSC.RSA** | Classe [**CryptographicEngine**](https://msdn.microsoft.com/library/windows/apps/br241490) |
| (SSC = **System.Security.Cryptography**) <br/> Classes **SSC.HMACSHA256**, **SSC.SHA256** | Classe [**HashAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241511) |
| (SSC = **System.Security.Cryptography**) <br/> Classe **SSC.ProtectedData** | Classe [**DataProtectionProvider**](https://msdn.microsoft.com/library/windows/apps/br241559) |
| (SSC = **System.Security.Cryptography**) <br/> Classe **SSC.RandomNumberGenerator** | Classe [**CryptographicBuffer**](https://msdn.microsoft.com/library/windows/apps/br227092) |
| (SSC = **System.Security.Cryptography**) <br/> Classe **SSC.X509Certificates.X509Certificate** | Classe [**CertificateEnrollmentManager**](https://msdn.microsoft.com/library/windows/apps/br212075) |
| Shell | |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ApplicationBar** | Classe [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/dn279427) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ApplicationBarIconButton** | Classe [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244) (quando usada na propriedade[**PrimaryCommands**](https://msdn.microsoft.com/library/windows/apps/dn279430)) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ApplicationBarMenuItem** | Classe [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244) (quando usada na propriedade [**SecondaryCommands**](https://msdn.microsoft.com/library/windows/apps/dn279434)) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classes **MPSh.CycleTileData**, **MPSh.FlipTileData**, **MPSh.IconicTileData**, **MPSh.ShellTileData**, **MPSh.StandardTileData** | Classe [**TileTemplateType**](https://msdn.microsoft.com/library/windows/apps/br208621) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.PhoneApplicationService** | Classes [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016), [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ProgressIndicator** | Classe [**StatusBarProgressIndicator**](https://msdn.microsoft.com/library/windows/apps/dn633865) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ShellTile** | Classe [**SecondaryTile**](https://msdn.microsoft.com/library/windows/apps/br242183) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ShellTileSchedule** | Classe [**TileUpdater**](https://msdn.microsoft.com/library/windows/apps/br208628) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ShellToast** | Classe [**ToastNotificationManager**](https://msdn.microsoft.com/library/windows/apps/br208642) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.SystemTray** | Classe [**StatusBar**](https://msdn.microsoft.com/library/windows/apps/dn633864) |
| Armazenamento e E/S | |
| Classes **Microsoft.Phone.Storage.ExternalStorage**, **ExternalStorageDevice**, **ExternalStorageFile**, **ExternalStorageFolder** | Classe [**KnownFolders**](https://msdn.microsoft.com/library/windows/apps/br227151) |
| Namespace **System.IO** | Namespaces [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346), [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/br241791) |
| Classe **System.IO.Directory** | Classe [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) |
| Classe **System.IO.File** | Classes [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) e [**PathIO**](https://msdn.microsoft.com/library/windows/apps/hh701663)
| (SII = **System.IO.IsolatedStorage**) <br/> Classe **SII.IsolatedStorageFile** |Propriedade [**ApplicationData.LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) |
| (SII = **System.IO.IsolatedStorage**) <br/> Classe **SII.IsolatedStorageSettings** | Propriedade [**ApplicationData.LocalSettings**](https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.localsettings.aspx) |
| Classe **System.IO.Stream** | Ainda há suporte, mas use ReadAsync() e WriteAsync() em vez de BeginRead()/EndRead() e BeginWrite()/EndWrite(). |
| Carteira | |
| Namespace **Microsoft.Phone.Wallet** | Namespace [**Windows.ApplicationModel.Wallet**](https://msdn.microsoft.com/library/windows/apps/dn631399) |
| Xml | |
| (SX = **System.Xml**) | Método **SX.XmlConvert.ToDateTime** |
| (SX = **System.Xml**) | Método **SX.XmlConvert.ToDateTimeOffset** |


O próximo tópico é [Portando o projeto](wpsl-to-uwp-porting-to-a-uwp-project.md).

