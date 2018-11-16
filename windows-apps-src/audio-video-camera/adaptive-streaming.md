---
author: drewbatgit
ms.assetid: AE98C22B-A071-4206-ABBB-C0F0FB7EF33C
description: Este artigo descreve como adicionar a reprodução de conteúdo multimídia de streaming adaptável ao aplicativo da Plataforma Universal do Windows (UWP). Atualmente, esse recurso oferece suporte à reprodução de conteúdo HLS (Http Live Streaming) e DASH (Dynamic Streaming over HTTP).
title: Streaming adaptável
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ef8e3ab4abd9ee9159dc7d5aa757f55e00817a51
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6985980"
---
# <a name="adaptive-streaming"></a>Streaming adaptável


Este artigo descreve como adicionar a reprodução de conteúdo multimídia de streaming adaptável ao aplicativo da Plataforma Universal do Windows (UWP). Esse recurso oferece suporte à reprodução de conteúdo HLS (Http Live Streaming) e DASH (Dynamic Streaming over HTTP). A partir do Windows 10, versão 1803, o Smooth Streaming é suportado por **[AdaptiveMediaSource](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)**.

Para obter uma lista de marcas de protocolo HLS com suporte, consulte [Suporte à marca de HLS](hls-tag-support.md). 

Para obter uma lista dos perfis DASH com suporte, veja [Suporte ao perfil DASH](dash-profile-support.md). 

> [!NOTE] 
> O código neste artigo foi adaptado do [Exemplo de streaming adaptável](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/AdaptiveStreaming) da UWP.

## <a name="simple-adaptive-streaming-with-mediaplayer-and-mediaplayerelement"></a>Streaming adaptável simples com MediaPlayer e MediaPlayerElement

Para reproduzir mídia de streaming adaptável em um aplicativo UWP, crie um objeto **Uri** apontando para um arquivo de manifesto DASH ou HLS. Crie uma instância da classe do [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer). Chame [**MediaSource.CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912) para criar um novo objeto **MediaSource** e, em seguida, defina-o como a propriedade [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) do **MediaPlayer**. Chame [**Reproduzir**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play) para iniciar a reprodução do conteúdo de mídia.

[!code-cs[DeclareMediaPlayer](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlayer)]

[!code-cs[ManifestSourceNoUI](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSourceNoUI)]

O exemplo acima reproduzirá o áudio do conteúdo de mídia, mas ele não renderizará automaticamente o conteúdo na interface do usuário. A maioria dos aplicativos que reproduz conteúdo de vídeo deverá renderizar o conteúdo em uma página XAML.  Para fazer isso, adicione um controle [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) à página XAML.

