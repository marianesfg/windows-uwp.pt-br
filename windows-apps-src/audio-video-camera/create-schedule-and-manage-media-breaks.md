---
ms.assetid: 0309c7a1-8e4c-4326-813a-cbd9f8b8300d
description: Este artigo mostra como criar, programar e gerenciar pausas de mídia para seu aplicativo de reprodução de mídia.
title: Criar, programar e gerenciar pausas de mídia
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 68260cadd4322d51c528bf6c6af7f5c8472f2199
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361856"
---
# <a name="create-schedule-and-manage-media-breaks"></a>Criar, programar e gerenciar pausas de mídia

Este artigo mostra como criar, programar e gerenciar pausas de mídia para seu aplicativo de reprodução de mídia. Quebras de mídia geralmente são usadas para inserir anúncios de áudio ou vídeos no conteúdo de mídia. A partir do Windows 10, versão 1607, você pode usar a classe [**MediaBreakManager**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaBreakManager) de forma rápida e fácil para adicionar quebras de mídia a qualquer [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem) que você usar com um [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer).


Depois de programar uma ou mais interrupções de mídia, o sistema reproduzirá automaticamente seu conteúdo de mídia no horário especificado durante a reprodução. O **MediaBreakManager** fornece eventos para que seu aplicativo possa reagir quando as pausas de mídia começarem, terminarem ou quando forem ignoradas pelo usuário. Você também pode acessar uma [**MediaPlaybackSession**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackSession) para que suas pausas de mídia monitorem eventos, como download e atualizações do andamento de buffer.

## <a name="schedule-media-breaks"></a>Programar pausas de mídia
Cada objeto **MediaPlaybackItem** tem sua própria [**MediaBreakSchedule**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaBreakSchedule) que você usa para configurar as pausas de mídia que serão executadas quando o item for reproduzido. A primeira etapa para usar pausas de mídia em seu aplicativo é criar um [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem) para o conteúdo de reprodução principal. 

[!code-cs[MoviePlaybackItem](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetMoviePlaybackItem)]

Para obter mais informações sobre como trabalhar com **MediaPlaybackItem**, [**MediaPlaybackList**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList) e outras APIs de reprodução de mídia fundamentais, consulte [Itens de mídia, playlists e faixas](media-playback-with-mediasource.md).

O próximo exemplo mostra como adicionar uma pausa de pré-rolagem ao **MediaPlaybackItem**, o que significa que o sistema reproduzirá a pausa de mídia antes de executar o item de reprodução ao qual pertence a pausa. Primeiro, um novo objeto [**MediaBreak**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaBreak) é instanciado. Neste exemplo, o construtor é chamado com [**MediaBreakInsertionMethod.Interrupt**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaBreakInsertionMethod), o que significa que o conteúdo principal será pausado enquanto o conteúdo da pausa for executado. 

Em seguida, um novo **MediaPlaybackItem** é criado para o conteúdo que será executado durante a pausa, como um anúncio. A propriedade [**CanSkip**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.canskip) desse item de reprodução é definida como false. Isso significa que o usuário não poderá ignorar o item usando os controles de mídia integrada. Seu aplicativo ainda pode optar por ignorar a inclusão de maneira programática chamando [**SkipCurrentBreak**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakmanager.skipcurrentbreak). 

A propriedade [**PlaybackList**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreak.playbacklist) da pausa de mídia é uma [**MediaPlaybackList**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList) que permite reproduzir vários itens de mídia como uma playlist. Adicione um ou mais objetos **MediaPlaybackItem** da coleção de **itens** da lista para incluí-los na playlist da pausa de mídia.

Por fim, programe a pausa de mídia usando a propriedade [**BreakSchedule**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.breakschedule) do item de reprodução de conteúdo principal. Especifique a pausa como uma pausa de pré-rolagem atribuindo-a à propriedade [**PrerollBreak**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakschedule.prerollbreak) do objeto da programação.

[!code-cs[PreRollBreak](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetPreRollBreak)]

Agora você pode reproduzir o item de mídia principal, e a pausa de mídia que você criou será reproduzida antes do conteúdo principal. Crie um novo objeto [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) e, opcionalmente, defina a propriedade [**AutoPlay**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.autoplay) como true para iniciar a reprodução automaticamente. Defina a propriedade [**Source**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.source) do **MediaPlayer** como seu item de reprodução de conteúdo principal. Não é obrigatório, mas você pode atribuir o **MediaPlayer** a um [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) para renderizar a mídia em uma página XAML. Para obter mais informações sobre como usar **MediaPlayer**, consulte [Reproduzir áudio e vídeo com MediaPlayer](play-audio-and-video-with-mediaplayer.md).

