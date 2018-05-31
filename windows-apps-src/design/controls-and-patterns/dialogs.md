---
author: mijacobs
Description: Dialogs and flyouts display transient UI elements that appear when the user requests them or when something happens that requires notification or approval.
title: Caixas de diálogo e submenus
label: Dialogs
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 9ceb698bfbe95693ff9d5785b4bea94f1ec3070c
ms.sourcegitcommit: 2470c6596d67e1f5ca26b44fad56a2f89773e9cc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
ms.locfileid: "1675373"
---
# <a name="dialogs-and-flyouts"></a>Caixas de diálogo e submenus



Caixas de diálogo e submenus são elementos transitórios da interface do usuário que aparecem quando acontece algo que requer notificação, aprovação ou informações adicionais do usuário.

> **APIs importantes**: [classe ContentDialog](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog), [classe de submenu](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout)


::: linha:::::: coluna::: **caixas de diálogo**
        
        ![Example of a dialog](images/dialogs/dialog_RS2_delete_file.png)

        Dialogs are modal UI overlays that provide contextual app information. Dialogs block interactions with the app window until being explicitly dismissed. They often request some kind of action from the user.
    :::column-end:::
    :::column::: 
        **Flyouts**

        ![Example of a flyout](images/flyout-example2.png)

        A flyout is a lightweight contextual popup that displays UI related to what the user is doing. It includes placement and sizing logic, and can be used to reveal a secondary control or show more detail about an item.

        Unlike a dialog, a flyout can be quickly dismissed by tapping or clicking somewhere outside the flyout, pressing the Escape key or Back button, resizing the app window, or changing the device's orientation.
    :::column-end:::
::: final da linha:::


## <a name="is-this-the-right-control"></a>Esse é o controle correto?

* Use caixas de diálogo para notificar os usuários sobre informações importantes ou para solicitar a confirmação ou informações adicionais antes de uma ação ser concluída.
* Não use um submenu no lugar de [tooltip](tooltips.md) ou [menu de contexto](menus.md). Use tooltip para mostrar uma breve descrição que fica oculta depois de um determinado tempo. Use um menu de contexto para ações contextuais relacionadas a um elemento da interface do usuário, como copiar e colar.  


Caixas de diálogo e submenus certificam-se de que os usuários estão cientes da informações importantes, mas eles também interrompem a experiência do usuário. Como as caixas de diálogo são modais (bloqueando), elas interrompem os usuários, impedindo que façam algo até que interajam com a caixa. Submenus fornecem uma experiência menos desagradável, mas exibir muitos submenus pode ser uma decisão traiçoeira.

Considere a importância das informações que você deseja compartilhar: é importante o suficiente para interromper o usuário? Considere também a frequência com que as informações precisam ser exibidas. Se você estiver mostrando uma caixa de diálogo ou notificação a cada poucos minutos, convém alocar espaço para essas informações na interface do usuário principal, em vez disso. Por exemplo, em um cliente de chat, em vez de mostrar um submenu sempre que um amigo entra, você pode exibir uma lista de amigos que estão online no momento e realçar amigos conforme eles entrarem.

As caixas de diálogo são usadas para confirmar uma ação (como excluir um arquivo) antes de executá-la. Se você espera que o usuário execute uma ação específica com frequência, considere fornecer uma maneira para que o usuário desfaça a ação se for um engano, em vez de forçar os usuários a confirmarem a ação toda vez.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para abrir o aplicativo para ver o <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> ou <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> em ação.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obter o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="dialogs-vs-flyouts"></a>Caixas de diálogo vs. submenus

Depois de determinar que você deseja usar uma caixa de diálogo ou menu suspenso, você precisa escolher qual usar.

Considerando que as caixas de diálogo bloqueiam interações e os submenus não, as caixas de diálogo devem ser reservadas para situações em que você deseja que o usuário solte tudo para focar em uma informação específica ou responder a uma pergunta. Submenus, por outro lado, podem ser usados quando você deseja chamar a atenção para algo, mas terá problemas se o usuário desejar ignorá-lo.

:::linha::: :::coluna:::
   <p><b>Use uma caixa de diálogo para...</b> <br/>
