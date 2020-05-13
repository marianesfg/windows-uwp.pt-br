---
description: Este artigo mostra como usar o AudioPlaybackConnection para habilitar dispositivos remotos conectados por Bluetooth para reproduzir áudio no computador local.
title: Habilitar reprodução de áudio de dispositivos conectados remotamente Bluetooth
ms.date: 05/03/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f03fc963e533ff29d49c326611c45437baa14f6c
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234949"
---
# <a name="enable-audio-playback-from-remote-bluetooth-connected-devices"></a>Habilitar reprodução de áudio de dispositivos conectados remotamente Bluetooth

Este artigo mostra como usar o [AudioPlaybackConnection](/uwp/api/windows.media.audio.audioplaybackconnection) para habilitar dispositivos remotos conectados por Bluetooth para reproduzir áudio no computador local.

A partir do Windows 10, as fontes de áudio remotas da versão 2004 podem transmitir áudio para dispositivos Windows, permitindo cenários como configurar um PC para se comportar como um palestrante Bluetooth e permitir que os usuários escutem áudio de seu telefone. A implementação usa os componentes Bluetooth no sistema operacional para processar os dados de entrada de áudio e reproduzi-los nos pontos de extremidade de áudio do sistema, como palestrantes de PC internos ou fones de ouvido com fio. A habilitação do coletor de A2DP Bluetooth subjacente é gerenciada por aplicativos, que são responsáveis pelo cenário do usuário final, e não pelo sistema.

A classe [AudioPlaybackConnection](/uwp/api/windows.media.audio.audioplaybackconnection) é usada para habilitar e desabilitar conexões de um dispositivo remoto, bem como para criar a conexão, permitindo que a reprodução de áudio remoto seja iniciada.

## <a name="add-a-user-interface"></a>Adicionar uma interface do usuário

Para os exemplos neste artigo, usaremos a seguinte interface do usuário XAML simples que define o controle **ListView** para exibir os dispositivos remotos disponíveis, um **TextBlock** para exibir o status da conexão e três botões para habilitar, desabilitar e abrir conexões.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml" id="snippet_AudioPlaybackConnectionXAML":::

## <a name="use-devicewatcher-to-monitor-for-remote-devices"></a>Usar o DeviceWatcher para monitorar dispositivos remotos

A classe [DeviceWatcher](/uwp/api/windows.devices.enumeration.devicewatcher) permite que você detecte dispositivos conectados. O método [AudioPlaybackConnection. GetDeviceSelector](/uwp/api/windows.media.audio.audioplaybackconnection.getdeviceselector) retorna uma cadeia de caracteres que informa ao inspetor de dispositivo quais tipos de dispositivos observar. Passe essa cadeia de caracteres para o construtor **DeviceWatcher** . 

O evento [DeviceWatcher. Added](/uwp/api/windows.devices.enumeration.devicewatcher.added) é gerado para cada dispositivo conectado quando o Inspetor de dispositivo é iniciado, bem como para qualquer dispositivo conectado enquanto o Inspetor de dispositivo está em execução. O evento [DeviceWatcher. removida](/uwp/api/windows.devices.enumeration.devicewatcher.removed) será gerado se um dispositivo conectado anteriormente for desconectado. 

Chame [DeviceWatcher. Start](/uwp/api/windows.devices.enumeration.devicewatcher.start) para começar a observar dispositivos conectados que dão suporte a conexões de reprodução de áudio. Neste exemplo, iniciaremos o Gerenciador de dispositivos quando o controle de **grade** principal na interface do usuário for carregado. Para obter mais informações sobre como usar o **DeviceWatcher**, consulte [enumerar dispositivos](/windows/uwp/devices-sensors/enumerate-devices).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_MainGridLoaded":::


No evento **adicionado** do Inspetor de dispositivo, cada dispositivo descoberto é representado por um objeto [DeviceInformation](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) . Adicione cada dispositivo descoberto a uma coleção observável associada ao controle **ListView** na interface do usuário.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_DeclareDevices":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_DeviceWatcher_Added":::


## <a name="enable-and-release-audio-playback-connections"></a>Habilitar e liberar conexões de reprodução de áudio

Antes de abrir uma conexão com um dispositivo, a conexão deve ser habilitada. Isso informa ao sistema que há um novo aplicativo que deseja que o áudio do dispositivo remoto seja reproduzido no PC, mas o áudio não começa a ser reproduzido até que a conexão seja aberta, o que é mostrado em uma etapa posterior.

