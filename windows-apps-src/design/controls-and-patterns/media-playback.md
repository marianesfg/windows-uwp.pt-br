---
Description: O media player é usado para exibir e ouvir vídeo, áudio e imagens.
title: Media player
ms.assetid: 9AABB5DE-1D81-4791-AB47-7F058F64C491
dev.assetid: AF2F2008-9B53-430C-BBC3-8888F631B0B0
label: Media player
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 49058568acccfa32c9c354a7e2570a5734c0c577
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081688"
---
# <a name="media-player"></a>Media player



O media player é usado para exibir e ouvir vídeo e áudio. A reprodução de mídia pode ser embutida (inserida em uma página ou com um grupo de outros controles) ou em um modo de exibição de tela inteira dedicado. Você pode modificar o conjunto de botões do player, alterar a tela de fundo da barra de controle e organizar os layouts como quiser. Apenas lembre-se de que os usuários esperam um conjunto básico de controles (reproduzir/pausar, retroceder, avançar).

![Elemento de player de mídia com controles de transporte](images/controls/mtc_double_video_inprod.png)

> **APIs importantes**: [classe MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement), [classe MediaTransportControls](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediatransportcontrols)


> [!NOTE]
> O **MediaPlayerElement** só está disponível no Windows 10, versão 1607, e posterior. Se estiver desenvolvendo um aplicativo para uma versão anterior do Windows 10, você precisará usar [MediaElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement). Todas as recomendações nesta página também se aplicam a MediaElement.

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Use um media player quando você quiser reproduzir áudio ou vídeo em seu aplicativo. Para exibir uma coleção de imagens, use um [Modo de exibição de inversão](flipview.md).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para abri-lo e conferir o <a href="xamlcontrolsgallery:/item/MediaPlayerElement">MediaPlayerElement</a> ou <a href="xamlcontrolsgallery:/item/MediaPlayer">MediaPlayer</a> em ação.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Um player de mídia no aplicativo Introdução do Windows 10.

![Um elemento de mídia no aplicativo Introdução do Windows 10](images/control-examples/mtc_getstarted_example.png)

## <a name="create-a-media-player"></a>Criar um media player
Adicione mídia ao aplicativo criando um objeto [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) em XAML e defina uma [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) como uma [MediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) que aponta para um arquivo de áudio ou vídeo.

Este XAML cria o [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) e define sua propriedade [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) como o URI de um arquivo de vídeo que seja local para o aplicativo. O **MediaPlayerElement** começa quando a página é carregada. É possível suprimir a reprodução imediata de mídia definindo a propriedade [AutoPlay](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.autoplay) como **false**.

```xaml
<MediaPlayerElement x:Name="mediaSimple"
                    Source="ms-appx:///Videos/video1.mp4"
                    Width="400" AutoPlay="True"/>
```

Este XAML cria um [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) com os controles de transporte internos habilitados e a propriedade [AutoPlay](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.autoplay) definida como **false**.


```xaml
<MediaPlayerElement x:Name="mediaPlayer"
                    Source="ms-appx:///Videos/video1.mp4"
                    Width="400"
                    AutoPlay="False"
                    AreTransportControlsEnabled="True"/>
```

### <a name="media-transport-controls"></a>Controles de transporte de mídia
[MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) tem controles de transporte internos que manipulam recursos de reprodução, parada, pausa, volume, ativação de mudo, busca/progresso, legendas ocultas e seleção de faixa de áudio. Para habilitar esses controles, defina [AreTransportControlsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.AreTransportControlsEnabled) como **true**. Para desabilitá-los, defina **AreTransportControlsEnabled** como **false**. Os controles de transporte são representados pela classe [MediaTransportControls](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls). Você pode usar os controles de transporte como estão ou personalizá-los de várias maneiras. Para saber mais, confira a referência de classe [MediaTransportControls](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls) e [Criar controles personalizados de transporte](custom-transport-controls.md).