<ul>
<li>Expressar informações importantes que o usuário <b>deve</b> ler e confirmar antes de prosseguir. Os exemplos incluem:
<ul>
  <li>Quando a segurança do usuário pode ser comprometida</li>
  <li>Quando o usuário está prestes a alterar um ativo valioso de forma permanente</li>
  <li>Quando o usuário está prestes a excluir um ativo valioso</li>
  <li>Para confirmar uma compra no aplicativo</li>
</ul>

</li>
<li>Mensagens de erro que se aplicam ao contexto geral do aplicativo, como um erro de conectividade.</li>
<li>Perguntas, quando o aplicativo precisar fazer uma pergunta de bloqueio ao usuário, por exemplo, quando o aplicativo não puder escolher em nome do usuário. Uma pergunta de bloqueio não pode ser ignorada ou adiada e deve oferecer ao usuário opções bem-definidas.</li>
</ul>
</p>
:::column-end::: :::column::: <p><b>Use um submenu para...</b> <br/>
<ul>
<li>Coletar informações adicionais necessárias para que uma ação possa ser concluída.</li>
<li>Exibir informações que são relevantes apenas algumas vezes. Por exemplo, em um aplicativo da galeria de fotos, quando o usuário clica em uma miniatura da imagem, você pode usar um submenu para exibir uma versão grande da imagem.</li>
<li>Exibindo mais informações, como detalhes ou descrições mais longas de um item na página.</li>
</ul></p>
:::column-end::: :::row-end:::



## <a name="dialogs"></a>Caixas de diálogo
### <a name="general-guidelines"></a>Diretrizes gerais

-   Identifique claramente o problema ou o objetivo do usuário na primeira linha do texto da caixa de diálogo.
-   O título da caixa de diálogo é a principal instrução e é opcional.
    -   Use um título curto para explicar o que as pessoas precisam fazer na caixa de diálogo.
    -   Se você está usando a caixa de diálogo para passar uma mensagem simples, de erro ou fazer uma pergunta, pode omitir o título. Utilize o texto do conteúdo para passar as informações principais.
    -   O título deve estar diretamente relacionado às opções de botão.
-   O conteúdo da caixa de diálogo contém o texto descritivo e é obrigatório.
    -   Apresente a mensagem, o erro ou a pergunta de bloqueio da maneira mais simples possível.
    -   Se um título da caixa de diálogo for usado, use a área de conteúdo para fornecer mais detalhes ou definir a terminologia. Não repita o título com palavras ligeiramente diferentes.
-   Pelo menos um botão da caixa de diálogo deve aparecer.
    -   Certifique-se de que a caixa de diálogo tem pelo menos um botão correspondente para uma ação segura e não destrutiva como "Acertou!", "Fechar" ou "Cancelar". Use a API CloseButton para adicionar esse botão.
    -   Use respostas específicas para a instrução ou o conteúdo principal como texto do botão. Um exemplo é "Você permite que AppName acesse sua localização?", seguido pelos botões "Permitir" e "Bloquear". Respostas específicas podem ser entendidas mais rapidamente, resultando na tomada de decisão eficiente.
    - Verifique se o texto dos botões de ação é conciso. Cadeias de caracteres curtas permitem que o usuário faça uma escolha de modo rápido e seguro.
    - Além da ação segura e não destrutiva, você pode, opcionalmente, apresentar ao usuário um ou dois botões de ação relacionados à instrução principal. Esses botões de confirmam o ponto principal da caixa de diálogo. Use as APIs PrimaryButton e SecondaryButton para adicionar essas ações.
    - Os botões de ação devem aparecer como os botões mais à esquerda. A ação segura e não destrutiva deve aparecer como o botão mais à direita.
    - Como opção, você pode optar por diferenciar um dos três botões como botão padrão da caixa de diálogo. Use a API DefaultButton para diferenciar um dos botões.  
-   Não use caixas de diálogo para erros que são contextuais a um local específico da página, como erros de validação (em campos de senha, por exemplo). Use a própria tela do aplicativo para mostrar erros embutidos.
- Use a [classe ContentDialog](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog) para criar sua experiência de caixa de diálogo. Não use a API MessageDialog preterida.

### <a name="dialog-scenarios"></a>Cenários de caixa de diálogo
Como caixas de diálogo bloqueiam a interação do usuário e como os botões são o mecanismo principal para os usuários ignorarem a caixa de diálogo, certifique-se de que a caixa de diálogo contém pelo menos um botão de ação "segura" e não destrutiva, como "Fechar" ou "Acertou!". **Todas as caixas de diálogo devem conter pelo menos um botão de ação segura para fechar a caixa de diálogo.** Isso garante que o usuário possa fechar a caixa de diálogo sem executar uma ação.<br>![Uma caixa de diálogo de um botão](images/dialogs/dialog_RS2_one_button.png)

