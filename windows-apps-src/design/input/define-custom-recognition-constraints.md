---
Description: Saiba como definir e usar restrições personalizadas para reconhecimento de fala.
title: Definir restrições de reconhecimento personalizadas
ms.assetid: 26289DE5-6AC9-42C3-A160-E522AE62D2FC
label: Define custom recognition constraints
template: detail.hbs
keywords: controle por voz, voz, reconhecimento de fala, linguagem natural, ditado, entrada, interação do usuário
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 539acb242cfe6ee70d1311133a3f1a193860541a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631721"
---
# <a name="define-custom-recognition-constraints"></a>Definir restrições de reconhecimento personalizadas



Saiba como definir e usar restrições personalizadas para reconhecimento de fala.

> **APIs importantes**: [**SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631446), [ **SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631421), [ **SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412)


O reconhecimento de fala requer pelo menos uma restrição para definir um vocabulário reconhecível. Se nenhuma restrição for especificada, será usada a gramática de ditado predefinida de Aplicativos Universais do Windows. Consulte [Reconhecimento de fala](speech-recognition.md).


## <a name="add-constraints"></a>Adicionar restrições


Use a propriedade [**SpeechRecognizer.Constraints**](https://msdn.microsoft.com/library/windows/apps/dn653241) para adicionar restrições a um reconhecedor de fala.

Aqui, abordamos os três tipos de restrições de reconhecimento de fala usados de dentro de um aplicativo. Para restrições de comando de voz, consulte [Iniciar um aplicativo em primeiro plano com comandos de voz na Cortana](https://msdn.microsoft.com/cortana/voicecommands/launch-a-foreground-app-with-voice-commands-in-cortana).

-   [**SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631446)— uma restrição com base em uma gramática predefinida (pesquisa de ditado ou web).
-   [**SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631421)— uma restrição com base em uma lista de palavras ou frases.
-   [**SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412)— uma restrição definida em um arquivo de especificação de gramática de reconhecimento de fala (SRGS).

Cada reconhecedor de fala pode ter uma coleção de restrição. Somente essas combinações de restrições são válidas:

- Uma restrição baseada em um único tópico (ditado ou pesquisa na Web)
- No Windows 10 Fall Creators Update (10.0.16299.15) e posteriores, uma restrição baseada em um único tópico pode ser combinada com uma restrição de lista
- Uma combinação da lista de restrições e/ou restrições de arquivo de gramática.

> [!Important]
> Chame o método **[SpeechRecognizer.CompileConstraintsAsync](https://msdn.microsoft.com/library/windows/apps/dn653240)** para compilar as restrições antes de iniciar o processo de reconhecimento.

## <a name="specify-a-web-search-grammar-speechrecognitiontopicconstraint"></a>Especifique uma gramática de pesquisa na Web (SpeechRecognitionTopicConstraint)

Restrições de tópico (ditado ou gramática de pesquisa na Web) devem ser adicionadas à coleção de restrições de um reconhecedor de fala.

> [!NOTE]
> Você pode usar uma [SpeechRecognitionListConstraint](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint) em conjunto com uma [SpeechRecognitionTopicConstraint](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint) para aumentar a precisão do ditado fornecendo um conjunto de palavras-chave específicas ao domínio que, a seu ver, provavelmente serão usadas durante o ditado.

Aqui, adicionamos uma gramática de pesquisa na Web para a coleção de restrições.

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

## <a name="specify-a-programmatic-list-constraint-speechrecognitionlistconstraint"></a>Especifique uma restrição de lista programática (SpeechRecognitionListConstraint)


Restrições de lista devem ser adicionadas à coleção de restrições de um reconhecedor de fala.

Lembre-se bem do seguinte:

-   É possível adicionar várias restrições de lista a uma coleção de restrições.
-   É possível usar qualquer coleção que implemente **IIterable&lt;String&gt;** para os valores da cadeia de caracteres.

Aqui, especificamos programaticamente uma matriz de palavras como uma restrição de lista e a adicionamos à coleção de restrições de um reconhecedor de fala.

```CSharp
private async void YesOrNo_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // You could create this array dynamically.
    string[] responses = { "Yes", "No" };


    // Add a list constraint to the recognizer.
    var listConstraint = new Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint(responses, "yesOrNo");

    speechRecognizer.UIOptions.ExampleText = @"Ex. 'yes', 'no'";
    speechRecognizer.Constraints.Add(listConstraint);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="specify-an-srgs-grammar-constraint-speechrecognitiongrammarfileconstraint"></a>Especifique uma restrição de gramática SRGS (SpeechRecognitionGrammarFileConstraint)


Arquivos de gramática SRGS devem ser adicionados à coleção de restrições de um reconhecedor de fala.

SRGS versão 1.0 é a linguagem de marcação padrão do setor para a criação de gramáticas em formato XML para reconhecimento de fala. Embora os Aplicativos Universais do Windows ofereçam alternativas ao uso da SRGS para criar gramáticas de reconhecimento de fala, você poderá descobrir que usar a SRGS para criar gramáticas produz os melhores resultados, especialmente para cenários de reconhecimento de fala mais complexos.

As gramáticas da SRGS fornecem um conjunto completo de recursos para ajudar você a definir a complexa arquitetura da interação de voz para seus aplicativos. Por exemplo, com as gramáticas da SRGS você pode:

-   Especificar a ordem na qual as palavras e frases devem ser ditas para serem reconhecidas.
-   Combinar palavras de várias listas e frases para serem reconhecidas.
-   Criar um link para outras gramáticas.
-   Atribuir um peso a uma outra palavra ou frase para aumentar ou diminuir a probabilidade de que ela será usada para corresponder à entrada de fala.
-   Incluir palavras ou frases opcionais.
-   Utilize regras especiais que ajudam a filtrar entradas inesperadas ou não especificadas, como fala aleatória que não corresponde à gramática, ou ruído de fundo.
-   Use a semântica para definir o reconhecimento de fala por meio de seu aplicativo.
-   Especifique as pronúncias, quer em linha com uma gramática ou por meio de um link de um léxico.

Para saber mais sobre elementos e atributos da SRGS, consulte a [Referência XML para gramáticas da SRGS](https://go.microsoft.com/fwlink/p/?LinkID=269886). Para começar a criar uma gramática SRGS, consulte [Como criar uma gramática XML básica](https://go.microsoft.com/fwlink/p/?LinkID=269887).

Lembre-se bem do seguinte:

-   É possível adicionar várias restrições de lista à coleção de restrições.
-   Use a extensão de arquivo .grxml para documentos de gramática baseados em XML que estão em conformidade com as regras SRGS.

Esse exemplo usa uma gramática SRGS definida em um arquivo denominado srgs.grxml (descrito posteriormente). Nas propriedades do arquivo, **Ação de Pacote** é definida para **Conteúdo** com **Copiar para diretório de saída** definido para **Copiar sempre**:

```CSharp
private async void Colors_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Add a grammar file constraint to the recognizer.
    var storageFile = await Windows.Storage.StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///Colors.grxml"));
    var grammarFileConstraint = new Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint(storageFile, "colors");

    speechRecognizer.UIOptions.ExampleText = @"Ex. 'blue background', 'green text'";
    speechRecognizer.Constraints.Add(grammarFileConstraint);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

Esse arquivo SRGS (srgs.grxml) inclui tags de interpretação semântica. Essas tags fornecem um mecanismo para retornar dados correspondentes de gramática para seu aplicativo. Gramáticas devem estar em conformidade com o World Wide Web Consortium (W3C) [interpretação de semântica de conversão de fala reconhecimento (SISR) 1.0](https://go.microsoft.com/fwlink/p/?LinkID=201765) especificação.

Aqui, escutamos as variantes de "sim" e "não".

```CSharp
<grammar xml:lang="en-US" 
         root="yesOrNo"
         version="1.0" 
         tag-format="semantics/1.0"
         xmlns="http://www.w3.org/2001/06/grammar">

    <!-- The following rules recognize variants of yes and no. -->
      <rule id="yesOrNo">
         <one-of>
            <item>
              <one-of>
                 <item>yes</item>
                 <item>yeah</item>
                 <item>yep</item>
                 <item>yup</item>
                 <item>un huh</item>
                 <item>yay yus</item>
              </one-of>
              <tag>out="yes";</tag>
            </item>
            <item>
              <one-of>
                 <item>no</item>
                 <item>nope</item>
                 <item>nah</item>
                 <item>uh uh</item>
               </one-of>
               <tag>out="no";</tag>
            </item>
         </one-of>
      </rule>
</grammar>
```

## <a name="manage-constraints"></a>Gerenciar restrições


Depois que uma coleção de restrições for carregada para reconhecimento, seu aplicativo poderá gerenciar quais restrições são permitidas para operações de reconhecimento definindo a propriedade [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn631402) de uma restrição como **true** ou **false**. A configuração padrão é **true**.

Geralmente, é mais eficaz carregar restrições uma vez e, em seguida, habilitá-las ou desabilitá-las conforme necessário, em vez de carregar, descarregar e compilar restrições para cada operação de reconhecimento. Use a propriedade [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn631402), conforme necessário.

Restringir o número de restrições para limitar a quantidade de dados que o reconhecedor de fala precisa pesquisar para localizar uma correspondência para entrada de fala. Isso pode melhorar o desempenho e a precisão do reconhecimento de fala.

Decida quais restrições são habilitadas com base nas frases que seu aplicativo espera no contexto da operação de reconhecimento atual. Por exemplo, se o contexto do aplicativo atual for exibir uma cor, você poderá não precisar habilitar uma restrição que reconheça os nomes de animais.

Para avisar o usuário sobre o que pode ser falado, use as propriedades [**SpeechRecognizerUIOptions.AudiblePrompt**](https://msdn.microsoft.com/library/windows/apps/dn653235) e [**SpeechRecognizerUIOptions.ExampleText**](https://msdn.microsoft.com/library/windows/apps/dn653236), que é possível definir usando a propriedade [**SpeechRecognizer.UIOptions**](https://msdn.microsoft.com/library/windows/apps/dn653254). Preparar usuários sobre o que eles podem dizer durante a operação de reconhecimento aumenta a probabilidade deles falarem uma frase que possa ser correspondida a uma restrição ativa.

## <a name="related-articles"></a>Artigos relacionados


* [Interações de controle por voz](speech-interactions.md)

**Exemplos**
* [Reconhecimento de fala e amostra de síntese de fala](https://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




