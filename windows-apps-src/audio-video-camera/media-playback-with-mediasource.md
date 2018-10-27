---
author: drewbatgit
ms.assetid: C5623861-6280-4352-8F22-80EB009D662C
description: Este artigo mostra como usar MediaSource, que fornece uma maneira comum de referenciar e reproduzir mídia de diferentes fontes, como arquivos locais ou remotos, e expõe um modelo comum para acessar dados de mídia, independentemente do formato de mídia subjacente.
title: Itens de mídia, playlists e faixas
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 73b6a19e2385f1a9b8afa4672df50d17ac16ec97
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5711223"
---
# <a name="media-items-playlists-and-tracks"></a>Itens de mídia, playlists e faixas


 Este artigo mostra como usar a classe [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource), que fornece uma maneira comum de referenciar e reproduzir mídia de diferentes fontes, como arquivos locais ou remotos, e expõe um modelo comum para acessar dados de mídia, independentemente do formato de mídia subjacente. A classe [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939) estende a funcionalidade do **MediaSource**, permitindo que você gerencie e selecione várias faixas de áudio, vídeo e metadados contidas em um item de mídia. [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) permite que você crie playlists a partir de um ou mais itens de reprodução de mídia.


## <a name="create-and-play-a-mediasource"></a>Criar e executar um MediaSource

Crie uma nova instância do **MediaSource** chamando um dos métodos de fábrica expostos pela classe:

