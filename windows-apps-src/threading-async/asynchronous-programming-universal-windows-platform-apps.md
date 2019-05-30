---
ms.assetid: 23FE28F1-89C5-4A17-A732-A722648F9C5E
title: Programação assíncrona
description: Este tópico descreve a programação assíncrona no Universal Windows Platform (UWP) e sua representação de C#, Microsoft Visual Basic .NET, C++ e JavaScript.
ms.date: 05/14/2018
ms.topic: article
keywords: windows 10, uwp, assíncrona
ms.localizationpriority: medium
ms.openlocfilehash: 26378473803b8963c0ca85eb414bae798f9607e4
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371572"
---
# <a name="asynchronous-programming"></a>Programação assíncrona
Este tópico descreve a programação assíncrona no Universal Windows Platform (UWP) e sua representação de C#, Microsoft Visual Basic .NET, C++ e JavaScript.

Usar a programação assíncrona permite que seu aplicativo permaneça responsivo quando ele realiza trabalhos que podem levar uma grande quantidade de tempo. Por exemplo, um aplicativo que baixa conteúdo da Internet pode passar diversos segundos aguardando a chegada do conteúdo. Se você usar um método síncrono no thread de IU para recuperar o conteúdo, o aplicativo ficará bloqueado até que o método retorne. O aplicativo não responderá à interação do usuário e, como ele parecerá não responsivo, o usuário poderá ficar frustrado. Uma maneira muito melhor é usar a programação assíncrona, na qual o aplicativo continua a ser executado e a responder à IU enquanto aguarda a conclusão de uma operação.

Nos métodos que podem levar mais tempo para serem concluídos, a programação assíncrona é a norma e não a exceção na UWP. JavaScript, C#, Visual Basic e C++ cada dão suporte a idioma para métodos assíncronos.

## <a name="asynchronous-programming-in-the-uwp"></a>Programação assíncrona na UWP
Muitos recursos UWP, como o [ **MediaCapture** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) APIs e [ **StorageFile** ](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) APIs, são expostos como APIs assíncronas. Por convenção, os nomes das APIs assíncronas terminam com "Async" para indicar que a parte de sua execução é provável que ocorra depois que o controle tem retornado ao chamador.

Quando você usa APIs assíncronas no seu aplicativo Plataforma Universal do Windows (UWP), o código faz chamadas não bloqueadoras de maneira consistente. Quando você implementa esses padrões assíncronos em suas definições de API, os chamadores reconhecem e usam seu código de forma previsível.

Aqui encontram-se algumas tarefas comuns que requerem a chamada de APIs assíncronas da UWP.

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
| C#                   | Palavra-chave **async**, operador **await** |
| Visual Basic         | Palavra-chave **Async**, operador **Await** |
| C++/WinRT            | corrotina, e **co_await** operador  |
| C++/CX               | classe **task**, método **.then**      |
| JavaScript           | objeto promise, função **then**     |

## <a name="asynchronous-patterns-in-uwp-using-c-and-visual-basic"></a>Padrões assíncronos na UWP usando C# e Visual Basic
Um segmento típico de código gravado em C# ou em Visual Basic é executado de forma síncrona. Isso significa que, quando você executa uma linha, ela termina antes da execução da próxima linha. Havia modelos de programação Microsoft .NET anteriores para execução assíncrona, porém, o código resultante tende a enfatizar a mecânica de execução do código assíncrono em vez de focar na tarefa que o código está tentando realizar. Os compiladores da UWP, do .NET framework, e do C# Visual Basic adicionaram recursos que reduzem a mecânica assíncrona fora do seu código. Para .NET e a UWP, você pode gravar o código assíncrono que focaliza o que o seu código faz em vez de como e quando fazê-lo. Seu código assíncrono parecerá razoavelmente similar ao código síncrono. Para saber mais, veja [Chamar APIs assíncronas em C# ou Visual Basic](call-asynchronous-apis-in-csharp-or-visual-basic.md).

## <a name="asynchronous-patterns-in-uwp-with-cwinrt"></a>Padrões assíncronos em UWP com o C++/WinRT
Com o C++/WinRT, que você use as co-rotinas e o **co_await** operador. Para obter mais informações e exemplos de código, consulte [programação assíncrona em C++/WinRT](../cpp-and-winrt-apis/concurrency.md).

## <a name="asynchronous-patterns-in-uwp-with-ccx"></a>Padrões assíncronos em UWP com o C++/CX
Em C++/CX, a programação assíncrona se baseia na [**classe task**](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class) e em seu [**método then**](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class?view=vs-2017). A sintaxe é semelhante àquela das promessas JavaScript. A **classe task** e seus tipos relacionados também fornecem a funcionalidade para cancelamento e gerenciamento do contexto do thread. Para obter mais informações, consulte [programação assíncrona em C++/CX](asynchronous-programming-in-cpp-universal-windows-platform-apps.md).

O [ **crie\_função async** ](https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace-functions?view=vs-2017) fornece suporte para a produção de APIs assíncronas que podem ser consumidas a partir de JavaScript ou qualquer outra linguagem compatível com a UWP. Para obter mais informações, consulte [Criando operações assíncronas em C++ c++ /CX](https://docs.microsoft.com/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps).

## <a name="asynchronous-patterns-in-uwp-using-javascript"></a>Padrões assíncronos em UWP usando JavaScript
No JavaScript, a programação assíncrona segue o padrão proposto [Common JS Promises/A](https://wiki.commonjs.org/wiki/Promises/A) ao ter métodos assíncronos retornando objetos promise. As promessas são usadas na UWP e na Biblioteca do Windows para JavaScript.

Um objeto promise representa um valor que será preenchido no futuro. Na UWP, você obtém um objeto promise a partir de uma função factory, cujo nome por convenção termina com "Async".

Em muitos casos, chamar uma função assíncrona é quase tão simples quando chamar uma função convencional. A diferença é que você usa o método [**then**](https://docs.microsoft.com/previous-versions/windows/apps/br229728(v=win.10)) ou [**done**](https://docs.microsoft.com/previous-versions/windows/apps/hh701079(v=win.10)) para atribuir os manipuladores para resultados ou erros e para iniciar a operação.

## <a name="related-topics"></a>Tópicos relacionados
* [Chamar APIs assíncronas no Visual Basic ou C#](call-asynchronous-apis-in-csharp-or-visual-basic.md)
* [Programação assíncrona com Async e Await (c# e Visual Basic)](https://msdn.microsoft.com/library/hh191443(vs.110).aspx)
* [Cenários de exemplo de recursos de Reversi: código assíncrono](https://docs.microsoft.com/previous-versions/windows/apps/jj712233(v=win.10))
