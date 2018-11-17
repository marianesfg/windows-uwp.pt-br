---
author: Karl-Bridge-Microsoft
Description: Learn how to capture and recognize long-form, continuous dictation speech input.
title: Habilitar o ditado contínuo
ms.assetid: 383B3E23-1678-4FBB-B36E-6DE2DA9CA9DC
label: Continuous dictation
template: detail.hbs
keywords: fala, voz, reconhecimento de fala, linguagem natural, ditado, entrada, interação do usuário
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ea7c0b92c5900e468023dd5b972942a89c2833c3
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7166507"
---
# <a name="continuous-dictation"></a>Ditado contínuo

Saiba como capturar e reconhecer entrada de fala de ditado contínuo de formato longo.

> **APIs importantes**: [**SpeechContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913896), [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913)

Em [Reconhecimento de fala](speech-recognition.md), você aprendeu como capturar e reconhecer entradas de fala relativamente curtas usando o método [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn653244) ou o método [**RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245) de um objeto [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226), por exemplo, ao compor uma mensagem SMS curta ou fazer uma pergunta.

Para sessões de reconhecimento de fala contínuas mais longas, como ditado ou email, use a propriedade [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913) de um [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) para obter um objeto [**SpeechContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913896).

> [!NOTE]
> Suporte a idiomas ditado depende do [dispositivo](https://docs.microsoft.com/windows/uwp/design/devices/) em que seu aplicativo é executado. Para computadores e notebooks, en-US só é reconhecida, enquanto Xbox e telefones podem reconhecer todos os idiomas compatíveis com o reconhecimento de fala. Para obter mais informações, consulte [especificar o idioma do reconhecedor de fala](specify-the-speech-recognizer-language.md).

## <a name="set-up"></a>Configurar

Seu aplicativo precisa de alguns objetos para gerenciar uma sessão de ditado contínuo:

- Uma instância de um objeto [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226).
- Uma referência a um dispatcher de interface do usuário para atualizar a interface do usuário durante o ditado.
- Uma maneira de acompanhar as palavras acumuladas faladas pelo usuário.

Aqui, declaramos uma instância [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) como um campo particular da classe code-behind. Seu aplicativo precisa armazenar uma referência em outro lugar se você quiser que o ditado contínuo persista além de uma única página XAML.

```CSharp
private SpeechRecognizer speechRecognizer;
```

Durante o ditado, o reconhecedor aciona eventos de um thread em segundo plano. Como um thread em segundo plano não pode atualizar diretamente a interface do usuário em XAML, seu aplicativo deve usar um dispatcher para atualizar a interface do usuário em resposta a eventos de reconhecimento.

Aqui, nós declaramos um campo particular que será inicializado posteriormente com o dispatcher de interface do usuário.

```CSharp
// Speech events may originate from a thread other than the UI thread.
// Keep track of the UI thread dispatcher so that we can update the
// UI in a thread-safe manner.
private CoreDispatcher dispatcher;
```

Para acompanhar o que o usuário está dizendo, você precisa tratar eventos de reconhecimento acionados pelo reconhecedor de fala. Esses eventos fornecem os resultados do reconhecimento de blocos de expressões do usuário.

Aqui, usamos um objeto [**StringBuilder**](https://msdn.microsoft.com/library/system.text.stringbuilder.aspx) para reter todos os resultados do reconhecimento obtidos durante a sessão. Novos resultados são acrescentados ao **StringBuilder** conforme eles são processados.

```CSharp
private StringBuilder dictatedTextBuilder;
```

## <a name="initialization"></a>Inicialização

Durante a inicialização do reconhecimento de fala contínua, você deve:

- Buscar o dispatcher do thread de interface do usuário se você atualizar a interface do usuário do aplicativo nos manipuladores de eventos de reconhecimento contínuo.
- Inicializar o reconhecedor de fala.
- Compilar a gramática de ditado interna.
    **Observação**  reconhecimento de fala requer pelo menos uma restrição para definir um vocabulário reconhecível. Se nenhuma restrição for especificada, uma gramática de ditado predefinida será usada. Consulte [Reconhecimento de fala](speech-recognition.md).
- Configure os ouvintes de eventos para eventos de reconhecimento.

Nesse exemplo, inicializamos o reconhecimento de fala no evento de página [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508).

1. Como os eventos acionados pelo reconhecedor de fala ocorrem em um thread em segundo plano, crie uma referência ao dispatcher para atualizações no thread de interface do usuário. [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508)  é sempre invocado no thread de interface do usuário.
```csharp
this.dispatcher = CoreWindow.GetForCurrentThread().Dispatcher;
```

2.  Em seguida, inicializamos a instância [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226).
```csharp
this.speechRecognizer = new SpeechRecognizer();
```

3.  Depois, adicionamos e compilamos a gramática que define todas as palavras e frases que podem ser reconhecidas pelo [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226).

    Se você não especificar explicitamente uma gramática, uma gramática de ditado predefinida será usada por padrão. Normalmente, a gramática padrão é melhor para ditado geral.

    Aqui, chamamos [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240) imediatamente, sem adicionar uma gramática.

    
```csharp
SpeechRecognitionCompilationResult result =
      await speechRecognizer.CompileConstraintsAsync();
```

## <a name="handle-recognition-events"></a>Tratar eventos de reconhecimento

Você pode capturar uma única expressão ou frase curta chamando [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn653244) ou [**RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245). 

No entanto, para capturar uma sessão de reconhecimento contínua mais longa, especificamos ouvintes de eventos a serem executados em segundo plano conforme o usuário fala e definimos manipuladores para criar a cadeia de caracteres de ditado.

Em seguida, usamos a propriedade [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913) do nosso reconhecedor para obter um objeto [**SpeechContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913896) que fornece métodos e eventos para gerenciar uma sessão de reconhecimento contínua.

Dois eventos em particular são essenciais:

- [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900), que ocorre quando o reconhecedor gerou alguns resultados.
- [**Completed**](https://msdn.microsoft.com/library/windows/apps/dn913899), que ocorre quando a sessão de reconhecimento contínua termina.

O evento [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) é acionado conforme o usuário fala. O reconhecedor escuta o usuário de forma contínua e periodicamente aciona um evento que passa um bloco de entrada de fala. Você deve examinar a entrada de fala usando a propriedade [**Result**](https://msdn.microsoft.com/library/windows/apps/dn913895) do argumento do evento e executar a ação apropriada no manipulador de eventos, por exemplo, acrescentando o texto a um objeto StringBuilder.

Como uma instância de [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432), a propriedade [**Result**](https://msdn.microsoft.com/library/windows/apps/dn913895) é útil para determinar se você deseja aceitar a entrada de fala Um [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) fornece duas propriedades para isso:

- [**Status**](https://msdn.microsoft.com/library/windows/apps/dn631440) indica se o reconhecimento foi executado com êxito. Pode haver falha no reconhecimento por vários motivos.
- [**Confidence**](https://msdn.microsoft.com/library/windows/apps/dn631434) indica a confiança relativa com que o reconhecedor compreendeu as palavras corretas.

Aqui estão as etapas básicas para dar suporte contínuo reconhecimento:  

1. Aqui, registramos o manipulador do evento de reconhecimento contínuo [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) no evento de página [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508).
```csharp
speechRecognizer.ContinuousRecognitionSession.ResultGenerated +=
        ContinuousRecognitionSession_ResultGenerated;
```

2.  Em seguida, verificamos a propriedade [**Confidence**](https://msdn.microsoft.com/library/windows/apps/dn631434). Se o valor de Confiança for [**Medium**](https://msdn.microsoft.com/library/windows/apps/dn631409) ou melhor, acrescentamos o texto ao StringBuilder. Também atualizamos a interface do usuário conforme coletamos entradas.

    **Observação**o evento [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) é acionado em um thread em segundo plano que não pode atualizar a interface do usuário diretamente. Se um manipulador precisar atualizar a interface do usuário (como faz a [\Amostra de fala e TTS\]), você deverá despachar as atualizações para o thread de interface do usuário por meio do método [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) do dispatcher.
```csharp
private async void ContinuousRecognitionSession_ResultGenerated(
      SpeechContinuousRecognitionSession sender,
      SpeechContinuousRecognitionResultGeneratedEventArgs args)
      {

        if (args.Result.Confidence == SpeechRecognitionConfidence.Medium ||
          args.Result.Confidence == SpeechRecognitionConfidence.High)
          {
            dictatedTextBuilder.Append(args.Result.Text + " ");

            await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              dictationTextBox.Text = dictatedTextBuilder.ToString();
              btnClearText.IsEnabled = true;
            });
          }
        else
        {
          await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              dictationTextBox.Text = dictatedTextBuilder.ToString();
            });
        }
      }
```

3.  Em seguida, tratamos o evento [**Completed**](https://msdn.microsoft.com/library/windows/apps/dn913899), que indica o final do ditado contínuo.

    A sessão termina quando você chama os métodos [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/dn913908) ou [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898) (descritos na próxima seção). A sessão também pode terminar quando ocorre um erro, ou quando o usuário para de falar. Verifique a propriedade [**Status**](https://msdn.microsoft.com/library/windows/apps/dn631440) do argumento do evento para determinar por que a sessão terminou ([**SpeechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433)).

    Aqui, registramos o manipulador do evento de reconhecimento contínuo [**Completed**](https://msdn.microsoft.com/library/windows/apps/dn913899) no evento de página [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508).
```csharp
speechRecognizer.ContinuousRecognitionSession.Completed +=
      ContinuousRecognitionSession_Completed;
```

4.  O manipulador de eventos verifica a propriedade Status para determinar se o reconhecimento foi executado com êxito. Ele também trata o caso em que o usuário parou de falar. Frequentemente, [**TimeoutExceeded**](https://msdn.microsoft.com/library/windows/apps/dn631433) é considerado um reconhecimento bem-sucedido porque significa que o usuário terminou de falar. Você deve tratar esse caso no seu código para obter uma boa experiência.

    **Observação**o evento [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) é acionado em um thread em segundo plano que não pode atualizar a interface do usuário diretamente. Se um manipulador precisar atualizar a interface do usuário (como faz a [\Amostra de fala e TTS\]), você deverá despachar as atualizações para o thread de interface do usuário por meio do método [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) do dispatcher.
```csharp
private async void ContinuousRecognitionSession_Completed(
      SpeechContinuousRecognitionSession sender,
      SpeechContinuousRecognitionCompletedEventArgs args)
      {
        if (args.Status != SpeechRecognitionResultStatus.Success)
        {
          if (args.Status == SpeechRecognitionResultStatus.TimeoutExceeded)
          {
            await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              rootPage.NotifyUser(
                "Automatic Time Out of Dictation",
                NotifyType.StatusMessage);

              DictationButtonText.Text = " Continuous Recognition";
              dictationTextBox.Text = dictatedTextBuilder.ToString();
            });
          }
          else
          {
            await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              rootPage.NotifyUser(
                "Continuous Recognition Completed: " + args.Status.ToString(),
                NotifyType.StatusMessage);

              DictationButtonText.Text = " Continuous Recognition";
            });
          }
        }
      }
```

## <a name="provide-ongoing-recognition-feedback"></a>Fornecer feedback de reconhecimento contínuo


Quando as pessoas conversam, elas frequentemente dependem do contexto para entender completamente o que está sendo dito. Da mesma forma, o reconhecedor de fala precisa de contexto para fornecer resultados de reconhecimento de alta confiabilidade. Por exemplo, sozinhas, as palavras "acento" e "assento" não podem ser diferenciadas até que mais contexto seja coletado das palavras adjacentes. Até que o reconhecedor tenha alguma confiança de que uma ou mais palavras foram reconhecidas corretamente, ele não acionará um evento [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900).

Isso pode resultar em uma experiência longe da ideal para o usuário, pois ele continua falando e nenhum resultado é fornecido até que o reconhecedor tenha confiança suficiente para acionar um evento [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900).

Trate o evento [**HypothesisGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913914) para melhorar essa aparente falta de capacidade de resposta. Esse evento é acionado sempre que o reconhecedor gera um novo conjunto de possíveis correspondências para a palavra que está sendo processada. O argumento do evento fornece uma propriedade [**Hypothesis**](https://msdn.microsoft.com/library/windows/apps/dn913911) que contém as correspondências atuais. Mostre as correspondências ao usuário conforme ele continua falando e assegure que o processamento ainda está ativo. Assim que a confiança for alta e um resultado de reconhecimento tiver sido determinado, substitua os resultados **Hypothesis** temporários pelo [**Result**](https://msdn.microsoft.com/library/windows/apps/dn913895) final fornecido no evento [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900).

Aqui, acrescentamos o texto hipotético e reticências ("…") ao valor atual da caixa de texto de saída [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683). O conteúdo da caixa de texto é atualizado conforme novas hipóteses são geradas e até que os resultados finais sejam obtidos do evento [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900).

```CSharp
private async void SpeechRecognizer_HypothesisGenerated(
  SpeechRecognizer sender,
  SpeechRecognitionHypothesisGeneratedEventArgs args)
  {

    string hypothesis = args.Hypothesis.Text;
    string textboxContent = dictatedTextBuilder.ToString() + " " + hypothesis + " ...";

    await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
      dictationTextBox.Text = textboxContent;
      btnClearText.IsEnabled = true;
    });
  }
```

## <a name="start-and-stop-recognition"></a>Iniciar e parar o reconhecimento


Antes de iniciar uma sessão de reconhecimento, verifique o valor da propriedade [**State**](https://msdn.microsoft.com/library/windows/apps/dn913915) do reconhecedor de fala. O reconhecedor de fala deve estar em um estado [**Idle**](https://msdn.microsoft.com/library/windows/apps/dn653227).

Depois de verificar o estado do reconhecedor de fala, começamos a sessão chamando o método [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/dn913901) da propriedade [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913) do reconhecedor de fala.

```CSharp
if (speechRecognizer.State == SpeechRecognizerState.Idle)
{
  await speechRecognizer.ContinuousRecognitionSession.StartAsync();
}
```

O reconhecimento pode ser interrompido de duas maneiras:

-   [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/dn913908) permite que qualquer evento de reconhecimento pendente seja concluído ([**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) continua sendo acionado até que todas as operações de reconhecimento pendentes sejam concluídas).
-   [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898) finaliza a sessão de reconhecimento imediatamente e descarta qualquer resultado pendente.

Depois de verificar o estado do reconhecedor de fala, interrompemos a sessão chamando o método [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898) da propriedade [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913) do reconhecedor de fala.

```CSharp
if (speechRecognizer.State != SpeechRecognizerState.Idle)
{
  await speechRecognizer.ContinuousRecognitionSession.CancelAsync();
}
```

> [!NOTE]
> Um evento [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) pode ocorrer após uma chamada para [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898).  
> Por causa do multithreading, um evento [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) ainda pode permanecer na pilha quando [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898) é chamado. Nesse caso, o evento **ResultGenerated** ainda é acionado.  
> Se você definir qualquer campo particular ao cancelar a sessão de reconhecimento, sempre confirme seus valores no manipulador [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900). Por exemplo, não suponha que um campo será inicializado em seu manipulador se você defini-lo como nulo ao cancelar a sessão.

 

## <a name="related-articles"></a>Artigos relacionados


* [Interações de controle por voz](speech-interactions.md)

**Exemplos**
* [Exemplo de reconhecimento de fala e sintetização de controle por voz](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




