---
author: QuinnRadich
Description: Radio buttons let users select one option from two or more choices.
title: Diretrizes de botões de opção
ms.assetid: 41E3F928-AA55-42A2-9281-EC3907C4F898
label: Radio buttons
template: detail.hbs
ms.author: quradic
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 48d830b388fee8a0007447a66aa58e3794cfaae0
ms.sourcegitcommit: 7aa1933e6970f878faf50d59e1f799b90afd7cc7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/05/2018
ms.locfileid: "3382871"
---
# <a name="radio-buttons"></a>Botões de opção

> **APIs importantes**: [classe RadioButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton), [evento Checked](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked), [propriedade IsChecked](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked)

Botões de opção permitem que os usuários selecionem uma opção de um conjunto. Cada opção é representada por um botão de opção, e os usuários podem selecionar apenas um botão de opção em um grupo de botões de opção.

(Se você tem curiosidade em saber a origem do nome, os botões de opção, também conhecidos como botões de rádio, receberam esse nome por causa dos botões predefinidos de canais em um rádio.)

![Botões de opção](images/controls/radio-button.png)

## <a name="is-this-the-right-control"></a>Este é o controle correto?

Use botões de opção para apresentar aos usuários duas ou mais opções mutuamente exclusivas.

![Um grupo de botões de opção](images/radiobutton_basic.png)

Use botões de opção quando os usuários precisam ver todas as opções para fazer uma seleção. Como os botões de opção enfatizam todas as opções da mesma forma, isso pode chamar mais atenção para as opções que o necessário. Considere usar outros controles, a menos que as opções mereçam atenção adicional do usuário. Por exemplo, se a opção padrão for recomendada para a maioria dos usuários na maioria das situações, use uma [lista suspensa](lists.md).

![lista suspensa](images/combo_box_collapsed.png)

Se houver apenas duas opções mutuamente exclusivas, combine-as em uma única [caixa de seleção](checkbox.md) ou [botão de alternância](toggles.md). Por exemplo, use uma caixa de seleção para "Concordo" em vez de dois botões de opção para "Concordo" e "Não concordo".

![Duas formas de apresentar uma opção binária](images/radiobutton_vs_checkbox.png)

Quando o usuário puder selecionar várias opções, use uma [caixa de seleção](checkbox.md).

![Selecionando várias opções com caixas de seleção](images/checkbox2.png)

Quando opções forem números que corrigimos etapas (10, 20, 30), use um [controle deslizante](slider.md).

![controle deslizante](images/controls/slider.png)

Se houver mais que oito opções, use uma [lista suspensa](lists.md) ou [caixa de listagem](lists.md).

![caixa de combinação](images/combo_box_scroll.png)

Se as opções disponíveis estiverem baseadas no contexto atual do aplicativo ou, de outra forma, variar dinamicamente use uma [caixa de listagem](lists.md) de seleção única.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem um aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/RadioButton">abrir o aplicativo e ver o RadioButton em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Botões de opção nas configurações do navegador Microsoft Edge.

![Botões de opção nas configurações do navegador Microsoft Edge](images/control-examples/radio-buttons-edge.png)

## <a name="create-a-radio-button"></a>Criar um botão de opção

Botões de opção funcionam em grupos. Há 2 maneiras de agrupar controles de botão de opção:
- Colocá-los dentro do mesmo contêiner pai.
- Defina a propriedade [GroupName](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName) em cada botão de opção como o mesmo valor.

Neste exemplo, o primeiro grupo de botões de opção é implicitamente agrupado por estar no painel empilhado. O segundo grupo é dividido entre 2 painéis empilhados e, portanto, são explicitamente agrupados por GroupName.

