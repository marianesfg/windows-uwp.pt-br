---
author: drewbatgit
ms.assetid: 58af5e9d-37a1-4f42-909c-db7cb02a0d12
description: "Este artigo mostra como reproduzir mídia em seu aplicativo Universal do Windows com o MediaPlayer."
title: "Reproduzir áudio e vídeo com o MediaPlayer"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 79eabb1341d9a14ec924a1933917e92af797b65a
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="play-audio-and-video-with-mediaplayer"></a>Reproduzir áudio e vídeo com o MediaPlayer

Este artigo mostra como reproduzir mídia em seu aplicativo Universal do Windows usando a classe [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer). Com o Windows 10, versão 1607, melhorias significativas foram feitas nas APIs de reprodução de mídia, incluindo um design de processo único simplificado para áudio em segundo plano, integração automática com os Controles de Transporte de Mídia do Sistema (SMTC), a capacidade de sincronizar vários media players, o recurso de uma superfície Windows.UI.Composition e uma interface simples para criar e programar pausas de mídia em seu conteúdo. Para tirar proveito dessas melhorias, a prática recomendada para a reprodução de mídia é usar a classe **MediaPlayer** em vez de **MediaElement**. O [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement), um controle XAML leve, foi introduzido para permitir que você renderize conteúdo de mídia em uma página XAML. Muitas das APIs de status e controle de reprodução fornecidas pelo **MediaElement** agora estão disponíveis por meio do novo objeto [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession). O **MediaElement** continua a funcionar a fim de dar suporte à compatibilidade com versões anteriores, mas nenhum outro recurso será adicionado a essa classe.

Este artigo fornecerá orientações sobre os recursos do **MediaPlayer** que serão usados por um aplicativo típico de reprodução de mídia. Observe que o **MediaPlayer** usa a classe [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource) como um contêiner para todos os itens de mídia. Essa classe permite carregar e reproduzir mídia de várias origens diferentes, incluindo arquivos locais, fluxos de memória e origens de rede, todos usando a mesma interface. Também há classes de nível superior que funcionam com o **MediaSource**, como [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) e [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList), que oferecem recursos mais avançados, como playlists e a capacidade de gerenciar origens de mídia com várias faixas de áudio, vídeo e metadados. Para obter mais informações sobre **MediaSource** e APIs relacionadas, consulte [Itens de mídia, playlists e faixas](media-playback-with-mediasource.md).


##<a name="play-a-media-file-with-mediaplayer"></a>Reproduzir um arquivo de mídia com o MediaPlayer  
A reprodução básica de mídia com o **MediaPlayer** é bastante simples de implementar. Primeiro, crie uma nova instância da classe **MediaPlayer**. Seu aplicativo pode ter várias instâncias ativas do **MediaPlayer** simultaneamente. Em seguida, defina a propriedade [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) do player como um objeto que implemente a [ **IMediaPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.IMediaPlaybackSource), como uma [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource), um [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) ou um [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList). Neste exemplo, um **MediaSource** é criado a partir de um arquivo contido no armazenamento local do aplicativo e, em seguida, um **MediaPlaybackItem** é criado a partir da origem e é atribuído à propriedade **Source** do player.

Diferentemente do **MediaElement**, o **MediaPlayer** não inicia a reprodução automaticamente por padrão. Para iniciar a reprodução, você pode chamar [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play), definir a propriedade [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AutoPlay) como true ou aguardar que o usuário inicie a reprodução com os controles de mídia integrados.

[!code-cs[SimpleFilePlayback](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSimpleFilePlayback)]

