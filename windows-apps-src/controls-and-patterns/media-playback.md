---
author: Jwmsft
Description: O media player é usado para exibir e ouvir vídeo, áudio e imagens.
title: Media player
ms.assetid: 9AABB5DE-1D81-4791-AB47-7F058F64C491
dev.assetid: AF2F2008-9B53-430C-BBC3-8888F631B0B0
label: Media player
template: detail.hbs
---
# Media player

O media player é usado para exibir e ouvir vídeo, áudio e imagens. A reprodução de mídia pode ser embutida (inserida em uma página ou com um grupo de outros controles) ou em um modo de exibição de tela inteira dedicado. Você pode modificar o conjunto de botões do player, alterar a tela de fundo da barra de controle e organizar os layouts como quiser. Apenas lembre-se de que os usuários esperam um conjunto básico de controles (reproduzir/pausar, retroceder, avançar).

![Elemento de mídia com controles de transporte](images/controls/media-transport-controls.png)

<span class="sidebar_heading" style="font-weight: bold;">APIs importantes</span>

-   [**Classe MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)
-   [**Classe MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediatransportcontrols)

## Esse é o controle correto?

Use um media player quando você quiser reproduzir áudio ou vídeo em seu aplicativo. Para exibir uma coleção de imagens, use um [Modo de exibição de inversão](flipview.md).

## Exemplos

Um elemento de mídia no aplicativo do de Introdução do Windows 10.

![Um elemento de mídia no aplicativo do de Introdução do Windows 10](images/control-examples/media-element-getstarted.png)

## Criar um media player
Adicione mídia a seu aplicativo criando um objeto [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) em XAML e defina [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) como um URI (Uniform Resource Identifier) que aponta para um arquivo de áudio ou vídeo.

Este XAML cria o [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) e define sua propriedade [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) como o URI de um arquivo de vídeo que seja local para o aplicativo. O **MediaElement** começa quando a página é carregada. É possível suprimir a reprodução imediata de mídia definindo a propriedade [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/br227360) como **false**.

```xaml
<MediaElement x:Name="mediaSimple" 
              Source="Videos/video1.mp4" 
              Width="400" AutoPlay="False"/>
```

Este XAML cria [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) com os controles de transporte internos habilitados e a propriedade [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/br227360) definida como **false**.


```csharp
<MediaElement x:Name="mediaPlayer" 
              Source="Videos/video1.mp4" 
              Width="400" 
              AutoPlay="False"
              AreTransportControlsEnabled="True"/>
```

### Controles de transporte de mídia
MediaElement tem controles de transporte internos que manipulam recursos de reprodução, parada, pausa, volume, ativação de mudo, busca/progresso e seleção de faixa de áudio. Para habilitar esses controles, defina [**AreTransportControlsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn298977) como **true**. Para desabilitá-los, defina **AreTransportControlsEnabled** como **false**. Os controles de transporte são representados pela classe [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn831962). Você pode usar os controles de transporte como estão ou personalizá-los de várias maneiras. Para obter mais informações, consulte a referência de classe [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn831962) e [Criar controles personalizados de transporte](custom-transport-controls.md).

