---
author: QuinnRadich
Description: The toggle switch represents a physical switch that allows users to turn things on or off.
title: Diretrizes para controles de switches de alternância
ms.assetid: 753CFEA4-80D3-474C-B4A9-555F872A3DEF
label: Toggle switches
template: detail.hbs
ms.author: quradic
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d166e872af0824c9eb5df15510f9597d0303f483
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5865072"
---
# <a name="toggle-switches"></a>Switches de alternância

O botão de alternância representa um comutador físico que permite aos usuários ativar ou desativar itens, como um interruptor. Use os controles de botão de alternância para apresentar aos usuários duas opções mutuamente excludentes (como ligar/desligar), em que a escolha de uma opção apresenta resultados imediatos.

Para criar um controle de botão de alternância, use a [classe ToggleSwitch](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch).

> **APIs importantes**: [classe ToggleSwitch](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch), [propriedade IsOn](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.ison), [evento Toggled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.toggled)

## <a name="is-this-the-right-control"></a>Este é o controle correto?

Use um switch de alternância para operações binárias que entram em vigor logo depois que o usuário gira o switch.

![Switch de alternância de Wi-Fi, ativado e desativado](images/toggleswitches01.png)

Pense no botão de alternância como uma opção de energia física para um dispositivo: você liga ou desliga quando deseja habilitar ou desabilitar a ação executada pelo dispositivo.

Para tornar o botão de alternância fácil de entender, rotule-o com uma ou duas palavras, preferencialmente substantivos, que descrevem a funcionalidade controlada. Por exemplo, "WiFi" ou "Luzes de cozinha". 

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para abrir o aplicativo para ver o <a href="xamlcontrolsgallery:/item/ToggleSwitch">ToggleSwitch</a> ou <a href="xamlcontrolsgallery:/item/ToggleButton">ToggleButton</a> em ação.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="choosing-between-toggle-switch-and-check-box"></a>Escolhendo entre o switch de alternância e a caixa de seleção

Para algumas ações, uma caixa de seleção ou um switch de alternância pode funcionar. Para decidir qual controle funcionaria melhor, siga estas dicas:

- Use um switch de alternância para configurações binárias quando as alterações são efetivadas imediatamente depois que o usuário as altera.

    ![Switch de alternância versus caixa de seleção](images/toggleswitches02.png)

    Neste exemplo, está claro pelo botão de alternância que as luzes da cozinha estão "Ativadas". Mas, pela caixa de seleção, o usuário precisa pensar se as luzes estão ativadas ou se ele precisa marcar a caixa para ativar as luzes.

- Use as caixas de seleção para itens opcionais ("úteis de ter").
- Use uma caixa de seleção quando o usuário precisar executar etapas extras para que as alterações sejam efetivadas. Por exemplo, se o usuário precisar clicar no botão "enviar" ou "avançar" para aplicar as alterações, use uma caixa de seleção.
- Use caixas de seleção quando o usuário pode selecionar vários itens que estão relacionados a uma única configuração ou recurso.

## <a name="toggle-switches-in-the-windows-ui"></a>Botões de alternância da interface do usuário do Windows

Essas imagens mostram como a interface do usuário do Windows usa os botões de alternância. Aqui está como a tela de Configurações de armazenamento inteligente usam os botões de alternância:

![Botões de alternância no Armazenamento inteligente](images/SmartStorageToggle.png)

Este exemplo é da página de Configurações de luz noturna:

![Switches de alternância nas configurações do menu Iniciar no Windows](images/NightLightToggle.png)

## <a name="create-a-toggle-switch"></a>Criar um switch de alternância

Veja a seguir como criar um switch de alternância simples. Este XAML cria o switch de alternância mostrado anteriormente.

```xaml
<ToggleSwitch x:Name="lightToggle" Header="Kitchen Lights"/>
```

Veja como criar o mesmo switch de alternância em código.

```csharp
ToggleSwitch lightToggle = new ToggleSwitch();
lightToggle.Header = "Kitchen Lights";

// Add the toggle switch to a parent container in the visual tree.
stackPanel1.Children.Add(lightToggle);
```

### <a name="ison"></a>IsOn

O switch pode ser ativado ou desativado. Use a propriedade [IsOn](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.ison) para determinar o estado do botão. Quando o switch é usado para controlar o estado de outra propriedade binária, você pode usar uma associação, conforme mostrado aqui.

```xaml
<StackPanel Orientation="Horizontal">
    <ToggleSwitch x:Name="ToggleSwitch1" IsOn="True"/>
    <ProgressRing IsActive="{x:Bind ToggleSwitch1.IsOn, Mode=OneWay}" Width="130"/>
</StackPanel>
```

### <a name="toggled"></a>Toggled

Em outros casos, você pode manipular o evento [Toggled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.toggled) para responder a alterações no estado.

Este exemplo mostra como adicionar um manipulador de evento Toggled no XAML e no código. O evento Toggled é manipulado para ativar ou desativar a um anel de progresso e alterar sua visibilidade.

```xaml
<ToggleSwitch x:Name="toggleSwitch1" IsOn="True"
              Toggled="ToggleSwitch_Toggled"/>
```

Veja como criar o mesmo switch de alternância em código.

```csharp
// Create a new toggle switch and add a Toggled event handler.
ToggleSwitch toggleSwitch1 = new ToggleSwitch();
toggleSwitch1.Toggled += ToggleSwitch_Toggled;

// Add the toggle switch to a parent container in the visual tree.
stackPanel1.Children.Add(toggleSwitch1);
```

Aqui está o manipulador para o evento Toggled.

```csharp
private void ToggleSwitch_Toggled(object sender, RoutedEventArgs e)
{
    ToggleSwitch toggleSwitch = sender as ToggleSwitch;
    if (toggleSwitch != null)
    {
        if (toggleSwitch.IsOn == true)
        {
            progress1.IsActive = true;
            progress1.Visibility = Visibility.Visible;
        }
        else
        {
            progress1.IsActive = false;
            progress1.Visibility = Visibility.Collapsed;
        }
    }
}
```

### <a name="onoff-labels"></a>Rótulos Ligar/desligar

Por padrão, o switch de alternância inclui rótulos Ligar/desligar literais, que são localizados automaticamente. Você pode substituir esses rótulos ao definir as propriedades [OnContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.oncontent) e [OffContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.offcontent).

Este exemplo substitui os rótulos Ligar/desligar por rótulos Mostrar/ocultar.

```xaml
<ToggleSwitch x:Name="imageToggle" Header="Show images"
              OffContent="Show" OnContent="Hide"
              Toggled="ToggleSwitch_Toggled"/>
```

Você também pode usar conteúdo mais complexo, ao definir as propriedades [OnContentTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.oncontenttemplate) e [OffContentTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.offcontenttemplate).

## <a name="recommendations"></a>Recomendações

- Use os rótulos padrão de Ligado e Desligado quando possível; somente substitua-os quando é necessário para o botão de alternância fazer sentido. Se você substituí-los, use uma única palavra que descreve com mais precisão a alternância. Em geral, se as palavras "Ligado" e "Desligado" não descrevem a ação vinculada a um botão de alternância, talvez seja necessário um controle diferente.
- Evite substituir os rótulos Ligar e Desligar a menos que seja necessário; use os rótulos padrão a menos que a situação peça rótulos personalizados.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Amostra do XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

- [Classe ToggleSwitch](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch)
- [Botões de opção](radio-button.md)
- [Switches de alternância](toggles.md)
- [Caixas de seleção](checkbox.md)
