---
description: This topic provides a comprehensive mapping of Windows Phone Silverlight APIs to their Universal Windows Platform (UWP) equivalents.
title: Windows Phone Silverlight to UWP namespace and class mappings
ms.assetid: 33f06706-4790-48f3-a2e4-ebef9ddb61a4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1af36b43e02c9ee08373098f57ba29c10badb6c8
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259064"
---
# <a name="windowsphone-silverlight-to-uwp-api-mappings"></a>Windows Phone Silverlight to UWP API mappings


This topic provides a comprehensive mapping of Windows Phone Silverlight APIs to their Universal Windows Platform (UWP) equivalents. Geralmente não há um mapeamento de funcionalidade um para um: qualquer plataforma pode ter mais ou menos funcionalidades que suas equivalentes em um namespace ou classe.

The mapping table will help you when you're working in a UWP project and you're re-using source code from a Windows Phone Silverlight project. Há diferenças nos nomes de namespaces e classes (inclusive controles de interface do usuário) entre as duas plataformas. Em muitos casos, isso é tão fácil quanto alterar um nome de namespace e, em seguida, o código será compilado. Às vezes, uma classe ou o nome da API mudou, bem como o nome do namespace. Em outras ocasiões, o mapeamento requer um pouco mais de trabalho e, em casos raros, requer uma mudança na abordagem.

**How to use the table:  ** First, search for the name of the class you're using. As classes estão listadas sempre que o mapeamento é mais complicado do que simplesmente alterar o nome do namespace. Caso a classe não esteja listada, o mapeamento é apenas uma alteração do namespace. Assim, encontre o nome do namespace da classe, e você encontrará o nome do namespace UWP equivalente. A classe estará nesse namespace. Caso o namespace não esteja listado é porque o nome não foi alterado.

**Note**  Windows 10 supports much more of the .NET Framework than a Windows Phone Store app does. For example, Windows 10 has several System.ServiceModel.\* namespaces as well as System.Net, System.Net.NetworkInformation, and System.Net.Sockets.
Also, in a Windows 10 app, you will benefit from .NET Native, which an ahead-of-time compilation technology that converts MSIL into natively-runnable machine code. Aplicativos .NET Native iniciam com mais rapidez, usam menos memória e bateria do que seus equivalentes MSIL.

