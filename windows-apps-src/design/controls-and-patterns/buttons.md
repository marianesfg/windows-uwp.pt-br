---
author: serenaz
Description: A button gives the user a way to trigger an immediate action.
title: Botões
label: Buttons
template: detail.hbs
ms.author: sezhen
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
ms.localizationpriority: medium
ms.openlocfilehash: f663ce9da6453922e35f850a8cd039f33770f434
ms.sourcegitcommit: 4b522af988273946414a04fbbd1d7fde40f8ba5e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2018
ms.locfileid: "1494163"
---
# <a name="buttons"></a>Botões


Um botão dá ao usuário uma forma de acionar uma ação imediata.

> **APIs importantes**: [classe Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx), [classe RepeatButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx), [evento Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx)

![Exemplo de botões](images/controls/button.png)

## <a name="is-this-the-right-control"></a>Este é o controle correto?

Um botão permite que o usuário inicie uma ação imediata, como enviar um formulário.

Não use um botão quando a ação é de navegar para outra página, use um link. Para saber mais, consulte [Hiperlinks](hyperlinks.md).
> Exceção: na navegação do assistente, use botões rotulados como "Voltar" e "Próximo". Para outros tipos de navegação regressiva ou para um nível superior, use o botão Voltar.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/Button">abrir o aplicativo e ver o Botão em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Baixe o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obter o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

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

Você pode alterar a forma como um botão aciona o evento Click alterando a propriedade [ClickMode](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.clickmode). O valor ClickMode padrão é **Release**, mas você também pode definir ClickMode de um botão como **Passar o mouse sobre** ou **Pressionar**. Se ClickMode for **Passar o mouse sobre**, o evento Click não poderá ser chamado com o teclado ou o toque.


### <a name="button-content"></a>Conteúdo do botão

Botão é um [ContentControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.aspx). Sua propriedade de conteúdo XAML é [Content](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx), que habilita uma sintaxe semelhante para XAML: `<Button>A button's content</Button>`. Você pode definir qualquer objeto como conteúdo do botão. Se o conteúdo for um [UIElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.aspx), ele é renderizado no botão. Se o conteúdo for outro tipo de objeto, a representação da cadeia de caracteres é exibida no botão.

Geralmente, o conteúdo de um botão é texto. Veja as recomendações de design para botões com conteúdo de texto:
-   Utilize um texto conciso, específico e autoexplicativo que descreva claramente a ação executada pelo botão. Geralmente, o conteúdo de texto do botão é uma única palavra, um verbo.
-   Use a fonte padrão a menos que suas diretrizes de marca digam para usar algo diferente.
-   Para texto mais curto, evite botões de comando estreitos usando uma largura mínima do botão de 120px.
- Para texto mais longo, evite botões de comando largos limitando o texto a um tamanho máximo de 26 caracteres.
-   Se o conteúdo em texto de um botão for dinâmico (isto é, [localizado](../globalizing/globalizing-portal.md)), considere como o botão será redimensionado e o que acontecerá para controles em volta dele.

<table>
<tr>
<td> <b>Você precisa corrigir:</b><br> Botões com estouro de texto. </td>
<td> <img src="images/button-wraptext.png"/> </td>
</tr>
<tr>
<td> <b>Opção 1:</b><br> Aumente a largura do botão, empilhe botões e delimite se o comprimento do texto for maior que 26 caracteres. </td>
<td> <img src="images/button-wraptext1.png"> </td>
</tr>
<tr>
<td> <b>Opção 2:</b><br> Aumente a altura do botão e delimite o texto. </td>
<td> <img src="images/button-wraptext2.png"> </td>
</tr>
</table>

Você também pode personalizar elementos visuais que compõem a aparência do botão. Por exemplo, você poderia substituir o texto por um ícone ou usar um ícone mais texto.

Aqui, um **StackPanel** que contém uma imagem e o texto está definido como conteúdo de um botão.

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
-   Quando houver vários botões para a mesma decisão (como em uma caixa de diálogo de confirmação), apresente os botões de confirmação nesta ordem, onde [Faça isso] e [Não faça isso] são respostas específicas para a instrução principal:
    -   OK/[Faça]/Sim
    -   [Não faça]/Não
    -   Cancelar
-   Exponha apenas um ou dois botões de cada vez para o usuário, por exemplo, Aceitar e Cancelar. Se precisar expor mais ações para o usuário, considere usar [checkboxes](checkbox.md) or [radio buttons](radio-button.md), de onde o usuário pode selecionar ações com apenas um botão de comando para acionar tais ações.
-   Para uma ação que precise ser avaliada em múltiplas páginas em seu aplicativo, em vez de duplicar um botão em múltiplas páginas, considere usar uma [barra inferior do aplicativo](app-bars.md).

### <a name="recommended-single-button-layout"></a>Layout de botão único recomendado

Se o layout requer apenas um botão, ele deve ser alinhado à esquerda ou à direita com base no contexto de contêiner.

-   As caixas de diálogo com apenas um botão devem **alinhar o botão à direita**. Se a caixa de diálogo contém apenas um botão, verifique se o botão executa a ação segura e não destrutiva. Se você usar [ContentDialog](dialogs.md) e especificar um único botão, ele será automaticamente para alinhar à direita.

![Um botão dentro de uma caixa de diálogo](images/pushbutton_doc_dialog.png)

-   Se o botão é exibido dentro de um contêiner da interface do usuário (por exemplo, em uma notificação do sistema, um submenu ou um item de modo de exibição de lista), você deve **alinhar o botão à direita** no contêiner.

![Um botão em um contêiner](images/pushbutton_doc_container.png)

-   Nas páginas com um único botão (por exemplo, um botão de "Aplicação" na parte inferior de uma página de configurações), você deve **alinhar o botão à esquerda**. Isso garante que o botão esteja alinhado com o restante do conteúdo da página.

![Um botão em uma página](images/pushbutton_doc_page.png)

## <a name="back-buttons"></a>Botões Voltar

O botão Voltar é um elemento de interface do usuário fornecida pelo sistema que permite a navegação regressiva através da pilha Voltar ou do histórico de navegação de um usuário. Você não precisa criar seu próprio botão Voltar, mas talvez precise trabalhar um pouco para permitir uma boa experiência de navegação regressiva. Para obter mais informações, consulte [Histórico e navegação regressiva](../basics/navigation-history-and-backwards-navigation.md)

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Amostra do XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Veja todos os controles XAML em um formato interativo.


## <a name="related-articles"></a>Artigos relacionados
- [Classe Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)
- [Botões de opção](radio-button.md)
- [Caixas de seleção](checkbox.md)
- [Chaves de alternância](toggles.md)
