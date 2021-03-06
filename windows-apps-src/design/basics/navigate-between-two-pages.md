---
Description: Saiba como habilitar a navegação ponto a ponto entre duas páginas básicas em um aplicativo do Windows.
title: Navegação ponto a ponto entre duas páginas
ms.assetid: 0A364C8B-715F-4407-9426-92267E8FB525
label: Peer-to-peer navigation between two pages
template: detail.hbs
op-migration-status: ready
ms.date: 07/13/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 50211600931fc67c43aa577fabe23f1277a0e897
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83233820"
---
# <a name="implement-navigation-between-two-pages"></a>Implementar a navegação entre duas páginas

Saiba como usar um quadro e páginas para habilitar a navegação básica ponto a ponto no aplicativo. 

> **APIs importantes**: Classe [**Windows.UI.Xaml.Controls.Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame), classe [**Windows.UI.Xaml.Controls.Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page), namespace [**Windows.UI.Xaml.Navigation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Navigation)

![navegação ponto a ponto](images/peertopeer.png)

## <a name="1-create-a-blank-app"></a>1. Crie um aplicativo em branco

1.  No menu do Microsoft Visual Studio, escolha **Arquivo** > **Novo Projeto**.
2.  No painel esquerdo da caixa de diálogo **Novo Projeto**, escolha o nó **Visual C#**  > **Windows** > **Universal** ou o nó **Visual C++**  > **Windows** > **Universal**.
3.  No painel central, selecione **Aplicativo em Branco**.
4.  Na caixa **Nome**, insira **NavApp1** e selecione o botão **OK**.
    A solução é criada e os arquivos de projeto aparecem no **Gerenciador de Soluções**.
5.  Para executar o programa, escolha **Depurar** > **Iniciar Depuração** no menu ou pressione F5.
    Uma página em branco é exibida.
6.  Para interromper a depuração e retornar ao Visual Studio, saia do aplicativo, ou clique em **Parar Depuração** no menu.

## <a name="2-add-basic-pages"></a>2. Adicionar páginas básicas

Em seguida, adicione duas páginas ao projeto.

1.  Em **Gerenciador de soluções**, clique com botão direito no nó de projeto **BlankApp** para abrir o menu de atalho.
2.  Escolha **Adicionar** > **Novo Item** no menu de atalho.
3.  Na caixa de diálogo **Adicionar novo item**, selecione **Página em branco** no painel do meio.
4.  Na caixa **Nome**, insira **Page1** (ou **Page2**) e pressione o botão **Adicionar**.
5. Repita as etapas 1 a 4 para adicionar a segunda página.

Esses arquivos agora devem ser listados como parte do seu projeto NavApp1.

<table>
<thead>
<tr class="header">
<th align="left">C#</th>
<th align="left">C++</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;"><ul>
<li>Page1.xaml</li>
<li>Page1.xaml.cs</li>
<li>Page2.xaml</li>
<li>Page2.xaml.cs</li>
</ul></td>
<td style="vertical-align:top;"><ul>
<li>Page1.xaml</li>
<li>Page1.xaml.cpp</li>
<li>Page1.xaml.h</li>
<li>Page2.xaml</li>
<li>Page2.xaml.cpp</li>
<li>Page2.xaml.h

</li>
</ul></td>
</tr>
</tbody>
</table>

Em Page1.xaml, adicione o seguinte conteúdo:

-   Um elemento [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) chamado `pageTitle` como elemento filho da raiz [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid). Altere a propriedade [**Text**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.text) para `Page 1`.
```xaml
<TextBlock x:Name="pageTitle" Text="Page 1" />
```