| Windows Phone Silverlight | Windows Runtime |
| ------------------------- | --------------- |
| Publicidade | |
| Classe **Microsoft.Advertising.Mobile.UI.AdControl** | Classe [AdControl](https://docs.microsoft.com/windows/uwp/monetize/display-ads-using-the-microsoft-advertising-libraries) |
| Alarmes, lembretes e agentes de segundo plano | |
| Classe **Microsoft.Phone.BackgroundAgent** | [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) class |
| Namespace **Microsoft.Phone.Scheduler** | [**Windows.ApplicationModel.Background**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background) namespace |
| Classe **Microsoft.Phone.Scheduler.Alarm** | [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) and [**ToastNotificationManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) classes |
| Classes **Microsoft.Phone.Scheduler.PeriodicTask**, **ScheduledAction**, **ScheduledActionService**, **ScheduledTask**, **ScheduledTaskAgent** | [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) class |
| Classe **Microsoft.Phone.Scheduler.Reminder** | [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) and [**ToastNotificationManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) classes |
| Classe **Microsoft.Phone.PictureDecoder** | [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) class |
| Namespace **Microsoft.Phone.BackgroundAudio** | [**Windows.Media.Playback**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback) namespace |
| Namespace **Microsoft.Phone.BackgroundTransfer** | [**Windows.Networking.BackgroundTransfer**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer) namespace |
| Modelo de aplicativo e ambiente | |
| Classe **System.AppDomain** | Nenhuma equivalência direta. Veja as classes [**Application**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Application), [**CoreApplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication) |
| Classe **System.Environment** | Nenhuma equivalência direta |
| Classe **System.ComponentModel.Annotations**  | Nenhuma equivalência direta |
| Classe **System.ComponentModel.BackgroundWorker** | [**ThreadPool**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPool) class |
| Classe **System.ComponentModel.DesignerProperties** | [**DesignMode**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DesignMode) class |
| Classes **System.Threading.Thread**, **System.Threading.ThreadPool** | [**ThreadPool**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPool) class |
| (ST = **System.Threading**) <br/> Método **ST.Thread.MemoryBarrier** | (ST = **System.Threading**) <br/> Método **ST.Interlocked.MemoryBarrier** |
| (ST = **System.Threading**) <br/> Propriedade **ST.Thread.ManagedThreadId** | (S = **System**) <br/> Propriedade **S.Environment.ManagedThreadId** |
| Classe **System.Threading.Timer** | [**ThreadPoolTimer**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPoolTimer) class |
| (SWT = **System.Windows.Threading**) <br/> Classe **SWT.Dispatcher** | [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) class |
| (SWT = **System.Windows.Threading**) <br/> Classe **SWT.DispatcherTimer** | [**DispatcherTimer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DispatcherTimer) class |
| Blend for Visual Studio | |
| (MEDC = **Microsoft.Expression.Drawing.Core**) <br/> Classe **MEDC.GeometryHelper** | Nenhuma equivalência direta |
| Namespace **Microsoft.Expression.Interactivity** | Namespace [Microsoft.Xaml.Interactivity](https://msdn.microsoft.com/library/windows/apps/microsoft.xaml.interactivity.aspx) |
| Namespace **Microsoft.Expression.Interactivity.Core** | Namespace [Microsoft.Xaml.Interactions.Core](https://msdn.microsoft.com/library/windows/apps/microsoft.xaml.interactions.core.aspx) |
| (MEIC = **Microsoft.Expression.Interactivity.Core**) <br/> Classe **MEIC.ExtendedVisualStateManager** | Nenhuma equivalência direta |
| Namespace **Microsoft.Expression.Interactivity.Input** | Nenhuma equivalência direta |
| Namespace **Microsoft.Expression.Interactivity.Media** | Namespace [Microsoft.Xaml.Interactions.Media](https://msdn.microsoft.com/library/windows/apps/microsoft.xaml.interactions.media.aspx) |
| Namespace **Microsoft.Expression.Shapes** | Nenhuma equivalência direta |
| (MI = **Microsoft.Internal**) <br/> Interface **MI.IManagedFrameworkInternalHelper** | Nenhuma equivalência direta |
| Dados de contato e do calendário | |
| Namespace **Microsoft.Phone.UserData** | [**Windows.ApplicationModel.Contacts**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts), [**Windows.ApplicationModel.Appointments**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments) namespaces |
| (MPU = **Microsoft.Phone.UserData**) <br/> Classes **MPU.Account**, **ContactAddress**, **ContactCompanyInformation**, **ContactEmailAddress**, **ContactPhoneNumber** | [**Contact**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact) class |
| (MPU = **Microsoft.Phone.UserData**) <br/> Classe **MPU.Appointments** | [**AppointmentCalendar**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments.AppointmentCalendar) class |
| (MPU = **Microsoft.Phone.UserData**) <br/> Classe **MPU.Contacts** | [**ContactStore**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.ContactStore) class |
| Controles e infraestrutura de interface do usuário | |
| Classe **ControlTiltEffect.TiltEffect** | As animações da biblioteca de animação do Windows Runtime são incorporadas os estilos padrão dos controles comuns. Consulte [Animação](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Namespace **Microsoft.Phone.Controls** | [**Windows.UI.Xaml.Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls) namespace |
| (MPC = **Microsoft.Phone.Controls**) <br/> Classe **MPC.ContextMenu** | [**PopupMenu**](https://docs.microsoft.com/uwp/api/Windows.UI.Popups.PopupMenu) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.DatePickerPage** | [**DatePickerFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePickerFlyout) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.GestureListener** | [**GestureRecognizer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.GestureRecognizer) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.LongListSelector** | [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.ObscuredEventArgs** | [**SystemProtection**](https://docs.microsoft.com/uwp/api/Windows.Phone.System.SystemProtection), [**WindowActivatedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.WindowActivatedEventArgs) classes |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.Panorama** | [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationFrame**,<br/>(SWN = **System.Windows.Navigation**) <br/>Classes **SWN.NavigationService** | [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.PhoneApplicationPage** | [**Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.TiltEffect** | [**PointerDownThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.TimePickerPage** | [**TimePickerFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePickerFlyout) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.WebBrowser** | [**WebView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.WebBrowserExtensions** | Nenhuma equivalência direta |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.WrapPanel** | Não há equivalente direto para fins de layout geral. [**ItemsWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) and [**WrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) can be used in the items panel template of an items control. |
| (MPD = **Microsoft.Phone.Data**) <br/>Namespace **MPD.Linq** | Nenhuma equivalência direta |
| (MPD = **Microsoft.Phone.Data**) <br/>Namespace **MPD.Linq.Mapping** | Nenhuma equivalência direta |
| Namespace **Microsoft.Phone.Globalization** | Nenhuma equivalência direta |
| (MPI = **Microsoft.Phone.Info**) <br/>Classes **MPI.DeviceExtendedProperties**, **DeviceStatus** | [**EasClientDeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Security.ExchangeActiveSyncProvisioning.EasClientDeviceInformation), [**MemoryManager**](https://docs.microsoft.com/uwp/api/Windows.System.MemoryManager) classes. Para obter mais detalhes, consulte [Status do dispositivo](wpsl-to-uwp-input-and-sensors.md). |
| (MPI = **Microsoft.Phone.Info**) <br/>Classe **MPI.MediaCapabilities** | Nenhuma equivalência direta |
| (MPI = **Microsoft.Phone.Info**) <br/>Classe **MPI.UserExtendedProperties** | [**AdvertisingManager**](https://docs.microsoft.com/uwp/api/Windows.System.UserProfile.AdvertisingManager) class |
| Namespace **System.Windows** | [**Windows.UI.Xaml**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml) namespace |
| Namespace **System.Windows.Automation** | [**Windows.UI.Xaml.Automation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation) namespace |
| Namespaces **System.Windows.Controls**, **System.Windows.Input** | [**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core), [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows.UI.Xaml.Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls) namespaces |
| Classes **System.Windows.Controls.DrawingSurface**, **DrawingSurfaceBackgroundGrid** | [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) class |
| Classe **System.Windows.Controls.RichTextBox** | [**RichEditBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) class |
| Classe **System.Windows.Controls.WrapPanel** | Não há equivalente direto para fins de layout geral. [**ItemsWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) and [**WrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) can be used in the items panel template of an items control. |
| Namespace **System.Windows.Controls.Primitives** | [**Windows.UI.Xaml.Controls.Primitives**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives) namespace |
| Namespace **System.Windows.Controls.Shapes** | [**Windows.UI.Xaml.Controls.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) namespace |
| Namespace **System.Windows.Data** | [**Windows.UI.Xaml.Data**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data) namespace |
| Namespace **System.Windows.Documents** | [**Windows.UI.Xaml.Documents**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents) namespace |
| Namespace **System.Windows.Ink** | Nenhuma equivalência direta |
| Namespace **System.Windows.Markup** | [**Windows.UI.Xaml.Markup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Markup) namespace | 
| Namespace **System.Windows.Navigation** | [**Windows.UI.Xaml.Navigation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Navigation) namespace |
| Evento **System.Windows.UIElement.Tap**, representante **EventHandler&lt;GestureEventArgs&gt;** | [**Tapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.tapped) event, [**TappedEventHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.tappedeventhandler) delegate |
| Dados e serviços |  |
| Classe **System.Data.Linq.DataContext** | Nenhuma equivalência direta |
| Classe **System.Data.Linq.Mapping.ColumnAttribute** | Nenhuma equivalência direta |
| Classe **System.Data.Linq.SqlClient.SqlHelpers** | Nenhuma equivalência direta |
| Dispositivos | |
| Namespaces **Microsoft.Devices**, **Microsoft.Devices.Sensors** | [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration), [**Windows.Devices.Enumeration.Pnp**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.Pnp), [**Windows.Devices.Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input), [**Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors) namespaces |
| Classes **Microsoft.Devices.Camera**, **Microsoft.Devices.PhotoCamera** | [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) class. Além disso, a classe [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) class (somente Windows). |
| Classe **Microsoft.Devices.CameraButtons** | [**HardwareButtons**](https://docs.microsoft.com/uwp/api/Windows.Phone.UI.Input.HardwareButtons) class |
| Classe **Microsoft.Devices.CameraVideoBrushExtensions** | [**CaptureElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) class |
| Classe **Microsoft.Devices.Environment** | Nenhuma equivalência direta. Como alternativa, use a compilação condicional e defina um símbolo personalizado. Ou você pode ser capaz de criar uma solução alternativa usando a propriedade [IsAttached](https://docs.microsoft.com/dotnet/api/system.diagnostics.debugger.isattached#System_Diagnostics_Debugger_IsAttached). |
| Classe **Microsoft.Devices.MediaHistory** | Nenhuma equivalência direta |
| Classe **Microsoft.Devices.VibrateController** | [**VibrationDevice**](https://docs.microsoft.com/uwp/api/Windows.Phone.Devices.Notification.VibrationDevice) class |
| Classe **Microsoft.Devices.Radio.FMRadio** | Nenhuma equivalência direta |
| Classes **Microsoft.Devices.Sensors.Accelerometer**, **Compass** | No namespace [**Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors) |
| Classe **Microsoft.Devices.Sensors.Gyroscope** | [**Gyrometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Gyrometer) class |
| Classe **Microsoft.Devices.Sensors.Motion** | [**Inclinometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Inclinometer) class |
| Globalização | |
| Namespace **System.Globalization** | [**Windows.Globalization**](https://docs.microsoft.com/uwp/api/Windows.Globalization) namespace |
| (ST = **System.Threading**) <br/> Propriedade **ST.Thread.CurrentCulture** | (SG = **System.Globalization**) <br/> Propriedade **S.CultureInfo.CurrentCulture** |
| (ST = **System.Threading**) <br/> Propriedade **ST.Thread.CurrentUICulture** | (SG = **System.Globalization**) <br/> Propriedade **S.CultureInfo.CurrentUICulture** |
| Elementos gráficos e animação | |
| **Microsoft.Xna.Framework.\*** namespaces, [XNA Framework Class Library](https://msdn.microsoft.com/library/bb203940.aspx), [Content Pipeline Class Library](https://msdn.microsoft.com/library/bb195587(v=XNAGameStudio.40).aspx) | Nenhuma equivalência direta. Em geral, use [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) com C++. Consulte [Desenvolvendo jogos](https://docs.microsoft.com/previous-versions/windows/apps/hh452744(v=win.10)) e [Interoperabilidade entre DirectX e XAML](https://docs.microsoft.com/previous-versions/windows/apps/hh825871(v=win.10)). |
| Classe **Microsoft.Xna.Framework.Audio.Microphone** | [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) class |
| Classe **Microsoft.Xna.Framework.Audio.SoundEffect** | [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) class |
| Namespace **Microsoft.Xna.Framework.GamerServices** | (WPS = **Windows.Phone.System**) <br/> [**WPS.UserProfile.GameServices.Core**](https://docs.microsoft.com/uwp/api/Windows.Phone.System.UserProfile.GameServices.Core) namespace |
| Classe **Microsoft.Xna.Framework.GamerServices.Guide** | Nenhuma equivalência direta |
| Classe **Microsoft.Xna.Framework.Input.GamePad** | [**HardwareButtons**](https://docs.microsoft.com/uwp/api/Windows.Phone.UI.Input.HardwareButtons) class |
| Classe **Microsoft.Xna.Framework.Input.Touch.TouchPanel** | [**GestureRecognizer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.GestureRecognizer) class |
| (MXFM = **Microsoft.Xna.Framework.Media**) <br/> Classes **MXFM.MediaLibrary**, **MXFM.PhoneExtensions.MediaLibraryExtensions** | [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders) class |
| Classe **Microsoft.Xna.Framework.Media.MediaQueue** | [**SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls) class |
| Classe **Microsoft.Xna.Framework.Media.Playlist** | [**BackgroundMediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.BackgroundMediaPlayer) class |
| Namespace **System.Windows.Media** | [**Windows.UI.Xaml.Media**](/uwp/api/Windows.UI.Xaml.Media) namespace |
| Classe **System.Windows.Media.RadialGradientBrush** | Nenhuma equivalência direta. Consulte [Mídia e elementos gráficos](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Namespace **System.Windows.Media.Animation** | [**Windows.UI.Xaml.Media.Animation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation) namespace |
| Namespace **System.Windows.Media.Effects** | Nenhuma equivalência direta |
| Namespace **System.Windows.Media.Imaging** | [**Windows.UI.Xaml.Media.Imaging**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging) namespace |
| Namespace **System.Windows.Media.Media3D** | [**Windows.UI.Xaml.Media.Media3D**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Media3D) namespace |
| Namespace **System.Windows.Shapes** | [**Windows.UI.Xaml.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) namespace |
| Iniciadores e seletores | |
| Classes **Microsoft.Phone.Tasks.AddressChooserTask**, **EmailAddressChooserTask**, **PhoneNumberChooserTask** | [**ContactPicker**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.ContactPicker) class |
| Classes **Microsoft.Phone.Tasks.AddWalletItemTask**, **AddWalletItemResult** | [**Windows.ApplicationModel.Wallet**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Wallet) namespace |
| Classes **Microsoft.Phone.Tasks.BingMapsDirectionsTask**, **BingMapsTask** | Nenhuma equivalência direta |
| Classe **Microsoft.Phone.Tasks.CameraCaptureTask** | [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) class. Além disso, a classe [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) class (somente Windows). |
| **Microsoft.Phone.Tasks.MarketplaceDetailTask** | [**CurrentApp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) class ([**RequestAppPurchaseAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) method) |
| Classes **Microsoft.Phone.Tasks.ConnectionSettingsTask**, **MarketplaceHubTask**, **MarketplaceReviewTask**, **MarketplaceSearchTask**, **MediaPlayerLauncher**, **SearchTask**, **SmsComposeTask**, **WebBrowserTask** | [**Launcher**](https://docs.microsoft.com/uwp/api/Windows.System.Launcher) class |
| Classe **Microsoft.Phone.Tasks.EmailComposeTask** | [**EmailMessage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Email.EmailMessage) class |
| Classe **Microsoft.Phone.Tasks.GameInviteTask** | Nenhuma equivalência direta |
| Classes **Microsoft.Phone.Tasks.MapDownloaderTask**, **MapsDirectionsTask**, **MapsTask**, **MapUpdaterTask** | Nenhuma equivalência direta |
| Classe **Microsoft.Phone.Tasks.PhoneCallTask** | [**PhoneCallManager**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls.PhoneCallManager) class |
| Classe **Microsoft.Phone.Tasks.PhotoChooserTask** | [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) class |
| Classe **Microsoft.Phone.Tasks.SaveAppointmentTask** | [**AppointmentManager**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments.AppointmentManager) class |
| Classes **Microsoft.Phone.Tasks.SaveContactTask**, **SaveEmailAddressTask**, **SavePhoneNumberTask** | [**StoredContact**](https://docs.microsoft.com/uwp/api/Windows.Phone.PersonalInformation.StoredContact) class (Windows Phone only) | 
| Classe **Microsoft.Phone.Tasks.SaveRingtoneTask** | Nenhuma equivalência direta |
| Classes **Microsoft.Phone.Tasks.ShareLinkTask**, **ShareMediaTask**, **ShareStatusTask** | [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) class |
| Location | |
| Namespace **System.Device.Location** | [**Windows.Devices.Geolocation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation) namespace |
| Classe **System.Device.GeoCoordinateWatcher** | [**Geolocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator) class |
| Mapas | |
| Namespaces **Microsoft.Phone.Maps** | [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) namespace |
| Namespace **Microsoft.Phone.Maps.Controls** | [**Windows.UI.Xaml.Controls.Maps**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps) namespace |
| Classe **Microsoft.Phone.Maps.Controls.Map** | [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) class |
| Namespace **Microsoft.Phone.Maps.Services** | [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) namespace |
| Classes **Microsoft.Phone.Maps.Services.GeocodeQuery**, **ReverseGeocodeQuery** | [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) class |
| Classe **System.Device.Location.GeoCoordinate** | [**Geopoint**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geopoint) class |
| Classe **Microsoft.Phone.Maps.Services.Route** | [**MapRoute**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute) class |
| Classe **Microsoft.Phone.Maps.Services.RouteQuery** | [**MapRouteFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinder) class |
| Monetização | |
| Namespace **Microsoft.Phone.Marketplace** | [**Windows.ApplicationModel.Store**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store) namespace |
| Mídia | |
| Namespace **Microsoft.Phone.Media** | [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) class |
| Rede do | |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.DeviceNetworkInformation** | [**Hostname**](https://docs.microsoft.com/uwp/api/Windows.Networking.HostName), [**NetworkInformation**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation) classes |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.NetworkInterface** | [**NetworkInformation**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation) class |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.NetworkInterfaceInfo** | [**ConnectionProfile**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.ConnectionProfile) class |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.NetworkInterfaceList** | [**NetworkInformation**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation) class |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.SocketExtensions** | Nenhuma equivalência direta |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.WebRequestExtensions** | Nenhuma equivalência direta |
| Namespace **Microsoft.Phone.Networking.Voip** | Nenhuma equivalência direta |
| Classe **System.Net.CookieCollection** | Ainda há suporte, mas algumas propriedades estão ausentes (por exemplo, IsReadOnly) |
| Classe **System.Net.DownloadProgressChangedEventArgs** e classes semelhantes relacionadas a **System.Net.WebClient** | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) class (or [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Derivar de [System.Net.Http.StreamContent](https://docs.microsoft.com/previous-versions/visualstudio/hh138119(v=vs.118)) para medir o progresso. |
| Classes **System.Net.DnsEndPoint**, **IPAddress** | Essas classes ainda têm suporte, mas algumas propriedades estão ausentes. Como alternativa, porte para a classe [**HostName**](https://docs.microsoft.com/uwp/api/Windows.Networking.HostName). |
| Classe **System.Net.HttpUtility** | [**HtmlFormatHelper**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.HtmlFormatHelper) class |
| Classe **System.Net.HttpWebRequest** | Suporte parcial, mas a alternativa recomendada e prospectiva é a classe [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (ou [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Essas APIs usam [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) para representar uma solicitação HTTP. |
| Classe **System.Net.HttpWebResponse** | Ainda compatível, mas use Dispose() em vez de Close(). Porém, a alternativa futura, recomendada, é a classe [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (ou [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Essas APIs usam [System.Net.Http.HttpResponseMessage](https://docs.microsoft.com/dotnet/api/system.net.http.httpresponsemessage) para representar uma resposta HTTP. |
| (SNN = **System.Net.NetworkInformation**) <br/> Classe **SNN.NetworkChange** | Ainda há suporte, exceto para o construtor. |
| Classe **System.Net.OpenReadCompletedEventArgs** e classes semelhantes relacionadas a **System.Net.WebClient** | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) class (or [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| Classe **System.Net.Sockets.Socket** | Ainda compatível, mas use Dispose() em vez de Close(). Como alternativa, porte para a classe [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket). |
| Classe **System.Net.Sockets.SocketException** | Ainda há suporte, mas use a propriedade SocketErrorCode em vez de ErrorCode. |
| Classes **System.Net.Sockets.UdpAnySourceMulticastClient**, **UdpSingleSourceMulticastClient** | [**DatagramSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket) class |
| Classe **System.Net.UploadProgressChangedEventArgs** e classes semelhantes relacionadas a **System.Net.WebClient** | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) class (or [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| Classe **System.Net.WebClient** | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) class (or [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| Classe **System.Net.WebRequest** | Suporte parcial (um conjunto de propriedades diferente), mas a alternativa recomendada e prospectiva é a classe [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (ou [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Essas APIs usam [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) para representar uma solicitação HTTP. |
| Classe **System.Net.WebResponse** | Ainda compatível, mas use Dispose() em vez de Close(). Porém, a alternativa futura, recomendada, é a classe [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (ou [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Essas APIs usam [System.Net.Http.HttpResponseMessage](https://docs.microsoft.com/dotnet/api/system.net.http.httpresponsemessage) para representar uma resposta HTTP. |
| (SN = **System.Net**) <br/> Classe **SN.WriteStreamClosedEventArgs** | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) class (or [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| (SN = **System.Net**) <br/> Classe **SN.WriteStreamClosedEventHandler** | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) class (or [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| Classe **System.UriFormatException** | Classe **System.FormatException** |
| Notificações | |
| Namespace MPN = **Microsoft.Phone.Notification** | [**Windows.UI.Notifications**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications), [**Windows.Networking.PushNotifications**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications) namespaces |
| MPN = **Microsoft.Phone.Notification** <br/> Classe **MPN.HttpNotification** | [**TileNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification) class |
| MPN = **Microsoft.Phone.Notification** <br/> Classe **MPN.HttpNotificationChannel** | [**PushNotificationChannel**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannel) class |
| Programação | |
| Namespace **System** | [**Windows.Foundation**](https://docs.microsoft.com/uwp/api/Windows.Foundation) namespace |
| Classes **System.Diagnostics.StackFrame**, **StackTrace** | Nenhuma equivalência direta |
| Namespace **System.Diagnostics** | [**Windows.Foundation.Diagnostics**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Diagnostics) namespace |
| Interface **System.ICloneable** | Um método personalizado que retorna o tipo adequado. |
| Classe **System.Reflection.Emit.ILGenerator** | Nenhuma equivalência direta |
| Extensões reativas | |
| Namespace **Microsoft.Phone.Reactive** | Nenhuma equivalência direta |
| Reflexão | |
| Classe **System.Type** | Classe **System.Reflection.TypeInfo**. Consulte [Reflexão no .NET Framework para aplicativos UWP](https://docs.microsoft.com/dotnet/framework/reflection-and-codedom/reflection-for-windows-store-apps). |
| Recursos | |
| Classe **System.Resources.ResourceManager** | (WA = **Windows.ApplicationModel**)<br/>[**WA.Resources.Core**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core) and [**WA.Resources**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources) namespaces, [**ResourceManager**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager) class. Consulte [Criando e recuperando recursos em aplicativos do Windows Runtime](https://docs.microsoft.com/previous-versions/windows/apps/hh694557(v=vs.140)). |
| Elemento seguro | |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> Classes **MPS.SecureElementChannel**, **MPS.SecureElementSession** | [**SmartCardConnection**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardConnection) class |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> Classe **MPS.SecureElementReader** | [**SmartCardReader**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardReader) class |
| Segurança | |
| (SSC = **System.Security.Cryptography**) <br/> Classes **SSC.Aes**, **SSC.RSA** | [**CryptographicEngine**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.CryptographicEngine) class |
| (SSC = **System.Security.Cryptography**) <br/> Classes **SSC.HMACSHA256**, **SSC.SHA256** | [**HashAlgorithmProvider**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.HashAlgorithmProvider) class |
| (SSC = **System.Security.Cryptography**) <br/> Classe **SSC.ProtectedData** | [**DataProtectionProvider**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.DataProtection.DataProtectionProvider) class |
| (SSC = **System.Security.Cryptography**) <br/> Classe **SSC.RandomNumberGenerator** | [**CryptographicBuffer**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.CryptographicBuffer) class |
| (SSC = **System.Security.Cryptography**) <br/> Classe **SSC.X509Certificates.X509Certificate** | [**CertificateEnrollmentManager**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Certificates.CertificateEnrollmentManager) class |
| Shell | |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ApplicationBar** | [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ApplicationBarIconButton** | [**AppBarButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) class (when used inside the [**PrimaryCommands**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands) property) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ApplicationBarMenuItem** | [**AppBarButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) class (when used inside the [**SecondaryCommands**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands) property) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classes **MPSh.CycleTileData**, **MPSh.FlipTileData**, **MPSh.IconicTileData**, **MPSh.ShellTileData**, **MPSh.StandardTileData** | [**TileTemplateType**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileTemplateType) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.PhoneApplicationService** | [**CoreApplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication), [**DisplayRequest**](https://docs.microsoft.com/uwp/api/Windows.System.Display.DisplayRequest) classes |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ProgressIndicator** | [**StatusBarProgressIndicator**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.StatusBarProgressIndicator) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ShellTile** | [**SecondaryTile**](https://docs.microsoft.com/uwp/api/Windows.UI.StartScreen.SecondaryTile) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ShellTileSchedule** | [**TileUpdater**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ShellToast** | [**ToastNotificationManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.SystemTray** | [**StatusBar**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.StatusBar) class |
| Armazenamento e E/S | |
| Classes **Microsoft.Phone.Storage.ExternalStorage**, **ExternalStorageDevice**, **ExternalStorageFile**, **ExternalStorageFolder** | [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders) class |
| Namespace **System.IO** | [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage), [**Windows.Storage.Streams**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams) namespaces |
| Classe **System.IO.Directory** | [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) class |
| Classe **System.IO.File** | [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) and [**PathIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.PathIO) classes
| (SII = **System.IO.IsolatedStorage**) <br/> Classe **SII.IsolatedStorageFile** |[**ApplicationData.LocalFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder) property |
| (SII = **System.IO.IsolatedStorage**) <br/> Classe **SII.IsolatedStorageSettings** | [**ApplicationData.LocalSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings) property |
| Classe **System.IO.Stream** | Ainda há suporte, mas use ReadAsync() e WriteAsync() em vez de BeginRead()/EndRead() e BeginWrite()/EndWrite(). |
| Carteira | |
| Namespace **Microsoft.Phone.Wallet** | [**Windows.ApplicationModel.Wallet**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Wallet) namespace |
| Xml | |
| (SX = **System.Xml**) | Método **SX.XmlConvert.ToDateTime** |
| (SX = **System.Xml**) | Método **SX.XmlConvert.ToDateTimeOffset** |


O próximo tópico é [Portando o projeto](wpsl-to-uwp-porting-to-a-uwp-project.md).

