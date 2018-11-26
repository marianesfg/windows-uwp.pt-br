---
ms.assetid: 40B97E0C-EB1B-40C2-A022-1AB95DFB085E
description: Este artigo mostra como converter mídia em dispositivos remotos de um aplicativo Universal do Windows.
title: Transmissão de mídia
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9e4b794e560c213e5c3796b11dd1a5fd77a98506
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7698515"
---
# <a name="media-casting"></a>Transmissão de mídia



Este artigo mostra como converter mídia em dispositivos remotos de um aplicativo Universal do Windows.

## <a name="built-in-media-casting-with-mediaplayerelement"></a>Transmissão de mídia integrada com o MediaPlayerElement

A maneira mais simples para converter a mídia de um Aplicativo Universal do Windows é usar a funcionalidade de transmissão interna do controle do [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement).

Para permitir que o usuário abra um arquivo de vídeo a ser reproduzido no controle do **MediaPlayerElement**, adicione os seguintes namespaces ao seu projeto.

[!code-cs[BuiltInCastingUsing](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetBuiltInCastingUsing)]

No arquivo XAML do seu aplicativo, adicione um **MediaPlayerElement** e defina [**AreTransportControlsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn298977) como verdadeiro.

[!code-xml[MediaElement](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetMediaElement)]

Adicione um botão para permitir que o usuário inicie selecionando um arquivo.

[!code-xml[OpenButton](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetOpenButton)]

No manipulador de eventos [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) para o botão, crie uma nova instância do [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847), adicione tipos de arquivo de vídeo à coleção do [**FileTypeFilter**](https://msdn.microsoft.com/library/windows/apps/br207850) e defina o local inicial para a biblioteca de vídeos do usuário.

Chame [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) para iniciar a caixa de diálogo do seletor de arquivos. Quando esse método retorna, o resultado é um objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) que representa o arquivo de vídeo. Verifique se o arquivo não é nulo, o que acontecerá se o usuário cancelar a operação de seleção. Chame o método [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227221.aspx) do arquivo para obter um [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241731) para o arquivo. Por fim, crie um novo objeto **MediaSource** com base no arquivo selecionado chamando [**CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909) e o atribua ao objeto **MediaPlayerElement**, propriedade [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.Source), para fazer do arquivo de vídeo a origem do controle.

[!code-cs[OpenButtonClick](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetOpenButtonClick)]

Depois que o vídeo é carregado no **MediaPlayerElement**, o usuário pode simplesmente pressionar o botão de transmissão nos controles de transporte para iniciar uma caixa de diálogo interna que permita a eles escolher um dispositivo para o qual a mídia carregada será convertida.

![Botão de transmissão mediaelement](images/media-element-casting-button.png)

> [!NOTE] 
> A partir do Windows 10, versão 1607, é recomendável que você use a classe **MediaPlayer** para reproduzir itens de mídia. O **MediaPlayerElement** é um controle XAML leve que é usado para renderizar o conteúdo de um **MediaPlayer** em uma página XAML. O controle **MediaElement** continua a ser suportado para compatibilidade com versões anteriores. Para obter mais informações sobre como usar o **MediaPlayer** e o **MediaPlayerElement** para reproduzir conteúdo de mídia, consulte [Reproduzir áudio e vídeo com o MediaPlayer](play-audio-and-video-with-mediaplayer.md). Para obter informações sobre como usar o **MediaSource** e as APIs relacionadas para trabalhar com conteúdo de mídia, consulte [Itens de mídia, playlists e faixas](media-playback-with-mediasource.md).

## <a name="media-casting-with-the-castingdevicepicker"></a>Transmissão de mídia com o CastingDevicePicker

