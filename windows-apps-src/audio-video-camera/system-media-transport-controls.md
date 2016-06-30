---
author: drewbatgit
ms.assetid: EFCF84D0-2F4C-454D-97DA-249E9EAA806C
description: "A classe SystemMediaTransportControls permite que seu aplicativo use os controles de transporte de mídia do sistema que estão integrados ao Windows e atualize os metadados que os controles exibem sobre a mídia que seu aplicativo está reproduzindo atualmente."
title: "Controles de transporte de mídia do sistema"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 5a94ce4112f7662d3fe9bf3c8a7d3f60b1569931

---

# Controles de transporte de mídia do sistema

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


A classe [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) permite que seu aplicativo use os controles de transporte de mídia do sistema que estão integrados ao Windows e atualize os metadados que os controles exibem sobre a mídia que seu aplicativo está reproduzindo atualmente.

Os controles de transporte do sistema são diferentes dos controles de transporte no objeto [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926). Esses controles de transporte aparecem quando você pressiona teclas de mídia no hardware, como o controle de volume nos fones de ouvido ou os botões de mídia nos teclados. Se o usuário pressionar a tecla de pausa em um teclado e seu aplicativo der suporte a [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677), seu aplicativo será notificado e você poderá tomar a medida apropriada.

Seu aplicativo também pode atualizar as informações de mídia, como o título da música e a imagem em miniatura mostrada pelos [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677).

