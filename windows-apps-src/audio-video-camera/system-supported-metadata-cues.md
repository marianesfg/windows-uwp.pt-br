---
author: drewbatgit
ms.assetid: F28162D4-AACC-4EE0-B243-5878F870F87F
description: Manipular indicações de metadados com suporte do sistema durante a reprodução de mídia
title: Indicações de metadados programados com suporte do sistema
ms.author: drewbat
ms.date: 04/18/2017
ms.topic: article
keywords: windows 10, uwp, metadados, indicação, controle por voz, capítulo
ms.localizationpriority: medium
ms.openlocfilehash: 1e97c913764db24c68ce7becdba0fc283e1a3b73
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7424088"
---
# <a name="system-supported-timed-metadata-cues"></a>Indicações de metadados programados com suporte do sistema
Este artigo descreve como tirar proveito dos vários formatos de metadados programados podem ser inseridos em arquivos de mídia ou em fluxos. Os aplicativos UWP podem se registrar em eventos gerados pelo pipeline de mídia durante a reprodução sempre que essas indicações de metadados forem encontradas. Usando a classe [**DataCue**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.DataCue), os apps podem implementar suas próprias indicações de metadados personalizados, mas este artigo se concentra em vários padrões de metadados detectados automaticamente pelo pipeline de mídia, incluindo:

* Legendas baseadas em imagem no formato VobSub
* Indicações de controle por voz, incluindo limites de palavra, limites de sentença e indicadores de SSML (Linguagem de Marcação de Sintetização de Voz)
* Indicações de capítulo
* Comentários de M3U estendidos
* Marcas ID3
* Caixas de mensagens de evento mp4 fragmentadas


Este artigo se baseia em conceitos discutidos no artigo [Itens de mídia, playlists e faixas](media-playback-with-mediasource.md), que inclui as noções básicas de trabalhar com as classes [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource), [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem), e [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580) e as diretrizes gerais para usar metadados programados em seu aplicativo.

As etapas de implementação básicas são as mesmas para todos os diferentes tipos de metadados programados descritos neste artigo:

1. Criar uma [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) e então um [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) para o conteúdo a ser reproduzido.
2. Registre-se no evento [**MediaPlaybackItem.TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged), que ocorre conforme as subfaixas do item de mídia são resolvidas pelo pipeline de mídia.
3. Registre-se nos eventos [**TimedMetadataTrack.CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) e [**TimedMetadataTrack.CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) para as faixas de metadados programados que deseja usar.
4. No manipulador de eventos **CueEntered**, atualize sua interface do usuário com base nos metadados passados nos argumentos do evento. Você pode atualizar a interface do usuário novamente, para remover o texto do subtítulo atual, por exemplo, no evento **CueExited**.

Neste artigo, a manipulação de cada tipo de metadados é mostrada como um cenário diferente, mas é possível manipular (ou ignorar) diferentes tipos de metadadados usando a maior parte do código compartilhado. Você pode verificar a propriedade [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) do objeto [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) em vários pontos no processo. Assim, por exemplo, você pode decidir se vai se registrar no evento **CueEntered** para faixas de metadados com o valor **TimedMetadataKind.ImageSubtitle**, mas não para faixas com o valor **TimedMetadataKind.Speech**. Em vez disso, é possível registrar um manipulador para todos os tipos de faixa de metadados e, em seguida, verifique o valor **TimedMetadataKind** dentro do manipulador **CueEntered** para determinar qual ação deve ser executada em resposta à indicação.

