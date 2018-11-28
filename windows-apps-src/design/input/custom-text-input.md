---
Description: The core text APIs in the Windows.UI.Text.Core namespace enable a Universal Windows Platform (UWP) app to receive text input from any text service supported on Windows devices.
title: Visão geral da entrada de texto personalizada
ms.assetid: 58F5F7AC-6A4B-45FC-8C2A-942730FD7B74
label: Custom text input
template: detail.hbs
keywords: teclado, texto, texto básico, texto personalizado, Estrutura de Serviços de Texto, entrada, interações do usuário
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 161278dc5fe0bb8c7d4c790def6a9f7ba88b83d2
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7841811"
---
# <a name="custom-text-input"></a>Entrada de texto personalizado



As APIs de texto básicas no namespace [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238) permitem que um aplicativo UWP (Plataforma Universal do Windows) receba a entrada de texto de qualquer serviço de texto compatível em dispositivos Windows. As APIs são semelhantes às APIs [Estrutura de Serviços de Texto](https://msdn.microsoft.com/library/windows/desktop/ms629032) em que o aplicativo não precisa ter conhecimento detalhado dos serviços de texto. Isso permite que o aplicativo receba texto em qualquer idioma e de qualquer tipo de entrada, como teclado, fala ou caneta.

> **APIs importantes**: [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238), [**CoreTextEditContext**](https://msdn.microsoft.com/library/windows/apps/dn958158)

## <a name="why-use-core-text-apis"></a>Por que usar APIs de texto básicas?


Para muitos aplicativos, os controles de caixa de texto XAML ou HTML são suficientes para entrada de texto e edição. No entanto, caso seu aplicativo trate cenários de texto complexos, como um aplicativo de processamento de texto, talvez você precise da flexibilidade de um controle de edição de texto personalizado. Você pode usar as APIs de teclado [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) para criar o controle de edição de texto, mas elas não oferecem uma maneira de receber entrada de texto com base na composição, algo necessário para dar suporte a idiomas do leste asiático.

Em vez disso, use as APIs [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238) quando você precisar criar um controle de edição de texto personalizado. Essas APIs foram projetadas para dar muita flexibilidade no processamento de entrada de texto, em qualquer idioma e permitir que você ofereça a experiência de texto mais adequada ao seu aplicativo. Os controles de edição e entrada de texto criados com APIs de texto básicas podem receber entrada de texto de todos os métodos de entrada de texto em dispositivos Windows, dos Editores de Método de Entrada (IMEs) baseados na [Estrutura de Serviços de Texto](https://msdn.microsoft.com/library/windows/desktop/ms629032) e manuscrito em computadores até o teclado WordFlow (que fornece correção automática, previsão e ditado) em dispositivos móveis.

## <a name="architecture"></a>Arquitetura


A seguir, uma representação simples do sistema de entrada de texto.

-   "Application" representa um aplicativo UWP que hospeda um controle de edição personalizado criado usando-se as APIs de texto básicas.
-   As APIs [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238) facilitam a comunicação com serviços de texto por meio do Windows. A comunicação entre o controle de edição de texto e os serviços de texto é tratada principalmente por meio de um objeto [**CoreTextEditContext**](https://msdn.microsoft.com/library/windows/apps/dn958158) que fornece os métodos e os eventos para facilitar a comunicação.

![diagrama da arquitetura de texto básica](images/coretext/architecture.png)

## <a name="text-ranges-and-selection"></a>Intervalos e seleção de texto


Os controles de edição dão espaço para entrada de texto, e os usuários esperam editar texto em qualquer lugar nesse espaço. Aqui, explicamos o sistema de posicionamento de texto usado pelas APIs de texto básicas e como os intervalos e as seleções são representados nesse sistema.

### <a name="application-caret-position"></a>Posição do sinal de interpolação do aplicativo

Os intervalos de texto usados com as APIs de texto básicas são expressados em termos de posições de sinal de interpolação. Uma "Application Caret Position (ACP)" é um número baseado em zero que indica a contagem de caracteres desde o início do fluxo de texto pouco antes do sinal de interpolação, conforme mostrado aqui.

![diagrama do fluxo de texto de exemplo](images/coretext/stream-1.png)
### <a name="text-ranges-and-selection"></a>Intervalos e seleção de texto

Os intervalos de texto e as seleções são representados pela estrutura [**CoreTextRange**](https://msdn.microsoft.com/library/windows/apps/dn958201) que contém dois campos:

| Campo                  | Tipo de dados                                                                 | Descrição                                                                      |
|------------------------|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| **StartCaretPosition** | **Number** \[JavaScript\] | **System.Int32** \[.NET\] | **int32** \[C++\] | A posição inicial de um intervalo é a ACP pouco antes do primeiro caractere. |
| **EndCaretPosition**   | **Number** \[JavaScript\] | **System.Int32** \[.NET\] | **int32** \[C++\] | A posição final de um intervalo é a ACP logo depois do último caractere.     |

 

Por exemplo, no intervalo de texto mostrado anteriormente, o intervalo \[0, 5\] especifica a palavra "Hello". **StartCaretPosition** sempre deve ser menor ou igual a **EndCaretPosition**. O intervalo \[5, 0\] é inválido.

### <a name="insertion-point"></a>Ponto de inserção

A posição do sinal de interpolação atual, normalmente conhecida como o ponto de inserção, é representada definindo-se **StartCaretPosition** para ser igual a **EndCaretPosition**.

### <a name="noncontiguous-selection"></a>Seleção não contígua

Alguns controles de edição dão suporte a seleções não contíguas. Por exemplo, os aplicativos do Microsoft Office dão suporte a seleções arbitrárias, e muitos editores de código-fonte dão suporte à seleção de coluna. No entanto, as APIs de texto básicas não dão suporte a seleções não contíguas. Os controles de edição devem informar somente uma seleção contígua única, normalmente o subintervalo ativo das seleções não contíguas.

Por exemplo, considere este fluxo de texto:

![exemplo de diagrama de fluxo de texto](images/coretext/stream-2.png) Há duas seleções: \[0, 1 \] e \[6, 11\]. O controle de edição deve informar somente uma delas; \[0, 1\] ou \[6, 11\].

## <a name="working-with-text"></a>Trabalhando com texto


A classe [**CoreTextEditContext**](https://msdn.microsoft.com/library/windows/apps/dn958158) permite o fluxo de texto entre o Windows e os controles de edição por meio do evento [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176), do evento [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) e do método [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172).

O controle de edição recebe texto por meio de eventos [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) gerados quando os usuários interagem com métodos de entrada de texto como teclados, fala ou IMEs.

Ao alterar o texto no seu controle de edição, por exemplo, colando o texto no controle, você precisa notificar o Windows chamando [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172).

Se o serviço de texto exigir o novo texto, um evento [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) será acionado. Você deve fornecer o novo texto no manipulador de eventos **TextRequested**.

### <a name="accepting-text-updates"></a>Aceitando atualizações de texto

O controle de edição normalmente deve aceitar solicitações de atualização de texto, porque elas representam o texto que o usuário deseja inserir. No manipulador de eventos [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176), essas ações são esperadas de seu controle de edição:

1.  Insira o texto especificado em [**CoreTextTextUpdatingEventArgs.Text**](https://msdn.microsoft.com/library/windows/apps/dn958236) na posição especificada em [**CoreTextTextUpdatingEventArgs.Range**](https://msdn.microsoft.com/library/windows/apps/dn958234).
2.  Coloque a seleção na posição especificada em [**CoreTextTextUpdatingEventArgs.NewSelection**](https://msdn.microsoft.com/library/windows/apps/dn958233).
3.  Notifique o sistema de que a atualização foi bem-sucedida definindo [**CoreTextTextUpdatingEventArgs.Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) como [**CoreTextTextUpdatingResult.Succeeded**](https://msdn.microsoft.com/library/windows/apps/dn958237).

Por exemplo, esse é o estado de um controle de edição antes de o usuário digitar "d". O ponto de inserção está em \[10, 10\].

![exemplo de diagrama de fluxo de texto](images/coretext/stream-3.png) Quando o usuário digita "d", um evento [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) é acionado com os seguintes dados [**CoreTextTextUpdatingEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn958229):

-   [**Range**](https://msdn.microsoft.com/library/windows/apps/dn958234) = \[10, 10\]
-   [**Text**](https://msdn.microsoft.com/library/windows/apps/dn958236) = "d"
-   [**NewSelection**](https://msdn.microsoft.com/library/windows/apps/dn958233) = \[11, 11\]

Em seu controle de edição, aplique as alterações especificadas e defina [**Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) como **Succeeded**. Aqui está o estado do controle após as alterações serem aplicadas.

![diagrama do fluxo de texto de exemplo](images/coretext/stream-4.png)
### <a name="rejecting-text-updates"></a>Rejeitando atualizações de texto

Às vezes, você não consegue aplicar atualizações de texto porque o intervalo solicitado está em uma área do controle de edição que não deve ser alterada. Nesse caso, você não deve aplicar alterações. Em vez disso, notifique o sistema de que a atualização falhou definindo [**CoreTextTextUpdatingEventArgs.Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) como [**CoreTextTextUpdatingResult.Failed**](https://msdn.microsoft.com/library/windows/apps/dn958237).

Por exemplo, considere um controle de edição que aceita apenas um endereço de email. Os espaços devem ser rejeitados porque endereços de email não podem conter espaços. Assim, quando eventos [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) forem acionados para a tecla de espaço, você deverá simplesmente definir [**Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) como **Failed** no seu controle de edição.

### <a name="notifying-text-changes"></a>Notificando alterações de texto

Às vezes, o controle de edição faz alterações no texto, como quando o texto é colado ou corrigido automaticamente. Nesses casos, você deve notificar os serviços de texto dessas alterações chamando o método [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172).

Por exemplo, esse é o estado de um controle de edição antes de o usuário colar "World". O ponto de inserção está em \[6, 6\].

![exemplo de diagrama de fluxo de texto](images/coretext/stream-5.png) O usuário executa a ação de colar e o controle de edição acaba com o seguinte texto:

![exemplo de diagrama de fluxo de texto](images/coretext/stream-4.png) Quando isso acontecer, você deverá chamar [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172) com estes argumentos:

-   *modifiedRange* = \[6, 6\]
-   *newLength* = 5
-   *newSelection* = \[11, 11\]

Um ou mais [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) eventos virão depois, tratados para atualizar o texto com que os serviços de texto estão trabalhando.

### <a name="overriding-text-updates"></a>Substituindo atualizações de texto

No controle de edição, convém substituir uma atualização de texto para fornecer recursos de correção automática.

Por exemplo, considere um controle de edição que forneça um recurso de correção que formaliza contrações. Esse é o estado do controle de edição antes de o usuário digitar a tecla de espaço para acionar a correção. O ponto de inserção está em \[3, 3\].

![exemplo de diagrama de fluxo de texto](images/coretext/stream-6.png) O usuário pressiona a tecla de espaço, e um evento [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) correspondente é acionado. O controle de edição aceita a atualização de texto. Esse é o estado do controle de edição para um breve momento antes de a correção ser concluída. O ponto de inserção está em \[4, 4\].

![exemplo de diagrama de fluxo de texto](images/coretext/stream-7.png) Fora do manipulador de eventos [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176), o controle de edição faz a seguinte correção. Este é o estado do controle de edição após a correção estar concluída. O ponto de inserção está em \[5, 5\].

![exemplo de diagrama de fluxo de texto](images/coretext/stream-8.png) Quando isso acontecer, você deverá chamar [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172) com estes argumentos:

-   *modifiedRange* = \[1, 2\]
-   *newLength* = 2
-   *newSelection* = \[5, 5\]

Um ou mais [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) eventos virão depois, tratados para atualizar o texto com que os serviços de texto estão trabalhando.

### <a name="providing-requested-text"></a>Fornecendo texto solicitado

É importante que os serviços de texto tenham o texto correto para fornecer recursos como a correção automática ou a previsão, especialmente para o texto que já existia no controle de edição, carregando um documento, por exemplo, ou texto inserido pelo controle de edição, conforme explicado nas seções anteriores. Portanto, sempre que um evento [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) é acionado, você deve fornecer o texto atualmente no seu controle de edição para o intervalo especificado.

Haverá vezes em que o [**Range**](https://msdn.microsoft.com/library/windows/apps/dn958227) em [**CoreTextTextRequest**](https://msdn.microsoft.com/library/windows/apps/dn958221) especificará um intervalo que o controle de edição não poderá acomodar como está. Por exemplo, o **Range** é maior do que o tamanho do controle de edição no momento do evento [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) ou o final do **Range** está fora dos limites. Nesses casos, você deve retornar o intervalo que fizer sentido, que normalmente é um subconjunto do intervalo solicitado.

## <a name="related-articles"></a>Artigos relacionados

**Exemplos**
* [Exemplo de controle de edição personalizado](https://go.microsoft.com/fwlink/?linkid=831024) 
 **Exemplos de arquivo morto**
* [Amostra de edição de texto XAML](http://go.microsoft.com/fwlink/p/?LinkID=251417)