```csharp
private async void DisplayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog
    {
        Title = "No wifi connection",
        Content = "Check your connection and try again.",
        CloseButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

Quando as caixas de diálogo são usadas para exibir uma pergunta de bloqueio, sua caixa de diálogo deve apresentar ao usuário botões de ação relacionados à pergunta. O botão de ação "segura" e não destrutiva pode ser acompanhado por um ou dois botões de ação. Ao apresentar para o usuário várias opções, certifique-se de que os botões expliquem claramente as ações de "fazer" seguras/"não fazer" relacionadas à pergunta proposta.

![Uma caixa de diálogo de dois botões](images/dialogs/dialog_RS2_two_button.png)

```csharp
private async void DisplayLocationPromptDialog()
{
    ContentDialog locationPromptDialog = new ContentDialog
    {
        Title = "Allow AppName to access your location?",
        Content = "AppName uses this information to help you find places, connect with friends, and more.",
        CloseButtonText = "Block",
        PrimaryButtonText = "Allow"
    };

    ContentDialogResult result = await locationPromptDialog.ShowAsync();
}
```

As caixas de diálogo de três botões são usadas quando você apresenta ao usuário duas ações do tipo "fazer" e "não fazer isso". As caixas de diálogo de três botões devem ser usadas com moderação com diferenças claras entre a ação secundária e a ação segura/fechar.

![Uma caixa de diálogo de três botões](images/dialogs/dialog_RS2_three_button.png)

```csharp
private async void DisplaySubscribeDialog()
{
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it"
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

### <a name="the-three-dialog-buttons"></a>Os três botões da caixa de diálogo
ContentDialog tem três tipos diferentes de botões que você pode usar para criar uma experiência de caixa de diálogo.

- **CloseButton** - Obrigatório - Representa a ação segura e não destrutiva que permite ao usuário sair da caixa de diálogo. Aparece como o botão mais à direita.
- **PrimaryButton** - Opcional – Representa a primeira ação de "fazer". Aparece como o botão mais à esquerda.
- **SecondaryButton** - Opcional – Representa a segunda ação de "fazer". Aparece como o botão do meio.

Ao usar os botões internos eles são posicionados adequadamente, portanto, certifique-se de que eles respondam corretamente a eventos do teclado, verifique se a área de comando permanece visível mesmo quando o teclado virtual aparece e deixa a caixa de diálogo com aparência consistente a de outras caixas de diálogo.

#### <a name="closebutton"></a>CloseButton
Cada caixa de diálogo deve conter um botão de ação segura e não destrutiva que permite ao usuário sair da caixa de diálogo com confiança.

Use a API ContentDialog.CloseButton para criar esse botão. Isso permite que você crie a experiência do usuário adequado para todas as entradas incluindo mouse, teclado, toque e gamepad. Essa experiência acontecerá quando:
<ol>
    <li>O usuário clica ou toca em CloseButton </li>
    <li>O usuário pressiona o botão de voltar do sistema. </li>
    <li>O usuário pressiona o botão ESC no teclado </li>
    <li>O usuário pressiona o Gamepad B </li>
</ol>

Quando o usuário clica em um botão de caixa de diálogo, o método [ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) retorna um [ContentDialogResult](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) para informar em qual botão o usuário clica. Pressionar em CloseButton retorna ContentDialogResult.None.

#### <a name="primarybutton-and-secondarybutton"></a>PrimaryButton e SecondaryButton
Além de CloseButton, como opção, você pode apresentar ao usuário um ou dois botões de ação relacionados à instrução principal.
Aproveite PrimaryButton como a primeira ação de "fazer" e SecondaryButton como a segunda ação de "fazer". Em caixas de diálogo de três botões, o PrimaryButton geralmente representa a ação de "fazer" afirmativa, enquanto o SecondaryButton geralmente representa uma ação de "fazer" neutra ou secundária.
Por exemplo, um aplicativo pode solicitar ao usuário para assinar um serviço. O PrimaryButton como ação de "fazer" afirmativa pode hospedar o texto Assinar, enquanto o SecondaryButton como a ação de "fazer" neutra pode hospedar o texto Tentar. O CloseButton permitiria ao usuário cancelar sem executar qualquer ação.

Quando o usuário clica em PrimaryButton, o método [ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) retorna ContentDialogResult.Primary.
Quando o usuário clica em SecondaryButton, o método [ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) retorna ContentDialogResult.Secondary.

![Uma caixa de diálogo de três botões](images/dialogs/dialog_RS2_three_button.png)

#### <a name="defaultbutton"></a>DefaultButton
Como opção, você pode optar por diferenciar um dos três botões como botão padrão. A especificação do botão padrão faz resulta em:
- O botão recebe o tratamento visual de Botão de destaque
- O botão responderá à tecla ENTER automaticamente
    - Quando o usuário pressiona a tecla ENTER no teclado, o manipulador de cliques associado ao botão padrão será acionado e o ContentDialogResult retornará o valor associado ao Botão padrão
    - Se o usuário tiver colocado o Foco do teclado em um controle que manipula ENTER, o Botão padrão não responderá ao pressionar ENTER
- O botão receberá o foco automaticamente quando a Caixa de diálogo é aberta, exceto quando o conteúdo da caixa de diálogo contém a interface do usuário focalizável

Use a propriedade ContentDialog.DefaultButton para indicar o botão padrão. Por padrão, nenhum botão padrão está definido.

![Uma caixa de diálogo de três botões com um botão padrão](images/dialogs/dialog_RS2_three_button_default.png)

```csharp
private async void DisplaySubscribeDialog()
{
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it",
        DefaultButton = ContentDialogButton.Primary
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

### <a name="confirmation-dialogs-okcancel"></a>Caixas de diálogo de confirmação (OK/Cancelar)
Uma caixa de diálogo de confirmação proporciona aos usuários a oportunidade de confirmar que desejam executar uma ação. Ele podem afirmar a ação, ou optar por cancelar.  
Uma caixa de diálogo de confirmação típica tem dois botões: um botão de afirmação ("OK") e um botão Cancelar.  

<ul>
    <li>
        <p>Em geral, o botão de afirmação fica à esquerda (o botão primário) e o botão Cancelar (o botão secundário) fica à direita.</p>
         ![Uma caixa de diálogo OK/Cancelar](images/dialogs/dialog_RS2_delete_file.png)

    </li>
    <li>Conforme observado na seção de recomendações gerais, use botões com texto que identifiquem respostas específicas para a instrução ou o conteúdo principal.
    </li>
</ul>

> Algumas plataformas colocam o botão de afirmação à direita em vez de à esquerda. Por que é recomendável colocá-lo no lado esquerdo?  Se você considerar que a maioria dos usuários estão à direita e eles segurar o telefone com a mão, é realmente mais confortável pressionar o botão de afirmação quando ele está à esquerda, como o botão está mais chances de serem dentro thumb-arco do usuário. Botões no lado direito da tela exigem que o usuário receber seu polegar para dentro para uma posição mais confortável.

### <a name="create-a-dialog"></a>Criar uma caixa de diálogo
Para criar uma caixa de diálogo, use a [classe ContentDialog](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog). Você pode criar uma caixa de diálogo no código ou na marcação. Embora seja geralmente mais fácil definir os elementos de interface do usuário em XAML, no caso de uma caixa de diálogo simples, é realmente mais fácil usar apenas o código. Este exemplo cria uma caixa de diálogo para notificar o usuário que não há nenhuma conexão Wi-Fi e, em seguida, usa o método [ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) para exibi-la.

```csharp
private async void DisplayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog
    {
        Title = "No wifi connection",
        Content = "Check your connection and try again.",
        CloseButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

Quando o usuário clica em um botão de caixa de diálogo, o método [ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) retorna um [ContentDialogResult](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) para informar em qual botão o usuário clica.

A caixa de diálogo neste exemplo faz uma pergunta e usa o [ContentDialogResult](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) retornado para determinar a resposta do usuário.

```csharp
private async void DisplayDeleteFileDialog()
{
    ContentDialog deleteFileDialog = new ContentDialog
    {
        Title = "Delete file permanently?",
        Content = "If you delete this file, you won't be able to recover it. Do you want to delete it?",
        PrimaryButtonText = "Delete",
        CloseButtonText = "Cancel"
    };

    ContentDialogResult result = await deleteFileDialog.ShowAsync();

    // Delete the file if the user clicked the primary button.
    /// Otherwise, do nothing.
    if (result == ContentDialogResult.Primary)
    {
        // Delete the file.
    }
    else
    {
        // The user clicked the CLoseButton, pressed ESC, Gamepad B, or the system back button.
        // Do nothing.
    }
}
```

## <a name="flyouts"></a>Submenus
###  <a name="create-a-flyout"></a>Criar um submenu

Um submenu é um contêiner de ignorar aberto que pode mostrar uma interface do usuário arbitrária como conteúdo. Os submenus podem conter outros submenus ou menus de contexto para criar uma experiência aninhada.

![Menu de contexto aninhado em um submenu](images/flyout-nested.png)

Submenus são anexados a controles específicos. Você pode usar a propriedade [Placement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase.Placement) para especificar onde um submenu aparece: parte superior, esquerda, inferior, direita ou inteira. Se você selecionar o [Modo de posicionamento completo](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode), o aplicativo se estende o submenu e centraliza na janela do aplicativo. Alguns controles, como [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button), fornecem uma propriedade [Flyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button.Flyout) que você pode usar para associar a um submenu ou um [menu de contexto](menus.md).

Este exemplo cria um submenu simples que exibe algum texto quando o botão é pressionado.
````xaml
<Button Content="Click me">
  <Button.Flyout>
     <Flyout>
        <TextBlock Text="This is a flyout!"/>
     </Flyout>
  </Button.Flyout>
</Button>
````

Se o controle não tem uma propriedade flyout, você pode usar a propriedade [Flyoutbase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.AttachedFlyoutProperty) anexada em vez disso. Quando você fizer isso, você também precisará chamar o método [Showattachedflyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase#Windows_UI_Xaml_Controls_Primitives_FlyoutBase_ShowAttachedFlyout_Windows_UI_Xaml_FrameworkElement_) para mostrar o submenu.

Este exemplo adiciona um submenu simples a uma imagem. Quando o usuário toca na imagem, o aplicativo mostra o submenu.

````xaml
<Image Source="Assets/cliff.jpg" Width="50" Height="50"
  Margin="10" Tapped="Image_Tapped">
  <FlyoutBase.AttachedFlyout>
    <Flyout>
      <TextBlock Text="This is some text in a flyout."  />
    </Flyout>        
  </FlyoutBase.AttachedFlyout>
</Image>
````

````csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);
}
````

Os exemplos anteriores definiram seus submenus embutido. Você pode também definir um submenu como um recurso estático e, em seguida, usá-lo com vários elementos. Este exemplo cria um submenu mais complicado que exibe uma versão maior de uma imagem quando sua miniatura é tocada.

````xaml
<!-- Declare the shared flyout as a resource. -->
<Page.Resources>
    <Flyout x:Key="ImagePreviewFlyout" Placement="Right">
        <!-- The flyout's DataContext must be the Image Source
             of the image the flyout is attached to. -->
        <Image Source="{Binding Path=Source}"
            MaxHeight="400" MaxWidth="400" Stretch="Uniform"/>
    </Flyout>
</Page.Resources>
````

````xaml
<!-- Assign the flyout to each element that shares it. -->
<StackPanel>
    <Image Source="Assets/cliff.jpg" Width="50" Height="50"
           Margin="10" Tapped="Image_Tapped"
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
    <Image Source="Assets/grapes.jpg" Width="50" Height="50"
           Margin="10" Tapped="Image_Tapped"
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
    <Image Source="Assets/rainier.jpg" Width="50" Height="50"
           Margin="10" Tapped="Image_Tapped"
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
</StackPanel>
````

````csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);  
}
````