## <a name="image-based-subtitles"></a>Legendas baseadas em imagem
A partir do Windows 10, versão 1703, os aplicativos UWP podem dar suporte a subtítulos externos baseados em imagem no formato VobSub. Para usar esse recurso, primeiro crie um objeto [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) para o conteúdo de mídia para os quais os subtítulos de imagem serão exibidos. Em seguida, crie um objeto [**TimedTextSource**](https://docs.microsoft.com/uwp/api/windows.media.core.timedtextsource) chamado [**CreateFromUriWithIndex**](https://docs.microsoft.com/uwp/api/windows.media.core.timedtextsource.CreateFromUriWithIndex) ou [**CreateFromStreamWithIndex**](https://docs.microsoft.com/uwp/api/windows.media.core.timedtextsource.CreateFromStreamWithIndex), passando no Uri do arquivo .sub com os dados de imagem de subtítulo e o arquivo .idx com as informações de intervalo para os subtítulos. Adicione a **TimedTextSource** à **MediaSource** ao adicioná-la à coleção [**ExternalTimedTextSources**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.ExternalTimedTextSources) da origem. Crie um [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) do **MediaSource**.

[!code-cs[ImageSubtitleLoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitleLoadContent)]

Registre-se nos eventos de metadados de legenda da imagem usando o objeto **MediaPlaybackItem** criado na etapa anterior. Este exemplo usa um método auxiliar, **RegisterMetadataHandlerForImageSubtitles**, para se registrar para os eventos. Uma expressão lambda é usada para implementar um manipulador para o evento [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged), que ocorre quando o sistema detecta uma alteração nas faixas de metadados associadas a um **MediaPlaybackItem**. Em alguns casos, as faixas de metadados podem estar disponíveis quando o item de reprodução é inicialmente resolvido e, fora do manipulador **TimedMetadataTracksChanged**, nós também percorremos em loop as faixas de metadados disponíveis e chamamos **RegisterMetadataHandlerForImageSubtitles**.

[!code-cs[ImageSubtitleTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitleTracksChanged)]

Depois de se registrar nos eventos de metadados de legenda da imagem, o **MediaItem** é atribuído a um [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) para reprodução dentro de um [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement).

[!code-cs[ImageSubtitlePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitlePlay)]

No método auxiliar **RegisterMetadataHandlerForImageSubtitles**, obtenha uma instância da classe [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) ao indexar a coleção **TimedMetadataTracks** do **MediaPlaybackItem**. Registre-se no evento [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) e no evento [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited). Então, você deve chamar [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) na coleção **TimedMetadataTracks** do item de reprodução para instruir o sistema de que o app quer receber eventos de indicação de metadados para esse item de reprodução.

[!code-cs[RegisterMetadataHandlerForImageSubtitles](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForImageSubtitles)]

No manipulador para o evento **CueEntered**, você pode verificar a propriedade [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) do objeto [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) passado para o manipulador para ver se os metadados são para legendas da imagem. Isso será necessário se você estiver usando o mesmo manipulador de eventos de indicação de dados para vários tipos de metadados. Se a faixa de metadados associados for do tipo **TimedMetadataKind.ImageSubtitle**, converta a indicação de dados contida na propriedade **Indicação** dos [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) em uma [**ImageCue**](https://docs.microsoft.com/uwp/api/windows.media.core.imagecue). A propriedade [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.media.core.imagecue.SoftwareBitmap) da **ImageCue** contém uma representação [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap) da imagem do subtítulo. Crie um [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource) e chame [**SetBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.SetBitmapAsync) para atribuir a imagem a um controle [**Image**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image) XAML. As propriedades [**Extent**](https://docs.microsoft.com/uwp/api/windows.media.core.imagecue.Extent) e [**Position**](https://docs.microsoft.com/uwp/api/windows.media.core.imagecue.Position) da **ImageCue** fornecem informações sobre o tamanho e a posição da imagem do subtítulo.

[!code-cs[ImageSubtitleCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitleCueEntered)]

## <a name="speech-cues"></a>Indicações de controle por voz
A partir do Windows 10, versão 1703, os aplicativos UWP podem se registrar para receber eventos em resposta a limites de palavras, limites de frases e indicadores SSML (Speech Synthesis Markup Language) em mídia reproduzida. Isso permite que você reproduza streamings de áudio gerados com a classe [**SpeechSynthesizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechSynthesis.SpeechSynthesizer) e atualize sua interface do usuário com base nesses eventos, como a exibição do texto da palavra ou frase que está sendo reproduzida.

O exemplo mostrado nesta seção usa uma variável de membro de classe para armazenar uma cadeia de caracteres de texto que será sintetizada e reproduzida.

[!code-cs[SpeechInputText](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechInputText)]

Crie uma nova instância da classe **SpeechSynthesizer**. Defina as opções [**IncludeWordBoundaryMetadata**](https://docs.microsoft.com/uwp/api/windows.media.speechsynthesis.speechsynthesizeroptions.IncludeWordBoundaryMetadata) e [**IncludeSentenceBoundaryMetadata**](https://docs.microsoft.com/uwp/api/windows.media.speechsynthesis.speechsynthesizeroptions.IncludeSentenceBoundaryMetadata) para o sintetizador como **true** para especificar que os metadados devem ser incluídos no streaming de mídia gerado. Chame [**SynthesizeTextToStreamAsync**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechSynthesis.SpeechSynthesizer.SynthesizeTextToStreamAsync) para gerar um streaming com o controle por voz sintetizado e os metadados correspondentes. Crie um [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) e um [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) do streaming sintetizado.

[!code-cs[SynthesizeSpeech](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSynthesizeSpeech)]

Registre-se nos eventos de metadados de controle por voz usando o objeto **MediaPlaybackItem** . Este exemplo usa um método auxiliar, **RegisterMetadataHandlerForSpeech**, para se registrar nos eventos. Uma expressão lambda é usada para implementar um manipulador para o evento [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged), que ocorre quando o sistema detecta uma alteração nas faixas de metadados associadas a um **MediaPlaybackItem**.  Em alguns casos, as faixas de metadados podem estar disponíveis quando o item de reprodução é inicialmente resolvido e, fora do manipulador **TimedMetadataTracksChanged**, nós também percorremos em loop as faixas de metadados disponíveis e chamamos **RegisterMetadataHandlerForSpeech**.

[!code-cs[SpeechTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechTracksChanged)]

Depois de se registrar nos eventos de metadados de controle por voz, o **MediaItem** é atribuído a um [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) para reprodução dentro de um [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement).

[!code-cs[SpeechPlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechPlay)]

No método auxiliar **RegisterMetadataHandlerForSpeech**, obtenha uma instância da classe [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) ao indexar a coleção **TimedMetadataTracks** do **MediaPlaybackItem**. Registre-se no evento [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) e no evento [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited). Então, você deve chamar [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) na coleção **TimedMetadataTracks** do item de reprodução para instruir o sistema de que o app quer receber eventos de indicação de metadados para esse item de reprodução.

[!code-cs[RegisterMetadataHandlerForWords](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForWords)]

No manipulador para o evento **CueEntered**, você pode verificar a propriedade [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) do objeto [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) passado para o manipulador para ver se os metadados são para controle por fala. Isso será necessário se você estiver usando o mesmo manipulador de eventos de indicação de dados para vários tipos de metadados. Se a faixa de metadados associados for do tipo **TimedMetadataKind.Speech**, converta a indicação de dados contida na propriedade **Indicação** dos [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) em uma [**SpeechCue**](https://docs.microsoft.com/uwp/api/windows.media.core.speechcue). Para indicações de fala, o tipo de indicação de fala incluído na faixa de metadados é determinado por meio da verificação da propriedade **Label**. O valor desta propriedade será "SpeechWord" para limites de palavras, "SpeechSentence" para limites de frases ou "SpeechBookmark" para indicadores SSML. Neste exemplo, verificamos o valor de "SpeechWord", e se esse valor for encontrado, as propriedades [**StartPositionInInput**](https://docs.microsoft.com/uwp/api/windows.media.core.speechcue.StartPositionInInput) e [**EndPositionInInput**](https://docs.microsoft.com/uwp/api/windows.media.core.speechcue.EndPositionInInput) da **SpeechCue** serão usadas para determinar a localização no texto de entrada da palavra que está sendo reproduzida. Este exemplo simplesmente gera cada palavra para a saída de depuração.

[!code-cs[SpeechWordCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechWordCueEntered)]

## <a name="chapter-cues"></a>Indicações de capítulo
A partir do Windows 10, versão 1703, os aplicativos UWP podem se registrar em indicações que correspondam aos capítulos dentro de um item de mídia. Para usar esse recurso, crie um objeto [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) para o conteúdo de mídia e então crie um [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) desde a **MediaSource**.

[!code-cs[ChapterCueLoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCueLoadContent)]

Registre-se nos eventos de metadados de capítulo usando o objeto **MediaPlaybackItem** criado na etapa anterior. Este exemplo usa um método auxiliar, **RegisterMetadataHandlerForChapterCues**, para se registrar nos eventos. Uma expressão lambda é usada para implementar um manipulador para o evento [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged), que ocorre quando o sistema detecta uma alteração nas faixas de metadados associadas a um **MediaPlaybackItem**. Em alguns casos, as faixas de metadados podem estar disponíveis quando o item de reprodução é inicialmente resolvido e, fora do manipulador **TimedMetadataTracksChanged**, nós também percorremos em loop as faixas de metadados disponíveis e chamamos **RegisterMetadataHandlerForChapterCues**.

[!code-cs[ChapterCueTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCueTracksChanged)]

Depois de se registrar nos eventos de metadados de capítulo, o **MediaItem** é atribuído a um [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) para reprodução dentro de um [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement).

[!code-cs[ChapterCuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCuePlay)]

No método auxiliar **RegisterMetadataHandlerForChapterCues**, obtenha uma instância da classe [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) ao indexar a coleção **TimedMetadataTracks** do **MediaPlaybackItem**. Registre-se no evento [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) e no evento [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited). Então, você deve chamar [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) na coleção **TimedMetadataTracks** do item de reprodução para instruir o sistema de que o app quer receber eventos de indicação de metadados para esse item de reprodução.

[!code-cs[RegisterMetadataHandlerForChapterCues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForChapterCues)]

No manipulador do evento **CueEntered**, você pode verificar a propriedade [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) do objeto [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) passado para o manipulador a fim de verificar se os metadados são para indicações de capítulo. Isso é necessário se você estiver utilizando o mesmo manipulador de evento de indicação de dados para vários tipos de metadados. Se a faixa de metadados associados for do tipo **TimedMetadataKind.Chapter**, converta a indicação de dados contida na propriedade **Indicação** dos [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) em uma [**ChapterCue**](https://docs.microsoft.com/uwp/api/windows.media.core.chaptercue). A propriedade [**Title**](https://docs.microsoft.com/uwp/api/windows.media.core.chaptercue.Title) da **ChapterCue** contém o título do capítulo que acabou de ser alcançado na reprodução.

[!code-cs[ChapterCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCueEntered)]

### <a name="seek-to-the-next-chapter-using-chapter-cues"></a>Buscar o próximo capítulo usando indicações de capítulo
Além de receber notificações quando o capítulo atual muda em um item em reprodução, você também pode usar indicações de capítulo para buscar o próximo capítulo dentro de um item em reprodução. O método de exemplo mostrado a seguir utiliza como argumentos um [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) e um [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) que representam o item de mídia atualmente em reprodução. A coleção [**TimedMetadataTracks**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracks) é pesquisada para vermos se qualquer uma das faixas tem as propriedades [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) do valor [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) de **TimedMetadataKind.Chapter**.  Se uma faixa de capítulo for encontrada, o método fará um loop em cada indicação na coleção [**Cues**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.Cues) da faixa para encontrar a primeira indicação com [**StartTime**](https://docs.microsoft.com/uwp/api/windows.media.core.chaptercue.StartTime) maior do que a [**Position**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.Position) atual da sessão de reprodução do player de mídia. Depois que a indicação correta for encontrada, a posição da sessão de reprodução será atualizada e o título do capítulo será atualizado na interface do usuário.

[!code-cs[GoToNextChapter](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetGoToNextChapter)]

## <a name="extended-m3u-comments"></a>Comentários de M3U estendidos
A partir do Windows 10, versão 1703, os aplicativos UWP podem se registrar em indicações que correspondam aos comentários em arquivo de manifesto M3U estendido. Este exemplo usa [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) para reproduzir o conteúdo de mídia. Para saber mais, veja [Streaming adaptável](adaptive-streaming.md). Crie um **AdaptiveMediaSource** para o conteúdo ao chamar [**CreateFromUriAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromUriAsync) ou [**CreateFromStreamAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromStreamAsync). Crie um objeto [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) ao chamar [**CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.CreateFromAdaptiveMediaSource) e então crie um [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) desde **MediaSource**.

[!code-cs[EXTM3ULoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3ULoadContent)]

Registre-se nos eventos de metadados M3U usando o objeto **MediaPlaybackItem** criado na etapa anterior. Este exemplo usa um método auxiliar, **RegisterMetadataHandlerForEXTM3UCues**, para se registrar nos eventos. Uma expressão lambda é usada para implementar um manipulador para o evento [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged), que ocorre quando o sistema detecta uma alteração nas faixas de metadados associadas a um **MediaPlaybackItem**. Em alguns casos, as faixas de metadados podem estar disponíveis quando o item de reprodução é inicialmente resolvido e, fora do manipulador **TimedMetadataTracksChanged**, nós também percorremos em loop as faixas de metadados disponíveis e chamamos **RegisterMetadataHandlerForEXTM3UCues**.

[!code-cs[EXTM3UCueTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3UCueTracksChanged)]

Depois de se registrar nos eventos de metadados M3U, o **MediaItem** é atribuído a um [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) para reprodução em um [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement).

[!code-cs[EXTM3UCuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3UCuePlay)]

No método auxiliar **RegisterMetadataHandlerForEXTM3UCues**, obtenha uma instância da classe [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) ao indexar a coleção **TimedMetadataTracks** do **MediaPlaybackItem**. Verifique a propriedade DispatchType da faixa de metadados, que terá um valor "EXTM3U" se a faixa representar comentários M3U. Registre-se no evento [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) e no evento [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited). Então, você deve chamar [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) na coleção **TimedMetadataTracks** do item de reprodução para instruir o sistema de que o app quer receber eventos de indicação de metadados para esse item de reprodução.

[!code-cs[RegisterMetadataHandlerForEXTM3UCues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForEXTM3UCues)]

No manipulador para o evento **CueEntered**, converta a indicação de dados contida na propriedade **Cue** de [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) em uma [**DataCue**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue).  Verifique se a **DataCue** e a propriedade [**Data**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue.Data) da indicação não são nulas. Os comentários EMU estendidos são fornecidos no formato de cadeias de caracteres UTF-16 little endian terminadas em nulo. Crie um novo **DataReader** ler os dados de indicação ao chamar [**DataReader.FromBuffer**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.FromBuffer). Defina a propriedade [**UnicodeEncoding**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.UnicodeEncoding) do leitor como [**Utf16LE**](https://docs.microsoft.com/uwp/api/windows.storage.streams.unicodeencoding) para ler os dados no formato correto. Chame [**ReadString**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.ReadString) para ler os dados, especificando a metade do comprimento do campo **Data**, já que cada caractere tem dois bytes de tamanho, e subtraia um deles para remover o caractere nulo à direita. Neste exemplo, o comentário M3U é simplesmente gravado na saída de depuração.

[!code-cs[EXTM3UCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3UCueEntered)]

## <a name="id3-tags"></a>Marcas ID3
A partir do Windows 10, versão 1703, os aplicativos UWP podem se registrar em indicações que correspondam às marcas ID3 no conteúdo HLS (Http Live Streaming). Este exemplo usa [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) para reproduzir o conteúdo de mídia. Para saber mais, veja [Streaming adaptável](adaptive-streaming.md). Crie um **AdaptiveMediaSource** para o conteúdo ao chamar [**CreateFromUriAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromUriAsync) ou [**CreateFromStreamAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromStreamAsync). Crie um objeto [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) ao chamar [**CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.CreateFromAdaptiveMediaSource) e então crie um [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) desde **MediaSource**.

[!code-cs[EXTM3ULoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3ULoadContent)]

Registre-se nos eventos de marca ID3 usando o objeto **MediaPlaybackItem** criado na etapa anterior. Este exemplo usa um método auxiliar, **RegisterMetadataHandlerForID3Cues**, para se registrar nos eventos. Uma expressão lambda é usada para implementar um manipulador para o evento [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged), que ocorre quando o sistema detecta uma alteração nas faixas de metadados associadas a um **MediaPlaybackItem**. Em alguns casos, as faixas de metadados podem estar disponíveis quando o item de reprodução é inicialmente resolvido e, fora do manipulador **TimedMetadataTracksChanged**, nós também percorremos em loop as faixas de metadados disponíveis e chamamos **RegisterMetadataHandlerForID3Cues**.

[!code-cs[ID3LoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3LoadContent)]

Depois de se registrar nos eventos de metadados ID3 o **MediaItem** é atribuído a um [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) para reprodução dentro de um [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement).

[!code-cs[ID3CuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3CuePlay)]


No método auxiliar **RegisterMetadataHandlerForID3Cues**, obtenha uma instância da classe [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) ao indexar a coleção **TimedMetadataTracks** do **MediaPlaybackItem**. Verifique a propriedade DispatchType da faixa de metadados, que terá um valor com a cadeia de caracteres do GUID "15260DFFFF49443320FF49443320000F" se a faixa representar marcas ID3. Registre-se no evento [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) e no evento [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited). Então, você deve chamar [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) na coleção **TimedMetadataTracks** do item de reprodução para instruir o sistema de que o app quer receber eventos de indicação de metadados para esse item de reprodução.

[!code-cs[RegisterMetadataHandlerForID3Cues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForID3Cues)]

No manipulador para o evento **CueEntered**, converta a indicação de dados contida na propriedade **Cue** de [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) em uma [**DataCue**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue).  Verifique se a **DataCue** e a propriedade [**Data**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue.Data) da indicação não são nulas. Os comentários EMU estendidos são fornecidos no formato de bytes brutos no streaming de transporte (veja [http://id3.org/id3v2.4.0-structure](http://id3.org/id3v2.4.0-structure)). Crie um novo **DataReader** ler os dados de indicação ao chamar [**DataReader.FromBuffer**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.FromBuffer).  Neste exemplo, os valores de cabeçalho da marca ID3 são lidos nos dados de indicação e gravados na saída de depuração.

[!code-cs[ID3CueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3CueEntered)]

## <a name="fragmented-mp4-emsg-boxes"></a>Caixas de mensagens de evento mp4 fragmentadas
A partir do Windows 10, versão 1703, os aplicativos UWP podem se registrar em indicações que correspondam às caixas de mensagens de evento dentro de streamings mp4 fragmentados. Um exemplo de uso desse tipo de metadados para provedores de conteúdo seria sinalizar aplicativos cliente para reproduzirem um anúncio durante a transmissão de conteúdo dinâmico. Este exemplo usa [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) para reproduzir o conteúdo de mídia. Para saber mais, veja [Streaming adaptável](adaptive-streaming.md). Crie um **AdaptiveMediaSource** para o conteúdo ao chamar [**CreateFromUriAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromUriAsync) ou [**CreateFromStreamAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromStreamAsync). Crie um objeto [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) ao chamar [**CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.CreateFromAdaptiveMediaSource) e então crie um [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) desde **MediaSource**.

[!code-cs[EmsgLoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEmsgLoadContent)]

Registre-se nos eventos de caixa de mensagens de evento usando o objeto **MediaPlaybackItem** criado na etapa anterior. Este exemplo usa um método auxiliar, **RegisterMetadataHandlerForEmsgCues**, para se registrar nos eventos. Uma expressão lambda é usada para implementar um manipulador para o evento [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged), que ocorre quando o sistema detecta uma alteração nas faixas de metadados associadas a um **MediaPlaybackItem**. Em alguns casos, as faixas de metadados podem estar disponíveis quando o item de reprodução é inicialmente resolvido e, fora do manipulador **TimedMetadataTracksChanged**, nós também percorremos em loop as faixas de metadados disponíveis e chamamos **RegisterMetadataHandlerForEmsgCues**.

[!code-cs[ID3LoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3LoadContent)]

Depois de se registrar nos eventos de metadados de caixa de mensagens de evento, o **MediaItem** é atribuído a um [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) para reprodução dentro de um [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement).

[!code-cs[EmsgCuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEmsgCuePlay)]


No método auxiliar **RegisterMetadataHandlerForEmsgCues**, obtenha uma instância da classe [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) ao indexar a coleção **TimedMetadataTracks** do **MediaPlaybackItem**. Verifique a propriedade DispatchType da faixa de metadados, que terá um valor "emsg:mp4" se a faixa representar caixas de mensagens de evento. Registre-se no evento [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) e no evento [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited). Então, você deve chamar [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) na coleção **TimedMetadataTracks** do item de reprodução para instruir o sistema de que o app quer receber eventos de indicação de metadados para esse item de reprodução.


[!code-cs[RegisterMetadataHandlerForEmsgCues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForEmsgCues)]


No manipulador para o evento **CueEntered**, converta a indicação de dados contida na propriedade **Cue** de [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) em uma [**DataCue**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue).  Verifique se o objeto **DataCue** não é nulo. As propriedades da caixa de mensagens de evento são fornecidas pelo pipeline de mídia como propriedades personalizadas na coleção [**Properties**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue.Properties) do objeto DataCue. Este exemplo tenta extrair diversos valores de propriedade diferentes usando o método **[TryGetValue](https://docs.microsoft.com/uwp/api/windows.foundation.collections.propertyset.trygetvalue)**. Se esse método retornar nulo, isso significa que a propriedade solicitada não está presente na caixa de mensagens de evento e, portanto, será definido um valor padrão.

A próxima parte do exemplo ilustra o cenário em que a reprodução de anúncio é disparada, que é o caso quando a propriedade *scheme_id_uri*, obtida na etapa anterior, tem um valor "urn: scte:scte35:2013:xml" (veja [http://dashif.org/identifiers/event-schemes/](http://dashif.org/identifiers/event-schemes/)). Observe que o padrão recomenda enviar essa mensagem de evento várias vezes para redundância, portanto, este exemplo mantém uma lista de IDs mensagens de evento que já foram processadas e processa somente as novas mensagens. Crie um novo **DataReader** para ler os dados de indicação ao chamar [**DataReader.FromBuffer**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.FromBuffer) e defina a codificação como UTF-8 ao configurar a propriedade [**UnicodeEncoding**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.UnicodeEncoding), então leia os dados. Neste exemplo, a carga da mensagem é gravada na saída de depuração. Um app real usaria os dados de carga para agendar a reprodução de um anúncio.

[!code-cs[EmsgCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEmsgCueEntered)]


## <a name="related-topics"></a>Tópicos relacionados

* [Reprodução de mídia](media-playback.md)
* [Itens de mídia, playlists e faixas](media-playback-with-mediasource.md)


 




