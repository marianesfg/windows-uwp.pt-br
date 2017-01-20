---
author: Jwmsft
Description: "Saiba como navegar em um aplicativo básico de duas páginas da Plataforma Universal do Windows (UWP) passo a passo."
title: "Navegação ponto a ponto entre duas páginas"
ms.assetid: 0A364C8B-715F-4407-9426-92267E8FB525
label: Peer-to-peer navigation between two pages
template: detail.hbs
op-migration-status: ready
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: 324bdcd8ae61826a2be765f6a6a93441036d6984

---

# <a name="peer-to-peer-navigation-between-two-pages"></a>Navegação ponto a ponto entre duas páginas

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Saiba como navegar em um aplicativo básico de duas páginas da Plataforma Universal do Windows (UWP) passo a passo.

![exemplo de navegação passo a passo de duas páginas](images/nav-peertopeer-2page.png)

<div class="important-apis" >
<b>APIs Importantes</b><br/>
<ul>
<li>[**Windows.UI.Xaml.Controls.Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)</li>
<li>[**Windows.UI.Xaml.Controls.Page**](https://msdn.microsoft.com/library/windows/apps/br227503)</li>
<li>[**Windows.UI.Xaml.Navigation**](https://msdn.microsoft.com/library/windows/apps/br243300)</li>
</ul>
</div>



## <a name="create-the-blank-app"></a>Criar o aplicativo em branco


1.  No menu do Microsoft Visual Studio, escolha **Arquivo &gt; Novo projeto**.
2.  No painel esquerdo da caixa de diálogo **Novo Projeto**, escolha o nó **Visual C# -&gt; Windows -&gt; Universal** ou o nó **Visual C++ -&gt; Windows -&gt; Universal**.
3.  No painel central, selecione **Aplicativo em Branco**.
4.  Na caixa **Nome**, insira **NavApp1** e selecione o botão **OK**.

    A solução é criada e os arquivos de projeto aparecem em **Gerenciador de soluções**.

5.  Para executar o programa, escolha **Depurar** &gt; **Iniciar Depuração** no menu ou pressione F5.

    Uma página em branco é exibida.

6.  Pressione Shift+F5 para parar de depurar e retorne ao Visual Studio.

## <a name="add-basic-pages"></a>Adicionar páginas básicas

Em seguida, adicione duas páginas de conteúdo ao projeto.

Execute as seguintes etapas duas vezes para adicionar duas páginas para navegar entre elas.

1.  Em **Gerenciador de soluções**, clique com botão direito no nó de projeto **BlankApp** para abrir o menu de atalho.
2.  Escolha **Adicionar** &gt; **Novo Item** no menu de atalho.
3.  Na caixa de diálogo **Adicionar novo item**, selecione **Página em branco** no painel do meio.
4.  Na caixa **Nome**, insira **Page1** (ou **Page2**) e pressione o botão **Adicionar**.

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

 

Adicione o conteúdo a seguir à IU de Page1.xaml.

-   Adicione um elemento [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) chamado `pageTitle` como elemento filho da raiz [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704). Altere a propriedade [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) para `Page 1`.

```xaml
<TextBlock x:Name="pageTitle" Text="Page 1" />
```

-   Adicione o seguinte elemento [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) como elemento filho do elemento raiz [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) and after the `pageTitle` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652).

    
```xaml
<HyperlinkButton Content="Click to go to page 2"
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

Adicione o código a seguir à classe `Page1` no arquivo de code-behind Page1.xaml para manipular o evento `Click` do [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) que você adicionou anteriormente. Aqui, navegamos para Page2.xaml.

> [!div class="tabbedCodeSnippets"]
```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2));
}
```
```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid));
}
```

Faça as seguintes alterações à IU de Page2.xaml.

-   Adicione um elemento [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) chamado `pageTitle` como elemento filho da raiz [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704). Altere o valor da propriedade [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) para `Page 2`.

```xaml
<TextBlock x:Name="pageTitle" Text="Page 2" />
```

-   Adicione o seguinte elemento [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) como elemento filho do elemento raiz [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) and after the `pageTitle` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652).

```xaml
<HyperlinkButton Content="Click to go to page 1" 
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

Adicione o código a seguir à classe `Page2` no arquivo de code-behind Page2.xaml para manipular o evento `Click` do [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) que você adicionou anteriormente. Aqui, navegamos para Page1.xaml.

> [!NOTE]
> Para projetos em C++, você deve adicionar uma diretiva `#include` no arquivo de cabeçalho de cada página que faz referência a outra página. Para o exemplo de navegação entre páginas apresentado aqui, page1.xaml.h arquivo contém `#include "Page2.xaml.h"` e, por sua vez, page2.xaml.h contém `#include "Page1.xaml.h"`.