-   Um elemento [**HyperlinkButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton) como elemento filho da raiz [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) e depois o elemento `pageTitle` [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock).
```xaml
<HyperlinkButton Content="Click to go to page 2"
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

No arquivo code-behind Page1.xaml, adicione o seguinte código para manipular o evento `Click` do [**HyperlinkButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton) adicionado para navegar para Page2.xaml.

```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2));
}
```

```cppwinrt
void Page1::HyperlinkButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args)
{
    Frame().Navigate(winrt::xaml_typename<NavApp1::Page2>());
}
```

```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid));
}
```

Em Page2.xaml, adicione o seguinte conteúdo:

-   Um elemento [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) chamado `pageTitle` como elemento filho da raiz [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid). Altere o valor da propriedade [**Text**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.text) para `Page 2`.
```xaml
<TextBlock x:Name="pageTitle" Text="Page 2" />
```

-   Um elemento [**HyperlinkButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton) como elemento filho da raiz [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) e depois o elemento `pageTitle` [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock).
```xaml
<HyperlinkButton Content="Click to go to page 1" 
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

No arquivo code-behind Page2.xaml, adicione o seguinte código para manipular o evento `Click` do [**HyperlinkButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton) adicionado para navegar para Page1.xaml.

```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page1));
}
```

```cppwinrt
void Page2::HyperlinkButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args)
{
    Frame().Navigate(winrt::xaml_typename<NavApp1::Page1>());
}
```

```cpp
void Page2::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid));
}
```

> [!NOTE]
> Para projetos em C++, você deve adicionar uma diretiva `#include` no arquivo de cabeçalho de cada página que faz referência a outra página. Para o exemplo de navegação entre páginas apresentado aqui, page1.xaml.h arquivo contém `#include "Page2.xaml.h"` e, por sua vez, page2.xaml.h contém `#include "Page1.xaml.h"`.

Agora que preparamos as páginas, precisamos fazer com que Page1.xaml seja exibida quando o aplicativo iniciar.

Abra o arquivo code-behind App.xaml e altere o manipulador `OnLaunched`.