No manipulador de cliques do botão **habilitar conexão de reprodução de áudio** , obtenha a ID do dispositivo associada ao dispositivo selecionado no momento no controle **ListView** . Este exemplo mantém um dicionário de objetos **AudioPlaybackConnection** que foram habilitados. Esse método verifica primeiro se já existe uma entrada no dicionário para o dispositivo selecionado. Em seguida, o método tenta criar um **AudioPlaybackConnection** para o dispositivo selecionado chamando [TryCreateFromId](/uwp/api/windows.media.audio.audioplaybackconnection.trycreatefromid) e passando a ID do dispositivo selecionado. 

Se a conexão for criada com êxito, adicione o novo objeto **AudioPlaybackConnection** ao dicionário do aplicativo, registre um manipulador para o evento [estadochanged](/uwp/api/windows.media.audio.audioplaybackconnection.statechanged) do objeto e chame[StartAsync](/uwp/api/windows.media.audio.audioplaybackconnection.startasync) para notificar o sistema de que a nova conexão está habilitada. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_DeclareConnections":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_EnableAudioPlaybackConnection":::


## <a name="open-the-audio-playback-connection"></a>Abrir a conexão de reprodução de áudio

Na etapa anterior, uma conexão de reprodução de áudio foi criada, mas o som não começa a ser executado até que a conexão seja aberta chamando [Open](/uwp/api/windows.media.audio.audioplaybackconnection.open) ou [OpenAsync](/uwp/api/windows.media.audio.audioplaybackconnection.openasync). No botão **abrir conexão de reprodução de áudio** , clique no manipulador, obtenha o dispositivo selecionado no momento e use a ID para recuperar o **AudioPlaybackConnection** do dicionário de conexões do aplicativo. Aguardar uma chamada para **OpenAsync** e verificar o valor de **status** do objeto [AudioPlaybackConnectionOpenResultStatus](/uwp/api/windows.media.audio.audioplaybackconnectionopenresult) retornado para ver se a conexão foi aberta com êxito e, nesse caso, atualizar a caixa de texto estado da conexão.


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_OpenAudioPlaybackConnectionButton":::

## <a name="monitor-audio-playback-connection-state"></a>Monitorar o estado da conexão de reprodução de áudio

O evento [AudioPlaybackConnection. ConnectionStateChanged](/uwp/api/windows.media.audio.audioplaybackconnection.statechanged) é gerado sempre que o estado da conexão é alterado. Neste exemplo, o manipulador para esse evento atualiza a caixa de texto status. Lembre-se de atualizar a interface do usuário dentro de uma chamada para [Dispatcher. RunAsync](/uwp/api/windows.ui.core.coredispatcher.runasync) para certificar-se de que a atualização seja feita no thread da interface do usuário.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_ConnectionStateChanged":::

## <a name="release-connections-and-handle-removed-devices"></a>Liberar conexões e manipular dispositivos removidos

Este exemplo fornece um botão **liberar conexão de reprodução de áudio** para permitir que o usuário libere uma conexão de reprodução de áudio. No manipulador deste evento, obtemos o dispositivo selecionado no momento e usamos a ID do dispositivo para pesquisar o **AudioPlaybackConnection** no dicionário. Chame **Dispose** para liberar a referência e liberar todos os recursos associados e remover a conexão do dicionário.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_ReleaseAudioPlaybackConnectionButton":::

Você deve lidar com o caso em que um dispositivo é removido enquanto uma conexão está habilitada ou aberta. Para fazer isso, implemente um manipulador para o evento [DeviceWatcher. removida](/uwp/api/windows.devices.enumeration.devicewatcher.removed) do Inspetor de dispositivo. Primeiro, a ID do dispositivo removido é usada para remover o dispositivo da coleção observável associada ao controle **ListView** do aplicativo. Em seguida, se uma conexão associada a esse dispositivo estiver no dicionário do aplicativo, **Dispose** será chamado para liberar os recursos associados e, em seguida, a conexão será removida do dicionário. Tudo isso é feito em uma chamada para **Dispatcher. RunAsync** para garantir que as atualizações da interface do usuário sejam executadas no thread da interface do usuário.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_DeviceWatcher_Removed":::

## <a name="related-topics"></a>Tópicos relacionados

[Reprodução de mídia](media-playback.md)


 




