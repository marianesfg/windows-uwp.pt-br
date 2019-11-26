---
Description: Use o reconhecimento de fala para fornecer entrada, especificar uma ação ou um comando e realizar tarefas.
title: Reconhecimento de fala
ms.assetid: 553C0FB7-35BC-4894-9EF1-906139E17552
label: Speech recognition
template: detail.hbs
keywords: controle por voz, voz, reconhecimento de fala, linguagem natural, ditado, entrada, interação do usuário
ms.date: 10/25/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1979e16853fc288b83f5e4216c970440300fc597
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258312"
---
# <a name="speech-recognition"></a>Reconhecimento de fala


Use o reconhecimento de fala para fornecer entrada, especificar uma ação ou um comando e realizar tarefas.

> **APIs importantes**: [**Windows.Media.SpeechRecognition**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition)

O reconhecimento de fala é realizado em um tempo de execução de fala, APIs de reconhecimento para programação do tempo de execução, gramáticas prontas para usar ditado e pesquisa na Web e uma interface do usuário do sistema padrão que auxilia os usuários a descobrir e usar recursos de reconhecimento de fala.

## <a name="configure-speech-recognition"></a>Configurar o reconhecimento de fala

Para dar suporte ao reconhecimento de fala com seu aplicativo, o usuário deve se conectar e habilitar um microfone em seu dispositivo e aceitar a permissão de concessão da política de privacidade da Microsoft para que seu aplicativo o use.

Para solicitar automaticamente ao usuário uma caixa de diálogo do sistema solicitando permissão para acessar e usar o feed de áudio do microfone (exemplo do [exemplo de reconhecimento de fala e síntese de fala](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis) mostrado abaixo), basta definir a capacidade de [dispositivo](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-devicecapability) do **microfone** no manifesto do [pacote do aplicativo](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest). Para obter mais detalhes, consulte [declarações de recurso de aplicativo](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).

![Política de privacidade para acesso ao microfone](images/speech/privacy.png)

Se o usuário clicar em Sim para conceder acesso ao microfone, seu aplicativo será adicionado à lista de aplicativos aprovados na página Configurações-> Privacidade-> microfone. No entanto, como o usuário pode optar por desativar essa configuração a qualquer momento, você deve confirmar que seu aplicativo tem acesso ao microfone antes de tentar usá-lo.