> [!div class="tabbedCodeSnippets"]
```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page1));
}
```
```cpp
void Page2::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid));
}
```

Agora que preparamos as páginas de conteúdo, precisamos fazer Page1.xaml ser exibida quando o aplicativo iniciar.

Abra o arquivo de code-behind app.xaml e altere o manipulador `OnLaunched`.

Aqui, especificamos `Page1` na chamada para [**Frame.Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) em vez de `MainPage`.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> protected override void OnLaunched(LaunchActivatedEventArgs e)
> {
>     Frame rootFrame = Window.Current.Content as Frame;
> 
>     // Do not repeat app initialization when the Window already has content,
>     // just ensure that the window is active
>     if (rootFrame == null)
>     {
>         // Create a Frame to act as the navigation context and navigate to the first page
>         rootFrame = new Frame();
> 
>         rootFrame.NavigationFailed += OnNavigationFailed;
> 
>         if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
>         {
>             //TODO: Load state from previously suspended application
>         }
> 
>         // Place the frame in the current Window
>         Window.Current.Content = rootFrame;
>     }
> 
>     if (rootFrame.Content == null)
>     {
>         // When the navigation stack isn&#39;t restored navigate to the first page,
>         // configuring the new page by passing required information as a navigation
>         // parameter
>         rootFrame.Navigate(typeof(Page1), e.Arguments);
>     }
>     // Ensure the current window is active
>     Window.Current.Activate();
> }
> ```
> ```cpp
> void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ e)
> {
>     auto rootFrame = dynamic_cast<Frame^>(Window::Current->Content);
> 
>     // Do not repeat app initialization when the Window already has content,
>     // just ensure that the window is active
>     if (rootFrame == nullptr)
>     {
>         // Create a Frame to act as the navigation context and associate it with
>         // a SuspensionManager key
>         rootFrame = ref new Frame();
> 
>         rootFrame->NavigationFailed += 
>             ref new Windows::UI::Xaml::Navigation::NavigationFailedEventHandler(
>                 this, &amp;App::OnNavigationFailed);
> 
>         if (e->PreviousExecutionState == ApplicationExecutionState::Terminated)
>         {
>             // TODO: Load state from previously suspended application
>         }
>         
>         // Place the frame in the current Window
>         Window::Current->Content = rootFrame;
>     }
> 
>     if (rootFrame->Content == nullptr)
>     {
>         // When the navigation stack isn&#39;t restored navigate to the first page,
>         // configuring the new page by passing required information as a navigation
>         // parameter
>         rootFrame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid), e->Arguments);
>     }
> 
>     // Ensure the current window is active
>     Window::Current->Activate();
> }
> ```

**Observação**  O código aqui usa o valor de retorno de [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) para lançar uma exceção de aplicativo caso a navegação ao quadro da janela inicial do aplicativo falhe. Quando **Navigate** retorna **true**, a navegação acontece.

Agora, crie e execute o aplicativo. Clique no link que diz "Clique para ir à página 2". A segunda página, que diz "Página 2" no topo, deve ser carregada e exibida no quadro.

## <a name="frame-and-page-classes"></a>Classes Frame e Page

Antes de adicionarmos mais funcionalidades ao nosso aplicativo, vejamos como as páginas que adicionamos oferecem suporte de navegação ao aplicativo.

Primeiro um [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) (`rootFrame`) é criado para o aplicativo no método `App.OnLaunched` do arquivo de code-behind .xaml do aplicativo. O método [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) é usado para exibir conteúdo nesse **Frame**.

**Observação**  
A classe [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) dá suporte a vários métodos de navegação como [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694), [**GoBack**](https://msdn.microsoft.com/library/windows/apps/dn996568) e [**GoForward**](https://msdn.microsoft.com/library/windows/apps/br242693), e a propriedades como [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279543), [**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/dn279547) e [**BackStackDepth**](https://msdn.microsoft.com/library/windows/apps/hh967995).

 
No nosso exemplo, `Page1` é passado ao método [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694). Esse método define o conteúdo da janela atual do aplicativo para o [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) e carrega o conteúdo da página que você especificar para o **Frame** (Page1.xaml no nosso exemplo, ou MainPage.xaml, por padrão).

`Page1` é uma subclasse da classe [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503). A classe **Page** tem uma propriedade [**Frame**](https://msdn.microsoft.com/library/windows/apps/br227504) somente para leitura que obtém o [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) que contém **Page**. Quando o manipulador de evento [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) do [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) chama` Frame.Navigate(typeof(Page2))`, o **Frame** na janela do aplicativo exibe o conteúdo de Page2.xaml.

Sempre que uma página é carregada no quadro, ela é adicionada como um [**PageStackEntry**](https://msdn.microsoft.com/library/windows/apps/dn298572) ao [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279543) ou [**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/dn279547) do [**Frame**](https://msdn.microsoft.com/library/windows/apps/br227504).

## <a name="pass-information-between-pages"></a>Transmitir informações entre páginas

Nosso aplicativo navega entre duas páginas, mas ainda não faz nada de interessante. Geralmente, quando um aplicativo tem várias páginas, as páginas precisam compartilhar informações. Vamos passar algumas informações da primeira para a segunda página.

Em Page1.xaml, substitua o [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) que você adicionou antes com o seguinte [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635).

Aqui, adicionamos um rótulo [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) e um [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) (`name`) para inserir uma cadeia de caracteres de texto.

```xaml
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Text="Enter your name"/>
    <TextBox HorizontalAlignment="Center" Width="200" Name="name"/>
    <HyperlinkButton Content="Click to go to page 2" 
                     Click="HyperlinkButton_Click"
                     HorizontalAlignment="Center"/>