Quando seu aplicativo terminar de usar o **MediaPlayer**, você deverá chamar o método [**Close**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Close) (projetado como **Dispose** em C#) para limpar os recursos usados pelo player.

[!code-cs[CloseMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCloseMediaPlayer)]

##<a name="use-mediaplayerelement-to-render-video-in-xaml"></a>Usar o MediaPlayerElement para renderizar vídeo em XAML
É possível reproduzir mídia em um **MediaPlayer** sem exibi-la em XAML, mas em muitos aplicativos de reprodução de mídia, você precisará renderizar a mídia em uma página XAML. Para fazer isso, use o controle leve [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement). Como o **MediaElement**, o **MediaPlayerElement** permite especificar se os controles de transporte integrados deverão ou não ser mostrados.

[!code-xml[MediaPlayerElementXAML](./code/MediaPlayer_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

Você pode definir a instância do**MediaPlayer** à qual o elemento está vinculado chamando [**SetMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/mt708764).

[!code-cs[SetMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetMediaPlayer)]

Você também pode definir a origem de reprodução no **MediaPlayerElement**, e o elemento criará automaticamente uma nova instância do **MediaPlayer** que poderá ser acessada com a propriedade [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.MediaPlayer).

[!code-cs[GetPlayerFromElement](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetGetPlayerFromElement)]

> [!NOTE] 
> Se você desabilitar o [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) do [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) definindo [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) como false, isso romperá o vínculo entre o **MediaPlayer** e o [**TransportControls**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.TransportControls) fornecido pelo **MediaPlayerElement**, portanto os controles de transporte internos não vão mais controlar automaticamente a reprodução do player. Em vez disso, você deve implementar seus próprios controles para regular o **MediaPlayer**.

##<a name="common-mediaplayer-tasks"></a>Tarefas comuns do MediaPlayer
Esta seção mostra como usar alguns recursos do **MediaPlayer**.

###<a name="set-the-audio-category"></a>Definir a categoria de áudio
Defina a propriedade [**AudioCategory**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AudioCategory) de um **MediaPlayer** como um dos valores da enumeração [**MediaPlayerAudioCategory**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerAudioCategory) para informar o sistema qual tipo de mídia você está executando. Jogos devem categorizar seus fluxos de música como **GameMedia** para que o mudo do jogo seja ativado automaticamente se outro aplicativo reproduzir música em segundo plano. Os aplicativos de música ou vídeo devem categorizar seus fluxos como **Media** ou **Movie** para que eles tenham prioridade sobre fluxos **GameMedia**.

[!code-cs[SetAudioCategory](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioCategory)]

###<a name="output-to-a-specific-audio-endpoint"></a>Saída para um ponto de extremidade de áudio específico
Por padrão, a saída de áudio de um **MediaPlayer** é roteada para o ponto de extremidade de áudio padrão do sistema, mas você pode especificar um ponto de extremidade de áudio específico que o **MediaPlayer** deverá usar para a saída. No exemplo a seguir, [**MediaDevice.GetAudioRenderSelector**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.MediaDevice.GetAudioRenderSelector) retorna uma cadeia de caracteres que identifica, de forma exclusiva, a categoria de renderização de áudio dos dispositivos. Em seguida, o método [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation) [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation.FindAllAsync) é chamado para obter uma lista de todos os dispositivos disponíveis do tipo selecionado. Você pode determinar programaticamente qual dispositivo deseja usar ou adicionar os dispositivos retornados a uma [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ComboBox) para permitir que o usuário selecione um dispositivo.

[!code-cs[SetAudioEndpointEnumerate](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpointEnumerate)]

No evento [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.Selector.SelectionChanged) para a caixa de combinação de dispositivos, a propriedade [**AudioDevice**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AudioDevice) do **MediaPlayer** está definida como o dispositivo selecionado, que foi armazenado na propriedade [**Tag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FrameworkElement.Tag) do **ComboBoxItem**.

[!code-cs[SetAudioEndpontSelectionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpontSelectionChanged)]

###<a name="playback-session"></a>Sessão de reprodução
Conforme descrito anteriormente neste artigo, muitas das funções que são expostas pela classe **MediaElement** foram transferidas para a classe [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession). Isso inclui informações sobre o estado de reprodução do player, como a posição de reprodução atual, se o player está pausado ou em reprodução e a velocidade de reprodução atual. O **MediaPlaybackSession** também fornece vários eventos para avisá-lo quando o estado é alterado, inclusive o status de transferência e o buffer atual do conteúdo que está sendo reproduzido, bem como o tamanho natural e a taxa de proporção do conteúdo de vídeo em reprodução no momento.

O exemplo a seguir mostra como implementar um manipulador de clique de botão que avança 10 segundos no conteúdo. Primeiro, o objeto**MediaPlaybackSession** do player é recuperado com a propriedade [**PlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.PlaybackSession). Em seguida, a propriedade [**Position**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.Position) é definida como a posição de reprodução atual mais 10 segundos.

[!code-cs[SkipForwardClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSkipForwardClick)]

O próximo exemplo ilustra o uso de um botão de alternância para alternar entre a velocidade de reprodução normal e 2 X a velocidade definindo a propriedade [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.PlaybackRate) da sessão.

[!code-cs[SpeedChecked](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSpeedChecked)]

###<a name="pinch-and-zoom-video"></a>Pinçar e aplicar zoom em vídeo
O **MediaPlayer** permite especificar o retângulo de origem no conteúdo de vídeo que deve ser renderizado, permitindo aplicar zoom ao vídeo de forma efetiva. O retângulo especificado é relativo a um retângulo normalizado (0,0,1,1), onde 0,0 é a parte superior esquerda do quadro, e 1,1 especifica a largura e a altura totais do quadro. Assim, por exemplo, para definir o retângulo de zoom para que o quadrante superior direito do vídeo seja renderizado, você especificaria o retângulo (.5,0,.5,.5).  É importante verificar seus valores para garantir que o retângulo de origem esteja dentro do retângulo normalizado (0,0,1,1). A tentativa de definir um valor fora desse intervalo fará com que uma exceção seja lançada.

Para implementar o recurso de pinçar e aplicar zoom usando gestos multitoque, primeiro é necessário especificar os gestos aos quais deseja dar suporte. Neste exemplo, é necessário dimensionar e fazer a translação dos gestos. O evento [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.ManipulationDelta) é acionado quando é feito um dos gestos inscritos. O evento [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DoubleTapped) será usado para redefinir o zoom para o quadro completo. 

[!code-cs[RegisterPinchZoomEvents](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPinchZoomEvents)]

Em seguida, declare um objeto **Rect** que armazenará o retângulo de origem de zoom atual.

[!code-cs[DeclareSourceRect](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareSourceRect)]

O manipulador **ManipulationDelta** ajusta a escala ou a translação do retângulo de zoom. Se o valor de escala delta não for 1, isso significa que o usuário realizou um gesto de pinçar. Se o valor for maior que 1, o retângulo de origem deverá ser menor para ampliar o conteúdo. Se o valor for menor que 1, o retângulo de origem deverá ser maior para diminuir o zoom. Antes de configurar os novos valores de escala, o retângulo resultante é verificado para garantir que ele esteja inteiramente dentro dos limites (0,0,1,1).

Se o valor de escala for 1, o gesto de translação será manipulado. O retângulo é simplesmente convertido pelo número de pixels contidos no gesto, dividido pela largura e a altura do controle. Novamente, o retângulo resultante é verificado para garantir que ele fique dentro dos limites (0,0,1,1).

Por fim, o [**NormalizedSourceRect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.NormalizedSourceRect) do **MediaPlaybackSession** é definido como o retângulo recém-ajustado, especificando a área dentro do quadro de vídeo que deve ser renderizado.

[!code-cs[ManipulationDelta](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetManipulationDelta)]

No manipulador de eventos [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DoubleTapped), o retângulo de origem é definido de volta como (0,0,1,1) para fazer com que o quadro de vídeo inteiro seja renderizado.

[!code-cs[DoubleTapped](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDoubleTapped)]
        
##<a name="use-mediaplayersurface-to-render-video-to-a-windowsuicomposition-surface"></a>Usar MediaPlayerSurface para renderizar vídeo em uma superfície Windows.UI.Composition
A partir do Windows 10, versão 1607, é possível usar o **MediaPlayer** para renderizar vídeo em um [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.ICompositionSurface), o que permite que o player interopere com as APIs no namespace [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition). A estrutura de composição permite trabalhar com elementos gráficos na camada visual entre as APIs gráficas DirectX de nível inferior e XAML. Isso permite cenários como a renderização de vídeo em qualquer controle XAML. Para obter mais informações sobre como usar APIs de composição, consulte [Camada Visual](https://msdn.microsoft.com/windows/uwp/graphics/visual-layer).

O exemplo a seguir ilustra como renderizar o conteúdo do player de vídeo em um controle [**Canvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Canvas). As chamadas específicas do media player neste exemplo são [**SetSurfaceSize**](https://msdn.microsoft.com/library/windows/apps/mt489968) e [**GetSurface**](https://msdn.microsoft.com/library/windows/apps/mt489963). **SetSurfaceSize** informa ao sistema o tamanho do buffer que deve ser alocado para a renderização do conteúdo. **GetSurface** considera o [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.Compositor) como um argumento e recupera uma instância da classe [**MediaPlayerSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface). Essa classe fornece acesso ao **MediaPlayer** e ao **Compositor** usados para criar a superfície e a expõe por meio da propriedade [**CompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface.CompositionSurface).

O restante do código neste exemplo cria um [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.SpriteVisual), para que o vídeo seja renderizado, e define o tamanho como o tamanho do elemento canvas que exibirá o elemento visual. Em seguida, um [**CompositionBrush**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.CompositionBrush) é criado a partir do [**MediaPlayerSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface) e é atribuído à propriedade [**Brush**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.SpriteVisual.Brush) do elemento visual. Em seguida, um [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.ContainerVisual) é criado, e o **SpriteVisual** é inserido na parte superior da árvore visual. Por fim, [**SetElementChildVisual**](https://msdn.microsoft.com/library/windows/apps/mt608981) é chamado para atribuir o elemento de contêiner visual ao **Canvas**.

[!code-cs[Compositor](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCompositor)]
        
##<a name="use-mediatimelinecontroller-to-synchronize-content-across-multiple-players"></a>Use MediaTimelineController para sincronizar o conteúdo entre vários players.
Conforme discutido anteriormente neste artigo, seu aplicativo pode ter vários objetos **MediaPlayer** ativos por vez. Por padrão, cada **MediaPlayer** criado funciona de forma independente. Para alguns cenários, como a sincronização de uma faixa de comentário com um vídeo, é possível sincronizar o estado do player, bem como a posição e a velocidade de reprodução de vários players. A partir do Windows 10, versão 1607, é possível implementar esse comportamento usando a classe [**MediaTimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController).

###<a name="implement-playback-controls"></a>Implementar controles de reprodução
O exemplo a seguir mostra como usar um **MediaTimelineController** para controlar duas instâncias do **MediaPlayer**. Primeiro, cada instância do **MediaPlayer** é instanciada e **Source** é definido como um arquivo de mídia. Em seguida, um novo **MediaTimelineController** é criado. Para cada **MediaPlayer**, o [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) associado a cada player é desativado definindo a propriedade [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) como false. E, em seguida, a propriedade [**TimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.TimelineController) é definida como o objeto do controlador de linha do tempo.

[!code-cs[DeclareMediaTimelineController](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaTimelineController)]

[!code-cs[SetTimelineController](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetTimelineController)]

**Cuidado** O [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) proporciona a integração automática entre o **MediaPlayer** e os Controles de Transporte de Mídia do Sistema (SMTC), mas essa integração automática não pode ser usada com media players que sejam controlados com um **MediaTimelineController**. Dessa forma, você deverá desabilitar o gerenciador de comandos do media player antes de configurar o controlador de linha do tempo do player. Não seguir esse procedimento lançará uma exceção com a seguinte mensagem: "A anexação do Controlador de Linha do Tempo de Mídia está bloqueada por causa do estado atual do objeto." Para obter mais informações sobre a integração do media player com SMTC, consulte [Integrar aos Controles de Transporte de Mídia do Sistema](integrate-with-systemmediatransportcontrols.md). Se você estiver usando um **MediaTimelineController**, ainda poderá controlar o SMTC manualmente. Para obter mais informações, consulte [Controle Manual dos Controles de Transporte de Mídia do Sistema](system-media-transport-controls.md).

Depois de associar um **MediaTimelineController** a um ou mais media players, você poderá controlar o estado de reprodução usando os métodos expostos pelo controlador. A exemplo a seguir chama [**Start**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController.Start) para iniciar a reprodução de todos os media players associados no início da mídia.

[!code-cs[PlayButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPlayButtonClick)]

Este exemplo ilustra as ações de pausar e retomar todos os media players conectados.

[!code-cs[PauseButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPauseButtonClick)]

Para avançar todos os media players conectados, defina a velocidade de reprodução como um valor maior que 1.

[!code-cs[FastForwardButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetFastForwardButtonClick)]

O próximo exemplo mostra como usar um controle **Slider** para mostrar a posição de reprodução atual do controlador de linha do tempo em relação à duração do conteúdo de um dos media players conectados. Primeiro, um novo **MediaSource** é criado e um manipulador do [**OpenOperationCompleted**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource.OpenOperationCompleted) da origem de mídia é registrado. 

[!code-cs[CreateSourceWithOpenCompleted](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCreateSourceWithOpenCompleted)]

O manipulador **OpenOperationCompleted** é usado como uma oportunidade de descobrir a duração do conteúdo da origem da mídia. Depois de determinar a duração, o valor máximo do controle **Slider** é definido como o número total de segundos do item de mídia. O valor é definido em uma chamada para [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) para garantir que ele seja executado no thread da interface do usuário.

[!code-cs[DeclareDuration](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareDuration)]

[!code-cs[OpenCompleted](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOpenCompleted)]

Em seguida, um manipulador para o evento [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController.PositionChanged) do controlador de linha do tempo é registrado. Isso é chamado periodicamente pelo sistema, aproximadamente 4 vezes por segundo.

[!code-cs[RegisterPositionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPositionChanged)]

No manipulador de **PositionChanged**, o valor do controle deslizante é atualizado para refletir a posição atual do controlador de linha do tempo.

[!code-cs[PositionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPositionChanged)]

###<a name="offset-the-playback-position-from-the-timeline-position"></a>Deslocar a posição de reprodução da posição da linha do tempo
Em alguns casos, pode ser conveniente que a posição de reprodução de um ou mais media players associados a um controlador de linha do tempo seja deslocada dos outros players. Para fazer isso, defina a propriedade [**TimelineControllerPositionOffset**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.TimelineControllerPositionOffset) do objeto **MediaPlayer** que você deseja deslocar. O exemplo a seguir usa as durações do conteúdo de dois media players para definir os valores mínimo e máximo dos dois controles deslizantes como mais e menos o comprimento do item.  

[!code-cs[OffsetSliders](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOffsetSliders)]

No evento [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.RangeBase.ValueChanged) para cada controle deslizante, o **TimelineControllerPositionOffset** para cada player está definido como o valor correspondente.

[!code-cs[TimelineOffset](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetTimelineOffset)]

Observe que se o valor de deslocamento de um player for mapeado para uma posição de reprodução negativa, o clipe permanecerá pausado até que o deslocamento atinja zero e, em seguida, a reprodução será iniciada. Da mesma forma, se o valor de deslocamento for mapeado para uma posição de reprodução maior que a duração do item de mídia, o quadro final será mostrado, assim como acontece quando um único media player atinge o fim do conteúdo.

## <a name="related-topics"></a>Tópicos relacionados
* [Reprodução de mídia](media-playback.md)
* [Itens de mídia, playlists e faixas](media-playback-with-mediasource.md)
* [Integrar aos Controles de Transporte de Mídia do Sistema](integrate-with-systemmediatransportcontrols.md)
* [Criar, programar e gerenciar pausas de mídia](create-schedule-and-manage-media-breaks.md)
* [Reproduzir mídia em segundo plano](background-audio.md)





 

 