Uma segunda maneira de converter mídia em um dispositivo é usar o [**CastingDevicePicker**](https://msdn.microsoft.com/library/windows/apps/dn972525). Para usar essa classe, inclua o namespace [**Windows.Media.Casting**](https://msdn.microsoft.com/library/windows/apps/dn972568) em seu projeto.

[!code-cs[CastingNamespace](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetCastingNamespace)]

Declare uma variável do membro para o objeto **CastingDevicePicker**.

[!code-cs[DeclareCastingPicker](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetDeclareCastingPicker)]

Quando sua página for inicializada, crie uma nova instância do seletor de transmissão e defina o [**Filter**](https://msdn.microsoft.com/library/windows/apps/dn972540) como a propriedade [**SupportsVideo**](https://msdn.microsoft.com/library/windows/apps/dn972526) para indicar que os dispositivos de transmissão listados pelo seletor devem dar suporte a vídeo. Registre um manipulador para o evento [**CastingDeviceSelected**](https://msdn.microsoft.com/library/windows/apps/dn972539), que é acionado quando o usuário seleciona um dispositivo para transmissão.

[!code-cs[InitCastingPicker](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetInitCastingPicker)]

No seu arquivo XAML, adicione um botão para permitir que o usuário inicie o seletor.

[!code-xml[CastPickerButton](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetCastPickerButton)]

No manipulador de eventos **Click** para o botão, chame [**TransformToVisual**](https://msdn.microsoft.com/library/windows/apps/br208986) para obter a transformação de um elemento de interface do usuário em relação a outro. Neste exemplo, a transformação é a posição do botão do seletor de conversão em relação à raiz visual da janela do aplicativo. Chame o método [**Show**](https://msdn.microsoft.com/library/windows/apps/dn972542) do objeto [**CastingDevicePicker**](https://msdn.microsoft.com/library/windows/apps/dn972525) para iniciar a caixa de diálogo do seletor de transmissão. Especifique o local e as dimensões do botão do seletor de conversão para que o sistema possa fazer a caixa de diálogo sair do botão que o usuário pressionou.

[!code-cs[CastPickerButtonClick](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetCastPickerButtonClick)]

No manipulador de eventos do **CastingDeviceSelected**, chame o método [**CreateCastingConnection**](https://msdn.microsoft.com/library/windows/apps/dn972547) da propriedade [**SelectedCastingDevice**](https://msdn.microsoft.com/library/windows/apps/dn972546) dos argumentos do evento, que representa o dispositivo de transmissão selecionado pelo usuário. Registre manipuladores para os eventos [**ErrorOccurred**](https://msdn.microsoft.com/library/windows/apps/dn972519) e [**StateChanged**](https://msdn.microsoft.com/library/windows/apps/dn972523). Por fim, chame [**RequestStartCastingAsync**](https://msdn.microsoft.com/library/windows/apps/dn972520) para começar a conversão, passando o resultado para o controle **MediaPlayerElement**, objeto **MediaPlayer**, método [**GetAsCastingSource**](https://msdn.microsoft.com/library/windows/apps/dn920012), para especificar que a mídia a ser convertida é o conteúdo do **MediaPlayer** associado ao **MediaPlayerElement**.

> [!NOTE] 
> A conexão de transmissão deve ser iniciada no thread da interface do usuário. Como o **CastingDeviceSelected** não é chamado no thread da interface do usuário, você deve colocar essas chamadas dentro de uma chamada para [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317), o que faz com que elas sejam chamadas no thread da interface do usuário.

[!code-cs[CastingDeviceSelected](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetCastingDeviceSelected)]

Nos manipuladores de eventos **ErrorOccurred** e **StateChanged**, você deve atualizar sua interface do usuário para informar o usuário sobre o status atual da transmissão. Esses eventos são abordados em detalhes na seção a seguir sobre a criação de um seletor de dispositivo personalizado de transmissão.

[!code-cs[EmptyStateHandlers](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetEmptyStateHandlers)]

## <a name="media-casting-with-a-custom-device-picker"></a>Transmissão de mídia com um seletor de dispositivo personalizado

A seção a seguir descreve como criar seu próprio seletor de dispositivo de transmissão da interface do usuário, enumerar os dispositivos de transmissão e inicializar a conexão do seu código.

Para enumerar os dispositivos de transmissão disponíveis, inclua o namespace do [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459) em seu projeto.

[!code-cs[EnumerationNamespace](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetEnumerationNamespace)]

Adicione os seguintes controles à página XAML para implementar a interface do usuário rudimentar para este exemplo:

-   Um botão para iniciar o observador de dispositivo que procura por dispositivos de transmissão disponíveis.
-   Um controle [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) para fornecer comentários ao usuário os quais a transmissão já está em andamento.
-   Um [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868) para listar os dispositivos de transmissão descobertos. Defina um [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/br242830) para o controle para que possamos atribuir os objetos de dispositivos de transmissão diretamente para o controle e ainda exibir a propriedade [**FriendlyName**](https://msdn.microsoft.com/library/windows/apps/dn972549).
-   Um botão para permitir que o usuário desconecte o dispositivo de transmissão.

[!code-xml[CustomPickerXAML](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetCustomPickerXAML)]

Em seu code-behind, declare variáveis de membro para o [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/br225446) e o [**CastingConnection**](https://msdn.microsoft.com/library/windows/apps/dn972510).

[!code-cs[DeclareDeviceWatcher](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetDeclareDeviceWatcher)]

No manipulador **Click** para o *startWatcherButton*, primeiro atualize a interface do usuário, desativando o botão e tornando o anel de progresso ativo enquanto a enumeração do dispositivo está em andamento. Desmarque a caixa de listagem dos dispositivos de transmissão.

Em seguida, crie um inspetor de dispositivos chamando [**DeviceInformation.CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/br225427). Esse método pode ser usado para assistir a muitos tipos diferentes de dispositivos. Especificar que você deseja ver dispositivos que suportam transmissão de vídeo usando a cadeia de caracteres do seletor de dispositivo retornado como [**CastingDevice.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn972551).

Por fim, registre manipuladores de eventos para os eventos [**Added**](https://msdn.microsoft.com/library/windows/apps/br225450), [**Removed**](https://msdn.microsoft.com/library/windows/apps/br225453), [**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/br225451) e [**Stopped**](https://msdn.microsoft.com/library/windows/apps/br225457).

[!code-cs[StartWatcherButtonClick](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetStartWatcherButtonClick)]

O evento **Added** é acionado quando um novo dispositivo é descoberto pelo inspetor. No manipulador para este evento, crie um novo objeto [**CastingDevice**](https://msdn.microsoft.com/library/windows/apps/dn972524) chamando [**CastingDevice.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/dn972550) e passando a ID do dispositivo descoberto para transmissão, que está contido no objeto **DeviceInformation** passado para o manipulador.

Adicione o **CastingDevice** para o dispositivo de transmissão **ListBox** para que o usuário possa selecioná-lo. Devido ao [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/br242830) definido em XAML, a propriedade [**FriendlyName**](https://msdn.microsoft.com/library/windows/apps/dn972549) será usada como o texto do item na caixa de listagem. Como esse manipulador de eventos não é chamado no thread da interface do usuário, você deve atualizar a interface do usuário de dentro de uma chamada para [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317).

[!code-cs[WatcherAdded](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherAdded)]

O evento **Removed** é acionado quando o inspetor detecta que um dispositivo de transmissão não está mais presente. Compare a propriedade do ID do objeto **Added** passado para o manipulador para o ID de cada **Added** na coleção da caixa de listagem [**Items**](https://msdn.microsoft.com/library/windows/apps/br242823). Se a ID corresponde, remova esse objeto da coleção. Novamente, como a interface do usuário está sendo atualizada, essa chamada deve ser feita de dentro do **RunAsync**.

[!code-cs[WatcherRemoved](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherRemoved)]

O evento **EnumerationCompleted** é acionado quando o inspetor concluiu a detecção de dispositivos. No manipulador para esse evento, atualize a interface do usuário para permitir que o usuário saiba que a enumeração do dispositivo foi concluída e pare o observador de dispositivo chamando [**Stop**](https://msdn.microsoft.com/library/windows/apps/br225456).

[!code-cs[WatcherEnumerationCompleted](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherEnumerationCompleted)]

O evento Stopped é gerado quando o inspetor de dispositivo tiver terminado de parar. No manipulador para este evento, interrompa o controle do [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) e reabilite o *startWatcherButton* para que o usuário possa reiniciar o processo de enumeração do dispositivo.

[!code-cs[WatcherStopped](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherStopped)]

Quando o usuário seleciona um dos dispositivos de transmissão na caixa de listagem, o evento [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) é acionado. É dentro desse manipulador que a conexão de transmissão será criada e a transmissão será iniciada.

Primeiro, verifique se que o inspetor de dispositivos é interrompido para que a enumeração de dispositivo não esteja interferindo na transmissão de mídia. Crie uma conexão de transmissão chamando [**CreateCastingConnection**](https://msdn.microsoft.com/library/windows/apps/dn972547) sobre o objeto **CastingDevice** selecionado pelo usuário. Adicione os manipuladores de eventos para os eventos [**StateChanged**](https://msdn.microsoft.com/library/windows/apps/dn972523) e [**ErrorOccurred**](https://msdn.microsoft.com/library/windows/apps/dn972519).

Inicie a transmissão de mídia chamando [**RequestStartCastingAsync**](https://msdn.microsoft.com/library/windows/apps/dn972520), passando na origem da transmissão retornada pela chamada do método **MediaPlayer** [**GetAsCastingSource**](https://msdn.microsoft.com/library/windows/apps/dn920012). Por fim, torne o botão desconectar visível para permitir que o usuário interrompa a transmissão de mídia.

[!code-cs[SelectionChanged](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetSelectionChanged)]

No manipulador de alteração de estado, a ação executada depende do novo estado da conexão da transmissão:

-   Se o estado for **Connected** ou **Rendering**, verifique se o controle do **ProgressRing** está inativo e o botão desconectar está visível.
-   Se o estado for **Disconnected**, desmarque o dispositivo de transmissão atual na caixa de listagem, deixe o controle do **ProgressRing** inativo e oculte o botão desconectar.
-   Se o estado for **Connecting**, deixe o controle do **ProgressRing** ativo e oculte o botão desconectar.
-   Se o estado for **Disconnecting**, deixe o controle do **ProgressRing** ativo e oculte o botão desconectar.

[!code-cs[StateChanged](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetStateChanged)]

No manipulador para o evento **ErrorOccurred**, atualize sua interface do usuário para permitir que o usuário saiba que ocorreu um erro de transmissão e desmarque o objeto **CastingDevice** atual na caixa de listagem.

[!code-cs[ErrorOccurred](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetErrorOccurred)]

Por fim, implemente o manipulador para o botão desconectar. Pare a transmissão de mídia e desconecte o dispositivo de transmissão chamando o método **CastingConnection** do objeto [**DisconnectAsync**](https://msdn.microsoft.com/library/windows/apps/dn972518). Essa chamada deve ser enviada para o thread da interface do usuário chamando [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317).

[!code-cs[DisconnectButton](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetDisconnectButton)]

 

 




