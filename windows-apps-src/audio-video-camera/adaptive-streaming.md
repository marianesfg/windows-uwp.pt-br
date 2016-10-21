---
author: drewbatgit
ms.assetid: AE98C22B-A071-4206-ABBB-C0F0FB7EF33C
description: "Este artigo descreve como adicionar a reprodução de conteúdo multimídia de streaming adaptável ao aplicativo da Plataforma Universal do Windows (UWP). Atualmente, esse recurso oferece suporte à reprodução de conteúdo HLS (Http Live Streaming) e DASH (Dynamic Streaming over HTTP)."
title: "Streaming adaptável"
translationtype: Human Translation
ms.sourcegitcommit: d0941887ebc17f3665302fae6c7b0a124dfb5a0b
ms.openlocfilehash: 431fa345c0135a08c1da68904a8d58d969490a8d

---

# Streaming adaptável

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este artigo descreve como adicionar a reprodução de conteúdo multimídia de streaming adaptável ao aplicativo da Plataforma Universal do Windows (UWP). Atualmente, esse recurso oferece suporte à reprodução de conteúdo HLS (Http Live Streaming) e DASH (Dynamic Streaming over HTTP).

Para obter uma lista de marcas de protocolo HLS com suporte, consulte [Suporte à marca de HLS](hls-tag-support.md). 

> [!NOTE] 
> O código neste artigo foi adaptado da [Amostra de Streaming adaptável](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/AdaptiveStreaming) da UWP.

## Streaming adaptável simples com MediaPlayer e MediaPlayerElement

Para reproduzir mídia de streaming adaptável em um aplicativo UWP, crie um objeto **Uri** apontando para um arquivo de manifesto DASH ou HLS. Crie uma instância da classe do [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer). Chame [**MediaSource.CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912) para criar um novo objeto **MediaSource** e, em seguida, defina-o como a propriedade [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) do **MediaPlayer**. Chame [**Reproduzir**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play) para iniciar a reprodução do conteúdo de mídia.

[!code-cs[DeclareMediaPlayer](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlayer)]

[!code-cs[ManifestSourceNoUI](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSourceNoUI)]

O exemplo acima reproduzirá o áudio do conteúdo de mídia, mas ele não renderizará automaticamente o conteúdo na interface do usuário. A maioria dos aplicativos que reproduz conteúdo de vídeo deverá renderizar o conteúdo em uma página XAML.  Para fazer isso, adicione um controle [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) à página XAML.

