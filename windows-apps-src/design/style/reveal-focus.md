---
description: O Foco de Revelação é um efeito de iluminação que anima a borda de elementos focalizáveis quando o usuário move o foco do gamepad ou do teclado até eles.
title: Foco de Revelação
template: detail.hbs
ms.date: 03/01/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: chphilip
design-contact: ''
dev-contact: stevenki
ms.localizationpriority: medium
ms.openlocfilehash: 824476cb098d0ff561fca67497a896586c70b8fb
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75681957"
---
# <a name="reveal-focus"></a>Foco de Revelação

![imagem Hero](images/header-reveal-focus.svg)

O Foco de Revelação é um efeito de iluminação para [experiências de 3 metros](/windows/uwp/design/devices/designing-for-tv), como Xbox One e telas de televisão. Ele anima a borda de elementos focalizáveis, como botões, quando o usuário move o foco do gamepad ou do teclado até eles. Ele está desativado por padrão, mas é fácil de habilitar. 

(Para saber mais sobre o efeito Realce de Revelação, um efeito de iluminação que destaca elementos interativos, confira o [artigo sobre o Realce de Revelação](/windows/uwp/design/style/reveal)).


> **APIs importantes**: [propriedade Application.FocusVisualKind](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind), [enumeração FocusVisualKind](https://docs.microsoft.com/uwp/api/windows.ui.xaml.focusvisualkind) e [propriedade Control.UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals)

## <a name="how-it-works"></a>Como funciona
O Foco de Revelação chama atenção para os elementos em foco adicionando um brilho animado ao redor da borda do elemento:

![Visual da Revelação](images/traveling-focus-fullscreen-light-rf.gif)

Isso é especialmente útil em cenários de 3 metros em que o usuário pode não estar prestando atenção total à tela inteira da TV. 

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/RevealFocus">abri-lo e ver o Foco de Revelação em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-to-use-it"></a>Como usá-lo

O Foco de Revelação está desativado por padrão. Para habilitá-lo:
1. no construtor do aplicativo, chame a propriedade [AnalyticsInfo.VersionInfo.DeviceFamily](/uwp/api/windows.system.profile.analyticsversioninfo.DeviceFamily) e verifique se a família de dispositivos atual é `Windows.Xbox`.
2. Se a família de dispositivos for `Windows.Xbox`, defina a propriedade [Application.FocusVisualKind](/uwp/api/windows.ui.xaml.application.FocusVisualKind) como `FocusVisualKind.Reveal`. 

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

Após definição da propriedade **FocusVisualKind**, o sistema aplica automaticamente o efeito Foco de Revelação a todos os controles cuja propriedade [UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals) esteja definida como **True** (o valor padrão da maioria dos controles). 

## <a name="why-isnt-reveal-focus-on-by-default"></a>Por que o Foco de Revelação não está ativado por padrão? 
Como você pode ver, é muito fácil ativar o Foco de Revelação quando o aplicativo detecta que ele está sendo executado em um Xbox. Então, por que o sistema não o ativa para você? Porque o Foco de Revelação aumenta o tamanho do visual do foco, o que pode causar problemas com o layout da interface do usuário. Em alguns casos, talvez seja conveniente personalizar o efeito Foco de Revelação a fim de otimizá-lo para seu aplicativo.

## <a name="customizing-reveal-focus"></a>Como personalizar o Foco de Revelação

Você pode personalizar o efeito Foco de Revelação modificando as propriedades visuais do foco para cada controle: [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness), [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness), [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) e [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush). Essas propriedades permitem que você personalize a cor e a espessura do retângulo do foco. (Elas são as mesmas propriedades usadas para criar [visuais de foco de alta visibilidade](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#high-visibility-focus-visuals)). 

Porém, antes de começar a personalizá-lo, vale a pena saber um pouco mais sobre os componentes que fazem parte do Foco de Revelação.

Há três partes nos visuais do Foco de Revelação padrão: a borda principal, a borda secundária e o brilho da Revelação. A borda principal tem espessura de **2px** e é traçada em torno da parte *externa* da borda secundária. A borda secundária tem espessura de **1px** e é traçada em torno da parte *interna* da borda principal. O brilho do Foco de Revelação tem espessura proporcional à espessura da borda principal e é visto na parte *externa* da borda principal.

Além dos elementos estáticos, os visuais do Foco de Revelação contam com uma luz animada que pulsa quando está em repouso e que se move na direção do foco quando o foco é movido.

![Camadas do Foco de Revelação](images/reveal-breakdown.svg)

## <a name="customize-the-border-thickness"></a>Personalizar a espessura da borda

Para alterar a espessura dos tipos de borda de um controle, use estas propriedades:

| Tipo de borda | Propriedade |
| --- | --- |
| Principal, brilho   | [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness)<br/> (A alteração da borda principal altera a espessura do brilho proporcionalmente).   |
| Secundário   | [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness)   |


Este exemplo altera a espessura da borda do visual do foco de um botão:

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1"/>
```

## <a name="customize-the-margin"></a>Personalizar a margem

A margem é o espaço entre os limites do visual do controle e o início da borda secundária dos visuais do foco. A distância entre a margem padrão e os limites de controle é de 1px. Você pode editar essa margem de acordo com o controle alterando a propriedade [FocusVisualMargin](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualMargin):

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1" FocusVisualMargin="-3"/>
```

Uma margem negativa move a borda para mais longe do centro do controle, e uma margem positiva move a borda para mais perto do centro do controle.

## <a name="customize-the-color"></a>Personalizar a cor

Para alterar a cor do visual do Foco de Revelação, use as propriedades [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) e [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush).

| Propriedade | Recurso padrão | Valor do recurso padrão |
| ---- | ---- | --- | 
| [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) | SystemControlRevealFocusVisualBrush  | SystemAccentColor |
| [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush)  | SystemControlFocusVisualSecondaryBrush  | SystemAltMediumColor |

(A propriedade FocusPrimaryBrush será padronizada somente para os recursos **SystemControlRevealFocusVisualBrush** quando **FocusVisualKind** for definida como **Reveal**. Caso contrário, ela usará **SystemControlFocusVisualPrimaryBrush**).


Para alterar a cor do visual do foco de um controle individual, defina as propriedades diretamente no controle. Este exemplo substitui as cores do visual do foco de um botão.

```xaml

<!-- Specifying a color directly -->
<Button
    FocusVisualPrimaryBrush="DarkRed"
    FocusVisualSecondaryBrush="Pink"/>

<!-- Using theme resources -->
<Button
    FocusVisualPrimaryBrush="{ThemeResource SystemBaseHighColor}"
    FocusVisualSecondaryBrush="{ThemeResource SystemAccentColor}"/>    
```

Para alterar a cor de todos os visuais do foco em todo o aplicativo, substitua os recursos **SystemControlRevealFocusVisualBrush** e **SystemControlFocusVisualSecondaryBrush** pela sua própria definição:

```xaml
<!-- App.xaml -->
<Application.Resources>

    <!-- Override Reveal Focus default resources. -->
    <SolidColorBrush x:Key="SystemControlRevealFocusVisualBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    <SolidColorBrush x:Key="SystemControlFocusVisualSecondaryBrush" Color="{ThemeResource SystemAccentColor}"/>
</Application.Resources>
```

Para saber mais sobre como modificar a cor do visual do foco, confira [Personalização e identidade visual de cores](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#color-branding--customizing).


## <a name="show-just-the-glow"></a>Mostrar apenas o brilho

Se desejar usar apenas o brilho sem o visual do foco principal ou secundário, basta definir a propriedade [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) do controle como `Transparent` e [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness) como `0`. Nesse caso, o brilho adotará a cor da tela de fundo do controle para dar uma sensação de que não há borda. Você pode modificar a espessura do brilho usando [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness).

```xaml

<!-- Show just the glow -->
<Button
    FocusVisualPrimaryBrush="Transparent"
    FocusVisualSecondaryThickness="0" />    
```


## <a name="use-your-own-focus-visuals"></a>Use seus próprios visuais de foco

Outra maneira de personalizar o Foco de Revelação é recusar os visuais de foco fornecidos pelo sistema desenhando os seus próprios usando os estados visuais. Para saber mais, confira a [amostra de visuais do foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals).


## <a name="reveal-focus-and-the-fluent-design-system"></a>Foco de Revelação e o Sistema de Design Fluente

O Foco de Revelação é um componente do Sistema de Design Fluente que adiciona iluminação ao seu aplicativo. Para saber mais sobre o Sistema de Design Fluente e seus outros componentes, confira a [Visão geral do Design Fluente para UWP](/windows/apps/fluent-design-system).

## <a name="related-articles"></a>Artigos relacionados

- [Realce de Revelação](https://docs.microsoft.com/windows/uwp/design/style/reveal)
- [Projetando para TV e Xbox](/windows/uwp/design/devices/designing-for-tv)
- [Interações de gamepad e de controle remoto](https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions)
- [Amostra de visuais de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)
- [Efeitos de composição](https://docs.microsoft.com/windows/uwp/graphics/composition-effects)
- [Ciência no sistema: Design Fluente e profundidade](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [Ciência no sistema: Design Fluente e iluminação](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
