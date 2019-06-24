---
ms.assetid: CB924E17-C726-48E7-A445-364781F4CCA1
description: Este artigo mostra como usar as APIs no namespace Windows.Media.Audio para criar gráficos de áudio para cenários de roteamento, mixagem e processamento de áudio.
title: Gráficos de áudio
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ff067729e71ed4d4a49a082adf9fc754804836a6
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317597"
---
# <a name="audio-graphs"></a>Gráficos de áudio



Este artigo mostra como usar as APIs no namespace [**Windows.Media.Audio**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio) para criar gráficos de áudio para cenários de roteamento, mixagem e processamento de áudio.

Um *gráfico de áudio* é um conjunto de nós de áudio interconectados, por meio dos quais fluem os dados de áudio. 

- Os *nós de entrada de áudio* fornecem dados de áudio ao gráfico a partir de dispositivos de entrada de áudio, de arquivos de áudio ou de código personalizado. 

- Os*nós de saída de áudio* são o destino do áudio processado pelo gráfico. O áudio pode ser roteado para fora do gráfico para dispositivos de saída de áudio, arquivos de áudio ou código personalizado. 

- Os *nós de submixagem* pegam áudio de um ou mais nós e combina-os em uma única saída que pode ser roteada para outros nós no gráfico. 

Depois que todos os nós forem criados e as conexões entre eles forem configuradas, basta iniciar o gráfico de áudio para que os dados de áudio fluam dos nós de entrada, por todos os nós de submixagem, para os nós de saída. Esse modelo torna cenários, como a gravação do microfone de um dispositivo para um arquivo de áudio, a reprodução de áudio de um arquivo do alto-falante do dispositivo ou a mixagem de áudio de várias fontes, rápidos e fáceis de implementar.

Cenários adicionais são habilitados com a adição de efeitos de áudio ao gráfico de áudio. Cada nó em um gráfico de áudio pode ser preenchido com zeros ou mais efeitos de áudio que executam o processamento de áudio no áudio que passa pelo nó. Há vários efeitos internos como eco, equalizador, limitação e reverberação que podem ser anexados a um nó de áudio com apenas algumas linhas de código. Você também pode criar seus próprios efeitos de áudio que funcionem exatamente como os efeitos internos.

