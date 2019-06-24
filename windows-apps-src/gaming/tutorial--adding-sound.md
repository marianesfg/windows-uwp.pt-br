---
title: Adicionar som
description: Desenvolva um mecanismo simple de som usando APIs do XAudio2 para música de jogo de reprodução e efeitos de som.
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, jogos, sons
ms.localizationpriority: medium
ms.openlocfilehash: 06c06e1ffe52cae37a000f748076d78ebf6afff4
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318858"
---
# <a name="add-sound"></a>Adicionar som

Neste tópico, criamos um mecanismo simples de som usando [XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-introduction) APIs. Se você for novo no __XAudio2__, incluímos uma breve introdução sob [conceitos de áudio](#audio-concepts).

>[!Note]
>Se você ainda não tiver baixado o código de jogo mais recente para este exemplo, acesse [Jogo de exemplo em Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Este exemplo faz parte de uma grande coleção de exemplos de recursos UWP. Para obter instruções sobre como baixar o exemplo, consulte [Obtenha os exemplos da Plataforma Universal do Windows (UWP) do GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objetivo

Adicionar sons para o jogo de exemplo usando [XAudio2](/windows/desktop/xaudio2/xaudio2-introduction).

## <a name="define-the-audio-engine"></a>Definir o mecanismo de áudio

No exemplo de jogo, os objetos de áudio e os comportamentos são definidos em três arquivos:

* __[Audio.h](#audioh)/.cpp__: Define o __áudio__ objeto, que contém o __XAudio2__ recursos para reprodução de som. Ele também define o método para suspender e retomar a reprodução de som, caso o jogo seja pausado ou desativado.
* __[MediaReader.h](#mediareaderh)/.cpp__: Define os métodos para ler arquivos de áudio wav do armazenamento local.
* __[SoundEffect.h](#soundeffecth)/.cpp__: Define um objeto para reprodução de som no jogo.

## <a name="overview"></a>Visão geral

Há três partes principais no configurá-lo para reprodução de áudio em seu jogo.

1. [Criar e inicializar os recursos de áudio](#create-and-initialize-the-audio-resources)
2. [Carregar o arquivo de áudio](#load-audio-file)
3. [Associar o som para objeto](#associate-sound-to-object)

Eles são definidos na [Simple3DGame::Initialize](#simple3dgameinitialize-method) método. Então, vamos primeiro examinar esse método e, em seguida, aprofunde-se em mais detalhes em cada uma das seções.

Depois de configurar, aprendemos como disparar os efeitos de som a reproduzir. Para obter mais informações, acesse [reproduzir o som](#play-the-sound).

### <a name="simple3dgameinitialize-method"></a>Método Simple3DGame::Initialize

Na __Simple3DGame::Initialize__, onde __m\_controlador__ e __m\_renderizador__ são também inicializado, podemos configurar o mecanismo de áudio e obtê-lo pronto para reproduzir sons.

 * Crie __m\_audioController__, que é uma instância das [áudio](#audioh) classe.
 * Criar os recursos de áudio necessários usando o [Audio::CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) método. Aqui, dois __XAudio2__ objetos &mdash; um objeto de mecanismo de música e um objeto do mecanismo de som e uma voz dominar para cada um deles foram criados. O objeto de mecanismo de música pode ser usado para reproduzir música em segundo plano para o seu jogo. O mecanismo de som pode ser usado para reproduzir os efeitos de som em seu jogo. Para obter mais informações, consulte [criar e inicializar os recursos de áudio](#create-and-initialize-the-audio-resources).
 * Crie __mediaReader__, que é uma instância do [MediaReader](#mediareaderh) classe. [MediaReader](#mediareaderh), que é uma classe auxiliar para o [SoundEffect](#soundeffecth) classe leituras áudio pequeno arquivos de forma síncrona de local de arquivo e retorna dados som como uma matriz de bytes.
 * Use [MediaReader::LoadMedia](#mediareaderloadmedia-method) para carregar arquivos de som de seu local e criar um __targetHitSound__ variável para reter os dados de som. wav carregado. Para obter mais informações, consulte [carregar o arquivo de áudio](#load-audio-file). 

Efeitos de som são associados com o objeto do jogo. Portanto, quando ocorrer uma colisão com esse objeto do jogo, ele dispara o efeito de som a ser reproduzido. Este exemplo do jogo, temos os efeitos de som para o ammo (o que usamos para solucionar os destinos com) e para o destino. 
    
* No __GameObject__ classe, há um __HitSound__ propriedade que é usada para associar o efeito de som para o objeto.
* Criar uma nova instância dos [SoundEffect](#soundeffecth) de classe e inicializá-lo. Durante a inicialização, uma voz de origem para o efeito de som é criada. 
* Essa classe toca um som usando uma voz dominar fornecida a partir de [áudio](#audioh) classe. Dados de som são lidos no arquivo local usando o [MediaReader](#mediareaderh) classe. Para obter mais informações, consulte [associar som ao objeto](#associate-sound-to-object).

>[!Note]
>O gatilho real para reproduzir o som é determinado pela movimentação e a colisão desses objetos do jogo. Portanto, a chamada para jogar de verdade esses sons são definidos na [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) método. Para obter mais informações, acesse [reproduzir o som](#play-the-sound).

```cpp
void Simple3DGame::Initialize(
    _In_ MoveLookController^ controller,
    _In_ GameRenderer^ renderer
    )
{
    // ...
    // Create a new Audio class object.
    m_audioController = ref new Audio;

    // Create the audio resources needed.
    // Two XAudio2 objects are created - one for music engine,
    // the other for sound engine. A mastering voice is also
    // created for each of the objects.
    m_audioController->CreateDeviceIndependentResources();

    m_ammo = std::vector<Sphere^>(GameConstants::MaxAmmo);

    // ..
    // Create a media reader which is used to read audio files from its file location.
    MediaReader^ mediaReader = ref new MediaReader;
    auto targetHitSound = mediaReader->LoadMedia("Assets\\hit.wav");

    // Instantiate the targets for use in the game.
    // Each target has a different initial position, size, and orientation.
    // But share a common set of material properties.
    for (int a = 1; a < GameConstants::MaxTargets; a++)
    {
        // ...
        // Create a new sound effect object and associate it
        // with the game object's (target) HitSound property.
        target->HitSound(ref new SoundEffect());

        // Initialize the sound effect object with
        // the sound effect engine, format of the audio wave, and audio data
        // During initialization, source voice of this sound effect is also created.
        target->HitSound()->Initialize(
            m_audioController->SoundEffectEngine(),
            mediaReader->GetOutputWaveFormatEx(),
            targetHitSound
            );
        // ...
    }

    // Instantiate a set of spheres to be used as ammunition for the game
    // and set the material properties of the spheres.
    auto ammoHitSound = mediaReader->LoadMedia("Assets\\bounce.wav");

    for (int a = 0; a < GameConstants::MaxAmmo; a++)
    {
        m_ammo[a] = ref new Sphere;
        m_ammo[a]->Radius(GameConstants::AmmoRadius);
        m_ammo[a]->HitSound(ref new SoundEffect());
        m_ammo[a]->HitSound()->Initialize(
            m_audioController->SoundEffectEngine(),
            mediaReader->GetOutputWaveFormatEx(),
            ammoHitSound
            );
        m_ammo[a]->Active(false);
        m_renderObjects.push_back(m_ammo[a]);
    }
    // ...
}
```

## <a name="create-and-initialize-the-audio-resources"></a>Criar e inicializar os recursos de áudio

* Use [XAudio2Create](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create), uma API XAudio2, para criar dois novos objetos XAudio2 que definem os mecanismos de efeito de música e som. Esse método retorna um ponteiro para o objeto [IXAudio2](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) interface que gerencia o mecanismo de áudio de todos os estados, o áudio de processamento de thread, o gráfico de voz e muito mais.
* Depois que os mecanismos de tem sido instanciados, use [IXAudio2::CreateMasteringVoice](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createmasteringvoice) para criar uma voz dominar para cada um dos objetos do mecanismo de som.

Para obter mais informações, vá para [como: Inicializar o XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/how-to--initialize-xaudio2).

### <a name="audiocreatedeviceindependentresources-method"></a>Método Audio::CreateDeviceIndependentResources

```cpp

void Audio::CreateDeviceIndependentResources()
{
    UINT32 flags = 0;

    DX::ThrowIfFailed(
        XAudio2Create(&m_musicEngine, flags)
        );

    HRESULT hr = m_musicEngine->CreateMasteringVoice(&m_musicMasteringVoice);
    if (FAILED(hr))
    {
        // Unable to create an audio device
        m_audioAvailable = false;
        return;
    }

    DX::ThrowIfFailed(
        XAudio2Create(&m_soundEffectEngine, flags)
        );

    DX::ThrowIfFailed(
        m_soundEffectEngine->CreateMasteringVoice(&m_soundEffectMasteringVoice)
        );

    m_audioAvailable = true;
}
```

## <a name="load-audio-file"></a>Carregar o arquivo de áudio

No exemplo de jogo, o código para ler arquivos de formato de áudio é definido no [MediaReader.h](#mediareaderh)/cpp__.  Para ler um arquivo de áudio. wav codificada, chame [MediaReader::LoadMedia](#mediareaderloadmedia-method), passando o nome do arquivo a. wav como o parâmetro de entrada.

### <a name="mediareaderloadmedia-method"></a>Método MediaReader::LoadMedia

Esse método usa as APIs [Media Foundation](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk) para ler o arquivo de áudio .wav como um buffer de modulação por código de pulso (PCM).

#### <a name="set-up-the-source-reader"></a>Configurar o leitor de código-fonte

1. Use [MFCreateSourceReaderFromURL](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-mfcreatesourcereaderfromurl) para criar uma mídia de leitor de código-fonte ([IMFSourceReader](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nn-mfreadwrite-imfsourcereader)).
2. Use [MFCreateMediaType](https://docs.microsoft.com/windows/desktop/api/mfapi/nf-mfapi-mfcreatemediatype) para criar um tipo de mídia ([IMFMediaType](https://docs.microsoft.com/windows/desktop/api/mfobjects/nn-mfobjects-imfmediatype)) objeto (_mediaType_). Ele representa uma descrição de um formato de mídia. 
3. Especificar que o _mediaType_da saída de decodificado é áudio PCM, que é áudio de um tipo que __XAudio2__ pode usar.
4. Conjuntos de mídia de saída decodificado de tipo para o leitor de código-fonte chamando [IMFSourceReader::SetCurrentMediaType](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentmediatype).

Para obter mais informações sobre por que podemos usar o leitor de código-fonte, acesse [leitor de código-fonte](https://docs.microsoft.com/windows/desktop/medfound/source-reader).

#### <a name="describe-the-data-format-of-the-audio-stream"></a>Descrever o formato de dados de fluxo de áudio

1. Use [IMFSourceReader::GetCurrentMediaType](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getcurrentmediatype) para obter o tipo de mídia atual no fluxo.
2. Use [IMFMediaType::MFCreateWaveFormatExFromMFMediaType](https://docs.microsoft.com/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype) para converter a mídia de áudio atual de tipo para um [WAVEFORMATEX](https://docs.microsoft.com/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) do buffer, usando os resultados da operação anterior como entrada. Essa estrutura Especifica o formato de dados de fluxo de áudio wave é usado depois que o áudio é carregado. 

O __WAVEFORMATEX__ formato pode ser usado para descrever o buffer PCM. Em comparação com o [WAVEFORMATEXTENSIBLE](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/ksmedia/ns-ksmedia-waveformatextensible) estrutura, ele só pode ser usado para descrever um subconjunto de formatos de áudio wave. Para obter mais informações sobre as diferenças entre __WAVEFORMATEX__ e __WAVEFORMATEXTENSIBLE__, consulte [extensível descritores de formato Wave](https://docs.microsoft.com/windows-hardware/drivers/audio/extensible-wave-format-descriptors).

#### <a name="read-the-audio-stream"></a>Ler o fluxo de áudio

1.  Obter a duração, em segundos, do fluxo de áudio, chamando [IMFSourceReader::GetPresentationAttribute](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getpresentationattribute) e, em seguida, converte a duração em bytes.
2.  Ler o arquivo de áudio no como um fluxo chamando [IMFSourceReader::ReadSample](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-readsample). __ReadSample__ lê o próximo exemplo da fonte de mídia.
3.  Use [IMFSample::ConvertToContiguousBuffer](https://docs.microsoft.com/windows/desktop/api/mfobjects/nf-mfobjects-imfsample-converttocontiguousbuffer) para copiar o conteúdo do buffer de amostra de áudio (_amostra_) em uma matriz (_mediaBuffer_).

```cpp
Platform::Array<byte>^ MediaReader::LoadMedia(_In_ Platform::String^ filename)
{
    DX::ThrowIfFailed(
        MFStartup(MF_VERSION)
        );

    // Creates a media source reader.
    ComPtr<IMFSourceReader> reader;
    DX::ThrowIfFailed(
        MFCreateSourceReaderFromURL(
            Platform::String::Concat(m_installedLocationPath, filename)->Data(),
            nullptr,
            &reader
            )
        );

    // Set the decoded output format as PCM.
    // XAudio2 on Windows can process PCM and ADPCM-encoded buffers.
    // When using MediaFoundation, this sample always decodes into PCM.
    Microsoft::WRL::ComPtr<IMFMediaType> mediaType;
    DX::ThrowIfFailed(
        MFCreateMediaType(&mediaType)
        );

    // Define the major category of the media as audio. For more info about major media types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa367377.aspx
    DX::ThrowIfFailed(
        mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
        );

    // Define the sub-type of the media as uncompressed PCM audio. For more info about audio sub-types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa372553.aspx
    DX::ThrowIfFailed(
        mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
        );
    
    // Sets the media type for a stream. This media type defines that format that the Source Reader 
    // produces as output. It can differ from the native format provided by the media source.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/dd374667.aspx
    DX::ThrowIfFailed(
        reader->SetCurrentMediaType(static_cast<uint32>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), 0, mediaType.Get())
        );

    // Get the current media type for the stream.
    // For more info, go to:
    // https://msdn.microsoft.com/library/windows/desktop/dd374660.aspx
        Microsoft::WRL::ComPtr<IMFMediaType> outputMediaType;
    DX::ThrowIfFailed(
        reader->GetCurrentMediaType(static_cast<uint32>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), &outputMediaType)
        );

    // Converts the current media type into the WaveFormatEx buffer structure.
    UINT32 size = 0;
    WAVEFORMATEX* waveFormat;
    DX::ThrowIfFailed(
        MFCreateWaveFormatExFromMFMediaType(outputMediaType.Get(), &waveFormat, &size)
        );

    // Copies the waveFormat's block of memory to the starting address of the m_waveFormat variable in MediaReader.
    // Then free the waveFormat memory block.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/aa366535.aspx and
    // https://msdn.microsoft.com/library/windows/desktop/ms680722.aspx
    CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
    CoTaskMemFree(waveFormat);

    PROPVARIANT propVariant;
    DX::ThrowIfFailed(
        reader->GetPresentationAttribute(static_cast<uint32>(MF_SOURCE_READER_MEDIASOURCE), MF_PD_DURATION, &propVariant)
        );

    // 'duration' is in 100ns units; convert to seconds, and round up
    // to the nearest whole byte.
    LONGLONG duration = propVariant.uhVal.QuadPart;
    unsigned int maxStreamLengthInBytes =
        static_cast<unsigned int>(
            ((duration * static_cast<ULONGLONG>(m_waveFormat.nAvgBytesPerSec)) + 10000000) /
            10000000
            );

    Platform::Array<byte>^ fileData = ref new Platform::Array<byte>(maxStreamLengthInBytes);

    ComPtr<IMFSample> sample;
    ComPtr<IMFMediaBuffer> mediaBuffer;
    DWORD flags = 0;

    int positionInData = 0;
    bool done = false;
    while (!done)
    {
        //...
        // Read audio data.
    }

    return fileData;
}
```
## <a name="associate-sound-to-object"></a>Associar o som para objeto

Sons para o objeto de associação ocorre quando o jogo é inicializado, além de [Simple3DGame::Initialize](#simple3dgameinitialize-method) método.

Recapitulação:
* No __GameObject__ classe, há um __HitSound__ propriedade que é usada para associar o efeito de som para o objeto.
* Criar uma nova instância dos [SoundEffect](#soundeffecth) classe de objeto e associá-la com o objeto do jogo. Essa classe reproduz um som usando __XAudio2__ APIs.  Ele usa uma voz dominar fornecida pelo [áudio](#audioh) classe. Os dados de som podem ser lidos do arquivo local usando o [MediaReader](#mediareaderh) classe.

[SoundEffect::Initialize](#soundeffectinitialize-method) é usado para inicializar o __SoundEffect__ instância com os seguintes parâmetros de entrada: ponteiro para objeto do mecanismo de som (objetos de IXAudio2 criados no [áudio:: CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) método), ponteiro para o formato do arquivo. wav usando __MediaReader::GetOutputWaveFormatEx__, e os dados de som carregados usando [MediaReader::LoadMedia ](#mediareaderloadmedia-method) método. Durante a inicialização, a voz do código-fonte para o efeito de som também é criada.

### <a name="soundeffectinitialize-method"></a>Método SoundEffect::Initialize

```cpp
void SoundEffect::Initialize(
    _In_ IXAudio2 *masteringEngine,
    _In_ WAVEFORMATEX *sourceFormat,
    _In_ Platform::Array<byte>^ soundData)
{
    m_soundData = soundData;

    if (masteringEngine == nullptr)
    {
        // Audio is not available so just return.
        m_audioAvailable = false;
        return;
    }

    // Create a source voice for this sound effect.
    DX::ThrowIfFailed(
        masteringEngine->CreateSourceVoice(
            &m_sourceVoice,
            sourceFormat
            )
        );
    m_audioAvailable = true;
}
```

## <a name="play-the-sound"></a>Reproduzir o som

Gatilhos para reproduzir os efeitos de som são definidos no [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) método porque isso é onde a movimentação dos objetos são atualizados e colisão entre objetos é determinado.

Uma vez que a interação de entre objetos muito, difere dependendo do jogo, não vamos discutir a dinâmica de objetos do jogo aqui. Se você estiver interessado em entender sua implementação, acesse [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) método.

Em princípio, quando uma colisão ocorre, ele dispara o efeito de som a reproduzir chamando **SoundEffect::PlaySound**. Esse método interrompe os efeitos de som que está sendo executado e coloca o buffer de memória com os dados de som desejados. Ele usa voz fonte para definir o volume, enviar dados de som e iniciar a reprodução.

### <a name="soundeffectplaysound-method"></a>Método SoundEffect::PlaySound

* Usa o objeto de fonte de voz **m\_sourceVoice** para iniciar a reprodução do buffer de dados som **m\_soundData**
* Cria uma [XAUDIO2\_BUFFER](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_buffer), para que ele fornece uma referência ao buffer de dados de som e, em seguida, envia-o com uma chamada para [IXAudio2SourceVoice::SubmitSourceBuffer](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-submitsourcebuffer). 
* Com os dados do som em fila **SoundEffect::PlaySound** reproduzir começa chamando [IXAudio2SourceVoice::Start](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start).

```cpp
void SoundEffect::PlaySound(_In_ float volume)
{
    XAUDIO2_BUFFER buffer = {0};
    XAUDIO2_VOICE_STATE state = {0};

    if (!m_audioAvailable)
    {
        // Audio is not available, so just return.
        return;
    }

    // Interrupt sound effect if currently playing.
    DX::ThrowIfFailed(
        m_sourceVoice->Stop()
        );
    DX::ThrowIfFailed(
        m_sourceVoice->FlushSourceBuffers()
        );

    // Queue in-memory buffer for playback and start the voice.
    buffer.AudioBytes = m_soundData->Length;
    buffer.pAudioData = m_soundData->Data;
    buffer.Flags = XAUDIO2_END_OF_STREAM;

    m_sourceVoice->SetVolume(volume);
    DX::ThrowIfFailed(
        m_sourceVoice->SubmitSourceBuffer(&buffer)
        );
    DX::ThrowIfFailed(
        m_sourceVoice->Start()
        );
}
```

### <a name="simple3dgameupdatedynamics-method"></a>Método Simple3DGame::UpdateDynamics

O __Simple3DGame::UpdateDynamics__ método se encarrega de interação e a colisão entre objetos do jogo. Quando objetos colidem (ou interseccionam), ele dispara o efeito de som associado a reproduzir.

```cpp
void Simple3DGame::UpdateDynamics()
{
    // ...
    // Check for collisions between ammo.
#pragma region inter-ammo collision detection
    if (m_ammoCount > 1)
    {
       // ...
       // Check collision between instances One and Two.
       // ...
       if (distanceSquared < (GameConstants::AmmoSize * GameConstants::AmmoSize))
       {
           // The two ammo are intersecting.
           // ...
           // Start playing the sounds for the impact between the two balls.
              m_ammo[one]->PlaySound(impact, m_player->Position());
              m_ammo[two]->PlaySound(impact, m_player->Position());

       }
    }
#pragma endregion

#pragma region Ammo-Object intersections
    // Check for intersections between the ammo and the other objects in the scene.
    // ...
    // Ball is in contact with Object.
    // ...

    // Make sure that the ball is actually headed towards the object. At grazing angles there
    // could appear to be an impact when the ball is actually already hit and moving away.
    if (impact > 0.0f)
    {
        // ...
        // Play the sound associated with the Ammo hitting something.
           m_ammo[one]->PlaySound(impact, m_player->Position());

        if (m_objects[i]->Target() && !m_objects[i]->Hit())
        {
            // The object is a target and isn't currently hit, so mark it as hit and
            // play the sound associated with the impact.
             m_objects[i]->Hit(true);
             m_objects[i]->HitTime(timeTotal);
             m_totalHits++;

             m_objects[i]->PlaySound(impact, m_player->Position());
        }
        // ...
    }
#pragma endregion

#pragma region Apply Gravity and world intersection
    // Apply gravity and check for collision against enclosing volume.
    // ...
    if (position.z < limit)
    {
        // The ammo instance hit the a wall in the min Z direction.
        // Align the ammo instance to the wall, invert the Z component of the velocity and
        // play the impact sound.
           position.z = limit;
           m_ammo[i]->PlaySound(-velocity.z, m_player->Position());
           velocity.z = -velocity.z * GameConstants::Physics::GroundRestitution;
    }
    // ...
#pragma endregion
}
```
## <a name="next-steps"></a>Próximas etapas

Abordamos a UWP framework, elementos gráficos, controles, interface do usuário e áudio de um jogo do Windows 10. A próxima parte deste tutorial [estendendo o exemplo do jogo](tutorial-resources.md), explica outras opções que podem ser usadas ao desenvolver um jogo.

## <a name="audio-concepts"></a>Conceitos de áudio

Para o desenvolvimento de jogos do Windows 10, use o XAudio2 versão 2.9. Esta versão é fornecida com o Windows 10. Para obter mais informações, acesse [XAudio2 versões](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-versions).

__AudioX2__ é uma API de nível baixo que fornece processamento de sinais e misturar foundation. Para obter mais informações, consulte [conceitos principais do XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-key-concepts).

### <a name="xaudio2-voices"></a>Vozes XAudio2

Há três tipos de objetos de voz XAudio2: código-fonte, submix e dominar as vozes. As vozes são que os objetos XAudio2 usam para processar, manipular e reproduzir dados de áudio. 
* As vozes de origem operam em dados de áudio fornecidos pelo cliente. 
* As vozes de origem e submixagem enviam sua saída para uma ou mais vozes de submixagem ou masterização. 
* As vozes de submixagem e masterização combinam o áudio de todas as vozes que as alimentam e operam no resultado. 
* Dominar vozes receber dados de vozes de origem e vozes submix e envia esses dados para o hardware de áudio.

Para obter mais informações, acesse [XAudio2 vozes](/windows/desktop/xaudio2/xaudio2-voices).

### <a name="audio-graph"></a>Gráfico de áudio

Gráfico de áudio é uma coleção de [XAudio2 vozes](/windows/desktop/xaudio2/xaudio2-voices). Áudio começa em um dos lados de um gráfico de áudio em vozes de código-fonte, opcionalmente passa por um ou mais vozes de submix e termina em uma voz de dominar. Gráfico de áudio conterá uma voz de origem para cada reprodução do som no momento, vozes submix zero ou mais e uma voz de dominar. O gráfico de áudio mais simples e o mínimo necessário para fazer um barulho no XAudio2, é uma voz de fonte única saída diretamente para uma voz de dominar. Para obter mais informações, acesse [grafos de áudio](https://docs.microsoft.com/windows/desktop/xaudio2/audio-graphs).

### <a name="additional-reading"></a>Leitura adicional

* [Como: Inicializar o XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/how-to--initialize-xaudio2)
* [Como: Carregar arquivos de dados de áudio no XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/how-to--load-audio-data-files-in-xaudio2)
* [Como: Reproduzir um som com XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/how-to--play-a-sound-with-xaudio2)

## <a name="key-audio-h-files"></a>Arquivos de chave. h de áudio

### <a name="audioh"></a>Audio.h

```cpp
// Audio:
// This class uses XAudio2 to provide sound output.  It creates two
// engines - one for music and the other for sound effects - each as
// a separate mastering voice.
// The SuspendAudio and ResumeAudio methods can be used to stop
// and start all audio playback.
public:
    Audio();

    void Initialize();
    void CreateDeviceIndependentResources();
    IXAudio2* MusicEngine();
    IXAudio2* SoundEffectEngine();
    void SuspendAudio();
    void ResumeAudio();

protected:
    // ...
};
```
### <a name="mediareaderh"></a>MediaReader.h

```cpp
// MediaReader:
// This is a helper class for the SoundEffect class.  It reads small audio files
// synchronously from the package installed folder and returns sound data as a
// byte array.

ref class MediaReader
{
internal:
    MediaReader();

    Platform::Array<byte>^          LoadMedia(_In_ Platform::String^ filename);
    WAVEFORMATEX*                   GetOutputWaveFormatEx();

protected private:
    Windows::Storage::StorageFolder^ m_installedLocation;
    Platform::String^               m_installedLocationPath;
    WAVEFORMATEX                    m_waveFormat;
};
```
### <a name="soundeffecth"></a>SoundEffect.h

```cpp
// SoundEffect:
// This class plays a sound using XAudio2.  It uses a mastering voice provided
// from the Audio class.  The sound data can be read from disk using the MediaReader
// class.

ref class SoundEffect
{
internal:
    SoundEffect();

    void Initialize(
        _In_ IXAudio2*              masteringEngine,
        _In_ WAVEFORMATEX*          sourceFormat,
        _In_ Platform::Array<byte>^ soundData
        );

    void PlaySound(_In_ float volume);

protected private:
    //...

};
```