### <a name="style-a-flyout"></a>Dê estilo ao submenu
Para estilizar um submenu, modifique o [FlyoutPresenterStyle](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout.FlyoutPresenterStyle). Este exemplo mostra um parágrafo de quebra de texto e torna o bloco de texto acessível para um leitor de tela.

![Submenu acessível com disposição de texto](images/flyout-wrapping-text.png)

````xaml
<Flyout>
  <Flyout.FlyoutPresenterStyle>
    <Style TargetType="FlyoutPresenter">
      <Setter Property="ScrollViewer.HorizontalScrollMode"
          Value="Disabled"/>
      <Setter Property="ScrollViewer.HorizontalScrollBarVisibility" Value="Disabled"/>
      <Setter Property="IsTabStop" Value="True"/>
      <Setter Property="TabNavigation" Value="Cycle"/>
    </Style>
  </Flyout.FlyoutPresenterStyle>
  <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat."/>
</Flyout>
````

#### <a name="styling-flyouts-for-10-foot-experience"></a>Definição de estilo de submenus para a experiência de 3 metros

Ignore rapidamente os controles, como teclado de interceptação de submenu e foco de gamepad na interface do usuário transitória, até serem ignorados. Para fornecer uma indicação visual desse comportamento, ao ignorar rapidamente os controles de ignorar no Xbox para desenhar uma sobreposição que esmaece o contraste e visibilidade da interface do usuário fora do escopo. Esse comportamento pode ser modificado com a propriedade [`LightDismissOverlayMode`](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase.LightDismissOverlayMode). Por padrão, os submenus desenham a sobreposição de ignorar rapidamente no Xbox, mas não em outras famílias de dispositivos, mas os aplicativos podem optar por forçar a sobreposição para estar sempre **Ativada** ou **Desativada**.

