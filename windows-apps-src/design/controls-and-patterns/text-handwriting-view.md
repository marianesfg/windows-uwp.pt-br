---
Description: Personalize o modo de exibição de internos de manuscrito para tinta à entrada de texto que é compatível com UWP controles de texto, como a caixa de texto, RichEditBox (e controles, como o AutoSuggestBox que fornecem uma experiência de entrada de texto semelhantes).
title: Entrada de texto com a exibição de manuscrito
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
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57634911"
---
# <a name="text-input-with-the-handwriting-view"></a>Entrada de texto com a exibição de manuscrito

![Caixa de texto se expande quando tocado com caneta](images/handwritingview/handwritingview2.gif)

Personalizar a exibição de internos de manuscrito para tinta à entrada de texto com suporte pelos controles de texto UWP, como o [caixa de texto](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox), [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox), e controles derivados desses, como o [ AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox).

## <a name="overview"></a>Visão geral

Caixas de entrada de texto XAML apresentam suporte incorporado para uso de entrada à caneta [tinta Windows](../input/pen-and-stylus-interactions.md). Quando um usuário tocar em uma caixa de entrada de texto usando uma caneta do Windows, a caixa de texto transforma em uma superfície de manuscrito, em vez de abrir um painel de entrada separado.

É o texto reconhecido como o usuário escreve em qualquer lugar na caixa de texto e um candidato a janela mostra os resultados de reconhecimento. O usuário pode tocar um resultado para escolhê-lo ou continuar escrevendo aceitar o candidato proposto. Os resultados do reconhecimento de (letra pela letra) literal estão incluídos na janela de candidatos, para que o reconhecimento não está restrito a palavras em um dicionário. Conforme o usuário escreve, a entrada de texto aceita é convertida em um script que mantém a sensação de escrita.

> [!NOTE]
> O modo de exibição de texto manuscrito é habilitado por padrão, mas você pode desabilitá-lo em uma base por controle e reverter para o painel de entrada de texto em vez disso.

![Caixa de texto com tinta e sugestões](images/handwritingview/handwritingview-inksuggestion1.gif)

Um usuário pode editar o texto usando gestos padrão e ações, como estes:

- _tachado_ ou _transitório out_ -desenhar por meio de excluir uma palavra ou parte de uma palavra
- _ingresso_ -desenhar um arco entre as palavras para excluir o espaço entre eles
- _Inserir_ -desenhar um símbolo de sinal de interpolação para inserir um espaço
- _substituir_ -substituir texto existente para substituí-lo

![Caixa de texto com a correção de tinta](images/handwritingview/handwritingview-inkcorrection1.gif)

## <a name="disable-the-handwriting-view"></a>Desabilitar o modo de exibição de manuscrito

O modo de exibição de internos de manuscrito é habilitado por padrão.

Você talvez queira desabilitar o modo de exibição de manuscrito, se você já fornece funcionalidade equivalente de tinta para texto em seu aplicativo ou sua experiência de entrada de texto se baseia em algum tipo de formatação ou caractere especial (como uma guia) não está disponível por meio de manuscrito.

Neste exemplo, podemos desabilitar o modo de exibição de manuscrito, definindo o [IsHandwritingViewEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.ishandwritingviewenabled) propriedade da [caixa de texto](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) controle como false. Todos os controles de texto que dão suporte à exibição de texto manuscrito suportam a uma propriedade semelhante.

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen" 
    IsHandwritingViewEnabled="False">
