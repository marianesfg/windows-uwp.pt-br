---
ms.assetid: 58af5e9d-37a1-4f42-909c-db7cb02a0d12
description: Este artigo mostra como reproduzir mídia em seu aplicativo Universal do Windows com o MediaPlayer.
title: Reproduzir áudio e vídeo com o MediaPlayer
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4d33a2bf1505618dca4e0e54c2bd9a534f58bcfc
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7706555"
---
# <a name="play-audio-and-video-with-mediaplayer"></a>Reproduzir áudio e vídeo com o MediaPlayer

Este artigo mostra como reproduzir mídia em seu aplicativo Universal do Windows usando a classe [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer). Com o Windows 10, versão 1607, melhorias significativas foram feitas nas APIs de reprodução de mídia, incluindo um design de processo único simplificado para áudio em segundo plano, integração automática com os Controles de Transporte de Mídia do Sistema (SMTC), a capacidade de sincronizar vários media players, o recurso de uma superfície Windows.UI.Composition e uma interface simples para criar e programar pausas de mídia em seu conteúdo. Para tirar proveito dessas melhorias, a prática recomendada para a reprodução de mídia é usar a classe **MediaPlayer** em vez de **MediaElement**. O [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement), um controle XAML leve, foi introduzido para permitir que você renderize conteúdo de mídia em uma página XAML. Muitas das APIs de status e controle de reprodução fornecidas pelo **MediaElement** agora estão disponíveis por meio do novo objeto [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession). O **MediaElement** continua a funcionar a fim de dar suporte à compatibilidade com versões anteriores, mas nenhum outro recurso será adicionado a essa classe.

Este artigo fornecerá orientações sobre os recursos do **MediaPlayer** que serão usados por um aplicativo típico de reprodução de mídia. Observe que o **MediaPlayer** usa a classe [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource) como um contêiner para todos os itens de mídia. Essa classe permite carregar e reproduzir mídia de várias origens diferentes, incluindo arquivos locais, fluxos de memória e origens de rede, todos usando a mesma interface. Também há classes de nível superior que funcionam com o **MediaSource**, como [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) e [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList), que oferecem recursos mais avançados, como playlists e a capacidade de gerenciar origens de mídia com várias faixas de áudio, vídeo e metadados. Para obter mais informações sobre **MediaSource** e APIs relacionadas, consulte [Itens de mídia, playlists e faixas](media-playback-with-mediasource.md).

