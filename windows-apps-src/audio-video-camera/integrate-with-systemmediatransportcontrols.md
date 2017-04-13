---
author: drewbatgit
ms.assetid: eb690f2b-3bf8-4a65-99a4-2a3a8c7760b7
description: "Este artigo mostra como interagir com os controles de transporte de mídia do sistema."
title: "Integrar aos Controles de transporte de mídia do sistema"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: ca8910129ba0993597be905a24fb5b2c5c4061a4
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="integrate-with-the-system-media-transport-controls"></a>Integrar aos Controles de transporte de mídia do sistema

Este artigo mostra como interagir com os controles de transporte de mídia do sistema (SMTC). O SMTC é um conjunto de controles que são comuns em todos os dispositivos Windows 10 e que oferecem aos usuários uma maneira consistente de controlar a reprodução de mídia para todos os aplicativos em execução que usam o [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) para reprodução.

Para ver um exemplo completo que demonstra a integração com o SMTC, consulte o [exemplo de controles de transporte de mídia do sistema no github](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls).
                    
##<a name="automatic-integration-with-smtc"></a>Integração automática com o SMTC
A partir do Windows 10, versão 1607, os aplicativos UWP que usam a classe [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) para reproduzir mídia são integrados automaticamente com o SMTC por padrão. Basta criar uma nova instância de **MediaPlayer** e atribuir [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource), [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) ou [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList) à propriedade [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) para o usuário ver o nome de seu aplicativo no SMTC e poder reproduzir, pausar e percorrer suas playlists usando os controles SMTC. 

Seu aplicativo pode criar e usar vários objetos **MediaPlayer** ao mesmo tempo. Para cada instância **MediaPlayer** ativa em seu aplicativo, uma guia separada é criada no SMTC, permitindo que o usuário alterne entre os players de mídia ativos e os outros aplicativos em execução. O player de mídia que estiver selecionado no momento no SMTC será o afetado pelos controles.

Para obter mais informações sobre como usar **MediaPlayer** em seu aplicativo, inclusive como associá-lo a um [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) na página XAML, consulte [Reproduzir áudio e vídeo com o MediaPlayer](play-audio-and-video-with-mediaplayer.md). 

Para obter mais informações sobre como trabalhar com **MediaSource**, **MediaPlaybackItem** e **MediaPlaybackList**, consulte [Itens de mídia, playlists e faixas](media-playback-with-mediasource.md).

##<a name="add-metadata-to-be-displayed-by-the-smtc"></a>Adicionar metadados a serem exibidos pelo SMTC
Se quiser adicionar ou modificar os metadados que são exibidos para seus itens de mídia no SMTC, como um título de vídeo ou música, você precisará atualizar as propriedades de exibição do **MediaPlaybackItem** que representa o item de mídia. Primeiro, obtenha uma referência para o objeto [**MediaItemDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties) chamando [**GetDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.GetDisplayProperties). Em seguida, defina o tipo de mídia, música ou vídeo, para o item com a propriedade [**Type**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.Type). Depois, você pode preencher os campos de [**MusicProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.MusicProperties) ou [**VideoProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.VideoProperties), dependendo do tipo de mídia especificado. Por fim, atualize os metadados para o item de mídia chamando [**ApplyDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/mt489923).

[!code-cs[SetVideoProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetVideoProperties)]

[!code-cs[SetMusicProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetMusicProperties)]

##<a name="use-commandmanager-to-modify-or-override-the-default-smtc-commands"></a>Usar CommandManager para modificar ou substituir os comandos SMTC padrão
Seu aplicativo pode modificar ou substituir completamente o comportamento dos controles SMTC com a classe [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager). Uma instância do gerenciador de comando pode ser obtida para cada instância da classe **MediaPlayer** acessando a propriedade [**CommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.CommandManager).

Para cada comando, como o comando *Next* que, por padrão, passa para o próximo item de uma **MediaPlaybackList**, o gerenciador de comando expõe um evento recebido, como [**NextReceived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.NextReceived), e um objeto que gerencia o comportamento do comando, como [**NextBehavior**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.NextBehavior). 

O exemplo a seguir registra um manipulador para o evento **NextReceived** e para o evento [**IsEnabledChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior.IsEnabledChanged) do **NextBehavior**.

[!code-cs[AddNextHandler](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetAddNextHandler)]