</TextBox>
```

## <a name="specify-the-alignment-of-the-handwriting-view"></a>Especificar o alinhamento do modo de exibição de manuscrito

O modo de exibição de texto manuscrito localizado acima do controle de texto subjacente e dimensionado para acomodar as preferências do usuário manuscrito (consulte **Configurações -> dispositivos -> caneta e tinta do Windows -> manuscrito -> tamanho da fonte ao escrever diretamente em campo de texto**). O modo de exibição também automaticamente é alinhado em relação ao controle de texto e sua localização dentro do aplicativo.

O interface do usuário do aplicativo não refluir para acomodar o maior controle, para que o sistema pode fazer com que o modo de exibição occlude importante da interface do usuário.

Aqui, mostramos como usar o [PlacementAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.placementalignment) propriedade de uma [caixa de texto](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) para especificar quais âncora no controle de texto subjacente é usada para alinhar o modo de exibição de manuscrito.

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

O pop-up sugestão de texto está habilitado por padrão para fornecer uma lista de tinta principais candidatos de reconhecimento do qual o usuário pode selecionar caso o candidato superior está incorreto.

Se seu aplicativo já fornece a funcionalidade de reconhecimento robusta e personalizado, você pode usar o [AreCandidatesEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.arecandidatesenabled) propriedade para desabilitar as sugestões internos, conforme mostrado no exemplo a seguir.

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

Um usuário pode escolher de uma coleção predefinida de fontes com base em texto manuscrito a ser usado ao renderizar texto com base no reconhecimento de tinta (consulte **Configurações -> dispositivos -> caneta e tinta do Windows -> manuscrito -> fonte ao usar o manuscrito**).

> [!NOTE]
> Os usuários podem até mesmo criar uma fonte com base em sua próprias manuscrito.
> [!VIDEO https://www.youtube.com/embed/YRR4qd4HCw8]

Seu aplicativo pode acessar essa configuração e usar a fonte selecionada para o texto reconhecido no controle de texto.

Neste exemplo, podemos ouvir para as [TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged) eventos de um [caixa de texto](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) e aplicar a fonte de selecionada do usuário se a alteração de texto se originou a HandwritingView (ou uma fonte padrão, se não estiver).

```csharp
private void SampleTextBox_TextChanged(object sender, TextChangedEventArgs e)
{
    ((TextBox)sender).FontFamily = 
        ((TextBox)sender).HandwritingView.IsOpen ?
            new FontFamily(PenAndInkSettings.GetDefault().FontFamilyName) : 
            new FontFamily("Segoe UI");
}
```

## <a name="access-the-handwritingview-in-composite-controls"></a>Acesso a HandwritingView em controles compostos

Controles de composição que usam o [caixa de texto](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) ou [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox) controles, como [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) também dão suporte a uma [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview).

Para acessar o [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) em um controle de composição, use o [VisualTreeHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper) API.

O trecho XAML a seguir exibe uma [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) controle.

```xaml
<AutoSuggestBox Name="SampleAutoSuggestBox" 
    Height="50" Width="500" 
    PlaceholderText="Auto Suggest Example" 
    FontSize="16" FontFamily="Segoe UI"  
    Loaded="SampleAutoSuggestBox_Loaded">
</AutoSuggestBox>
```

No code-behind correspondente, mostramos como desativar o [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) sobre o [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox).

1. Primeiro, tratamos o evento Loaded da caixa de diálogo em que podemos chamar uma função FindInnerTextBox para iniciar a passagem da árvore visual.

    ```csharp
    private void SampleAutoSuggestBox_Loaded(object sender, RoutedEventArgs e)
    {
        if (FindInnerTextBox((AutoSuggestBox)sender))
            autoSuggestInnerTextBox.IsHandwritingViewEnabled = false;
    }
    ```

2. Em seguida, começamos a iterar através da árvore visual (começando em um AutoSuggestBox) na função FindInnerTextBox com uma chamada para FindVisualChildByName.

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

3. Por fim, essa função percorre a árvore visual até que a caixa de texto é recuperada.

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

Em alguns casos, talvez seja necessário garantir que o [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) abrange elementos de interface do usuário caso contrário, pode não ser.

Aqui, podemos criar uma caixa de texto que dá suporte a ditado (implementado colocando uma caixa de texto e um botão de ditado em um StackPanel).

![Caixa de texto com ditado](images/handwritingview/textbox-with-dictation.png)

Como o StackPanel agora é maior do que a caixa de texto, o [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) talvez não occlude todas a cotnrol composto.

![Caixa de texto com ditado](images/handwritingview/textbox-with-dictation-handwritingview.png)

Para resolver isso, defina a propriedade de PlacementTarget do [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) para o elemento de interface do usuário para o qual deve ser alinhada.

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

Você também pode definir o tamanho do [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview), que pode ser útil quando você precisa garantir que o modo de exibição não occlude importante da interface do usuário.

Como no exemplo anterior, criamos uma caixa de texto que dá suporte a ditado (implementado colocando uma caixa de texto e um botão de ditado em um StackPanel).

![Caixa de texto com ditado](images/handwritingview/textbox-with-dictation.png)

Nesse caso, queremos garantir que o botão de ditado esteja sempre visível.

![Caixa de texto com ditado](images/handwritingview/textbox-with-dictation-handwritingview-resize.png)

Para fazer isso, podemos associar a propriedade MaxWidth do [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) para a largura do elemento da interface do usuário que ele deve occlude.

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

Se você tiver interface do usuário personalizada que aparece em resposta à entrada de texto, como um pop-up informativa, você precisa reposicionar a interface do usuário para que ele não occlude o modo de exibição de manuscrito.

![Caixa de texto com a interface do usuário personalizada](images/handwritingview/textbox-with-customui.png)

O exemplo a seguir mostra como detectar para o [Opened](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.opened), [Closed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.closed
), e [SizeChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) eventos do [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) para definir o posição de um [pop-up](https://docs.microsoft.com/uwp/api/windows.ui.popups).

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

## <a name="retemplate-the-handwritingview-control"></a>Retemplate controle HandwritingView

Como com todos os controles de estrutura XAML, você pode personalizar a estrutura e o comportamento visuais de um [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) para suas necessidades específicas.

Para ver um exemplo completo de criação de uma modelo personalizado Confira a [criar controles personalizados de transporte](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/custom-transport-controls) instruções ou o [exemplo de controle de edição personalizada](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl).








