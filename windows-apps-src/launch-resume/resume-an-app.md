---
title: Manipular a retomada do aplicativo
description: Saiba como atualizar o conteúdo exibido quando o sistema retomar o app.
ms.assetid: DACCC556-B814-4600-A10A-90B82664EA15
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: f424a274d3e96b58f32875620f3165ccfac82ba6
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8792232"
---
# <a name="handle-app-resume"></a>Tratar a retomada do app

**APIs Importantes**

- [**Retomando**](https://msdn.microsoft.com/library/windows/apps/br242339)

Saiba onde atualizar a interface do usuário quando o sistema retomar o aplicativo. O exemplo deste tópico registra um manipulador de evento para o evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339).

## <a name="register-the-resuming-event-handler"></a>Registrar o manipulador de eventos de retomada

Registre para manipular o evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339), que indica que o usuário alternou para outro aplicativo que não o seu e, depois, de volta.

```csharp
partial class MainPage
{
   public MainPage()
   {
      InitializeComponent();
      Application.Current.Resuming += new EventHandler<Object>(App_Resuming);
   }
}
```

```vb
Public NonInheritable Class MainPage

   Public Sub New()
      InitializeComponent()
      AddHandler Application.Current.Resuming, AddressOf App_Resuming
   End Sub

End Class
```

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();
    Windows::UI::Xaml::Application::Current().Resuming({ this, &MainPage::App_Resuming });
}
```

```cpp
MainPage::MainPage()
{
    InitializeComponent();
    Application::Current->Resuming +=
        ref new EventHandler<Platform::Object^>(this, &MainPage::App_Resuming);
}
```

## <a name="refresh-displayed-content-and-reacquire-resources"></a>Atualizar conteúdo exibido e readquirir recursos

O sistema suspende o aplicativo após alguns segundos depois que o usuário alterna para outro aplicativo ou para a área de trabalho. O sistema retoma o seu aplicativo quando o usuário alterna de volta para ele. Quando o sistema retoma o aplicativo, o conteúdo das variáveis e estruturas de dados é o mesmo de antes da suspensão do aplicativo pelo sistema. O sistema restaura o aplicativo onde ele parou. Para o usuário, ele seria exibido como se o aplicativo tivesse executado no segundo plano.

Quando o aplicativo manipula o evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339), o aplicativo pode ter sido suspenso por horas ou dias. Ele deve atualizar qualquer conteúdo que possa ter ficado obsoleto quando o aplicativo foi suspenso, como feeds de notícias ou a localização do usuário.

Esse também é um bom momento para restaurar todos os recursos exclusivos lançados quando o aplicativo foi suspenso, como identificadores de arquivo, câmeras, dispositivos E/S, dispositivos externos e recursos de rede.

```csharp
partial class MainPage
{
    private void App_Resuming(Object sender, Object e)
    {
        // TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
    }
}
```

```vb
Public NonInheritable Class MainPage

    Private Sub App_Resuming(sender As Object, e As Object)
 
        ' TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.

    End Sub
>
End Class
```

```cppwinrt
void MainPage::App_Resuming(
    Windows::Foundation::IInspectable const& /* sender */,
    Windows::Foundation::IInspectable const& /* e */)
{
    // TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
}
```

```cpp
void MainPage::App_Resuming(Object^ sender, Object^ e)
{
    // TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
}
```

> [!NOTE]
> Como o evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) não é gerado do thread da interface do usuário, um dispatcher deverá ser usado em seu manipulador para despachar todas as chamadas para a interface do usuário.

## <a name="remarks"></a>Comentários

Quando seu aplicativo está anexado ao depurador do Visual Studio, ele não será suspenso. Porém, você pode suspendê-lo no depurador e enviar um evento **Resume** de maneira que você possa depurar seu código. Verifique se a **barra de ferramentas Local de Depuração** está visível e clique no menu suspenso ao lado do ícone **Suspender**. Escolha **Retomar**.

Para aplicativos da Loja do Windows Phone, o evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) sempre é seguido por [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335), mesmo quando seu aplicativo está suspenso e o usuário inicia novamente seu aplicativo de um bloco principal ou da lista de aplicativos. Os aplicativos podem pular a inicialização se já houver conteúdo definido na janela atual. Você pode verificar a propriedade [**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736) para determinar se o aplicativo foi aberto de um bloco principal ou secundário e, com base nisso, decidir se você deve ou não retomar a experiência de aplicativo a partir daquele ponto ou apresentar uma nova.

## <a name="related-topics"></a>Tópicos relacionados

* [Ciclo de vida do aplicativo](app-lifecycle.md)
* [Tratar a ativação do aplicativo](activate-an-app.md)
* [Tratar a suspensão do aplicativo](suspend-an-app.md)