[!code-cs[Play](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetPlay)]

Adicione uma pausa de pós-rolagem que será reproduzida após o término da reprodução do **MediaPlaybackItem** que apresenta o conteúdo principal, usando a mesma técnica que uma pausa de pré-rolagem, exceto que o objeto **MediaBreak** é atribuído à propriedade [**PostrollBreak**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakschedule.postrollbreak).

[!code-cs[PostRollBreak](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetPostRollBreak)]

Você também pode programar uma ou mais pausas de meio de rolagem que são executadas em um horário especificado na reprodução do conteúdo principal. No exemplo a seguir, a [**MediaBreak**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaBreak) é criada com a sobrecarga do construtor que aceita um objeto **TimeSpan**, o que especifica o horário na reprodução do item de mídia principal quando a pausa será reproduzida. Novamente, [**MediaBreakInsertionMethod.Interrupt**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaBreakInsertionMethod) é especificado para indicar que a reprodução do conteúdo principal será pausada durante a reprodução da pausa. A pausa de meio de rolagem é adicionada à programação chamando [**InsertMidrollBreak**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakschedule.insertmidrollbreak). Você pode obter uma lista de somente leitura das pausas de meio de rolagem atuais na programação acessando a propriedade [**MidrollBreaks**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakschedule.midrollbreaks).

[!code-cs[MidrollBreak](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetMidrollBreak)]

O próximo exemplo de pausa de meio de rolagem mostrado usa o método de inserção [**MediaBreakInsertionMethod.Replace**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaBreakInsertionMethod), o que significa que o sistema continuará a processar o conteúdo principal durante a reprodução da pausa. Essa opção é normalmente usada por aplicativos de mídia de streaming ao vivo, quando você não deseja que o conteúdo seja pausado e fique atrás do fluxo ao vivo enquanto o anúncio é reproduzido. 

