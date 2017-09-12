---
author: Jwmsft
Description: "Um botão dá ao usuário uma forma de acionar uma ação imediata."
title: "Botões"
label: Buttons
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: f04d1a3c-7dcd-4bc8-9586-3396923b312e
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.openlocfilehash: 2cb15d21496c080002411849682278df7f927e41
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2017
---
# <a name="buttons"></a>Botões
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Um botão dá ao usuário uma forma de acionar uma ação imediata.

> **APIs importantes**: [classe Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx), [classe RepeatButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx), [evento Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx)

![Exemplo de botões](images/controls/button.png)

## <a name="is-this-the-right-control"></a>Este é o controle correto?

Um botão permite que o usuário inicie uma ação imediata, como enviar um formulário.

Não use um botão quando a ação é de navegar para outra página, use um link. Para saber mais, consulte [Hiperlinks](hyperlinks.md).

> Exceção: na navegação do assistente, use botões rotulados como "Voltar" e "Próximo". Para outros tipos de navegação regressiva ou para um nível superior, use o botão Voltar.

## <a name="example"></a>Exemplo

Este exemplo usa dois botões, Permitir e Bloquear, em uma caixa de diálogo solicitando acesso à localização.

![Exemplo de botões, usados em uma caixa de diálogo](images/dialogs/dialog_RS2_two_button.png)

## <a name="create-a-button"></a>Criar um botão

Este exemplo mostra um botão que responde a um clique.

Crie o botão em XAML.

```xaml
<Button Content="Subscribe" Click="SubscribeButton_Click"/>
```

Ou crie o botão em código.

```csharp
Button subscribeButton = new Button();
subscribeButton.Content = "Subscribe";
subscribeButton.Click += SubscribeButton_Click;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(subscribeButton);
```

Manipule o evento Click.

```csharp
private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
{
    // Call app specific code to subscribe to the service. For example:
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

### <a name="button-interaction"></a>Interação de botão

Quando você toca em um botão com um dedo ou uma caneta, ou pressiona o botão esquerdo do mouse enquanto o ponteiro está sobre ele, o botão gera o evento [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx). Se um botão tem foco do teclado, pressionar a tecla Enter ou a barra de espaço também aciona o evento Click.

Geralmente, não se pode manipular eventos de baixo nível [PointerPressed](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx) em um Botão porque, em vez disso, ele tem o comportamento Click. Para saber mais, consulte [Events and routed events overview](https://msdn.microsoft.com/library/windows/apps/mt185584.aspx).

Você pode alterar a forma como um botão aciona o evento Click ao alterar a propriedade [ClickMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.clickmode.aspx). O valor ClickMode padrão é **Release**. Se ClickMode for **Hover**, o evento Click não poderá ser chamado com o teclado ou o toque.


### <a name="button-content"></a>Conteúdo do botão

Botão é um [ContentControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.aspx). Sua propriedade de conteúdo XAML é [Content](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx), que habilita uma sintaxe semelhante para XAML: `<Button>A button's content</Button>`. Você pode definir qualquer objeto como conteúdo do botão. Se o conteúdo for um [UIElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.aspx), ele é renderizado no botão. Se o conteúdo for outro tipo de objeto, a representação da cadeia de caracteres é apresentada no botão.

Segue um **StackPanel** que contém a imagem de uma laranja e o texto está definido como conteúdo de um botão.

```xaml
<Button Click="Button_Click"
        Background="LightGray"
        Height="100" Width="80">
    <StackPanel>
        <Image Source="Assets/Photo.png" Height="62"/>
        <TextBlock Text="Photos" Foreground="Black"
                   HorizontalAlignment="Center"/>
    </StackPanel>
</Button>
```

O botão fica assim.

![Um botão com conteúdo de imagem e texto](images/button-orange.png)

## <a name="create-a-repeat-button"></a>Criar um botão de repetição

Um [RepeatButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx) é um botão que gera eventos [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) repetidas vezes a partir do momento em que é pressionado até ser liberado. Defina a propriedade [Delay](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.delay.aspx) para especificar o tempo que o RepeatButton aguarda após ser pressionado antes de começar a repetir a ação de clique. Defina a propriedade [Interval](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.interval.aspx) para especificar o tempo entre as repetições da ação de clique. O tempo para as duas propriedades são especificados em milissegundos.

O exemplo a seguir mostra dois controles RepeatButton cujos respectivos eventos Click são usados para aumentar e diminuir o valor mostrado em um bloco de texto.

```xaml
<StackPanel>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Increase_Click">Increase</RepeatButton>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Decrease_Click">Decrease</RepeatButton>
    <TextBlock x:Name="clickTextBlock" Text="Number of Clicks:" />
</StackPanel>
```