Os controles de transporte permitem que o usuário controle a maioria dos aspectos do [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926), mas o **MediaElement** também oferece diversas propriedades e métodos que você pode usar para controlar a reprodução de áudio e vídeo. Para obter mais informações, consulte a seção [Controlar MediaElement programaticamente](#control_mediaelement_programmatically) mais adiante neste artigo.

Os controles de transporte dão suporte a layouts de linha única e dupla. O primeiro exemplo é um layout de linha única, com o botão Reproduzir/Pausar localizado à esquerda da linha do tempo de mídia. Esse layout é mais adequado para telas compactas. 

![Exemplo de controles MTC no telefone, linha única](images/controls_mtc_singlerow_phone.png)

O layout de controles de linha dupla (abaixo) é recomendado para a maioria dos cenários de uso, especialmente em telas maiores. Esse layout oferece mais espaço para controles e facilita a operação da linha do tempo pelo usuário.

![Exemplo de controles MTC no telefone, linha dupla](images/controls_mtc_doublerow_phone.png)

**Controles de transporte de mídia do sistema**

Você também pode integrar o [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) aos controles de transporte de mídia do sistema. Os controles de transporte do sistema são os controles exibidos quando teclas de mídia de hardware são pressionadas, como os botões de mídia em teclados. Se o usuário pressionar a tecla de pausa em um teclado e seu aplicativo der suporte a [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677), seu aplicativo será notificado e você poderá tomar a medida apropriada. Para obter mais informações, consulte [Controles de transporte de mídia do sistema](https://msdn.microsoft.com/library/windows/apps/mt228338).

### Definir a origem da mídia
Para reproduzir arquivos na rede ou arquivos inseridos com o aplicativo, defina a propriedade [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) como o caminho do arquivo.

**Dica** para abrir arquivos da Internet, você precisa declarar a funcionalidade **Internet (Client)** no manifesto do aplicativo (Package.appxmanifest). Para obter mais informações sobre como declarar recursos, consulte [Declarações de recursos de aplicativos](https://msdn.microsoft.com/library/windows/apps/mt270968).

 

Esse código tenta definir a propriedade [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) do [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) definida em XAML como o caminho de um arquivo inserido em uma [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683).

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
        mediaPlayer.Source = pathUri;
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

Para definir a origem da mídia para um arquivo de mídia inserido no aplicativo, crie um [**Uri**](https://msdn.microsoft.com/library/windows/apps/br226017) com o caminho com o prefixo **ms-appx:///** e defina o [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) para ele. Por exemplo, para um arquivo chamado **video1. mp4** que está em uma subpasta **Vídeos**, o caminho ficaria: **ms-appx:///Vídeos/vídeo1.mp4**

Esse código define a propriedade [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) do [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) definido anteriormente em XAML para **ms-appx:///Videos/video1.mp4**.

```csharp
private void LoadEmbeddedAppFile()
{
    try
    {
        Uri pathUri = new Uri("ms-appx:///Videos/video1.mp4");
        mediaPlayer.Source = pathUri;
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

### Abrir arquivos de mídia local
Para abrir arquivos no sistema local ou no OneDrive, você pode usar o [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) para obter o arquivo e [**SetSource**](https://msdn.microsoft.com/library/windows/apps/br244338) para definir a origem da mídia, ou ainda acessar programaticamente as pastas de mídia do usuário.

Se o aplicativo precisar ter acesso sem a interação do usuário com as pastas **Música** ou **Vídeo**, por exemplo, se você precisar enumerar todos os arquivos de música ou vídeo na coleção do usuário e exibi-los em seu aplicativo, declare os recursos **Biblioteca de Músicas** e **Biblioteca de Vídeos**. Para obter mais informações, consulte [Files and folders in the Music, Pictures, and Videos libraries](https://msdn.microsoft.com/library/windows/apps/mt188703).

O [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) não precisa de recursos especiais para acessar os arquivos no sistema de arquivos local, como as pastas **Música** ou **Vídeo** do usuário, já que o usuário tem controle total sobre qual arquivo está sendo acessado. Em relação à segurança e à privacidade, é melhor minimizar as funcionalidades que seu aplicativo usa.

**Para abrir a mídia local usando FileOpenPicker**

1.  Chame [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) para permitir que o usuário escolha um arquivo de mídia.

    Use a classe [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) para escolher um arquivo de mídia. Defina o [**FileTypeFilter**](https://msdn.microsoft.com/library/windows/apps/br207850) para especificar quais tipos de arquivo o **FileOpenPicker** exibe. Chame [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) para iniciar o seletor de arquivos e obter o arquivo.

2.  Chame [**SetSource**](https://msdn.microsoft.com/library/windows/apps/br244338) para definir o arquivo de mídia escolhido como o [**MediaElement.Source**](https://msdn.microsoft.com/library/windows/apps/br227419).

    Para definir o [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) do [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) para o [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) retornado do [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847), você precisa abrir um fluxo. Chame o método [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn889851) no **StorageFile**, que retorna um fluxo que você pode transmitir para o método [**MediaElement.SetSource**](https://msdn.microsoft.com/library/windows/apps/br244338). Em seguida, chame o [**Play**](https://msdn.microsoft.com/library/windows/apps/br227402) no **MediaElement** para iniciar a mídia.

Este exemplo mostra como usar o [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) para escolher um arquivo e definir o arquivo como o [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) de um [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926).

```xaml
<MediaElement x:Name="mediaPlayer"/>
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
    
    // mediaPlayer is a MediaElement defined in XAML
    if (file != null)
    {
        var stream = await file.OpenAsync(Windows.Storage.FileAccessMode.Read);
        mediaPlayer.SetSource(stream, file.ContentType);

        mediaPlayer.Play();
    }
}
```

### Definir a origem do cartaz
Você pode usar a propriedade [**PosterSource**](https://msdn.microsoft.com/library/windows/apps/br227409) para fornecer seu [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) com uma representação visual antes de a mídia ser carregada. Uma **PosterSource** é uma imagem, como uma captura de tela ou poster de filme, que é exibida no lugar da mídia. A **PosterSource** é exibida nas seguintes situações:

-   Quando uma fonte válida não está definida. Por exemplo, [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) não está definido, i **Source** estava definida como **Null**, ou a origem é inválida (como quando um evento [**MediaFailed**](https://msdn.microsoft.com/library/windows/apps/br227393) ocorre).
-   Enquanto a mídia está sendo carregada. Por exemplo, uma origem válida está definida, mas o evento [**MediaOpened**](https://msdn.microsoft.com/library/windows/apps/br227394) ainda não ocorreu.
-   Quando há streaming de mídia para outro dispositivo.
-   Quando a mídia é somente áudio.

Aqui está um [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) com seu [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) definido como uma faixa de álbum, e [**PosterSource**](https://msdn.microsoft.com/library/windows/apps/br227409) definido como uma imagem da capa do álbum.

```xaml
<MediaElement Source="Media/Track1.mp4" PosterSource="Media/AlbumCover.png"/> 
```

### Mantenha a tela do dispositivo ativa
Normalmente, um dispositivo reduz a luminosidade da tela (e depois a desliga) para economizar bateria quando o usuário está longe, mas os aplicativos de vídeo precisam manter a tela ligada para que o usuário possa vê-lo. Para que a exibição não seja desativada quando não é detectada nenhuma ação do usuário, como quando o aplicativo está reproduzindo vídeo em tela inteira, você pode chamar [**DisplayRequest.RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818). A classe [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) permite que você informe ao Windows que mantenha a tela ligada para que o usuário possa ver o vídeo.

Para economizar energia e a vida útil da bateria, chame [**DisplayRequest.RequestRelease**](https://msdn.microsoft.com/library/windows/apps/br241819) para liberar a solicitação de exibição quando ela não for mais necessária. O Windows desativa automaticamente as solicitações de exibição ativas do aplicativo quando o aplicativo for movido para fora da tela e as reativa quando ele voltar ao primeiro plano.

Consulte algumas situações em que você deve liberar a solicitação de exibição:

-   Por exemplo, a reprodução do vídeo foi pausada por uma ação do usuário, buffer ou ajuste devido à largura de banda limitada.
-   A reprodução foi interrompida. Por exemplo, a reprodução do vídeo ou a apresentação acabou.
-   Erro na reprodução. Por exemplo, problemas com a conectividade de rede ou um arquivo corrompido.

**Para manter a tela ativa**

1.  Crie uma variável [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) global. Inicialize-a com nulo.
```csharp
// Create this variable at a global scope. Set it to null.
private DisplayRequest appDisplayRequest = null;
```

2.  Chame [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818) para notificar o Windows que o aplicativo requer que a tela fique ligada.

3.  Chame [**RequestRelease**](https://msdn.microsoft.com/library/windows/apps/br241819) para liberar a solicitação de exibição sempre que a reprodução do vídeo por parada, pausada ou interrompida por um erro de reprodução. Quando o seu aplicativo não tem mais solicitações de exibição ativas, o Windows economiza a bateria ao reduzir a luminosidade da tela (e, depois, desligando-a) quando o dispositivo não estiver em uso.

    Aqui você usa o evento [**CurrentStateChanged**](https://msdn.microsoft.com/library/windows/apps/br227375) para detectar essas situações. Em seguida, use a propriedade [**IsAudioOnly**](https://msdn.microsoft.com/library/windows/apps/hh965334) para determinar se um arquivo de áudio ou vídeo está sendo executado, e mantenha a tela ativa somente se o vídeo estiver sendo executado.
    ```xaml
<MediaElement Source="Media/video1.mp4"
              CurrentStateChanged="MediaElement_CurrentStateChanged"/>
    ```
 
    ```csharp
private void MediaElement_CurrentStateChanged(object sender, RoutedEventArgs e)
{
    MediaElement mediaElement = sender as MediaElement;
    if (mediaElement != null && mediaElement.IsAudioOnly == false)
    {
        if (mediaElement.CurrentState == Windows.UI.Xaml.Media.MediaElementState.Playing)
        {                
            if (appDisplayRequest == null)
            {
                // This call creates an instance of the DisplayRequest object. 
                appDisplayRequest = new DisplayRequest();
                appDisplayRequest.RequestActive();
            }
        }
        else // CurrentState is Buffering, Closed, Opening, Paused, or Stopped. 
        {
            if (appDisplayRequest != null)
            {
                // Deactivate the display request and set the var to null.
                appDisplayRequest.RequestRelease();
                appDisplayRequest = null;
            }
        }            
    }
} 
    ```

### Controlar o media player de forma programática
[
              **MediaElement**
            ](https://msdn.microsoft.com/library/windows/apps/br242926) oferece inúmeras propriedades, métodos e eventos para controlar a reprodução de áudio e vídeo. Para uma listagem completa de propriedades, métodos e eventos, consulte página de referência do [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926).
    

### Selecionar faixas de áudio em idiomas diferentes

Use a propriedade [**AudioStreamIndex**](https://msdn.microsoft.com/library/windows/apps/br227358) e o método [**GetAudioStreamLanguage**](https://msdn.microsoft.com/library/windows/apps/br227384) para mudar o áudio para outra faixa de idioma em um vídeo. Os vídeos também podem ter várias faixas de áudio no mesmo idioma, como comentários do diretor em filmes. Este exemplo mostra especificamente como alternar idiomas diferentes, mas você pode modificar esse código para alternar faixas de áudio.

**Para selecionar faixas de áudio em idiomas diferentes**

1.  Obtenha as faixas de áudio.

    Para procurar uma faixa em um idioma específico, comece fazendo a iteração de cada faixa de áudio no vídeo. Use [**AudioStreamCount**](https://msdn.microsoft.com/library/windows/apps/br227356) como valor máximo de um loop **for**.

2.  Obtenha o idioma da faixa de áudio.

    Use o método [**GetAudioStreamLanguage**](https://msdn.microsoft.com/library/windows/apps/br227384) para obter o idioma da faixa. O idioma da faixa é identificado por um [código de idioma](http://msdn.microsoft.com/library/ms533052(vs.85).aspx), como **"en"** para inglês ou **"ja"** para japonês.

3.  Defina a faixa de áudio ativa.

    Quando encontrar a faixa com o idioma desejado, defina [**AudioStreamIndex**](https://msdn.microsoft.com/library/windows/apps/br227358) como o índice da faixa. Definir **AudioStreamIndex** como **null** seleciona a faixa de áudio padrão que é definida pelo conteúdo.

Este código define a faixa de áudio para o idioma especificado. Ele faz a iteração por meio das faixas de áudio em um objeto [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) e usa [**GetAudioStreamLanguage**](https://msdn.microsoft.com/library/windows/apps/br227384) para obter o idioma de cada faixa. Se a faixa de idioma desejada existe, o [**AudioStreamIndex**](https://msdn.microsoft.com/library/windows/apps/br227358) é definido para o índice dessa faixa.

```csharp
/// <summary>
/// Attemps to set the audio track of a video to a specific language
/// </summary>
/// <param name="lcid">The id of the language. For example, "en" or "ja"</param>
/// <returns>true if the track was set; otherwise, false.</returns>
private bool SetAudioLanguage(string lcid, MediaElement media)
{
    bool wasLanguageSet = false;

    for (int index = 0; index < media.AudioStreamCount; index++)
    {
        if (media.GetAudioStreamLanguage(index) == lcid)
        {
            media.AudioStreamIndex = index;
            wasLanguageSet = true;
        }
    }

    return wasLanguageSet;
}
```

### Habilitar a renderização de vídeo da janela inteira

Defina a propriedade [**IsFullWindow**](https://msdn.microsoft.com/library/windows/apps/dn298980) para habilitar ou desabilitar a renderização de janela inteira. Ao definir de forma programática a renderização de janela inteira em seu aplicativo, você sempre deve usar **IsFullWindow** em vez de fazer isso manualmente. **IsFullWindow** garante que as otimizações em nível de sistema sejam executadas, o que melhora o desempenho e a duração da bateria. Se a renderização de janela inteira não estiver configurada corretamente, essas otimizações não poderão ser habilitadas.

Este código cria um [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244) que alterna a renderização da janela inteira.

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

### Redimensionar e ampliar vídeo

Use a propriedade [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br227422) para mudar a forma como o conteúdo de vídeo preenche seu contêiner. Isso redimensiona e amplia o vídeo de acordo com o valor de [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br242968). Os estados de **Stretch** são parecidos com as configurações de tamanho de imagem em aparelhos de TV. Você pode enganchá-la em um botão para que o usuário possa escolher a configuração de sua preferência.

-   [
              **None**
            ](https://msdn.microsoft.com/library/windows/apps/br242968) mostra a resolução nativa do conteúdo em seu tamanho original.
-   [
              **Uniform**
            ](https://msdn.microsoft.com/library/windows/apps/br242968) preenche o máximo do espaço possível, mantendo a taxa de proporção e o conteúdo da imagem. Isso pode produzir barras pretas horizontais ou verticais nas bordas do vídeo. Isso é semelhante aos modos widescreen.
-   [
              **UniformToFill**
            ](https://msdn.microsoft.com/library/windows/apps/br242968) preenche todo o espaço, mantendo a taxa de proporção. Isso pode fazer parte da imagem ser cortada. Isso é semelhante aos modos de tela inteira.
-   [
              **Fill**
            ](https://msdn.microsoft.com/library/windows/apps/br242968) preenche todo o espaço, mas não mantém a taxa de proporção. A imagem não é cortada, mas pode ocorrer um alongamento. Isso é semelhante aos modos de alongamento.

![Valores de enumeração de alongamento](images/Image_Stretch.jpg) Aqui, um [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244) é usado para percorrer as opções de [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br242968). Uma declaração **switch** verifica o estado atual da propriedade [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br227422) e o define para o próximo valor na enumeração de **Stretch**. Isso permite ao usuário circular pelos vários estados de ampliação.

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

### Habilitar a reprodução de baixa latência

Defina a propriedade [**RealTimePlayback**](https://msdn.microsoft.com/library/windows/apps/br227414) como **true** em um [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) para habilitar o elemento de mídia a fim de reduzir a latência inicial da reprodução. Isso é essencial para aplicativos de comunicação bidirecionais e pode ser aplicável a alguns cenários de jogos. Esse modo consome mais recursos e energia.

Este exemplo cria um [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) e define [**RealTimePlayback**](https://msdn.microsoft.com/library/windows/apps/br227414) como **true**.

```xaml
<MediaElement x:Name="mediaPlayer" RealTimePlayback="True"/>
```

```csharp
MediaElement mediaPlayer = new MediaElement();
mediaPlayer.RealTimePlayback = true;
```
    
## Recomendações 

O media player é fornecido em um tema escuro e um tema claro, mas opte pelo tema escuro na maioria das situações. A tela de fundo escura oferece melhor contraste, particularmente em condições com pouca luz, e limita a interferência da barra de controle na experiência de visualização.

Incentive uma experiência de visualização dedicada ao promover o modo de tela inteira em vez do modo embutido. A experiência de visualização em tela inteira é ideal, e as opções são restritas no modo embutido.

Se você tiver o estado real da tela, escolha o layout de linha dupla. Ele fornece mais espaço para controles do que o layout de linha única compacto.

Adicione qualquer opção personalizada necessária ao media player para oferecer a melhor experiência a seu aplicativo, mas lembre-se do seguinte:

-   Limite a personalização dos controles padrão, que foram otimizados para a experiência de reprodução de mídia.
-   Em telefones e outros dispositivos móveis, os elementos visuais do dispositivo permanecem pretos, mas, em áreas de trabalho e notebooks, os elementos visuais do dispositivo herdam a cor do tema do usuário.
-   Tente não sobrecarregar a barra de controle com muitas opções.
-   Não reduza a linha do tempo de mídia abaixo de seu tamanho mínimo padrão, o que limitará severamente sua eficácia.

## Artigos relacionados

- [Noções básicas de design de comandos para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/dn958433)
- [Noções básicas de design de conteúdo para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/dn958434)


<!--HONumber=May16_HO2-->


