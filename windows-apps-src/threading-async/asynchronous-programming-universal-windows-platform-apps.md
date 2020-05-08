---
ms.assetid: 23FE28F1-89C5-4A17-A732-A722648F9C5E
title: Programação assíncrona
description: Este tópico descreve a programação assíncrona no Plataforma Universal do Windows (UWP) e sua representação em C#, Microsoft Visual Basic .NET, C++ e JavaScript.
ms.date: 05/14/2018
ms.topic: article
keywords: windows 10, uwp, assíncrona
ms.localizationpriority: medium
ms.openlocfilehash: 77c3080728915ae9a288a57fe0200c43e7d119f3
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/02/2020
ms.locfileid: "82729992"
---
# <a name="asynchronous-programming"></a>Programação assíncrona
Este tópico descreve a programação assíncrona no Plataforma Universal do Windows (UWP) e sua representação em C#, Microsoft Visual Basic .NET, C++ e JavaScript.

Usar a programação assíncrona permite que seu aplicativo permaneça responsivo quando ele realiza trabalhos que podem levar uma grande quantidade de tempo. Por exemplo, um aplicativo que baixa conteúdo da Internet pode passar diversos segundos aguardando a chegada do conteúdo. Se você usar um método síncrono no thread de IU para recuperar o conteúdo, o aplicativo ficará bloqueado até que o método retorne. O aplicativo não responderá à interação do usuário e, como ele parecerá não responsivo, o usuário poderá ficar frustrado. Uma maneira muito melhor é usar a programação assíncrona, na qual o aplicativo continua a ser executado e a responder à IU enquanto aguarda a conclusão de uma operação.

Nos métodos que podem levar mais tempo para serem concluídos, a programação assíncrona é a norma e não a exceção na UWP. O JavaScript, o C#, o Visual Basic e o C++ fornecem suporte a idiomas para métodos assíncronos.

## <a name="asynchronous-programming-in-the-uwp"></a>Programação assíncrona na UWP
Muitos recursos de UWP, como as APIs [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) e APIs de [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) , são expostos como APIs assíncronas. Por convenção, os nomes das APIs assíncronas terminam com "Async" para indicar que parte de sua execução provavelmente ocorrerá após o controle ser retornado ao chamador.

Quando você usa APIs assíncronas no seu aplicativo Plataforma Universal do Windows (UWP), o código faz chamadas não bloqueadoras de maneira consistente. Quando você implementa esses padrões assíncronos em suas definições de API, os chamadores reconhecem e usam seu código de forma previsível.

Aqui estão algumas tarefas comuns que exigem a chamada de APIs de Windows Runtime assíncronas.

-   Exibição de uma caixa de diálogo de mensagem

-   Trabalhar com o arquivo do sistema, exibindo um seletor de arquivos

-   Envio e recebimento de dados de e para a Internet

-   Usando soquetes, fluxos, conectividade

-   Trabalhando com compromissos, contatos, calendário

-   Trabalhar com tipos de arquivos como abrir arquivos em formato PDF (Portable Document Format) ou decodificar formatos de imagem ou mídia

-   Interagir com um dispositivo ou serviço

Com um padrão assíncrono UWP, você pode evitar explicitamente gerenciar todos os threads de uma só vez. Cada linguagem de programação dá suporte ao padrão assíncrono para a UWP em sua maneira específica:

| Linguagem de programação | Representação assíncrona           |
|----------------------|---------------------------------------|
| C#                   | palavra-chave **Async** , operador **Await** |
| Visual Basic         | Palavra-chave **Async**, operador **Await** |
| C++/WinRT            | operador de corrotina e de **co_await**  |
| C++/CX               | classe **task**, método **.then**      |
| JavaScript           | objeto promise, função **then**     |

## <a name="asynchronous-patterns-in-uwp-using-c-and-visual-basic"></a>Padrões assíncronos na UWP usando C# e Visual Basic
Um segmento típico de código gravado em C# ou em Visual Basic é executado de forma síncrona. Isso significa que, quando você executa uma linha, ela termina antes da execução da próxima linha. Havia modelos de programação Microsoft .NET anteriores para execução assíncrona, porém, o código resultante tende a enfatizar a mecânica de execução do código assíncrono em vez de focar na tarefa que o código está tentando realizar. Os compiladores da UWP, do .NET framework, e do C# Visual Basic adicionaram recursos que reduzem a mecânica assíncrona fora do seu código. Para .NET e a UWP, você pode gravar o código assíncrono que focaliza o que o seu código faz em vez de como e quando fazê-lo. Seu código assíncrono parecerá razoavelmente similar ao código síncrono. Para saber mais, veja [Chamar APIs assíncronas em C# ou Visual Basic](call-asynchronous-apis-in-csharp-or-visual-basic.md).

## <a name="asynchronous-patterns-in-uwp-with-cwinrt"></a>Padrões assíncronos no UWP com C++/WinRT
Com o C++/WinRT, você usa corrotinas e o operador de **co_await** . Para obter mais informações e exemplos de código, consulte [programação assíncrona em C++/WinRT](../cpp-and-winrt-apis/concurrency.md).

## <a name="asynchronous-patterns-in-uwp-with-ccx"></a>Padrões assíncronos no UWP com C++/CX
Em C++/CX, a programação assíncrona se baseia na [**classe task**](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class) e em seu [**método then**](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class?view=vs-2017). A sintaxe é semelhante àquela das promessas JavaScript. A **classe task** e seus tipos relacionados também fornecem a funcionalidade para cancelamento e gerenciamento do contexto do thread. Para obter mais informações, consulte [programação assíncrona em C++/CX](asynchronous-programming-in-cpp-universal-windows-platform-apps.md).

A [**função\_Create Async**](https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace-functions?view=vs-2017) fornece suporte para a produção de APIs assíncronas que podem ser consumidas do JavaScript ou de qualquer outra linguagem que ofereça suporte a UWP. Para obter mais informações, consulte [criando operações assíncronas em C++/CX](https://docs.microsoft.com/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps).

## <a name="asynchronous-patterns-in-uwp-using-javascript"></a>Padrões assíncronos em UWP usando JavaScript
No JavaScript, a programação assíncrona segue o padrão proposto [Common JS Promises/A](https://wiki.commonjs.org/wiki/Promises/A) ao ter métodos assíncronos retornando objetos promise. As promessas são usadas na UWP e na Biblioteca do Windows para JavaScript.

Um objeto promise representa um valor que será preenchido no futuro. Na UWP, você obtém um objeto promise a partir de uma função factory, cujo nome por convenção termina com "Async".

Em muitos casos, chamar uma função assíncrona é quase tão simples quando chamar uma função convencional. A diferença é que você usa o método [**then**](https://docs.microsoft.com/previous-versions/windows/apps/br229728(v=win.10)) ou [**done**](https://docs.microsoft.com/previous-versions/windows/apps/hh701079(v=win.10)) para atribuir os manipuladores para resultados ou erros e para iniciar a operação.

## <a name="related-topics"></a>Tópicos relacionados
* [Chamar APIs assíncronas no Visual Basic ou C#](call-asynchronous-apis-in-csharp-or-visual-basic.md)
* [Programação assíncrona com Async e Await (C# e Visual Basic)](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2012/hh191443(v=vs.110))
* [Cenários de recursos de exemplo de Reversi: código assíncrono](https://docs.microsoft.com/previous-versions/windows/apps/jj712233(v=win.10))