```csharp
private static int _clicks = 0;
private void Increase_Click(object sender, RoutedEventArgs e)
{
    _clicks += 1;
    clickTextBlock.Text = "Number of Clicks: " + _clicks;
}

private void Decrease_Click(object sender, RoutedEventArgs e)
{
    if(_clicks > 0)
    {
        _clicks -= 1;
        clickTextBlock.Text = "Number of Clicks: " + _clicks;
    }
}
```

## <a name="recommendations"></a>Recomendações

-   Assegure que a finalidade e o estado de um botão estejam claros para o usuário.
-   Utilize um texto conciso, específico e autoexplicativo que descreva claramente a ação executada pelo botão. Geralmente, o conteúdo de texto do botão é uma única palavra, um verbo.
-   Quando houver vários botões para a mesma decisão (como em uma caixa de diálogo de confirmação), apresente os botões de confirmação nesta ordem:
    -   OK/[Faça]/Sim
    -   [Não faça]/Não
    -   Cancelar

    (onde [Faça] e [Não faça] são respostas específicas à instrução principal.)

-   Se o conteúdo em texto de um botão for dinâmico, por exemplo, estiver localizado, considere como o botão redimensionará e o que acontecerá para controles em volta dele.
-   Para botões de comando com conteúdo de texto, use a largura mínima do botão.
-   Não use botões estreitos, baixos ou altos com conteúdos em texto.
-   Use a fonte padrão a menos que suas diretrizes de marca digam para usar algo diferente.
-   Para uma ação que precise ser avaliada em múltiplas páginas em seu aplicativo, em vez de duplicar um botão em múltiplas páginas, considere usar uma [barra inferior do aplicativo](app-bars.md).
-   Exponha apenas um ou dois botões de cada vez para o usuário, por exemplo, Aceitar e Cancelar. Se precisar expor mais ações para o usuário, considere usar [checkboxes](checkbox.md) or [radio buttons](radio-button.md), de onde o usuário pode selecionar ações com apenas um botão de comando para acionar tais ações.
-   Use o botão de comando padrão para indicar a ação mais comum ou recomendada.
-   Considere personalizar seus botões. O formato de um botão é retangular por padrão, mas você pode customizar os recursos visuais que criam a aparência do botão. O conteúdo de um botão costuma ser texto—por exemplo, Aceitar ou Cancelar—mas você pode substituir o texto por um ícone ou usar ícone e texto.
-   Certifique-se de que conforme o usuário interaja com o botão, o botão mude de estado e aparência para fornecer um comentário ao usuário. Normal, pressionado e desativado são exemplos de estados do botão.
-   Acione os botões quando o usuário tocar ou pressionar o botão. Geralmente, a ação é disparada quando o usuário libera o botão, mas você também pode definir a ação de um botão para disparar assim que um dedo o pressiona.
-   Não altere os estilos padrão de enviar, redefinir e botão.
-   Não coloque conteúdo em excesso dentro de um botão. Faça o conteúdo ser conciso e fácil de entender.

### <a name="recommended-single-button-layout"></a>Layout de botão único recomendado

Se o layout requer apenas um botão, ele deve ser alinhado à esquerda ou à direita com base no contexto de contêiner.

-   As caixas de diálogo com apenas um botão devem **alinhar o botão à direita**. Se a caixa de diálogo contém apenas um botão, verifique se o botão executa a ação segura e não destrutiva. Se você usar [ContentDialog](dialogs.md) e especificar um único botão, ele será automaticamente para alinhar à direita.

![Um botão dentro de uma caixa de diálogo](images/pushbutton_doc_dialog.png)

-   Se o botão é exibido dentro de um contêiner da interface do usuário (por exemplo, em uma notificação do sistema, um submenu ou um item de modo de exibição de lista), você deve **alinhar o botão à direita** no contêiner.

![Um botão em um contêiner](images/pushbutton_doc_container.png)

-   Nas páginas com um único botão (por exemplo, um botão de "Aplicação" na parte inferior de uma página de configurações), você deve **alinhar o botão à esquerda**. Isso garante que o botão esteja alinhado com o restante do conteúdo da página.

![Um botão em uma página](images/pushbutton_doc_page.png)

## <a name="back-buttons"></a>Botões Voltar

O botão Voltar é um elemento de interface do usuário fornecida pelo sistema que permite a navegação regressiva através da pilha Voltar ou do histórico de navegação de um usuário. Você não precisa criar seu próprio botão Voltar, mas talvez precise trabalhar um pouco para permitir uma boa experiência de navegação regressiva. Para obter mais informações, consulte [Histórico e navegação regressiva](../layout/navigation-history-and-backwards-navigation.md)

## <a name="get-the-sample-code"></a>Obter o código de exemplo
*   [Amostra de noções básicas de interface do usuário XAML](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)<br/>
    Veja todos os controles XAML em um formato interativo.


## <a name="related-articles"></a>Artigos relacionados

- [Botões de opção](radio-button.md)
- [Switches de alternância](toggles.md)
- [Caixas de seleção](checkbox.md)
- [Classe Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)
