---
title: Adicionar som
description: Desenvolva um mecanismo de som simples usando APIs XAudio2 para reproduzir música de jogos e efeitos sonoros.
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, jogos, sons
ms.localizationpriority: medium
ms.openlocfilehash: 0e624c750bfce0633bc91d440fd883341b831836
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409645"
---
# <a name="add-sound"></a>Adicionar som

> [!NOTE]
> Este tópico faz parte do [jogo criar um simples plataforma universal do Windows (UWP) com a série de tutoriais do DirectX](tutorial--create-your-first-uwp-directx-game.md) . O tópico nesse link define o contexto para a série.

Neste tópico, criamos um mecanismo de som simples usando APIs [XAudio2](/windows/desktop/xaudio2/xaudio2-introduction) . Se você for novo no __XAudio2__, incluímos uma breve introdução nos [conceitos de áudio](#audio-concepts).

>[!Note]
>Se você não tiver baixado o código de jogo mais recente para este exemplo, vá para [jogo de exemplo do Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Este exemplo faz parte de uma grande coleção de exemplos de recursos UWP. Para obter instruções sobre como baixar o exemplo, consulte [Obtenha os exemplos da Plataforma Universal do Windows (UWP) do GitHub](/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objetivo

Adicione sons ao jogo de exemplo usando [XAudio2](/windows/desktop/xaudio2/xaudio2-introduction).

## <a name="define-the-audio-engine"></a>Definir o mecanismo de áudio

No jogo de exemplo, os objetos de áudio e os comportamentos são definidos em três arquivos:

* __[Audio. h](#audioh)/.cpp__: define o objeto de __áudio__ , que contém os recursos de __XAudio2__ para reprodução de som. Ele também define o método para suspender e retomar a reprodução de som, caso o jogo seja pausado ou desativado.
* __ [MediaReader. h](#mediareaderh)/.cpp__: define os métodos para ler arquivos. wav de áudio do armazenamento local.
* __ [SoundEffect. h](#soundeffecth)/.cpp__: define um objeto para reprodução de som em jogo.

## <a name="overview"></a>Visão geral

Há três partes principais na configuração para a reprodução de áudio em seu jogo.

1. [Criar e inicializar os recursos de áudio](#create-and-initialize-the-audio-resources)
2. [Carregar arquivo de áudio](#load-audio-file)
3. [Associar som ao objeto](#associate-sound-to-object)

Eles são todos definidos no método [Simple3DGame:: Initialize](#simple3dgameinitialize-method) . Vamos primeiro examinar esse método e, em seguida, aprofundar-se em mais detalhes em cada uma das seções.

Depois de configurar, aprenderemos como disparar os efeitos de som a serem reproduzidos. Para obter mais informações, vá para [reproduzir o som](#play-the-sound).

### <a name="simple3dgameinitialize-method"></a>Método Simple3DGame:: Initialize

Em __Simple3DGame:: Initialize__, em que o __ \_ controlador m__ e o __ \_ processador m__ também são inicializados, configuramos o mecanismo de áudio e o preparandoi para tocar sons.

 * Crie __m \_ audioController__, que é uma instância da classe [Audio](#audioh) .
 * Crie os recursos de áudio necessários usando o método [Audio:: CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) . Aqui, dois __XAudio2__ objetos XAudio2 &mdash; um objeto de mecanismo de música e um objeto de mecanismo de som e uma voz de mestre para cada um deles foram criados. O objeto do mecanismo de música pode ser usado para reproduzir música em segundo plano para seu jogo. O mecanismo de som pode ser usado para reproduzir efeitos sonoros em seu jogo. Para obter mais informações, consulte [criar e inicializar os recursos de áudio](#create-and-initialize-the-audio-resources).
 * Crie __mediaReader__, que é uma instância da classe [mediaReader](#mediareaderh) . [MediaReader](#mediareaderh), que é uma classe auxiliar para a classe [SoundEffect](#soundeffecth) , lê arquivos de áudio pequenos de forma síncrona do local do arquivo e retorna dados de som como uma matriz de bytes.
 * Use [MediaReader:: loadmedia](#mediareaderloadmedia-method) para carregar arquivos de som de seu local e criar uma variável __targetHitSound__ para manter os dados de som. wav carregados. Para obter mais informações, consulte [carregar arquivo de áudio](#load-audio-file). 

Os efeitos de som são associados ao objeto Game. Assim, quando ocorre uma colisão com esse objeto de jogo, ele aciona o efeito de som a ser reproduzido. Neste jogo de exemplo, temos efeitos sonoros para o Ammo (o que usamos para fazer destinos) e para o destino. 
    
* Na classe __gameobject__ , há uma propriedade __HitSound__ que é usada para associar o efeito de som ao objeto.
* Crie uma nova instância da classe [SoundEffect](#soundeffecth) e inicialize-a. Durante a inicialização, uma voz de origem para o efeito de som é criada. 
* Essa classe desempenha um som usando uma voz de mestre fornecida da classe [Audio](#audioh) . Os dados de som são lidos do local do arquivo usando a classe [MediaReader](#mediareaderh) . Para obter mais informações, consulte [associar som ao objeto](#associate-sound-to-object).

>[!Note]
>O gatilho real para tocar o som é determinado pela movimentação e colisão desses objetos de jogo. Portanto, a chamada para realmente reproduzir esses sons é definida no método [Simple3DGame:: UpdateDynamics](#simple3dgameupdatedynamics-method) . Para obter mais informações, vá para [reproduzir o som](#play-the-sound).

```cppwinrt
void Simple3DGame::Initialize(
    _In_ std::shared_ptr<MoveLookController> const& controller,
    _In_ std::shared_ptr<GameRenderer> const& renderer
    )
{
    // The following member is defined in the header file:
    // Audio m_audioController;

    ...

    // Create the audio resources needed.
    // Two XAudio2 objects are created - one for music engine,
    // the other for sound engine. A mastering voice is also
    // created for each of the objects.
    m_audioController.CreateDeviceIndependentResources();

    m_ammo.resize(GameConstants::MaxAmmo);

    ...

    // Create a media reader which is used to read audio files from its file location.
    MediaReader mediaReader;
    auto targetHitSoundX = mediaReader.LoadMedia(L"Assets\\hit.wav");

    // Instantiate the targets for use in the game.
    // Each target has a different initial position, size, and orientation.
    // But share a common set of material properties.
    for (int a = 1; a < GameConstants::MaxTargets; a++)
    {
        ...
        // Create a new sound effect object and associate it
        // with the game object's (target) HitSound property.
        target->HitSound(std::make_shared<SoundEffect>());

        // Initialize the sound effect object with
        // the sound effect engine, format of the audio wave, and audio data
        // During initialization, source voice of this sound effect is also created.
        target->HitSound()->Initialize(
            m_audioController.SoundEffectEngine(),
            mediaReader.GetOutputWaveFormatEx(),
            targetHitSoundX
            );
        ...
    }

    // Instantiate a set of spheres to be used as ammunition for the game
    // and set the material properties of the spheres.
    auto ammoHitSound = mediaReader.LoadMedia(L"Assets\\bounce.wav");

    for (int a = 0; a < GameConstants::MaxAmmo; a++)
    {
        m_ammo[a] = std::make_shared<Sphere>();
        m_ammo[a]->Radius(GameConstants::AmmoRadius);
        m_ammo[a]->HitSound(std::make_shared<SoundEffect>());
        m_ammo[a]->HitSound()->Initialize(
            m_audioController.SoundEffectEngine(),
            mediaReader.GetOutputWaveFormatEx(),
            ammoHitSound
            );
        m_ammo[a]->Active(false);
        m_renderObjects.push_back(m_ammo[a]);
    }
    ...
}
```

## <a name="create-and-initialize-the-audio-resources"></a>Criar e inicializar os recursos de áudio

* Use [XAudio2Create](/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create), uma API do XAudio2, para criar dois novos objetos XAudio2 que definem os mecanismos de música e de efeito de som. Esse método retorna um ponteiro para a interface [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) do objeto que gerencia todos os Estados do mecanismo de áudio, o thread de processamento de áudio, o grafo de voz e muito mais.
* Depois que os mecanismos tiverem sido instanciados, use [IXAudio2:: CreateMasteringVoice](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createmasteringvoice) para criar uma voz de mestre para cada um dos objetos do mecanismo de som.

Para obter mais informações, acesse [como: inicializar XAudio2](/windows/desktop/xaudio2/how-to--initialize-xaudio2).

### <a name="audiocreatedeviceindependentresources-method"></a>Método Audio:: CreateDeviceIndependentResources

```cppwinrt
void Audio::CreateDeviceIndependentResources()
{
    UINT32 flags = 0;

    winrt::check_hresult(
        XAudio2Create(m_musicEngine.put(), flags)
        );

    HRESULT hr = m_musicEngine->CreateMasteringVoice(&m_musicMasteringVoice);
    if (FAILED(hr))
    {
        // Unable to create an audio device
        m_audioAvailable = false;
        return;
    }

    winrt::check_hresult(
        XAudio2Create(m_soundEffectEngine.put(), flags)
        );

    winrt::check_hresult(
        m_soundEffectEngine->CreateMasteringVoice(&m_soundEffectMasteringVoice)
        );

    m_audioAvailable = true;
}
```

## <a name="load-audio-file"></a>Carregar arquivo de áudio

No jogo de exemplo, o código para ler arquivos de formato de áudio é definido em [MediaReader. h](#mediareaderh)/cpp__.  Para ler um arquivo de áudio. wav codificado, chame [MediaReader:: loadmedia](#mediareaderloadmedia-method), passando o nome de arquivo do. wav como o parâmetro de entrada.

### <a name="mediareaderloadmedia-method"></a>Método MediaReader:: loadmedia

Esse método usa as APIs [Media Foundation](/windows/desktop/medfound/microsoft-media-foundation-sdk) para ler o arquivo de áudio .wav como um buffer de modulação por código de pulso (PCM).

#### <a name="set-up-the-source-reader"></a>Configurar o leitor de origem

1. Use [MFCreateSourceReaderFromURL](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-mfcreatesourcereaderfromurl) para criar um leitor de origem de mídia ([IMFSourceReader](/windows/desktop/api/mfreadwrite/nn-mfreadwrite-imfsourcereader)).
2. Use [MFCreateMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatemediatype) para criar um objeto de tipo de mídia ([IMFMediaType](/windows/desktop/api/mfobjects/nn-mfobjects-imfmediatype)) (_MediaType_). Ele representa uma descrição de um formato de mídia. 
3. Especifique que a saída decodificada de _MediaType_é áudio PCM, que é um tipo de áudio que __XAudio2__ pode usar.
4. Define o tipo de mídia de saída decodificada para o leitor de origem chamando [IMFSourceReader:: SetCurrentMediaType](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentmediatype).

Para obter mais informações sobre por que usamos o leitor de origem, vá para [leitor de origem](/windows/desktop/medfound/source-reader).

#### <a name="describe-the-data-format-of-the-audio-stream"></a>Descrever o formato de dados do fluxo de áudio

1. Use [IMFSourceReader:: GetCurrentMediaType](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getcurrentmediatype) para obter o tipo de mídia atual para o fluxo.
2. Use [IMFMediaType:: MFCreateWaveFormatExFromMFMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype) para converter o tipo de mídia de áudio atual em um buffer [WAVEFORMATEX](/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) , usando os resultados da operação anterior como entrada. Essa estrutura especifica o formato de dados do fluxo de áudio de som wave usado após o carregamento do áudio. 

O formato __WAVEFORMATEX__ pode ser usado para descrever o buffer do PCM. Em comparação com a estrutura [WAVEFORMATEXTENSIBLE](/windows-hardware/drivers/ddi/content/ksmedia/ns-ksmedia-waveformatextensible) , ela só pode ser usada para descrever um subconjunto de formatos de Wave de áudio. Para obter mais informações sobre as diferenças entre __WAVEFORMATEX__ e __WAVEFORMATEXTENSIBLE__, consulte [descritores Wave-Format extensível](/windows-hardware/drivers/audio/extensible-wave-format-descriptors).

#### <a name="read-the-audio-stream"></a>Ler o fluxo de áudio

1.  Obtenha a duração, em segundos, do fluxo de áudio chamando [IMFSourceReader:: Getpresentationattribute](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getpresentationattribute) e, em seguida, converte a duração em bytes.
2.  Leia o arquivo de áudio em como um fluxo chamando [IMFSourceReader:: ReadSample](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-readsample). __ReadSample__ lê a próxima amostra da origem da mídia.
3.  Use [IMFSample:: ConvertToContiguousBuffer](/windows/desktop/api/mfobjects/nf-mfobjects-imfsample-converttocontiguousbuffer) para copiar o conteúdo do buffer de exemplo de áudio (_exemplo_) em uma matriz (_mediaBuffer_).

```cppwinrt
std::vector<byte> MediaReader::LoadMedia(_In_ winrt::hstring const& filename)
{
    winrt::check_hresult(
        MFStartup(MF_VERSION)
        );

    // Creates a media source reader.
    winrt::com_ptr<IMFSourceReader> reader;
    winrt::check_hresult(
        MFCreateSourceReaderFromURL(
        (m_installedLocationPath + filename).c_str(),
            nullptr,
            reader.put()
            )
        );

    // Set the decoded output format as PCM.
    // XAudio2 on Windows can process PCM and ADPCM-encoded buffers.
    // When using MediaFoundation, this sample always decodes into PCM.
    winrt::com_ptr<IMFMediaType> mediaType;
    winrt::check_hresult(
        MFCreateMediaType(mediaType.put())
        );

    // Define the major category of the media as audio. For more info about major media types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa367377.aspx
    winrt::check_hresult(
        mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
        );

    // Define the sub-type of the media as uncompressed PCM audio. For more info about audio sub-types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa372553.aspx
    winrt::check_hresult(
        mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
        );

    // Sets the media type for a stream. This media type defines that format that the Source Reader 
    // produces as output. It can differ from the native format provided by the media source.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/dd374667.aspx
    winrt::check_hresult(
        reader->SetCurrentMediaType(static_cast<uint32_t>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), 0, mediaType.get())
        );

    // Get the current media type for the stream.
    // For more info, go to:
    // https://msdn.microsoft.com/library/windows/desktop/dd374660.aspx
    winrt::com_ptr<IMFMediaType> outputMediaType;
    winrt::check_hresult(
        reader->GetCurrentMediaType(static_cast<uint32_t>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), outputMediaType.put())
        );

    // Converts the current media type into the WaveFormatEx buffer structure.
    UINT32 size = 0;
    WAVEFORMATEX* waveFormat;
    winrt::check_hresult(
        MFCreateWaveFormatExFromMFMediaType(outputMediaType.get(), &waveFormat, &size)
        );

    // Copies the waveFormat's block of memory to the starting address of the m_waveFormat variable in MediaReader.
    // Then free the waveFormat memory block.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/aa366535.aspx and
    // https://msdn.microsoft.com/library/windows/desktop/ms680722.aspx
    CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
    CoTaskMemFree(waveFormat);

    PROPVARIANT propVariant;
    winrt::check_hresult(
        reader->GetPresentationAttribute(static_cast<uint32_t>(MF_SOURCE_READER_MEDIASOURCE), MF_PD_DURATION, &propVariant)
        );

    // 'duration' is in 100ns units; convert to seconds, and round up
    // to the nearest whole byte.
    LONGLONG duration = propVariant.uhVal.QuadPart;
    unsigned int maxStreamLengthInBytes =
        static_cast<unsigned int>(
            ((duration * static_cast<ULONGLONG>(m_waveFormat.nAvgBytesPerSec)) + 10000000) /
            10000000
            );

    std::vector<byte> fileData(maxStreamLengthInBytes);

    winrt::com_ptr<IMFSample> sample;
    winrt::com_ptr<IMFMediaBuffer> mediaBuffer;
    DWORD flags = 0;

    int positionInData = 0;
    bool done = false;
    while (!done)
    {
        // Read audio data.
        ...
    }

    return fileData;
}
```

## <a name="associate-sound-to-object"></a>Associar som ao objeto

Associar sons ao objeto ocorre quando o jogo é inicializado, no método [Simple3DGame:: Initialize](#simple3dgameinitialize-method) .

Recapitulação:
* Na classe __gameobject__ , há uma propriedade __HitSound__ que é usada para associar o efeito de som ao objeto.
* Crie uma nova instância do objeto de classe [SoundEffect](#soundeffecth) e associe-a ao objeto Game. Essa classe desempenha um som usando APIs __XAudio2__ .  Ele usa uma voz de mestre fornecida pela classe [Audio](#audioh) . Os dados de som podem ser lidos do local do arquivo usando a classe [MediaReader](#mediareaderh) .

[SoundEffect:: Initialize](#soundeffectinitialize-method) é usado para inicializar a instância __SoundEffect__ com os seguintes parâmetros de entrada: ponteiro para objeto do mecanismo de som (objetos IXAudio2 criados no método [Audio:: CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) ), ponteiro para o formato do arquivo. wav usando __MediaReader:: GetOutputWaveFormatEx__e os dados de som carregados usando o método [MediaReader:: loadmedia](#mediareaderloadmedia-method) . Durante a inicialização, a voz de origem do efeito de som também é criada.

### <a name="soundeffectinitialize-method"></a>Método SoundEffect:: Initialize

```cppwinrt
void SoundEffect::Initialize(
    _In_ IXAudio2* masteringEngine,
    _In_ WAVEFORMATEX* sourceFormat,
    _In_ std::vector<byte> const& soundData)
{
    m_soundData = soundData;

    if (masteringEngine == nullptr)
    {
        // Audio is not available so just return.
        m_audioAvailable = false;
        return;
    }

    // Create a source voice for this sound effect.
    winrt::check_hresult(
        masteringEngine->CreateSourceVoice(
            &m_sourceVoice,
            sourceFormat
            )
        );
    m_audioAvailable = true;
}
```

## <a name="play-the-sound"></a>Tocar o som

Os gatilhos para reproduzir efeitos sonoros são definidos no método [Simple3DGame:: UpdateDynamics](#simple3dgameupdatedynamics-method) porque é aí que a movimentação dos objetos é atualizada e a colisão entre os objetos é determinada.

Como a interação entre objetos difere muito, dependendo do jogo, não vamos discutir a dinâmica dos objetos do jogo aqui. Se você estiver interessado em entender sua implementação, vá para o método [Simple3DGame:: UpdateDynamics](#simple3dgameupdatedynamics-method) .

Em princípio, quando ocorre uma colisão, ela dispara a reprodução do efeito de som chamando **SoundEffect::P laysound**. Esse método interrompe os efeitos de som que estão sendo executados no momento e enfileira o buffer na memória com os dados de som desejados. Ele usa a voz de origem para definir o volume, enviar dados de som e iniciar a reprodução.

### <a name="soundeffectplaysound-method"></a>SoundEffect::P método laySound

* Usa o objeto de voz de origem **m \_ sourceVoice** para iniciar a reprodução do buffer de dados de som **m \_ soundData**
* Cria um [ \_ buffer XAudio2](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_buffer), ao qual ele fornece uma referência ao buffer de dados de som e, em seguida, o envia com uma chamada para [IXAudio2SourceVoice:: SubmitSourceBuffer](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-submitsourcebuffer). 
* Com os dados de som na fila, **SoundEffect::PlaySound** começa a reproduzir chamando [IXAudio2SourceVoice::Start](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start).

```cppwinrt
void SoundEffect::PlaySound(_In_ float volume)
{
    XAUDIO2_BUFFER buffer = { 0 };

    if (!m_audioAvailable)
    {
        // Audio is not available so just return.
        return;
    }

    // Interrupt sound effect if it is currently playing.
    winrt::check_hresult(
        m_sourceVoice->Stop()
        );
    winrt::check_hresult(
        m_sourceVoice->FlushSourceBuffers()
        );

    // Queue the memory buffer for playback and start the voice.
    buffer.AudioBytes = (UINT32)m_soundData.size();
    buffer.pAudioData = m_soundData.data();
    buffer.Flags = XAUDIO2_END_OF_STREAM;

    winrt::check_hresult(
        m_sourceVoice->SetVolume(volume)
        );
    winrt::check_hresult(
        m_sourceVoice->SubmitSourceBuffer(&buffer)
        );
    winrt::check_hresult(
        m_sourceVoice->Start()
        );
}
```

### <a name="simple3dgameupdatedynamics-method"></a>Método Simple3DGame:: UpdateDynamics

O método __Simple3DGame:: UpdateDynamics__ cuida da interação e da colisão entre os objetos do jogo. Quando os objetos colidem (ou interseccionam), ele dispara o efeito de som associado para reprodução.

```cppwinrt
void Simple3DGame::UpdateDynamics()
{
    ...
    // Check for collisions between ammo.
#pragma region inter-ammo collision detection
if (m_ammoCount > 1)
{
    ...
    // Check collision between instances One and Two.
    ...
    if (distanceSquared < (GameConstants::AmmoSize * GameConstants::AmmoSize))
    {
        // The two ammo are intersecting.
        ...
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
        ...
        // Play the sound associated with the Ammo hitting something.
        m_objects[i]->PlaySound(impact, m_player->Position());

        if (m_objects[i]->Target() && !m_objects[i]->Hit())
        {
            // The object is a target and isn't currently hit, so mark
            // it as hit and play the sound associated with the impact.
            m_objects[i]->Hit(true);
            m_objects[i]->HitTime(timeTotal);
            m_totalHits++;

            m_objects[i]->PlaySound(impact, m_player->Position());
        }
        ...
    }
#pragma endregion

#pragma region Apply Gravity and world intersection
            // Apply gravity and check for collision against enclosing volume.
            ...
                if (position.z < limit)
                {
                    // The ammo instance hit the a wall in the min Z direction.
                    // Align the ammo instance to the wall, invert the Z component of the velocity and
                    // play the impact sound.
                    position.z = limit;
                    m_ammo[i]->PlaySound(-velocity.z, m_player->Position());
                    velocity.z = -velocity.z * GameConstants::Physics::GroundRestitution;
                }
                ...
#pragma endregion
}
```

## <a name="next-steps"></a>Próximas etapas

Nós abordamos a estrutura UWP, os elementos gráficos, os controles, a interface do usuário e o áudio de um jogo do Windows 10. A próxima parte deste tutorial, [ampliando o jogo de exemplo](tutorial-resources.md), explica outras opções que podem ser usadas durante o desenvolvimento de um jogo.

## <a name="audio-concepts"></a>Conceitos de áudio

Para o desenvolvimento de jogos do Windows 10, use o XAudio2 versão 2,9. Esta versão é fornecida com o Windows 10. Para obter mais informações, acesse [XAudio2 versões](/windows/desktop/xaudio2/xaudio2-versions).

__AudioX2__ é uma API de nível baixo que fornece processamento de sinal e combinação de base. Para obter mais informações, consulte [XAudio2 Key Concepts](/windows/desktop/xaudio2/xaudio2-key-concepts).

### <a name="xaudio2-voices"></a>XAudio2 vozes

Há três tipos de objetos de voz XAudio2: origem, submix e vozes de mestre. As vozes são os objetos XAudio2 usados para processar, manipular e reproduzir dados de áudio. 
* As vozes de origem operam em dados de áudio fornecidos pelo cliente. 
* As vozes de origem e submixagem enviam sua saída para uma ou mais vozes de submixagem ou masterização. 
* As vozes de submixagem e masterização combinam o áudio de todas as vozes que as alimentam e operam no resultado. 
* As vozes de controle de voz recebem dados de vozes de origem e vozes submix e envia esses dados para o hardware de áudio.

Para obter mais informações, acesse [XAudio2 vozes](/windows/desktop/xaudio2/xaudio2-voices).

### <a name="audio-graph"></a>Grafo de áudio

O grafo de áudio é uma coleção de [vozes XAudio2](/windows/desktop/xaudio2/xaudio2-voices). O áudio começa em um lado de um gráfico de áudio em vozes de origem, e opcionalmente passa por uma ou mais vozes submix e termina em uma voz de mestre. Um grafo de áudio conterá uma voz de origem para cada som atualmente em execução, zero ou mais vozes submix e uma voz de mestre. O grafo de áudio mais simples, e o mínimo necessário para fazer um ruído em XAudio2, é uma saída de voz de origem única diretamente para uma voz de mestre. Para obter mais informações, vá para [grafos de áudio](/windows/desktop/xaudio2/audio-graphs).

### <a name="additional-reading"></a>Leitura adicional

* [Como: Inicializar o XAudio2](/windows/desktop/xaudio2/how-to--initialize-xaudio2)
* [Como: Carregar arquivos de dados de áudio no XAudio2](/windows/desktop/xaudio2/how-to--load-audio-data-files-in-xaudio2)
* [Como: Reproduzir um som com o XAudio2](/windows/desktop/xaudio2/how-to--play-a-sound-with-xaudio2)

## <a name="key-audio-h-files"></a>Arquivos Key Audio. h

### <a name="audioh"></a>Audio.h

```cppwinrt
// Audio:
// This class uses XAudio2 to provide sound output. It creates two
// engines - one for music and the other for sound effects - each as
// a separate mastering voice.
// The SuspendAudio and ResumeAudio methods can be used to stop
// and start all audio playback.

class Audio
{
public:
    Audio();

    void Initialize();
    void CreateDeviceIndependentResources();
    IXAudio2* MusicEngine();
    IXAudio2* SoundEffectEngine();
    void SuspendAudio();
    void ResumeAudio();

private:
    ...
};
```

### <a name="mediareaderh"></a>MediaReader. h

```cppwinrt
// MediaReader:
// This is a helper class for the SoundEffect class. It reads small audio files
// synchronously from the package installed folder and returns sound data as a
// vector of bytes.

class MediaReader
{
public:
    MediaReader();

    std::vector<byte> LoadMedia(_In_ winrt::hstring const& filename);
    WAVEFORMATEX* GetOutputWaveFormatEx();

private:
    winrt::Windows::Storage::StorageFolder  m_installedLocation{ nullptr };
    winrt::hstring                          m_installedLocationPath;
    WAVEFORMATEX                            m_waveFormat;
};
```

### <a name="soundeffecth"></a>SoundEffect.h

```cppwinrt
// SoundEffect:
// This class plays a sound using XAudio2. It uses a mastering voice provided
// from the Audio class. The sound data can be read from disk using the MediaReader
// class.

class SoundEffect
{
public:
    SoundEffect();

    void Initialize(
        _In_ IXAudio2* masteringEngine,
        _In_ WAVEFORMATEX* sourceFormat,
        _In_ std::vector<byte> const& soundData
        );

    void PlaySound(_In_ float volume);

private:
    ...
};
```
