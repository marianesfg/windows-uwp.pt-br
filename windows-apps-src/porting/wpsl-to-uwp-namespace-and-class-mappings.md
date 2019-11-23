---
description: Este tópico fornece um mapeamento abrangente de Windows Phone APIs do Silverlight para seus equivalentes de Plataforma Universal do Windows (UWP).
title: Windows Phone namespace do Silverlight para UWP e mapeamentos de classe
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
# <a name="windowsphone-silverlight-to-uwp-api-mappings"></a>Windows Phone mapeamentos de API do Silverlight para UWP


Este tópico fornece um mapeamento abrangente de Windows Phone APIs do Silverlight para seus equivalentes de Plataforma Universal do Windows (UWP). Geralmente não há um mapeamento de funcionalidade um para um: qualquer plataforma pode ter mais ou menos funcionalidades que suas equivalentes em um namespace ou classe.

A tabela de mapeamento ajudará você quando estiver trabalhando em um projeto UWP e você estiver reutilizando o código-fonte de um projeto Windows Phone Silverlight. Há diferenças nos nomes de namespaces e classes (inclusive controles de interface do usuário) entre as duas plataformas. Em muitos casos, isso é tão fácil quanto alterar um nome de namespace e, em seguida, o código será compilado. Às vezes, uma classe ou o nome da API mudou, bem como o nome do namespace. Em outras ocasiões, o mapeamento requer um pouco mais de trabalho e, em casos raros, requer uma mudança na abordagem.

**Como usar a tabela:  ** Primeiro, pesquise o nome da classe que você está usando. As classes estão listadas sempre que o mapeamento é mais complicado do que simplesmente alterar o nome do namespace. Caso a classe não esteja listada, o mapeamento é apenas uma alteração do namespace. Assim, encontre o nome do namespace da classe, e você encontrará o nome do namespace UWP equivalente. A classe estará nesse namespace. Caso o namespace não esteja listado é porque o nome não foi alterado.

**Observação**  o Windows 10 dá suporte a muito mais do .NET Framework do que um aplicativo da loja Windows Phone. Por exemplo, o Windows 10 tem vários System. ServiceModel.\* namespaces, bem como System.Net, System .net. NetworkInformation e System .net. Sockets.
Além disso, em um aplicativo do Windows 10, você se beneficiará do .NET Native, que é uma tecnologia de compilação antecipada que converte MSIL em código de máquina executável nativamente. Aplicativos .NET Native iniciam com mais rapidez, usam menos memória e bateria do que seus equivalentes MSIL.

