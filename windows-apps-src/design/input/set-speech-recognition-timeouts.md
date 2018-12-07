---
Description: Set how long a speech recognizer ignores silence or unrecognizable sounds (babble) and continues listening for speech input.
title: Definir tempos limite de reconhecimento de fala
ms.assetid: 58F446AC-4A56-454D-8125-62A2C4DBFCC8
label: Speech recognition timeouts
template: detail.hbs
keywords: controle por voz, voz, reconhecimento de fala, linguagem natural, ditado, entrada, interação do usuário
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 73c7e889b4633dae9e416cf7ccde13eb3f58e8ee
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8788915"
---
# <a name="set-speech-recognition-timeouts"></a>Definir tempos limite de reconhecimento de fala


Defina quanto tempo um reconhecedor de fala ignora o silêncio ou sons não reconhecíveis (murmúrios) e continua escutando a entrada de fala.

> **APIs importantes**: [**Timeouts**](https://msdn.microsoft.com/library/windows/apps/dn653253), [**SpeechRecognizerTimeouts**](https://msdn.microsoft.com/library/windows/apps/dn653230)

## <a name="set-a-timeout"></a>Definir um tempo limite


Aqui, especificamos vários valores de [**Timeouts**](https://msdn.microsoft.com/library/windows/apps/dn653253).

-   InitialSilenceTimeout — o período de tempo que um SpeechRecognizer detecta o silêncio (antes que qualquer resultado de reconhecimento seja gerado) e presume que não há entrada de fala.
-   BabbleTimeout — o período de tempo que um SpeechRecognizer continua a ouvir sons não reconhecíveis (murmúrios) antes que ele presuma que a entrada de fala terminou e finalize a operação de reconhecimento.
-   EndSilenceTimeout — o período de tempo que um SpeechRecognizer detecta o silêncio (depois que os resultados de reconhecimento foram gerados) e presume que a entrada de fala terminou.

**Observação**tempos limite pode ser definida em uma base do reconhecedor.

 

```CSharp
// Set timeout settings.
recognizer.Timeouts.InitialSilenceTimeout = TimeSpan.FromSeconds(6.0);
recognizer.Timeouts.BabbleTimeout = TimeSpan.FromSeconds(4.0);
recognizer.Timeouts.EndSilenceTimeout = TimeSpan.FromSeconds(1.2);
```

## <a name="related-articles"></a>Artigos relacionados


* [Interações de fala](speech-interactions.md)
**Amostras**
* [Amostra de reconhecimento de fala e sintetização de voz](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




