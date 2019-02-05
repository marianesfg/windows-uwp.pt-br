---
title: Usando objetos do Windows Runtime em um ambiente multithread | Microsoft Docs
description: Este artigo discute a maneira como o .NET Framework lida com chamadas de códigos C# e Visual Basic para objetos fornecidos pelo Windows Runtime ou pelo Componentes do Tempo de Execução do Windows.
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 43ffd28c-c4df-405c-bf5c-29c94e0d142b
keywords: windows 10, uwp, temporizador, threads
ms.localizationpriority: medium
ms.openlocfilehash: f11207a774b1ffcebde95e316634592020e6ed49
ms.sourcegitcommit: b975c8fc8cf0770dd73d8749733ae5636f2ee296
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/05/2019
ms.locfileid: "9058497"
---
# <a name="using-windows-runtime-objects-in-a-multithreaded-environment"></a>Usando objetos do Windows Runtime em um ambiente multithread
Este artigo discute a maneira como o .NET Framework lida com chamadas de códigos C# e Visual Basic para objetos fornecidos pelo Windows Runtime ou pelo Componentes do Tempo de Execução do Windows.

No .NET Framework, você pode acessar qualquer objeto de vários threads por padrão, sem tratamento especial. Tudo o que você precisa é uma referência ao objeto. No Windows Runtime, esses objetos são chamados de *ágil*. A maioria das classes do Windows Runtime são ágeis, mas algumas classes não são e até mesmo classes ágeis podem exigir um tratamento especial.

Sempre que possível, o common language runtime (CLR) lida com objetos de outras fontes, como o Windows Runtime, como se fossem objetos do .NET Framework:

