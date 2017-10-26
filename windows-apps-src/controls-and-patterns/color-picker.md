---
author: Jwmsft
Description: "Um selecionador de cores permite que um usuário navegue e selecione cores."
title: Selecionador de cores
label: Color Picker
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: llongley
doc-status: Published
ms.openlocfilehash: 546f904239c61c36422070e946d3cd1c6496c9aa
ms.sourcegitcommit: 45490bd85e6f8d247a041841d547ecac2ff48250
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2017
---
# <a name="color-picker"></a>Selecionador de cores

> [!IMPORTANT]
> Este artigo descreve uma funcionalidade que ainda não foi lançada e pode ser modificada substancialmente antes de ser lançada comercialmente. A Microsoft não oferece nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Um selecionador de cores é usado para procurar e selecionar cores. Por padrão, ele permite que um usuário navegue por cores dentre uma variedade de opções ou especifique uma cor em caixas de texto RGB, valor de matiz e saturação (HSV) ou Hexadecimal.

> **APIs importantes**: [classe ColorPicker](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker), [propriedade Color](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker_color), [evento ColorChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker_colorchanged)

![Um selecionador de cores padrão](images/color-picker-default.png)


## <a name="is-this-the-right-control"></a>Este é o controle correto?

Use o selecionador de cores para permitir que o usuário selecione cores no aplicativo. Por exemplo, usá-lo para alterar as configurações de cor, como cores de fonte, tela de fundo ou cores do tema de aplicativo.