```xaml
<StackPanel>
    <StackPanel>
        <TextBlock Text="Background" Style="{ThemeResource BaseTextBlockStyle}"/>
        <StackPanel Orientation="Horizontal">
            <RadioButton Content="Green" Tag="Green" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Yellow" Tag="Yellow" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Blue" Tag="Blue" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="White" Tag="White" Checked="BGRadioButton_Checked" IsChecked="True"/>
        </StackPanel>
    </StackPanel>
    <StackPanel>
        <TextBlock Text="BorderBrush" Style="{ThemeResource BaseTextBlockStyle}"/>
        <StackPanel Orientation="Horizontal">
            <StackPanel>
                <RadioButton Content="Green" GroupName="BorderBrush" Tag="Green" Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="Yellow" GroupName="BorderBrush" Tag="Yellow" Checked="BorderRadioButton_Checked" IsChecked="True"/>
            </StackPanel>
            <StackPanel>
                <RadioButton Content="Blue" GroupName="BorderBrush" Tag="Blue" Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="White" GroupName="BorderBrush" Tag="White"  Checked="BorderRadioButton_Checked"/>
            </StackPanel>
        </StackPanel>
    </StackPanel>
    <Border x:Name="BorderExample1" BorderThickness="10" BorderBrush="#FFFFD700" Background="#FFFFFFFF" Height="50" Margin="0,10,0,10"/>
</StackPanel>
```

```csharp
private void BGRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && BorderExample1 != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "Yellow":
                BorderExample1.Background = new SolidColorBrush(Colors.Yellow);
                break;
            case "Green":
                BorderExample1.Background = new SolidColorBrush(Colors.Green);
                break;
            case "Blue":
                BorderExample1.Background = new SolidColorBrush(Colors.Blue);
                break;
            case "White":
                BorderExample1.Background = new SolidColorBrush(Colors.White);
                break;
        }
    }
}

private void BorderRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && BorderExample1 != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "Yellow":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.Gold);
                break;
            case "Green":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.DarkGreen);
                break;
            case "Blue":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.DarkBlue);
                break;
            case "White":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.White);
                break;
        }
    }
}
```

Os grupos de botões de opção têm a seguinte aparência.

![Botões de opção em dois grupos](images/radio-button-groups.png)

Um botão de opção tem dois estados: *marcado* ou *desmarcado*. Quando um botão de opção está marcado, sua propriedade [IsChecked](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked) é **true**. Quando um botão de opção está desmarcado, sua propriedade **IsChecked** é **false**. Um botão de opção pode ser desmarcado clicando em outro botão de opção no mesmo grupo, mas não pode ser desmarcado com um novo clique nele. No entanto, você pode desmarcar um botão de opção programaticamente, definindo sua propriedade IsChecked como **false**. Na verdade, você pode comparar o **IsChecked** propriedade com um bool, obtendo o **valor** da propriedade **IsChecked**

## <a name="recommendations"></a>Recomendações

-   Certifique-se de que a finalidade e o estado atual de um conjunto de botões de opção seja clara.
-   Limite o conteúdo em texto do botão de opção para uma única linha.
-   Se o conteúdo do texto for dinâmico, considere como o botão redimensionará e o que acontecerá com os elementos visuais ao redor dele.
-   Use a fonte padrão, a menos que as diretrizes da marca o orientem de outra forma.
-   Não coloque dois grupos de botão de opção próximos um ao lado do outro. Quando dois grupos de botão de opção estão próximos um do outro é difícil determinar quais botões pertencem a qual grupo.

## <a name="additional-usage-guidance"></a>Diretrizes de uso adicional

Esta ilustração mostra a maneira correta de posicionar e espaçar os botões de opção.

![Um conjunto de botões de opção](images/radiobutton-layout.png)

![diretrizes de espaçamento de botões de opção](images/radiobutton-redlines.png)

## <a name="related-topics"></a>Tópicos relacionados

**Para designers**
- [Botões](buttons.md)
- [Switches de alternância](toggles.md)
- [Caixas de seleção](checkbox.md)
- [Listas e caixas de combinação](lists.md)
- [Controles deslizantes](slider.md)

**Para desenvolvedores (XAML)**
- [Classe RadioButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.radiobutton)
