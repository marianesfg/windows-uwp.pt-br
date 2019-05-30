---
title: Introdução à navegação
description: Introdução à navegação
ms.assetid: F4DF5C5F-C886-4483-BBDA-498C4E2C1BAF
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: de9c5261afb7b76b2409599c9c1f88814d1dd6a1
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371785"
---
# <a name="getting-started-navigation"></a>Introdução: Navegação


## <a name="adding-navigation"></a>Adicionando navegação

O iOS oferece a classe **UINavigationController** para auxiliar a navegação no aplicativo: você pode inserir e remover exibições para criar a hierarquia de **UIViewControllers** que definem o seu aplicativo.

Por outro lado, um aplicativo do Windows 10 que contém vários modos de exibição leva mais de uma abordagem de site da web para navegação. Você pode imaginar os usuários percorrendo as páginas conforme clicam nos controles para trabalhar de sua maneira pelo aplicativo. Para obter mais informações, consulte [Noções básicas de design de navegação](https://docs.microsoft.com/windows/uwp/layout/navigation-basics).

Uma das maneiras de gerenciar essa navegação em um aplicativo do Windows 10 é usar o [ **quadro** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) classe. O guia passo a passo a seguir mostra como você pode tentar fazer isso.

Continuando com a solução que você começou antes, abra o arquivo **MainPage. XAML** e adicione um botão na exibição **Design**. Altere a propriedade **Content** do botão de "Botão" para "Ir para Página". Então, crie um manipulador para o evento **Click** do botão, conforme a imagem a seguir. Se você não se lembra de como fazer isso, consulte o passo a passo da seção anterior (Dica: clique duas vezes no botão na exibição **Design**).

![adicionando um botão e seu evento de clique no visual studio](images/ios-to-uwp/vs-go-to-page.png)

Vamos adicionar uma nova página. Na exibição **Solução**, toque no menu **Projeto** e em **Adicionar novo item**. Toque em **Página em branco**, como mostrado na imagem seguir e, em seguida, em **Adicionar**.

![adicionando uma nova página no visual studio](images/ios-to-uwp/vs-add-new-page.png)

Em seguida, adicione um botão ao arquivo BlankPage.xaml. Vamos usar o controle AppBarButton e dar a ele uma imagem de seta voltar: na exibição **XAML**, adicione ` <AppBarButton Icon="Back"/>` entre os elementos `<Grid> </Grid>`.

Agora, vamos adicionar um manipulador de eventos para o botão: clique duas vezes no controle na **Design** exibição e o Microsoft Visual Studio adiciona o texto "AppBarButton\_clique em" para o **clique** caixa, conforme mostrado no a figura, a seguir e, em seguida, adiciona e exibe o manipulador de eventos correspondentes no arquivo BlankPage.xaml.cs.

![adicionando um botão voltar e seu evento de clique no visual studio](images/ios-to-uwp/vs-add-back-button.png)

Se você voltar à exibição **XAML** do arquivo BlankPage.xaml, o código Extensible Application Markup Language (XAML) do elemento `<AppBarButton>` deverá aparecer desta forma agora:

` <AppBarButton Icon="Back" Click="AppBarButton_Click"/>`

Volte ao arquivo BlankPage.xaml.cs e adicione esse código para ir para a página anterior depois que o usuário tocar no botão.

```csharp
private void AppBarButton_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.    
    Frame.GoBack();
}
```

Por fim, abra o arquivo MainPage.xaml.cs e adicione este código. Ele abre BlankPage depois que o usuário toca no botão.

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.
    Frame.Navigate(typeof(BlankPage1));
}
```

Agora, execute o programa. Toque no botão "Ir para Página" para ir para a outra página e, depois, toque no botão de seta voltar para retornar à página anterior.

A navegação de páginas é gerenciada pela classe [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame). Como o **UINavigationController** classe no iOS usa **pushViewController** e **popViewController** métodos, o **quadro** de classe para Fornece aplicativos UWP [ **Navigate** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate) e [ **GoBack** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goback) métodos. A classe **Frame** também tem um método chamado [**GoForward**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goforward), que faz avançar.

Esse passo a passo cria uma nova instância de BlankPage cada vez que você navega por ele. (A instância anterior será *liberada*, automaticamente). Se você não quer que cada vez uma nova instância seja criada, adicione o código a seguir ao construtor da classe BlankPage no arquivo BlankPage.xaml.cs. Isso ativará o comportamento [**NavigationCacheMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.navigationcachemode).

```csharp
public BlankPage()
{
    this.InitializeComponent();
    // Add the following line of code.
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
}
```

Você também pode ter ou definir a propriedade da classe **Frame**[**CacheSize**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.cachesize) para gerenciar quantas páginas no histórico de navegação podem ser armazenas em cache.

Para saber mais sobre navegação, consulte [Navegação](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) e [XAML personality animations sample](https://go.microsoft.com/fwlink/p/?LinkID=242401).

**Observação**  para obter informações sobre a navegação para aplicativos UWP usando JavaScript e HTML, consulte [guia de início rápido: Usando a navegação de página única](https://docs.microsoft.com/previous-versions/windows/apps/hh452768(v=win.10)).
 
### <a name="next-step"></a>Próximas etapas

[Guia de Introdução: Animação](getting-started-animation.md)