> [!NOTE]
> A [amostra AudioGraph UWP](https://go.microsoft.com/fwlink/?LinkId=619481) implementa o código abordado nesta visão geral. Você pode baixar a amostra para ver o código usado no contexto ou usá-lo como ponto de partida para seu próprio aplicativo.

## <a name="choosing-windows-runtime-audiograph-or-xaudio2"></a>Escolhendo AudioGraph ou XAudio2 do Windows Runtime

As APIs de gráfico de áudio do Windows Runtime oferecem a funcionalidade que também pode ser implementada usando-se as [APIs do XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-apis-portal) baseadas em COM. A seguir estão os recursos da estrutura de gráfico de áudio do Windows Runtime que diferem do XAudio2.

As APIs de gráfico de áudio do Windows Runtime:

-   São significativamente mais fáceis de usar que as do XAudio2.
-   Podem ser usadas em C#, além de oferecerem suporte a C++.
-   Podem usar arquivos de áudio, incluindo formatos de arquivo compactado, diretamente. O XAudio2 opera somente em buffers de áudio e não fornece nenhuma funcionalidade de E/S.
-   Pode usar o pipeline de áudio de baixa latência no Windows 10.
-   Oferecem suporte à troca automática de ponto de extremidade quando são usados parâmetros de ponto de extremidade padrão. Por exemplo, se o usuário alterna do alto-falante do dispositivo para um fone de ouvido, o áudio é automaticamente redirecionado para a nova entrada.

## <a name="audiograph-class"></a>Classe AudioGraph

A classe [**AudioGraph**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioGraph) é pai de todos os nós que compõem o gráfico. Use esse objeto para criar instâncias de todos os tipos de nós de áudio. Crie uma instância da classe **AudioGraph** inicializando um objeto [**AudioGraphSettings**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioGraphSettings) contendo definições de configuração para o gráfico e chame [**AudioGraph.CreateAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createasync). O [**CreateAudioGraphResult**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.CreateAudioGraphResult) retornado dá acesso ao gráfico de áudio criado ou fornece um valor de erro em caso de falha na criação do gráfico de áudio.

[!code-cs[DeclareAudioGraph](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareAudioGraph)]

[!code-cs[InitAudioGraph](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetInitAudioGraph)]

-   Todos os tipos de nó de áudio são criados usando Create\* métodos do **AudioGraph** classe.
-   O método [**AudioGraph.Start**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.start) faz com que o gráfico de áudio comece a processar dados de áudio. O método [**AudioGraph.Stop**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.stop) para o processamento do áudio. Cada nó no gráfico pode ser iniciado e interrompido independentemente enquanto o gráfico está sendo executado, mas nenhum nó está ativo quando o gráfico é interrompido. [**ResetAllNodes** ](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.resetallnodes) faz com que todos os nós no gráfico para descartar qualquer dado atualmente em seus buffers de áudio.
-   O evento [**QuantumStarted**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.quantumstarted) ocorre quando o gráfico está iniciando o processamento de um novo quantum de dados de áudio. O evento [**QuantumProcessed**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.quantumprocessed) ocorre quando o processamento de um quantum é concluído.

-   A única propriedade [**AudioGraphSettings**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioGraphSettings) necessária é [**AudioRenderCategory**](https://docs.microsoft.com/uwp/api/Windows.Media.Render.AudioRenderCategory). A especificação desse valor permite que o sistema otimize o pipeline de áudio para a categoria especificada.
-   O tamanho de quantum do gráfico de áudio determina o número de amostras que são processadas de uma só vez. Por padrão, o tamanho do quantum é de 10 ms com base na taxa de amostra padrão. Se você especificar um tamanho de quantum definido a propriedade [**DesiredSamplesPerQuantum**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.desiredsamplesperquantum), também deve definir a propriedade [**QuantumSizeSelectionMode**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.quantumsizeselectionmode) como **ClosestToDesired**; caso contrário, o valor fornecido será ignorado. Se esse valor for usado, o sistema escolherá o tamanho de quantum mais próximo possível daquele que você especificou. Para determinar o tamanho real quantum, verifique o [**SamplesPerQuantum**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.samplesperquantum) de **AudioGraph** depois que ele for criado.
-   Se você pretende usar o gráfico de áudio com arquivos apenas e não pretende enviar saída para um dispositivo de áudio, é recomendável que você use o tamanho de quantum padrão não definindo a propriedade [**DesiredSamplesPerQuantum**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.desiredsamplesperquantum).
-   A propriedade [**DesiredRenderDeviceAudioProcessing**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.desiredrenderdeviceaudioprocessing) determina a quantidade de processamento que o dispositivo de renderização principal realiza na saída do gráfico de áudio. A configuração **Default** permite que o sistema use o processamento de áudio padrão na categoria de renderização de áudio especificada. Esse processamento pode melhorar significativamente o som do áudio em alguns dispositivos, especialmente dispositivos móveis com alto-falantes pequenos. A configuração **Raw** pode melhorar o desempenho, minimizando a quantidade de processamento de sinal realizada, mas pode resultar em som de qualidade inferior em alguns dispositivos.
-   Se o [**QuantumSizeSelectionMode**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.quantumsizeselectionmode) é definido como **LowestLatency**, o gráfico de áudio usará automaticamente **Raw** para [**DesiredRenderDeviceAudioProcessing**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.desiredrenderdeviceaudioprocessing).
- A partir do Windows 10, versão 1803, você pode definir a propriedade [**AudioGraphSettings.MaxPlaybackSpeedFactor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.maxplaybackspeedfactor) a fim de definir um valor máximo usado pelas propriedades [**AudioFileInputNode.PlaybackSpeedFactor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.playbackspeedfactor), [**AudioFrameInputNode.PlaybackSpeedFactor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeinputnode.playbackspeedfactor) e [**MediaSourceInputNode.PlaybackSpeedFactor**](https://docs.microsoft.com/uwp/api/windows.media.audio.mediasourceinputnode.playbackspeedfactor). Quando um gráfico de áudio oferece suporte a um fator de velocidade de reprodução maior que 1, o sistema deve alocar memória adicional para manter um buffer suficiente de dados de áudio. Por esse motivo, a configuração de **MaxPlaybackSpeedFactor** com o valor mais baixo exigido pelo seu aplicativo reduz o consumo de memória do aplicativo. Caso seu aplicativo reproduza somente conteúdo na velocidade normal, é recomendável que você defina MaxPlaybackSpeedFactor como 1.
-   [  **EncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.encodingproperties) determina o formato de áudio usado pelo gráfico. Há suporte para formatos float de 32 bits somente.
-   O [**PrimaryRenderDevice**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.primaryrenderdevice) define o dispositivo de renderização principal para o gráfico de áudio. Se você não definir isso, o dispositivo padrão do sistema será usado. O dispositivo de renderização principal é usado para calcular os tamanhos de quantum para outros nós do gráfico. Se não houver dispositivos de renderização de áudio presentes no sistema, a criação de gráfico de áudio falhará.

Você pode permitir que o gráfico de áudio use o dispositivo de renderização de áudio padrão ou usar a classe [**Windows.Devices.Enumeration.DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) para obter uma lista dos dispositivos de renderização de áudio disponíveis no sistema chamando [**FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) e passando o seletor de dispositivo de renderização de áudio retornado por [**Windows.Media.Devices.MediaDevice.GetAudioRenderSelector**](https://docs.microsoft.com/uwp/api/windows.media.devices.mediadevice.getaudiorenderselector). É possível escolher um dos objetos **DeviceInformation** retornados programaticamente ou mostrar a interface do usuário para permitir que o usuário selecione um dispositivo e, em seguida, usá-lo para definir a propriedade [**PrimaryRenderDevice**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.primaryrenderdevice).

[!code-cs[EnumerateAudioRenderDevices](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetEnumerateAudioRenderDevices)]

##  <a name="device-input-node"></a>Nó de entrada de dispositivo

Um nó de entrada de dispositivo passa áudio para o gráfico a partir de um dispositivo de captura de áudio conectado ao sistema, como um microfone. Crie um objeto [**DeviceInputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioDeviceInputNode) que usa o dispositivo de captura de áudio padrão do sistema chamando [**CreateDeviceInputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createdeviceinputnodeasync). Forneça uma [**AudioRenderCategory**](https://docs.microsoft.com/uwp/api/Windows.Media.Render.AudioRenderCategory) para permitir que o sistema otimize o pipeline de áudio para a categoria especificada.

[!code-cs[DeclareDeviceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareDeviceInputNode)]


[!code-cs[CreateDeviceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateDeviceInputNode)]

Se você quiser especificar um dispositivo de captura de áudio específica para o nó de entrada do dispositivo, você pode usar o [ **Windows.Devices.Enumeration.DeviceInformation** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) classe para obter uma lista de áudio de disponíveis do sistema dispositivos de captura, chamando [ **FindAllAsync** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) e passando no seletor de dispositivo de processamento de áudio retornado pela [  **Windows.Media.Devices.MediaDevice.GetAudioCaptureSelector**](https://docs.microsoft.com/uwp/api/windows.media.devices.mediadevice.getaudiocaptureselector). É possível escolher um dos objetos **DeviceInformation** retornados programaticamente ou mostrar a interface do usuário para permitir que o usuário selecione um dispositivo e, em seguida, passá-lo para [**CreateDeviceInputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createdeviceinputnodeasync).

[!code-cs[EnumerateAudioCaptureDevices](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetEnumerateAudioCaptureDevices)]

##  <a name="device-output-node"></a>Nó de saída do dispositivo

Um nó de saída do dispositivo empurra áudio do gráfico para um dispositivo de renderização de áudio, como alto-falantes ou um fone de ouvido. Crie um [**DeviceOutputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode) chamando [**CreateDeviceOutputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createdeviceoutputnodeasync). O nó de saída usa o [**PrimaryRenderDevice**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.primaryrenderdevice) do gráfico de áudio.

[!code-cs[DeclareDeviceOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareDeviceOutputNode)]

[!code-cs[CreateDeviceOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateDeviceOutputNode)]

##  <a name="file-input-node"></a>Nó de entrada do arquivo

Um nó de entrada de arquivo permite que você alimente dados de um arquivo de áudio no gráfico. Crie um [**AudioFileInputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFileInputNode) chamando [**CreateFileInputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createfileinputnodeasync).

[!code-cs[DeclareFileInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFileInputNode)]


[!code-cs[CreateFileInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFileInputNode)]

-   Nós de entrada do arquivo oferecem suporte aos seguintes formatos de arquivo: mp3, wav, wma, m4a.
-   Defina a propriedade [**StartTime**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.starttime) para especificar a diferença de horário para o arquivo no qual a reprodução deve começar. Se essa propriedade for null, o início do arquivo será usado. Defina a propriedade [**EndTime**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.endtime) para especificar o deslocamento de tempo para o arquivo no qual a reprodução deve terminar. Se essa propriedade for nula, será usado o fim do arquivo. O valor de hora inicial deve ser menor que o valor de hora final, e o valor de hora final deve ser menor ou igual à duração do arquivo de áudio, que pode ser determinada verificando o valor da propriedade [**Duration**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.duration).
-   Busque uma posição no arquivo de áudio chamando [**Seek**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.seek) e especificando o deslocamento de tempo para o arquivo no qual a posição de reprodução deve ser movida. O valor especificado deve estar dentro do intervalo de [**StartTime**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.starttime) e [**EndTime**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.endtime). Obtenha a posição de reprodução atual do nó com a propriedade somente leitura [**Position**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.position).
-   Habilite o loop do arquivo de áudio definindo a propriedade [**LoopCount**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.loopcount). Quando não é nulo, esse valor indica o número de vezes em que o arquivo será reproduzido após a reprodução inicial. Assim, por exemplo, a definição de **LoopCount** como 1 fará com que o arquivo seja reproduzido 2 vezes no total e defini-lo como 5 fará com que o arquivo seja reproduzido 6 vezes no total. A definição de **LoopCount** como nulo fará com que o arquivo entre em um loop infinito. Para interromper o loop, defina o valor como 0.
-   Ajuste a velocidade em que o arquivo de áudio é reproduzido definindo [**PlaybackSpeedFactor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.playbackspeedfactor). Um valor 1 indica a velocidade original do arquivo, 0,5 indica metade da velocidade e 2 é velocidade dupla.

##  <a name="mediasource-input-node"></a>Nó de entrada de MediaSource

A classe [**MediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource) fornece uma maneira comum de referenciar mídia de diferentes fontes e expõe um modelo comum para acessar dados de mídia, independentemente do formato de mídia subjacente, que pode ser um arquivo no disco, um streaming ou uma fonte de rede de streaming adaptável. Um nó [**MediaSourceAudioInputNode](https://docs.microsoft.com/uwp/api/windows.media.audio.mediasourceaudioinputnode) permite que você direcione dados de áudio de uma **MediaSource** para o gráfico de áudio. Crie um **MediaSourceAudioInputNode** ao chamar [**CreateMediaSourceAudioInputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createmediasourceaudioinputnodeasync#Windows_Media_Audio_AudioGraph_CreateMediaSourceAudioInputNodeAsync_Windows_Media_Core_MediaSource_), passando um objeto **MediaSource** representando o conteúdo que você deseja reproduzir. Um [**CreateMediaSourceAudioInputNodeResult](https://docs.microsoft.com/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult) é retornado, o qual você pode usar para determinar o status da operação ao verificar a propriedade [**Status**](https://docs.microsoft.com/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult.status). Se o status for **Sucesso**, você pode obter o **MediaSourceAudioInputNode** criado ao acessar a propriedade [**Node**](https://docs.microsoft.com/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult.node). O exemplo a seguir mostra a criação de um nó de um objeto AdaptiveMediaSource representando o streaming de conteúdo pela rede. Para obter mais informações sobre como trabalhar com **MediaSource**, consulte [Itens de mídia, playlists e faixas](media-playback-with-mediasource.md). Para obter mais informações sobre o conteúdo de mídia de streaming pela Internet, consulte [Streaming adaptável](adaptive-streaming.md).

[!code-cs[DeclareMediaSourceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareMediaSourceInputNode)]

[!code-cs[CreateMediaSourceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateMediaSourceInputNode)]

Para receber uma notificação quando a reprodução chegar ao fim do conteúdo de **MediaSource**, registre um manipulador para o evento [**MediaSourceCompleted**](https://docs.microsoft.com/uwp/api/windows.media.audio.mediasourceaudioinputnode.mediasourcecompleted). 

[!code-cs[RegisterMediaSourceCompleted](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetRegisterMediaSourceCompleted)]

[!code-cs[MediaSourceCompleted](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetMediaSourceCompleted)]

Ao reproduzir um arquivo do disco, é provável que seja concluído com sucesso, a mídia transmitida de uma fonte de rede pode falhar durante a reprodução devido a uma alteração na conexão de rede ou outros problemas fora do controle do gráfico de áudio. Se não for possível reproduzir uma **MediaSource**, o gráfico de áudio acionará o evento [**UnrecoverableErrorOccurred**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.unrecoverableerroroccurred). Você pode usar o manipulador para esse evento a fim de parar e descartar o gráfico de áudio e, em seguida, reinicializar o gráfico. 

[!code-cs[RegisterUnrecoverableError](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetRegisterUnrecoverableError)]

[!code-cs[UnrecoverableError](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetUnrecoverableError)]

##  <a name="file-output-node"></a>Nó de saída de arquivo

Um nó de saída de arquivo permite que você direcione dados de áudio do gráfico para um arquivo de áudio. Crie um [**AudioFileOutputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFileOutputNode) chamando [**CreateFileOutputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createfileoutputnodeasync).

[!code-cs[DeclareFileOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFileOutputNode)]


[!code-cs[CreateFileOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFileOutputNode)]

-   Nós de saída do arquivo oferecem suporte aos seguintes formatos de arquivo: mp3, wav, wma, m4a.
-   Você deve chamar [**AudioFileOutputNode.Stop**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileoutputnode.stop) para parar o processamento do nó antes de chamar [**AudioFileOutputNode.FinalizeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileoutputnode.finalizeasync). Caso contrário, uma exceção será gerada.

##  <a name="audio-frame-input-node"></a>Nó de entrada de quadro de áudio

Um nó de entrada de quadro de áudio permite que você envie dados de áudio que você gera em seu próprio código para o gráfico de áudio. Isso habilita cenários como a criação de um sintetizador de software personalizado. Crie um [**AudioFrameInputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFrameInputNode) chamando [**CreateFrameInputNode**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createframeinputnode).

[!code-cs[DeclareFrameInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFrameInputNode)]


[!code-cs[CreateFrameInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFrameInputNode)]

O evento [**FrameInputNode.QuantumStarted**](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeinputnode.quantumstarted) é disparado quando o gráfico de áudio está pronto para começar o processamento do próximo quantum de dados de áudio. Forneça dados de áudio personalizados gerados de dentro do manipulador para esse evento.

[!code-cs[QuantumStarted](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetQuantumStarted)]

-   O objeto [**FrameInputNodeQuantumStartedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.FrameInputNodeQuantumStartedEventArgs) passado para o manipulador de evento **QuantumStarted** expõe a propriedade [**RequiredSamples**](https://docs.microsoft.com/uwp/api/windows.media.audio.frameinputnodequantumstartedeventargs.requiredsamples) que indica quantas amostras são necessárias para que o gráfico de áudio preencha o quantum a ser processado.
-   Chame [**AudioFrameInputNode.AddFrame**](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeinputnode.addframe) para passar um objeto [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame) preenchido com dados de áudio para o gráfico.
- Um novo conjunto de APIs para usar **MediaFrameReader** com dados de áudio foi introduzido no Windows 10, versão 1803. Essas APIs permitem que você obtenha objetos **AudioFrame** a partir de uma fonte de quadro de mídia, que pode ser passada para um **FrameInputNode** usando o método **AddFrame**. Para obter mais informações, consulte [Processar quadros de áudio com o MediaFrameReader](process-audio-frames-with-mediaframereader.md).
-   Veja a seguir um exemplo de implementação do método auxiliar **GenerateAudioData**.

Para preencher um [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame) com dados de áudio, você deve obter acesso ao buffer de memória subjacente do quadro de áudio. Para fazer isso, você deve inicializar a interface COM **IMemoryBufferByteAccess** adicionando o seguinte código dentro de seu namespace.

[!code-cs[ComImportIMemoryBufferByteAccess](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetComImportIMemoryBufferByteAccess)]

O código a seguir mostra um exemplo de implementação de um método auxiliar **GenerateAudioData** que cria um [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame) e o preenche com dados de áudio.

[!code-cs[GenerateAudioData](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetGenerateAudioData)]

-   Como esse método acessa o buffer bruto subjacente aos tipos do Windows Runtime, ele deve ser declarado usando a palavra-chave **unsafe**. Você também deve configurar seu projeto no Microsoft Visual Studio para permitir a compilação de código não seguro. Para fazer isso, abra a página **Propriedades** do projeto, clique na página de propriedade **Build** e selecione a caixa de seleção **Permitir Código Não Seguro**.
-   Inicialize uma nova instância de [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame), no namespace **Windows.Media**, passando o tamanho de buffer desejado para o construtor. O tamanho do buffer é o número de amostras multiplicado pelo tamanho de cada exemplo.
-   Obtenha o [**AudioBuffer**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioBuffer) do quadro de áudio chamando [**LockBuffer**](https://docs.microsoft.com/uwp/api/windows.media.audioframe.lockbuffer).
-   Obtenha uma instância da interface COM [**IMemoryBufferByteAccess**](https://docs.microsoft.com/previous-versions/mt297505(v=vs.85)) do buffer de áudio chamando [**CreateReference**](https://docs.microsoft.com/uwp/api/windows.media.audiobuffer.createreference).
-   Obtenha um ponteiro para dados de buffer de áudio bruto chamando [**IMemoryBufferByteAccess.GetBuffer**](https://docs.microsoft.com/windows/desktop/WinRT/imemorybufferbyteaccess-getbuffer) e transmita-o para o tipo de dados de amostra dos dados de áudio.
-   Preencha o buffer com dados e retorne o [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame) para envio para o gráfico de áudio.

##  <a name="audio-frame-output-node"></a>Nó de saída de quadro de áudio

Um nó de saída de quadro de áudio permite receber e processar a saída de dados de áudio do gráfico de áudio com o código personalizado que você criar. Um cenário de exemplo para isso é a análise do sinal na saída de áudio. Crie um [**AudioFrameOutputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFrameOutputNode) chamando [**CreateFrameOutputNode**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createframeoutputnode).

[!code-cs[DeclareFrameOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFrameOutputNode)]

[!code-cs[CreateFrameOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFrameOutputNode)]

O evento [**AudioGraph.QuantumStarted**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioGraph.QuantumStarted) é disparado quando o gráfico de áudio inicia o processamento de um quantum de dados de áudio. Você pode acessar os dados de áudio de dentro do manipulador para esse evento. 

> [!NOTE]
> Se você quiser recuperar quadros de áudio em uma cadência regular, sincronizado com o gráfico de áudio, chame [AudioFrameOutputNode.GetFrame](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeoutputnode.GetFrame) dentro do manipulador de eventos **QuantumStarted** síncronos. O evento **QuantumProcessed** é gerado assincronamente depois que o mecanismo de áudio conclui o processamento de áudio, o que significa que sua cadência pode ser irregular. Portanto, você não deve usar o evento **QuantumProcessed** para o processamento sincronizado de dados de quadros de áudio.

[!code-cs[SnippetQuantumStartedFrameOutput](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetQuantumStartedFrameOutput)]

-   Chame [**GetFrame**](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeoutputnode.getframe) para obter um objeto [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame) preenchido com dados de áudio do gráfico.
-   Veja a seguir um exemplo de implementação do método auxiliar **ProcessFrameOutput**.

[!code-cs[ProcessFrameOutput](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetProcessFrameOutput)]

-   Como o exemplo de nó de entrada de quadro de áudio acima, você precisa declarar a interface COM **IMemoryBufferByteAccess** e configurar seu projeto para permitir código não seguro a fim de acessar o buffer de áudio subjacente.
-   Obtenha o [**AudioBuffer**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioBuffer) do quadro de áudio chamando [**LockBuffer**](https://docs.microsoft.com/uwp/api/windows.media.audioframe.lockbuffer).
-   Obtenha uma instância da interface COM **IMemoryBufferByteAccess** do buffer de áudio chamando [**CreateReference**](https://docs.microsoft.com/uwp/api/windows.media.audiobuffer.createreference).
-   Obtenha um ponteiro para dados de buffer de áudio bruto chamando **IMemoryBufferByteAccess.GetBuffer** e transmita-o para o tipo de dados de amostra dos dados de áudio.

## <a name="node-connections-and-submix-nodes"></a>Conexões de nós e nós de submixagem

Todos os nós a exposição de tipos de entrada do método **AddOutgoingConnection** que encaminha o áudio produzido pelo nó para o nó que é passado para o método. O exemplo a seguir conecta um [**AudioFileInputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFileInputNode) a um [**AudioDeviceOutputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode), que é uma configuração simples para reproduzir um arquivo de áudio no alto-falante do dispositivo.

[!code-cs[AddOutgoingConnection1](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection1)]

Você pode criar mais de uma conexão de um nó de entrada para outros nós. O exemplo a seguir adiciona outra conexão do [**AudioFileInputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFileInputNode) para um [**AudioFileOutputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFileOutputNode). Agora, o áudio do arquivo de áudio é reproduzido no alto-falante do dispositivo e também é gravado em um arquivo de áudio.

[!code-cs[AddOutgoingConnection2](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection2)]

Nós de saída também podem receber mais de uma conexão de outros nós. No exemplo a seguir, uma conexão é feita de um [**AudioDeviceInputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioDeviceInputNode) para o nó [**AudioDeviceOutput**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode). Como o nó de saída tem conexões do nó de entrada do arquivo e do nó de entrada do dispositivo, a saída conterá uma combinação das duas fontes de áudio. **AddOutgoingConnection** fornece uma sobrecarga que permite a especificação de um valor de ganho para o sinal que passa pela conexão.

[!code-cs[AddOutgoingConnection3](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection3)]

Embora nós de saída possam aceitar conexões de vários nós, convém criar uma combinação intermediária de sinais de um ou mais nós antes de passar a combinação para uma saída. Por exemplo, pode ser desejável definir o nível ou aplicar efeitos em um subconjunto de sinais de áudio em um gráfico. Para fazer isso, use o [**AudioSubmixNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioSubmixNode). Você pode se conectar a um nó de submixagem a partir de um ou mais nós ou outros nós de submixagem de entrada. No exemplo a seguir, um novo nó de submixagem é criado com [**AudioGraph.CreateSubmixNode**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createsubmixnode). Em seguida, são adicionadas conexões de um nó de entrada de arquivo e um nó de saída de quadro para o nó de submixagem. Por fim, o nó de submixagem é conectado a um nó de saída do arquivo.

[!code-cs[CreateSubmixNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateSubmixNode)]

## <a name="starting-and-stopping-audio-graph-nodes"></a>Iniciando e interrompendo nós de gráfico de áudio

Quando [**AudioGraph.Start**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.start) é chamado, o gráfico de áudio começa a processar dados de áudio. Cada tipo de nó fornece métodos **Start** e **Stop** que fazem com que o nó individual inicie ou pare o processamento de dados. Quando [**AudioGraph.Stop**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.stop) é chamado, todo o processamento de áudio em todos os nós é parado independentemente do estado dos nós individuais, mas o estado de cada nó pode ser definido enquanto o gráfico de áudio é interrompido. Por exemplo, você pode chamar **Stop** em um nó individual enquanto o gráfico é parado e, em seguida, chamar **AudioGraph.Start** para que o nó individual permaneça no estado parado.

Todos os tipos de nó expõem a propriedade **ConsumeInput** que, quando definida como false, permite que o nó continue o processamento de áudio mas interrompe seu consumo de quaisquer dados de áudio que estejam sendo recebidos de outros nós.

Todos os tipos de nó expõem o método **Reset** que faz o nó descartar todos os dados de áudio no buffer no momento.

## <a name="adding-audio-effects"></a>Adicionando efeitos de áudio

A API do gráfico de áudio permite que você adicione efeitos de áudio a cada tipo de nó em um gráfico. Nós de saída, nós de entrada e nós de submixagem podem ter um número ilimitado de efeitos de áudio, limitado somente pelos recursos do hardware. O exemplo a seguir demonstra a adição do efeito de eco interno a um nó de submixagem.

[!code-cs[AddEffect](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddEffect)]

-   Todos os efeitos de áudio implementam [**IAudioEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IAudioEffectDefinition). Cada nó expõe uma propriedade **EffectDefinitions** que representa a lista de efeitos aplicados ao nó. Adicione um efeito adicionando seu objeto de definição à lista.
-   Várias classes de definição de efeito são fornecidas no namespace **Windows.Media.Audio**. Como por exemplo:
    -   [**EchoEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.EchoEffectDefinition)
    -   [**EqualizerEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.EqualizerEffectDefinition)
    -   [**LimiterEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.LimiterEffectDefinition)
    -   [**ReverbEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.ReverbEffectDefinition)
-   Você pode criar seus próprios efeitos de áudio que implementam [**IAudioEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IAudioEffectDefinition) e aplicá-los a qualquer nó em um gráfico de áudio.
-   Cada nó expõe um método **DisableEffectsByDefinition** que desabilita todos os efeitos na lista **EffectDefinitions** do nó que foram adicionados usando a definição especificada. **EnableEffectsByDefinition** habilita os efeitos com a definição especificada.

## <a name="spatial-audio"></a>Áudio espacial
A partir do Windows 10, versão 1607, **AudioGraph** oferece suporte ao áudio espacial, que permite que você especifique o local no espaço 3D do qual o audio de qualquer entrada ou nó de submixagem seja emitido. Você também pode especificar uma forma e a direção em que o áudio é emitido, uma velocidade que será usada para fazer a mudança de Doppler do áudio do nó e definir um modelo de decaimento que descreve como o áudio é atenuado com a distância. 

Para criar um emissor, primeiro você pode criar uma forma na qual o som é projetado a partir do emissor, que pode ser um cone ou unidirecional. A classe [**AudioNodeEmitterShape**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioNodeEmitterShape) fornece métodos estáticos para criar cada uma dessas formas. Em seguida, crie um modelo de decaimento. Isso define como o volume do áudio do emissor diminui à medida que a distância do ouvinte aumenta. O método [**CreateNatural**](https://docs.microsoft.com/uwp/api/windows.media.audio.audionodeemitterdecaymodel.createnatural) cria um modelo de decaimento que emula o decaimento natural de som usando um modelo de queda do quadrado da distância. Por fim, crie um objeto [**AudioNodeEmitterSettings**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioNodeEmitterSettings). Atualmente, esse objeto é usado somente para habilitar e desabilitar atenuações de Doppler baseadas na velocidade do áudio do emissor. Chame o construtor [**AudioNodeEmitter**](https://docs.microsoft.com/uwp/api/windows.media.audio.audionodeemitter.-ctor), transmitindo os objetos de inicialização que você acabou de criar. Por padrão, o emissor é colocado na origem, mas você pode definir a posição do emissor com a propriedade [**Position**](https://docs.microsoft.com/uwp/api/windows.media.audio.audionodeemitter.position).

> [!NOTE]
> Os emissores de nó de áudio somente podem processar o áudio formatado em mono com uma taxa de amostragem de 48 kHz. A tentativa de usar o áudio estéreo ou o áudio com uma taxa de amostragem diferente resultará em uma exceção.

Você deve atribuir o emissor a um nó de áudio ao criá-lo, usando o método sobrecarregado de criação para o tipo de nó que deseja. Neste exemplo, [**CreateFileInputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createfileinputnodeasync) é usado para criar um nó de entrada do arquivo de um arquivo especificado e o objeto [**AudioNodeEmitter**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioNodeEmitter) que você deseja associar ao nó.

[!code-cs[CreateEmitter](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateEmitter)]

O [**AudioDeviceOutputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode) que gera áudio do gráfico para o usuário tem um objeto ouvinte, acessado com a propriedade [**Listener**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiodeviceoutputnode.listener), que representa a localização, a orientação e a velocidade do usuário no espaço 3D. As posições de todos os emissores no gráfico são relativas à posição e à orientação do objeto emissor. Por padrão, o ouvinte está localizado na origem (0, 0, 0) voltado para frente, ao longo do eixo Z, mas você pode definir sua posição e orientação com as propriedades [**Position**](https://docs.microsoft.com/uwp/api/windows.media.audio.audionodelistener.position) e [**Orientation**](https://docs.microsoft.com/uwp/api/windows.media.audio.audionodelistener.orientation).

[!code-cs[Listener](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetListener)]

Você pode atualizar a localização, a velocidade e a direção dos emissores no tempo de execução para simular o movimento de uma fonte de áudio pelo espaço 3D.

[!code-cs[UpdateEmitter](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetUpdateEmitter)]

Você também pode atualizar a localização, a velocidade e a orientação do objeto ouvinte no tempo de execução para simular o movimento do usuário pelo espaço 3D.

[!code-cs[UpdateListener](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetUpdateListener)]

Por padrão, o áudio espacial é calculado usando o algoritmo de função de transferência relativas à cabeça (HRTF) da Microsoft para atenuar o áudio com base em sua forma, velocidade e posição relativa ao ouvinte. Você pode definir a propriedade [**SpatialAudioModel**](https://docs.microsoft.com/uwp/api/windows.media.audio.audionodeemitter.spatialaudiomodel) como **FoldDown** para usar um método simples de mixagem estéreo de simulação de áudio espacial que é menos preciso, mas exige menos recursos de CPU e de memória.

## <a name="see-also"></a>Consulte também
- [Reprodução de mídia](media-playback.md)
 

 