[!code-xml[MediaPlayerElementXAML](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

Chame [**MediaSource.CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912) para criar um **MediaSource** da URI de um arquivo de manifesto DASH ou HLS. Defina a propriedade [**Source**](https://msdn.microsoft.com/library/windows/apps/br227420) do **MediaPlayerElement**. O **MediaPlayerElement** criará automaticamente um novo objeto **MediaPlayer** para o conteúdo. Você pode chamar **Reproduzir** no **MediaPlayer** para iniciar a reprodução do conteúdo.

[!code-cs[ManifestSource](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSource)]

> [!NOTE] 
> A partir do Windows 10, versão 1607, é recomendável que você use a classe **MediaPlayer** para reproduzir itens de mídia. O **MediaPlayerElement** é um controle XAML leve que é usado para renderizar o conteúdo de um **MediaPlayer** em uma página XAML. O controle **MediaElement** continua a ser suportado para compatibilidade com versões anteriores. Para obter mais informações sobre como usar o **MediaPlayer** e o **MediaPlayerElement** para reproduzir conteúdo de mídia, consulte [Reproduzir áudio e vídeo com o MediaPlayer](play-audio-and-video-with-mediaplayer.md). Para obter informações sobre como usar o **MediaSource** e as APIs relacionadas para trabalhar com conteúdo de mídia, consulte [Itens de mídia, listas de reprodução e faixas](media-playback-with-mediasource.md).

## <a name="adaptive-streaming-with-adaptivemediasource"></a>Streaming adaptável com AdaptiveMediaSource

Se o seu aplicativo exige recursos de streaming adaptável mais avançados, como o fornecimento de cabeçalhos em HTTP personalizados, monitoramento da taxa de bits atual de download e reprodução ou ajuste de índices que determinam quando o sistema mudar a taxa de bits do stream adaptável, use o objeto **[AdaptiveMediaSource](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)**.

As APIs de streaming adaptáveis são encontradas no namespace [**Windows.Media.Streaming.Adaptive**](https://msdn.microsoft.com/library/windows/apps/dn931279). Os exemplos deste artigo usam APIs dos namespaces a seguir.

[!code-cs[AdaptiveStreamingUsing](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAdaptiveStreamingUsing)]

## <a name="initialize-an-adaptivemediasource-from-a-uri"></a>Inicialize um AdaptiveMediaSource de uma URI.

Inicialize o **AdaptiveMediaSource** com o URI de um arquivo de manifesto de streaming adaptável chamando o [**CreateFromUriAsync**](https://msdn.microsoft.com/library/windows/apps/dn931261). O valor [**AdaptiveMediaSourceCreationStatus**](https://msdn.microsoft.com/library/windows/apps/dn946917) devolvido desse método faz com que você saiba se a origem de mídia foi criada com sucesso. Dessa forma, você pode definir o objeto como a origem de stream para o seu **MediaPlayer** criando um objeto **MediaSource** chamando  [**MediaSource.CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource.AdaptiveMediaSource)e, em seguida, atribuí-lo à propriedade [**Origem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Source) do media player. Neste exemplo, a propriedade [**AvailableBitrates**](https://msdn.microsoft.com/library/windows/apps/dn931257) é consultada para determinar a taxa de bits máxima suportada para esse stream e, em seguida, tal valor é definido como a taxa de bits inicial. Este exemplo também registra manipuladores para vários eventos **AdaptiveMediaSource**, que serão explicados posteriormente neste artigo.

[!code-cs[InitializeAMS](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMS)]

## <a name="initialize-an-adaptivemediasource-using-httpclient"></a>Inicializar uma AdaptiveMediaSource usando HttpClient

Se precisar personalizar cabeçalhos em HTTP para obter o arquivo de manifesto, você pode criar um objeto [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639), definir os cabeçalhos desejados e, então, passar o objeto na sobrecarga do **CreateFromUriAsync**.

[!code-cs[InitializeAMSWithHttpClient](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMSWithHttpClient)]

O evento [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272) é levantado quando o sistema está prestes a recuperar um recurso do servidor. O [**AdaptiveMediaSourceDownloadRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn946935) passado no manipulador de eventos expõe propriedades que fornecem informações sobre o recurso ser solicitado, como o tipo e o URI do mesmo.

## <a name="modify-resource-request-properties-using-the-downloadrequested-event"></a>Modificar propriedades da solicitação de recurso usando o evento DownloadRequested

Você pode usar o manipulador de eventos **DownloadRequested** para modificar a solicitação de recurso atualizando as propriedades do objeto [**AdaptiveMediaSourceDownloadResult**](https://msdn.microsoft.com/library/windows/apps/dn946942) fornecido pelos argumentos de evento. No exemplo abaixo, o URI de onde o recurso será recuperado é modificado ao se atualizar as propriedades [**ResourceUri**](https://msdn.microsoft.com/library/windows/apps/dn931250) do objeto do resultado. Você também pode reescrever o deslocamento do intervalo de bytes e o comprimento para segmentos de mídia ou, como mostrado no exemplo abaixo, alterar o URI do recurso para baixar o recurso completo e definir o deslocamento do intervalo de bytes e o comprimento como nulo.

Você pode substituir o conteúdo do recurso de solicitação configurando as propriedades [**Buffer**](https://msdn.microsoft.com/library/windows/apps/dn946943) ou [**InputStream**](https://msdn.microsoft.com/library/windows/apps/dn931249) do objeto do resultado. No exemplo abaixo, os conteúdos do recurso de manifesto são substituídos definindo-se a propriedade **Buffer**. Observe que se você estiver adaptando a solicitação de recurso com dados obtidos de forma assíncrona, como recuperação de dados de um servidor remoto ou autenticação do usuário assíncrona, você deve chamar [**AdaptiveMediaSourceDownloadRequestedEventArgs.GetDeferral**](https://msdn.microsoft.com/library/windows/apps/dn946936) para obter um adiamento e, em seguida, chamar o [**Complete**](https://msdn.microsoft.com/library/windows/apps/dn946934) quando a operação estiver completa para sinalizar o sistema que a operação de solicitação de download pode continuar.

[!code-cs[AMSDownloadRequested](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadRequested)]

## <a name="use-bitrate-events-to-manage-and-respond-to-bitrate-changes"></a>Use eventos de taxa de bits para gerenciar e responder a alterações de taxa de bits

O objeto **AdaptiveMediaSource** fornece eventos que permitem que você reaja quando a taxa de bits de download ou reprodução muda. Neste exemplo, as taxas de bits atuais são simplesmente atualizadas na interface do usuário. Observe que você pode modificar os índices que determinam quando o sistema muda taxas de bits do stream adaptável. Para obter mais informações, consulte a propriedade [**AdvancedSettings**](https://msdn.microsoft.com/library/windows/apps/mt628697).

[!code-cs[AMSBitrateEvents](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSBitrateEvents)]

## <a name="handle-download-completion-and-failure-events"></a>Manipular eventos de falha e de conclusão do download
O objeto **AdaptiveMediaSource** gera o evento [**DownloadFailed**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource.DownloadFailed) quando ocorre falha no download de um recurso solicitado. Você pode usar esse evento para atualizar sua interface do usuário em resposta à falha. Você também pode usar o evento para registrar em log informações estatísticas sobre a operação de download e a falha. 

O objeto [**AdaptiveMediaSourceDownloadFailedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs) passado para o manipulador de eventos contém metadados sobre o download de recurso que falhou, como o tipo de recurso, o URI do recurso e a posição dentro do streaming onde a falha ocorreu. A [**RequestId**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs.RequestId) obtém um identificador exclusivo gerado pelo sistema para a solicitação que pode ser usado para correlacionar as informações de status sobre uma solicitação individual entre vários eventos.

A propriedade [**Statistics**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs.Statistics) retorna um objeto [**AdaptiveMediaSourceDownloadStatistics**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadstatistics) que fornece informações detalhadas sobre o número de bytes recebidos no momento do evento e sobre o intervalo de diversos marcos na operação de download. Você pode registrar essas informações para identificar problemas de desempenho em sua implementação de streaming adaptável.

[!code-cs[AMSDownloadFailed](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadFailed)]


O evento [**DownloadCompleted**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource.DownloadCompleted) ocorre quando um download de recurso é concluído e fornece dados semelhantes para o evento **DownloadFailed**. Mais uma vez, uma **RequestId** é fornecida para correlacionar eventos para uma única solicitação. Além disso, um objeto **AdaptiveMediaSourceDownloadStatistics** é fornecido para permitir o registro em log de estatísticas de download.

[!code-cs[AMSDownloadCompleted](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadCompleted)]

## <a name="gather-adaptive-streaming-telemetry-data-with-adaptivemediasourcediagnostics"></a>Coletar dados de telemetria de streaming adaptável com AdaptiveMediaSourceDiagnostics
A **AdaptiveMediaSource** expõe uma propriedade [**Diagnostics**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource?branch=master.Diagnostics) que retorna um objeto [**AdaptiveMediaSourceDiagnostics**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostics). Use esse objeto para registrar o evento [**DiagnosticAvailable**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostics.DiagnosticAvailable). Esse evento se destina a ser usado para a coleta de telemetria e não deve ser usado para modificar o comportamento do app em tempo de execução. Esse evento de diagnóstico é gerado por diversos motivos. Verifique a propriedade [**DiagnosticType**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs.DiagnosticType) do objeto [**AdaptiveMediaSourceDiagnosticAvailableEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs) passado para o evento para determinar o motivo pelo qual o evento foi gerado. Os possíveis motivos incluem erros de acesso ao recurso solicitado e ao analisar o arquivo de manifesto de streaming. Para obter uma lista das situações que podem disparar um evento de diagnóstico, veja [**AdaptiveMediaSourceDiagnosticType**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostictype). Assim como os argumentos para outros eventos de streaming adaptáveis, **AdaptiveMediaSourceDiagnosticAvailableEventArgs** fornece uma propriedade **RequestId** para correlacionar as informações de solicitação entre eventos diferentes.

[!code-cs[AMSDiagnosticAvailable](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDiagnosticAvailable)]

## <a name="defer-binding-of-adaptive-streaming-content-for-items-in-a-playback-list-by-using-mediabinder"></a>Adiar a associação de conteúdo de streaming adaptável para itens em uma playlist usando o MediaBinder
A classe [**MediaBinder**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder) permite que você adie a associação de conteúdo de mídia em uma [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955). A partir do Windows 10, versão 1703, você pode fornecer uma [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) como conteúdo associado. O processo de associação adiada de uma origem de mídia adaptável é basicamente igual ao da associação de outros tipos de mídia, descrita em [Itens de mídia, playlists e faixas](media-playback-with-mediasource.md). 

Crie uma instância de **MediaBinder**, defina uma cadeia de caracteres [**Token**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Token) definida pelo app para identificar o conteúdo como associado e inscreva-se no evento [**Binding**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Binding). Crie uma **MediaSource** desde o **Binder** ao chamar [**MediaSource.CreateFromMediaBinder**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediabinder). Em seguida, crie um **MediaPlaybackItem** da **MediaSource** e o adicione à playlist.

[!code-cs[InitMediaBinder](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetInitMediaBinder)]

No manipulador de eventos **Binding**, use a cadeia de caracteres de token para identificar o conteúdo a ser associado e então crie a origem de mídia adaptável ao chamar uma das sobrecargas de **[CreateFromStreamAsync](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromstreamasync)** ou **[CreateFromUriAsync](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromuriasync)**. Como eles são métodos assíncronos, primeiro você deve chamar o método [**MediaBindingEventArgs.GetDeferral**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.GetDeferral) para instruir o sistema a aguardar até que a operação seja concluída antes de continuar.  Defina a origem de mídia adaptável como o conteúdo associado ao chamar **[SetAdaptiveMediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setadaptivemediasource)**. Por fim, chame [**Deferral.Complete**](https://docs.microsoft.com/uwp/api/windows.foundation.deferral.Complete) depois que a operação estiver concluída para instruir o sistema a continuar.

[!code-cs[BinderBindingAMS](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetBinderBindingAMS)]

Se você deseja registrar manipuladores de eventos para a origem de mídia adaptável associada, pode fazer isso no manipulador para o evento [**CurrentItemChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacklist.CurrentItemChanged) da **MediaPlaybackList**. A propriedade [**CurrentMediaPlaybackItemChangedEventArgs.NewItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.NewItem) contém o novo **MediaPlaybackItem** que está sendo reproduzido no momento na lista. Obtenha uma instância da **AdaptiveMediaSource** que representa o novo item ao acessar a propriedade [**Source**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem.Source) do **MediaPlaybackItem** e então a propriedade [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.AdaptiveMediaSource) da origem de mídia. Essa propriedade será nula se o novo item de reprodução não for uma **AdaptiveMediaSource** e, portanto, você deve testar se ela é nula antes de tentar registrar manipuladores para qualquer um dos eventos do objeto.

[!code-cs[AMSBindingCurrentItemChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAMSBindingCurrentItemChanged)]

## <a name="related-topics"></a>Tópicos relacionados
* [Reprodução de mídia](media-playback.md)
* [Suporte para a marca HLS](hls-tag-support.md) 
* [Suporte a perfil Dash](dash-profile-support.md) 
* [Reproduzir áudio e vídeo com o MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Reproduzir mídia em segundo plano](background-audio.md) 