Aqui, especificamos `Page1` na chamada para [**Frame.Navigate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate) em vez de `MainPage`.

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
 
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();
        rootFrame.NavigationFailed += OnNavigationFailed;
 
        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }
 
        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }
 
    if (rootFrame.Content == null)
    {
        // When the navigation stack isn't restored navigate to the first page,
        // configuring the new page by passing required information as a navigation
        // parameter
        rootFrame.Navigate(typeof(Page1), e.Arguments);
    }
    // Ensure the current window is active
    Window.Current.Activate();
}
```

```cppwinrt
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    Frame rootFrame{ nullptr };
    auto content = Window::Current().Content();
    if (content)
    {
        rootFrame = content.try_as<Frame>();
    }

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // Create a Frame to act as the navigation context and associate it with
        // a SuspensionManager key
        rootFrame = Frame();

        rootFrame.NavigationFailed({ this, &App::OnNavigationFailed });

        if (e.PreviousExecutionState() == ApplicationExecutionState::Terminated)
        {
            // Restore the saved session state only when appropriate, scheduling the
            // final launch steps after the restore is complete
        }

        if (e.PrelaunchActivated() == false)
        {
            if (rootFrame.Content() == nullptr)
            {
                // When the navigation stack isn't restored navigate to the first page,
                // configuring the new page by passing required information as a navigation
                // parameter
                rootFrame.Navigate(xaml_typename<NavApp1::Page1>(), box_value(e.Arguments()));
            }
            // Place the frame in the current Window
            Window::Current().Content(rootFrame);
            // Ensure the current window is active
            Window::Current().Activate();
        }
    }
    else
    {
        if (e.PrelaunchActivated() == false)
        {
            if (rootFrame.Content() == nullptr)
            {
                // When the navigation stack isn't restored navigate to the first page,
                // configuring the new page by passing required information as a navigation
                // parameter
                rootFrame.Navigate(xaml_typename<NavApp1::Page1>(), box_value(e.Arguments()));
            }
            // Ensure the current window is active
            Window::Current().Activate();
        }
    }
}
```

```cpp
void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ e)
{
    auto rootFrame = dynamic_cast<Frame^>(Window::Current->Content);

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // Create a Frame to act as the navigation context and associate it with
        // a SuspensionManager key
        rootFrame = ref new Frame();

        rootFrame->NavigationFailed += 
            ref new Windows::UI::Xaml::Navigation::NavigationFailedEventHandler(
                this, &App::OnNavigationFailed);

        if (e->PreviousExecutionState == ApplicationExecutionState::Terminated)
        {
            // TODO: Load state from previously suspended application
        }
        
        // Place the frame in the current Window
        Window::Current->Content = rootFrame;
    }

    if (rootFrame->Content == nullptr)
    {
        // When the navigation stack isn't restored navigate to the first page,
        // configuring the new page by passing required information as a navigation
        // parameter
        rootFrame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid), e->Arguments);
    }

    // Ensure the current window is active
    Window::Current->Activate();
}
```

> [!NOTE]
> O código aqui usa o valor de retorno de [**Navigate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate) para lançar uma exceção de aplicativo caso a navegação ao quadro da janela inicial do aplicativo falhe. Quando **Navigate** retorna **true**, a navegação acontece.

Agora, crie e execute o aplicativo. Clique no link que diz "Clique para ir à página 2". A segunda página, que diz "Página 2" no topo, deve ser carregada e exibida no quadro.

### <a name="about-the-frame-and-page-classes"></a>Sobre as classes Frame e Page

Antes de adicionarmos mais recursos ao nosso aplicativo, vejamos como as páginas que adicionamos oferecem navegação dentro de nosso aplicativo.

Primeiro, um [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) chamado `rootFrame` é criado para o aplicativo no método `App.OnLaunched` no arquivo code-behind App.xaml. A classe **Frame** dá suporte a vários métodos de navegação como [**Navigate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate), [**GoBack**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goback) e [ **GoForward**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goforward), e a propriedades como [**BackStack**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.backstack), [**ForwardStack**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.forwardstack) e [**BackStackDepth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.backstackdepth).
 
O método [**Navigate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate) é usado para exibir conteúdo nesse **Frame**. Por padrão, esse método carrega MainPage.xaml. No nosso exemplo, `Page1` é passada para o método **Navigate**, então o método carrega `Page1` em **Frame**. 

`Page1` é uma subclasse da classe [**Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page). A classe **Page** tem uma propriedade **Frame** somente leitura que obtém o **Frame** que contém **Page**. Quando o manipulador de eventos **Click** de **HyperlinkButton** na `Page1` chama `this.Frame.Navigate(typeof(Page2))`, o **Frame** exibe o conteúdo de Page2.xaml.

Por fim, sempre que uma página é carregada no quadro, ela é adicionada como um [**PageStackEntry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Navigation.PageStackEntry) ao [**BackStack**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.backstack) ou [**ForwardStack**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.forwardstack) do [**Frame**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.frame), permitindo [navegar para o histórico e para a página anterior](navigation-history-and-backwards-navigation.md).

## <a name="3-pass-information-between-pages"></a>3. Transmitir informações entre páginas

Nosso aplicativo navega entre duas páginas, mas ainda não faz nada de interessante. Geralmente, quando um aplicativo tem várias páginas, as páginas precisam compartilhar informações. Vamos passar algumas informações da primeira para a segunda página.

Em Page1.xaml, substitua o **HyperlinkButton** adicionado anteriormente pelo seguinte [**StackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel).

Aqui, adicionamos um rótulo [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) e um [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) `name` para inserir uma cadeia de texto.

```xaml
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Text="Enter your name"/>
    <TextBox HorizontalAlignment="Center" Width="200" Name="name"/>
    <HyperlinkButton Content="Click to go to page 2" 
                     Click="HyperlinkButton_Click"
                     HorizontalAlignment="Center"/>
</StackPanel>
```

No manipulador de evento `HyperlinkButton_Click` do arquivo code-behind Page1.xaml, adicione um parâmetro fazendo referência à propriedade `Text` de `name` **TextBox** para o método `Navigate`.

```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2), name.Text);
}
```

```cppwinrt
void Page1::HyperlinkButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args)
{
    Frame().Navigate(winrt::xaml_typename<NavApp1::Page2>(), winrt::box_value(name().Text()));
}
```

```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid), name->Text);
}
```

Em Page2.xaml, substitua o **HyperlinkButton** adicionado anteriormente pelo seguinte **StackPanel**.

Aqui, adicionamos um [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) para exibir a cadeia de caracteres de texto transmitida por Page1.

```xaml
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Name="greeting"/>
    <HyperlinkButton Content="Click to go to page 1" 
                     Click="HyperlinkButton_Click"
                     HorizontalAlignment="Center"/>