Se você também quiser dar suporte a ditado, Cortana ou outros serviços de reconhecimento de fala (como uma [gramática predefinida](#predefined-grammars) definida em uma restrição de tópico), também deverá confirmar se o **reconhecimento de fala Online** (configurações > privacidade-> fala) está habilitado.

Este trecho de código mostra como seu aplicativo pode verificar se um microfone está presente e se tem permissão para usá-lo.

```csharp
public class AudioCapturePermissions
{
    // If no microphone is present, an exception is thrown with the following HResult value.
    private static int NoCaptureDevicesHResult = -1072845856;

    /// <summary>
    /// Note that this method only checks the Settings->Privacy->Microphone setting, it does not handle
    /// the Cortana/Dictation privacy check.
    ///
    /// You should perform this check every time the app gets focus, in case the user has changed
    /// the setting while the app was suspended or not in focus.
    /// </summary>
    /// <returns>True, if the microphone is available.</returns>
    public async static Task<bool> RequestMicrophonePermission()
    {
        try
        {
            // Request access to the audio capture device.
            MediaCaptureInitializationSettings settings = new MediaCaptureInitializationSettings();
            settings.StreamingCaptureMode = StreamingCaptureMode.Audio;
            settings.MediaCategory = MediaCategory.Speech;
            MediaCapture capture = new MediaCapture();

            await capture.InitializeAsync(settings);
        }
        catch (TypeLoadException)
        {
            // Thrown when a media player is not available.
            var messageDialog = new Windows.UI.Popups.MessageDialog("Media player components are unavailable.");
            await messageDialog.ShowAsync();
            return false;
        }
        catch (UnauthorizedAccessException)
        {
            // Thrown when permission to use the audio capture device is denied.
            // If this occurs, show an error or disable recognition functionality.
            return false;
        }
        catch (Exception exception)
        {
            // Thrown when an audio capture device is not present.
            if (exception.HResult == NoCaptureDevicesHResult)
            {
                var messageDialog = new Windows.UI.Popups.MessageDialog("No Audio Capture devices are present on this system.");
                await messageDialog.ShowAsync();
                return false;
            }
            else
            {
                throw;
            }
        }
        return true;
    }
}
```

```cpp
/// <summary>
/// Note that this method only checks the Settings->Privacy->Microphone setting, it does not handle
/// the Cortana/Dictation privacy check.
///
/// You should perform this check every time the app gets focus, in case the user has changed
/// the setting while the app was suspended or not in focus.
/// </summary>
/// <returns>True, if the microphone is available.</returns>
IAsyncOperation<bool>^  AudioCapturePermissions::RequestMicrophonePermissionAsync()
{
    return create_async([]() 
    {
        try
        {
            // Request access to the audio capture device.
            MediaCaptureInitializationSettings^ settings = ref new MediaCaptureInitializationSettings();
            settings->StreamingCaptureMode = StreamingCaptureMode::Audio;
            settings->MediaCategory = MediaCategory::Speech;
            MediaCapture^ capture = ref new MediaCapture();

            return create_task(capture->InitializeAsync(settings))
                .then([](task<void> previousTask) -> bool
            {
                try
                {
                    previousTask.get();
                }
                catch (AccessDeniedException^)
                {
                    // Thrown when permission to use the audio capture device is denied.
                    // If this occurs, show an error or disable recognition functionality.
                    return false;
                }
                catch (Exception^ exception)
                {
                    // Thrown when an audio capture device is not present.
                    if (exception->HResult == AudioCapturePermissions::NoCaptureDevicesHResult)
                    {
                        auto messageDialog = ref new Windows::UI::Popups::MessageDialog("No Audio Capture devices are present on this system.");
                        create_task(messageDialog->ShowAsync());
                        return false;
                    }

                    throw;
                }
                return true;
            });
        }
        catch (Platform::ClassNotRegisteredException^ ex)
        {
            // Thrown when a media player is not available. 
            auto messageDialog = ref new Windows::UI::Popups::MessageDialog("Media Player Components unavailable.");
            create_task(messageDialog->ShowAsync());
            return create_task([] {return false; });
        }
    });
}
```

```js
var AudioCapturePermissions = WinJS.Class.define(
    function () { }, {},
    {
        requestMicrophonePermission: function () {
            /// <summary>
            /// Note that this method only checks the Settings->Privacy->Microphone setting, it does not handle
            /// the Cortana/Dictation privacy check.
            ///
            /// You should perform this check every time the app gets focus, in case the user has changed
            /// the setting while the app was suspended or not in focus.
            /// </summary>
            /// <returns>True, if the microphone is available.</returns>
            return new WinJS.Promise(function (completed, error) {

                try {
                    // Request access to the audio capture device.
                    var captureSettings = new Windows.Media.Capture.MediaCaptureInitializationSettings();
                    captureSettings.streamingCaptureMode = Windows.Media.Capture.StreamingCaptureMode.audio;
                    captureSettings.mediaCategory = Windows.Media.Capture.MediaCategory.speech;

                    var capture = new Windows.Media.Capture.MediaCapture();
                    capture.initializeAsync(captureSettings).then(function () {
                        completed(true);
                    },
                    function (error) {
                        // Audio Capture can fail to initialize if there's no audio devices on the system, or if
                        // the user has disabled permission to access the microphone in the Privacy settings.
                        if (error.number == -2147024891) { // Access denied (microphone disabled in settings)
                            completed(false);
                        } else if (error.number == -1072845856) { // No recording device present.
                            var messageDialog = new Windows.UI.Popups.MessageDialog("No Audio Capture devices are present on this system.");
                            messageDialog.showAsync();
                            completed(false);
                        } else {
                            error(error);
                        }
                    });
                } catch (exception) {
                    if (exception.number == -2147221164) { // REGDB_E_CLASSNOTREG
                        var messageDialog = new Windows.UI.Popups.MessageDialog("Media Player components not available on this system.");
                        messageDialog.showAsync();
                        return false;
                    }
                }
            });
        }
    })
```

## <a name="recognize-speech-input"></a>Reconhecer a entrada da fala

Uma *restrição* define as palavras e frases (vocabulário) que um aplicativo reconhece na entrada de fala. As restrições estão no núcleo do reconhecimento de fala e proporcionam ao seu aplicativo mais exatidão no reconhecimento de fala.

Você pode usar os seguintes tipos de restrições para reconhecer a entrada de fala.

### <a name="predefined-grammars"></a>Gramáticas pré-definidas

Ditado predefinido e gramáticas de pesquisa na Web fornecem o reconhecimento de fala de seu aplicativo sem precisar que você crie uma gramática. Ao utilizar essas gramáticas, o reconhecimento de fala é realizado por um serviço Web remoto, e os resultados são retornados ao dispositivo.

A gramática de ditado de texto livre padrão pode reconhecer a maioria das palavras e frases que um usuário pode dizer em um determinado idioma e é otimizada para reconhecer frases curtas. A gramática de ditado predefinida será usada se você não especificar uma restrição para seu objeto [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer). O ditado de texto livre é útil quando você não deseja limitar os tipos de coisas que um usuário pode dizer. Os usos típicos incluem criação de notas ou ditado de conteúdo para uma mensagem.

A gramática de pesquisa na Web, assim como uma gramática de ditado, contém um grande número de palavras e frases que um usuário pode dizer. No entanto, ela é otimizada para reconhecer termos que as pessoas normalmente usam ao pesquisar na Web.

**Observe**  como ditado predefinido e gramáticas de pesquisa da Web podem ser grandes, e como estão online (não no dispositivo), o desempenho pode não ser tão rápido quanto com uma gramática personalizada instalada no dispositivo.     

Essas gramáticas predefinidas podem ser usadas para reconhecer até 10 segundos de entrada de fala e não exigem nenhum esforço de criação de sua parte. No entanto, elas exigem uma conexão com uma rede.

Para usar restrições de serviço Web, você deve habilitar o suporte à entrada de fala e ao ditado em **Configurações** ativando a opção "Acessar meus dados" em **Configurações -> Privacidade -> Fala, escrita à tinta e digitação**.

Aqui, nós mostramos como testar se a entrada de fala está habilitada. Caso não esteja, abra página Configurações -> Privacidade -> Fala, escrita à tinta e digitação.

Primeiro, inicializamos uma variável global (HResultPrivacyStatementDeclined) para o valor HResult de 0x80045509. Consulte [tratamento de exceção para em C\# ou Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/dn532194(v=win.10)).

```csharp
private static uint HResultPrivacyStatementDeclined = 0x80045509;
```

Em seguida, pegamos quaisquer exceções padrão durante o reconhecimento e testamos se o valor [**HResult**](https://docs.microsoft.com/uwp/api/Windows.Foundation.HResult) é igual ao valor da variável HResultPrivacyStatementDeclined. Se for, exibimos um aviso e chamamos `await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts"));` para abrir a página Configurações.

```csharp
catch (Exception exception)
{
  // Handle the speech privacy policy error.
  if ((uint)exception.HResult == HResultPrivacyStatementDeclined)
  {
    resultTextBlock.Visibility = Visibility.Visible;
    resultTextBlock.Text = "The privacy statement was declined." + 
      "Go to Settings -> Privacy -> Speech, inking and typing, and ensure you" +
      "have viewed the privacy policy, and 'Get To Know You' is enabled.";
    // Open the privacy/speech, inking, and typing settings page.
    await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts")); 
  }
  else
  {
    var messageDialog = new Windows.UI.Popups.MessageDialog(exception.Message, "Exception");
    await messageDialog.ShowAsync();
  }
}
```

Consulte [**SpeechRecognitionTopicConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint).

### <a name="programmatic-list-constraints"></a>Restrições de lista programática 

Restrições de lista programática fornecem uma abordagem leve para criar gramáticas simples usando uma lista de palavras ou frases. Uma lista de restrições funciona bem para o reconhecimento de frases curtas e distintas. Especificar explicitamente todas as palavras em uma gramática também melhora a precisão do reconhecimento, porque o mecanismo de reconhecimento de fala deve processar somente a fala para confirmar uma correspondência. A lista também pode ser atualizada programaticamente.

Uma restrição de lista consiste em uma matriz de cadeia de caracteres que representa a entrada de fala que seu aplicativo aceitará para uma operação de reconhecimento. Você pode criar uma restrição de lista em seu aplicativo criando um objeto de restrição de lista de reconhecimento de fala e passando uma matriz de cadeias de caracteres. Em seguida, adicione o objeto à coleção de restrições do reconhecedor. O reconhecimento é bem-sucedido quando o reconhecedor de fala reconhece qualquer uma das sequências de caracteres na matriz.

Consulte [**SpeechRecognitionListConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint).

### <a name="srgs-grammars"></a>Gramáticas SRGS

Uma gramática SRGS (Especificação de Gramática de Reconhecimento de Fala) é um documento estático que, ao contrário de uma restrição de lista programática, usa o formato XML definido pela [SRGS Versão 1.0](https://www.w3.org/TR/speech-grammar/). Uma gramática SRGS oferece maior controle sobre a experiência de reconhecimento de fala, permitindo a você capturar diversos significados semânticos em um único reconhecimento.

 Consulte [**SpeechRecognitionGrammarFileConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint).

### <a name="voice-command-constraints"></a>Restrições de comando de voz

Use um arquivo XML de Definição de comando de voz (VCD) para definir os comandos que o usuário pode usar para iniciar ações ao ativar seu aplicativo. Para obter mais detalhes, consulte [ativar um aplicativo em primeiro plano com comandos de voz por meio da Cortana](https://docs.microsoft.com/cortana/voice-commands/launch-a-foreground-app-with-voice-commands-in-cortana).

Consulte [**SpeechRecognitionVoiceCommandDefinitionConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionVoiceCommandDefinitionConstraint)/

**Observação**  o tipo de tipo de restrição usado depende da complexidade da experiência de reconhecimento que você deseja criar. Qualquer tipo pode ser a melhor escolha para uma tarefa específica de reconhecimento, e você pode encontrar usos para todos os tipos de restrição em seu aplicativo.
Para começar a usar restrições, consulte [Definir restrições de reconhecimento personalizadas](define-custom-recognition-constraints.md).

A gramática de ditado predefinida do Aplicativo Universal do Windows reconhece a maioria das palavras e frases curtas em um idioma. Ela é ativada por padrão quando um objeto reconhecedor de fala é instanciado sem restrições personalizadas.

Neste exemplo, você verá como:

- Crie um reconhecedor de fala.
- Compile as restrições padrão do Aplicativo Universal do Windows (nenhuma gramática foi adicionada ao conjunto de gramáticas do reconhecedor de fala).
- Inicie a escuta da fala usando a interface do usuário de reconhecimento básica e o comentário TTS fornecido pelo método [**RecognizeWithUIAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizewithuiasync). Use o método [**RecognizeAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizeasync) se a interface do usuário padrão não for necessária.

```CSharp
private async void StartRecognizing_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Compile the dictation grammar by default.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="customize-the-recognition-ui"></a>Personalizar o reconhecimento da interface do usuário


Quando seu aplicativo tenta o reconhecimento de fala chamando [**SpeechRecognizer.RecognizeWithUIAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizewithuiasync), várias telas são mostradas na ordem a seguir.

Se estiver usando uma restrição baseada em uma gramática predefinida (ditado ou pesquisa na Web):

-   A tela **Ouvindo** .
-   A tela **Pensando** .
-   A tela **Ouvi você dizer** ou a tela de erro.

Se estiver usando uma restrição baseada em lista de palavras ou frases ou uma restrição baseada em um arquivo de gramática SRGS:

-   A tela **Ouvindo** .
-   A tela **Você disse**, se o que o usuário disse puder ser interpretado como mais de um resultado potencial.
-   A tela **Ouvi você dizer** ou a tela de erro.

A imagem a seguir mostra um exemplo do fluxo entre telas de um reconhecedor de fala que utiliza uma restrição baseada em um arquivo de gramática SRGS. Neste exemplo, o reconhecimento de fala foi bem-sucedido.

![tela de reconhecimento inicial para uma restrição baseada em um arquivo de gramática sgrs](images/speech-listening-initial.png)

![reconhecimento intermediário para uma restrição baseada em um arquivo de gramática sgrs](images/speech-listening-intermediate.png)

![tela de reconhecimento final para uma restrição baseada em um arquivo de gramática sgrs](images/speech-listening-complete.png)

A tela **Ouvindo** pode fornecer exemplos de palavras ou frases que o aplicativo pode reconhecer. Aqui, mostramos como usar as propriedades da classe [**SpeechRecognizerUIOptions**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizerUIOptions) (obtida chamando a propriedade [**SpeechRecognizer.UIOptions**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.uioptions) para personalizar o conteúdo na tela **Ouvindo**.

```CSharp
private async void WeatherSearch_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Listen for audio input issues.
    speechRecognizer.RecognitionQualityDegrading += speechRecognizer_RecognitionQualityDegrading;

    // Add a web search grammar to the recognizer.
    var webSearchGrammar = new Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint(Windows.Media.SpeechRecognition.SpeechRecognitionScenario.WebSearch, "webSearch");


    speechRecognizer.UIOptions.AudiblePrompt = "Say what you want to search for...";
    speechRecognizer.UIOptions.ExampleText = @"Ex. 'weather for London'";
    speechRecognizer.Constraints.Add(webSearchGrammar);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();
    //await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="related-articles"></a>Artigos relacionados


**Desenvolvedores**
* [Interações de fala](speech-interactions.md)
**Designers**
* [Diretrizes para design de controle por voz](https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions)
**Exemplos**
* [Exemplo de reconhecimento de fala e síntese de fala](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
 

 




