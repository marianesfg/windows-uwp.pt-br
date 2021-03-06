---
Description: Use uma dica de ferramenta para descobrir mais informações sobre um controle antes de solicitar que o usuário execute uma ação.
title: Dicas de ferramenta
ms.assetid: A21BB12B-301E-40C9-B84B-C055FD43D307
label: Tooltips
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 621b687e54cfba55bfd176d4fe0072e47cf79183
ms.sourcegitcommit: db48036af630f33f0a2f7a908bfdfec945f3c241
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84437140"
---
# <a name="tooltips"></a>Dicas de ferramenta

Dica de ferramenta é uma breve descrição vinculada a outro controle ou objeto. Dicas de ferramenta ajudam os usuários a entender objetos desconhecidos que não estão descritos diretamente na UI. Eles são exibidos automaticamente quando o usuário move o foco, pressiona e mantém ou passa o ponteiro do mouse sobre um controle. A dica de ferramenta desaparece após alguns segundos, ou quando o usuário move o foco do dedo, do ponteiro ou do teclado/gamepad.

![Dica de ferramenta](images/controls/tool-tip.png)

**Obter a biblioteca de interface do usuário do Windows**

|  |  |
| - | - |
| ![Logotipo do WinUI](images/winui-logo-64x64.png) | A Biblioteca de interface do usuário do Windows 2.2 ou posterior inclui um novo modelo para esse controle que usa cantos arredondados. Para obter mais informações, confira [Raio de canto](/windows/uwp/design/style/rounded-corner). WinUI é um pacote NuGet que contém novos controles e recursos de interface do usuário para aplicativos do Windows. Para obter mais informações, incluindo instruções de instalação, confira [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

> **APIs da plataforma**: [classe ToolTip](/uwp/api/Windows.UI.Xaml.Controls.ToolTip), [classe ToolTipService](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.tooltipservice)

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Use uma dica de ferramenta para descobrir mais informações sobre um controle antes de solicitar que o usuário execute uma ação. As dicas de ferramenta devem ser usadas de forma comedida e apenas quando são valiosas para o usuário que está tentando executar uma tarefa. Uma regra prática é que, se as informações estiverem disponíveis em outro lugar na mesma experiência, você não precisa de uma dica de ferramenta. Uma dica de ferramenta útil esclarece uma ação que não é clara.

Quando devo usar uma dica de ferramenta? Para decidir, considere estas perguntas:

- **Informações devem se tornar visíveis com base no foco do ponteiro?**
    Se não, use outro controle. Exiba dicas apenas como resultado da interação do usuário; nunca as exiba por conta própria.

- **Um controle tem um rótulo de texto?**
    Se não, use uma dica de ferramenta para fornecer o rótulo. É uma boa prática de design de UX rotular a maioria dos controles em linha, e para isso você não precisa de dicas de ferramenta. Controles de barra de ferramentas e botões de comando que mostrem apenas ícones precisam de dicas de ferramentas.

- **Um objeto se beneficia de uma descrição ou informações adicionais?**
    Se sim, use uma dica de ferramenta. Mas o texto deve ser complementar, ou seja, não essencial às tarefas principais. Se for essencial, coloque-o diretamente na interface do usuário para que os usuários não precisem procurar nem buscá-lo.

- **As informações complementares são um erro, aviso ou status?**
    Se sim, use outro elemento da interface do usuário, como um menu suspenso.

- **Os usuários precisam interagir com a dica?**
    Se sim, use outro controle. Os usuários não podem interagir com dicas porque mover o mouse faz com que elas desapareçam.

- **Os usuários precisam imprimir as informações complementares?**
    Se sim, use outro controle.

- **Os usuários considerarão as dicas incômodas ou distrativas?**
    Se sim, considere usar outra solução, incluindo não fazer nada. Se você usar dicas onde elas possam distrair os usuários, deixe que eles as ative ou desative.

## <a name="example"></a>Exemplo

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/ToolTip">abrir o aplicativo e ver o ToolTip em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Uma dica de ferramenta no aplicativo Mapas do Windows.

![Uma dica de ferramenta no aplicativo Mapas do Windows](images/control-examples/tool-tip-maps.png)

## <a name="create-a-tooltip"></a>Criar uma dica de ferramenta

Uma [ToolTip](/uwp/api/Windows.UI.Xaml.Controls.ToolTip) deve ser atribuída a outro elemento de interface do usuário que é seu proprietário. A classe [ToolTipService](/uwp/api/windows.ui.xaml.controls.tooltipservice) fornece métodos estáticos para exibir uma ToolTip.

No XAML, use a propriedade associada **ToolTipService.Tooltip** para atribuir a ToolTip a um proprietário.

```xaml
<Button Content="Submit" ToolTipService.ToolTip="Click to submit"/>
```

No código, use o método [ToolTipService.SetToolTip](/uwp/api/windows.ui.xaml.controls.tooltipservice.settooltip) para atribuir a ToolTip a um proprietário.

```xaml
<Button x:Name="submitButton" Content="Submit"/>
```

```csharp
ToolTip toolTip = new ToolTip();
toolTip.Content = "Click to submit";
ToolTipService.SetToolTip(submitButton, toolTip);
```

### <a name="content"></a>Conteúdo

Você pode usar qualquer objeto como o [Conteúdo](/uwp/api/windows.ui.xaml.controls.contentcontrol.content) de uma ToolTip. Veja um exemplo de uso de uma [Imagem](/uwp/api/windows.ui.xaml.controls.image) em uma ToolTip.

```xaml
<TextBlock Text="store logo">
    <ToolTipService.ToolTip>
        <Image Source="Assets/StoreLogo.png"/>
    </ToolTipService.ToolTip>
</TextBlock>
```

### <a name="placement"></a>Posicionamento

Por padrão, uma ToolTip é exibida centralizada acima do ponteiro. O posicionamento não é restringido pela janela do aplicativo, para que a ToolTip seja exibida parcial ou totalmente fora dos limites da janela de aplicativo.

Use a propriedade [Placement](/uwp/api/windows.ui.xaml.controls.tooltip.placement) ou a propriedade **ToolTipService.Placement** associada para posicionar a ToolTip acima, abaixo, à esquerda ou à direita do ponteiro. Você pode definir as propriedades [VerticalOffset](/uwp/api/windows.ui.xaml.controls.tooltip.verticaloffset) ou [HorizontalOffset](/uwp/api/windows.ui.xaml.controls.tooltip.horizontaloffset) para alterar a distância entre o ponteiro e a ToolTip. Somente um dos dois valores de deslocamento influenciará a posição final – VerticalOffset quando Placement for Top ou Bottom, HorizontalOffset quando Placement for Left ou Right.

```xaml
<!-- An Image with an offset ToolTip. -->
<Image Source="Assets/StoreLogo.png">
    <ToolTipService.ToolTip>
        <ToolTip Content="Offset ToolTip."
                 Placement="Right"
                 HorizontalOffset="20"/>
    </ToolTipService.ToolTip>
</Image>
```

Se uma ToolTip cobrir o conteúdo a que está se referindo, você poderá ajustar seu posicionamento com precisão usando a nova propriedade **PlacementRect**. PlacementRect ancora a posição da ToolTip e também serve como uma área que não será coberta por ela, desde que haja espaço suficiente na tela para desenhar a ToolTip fora desta área. Você pode especificar a origem do retângulo em relação ao proprietário da dica de ferramenta e a altura e largura da área de exclusão. A propriedade [Placement](/uwp/api/windows.ui.xaml.controls.tooltip.placement) definirá se a ToolTip deve ser desenhada acima, abaixo, à esquerda ou à direita de PlacementRect. 

```xaml
<!-- An Image with a non-occluding ToolTip. -->
<Image Source="Assets/StoreLogo.png" Height="64" Width="96">
    <ToolTipService.ToolTip>
        <ToolTip Content="Non-occluding ToolTip."
                 PlacementRect="0,0,96,64"/>
    </ToolTipService.ToolTip>
</Image>
```

## <a name="recommendations"></a>Recomendações

- Use dicas de ferramentas com moderação (ou não as use). Dicas de ferramentas são uma interrupção. Uma dica de ferramenta pode distrair tanto quanto um pop-up, portanto não os utilize, a menos que eles adicionem valor significativo.
- Mantenha o texto de dica de ferramenta conciso. Dicas de ferramentas são perfeitas para frases curtas e fragmentos de frases. Grandes blocos de texto podem ser complicados, e a dica de ferramenta pode expirar antes de o usuário terminar a leitura.
- Crie textos de dica de ferramenta que sejam úteis e adicionem algo. O texto da dica de ferramenta deve ser informativo. Não crie um texto óbvio nem repita apenas o que já está na tela. Como o texto de dica de ferramenta nem sempre fica visível, devem ser informações complementares que os usuários não precisam ler. Comunique informações importantes usando rótulos de controle autoexplicativos ou texto complementar in-loco.
- Use imagens quando for apropriado. Às vezes é melhor usar uma imagem em uma dica de ferramenta. Por exemplo, quando o usuário passar o ponteiro do mouse sobre um hiperlink, você poderá usar uma dica de ferramenta para mostrar uma prévia da página vinculada.
- Não use uma dica de ferramenta para exibir texto já visível na interface do usuário. Por exemplo, não coloque uma dica de ferramenta em um botão que mostre o mesmo texto do botão.
- Não coloque controles interativos dentro da dica de ferramenta.
- Não coloque imagens que pareçam interativas dentro da dica de ferramenta.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

- [Classe ToolTip](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToolTip)