| Windows Phone Silverlight | Windows Runtime |
| ------------------------- | --------------- |
| Publicidade | |
| Classe **Microsoft.Advertising.Mobile.UI.AdControl** | Classe [AdControl](https://docs.microsoft.com/windows/uwp/monetize/display-ads-using-the-microsoft-advertising-libraries) |
| Alarmes, lembretes e agentes de segundo plano | |
| Classe **Microsoft.Phone.BackgroundAgent** | Classe [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) |
| Namespace **Microsoft.Phone.Scheduler** | Namespace [**Windows. ApplicationModel. Background**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background) |
| Classe **Microsoft.Phone.Scheduler.Alarm** | Classes [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) e [**ToastNotificationManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) |
| Classes **Microsoft.Phone.Scheduler.PeriodicTask**, **ScheduledAction**, **ScheduledActionService**, **ScheduledTask** , **ScheduledTaskAgent** | Classe [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) |
| Classe **Microsoft.Phone.Scheduler.Reminder** | Classes [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) e [**ToastNotificationManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) |
| Classe **Microsoft.Phone.PictureDecoder** | Classe [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) |
| Namespace **Microsoft.Phone.BackgroundAudio** | Namespace do [**Windows. Media. playback**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback) |
| Namespace **Microsoft.Phone.BackgroundTransfer** | Namespace [**Windows. Networking. BackgroundTransfer**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer) |
| Modelo de aplicativo e ambiente | |
| Classe **System.AppDomain** | Nenhuma equivalência direta. Veja as classes [**Application**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Application), [**CoreApplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication) |
| Classe **System.Environment** | Nenhuma equivalência direta |
| Classe **System.ComponentModel.Annotations**  | Nenhuma equivalência direta |
| Classe **System.ComponentModel.BackgroundWorker** | Classe [**ThreadPool**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPool) |
| Classe **System.ComponentModel.DesignerProperties** | Classe [**DesignMode**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DesignMode) |
| **Classes System.Threading.Thread**, **System.Threading.ThreadPool** | Classe [**ThreadPool**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPool) |
| (ST = **System.Threading**) <br/> Método **ST.Thread.MemoryBarrier** | (ST = **System.Threading**) <br/> Método **ST.Interlocked.MemoryBarrier** |
| (ST = **System.Threading**) <br/> Propriedade **ST.Thread.ManagedThreadId** | (S = **System**) <br/> Propriedade **S.Environment.ManagedThreadId** |
| Classe **System.Threading.Timer** | Classe [**ThreadPoolTimer**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPoolTimer) |
| (SWT = **System.Windows.Threading**) <br/> Classe **SWT.Dispatcher** | Classe [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) |
| (SWT = **System.Windows.Threading**) <br/> Classe **SWT.DispatcherTimer** | Classe [**DispatcherTimer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DispatcherTimer) |
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
| Namespace **Microsoft.Phone.UserData** | Namespaces do [**Windows. ApplicationModel. Contacts**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts), [**Windows. ApplicationModel. compromissos**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments) |
| (MPU = **Microsoft.Phone.UserData**) <br/> Classes **MPU.Account**, **ContactAddress**, **ContactCompanyInformation**, **ContactEmailAddress**, **ContactPhoneNumber** | Classe de [**contato**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact) |
| (MPU = **Microsoft.Phone.UserData**) <br/> Classe **MPU.Appointments** | Classe [**AppointmentCalendar**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments.AppointmentCalendar) |
| (MPU = **Microsoft.Phone.UserData**) <br/> Classe **MPU.Contacts** | Classe [**ContactStore**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.ContactStore) |
| Controles e infraestrutura de interface do usuário | |
| Classe **ControlTiltEffect.TiltEffect** | As animações da biblioteca de animação do Windows Runtime são incorporadas os estilos padrão dos controles comuns. Consulte [Animação](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Namespace **Microsoft.Phone.Controls** | Namespace [**Windows. UI. XAML. Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls) |
| (MPC = **Microsoft.Phone.Controls**) <br/> Classe **MPC.ContextMenu** | Classe [**PopupMenu**](https://docs.microsoft.com/uwp/api/Windows.UI.Popups.PopupMenu) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.DatePickerPage** | Classe [**DatePickerFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePickerFlyout) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.GestureListener** | Classe [**GestureRecognizer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.GestureRecognizer) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.LongListSelector** | Classe [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.ObscuredEventArgs** | Classes [**SystemProtection**](https://docs.microsoft.com/uwp/api/Windows.Phone.System.SystemProtection), [**WindowActivatedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.WindowActivatedEventArgs) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.Panorama** | Classe de [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationFrame**,<br/>(SWN = **System.Windows.Navigation**) <br/>Classes **SWN.NavigationService** | Classe [**frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.PhoneApplicationPage** | Classe de [**página**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.TiltEffect** | Classe [**PointerDownThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.TimePickerPage** | Classe [**TimePickerFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePickerFlyout) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.WebBrowser** | Classe [**WebView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.WebBrowserExtensions** | Nenhuma equivalência direta |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.WrapPanel** | Não há equivalente direto para fins de layout geral. [**ItemsWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) e [**WrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) podem ser usados no modelo de painel de itens de um controle de itens. |
| (MPD = **Microsoft.Phone.Data**) <br/>Namespace **MPD.Linq** | Nenhuma equivalência direta |
| (MPD = **Microsoft.Phone.Data**) <br/>Namespace **MPD.Linq.Mapping** | Nenhuma equivalência direta |
| Namespace **Microsoft.Phone.Globalization** | Nenhuma equivalência direta |
| (MPI = **Microsoft.Phone.Info**) <br/>Classes **MPI.DeviceExtendedProperties**, **DeviceStatus** | Classes [**EasClientDeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Security.ExchangeActiveSyncProvisioning.EasClientDeviceInformation), [**MemoryManager**](https://docs.microsoft.com/uwp/api/Windows.System.MemoryManager) . Para obter mais detalhes, consulte [Status do dispositivo](wpsl-to-uwp-input-and-sensors.md). |
| (MPI = **Microsoft.Phone.Info**) <br/>Classe **MPI.MediaCapabilities** | Nenhuma equivalência direta |
| (MPI = **Microsoft.Phone.Info**) <br/>Classe **MPI.UserExtendedProperties** | Classe [**advertisingmanager**](https://docs.microsoft.com/uwp/api/Windows.System.UserProfile.AdvertisingManager) |
| Namespace **System.Windows** | Namespace [**do Windows. UI. XAML**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml) |
| Namespace **System.Windows.Automation** | Namespace [**do Windows. UI. XAML. Automation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation) |
| Namespaces **System.Windows.Controls**, **System.Windows.Input** | Namespaces [**do Windows. UI. Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core), do [**Windows. UI. Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**do Windows. UI. XAML. Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls) |
| Classes **System.Windows.Controls.DrawingSurface**, **DrawingSurfaceBackgroundGrid** | Classe [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) |
| Classe **System.Windows.Controls.RichTextBox** | Classe [**RichEditBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) |
| Classe **System.Windows.Controls.WrapPanel** | Não há equivalente direto para fins de layout geral. [**ItemsWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) e [**WrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) podem ser usados no modelo de painel de itens de um controle de itens. |
| Namespace **System.Windows.Controls.Primitives** | Namespace [**Windows. UI. XAML. Controls. primitivas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives) |
| Namespace **System.Windows.Controls.Shapes** | Namespace [**Windows. UI. XAML. Controls. Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) |
| Namespace **System.Windows.Data** | Namespace [**Windows. UI. XAML. Data**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data) |
| Namespace **System.Windows.Documents** | Namespace [**do Windows. UI. XAML. Documents**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents) |
| Namespace **System.Windows.Ink** | Nenhuma equivalência direta |
| Namespace **System.Windows.Markup** | Namespace [**Windows. UI. XAML. Markup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Markup) | 
| Namespace **System.Windows.Navigation** | Namespace [**Windows. UI. XAML. Navigation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Navigation) |
| Evento **System.Windows.UIElement.Tap**, **EventHandler&lt;GestureEventArgs&gt;** delegado | Evento [**tocado**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.tapped) , delegado [**TappedEventHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.tappedeventhandler) |
| Dados e serviços |  |
| Classe **System.Data.Linq.DataContext** | Nenhuma equivalência direta |
| Classe **System.Data.Linq.Mapping.ColumnAttribute** | Nenhuma equivalência direta |
| Classe **System.Data.Linq.SqlClient.SqlHelpers** | Nenhuma equivalência direta |
| Dispositivos | |
| Namespaces **Microsoft.Devices**, **Microsoft.Devices.Sensors** | Os namespaces [**Windows. Devices. Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration), [**Windows. Devices. Enumeration. PNP**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.Pnp), [**Windows. Devices. Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input), [**Windows. Devices. Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors) |
| Classes **Microsoft.Devices.Camera**, **Microsoft.Devices.PhotoCamera** | Classe [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) . Além disso, a classe [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) class (somente Windows). |
| Classe **Microsoft.Devices.CameraButtons** | Classe [**HardwareButtons**](https://docs.microsoft.com/uwp/api/Windows.Phone.UI.Input.HardwareButtons) |
| Classe **Microsoft.Devices.CameraVideoBrushExtensions** | Classe [**capturáelement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) |
| Classe **Microsoft.Devices.Environment** | Nenhuma equivalência direta. Como alternativa, use a compilação condicional e defina um símbolo personalizado. Ou você pode ser capaz de criar uma solução alternativa usando a propriedade [IsAttached](https://docs.microsoft.com/dotnet/api/system.diagnostics.debugger.isattached#System_Diagnostics_Debugger_IsAttached). |
| Classe **Microsoft.Devices.MediaHistory** | Nenhuma equivalência direta |
| Classe **Microsoft.Devices.VibrateController** | Classe [**VibrationDevice**](https://docs.microsoft.com/uwp/api/Windows.Phone.Devices.Notification.VibrationDevice) |
| Classe **Microsoft.Devices.Radio.FMRadio** | Nenhuma equivalência direta |
| Classes **Microsoft.Devices.Sensors.Accelerometer**, **Compass** | No namespace [**Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors) |
| Classe **Microsoft.Devices.Sensors.Gyroscope** | Classe [**girômetro**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Gyrometer) |
| Classe **Microsoft.Devices.Sensors.Motion** | Classe [**inclinômetro**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Inclinometer) |
| Globalização | |
| Namespace **System.Globalization** | Namespace do [**Windows. Globalization**](https://docs.microsoft.com/uwp/api/Windows.Globalization) |
| (ST = **System.Threading**) <br/> Propriedade **ST.Thread.CurrentCulture** | (SG = **System.Globalization**) <br/> Propriedade **S.CultureInfo.CurrentCulture** |
| (ST = **System.Threading**) <br/> Propriedade **ST.Thread.CurrentUICulture** | (SG = **System.Globalization**) <br/> Propriedade **S.CultureInfo.CurrentUICulture** |
| Elementos gráficos e animação | |
| Namespaces **Microsoft. XNA. Framework.\*** , [biblioteca de classes do XNA Framework](https://msdn.microsoft.com/library/bb203940.aspx), [biblioteca de classes de pipeline de conteúdo](https://msdn.microsoft.com/library/bb195587(v=XNAGameStudio.40).aspx) | Nenhuma equivalência direta. Em geral, use [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) com C++. Consulte [Desenvolvendo jogos](https://docs.microsoft.com/previous-versions/windows/apps/hh452744(v=win.10)) e [Interoperabilidade entre DirectX e XAML](https://docs.microsoft.com/previous-versions/windows/apps/hh825871(v=win.10)). |
| Classe **Microsoft.Xna.Framework.Audio.Microphone** | Classe [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) |
| Classe **Microsoft.Xna.Framework.Audio.SoundEffect** | Classe [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) |
| Namespace **Microsoft.Xna.Framework.GamerServices** | (WPS = **Windows.Phone.System**) <br/> [**WPS. Namespace UserProfile. Gameservices. Core**](https://docs.microsoft.com/uwp/api/Windows.Phone.System.UserProfile.GameServices.Core) |
| Classe **Microsoft.Xna.Framework.GamerServices.Guide** | Nenhuma equivalência direta |
| Classe **Microsoft.Xna.Framework.Input.GamePad** | Classe [**HardwareButtons**](https://docs.microsoft.com/uwp/api/Windows.Phone.UI.Input.HardwareButtons) |
| Classe **Microsoft.Xna.Framework.Input.Touch.TouchPanel** | Classe [**GestureRecognizer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.GestureRecognizer) |
| (MXFM = **Microsoft.Xna.Framework.Media**) <br/> Classes **MXFM.MediaLibrary**, **MXFM.PhoneExtensions.MediaLibraryExtensions** | Classe [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders) |
| Classe **Microsoft.Xna.Framework.Media.MediaQueue** | Classe [**SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls) |
| Classe **Microsoft.Xna.Framework.Media.Playlist** | Classe [**BackgroundMediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.BackgroundMediaPlayer) |
| Namespace **System.Windows.Media** | Namespace [**Windows. UI. XAML. Media**](/uwp/api/Windows.UI.Xaml.Media) |
| Classe **System.Windows.Media.RadialGradientBrush** | Nenhuma equivalência direta. Consulte [Mídia e elementos gráficos](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Namespace **System.Windows.Media.Animation** | Namespace [**Windows. UI. XAML. Media. Animation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation) |
| Namespace **System.Windows.Media.Effects** | Nenhuma equivalência direta |
| Namespace **System.Windows.Media.Imaging** | Namespace [**Windows. UI. XAML. Media. Imaging**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging) |
| Namespace **System.Windows.Media.Media3D** | Namespace [**Windows. UI. XAML. Media. Media3D**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Media3D) |
| Namespace **System.Windows.Shapes** | Namespace [**Windows. UI. XAML. Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) |
| Iniciadores e seletores | |
| Classes **Microsoft.Phone.Tasks.AddressChooserTask**, **EmailAddressChooserTask**, **PhoneNumberChooserTask** | Classe [**ContactPicker**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.ContactPicker) |
| Classes **Microsoft.Phone.Tasks.AddWalletItemTask**, **AddWalletItemResult** | Namespace do [**Windows. ApplicationModel. carteira**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Wallet) |
| Classes **Microsoft.Phone.Tasks.BingMapsDirectionsTask**, **BingMapsTask** | Nenhuma equivalência direta |
| Classe **Microsoft.Phone.Tasks.CameraCaptureTask** | Classe [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) . Além disso, a classe [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) class (somente Windows). |
| **Microsoft. Phone. Tasks. MarketplaceDetailTask** | Classe [**CurrentApp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) (método[**RequestAppPurchaseAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) ) |
| Classes **Microsoft.Phone.Tasks.ConnectionSettingsTask**, **MarketplaceHubTask**, **MarketplaceReviewTask**, **MarketplaceSearchTask**, **MediaPlayerLauncher**, **SearchTask**, **SmsComposeTask**, **WebBrowserTask** | Classe do [**iniciador**](https://docs.microsoft.com/uwp/api/Windows.System.Launcher) |
| Classe **Microsoft.Phone.Tasks.EmailComposeTask** | Classe [**EmailMessage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Email.EmailMessage) |
| Classe **Microsoft.Phone.Tasks.GameInviteTask** | Nenhuma equivalência direta |
| Classes **Microsoft.Phone.Tasks.MapDownloaderTask**, **MapsDirectionsTask**, **MapsTask**, **MapUpdaterTask** | Nenhuma equivalência direta |
| Classe **Microsoft.Phone.Tasks.PhoneCallTask** | Classe do [**telefonemamanager**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls.PhoneCallManager) |
| Classe **Microsoft.Phone.Tasks.PhotoChooserTask** | Classe [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) |
| Classe **Microsoft.Phone.Tasks.SaveAppointmentTask** | Classe de [**compromissomanager**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments.AppointmentManager) |
| Classes **Microsoft.Phone.Tasks.SaveContactTask**, **SaveEmailAddressTask**, **SavePhoneNumberTask** | Classe [**StoredContact**](https://docs.microsoft.com/uwp/api/Windows.Phone.PersonalInformation.StoredContact) (somente Windows Phone) | 
| Classe **Microsoft.Phone.Tasks.SaveRingtoneTask** | Nenhuma equivalência direta |
| Classes **Microsoft.Phone.Tasks.ShareLinkTask**, **ShareMediaTask**, **ShareStatusTask** | Classe [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) |
| Location | |
| Namespace **System.Device.Location** | Namespace [**Windows. Devices. geolocalização**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation) |
| Classe **System.Device.GeoCoordinateWatcher** | Classe [**geolocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator) |
| Mapas | |
| Namespaces **Microsoft.Phone.Maps** | Namespace do [**Windows. Services. Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) |
| Namespace **Microsoft.Phone.Maps.Controls** | Namespace [**Windows. UI. XAML. Controls. Maps**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps) |
| Classe **Microsoft.Phone.Maps.Controls.Map** | Classe [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) |
| Namespace **Microsoft.Phone.Maps.Services** | Namespace do [**Windows. Services. Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) |
| Classes **Microsoft.Phone.Maps.Services.GeocodeQuery**, **ReverseGeocodeQuery** | Classe [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) |
| Classe **System.Device.Location.GeoCoordinate** | Classe [**geopoint**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geopoint) |
| Classe **Microsoft.Phone.Maps.Services.Route** | Classe [**MapRoute**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute) |
| Classe **Microsoft.Phone.Maps.Services.RouteQuery** | Classe [**MapRouteFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinder) |
| Monetização | |
| Namespace **Microsoft.Phone.Marketplace** | Namespace do [**Windows. ApplicationModel. Store**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store) |
| Media | |
| Namespace **Microsoft.Phone.Media** | Classe [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) |
| Rede | |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.DeviceNetworkInformation** | [**Nome do host**](https://docs.microsoft.com/uwp/api/Windows.Networking.HostName), classes [**NetworkInformation**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation) |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.NetworkInterface** | Classe [**NetworkInformation**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation) |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.NetworkInterfaceInfo** | Classe [**ConnectionProfile**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.ConnectionProfile) |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.NetworkInterfaceList** | Classe [**NetworkInformation**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation) |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.SocketExtensions** | Nenhuma equivalência direta |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.WebRequestExtensions** | Nenhuma equivalência direta |
| Namespace **Microsoft.Phone.Networking.Voip** | Nenhuma equivalência direta |
| Classe **System.Net.CookieCollection** | Ainda há suporte, mas algumas propriedades estão ausentes (por exemplo, IsReadOnly) |
| Classe **System.Net.DownloadProgressChangedEventArgs** e classes semelhantes relacionadas a **System.Net.WebClient** | Classe [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (ou [System .net. http. HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Derivar de [System.Net.Http.StreamContent](https://docs.microsoft.com/previous-versions/visualstudio/hh138119(v=vs.118)) para medir o progresso. |
| Classes **System.Net.DnsEndPoint**, **IPAddress** | Essas classes ainda têm suporte, mas algumas propriedades estão ausentes. Como alternativa, porte para a classe [**HostName**](https://docs.microsoft.com/uwp/api/Windows.Networking.HostName). |
| Classe **System.Net.HttpUtility** | Classe [**HtmlFormatHelper**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.HtmlFormatHelper) |
| Classe **System.Net.HttpWebRequest** | Suporte parcial, mas a alternativa recomendada e prospectiva é a classe [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (ou [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Essas APIs usam [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) para representar uma solicitação HTTP. |
| Classe **System.Net.HttpWebResponse** | Ainda compatível, mas use Dispose() em vez de Close(). Porém, a alternativa futura, recomendada, é a classe [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (ou [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Essas APIs usam [System.Net.Http.HttpResponseMessage](https://docs.microsoft.com/dotnet/api/system.net.http.httpresponsemessage) para representar uma resposta HTTP. |
| (SNN = **System.Net.NetworkInformation**) <br/> Classe **SNN.NetworkChange** | Ainda há suporte, exceto para o construtor. |
| Classe **System.Net.OpenReadCompletedEventArgs** e classes semelhantes relacionadas a **System.Net.WebClient** | Classe [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (ou [System .net. http. HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| Classe **System.Net.Sockets.Socket** | Ainda compatível, mas use Dispose() em vez de Close(). Como alternativa, porte para a classe [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket). |
| Classe **System.Net.Sockets.SocketException** | Ainda há suporte, mas use a propriedade SocketErrorCode em vez de ErrorCode. |
| Classes **System.Net.Sockets.UdpAnySourceMulticastClient**, **UdpSingleSourceMulticastClient** | Classe [**DatagramSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket) |
| Classe **System.Net.UploadProgressChangedEventArgs** e classes semelhantes relacionadas a **System.Net.WebClient** | Classe [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (ou [System .net. http. HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| Classe **System.Net.WebClient** | Classe [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (ou [System .net. http. HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| Classe **System.Net.WebRequest** | Suporte parcial (um conjunto de propriedades diferente), mas a alternativa recomendada e prospectiva é a classe [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (ou [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Essas APIs usam [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) para representar uma solicitação HTTP. |
| Classe **System.Net.WebResponse** | Ainda compatível, mas use Dispose() em vez de Close(). Porém, a alternativa futura, recomendada, é a classe [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (ou [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Essas APIs usam [System.Net.Http.HttpResponseMessage](https://docs.microsoft.com/dotnet/api/system.net.http.httpresponsemessage) para representar uma resposta HTTP. |
| (SN = **System.Net**) <br/> Classe **SN.WriteStreamClosedEventArgs** | Classe [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (ou [System .net. http. HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| (SN = **System.Net**) <br/> Classe **SN.WriteStreamClosedEventHandler** | Classe [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (ou [System .net. http. HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| Classe **System.UriFormatException** | Classe **System.FormatException** |
| Notificações | |
| Namespace MPN = **Microsoft.Phone.Notification** | Namespaces [**do Windows. UI. Notifications**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications), [**Windows. Networking. PushNotifications**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications) |
| MPN = **Microsoft.Phone.Notification** <br/> Classe **MPN.HttpNotification** | Classe [**TileNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification) |
| MPN = **Microsoft.Phone.Notification** <br/> Classe **MPN.HttpNotificationChannel** | Classe [**PushNotificationChannel**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannel) |
| Programação | |
| Namespace **System** | Namespace do [**Windows. Foundation**](https://docs.microsoft.com/uwp/api/Windows.Foundation) |
| Classes **System.Diagnostics.StackFrame**, **StackTrace** | Nenhuma equivalência direta |
| Namespace **System.Diagnostics** | Namespace do [**Windows. Foundation. Diagnostics**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Diagnostics) |
| Interface **System.ICloneable** | Um método personalizado que retorna o tipo adequado. |
| Classe **System.Reflection.Emit.ILGenerator** | Nenhuma equivalência direta |
| Extensões reativas | |
| Namespace **Microsoft.Phone.Reactive** | Nenhuma equivalência direta |
| Reflexão | |
| Classe **System.Type** | Classe **System.Reflection.TypeInfo**. Consulte [Reflexão no .NET Framework para aplicativos UWP](https://docs.microsoft.com/dotnet/framework/reflection-and-codedom/reflection-for-windows-store-apps). |
| Recursos | |
| Classe **System.Resources.ResourceManager** | (WA = **Windows.ApplicationModel**)<br/>[**Wa. Resources. Core**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core) e [**wa.** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources)Namespaces de recursos, classe [**ResourceManager**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager) . Consulte [Criando e recuperando recursos em aplicativos do Windows Runtime](https://docs.microsoft.com/previous-versions/windows/apps/hh694557(v=vs.140)). |
| Elemento seguro | |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> Classes **MPS.SecureElementChannel**, **MPS.SecureElementSession** | Classe [**SmartCardConnection**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardConnection) |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> Classe **MPS.SecureElementReader** | Classe [**SmartCardReader**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardReader) |
| Segurança | |
| (SSC = **System.Security.Cryptography**) <br/> Classes **SSC.Aes**, **SSC.RSA** | Classe [**CryptographicEngine**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.CryptographicEngine) |
| (SSC = **System.Security.Cryptography**) <br/> Classes **SSC.HMACSHA256**, **SSC.SHA256** | Classe [**hashalgorithmprovider**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.HashAlgorithmProvider) |
| (SSC = **System.Security.Cryptography**) <br/> Classe **SSC.ProtectedData** | Classe [**Dataprotectionprovider**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.DataProtection.DataProtectionProvider) |
| (SSC = **System.Security.Cryptography**) <br/> Classe **SSC.RandomNumberGenerator** | Classe [**CryptographicBuffer**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.CryptographicBuffer) |
| (SSC = **System.Security.Cryptography**) <br/> Classe **SSC.X509Certificates.X509Certificate** | Classe [**CertificateEnrollmentManager**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Certificates.CertificateEnrollmentManager) |
| Shell | |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ApplicationBar** | Classe [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ApplicationBarIconButton** | Classe [**AppBarButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) (quando usada dentro da propriedade [**PrimaryCommands**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands) ) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ApplicationBarMenuItem** | Classe [**AppBarButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) (quando usada dentro da propriedade [**SecondaryCommands**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands) ) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classes **MPSh.CycleTileData**, **MPSh.FlipTileData**, **MPSh.IconicTileData**, **MPSh.ShellTileData**, **MPSh.StandardTileData** | Classe [**TileTemplateType**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileTemplateType) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.PhoneApplicationService** | Classes [**CoreApplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication), [**DisplayRequest**](https://docs.microsoft.com/uwp/api/Windows.System.Display.DisplayRequest) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ProgressIndicator** | Classe [**StatusBarProgressIndicator**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.StatusBarProgressIndicator) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ShellTile** | Classe [**SecondaryTile**](https://docs.microsoft.com/uwp/api/Windows.UI.StartScreen.SecondaryTile) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ShellTileSchedule** | Classe [**TileUpdater**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ShellToast** | Classe [**ToastNotificationManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.SystemTray** | Classe [**StatusBar**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.StatusBar) |
| Armazenamento e E/S | |
| Classes **Microsoft.Phone.Storage.ExternalStorage**, **ExternalStorageDevice**, **ExternalStorageFile**, **ExternalStorageFolder** | Classe [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders) |
| Namespace **System.IO** | Namespaces [**Windows. Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage), [**Windows. Storage. streams**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams) |
| Classe **System.IO.Directory** | Classe [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) |
| Classe **System.IO.File** | Classes [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) e [**PathIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.PathIO)
| (SII = **System.IO.IsolatedStorage**) <br/> Classe **SII.IsolatedStorageFile** |Propriedade [**applicationData. LocalFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder) |
| (SII = **System.IO.IsolatedStorage**) <br/> Classe **SII.IsolatedStorageSettings** | Propriedade [**applicationData. LocalSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings) |
| Classe **System.IO.Stream** | Ainda há suporte, mas use ReadAsync() e WriteAsync() em vez de BeginRead()/EndRead() e BeginWrite()/EndWrite(). |
| Carteira | |
| Namespace **Microsoft.Phone.Wallet** | Namespace do [**Windows. ApplicationModel. carteira**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Wallet) |
| Xml | |
| (SX = **System.Xml**) | Método **SX.XmlConvert.ToDateTime** |
| (SX = **System.Xml**) | Método **SX.XmlConvert.ToDateTimeOffset** |


O próximo tópico é [Portando o projeto](wpsl-to-uwp-porting-to-a-uwp-project.md).