[!code-xml[MediaPlayerElementXAML](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

Chame [**MediaSource.CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912) para criar um **MediaSource** da URI de um arquivo de manifesto DASH ou HLS. Defina a propriedade [**Source**](https://msdn.microsoft.com/library/windows/apps/br227420) do **MediaPlayerElement**. O **MediaPlayerElement** criará automaticamente um novo objeto **MediaPlayer** para o conteúdo. Você pode chamar **Reproduzir** no **MediaPlayer** para iniciar a reprodução do conteúdo.

[!code-cs[ManifestSource](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSource)]

> [!NOTE] 
> A partir do Windows 10, versão 1607, é recomendável que você use a classe **MediaPlayer** para reproduzir itens de mídia. O **MediaPlayerElement** é um controle XAML leve que é usado para renderizar o conteúdo de um **MediaPlayer** em uma página XAML. O controle **MediaElement** continua a ser suportado para compatibilidade com versões anteriores. Para obter mais informações sobre como usar o **MediaPlayer** e o **MediaPlayerElement** para reproduzir conteúdo de mídia, consulte [Reproduzir áudio e vídeo com o MediaPlayer](play-audio-and-video-with-mediaplayer.md). Para obter informações sobre como usar o **MediaSource** e as APIs relacionadas para trabalhar com conteúdo de mídia, consulte [Itens de mídia, listas de reprodução e faixas](media-playback-with-mediasource.md).

## Streaming adaptável com AdaptiveMediaSource

Se o seu aplicativo exige recursos de streaming adaptável mais avançados, como o fornecimento de cabeçalhos em HTTP personalizados, monitoramento da taxa de bits atual de download e reprodução ou ajuste de índices que determinam quando o sistema mudar a taxa de bits do stream adaptável, use o objeto [**AdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn946912).

As APIs de streaming adaptáveis são encontradas no namespace [**Windows.Media.Streaming.Adaptive**](https://msdn.microsoft.com/library/windows/apps/dn931279).

[!code-cs[AdaptiveStreamingUsing](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAdaptiveStreamingUsing)]

Inicialize o **AdaptiveMediaSource** com o URI de um arquivo de manifesto de streaming adaptável chamando o [**CreateFromUriAsync**](https://msdn.microsoft.com/library/windows/apps/dn931261). O valor [**AdaptiveMediaSourceCreationStatus**](https://msdn.microsoft.com/library/windows/apps/dn946917) devolvido desse método faz com que você saiba se a origem de mídia foi criada com sucesso. Caso tenha sido, você pode definir o objeto como a origem de stream para o seu **MediaPlayer** chamando [**SetMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn652653). Neste exemplo, a propriedade [**AvailableBitrates**](https://msdn.microsoft.com/library/windows/apps/dn931257) é consultada para determinar a taxa de bits máxima suportada para esse stream e, em seguida, tal valor é definido como a taxa de bits inicial. Este exemplo também registra manipuladores dos eventos [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272), [**DownloadBitrateChanged**](https://msdn.microsoft.com/library/windows/apps/dn931269) e [**PlaybackBitrateChanged**](https://msdn.microsoft.com/library/windows/apps/dn931278), que são discutidos ainda neste artigo.

[!code-cs[InitializeAMS](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMS)]

Se precisar personalizar cabeçalhos em HTTP para obter o arquivo de manifesto, você pode criar um objeto [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639), definir os cabeçalhos desejados e, então, passar o objeto na sobrecarga do **CreateFromUriAsync**.

[!code-cs[InitializeAMSWithHttpClient](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMSWithHttpClient)]

O evento [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272) é levantado quando o sistema está prestes a recuperar um recurso do servidor. O [**AdaptiveMediaSourceDownloadRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn946935) passado no manipulador de eventos expõe propriedades que fornecem informações sobre o recurso ser solicitado, como o tipo e o URI do mesmo.

Você pode usar o manipulador de eventos **DownloadRequested** para modificar a solicitação de recurso atualizando as propriedades do objeto [**AdaptiveMediaSourceDownloadResult**](https://msdn.microsoft.com/library/windows/apps/dn946942) fornecido pelos argumentos de evento. No exemplo abaixo, o URI de onde o recurso será recuperado é modificado ao se atualizar as propriedades [**ResourceUri**](https://msdn.microsoft.com/library/windows/apps/dn931250) do objeto do resultado.

Você pode substituir o conteúdo do recurso de solicitação configurando as propriedades [**Buffer**](https://msdn.microsoft.com/library/windows/apps/dn946943) ou [**InputStream**](https://msdn.microsoft.com/library/windows/apps/dn931249) do objeto do resultado. No exemplo abaixo, os conteúdos do recurso de manifesto são substituídos definindo-se a propriedade **Buffer**. Observe que se você estiver adaptando a solicitação de recurso com dados obtidos de forma assíncrona, como recuperação de dados de um servidor remoto ou autenticação do usuário assíncrona, você deve chamar [**AdaptiveMediaSourceDownloadRequestedEventArgs.GetDeferral**](https://msdn.microsoft.com/library/windows/apps/dn946936) para obter um adiamento e, em seguida, chamar o [**Complete**](https://msdn.microsoft.com/library/windows/apps/dn946934) quando a operação estiver completa para sinalizar o sistema que a operação de solicitação de download pode continuar.

[!code-cs[AMSDownloadRequested](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadRequested)]

O objeto **AdaptiveMediaSource** fornece eventos que permitem que você reaja quando a taxa de bits de download ou reprodução muda. Neste exemplo, as taxas de bits atuais são simplesmente atualizadas na interface do usuário. Observe que você pode modificar os índices que determinam quando o sistema muda taxas de bits do stream adaptável. Para obter mais informações, consulte a propriedade [**AdvancedSettings**](https://msdn.microsoft.com/library/windows/apps/mt628697).

[!code-cs[AMSBitrateEvents](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSBitrateEvents)]

## Tópicos relacionados
* [Reprodução de mídia](media-playback.md)
* [Suporte para a marca HLS](hls-tag-support.md) 
* [Reproduzir áudio e vídeo com o MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Reproduzir mídia em segundo plano](background-audio.md) 








<!--HONumber=Aug16_HO3-->


