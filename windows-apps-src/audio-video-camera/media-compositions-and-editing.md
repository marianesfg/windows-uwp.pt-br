---
ms.assetid: C4DB495D-1F91-40EF-A55C-5CABBF3269A2
description: As APIs no namespace Windows.Media.Editing permitem que você desenvolva rapidamente aplicativos que permitem que os usuários criem composições de mídia de arquivos de origem de áudio e vídeo.
title: Composições e edição de mídia
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4054f1f4ce4db7f158c1297b748ecea8cab83602
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360738"
---
# <a name="media-compositions-and-editing"></a>Composições e edição de mídia



Este artigo mostra como usar as APIs no namespace [**Windows.Media.Editing**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing) para desenvolver rapidamente aplicativos que permitem que os usuários criem composições de mídia de arquivos de origem de áudio e vídeo. Recursos da estrutura incluem a capacidade acrescentar vários videoclipes juntos, adicionar sobreposições de vídeo e imagem, adicionar áudio em segundo plano e aplicar efeitos de áudio e vídeos de forma programática. Uma vez criadas, composições de mídia podem ser renderizadas em um arquivo de mídia simples para reprodução ou compartilhamento, mas composições também podem ser serializadas para o disco e desserializadas do mesmo, permitindo que o usuário carregue e modifique composições que eles criaram anteriormente. Toda essa funcionalidade é fornecida em uma interface de Windows Runtime fácil de usar que reduz significativamente a quantidade e a complexidade do código necessário para executar essas tarefas quando comparado com a API de nível inferior da [Microsoft Media Foundation](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk) .

## <a name="create-a-new-media-composition"></a>Crie uma nova composição de mídia

