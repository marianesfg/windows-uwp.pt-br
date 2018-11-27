---
ms.assetid: EFCF84D0-2F4C-454D-97DA-249E9EAA806C
description: A classe SystemMediaTransportControls permite que seu aplicativo use os controles de transporte de mídia do sistema que estão integrados ao Windows e atualize os metadados que os controles exibem sobre a mídia que seu aplicativo está reproduzindo atualmente.
title: Controle manual dos controles de transporte de mídia do sistema
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6be1680d1ce843c1fbe7105dc2027e764095495a
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7717221"
---
# <a name="manual-control-of-the-system-media-transport-controls"></a>Controle manual dos controles de transporte de mídia do sistema


A partir do Windows 10, versão 1607, os aplicativos UWP que usam a classe [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) para reproduzir mídia automaticamente são integrados com os controles de transporte do sistema mídia (SMTC) por padrão. Essa é a maneira recomendada de interagir com o SMTC para a maioria dos cenários. Para obter mais informações sobre como personalizar a integração padrão do SMTC com **MediaPlayer**, consulte [Integrar com Controles de transporte de mídia do sistema](integrate-with-systemmediatransportcontrols.md).

Existem alguns cenários em que talvez você precise implementar o controle manual do SMTC. Entre eles, se você estiver usando um [**MediaTimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController) para controlar a reprodução de um ou mais media players. Ou se você estiver usando vários media players e apenas deseja ter uma instância de SMTC para seu aplicativo. Você deverá controlar o SMTC manualmente se estiver usando [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaElement) para reproduzir mídia.

## <a name="set-up-transport-controls"></a>Configurar os controles de transporte
Se você estiver usando o **MediaPlayer** para reproduzir mídia, poderá obter uma instância da classe [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.SystemMediaTransportControls) acessando a propriedade [**MediaPlayer.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.SystemMediaTransportControls). Se você pretende controlar manualmente o SMTC, deverá desabilitar a integração automática fornecida pelo **MediaPlayer** definindo a propriedade [**CommandManager.IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) como false.

> [!NOTE] 
> Se você desabilitar o [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) do [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) definindo [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) como false, isso romperá o vínculo entre o **MediaPlayer** e o [**TransportControls**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.TransportControls) fornecido pelo **MediaPlayerElement**, portanto os controles de transporte internos não vão mais controlar automaticamente a reprodução do player. Em vez disso, você deve implementar seus próprios controles para regular o **MediaPlayer**.

[!code-cs[InitSMTCMediaPlayer](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetInitSMTCMediaPlayer)]

Você também pode obter uma instância dos [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) chamando [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708). Você deverá obter o objeto com esse método se estiver usando **MediaElement** para reproduzir mídia.

[!code-cs[InitSMTCMediaElement](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetInitSMTCMediaElement)]

Ative os botões que seu aplicativo usará configurando a propriedade "is enabled" correspondente do objeto **SystemMediaTransportControls**, como [**IsPlayEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278714), [**IsPauseEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278713), [**IsNextEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278712) e [**IsPreviousEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278715). Veja uma lista completa de controles disponíveis na documentação de referência aos **SystemMediaTransportControls**.

[!code-cs[EnableContols](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetEnableContols)]

Registre um manipulador para o evento [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706) para receber notificações quando o usuário pressionar um botão.

[!code-cs[RegisterButtonPressed](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetRegisterButtonPressed)]

## <a name="handle-system-media-transport-controls-button-presses"></a>Manipular pressionamentos de botões de controles de transporte de mídia do sistema

