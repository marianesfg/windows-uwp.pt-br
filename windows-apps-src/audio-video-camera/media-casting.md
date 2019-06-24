---
ms.assetid: 40B97E0C-EB1B-40C2-A022-1AB95DFB085E
description: Este artigo mostra como converter mídia em dispositivos remotos de um aplicativo universal do Windows.
title: Transmissão de mídia
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 50f588caaf36d9a2a74222029e17785663cf3953
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318302"
---
# <a name="media-casting"></a>Transmissão de mídia



Este artigo mostra como converter mídia em dispositivos remotos de um aplicativo universal do Windows.

## <a name="built-in-media-casting-with-mediaplayerelement"></a>Transmissão de mídia integrada com o MediaPlayerElement

A maneira mais simples para converter a mídia de um Aplicativo Universal do Windows é usar a funcionalidade de transmissão interna do controle do [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement).

Para permitir que o usuário abra um arquivo de vídeo a ser reproduzido no controle do **MediaPlayerElement**, adicione os seguintes namespaces ao seu projeto.

[!code-cs[BuiltInCastingUsing](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetBuiltInCastingUsing)]

No arquivo XAML do seu aplicativo, adicione um **MediaPlayerElement** e defina [**AreTransportControlsEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaelement.aretransportcontrolsenabled) como verdadeiro.

[!code-xml[MediaElement](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetMediaElement)]

Adicione um botão para permitir que o usuário inicie selecionando um arquivo.

[!code-xml[OpenButton](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetOpenButton)]

No manipulador de eventos [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) para o botão, crie uma nova instância do [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker), adicione tipos de arquivo de vídeo à coleção do [**FileTypeFilter**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.filetypefilter) e defina o local inicial para a biblioteca de vídeos do usuário.

Chame [**PickSingleFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) para iniciar a caixa de diálogo do seletor de arquivos. Quando esse método retorna, o resultado é um objeto [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) que representa o arquivo de vídeo. Verifique se o arquivo não é nulo, o que acontecerá se o usuário cancelar a operação de seleção. Chame o método [**OpenAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.openasync) do arquivo para obter um [**IRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IRandomAccessStream) para o arquivo. Por fim, crie um novo objeto **MediaSource** com base no arquivo selecionado chamando [**CreateFromStorageFile**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromstoragefile) e o atribua ao objeto **MediaPlayerElement**, propriedade [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source), para fazer do arquivo de vídeo a origem do controle.

[!code-cs[OpenButtonClick](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetOpenButtonClick)]

Depois que o vídeo é carregado no **MediaPlayerElement**, o usuário pode simplesmente pressionar o botão de transmissão nos controles de transporte para iniciar uma caixa de diálogo interna que permita a eles escolher um dispositivo para o qual a mídia carregada será convertida.

![Botão de transmissão mediaelement](images/media-element-casting-button.png)

> [!NOTE] 
> A partir do Windows 10, versão 1607, é recomendável que você use a classe **MediaPlayer** para reproduzir itens de mídia. O **MediaPlayerElement** é um controle XAML leve que é usado para renderizar o conteúdo de um **MediaPlayer** em uma página XAML. O controle **MediaElement** continua a ser suportado para compatibilidade com versões anteriores. Para obter mais informações sobre como usar o **MediaPlayer** e o **MediaPlayerElement** para reproduzir conteúdo de mídia, consulte [Reproduzir áudio e vídeo com o MediaPlayer](play-audio-and-video-with-mediaplayer.md). Para obter informações sobre como usar o **MediaSource** e as APIs relacionadas para trabalhar com conteúdo de mídia, consulte [Itens de mídia, playlists e faixas](media-playback-with-mediasource.md).

## <a name="media-casting-with-the-castingdevicepicker"></a>Transmissão de mídia com o CastingDevicePicker

