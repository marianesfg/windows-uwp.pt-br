---
author: Karl-Bridge-Microsoft
Description: Customize the built-in handwriting view for ink to text input that is supported by UWP text controls such as the TextBox, RichEditBox (and controls like the AutoSuggestBox that provide a similar text input experience).
title: Entrada de texto com o modo de exibição de manuscrito
label: Text input with the handwriting view
template: detail.hbs
ms.author: kbridge
ms.date: 10/13/18
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: sewen
design-contact: minah.kim
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 3117cde7b8b00973c135fbc759fa99b6a48ec6ac
ms.sourcegitcommit: 310a4555fedd4246188a98b31f6c094abb33ec60
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/19/2018
ms.locfileid: "5128567"
---
# <a name="text-input-with-the-handwriting-view"></a>Entrada de texto com o modo de exibição de manuscrito

![Caixa de texto se expande quando tocado com caneta](images/handwritingview/handwritingview2.gif)

Personalize o modo de exibição de manuscrito interno para tinta para entrada de texto que seja compatível com controles de texto UWP como [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox), [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox)e outros controles que fornecem uma experiência de entrada de texto semelhantes (como [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)).

## <a name="overview"></a>Visão geral

Caixas de entrada de texto XAML têm suporte incorporado para caneta de entrada usando o [Windows Ink](../input/pen-and-stylus-interactions.md). Quando um usuário toca em uma caixa de entrada de texto usando uma caneta do Windows, a caixa de texto transforma em uma superfície de manuscrito, em vez de abrir um painel de entrada separado.

Texto é reconhecido como o usuário escreve em qualquer lugar na caixa de texto e um candidato a janela mostra os resultados do reconhecimento. O usuário pode tocar um resultado para escolhê-lo ou continuar escrevendo aceitar o candidato proposto. Os resultados do reconhecimento de (letra pela letra) literal estão incluídos na janela de candidatos, para que o reconhecimento não está restrito a palavras em um dicionário. Conforme o usuário escreve, a entrada de texto aceita é convertida em um script que mantém a sensação de escrita.

> [!NOTE]
> O modo de exibição de manuscrito está habilitado por padrão, mas você pode desabilitá-lo em uma base por controle e reverter para o painel de entrada de texto em vez disso.

![Caixa de texto com tinta e sugestões](images/handwritingview/handwritingview-inksuggestion1.gif)

Um usuário pode editar o texto usando gestos padrão e ações, como estes:

- _tachado_ ou _transitório out_ -desenhar por meio de excluir uma palavra ou parte de uma palavra
- _ingresso_ -desenhar um arco entre as palavras para excluir o espaço entre eles
- _Inserir_ -desenhar um símbolo de sinal de interpolação para inserir um espaço
- _substituir_ -substituir texto existente para substituí-lo

![Caixa de texto com correção de tinta](images/handwritingview/handwritingview-inkcorrection1.gif)

## <a name="disable-the-handwriting-view"></a>Desabilitar o modo de exibição de manuscrito

O modo de exibição de manuscrito interno é habilitado por padrão.

Convém desabilitar o modo de exibição de manuscrito se você já fornece funcionalidade de tinta em texto equivalente em seu aplicativo, ou sua experiência de entrada de texto depende de algum tipo de formatação ou caractere especial (como uma guia) não está disponível por meio de manuscrito.

Neste exemplo, podemos desabilitar o modo de exibição de manuscrito, definindo a propriedade [IsHandwritingViewEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.ishandwritingviewenabled) do controle [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) como false. Todos os controles de texto que dão suporte a exibição de manuscrito dar suporte a uma propriedade semelhante.

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen" 
    IsHandwritingViewEnabled="False">
