---
Description: Personalize a exibição interna de manuscrito para entrada tinta em texto que é compatível com controles de texto UWP, como TextBox, RichEditBox (e controles como o AutoSuggestBox, que fornecem uma experiência de entrada de texto semelhantes).
title: Entrada de texto com exibição de texto manuscrito
label: Text input with the handwriting view
template: detail.hbs
ms.date: 10/13/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: sewen
design-contact: minah.kim
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f7b31898e6a90410e4edc73ee36f71a7e4d94155
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63774766"
---
# <a name="text-input-with-the-handwriting-view"></a>Entrada de texto com exibição de texto manuscrito

![Caixa de texto se expande quando tocada com a caneta](images/handwritingview/handwritingview2.gif)

Personalize a exibição interna de manuscrito para entrada tinta em texto que é compatível com controles de texto UWP, como [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox), [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox), e controles derivados destes, como [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox).

## <a name="overview"></a>Visão geral

Caixas de entrada de texto XAML têm suporte inserido para entrada à caneta usando [Windows Ink](../input/pen-and-stylus-interactions.md). Quando um usuário toca em uma caixa de entrada de texto usando uma caneta do Windows, a caixa de texto se transforma em uma superfície para manuscrito, em vez de abrir um painel de entrada separado.

O texto é reconhecido à medida que o usuário escreve em qualquer lugar na caixa de texto e uma janela de candidatos mostra os resultados do reconhecimento. O usuário pode tocar em um resultado para escolhê-lo ou continuar escrevendo para aceitar o candidato proposto. Os resultados do reconhecimento literal (letra pela letra) estão incluídos na janela de candidatos, para que o reconhecimento não fique restrito a palavras em um dicionário. Conforme o usuário escreve, a entrada de texto aceita é convertida em uma fonte de script que mantém a sensação de escrita.

> [!NOTE]
> A exibição de texto manuscrito é habilitada por padrão, mas você pode desabilitá-la por controle e reverter para o painel de entrada de texto.

![Caixa de texto com tinta e sugestões](images/handwritingview/handwritingview-inksuggestion1.gif)

Um usuário pode editar o texto usando gestos e ações padrão, como estes:

- _tachado_ ou _rabisco_ – desenhar para excluir uma palavra ou parte de uma palavra
- _unir_ – desenhar um arco entre as palavras para excluir o espaço entre elas
- _inserir_ – desenhar um símbolo de acento circunflexo para inserir um espaço
- _substituir_ – escrever sobre o texto existente para substituí-lo

![Caixa de texto com a correção de tinta](images/handwritingview/handwritingview-inkcorrection1.gif)

## <a name="disable-the-handwriting-view"></a>Desabilitar o modo de exibição de manuscrito

O modo de exibição interno de manuscrito é habilitado por padrão.

Talvez você queira desabilitar o modo de exibição de manuscrito se você já fornece funcionalidade equivalente de tinta em texto em seu aplicativo ou sua experiência de entrada de texto se baseia em algum tipo de formatação ou caractere especial (como uma guia) não disponível por meio de manuscrito.

Neste exemplo, podemos desabilitar o modo de exibição de manuscrito definindo a propriedade [IsHandwritingViewEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.ishandwritingviewenabled) do controle [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) como false. Todos os controles de texto que dão suporte à exibição de texto manuscrito oferecem suporte a uma propriedade semelhante.

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen" 
    IsHandwritingViewEnabled="False">
</TextBox>
```

## <a name="specify-the-alignment-of-the-handwriting-view"></a>Especificar o alinhamento do modo de exibição de manuscrito

O modo de exibição de manuscrito localizado acima do controle de texto subjacente e dimensionado para acomodar as preferências de manuscrito do usuário (confira **Configurações -> Dispositivos -> Caneta e Windows Ink -> Manuscrito -> Tamanho da fonte ao escrever diretamente no campo de texto**). O modo de exibição também é alinhado automaticamente em relação ao controle de texto e sua localização dentro do aplicativo.

A interface do usuário do aplicativo não reflui para acomodar o controle maior, de modo que o sistema pode fazer com que o modo de exibição obstrua uma interface do usuário importante.

Aqui, mostramos como usar a propriedade [PlacementAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.placementalignment) de um [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) para especificar qual âncora no controle de texto subjacente é usada para alinhar o modo de exibição de manuscrito.

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen">
        <TextBox.HandwritingView>
            <HandwritingView PlacementAlignment="TopLeft"/>
        </TextBox.HandwritingView>
</TextBox>
```

## <a name="disable-auto-completion-candidates"></a>Desabilitar candidatos de preenchimento automático

O pop-up de sugestão de texto está habilitado por padrão para fornecer uma lista de candidatos de reconhecimento de tinta principais da qual o usuário pode selecionar caso o candidato superior esteja incorreto.

