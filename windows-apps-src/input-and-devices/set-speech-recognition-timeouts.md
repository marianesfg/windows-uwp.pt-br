---
author: Karl-Bridge-Microsoft
Description: "Defina quanto tempo um reconhecedor de fala ignora o silêncio ou sons não reconhecíveis (murmúrios) e continua escutando a entrada de fala."
title: Definir tempos limite de reconhecimento de fala
ms.assetid: 58F446AC-4A56-454D-8125-62A2C4DBFCC8
label: Speech recognition timeouts
template: detail.hbs
keywords: "controle por voz, voz, reconhecimento de fala, linguagem natural, ditado, entrada, interação do usuário"
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 93314c29de926b988d01da09a6ce7feb21fdcf20
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="set-speech-recognition-timeouts"></a>Definir tempos limite de reconhecimento de fala
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Defina quanto tempo um reconhecedor de fala ignora o silêncio ou sons não reconhecíveis (murmúrios) e continua escutando a entrada de fala.

<div class="important-apis" >
<b>APIs importantes</b><br/>
<ul>
<li>[**Tempos limite**](https://msdn.microsoft.com/library/windows/apps/dn653253)</li>
<li>[**SpeechRecognizerTimeouts**](https://msdn.microsoft.com/library/windows/apps/dn653230)</li>
</ul>
</div>

## <a name="set-a-timeout"></a>Definir um tempo limite


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

## <a name="related-articles"></a>Artigos relacionados


* [Interações de fala](speech-interactions.md)
**Amostras**
* [Amostra de reconhecimento de fala e sintetização de voz](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