A classe [**MediaComposition**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaComposition) é o contêiner para todos os clipes de mídia presentes na composição e é responsável pela renderização final da composição, carregando e salvando composições no disco e fornecendo um fluxo de visualização da composição para que o usuário possa visualizá-la na interface do usuário. Para usar **MediaComposition** em seu aplicativo, inclua o namespace [**Windows.Media.Editing**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing), bem como o namespace [**Windows.Media.Core**](https://docs.microsoft.com/uwp/api/Windows.Media.Core) que fornece APIs relacionadas que você precisará.

[!code-cs[Namespace1](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetNamespace1)]


O objeto **MediaComposition** será acessado em vários pontos no seu código, então normalmente você irá declarar uma variável do membro no qual armazená-lo.

[!code-cs[DeclareMediaComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetDeclareMediaComposition)]

O construtor para **MediaComposition** não tem argumentos.

[!code-cs[MediaCompositionConstructor](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetMediaCompositionConstructor)]

## <a name="add-media-clips-to-a-composition"></a>Adicione clipes de mídia a uma composição

Composições de mídia geralmente contêm um ou mais clipes de vídeo. Você pode usar um [**FileOpenPicker**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-fileopenpicker) para permitir que o usuário selecione um arquivo de vídeo. Depois que o arquivo foi selecionado, crie um novo objeto [**MediaClip**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaClip) para conter o videoclipe, chamando [**MediaClip.CreateFromFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.createfromfileasync). Em seguida, você adiciona o clipe à lista **MediaComposition** do objeto [**Clips**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.clips).

[!code-cs[PickFileAndAddClip](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetPickFileAndAddClip)]

-   Clipes de mídia aparecem no **MediaComposition** na mesma ordem em que aparecem na lista [**Clips**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.clips).

-   Um **MediaClip** só pode ser incluído em uma composição uma vez. Tentar adicionar um **MediaClip** que já está sendo usado pela composição resultará em erro. Para reutilizar um videoclipe várias vezes em uma composição, chame [**Clone**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.clone) para criar novos objetos **MediaClip**, que podem então ser adicionados à composição.

-   Aplicativos universais do Windows não têm permissão para acessar o sistema de arquivos inteiro. A propriedade [**FutureAccessList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) da classe [**StorageApplicationPermissions**](https://docs.microsoft.com/uwp/api/Windows.Storage.AccessCache.StorageApplicationPermissions) permite que seu aplicativo armazene um registro de um arquivo que foi selecionado pelo usuário para que você possa manter as permissões para acessar o arquivo. O **FutureAccessList** tem um máximo de 1.000 entradas, assim seu aplicativo precisa gerenciar a lista para garantir que não fique cheia. Isso é especialmente importante se você planeja suportar o carregamento e modificação de composições criadas anteriormente.

-   Uma **MediaComposition** suporta videoclipes em formato MP4.

-   Se um arquivo de vídeo contém várias faixas de áudio incorporadas, você pode selecionar qual faixa de áudio será usada na composição, definindo a propriedade [**SelectedEmbeddedAudioTrackIndex**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.selectedembeddedaudiotrackindex).

-   Crie um **MediaClip** com uma única cor preenchendo todo o quadro, chamando [**CreateFromColor**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.createfromcolor) e especificando uma cor e uma duração para o clipe.

-   Crie um **MediaClip** a partir de um arquivo de imagem, chamando [**CreateFromImageFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.createfromimagefileasync) e especificando um arquivo de imagem e uma duração para o clipe.

-   Crie um **MediaClip** a partir de um [**IDirect3DSurface**](https://docs.microsoft.com/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) chamando [**CreateFromSurface**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.createfromsurface) e especificando uma superfície e uma duração do clipe.

## <a name="preview-the-composition-in-a-mediaelement"></a>Visualize a composição em um MediaElement

Para permitir que o usuário exiba a composição de mídia, adicione um [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) no arquivo XAML que define a interface do usuário.

[!code-xml[MediaElement](./code/MediaEditing/cs/MainPage.xaml#SnippetMediaElement)]

Declare uma variável de membro do tipo [**MediaStreamSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaStreamSource).


[!code-cs[DeclareMediaStreamSource](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetDeclareMediaStreamSource)]

Chame o método **MediaComposition** do objeto [**GeneratePreviewMediaStreamSource**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.generatepreviewmediastreamsource) para criar um **MediaStreamSource** para a composição. Crie um objeto [**MediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource) chamando o método de fábrica [**CreateFromMediaStreamSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediastreamsource) e atribua-o à propriedade [**Origem**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) do **MediaPlayerElement**. Agora a composição pode ser exibida na interface do usuário.


[!code-cs[UpdateMediaElementSource](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetUpdateMediaElementSource)]

-   O **MediaComposition** deve conter pelo menos um clipe de mídia antes de chamar [**GeneratePreviewMediaStreamSource**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.generatepreviewmediastreamsource), ou o objeto retornado será nulo.

-   A linha do tempo **MediaElement** não é atualizada automaticamente para refletir as alterações na composição. É recomendável que você chame ambos **GeneratePreviewMediaStreamSource** e defina as **MediaPlayerElement** **origem** propriedade sempre que você fizer um conjunto de alterações para o composição e quiser atualizar a interface do usuário.

É recomendável que você defina o objeto **MediaStreamSource** e a propriedade [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaelement.source) do **MediaPlayerElement** como nula quando o usuário navegar fora da página para liberar os recursos associados.

[!code-cs[OnNavigatedFrom](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

## <a name="render-the-composition-to-a-video-file"></a>Renderize a composição de um arquivo de vídeo

Para renderizar uma composição de mídia para um arquivo de vídeo simples para que possa ser compartilhado e exibido em outros dispositivos, você precisará usar APIs do namespace [**Windows.Media.Transcoding**](https://docs.microsoft.com/uwp/api/Windows.Media.Transcoding). Para atualizar a interface do usuário sobre o progresso da operação assíncrona, você também precisará de APIs do namespace [**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core).

[!code-cs[Namespace2](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetNamespace2)]

Depois de permitir que o usuário selecione um arquivo de saída com um [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker), renderize a composição para o arquivo selecionado chamando o objeto **MediaComposition** [**RenderToFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.rendertofileasync). O restante do código no exemplo a seguir simplesmente segue o padrão de manipulação de um [**AsyncOperationWithProgress**](https://docs.microsoft.com/previous-versions//br205807(v=vs.85)).

[!code-cs[RenderCompositionToFile](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetRenderCompositionToFile)]

-   O [**MediaTrimmingPreference**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaTrimmingPreference) permite a você priorizar a velocidade da operação de transcodificação contra a precisão de corte de clipes de mídia adjacentes. **Rápido** faz com que a transcodificação seja mais rápida com os recursos de precisão mais baixos, **Preciso** faz com que a transcodificação seja mais lenta, mas com os recursos mais precisos.

## <a name="trim-a-video-clip"></a>Cortar um videoclipe

Corte a duração de um videoclipe em uma composição, definindo a propriedade [**TrimTimeFromStart**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.trimtimefromstart) dos objetos [**MediaClip**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaClip), a propriedade [**TrimTimeFromEnd**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.trimtimefromend), ou ambas.

[!code-cs[TrimClipBeforeCurrentPosition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetTrimClipBeforeCurrentPosition)]

-   Você pode usar qualquer interface do usuário que você deseja para permitir que o usuário especifique o início e o término dos valores de corte. O exemplo acima usa a propriedade [**Position**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.position) do [**MediaPlaybackSession**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackSession) associada ao **MediaPlayerElement** para determinar primeiro qual **MediaClip** está reproduzindo na posição atual na composição verificando o [**StartTimeInComposition**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.starttimeincomposition) e [**EndTimeInComposition**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.endtimeincomposition). Em seguida, as propriedades **Position** e **StartTimeInComposition** são usadas novamente para calcular o tempo de corte desde o início do clipe. o método **FirstOrDefault** é um método de extensão do namespace **System.Linq** que simplifica o código para selecionar itens de uma lista.
-   A propriedade [**OriginalDuration**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.originalduration) do objeto **MediaClip** permite que você saiba a duração do clipe de mídia sem qualquer recorte aplicado.
-   A propriedade [**TrimmedDuration**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.trimmedduration) permite saber a duração do clip de mídia depois da aplicação de corte.
-   Especificar um valor de corte maior do que a duração original do clipe não gera um erro. No entanto, se uma composição contém apenas um único clipe e está cortado para tamanho zero, especificando um valor de corte grande, uma chamada subsequente para [**GeneratePreviewMediaStreamSource**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.generatepreviewmediastreamsource) retornará nulo, como se a composição não tivesse nenhum clipe.

## <a name="add-a-background-audio-track-to-a-composition"></a>Adicione uma faixa de áudio em segundo plano em uma composição

Para adicionar uma faixa em segundo plano para uma composição, carregue um arquivo de áudio e, em seguida, crie um objeto [**BackgroundAudioTrack**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.BackgroundAudioTrack) chamando o método de fábrica [**BackgroundAudioTrack.CreateFromFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.backgroundaudiotrack.createfromfileasync). Em seguida, adicione o **BackgroundAudioTrack** à propriedade [**BackgroundAudioTracks**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.backgroundaudiotracks) da composição.

[!code-cs[AddBackgroundAudioTrack](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddBackgroundAudioTrack)]

-   Um **MediaComposition** dá suporte a faixas de áudio nos seguintes formatos de plano de fundo: MP3, WAV, FLAC

-   Uma faixa de áudio em segundo plano

-   Como com arquivos de vídeo, você deve usar a classe [**StorageApplicationPermissions**](https://docs.microsoft.com/uwp/api/Windows.Storage.AccessCache.StorageApplicationPermissions) para preservar o acesso a arquivos na composição.

-   Como com **MediaClip**, um **BackgroundAudioTrack** só pode ser incluído em uma composição uma vez. Tentar adicionar um **BackgroundAudioTrack** que já está sendo usado pela composição resultará em erro. Para reutilizar uma faixa de áudio várias vezes em uma composição, chame [**Clone**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.clone) para criar novos objetos **MediaClip** que podem então ser adicionados à composição.

-   Por padrão, faixas de áudio começam a reproduzir em segundo plano no início da composição. Se houver várias faixas em segundo plano, todas as faixas começarão a tocar no início da composição. Para fazer com que uma faixa de áudio comece a reproduzir em segundo plano em um outro momento, defina a propriedade [**Delay**](https://docs.microsoft.com/uwp/api/windows.media.editing.backgroundaudiotrack.delay) para o intervalo de tempo desejado.

## <a name="add-an-overlay-to-a-composition"></a>Adicionar uma sobreposição de uma composição

Sobreposições permitem colocar várias camadas de vídeo umas sobre as outras como uma pilha. Uma composição pode conter várias camadas de sobreposição, cada uma delas pode incluir várias sobreposições. Crie um objeto [**MediaOverlay**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaOverlay) passando um **MediaClip** em seu construtor. Defina a posição e a opacidade da sobreposição e, em seguida, crie um novo [**MediaOverlayLayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaOverlayLayer) e adicione o **MediaOverlay** para sua lista [**Overlays**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgioutput2-supportsoverlays). Por fim, adicione o **MediaOverlayLayer** à lista [**OverlayLayers**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.overlaylayers) da composição.

[!code-cs[AddOverlay](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddOverlay)]

-   Sobreposições dentro de uma camada são ordenadas de Z a A com base em sua ordem na sua lista **Overlays** contendo a camada. Índices superiores dentro da lista são renderizados em cima de índices inferiores. O mesmo é verdadeiro para camadas de sobreposição dentro de uma composição. Uma camada com índice superior na lista **OverlayLayers** da composição será renderizada sobre índices inferiores.

-   Como sobreposições são empilhadas umas sobre as outras ao invés de serem reproduzidas sequencialmente, todas as sobreposições começarão a se reproduzir no início da composição por padrão. Para fazer com que uma sobreposição seja reproduzida em um outro momento, defina a propriedade [**Delay**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaoverlay.delay) para o intervalo de tempo desejado.

## <a name="add-effects-to-a-media-clip"></a>Adicione efeitos a um clipe de mídia

Cada **MediaClip** em uma composição tem uma lista de efeitos de áudio e vídeos ao qual vários efeitos podem ser adicionados. Os efeitos devem implementar [**IAudioEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IAudioEffectDefinition) e [**IVideoEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IVideoEffectDefinition) respectivamente. O exemplo a seguir usa a posição atual do **MediaPlayerElement** para escolher o **MediaClip** visualizado no momento e, em seguida, cria uma nova instância do [**VideoStabilizationEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.VideoStabilizationEffectDefinition) e acrescenta-a à lista [**VideoEffectDefinitions**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.videoeffectdefinitions) do clipe de mídia.

[!code-cs[AddVideoEffect](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddVideoEffect)]

## <a name="save-a-composition-to-a-file"></a>Salvar uma composição em um arquivo

Composições de mídia podem ser serializadas para um arquivo para ser modificada em um momento posterior. Selecione um arquivo de saída e, em seguida, chame o método [**SaveAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.saveasync) no [**MediaComposition**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaComposition) para salvar a composição.

[!code-cs[SaveComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetSaveComposition)]

## <a name="load-a-composition-from-a-file"></a>Carregar uma composição de um arquivo

Composições de mídia podem ser desserializadas de um arquivo para permitir que o usuário exiba e modifique a composição. Selecione um arquivo de composição e, em seguida, chame o método [**LoadAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.loadasync) no [**MediaComposition**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaComposition) para carregar a composição.

[!code-cs[OpenComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetOpenComposition)]

-   Se o arquivo de mídia na composição não estiver em um local que possa ser acessado por seu aplicativo e não estiver na propriedade [**FutureAccessList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) da classe [**StorageApplicationPermissions**](https://docs.microsoft.com/uwp/api/Windows.Storage.AccessCache.StorageApplicationPermissions) para seu aplicativo, será gerado um erro ao carregar a composição.

 

 




