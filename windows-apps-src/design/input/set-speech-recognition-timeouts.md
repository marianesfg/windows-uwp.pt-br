---
Description: Defina quanto tempo um reconhecedor de fala ignora o silêncio ou sons não reconhecíveis (murmúrios) e continua escutando a entrada de fala.
title: Definir tempos limite de reconhecimento de fala
ms.assetid: 58F446AC-4A56-454D-8125-62A2C4DBFCC8
label: Speech recognition timeouts
template: detail.hbs
keywords: controle por voz, voz, reconhecimento de fala, linguagem natural, ditado, entrada, interação do usuário
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0df5f6c2e12b3b2e761ce45f95930dc179ef367f
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258261"
---
# <a name="set-speech-recognition-timeouts"></a>Definir tempos limite de reconhecimento de fala


Defina quanto tempo um reconhecedor de fala ignora o silêncio ou sons não reconhecíveis (murmúrios) e continua escutando a entrada de fala.

> **APIs importantes**: [**Timeouts**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.timeouts), [**SpeechRecognizerTimeouts**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizerTimeouts)

## <a name="set-a-timeout"></a>Definir um tempo limite


Aqui, especificamos vários valores de [**Timeouts**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.timeouts).

-   InitialSilenceTimeout — o período de tempo que um SpeechRecognizer detecta o silêncio (antes que qualquer resultado de reconhecimento seja gerado) e presume que não há entrada de fala.
-   BabbleTimeout — o período de tempo que um SpeechRecognizer continua a ouvir sons não reconhecíveis (murmúrios) antes que ele presuma que a entrada de fala terminou e finalize a operação de reconhecimento.
-   EndSilenceTimeout — o período de tempo que um SpeechRecognizer detecta o silêncio (depois que os resultados de reconhecimento foram gerados) e presume que a entrada de fala terminou.

**Note**  Timeouts can be set on a per-recognizer basis.

 

```CSharp
// Set timeout settings.
recognizer.Timeouts.InitialSilenceTimeout = TimeSpan.FromSeconds(6.0);
recognizer.Timeouts.BabbleTimeout = TimeSpan.FromSeconds(4.0);
recognizer.Timeouts.EndSilenceTimeout = TimeSpan.FromSeconds(1.2);
```

## <a name="related-articles"></a>Artigos relacionados


* [Interações de fala](speech-interactions.md)
**Amostras**
* [Speech recognition and speech synthesis sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
 

 