</TextBox>
```

## <a name="specify-the-alignment-of-the-handwriting-view"></a>Especificar o alinhamento do modo de exibição de manuscrito

O modo de exibição de manuscrito está localizado acima do controle de texto subjacente e dimensionado para acomodar as preferências do usuário manuscrito (consulte **Configurações -> dispositivos -> caneta e Windows Ink -> manuscrito -> tamanho de fonte ao escrever diretamente no campo de texto **). O modo de exibição é alinhado também automaticamente em relação ao controle de texto e seu local dentro do aplicativo.

O aplicativo da interface do usuário não reorganiza para acomodar o maior controle, para que o sistema pode fazer com que o modo de exibição ocultar o importante da interface do usuário.

Aqui, mostramos como usar a propriedade [PlacementAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.placementalignment) de uma [caixa de texto](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) para especificar quais âncora no controle de texto subjacente é usada para alinhar o modo de exibição de manuscrito.

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

O pop-up sugestões de texto é habilitado por padrão para fornecer uma lista de tinta superior candidatos de reconhecimento do qual o usuário pode selecionar caso o candidato superior está incorreto.

Se seu aplicativo já fornece robusta, funcionalidade de reconhecimento personalizadas, você pode usar a propriedade [AreCandidatesEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.arecandidatesenabled) para desativar as sugestões internas, conforme mostrado no exemplo a seguir.

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

## <a name="use-handwriting-font-preferences"></a>Use as preferências de fonte de manuscrito

Um usuário pode escolher entre uma coleção predefinida de fontes baseadas em texto manuscrito usar quando renderizar texto com base no reconhecimento de tinta (consulte **Configurações -> dispositivos -> caneta e Windows Ink -> manuscrito -> fonte ao usar manuscrito**).

> [!NOTE]
> Os usuários ainda podem criar uma fonte com base em sua próprias manuscrito.
> [!VIDEO https://www.youtube.com/embed/YRR4qd4HCw8]

Seu aplicativo pode acessar essa configuração e usar a fonte selecionada para o texto reconhecido no controle de texto.

Neste exemplo, estamos escutar o evento [TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged) de um [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) e aplicar fonte selecionada do usuário se a alteração de texto foi originada do HandwritingView (ou uma fonte padrão, se não).

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

Composição de controles que usam os controles [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) ou [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox) , como [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) também oferecem suporte a um [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview).

Para acessar o [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) em um controle composto, use o [VisualTreeHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper) API.

O trecho XAML a seguir exibe um controle [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) .

```xaml
<AutoSuggestBox Name="SampleAutoSuggestBox" 
    Height="50" Width="500" 
    PlaceholderText="Auto Suggest Example" 
    FontSize="16" FontFamily="Segoe UI"  
    Loaded="SampleAutoSuggestBox_Loaded">
</AutoSuggestBox>
```

No code-behind correspondente, mostramos como desabilitar o [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) em [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox).

1. Primeiro, manipulamos o evento Loaded do aplicativo onde podemos chamar uma função de FindInnerTextBox para iniciar o percurso de árvore visual.

    ```csharp
    private void SampleAutoSuggestBox_Loaded(object sender, RoutedEventArgs e)
    {
        if (FindInnerTextBox((AutoSuggestBox)sender))
            autoSuggestInnerTextBox.IsHandwritingViewEnabled = false;
    }
    ```

2. Em seguida, começamos Iterando através da árvore visual (começando em uma AutoSuggestBox) na função FindInnerTextBox com uma chamada para FindVisualChildByName.

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

Em alguns casos, talvez seja necessário garantir que o [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) aborda os elementos de interface do usuário que caso contrário, pode não ser.

Aqui, vamos criar uma caixa de texto que dá suporte a ditado (implementado colocando um TextBox e um botão de ditado em um StackPanel).

![TextBox com ditado](images/handwritingview/textbox-with-dictation.png)

Como StackPanel agora está maior do que a caixa de texto, o [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) não pode ocultar todas a cotnrol composto.

![TextBox com ditado](images/handwritingview/textbox-with-dictation-handwritingview.png)

Para resolver isso, defina a propriedade PlacementTarget do [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) para o elemento de interface do usuário ao qual ele deve ser alinhado.

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

Você também pode definir o tamanho do [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview), que pode ser útil quando você precisa garantir que o modo de exibição não ocultar o importante da interface do usuário.

Como o exemplo anterior, criamos um TextBox que dá suporte a ditado (implementado colocando um TextBox e um botão de ditado em um StackPanel).

![TextBox com ditado](images/handwritingview/textbox-with-dictation.png)

Nesse caso, queremos garantir que o botão de ditado está sempre visível.

![TextBox com ditado](images/handwritingview/textbox-with-dictation-handwritingview-resize.png)

Para fazer isso, podemos vincular a propriedade MaxWidth do [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) para a largura do elemento da interface do usuário que ele deve ocultar.

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

## <a name="reposition-custom-ui"></a>Reposicionar interface do usuário personalizada

Se você tiver interface do usuário personalizada que aparece em resposta à entrada de texto, como um pop-up informativa, convém reposicionar essa interface do usuário para que ele não ocultar o modo de exibição de manuscrito.

![TextBox com interface do usuário personalizada](images/handwritingview/textbox-with-customui.png)

O exemplo a seguir mostra como escutar os eventos [Opened](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.opened)e [Closed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.closed
) [SizeChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) do [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) para definir a posição de um [pop-up](https://docs.microsoft.com/uwp/api/windows.ui.popups).

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

## <a name="retemplate-the-handwritingview-control"></a>Criar o controle HandwritingView

Assim como acontece com todos os controles de estrutura XAML, você pode personalizar a estrutura e o comportamento visual de um [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) para seus requisitos específicos.

Para ver um exemplo completo de criação de uma modelo personalizado Confira as instruções de [criar controles personalizados de transporte](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/custom-transport-controls) ou a [amostra de controle de edição personalizado](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl).