O exemplo a seguir ilustra um cenário em que o aplicativo deseja desabilitar o comando *Next* depois que o usuário clicar em cinco itens da playlist, talvez exigindo alguma interação do usuário antes de continuar a reproduzir o conteúdo. Cada vez que o evento **NextReceived** é gerado, um contador é incrementado. Depois que o contador atingir o número de destino, [**EnablingRule**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior.EnablingRule) para o comando *Next* será definido como [**Never**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaCommandEnablingRule), o que desabilita o comando. 

[!code-cs[NextReceived](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetNextReceived)]

Você também pode definir o comando como **Always**, que significa que o comando sempre será habilitado mesmo que, para o exemplo de comando *Next*, não haja nenhuma mais itens na playlist. Ou você pode definir o comando como **Auto**, onde o sistema determina se o comando deve ser habilitado com base no conteúdo atual em reprodução.

Para o cenário descrito acima, em algum momento o aplicativo vai querer reabilitar o comando *Next* e fará isso definindo **EnablingRule** como **Auto**.

[!code-cs[EnableNextButton](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetEnableNextButton)]

Como seu aplicativo pode ter a própria interface do usuário para controlar a reprodução enquanto estiver em primeiro plano, você pode usar os eventos [**IsEnabledChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior.IsEnabledChanged) para atualizar sua própria interface do usuário para corresponder ao SMTC à medida que os comandos são habilitados ou desabilitados acessando o [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior.IsEnabled) do [**MediaPlaybackCommandManagerCommandBehavior**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior) passado para o manipulador.

[!code-cs[IsEnabledChanged](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetIsEnabledChanged)]

Em alguns casos, convém substituir completamente o comportamento de um comando SMTC. O exemplo a seguir ilustra um cenário em que um aplicativo usa os comandos *Next* e *Previous* para alternar entre estações de rádio da internet em vez de pular faixas da playlist atual. Como no exemplo anterior, um manipulador é registrado quando um comando é recebido, neste caso é o evento [**PreviousReceived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.PreviousReceived).

[!code-cs[AddPreviousHandler](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetAddPreviousHandler)]

No manipulador **PreviousReceived**, primeiro um [**Deferral**](https://msdn.microsoft.com/library/windows/apps/Windows.Foundation.Deferral) é obtido chamando o [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerPreviousReceivedEventArgs.GetDeferral) do [**MediaPlaybackCommandManagerPreviousReceivedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerPreviousReceivedEventArgs) passado para o manipulador. Isso diz para o sistema aguardar até que o adiamento seja concluído antes de executar o comando. Isso é extremamente importante se você pretende fazer chamadas assíncronas no manipulador. Neste ponto, o exemplo chama um método personalizado que retorna um **MediaPlaybackItem** que representa a estação de rádio anterior.

Em seguida, a propriedade [**Handled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerPreviousReceivedEventArgs.Handled) é verificada para garantir que o evento ainda não tenha sido manipulado por outro manipulador. Caso contrário, a propriedade **Handled** é definida como true. Isso permite que o SMTC e outros manipuladores inscritos saibam que não devem tomar nenhuma medida ao executar esse comando, pois ele já foi manipulado. Em seguida, o código define a nova fonte para o media player e inicia o player.

Por fim, [**Complete**](https://msdn.microsoft.com/library/windows/apps/Windows.Foundation.Deferral.Complete) é chamado no objeto de adiamento para informar ao sistema que você terminou de processar o comando.

[!code-cs[PreviousReceived](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetPreviousReceived)]
                 
##<a name="manual-control-of-the-smtc"></a>Controle manual do SMTC
Como mencionado anteriormente neste artigo, o SMTC detectará e exibirá automaticamente informações de cada instância de **MediaPlayer** que seu aplicativo criar. Se você quiser usar várias instâncias de **MediaPlayer**, mas quiser que o SMTC forneça uma única entrada para seu aplicativo, deverá controlar manualmente o comportamento do SMTC em vez de depender da integração automática. Além disso, se você estiver usando [**MediaTimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController) para controlar um ou mais media players, deverá usar a integração do SMTC manual. Além disso, se seu aplicativo usa uma API diferente de **MediaPlayer**, como a classe [**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioGraph), para reproduzir mídia, você deve implementar a integração do SMTC manual para o usuário usar o SMTC para controlar seu aplicativo. Para obter informações sobre como controlar o SMTC manualmente, consulte [Controle manual dos controles de transporte de mídia do sistema](system-media-transport-controls.md).



## <a name="related-topics"></a>Tópicos relacionados
* [Reprodução de mídia](media-playback.md)
* [Reproduzir áudio e vídeo com o MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Controle manual dos controles de transporte de mídia do sistema](system-media-transport-controls.md)
* [Exemplo de controles de transporte de mídia do sistema no github](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls)
 

 




