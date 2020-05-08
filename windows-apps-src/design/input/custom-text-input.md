---
Description: As APIs de texto principal no namespace Windows. UI. Text. Core permitem que um aplicativo de aplicativo do Windows receba entrada de texto de qualquer serviço de texto com suporte em dispositivos Windows.
title: Visão geral da entrada de texto personalizada
ms.assetid: 58F5F7AC-6A4B-45FC-8C2A-942730FD7B74
label: Custom text input
template: detail.hbs
keywords: teclado, texto, texto básico, texto personalizado, Estrutura de Serviços de Texto, entrada, interações do usuário
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f42f7da525211442c37d34a2e3ce96ec9f7af568
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970931"
---
# <a name="custom-text-input"></a>Entrada de texto personalizado



As APIs de texto principal no namespace [**Windows. UI. Text. Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Text.Core) permitem que um aplicativo de aplicativo do Windows receba entrada de texto de qualquer serviço de texto com suporte em dispositivos Windows. As APIs são semelhantes às APIs [Estrutura de Serviços de Texto](https://docs.microsoft.com/windows/desktop/TSF/text-services-framework) em que o aplicativo não precisa ter conhecimento detalhado dos serviços de texto. Isso permite que o aplicativo receba texto em qualquer idioma e de qualquer tipo de entrada, como teclado, fala ou caneta.

> **APIs importantes**: [**Windows.UI.Text.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Text.Core), [**CoreTextEditContext**](https://docs.microsoft.com/uwp/api/Windows.UI.Text.Core.CoreTextEditContext)

## <a name="why-use-core-text-apis"></a>Por que usar APIs de texto básicas?


Para muitos aplicativos, os controles de caixa de texto XAML ou HTML são suficientes para entrada de texto e edição. No entanto, caso seu aplicativo trate cenários de texto complexos, como um aplicativo de processamento de texto, talvez você precise da flexibilidade de um controle de edição de texto personalizado. Você pode usar as APIs de teclado [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) para criar o controle de edição de texto, mas elas não oferecem uma maneira de receber entrada de texto com base na composição, algo necessário para dar suporte a idiomas do leste asiático.

Em vez disso, use as APIs [**Windows.UI.Text.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Text.Core) quando você precisar criar um controle de edição de texto personalizado. Essas APIs foram projetadas para dar muita flexibilidade no processamento de entrada de texto, em qualquer idioma e permitir que você ofereça a experiência de texto mais adequada ao seu aplicativo. Os controles de edição e entrada de texto criados com APIs de texto básicas podem receber entrada de texto de todos os métodos de entrada de texto em dispositivos Windows, dos Editores de Método de Entrada (IMEs) baseados na [Estrutura de Serviços de Texto](https://docs.microsoft.com/windows/desktop/TSF/text-services-framework) e manuscrito em computadores até o teclado WordFlow (que fornece correção automática, previsão e ditado) em dispositivos móveis.

## <a name="architecture"></a>Arquitetura


A seguir, uma representação simples do sistema de entrada de texto.

-   "Application" representa um aplicativo do Windows que hospeda um controle de edição personalizado criado usando as APIs de texto principal.
-   As APIs [**Windows.UI.Text.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Text.Core) facilitam a comunicação com serviços de texto por meio do Windows. A comunicação entre o controle de edição de texto e os serviços de texto é tratada principalmente por meio de um objeto [**CoreTextEditContext**](https://docs.microsoft.com/uwp/api/Windows.UI.Text.Core.CoreTextEditContext) que fornece os métodos e os eventos para facilitar a comunicação.

![diagrama da arquitetura de texto básica](images/coretext/architecture.png)

## <a name="text-ranges-and-selection"></a>Intervalos e seleção de texto


Os controles de edição dão espaço para entrada de texto, e os usuários esperam editar texto em qualquer lugar nesse espaço. Aqui, explicamos o sistema de posicionamento de texto usado pelas APIs de texto básicas e como os intervalos e as seleções são representados nesse sistema.

### <a name="application-caret-position"></a>Posição do sinal de interpolação do aplicativo

Os intervalos de texto usados com as APIs de texto básicas são expressados em termos de posições de sinal de interpolação. Uma "Application Caret Position (ACP)" é um número baseado em zero que indica a contagem de caracteres desde o início do fluxo de texto pouco antes do sinal de interpolação, conforme mostrado aqui.

![diagrama do fluxo de texto de exemplo](images/coretext/stream-1.png)
### <a name="text-ranges-and-selection"></a>Intervalos e seleção de texto

Os intervalos de texto e as seleções são representados pela estrutura [**CoreTextRange**](https://docs.microsoft.com/uwp/api/Windows.UI.Text.Core.CoreTextRange) que contém dois campos:

| Campo                  | Tipo de dados                                                                 | Descrição                                                                      |
|------------------------|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| **StartCaretPosition** | **Número** \[de JavaScript\] | .NET **System. Int32** \[\] | C++ **Int32** \[\] | A posição inicial de um intervalo é a ACP pouco antes do primeiro caractere. |
| **EndCaretPosition**   | **Número** \[de JavaScript\] | .NET **System. Int32** \[\] | C++ **Int32** \[\] | A posição final de um intervalo é a ACP logo depois do último caractere.     |

 

Por exemplo, no intervalo de texto mostrado anteriormente, o intervalo \[0, 5\] especifica a palavra "Olá". **StartCaretPosition** sempre deve ser menor ou igual a **EndCaretPosition**. O intervalo \[de 5,\] 0 é inválido.

### <a name="insertion-point"></a>Ponto de inserção

A posição do sinal de interpolação atual, normalmente conhecida como o ponto de inserção, é representada definindo-se **StartCaretPosition** para ser igual a **EndCaretPosition**.

### <a name="noncontiguous-selection"></a>Seleção não contígua

Alguns controles de edição dão suporte a seleções não contíguas. Por exemplo, os aplicativos do Microsoft Office dão suporte a seleções arbitrárias, e muitos editores de código-fonte dão suporte à seleção de coluna. No entanto, as APIs de texto básicas não dão suporte a seleções não contíguas. Os controles de edição devem informar somente uma seleção contígua única, normalmente o subintervalo ativo das seleções não contíguas.

Por exemplo, considere este fluxo de texto:

![diagrama](images/coretext/stream-2.png) de fluxo de texto de exemplo há duas seleções \[: 0,\] 1 \[e 6,\]11. O controle de edição deve relatar apenas um deles; \[0, 1\] ou \[6, 11\].

## <a name="working-with-text"></a>Trabalhando com texto


A classe [**CoreTextEditContext**](https://docs.microsoft.com/uwp/api/Windows.UI.Text.Core.CoreTextEditContext) permite o fluxo de texto entre o Windows e os controles de edição por meio do evento [**TextUpdating**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating), do evento [**TextRequested**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) e do método [**NotifyTextChanged**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged).

O controle de edição recebe texto por meio de eventos [**TextUpdating**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) gerados quando os usuários interagem com métodos de entrada de texto como teclados, fala ou IMEs.

Ao alterar o texto no seu controle de edição, por exemplo, colando o texto no controle, você precisa notificar o Windows chamando [**NotifyTextChanged**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged).

Se o serviço de texto exigir o novo texto, um evento [**TextRequested**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) será acionado. Você deve fornecer o novo texto no manipulador de eventos **TextRequested**.

### <a name="accepting-text-updates"></a>Aceitando atualizações de texto

O controle de edição normalmente deve aceitar solicitações de atualização de texto, porque elas representam o texto que o usuário deseja inserir. No manipulador de eventos [**TextUpdating**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating), essas ações são esperadas de seu controle de edição:

1.  Insira o texto especificado em [**CoreTextTextUpdatingEventArgs.Text**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.text) na posição especificada em [**CoreTextTextUpdatingEventArgs.Range**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.range).
2.  Coloque a seleção na posição especificada em [**CoreTextTextUpdatingEventArgs.NewSelection**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.newselection).
3.  Notifique o sistema de que a atualização foi bem-sucedida definindo [**CoreTextTextUpdatingEventArgs.Result**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.result) como [**CoreTextTextUpdatingResult.Succeeded**](https://docs.microsoft.com/uwp/api/Windows.UI.Text.Core.CoreTextTextUpdatingResult).

Por exemplo, esse é o estado de um controle de edição antes de o usuário digitar "d". O ponto de inserção é \[de 10,\]10.

![exemplo de diagrama](images/coretext/stream-3.png) de fluxo de texto quando o usuário digita "d", um evento [**textupdating**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) é gerado com os seguintes dados de [**CoreTextTextUpdatingEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Text.Core.CoreTextTextUpdatingEventArgs) :

-   [**Range**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.range) = Intervalo\[de 10, 10\]
-   [**Text**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.text) = "d"
-   [**NewSelection**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.newselection) = NewSelection\[11, 11\]

Em seu controle de edição, aplique as alterações especificadas e defina [**Result**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.result) como **Succeeded**. Aqui está o estado do controle após as alterações serem aplicadas.

![diagrama do fluxo de texto de exemplo](images/coretext/stream-4.png)
### <a name="rejecting-text-updates"></a>Rejeitando atualizações de texto

Às vezes, você não consegue aplicar atualizações de texto porque o intervalo solicitado está em uma área do controle de edição que não deve ser alterada. Nesse caso, você não deve aplicar alterações. Em vez disso, notifique o sistema de que a atualização falhou definindo [**CoreTextTextUpdatingEventArgs.Result**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.result) como [**CoreTextTextUpdatingResult.Failed**](https://docs.microsoft.com/uwp/api/Windows.UI.Text.Core.CoreTextTextUpdatingResult).

Por exemplo, considere um controle de edição que aceita apenas um endereço de email. Os espaços devem ser rejeitados porque endereços de email não podem conter espaços. Assim, quando eventos [**TextUpdating**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) forem acionados para a tecla de espaço, você deverá simplesmente definir [**Result**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.result) como **Failed** no seu controle de edição.

### <a name="notifying-text-changes"></a>Notificando alterações de texto

Às vezes, o controle de edição faz alterações no texto, como quando o texto é colado ou corrigido automaticamente. Nesses casos, você deve notificar os serviços de texto dessas alterações chamando o método [**NotifyTextChanged**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged).

Por exemplo, esse é o estado de um controle de edição antes de o usuário colar "World". O ponto de inserção está \[em 6,\]6.

![diagrama](images/coretext/stream-5.png) de fluxo de texto de exemplo o usuário executa a ação colar e o controle de edição termina com o seguinte texto:

![exemplo de diagrama](images/coretext/stream-4.png) de fluxo de texto quando isso acontece, você deve chamar [**NotifyTextChanged**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged) com estes argumentos:

-   *modifiedRange* = modifiedRange\[6, 6\]
-   *newLength* = 5
-   *newSelection* = newSelection\[11, 11\]

Um ou mais [**TextRequested**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) eventos virão depois, tratados para atualizar o texto com que os serviços de texto estão trabalhando.

### <a name="overriding-text-updates"></a>Substituindo atualizações de texto

No controle de edição, convém substituir uma atualização de texto para fornecer recursos de correção automática.

Por exemplo, considere um controle de edição que forneça um recurso de correção que formaliza contrações. Esse é o estado do controle de edição antes de o usuário digitar a tecla de espaço para acionar a correção. O ponto de inserção está \[em 3,\]3.

![diagrama](images/coretext/stream-6.png) de fluxo de texto de exemplo o usuário pressiona a tecla de espaço e um evento [**textupdating**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) correspondente é gerado. O controle de edição aceita a atualização de texto. Esse é o estado do controle de edição para um breve momento antes de a correção ser concluída. O ponto de inserção é \[de 4,\]4.

![exemplo de diagrama](images/coretext/stream-7.png) de fluxo de texto fora do manipulador de eventos [**textupdating**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) , o controle de edição faz a correção a seguir. Este é o estado do controle de edição após a correção estar concluída. O ponto de inserção está \[em 5,\]5.

![exemplo de diagrama](images/coretext/stream-8.png) de fluxo de texto quando isso acontece, você deve chamar [**NotifyTextChanged**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged) com estes argumentos:

-   *modifiedRange* = modifiedRange\[1, 2\]
-   *newLength* = 2
-   *newSelection* = newSelection\[5, 5\]

Um ou mais [**TextRequested**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) eventos virão depois, tratados para atualizar o texto com que os serviços de texto estão trabalhando.

### <a name="providing-requested-text"></a>Fornecendo texto solicitado

É importante que os serviços de texto tenham o texto correto para fornecer recursos como a correção automática ou a previsão, especialmente para o texto que já existia no controle de edição, carregando um documento, por exemplo, ou texto inserido pelo controle de edição, conforme explicado nas seções anteriores. Portanto, sempre que um evento [**TextRequested**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) é acionado, você deve fornecer o texto atualmente no seu controle de edição para o intervalo especificado.

Haverá vezes em que o [**Range**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexttextrequest.range) em [**CoreTextTextRequest**](https://docs.microsoft.com/uwp/api/Windows.UI.Text.Core.CoreTextTextRequest) especificará um intervalo que o controle de edição não poderá acomodar como está. Por exemplo, o **Range** é maior do que o tamanho do controle de edição no momento do evento [**TextRequested**](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) ou o final do **Range** está fora dos limites. Nesses casos, você deve retornar o intervalo que fizer sentido, que normalmente é um subconjunto do intervalo solicitado.

## <a name="related-articles"></a>Artigos relacionados

### <a name="samples"></a>Exemplos

- [Exemplo de controle de edição personalizado](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl)

### <a name="archive-samples"></a>Exemplos de arquivo-morto

- [Amostra de edição de texto XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BVB%5D-Windows%208%20app%20samples/VB/Windows%208%20app%20samples/XAML%20text%20editing%20sample%20(Windows%208))
