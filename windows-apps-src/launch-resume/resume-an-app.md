---
author: TylerMSFT
title: Tratar a retomada do app
description: "Saiba como atualizar o conteúdo exibido quando o sistema retomar o app."
ms.assetid: DACCC556-B814-4600-A10A-90B82664EA15
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 113f0ce8e59bab716443c0a74ca39649a1bb83ac
ms.lasthandoff: 02/07/2017

---

# <a name="handle-app-resume"></a>Tratar a retomada do app

[ Atualizado para apps UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**APIs Importantes**

-   [**Retomando**](https://msdn.microsoft.com/library/windows/apps/br242339)

Saiba onde atualizar a interface do usuário quando o sistema retomar o app. O exemplo deste tópico registra um manipulador de evento para o evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339).

## <a name="register-the-resuming-event-handler"></a>Registrar o manipulador de eventos de retomada

Registre para manipular o evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339), que indica que o usuário alternou para outro app que não o seu e, depois, de volta.

> [!div class="tabbedCodeSnippets"]
> ```cs
> partial class MainPage
> {
>    public MainPage()
>    {
>       InitializeComponent();
>       Application.Current.Resuming += new EventHandler<Object>(App_Resuming);
>    }
> }
> ```
> ```vb
> Public NonInheritable Class MainPage
>
>    Public Sub New()
>       InitializeComponent()
>       AddHandler Application.Current.Resuming, AddressOf App_Resuming
>    End Sub
>
> End Class
> ```
> ```cpp
> MainPage::MainPage()
> {
>     InitializeComponent();
>     Application::Current->Resuming +=
>         ref new EventHandler<Platform::Object^>(this, &MainPage::App_Resuming);
> }
> ```

## <a name="refresh-displayed-content-and-reacquire-resources"></a>Atualizar conteúdo exibido e readquirir recursos

O sistema suspende o app após alguns segundos depois que o usuário alterna para outro app ou para a área de trabalho. O sistema retoma o seu app quando o usuário alterna de volta para ele. Quando o sistema retoma o app, o conteúdo das variáveis e estruturas de dados é o mesmo de antes da suspensão do app pelo sistema. O sistema restaura o app onde ele parou. Para o usuário, ele seria exibido como se o app tivesse executado no segundo plano.

Quando o app manipula o evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339), o app pode ter sido suspenso por horas ou dias. Ele deve atualizar qualquer conteúdo que possa ter ficado obsoleto quando o app foi suspenso, como feeds de notícias ou a localização do usuário.

Esse também é um bom momento para restaurar todos os recursos exclusivos lançados quando o app foi suspenso, como identificadores de arquivo, câmeras, dispositivos E/S, dispositivos externos e recursos de rede.

> [!div class="tabbedCodeSnippets"]
> ```cs
> partial class MainPage
> {
>     private void App_Resuming(Object sender, Object e)
>     {
>         // TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
>     }
> }
> ```
> ```vb
> Public NonInheritable Class MainPage
>
>     Private Sub App_Resuming(sender As Object, e As Object)
>  
>         ' TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
>
>     End Sub
>
> End Class
> ```
> ```cpp
> void MainPage::App_Resuming(Object^ sender, Object^ e)
> {
>     // TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
> }
> ```

> **Observação**  Como o evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) não é gerado do thread da interface do usuário, um dispatcher deverá ser usado no identificador para despachar todas as chamadas para a interface do usuário.

## <a name="remarks"></a>Comentários

Quando seu app está anexado ao depurador do Visual Studio, ele não será suspenso. Porém, você pode suspendê-lo no depurador e enviar um evento **Resume** de maneira que você possa depurar seu código. Verifique se a **barra de ferramentas Local de Depuração** está visível e clique no menu suspenso ao lado do ícone **Suspender**. Escolha **Retomar**.

Para apps da Loja do Windows Phone, o evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) sempre é seguido por [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335), mesmo quando seu app está suspenso e o usuário inicia novamente seu app de um bloco principal ou da lista de apps. Os apps podem pular a inicialização se já houver conteúdo definido na janela atual. Você pode verificar a propriedade [**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736) para determinar se o app foi aberto de um bloco principal ou secundário e, com base nisso, decidir se você deve ou não retomar a experiência de app a partir daquele ponto ou apresentar uma nova.

## <a name="related-topics"></a>Tópicos relacionados

* [Ciclo de vida do app](app-lifecycle.md)
* [Tratar a ativação do app](activate-an-app.md)
* [Tratar a suspensão do app](suspend-an-app.md)