Se o aplicativo já fornece a funcionalidade de reconhecimento robusta e personalizada, você pode usar a propriedade [AreCandidatesEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.arecandidatesenabled) para desabilitar as sugestões internas, conforme mostrado no exemplo a seguir.

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen">
        <TextBox.HandwritingView>
            <HandwritingView AreCandidatesEnabled="False"/>
        </TextBox.HandwritingView>
</TextBox>
```

## <a name="use-handwriting-font-preferences"></a>Usar as preferências de fonte de manuscrito

Um usuário pode escolher de uma coleção predefinida de fontes com base em texto manuscrito para usar ao renderizar texto com base no reconhecimento de tinta (confira **Configurações -> Dispositivos -> Caneta e Windows Ink -> Manuscrito -> Fonte ao usar o manuscrito**).

> [!NOTE]
> Os usuários podem até mesmo criar uma fonte com base em seu próprio manuscrito.
> [!VIDEO https://www.youtube.com/embed/YRR4qd4HCw8]

Seu aplicativo pode acessar essa configuração e usar a fonte selecionada para o texto reconhecido no controle de texto.

Neste exemplo, podemos escutar o evento [TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged) de um [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) e aplicar a fonte de selecionada do usuário se a alteração de texto se originou do HandwritingView (ou uma fonte padrão, caso contrário).

```csharp
private void SampleTextBox_TextChanged(object sender, TextChangedEventArgs e)
{
    ((TextBox)sender).FontFamily = 
        ((TextBox)sender).HandwritingView.IsOpen ?
            new FontFamily(PenAndInkSettings.GetDefault().FontFamilyName) : 
            new FontFamily("Segoe UI");
}
```

## <a name="access-the-handwritingview-in-composite-controls"></a>Acessar HandwritingView em controles compostos

Controles compostos que usam os controles [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) ou [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox), como [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox), também dão suporte a [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview).

Para acessar o [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) em um controle composto, use a API [VisualTreeHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper).

O snippet XAML a seguir exibe um controle [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox).

```xaml
<AutoSuggestBox Name="SampleAutoSuggestBox" 
    Height="50" Width="500" 
    PlaceholderText="Auto Suggest Example" 
    FontSize="16" FontFamily="Segoe UI"  
    Loaded="SampleAutoSuggestBox_Loaded">
</AutoSuggestBox>
```

No code-behind correspondente, mostramos como desabilitar o [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) no [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox).

1. Primeiro, tratamos o evento Loaded do aplicativo, no qual podemos chamar uma função FindInnerTextBox para iniciar a passagem da árvore visual.

    ```csharp
    private void SampleAutoSuggestBox_Loaded(object sender, RoutedEventArgs e)
    {
        if (FindInnerTextBox((AutoSuggestBox)sender))
            autoSuggestInnerTextBox.IsHandwritingViewEnabled = false;
    }
    ```

2. Em seguida, começamos a iterar na árvore visual (começando em um AutoSuggestBox) na função FindInnerTextBox com uma chamada para FindVisualChildByName.

    ```csharp
    private bool FindInnerTextBox(AutoSuggestBox autoSuggestBox)
    {
        if (autoSuggestInnerTextBox == null)
        {
            // Cache textbox to avoid multiple tree traversals. 
            autoSuggestInnerTextBox = 
                (TextBox)FindVisualChildByName<TextBox>(autoSuggestBox);
        }
        return (autoSuggestInnerTextBox != null);
    }
    ```

3. Por fim, essa função itera na árvore visual até que TextBox seja recuperado.

    ```csharp
    private FrameworkElement FindVisualChildByName<T>(DependencyObject obj)
    {
        FrameworkElement element = null;
        int childrenCount = 
            VisualTreeHelper.GetChildrenCount(obj);
        for (int i = 0; (i < childrenCount) && (element == null); i++)
        {
            FrameworkElement child = 
                (FrameworkElement)VisualTreeHelper.GetChild(obj, i);
            if ((child.GetType()).Equals(typeof(T)) || (child.GetType().GetTypeInfo().IsSubclassOf(typeof(T))))
            {
                element = child;
            }
            else
            {
                element = FindVisualChildByName<T>(child);
            }
        }
        return (element);
    }
    ```

## <a name="reposition-the-handwritingview"></a>Reposicionar o HandwritingView

Em alguns casos, talvez seja necessário garantir que o [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) abranja elementos de interface do usuário que, de outra forma, não faria.

Aqui, podemos criar um TextBox que dá suporte a ditado (implementado colocando um TextBox e um botão de ditado em um StackPanel).

![TextBox com ditado](images/handwritingview/textbox-with-dictation.png)

Como o StackPanel agora é maior do que o TextBox, o [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) talvez não obstrua todos os controles compostos.

![TextBox com ditado](images/handwritingview/textbox-with-dictation-handwritingview.png)

Para resolver isso, defina a propriedade PlacementTarget do [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) como o elemento de interface do usuário para o qual deve ser alinhada.

```xaml
<StackPanel Name="DictationBox" 
    Orientation="Horizontal" 
    VerticalAlignment="Top" 
    HorizontalAlignment="Left" 
    BorderThickness="1" BorderBrush="DarkGray" 
    Height="55" Width="500" Margin="50">
    <TextBox Name="DictationTextBox" 
        Width="450" BorderThickness="0" 
        FontSize="24" VerticalAlignment="Center">
        <TextBox.HandwritingView>
            <HandwritingView PlacementTarget="{Binding ElementName=DictationBox}"/>
        </TextBox.HandwritingView>
    </TextBox>
    <Button Name="DictationButton" 
        Height="48" Width="48" 
        FontSize="24" 
        FontFamily="Segoe MDL2 Assets" 
        Content="&#xE720;" 
        Background="White" Foreground="DarkGray"     Tapped="DictationButton_Tapped" />
