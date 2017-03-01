---
author: TylerMSFT
ms.assetid: 23FE28F1-89C5-4A17-A732-A722648F9C5E
title: "Programação assíncrona"
description: "Este tópico descreve a programação assíncrona na Plataforma Universal do Windows (UWP) e sua representação em C#, Microsoft Visual Basic .NET, extensões de componente do Visual C++ (C++/CX) e JavaScript."
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, assíncrona"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 1f157db0dc9124ce839f608e5f1a2325c1ce922f
ms.lasthandoff: 02/07/2017

---
# <a name="asynchronous-programming"></a>Programação assíncrona

\[ Atualizado para apps UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Este tópico descreve a programação assíncrona na Plataforma Universal do Windows (UWP) e sua representação em C#, Microsoft Visual Basic .NET, extensões de componente do Visual C++ (C++/CX) e JavaScript.

Usar a programação assíncrona permite que seu app permaneça responsivo quando ele realiza trabalhos que podem levar uma grande quantidade de tempo. Por exemplo, um app que baixa conteúdo da Internet pode passar diversos segundos aguardando a chegada do conteúdo. Se você usar um método síncrono no thread de IU para recuperar o conteúdo, o app ficará bloqueado até que o método retorne. O app não responderá à interação do usuário e, como ele parecerá não responsivo, o usuário poderá ficar frustrado. Uma maneira muito melhor é usar a programação assíncrona, na qual o app continua a ser executado e a responder à IU enquanto aguarda a conclusão de uma operação.

Nos métodos que podem levar mais tempo para serem concluídos, a programação assíncrona é a norma e não a exceção na UWP. JavaScript, C#, Visual Basic e C++/CX oferecem suporte de linguagem para os métodos assíncronos.

## <a name="asynchronous-programming-in-the-uwp"></a>Programação assíncrona na UWP

Muitos recursos da UWP, como as APIs [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/BR241124) e [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171), são expostos como APIs assíncronas. Por convenção, os nomes de APIs assíncronas terminam com "Async" para indicar que parte de sua execução pode ocorrer após a API ser invocada.

Quando você usa APIs assíncronas no seu app Plataforma Universal do Windows (UWP), o código faz chamadas não bloqueadoras de maneira consistente. Quando você implementa esses padrões assíncronos em suas definições de API, os chamadores reconhecem e usam seu código de forma previsível.

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
| C#                  | Palavra-chave **async**, operador **await** |
| Visual Basic         | Palavra-chave **Async**, operador **Await** |
| C++/CX               | classe **task**, método **.then**      |
| JavaScript           | objeto promise, função **then**     |

 

## <a name="asynchronous-patterns-in-uwp-using-c-and-visual-basic"></a>Padrões assíncronos na UWP usando C# e Visual Basic


Um segmento típico de código gravado em C# ou em Visual Basic é executado de forma síncrona. Isso significa que, quando você executa uma linha, ela termina antes da execução da próxima linha. Havia modelos de programação Microsoft .NET anteriores para execução assíncrona, porém, o código resultante tende a enfatizar a mecânica de execução do código assíncrono em vez de focar na tarefa que o código está tentando realizar. Os compiladores da UWP, do .NET framework, e do C# Visual Basic adicionaram recursos que reduzem a mecânica assíncrona fora do seu código. Para .NET e a UWP, você pode gravar o código assíncrono que focaliza o que o seu código faz em vez de como e quando fazê-lo. Seu código assíncrono parecerá razoavelmente similar ao código síncrono. Para saber mais, veja [Chamar APIs assíncronas em C# ou Visual Basic](call-asynchronous-apis-in-csharp-or-visual-basic.md).

## <a name="asynchronous-patterns-in-uwp-with-c"></a>Padrões assíncronos na UWP com C++


Em C++/CX, a programação assíncrona se baseia na [**classe task**](https://msdn.microsoft.com/library/windows/apps/xaml/hh750113.aspx) e em seu [**método then**](https://msdn.microsoft.com/library/windows/apps/xaml/hh750044.aspx). A sintaxe é semelhante àquela das promessas JavaScript. A **classe task** e seus tipos relacionados também fornecem a funcionalidade para cancelamento e gerenciamento do contexto do thread. Para obter mais informações, consulte [Programação assíncrona em C++](asynchronous-programming-in-cpp-universal-windows-platform-apps.md).

A [**função create\_async**](https://msdn.microsoft.com/library/windows/apps/xaml/hh750102.aspx) dá suporte à produção de APIs assíncronas que podem ser consumidas a partir de JavaScript ou de qualquer outra linguagem que dê suporte à UWP. Para obter mais informações, veja [Criando operações assíncronas em C++](https://msdn.microsoft.com/library/windows/apps/xaml/hh750082.aspx).

## <a name="asynchronous-patterns-in-uwp-using-javascript"></a>Padrões assíncronos em UWP usando JavaScript

No JavaScript, a programação assíncrona segue o padrão proposto [Common JS Promises/A](http://wiki.commonjs.org/wiki/Promises/A) ao ter métodos assíncronos retornando objetos promise. As promessas são usadas na UWP e na Biblioteca do Windows para JavaScript.

Um objeto promise representa um valor que será preenchido no futuro. Na UWP, você obtém um objeto promise a partir de uma função factory, cujo nome por convenção termina com "Async".

Em muitos casos, chamar uma função assíncrona é quase tão simples quando chamar uma função convencional. A diferença é que você usa o método [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728) ou [**done**](https://msdn.microsoft.com/library/windows/apps/Hh701079) para atribuir os manipuladores para resultados ou erros e para iniciar a operação.

## <a name="related-topics"></a>Tópicos relacionados

* [Chamar APIs assíncronas no Visual Basic ou C#](call-asynchronous-apis-in-csharp-or-visual-basic.md)
* [Programação assíncrona com Async e Await (C# e Visual Basic)](http://msdn.microsoft.com/library/hh191443(vs.110).aspx)
* [Cenários de recursos de exemplo de Reversi: código assíncrono](https://msdn.microsoft.com/library/windows/apps/xaml/jj712233.aspx#async)

