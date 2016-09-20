---
author: TylerMSFT
title: Manipular a retomada do aplicativo
description: "Saiba como atualizar o conteúdo exibido quando o sistema retomar o aplicativo."
ms.assetid: DACCC556-B814-4600-A10A-90B82664EA15
ms.sourcegitcommit: e6957dd44cdf6d474ae247ee0e9ba62bf17251da
ms.openlocfilehash: dd3d75c7f3dfe325324e1fe31c039cd207b68d0b

---

# Manipular a retomada do aplicativo


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**APIs importantes**

-   [**Retomando**](https://msdn.microsoft.com/library/windows/apps/br242339)

Saiba como atualizar o conteúdo exibido quando o sistema retomar o aplicativo. O exemplo deste tópico registra um manipulador de evento para o evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339).


            **Mapa:** como este tópico está relacionado aos outros? Veja:

-   [Mapa de aplicativos do Windows Runtime em C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/br229583)
-   [Mapa de aplicativos do Tempo de Execução do Windows em C++](https://msdn.microsoft.com/library/windows/apps/hh700360)

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

## [!div class="tabbedCodeSnippets"]

Atualizar o conteúdo exibido após a suspensão

> [!div class="tabbedCodeSnippets"]
> ```cs
> partial class MainPage
> {
>     private void App_Resuming(Object sender, Object e)
>     {
>         // TODO: Refresh network data
>     }
> }
> ```
> ```vb
> Public NonInheritable Class MainPage
>
>     Private Sub App_Resuming(sender As Object, e As Object)
>  
>         ' TODO: Refresh network data
>
>     End Sub
>
> End Class
> ```
> ```cpp
> void MainPage::App_Resuming(Object^ sender, Object^ e)
> {
>     // TODO: Refresh network data
> }
> ```

> Quando o seu aplicativo manipula o evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339), ele tem a oportunidade de atualizar o seu conteúdo exibido.

## [!div class="tabbedCodeSnippets"]



            **Observação**  Como o evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) não é gerado a partir do thread da interface de usuário, um dispatcher deve ser usado para obter esse thread e injetar uma atualização para a interface de usuário, se for isso que você deseja fazer com seu manipulador. Comentários O sistema suspende o aplicativo sempre que o usuário alterna para outro aplicativo ou para a área de trabalho. O sistema retoma o seu aplicativo sempre que o usuário alterna de volta para ele. Quando o sistema retoma o aplicativo, o conteúdo das variáveis e estruturas de dados é o mesmo de antes da suspensão do aplicativo pelo sistema.

O sistema restaura o aplicativo exatamente como ele havia parado, de maneira que o usuário tem impressão de que ele estava sendo executado em tela de fundo.

> Contudo, o aplicativo pode ter sido suspendo por uma quantidade significativa de tempo, de maneira que ele deve atualizar o conteúdo exibido que pode ter mudado enquanto o aplicativo estava suspenso, como é o caso de feeds de notícias ou da localização do usuário. Caso o seu aplicativo não tenha conteúdo algum para atualizar, não há necessidade de manipular o evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339). 
            **Observação** Quando seu aplicativo está anexado ao depurador do Visual Studio, você pode enviar a ele um evento **Retomar**.

> Verifique se a **barra de ferramentas Local de Depuração** está visível e clique no menu suspenso ao lado do ícone **Suspender**. Escolha **Retomar**. 
            **Observação**  Para aplicativos da Loja do Windows Phone, o evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) é sempre seguido por [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335), mesmo quando o seu aplicativo está suspenso no momento e o usuário reabri-lo em um bloco principal ou lista de aplicativos.

## Os aplicativos podem pular a inicialização se já houver conteúdo definido na janela atual.

* [Você pode verificar a propriedade [**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736) para determinar se o aplicativo foi aberto de um bloco principal ou secundário e, com base nisso, decidir se você deve ou não retomar a experiência de aplicativo a partir daquele ponto ou apresentar uma nova.](activate-an-app.md)
* [Tópicos relacionados](suspend-an-app.md)
* [Manipular a ativação do aplicativo](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [Manipular a suspensão do aplicativo](app-lifecycle.md)



<!--HONumber=Jun16_HO5-->


