---
ms.assetid: C4DB495D-1F91-40EF-A55C-5CABBF3269A2
description: As APIs no namespace Windows.Media.Editing permitem que você desenvolva rapidamente aplicativos que permitem que os usuários criem composições de mídia de arquivos de origem de áudio e vídeo.
title: Composições e edição de mídia
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1e342094509dd5d8fb06657d147ac6468a5f8cd6
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7711315"
---
# <a name="media-compositions-and-editing"></a>Composições e edição de mídia



Este artigo mostra como usar as APIs no namespace [**Windows.Media.Editing**](https://msdn.microsoft.com/library/windows/apps/dn640565) para desenvolver rapidamente aplicativos que permitem que os usuários criem composições de mídia de arquivos de origem de áudio e vídeo. Recursos da estrutura incluem a capacidade acrescentar vários videoclipes juntos, adicionar sobreposições de vídeo e imagem, adicionar áudio em segundo plano e aplicar efeitos de áudio e vídeos de forma programática. Uma vez criadas, composições de mídia podem ser renderizadas em um arquivo de mídia simples para reprodução ou compartilhamento, mas composições também podem ser serializadas para o disco e desserializadas do mesmo, permitindo que o usuário carregue e modifique composições que eles criaram anteriormente. Toda essa funcionalidade é fornecida em uma interface de Windows Runtime fácil de usar que reduz significativamente a quantidade e a complexidade do código necessário para executar essas tarefas quando comparado com a API de nível inferior da [Microsoft Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197) .

## <a name="create-a-new-media-composition"></a>Crie uma nova composição de mídia

A classe [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646) é o contêiner para todos os clipes de mídia presentes na composição e é responsável pela renderização final da composição, carregando e salvando composições no disco e fornecendo um fluxo de visualização da composição para que o usuário possa visualizá-la na interface do usuário. Para usar **MediaComposition** em seu aplicativo, inclua o namespace [**Windows.Media.Editing**](https://msdn.microsoft.com/library/windows/apps/dn640565), bem como o namespace [**Windows.Media.Core**](https://msdn.microsoft.com/library/windows/apps/dn278962) que fornece APIs relacionadas que você precisará.

[!code-cs[Namespace1](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetNamespace1)]


O objeto **MediaComposition** será acessado em vários pontos no seu código, então normalmente você irá declarar uma variável do membro no qual armazená-lo.

[!code-cs[DeclareMediaComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetDeclareMediaComposition)]

O construtor para **MediaComposition** não tem argumentos.

[!code-cs[MediaCompositionConstructor](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetMediaCompositionConstructor)]

## <a name="add-media-clips-to-a-composition"></a>Adicione clipes de mídia a uma composição

Composições de mídia geralmente contêm um ou mais clipes de vídeo. Você pode usar um [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/hh738369) para permitir que o usuário selecione um arquivo de vídeo. Depois que o arquivo foi selecionado, crie um novo objeto [**MediaClip**](https://msdn.microsoft.com/library/windows/apps/dn652596) para conter o videoclipe, chamando [**MediaClip.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652607). Em seguida, você adiciona o clipe à lista **MediaComposition** do objeto [**Clips**](https://msdn.microsoft.com/library/windows/apps/dn652648).

[!code-cs[PickFileAndAddClip](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetPickFileAndAddClip)]

-   Clipes de mídia aparecem no **MediaComposition** na mesma ordem em que aparecem na lista [**Clips**](https://msdn.microsoft.com/library/windows/apps/dn652648).

-   Um **MediaClip** só pode ser incluído em uma composição uma vez. Tentar adicionar um **MediaClip** que já está sendo usado pela composição resultará em erro. Para reutilizar um videoclipe várias vezes em uma composição, chame [**Clone**](https://msdn.microsoft.com/library/windows/apps/dn652599) para criar novos objetos **MediaClip**, que podem então ser adicionados à composição.

-   Aplicativos universais do Windows não têm permissão para acessar o sistema de arquivos inteiro. A propriedade [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) da classe [**StorageApplicationPermissions**](https://msdn.microsoft.com/library/windows/apps/br207456) permite que seu aplicativo armazene um registro de um arquivo que foi selecionado pelo usuário para que você possa manter as permissões para acessar o arquivo. O **FutureAccessList** tem um máximo de 1.000 entradas, assim seu aplicativo precisa gerenciar a lista para garantir que não fique cheia. Isso é especialmente importante se você planeja suportar o carregamento e modificação de composições criadas anteriormente.

-   Uma **MediaComposition** suporta videoclipes em formato MP4.

-   Se um arquivo de vídeo contém várias faixas de áudio incorporadas, você pode selecionar qual faixa de áudio será usada na composição, definindo a propriedade [**SelectedEmbeddedAudioTrackIndex**](https://msdn.microsoft.com/library/windows/apps/dn652627).

-   Crie um **MediaClip** com uma única cor preenchendo todo o quadro, chamando [**CreateFromColor**](https://msdn.microsoft.com/library/windows/apps/dn652605) e especificando uma cor e uma duração para o clipe.

-   Crie um **MediaClip** a partir de um arquivo de imagem, chamando [**CreateFromImageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652610) e especificando um arquivo de imagem e uma duração para o clipe.

-   Crie um **MediaClip** a partir de um [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn965505) chamando [**CreateFromSurface**](https://msdn.microsoft.com/library/dn764774) e especificando uma superfície e uma duração do clipe.

## <a name="preview-the-composition-in-a-mediaelement"></a>Visualize a composição em um MediaElement

Para permitir que o usuário exiba a composição de mídia, adicione um [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) no arquivo XAML que define a interface do usuário.

[!code-xml[MediaElement](./code/MediaEditing/cs/MainPage.xaml#SnippetMediaElement)]

Declare uma variável de membro do tipo [**MediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn282716).


[!code-cs[DeclareMediaStreamSource](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetDeclareMediaStreamSource)]

Chame o método **MediaComposition** do objeto [**GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674) para criar um **MediaStreamSource** para a composição. Crie um objeto [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource) chamando o método de fábrica [**CreateFromMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn930907) e atribua-o à propriedade [**Origem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.Source) do **MediaPlayerElement**. Agora a composição pode ser exibida na interface do usuário.


[!code-cs[UpdateMediaElementSource](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetUpdateMediaElementSource)]

-   O **MediaComposition** deve conter pelo menos um clipe de mídia antes de chamar [**GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674), ou o objeto retornado será nulo.

-   A linha do tempo **MediaElement** não é atualizada automaticamente para refletir as alterações na composição. É recomendável que você chame **GeneratePreviewMediaStreamSource** e defina a propriedade do **MediaPlayerElement**, **Origem**, sempre que fizer um conjunto de alterações na composição e desejar atualizar a interface do usuário.

É recomendável que você defina o objeto **MediaStreamSource** e a propriedade [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) do **MediaPlayerElement** como nula quando o usuário navegar fora da página para liberar os recursos associados.

[!code-cs[OnNavigatedFrom](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

## <a name="render-the-composition-to-a-video-file"></a>Renderize a composição de um arquivo de vídeo

Para renderizar uma composição de mídia para um arquivo de vídeo simples para que possa ser compartilhado e exibido em outros dispositivos, você precisará usar APIs do namespace [**Windows.Media.Transcoding**](https://msdn.microsoft.com/library/windows/apps/br207105). Para atualizar a interface do usuário sobre o progresso da operação assíncrona, você também precisará de APIs do namespace [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383).

[!code-cs[Namespace2](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetNamespace2)]

Depois de permitir que o usuário selecione um arquivo de saída com um [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871), renderize a composição para o arquivo selecionado chamando o objeto **MediaComposition **[**RenderToFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652690). O restante do código no exemplo a seguir simplesmente segue o padrão de manipulação de um [**AsyncOperationWithProgress**](https://msdn.microsoft.com/library/windows/desktop/br205807).

[!code-cs[RenderCompositionToFile](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetRenderCompositionToFile)]

-   O [**MediaTrimmingPreference**](https://msdn.microsoft.com/library/windows/apps/dn640561) permite a você priorizar a velocidade da operação de transcodificação contra a precisão de corte de clipes de mídia adjacentes. **Rápido** faz com que a transcodificação seja mais rápida com os recursos de precisão mais baixos, **Preciso** faz com que a transcodificação seja mais lenta, mas com os recursos mais precisos.

## <a name="trim-a-video-clip"></a>Cortar um videoclipe

Corte a duração de um videoclipe em uma composição, definindo a propriedade [**TrimTimeFromStart**](https://msdn.microsoft.com/library/windows/apps/dn652637) dos objetos [**MediaClip**](https://msdn.microsoft.com/library/windows/apps/dn652596), a propriedade [**TrimTimeFromEnd**](https://msdn.microsoft.com/library/windows/apps/dn652634), ou ambas.

[!code-cs[TrimClipBeforeCurrentPosition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetTrimClipBeforeCurrentPosition)]

-   Você pode usar qualquer interface do usuário que você deseja para permitir que o usuário especifique o início e o término dos valores de corte. O exemplo acima usa a propriedade [**Position**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.Position) do [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession) associada ao **MediaPlayerElement** para determinar primeiro qual **MediaClip** está reproduzindo na posição atual na composição verificando o [**StartTimeInComposition**](https://msdn.microsoft.com/library/windows/apps/dn652629) e [**EndTimeInComposition**](https://msdn.microsoft.com/library/windows/apps/dn652618). Em seguida, as propriedades **Position** e **StartTimeInComposition** são usadas novamente para calcular o tempo de corte desde o início do clipe. o método **FirstOrDefault** é um método de extensão do namespace **System.Linq** que simplifica o código para selecionar itens de uma lista.
-   A propriedade [**OriginalDuration**](https://msdn.microsoft.com/library/windows/apps/dn652625) do objeto **MediaClip** permite que você saiba a duração do clipe de mídia sem qualquer recorte aplicado.
-   A propriedade [**TrimmedDuration**](https://msdn.microsoft.com/library/windows/apps/dn652631) permite saber a duração do clip de mídia depois da aplicação de corte.
-   Especificar um valor de corte maior do que a duração original do clipe não gera um erro. No entanto, se uma composição contém apenas um único clipe e está cortado para tamanho zero, especificando um valor de corte grande, uma chamada subsequente para [**GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674) retornará nulo, como se a composição não tivesse nenhum clipe.

## <a name="add-a-background-audio-track-to-a-composition"></a>Adicione uma faixa de áudio em segundo plano em uma composição

Para adicionar uma faixa em segundo plano para uma composição, carregue um arquivo de áudio e, em seguida, crie um objeto [**BackgroundAudioTrack**](https://msdn.microsoft.com/library/windows/apps/dn652544) chamando o método de fábrica [**BackgroundAudioTrack.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652561). Em seguida, adicione o **BackgroundAudioTrack** à propriedade [**BackgroundAudioTracks**](https://msdn.microsoft.com/library/windows/apps/dn652647) da composição.

[!code-cs[AddBackgroundAudioTrack](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddBackgroundAudioTrack)]

-   Um **MediaComposition** dá suporte a faixas de áudio em segundo plano nos seguintes formatos: MP3, WAV e FLAC

-   Uma faixa de áudio em segundo plano

-   Como com arquivos de vídeo, você deve usar a classe [**StorageApplicationPermissions**](https://msdn.microsoft.com/library/windows/apps/br207456) para preservar o acesso a arquivos na composição.

-   Como com **MediaClip**, um **BackgroundAudioTrack** só pode ser incluído em uma composição uma vez. Tentar adicionar um **BackgroundAudioTrack** que já está sendo usado pela composição resultará em erro. Para reutilizar uma faixa de áudio várias vezes em uma composição, chame [**Clone**](https://msdn.microsoft.com/library/windows/apps/dn652599) para criar novos objetos **MediaClip** que podem então ser adicionados à composição.

-   Por padrão, faixas de áudio começam a reproduzir em segundo plano no início da composição. Se houver várias faixas em segundo plano, todas as faixas começarão a tocar no início da composição. Para fazer com que uma faixa de áudio comece a reproduzir em segundo plano em um outro momento, defina a propriedade [**Delay**](https://msdn.microsoft.com/library/windows/apps/dn652563) para o intervalo de tempo desejado.

## <a name="add-an-overlay-to-a-composition"></a>Adicionar uma sobreposição de uma composição

Sobreposições permitem colocar várias camadas de vídeo umas sobre as outras como uma pilha. Uma composição pode conter várias camadas de sobreposição, cada uma delas pode incluir várias sobreposições. Crie um objeto [**MediaOverlay**](https://msdn.microsoft.com/library/windows/apps/dn764793) passando um **MediaClip** em seu construtor. Defina a posição e a opacidade da sobreposição e, em seguida, crie um novo [**MediaOverlayLayer**](https://msdn.microsoft.com/library/windows/apps/dn764795) e adicione o **MediaOverlay** para sua lista [**Overlays**](https://msdn.microsoft.com/library/windows/desktop/dn280411). Por fim, adicione o **MediaOverlayLayer** à lista [**OverlayLayers**](https://msdn.microsoft.com/library/windows/apps/dn764791) da composição.

[!code-cs[AddOverlay](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddOverlay)]

-   Sobreposições dentro de uma camada são ordenadas de Z a A com base em sua ordem na sua lista **Overlays** contendo a camada. Índices superiores dentro da lista são renderizados em cima de índices inferiores. O mesmo é verdadeiro para camadas de sobreposição dentro de uma composição. Uma camada com índice superior na lista **OverlayLayers** da composição será renderizada sobre índices inferiores.

-   Como sobreposições são empilhadas umas sobre as outras ao invés de serem reproduzidas sequencialmente, todas as sobreposições começarão a se reproduzir no início da composição por padrão. Para fazer com que uma sobreposição seja reproduzida em um outro momento, defina a propriedade [**Delay**](https://msdn.microsoft.com/library/windows/apps/dn764810) para o intervalo de tempo desejado.

## <a name="add-effects-to-a-media-clip"></a>Adicione efeitos a um clipe de mídia

Cada **MediaClip** em uma composição tem uma lista de efeitos de áudio e vídeos ao qual vários efeitos podem ser adicionados. Os efeitos devem implementar [**IAudioEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608044) e [**IVideoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608047) respectivamente. O exemplo a seguir usa a posição atual do **MediaPlayerElement** para escolher o **MediaClip** visualizado no momento e, em seguida, cria uma nova instância do [**VideoStabilizationEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn926762) e acrescenta-a à lista [**VideoEffectDefinitions**](https://msdn.microsoft.com/library/windows/apps/dn652643) do clipe de mídia.

[!code-cs[AddVideoEffect](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddVideoEffect)]

## <a name="save-a-composition-to-a-file"></a>Salvar uma composição em um arquivo

Composições de mídia podem ser serializadas para um arquivo para ser modificada em um momento posterior. Selecione um arquivo de saída e, em seguida, chame o método [**SaveAsync**](https://msdn.microsoft.com/library/windows/apps/dn640554) no [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646) para salvar a composição.

[!code-cs[SaveComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetSaveComposition)]

## <a name="load-a-composition-from-a-file"></a>Carregar uma composição de um arquivo

Composições de mídia podem ser desserializadas de um arquivo para permitir que o usuário exiba e modifique a composição. Selecione um arquivo de composição e, em seguida, chame o método [**LoadAsync**](https://msdn.microsoft.com/library/windows/apps/dn652684) no [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646) para carregar a composição.

[!code-cs[OpenComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetOpenComposition)]

-   Se o arquivo de mídia na composição não estiver em um local que possa ser acessado por seu aplicativo e não estiver na propriedade [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) da classe [**StorageApplicationPermissions**](https://msdn.microsoft.com/library/windows/apps/br207456) para seu aplicativo, será gerado um erro ao carregar a composição.

 

 




