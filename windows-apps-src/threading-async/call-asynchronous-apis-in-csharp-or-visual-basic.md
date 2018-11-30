---
ms.assetid: 066711E0-D5C4-467E-8683-3CC64EDBCC83
title: Chamar APIs assíncronas no Visual Basic ou C#
description: A Plataforma Universal do Windows (UWP) inclui muitas APIs assíncronas para garantir que o seu aplicativo permaneça responsivo ao executar trabalhos demorados.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, C#, Visual Basic, assíncrona
ms.localizationpriority: medium
ms.openlocfilehash: 899af2ffd26419d4c8906d703d6708d202f8c150
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8213841"
---
# <a name="call-asynchronous-apis-in-c-or-visual-basic"></a>Chamar APIs assíncronas no Visual Basic ou C#



A Plataforma Universal do Windows (UWP) inclui muitas APIs assíncronas para garantir que o seu aplicativo permaneça responsivo ao executar trabalhos demorados. Este tópico descreve como usar métodos assíncronos da UWP em C# ou Microsoft Visual Basic.

As APIs assíncronas evitam que o aplicativo aguarde a conclusão de operações grandes antes de continuar a execução. Por exemplo, um aplicativo que baixa informações da Internet pode passar diversos segundos aguardando a chegada das informações. Se você usar um método síncrono para recuperar as informações, o aplicativo ficará bloqueado até que o método seja retornado. O aplicativo não responderá à interação do usuário e, como ele parecerá não responsivo, o usuário poderá ficar frustrado. Ao fornecer APIs assíncronas, a UWP ajuda a garantir que seu aplicativo permaneça responsivo ao usuário quando estiver executando operações longas.

A maioria das APIs assíncronas da UWP não possui equivalentes síncronos, por isso você tem de saber como usá-las com C# ou Visual Basic em seu aplicativo da Plataforma Universal do Windows (UWP). Nós vamos mostrar como chamar as APIs assíncronas da UWP.

## <a name="using-asynchronous-apis"></a>Usando APIs assíncronas


Por convenção, os métodos assíncronos recebem nomes que terminam em "Async". Você normalmente chama APIs assíncronas em resposta à ação de um usuário, como, por exemplo, quando o usuário clica em um botão. Chamar um método assíncrono em um manipulador de eventos é uma das maneiras mais simples de usar APIs assíncronas. Aqui, usamos o operador de **espera** como exemplo.

