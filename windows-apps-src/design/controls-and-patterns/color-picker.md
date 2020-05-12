---
Description: Um seletor de cor permite que um usuário navegue e selecione cores.
title: Seletor de cor
label: Color Picker
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 6e7661beb52438640c570e1a5ec4d7f60502e119
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82968891"
---
# <a name="color-picker"></a>Seletor de cor

Um seletor de cor é usado para navegar e selecionar cores. Por padrão, ele permite que um usuário navegue por cores dentre uma variedade de opções ou especifique uma cor em caixas de texto RGB, valor de matiz e saturação (HSV) ou hexadecimal.

![Um seletor de cor padrão](images/color-picker-default.png)

**Obter a biblioteca de interface do usuário do Windows**

|  |  |
| - | - |
| ![Logotipo do WinUI](images/winui-logo-64x64.png) | O controle **ColorPicker** está incluído como parte da Biblioteca de interface do usuário do Windows, um pacote NuGet que contém novos controles e recursos de interface do usuário para aplicativos do Windows. Para obter mais informações, incluindo instruções de instalação, confira [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

> **APIs da Biblioteca de interface do usuário do Windows:** [classe ColorPicker](/uwp/api/microsoft.ui.xaml.controls.colorpicker), [propriedade Color](/uwp/api/microsoft.ui.xaml.controls.colorpicker.Color), [evento ColorChanged](/uwp/api/microsoft.ui.xaml.controls.colorpicker.ColorChanged)
>
> **APIs de plataforma:** [classe ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker), [propriedade Color](/uwp/api/windows.ui.xaml.controls.colorpicker.Color), [evento ColorChanged](/uwp/api/windows.ui.xaml.controls.colorpicker.ColorChanged)

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Use o seletor de cor para permitir que o usuário selecione cores no aplicativo. Por exemplo, use-o para alterar as configurações de cor, como cores de fonte, tela de fundo ou cores do tema de aplicativo.

Se o aplicativo for para tarefas de desenho ou semelhantes usando a caneta, considere usar [controles Inking](inking-controls.md) juntamente com o seletor de cor.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/ColorPicker">abrir o aplicativo e ver o ColorPicker em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-color-picker"></a>Criar um seletor de cor

Este exemplo mostra como criar um seletor de cor padrão no XAML.

```xaml
<ColorPicker x:Name="myColorPicker"/>
```

Por padrão, o seletor de cor mostra uma versão prévia da cor escolhida na barra retangular ao lado do espectro de cores. É possível usar o evento [ColorChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.ColorChanged) ou a propriedade [Color](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.Color) para acessar a cor selecionada e usá-la no aplicativo. Veja os exemplos de código detalhado a seguir.

### <a name="bind-to-the-chosen-color"></a>Associar à cor escolhida

Quando a seleção de cor precisa entrar em vigor imediatamente, você pode usar a associação de dados para associar a propriedade Color ou manipular o evento ColorChanged para acessar a cor selecionada no código.

Neste exemplo, você associa a propriedade Color de um SolidColorBrush usado como Preenchimento de um retângulo diretamente para a cor selecionada no seletor de cor. Qualquer alteração no seletor de cor resulta em uma alteração em tempo real da propriedade associada.

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape="Ring"
             IsColorPreviewVisible="False"
             IsColorChannelTextInputVisible="False"
             IsHexInputVisible="False"/>

<Rectangle Height="50" Width="50">
    <Rectangle.Fill>
        <SolidColorBrush Color="{x:Bind myColorPicker.Color, Mode=OneWay}"/>
    </Rectangle.Fill>
</Rectangle>
```

Este exemplo usa um seletor de cor simplificado com apenas o círculo e o controle deslizante, uma experiência comum de seleção de cores "casuais". Quando a mudança de cor pode ser vista e acontece em tempo real no objeto afetado, não é necessário mostrar a barra de visualização de cor. Veja a seção *Personalizar o seletor de cor* para saber mais informações.

### <a name="save-the-chosen-color"></a>Salvar a cor escolhida

Em alguns casos, você não quer aplicar imediatamente a mudança de cor. Por exemplo, quando você hospeda um seletor de cor em um submenu, é recomendável aplicar a cor selecionada somente após o usuário confirmar a seleção ou fechar o submenu. Você também pode salvar o valor de cor selecionado para uso posterior.

Neste exemplo, você hospeda um seletor de cor em um Submenu com os botões Confirmar e Cancelar. Quando o usuário confirmar a cor escolhida, você poderá salvá-la para usar posteriormente no aplicativo.

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

### <a name="configure-the-color-picker"></a>Configurar o seletor de cor

Nem todos os campos são necessários para permitir que o usuário escolha uma cor, portanto, o seletor de cor é flexível. Ele oferece uma variedade de opções que permitem a você configurar o controle para atender às suas necessidades.

Por exemplo, quando o usuário não precisa de um controle preciso, como escolher uma cor de marca-texto em um aplicativo de notas, use uma interface do usuário simplificada. É possível ocultar os campos de entrada de texto e alterar o espectro de cores para um círculo.

Quando o usuário precisa de um controle preciso, como em um aplicativo de design gráfico, você pode mostrar os controles deslizantes e os campos de entrada de texto para cada aspecto da cor.

#### <a name="show-the-circle-spectrum"></a>Mostrar o espectro do círculo

Este exemplo mostra como usar a propriedade [ColorSpectrumShape](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.ColorSpectrumShape) para configurar o seletor de cor a fim de usar um espectro circular em vez do padrão quadrado.

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape="Ring"/>
```

![Um seletor de cor com um espectro de círculo](images/color-picker-ring.png)

Quando você precisa escolher entre um espectro de cores de quadrado e de círculo, o principal fator a se considerar é a precisão. Um usuário tem mais controle quando seleciona uma cor específica usando um quadrado, pois exibe mais a gama de cores. Considere o espectro de círculo mais como uma experiência de seleção de cor "casual".

#### <a name="show-the-alpha-channel"></a>Mostrar o canal alfa

Neste exemplo, você pode habilitar um controle deslizante de opacidade e a caixa de texto no seletor de cor.

```xaml
<ColorPicker x:Name="myColorPicker"
             IsAlphaEnabled="True"/>
```

![Um seletor de cor com IsAlphaEnabled definido como true](images/color-picker-alpha.png)

#### <a name="show-a-simple-picker"></a>Mostrar um seletor simples

Este exemplo mostra como configurar o seletor de cor com uma interface do usuário simples para uso "casual". Você mostra o espectro circular e oculta as caixas de entrada de texto padrão. Quando a mudança de cor pode ser vista e acontece em tempo real no objeto afetado, não é necessário mostrar a barra de visualização de cor. Caso contrário, você deve deixar a visualização de cor visível.

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape="Ring"
             IsColorPreviewVisible="False"
             IsColorChannelTextInputVisible="False"
             IsHexInputVisible="False"/>
```

![Um seletor de cor simples](images/color-picker-casual.png)

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
> IsAlphaEnabled deve ser **true** para mostrar a caixa de texto de opacidade e o controle deslizante. A visibilidade dos controles de entrada pode ser modificada usando as propriedades IsAlphaTextInputVisible e IsAlphaSliderVisible. Confira a documentação da API para saber mais detalhes.

## <a name="dos-and-donts"></a>O que fazer e o que não fazer

- Pense sobre que tipo de experiência de seleção de cor é apropriado para o aplicativo. Alguns cenários podem não exigir a seleção de cor granular e se beneficiariam de um seletor simplificado
- Para obter a experiência de seleção de cores mais precisa, use o espectro quadrado e verifique se há pelo menos 256 x 256 px ou inclua os campos de entrada de texto para permitir que os usuários refinem a cor selecionada.
- Quando usado em um submenu, a seleção de cor não é confirmada ao tocar no espectro ou ajustar o controle deslizante. Para confirmar a seleção:
  - Forneça os botões de confirmação e cancelamento para aplicar ou cancelar a seleção. Ao pressionar o botão Voltar ou tocar fora do submenu, você o ignora e não salva a seleção do usuário.
  - Ou confirme a seleção ao descartar o submenu tocando fora do submenu ou pressionando o botão Voltar.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

- [Interações com canetas em aplicativos do Windows](../input/pen-and-stylus-interactions.md)
- [Escrita à tinta](inking-controls.md)

<!--
<div class="microsoft-internal-note">
<p>
<p>
Note: For more info, see the [color picker redlines](https://uni/DesignDepot.FrontEnd/#/ProductNav/3666/15/dv/?t=Windows%7CControls&f=RS2) on UNI.
</div>
-->
