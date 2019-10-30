---
Description: Saiba como selecionar um idioma instalado para usá-lo para reconhecimento de fala.
title: Especificar o idioma do reconhecedor de fala
ms.assetid: 4C463A1B-AF6A-46FD-A839-5D6724955B38
label: Specify the speech recognizer language
template: detail.hbs
keywords: controle por voz, voz, reconhecimento de fala, linguagem natural, ditado, entrada, interação do usuário
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1aa57af7e51fd7d6ef151909eccc444da2c44707
ms.sourcegitcommit: 05be6929cd380a9dd241cc1298fd53f11c93d774
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73062051"
---
# <a name="specify-the-speech-recognizer-language"></a>Especificar o idioma do reconhecedor de fala


Saiba como selecionar um idioma instalado para usá-lo para reconhecimento de fala.

> **APIs importantes**: [**SupportedTopicLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages), [**SupportedGrammarLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages), [**Language**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language)


Aqui, enumeramos os idiomas instalados em um sistema, identificamos qual é o idioma padrão e selecionamos um idioma diferente para o reconhecimento.

**Pré-requisitos**

Este tópico complementa [Reconhecimento de fala](speech-recognition.md).

Você deve ter uma noção básica de reconhecimento de fala e restrições de reconhecimento.

Se você for iniciante no desenvolvimento de aplicativos da Plataforma Universal do Windows (UWP), consulte estes tópicos para familiarizar-se com as tecnologias discutidas aqui.

-   [Crie seu primeiro aplicativo](https://docs.microsoft.com/windows/uwp/get-started/your-first-app)
-   Saiba mais sobre eventos com [Visão geral de eventos e eventos roteados](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview)

**Diretrizes de experiência do usuário:**

Para obter dicas úteis sobre o design de um aplicativo habilitado para controle por voz interessante e prático, consulte [Diretrizes para design de controle por voz](https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions).

## <a name="identify-the-default-language"></a>Identifique o idioma padrão


Um reconhecedor de fala usa o idioma do controle por voz do sistema como seu idioma de reconhecimento padrão. Esse idioma é definido pelo usuário na tela Settings &gt; System &gt; Speech &gt; Speech Language do dispositivo.

Identificamos o idioma padrão verificando a propriedade estática [**SystemSpeechLanguage**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.systemspeechlanguage).

```CSharp
var language = SpeechRecognizer.SystemSpeechLanguage; 
```

## <a name="confirm-an-installed-language"></a>Confirmar um idioma instalado


Os idiomas instalados podem variar entre dispositivos. Verifique a existência de um idioma se você depender dele para uma determinada restrição.

**Observação**  uma reinicialização é necessária após A instalação de um novo pacote de idiomas. Uma exceção com o código de erro SPERR\_não\_encontrada (0x8004503a) será gerada se o idioma especificado não tiver suporte ou se a instalação não tiver sido concluída.

 

Determine os idiomas com suporte em um dispositivo verificando uma das duas propriedades estáticas da classe [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer):

-   [**SupportedTopicLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages)— a coleção de objetos de [**linguagem**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language) usada com ditado predefinido e gramáticas de pesquisa na Web.

-   [**SupportedGrammarLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages)— a coleção de objetos de [**linguagem**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language) usada com uma restrição de lista ou um arquivo de especificação de gramática de reconhecimento de fala (SRGS).

## <a name="specify-a-language"></a>Especificar um idioma


Para especificar um idioma, passe um objeto [**Language**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language) no construtor [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer).

Aqui, especificamos "en-US" como o idioma de reconhecimento.


```CSharp
var language = new Windows.Globalization.Language("en-US"); 
var recognizer = new SpeechRecognizer(language); 
```

## <a name="remarks"></a>Comentários


Uma restrição de tópico pode ser configurada adicionando uma [**SpeechRecognitionTopicConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint) à coleção [**Constraints**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) do [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) e chamando [**CompileConstraintsAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync). Um [**SpeechRecognitionResultStatus**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus) de **TopicLanguageNotSupported** é retornado se o reconhecedor não for inicializado com um idioma de tópico com suporte.

Uma restrição de lista é configurada adicionando uma [**SpeechRecognitionListConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint) à coleção [**Constraints**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) do [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) e chamando [**CompileConstraintsAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync). Você não pode especificar o idioma de uma lista personalizada diretamente. Em vez disso, a lista será processada usando o idioma do reconhecedor.

Uma gramática SRGS é um formato XML de padrão aberto representado pela classe [**SpeechRecognitionGrammarFileConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint). Ao contrário de listas personalizadas, você pode especificar o idioma da gramática na marcação SRGS. [**CompileConstraintsAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync) falhará com um [**SpeechRecognitionResultStatus**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus) de **TopicLanguageNotSupported** se o reconhecedor não for inicializado para o mesmo idioma da marcação SRGS.

## <a name="related-articles"></a>Artigos relacionados

**Desenvolvedores**

* [Interações de controle por voz](speech-interactions.md)

**Designers**

* [Diretrizes de design de controle por voz](https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions)

**Exemplos**

* [Exemplo de reconhecimento de fala e síntese de fala](https://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




