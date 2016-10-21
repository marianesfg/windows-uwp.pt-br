---
author: TylerMSFT
title: Manipular a retomada do aplicativo
description: "Saiba como atualizar o conteúdo exibido quando o sistema retomar o aplicativo."
ms.assetid: DACCC556-B814-4600-A10A-90B82664EA15
translationtype: Human Translation
ms.sourcegitcommit: 231161ba576a140859952a7e9a4e8d3bd0ba4596
ms.openlocfilehash: 2813a112f9d60c5b133284903c98a152bd027bee

---

# Manipular a retomada do aplicativo

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**APIs Importantes**

-   [**Retomando**](https://msdn.microsoft.com/library/windows/apps/br242339)

Saiba onde atualizar a interface do usuário quando o sistema retomar o aplicativo. O exemplo deste tópico registra um manipulador de evento para o evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339).

## Registrar o manipulador de eventos de retomada

Registre para manipular o evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339), que indica que o usuário alternou para outro aplicativo que não o seu e, depois, de volta.

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

## Atualizar conteúdo exibido e readquirir recursos

O sistema suspende o aplicativo após alguns segundos depois que o usuário alterna para outro aplicativo ou para a área de trabalho. O sistema retoma o seu aplicativo quando o usuário alterna de volta para ele. Quando o sistema retoma o aplicativo, o conteúdo das variáveis e estruturas de dados é o mesmo de antes da suspensão do aplicativo pelo sistema. O sistema restaura o aplicativo onde ele parou. Para o usuário, ele seria exibido como se o aplicativo tivesse executado no segundo plano.

Quando o aplicativo manipula o evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339), o aplicativo pode ter sido suspenso por horas ou dias. Ele deve atualizar qualquer conteúdo que possa ter ficado obsoleto quando o aplicativo foi suspenso, como feeds de notícias ou a localização do usuário.

Esse também é um bom momento para restaurar todos os recursos exclusivos lançados quando o aplicativo foi suspenso, como identificadores de arquivo, câmeras, dispositivos E/S, dispositivos externos e recursos de rede.

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

## Comentários

Quando seu aplicativo está anexado ao depurador do Visual Studio, ele não será suspenso. Porém, você pode suspendê-lo no depurador e enviar um evento **Resume** de maneira que você possa depurar seu código. Verifique se a **barra de ferramentas Local de Depuração** está visível e clique no menu suspenso ao lado do ícone **Suspender**. Escolha **Retomar**.

Para aplicativos da Loja do Windows Phone, o evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) sempre é seguido por [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335), mesmo quando seu aplicativo está suspenso e o usuário inicia novamente seu aplicativo de um bloco principal ou da lista de aplicativos. Os aplicativos podem pular a inicialização se já houver conteúdo definido na janela atual. Você pode verificar a propriedade [**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736) para determinar se o aplicativo foi aberto de um bloco principal ou secundário e, com base nisso, decidir se você deve ou não retomar a experiência de aplicativo a partir daquele ponto ou apresentar uma nova.

## Tópicos relacionados

* [Ciclo de vida do aplicativo](app-lifecycle.md)
* [Tratar a ativação do aplicativo](activate-an-app.md)
* [Tratar a suspensão do aplicativo](suspend-an-app.md)



<!--HONumber=Aug16_HO3-->