![Submenu com sobreposição de esmaecimento](images/flyout-smoke.png)

```xaml
<MenuFlyout LightDismissOverlayMode="On">
```

### <a name="light-dismiss-behavior"></a>Ignorar rapidamente comportamento
Submenus podem ser fechados com uma ação de ignorar rápida, incluindo
-   Tocar fora do submenu
-   Pressionar a tecla Escape no teclado
-   Pressionar o botão de Voltar do sistema de hardware ou software
-   Pressionar o botão do gamepad B

Quando ignorar com um toque, esse gesto é normalmente absorvido e não passado para a interface do usuário abaixo. Por exemplo, se houver um botão visível por trás de um submenu aberto, o primeiro toque do usuário fecha o submenu, mas não ativa esse botão. Pressionar o botão requer um segundo toque.

Você pode alterar esse comportamento ao especificar o botão como um elemento de entrada de passagem para o submenu. O submenu fechará como resultado da ação de ignorar descrita acima e passará o evento de toque para o `OverlayInputPassThroughElement` designado. Considere adotar esse comportamento para acelerar as interações do usuário em itens com funcionalidade semelhante. Se o aplicativo tem uma coleção de favoritos e cada item na coleção inclui um submenu anexado, é razoável que os usuários possam querer interagir com diversos submenus em sucessão rápida.