Suponha que você tenha um aplicativo que lista os títulos das postagens de um blog de um determinado local. O aplicativo tem um [**Botão**](https://msdn.microsoft.com/library/windows/apps/BR209265) em que o usuário clica para obter os títulos. Os títulos são exibidos em um [**Bloco de Texto**](https://msdn.microsoft.com/library/windows/apps/BR209652). Quando o usuário clica no botão, é importante que o aplicativo continue respondendo enquanto aguarda a informação do site do blog. Para garantir essa capacidade de resposta, a UWP fornece um método assíncrono, [**SyndicationClient.RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460), para baixar o feed.

O exemplo a seguir obtém as listas de postagens de um blog chamando o método assíncrono, [**SyndicationClient.RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460), e aguardando o resultado.

> [!div class="tabbedCodeSnippets" data-resources="OutlookServices.Calendar"]
[!code-csharp[Main](./AsyncSnippets/csharp/MainPage.xaml.cs#SnippetDownloadRSS)]
[!code-vb[Main](./AsyncSnippets/vbnet/MainPage.xaml.vb#SnippetDownloadRSS)]

Há algumas coisas importantes sobre este exemplo. Primeiro, a linha `SyndicationFeed feed = await client.RetrieveFeedAsync(feedUri)` usa o operador de **espera** com a chamada para o método assíncrono, [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460). Você pode pensar no operador de **espera** dizendo ao compilador que está chamando um método assíncrono, o que faz com que o compilador realize um trabalho extra para que você não precise fazê-lo. Em seguida, a declaração do manipulador de eventos inclui a palavra-chave **async**. Você deve incluir essa palavra-chave na declaração de método de qualquer método em que usar o operador de **espera**.

Neste tópico, não entraremos em muitos detalhes sobre o que o compilador faz com o operador de **espera**, mas examinemos o que seu aplicativo faz para permanecer assíncrono e responsivo. Considere o que acontece quando você usa o código síncrono. Por exemplo, suponha que exista um método chamado `SyndicationClient.RetrieveFeed` que seja síncrono. (Esse método não existe, mas imagine que exista.) Se seu aplicativo incluísse a linha `SyndicationFeed feed = client.RetrieveFeed(feedUri)`, em vez de `SyndicationFeed feed = await client.RetrieveFeedAsync(feedUri)`, a execução do aplicativo seria interrompida até que o valor de retorno `RetrieveFeed` estivesse disponível. Além disso, enquanto seu aplicativo aguarda a conclusão do método, ele não pode responder a nenhum outro evento, como outro evento [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737). Ou seja, seu aplicativo fica bloqueado até que `RetrieveFeed` seja retornado.

Mas, se você chamar `client.RetrieveFeedAsync`, o método iniciará a recuperação e será retornado imediatamente. Quando você usa **espera** com [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460), o aplicativo sai temporariamente do manipulador de eventos. Dessa forma, ele pode processar outros eventos enquanto **RetrieveFeedAsync** é executado de maneira assíncrona. Isso mantém o aplicativo responsivo para o usuário. Quando **RetrieveFeedAsync** é finalizado e [**SyndicationFeed**](https://msdn.microsoft.com/library/windows/apps/BR243485) está disponível, o aplicativo basicamente volta a entrar no manipulador de eventos de onde saiu, depois de `SyndicationFeed feed = await client.RetrieveFeedAsync(feedUri)`, e conclui o restante do método.

A vantagem de usar o operador de **espera** é que o código não parece muito diferente daquele que utiliza o método imaginário `RetrieveFeed`. Existem maneiras de escrever código assíncrono em C# ou Visual Basic sem o operador de **espera**, mas o código resultante tende a enfatizar os mecanismos de execução de maneira assíncrona. Isso torna o código assíncrono difícil de escrever, entender e manter. Utilizando o operador de **espera**, você obtém os benefícios de um aplicativo assíncrono sem tornar o seu código complexo.

## <a name="return-types-and-results-of-asynchronous-apis"></a>Tipos de retorno e resultados de APIs assíncronas


Se você tiver seguido o link para [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460), deve ter notado que o tipo de retorno de **RetrieveFeedAsync** não é um [**SyndicationFeed**](https://msdn.microsoft.com/library/windows/apps/BR243485). Em vez disso, o tipo de retorno é `IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress>`. Exibida como sintaxe simples, uma API assíncrona retorna um objeto que contém o resultado no seu interior. Embora seja comum, e às vezes útil, pensar em um método assíncrono como sendo aguardado, o operador de **espera** trabalha, na realidade, no valor de retorno do método, não no método. Quando você aplica o operador de **espera**, o que obtém é o resultado da chamada de **GetResult** no objeto retornado pelo método. No exemplo, o **SyndicationFeed** é o resultado de **RetrieveFeedAsync.GetResult()**.

Ao usar um método assíncrono, você pode examinar a assinatura para ver o que receberá depois de aguardar o valor retornado pelo método. Todas as APIs assíncronas na UWP retornam um dos seguintes tipos:

-   [**IAsyncOperation&lt;TResult&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206598)
-   [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206594)
-   [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncaction.aspx)
-   [**IAsyncActionWithProgress&lt;TProgress&gt;**](https://msdn.microsoft.com/library/windows/apps/br206581.aspx)

O tipo de resultado de um método assíncrono é o mesmo que o parâmetro do tipo `      TResult`. Os tipos sem um `TResult` não têm um resultado. Você pode pensar no resultado como sendo **nulo**. No Visual Basic, um procedimento [Sub](https://msdn.microsoft.com/library/windows/apps/xaml/831f9wka.aspx) é equivalente a um método com um tipo de retorno **nulo**.

A tabela abaixo dá exemplos de métodos assíncronos e lista o tipo de retorno e o tipo de resultado de cada um.

| Método assíncrono                                                                           | Tipo de retorno                                                                                                                                        | Tipo de resultado                                       |
|-----------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|
| [**SyndicationClient.RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460)     | [**IAsyncOperationWithProgress&lt;SyndicationFeed, RetrievalProgress&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206594)                                 | [**SyndicationFeed**](https://msdn.microsoft.com/library/windows/apps/BR243485) |
| [**FileOpenPicker.PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/JJ635275) | [**IAsyncOperation&lt;StorageFile&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206598)                                                                                | [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171)          |
| [**XmlDocument.SaveToFileAsync**](https://msdn.microsoft.com/library/windows/apps/BR206284)                 | [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncaction.aspx)                                                                                                           | **nulo**                                          |
| [**InkStrokeContainer.LoadAsync**](https://msdn.microsoft.com/library/windows/apps/Hh701757)               | [**IAsyncActionWithProgress&lt;UInt64&gt;**](https://msdn.microsoft.com/library/windows/apps/br206581.aspx)                                                                   | **nulo**                                          |
| [**DataReader.LoadAsync**](https://msdn.microsoft.com/library/windows/apps/BR208135)                            | [**DataReaderLoadOperation**](https://msdn.microsoft.com/library/windows/apps/BR208120), uma classe de resultados personalizada que implementa **IAsyncOperation&lt;UInt32&gt;** | [**UInt32**](https://msdn.microsoft.com/library/windows/apps/br206598.aspx)                     |

 

Métodos assíncronos que são definidos em [**.NET para aplicativos UWP**](https://msdn.microsoft.com/library/windows/apps/xaml/br230232.aspx) têm o tipo de retorno [**Tarefa**](https://msdn.microsoft.com/library/windows/apps/xaml/system.threading.tasks.task.aspx) ou [**Task&lt;TResult&gt;**](https://msdn.microsoft.com/library/windows/apps/xaml/dd321424.aspx). Os métodos que retornam **Tarefa** são semelhantes aos métodos assíncronos na UWP que retornam [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncaction.aspx). Em cada caso, o resultado do método assíncrono é **nulo**. O tipo de retorno **Task&lt;TResult&gt;** é semelhante ao [**IAsyncOperation&lt;TResult&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206598) no sentido de que o resultado do método assíncrono ao executar a tarefa é do mesmo tipo do parâmetro tipo `TResult`. Para saber mais sobre como usar **.NET para aplicativos UWP** e tarefas, veja [Visão geral do .NET para aplicativos do Windows Runtime](https://msdn.microsoft.com/library/windows/apps/xaml/br230302.aspx).

## <a name="handling-errors"></a>Tratando erros


Ao usar o operador de **espera** para recuperar seus resultados de um método assíncrono, você pode usar um bloco **tentativa/obtenção** para manipular erros que ocorrem em métodos assíncronos, da mesma forma que você faz para os métodos síncronos. O exemplo anterior encapsula o método **RetrieveFeedAsync** e a operação de **espera** num bloco **tentativa/obtenção** para manipular erros quando uma exceção é acionada.

Quando métodos assíncronos chamam outros métodos assíncronos, qualquer método assíncrono que resulta numa exceção será propagado para os métodos externos. Isso significa que você pode colocar um bloco **tentativa/obtenção** no método mais externo para encontrar erros para os métodos assíncronos aninhados. Novamente, isso é semelhante à maneira como você obtém exceções para métodos síncronos. Porém, não é possível usar **espera** no bloco **obtenção**.

**Dica**começando com c# no Microsoft Visual Studio2005, você pode usar **await** no bloco **catch** .

## <a name="summary-and-next-steps"></a>Resumo e próximas etapas

O padrão de chamada de um método assíncrono que mostramos aqui é o mais simples de usar quando você chama APIs assíncronas em um manipulador de eventos. Você também pode usar esse padrão quando chamar um método assíncrono em um método substituído que retorna **nulo** ou um **Sub** no Visual Basic.

Quando você encontrar métodos assíncronos na UWP, é importante ter em mente os seguintes pontos:

-   Por convenção, os métodos assíncronos recebem nomes que terminam em "Async".
-   Qualquer método que use o operador de **espera** deve ter sua declaração marcada com a palavra-chave **async**.
-   Quando encontra o operador de **espera**, o aplicativo continua respondendo à interação do usuário enquanto o método assíncrono é executado.
-   Aguardar o valor retornado por um método assíncrono retorna um objeto que contém o resultado. Na maioria dos casos, o resultado contido no valor de retorno é o que é útil, e não o valor de retorno propriamente dito. É possível encontrar o tipo do valor contido dentro do resultado, observando o tipo de retorno do método assíncrono.
-   O uso de APIs assíncronas e padrões **async** costuma ser uma maneira de aprimorar a resposta de seu aplicativo.

O exemplo neste tópico envia um texto com essa aparência.

``` syntax
Windows Experience Blog
PC Snapshot: Sony VAIO Y, 8/9/2011 10:26:56 AM -07:00
Tech Tuesday Live Twitter #Chat: Too Much Tech #win7tech, 8/8/2011 12:48:26 PM -07:00
Windows 7 themes: what’s new and what’s popular!, 8/4/2011 11:56:28 AM -07:00
PC Snapshot: Toshiba Satellite A665 3D, 8/2/2011 8:59:15 AM -07:00
Time for new school supplies? Find back-to-school deals on Windows 7 PCs and Office 2010, 8/1/2011 2:14:40 PM -07:00
Best PCs for blogging (or working) on the go, 8/1/2011 10:08:14 AM -07:00
Tech Tuesday – Blogging Tips and Tricks–#win7tech, 8/1/2011 9:35:54 AM -07:00
PC Snapshot: Lenovo IdeaPad U460, 7/29/2011 9:23:05 AM -07:00
GIVEAWAY: Survive BlogHer with a Sony VAIO SA and a Samsung Focus, 7/28/2011 7:27:14 AM -07:00
3 Ways to Stay Cool This Summer, 7/26/2011 4:58:23 PM -07:00
Getting RAW support in Photo Gallery & Windows 7 (…and a contest!), 7/26/2011 10:40:51 AM -07:00
Tech Tuesdays Live Twitter Chats: Photography Tips, Tricks and Essentials, 7/25/2011 12:33:06 PM -07:00
3 Tips to Go Green With Your PC, 7/22/2011 9:19:43 AM -07:00
How to: Buy a Green PC, 7/22/2011 9:13:22 AM -07:00
Windows 7 themes: the distinctive artwork of Cheng Ling, 7/20/2011 9:53:07 AM -07:00
```
