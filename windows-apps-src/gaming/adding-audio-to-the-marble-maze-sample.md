---
title: Adicionando áudio ao exemplo do Marble Maze
description: Este documento descreve as principais práticas a serem consideradas quando se trabalha com áudio e mostra como o Marble Maze as aplica.
ms.assetid: 77c23d0a-af6d-17b5-d69e-51d9885b0d44
ms.date: 10/18/2017
ms.topic: article
keywords: windows 10, uwp, áudio, jogos, amostra
ms.localizationpriority: medium
ms.openlocfilehash: ba0b4882803d9e6ddf059ae0f431c5bfba7eb227
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369153"
---
# <a name="adding-audio-to-the-marble-maze-sample"></a>Adicionando áudio ao exemplo do Marble Maze

Este documento descreve as principais práticas a serem consideradas quando se trabalha com áudio e mostra como o Marble Maze as aplica. O Marble Maze usa [Microsoft Media Foundation](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk) para carregar recursos dos arquivos e o [XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-apis-portal) para mixar, reproduzir áudio e aplicar efeitos ao áudio.

O Marble Maze executa música em segundo plano e usa sons do jogo para indicar eventos do jogo, por exemplo, quando a bolinha bate em uma parede. Uma parte importante da implementação é que o Marble Maze usa um efeito de reverberação (ou eco) para simular o som de uma bolinha quando ela quica. A implementação do efeito de reverberação faz com que ecos cheguem a você mais rapidamente e mais altos em ambientes pequenos; os ecos chegam até você mais lentamente e mais baixos em ambientes maiores.