Os controles de transporte dão suporte a layouts de linha única e dupla. O primeiro exemplo é um layout de linha única, com o botão Reproduzir/Pausar localizado à esquerda da linha do tempo de mídia. Este layout é mais reservado para telas de reprodução de mídia embutidas e compactas.

![Exemplo de controles MTC, linha única](images/controls/mtc_single_inprod_02.png)

O layout de controles de linha dupla (abaixo) é recomendado para a maioria dos cenários de uso, especialmente em telas maiores. Esse layout oferece mais espaço para controles e facilita a operação da linha do tempo pelo usuário.

![Exemplo de controles MTC no telefone, linha dupla](images/controls/mtc_double_inprod.png)

**Controles de transporte de mídia do sistema**

[MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) é integrado automaticamente aos controles de transporte de mídia do sistema. Os controles de transporte de mídia do sistema são os controles exibidos quando teclas de mídia de hardware são pressionadas, como os botões de mídia em teclados. Para saber mais, confira [SystemMediaTransportControls](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls).

> **Observação**&nbsp;&nbsp; [MediaElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) não se integra automaticamente aos controles de transporte de mídia do sistema, logo, você deve conectá-los por conta própria. Para obter mais informações, consulte [Controles de transporte de mídia do sistema](https://docs.microsoft.com/windows/uwp/audio-video-camera/system-media-transport-controls).


### <a name="set-the-media-source"></a>Definir a origem da mídia
Para reproduzir arquivos na rede ou arquivos inseridos com o aplicativo, defina a propriedade [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) como uma [MediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) com o caminho do arquivo.

**Dica**  para abrir arquivos da Internet, você precisa declarar a funcionalidade **Internet (Client)** no manifesto do aplicativo (Package.appxmanifest). Para obter mais informações sobre como declarar recursos, consulte [Declarações de recursos de aplicativos](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).

 

Esse código tenta definir a propriedade [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) do [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) definida em XAML como o caminho de um arquivo inserido em uma [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox).

```xaml
<TextBox x:Name="txtFilePath" Width="400"
         FontSize="20"
         KeyUp="TxtFilePath_KeyUp"
         Header="File path"
         PlaceholderText="Enter file path"/>
```

```csharp
private void TxtFilePath_KeyUp(object sender, KeyRoutedEventArgs e)
{
    if (e.Key == Windows.System.VirtualKey.Enter)
    {
        TextBox tbPath = sender as TextBox;

        if (tbPath != null)
        {
            LoadMediaFromString(tbPath.Text);
        }
    }
}

private void LoadMediaFromString(string path)
{
    try
    {
        Uri pathUri = new Uri(path);
        mediaPlayer.Source = MediaSource.CreateFromUri(pathUri);
    }
    catch (Exception ex)
    {
        if (ex is FormatException)
        {
            // handle exception.
            // For example: Log error or notify user problem with file
        }
    }
}
```

Para definir a origem da mídia para um arquivo de mídia inserido no aplicativo, inicialize um [URI](https://docs.microsoft.com/uwp/api/windows.foundation.uri) com o caminho com o prefixo **ms-appx:///** , crie uma [MediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) com o URI e defina a [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) como o URI. Por exemplo, para um arquivo chamado **video1. mp4** que está em uma subpasta **Videos**, o caminho ficaria: **ms-appx:///Videos/video1.mp4**

Esse código define a propriedade [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) do [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) definido anteriormente em XAML para **ms-appx:///Videos/video1.mp4**.

```csharp
private void LoadEmbeddedAppFile()
{
    try
    {
        Uri pathUri = new Uri("ms-appx:///Videos/video1.mp4");
        mediaPlayer.Source = MediaSource.CreateFromUri(pathUri);
    }
    catch (Exception ex)
    {
        if (ex is FormatException)
        {
            // handle exception.
            // For example: Log error or notify user problem with file
        }
    }
}
```

### <a name="open-local-media-files"></a>Abrir arquivos de mídia local
Para abrir arquivos no sistema local ou no OneDrive, você pode usar o [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) para obter o arquivo e [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) para definir a origem da mídia, ou ainda acessar programaticamente as pastas de mídia do usuário.

Se o aplicativo precisar ter acesso sem a interação do usuário com as pastas **Música** ou **Vídeo**, por exemplo, se você precisar enumerar todos os arquivos de música ou vídeo na coleção do usuário e exibi-los em seu aplicativo, declare os recursos **Biblioteca de Músicas** e **Biblioteca de Vídeos**. Para obter mais informações, consulte [arquivos e pastas nas bibliotecas de música, imagens e vídeos](https://docs.microsoft.com/windows/uwp/files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries).

O [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) não precisa de recursos especiais para acessar os arquivos no sistema de arquivos local, como as pastas **Música** ou **Vídeo** do usuário, já que o usuário tem controle total sobre qual arquivo está sendo acessado. Em relação à segurança e à privacidade, é melhor minimizar as funcionalidades que seu aplicativo usa.

**Para abrir a mídia local usando FileOpenPicker**

1.  Chame [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) para permitir que o usuário escolha um arquivo de mídia.

    Use a classe [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) para escolher um arquivo de mídia. Defina o [FileTypeFilter](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.filetypefilter) para especificar quais tipos de arquivo o **FileOpenPicker** exibe. Chame [PickSingleFileAsync](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) para iniciar o seletor de arquivos e obter o arquivo.

2.  Use uma [MediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) para definir o arquivo de mídia escolhido como o [MediaPlayerElement.Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source).

    Para usar a [StorageFile](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) retornada da [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker), você precisa chamar o método [CreateFromStorageFile](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromstoragefile) em [MediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) e defini-lo como a [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) de [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement). Em seguida, chame [Play](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.play) no [MediaPlayerElement.MediaPlayer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.mediaplayer) para iniciar a mídia.


Este exemplo mostra como usar o [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) para escolher um arquivo e definir o arquivo como o [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) de um [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement).

```xaml
<MediaPlayerElement x:Name="mediaPlayer"/>
...
<Button Content="Choose file" Click="Button_Click"/>
```

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    await SetLocalMedia();
}

async private System.Threading.Tasks.Task SetLocalMedia()
{
    var openPicker = new Windows.Storage.Pickers.FileOpenPicker();

    openPicker.FileTypeFilter.Add(".wmv");
    openPicker.FileTypeFilter.Add(".mp4");
    openPicker.FileTypeFilter.Add(".wma");
    openPicker.FileTypeFilter.Add(".mp3");

    var file = await openPicker.PickSingleFileAsync();

    // mediaPlayer is a MediaPlayerElement defined in XAML
    if (file != null)
    {
        mediaPlayer.Source = MediaSource.CreateFromStorageFile(file);

        mediaPlayer.MediaPlayer.Play();
    }
}
```

### <a name="set-the-poster-source"></a>Definir a origem do cartaz
Você pode usar a propriedade [PosterSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.PosterSource) para fornecer seu [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) com uma representação visual antes de a mídia ser carregada. Uma **PosterSource** é uma imagem, como uma captura de tela ou poster de filme, que é exibida no lugar da mídia. A **PosterSource** é exibida nas seguintes situações:

-   Quando uma fonte válida não está definida. Por exemplo, [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) não está definido, **Source** estava definida como **Null**, ou a origem é inválida (como quando um evento [MediaFailed](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.mediafailed) ocorre).
-   Enquanto a mídia está sendo carregada. Por exemplo, uma origem válida está definida, mas o evento [MediaOpened](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.mediaopened) ainda não ocorreu.
-   Quando há streaming de mídia para outro dispositivo.
-   Quando a mídia é somente áudio.

Aqui está um [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) com seu [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) definido como uma faixa de álbum, e [PosterSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.PosterSource) definido como uma imagem da capa do álbum.

```xaml
<MediaPlayerElement Source="ms-appx:///Media/Track1.mp4" PosterSource="Media/AlbumCover.png"/>
```

### <a name="keep-the-devices-screen-active"></a>Mantenha a tela do dispositivo ativa
Normalmente, um dispositivo reduz a luminosidade da tela (e depois a desliga) para economizar bateria quando o usuário está longe, mas os aplicativos de vídeo precisam manter a tela ligada para que o usuário possa vê-lo. Para evitar que a exibição seja desativada quando não for mais detectada atividade de usuário, como quando um aplicativo estiver reproduzindo um vídeo, você pode chamar [DisplayRequest.RequestActive](https://docs.microsoft.com/uwp/api/windows.system.display.displayrequest.requestactive). A classe [DisplayRequest](https://docs.microsoft.com/uwp/api/Windows.System.Display.DisplayRequest) permite que você informe ao Windows que mantenha a tela ligada para que o usuário possa conferir o vídeo.

Para economizar energia e a vida útil da bateria, chame [DisplayRequest.RequestRelease](https://docs.microsoft.com/uwp/api/windows.system.display.displayrequest.requestrelease) para liberar a solicitação de exibição quando ela não for mais necessária. O Windows desativa automaticamente as solicitações de exibição ativas do aplicativo quando o aplicativo for movido para fora da tela e as reativa quando ele voltar ao primeiro plano.

Consulte algumas situações em que você deve liberar a solicitação de exibição:

-   Por exemplo, a reprodução do vídeo foi pausada por uma ação do usuário, buffer ou ajuste devido à largura de banda limitada.
-   A reprodução foi interrompida. Por exemplo, a reprodução do vídeo ou a apresentação acabou.
-   Erro na reprodução. Por exemplo, problemas com a conectividade de rede ou um arquivo corrompido.

> **Observação**&nbsp;&nbsp; Se [MediaPlayerElement.IsFullWindow](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.IsFullWindow) for definido como true e a mídia estiver em execução, a exibição será automaticamente impedida de desativar.

**Para manter a tela ativa**

1.  Crie uma variável [DisplayRequest](https://docs.microsoft.com/uwp/api/Windows.System.Display.DisplayRequest) global. Inicialize-a com nulo.
```csharp
// Create this variable at a global scope. Set it to null.
private DisplayRequest appDisplayRequest = null;
```

2.  Chame [RequestActive](https://docs.microsoft.com/uwp/api/windows.system.display.displayrequest.requestactive) para notificar o Windows que o aplicativo requer que a tela fique ligada.

3.  Chame [RequestRelease](https://docs.microsoft.com/uwp/api/windows.system.display.displayrequest.requestrelease) para liberar a solicitação de exibição sempre que a reprodução do vídeo por parada, pausada ou interrompida por um erro de reprodução. Quando o seu aplicativo não tem mais solicitações de exibição ativas, o Windows economiza a duração da bateria ao reduzir a luminosidade da tela (e, depois, desligando-a) quando o dispositivo não estiver em uso.

    Cada [MediaPlayerElement.MediaPlayer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.mediaplayer) tem uma [PlaybackSession](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.playbacksession) do tipo [MediaPlaybackSession](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession) que controla diversos aspectos da reprodução de mídia como [PlaybackRate](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.playbackrate), [PlaybackState](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.playbackstate) e [Position](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.position). Aqui, você usa o evento [PlaybackStateChanged](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.playbackstatechanged) em [MediaPlayer.PlaybackSession](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.playbacksession) para detectar situações em que deve liberar a solicitação de exibição. Em seguida, use a propriedade [NaturalVideoHeight](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.naturalvideoheight) para determinar se um arquivo de áudio ou vídeo está sendo executado, e mantenha a tela ativa somente se o vídeo estiver sendo executado.

    ```xaml
    <MediaPlayerElement x:Name="mpe" Source="ms-appx:///Media/video1.mp4"/>
    ```

    ```csharp
    protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        mpe.MediaPlayer.PlaybackSession.PlaybackStateChanged += MediaPlayerElement_CurrentStateChanged;
        base.OnNavigatedTo(e);
    }

    private void MediaPlayerElement_CurrentStateChanged(object sender, RoutedEventArgs e)
    {
        MediaPlaybackSession playbackSession = sender as MediaPlaybackSession;
        if (playbackSession != null && playbackSession.NaturalVideoHeight != 0)
        {
            if(playbackSession.PlaybackState == MediaPlaybackState.Playing)
            {
                if(appDisplayRequest == null)
                {
                    // This call creates an instance of the DisplayRequest object
                    appDisplayRequest = new DisplayRequest();
                    appDisplayRequest.RequestActive();
                }
            }
            else // PlaybackState is Buffering, None, Opening or Paused
            {
                if(appDisplayRequest != null)
                {
                      // Deactivate the displayr request and set the var to null
                      appDisplayRequest.RequestRelease();
                      appDisplayRequest = null;
                }
            }
        }

    }
    ```

### <a name="control-the-media-player-programmatically"></a>Controlar o media player de forma programática
[MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) oferece inúmeras propriedades, métodos e eventos para controlar a reprodução de áudio e vídeo por meio da propriedade [MediaPlayerElement.MediaPlayer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.mediaplayer). Para uma listagem completa de propriedades, métodos e eventos, confira página de referência do [MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer).

### <a name="advanced-media-playback-scenarios"></a>Cenários de reprodução de mídia avançada
Para cenários de reprodução de mídia mais complexos como a execução de uma playlist, alternar idiomas de áudio ou criar faixas de metadados personalizadas define [MediaPlayerElement.Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) como [MediaPlaybackItem](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) ou [MediaPlaybackList](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacklist). Confira a página [Reprodução de mídia](https://docs.microsoft.com/windows/uwp/audio-video-camera/media-playback-with-mediasource) no Centro de Desenvolvimento para obter mais informações sobre como habilitar diversas funcionalidades de mídia avançada.

### <a name="enable-full-window-video-rendering"></a>Habilitar a renderização de vídeo da janela inteira

Defina a propriedade [IsFullWindow](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.isfullwindow) para habilitar ou desabilitar a renderização de janela inteira. Ao definir de forma programática a renderização de janela inteira em seu aplicativo, você sempre deve usar **IsFullWindow** em vez de fazer isso manualmente. **IsFullWindow** garante que as otimizações em nível de sistema sejam executadas, o que melhora o desempenho e a vida útil da bateria. Se a renderização de janela inteira não estiver configurada corretamente, essas otimizações não poderão ser habilitadas.

Este código cria um [AppBarButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) que alterna a renderização da janela inteira.

```xaml
<AppBarButton Icon="FullScreen"
              Label="Full Window"
              Click="FullWindow_Click"/>>
```

```csharp
private void FullWindow_Click(object sender, object e)
{
    mediaPlayer.IsFullWindow = !media.IsFullWindow;
}
```

### <a name="resize-and-stretch-video"></a>Redimensionar e ampliar vídeo

Use a propriedade [Stretch](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.stretch) para mudar a forma como o conteúdo e/ou a [PosterSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.postersource) preenche seu contêiner. Isso redimensiona e amplia o vídeo de acordo com o valor de [Stretch](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Stretch). Os estados de **Stretch** são parecidos com as configurações de tamanho de imagem em aparelhos de TV. Você pode enganchá-la em um botão para que o usuário possa escolher a configuração de sua preferência.

-   [None](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Stretch) mostra a resolução nativa do conteúdo em seu tamanho original.
-   [Uniform](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Stretch) preenche o máximo do espaço possível, mantendo a taxa de proporção e o conteúdo da imagem. Isso pode produzir barras pretas horizontais ou verticais nas bordas do vídeo. Isso é semelhante aos modos widescreen.
-   [UniformToFill](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Stretch) preenche todo o espaço, mantendo a taxa de proporção. Isso pode fazer parte da imagem ser cortada. Isso é semelhante aos modos de tela inteira.
-   [Fill](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Stretch) preenche todo o espaço, mas não mantém a taxa de proporção. A imagem não é cortada, mas pode ocorrer um alongamento. Isso é semelhante aos modos de alongamento.

![Valores de enumeração de alongamento](images/Image_Stretch.jpg)

Aqui, um [AppBarButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) é usado para percorrer as opções de [Stretch](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Stretch). Uma declaração **switch** verifica o estado atual da propriedade [Stretch](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaelement.stretch) e o define para o próximo valor na enumeração de **Stretch**. Isso permite ao usuário circular pelos vários estados de ampliação.

```xaml
<AppBarButton Icon="Switch"
              Label="Resize Video"
              Click="PictureSize_Click" />
```

```csharp
private void PictureSize_Click(object sender, RoutedEventArgs e)
{
    switch (mediaPlayer.Stretch)
    {
        case Stretch.Fill:
            mediaPlayer.Stretch = Stretch.None;
            break;
        case Stretch.None:
            mediaPlayer.Stretch = Stretch.Uniform;
            break;
        case Stretch.Uniform:
            mediaPlayer.Stretch = Stretch.UniformToFill;
            break;
        case Stretch.UniformToFill:
            mediaPlayer.Stretch = Stretch.Fill;
            break;
        default:
            break;
    }
}
```

### <a name="enable-low-latency-playback"></a>Habilitar a reprodução de baixa latência

Defina a propriedade [RealTimePlayback](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.realtimeplayback) como **true** em um [MediaPlayerElement.MediaPlayer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.mediaplayer) para habilitar o elemento do player de mídia a fim de reduzir a latência inicial da reprodução. Isso é essencial para aplicativos de comunicação bidirecionais e pode ser aplicável a alguns cenários de jogos. Esse modo consome mais recursos e energia.

Este exemplo cria um [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) e define [RealTimePlayback](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.realtimeplayback) como **true**.


```csharp
MediaPlayerElement mp = new MediaPlayerElement();
mp.MediaPlayer.RealTimePlayback = true;
```

## <a name="recommendations"></a>Recomendações

O player de mídia dá suporte a temas claras e escuros, mas o tema escuro proporciona uma experiência melhor para a maioria dos cenários de entretenimento. A tela de fundo escura oferece melhor contraste, particularmente em condições com pouca luz, e limita a interferência da barra de controle na experiência de visualização.

Durante a execução de conteúdo em vídeo, incentive uma experiência de visualização dedicada ao promover o modo de tela inteira em vez do modo embutido. A experiência de visualização em tela inteira é ideal, e as opções são restritas no modo embutido.

Se você tiver o estado real da tela ou estiver projetando para a experiência de 10 pés, acompanhe o layout de linha dupla. Ele oferece mais espaço para controles do que o layout de linha única compacto e é mais fácil de navegar com gamepad de 10 pés.

> **Observação**&nbsp;&nbsp; Visite o artigo [Projetando para TV e Xbox](../devices/designing-for-tv.md) para obter mais informações sobre como otimizar o aplicativo para a experiência de 10 pés.

Embora os controles padrão tenham sido otimizados para reprodução de mídia, você tem a capacidade de adicionar opções personalizadas de que precisa ao player de mídia para proporcionar a melhor experiência ao aplicativo. Visite [Criar controles personalizados de transporte](custom-transport-controls.md) para saber mais sobre como adicionar controles personalizados.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

- [Noções básicas de design de comandos para aplicativos UWP](https://docs.microsoft.com/windows/uwp/layout/commanding-basics)
- [Noções básicas de design de conteúdo para aplicativos UWP](https://docs.microsoft.com/windows/uwp/layout/content-basics)
