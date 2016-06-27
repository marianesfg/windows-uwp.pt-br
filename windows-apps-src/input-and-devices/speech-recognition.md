---
author: Karl-Bridge-Microsoft
Description: "Use o reconhecimento de fala para fornecer entrada, especificar uma ação ou um comando e realizar tarefas."
title: Reconhecimento de fala
ms.assetid: 553C0FB7-35BC-4894-9EF1-906139E17552
label: Speech recognition
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 6d1449ede6912add8b8f7e60760d4547c035ed60

---

# Reconhecimento de fala


Use o reconhecimento de fala para fornecer entrada, especificar uma ação ou um comando e realizar tarefas.

**APIs importantes**

-   [**Windows.Media.SpeechRecognition**](https://msdn.microsoft.com/library/windows/apps/dn653262)



O reconhecimento de fala é realizado em um tempo de execução de fala, APIs de reconhecimento para programação do tempo de execução, gramáticas prontas para usar ditado e pesquisa na Web e uma interface do usuário do sistema padrão que auxilia os usuários a descobrir e usar recursos de reconhecimento de fala.


## <span id="Set_up_the_audio_feed"></span><span id="set_up_the_audio_feed"></span><span id="SET_UP_THE_AUDIO_FEED"></span>Configurar o feed de áudio


Certifique-se de que o dispositivo possui microfone ou equivalente.

Configure a capacidade do dispositivo **Microfone** ([**DeviceCapability**](https://msdn.microsoft.com/library/windows/apps/br211430)) no [Manifesto de pacote de aplicativo](https://msdn.microsoft.com/library/windows/apps/br211474) (arquivo **package.appxmanifest**) para obter acesso ao feed de áudio do microfone. Isso permite que o aplicativo grave áudio de microfones conectados.

Consulte [Declarações de funcionalidades do aplicativo](https://msdn.microsoft.com/library/windows/apps/mt270968).

## <span id="Recognize_speech_input"></span><span id="recognize_speech_input"></span><span id="RECOGNIZE_SPEECH_INPUT"></span>Reconhecer a entrada da fala


Uma *restrição* define as palavras e frases (vocabulário) que um aplicativo reconhece na entrada de fala. As restrições estão no núcleo do reconhecimento de fala e proporcionam a seu aplicativo mais exatidão no reconhecimento de fala.

Você pode usar vários tipos de restrições quando executar reconhecimento de fala:

1.  **Gramáticas predefinidas** ([**SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631446)).

    Ditado predefinido e gramáticas de pesquisa na Web fornecem o reconhecimento de fala de seu aplicativo sem precisar que você crie uma gramática. Ao utilizar essas gramáticas, o reconhecimento de fala é realizado por um serviço Web remoto, e os resultados são retornados ao dispositivo.

    A gramática de ditado de texto livre padrão pode reconhecer a maioria das palavras e frases que um usuário pode dizer em um determinado idioma e é otimizada para reconhecer frases curtas. A gramática de ditado predefinida será usada se você não especificar uma restrição para seu objeto [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226). O ditado de texto livre é útil quando você não deseja limitar os tipos de coisas que um usuário pode dizer. Os usos típicos incluem criação de notas ou ditado de conteúdo para uma mensagem.

    A gramática de pesquisa na Web, assim como uma gramática de ditado, contém um grande número de palavras e frases que um usuário pode dizer. No entanto, ela é otimizada para reconhecer termos que as pessoas normalmente usam ao pesquisar na Web.

    **Observação**  Como as gramáticas de pesquisa na Web e de ditado predefinidas podem ser grandes e online (não no dispositivo), o desempenho pode não ser tão rápido quanto o das gramáticas personalizadas instaladas no dispositivo.

     

    Essas gramáticas predefinidas podem ser usadas para reconhecer até 10 segundos de entrada de fala e não exigem nenhum esforço de criação de sua parte. No entanto, elas exigem uma conexão com uma rede.

    Para usar restrições de serviço Web, você deve habilitar o suporte à entrada de fala e ao ditado em **Configurações** ativando a opção "Acessar meus dados" na página Configurações -&gt; Privacidade -&gt; Controle por voz, escrita a tinta e digitação.

    Aqui, nós mostramos como testar se a entrada de fala está habilitada. Caso não esteja, abra página Configurações -&gt; Privacidade -&gt; Controle por voz, escrita a tinta e digitação.

    Primeiro, inicializamos uma variável global (HResultPrivacyStatementDeclined) para o valor HResult de 0x80045509. Consulte [Tratamento de exceções para em C\# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/dn532194).

```    CSharp
private static uint HResultPrivacyStatementDeclined = 0x80045509;</code></pre></td>
    </tr>
    </tbody>
    </table>
```

    We then catch any standard exceptions during recogntion and test if the [**HResult**](https://msdn.microsoft.com/library/windows/apps/br206579) value is equal to the value of the HResultPrivacyStatementDeclined variable. If so, we display a warning and call `await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts"));` to open the Settings page.

    <span codelanguage="CSharp"></span>
```    CSharp
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">C#</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
catch (Exception exception)
    {
      // Handle the speech privacy policy error.
      if ((uint)exception.HResult == HResultPrivacyStatementDeclined)
      {
        resultTextBlock.Visibility = Visibility.Visible;
        resultTextBlock.Text = "The privacy statement was declined. 
          Go to Settings -> Privacy -> Speech, inking and typing, and ensure you 
          have viewed the privacy policy, and &#39;Get To Know You&#39; is enabled.";
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

2.  **Restrições da lista programática** ([**SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631421)).

    Restrições de lista programática fornecem uma abordagem leve para criar gramáticas simples usando uma lista de palavras ou frases. Uma lista de restrições funciona bem para o reconhecimento de frases curtas e distintas. Especificar explicitamente todas as palavras em uma gramática também melhora a precisão do reconhecimento, porque o mecanismo de reconhecimento de fala deve processar somente a fala para confirmar uma correspondência. A lista também pode ser atualizada programaticamente.

    Uma restrição de lista consiste em uma matriz de cadeia de caracteres que representa a entrada de fala que seu aplicativo aceitará para uma operação de reconhecimento. Você pode criar uma restrição de lista em seu aplicativo criando um objeto de restrição de lista de reconhecimento de fala e passando uma matriz de cadeias de caracteres. Em seguida, adicione o objeto à coleção de restrições do reconhecedor. O reconhecimento é bem-sucedido quando o reconhecedor de fala reconhece qualquer uma das sequências de caracteres na matriz.

3.  **Gramáticas SRGS** ([**SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412)).

    Uma gramática SRGS (Especificação de Gramática de Reconhecimento de Fala) é um documento estático que, ao contrário de uma restrição de lista programática, usa o formato XML definido pela [SRGS Versão 1.0](http://go.microsoft.com/fwlink/p/?LinkID=262302). Uma gramática SRGS oferece maior controle sobre a experiência de reconhecimento de fala, permitindo a você capturar diversos significados semânticos em um único reconhecimento.

4.  **Restrições de comando de voz** ([**SpeechRecognitionVoiceCommandDefinitionConstraint**](https://msdn.microsoft.com/library/windows/apps/dn653220))

    Use um arquivo XML de Definição de comando de voz (VCD) para definir os comandos que o usuário pode usar para iniciar ações ao ativar seu aplicativo. Para obter mais detalhes, consulte [Iniciar um aplicativo em primeiro plano com comandos de voz na Cortana](launch-a-foreground-app-with-voice-commands-in-cortana.md).

**Observação**  O tipo de restrição que você usa depende da complexidade da experiência de reconhecimento que você deseja criar. Qualquer tipo pode ser a melhor escolha para uma tarefa específica de reconhecimento, e você pode encontrar usos para todos os tipos de restrição em seu aplicativo.
Para começar a usar restrições, consulte [Definir restrições de reconhecimento personalizadas](define-custom-recognition-constraints.md).

 

A gramática de ditado predefinida do Aplicativo Universal do Windows reconhece a maioria das palavras e frases curtas em um idioma. Ela é ativada por padrão quando um objeto reconhecedor de fala é instanciado sem restrições personalizadas.

Neste exemplo, você verá como:

-   Crie um reconhecedor de fala.
-   Compile as restrições padrão do Aplicativo Universal do Windows (nenhuma gramática foi adicionada ao conjunto de gramáticas do reconhecedor de fala).
-   Inicie a escuta da fala usando a interface do usuário de reconhecimento básica e o comentário TTS fornecido pelo método [**RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245). Use o método [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn653244) se a interface do usuário padrão não for necessária.

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

## <span id="Customize_the_recognition_UI"></span><span id="customize_the_recognition_ui"></span><span id="CUSTOMIZE_THE_RECOGNITION_UI"></span>Personalizar o reconhecimento da interface do usuário


Quando seu aplicativo tenta o reconhecimento de fala chamando [**SpeechRecognizer.RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245), várias telas são mostradas na ordem a seguir.

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

A tela **Ouvindo** pode fornecer exemplos de palavras ou frases que o aplicativo pode reconhecer. Aqui, mostramos como usar as propriedades da classe [**SpeechRecognizerUIOptions**](https://msdn.microsoft.com/library/windows/apps/dn653234) (obtida chamando a propriedade [**SpeechRecognizer.UIOptions**](https://msdn.microsoft.com/library/windows/apps/dn653254) para personalizar o conteúdo na tela **Ouvindo**.

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
    speechRecognizer.UIOptions.ExampleText = @"Ex. &#39;weather for London&#39;";
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

## <span id="related_topics"></span>Artigos relacionados


**Desenvolvedores**
* [Interações de fala](speech-interactions.md)
            
          
            **Designers**
* [Diretrizes para design de controle por voz](https://msdn.microsoft.com/library/windows/apps/dn596121)
            
          
            **Exemplos**
* [Exemplo de reconhecimento de fala e sintetização de controle por voz](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 







<!--HONumber=Jun16_HO3-->