> [!NOTE] 
> As edições do Windows 10 N e Windows 10 KN não incluem os recursos de mídia necessários para usar o **MediaPlayer** para reprodução. Esses recursos podem ser instalados manualmente. Para obter mais informações, consulte [Pacote de recursos de mídia para Windows 10 N e Windows 10 KN](https://support.microsoft.com/en-us/help/3010081/media-feature-pack-for-windows-10-n-and-windows-10-kn-editions).

## <a name="play-a-media-file-with-mediaplayer"></a>Reproduzir um arquivo de mídia com o MediaPlayer  
A reprodução básica de mídia com o **MediaPlayer** é bastante simples de implementar. Primeiro, crie uma nova instância da classe **MediaPlayer**. Seu aplicativo pode ter várias instâncias ativas do **MediaPlayer** simultaneamente. Em seguida, defina a propriedade [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) do player como um objeto que implemente a [ **IMediaPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.IMediaPlaybackSource), como uma [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource), um [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) ou um [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList). Neste exemplo, um **MediaSource** é criado a partir de um arquivo contido no armazenamento local do aplicativo e, em seguida, um **MediaPlaybackItem** é criado a partir da origem e é atribuído à propriedade **Source** do player.

Diferentemente do **MediaElement**, o **MediaPlayer** não inicia a reprodução automaticamente por padrão. Para iniciar a reprodução, você pode chamar [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play), definir a propriedade [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AutoPlay) como true ou aguardar que o usuário inicie a reprodução com os controles de mídia integrados.

[!code-cs[SimpleFilePlayback](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSimpleFilePlayback)]

Quando seu aplicativo terminar de usar o **MediaPlayer**, você deverá chamar o método [**Close**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Close) (projetado como **Dispose** em C#) para limpar os recursos usados pelo player.

[!code-cs[CloseMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCloseMediaPlayer)]

## <a name="use-mediaplayerelement-to-render-video-in-xaml"></a>Usar o MediaPlayerElement para renderizar vídeo em XAML
É possível reproduzir mídia em um **MediaPlayer** sem exibi-la em XAML, mas em muitos aplicativos de reprodução de mídia, você precisará renderizar a mídia em uma página XAML. Para fazer isso, use o controle leve [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement). Como o **MediaElement**, o **MediaPlayerElement** permite especificar se os controles de transporte integrados deverão ou não ser mostrados.

[!code-xml[MediaPlayerElementXAML](./code/MediaPlayer_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

Você pode definir a instância do**MediaPlayer** à qual o elemento está vinculado chamando [**SetMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/mt708764).

[!code-cs[SetMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetMediaPlayer)]

Você também pode definir a origem de reprodução no **MediaPlayerElement**, e o elemento criará automaticamente uma nova instância do **MediaPlayer** que poderá ser acessada com a propriedade [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.MediaPlayer).

[!code-cs[GetPlayerFromElement](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetGetPlayerFromElement)]

> [!NOTE] 
> Se você desabilitar o [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) do [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) definindo [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) como false, isso romperá o vínculo entre o **MediaPlayer** e o [**TransportControls**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.TransportControls) fornecido pelo **MediaPlayerElement**, portanto os controles de transporte internos não vão mais controlar automaticamente a reprodução do player. Em vez disso, você deve implementar seus próprios controles para regular o **MediaPlayer**.

## <a name="common-mediaplayer-tasks"></a>Tarefas comuns do MediaPlayer
Esta seção mostra como usar alguns recursos do **MediaPlayer**.

### <a name="set-the-audio-category"></a>Definir a categoria de áudio
Defina a propriedade [**AudioCategory**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AudioCategory) de um **MediaPlayer** como um dos valores da enumeração [**MediaPlayerAudioCategory**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerAudioCategory) para informar o sistema qual tipo de mídia você está executando. Jogos devem categorizar seus fluxos de música como **GameMedia** para que o mudo do jogo seja ativado automaticamente se outro aplicativo reproduzir música em segundo plano. Os aplicativos de música ou vídeo devem categorizar seus fluxos como **Media** ou **Movie** para que eles tenham prioridade sobre fluxos **GameMedia**.

[!code-cs[SetAudioCategory](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioCategory)]

### <a name="output-to-a-specific-audio-endpoint"></a>Saída para um ponto de extremidade de áudio específico
Por padrão, a saída de áudio de um **MediaPlayer** é roteada para o ponto de extremidade de áudio padrão do sistema, mas você pode especificar um ponto de extremidade de áudio específico que o **MediaPlayer** deverá usar para a saída. No exemplo a seguir, [**MediaDevice.GetAudioRenderSelector**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.MediaDevice.GetAudioRenderSelector) retorna uma cadeia de caracteres que identifica, de forma exclusiva, a categoria de renderização de áudio dos dispositivos. Em seguida, o método [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation) [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation.FindAllAsync) é chamado para obter uma lista de todos os dispositivos disponíveis do tipo selecionado. Você pode determinar programaticamente qual dispositivo deseja usar ou adicionar os dispositivos retornados a uma [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ComboBox) para permitir que o usuário selecione um dispositivo.

[!code-cs[SetAudioEndpointEnumerate](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpointEnumerate)]

No evento [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.Selector.SelectionChanged) para a caixa de combinação de dispositivos, a propriedade [**AudioDevice**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AudioDevice) do **MediaPlayer** está definida como o dispositivo selecionado, que foi armazenado na propriedade [**Tag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FrameworkElement.Tag) do **ComboBoxItem**.

[!code-cs[SetAudioEndpontSelectionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpontSelectionChanged)]

### <a name="playback-session"></a>Sessão de reprodução
Conforme descrito anteriormente neste artigo, muitas das funções que são expostas pela classe **MediaElement** foram transferidas para a classe [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession). Isso inclui informações sobre o estado de reprodução do player, como a posição de reprodução atual, se o player está pausado ou em reprodução e a velocidade de reprodução atual. O **MediaPlaybackSession** também fornece vários eventos para avisá-lo quando o estado é alterado, inclusive o status de transferência e o buffer atual do conteúdo que está sendo reproduzido, bem como o tamanho natural e a taxa de proporção do conteúdo de vídeo em reprodução no momento.

O exemplo a seguir mostra como implementar um manipulador de clique de botão que avança 10 segundos no conteúdo. Primeiro, o objeto**MediaPlaybackSession** do player é recuperado com a propriedade [**PlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.PlaybackSession). Em seguida, a propriedade [**Position**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.Position) é definida como a posição de reprodução atual mais 10 segundos.

[!code-cs[SkipForwardClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSkipForwardClick)]

O próximo exemplo ilustra o uso de um botão de alternância para alternar entre a velocidade de reprodução normal e 2 X a velocidade definindo a propriedade [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.PlaybackRate) da sessão.

[!code-cs[SpeedChecked](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSpeedChecked)]

A partir do Windows 10, versão 1803, você pode definir a rotação de apresentação do vídeo no **MediaPlayer** em incrementos de 90 graus.

[!code-cs[SetRotation](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetRotation)]

### <a name="detect-expected-and-unexpected-buffering"></a>Detectar buffer esperado e inesperado
O objeto **MediaPlaybackSession** descrito na seção anterior fornece dois eventos de detecção quando o arquivo de mídia em reprodução começa e encerra o buffer, **[BufferingStarted](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.BufferingStarted)** e **[BufferingEnded](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.BufferingEnded)**. Isso permite que você atualize a interface do usuário para mostrar ao usuário que o buffer está ocorrendo. O buffer inicial é esperado quando um arquivo de mídia é aberto pela primeira vez ou quando o usuário alterna para um novo item de uma playlist. O buffer inesperado pode ocorrer quando a velocidade de rede degrada ou se o sistema de gerenciamento de conteúdo que fornece o conteúdo tiver problemas técnicos. A partir do RS3, você pode usar o evento **BufferingStarted** para determinar se o evento de buffer é esperado ou se é inesperado e interrompe a reprodução. Você pode usar essas informações como dados de telemetria para o serviço de entrega do aplicativo ou mídia. 

Registre manipuladores para os eventos **BufferingStarted** e **BufferingEnded** a fim de receber notificações de estado de buffer.

[!code-cs[RegisterBufferingHandlers](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterBufferingHandlers)]

No manipulador de evento **BufferingStarted**, transmita os argumentos de evento passados para o evento em um objeto **[MediaPlaybackSessionBufferingStartedEventArgs](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksessionbufferingstartedeventargs)** e verifique a propriedade **[IsPlaybackInterruption](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksessionbufferingstartedeventargs.IsPlaybackInterruption)**. Se esse valor for true, o buffer que disparou o evento é inesperado e interrompe a reprodução. Caso contrário, o buffer inicial é esperado. 

[!code-cs[BufferingHandlers](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetBufferingHandlers)]


### <a name="pinch-and-zoom-video"></a>Pinçar e aplicar zoom em vídeo
O **MediaPlayer** permite especificar o retângulo de origem no conteúdo de vídeo que deve ser renderizado, permitindo aplicar zoom ao vídeo de forma efetiva. O retângulo especificado é relativo a um retângulo normalizado (0,0,1,1), onde 0,0 é a parte superior esquerda do quadro, e 1,1 especifica a largura e a altura totais do quadro. Assim, por exemplo, para definir o retângulo de zoom para que o quadrante superior direito do vídeo seja renderizado, você especificaria o retângulo (.5,0,.5,.5).  É importante verificar seus valores para garantir que o retângulo de origem esteja dentro do retângulo normalizado (0,0,1,1). A tentativa de definir um valor fora desse intervalo fará com que uma exceção seja lançada.

Para implementar o recurso de pinçar e aplicar zoom usando gestos multitoque, primeiro é necessário especificar os gestos aos quais deseja dar suporte. Neste exemplo, é necessário dimensionar e fazer a translação dos gestos. O evento [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.ManipulationDelta) é acionado quando é feito um dos gestos inscritos. O evento [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DoubleTapped) será usado para redefinir o zoom para o quadro completo. 

[!code-cs[RegisterPinchZoomEvents](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPinchZoomEvents)]

Em seguida, declare um objeto **Rect** que armazenará o retângulo de origem de zoom atual.

[!code-cs[DeclareSourceRect](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareSourceRect)]

O manipulador **ManipulationDelta** ajusta a escala ou a translação do retângulo de zoom. Se o valor de escala delta não for 1, isso significa que o usuário realizou um gesto de pinçar. Se o valor for maior que 1, o retângulo de origem deverá ser menor para ampliar o conteúdo. Se o valor for menor que 1, o retângulo de origem deve ser maior para reduzir. Antes de configurar os novos valores de escala, o retângulo resultante é verificado para garantir que esteja inteiramente dentro dos limites (0,0,1,1).

Se o valor de escala for 1, o gesto de translação será manipulado. O retângulo é simplesmente convertido pelo número de pixels contidos no gesto, dividido pela largura e a altura do controle. Novamente, o retângulo resultante é verificado para garantir que ele fique dentro dos limites (0,0,1,1).

Por fim, o [**NormalizedSourceRect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.NormalizedSourceRect) do **MediaPlaybackSession** é definido como o retângulo recém-ajustado, especificando a área dentro do quadro de vídeo que deve ser renderizado.

[!code-cs[ManipulationDelta](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetManipulationDelta)]

No manipulador de eventos [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DoubleTapped), o retângulo de origem é definido de volta como (0,0,1,1) para fazer com que o quadro de vídeo inteiro seja renderizado.

[!code-cs[DoubleTapped](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDoubleTapped)]

### <a name="handling-policy-based-playback-degradation"></a>Processamento da degradação da reprodução com base em política

Em algumas circunstâncias, o sistema pode degradar a reprodução de um item de mídia, por exemplo, ao reduzir a resolução (restrição), com base em uma política em vez de um problema de desempenho. Por exemplo, o vídeo pode ser degradado pelo sistema se estiver sendo reproduzido usando um driver de vídeo não assinado. Você pode chamar [**MediaPlaybackSession.GetOutputDegradationPolicyState**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.getoutputdegradationpolicystate#Windows_Media_Playback_MediaPlaybackSession_GetOutputDegradationPolicyState) para determinar se e porque essa degradação com base em política ocorre, assim como alertar o usuário ou registrar o motivo para fins de telemetria.

O exemplo a seguir mostra uma implementação de um manipulador do evento **MediaPlayer.MediaOpened** é acionado quando o jogador abre um novo item de mídia. **GetOutputDegradationPolicyState** é chamado no **MediaPlayer** passado para o manipulador. O valor de [**VideoConstrictionReason**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksessionoutputdegradationpolicystate.videoconstrictionreason#Windows_Media_Playback_MediaPlaybackSessionOutputDegradationPolicyState_VideoConstrictionReason) indica que o motivo da política usada para restringir o vídeo. Se o valor não for **None**, esse exemplo registra um motivo de degradação para fins de telemetria. Esse exemplo também mostra a configuração da taxa de bits da **AdaptiveMediaSource** reproduzida no momento com a menor largura de banda para economizar o uso de dados, pois o vídeo está restrito e não será exibido com alta resolução. Para saber mais sobre como usar o **AdaptiveMediaSource**, consulte [Streaming adaptável](adaptive-streaming.md).

[!code-cs[PolicyDegradation](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPolicyDegradation)]
        
## <a name="use-mediaplayersurface-to-render-video-to-a-windowsuicomposition-surface"></a>Usar MediaPlayerSurface para renderizar vídeo em uma superfície Windows.UI.Composition
A partir do Windows 10, versão 1607, é possível usar o **MediaPlayer** para renderizar vídeo em um [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.ICompositionSurface), o que permite que o player interopere com as APIs no namespace [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition). A estrutura de composição permite trabalhar com elementos gráficos na camada visual entre as APIs gráficas DirectX de nível inferior e XAML. Isso permite cenários como a renderização de vídeo em qualquer controle XAML. Para obter mais informações sobre como usar APIs de composição, consulte [Camada Visual](https://msdn.microsoft.com/windows/uwp/composition/visual-layer).

O exemplo a seguir ilustra como renderizar o conteúdo do player de vídeo em um controle [**Canvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Canvas). As chamadas específicas do media player neste exemplo são [**SetSurfaceSize**](https://msdn.microsoft.com/library/windows/apps/mt489968) e [**GetSurface**](https://msdn.microsoft.com/library/windows/apps/mt489963). **SetSurfaceSize** informa ao sistema o tamanho do buffer que deve ser alocado para a renderização do conteúdo. **GetSurface** considera o [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.Compositor) como um argumento e recupera uma instância da classe [**MediaPlayerSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface). Essa classe fornece acesso ao **MediaPlayer** e ao **Compositor** usados para criar a superfície e a expõe por meio da propriedade [**CompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface.CompositionSurface).

O restante do código neste exemplo cria um [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.SpriteVisual), para que o vídeo seja renderizado, e define o tamanho como o tamanho do elemento canvas que exibirá o elemento visual. Em seguida, um [**CompositionBrush**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.CompositionBrush) é criado a partir do [**MediaPlayerSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface) e é atribuído à propriedade [**Brush**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.SpriteVisual.Brush) do elemento visual. Em seguida, um [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.ContainerVisual) é criado, e o **SpriteVisual** é inserido na parte superior da árvore visual. Por fim, [**SetElementChildVisual**](https://msdn.microsoft.com/library/windows/apps/mt608981) é chamado para atribuir o elemento de contêiner visual ao **Canvas**.

[!code-cs[Compositor](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCompositor)]
        
## <a name="use-mediatimelinecontroller-to-synchronize-content-across-multiple-players"></a>Use MediaTimelineController para sincronizar o conteúdo entre vários players.
Conforme discutido anteriormente neste artigo, seu aplicativo pode ter vários objetos **MediaPlayer** ativos por vez. Por padrão, cada **MediaPlayer** criado funciona de forma independente. Para alguns cenários, como a sincronização de uma faixa de comentário com um vídeo, é possível sincronizar o estado do player, bem como a posição e a velocidade de reprodução de vários players. A partir do Windows 10, versão 1607, é possível implementar esse comportamento usando a classe [**MediaTimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController).

### <a name="implement-playback-controls"></a>Implementar controles de reprodução
O exemplo a seguir mostra como usar um **MediaTimelineController** para controlar duas instâncias do **MediaPlayer**. Primeiro, cada instância do **MediaPlayer** é instanciada e **Source** é definido como um arquivo de mídia. Em seguida, um novo **MediaTimelineController** é criado. Para cada **MediaPlayer**, o [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) associado a cada player é desativado definindo a propriedade [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) como false. Em seguida, a propriedade [**TimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.TimelineController) é definida como o objeto do controlador de linha do tempo.

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

### <a name="offset-the-playback-position-from-the-timeline-position"></a>Deslocar a posição de reprodução da posição da linha do tempo
Em alguns casos, pode ser conveniente que a posição de reprodução de um ou mais media players associados a um controlador de linha do tempo seja deslocada dos outros players. Para fazer isso, defina a propriedade [**TimelineControllerPositionOffset**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.TimelineControllerPositionOffset) do objeto **MediaPlayer** que você deseja deslocar. O exemplo a seguir usa as durações do conteúdo de dois media players para definir os valores mínimo e máximo dos dois controles deslizantes como mais e menos o comprimento do item.  

[!code-cs[OffsetSliders](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOffsetSliders)]

No evento [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.RangeBase.ValueChanged) para cada controle deslizante, o **TimelineControllerPositionOffset** para cada player está definido como o valor correspondente.

[!code-cs[TimelineOffset](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetTimelineOffset)]

Observe que se o valor de deslocamento de um player for mapeado para uma posição de reprodução negativa, o clipe permanecerá pausado até que o deslocamento atinja zero e, em seguida, a reprodução será iniciada. Da mesma forma, se o valor de deslocamento for mapeado para uma posição de reprodução maior que a duração do item de mídia, o quadro final será mostrado, assim como acontece quando um único media player atinge o fim do conteúdo.

## <a name="play-spherical-video-with-mediaplayer"></a>Reproduzir áudio esférico com o MediaPlayer
A partir do Windows 10, versão 1703, o **MediaPlayer** dá suporte à projeção equirretangular para reprodução de vídeo esférico. O conteúdo de vídeo esférico não é diferente do vídeo regular e simples em que o **MediaPlayer** renderizará o vídeo, desde que a codificação de vídeo seja compatível. Para o vídeo esférico que contém uma marca de metadados que especifica que o vídeo usa a projeção equirretangular, o **MediaPlayer** pode renderizar o vídeo usando um campo de visão e uma orientação de exibição especificados. Isso possibilita cenários como reprodução de vídeo de realidade virtual com um capacete de realidade virtual ou simplesmente permite que o usuário faça uma panorâmica em torno do conteúdo de vídeo esférico usando o mouse ou o teclado.

Para reproduzir vídeo esférico, use as etapas para reproduzir o conteúdo de vídeo descrito anteriormente neste artigo. Uma etapa adicional é registrar um manipulador para o evento [**MediaPlayer.MediaOpened**])https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer#Windows_Media_Playback_MediaPlayer_MediaOpened). Esse evento oferece uma oportunidade para habilitar e controlar os parâmetros de reprodução de vídeo esférico.

[!code-cs[OpenSphericalVideo](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOpenSphericalVideo)]

No manipulador **MediaOpened**, primeiro verifique o formato do quadro do item de mídia recém-aberto ao verificar a propriedade [**PlaybackSession.SphericalVideoProjection.FrameFormat**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.FrameFormat). Se esse valor for [**SphericaVideoFrameFormat.Equirectangular**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.sphericalvideoframeformat), então o sistema poderá projetar automaticamente o conteúdo de vídeo. Primeiro, defina a propriedade [**PlaybackSession.SphericalVideoProjection.IsEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.IsEnabled) como **true**. Você também pode ajustar propriedades, como a orientação de exibição e o campo de visão que o media player usará para projetar o conteúdo de vídeo. Neste exemplo, o campo de visão é definido como um valor amplo de 120 graus por meio da definição da propriedade [**HorizontalFieldOfViewInDegrees**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.HorizontalFieldOfViewInDegrees).

Se o conteúdo de vídeo for esférico, mas se estiver em um formato diferente do equirretangular, você poderá implementar seu próprio algoritmo de projeção usando o modo de servidor de quadros do player de mídia para receber e processar quadros individuais.

[!code-cs[SphericalMediaOpened](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalMediaOpened)]

O código de exemplo a seguir ilustra como ajustar a orientação de exibição de vídeo esférico usando as teclas de seta para a esquerda e direita.

[!code-cs[SphericalOnKeyDown](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalOnKeyDown)]

Se seu aplicativo der suporte às playlists de vídeo, convém identificar itens de reprodução que contenham vídeo esférico em sua interface do usuário. As playlists de mídia são abordadas em detalhes no artigo [Itens de mídia, playlists e faixas](media-playback-with-mediasource.md). O exemplo a seguir mostra a criação de uma nova playlist, adicionando um item e registrando um manipulador para o evento [**MediaPlaybackItem.VideoTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.VideoTracksChanged), que ocorre quando as faixas de vídeo de um item de mídia são resolvidas.

[!code-cs[SphericalList](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalList)]

No manipulador de eventos **VideoTracksChanged**, obtenha as propriedades de codificação para quaisquer faixas de vídeo adicionadas ao chamar [**VideoTrack.GetEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.core.videotrack.GetEncodingProperties). Se a propriedade [**SphericalVideoFrameFormat**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.videoencodingproperties.SphericalVideoFrameFormat) das propriedades de codificação for um valor diferente de [**SphericaVideoFrameFormat.None**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.sphericalvideoframeformat), então a faixa de vídeo conterá vídeo esférico e será possível atualizar sua interface do usuário adequadamente, caso você queira.

[!code-cs[SphericalTracksChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalTracksChanged)]

## <a name="use-mediaplayer-in-frame-server-mode"></a>Usar o MediaPlayer no modo de servidor de quadros
A partir do Windows 10, versão 1703, você poderá usar o **MediaPlayer** no modo de servidor de quadros. Nesse modo, o **MediaPlayer** não renderiza automaticamente os quadros para um **MediaPlayerElement** associado. Em vez disso, seu app copia o quadro atual do **MediaPlayer** para um objeto que implementa [**IDirect3DSurface**](https://docs.microsoft.com/uwp/api/windows.graphics.directx.direct3d11.idirect3dsurface). O principal cenário que esse recurso habilita é o uso de sombreadores de pixel para processar os quadros de vídeo fornecidos pelo **MediaPlayer**. Seu aplicativo é responsável pela exibição de cada quadro após o processamento, como ao mostrar o quadro em um controle [**Image**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image) XAML.

No exemplo a seguir, um novo **MediaPlayer** é inicializado e o conteúdo de vídeo é carregado. Em seguida, um manipulador para [**VideoFrameAvailable**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.VideoFrameAvailable) é registrado. O modo de servidor de quadros é habilitado por meio da configuração da propriedade [**IsVideoFrameServerEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.IsVideoFrameServerEnabled) do objeto **MediaPlayer** como **true**. Por fim, a reprodução de mídia é iniciada com uma chamada a [**Play**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Play).

[!code-cs[FrameServerInit](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetFrameServerInit)]

O próximo exemplo mostra um manipulador para **VideoFrameAvailable** que usa [Win2D](https://github.com/Microsoft/Win2D) para adicionar um efeito simples de desfoque a cada quadro de um vídeo e então exibe os quadros processados em um controle[Image](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image) XAML.

Sempre que o manipulador **VideoFrameAvailable** for chamado, o método [**CopyFrameToVideoSurface**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.copyframetovideosurface) será usado para copiar o conteúdo do quadro para uma [**IDirect3DSurface**](https://docs.microsoft.com/uwp/api/windows.graphics.directx.direct3d11.idirect3dsurface). Você também pode usar [**CopyFrameToStereoscopicVideoSurfaces**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.copyframetostereoscopicvideosurfaces) para copiar o conteúdo 3D para duas superfícies, para processamento do conteúdo do olho esquerdo e do olho direito separadamente. Para obter um objeto que implemente **IDirect3DSurface**, este exemplo cria um [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap) e então usa esse objeto para criar um **CanvasBitmap** Win2D, que implementa a interface necessária. A **CanvasImageSource** é um objeto Win2D que pode ser usado como a origem para um controle **Imagem** e, portanto, um novo é criado e definido como a origem para a **Imagem** na qual o conteúdo será exibido. Em seguida, uma **CanvasDrawingSession** é criada. Ela é usada pelo Win2D para renderizar o efeito de desfoque.

Depois que todos os objetos necessários tenham sido criados, **CopyFrameToVideoSurface** será chamado, o que copia o quadro atual do **MediaPlayer** para o **CanvasBitmap**. Em seguida, um **GaussianBlurEffect** Win2D é criado, com o **CanvasBitmap** definido como a origem da operação. Por fim, **CanvasDrawingSession.DrawImage** é chamada para desenhar a imagem de origem, com o efeito de desfoque aplicado, para a **CanvasImageSource** que foi associada ao controle **Image**, fazendo com que ela seja desenhada na interface do usuário.

[!code-cs[VideoFrameAvailable](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetVideoFrameAvailable)]

Para saber mais sobre como usar o Win2D, veja o [Repositório do Win2D no GitHub](https://github.com/Microsoft/Win2D). Para experimentar o código de exemplo mostrado acima, você precisará adicionar o pacote NuGet Win2D ao seu projeto com as instruções a seguir.

**Para adicionar o pacote NuGet Win2D ao seu projeto de efeito**

1.  No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto e selecione **Gerenciar Pacotes NuGet**.
2.  Na parte superior da janela, selecione a guia **Procurar**.
3.  Na caixa de pesquisa, digite **Win2D**.
4.  Selecione **Win2D.uwp** e, em seguida, selecione **Instalar** no painel à direita.
5.  A caixa de diálogo **Revisar Alterações** mostra o pacote a ser instalado. Clique em **OK**.
6.  Aceite a licença do pacote.

## <a name="detect-and-respond-to-audio-level-changes-by-the-system"></a>Detectar e responder às alterações no nível de áudio pelo sistema
A partir do Windows 10, versão 1803, seu aplicativo pode detectar quando o sistema reduz o nível ou ativa o mudo de um **MediaPlayer** reproduzido no momento. Por exemplo, o sistema pode reduzir ou "ignorar", o nível de reprodução de áudio quando um alarme está tocando. O sistema ativa o mudo do aplicativo quando entra em segundo plano caso não tenha declarado a funcionalidade *backgroundMediaPlayback* no manifesto. A classe [**AudioStateMonitor**](https://docs.microsoft.comuwp/api/windows.media.audio.audiostatemonitor) permite que você registre-se para receber um evento quando o sistema modifica o volume de um fluxo de áudio. Acesse a propriedade **AudioStateMonitor** de um **MediaPlayer** e registre um manipulador do evento [**SoundLevelChanged**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged) para ser notificado quando o nível de áudio do **MediaPlayer** é alterado pelo sistema.

[!code-cs[RegisterAudioStateMonitor](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterAudioStateMonitor)]

Ao manipular o evento **SoundLevelChanged**, você pode ter ações diferentes dependendo do tipo de conteúdo reproduzido. Se você está reproduzindo músicas, convém permitir que a música continue a ser reproduzida enquanto o volume é abaixado. Entretanto, se estiver reproduzindo um podcast, você provavelmente desejará pausar a reprodução enquanto o áudio é abaixado para que o usuário não perca qualquer conteúdo.

Esse exemplo declara uma variável para controlar se o conteúdo em execução é um podcast; presume-se que você define isso com o valor apropriado ao selecionar o conteúdo do **MediaPlayer**. Também criamos uma variável de classe para controlar quando pausamos a reprodução programaticamente e o nível de áudio é alterado.

[!code-cs[AudioStateVars](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

No manipulador de eventos **SoundLevelChanged**, verifique a propriedade [**SoundLevel**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) do remetente de **AudioStateMonitor** para determinar o novo nível de som. Esse exemplo verifica se o nível do novo som está com volume total, ou seja, se o sistema deixou de ativar o mudo ou diminuir o volume, ou se o nível de som foi reduzido, mas está reproduzindo o conteúdo diferente de um podcast. Se qualquer um dos seguintes procedimentos for verdadeiro e o conteúdo foi pausado anteriormente de modo programático, a reprodução é retomada. Se o nível do som novo for silenciado ou se o conteúdo atual for um podcast e o nível de som for baixo, a reprodução é pausada e a variável é definida para verificar se a pausa foi iniciada de forma programática.

[!code-cs[SoundLevelChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSoundLevelChanged)]

O usuário pode decidir que eles querem pausar ou continuar a reprodução, mesmo se o áudio for ignorado pelo sistema. Esse exemplo mostra os manipuladores de eventos de um botão de reprodução e pausa. No botão de pausa, o manipulador de clique é pausado. Se a reprodução já foi pausada programaticamente, então atualizamos a variável para indicar que o usuário pausou o conteúdo. No manipulador de clique do botão de reprodução, é possível continuar a reprodução e limpar a variável de rastreamento.

[!code-cs[ButtonUserClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetButtonUserClick)]

## <a name="related-topics"></a>Tópicos relacionados
* [Reprodução de mídia](media-playback.md)
* [Itens de mídia, playlists e faixas](media-playback-with-mediasource.md)
* [Integrar aos Controles de Transporte de Mídia do Sistema](integrate-with-systemmediatransportcontrols.md)
* [Criar, programar e gerenciar pausas de mídia](create-schedule-and-manage-media-breaks.md)
* [Reproduzir mídia em segundo plano](background-audio.md)





 

 