-   [**CreateFromAdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930906)
-   [**CreateFromIMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn965527)
-   [**CreateFromMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn930907)
-   [**CreateFromMseStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn930908)
-   [**CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909)
-   [**CreateFromStream**](https://msdn.microsoft.com/library/windows/apps/dn930910)
-   [**CreateFromStreamReference**](https://msdn.microsoft.com/library/windows/apps/dn930911)
-   [**CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912)
-   [**CreateFromDownloadOperation**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromdownloadoperation)

Depois de criar uma **MediaSource**, você poderá reproduzi-la com um [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/dn652535) definindo a propriedade [**Source**](https://msdn.microsoft.com/library/windows/apps/dn987010). Desde o Windows 10, versão 1607, você pode atribuir um **MediaPlayer** a um [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) chamando [**SetMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/mt708764) para renderizar o conteúdo do player de mídia em uma página XAML. Este é o método preferido em relação ao uso de **MediaElement**. Para obter mais informações sobre como usar **MediaPlayer**, consulte [**Reproduzir áudio e vídeo com MediaPlayer**](play-audio-and-video-with-mediaplayer.md).

O exemplo a seguir mostra como reproduzir um arquivo de mídia selecionado pelo usuário um **MediaPlayer** usando **MediaSource**.

Você precisará incluir os namespaces [**Windows.Media.Core**](https://msdn.microsoft.com/library/windows/apps/dn278962) e [**Windows.Media.Playback**](https://msdn.microsoft.com/library/windows/apps/dn640562) para completar esse cenário.

[!code-cs[Using](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetUsing)]

Declare uma variável de membro do tipo **MediaSource**. Para os exemplos deste artigo, a fonte de mídia é declarada como membro da classe para poder ser acessada a partir de vários locais.

[!code-cs[DeclareMediaSource](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaSource)]

Declare uma variável para armazenar o objeto **MediaPlayer** e, se você quiser renderizar o conteúdo de mídia em XAML, adicione um **MediaPlayerElement** à página.

[!code-cs[DeclareMediaPlayer](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlayer)]

[!code-xml[MediaPlayerElement](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetMediaPlayerElement)]

Para permitir que o usuário selecione um arquivo de mídia para reproduzir, use um [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847). Com o objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) retornado no método [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) do seletor, inicialize um novo MediaObject chamando [**MediaSource.CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909). Por fim, defina a fonte de mídia como a fonte de reprodução para o **MediaElement** chamando o método [**SetPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/dn899085).

[!code-cs[PlayMediaSource](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlayMediaSource)]

Por padrão, o **MediaPlayer** não começa reproduzido automaticamente quando a origem da mídia é definida. Você pode começar manualmente a reprodução chamando [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play).

[!code-cs[Play](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlay)]

Você também pode definir a propriedade [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AutoPlay) do **MediaPlayer** como true para solicitar ao player que inicie a reprodução assim que a origem da mídia for definida.

[!code-cs[AutoPlay](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAutoPlay)]

### <a name="create-a-mediasource-from-a-downloadoperation"></a>Criar um MediaSource de DownloadOperation
A partir do Windows, versão 1803, você pode criar um objeto **MediaSource** a partir de um **DownloadOperation**.

[!code-cs[CreateMediaSourceFromDownload](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetCreateMediaSourceFromDownload)]

Observe que, embora você possa criar um **MediaSource** a partir de um download sem iniciá-lo ou configurar a propriedade **IsRandomAccessRequired** como true, você deve executar ambas as ações antes de tentar associar **MediaSource** a **MediaPlayer** ou **MediaPlayerElement** para reprodução.

[!code-cs[StartDownload](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetStartDownload)]


## <a name="handle-multiple-audio-video-and-metadata-tracks-with-mediaplaybackitem"></a>Manipular várias faixas de áudio, vídeo e metadados com MediaPlaybackItem

Usar um [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930905) para reprodução é conveniente porque ele fornece uma maneira comum de reproduzir mídia de tipos de fontes diferentes, mas o comportamento mais avançado pode ser acessado criando um [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939) na **MediaSource**. Isso inclui a capacidade de acessar e gerenciar várias faixas de áudio, vídeo e dados de um item de mídia.

Declare uma variável para armazenar seu **MediaPlaybackItem**.

[!code-cs[DeclareMediaPlaybackItem](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlaybackItem)]

Crie um **MediaPlaybackItem** chamando o construtor e passando um objeto **MediaSource** inicializado.

Se seu aplicativo possui suporte a várias faixas de áudio, vídeo ou dados em um item de reprodução de mídia, registre manipuladores de eventos para os eventos [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930948), [**VideoTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930954) ou [**TimedMetadataTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930952).

Por fim, defina a origem da reprodução do **MediaElement** ou **MediaPlayer** para sua **MediaPlaybackItem**.

[!code-cs[PlayMediaPlaybackItem](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlayMediaPlaybackItem)]

> [!NOTE] 
> Um **MediaSource** só pode ser associado a um único **MediaPlaybackItem**. Depois de criar um **MediaPlaybackItem** a partir de uma fonte, a tentativa de criar outro item de reprodução a partir da mesma fonte resultará em um erro. Além disso, depois de criar um **MediaPlaybackItem** a partir de uma fonte de mídia, você não pode definir o objeto **MediaSource** diretamente como a fonte de um **MediaPlayer**, mas deve usar o **MediaPlaybackItem**.

O evento [**VideoTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930954) é lançado depois que um **MediaPlaybackItem** que contém várias faixas de vídeos é atribuído como fonte de reprodução, e pode ser gerado novamente se a lista de faixas de vídeo do item for alterada. O manipulador desse evento oferece a oportunidade de atualizar sua interface do usuário para permitir que o usuário alterne entre as faixas disponíveis. Este exemplo usa um [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348) para exibir as faixas de vídeo disponíveis.

[!code-xml[VideoComboBox](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetVideoComboBox)]

No manipulador **VideoTracksChanged**, execute um loop por todas as faixas na lista [**VideoTracks**](https://msdn.microsoft.com/library/windows/apps/dn930953) do item de reprodução. Para cada faixa, um novo [**ComboBoxItem**](https://msdn.microsoft.com/library/windows/apps/br209349) é criado. Se a faixa ainda não tiver um rótulo, um rótulo será gerado do índice de faixas. A propriedade [**Tag**](https://msdn.microsoft.com/library/windows/apps/br208745) do item da caixa de combinação é definida como o índice de faixas para que ele possa ser identificado mais tarde. Por fim, o item é adicionado à caixa de combinação. Observe que essas operações serão executadas em uma chamada [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317), pois todas as alterações da interface do usuário devem ser feitas no thread da interface do usuário, e esse evento é lançado em um thread diferente.

[!code-cs[VideoTracksChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetVideoTracksChanged)]

No manipulador [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) da caixa de combinação, o índice de faixas é recuperado a partir da propriedade **Tag** do item selecionado. Definir a propriedade [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/dn956634) da lista [**VideoTracks**](https://msdn.microsoft.com/library/windows/apps/dn930953) do item de reprodução de mídia faz com que o **MediaElement** ou o **MediaPlayer** alterne a faixa de vídeo ativa para o índice especificado.

[!code-cs[VideoTracksSelectionChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetVideoTracksSelectionChanged)]

O gerenciamento de itens de mídia com várias faixas de áudio funciona exatamente da mesma forma do que com faixas de vídeo. Manipule o evento [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930948) para atualizar a interface do usuário com as faixas de áudio encontradas na lista [**AudioTracks**](https://msdn.microsoft.com/library/windows/apps/dn930947) do item de reprodução. Quando o usuário selecionar uma faixa de áudio, defina a propriedade [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/dn930937) da lista **AudioTracks** para fazer com que o **MediaElement** ou o **MediaPlayer** alterne a faixa de áudio ativa para o índice especificado.

[!code-xml[AudioComboBox](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetAudioComboBox)]

[!code-cs[AudioTracksChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAudioTracksChanged)]

[!code-cs[AudioTracksSelectionChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAudioTracksSelectionChanged)]

Além de áudio e vídeo, um objeto **MediaPlaybackItem** pode conter zero ou mais objetos [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580). Uma faixa de metadados com tempo pode conter texto de legenda ou pode conter dados personalizados que são próprios de seu aplicativo. Uma faixa de metadados com tempo contém uma lista de indicações representadas por objetos herdados de [**IMediaCue**](https://msdn.microsoft.com/library/windows/apps/dn930899), como [**DataCue**](https://msdn.microsoft.com/library/windows/apps/dn930892) ou [**TimedTextCue**](https://msdn.microsoft.com/library/windows/apps/dn956655) Cada indicação tem uma hora de início e uma duração que determinam quando a indicação será ativada e por quanto tempo.

Semelhante às faixas de áudio e vídeo, as faixas de metadados com tempo de um item de mídia podem ser descobertas manipulando o evento [**TimedMetadataTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930952) de um **MediaPlaybackItem**. Com as faixas de metadados com tempo, porém, o usuário pode habilitar mais de uma faixa de metadados por vez. Além disso, dependendo do cenário do aplicativo, convém habilitar ou desabilitar faixas de metadados automaticamente, sem a intervenção do usuário. Para fins de ilustração, esse exemplo adiciona um [**ToggleButton**](https://msdn.microsoft.com/library/windows/apps/br209795) para cada controle de metadados em um item de mídia a fim de permitir que o usuário habilite ou desabilite o controle. A propriedade **Tag** de cada botão está definida no índice do controle de metadados associado para que possa ser identificado quando o botão é acionado.

[!code-xml[MetaStackPanel](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetMetaStackPanel)]

[!code-cs[TimedMetadataTrackschanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedMetadataTrackschanged)]

Como mais de uma faixa de metadados pode estar ativa por vez, você simplesmente não define o índice ativo para a lista de faixas de metadados. Em vez disso, chame o método [**SetPresentationMode**](https://msdn.microsoft.com/library/windows/apps/dn986977) do objeto **MediaPlaybackItem**, passando o índice da faixa que você deseja alternar e fornecendo um valor da enumeração [**TimedMetadataTrackPresentationMode**](https://msdn.microsoft.com/library/windows/apps/dn987016). O modo de apresentação que você escolher depende da implementação de seu aplicativo. Neste exemplo, a faixa de metadados é definida como **PlatformPresented** quando habilitada. Para controles com base em texto, isso significa que o sistema exibe automaticamente as indicações de texto no controle. Quando o botão de alternância é desativado, o modo de apresentação é definido como **Desabilitado**, ou seja, nenhum texto é exibido e nenhum evento de indicação é gerado. Os eventos de indicação são explicados mais adiante neste artigo.

[!code-cs[ToggleChecked](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetToggleChecked)]

[!code-cs[ToggleUnchecked](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetToggleUnchecked)]

Na medida em que processa as faixas de metadados, você pode acessar o conjunto de indicações dentro da faixa acessando a propriedade [**Cues**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.TimedMetadataTrack.Cues) ou [**ActiveCues**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.TimedMetadataTrack.ActiveCues). Você pode fazer isso para atualizar a interface do usuário a fim de mostrar os locais de indicação de um item de mídia.

## <a name="handle-unsupported-codecs-and-unknown-errors-when-opening-media-items"></a>Manipular codecs incompatíveis e erros desconhecidos durante a abertura de itens de mídia
Desde o Windows 10, versão 1607, você pode verificar se o codec necessário para reprodução de um item de mídia é compatível ou parcialmente compatível com o dispositivo no qual o aplicativo está em execução. No manipulador de evento eventos com alteração controlada de **MediaPlaybackItem**, como [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.AudioTracksChanged), primeiro verifique se o controle de alterações é uma inserção do novo controle. Caso afirmativo, você pode obter uma referência para que continue inserido usando o índice passado no parâmetro **IVectorChangedEventArgs.Index** com a coleção de controle adequada do parâmetro **MediaPlaybackItem**, como a coleção [**AudioTracks**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.AudioTracks).

Quando você tem uma referência à faixa inserida, verifique o [**DecoderStatus**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrackSupportInfo.DecoderStatus) da propriedade [**SupportInfo**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.SupportInfo) da faixa. Se o valor for [**FullySupported**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus), o codec adequado necessário para reproduzir a faixa estará presente no dispositivo. Se o valor for [**Degraded**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus), a faixa poderá ser reproduzida pelo sistema, mas a reprodução será degradada de alguma forma. Por exemplo, uma faixa de áudio 5.1 pode ser reproduzida como estéreo em 2 canais. Se esse for o caso, será necessário atualizar a interface do usuário para alertar o usuário da degradação. Se o valor for [**UnsupportedSubtype**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus) ou [**UnsupportedEncoderProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus), a faixa não poderá ser reproduzida em todos os codecs atuais no dispositivo. É necessário alertar o usuário e ignorar a reprodução do item ou implementar a interface do usuário para permitir que o usuário baixe o codec correto. O método [**GetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.GetEncodingProperties) da faixa pode ser usado para determinar o codec necessário para reprodução.

Por fim, você pode se registrar no evento [**OpenFailed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.OpenFailed) da faixa, que será acionado se a faixa for compatível com o dispositivo, mas deixará de ser aberta por causa de um erro desconhecido no pipeline.

[!code-cs[AudioTracksChanged_CodecCheck](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAudioTracksChanged_CodecCheck)]

No manipulador de eventos [**OpenFailed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.OpenFailed), você pode verificar se o status **MediaSource** é desconhecido e, em caso afirmativo, selecionar programaticamente uma faixa diferente a ser reproduzida, permitir que o usuário escolha uma faixa diferente ou abandone a reprodução.

[!code-cs[OpenFailed](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetOpenFailed)]

## <a name="set-display-properties-used-by-the-system-media-transport-controls"></a>Definir propriedades de exibição usadas pelos controles de transporte de mídia do sistema
Desde o Windows 10, versão 1607, a mídia reproduzida em um [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) é integrada automaticamente aos controles de transporte de mídia do sistema (SMTC) por padrão. Você pode especificar os metadados que serão exibidos pelo SMTC atualizando as propriedades de exibição de um **MediaPlaybackItem**. Obtenha um objeto representando as propriedades de exibição para um item chamando [**GetDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.GetDisplayProperties). Defina se o item de reprodução é música ou vídeo configurando a propriedade [**Type**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.Type). Em seguida, defina as propriedades de [**VideoProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.VideoProperties) ou [**MusicProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.MusicProperties) do objeto. Chame [**ApplyDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/mt489923) para atualizar as propriedades do item para os valores fornecidos. Normalmente, um aplicativo recuperará os valores de exibição dinamicamente de um serviço web, mas o exemplo a seguir ilustra esse processo com valores codificados.

[!code-cs[SetVideoProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetVideoProperties)]

[!code-cs[SetMusicProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetMusicProperties)]

## <a name="add-external-timed-text-with-timedtextsource"></a>Adicionar texto com tempo externo com TimedTextSource

Para alguns cenários, você pode ter arquivos externos que contenham texto com tempo associado a um item de mídia, como arquivos separados que contêm legendas para localidades diferentes. Use a classe [**TimedTextSource**](https://msdn.microsoft.com/library/windows/apps/dn956679) para carregar em arquivos de texto com tempo externos de um fluxo ou URI.

Este exemplo usa uma coleção **Dictionary** para armazenar uma lista das fontes de texto com tempo para o item de mídia usando o URI da fonte e o objeto **TimedTextSource** como o par de chave/valor para identificar as faixas depois que elas forem resolvidas.

[!code-cs[TimedTextSourceMap](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedTextSourceMap)]

Crie um novo **TimedTextSource** para cada arquivo de texto com tempo externo chamando [**CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn708190). Adicione uma entrada ao **Dictionary** para a fonte de texto com tempo. Adicione um manipulador para o evento [**TimedTextSource.Resolved**](https://msdn.microsoft.com/library/windows/apps/dn965540) a ser manipulado se o item não for carregado ou para definir propriedades adicionais depois que o item for carregado com êxito.

Registre todos os seus objetos **TimedTextSource** com o **MediaSource** adicionando-os à coleção [**ExternalTimedTextSources**](https://msdn.microsoft.com/library/windows/apps/dn930916). Observe que fontes de texto com tempo externas são adicionadas diretamente ao **MediaSource** e não ao **MediaPlaybackItem** criado a partir da fonte. Para atualizar a interface do usuário para refletir as faixas de texto externo, registre e manipule o evento **TimedMetadataTracksChanged** conforme descrito anteriormente neste artigo.

[!code-cs[TimedTextSource](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedTextSource)]

No manipulador para o evento [**TimedTextSource.Resolved**](https://msdn.microsoft.com/library/windows/apps/dn965540), verifique a propriedade **Error** do [**TimedTextSourceResolveResultEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn965537) passado para o manipulador para determinar se ocorreu um erro ao tentar carregar os dados de texto com tempo. Se o item for resolvido com êxito, você pode usar esse manipulador para atualizar as propriedades adicionais do controle resolvido. Esse exemplo adiciona um rótulo para cada controle com base no URI armazenado anteriormente no **Dicionário**.

[!code-cs[TimedTextSourceResolved](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedTextSourceResolved)]

## <a name="add-additional-metadata-tracks"></a>Adicionar mais faixas de metadados

Você pode criar faixas de metadados personalizadas dinamicamente no código e associá-las a uma fonte de mídia. As faixas que você cria podem conter texto de legenda ou podem conter seus dados próprios do aplicativo.

Crie um novo [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580) chamando o construtor e especificando um ID, o identificador do idioma e um valor da enumeração [**TimedMetadataKind**](https://msdn.microsoft.com/library/windows/apps/dn956578). Registre manipuladores para os eventos [**CueEntered**](https://msdn.microsoft.com/library/windows/apps/dn956583) e [**CueExited**](https://msdn.microsoft.com/library/windows/apps/dn956584). Esses eventos são lançados na chegada da hora de início de uma indicação e quando a duração de uma indicação tiver expirado, respectivamente.

Crie um novo objeto de indicação, adequado para o tipo de controle de metadados criado, e defina a ID, a hora de início e a duração do controle. Esse exemplo cria um controle de dados, portanto, um conjunto de objetos [**DataCue**](https://msdn.microsoft.com/library/windows/apps/dn930892) é gerado e um buffer contendo dados específicos do aplicativo é fornecido para cada indicação. Para registrar a nova faixa, adicione-a à coleção [**ExternalTimedMetadataTracks**](https://msdn.microsoft.com/library/windows/apps/dn930915) do objeto **MediaSource**.

A partir do Windows 10, versão 1703, a propriedade **DataCue.Properties** expõe um [**PropertySet**](https://docs.microsoft.com/uwp/api/windows.foundation.collections.propertyset) que você pode usar para armazenar propriedades personalizadas em pares de chave/dados que podem ser recuperados nos eventos **CueEntered** e **CueExited**.  

[!code-cs[AddDataTrack](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAddDataTrack)]

O evento **CueEntered** é acionado quando o horário de início de uma indicação tiver sido atingido, desde que a faixa associada tenha um modo de apresentação de **ApplicationPresented**, **Hidden** ou **PlatformPresented.** Eventos de sinalização não serão gerados para faixas de metadados enquanto o modo de apresentação para a faixa for **Desabilitado**. Este exemplo simplesmente gera os dados personalizados associados à indicação para a janela de depuração.

[!code-cs[DataCueEntered](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDataCueEntered)]

Este exemplo adiciona uma faixa de texto personalizada especificando **TimedMetadataKind.Caption** ao criar a faixa e usando objetos [**TimedTextCue**](https://msdn.microsoft.com/library/windows/apps/dn956655) para adicionar indicações à faixa.

[!code-cs[AddTextTrack](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAddTextTrack)]

[!code-cs[TextCueEntered](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTextCueEntered)]

## <a name="play-a-list-of-media-items-with-mediaplaybacklist"></a>Reproduzir uma lista de itens de mídia com MediaPlaybackList

[**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) permite criar uma playlist de itens de mídia, que são representados por objetos **MediaPlaybackItem**.

**Observação**itens em uma [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) são renderizados por meio de reprodução sem intervalos. O sistema usará metadados fornecidos em arquivos MP3 ou AAC codificados para determinar o atraso ou a compensação de preenchimento necessária à reprodução sem intervalos. Se os arquivos MP3 ou AAC codificados não fornecerem esses metadados, o sistema determinará o atraso ou o preenchimento heuristicamente. Para os formatos sem perdas, como PCM, FLAC ou ALAC, o sistema não executa nenhuma ação porque esses codificadores não apresentam atraso ou preenchimento.

Para começar, declare uma variável para armazenar seu **MediaPlaybackList**.

[!code-cs[DeclareMediaPlaybackList](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlaybackList)]

Crie um **MediaPlaybackItem** para cada item de mídia que você deseja adicionar à sua lista usando o mesmo procedimento descrito anteriormente neste artigo. Inicialize seu objeto **MediaPlaybackList** e adicione os itens de reprodução de mídia a ele. Registre um manipulador para o evento [**CurrentItemChanged**](https://msdn.microsoft.com/library/windows/apps/dn930957). Esse evento permite atualizar a interface do usuário para refletir o item de mídia que está sendo reproduzido. Você também pode registrar o evento [ItemOpened](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList.ItemOpened), que é acionado quando um item na lista é aberto com êxito, e o evento [ItemFailed](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList.ItemFailed), que é acionado quando um item na lista não pode ser aberto.

A partir do Windows 10, versão 1703, você pode especificar o número máximo de objetos **MediaPlaybackItem** na **MediaPlaybackList** que o sistema manterá aberto depois que eles tiverem sido reproduzidos por meio da definição da propriedade [MaxPlayedItemsToKeepOpen](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList.MaxPlayedItemsToKeepOpen) propriedade. Quando um **MediaPlaybackItem** é mantido aberto, a reprodução do item pode começar instantaneamente quando o usuário alterna para esse item porque o item não precisa ser recarregado. Mas manter itens abertos também aumenta o consumo de memória do seu aplicativo, portanto, você deve considerar o equilíbrio entre a capacidade de resposta e o uso de memória ao definir esse valor. 

Para habilitar a reprodução da sua lista, defina a origem da reprodução do **MediaPlayer** para sua **MediaPlaybackList**.

[!code-cs[PlayMediaPlaybackList](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlayMediaPlaybackList)]

No manipulador de eventos **CurrentItemChanged**, atualize a interface do usuário para refletir o item atualmente em reprodução, que pode ser recuperado usando a propriedade [**NewItem**](https://msdn.microsoft.com/library/windows/apps/dn930930) do objeto [**CurrentMediaPlaybackItemChangedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn930929) passado para o evento. Lembre-se: se você atualizar a interface do usuário a partir desse evento, deverá fazer isso chamando [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) para que as atualizações sejam feitas no thread da interface do usuário.

A partir do Windows 10, versão 1703, você pode marcar a propriedade [CurrentMediaPlaybackItemChangedEventArgs.Reason](https://docs.microsoft.com/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.Reason) para obter um valor que indica o motivo pelo qual que o item foi alterado, como o app alternar itens de maneira programática, o item anterior chegar ao seu final ou um erro ocorrer.

[!code-cs[MediaPlaybackListItemChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetMediaPlaybackListItemChanged)]


Chame [**MovePrevious**](https://msdn.microsoft.com/library/windows/apps/mt146455) ou [**MoveNext**](https://msdn.microsoft.com/library/windows/apps/mt146454) para fazer o player de mídia reproduzir o item anterior ou o próximo na **MediaPlaybackList**.

[!code-cs[PrevButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPrevButton)]

[!code-cs[NextButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetNextButton)]

Defina a propriedade [**ShuffleEnabled**](https://msdn.microsoft.com/library/windows/apps/mt146457) para especificar se o media player deve reproduzir os itens da lista em ordem aleatória.

[!code-cs[ShuffleButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetShuffleButton)]

Defina a propriedade [**AutoRepeatEnabled**](https://msdn.microsoft.com/library/windows/apps/mt146452) para especificar se o media player deve reproduzir sua lista em loop.

[!code-cs[RepeatButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetRepeatButton)]


### <a name="handle-the-failure-of-media-items-in-a-playback-list"></a>Manipular a falha de itens de mídia em uma playlist
O evento [**ItemFailed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList.ItemFailed) é acionado quando um item na lista deixa de abrir. A propriedade [**ErrorCode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItemError.ErrorCode) do objeto [**MediaPlaybackItemError**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItemError) passado para o manipulador enumera a causa específica da falha quando possível, inclusive erros de rede, de decodificação ou de criptografia.

[!code-cs[ItemFailed](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetItemFailed)]

### <a name="disable-playback-of-items-in-a-playback-list"></a>Desabilitar a reprodução de itens em uma playlist
A partir do Windows 10, versão 1703, você pode desabilitar a reprodução de um ou mais itens em uma **MediaPlaybackItemList** ao definir a propriedade [IsDisabledInPlaybackList](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem.IsDisabledInPlaybackList) de um [MediaPlaybackItem](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem) como falsa. 

Um cenário típico para esse recurso é para apps que reproduzem música transmitida da Internet. O aplicativo pode ouvir as alterações no status de conexão de rede do dispositivo e desabilitar a reprodução de itens que não são completamente baixados. No exemplo a seguir, um manipulador é registrado para o evento [NetworkInformation.NetworkStatusChanged](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation.NetworkStatusChanged).

[!code-cs[RegisterNetworkStatusChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetRegisterNetworkStatusChanged)]

No manipulador para **NetworkStatusChanged**, verifique se [GetInternetConnectionProfile](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation.GetInternetConnectionProfile) retorna null, o que indica que a rede não está conectada. Se esse for o caso, percorra todos os itens da playlist em loop e, se [TotalDownloadProgress](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TotalDownloadProgress) para o item for menor do que 1, o que significa que o item não foi completamente baixado, desabilite o item. Se a conexão de rede estiver habilitada, percorra todos os itens na playlist em loop e habilite cada item.

[!code-cs[NetworkStatusChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetNetworkStatusChanged)]

### <a name="defer-binding-of-media-content-for-items-in-a-playback-list-by-using-mediabinder"></a>Adie a associação de conteúdo de mídia para itens em uma playlist usando o MediaBinder
Nos exemplos anteriores, uma **MediaSource** é criada de um arquivo, URL ou streaming, depois do qual um **MediaPlaybackItem** é criado e adicionado a uma **MediaPlaybackList**. Para alguns cenários, por exemplo, se o usuário está sendo cobrado pela exibição de conteúdo, talvez você queira adiar a recuperação do conteúdo de uma **MediaSource** até que o item da playlist esteja pronto para ser reproduzido. Para implementar esse cenário, crie uma instância da classe [**MediaBinder**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder). Defina a propriedade [**Token**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Token) como uma cadeia de caracteres definida pelo aplicativo que identifica o conteúdo para o qual você deseja adiar a recuperação e, em seguida, registre um manipulador para o evento [**Binding**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Binding). Em seguida, crie uma **MediaSource** do **Binder** ao chamar [**MediaSource.CreateFromMediaBinder**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediabinder). Em seguida, crie um **MediaPlaybackItem** da **MediaSource** e o adicione à playlist normalmente.

[!code-cs[InitMediaBinder](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetInitMediaBinder)]

Quando o sistema determina que o conteúdo associado a **MediaBinder** precisa ser recuperado, ele gerará o evento **Binding**. No manipulador para este evento, você pode recuperar a instância **MediaBinder** dos [**MediaBindingEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs) passados para o evento. Recupere a cadeia de caracteres especificada para a propriedade **Token** e use-a para determinar qual conteúdo deve ser recuperado. **MediaBindingEventArgs** fornece métodos para a configuração do conteúdo associado em diversas representações diferentes, incluindo [**SetStorageFile**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setstoragefile), [**SetStream**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setstream), [**SetStreamReference**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setstreamreference) e [**SetUri**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.seturi). 

[!code-cs[BinderBinding](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetBinderBinding)]

Observe que, se você estiver executando operações assíncronas, como solicitações da Web, no manipulador de eventos **Binding**, você deve chamar o método [**MediaBindingEventArgs.GetDeferral**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.GetDeferral) para instruir o sistema a aguardar até que a operação seja concluída antes de continuar. Chame [**Deferral.Complete**](https://docs.microsoft.com/uwp/api/windows.foundation.deferral.Complete) depois que a operação estiver concluída para instruir o sistema a continuar.

A partir do Windows 10, versão 1703, você pode fornecer uma [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) como conteúdo associado ao chamar [**SetAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setadaptivemediasource). Para saber mais sobre como usar o streaming adaptável em seu app, veja [Streaming adaptável](adaptive-streaming.md).



## <a name="related-topics"></a>Tópicos relacionados
* [Reprodução de mídia](media-playback.md)
* [Reproduzir áudio e vídeo com o MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Integrar aos controles de transporte de mídia do sistema](integrate-with-systemmediatransportcontrols.md)
* [Reproduzir mídia em segundo plano](background-audio.md)