Uma segunda maneira de converter mídia em um dispositivo é usar o [**CastingDevicePicker**](https://docs.microsoft.com/uwp/api/Windows.Media.Casting.CastingDevicePicker). Para usar essa classe, inclua o namespace [**Windows.Media.Casting**](https://docs.microsoft.com/uwp/api/Windows.Media.Casting) em seu projeto.

[!code-cs[CastingNamespace](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetCastingNamespace)]

Declare uma variável do membro para o objeto **CastingDevicePicker**.

[!code-cs[DeclareCastingPicker](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetDeclareCastingPicker)]

Quando sua página for inicializada, crie uma nova instância do seletor de transmissão e defina o [**Filter**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingdevicepicker.filter) como a propriedade [**SupportsVideo**](https://docs.microsoft.com/uwp/api/Windows.Media.Casting.CastingDevicePickerFilter) para indicar que os dispositivos de transmissão listados pelo seletor devem dar suporte a vídeo. Registre um manipulador para o evento [**CastingDeviceSelected**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingdevicepicker.castingdeviceselected), que é acionado quando o usuário seleciona um dispositivo para transmissão.

[!code-cs[InitCastingPicker](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetInitCastingPicker)]

No seu arquivo XAML, adicione um botão para permitir que o usuário inicie o seletor.

[!code-xml[CastPickerButton](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetCastPickerButton)]

No manipulador de eventos **Click** para o botão, chame [**TransformToVisual**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transformtovisual) para obter a transformação de um elemento de interface do usuário em relação a outro. Neste exemplo, a transformação é a posição do botão do seletor de conversão em relação à raiz visual da janela do aplicativo. Chame o método [**Show**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingdevicepicker.show) do objeto [**CastingDevicePicker**](https://docs.microsoft.com/uwp/api/Windows.Media.Casting.CastingDevicePicker) para iniciar a caixa de diálogo do seletor de transmissão. Especifique o local e as dimensões do botão do seletor de conversão para que o sistema possa fazer a caixa de diálogo sair do botão que o usuário pressionou.

[!code-cs[CastPickerButtonClick](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetCastPickerButtonClick)]

No manipulador de eventos do **CastingDeviceSelected**, chame o método [**CreateCastingConnection**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingdevice.createcastingconnection) da propriedade [**SelectedCastingDevice**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingdeviceselectedeventargs.selectedcastingdevice) dos argumentos do evento, que representa o dispositivo de transmissão selecionado pelo usuário. Registre manipuladores para os eventos [**ErrorOccurred**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingconnection.erroroccurred) e [**StateChanged**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingconnection.statechanged). Por fim, chame [**RequestStartCastingAsync**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingconnection.requeststartcastingasync) para começar a conversão, passando o resultado para o controle **MediaPlayerElement**, objeto **MediaPlayer**, método [**GetAsCastingSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaelement.getascastingsource), para especificar que a mídia a ser convertida é o conteúdo do **MediaPlayer** associado ao **MediaPlayerElement**.

> [!NOTE] 
> A conexão de transmissão deve ser iniciada no thread da interface do usuário. Como o **CastingDeviceSelected** não é chamado no thread da interface do usuário, você deve colocar essas chamadas dentro de uma chamada para [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync), o que faz com que elas sejam chamadas no thread da interface do usuário.

[!code-cs[CastingDeviceSelected](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetCastingDeviceSelected)]

Nos manipuladores de eventos **ErrorOccurred** e **StateChanged**, você deve atualizar sua interface do usuário para informar o usuário sobre o status atual da transmissão. Esses eventos são abordados em detalhes na seção a seguir sobre a criação de um seletor de dispositivo personalizado de transmissão.

[!code-cs[EmptyStateHandlers](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetEmptyStateHandlers)]

## <a name="media-casting-with-a-custom-device-picker"></a>Transmissão de mídia com um seletor de dispositivo personalizado

A seção a seguir descreve como criar seu próprio seletor de dispositivo de transmissão da interface do usuário, enumerar os dispositivos de transmissão e inicializar a conexão do seu código.

Para enumerar os dispositivos de transmissão disponíveis, inclua o namespace do [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) em seu projeto.

[!code-cs[EnumerationNamespace](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetEnumerationNamespace)]

Adicione os seguintes controles à página XAML para implementar a interface do usuário rudimentar para este exemplo:

-   Um botão para iniciar o observador de dispositivo que procura por dispositivos de transmissão disponíveis.
-   Um controle [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) para fornecer comentários ao usuário os quais a transmissão já está em andamento.
-   Um [**ListBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox) para listar os dispositivos de transmissão descobertos. Defina um [**ItemTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) para o controle para que possamos atribuir os objetos de dispositivos de transmissão diretamente para o controle e ainda exibir a propriedade [**FriendlyName**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingdevice.friendlyname).
-   Um botão para permitir que o usuário desconecte o dispositivo de transmissão.

[!code-xml[CustomPickerXAML](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetCustomPickerXAML)]

Em seu code-behind, declare variáveis de membro para o [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) e o [**CastingConnection**](https://docs.microsoft.com/uwp/api/Windows.Media.Casting.CastingConnection).

[!code-cs[DeclareDeviceWatcher](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetDeclareDeviceWatcher)]

No manipulador **Click** para o *startWatcherButton*, primeiro atualize a interface do usuário, desativando o botão e tornando o anel de progresso ativo enquanto a enumeração do dispositivo está em andamento. Desmarque a caixa de listagem dos dispositivos de transmissão.

Em seguida, crie um inspetor de dispositivos chamando [**DeviceInformation.CreateWatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher). Esse método pode ser usado para assistir a muitos tipos diferentes de dispositivos. Especificar que você deseja ver dispositivos que suportam transmissão de vídeo usando a cadeia de caracteres do seletor de dispositivo retornado como [**CastingDevice.GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingdevice.getdeviceselector).

Por fim, registre manipuladores de eventos para os eventos [**Added**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added), [**Removed**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.removed), [**EnumerationCompleted**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted) e [**Stopped**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.stopped).

[!code-cs[StartWatcherButtonClick](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetStartWatcherButtonClick)]

O evento **Added** é acionado quando um novo dispositivo é descoberto pelo inspetor. No manipulador para este evento, crie um novo objeto [**CastingDevice**](https://docs.microsoft.com/uwp/api/Windows.Media.Casting.CastingDevice) chamando [**CastingDevice.FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingdevice.fromidasync) e passando a ID do dispositivo descoberto para transmissão, que está contido no objeto **DeviceInformation** passado para o manipulador.

Adicione o **CastingDevice** para o dispositivo de transmissão **ListBox** para que o usuário possa selecioná-lo. Devido ao [**ItemTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) definido em XAML, a propriedade [**FriendlyName**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingdevice.friendlyname) será usada como o texto do item na caixa de listagem. Como esse manipulador de eventos não é chamado no thread da interface do usuário, você deve atualizar a interface do usuário de dentro de uma chamada para [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync).

[!code-cs[WatcherAdded](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherAdded)]

O evento **Removed** é acionado quando o inspetor detecta que um dispositivo de transmissão não está mais presente. Compare a propriedade do ID do objeto **Added** passado para o manipulador para o ID de cada **Added** na coleção da caixa de listagem [**Items**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items). Se a ID corresponde, remova esse objeto da coleção. Novamente, como a interface do usuário está sendo atualizada, essa chamada deve ser feita de dentro do **RunAsync**.

[!code-cs[WatcherRemoved](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherRemoved)]

O evento **EnumerationCompleted** é acionado quando o inspetor concluiu a detecção de dispositivos. No manipulador para esse evento, atualize a interface do usuário para permitir que o usuário saiba que a enumeração do dispositivo foi concluída e pare o observador de dispositivo chamando [**Stop**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.stop).

[!code-cs[WatcherEnumerationCompleted](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherEnumerationCompleted)]

O evento Stopped é gerado quando o inspetor de dispositivo tiver terminado de parar. No manipulador para este evento, interrompa o controle do [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) e reabilite o *startWatcherButton* para que o usuário possa reiniciar o processo de enumeração do dispositivo.

[!code-cs[WatcherStopped](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherStopped)]

Quando o usuário seleciona um dos dispositivos de transmissão na caixa de listagem, o evento [**SelectionChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) é acionado. É dentro desse manipulador que a conexão de transmissão será criada e a transmissão será iniciada.

Primeiro, verifique se que o inspetor de dispositivos é interrompido para que a enumeração de dispositivo não esteja interferindo na transmissão de mídia. Crie uma conexão de transmissão chamando [**CreateCastingConnection**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingdevice.createcastingconnection) sobre o objeto **CastingDevice** selecionado pelo usuário. Adicione os manipuladores de eventos para os eventos [**StateChanged**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingconnection.statechanged) e [**ErrorOccurred**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingconnection.erroroccurred).

Inicie a transmissão de mídia chamando [**RequestStartCastingAsync**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingconnection.requeststartcastingasync), passando na origem da transmissão retornada pela chamada do método **MediaPlayer**[**GetAsCastingSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaelement.getascastingsource). Por fim, torne o botão desconectar visível para permitir que o usuário interrompa a transmissão de mídia.

[!code-cs[SelectionChanged](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetSelectionChanged)]

No manipulador de alteração de estado, a ação executada depende do novo estado da conexão da transmissão:

-   Se o estado for **Connected** ou **Rendering**, verifique se o controle do **ProgressRing** está inativo e o botão desconectar está visível.
-   Se o estado for **Disconnected**, desmarque o dispositivo de transmissão atual na caixa de listagem, deixe o controle do **ProgressRing** inativo e oculte o botão desconectar.
-   Se o estado for **Connecting**, deixe o controle do **ProgressRing** ativo e oculte o botão desconectar.
-   Se o estado for **Disconnecting**, deixe o controle do **ProgressRing** ativo e oculte o botão desconectar.

[!code-cs[StateChanged](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetStateChanged)]

No manipulador para o evento **ErrorOccurred**, atualize sua interface do usuário para permitir que o usuário saiba que ocorreu um erro de transmissão e desmarque o objeto **CastingDevice** atual na caixa de listagem.

[!code-cs[ErrorOccurred](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetErrorOccurred)]

Por fim, implemente o manipulador para o botão desconectar. Pare a transmissão de mídia e desconecte o dispositivo de transmissão chamando o método **CastingConnection** do objeto [**DisconnectAsync**](https://docs.microsoft.com/uwp/api/windows.media.casting.castingconnection.disconnectasync). Essa chamada deve ser enviada para o thread da interface do usuário chamando [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync).

[!code-cs[DisconnectButton](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetDisconnectButton)]

 

 




