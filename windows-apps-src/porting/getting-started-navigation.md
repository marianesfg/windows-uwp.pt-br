---
author: mcleblanc
title: "Introdução à navegação"
description: "Introdução à navegação"
ms.assetid: F4DF5C5F-C886-4483-BBDA-498C4E2C1BAF
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: daf56299b3a5e81bb756bc9fcb8112da044b00f3

---

# Introdução: navegação

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

## Adicionando navegação

O iOS oferece a classe **UINavigationController** para auxiliar a navegação no aplicativo: você pode inserir e remover exibições para criar a hierarquia de **UIViewControllers** que definem o seu aplicativo.

Por outro lado, um aplicativo do Windows 10 com muitas exibições precisa de uma abordagem no estilo de site da web para a navegação. Você pode imaginar os usuários percorrendo as páginas conforme clicam nos controles para trabalhar de sua maneira pelo aplicativo. Para obter mais informações, consulte [Noções básicas de design de navegação](https://msdn.microsoft.com/library/windows/apps/dn958438).

Uma das formas de gerenciar essa navegação em um aplicativo do Windows 10 é usar a classe [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682). O passo a passo a seguir mostra como experimentar isso.

Continuando com a solução que você começou antes, abra o arquivo **MainPage. XAML** e adicione um botão na exibição **Design**. Altere a propriedade **Content** do botão de "Botão" para "Ir para Página". Então, crie um manipulador para o evento **Click** do botão, conforme a imagem a seguir. Se você não se lembra de como fazer isso, consulte o passo a passo da seção anterior (Dica: clique duas vezes no botão na exibição **Design**).

![adicionando um botão e seu evento de clique no visual studio](images/ios-to-uwp/vs-go-to-page.png)

Vamos adicionar uma nova página. Na exibição **Solução**, toque no menu **Projeto** e em **Adicionar novo item**. Toque em **Página em branco**, como mostrado na imagem seguir e, em seguida, em **Adicionar**.

![adicionando uma nova página no visual studio](images/ios-to-uwp/vs-add-new-page.png)

Em seguida, adicione um botão ao arquivo BlankPage.xaml. Vamos usar o controle AppBarButton e dar a ele uma imagem de seta voltar: na exibição **XAML**, adicione ` <AppBarButton Icon="Back"/>` entre os elementos `<Grid> </Grid>`.

Agora, vamos adicionar um manipulador de evento ao botão: clique duas vezes no controle na exibição **Design**, e o Microsoft Visual Studio adicionará o texto "AppBarButton\_Click" à caixa **Clique**, como mostrado na imagem a seguir, e, então, adicionará e exibirá o manipulador de evento correspondente no arquivo BlankPage.xaml.cs.

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

A navegação de páginas é gerenciada pela classe [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682). Como a classe **UINavigationController** no iOS usa métodos **pushViewController** e **popViewController**, a classe **Frame** para aplicativos da Windows Store fornece métodos [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) e [**GoBack**](https://msdn.microsoft.com/library/windows/apps/dn996568). A classe **Frame** também tem um método chamado [**GoForward**](https://msdn.microsoft.com/library/windows/apps/br242693), que faz avançar.

Esse passo a passo cria uma nova instância de BlankPage cada vez que você navega por ele. (A instância anterior será *liberada*, automaticamente). Se você não quer que cada vez uma nova instância seja criada, adicione o código a seguir ao construtor da classe BlankPage no arquivo BlankPage.xaml.cs. Isso ativará o comportamento [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506).

```csharp
public BlankPage()
{
    this.InitializeComponent();
    // Add the following line of code.
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
}
```

Você também pode ter ou definir a propriedade da classe **Frame** [**CacheSize**](https://msdn.microsoft.com/library/windows/apps/br242683) para gerenciar quantas páginas no histórico de navegação podem ser armazenas em cache.

Para saber mais sobre navegação, consulte [Navegação](https://msdn.microsoft.com/library/windows/apps/mt187344) e [XAML personality animations sample](http://go.microsoft.com/fwlink/p/?LinkID=242401).

**Observação**  Para saber mais sobre navegação para aplicativos da Windows Store usando JavaScript e HTML, consulte [Guia de início rápido: usando a navegação de página única (HTML)](https://msdn.microsoft.com/library/windows/apps/hh452768).
 
### Próxima etapa

[Introdução: Animação](getting-started-animation.md)




<!--HONumber=Aug16_HO3-->


