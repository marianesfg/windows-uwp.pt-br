---
author: drewbatgit
ms.assetid: C5623861-6280-4352-8F22-80EB009D662C
description: "A classe MediaSource fornece uma maneira comum de fazer referência e reproduzir mídia de diferentes origens, como arquivos locais ou remotos, e expõe um modelo comum para acessar dados de mídia, independentemente do formato da mídia subjacente."
title: "Reprodução de mídia com o MediaSource"
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: d64f4484566d80eaf2a353b1aba954c15079343c

---

# Reprodução de mídia com o MediaSource

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


\[Algumas informações dizem respeito a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não faz nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.\]

A classe [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930905) fornece uma maneira comum de referenciar e reproduzir mídia de diferentes fontes, como arquivos locais ou remotos, e expõe um modelo comum para acessar dados de mídia, independentemente do formato de mídia subjacente. A classe [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939) estende a funcionalidade do **MediaSource**, permitindo que você gerencie e selecione várias faixas de áudio, vídeo e metadados contidas em um item de mídia. 
            [
              **MediaPlaybackList**
            ](https://msdn.microsoft.com/library/windows/apps/dn930955) permite que você crie listas de reprodução a partir de um ou mais itens de reprodução de mídia.

O código neste artigo foi adaptado da amostra do [SDK de reprodução de vídeo](http://go.microsoft.com/fwlink/p/?LinkId=620020&clcid=0x409). Você pode baixar essa amostra para ver o código usado no contexto ou usá-lo como ponto de partida para seu próprio aplicativo.

## Criar e executar um MediaSource

Crie uma nova instância do **MediaSource** chamando um dos métodos de fábrica expostos pela classe:

-   [**CreateFromAdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930906)
-   [**CreateFromIMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn965527)
-   [**CreateFromMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn930907)
-   [**CreateFromMseStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn930908)
-   [**CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909)
-   [**CreateFromStream**](https://msdn.microsoft.com/library/windows/apps/dn930910)
-   [**CreateFromStreamReference**](https://msdn.microsoft.com/library/windows/apps/dn930911)
-   [**CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912)

Depois de criar um **MediaSource**, você pode reproduzir a fonte diretamente com um [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926), chamando [**SetPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/dn899085), ou com um [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/dn652535), definindo a propriedade [**Source**](https://msdn.microsoft.com/library/windows/apps/dn987010). O exemplo a seguir mostra como reproduzir um arquivo de mídia selecionado pelo usuário um **MediaElement** usando **MediaSource**.

Você precisará incluir os namespaces [**Windows.Media.Core**](https://msdn.microsoft.com/library/windows/apps/dn278962) e [**Windows.Media.Playback**](https://msdn.microsoft.com/library/windows/apps/dn640562) para completar esse cenário.

[!code-cs[Usando](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetUsing)]

Declare uma variável de membro do tipo **MediaSource**. Para os exemplos deste artigo, a fonte de mídia é declarada como membro da classe para poder ser acessada a partir de vários locais.

[!code-cs[DeclareMediaSource](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaSource)]

Para permitir que o usuário selecione um arquivo de mídia para reproduzir, use um [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847). Com o objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) retornado no método [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) do seletor, inicialize um novo MediaObject chamando [**MediaSource.CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909). Por fim, defina a fonte de mídia como a fonte de reprodução para o **MediaElement** chamando o método [**SetPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/dn899085).

[!code-cs[PlayMediaSource](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetPlayMediaSource)]

## Manipular várias faixas de áudio, vídeo e metadados com MediaPlaybackItem

Usar um [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930905) para reprodução é conveniente porque ele fornece uma maneira comum de reproduzir mídia de tipos de fontes diferentes, mas o comportamento mais avançado pode ser acessado usando um [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939). Isso inclui a capacidade de acessar e gerenciar várias faixas de áudio, vídeo e dados de um item de mídia.

Declare uma variável para armazenar seu **MediaPlaybackItem**.

[!code-cs[DeclareMediaPlaybackItem](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaPlaybackItem)]

Crie um **MediaPlaybackItem** chamando o construtor e passando um objeto **MediaSource** inicializado.

Se seu aplicativo possui suporte a várias faixas de áudio, vídeo ou dados em um item de reprodução de mídia, registre manipuladores de eventos para os eventos [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930948), [**VideoTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930954) ou [**TimedMetadataTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930952).

Por fim, defina a origem da reprodução do **MediaElement** ou **MediaPlayer** para sua **MediaPlaybackItem**.

[!code-cs[PlayMediaPlaybackItem](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetPlayMediaPlaybackItem)]

**Observação**  
Um **MediaSource** só pode ser associado a um único **MediaPlaybackItem**. Depois de criar um **MediaPlaybackItem** a partir de uma fonte, a tentativa de criar outro item de reprodução a partir da mesma fonte resultará em um erro. Além disso, depois de criar um **MediaPlaybackItem** a partir de uma fonte de mídia, você não pode definir o objeto **MediaSource** diretamente como a fonte de um **MediaElement** ou **MediaPlayer**, mas deve usar o **MediaPlaybackItem**.

O evento [**VideoTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930954) é lançado depois que um **MediaPlaybackItem** que contém várias faixas de vídeos é atribuído como fonte de reprodução, e pode ser gerado novamente se a lista de faixas de vídeo do item for alterada. O manipulador desse evento oferece a oportunidade de atualizar sua interface do usuário para permitir que o usuário alterne entre as faixas disponíveis. Este exemplo usa um [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348) para exibir as faixas de vídeo disponíveis.

[!code-xml[VideoComboBox](./code/MediaSource_Win10/cs/MainPage.xaml#SnippetVideoComboBox)]

No manipulador **VideoTracksChanged**, execute um loop por todas as faixas na lista [**VideoTracks**](https://msdn.microsoft.com/library/windows/apps/dn930953) do item de reprodução. Para cada faixa, um novo [**ComboBoxItem**](https://msdn.microsoft.com/library/windows/apps/br209349) é criado. Se a faixa ainda não tiver um rótulo, um rótulo será gerado do índice de faixas. A propriedade [**Tag**](https://msdn.microsoft.com/library/windows/apps/br208745) do item da caixa de combinação é definida como o índice de faixas para que ele possa ser identificado mais tarde. Por fim, o item é adicionado à caixa de combinação. Observe que essas operações serão executadas em uma chamada [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317), pois todas as alterações da interface do usuário devem ser feitas no thread da interface do usuário, e esse evento é lançado em um thread diferente.

[!code-cs[VideoTracksChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetVideoTracksChanged)]

No manipulador [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) da caixa de combinação, o índice de faixas é recuperado a partir da propriedade **Tag** do item selecionado. Definir a propriedade [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/dn956634) da lista [**VideoTracks**](https://msdn.microsoft.com/library/windows/apps/dn930953) do item de reprodução de mídia faz com que o **MediaElement** ou o **MediaPlayer** alterne a faixa de vídeo ativa para o índice especificado.

[!code-cs[VideoTracksSelectionChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetVideoTracksSelectionChanged)]

O gerenciamento de itens de mídia com várias faixas de áudio funciona exatamente da mesma forma do que com faixas de vídeo. Manipule o evento [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930948) para atualizar a interface do usuário com as faixas de áudio encontradas na lista [**AudioTracks**](https://msdn.microsoft.com/library/windows/apps/dn930947) do item de reprodução. Quando o usuário selecionar uma faixa de áudio, defina a propriedade [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/dn930937) da lista **AudioTracks** para fazer com que o **MediaElement** ou o **MediaPlayer** alterne a faixa de áudio ativa para o índice especificado.

[!code-xml[AudioComboBox](./code/MediaSource_Win10/cs/MainPage.xaml#SnippetAudioComboBox)]

[!code-cs[AudioTracksChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetAudioTracksChanged)]

[!code-cs[AudioTracksSelectionChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetAudioTracksSelectionChanged)]

Além de áudio e vídeo, um objeto **MediaPlaybackItem** pode conter zero ou mais objetos [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580). Uma faixa de metadados com tempo pode conter texto de legenda ou pode conter dados personalizados que são próprios de seu aplicativo. Uma faixa de metadados com tempo contém uma lista de indicações representadas por objetos herdados de [**IMediaCue**](https://msdn.microsoft.com/library/windows/apps/dn930899), como [**DataCue**](https://msdn.microsoft.com/library/windows/apps/dn930892) ou [**TimedTextCue**](https://msdn.microsoft.com/library/windows/apps/dn956655) Cada indicação tem uma hora de início e uma duração que determinam quando a indicação será ativada e por quanto tempo.

Semelhante às faixas de áudio e vídeo, as faixas de metadados com tempo de um item de mídia podem ser descobertas manipulando o evento [**TimedMetadataTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930952) de um **MediaPlaybackItem**. Com as faixas de metadados com tempo, porém, o usuário pode habilitar mais de uma faixa de metadados por vez. Além disso, dependendo do cenário do aplicativo, convém habilitar ou desabilitar faixas de metadados automaticamente, sem a intervenção do usuário. Para fins de ilustração, este exemplo adiciona um [**ToggleButton**](https://msdn.microsoft.com/library/windows/apps/br209795) para cada faixa de metadados em um item de mídia para permitir que o usuário habilite e desabilite a faixa. A propriedade **Tag** de cada botão é definida como o índice da faixa de metadados associada para que ela possa ser identificada quando o botão for pressionado.

[!code-xml[MetaStackPanel](./code/MediaSource_Win10/cs/MainPage.xaml#SnippetMetaStackPanel)]

[!code-cs[TimedMetadataTrackschanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTimedMetadataTrackschanged)]

Como mais de uma faixa de metadados pode estar ativa por vez, você simplesmente não define o índice ativo para a lista de faixas de metadados. Em vez disso, chame o método [**SetPresentationMode**](https://msdn.microsoft.com/library/windows/apps/dn986977) do objeto **MediaPlaybackItem**, passando o índice da faixa que você deseja alternar e fornecendo um valor da enumeração [**TimedMetadataTrackPresentationMode**](https://msdn.microsoft.com/library/windows/apps/dn987016). O modo de apresentação que você escolher depende da implementação de seu aplicativo. Neste exemplo, a faixa de metadados é definida como **PlatformPresented** quando habilitada. Para faixas baseadas em texto, isso significa que o sistema exibirá automaticamente as indicações de texto na faixa. Quando o botão de alternância é desativado, o modo de apresentação é definido como **Disabled**, o que significa que nenhum texto é exibido e nenhum evento de indicação é lançado. Os eventos de indicação são explicados mais adiante neste artigo.

[!code-cs[ToggleChecked](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetToggleChecked)]

[!code-cs[ToggleUnchecked](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetToggleUnchecked)]

## Adicionar texto com tempo externo com TimedTextSource

Para alguns cenários, você pode ter arquivos externos que contenham texto com tempo associado a um item de mídia, como arquivos separados que contêm legendas para localidades diferentes. Use a classe [**TimedTextSource**](https://msdn.microsoft.com/library/windows/apps/dn956679) para carregar em arquivos de texto com tempo externos de um fluxo ou URI.

Este exemplo usa uma coleção **Dictionary** para armazenar uma lista das fontes de texto com tempo para o item de mídia usando o URI da fonte e o objeto **TimedTextSource** como o par de chave/valor para identificar as faixas depois que elas forem resolvidas.

[!code-cs[TimedTextSourceMap](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTimedTextSourceMap)]

Crie um novo **TimedTextSource** para cada arquivo de texto com tempo externo chamando [**CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn708190). Adicione uma entrada ao **Dictionary** para a fonte de texto com tempo. Adicione um manipulador para o evento [**TimedTextSource.Resolved**](https://msdn.microsoft.com/library/windows/apps/dn965540) a ser manipulado se o item não for carregado ou para definir propriedades adicionais depois que o item for carregado com êxito.

Registre todos os seus objetos **TimedTextSource** com o **MediaSource** adicionando-os à coleção [**ExternalTimedTextSources**](https://msdn.microsoft.com/library/windows/apps/dn930916). Observe que fontes de texto com tempo externas são adicionadas diretamente ao **MediaSource** e não ao **MediaPlaybackItem** criado a partir da fonte. Para atualizar a interface do usuário para refletir as faixas de texto externo, registre e manipule o evento **TimedMetadataTracksChanged** conforme descrito anteriormente neste artigo.

[!code-cs[TimedTextSource](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTimedTextSource)]

No manipulador para o evento [**TimedTextSource.Resolved**](https://msdn.microsoft.com/library/windows/apps/dn965540), verifique a propriedade **Error** do [**TimedTextSourceResolveResultEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn965537) passado para o manipulador para determinar se ocorreu um erro ao tentar carregar os dados de texto com tempo. Se o item for resolvido com êxito, você poderá usar esse manipulador para atualizar as propriedades adicionais da faixa resolvida. Este exemplo adiciona um rótulo para cada faixa com base no URI armazenado anteriormente no **Dictionary**.

[!code-cs[TimedTextSourceResolved](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTimedTextSourceResolved)]

## Adicionar mais faixas de metadados

Você pode criar faixas de metadados personalizadas dinamicamente no código e associá-las a uma fonte de mídia. As faixas que você cria podem conter texto de legenda ou podem conter seus dados próprios do aplicativo.

Crie um novo [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580) chamando o construtor e especificando um ID, o identificador do idioma e um valor da enumeração [**TimedMetadataKind**](https://msdn.microsoft.com/library/windows/apps/dn956578). Registre manipuladores para os eventos [**CueEntered**](https://msdn.microsoft.com/library/windows/apps/dn956583) e [**CueExited**](https://msdn.microsoft.com/library/windows/apps/dn956584). Esses eventos são lançados na chegada da hora de início de uma indicação e quando a duração de uma indicação tiver expirado, respectivamente.

Crie um novo objeto de indicação, adequado para o tipo de faixa de metadados que você criou, e defina a ID, a hora de início e a duração da faixa. Este exemplo cria uma faixa de dados, então, um conjunto de objetos [**DataCue**](https://msdn.microsoft.com/library/windows/apps/dn930892) é gerado, e um buffer que contém dados específicos do aplicativo é fornecido para cada indicação. Para registrar a nova faixa, adicione-a à coleção [**ExternalTimedMetadataTracks**](https://msdn.microsoft.com/library/windows/apps/dn930915) do objeto **MediaSource**.

[!code-cs[AddDataTrack](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetAddDataTrack)]

O evento **CueEntered** é acionado quando o horário de início de uma indicação tiver sido atingido, desde que a faixa associada tenha um modo de apresentação de **ApplicationPresented**, **Hidden** ou **PlatformPresented.** Eventos de sinalização não serão gerados para faixas de metadados enquanto o modo de apresentação para a faixa for **Desabilitado**. Este exemplo simplesmente gera os dados personalizados associados à indicação para a janela de depuração.

[!code-cs[DataCueEntered](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetDataCueEntered)]

Este exemplo adiciona uma faixa de texto personalizada especificando **TimedMetadataKind.Caption** ao criar a faixa e usando objetos [**TimedTextCue**](https://msdn.microsoft.com/library/windows/apps/dn956655) para adicionar indicações à faixa.

[!code-cs[AddTextTrack](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetAddTextTrack)]

[!code-cs[TextCueEntered](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTextCueEntered)]

## Reproduzir uma lista de itens de mídia com MediaPlaybackList

[**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) permite criar uma playlist de itens de mídia, que são representados por objetos **MediaPlaybackItem**.


            **Observação**  Itens em uma [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) são renderizadas usando-se a reprodução sem intervalos. O sistema usará metadados fornecidos em arquivos MP3 ou AAC codificados para determinar o atraso ou a compensação de preenchimento necessária à reprodução sem intervalos. Se os arquivos MP3 ou AAC codificados não fornecerem esses metadados, o sistema determinará o atraso ou o preenchimento heuristicamente. Para os formatos sem perdas, como PCM, FLAC ou ALAC, o sistema não executa nenhuma ação porque esses codificadores não apresentam atraso ou preenchimento.

Para começar, declare uma variável para armazenar seu **MediaPlaybackList**.

[!code-cs[DeclareMediaPlaybackList](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaPlaybackList)]

Crie um **MediaPlaybackItem** para cada item de mídia que você deseja adicionar à sua lista usando o mesmo procedimento descrito anteriormente neste artigo. Inicialize seu objeto **MediaPlaybackList** e adicione os itens de reprodução de mídia a ele. Registre um manipulador para o evento [**CurrentItemChanged**](https://msdn.microsoft.com/library/windows/apps/dn930957). Esse evento permite atualizar a interface do usuário para refletir o item de mídia que está sendo reproduzido. Por fim, defina a origem da reprodução do **MediaElement** ou **MediaPlayer** para sua **MediaPlaybackList**.

[!code-cs[PlayMediaPlaybackList](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetPlayMediaPlaybackList)]

No manipulador de eventos **CurrentItemChanged**, atualize a interface do usuário para refletir o item atualmente em reprodução, que pode ser recuperado usando a propriedade [**NewItem**](https://msdn.microsoft.com/library/windows/apps/dn930930) do objeto [**CurrentMediaPlaybackItemChangedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn930929) passado para o evento. Lembre-se: se você atualizar a interface do usuário a partir desse evento, deverá fazer isso chamando [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) para que as atualizações sejam feitas no thread da interface do usuário.

[!code-cs[MediaPlaybackListItemChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetMediaPlaybackListItemChanged)]

Chame [**MovePrevious**](https://msdn.microsoft.com/library/windows/apps/mt146455) ou [**MoveNext**](https://msdn.microsoft.com/library/windows/apps/mt146454) para fazer com que o media player reproduza o item anterior ou seguinte na sua **MediaPlaybackList**.

[!code-cs[PrevButton](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetPrevButton)]

[!code-cs[NextButton](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetNextButton)]

Defina a propriedade [**ShuffleEnabled**](https://msdn.microsoft.com/library/windows/apps/mt146457) para especificar se o media player deve reproduzir os itens da lista em ordem aleatória.

[!code-cs[ShuffleButton](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetShuffleButton)]

Defina a propriedade [**AutoRepeatEnabled**](https://msdn.microsoft.com/library/windows/apps/mt146452) para especificar se o media player deve reproduzir sua lista em loop.

[!code-cs[RepeatButton](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetRepeatButton)]

 

 







<!--HONumber=Jun16_HO4-->