</StackPanel>
```

No arquivo code-behind Page2.xaml, adicione o seguinte para substituir o método `OnNavigatedTo`:

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    if (e.Parameter is string && !string.IsNullOrWhiteSpace((string)e.Parameter))
    {
        greeting.Text = $"Hi, {e.Parameter.ToString()}";
    }
    else
    {
        greeting.Text = "Hi!";
    }
    base.OnNavigatedTo(e);
}
```

```cppwinrt
void Page2::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
{
    auto propertyValue{ e.Parameter().as<Windows::Foundation::IPropertyValue>() };
    if (propertyValue.Type() == Windows::Foundation::PropertyType::String)
    {
        greeting().Text(L"Hi, " + winrt::unbox_value<winrt::hstring>(e.Parameter()));
    }
    else
    {
        greeting().Text(L"Hi!");
    }
    __super::OnNavigatedTo(e);
}
```

```cpp
void Page2::OnNavigatedTo(NavigationEventArgs^ e)
{
    if (dynamic_cast<Platform::String^>(e->Parameter) != nullptr)
    {
        greeting->Text = "Hi, " + e->Parameter->ToString();
    }
    else
    {
        greeting->Text = "Hi!";
    }
    ::Windows::UI::Xaml::Controls::Page::OnNavigatedTo(e);
}
```

Execute o aplicativo, digite o seu nome na caixa de texto e clique no link que diz **Clique para ir à página 2**. 

Quando o evento **Click** de **HyperlinkButton** na `Page1` chamar `this.Frame.Navigate(typeof(Page2), name.Text)`, a propriedade `name.Text` é passada para `Page2` e o valor dos dados do evento é usado para a mensagem exibida na página.

## <a name="4-cache-a-page"></a>4. Armazenar uma página em cache

O estado e o conteúdo da página não são armazenados em cache por padrão. Portanto, se quiser informações de cache, habilite-o em cada página do seu aplicativo.

No nosso exemplo básico de ponto a ponto, não há botão Voltar (demonstramos a navegação regressiva em [navegação regressiva](navigation-history-and-backwards-navigation.md)), mas, se você clicou em um botão Voltar em `Page2`, **TextBox** (e qualquer outro campo) em `Page1` seria definido como seu estado padrão. Uma maneira de contornar isso é usar a propriedade [**NavigationCacheMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.navigationcachemode) para especificar que uma página foi adicionada ao cache de página do quadro. 

No construtor da `Page1`, é possível definir o **NavigationCacheMode** como **Enabled** para reter todos os valores de estado e conteúdo da página até que o cache de página para o quadro seja excedido. Defina [**NavigationCacheMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.navigationcachemode) como [**Required**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Navigation.NavigationCacheMode) se quiser ignorar os limites de [**CacheSize**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.cachesize), que especificam o número de páginas no histórico de navegação que podem ser armazenadas em cache para o quadro. No entanto, lembre-se que os limites de tamanho de cache podem ser cruciais, dependendo dos limites de memória de um dispositivo.

```csharp
public Page1()
{
    this.InitializeComponent();
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
}
```

```cppwinrt
Page1::Page1()
{
    InitializeComponent();
    NavigationCacheMode(Windows::UI::Xaml::Navigation::NavigationCacheMode::Enabled);
}
```

```cpp
Page1::Page1()
{
    this->InitializeComponent();
    this->NavigationCacheMode = Windows::UI::Xaml::Navigation::NavigationCacheMode::Enabled;
}
```

## <a name="related-articles"></a>Artigos relacionados
* [Noções básicas de design de navegação para aplicativos do Windows](https://docs.microsoft.com/windows/uwp/layout/navigation-basics)
* [Pivô](../controls-and-patterns/pivot.md)
* [Modo de exibição de navegação](../controls-and-patterns/navigationview.md)