Se o aplicativo for para tarefas de desenho ou semelhantes usando a caneta, considere usar [Controles de escrita à tinta](http://windowsstyleguide/controls-and-patterns/inking-controls/) juntamente com o selecionador de cores.

## <a name="create-a-color-picker"></a>Criar um selecionador de cores

Este exemplo mostra como criar um selecionador de cores padrão no XAML.

```xaml
<ColorPicker x:Name="myColorPicker"/>
```

Por padrão, o selecionador de cores mostra uma visualização da cor escolhida na barra de retangular ao lado do espectro de cores. Você pode usar o evento [ColorChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker_colorchanged) ou a propriedade [Color](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker_color) para acessar a cor selecionada e usá-la em seu aplicativo. Veja os exemplos de código detalhado a seguir.

### <a name="bind-to-the-chosen-color"></a>Associar à cor escolhida

Quando a seleção de cor deve entrar em vigor imediatamente, você pode usar a vinculação de dados para associar a propriedade Color ou manipular o evento ColorChanged para acessar a cor selecionada em seu código.

Neste exemplo, você associa a propriedade Color de SolidColorBrush que é usado como o Preenchimento de um retângulo diretamente para a cor selecionada do selecionador de cores. Qualquer alteração no selecionador de cores resulta em uma alteração ao vivo da propriedade vinculada.

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape=”Ring”
             IsColorPreviewVisible="False"
             IsColorChannelTextInputVisible="False"
             IsHexInputVisible="False"/>

<Rectangle Height="50" Width="50">
    <Rectangle.Fill>
        <SolidColorBrush Color="{x:Bind myColorPicker.Color, Mode=OneWay}"/>
    </Rectangle.Fill>
</Rectangle>
```

Este exemplo usa um seletor de cores simplificado com apenas o círculo e o controle deslizante, que é uma experiência comum de separação de cores "casuais". Quando a mudança de cor pode ser vista e acontece em tempo real no objeto afetado, não é necessário mostrar a barra de visualização de cor. Consulte a seção *Personalizar o selecionador de cores* para obter mais informações.

### <a name="save-the-chosen-color"></a>Salve a cor escolhida

Em alguns casos, você não quer aplicar imediatamente a mudança de cor. Por exemplo, quando você hospeda um selecionador de cores em um submenu, recomendamos que você aplique a cor selecionada somente depois que o usuário confirma a seleção ou fecha o submenu. Você também pode salvar o valor de cor selecionado para uso posterior.

Neste exemplo, você hospeda um selecionador de cores em um Submenu com os botões Confirmar e Cancelar. Quando o usuário confirma sua escolha de cor, você pode salvar a cor selecionada para usar posteriormente no aplicativo.

```xaml
<Page.Resources>
    <Flyout x:Key="myColorPickerFlyout">
        <RelativePanel>
            <ColorPicker x:Name="myColorPicker"
                         IsColorChannelTextInputVisible="False"
                         IsHexInputVisible="False"/>

            <Grid RelativePanel.Below="myColorPicker"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Button Content="OK" Click="confirmColor_Click"
                        Margin="0,12,2,0" HorizontalAlignment="Stretch"/>
                <Button Content="Cancel" Click="cancelColor_Click"
                        Margin="2,12,0,0" HorizontalAlignment="Stretch"
                        Grid.Column="1"/>
            </Grid>
        </RelativePanel>
    </Flyout>
</Page.Resources>

<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Button x:Name="colorPickerButton"
            Content="Pick a color"
            Flyout="{StaticResource myColorPickerFlyout}"/>
</Grid>
```

```csharp
private Color mycolor;

private void confirmColor_Click(object sender, RoutedEventArgs e)
{
    // Assign the selected color to a variable to use outside the popup.
    myColor = myColorPicker.Color;

    // Close the Flyout.
    colorPickerButton.Flyout.Hide();
}

private void cancelColor_Click(object sender, RoutedEventArgs e)
{
    // Close the Flyout.
    colorPickerButton.Flyout.Hide();
}
```

### <a name="configure-the-color-picker"></a>Configurar o selecionador de cores

Nem todos os campos são necessários para permitir que o usuário escolha uma cor, portanto, o selecionador de cores é flexível. Ele oferece uma variedade de opções que permitem a você configurar o controle para atender às suas necessidades.

Por exemplo, quando o usuário não precisa de um controle preciso, como escolher uma cor de destaque em um aplicativo de notas, você pode usar uma interface do usuário simplificada. É possível ocultar os campos de entrada de texto e alterar o espectro de cores para um círculo.

Quando o usuário precisa de um controle preciso, como em um aplicativo de design gráfico, você pode mostrar os controles deslizantes e os campos de entrada de texto para cada aspecto da cor.

#### <a name="show-the-circle-spectrum"></a>Mostrar o espectro do círculo

Este exemplo mostra como usar a propriedade [ColorSpectrumShape](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker_colorspectrumshape) para configurar o seletor de cores a fim de usar um espectro circular em vez de quadrado padrão.

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape="Ring"/>
```

![Um seletor de cor com um espectro de círculo](images/color-picker-ring.png)

Quando você deve escolher entre um espectro de cores de quadrado e círculo, uma consideração principal é a precisão. Um usuário tem mais controle quando seleciona uma cor específica usando um quadrado, pois exibe mais a gama de cores. Você deve considerar o espectro de círculo mais como uma experiência de seleção de cor "casual".

#### <a name="show-the-alpha-channel"></a>Mostrar o canal alfa

Neste exemplo, você pode habilitar um controle deslizante de opacidade e textbox no selecionador de cores.

```xaml
<ColorPicker x:Name="myColorPicker"
             IsAlphaEnabled="True"/>
```

![Um selecionador de cores com IsAlphaEnabled definido como true](images/color-picker-alpha.png)

#### <a name="show-a-simple-picker"></a>Mostrar um seletor simples

Este exemplo mostra como configurar o selecionador de cores com uma interface do usuário simples para uso "casual". Você mostra o espectro circular e e oculta as caixas de entrada de texto padrão. Quando a mudança de cor pode ser vista e acontece em tempo real no objeto afetado, não é necessário mostrar a barra de visualização de cor. Caso contrário, você deve deixar a visualização de cor visível.

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape="Ring"
             IsColorPreviewVisible="False"
             IsColorChannelTextInputVisible="False"
             IsHexInputVisible="False"/>
```

![Um selecionador de cores simples](images/color-picker-casual.png)

#### <a name="show-or-hide-additional-features"></a>Mostrar ou ocultar recursos adicionais

Esta tabela mostra todas as opções que você pode usar para configurar o controle ColorPicker.

Recurso | Propriedades
--------|-----------
Espectro de cores | IsColorSpectrumVisible, ColorSpectrumShape, ColorSpectrumComponents
Visualização de cores | IsColorPreviewVisible
Valores de cor| IsColorSliderVisible, IsColorChannelTextInputVisible
Valores de opacidade | IsAlphaEnabled, IsAlphaSliderVisible, IsAlphaTextInputVisible
Valores hexadecimais | IsHexInputVisible

> [!NOTE]
> IsAlphaEnabled deve ser **true** para mostrar a textbox de opacidade e o controle deslizante. A visibilidade dos controles de entrada pode ser modificada usando propriedades IsAlphaTextInputVisible e IsAlphaSliderVisible. Consulte a documentação da API para obter mais detalhes.

## <a name="dos-and-donts"></a>Ações permitidas e proibidas

- Pense sobre que tipo de experiência de seleção de cor é apropriada para seu aplicativo. Alguns cenários podem não exigir a seleção de cor granular e se beneficiam de um selecionador simplificado
- Para obter a experiência de seleção de cores mais precisa, use o espectro quadrado e verifique se tem pelo menos 256 x 256 px ou inclua os campos de entrada de texto para permitir que os usuários refinem a cor selecionada.
- Quando usado em um submenu, a seleção de cor não é confirmada ao tocar no espectro ou ajustar o controle deslizante. Para confirmar a seleção:
  - Forneça os botões de confirmação e cancelamento para aplicar ou cancelar a seleção. Ao pressionar o botão de voltar ou tocar fora do submenu você o ignora e não salva a seleção do usuário.
  - Ou confirme a seleção ignorando o submenu, tocando fora do submenu ou pressionando o botão de voltar.

## <a name="related-articles"></a>Artigos relacionados

- [Interações com caneta em aplicativos UWP](../input-and-devices/pen-and-stylus-interactions.md)
- [Escrita à tinta](inking-controls.md)

<!--
<div class=”microsoft-internal-note”>
<p>
<p>
Note: For more info, see the [color picker redlines](http://uni/DesignDepot.FrontEnd/#/ProductNav/3666/15/dv/?t=Windows%7CControls&f=RS2) on UNI.
</div>
-->