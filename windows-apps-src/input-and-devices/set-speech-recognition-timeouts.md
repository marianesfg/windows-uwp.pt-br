---
author: Karl-Bridge-Microsoft
Description: "Defina quanto tempo um reconhecedor de fala ignora o silêncio ou sons não reconhecíveis (murmúrios) e continua escutando a entrada de fala."
title: Definir tempos limite de reconhecimento de fala
ms.assetid: 58F446AC-4A56-454D-8125-62A2C4DBFCC8
label: Speech recognition timeouts
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a2ec5e64b91c9d0e401c48902a18e5496fc987ab
ms.openlocfilehash: 0e5b60b2a59478899343ed4dee9d9c2039607491

---

# Definir tempos limite de reconhecimento de fala
Defina quanto tempo um reconhecedor de fala ignora o silêncio ou sons não reconhecíveis (murmúrios) e continua escutando a entrada de fala.

**APIs importantes**

-   [**Tempos limite**](https://msdn.microsoft.com/library/windows/apps/dn653253)
-   [**SpeechRecognizerTimeouts**](https://msdn.microsoft.com/library/windows/apps/dn653230)


## Definir um tempo limite


Aqui, especificamos vários valores de [**Timeouts**](https://msdn.microsoft.com/library/windows/apps/dn653253).

-   InitialSilenceTimeout — o período de tempo que um SpeechRecognizer detecta o silêncio (antes que qualquer resultado de reconhecimento seja gerado) e presume que não há entrada de fala.
-   BabbleTimeout — o período de tempo que um SpeechRecognizer continua a ouvir sons não reconhecíveis (murmúrios) antes que ele presuma que a entrada de fala terminou e finalize a operação de reconhecimento.
-   EndSilenceTimeout — o período de tempo que um SpeechRecognizer detecta o silêncio (depois que os resultados de reconhecimento foram gerados) e presume que a entrada de fala terminou.

**Observação**  É possível definir Tempos limite com base no reconhecedor.

 

```CSharp
// Set timeout settings.
recognizer.Timeouts.InitialSilenceTimeout = TimeSpan.FromSeconds(6.0);
recognizer.Timeouts.BabbleTimeout = TimeSpan.FromSeconds(4.0);
recognizer.Timeouts.EndSilenceTimeout = TimeSpan.FromSeconds(1.2);
```

## Artigos relacionados


* [Interações de fala](speech-interactions.md)
**Amostras**
* [Amostra de reconhecimento de fala e sintetização de voz](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 







<!--HONumber=Aug16_HO3-->