Este exemplo também utiliza uma sobrecarga do construtor [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem) que aceita dois parâmetros [**TimeSpan**](https://docs.microsoft.com/uwp/api/Windows.Foundation.TimeSpan). O primeiro parâmetro especifica o ponto de partida no item de pausa de mídia em que a reprodução será iniciada. O segundo parâmetro especifica a duração em que a pausa de mídia será reproduzida. Portanto, no exemplo a seguir, a **MediaBreak** começará aos 20 minutos do conteúdo principal. Durante sua reprodução, o item de mídia começará 30 segundos após o início do item de pausa de mídia e será reproduzido por 15 segundos antes de o conteúdo de mídia principal retomar a reprodução.

[!code-cs[MidrollBreak2](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetMidrollBreak2)]

## <a name="skip-media-breaks"></a>Ignorar pausas de mídia
Como mencionado anteriormente neste artigo, a propriedade [**CanSkip**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.canskip) de um **MediaPlaybackItem** podem ser definido para impedir que o usuário ignore o conteúdo com os controles internos. No entanto, você pode chamar [**SkipCurrentBreak**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakmanager.skipcurrentbreak) do seu código a qualquer momento para ignorar a pausa atual.

[!code-cs[SkipButtonClick](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetSkipButtonClick)]

## <a name="handle-mediabreak-events"></a>Tratar eventos de MediaBreak

Há vários eventos relacionados a pausas de mídia que você pode assinar para agir com base na alteração das estado das pausas.

[!code-cs[RegisterMediaBreakEvents](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetRegisterMediaBreakEvents)]

O [**BreakStarted**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakmanager.breakstarted) é acionado quando uma pausa de mídia é iniciada. É possível atualizar sua interface do usuário para informar o usuário que o conteúdo de pausa de mídia será reproduzido. Este exemplo usa o [**MediaBreakStartedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaBreakStartedEventArgs) transmitido para o manipulador para obter uma referência da pausa de mídia iniciada. A propriedade [**CurrentItemIndex**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacklist.currentitemindex) é usada para determinar qual item de mídia na playlist da pausa está sendo reproduzido. Em seguida, a interface do usuário é atualizada para mostrar ao usuário o índice de anúncios atual e o número de anúncios restantes na pausa. Lembre-se de que as atualizações da interface do usuário devem ser feitas no thread da interface do usuário, portanto, a chamada deve ser feita dentro de uma chamada para [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.windows). 

[!code-cs[BreakStarted](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBreakStarted)]

[**BreakEnded** ](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakmanager.breakended) é gerado quando todos os itens de mídia em que a interrupção tem terminado sua execução ou foram ignorados pela. Você pode usar o manipulador desse evento para atualizar a interface do usuário a fim de indicar que o conteúdo da quebra de mídia não está mais sendo reproduzido.

[!code-cs[BreakEnded](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBreakEnded)]

O evento **BreakSkipped** é acionado quando o usuário pressiona o botão *Avançar* na interface do usuário interna durante a reprodução de um item para o qual [**CanSkip**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.canskip) é true, ou quando você ignora uma pausa em seu código chamando [**SkipCurrentBreak**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakmanager.skipcurrentbreak).

O exemplo a seguir usa a propriedade [**Source**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.source) do **MediaPlayer** para obter uma referência do item de mídia para o conteúdo principal. A pausa de mídia ignorada pertence à programação de pausa desse item. Em seguida, o código verifica se a pausa de mídia que foi ignorada é a mesma que a pausa de mídia definida como a propriedade [**PrerollBreak**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakschedule.prerollbreak) da programação. Em caso afirmativo, isso significa que a pausa de pré-rolagem foi a pausa ignorada e, nesse caso, uma nova pausa de meio de rolagem é criada e programada para ser reproduzida 10 minutos no conteúdo principal.

[!code-cs[BreakSkipped](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBreakSkipped)]

[**BreaksSeekedOver** ](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakmanager.breaksseekedover) é gerado quando a posição de reprodução do item de mídia principal passa sobre o horário agendado para um ou mais intervalos de mídia. O exemplo a seguir verifica se mais de uma pausa de mídia foi buscada, se a posição de reprodução foi avançada e se ela foi avançada menos de 10 minutos. Se sim, a primeira pausa buscada, obtida da coleção [**SeekedOverBreaks**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakseekedovereventargs.seekedoverbreaks) exposta pelos argumentos do evento será executada imediatamente com uma chamada para o método [**PlayBreak**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakmanager.playbreak) do **MediaPlayer.BreakManager**.

[!code-cs[BreakSeekedOver](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBreakSeekedOver)]


## <a name="access-the-current-playback-session"></a>Acessar a sessão de reprodução atual
O objeto [**MediaPlaybackSession**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackSession) usa a classe **MediaPlayer** para fornecer dados e eventos relacionados ao conteúdo de mídia que está em execução no momento. O [**MediaBreakManager**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaBreakManager) também tem uma **MediaPlaybackSession** que você pode acessar para obter dados e eventos especificamente relacionados ao conteúdo da pausa de mídia que está em reprodução. As informações que você pode obter da sessão de reprodução incluem o estado atual da reprodução, em reprodução ou pausado, e a posição de reprodução atual no conteúdo. Você pode usar as propriedades [**NaturalVideoWidth**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.naturalvideowidth) e [**NaturalVideoHeight**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.naturalvideoheight) e o [**NaturalVideoSizeChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.naturalvideosizechanged) para ajustar sua interface do usuário de vídeo se o conteúdo da pausa de mídia tiver uma taxa de proporção diferente de seu conteúdo principal. Você também pode receber eventos como [**BufferingStarted**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.bufferingstarted), [**BufferingEnded**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.bufferingended) e [**DownloadProgressChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.downloadprogresschanged) que podem fornecer dados de telemetria valiosos sobre o desempenho do seu aplicativo.

O exemplo a seguir registra um manipulador para o **evento BufferingProgressChanged**; no manipulador de eventos, ele atualiza a interface do usuário para mostrar o andamento atual do buffer.

[!code-cs[RegisterBufferingProgressChanged](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetRegisterBufferingProgressChanged)]

[!code-cs[BufferingProgressChanged](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBufferingProgressChanged)]

## <a name="related-topics"></a>Tópicos relacionados
* [Reprodução de mídia](media-playback.md)
* [Reproduzir áudio e vídeo com o Media Player](play-audio-and-video-with-mediaplayer.md)
* [Controle manual, o sistema de transporte de controles de mídia](system-media-transport-controls.md)

 

 