</StackPanel>
```

No manipulador evento `HyperlinkButton_Click` do arquivo de code-behind Page1.xaml, adicione um parâmetro referenciando a propriedade `Text` do `name` [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) para o método `Navigate`.

> [!div class="tabbedCodeSnippets"]
```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2), name.Text);
}
```
```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid), name->Text);
}
```

No arquivo de code-behind Page2.xaml, substitua o método `OnNavigatedTo` com o seguinte:

> [!div class="tabbedCodeSnippets"]
```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    if (e.Parameter is string)
    {
        greeting.Text = "Hi, " + e.Parameter.ToString();
    }
    else
    {
        greeting.Text = "Hi!";
    }
    base.OnNavigatedTo(e);
}
```
```cpp
void Page2::OnNavigatedTo(NavigationEventArgs^ e)
{
    if (dynamic_cast<Platform::String^>(e->Parameter) != nullptr)
    {
        greeting->Text = "Hi," + e->Parameter->ToString();
    }
    else
    {
        greeting->Text = "Hi!";
    }
    ::Windows::UI::Xaml::Controls::Page::OnNavigatedTo(e);
}
```

Execute o aplicativo, digite o seu nome na caixa de texto e clique no link que diz **Clique para ir à página 2**. Quando você chamou o `this.Frame.Navigate(typeof(Page2), tb1.Text)` no evento [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) do [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739), a propriedade `name.Text` foi passada para `Page2`, e o valor dos dados do evento é usado para a mensagem exibida na página.

## <a name="cache-a-page"></a>Armazenar uma página em cache

O estado e o conteúdo da página não é armazenado em cache por padrão, você deve habilitá-lo em cada página do seu aplicativo.

No nosso exemplo básico de passo a passo, não há botão Voltar (demonstramos navegação regressiva em [Back button navigation](navigation-history-and-backwards-navigation.md)), mas se você não clicou em um botão Voltar em `Page2`, o [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) (e qualquer outro campo) em `Page1` seria definido como seu estado padrão. Uma maneira de contornar isso é usar a propriedade [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) para especificar que uma página foi adicionada ao cache de página do quadro.

No construtor de `Page1`, defina [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) para [**Habilitado**](https://msdn.microsoft.com/library/windows/apps/br243284). Isso retém todos os valores de estado e conteúdos na página até que o cache de página do quadro seja excedido.

Defina [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) como [**Required**](https://msdn.microsoft.com/library/windows/apps/br243284) se você quiser ignorar limites de tamanho de cache do quadro. No entanto, os limites de tamanho de cache podem ser cruciais, dependendo dos limites de memória de um dispositivo.

> [!NOTE]
> A propriedade [**CacheSize**](https://msdn.microsoft.com/library/windows/apps/br242683) especifica o número de páginas no histórico de navegação que podem ser armazenados em cache para o quadro.

> [!div class="tabbedCodeSnippets"]
```csharp
public Page1()
{
    this.InitializeComponent();
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
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

* [Noções básicas de design de navegação para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/dn958438)
* [Diretrizes para pivôs e guias](https://msdn.microsoft.com/library/windows/apps/dn997788)
* [Diretrizes para painéis de navegação](https://msdn.microsoft.com/library/windows/apps/dn997766)
 

 







<!--HONumber=Dec16_HO2-->