[!NOTE] Tenha cuidado para não designar um sobreposição de elemento de passagem de entrada que resulta em uma ação destrutiva. Os usuários se acostumaram às ações de ignorar rápidas que não ativam a interface do usuário principal. Fechar, Excluir ou botões destrutivos semelhantes não devem ser ativados ao ignorar rapidamente para evitar o comportamento inesperado e interrupções.

No exemplo a seguir, todos os três botões dentro FavoritesBar serão ativados no primeiro toque.

````xaml
<Page>
    <Page.Resources>
        <Flyout x:Name="TravelFlyout" x:Key="TravelFlyout"
                OverlayInputPassThroughElement="{x:Bind FavoritesBar}">
            <StackPanel>
                <HyperlinkButton Content="Washington Trails Association"/>
                <HyperlinkButton Content="Washington Cascades - Go Northwest! A Travel Guide"/>  
            </StackPanel>
        </Flyout>
    </Page.Resources>

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="FavoritesBar" Orientation="Horizontal">
            <HyperlinkButton x:Name="PageLinkBtn">Bing</HyperlinkButton>  
            <Button x:Name="Folder1" Content="Travel" Flyout="{StaticResource TravelFlyout}"/>
            <Button x:Name="Folder2" Content="Entertainment" Click="Folder2_Click"/>
        </StackPanel>
        <ScrollViewer Grid.Row="1">
            <WebView x:Name="WebContent"/>
        </ScrollViewer>
    </Grid>
</Page>
````
````csharp
private void Folder2_Click(object sender, RoutedEventArgs e)
{
     Flyout flyout = new Flyout();
     flyout.OverlayInputPassThroughElement = FavoritesBar;
     ...
     flyout.ShowAt(sender as FrameworkElement);
{
````

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Amostra do XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados
- [Dicas de ferramenta](tooltips.md)
- [Menus e menu de contexto](menus.md)
- [Classe Flyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout)
- [Classe ContentDialog](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)
