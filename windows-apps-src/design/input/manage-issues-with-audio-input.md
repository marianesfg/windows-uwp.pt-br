---
Description: Learn how to manage issues with speech-recognition accuracy caused by audio-input quality.
title: Gerenciar problemas com entrada de áudio
ms.assetid: 3E36C683-C96A-4FEE-AD52-FDB87E0CC299
label: Manage audio input issues
template: detail.hbs
keywords: controle por voz, voz, reconhecimento de fala, linguagem natural, ditado, entrada, interação do usuário
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5c33e2890ce962f1321ef40ca0e0605e0f4e1f5f
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7831797"
---
# <a name="manage-issues-with-audio-input"></a>Gerenciar problemas com entrada de áudio


Saiba como gerenciar problemas na precisão do reconhecimento de fala causados pela qualidade da entrada de áudio.

> **APIs importantes**: [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226), [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243), [**SpeechRecognitionAudioProblem**](https://msdn.microsoft.com/library/windows/apps/dn631406)


## <a name="assess-audio-input-quality"></a>Avaliar a qualidade da entrada de áudio


Quando o reconhecimento de fala estiver ativo, use o evento [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243) de seu reconhecedor de fala para determinar se um ou mais problemas de áudio podem estar interferindo na entrada de fala. O argumento do evento ([**SpeechRecognitionQualityDegradingEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn631430)) fornece a propriedade [**Problem**](https://msdn.microsoft.com/library/windows/apps/dn631431), que descreve os problemas detectados com a entrada de áudio.

O reconhecimento pode ser afetado por excesso de ruído de fundo, um microfone mudo e o volume ou a velocidade do alto-falante.

Aqui, configuramos um reconhecedor de fala e começamos a escutar o evento [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243).

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
    speechRecognizer.UIOptions.ExampleText = "Ex. 'weather for London'";
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

## <a name="manage-the-speech-recognition-experience"></a>Gerenciar a experiência de reconhecimento de fala


Use a descrição fornecida pela propriedade [**Problem**](https://msdn.microsoft.com/library/windows/apps/dn631431) para ajudar o usuário a melhorar condições para o reconhecimento.

Aqui, criamos um manipulador para o evento [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243) que verifica se há um nível de volume baixo. Em seguida, usamos um objeto [**SpeechSynthesizer**](https://msdn.microsoft.com/library/windows/apps/dn298152) para sugerir que o usuário tente falar mais alto.

```CSharp
private async void speechRecognizer_RecognitionQualityDegrading(
    Windows.Media.SpeechRecognition.SpeechRecognizer sender,
    Windows.Media.SpeechRecognition.SpeechRecognitionQualityDegradingEventArgs args)
{
    // Create an instance of a speech synthesis engine (voice).
    var speechSynthesizer =
        new Windows.Media.SpeechSynthesis.SpeechSynthesizer();

    // If input speech is too quiet, prompt the user to speak louder.
    if (args.Problem == Windows.Media.SpeechRecognition.SpeechRecognitionAudioProblem.TooQuiet)
    {
        // Generate the audio stream from plain text.
        Windows.Media.SpeechSynthesis.SpeechSynthesisStream stream;
        try
        {
            stream = await speechSynthesizer.SynthesizeTextToStreamAsync("Try speaking louder");
            stream.Seek(0);
        }
        catch (Exception)
        {
            stream = null;
        }

        // Send the stream to the MediaElement declared in XAML.
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.High, () =>
        {
            this.media.SetSource(stream, stream.ContentType);
        });
    }
}
```

## <a name="related-articles"></a>Artigos relacionados


* [Interações de controle por voz](speech-interactions.md)

**Exemplos**
* [Exemplo de reconhecimento de fala e sintetização de controle por voz](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