- Se o objeto implementa a interface [IAgileObject](https://msdn.microsoft.com/library/Hh802476.aspx), ou possui o atributo [MarshalingBehaviorAttribute](https://go.microsoft.com/fwlink/p/?LinkId=256022) com [MarshalingType.Agile](https://go.microsoft.com/fwlink/p/?LinkId=256023), o CLR o trata como ágil.

- Se o CLR puder fazer uma chamada do thread onde foi feita para o contexto de threading do objeto alvo, ele faz isso de forma transparente.

- Se o objeto possui o atributo [MarshalingBehaviorAttribute](https://go.microsoft.com/fwlink/p/?LinkId=256022) com [MarshalingType.None](https://go.microsoft.com/fwlink/p/?LinkId=256023), a classe não fornece informações de integração. O CLR não pode marcar a chamada, então ele cria uma exceção [InvalidCastException](/dotnet/api/system.invalidcastexception) com uma mensagem indicando que o objeto pode ser usado apenas no contexto de threading onde foi criado.

As seções a seguir descrevem os efeitos desse comportamento em objetos de várias fontes.

## <a name="objects-from-a-windows-runtime-component-that-is-written-in-c-or-visual-basic"></a>Objetos de um componente do Tempo de Execução do Windows escrito em C# ou Visual Basic
Todos os tipos no componente que podem ser ativados são ágeis por padrão.

> [!NOTE]
>  Agilidade não implica a segurança do thread. Tanto no Windows Runtime como no .NET Framework, a maioria das classes não são seguras porque a segurança do segmento tem um custo de desempenho e a maioria dos objetos nunca é acessada por múltiplos segmentos. É mais eficiente sincronizar o acesso a objetos individuais (ou usar classes seguras) apenas conforme necessário.

Quando você autoriza um componente do Tempo de Execução do Windows, você pode substituir o padrão. Consulte as interfaces [ICustomQueryInterface](/dotnet/api/system.runtime.interopservices.icustomqueryinterface) e [IAgileObject](https://msdn.microsoft.com/library/Hh802476.aspx).

## <a name="objects-from-the-windows-runtime"></a>Objeto do Windows Runtime
A maioria das classes no Windows Runtime são ágeis, e o CLR as trata como ágil. A documentação para essas classes lista "MarshalingBehaviorAttribute(Agile)" entre os atributos da classe. No entanto, os membros de algumas dessas classes ágeis, como os controles XAML, lançam exceções se não forem chamados no segmento da interface de usuário. Por exemplo, o código a seguir tenta usar uma linha de fundo para definir uma propriedade do botão que foi clicado. A propriedade [Conteúdo](https://go.microsoft.com/fwlink/p/?LinkId=256025) do botão lança uma exceção.

```csharp
private async void Button_Click_2(object sender, RoutedEventArgs e)
{
    Button b = (Button) sender;
    await Task.Run(() => {
        b.Content += ".";
    });
}
```

```vb
Private Async Sub Button_Click_2(sender As Object, e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    Await Task.Run(Sub()
                       b.Content &= "."
                   End Sub)
End Sub
```

Você pode acessar o botão com segurança usando sua propriedade [Expedidor](https://go.microsoft.com/fwlink/p/?LinkId=256026), ou a propriedade `Dispatcher` de qualquer objeto que exista no contexto do segmento da interface de usuário (como a página em que o botão está). O código a seguir usa, do objeto [CoreDispatcher](https://go.microsoft.com/fwlink/p/?LinkId=256029), o método [RunAsync](https://go.microsoft.com/fwlink/p/?LinkId=256030) para enviar a chamada no segmento da interface de usuário.

```csharp
private async void Button_Click_2(object sender, RoutedEventArgs e)
{
    Button b = (Button) sender;
    await b.Dispatcher.RunAsync(
        Windows.UI.Core.CoreDispatcherPriority.Normal,
        () => {
            b.Content += ".";
    });
}

```

```vb
Private Async Sub Button_Click_2(sender As Object, e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    Await b.Dispatcher.RunAsync(
        Windows.UI.Core.CoreDispatcherPriority.Normal,
        Sub()
            b.Content &= "."
        End Sub)
End Sub
```

> [!NOTE]
>  A propriedade `Dispatcher` não lança uma exceção quando ela é chamada a partir de outro segmento.

O tempo de vida de um objeto do Windows Runtime que é criado no segmento da interface de usuário é delimitado pelo tempo de vida do segmento. Não tente acessar objetos em um segmento da interface de usuário após a janela ter fechado.

Se você criar seu próprio controle ao herdar um controle XAML ou ao compor um conjunto de controles XAML, seu controle é ágil porque é um objeto do .NET Framework. No entanto, se ele chama membros da sua classe base ou de classes constituintes, ou se você chama membros herdados, esses membros lançarão exceções quando forem chamados de qualquer segmento, exceto o segmento da interface de usuário.

### <a name="classes-that-cant-be-marshaled"></a>Classes que não podem ser integradas
Classes do Windows Runtime que não fornecem informações de integração possuem o atributo [MarshalingBehaviorAttribute](https://go.microsoft.com/fwlink/p/?LinkId=256022) com [MarshalingType.None](https://go.microsoft.com/fwlink/p/?LinkId=256023). A documentação para tal classe lista "MarshalingBehaviorAttribute(None)" entre seus atributos.

O código a seguir cria um objeto [CameraCaptureUI](https://go.microsoft.com/fwlink/p/?LinkId=256027) no segmento da interface de usuário, e então tenta definir uma propriedade do objeto a partir de um segmento do grupo de threads. O CLR não consegue fazer a chamada, e cria uma exceção [InvalidCastException](/dotnet/api/system.invalidcastexception) com uma mensagem indicando que o objeto pode ser usado apenas no contexto de threading onde foi criado.

```csharp
Windows.Media.Capture.CameraCaptureUI ccui;

private async void Button_Click_1(object sender, RoutedEventArgs e)
{
    ccui = new Windows.Media.Capture.CameraCaptureUI();

    await Task.Run(() => {
        ccui.PhotoSettings.AllowCropping = true;
    });
}

```

```vb
Private ccui As Windows.Media.Capture.CameraCaptureUI

Private Async Sub Button_Click_1(sender As Object, e As RoutedEventArgs)
    ccui = New Windows.Media.Capture.CameraCaptureUI()

    Await Task.Run(Sub()
                       ccui.PhotoSettings.AllowCropping = True
                   End Sub)
End Sub
```

A documentação do [CameraCaptureUI](https://go.microsoft.com/fwlink/p/?LinkId=256027) também lista "ThreadingAttribute(STA)" entre os atributos da classe, porque ela deve ser criada em um contexto de segmento único, como o segmento da interface de usuário.

Se você quiser acessar o objeto [CameraCaptureUI](https://go.microsoft.com/fwlink/p/?LinkId=256027) a partir de outro segmento você pode armazenar o objeto [CoreDispatcher](https://go.microsoft.com/fwlink/p/?LinkId=256029) em cache para o segmento da interface de usuário, e usá-lo mais tarde para despachar a chamada nesse tópico. Ou você pode obter o expedidor de um objeto XAML, como a página, conforme mostrado no código a seguir.

```csharp
Windows.Media.Capture.CameraCaptureUI ccui;

private async void Button_Click_3(object sender, RoutedEventArgs e)
{
    ccui = new Windows.Media.Capture.CameraCaptureUI();

    await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
        () => {
            ccui.PhotoSettings.AllowCropping = true;
        });
}

```

```vb
Dim ccui As Windows.Media.Capture.CameraCaptureUI

Private Async Sub Button_Click_3(sender As Object, e As RoutedEventArgs)

    ccui = New Windows.Media.Capture.CameraCaptureUI()

    Await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                                Sub()
                                    ccui.PhotoSettings.AllowCropping = True
                                End Sub)
End Sub
```

## <a name="objects-from-a-windows-runtime-component-that-is-written-in-c"></a>Objetos de um componente do Tempo de Execução do Windows que está escrito em C ++
Por padrão, as classes no componente que podem ser ativadas são ágeis. No entanto, o C ++ permite uma quantidade significativa de controle sobre os modelos de threading e sobre o comportamento de integração. Conforme descrito anteriormente neste artigo, o CLR reconhece classes ágeis, tenta ordenar chamadas quando as classes não são ágeis, e gera uma exceção [System. InvalidCastException](/dotnet/api/system.invalidcastexception) quando uma classe não tem nenhuma informação de integração.

Para objetos que são executados no segmento da interface de usuário e lançam exceções quando são chamados de um segmento diferente do segmento da interface de usuário, você pode usar o objeto [CoreDispatcher](https://go.microsoft.com/fwlink/p/?LinkId=256029) do segmento da interface de usuário para despachar a chamada.

## <a name="see-also"></a>Veja também
[Guia de C#](/dotnet/csharp/)

[Guia do Visual Basic](/dotnet/visual-basic/)