> [!NOTE]
> O exemplo de código que corresponde a este documento pode ser encontrado no [Exemplo do jogo Marble Maze em DirectX](https://go.microsoft.com/fwlink/?LinkId=624011).

Seguem alguns dos pontos principais discutidos neste documento para quando você trabalhar com o áudio do seu jogo:

- Considere usar o Media Foundation para decodificar ativos de áudio e o XAudio2 para reproduzir áudio. Entretanto, se você já tem um mecanismo de carregamento ativo de áudio que funciona em um aplicativo da Plataforma Universal do Windows (UWP), você pode usá-lo.

- Um gráfico de áudio contém uma voz de origem para cada som ativo, zero ou mais vozes de submix e uma voz de masterização. As vozes de origem podem alimentar as vozes de submix e/ou a voz de masterização. As vozes de submix alimentam outras vozes de submix ou a voz de masterização.

- Se seus arquivos de música em segundo plano forem grandes, considere transmitir suas músicas em buffers menores para usar menos memória.

- Se isso for melhor, pause a reprodução de áudio quando o aplicativo perder o foco ou a visibilidade, ou quando for suspenso. Continue a reprodução quando o aplicativo recobrar o foco, ficar visível ou for retomado.

- Configure as categorias de áudio para refletirem a função de cada som. Por exemplo, você normalmente usa **AudioCategory\_GameMedia** para áudio de fundo do jogo e **AudioCategory\_GameEffects** para efeitos de som.

- Lide com mudanças de dispositivos, como fones de ouvido, lançando e recriando todos os recursos e interfaces de áudio.

- Considere se deseja compactar arquivos de áudio quando for necessário reduzir o espaço em disco e os custos de streaming. Caso contrário, você pode deixar o áudio descompactado para que seja carregado mais rapidamente.

## <a name="introducing-xaudio2-and-microsoft-media-foundation"></a>Apresentando o XAudio2 e o Microsoft Media Foundation

O XAudio2 é uma biblioteca de áudio de baixo nível para Windows que dá suporte especificamente a áudio de jogo. Ele fornece um DSP (processamento de sinal digital) e um mecanismo de gráfico de áudio para jogos. O XAudio2 supera seus antecessores, DirectSound e XAudio, sendo compatível com tendências da computação, como arquiteturas de ponto flutuante SIMD e áudio HD. Ele também dá suporte às mais complexas demandas de processamento de som dos jogos de hoje.

O documento [XAudio2 Key Concepts](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-key-concepts) explica os principais conceitos para usar o XAudio2. De forma resumida, os conceitos são:

- A interface do [IXAudio2](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) é o núcleo do mecanismo XAudio2. O Marble Maze usa essa interface para criar vozes e receber notificações quando o dispositivo de saída muda ou falha.

- Uma **voz** processa, ajusta e executa dados de áudio.

- Uma **voz de origem** é uma coleção de canais de áudio (mono, 5.1 e assim por diante) e representa um fluxo de dados de áudio. No XAudio2, uma voz de origem é onde o processamento de áudio começa. Normalmente, os dados de som são carregados a partir de uma origem externa, como um arquivo ou uma rede, e enviados para uma voz de origem. O Marble Maze usa o [Media Foundation](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk) para carregar dados de som a partir de arquivos. O Media Foundation será apresentado posteriormente neste documento.

- Uma **voz de submix** processa dados de áudio. Esse processamento pode incluir a alteração do fluxo de áudio ou a integração de vários fluxos em um só. O Marble Maze usa submixes para criar o efeito de reverberação.

- Uma **voz de masterização** combina dados de vozes de origem e de submix e envia esses dados para o hardware de áudio.

- Um **gráfico de áudio** contém uma voz de origem para cada som ativo, zero ou mais vozes de submix e somente uma voz de masterização.

- Um **retorno de chamada** informa o código do cliente de que ocorreu algum evento em uma voz ou em um objeto do mecanismo. Ao usar retornos de chamada, você pode reutilizar a memória quando o XAudio2 esgotar um buffer, reagir quando o dispositivo de áudio for trocado (por exemplo, quando você conecta ou desconecta fones de ouvido) e muito mais. A seção [Como lidar com mudanças de dispositivo e fones de ouvido](#handling-headphones-and-device-changes), ainda neste documento, explica como o Marble Maze usa esse mecanismo para lidar com mudanças de dispositivo.

O Marble Maze usa dois mecanismos de áudio (em outras palavras, dois objetos [IXAudio2](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2)) para processar o áudio. Um mecanismo processa a música em segundo plano e o outro, os sons do jogo.

O Marble Maze também deve criar uma voz de masterização para cada mecanismo. Lembre-se de que um mecanismo de masterização combina fluxos de áudio em um único fluxo e envia esse fluxo para o hardware de áudio. O fluxo de música em segundo plano, uma voz de origem, emite dados para uma voz de masterização e para duas vozes de submix. As vozes de submix executam o efeito de reverberação.

O Media Foundation é uma biblioteca multimídia que dá suporte a muitos formatos de áudio e vídeo. O XAudio2 e o Media Foundation são complementares. O Marble Maze usa o Media Foundation para carregar ativos de áudio a partir dos arquivos e usa o XAudio2 para executar áudio. Não é preciso usar o Media Foundation para carregar ativos de áudio. Se você já tem um mecanismo de carregamento de ativos de áudio que funcione com aplicativos da UWP (Plataforma Universal do Windows), você poderá usá-lo. [Áudio, vídeo e câmera](../audio-video-camera/index.md) aborda várias formas de implementação de áudio em um aplicativo UWP.

Para saber mais sobre o XAudio2, consulte o [Guia de Programação](https://docs.microsoft.com/windows/desktop/xaudio2/programming-guide). Para saber mais sobre o Media Foundation, veja o [Microsoft Media Foundation](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk).

## <a name="initializing-audio-resources"></a>Inicializando recursos de áudio

O Marble Mazes usa um arquivo .wma (Windows Media Audio) para a música em segundo plano e arquivos .wav (WAV) para sons do jogo. Esses formatos são compatíveis com o Media Foundation. Embora o formato de arquivo .wav seja nativamente compatível com o XAudio2, um jogo precisa analisar o formato de arquivo manualmente para preencher as estruturas de dados apropriadas do XAudio2. O Marble Maze usa o Media Foundation para trabalhar com arquivos .wav com mais facilidade. Para ver uma lista completa dos formatos de mídia com suporte no Media Foundation, consulte [Supported Media Formats in Media Foundation](https://docs.microsoft.com/windows/desktop/medfound/supported-media-formats-in-media-foundation). O Marble Maze não usa formatos de áudio de tempo de design e de tempo de execução separados. Ele também não usa o suporte para compactação ADPCM do XAudio2. Para mais informações sobre a compactação ADPCM no XAudio2, consulte [ADPCM Overview](https://docs.microsoft.com/windows/desktop/xaudio2/adpcm-overview).

O método **Audio::CreateResources**, chamado do **MarbleMazeMain::LoadDeferredResources**, carrega os streams de áudio dos arquivos, inicializa os objetos do mecanismo de XAudio2 e cria as vozes de origem, submixagem e masterização.

### <a name="creating-the-xaudio2-engines"></a>Criando os mecanismos de XAudio2

Lembre-se de que o Marble Maze cria um objeto de [IXAudio2](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) para representar cada mecanismo de áudio que ele usa. Para criar um mecanismo de áudio, chame o método [XAudio2Create](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create). O exemplo a seguir mostra como o Marble Maze cria o mecanismo de áudio que processa a música em segundo plano.

```cpp
// In Audio.h
class Audio
{
private:
    IXAudio2*                   m_musicEngine;
// ...
}

// In Audio.cpp
void Audio::CreateResources()
{
    try
    {
        // ...
        DX::ThrowIfFailed(
            XAudio2Create(&m_musicEngine)
            );
        // ...
    }
    // ...
}
```

O Marble Maze executa uma etapa semelhante para criar o mecanismo de áudio que reproduz os sons do jogo.

Trabalhar com a interface do [IXAudio2](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) em um aplicativo UWP diferencia-se de um aplicativo de área de trabalho em duas formas. Primeiro, você não precisa chamar o [CoInitializeEx](https://docs.microsoft.com/windows/desktop/api/combaseapi/nf-combaseapi-coinitializeex) antes de chamar o [XAudio2Create](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create). Além disso, o **IXAudio2** não dá mais suporte à enumeração do dispositivo. Para informações sobre como enumerar dispositivos de áudio, consulte [Enumerando dispositivos](https://docs.microsoft.com/previous-versions/windows/apps/hh464977(v=win.10)).

### <a name="creating-the-mastering-voices"></a>Criando as vozes de masterização

O exemplo a seguir mostra como o método **Audio::CreateResources** cria a voz de masterização para a música em segundo plano usando o método [IXAudio2::CreateMasteringVoice](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createmasteringvoice). Neste exemplo, **m\_musicMasteringVoice** é uma [IXAudio2MasteringVoice](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2masteringvoice) objeto. Especificamos dois canais de entrada; isso simplifica a lógica do efeito de reverberação. 

Especificamos 48000 como a taxa de amostragem de entrada. Escolhemos essa taxa de amostragem porque ela representa um equilíbrio entre a qualidade de áudio e a quantidade necessária de processamento da CPU. Uma taxa de amostragem maior teria exigido mais processamento de CPU sem ter um benefício notável na qualidade. 

Por fim, especificamos **AudioCategory_GameMedia** como a categoria de stream de áudio para que os usuários possam ouvir música de um aplicativo diferente enquanto jogam. Durante a reprodução de um aplicativo de música, o Windows retira qualquer vozes que são criadas pela **AudioCategory\_GameMedia** opção. O usuário ainda ouve a jogabilidade parece porque eles são criados pela **AudioCategory\_GameEffects** opção. Para obter mais informações sobre as categorias de áudio, consulte [áudio\_STREAM\_categoria](https://docs.microsoft.com/windows/desktop/api/audiosessiontypes/ne-audiosessiontypes-_audio_stream_category).

```cpp
// This sample plays the equivalent of background music, which we tag on the  
// mastering voice as AudioCategory_GameMedia. In ordinary usage, if we were  
// playing the music track with no effects, we could route it entirely through 
// Media Foundation. Here, we are using XAudio2 to apply a reverb effect to the 
// music, so we use Media Foundation to decode the data then we feed it through 
// the XAudio2 pipeline as a separate Mastering Voice, so that we can tag it 
// as Game Media. We default the mastering voice to 2 channels to simplify  
// the reverb logic.
DX::ThrowIfFailed(
    m_musicEngine->CreateMasteringVoice(
        &m_musicMasteringVoice,
        2,
        48000,
        0,
        nullptr,
        nullptr,
        AudioCategory_GameMedia
        )
);
```

O **Audio::CreateResources** método executa uma etapa semelhante para criar a dominar voz para os sons do jogo, exceto que ele especifica **AudioCategory\_GameEffects** para o  *StreamCategory* parâmetro, que é o padrão.

### <a name="creating-the-reverb-effect"></a>Criando o efeito de reverberação

Para cada voz, você pode usar o XAudio2 para criar sequências de efeitos que processam áudio. Essa sequência é conhecida como uma cadeia de efeitos. Use cadeias de efeitos quando quiser aplicar um ou vários efeitos a uma voz. As cadeias de efeitos podem ser destrutivas, ou seja, cada efeito na cadeia pode substituir o buffer de áudio. Essa propriedade é importante porque o XAudio2 não dá nenhuma garantia de que os buffers de saída sejam inicializados com silêncio. Os objetos do efeito são representados no XAudio2 por objetos XAPO (objetos de processamento de áudio entre plataformas). Para saber mais sobre XAPO, consulte [Visão geral de XAPO](https://docs.microsoft.com/windows/desktop/xaudio2/xapo-overview).

Quando criar uma cadeia de efeitos, siga as etapas abaixo:

1. Criando o objeto de efeito.

2. Preencher uma [XAUDIO2\_efeito\_DESCRITOR](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_descriptor) estrutura com dados em vigor.

3. Preencher uma [XAUDIO2\_efeito\_cadeia](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_chain) estrutura com dados.

4. Aplique a cadeia de efeitos a uma voz.

5. Popule uma estrutura de parâmetro de efeitos e aplique ao efeito.

6. Desabilite ou habilite o efeito quando for adequado.

A classe **Audio** define o método **CreateReverb** para criar a cadeia de efeitos que implementa a reverberação. Esse método chama o método [XAudio2CreateReverb](https://docs.microsoft.com/windows/desktop/api/xaudio2fx/nf-xaudio2fx-xaudio2createreverb) para criar um objeto **ComPtr&lt;IUnknown&gt;** , **soundEffectXAPO**, que age como a voz de submixagem para o efeito de reverberação.

```cpp
Microsoft::WRL::ComPtr<IUnknown> soundEffectXAPO;

DX::ThrowIfFailed(
    XAudio2CreateReverb(&soundEffectXAPO)
    );
```

O [XAUDIO2\_efeito\_DESCRITOR](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_descriptor) estrutura contém informações sobre um XAPO para uso em uma cadeia de efeito, por exemplo, o número de destino de canais de saída. O **Audio::CreateReverb** método cria um **XAUDIO2\_efeito\_DESCRITOR** objeto **soundEffectdescriptor**, que é definido como o desabilitado o estado, usa dois canais de saída e as referências **soundEffectXAPO** para o efeito de reverberação. O **soundEffectdescriptor** começa no estado desabilitado porque o jogo deve definir parâmetros antes do efeito começar a modificar os sons do jogo. O Marble Maze usa dois canais de saída para simplificar a lógica do efeito de reverberação.

```cpp
soundEffectdescriptor.InitialState = false;
soundEffectdescriptor.OutputChannels = 2;
soundEffectdescriptor.pEffect = soundEffectXAPO.Get();
```

Se sua cadeia de efeitos tem vários efeitos, cada um deles exige um objeto. O [XAUDIO2\_efeito\_cadeia](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_chain) estrutura contém a matriz de [XAUDIO2\_efeito\_DESCRITOR](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_descriptor) objetos que participam do efeito. O exemplo a seguir mostra como o método **Audio::CreateReverb** especifica o efeito que implementará a reverberação.

```cpp
XAUDIO2_EFFECT_CHAIN soundEffectChain;

// ...

soundEffectChain.EffectCount = 1;
soundEffectChain.pEffectDescriptors = &soundEffectdescriptor;
```

O método **Audio::CreateReverb** chama o método [IXAudio2::CreateSubmixVoice](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createsubmixvoice) para criar a voz de submixagem para o efeito. Especifica o [XAUDIO2\_efeito\_cadeia](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_chain) objeto **soundEffectChain**, para o *pEffectChain* parâmetro para associar o efeito cadeia com a voz. O Marble Maze também especifica dois canais de saída e uma taxa de amostragem de 48 kilohertz.

```cpp
DX::ThrowIfFailed(
    engine->CreateSubmixVoice(newSubmix, 2, 48000, 0, 0, nullptr, &soundEffectChain)
    );
```

> [!TIP]
> Se você quiser anexar uma cadeia de efeitos existente a uma voz de submixagem existente ou substituir a cadeia de efeitos atual, use o método [IXAudio2Voice::SetEffectChain](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-seteffectchain).

O método **Audio::CreateReverb** chama o [IXAudio2Voice::SetEffectParameters](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-seteffectparameters) para definir parâmetros adicionais associados ao efeito. Esse método usa uma estrutura de parâmetro específica ao efeito. Uma [XAUDIO2FX\_REVERBERAÇÃO\_parâmetros](https://docs.microsoft.com/windows/desktop/api/xaudio2fx/ns-xaudio2fx-xaudio2fx_reverb_parameters) objeto **m_reverbParametersSmall**, que contém os parâmetros de efeito de reverberação, é inicializado no  **Audio::Initialize** método porque cada efeito de reverberação compartilha os mesmos parâmetros. O exemplo a seguir mostra como o método **Audio::Initialize** inicializa os parâmetros de reverberação para reverberação à curta distância.

```cpp
m_reverbParametersSmall.ReflectionsDelay = XAUDIO2FX_REVERB_DEFAULT_REFLECTIONS_DELAY;
m_reverbParametersSmall.ReverbDelay = XAUDIO2FX_REVERB_DEFAULT_REVERB_DELAY;
m_reverbParametersSmall.RearDelay = XAUDIO2FX_REVERB_DEFAULT_REAR_DELAY;
m_reverbParametersSmall.PositionLeft = XAUDIO2FX_REVERB_DEFAULT_POSITION;
m_reverbParametersSmall.PositionRight = XAUDIO2FX_REVERB_DEFAULT_POSITION;
m_reverbParametersSmall.PositionMatrixLeft = XAUDIO2FX_REVERB_DEFAULT_POSITION_MATRIX;
m_reverbParametersSmall.PositionMatrixRight = XAUDIO2FX_REVERB_DEFAULT_POSITION_MATRIX;
m_reverbParametersSmall.EarlyDiffusion = 4;
m_reverbParametersSmall.LateDiffusion = 15;
m_reverbParametersSmall.LowEQGain = XAUDIO2FX_REVERB_DEFAULT_LOW_EQ_GAIN;
m_reverbParametersSmall.LowEQCutoff = XAUDIO2FX_REVERB_DEFAULT_LOW_EQ_CUTOFF;
m_reverbParametersSmall.HighEQGain = XAUDIO2FX_REVERB_DEFAULT_HIGH_EQ_GAIN;
m_reverbParametersSmall.HighEQCutoff = XAUDIO2FX_REVERB_DEFAULT_HIGH_EQ_CUTOFF;
m_reverbParametersSmall.RoomFilterFreq = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_FREQ;
m_reverbParametersSmall.RoomFilterMain = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_MAIN;
m_reverbParametersSmall.RoomFilterHF = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_HF;
m_reverbParametersSmall.ReflectionsGain = XAUDIO2FX_REVERB_DEFAULT_REFLECTIONS_GAIN;
m_reverbParametersSmall.ReverbGain = XAUDIO2FX_REVERB_DEFAULT_REVERB_GAIN;
m_reverbParametersSmall.DecayTime = XAUDIO2FX_REVERB_DEFAULT_DECAY_TIME;
m_reverbParametersSmall.Density = XAUDIO2FX_REVERB_DEFAULT_DENSITY;
m_reverbParametersSmall.RoomSize = XAUDIO2FX_REVERB_DEFAULT_ROOM_SIZE;
m_reverbParametersSmall.WetDryMix = XAUDIO2FX_REVERB_DEFAULT_WET_DRY_MIX;
m_reverbParametersSmall.DisableLateField = TRUE;
```

Esse exemplo usa os valores padrão para a maioria dos parâmetros de reverberação, mas define o **DisableLateField** como TRUE para especificar a reverberação à curta distância, o **EarlyDiffusion** como 4 para simular superfícies planas próximas e o **LateDiffusion** como 15 para simular superfícies distantes muito difusas. Superfícies planas próximas fazem com que os ecos cheguem a você mais rapidamente e mais altos; superfícies distantes difusas fazem os ecos chegarem até você mais baixos e lentamente. Você pode testar os valores de reverberação para chegar ao efeito desejado no seu jogo ou usar o método **ReverbConvertI3DL2ToNative** para usar os parâmetros I3DL2 (Interactive 3D Audio Rendering Guidelines Level 2.0), padrão de fábrica.

O exemplo a seguir mostra como o **Audio::CreateReverb** define os parâmetros de reverberação. **newSubmix** é um objeto [IXAudio2SubmixVoice](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2submixvoice)**. **parâmetros** é um [XAUDIO2FX\_REVERBERAÇÃO\_parâmetros](https://docs.microsoft.com/windows/desktop/api/xaudio2fx/ns-xaudio2fx-xaudio2fx_reverb_parameters)* objeto.

```cpp
DX::ThrowIfFailed(
    (*newSubmix)->SetEffectParameters(0, parameters, sizeof(m_reverbParametersSmall))
    );
```

O método **Audio::CreateReverb** finaliza habilitando o efeito usando [IXAudio2Voice::EnableEffect](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-enableeffect) caso o sinalizador **enableEffect** esteja definido. Também define seu volume usando [IXAudio2Voice::SetVolume](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-setvolume) e a matriz de saída usando [IXAudio2Voice::SetOutputMatrix](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-setoutputmatrix). Essa parte define o volume para máximo (1.0) e especifica a matriz do volume para ser silenciada tanto nas entradas esquerda e direita quanto nos auto-falantes de saída esquerdo e direito. Fazemos isso porque outro código faz, posteriormente, fading cruzado entre as duas reverberações (simulando a transição de estar próximo a uma parede a estar em uma sala grande) ou ativa mudo de ambas as reverberações, se necessário. Quando o mudo do caminho de reverberação é desativado, o jogo define uma matriz de {1.0f, 0.0f, 0.0f, 1.0f} para enviar a saída de reverberação esquerda para a entrada esquerda da voz de masterização e a saída de reverberação direita para a entrada direita da voz de masterização.

```cpp
if (enableEffect)
{
    DX::ThrowIfFailed(
        (*newSubmix)->EnableEffect(0)
        );    
}

DX::ThrowIfFailed(
    (*newSubmix)->SetVolume (1.0f)
    );

float outputMatrix[4] = {0, 0, 0, 0};
DX::ThrowIfFailed(
    (*newSubmix)->SetOutputMatrix(masteringVoice, 2, 2, outputMatrix)
    );
```

O Marble Maze chama o método **Audio::CreateReverb** quatro vezes: duas vezes para a música em segundo plano e duas vezes para os sons do jogo. O exemplo a seguir mostra como o Marble Maze chama o método **CreateReverb** para a música em segundo plano.

```cpp
CreateReverb(
    m_musicEngine, 
    m_musicMasteringVoice, 
    &m_reverbParametersSmall, 
    &m_musicReverbVoiceSmallRoom, 
    true
    );
CreateReverb(
    m_musicEngine, 
    m_musicMasteringVoice, 
    &m_reverbParametersLarge, 
    &m_musicReverbVoiceLargeRoom, 
    true
    );
```

Para ver uma lista de possíveis fontes de efeitos para usar com o XAudio2, consulte [XAudio2 Audio Effects](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-audio-effects).

### <a name="loading-audio-data-from-file"></a>Carregando dados de áudio do arquivo

O Marble Maze define a classe **MediaStreamer**, que usa o Media Foundation para carregar os recursos de áudio dos arquivos. O Marble Maze usa um objeto **MediaStreamer** para carregar cada arquivo de áudio.

O Marble Maze chama o método **MediaStreamer::Initialize** para inicializar cada stream de áudio. É assim que o método **Audio::CreateResources** chama o **MediaStreamer::Initialize** para inicializar o stream de áudio para a música em segundo plano:

```cpp
// Media Foundation is a convenient way to get both file I/O and format decode for 
// audio assets. You can replace the streamer in this sample with your own file I/O 
// and decode routines.
m_musicStreamer.Initialize(L"Media\\Audio\\background.wma");
```

O método **MediaStreamer::Initialize** inicia chamando o método [MFStartup](https://docs.microsoft.com/windows/desktop/api/mfapi/nf-mfapi-mfstartup) para inicializar o Media Foundation. **MF_VERSION** é uma macro definida em **mfapi.h**, além de ser o que especificamos como a versão do Media Foundation a ser usada.

```cpp
DX::ThrowIfFailed(
    MFStartup(MF_VERSION)
    );
```

**MediaStreamer::Initialize** , em seguida, chama o [MFCreateSourceReaderFromURL](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-mfcreatesourcereaderfromurl) para criar um objeto [IMFSourceReader](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nn-mfreadwrite-imfsourcereader). Um objeto **IMFSourceReader**, **m_reader**, lê os dados de mídia do arquivo especificado por uma **url**.

```cpp
DX::ThrowIfFailed(
    MFCreateSourceReaderFromURL(url, nullptr, &m_reader)
    );
```

O método **MediaStreamer::Initialize**, então, cria um objeto [IMFMediaType](https://docs.microsoft.com/windows/desktop/api/mfobjects/nn-mfobjects-imfmediatype) usando [MFCreateMediaType](https://docs.microsoft.com/windows/desktop/api/mfapi/nf-mfapi-mfcreatemediatype) para descrever o formato do stream de áudio. Um formato de áudio tem dois tipos: um tipo principal e um subtipo. O tipo principal define o formato geral da mídia, como vídeo, áudio, script e assim por diante. O subtipo define o formato, como PCM, ADPCM ou WMA.

O **MediaStreamer::Initialize** método usa o [IMFAttributes::SetGUID](https://docs.microsoft.com/windows/desktop/api/mfobjects/nf-mfobjects-imfattributes-setguid) método para especificar o tipo principal ([MF_MT_MAJOR_TYPE](https://docs.microsoft.com/windows/desktop/medfound/mf-mt-major-type-attribute)) como áudio ( **MFMediaType\_áudio**) e o tipo de pequeno ([MF_MT_SUBTYPE](https://docs.microsoft.com/windows/desktop/medfound/mf-mt-subtype-attribute)) como áudio descompactados de PCM (**MFAudioFormat\_PCM**). **MF_MT_MAJOR_TYPE** e **MF_MT_SUBTYPE** são [Atributos do Media Foundation](https://docs.microsoft.com/windows/desktop/medfound/media-foundation-attributes). **MFMediaType_Audio** e **MFAudioFormat_PCM** são GUIDs de tipo e subtipo; consulte [Audio Media Types](https://docs.microsoft.com/windows/desktop/medfound/audio-media-types) para obter mais informações. O método [IMFSourceReader::SetCurrentMediaType](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentmediatype) associa o tipo de mídia ao leitor de stream.

```cpp
// Set the decoded output format as PCM. 
// XAudio2 on Windows can process PCM and ADPCM-encoded buffers. 
// When this sample uses Media Foundation, it always decodes into PCM.

DX::ThrowIfFailed(
    MFCreateMediaType(&mediaType)
    );

DX::ThrowIfFailed(
    mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
    );

DX::ThrowIfFailed(
    mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
    );

DX::ThrowIfFailed(
    m_reader->SetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, 0, mediaType.Get())
    );
```

O método **MediaStreamer::Initialize** obtém, então, o formato de mídia de saída completo do Media Foundation usando [IMFSourceReader::GetCurrentMediaType](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getcurrentmediatype) e chama o método [MFCreateWaveFormatExFromMFMediaType](https://docs.microsoft.com/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype) para converter o tipo de mídia de áudio do Media Foundation para uma estrutura [WAVEFORMATEX](https://docs.microsoft.com/windows/desktop/api/mmreg/ns-mmreg-twaveformatex). A estrutura **WAVEFORMATEX** define o formato dos dados de áudio em ondas. O Marble Maze usa essa estrutura para criar as vozes de origem e aplicar o filtro passa baixa para o som de rolagem da bola de gude.

```cpp
// Get the complete WAVEFORMAT from the Media Type.
DX::ThrowIfFailed(
    m_reader->GetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, &outputMediaType)
    );

uint32 formatSize = 0;
WAVEFORMATEX* waveFormat;
DX::ThrowIfFailed(
    MFCreateWaveFormatExFromMFMediaType(outputMediaType.Get(), &waveFormat, &formatSize)
    );
CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
CoTaskMemFree(waveFormat);
```

> [!IMPORTANT]
> O método [MFCreateWaveFormatExFromMFMediaType](https://docs.microsoft.com/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype) usa o **CoTaskMemAlloc** para alocar o objeto [WAVEFORMATEX](https://docs.microsoft.com/windows/desktop/api/mmreg/ns-mmreg-twaveformatex). Portanto, tenha certeza de chamar o **CoTaskMemFree** quando tiver terminado de usar esse objeto.

 

O **MediaStreamer::Initialize** método termina ao calcular o comprimento do fluxo **m\_maxStreamLengthInBytes**, em bytes. Para isso, ele chama o método [IMFSourceReader::GetPresentationAttribute](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getpresentationattribute) para obter a duração do stream de áudio em 100 unidades de nanossegundos, converte a duração em seções e, em seguida, multiplica pela taxa de transferência média em bytes por segundo. O Marble Maze usa esse valor posteriormente para alocar o buffer que armazena cada som do jogo.

```cpp
// Get the total length of the stream, in bytes.
PROPVARIANT var;
DX::ThrowIfFailed(
    m_reader->
        GetPresentationAttribute(MF_SOURCE_READER_MEDIASOURCE, MF_PD_DURATION, &var)
    );

// duration is in 100ns units; convert to seconds, and round up
// to the nearest whole byte.
ULONGLONG duration = var.uhVal.QuadPart;
m_maxStreamLengthInBytes =
    static_cast<unsigned int>(
        ((duration * static_cast<ULONGLONG>(m_waveFormat.nAvgBytesPerSec)) + 10000000)
        / 10000000
        );
```

### <a name="creating-the-source-voices"></a>Criando vozes de origem

O Marble Maze cria vozes de origem do XAudio2 para reproduzir cada um dos sons do jogo e música nas vozes de origem. A classe **Audio** define um objeto [IXAudio2SourceVoice](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2sourcevoice) object para a música em segundo plano e uma matriz de objetos **SoundEffectData** para armazenar os sons do jogo. A estrutura **SoundEffectData** armazena o objeto **IXAudio2SourceVoice** para um efeito e também define outros dados relacionados a efeitos, como o buffer de áudio. O **Audio.h** define a enumeração **SoundEvent**. O Marble Maze usa essa enumeração para identificar cada som do jogo. A classe **Audio** também usa essa enumeração para indexar a matriz de objetos **SoundEffectData**.

```cpp
enum SoundEvent
{
    RollingEvent        = 0,
    FallingEvent        = 1,
    CollisionEvent      = 2,
    CheckpointEvent     = 3,
    MenuChangeEvent     = 4,
    MenuSelectedEvent   = 5,
    LastSoundEvent,
};
```

A tabela a seguir mostra a relação entre cada um desses valores, o arquivo que contém os dados de som associados e uma breve descrição do que cada som representa. Os arquivos de áudio estão localizados na  **\\Media\\áudio** pasta.

| Valor de SoundEvent  | Nome do arquivo      | Descrição                                              |
|-------------------|----------------|----------------------------------------------------------|
| RollingEvent      | MarbleRoll.wav | Executado enquanto a bolinha rola.                              |
| FallingEvent      | MarbleFall.wav | Executado quando a bolinha cai no labirinto.               |
| CollisionEvent    | MarbleHit.wav  | Executado quando a bolinha colide com o labirinto.           |
| CheckpointEvent   | Checkpoint.wav | Executado quando a bolinha passar por um ponto de controle.         |
| MenuChangeEvent   | MenuChange.wav | Executado quando o usuário muda o item do menu atual. |
| MenuSelectedEvent | MenuSelect.wav | Reproduzido quando o usuário seleciona um item no menu.           |

 

O exemplo a seguir mostra como o método **Audio::CreateResources** cria a voz de origem para a música em segundo plano. O [XAUDIO2\_enviar\_DESCRITOR](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_send_descriptor) estrutura define a voz de destino de destino de outro voz e especifica se um filtro deve ser usado. O Marble Maze chama o método **Audio::SetSoundEffectFilter** para usar os filtros e mudar o som da bola conforme ela rola. O [XAUDIO2\_voz\_ENVIA](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_voice_sends) estrutura define o conjunto de vozes receba dados de uma voz única saída. O Marble Maze envia dados da voz de origem para a voz de masterização (para a parte seca ou inalterada de um som em execução) e para as duas vozes de submix que implementam a parte molhada ou reverberante de um som em execução.

O método [IXAudio2::CreateSourceVoice](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createsourcevoice) cria e configura a voz de origem. É necessária uma estrutura [WAVEFORMATEX](https://docs.microsoft.com/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) que define o formato dos buffers de áudio enviados para a voz. Como dito anteriormente, o Marble Maze usa o formato PCM.

```cpp
XAUDIO2_SEND_DESCRIPTOR descriptors[3];
descriptors[0].pOutputVoice = m_musicMasteringVoice;
descriptors[0].Flags = 0;
descriptors[1].pOutputVoice = m_musicReverbVoiceSmallRoom;
descriptors[1].Flags = 0;
descriptors[2].pOutputVoice = m_musicReverbVoiceLargeRoom;
descriptors[2].Flags = 0;
XAUDIO2_VOICE_SENDS sends = {0};
sends.SendCount = 3;
sends.pSends = descriptors;
WAVEFORMATEX& waveFormat = m_musicStreamer.GetOutputWaveFormatEx();

DX::ThrowIfFailed(
    m_musicEngine->CreateSourceVoice(&m_musicSourceVoice, &waveFormat, 0, 1.0f, &m_voiceContext, &sends, nullptr)
    );

DX::ThrowIfFailed(
    m_musicMasteringVoice->SetVolume(0.4f)
    );
```

## <a name="playing-background-music"></a>Executando música em segundo plano


Uma voz de origem é criada no estado parado. O Marble Maze inicia a música em segundo plano no loop do jogo. A primeira chamada para o **MarbleMazeMain::Update** chama o **Audio::Start** para iniciar a música em segundo plano.

```cpp
if (!m_audio.m_isAudioStarted)
{
    m_audio.Start();
}
```

O método **Audio::Start** chama o [IXAudio2SourceVoice::Start](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start) para começar a processar a voz de origem para a música em segundo plano.

```cpp
void Audio::Start()
{     
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    HRESULT hr = m_musicSourceVoice->Start(0);

    if SUCCEEDED(hr) {
        m_isAudioStarted = true;
    }
    else
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

A voz de origem passa esses dados de áudio para o próximo estágio do gráfico de áudio. No caso do Marble Maze, o próximo estágio contém duas vozes de submixagem que aplicam os dois efeitos de reverberação ao áudio.  Uma voz de submixagem aplica uma reverberação tardia a curta distância; a segundo aplica uma reverberação tardia a longa distância.

A intensidade com que cada voz de submixagem contribui para a combinação final é determinada pelo tamanho e pela forma da sala. A reverberação a curta distância contribui mais quando a bola está próxima de uma parede ou em uma sala pequena, e a reverberação a longa distância contribui mais quando a bola está em um espaço grande. Essa técnica produz um efeito de eco mais realista quando a bolinha se movimenta pelo labirinto. Para saber mais sobre como o Marble Maze implementa esse efeito, consulte **Audio::SetRoomSize** and **Physics::CalculateCurrentRoomSize** no código-fonte do in the Marble Maze.

> [!NOTE]
> Em um jogo no qual a maioria dos tamanhos de sala é relativamente a mesma, é possível usar um modelo de reverberação mais básico. Por exemplo, você pode usar uma configuração de reverberação para todos os ambientes ou pode criar uma configuração de reverberação pré-definida para cada ambiente.

O método **Audio::CreateResources** usa o Media Foundation para carregar a música em segundo plano. A esta altura, no entanto, a voz de origem não tem dados de áudio com os quais trabalhar. Além disso, como a música em segundo plano fica em loop, a voz de origem deve ser atualizada regularmente com dados para que a música continue sendo executada.

Para manter a voz de origem com os dados, a atualização do loop do jogo atualiza o buffer do áudio a cada quadro. O método **MarbleMazeMain::Render** chama o **Audio:: Render** para processar o buffer de áudio da música em segundo plano. O **áudio** classe define uma matriz de três buffers de áudio, **m\_audioBuffers**. Cada buffer armazena 64 KB (65536 bytes) de dados. O loop lê dados do objeto do Media Foundation e os escreve na voz de origem até que ela tenha três buffers em fila.

> [!CAUTION]
> Embora o Marble Maze use um buffer de 64 KB para armazenar dados de música, talvez você precise usar um buffer maior ou menor. A quantidade depende dos requisitos do jogo.

```cpp
// This sample processes audio buffers during the render cycle of the application.
// As long as the sample maintains a high-enough frame rate, this approach should
// not glitch audio. In game code, it is best for audio buffers to be processed
// on a separate thread that is not synced to the main render loop of the game.
void Audio::Render()
{
    if (m_engineExperiencedCriticalError)
    {
        m_engineExperiencedCriticalError = false;
        ReleaseResources();
        Initialize();
        CreateResources();
        Start();
        if (m_engineExperiencedCriticalError)
        {
            return;
        }
    }

    try
    {
        bool streamComplete;
        XAUDIO2_VOICE_STATE state;
        uint32 bufferLength;
        XAUDIO2_BUFFER buf = {0};

        // Use MediaStreamer to stream the buffers.
        m_musicSourceVoice->GetState(&state);
        while (state.BuffersQueued <= MAX_BUFFER_COUNT - 1)
        {
            streamComplete = m_musicStreamer.GetNextBuffer(
                m_audioBuffers[m_currentBuffer],
                STREAMING_BUFFER_SIZE,
                &bufferLength
                );

            if (bufferLength > 0)
            {
                buf.AudioBytes = bufferLength;
                buf.pAudioData = m_audioBuffers[m_currentBuffer];
                buf.Flags = (streamComplete) ? XAUDIO2_END_OF_STREAM : 0;
                buf.pContext = 0;
                DX::ThrowIfFailed(
                    m_musicSourceVoice->SubmitSourceBuffer(&buf)
                    );

                m_currentBuffer++;
                m_currentBuffer %= MAX_BUFFER_COUNT;
            }

            if (streamComplete)
            {
                // Loop the stream.
                m_musicStreamer.Restart();
                break;
            }

            m_musicSourceVoice->GetState(&state);
        }
    }
    catch (...)
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

O loop também age quando o objeto do Media Foundation atinge o fim do stream. Nesse caso, ele chama o método [IMFSourceReader::SetCurrentPosition](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentposition) para redefinir a posição da fonte de áudio.

```cpp
void MediaStreamer::Restart()
{
    if (m_reader == nullptr)
    {
        return;
    }

    PROPVARIANT var = {0};
    var.vt = VT_I8;

    DX::ThrowIfFailed(
        m_reader->SetCurrentPosition(GUID_NULL, var)
        );
}
```

Para implementar o looping de áudio para um único buffer (ou um som inteiro que é totalmente carregado na memória), você pode definir a [XAUDIO2_BUFFER](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_buffer):: campo LoopCount **XAUDIO2\_LOOP\_infinito** quando você inicializar o som. O Marble Maze usa essa técnica para reproduzir o som de rolagem da bola de gude.

```cpp
if (sound == RollingEvent)
{
    m_soundEffects[sound].m_audioBuffer.LoopCount = XAUDIO2_LOOP_INFINITE;
}
```

No entanto, para a música em segundo plano, o Marble Maze gerencia os buffers diretamente de forma que ele possa controlar melhor a quantidade de memória usada. Quando os arquivos de música são grandes, é possível transmitir os dados de música em buffers menores. Isso ajuda a equilibrar o tamanho da memória com a frequência da capacidade do jogo de processar e transmitir dados de áudio.

> [!TIP]
> Se o jogo tiver uma taxa de quadros baixa ou variável, o processamento de áudio no thread principal poderá produzir pausas ou interrupções inesperadas no áudio porque o mecanismo de áudio tem dados de áudio insuficientes em buffer com os quais trabalhar. Se o jogo for sensível a esse problema, considere processar áudio em um thread separado que não execute a renderização. Essa abordagem é útil principalmente em computadores que têm processadores múltiplos, porque o seu jogo pode usar os que estiverem ociosos.

## <a name="reacting-to-game-events"></a>Reagindo a eventos de jogos

A classe **Audio** fornece métodos como **PlaySoundEffect**, **IsSoundEffectStarted**, **StopSoundEffect**, **SetSoundEffectVolume**, **SetSoundEffectPitch**, e **SetSoundEffectFilter** para permitir que o jogo controle quando sons são reproduzidos e são interrompidos e as propriedades de som, como volume e timbre. Por exemplo, se a bola de gude cair do labirinto, o **MarbleMazeMain::Update** chama o método **Audio::PlaySoundEffect** para tocar o som **FallingEvent**.

```cpp
m_audio.PlaySoundEffect(FallingEvent);
```

O método **Audio::PlaySoundEffect** chama o método [IXAudio2SourceVoice::Start](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start) para iniciar a reprodução do som. Se o método **IXAudio2SourceVoice::Start** já tiver sido chamado, ele não iniciará de novo. O **Audio::PlaySoundEffect**, em seguida, executa a lógica personalizada de certos sons.

```cpp
void Audio::PlaySoundEffect(SoundEvent sound)
{
    XAUDIO2_BUFFER buf = {0};
    XAUDIO2_VOICE_STATE state = {0};

    if (m_engineExperiencedCriticalError)
    {
        // If there's an error, then we'll recreate the engine on the next
        // render pass.
        return;
    }

    SoundEffectData* soundEffect = &m_soundEffects[sound];
    HRESULT hr = soundEffect->m_soundEffectSourceVoice->Start();

    if FAILED(hr)
    {
        m_engineExperiencedCriticalError = true;
        return;
    }

    // For one-off voices, submit a new buffer if there's none queued up,
    // and allow up to two collisions to be queued up. 
    if (sound != RollingEvent)
    {
        XAUDIO2_VOICE_STATE state = {0};

        soundEffect->m_soundEffectSourceVoice->
            GetState(&state, XAUDIO2_VOICE_NOSAMPLESPLAYED);

        if (state.BuffersQueued == 0)
        {
            soundEffect->m_soundEffectSourceVoice->
                SubmitSourceBuffer(&soundEffect->m_audioBuffer);
        }
        else if (state.BuffersQueued < 2 && sound == CollisionEvent)
        {
            soundEffect->m_soundEffectSourceVoice->
                SubmitSourceBuffer(&soundEffect->m_audioBuffer);
        }

        // For the menu clicks, we want to stop the voice and replay the click
        // right away.
        // Note that stopping and then flushing could cause a glitch due to the
        // waveform not being at a zero-crossing, but due to the nature of the 
        // sound (fast and 'clicky'), we don't mind.
        if (state.BuffersQueued > 0 && sound == MenuChangeEvent)
        {
            soundEffect->m_soundEffectSourceVoice->Stop();
            soundEffect->m_soundEffectSourceVoice->FlushSourceBuffers();

            soundEffect->m_soundEffectSourceVoice->
                SubmitSourceBuffer(&soundEffect->m_audioBuffer);

            soundEffect->m_soundEffectSourceVoice->Start();
        }
    }

    m_soundEffects[sound].m_soundEffectStarted = true;
}
```

Para sons além do de rolagem, o método **Audio::PlaySoundEffect** chama o [IXAudio2SourceVoice::GetState](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-getstate) para determinar o número de buffers que a voz de origem está reproduzindo. Ele chama o [IXAudio2SourceVoice::SubmitSourceBuffer](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-submitsourcebuffer) para inserir os dados de áudio do som na fila de entrada da voz, caso não haja buffers ativos. O método **Audio::PlaySoundEffect** também habilita o som de colisão para ser tocado duas vezes seguidas. Isso ocorre, por exemplo, quando a bola de gude colide com um canto do labirinto.

Como já descrito, os usos da classe de áudio a **XAUDIO2\_LOOP\_infinito** sinalizar quando ele inicializa o som para o evento sem interrupção. O som inicia a reprodução em loop na primeira vez em que o **Audio::PlaySoundEffect** for chamado para esse evento. Para simplificar a lógica de reprodução do som de rolagem, o Marble Maze ativa o mudo no som em vez de interrompê-lo. Conforme a bola de gude muda a velocidade, o Marble Maze muda o timbre e o volume do som para dar um efeito mais realista. A seguir, veja como o método **MarbleMazeMain::Update** atualiza o timbre e o volume da bola de gude quando sua velocidade muda e como ativa o mudo no som definindo o volume como zero quando a bola de gude para.

```cpp
// Play the roll sound only if the marble is actually rolling.
if (ci.isRollingOnFloor && volume > 0)
{
    if (!m_audio.IsSoundEffectStarted(RollingEvent))
    {
        m_audio.PlaySoundEffect(RollingEvent);
    }

    // Update the volume and pitch by the velocity.
    m_audio.SetSoundEffectVolume(RollingEvent, volume);
    m_audio.SetSoundEffectPitch(RollingEvent, pitch);

    // The rolling sound has at most 8000Hz sounds, so we linearly
    // ramp up the low-pass filter the faster we go.
    // We also reduce the Q-value of the filter, starting with a
    // relatively broad cutoff and get progressively tighter.
    m_audio.SetSoundEffectFilter(
        RollingEvent,
        600.0f + 8000.0f * volume,
        XAUDIO2_MAX_FILTER_ONEOVERQ - volume*volume
        );
}
else
{
    m_audio.SetSoundEffectVolume(RollingEvent, 0);
}
```

## <a name="reacting-to-suspend-and-resume-events"></a>Reagindo à suspensão e retomada de eventos

[Estrutura de aplicativo do Marble Maze](marble-maze-application-structure.md) descreve como o Marble Maze dá suporte à suspensão e à retomada. Quando o jogo é suspenso, o jogo pausa o áudio. Quando o jogo é retomado, ele retoma o áudio de onde ele parou. Fazemos assim para seguir as melhores práticas de não usar recursos quando não são necessários.

O método **Audio::SuspendAudio** é chamado quando o jogo é suspenso. Esse método chama o método [IXAudio2::StopEngine](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-stopengine) para interromper o áudio. Apesar de o **IXAudio2::StopEngine** interromper todo o áudio imediatamente, ele preserva o gráfico de áudio e seus parâmetros de efeito (por exemplo, o efeito de reverberação que é aplicado quando a bola de gude quica).

```cpp
// Uses the IXAudio2::StopEngine method to stop all audio immediately.  
// It leaves the audio graph untouched, which preserves all effect parameters   
// and effect histories (like reverb effects) voice states, pending buffers,  
// cursor positions and so on. 
// When the engines are restarted, the resulting audio will sound as if it had  
// never been stopped except for the period of silence. 
void Audio::SuspendAudio()
{
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    if (m_isAudioStarted)
    {
        m_musicEngine->StopEngine();
        m_soundEffectEngine->StopEngine();
    }

    m_isAudioStarted = false;
}
```

O método **Audio::ResumeAudio** é chamado quando o jogo é retomado. Esse método usa o método [IXAudio2::StartEngine](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-startengine) para reiniciar o áudio. Devido à chamada do [IXAudio2::StopEngine](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-stopengine) preservar o gráfico de áudio e seus parâmetros de efeito, a saída áudio é retomada de onde parou.

```cpp
// Restarts the audio streams. A call to this method must match a previous call
// to SuspendAudio. This method causes audio to continue where it left off.
// If there is a problem with the restart, the m_engineExperiencedCriticalError
// flag is set. The next call to Render will recreate all the resources and
// reset the audio pipeline.
void Audio::ResumeAudio()
{
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    HRESULT hr = m_musicEngine->StartEngine();
    HRESULT hr2 = m_soundEffectEngine->StartEngine();

    if (FAILED(hr) || FAILED(hr2))
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

## <a name="handling-headphones-and-device-changes"></a>Gerenciando fones de ouvido e trocas de dispositivos

O Marble Maze usa retornos de chamada do mecanismo para administrar falhas no mecanismo XAudio2, por exemplo, quando o dispositivo de áudio é trocado. Uma causa provável de troca de dispositivo é quando o usuário do jogo conecta ou desconecta fones de ouvido. Recomendamos que você implemente o retorno de chamada do mecanismo que administra trocas de dispositivos. Do contrário, seu jogo parará de reproduzir sons quando o usuário plugar ou remover os fones de ouvido, até que o jogo seja reiniciado.

**Audio.h** define a classe **AudioEngineCallbacks**. Essa classe implementa a interface [IXAudio2EngineCallback](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2enginecallback).

```cpp
class AudioEngineCallbacks: public IXAudio2EngineCallback
{
private:
    Audio* m_audio;

public :
    AudioEngineCallbacks(){};
    void Initialize(Audio* audio);

    // Called by XAudio2 just before an audio processing pass begins.
    void _stdcall OnProcessingPassStart(){};

    // Called just after an audio processing pass ends.
    void  _stdcall OnProcessingPassEnd(){};

    // Called when a critical system error causes XAudio2
    // to be closed and restarted. The error code is given in Error.
    void  _stdcall OnCriticalError(HRESULT Error);
};
```

A interface [IXAudio2EngineCallback](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2enginecallback) habilita seu código para ser notificado quando eventos de processamento de áudio ocorrerem ou quando o mecanismo encontrar um erro crítico. Para registrar retornos de chamadas, o Marble Maze chama o método [IXAudio2::RegisterForCallbacks](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-registerforcallbacks) em **Audio::CreateResources**, depois de criar um objeto [IXAudio2](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) para o mecanismo de música.

```cpp
m_musicEngineCallback.Initialize(this);
m_musicEngine->RegisterForCallbacks(&m_musicEngineCallback);
```

O Marble Maze não exige notificação quando o processamento de áudio iniciar ou finalizar. Portanto, ele implementa os métodos [IXAudio2EngineCallback::OnProcessingPassStart](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2enginecallback-onprocessingpassstart) e [IXAudio2EngineCallback::OnProcessingPassEnd](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2enginecallback-onprocessingpassend) para não fazer nada. Para o [IXAudio2EngineCallback::OnCriticalError](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2enginecallback-oncriticalerror) método, chamadas de labirinto de mármore a **SetEngineExperiencedCriticalError** método, que define os **m\_ engineExperiencedCriticalError** sinalizador.

```cpp
// Audio.cpp

// Called when a critical system error causes XAudio2 
// to be closed and restarted. The error code is given in Error. 
void  _stdcall AudioEngineCallbacks::OnCriticalError(HRESULT Error)
{
    m_audio->SetEngineExperiencedCriticalError();
}
```

```cpp
// Audio.h (Audio class)

// This flag can be used to tell when the audio system 
// is experiencing critial errors.
// XAudio2 gives a critical error when the user unplugs
// the headphones and a new speaker configuration is generated.
void SetEngineExperiencedCriticalError()
{
    m_engineExperiencedCriticalError = true;
}
```

Quando ocorre um erro crítico, o processamento de áudio para e todas as chamadas adicionais pelo XAudio2 falham. Para recuperar-se desse problema, você deve liberar a instância do XAudio2 e criar uma nova. O **Audio::Render** método, que é chamado do loop do jogo cada quadro, primeiro verifica o **m\_engineExperiencedCriticalError** sinalizador. Se esse sinalizador estiver definido, ele limpa o sinalizador, lança a instância atual do XAudio2, inicializa recursos e, em seguida, inicia a música em segundo plano.

```cpp
if (m_engineExperiencedCriticalError)
{
    m_engineExperiencedCriticalError = false;
    ReleaseResources();
    Initialize();
    CreateResources();
    Start();
    if (m_engineExperiencedCriticalError)
    {
        return;
    }
}
```

O Marble Maze usa também o **m\_engineExperiencedCriticalError** sinalizador para se proteger contra chamando o XAudio2 quando nenhum dispositivo de áudio está disponível. Por exemplo, o método **MarbleMazeMain::Update** não processa áudio para eventos de rolagem ou colisão quando o sinalizador está definido. O aplicativo tenta reparar o mecanismo de áudio cada quadro, se for necessário; No entanto, o **m\_engineExperiencedCriticalError** sinalizador sempre pode ser definido se o computador não tiver um dispositivo de áudio ou os fones de ouvido estão desconectados, e não há nenhum outro dispositivo de áudio disponível.

> [!CAUTION]
> Como regra geral, não execute operações de bloqueio no corpo de um retorno de chamada do mecanismo. Fazer isso pode causar problemas de desempenho. O Marble Maze define um sinalizador no retorno de chamada **OnCriticalError** e, depois, lida com o erro durante a fase de processamento de áudio regular. Para mais informações sobre retornos de chamada de XAudio2, consulte [XAudio2 Callbacks](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-callbacks).

## <a name="conclusion"></a>Conclusão

Isso conclui o jogo de exemplo Marble Maze! Embora seja um jogo relativamente simples, ele contém muitas das partes importantes que compõem qualquer jogo do DirectX UWP e é um bom exemplo a ser seguido na criação do seu próprio jogo.

Agora que você terminou o acompanhamento, faça experiências com o código-fonte e veja o que acontece. Ou confira [Criar um jogo simples UWP com o DirectX](tutorial--create-your-first-uwp-directx-game.md), outro jogo de exemplo do UWP DirectX.

Pronto para avançar mais com o DirectX? Confira nossos guias em [Programação DirectX](directx-programming.md).

Se você estiver interessado em desenvolvimento na UWP em geral, consulte a documentação em [Programação de jogos](index.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Adicionando entrada e interatividade ao exemplo Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [Desenvolvendo o Marble Maze, um jogo UWP em C++ e DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)