O evento [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706) é acionado pelos controles de transporte do sistema quando um dos botões ativados é pressionado. A propriedade [**Button**](https://msdn.microsoft.com/library/windows/apps/dn278685) dos [**SystemMediaTransportControlsButtonPressedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn278683) passados para o manipulador de eventos é membro da enumeração [**SystemMediaTransportControlsButton**](https://msdn.microsoft.com/library/windows/apps/dn278681) que indica quais dos botões ativados foram pressionados.

Para atualizar objetos no thread da interface do usuário no manipulador de eventos [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706), como um objeto [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926), você precisa realizar marshaling das chamadas por meio de [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211). Isso ocorre porque o manipulador de eventos **ButtonPressed** não é chamado a partir do thread da interface do usuário e, portanto, uma exceção será gerada se você tentar modificar diretamente a interface do usuário.

[!code-cs[SystemMediaTransportControlsButtonPressed](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsButtonPressed)]

## <a name="update-the-system-media-transport-controls-with-the-current-media-status"></a>Atualizar os controles de transporte de mídia do sistema com o status atual da mídia

Você deve notificar os [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) quando o estado da mídia mudar para que o sistema possa atualizar os controles para refletir o estado atual. Para fazer isso, configure a propriedade [**PlaybackStatus**](https://msdn.microsoft.com/library/windows/apps/dn278719) com o valor [**MediaPlaybackStatus**](https://msdn.microsoft.com/library/windows/apps/dn278665) apropriado no evento [**CurrentStateChanged**](https://msdn.microsoft.com/library/windows/apps/br227375) do [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926), que é lançado quando o estado da mídia muda.

[!code-cs[SystemMediaTransportControlsStateChange](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsStateChange)]

## <a name="update-the-system-media-transport-controls-with-media-info-and-thumbnails"></a>Atualizar os controles de transporte de mídia do sistema com miniaturas e informações de mídia

Use a classe [**SystemMediaTransportControlsDisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278686) para atualizar as informações da mídia mostradas pelos controles de transporte, como o título da música ou a capa do álbum do item de mídia atualmente em reprodução. Obtenha uma instância dessa classe com a propriedade [**SystemMediaTransportControls.DisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278707). Para cenários típicos, a maneira recomendada de passar os metadados é chamar [**CopyFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn278694), passando o arquivo de mídia atualmente em execução. O atualizador de exibição extrairá automaticamente os metadados e a imagem em miniatura do arquivo.

Chame [**Update**](https://msdn.microsoft.com/library/windows/apps/dn278701) para fazer com que os controles de transporte de mídia do sistema atualizem sua interface do usuário com os novos metadados e a miniatura.

[!code-cs[SystemMediaTransportControlsUpdater](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsUpdater)]

Se seu cenário exigir, você poderá atualizar os metadados exibidos pelos controles de transporte de mídia do sistema manualmente configurando os valores dos objetos [**MusicProperties**](https://msdn.microsoft.com/library/windows/apps/dn278696), [**ImageProperties**](https://msdn.microsoft.com/library/windows/apps/dn278695) ou [**VideoProperties**](https://msdn.microsoft.com/library/windows/apps/dn278702) expostos pela classe [**DisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278707).

[!code-cs[SystemMediaTransportControlsUpdaterManual](./code/SMTCWin10/cs/MainPage.xaml.cs#SystemMediaTransportControlsUpdaterManual)]

## <a name="update-the-system-media-transport-controls-timeline-properties"></a>Atualizar as propriedades de linha do tempo dos controles de transporte de mídia do sistema

Os controles de transporte do sistema exibem informações sobre a linha do tempo do item de mídia atualmente em execução, incluindo a posição de reprodução atual, a hora de início e a hora de término do item de mídia. Para atualizar as propriedades de linha do tempo dos controles de transporte do sistema, crie um novo objeto [**SystemMediaTransportControlsTimelineProperties**](https://msdn.microsoft.com/library/windows/apps/mt218746). Configure as propriedades do objeto para refletir o estado atual do item de mídia em reprodução. Chame [**SystemMediaTransportControls.UpdateTimelineProperties**](https://msdn.microsoft.com/library/windows/apps/mt218760) para fazer com que os controles atualizem a linha do tempo.

[!code-cs[UpdateTimelineProperties](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetUpdateTimelineProperties)]

-   Você deve fornecer um valor para [**StartTime**](https://msdn.microsoft.com/library/windows/apps/mt218751), [**EndTime**](https://msdn.microsoft.com/library/windows/apps/mt218747) e [**Position**](https://msdn.microsoft.com/library/windows/apps/mt218755) para que os controles do sistema mostrem uma linha do tempo para o item em execução.

-   [**MinSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218749) e [**MaxSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218748) permitem que você especifique o intervalo na linha do tempo que o usuário pode buscar. Um cenário típico para isso é permitir que os provedores de conteúdo incluam pausas para anúncios em suas mídias.

    Você deve configurar [**MinSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218749) e [**MaxSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218748) para que a [**PositionChangeRequest**](https://msdn.microsoft.com/library/windows/apps/mt218755) seja lançada.

-   É recomendável manter os controles do sistema em sincronia com a reprodução da mídia, atualizando essas propriedades aproximadamente a cada 5 segundos durante a reprodução e novamente sempre que o estado da reprodução mudar, por exemplo, ao pausar ou buscar uma nova posição.

## <a name="respond-to-player-property-changes"></a>Responder às alterações de propriedade do player

Há um conjunto de propriedades de controles de transporte do sistema que se relacionam ao estado atual do player de mídia em si, em vez do estado do item de mídia em reprodução. Cada uma dessas propriedades é relacionada a um evento que é lançado quando o usuário ajusta o controle associado. Essas propriedades e eventos incluem:

| Propriedade                                                                  | Evento                                                                                                   |
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| [**AutoRepeatMode**](https://msdn.microsoft.com/library/windows/apps/mt218753) | [**AutoRepeatModeChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218754) |
| [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/mt218756)     | [**PlaybackRateChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218757)     |
| [**ShuffleEnabled**](https://msdn.microsoft.com/library/windows/apps/mt218758) | [**ShuffleEnabledChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218759) |

 
Para manipular a interação do usuário com um desses controles, registre um manipulador para o evento associado primeiro.

[!code-cs[RegisterPlaybackChangedHandler](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetRegisterPlaybackChangedHandler)]

No manipulador do evento, certifique-se primeiramente de que o valor solicitado esteja em um intervalo válido e esperado. Se estiver, configure a propriedade correspondente no [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) e configure a propriedade correspondente no objeto [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677).

[!code-cs[PlaybackChangedHandler](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetPlaybackChangedHandler)]

-   Para que um desses eventos de propriedade do player seja lançado, você deve definir um valor inicial para a propriedade. Por exemplo, [**PlaybackRateChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218757) não será lançado até que você tenha definido um valor para a propriedade [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/mt218756) pelo menos uma vez.

## <a name="use-the-system-media-transport-controls-for-background-audio"></a>Usar os controles de transporte de mídia do sistema para áudio em segundo plano

Se você não estiver usando a integração de SMTC automática fornecida pelo **MediaPlayer** manualmente, deverá efetuar a integração com o SMTC para habilitar o áudio em segundo plano. No mínimo, seu aplicativo deverá habilitar os botões de reprodução e pausa definindo [**IsPlayEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278714) e [**IsPauseEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278713) como true. O aplicativo também deve manipular o evento [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706). Se seu aplicativo não cumprir esses requisitos, reprodução de áudio será interrompida quando seu aplicativo for movido para segundo plano.

Aplicativos que usam o novo modelo de um processo para áudio em segundo plano devem obter uma instância do [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) chamando [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708). Aplicativos que usam o modelo de dois processos herdado para áudio em segundo plano devem usar [**BackgroundMediaPlayer.Current.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn926635) para acessar o SMTC de seu processo em segundo plano.

Para obter informações sobre como reproduzir áudio em segundo plano, consulte [Reproduzir mídia em segundo plano](background-audio.md).

## <a name="related-topics"></a>Tópicos relacionados
* [Reprodução de mídia](media-playback.md)
* [Integrar aos Controles de transporte de mídia do sistema](integrate-with-systemmediatransportcontrols.md) 
* [Exemplo de transporte de mídia do sistema](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls) 

 




