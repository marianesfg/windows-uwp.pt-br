---
author: Karl-Bridge-Microsoft
Description: Learn how to select an installed language to use for speech recognition.
title: Especificar o idioma do reconhecedor de fala
ms.assetid: 4C463A1B-AF6A-46FD-A839-5D6724955B38
label: Specify the speech recognizer language
template: detail.hbs
keywords: controle por voz, voz, reconhecimento de fala, linguagem natural, ditado, entrada, interação do usuário
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 471f222bb22293ccb3b66f5387d43b74fc53b910
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1653865"
---
# <a name="specify-the-speech-recognizer-language"></a>Especificar o idioma do reconhecedor de fala


Saiba como selecionar um idioma instalado para usá-lo para reconhecimento de fala.

> **APIs importantes**: [**SupportedTopicLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653251), [**SupportedGrammarLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653250), [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804)


Aqui, enumeramos os idiomas instalados em um sistema, identificamos qual é o idioma padrão e selecionamos um idioma diferente para o reconhecimento.

**Pré-requisitos:**

Este tópico complementa [Reconhecimento de fala](speech-recognition.md).

Você deve ter uma noção básica de reconhecimento de fala e restrições de reconhecimento.

Se você for iniciante no desenvolvimento de aplicativos da Plataforma Universal do Windows (UWP), consulte estes tópicos para familiarizar-se com as tecnologias discutidas aqui.

-   [Criar seu primeiro aplicativo](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Saiba mais sobre eventos com [Visão geral de eventos e eventos roteados](https://msdn.microsoft.com/library/windows/apps/mt185584)

**Diretrizes de experiência do usuário:**

Para obter dicas úteis sobre o design de um aplicativo habilitado para controle por voz interessante e prático, consulte [Diretrizes para design de controle por voz](https://msdn.microsoft.com/library/windows/apps/dn596121).

## <a name="identify-the-default-language"></a>Identifique o idioma padrão


Um reconhecedor de fala usa o idioma do controle por voz do sistema como seu idioma de reconhecimento padrão. Esse idioma é definido pelo usuário na tela Settings &gt; System &gt; Speech &gt; Speech Language do dispositivo.

Identificamos o idioma padrão verificando a propriedade estática [**SystemSpeechLanguage**](https://msdn.microsoft.com/library/windows/apps/dn653252).

```CSharp
var language = SpeechRecognizer.SystemSpeechLanguage; 
```

## <a name="confirm-an-installed-language"></a>Confirmar um idioma instalado


Os idiomas instalados podem variar entre dispositivos. Verifique a existência de um idioma se você depender dele para uma determinada restrição.

**Observação**  É necessário reinicializar depois da instalação de um novo pacote de idiomas. Uma exceção com o código de erro SPERR\_NOT\_FOUND (0x8004503a) será acionada se o idioma especificado não tiver suporte ou sua instalação não tiver sido concluída.

 

Determine os idiomas com suporte em um dispositivo verificando uma das duas propriedades estáticas da classe [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226):

-   [**SupportedTopicLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653251) - A coleção de objetos [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804) usada com ditado predefinido e gramáticas de pesquisa na Web.

-   [**SupportedGrammarLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653250) - A coleção de objetos [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804) usada com uma restrição de lista ou um arquivo SRGS (Especificação de Gramática de Reconhecimento de Fala).

## <a name="specify-a-language"></a>Especificar um idioma


Para especificar um idioma, passe um objeto [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804) no construtor [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226).

Aqui, especificamos "en-US" como o idioma de reconhecimento.


```CSharp
var language = new Windows.Globalization.Language(“en-US”); 
var recognizer = new SpeechRecognizer(language); 
```

## <a name="remarks"></a>Comentários


Uma restrição de tópico pode ser configurada adicionando uma [**SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631446) à coleção [**Constraints**](https://msdn.microsoft.com/library/windows/apps/dn653241) do [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) e chamando [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240). Um [**SpeechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433) de **TopicLanguageNotSupported** é retornado se o reconhecedor não for inicializado com um idioma de tópico com suporte.

Uma restrição de lista é configurada adicionando uma [**SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631421) à coleção [**Constraints**](https://msdn.microsoft.com/library/windows/apps/dn653241) do [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) e chamando [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240). Você não pode especificar o idioma de uma lista personalizada diretamente. Em vez disso, a lista será processada usando o idioma do reconhecedor.

Uma gramática SRGS é um formato XML de padrão aberto representado pela classe [**SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412). Ao contrário de listas personalizadas, você pode especificar o idioma da gramática na marcação SRGS. Haverá falha de [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240) com um [**SpeechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433) de **TopicLanguageNotSupported** se o reconhecedor não for inicializado com o mesmo idioma que a marcação SRGS.

## <a name="related-articles"></a>Artigos relacionados

**Desenvolvedores**

* [Interações de controle por voz](speech-interactions.md)

**Designers**

* [Diretrizes para design de controle por voz](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Exemplos**

* [Exemplo de reconhecimento de fala e sintetização de controle por voz](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 