**Observação**  
A [Amostra UWP de controles de transporte de mídia do sistema](http://go.microsoft.com/fwlink/?LinkId=619488) implementa o código abordado nesta visão geral. Você pode baixar a amostra para ver o código usado no contexto ou usá-lo como ponto de partida para seu próprio aplicativo.

## Configurar os controles de transporte

No arquivo XAML da página, defina um [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) que será controlado pelos controles de transporte de mídia do sistema. Os eventos [**CurrentStateChanged**](https://msdn.microsoft.com/library/windows/apps/br227375) e [**MediaOpened**](https://msdn.microsoft.com/library/windows/apps/br227394) são usados para atualizar os controles de transporte de mídia do sistema e serão abordados neste artigo.

[!code-xml[MediaElementSystemMediaTransportControls](./code/SMTCWin10/cs/MainPage.xaml#SnippetMediaElementSystemMediaTransportControls)]

Adicione um botão ao arquivo XAML que permita que o usuário selecione um arquivo para reproduzir.

[!code-xml[OpenButton](./code/SMTCWin10/cs/MainPage.xaml#SnippetOpenButton)]

Em sua página code-behind, adicione diretivas de uso aos namespaces a seguir.

[!code-cs[Namespace](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetNamespace)]

Adicione um manipulador de clique de botão que use um [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) para permitir que o usuário selecione um arquivo e chame [**SetSource**](https://msdn.microsoft.com/library/windows/apps/br244338) para torná-lo o arquivo ativo para o **MediaElement**.

[!code-cs[OpenMediaFile](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetOpenMediaFile)]

Obtenha uma instância dos [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) chamando [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708).

Ative os botões que seu aplicativo usará configurando a propriedade "is enabled" correspondente do objeto **SystemMediaTransportControls**, como [**IsPlayEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278714), [**IsPauseEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278713), [**IsNextEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278712) e [**IsPreviousEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278715). Veja uma lista completa de controles disponíveis na documentação de referência aos **SystemMediaTransportControls**.

Registre um manipulador para o evento [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706) para receber notificações quando o usuário pressionar um botão.

[!code-cs[SystemMediaTransportControlsSetup](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsSetup)]

## Manipular pressionamentos de botões de controles de transporte de mídia do sistema

O evento [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706) é acionado pelos controles de transporte do sistema quando um dos botões ativados é pressionado. A propriedade [**Button**](https://msdn.microsoft.com/library/windows/apps/dn278685) dos [**SystemMediaTransportControlsButtonPressedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn278683) passados para o manipulador de eventos é membro da enumeração [**SystemMediaTransportControlsButton**](https://msdn.microsoft.com/library/windows/apps/dn278681) que indica quais dos botões ativados foram pressionados.

Para atualizar objetos no thread da interface do usuário no manipulador de eventos [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706), como um objeto [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926), você precisa realizar marshaling das chamadas por meio de [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211). Isso ocorre porque o manipulador de eventos **ButtonPressed** não é chamado a partir do thread da interface do usuário e, portanto, uma exceção será gerada se você tentar modificar diretamente a interface do usuário.

[!code-cs[SystemMediaTransportControlsButtonPressed](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsButtonPressed)]

## Atualizar os controles de transporte de mídia do sistema com o status atual da mídia

Você deve notificar os [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) quando o estado da mídia mudar para que o sistema possa atualizar os controles para refletir o estado atual. Para fazer isso, configure a propriedade [**PlaybackStatus**](https://msdn.microsoft.com/library/windows/apps/dn278719) com o valor [**MediaPlaybackStatus**](https://msdn.microsoft.com/library/windows/apps/dn278665) apropriado no evento [**CurrentStateChanged**](https://msdn.microsoft.com/library/windows/apps/br227375) do [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926), que é lançado quando o estado da mídia muda.

[!code-cs[SystemMediaTransportControlsStateChange](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsStateChange)]

## Atualizar os controles de transporte de mídia do sistema com miniaturas e informações de mídia

Use a classe [**SystemMediaTransportControlsDisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278686) para atualizar as informações da mídia mostradas pelos controles de transporte, como o título da música ou a capa do álbum do item de mídia atualmente em reprodução. Obtenha uma instância dessa classe com a propriedade [**SystemMediaTransportControls.DisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278707). Para cenários típicos, a maneira recomendada de passar os metadados é chamar [**CopyFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn278694), passando o arquivo de mídia atualmente em execução. O atualizador de exibição extrairá automaticamente os metadados e a imagem em miniatura do arquivo.

Chame [**Update**](https://msdn.microsoft.com/library/windows/apps/dn278701) para fazer com que os controles de transporte de mídia do sistema atualizem sua interface do usuário com os novos metadados e a miniatura.

[!code-cs[SystemMediaTransportControlsUpdater](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsUpdater)]

Se seu cenário exigir, você poderá atualizar os metadados exibidos pelos controles de transporte de mídia do sistema manualmente configurando os valores dos objetos [**MusicProperties**](https://msdn.microsoft.com/library/windows/apps/dn278696), [**ImageProperties**](https://msdn.microsoft.com/library/windows/apps/dn278695) ou [**VideoProperties**](https://msdn.microsoft.com/library/windows/apps/dn278702) expostos pela classe [**DisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278707).

[!code-cs[SystemMediaTransportControlsUpdaterManual](./code/SMTCWin10/cs/MainPage.xaml.cs#SystemMediaTransportControlsUpdaterManual)]

## Atualizar as propriedades de linha do tempo dos controles de transporte de mídia do sistema

Os controles de transporte do sistema exibem informações sobre a linha do tempo do item de mídia atualmente em execução, incluindo a posição de reprodução atual, a hora de início e a hora de término do item de mídia. Para atualizar as propriedades de linha do tempo dos controles de transporte do sistema, crie um novo objeto [**SystemMediaTransportControlsTimelineProperties**](https://msdn.microsoft.com/library/windows/apps/mt218746). Configure as propriedades do objeto para refletir o estado atual do item de mídia em reprodução. Chame [**SystemMediaTransportControls.UpdateTimelineProperties**](https://msdn.microsoft.com/library/windows/apps/mt218760) para fazer com que os controles atualizem a linha do tempo.

[!code-cs[UpdateTimelineProperties](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetUpdateTimelineProperties)]

-   Você deve fornecer um valor para [**StartTime**](https://msdn.microsoft.com/library/windows/apps/mt218751), [**EndTime**](https://msdn.microsoft.com/library/windows/apps/mt218747) e [**Position**](https://msdn.microsoft.com/library/windows/apps/mt218755) para que os controles do sistema mostrem uma linha do tempo para o item em execução.

-   [
              **MinSeekTime**
            ](https://msdn.microsoft.com/library/windows/apps/mt218749) e [**MaxSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218748) permitem que você especifique o intervalo na linha do tempo que o usuário pode buscar. Um cenário típico para isso é permitir que os provedores de conteúdo incluam pausas para anúncios em suas mídias.

    Você deve configurar [**MinSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218749) e [**MaxSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218748) para que a [**PositionChangeRequest**](https://msdn.microsoft.com/library/windows/apps/mt218755) seja lançada.

-   É recomendável manter os controles do sistema em sincronia com a reprodução da mídia, atualizando essas propriedades aproximadamente a cada 5 segundos durante a reprodução e novamente sempre que o estado da reprodução mudar, por exemplo, ao pausar ou buscar uma nova posição.

## Responder às alterações de propriedade do player

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

## Usar os controles de transporte de mídia do sistema para áudio em segundo plano

Para usar os controles de transporte de mídia do sistema para áudio em segundo plano, você deve ativar os botões de reprodução e pausa configurando [**IsPlayEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278714) e [**IsPauseEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278713) como true. O aplicativo também deve manipular o evento [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706).

Para obter uma instância dos [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) a partir da tarefa em segundo plano do aplicativo, você deve usar [**BackgroundMediaPlayer.Current.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn926635), em vez de [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708), que só pode ser usado em seu aplicativo em primeiro plano.

Para obter informações sobre como reproduzir áudio em segundo plano, consulte [Áudio em segundo plano](background-audio.md).

 

 







<!--HONumber=Jun16_HO4-->