</StackPanel>
```

## <a name="resize-the-handwritingview"></a>Redimensionar o HandwritingView

Você também pode definir o tamanho do [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview), que pode ser útil quando você precisa garantir que o modo de exibição não obstrua uma interface do usuário importante.

Assim como no exemplo anterior, podemos criar um TextBox que dá suporte a ditado (implementado colocando um TextBox e um botão de ditado em um StackPanel).

![TextBox com ditado](images/handwritingview/textbox-with-dictation.png)

Nesse caso, queremos garantir que o botão de ditado esteja sempre visível.

![TextBox com ditado](images/handwritingview/textbox-with-dictation-handwritingview-resize.png)

Para fazer isso, podemos associar a propriedade MaxWidth do [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) com a largura do elemento da interface do usuário que ele deve obstruir.

```xaml
<StackPanel Name="DictationBox" 
    Orientation="Horizontal" 
    VerticalAlignment="Top" 
    HorizontalAlignment="Left" 
    BorderThickness="1" 
    BorderBrush="DarkGray" 
    Height="55" Width="500" 
    Margin="50">
    <TextBox Name="DictationTextBox" 
        Width="450" 
        BorderThickness="0" 
        FontSize="24" 
        VerticalAlignment="Center">
        <TextBox.HandwritingView>
            <HandwritingView 
                PlacementTarget="{Binding ElementName=DictationBox}“
                MaxWidth="{Binding ElementName=DictationTextBox, Path=Width"/>
        </TextBox.HandwritingView>
    </TextBox>
    <Button Name="DictationButton" 
        Height="48" Width="48" 
        FontSize="24" 
        FontFamily="Segoe MDL2 Assets" 
        Content="&#xE720;" 
        Background="White" Foreground="DarkGray" 
        Tapped="DictationButton_Tapped" />
</StackPanel>
```

## <a name="reposition-custom-ui"></a>Reposicionar a interface do usuário personalizada

Se você tiver interface do usuário personalizada que aparece em resposta à entrada de texto, como um pop-up informativo, talvez seja necessário reposicionar a interface do usuário para que ela não obstrua o modo de exibição de manuscrito.

![TextBox com a interface do usuário personalizada](images/handwritingview/textbox-with-customui.png)

O exemplo a seguir mostra como escutar eventos [Opened](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.opened), [Closed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.closed
) e [SizeChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) do [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) para definir o posição de um [pop-up](https://docs.microsoft.com/uwp/api/windows.ui.popups).

```csharp
private void Search_HandwritingViewOpened(
    HandwritingView sender, HandwritingPanelOpenedEventArgs args)
{
    UpdatePopupPositionForHandwritingView();
}

private void Search_HandwritingViewClosed(
    HandwritingView sender, HandwritingPanelClosedEventArgs args)
{
    UpdatePopupPositionForHandwritingView();
}

private void Search_HandwritingViewSizeChanged(
    object sender, SizeChangedEventArgs e)
{
    UpdatePopupPositionForHandwritingView();
}

private void UpdatePopupPositionForHandwritingView()
{
if (CustomSuggestionUI.IsOpen)
    CustomSuggestionUI.VerticalOffset = GetPopupVerticalOffset();
}

private double GetPopupVerticalOffset()
{
    if (SearchTextBox.HandwritingView.IsOpen)
        return (SearchTextBox.Margin.Top + SearchTextBox.HandwritingView.ActualHeight);
    else
        return (SearchTextBox.Margin.Top + SearchTextBox.ActualHeight);    
}
```

## <a name="retemplate-the-handwritingview-control"></a>Remodelar o controle HandwritingView

Assim como com todos os controles de estrutura XAML, você pode personalizar a estrutura e o comportamento visuais de um [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) para suas necessidades específicas.

Para ver um exemplo completo de criação de um modelo personalizado, confira a instrução [Criar controles personalizados de transporte](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/custom-transport-controls) ou [Exemplo de controle de edição personalizada](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl).








