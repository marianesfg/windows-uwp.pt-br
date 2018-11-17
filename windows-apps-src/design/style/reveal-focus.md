---
author: cphilippona
description: Revela que foco é um efeito de iluminação que anima a borda de elementos focalizáveis quando o usuário move o foco do gamepad ou teclado até eles.
title: Foco do revelação
template: detail.hbs
ms.author: mijacobs
ms.date: 03/1/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: chphilip
design-contact: ''
dev-contact: stevenki
ms.localizationpriority: medium
ms.openlocfilehash: b7c80ed7521d797602cde15607f966a1fc3665cd
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7166458"
---
# <a name="reveal-focus"></a>Foco do revelação

![imagem hero](images/header-reveal-focus.svg)

Revela que foco é um efeito de iluminação para [experiências de 10 pés](/windows/uwp/design/devices/designing-for-tv), como Xbox One e telas de televisão. Ele anima a borda de elementos focalizáveis, como botões, quando o usuário move o foco do gamepad ou do teclado até eles. Ele está desativado por padrão, mas é fácil de habilitar. 

(Para o efeito de realce de revelação, um efeito de iluminação que destaca elementos interativos, consulte o [artigo do realce de revelação](/windows/uwp/design/style/reveal).)


> **APIs importantes**: [propriedade Application.FocusVisualKind](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind), [enumeração FocusVisualKind](https://docs.microsoft.com/uwp/api/windows.ui.xaml.focusvisualkind) e [propriedade Control.UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals)

## <a name="how-it-works"></a>Como funciona
Revela foco chama a atenção para elementos em foco adicionando um brilho animado ao redor da borda do elemento:

![Visual do Revelação](images/traveling-focus-fullscreen-light-rf.gif)

Isso é especialmente útil em cenários de 10 pés onde o usuário pode não estar prestando atenção total à toda a tela de TV. 

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/RevealFocus">Abrir o aplicativo e ver o foco do revelação em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-to-use-it"></a>Como usá-lo

Revela que foco está desativada por padrão. Para habilitá-lo:
1. No construtor do app, chame a propriedade [AnalyticsInfo.VersionInfo.DeviceFamily](/uwp/api/windows.system.profile.analyticsversioninfo.DeviceFamily) e verifique se a família de dispositivos atual é `Windows.Xbox`.
2. Se a família de dispositivos for `Windows.Xbox`, defina a propriedade [Application.FocusVisualKind](/uwp/api/windows.ui.xaml.application.FocusVisualKind) como `FocusVisualKind.Reveal`. 

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

Depois de definir a propriedade **FocusVisualKind** , o sistema aplica automaticamente o efeito de foco do revelação a todos os controles cuja propriedade [UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals) é definida como **True** (o valor padrão da maioria dos controles). 

## <a name="why-isnt-reveal-focus-on-by-default"></a>Por que não revelar foco por padrão? 
Como você pode ver, é muito fácil ativar revelar o foco quando o app detectar que ele é executado em um Xbox. Então, por que o sistema não o ativa para você? Porque o foco do revelação aumenta o tamanho do foco visual, que pode causar problemas com o layout de interface do usuário. Em alguns casos, convém personalizar o efeito de foco do revelação para otimizá-lo para seu aplicativo.

## <a name="customizing-reveal-focus"></a>Personalizando o foco do revelação

Você pode personalizar o efeito de foco do revelação modificando as propriedades visuais de foco para cada controle: [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness), [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness), [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush)e [ FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush). Essas propriedades permitem que você personalize a cor e a espessura do retângulo de foco. (Elas são as mesmas propriedades que você usa para criar [elementos visuais de foco de alta visibilidade](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#high-visibility-focus-visuals).) 

Mas antes de começar a pena, vale a pena saber um pouco mais sobre os componentes que compõem o foco do revelação.

Há três partes nos elementos visuais de foco do revelação padrão: a borda principal, a borda secundária e o brilho do revelação. A borda principal apresenta espessura de **2px** e é moldada em torno da parte *externa* da borda secundária. A borda secundária apresenta espessura de **1px** e é moldada em torno da parte *interna* da borda secundária. O brilho do foco do revelação tem espessura proporcional à espessura da borda principal e é executado em torno do *fora* da borda principal.

Além dos elementos estáticos, elementos visuais de foco do revelação apresentam uma luz animada que pulsates quando estão em repouso e se move na direção do foco ao mover o foco.

![Camadas do foco do revelação](images/reveal-breakdown.svg)

## <a name="customize-the-border-thickness"></a>Personalizar a espessura da borda

Para alterar a espessura dos tipos de borda de um controle, use estas propriedades:

| Tipo de borda | Propriedade |
| --- | --- |
| Principal, brilho   | [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness)<br/> (A alteração da borda principal também altera a espessura do brilho proporcionalmente.)   |
| Secundária   | [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness)   |


Este exemplo altera a espessura da borda de um botão do elemento visual de foco:

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1"/>
```

## <a name="customize-the-margin"></a>Personalizar a margem

A margem é o espaço entre os limites dos elementos visuais do controle e o início da borda secundária dos elementos visuais de foco. A margem padrão está a 1 px de distância dos limites do controle. Você pode editar essa margem em uma base por controle alterando a propriedade [FocusVisualMargin](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualMargin):

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1" FocusVisualMargin="-3"/>
```

Uma margem negativa move a borda para mais longe do centro do controle, e uma margem positiva move a borda para mais perto do centro do controle.

## <a name="customize-the-color"></a>Personalizar a cor

Para alterar a cor do foco do revelação visual, use as propriedades [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) e [FocusVisualSecondaryBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush) .

| Propriedade | Recurso padrão | Valor do recurso padrão |
| ---- | ---- | --- | 
| [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) | SystemControlRevealFocusVisualBrush  | SystemAccentColor |
| [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush)  | SystemControlFocusVisualSecondaryBrush  | SystemAltMediumColor |

(A propriedade FocusPrimaryBrush somente assumirá como padrão os recursos **SystemControlRevealFocusVisualBrush** quando **FocusVisualKind** estiver definida como **Reveal**. Caso contrário, ela usará **SystemControlFocusVisualPrimaryBrush**.)


Para alterar a cor do elemento visual de foco de um controle individual, defina as propriedades diretamente no controle. Este exemplo substitui as cores do elemento visual de foco de um botão.

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

Para alterar a cor de todos os elementos visuais de foco em todo o app, substitua os recursos **SystemControlRevealFocusVisualBrush** e **SystemControlFocusVisualSecondaryBrush** por sua própria definição:

```xaml
<!-- App.xaml -->
<Application.Resources>

    <!-- Override Reveal Focus default resources. -->
    <SolidColorBrush x:Key="SystemControlRevealFocusVisualBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    <SolidColorBrush x:Key="SystemControlFocusVisualSecondaryBrush" Color="{ThemeResource SystemAccentColor}"/>
</Application.Resources>
```

Para obter mais informações sobre como modificar a cor do elemento visual de foco, consulte [Personalização e identidade visual de cores](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#color-branding--customizing).


## <a name="show-just-the-glow"></a>Mostrar apenas o brilho

Se você gostaria de usar apenas o brilho sem o elemento visual de foco principal ou secundário, basta definir a propriedade [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) do controle como `Transparent` e [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness) como `0`. Nesse caso, o brilho adotará a cor da tela de fundo do controle para dar uma sensação de que não há borda. Você pode modificar a espessura do brilho usando [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness).

```xaml

<!-- Show just the glow -->
<Button
    FocusVisualPrimaryBrush="Transparent"
    FocusVisualSecondaryThickness="0" />    
```


## <a name="use-your-own-focus-visuals"></a>Use seus próprios elementos visuais de foco

Outra maneira para personalizar o foco do revelação é recusar os elementos visuais de foco fornecida pelo sistema desenhando seus próprios estados visuais de uso. Para saber mais, consulte a [amostra de elementos visuais de foco](http://go.microsoft.com/fwlink/p/?LinkID=619895).


## <a name="reveal-focus-and-the-fluent-design-system"></a>Revelar foco e o sistema de Design fluente

Revela que foco é um componente do sistema de Design Fluent que acrescenta luz ao seu aplicativo. Para saber mais sobre o sistema de Design Fluente e seus outros componentes, consulte a [visão geral do Design Fluente para UWP](../fluent-design-system/index.md).

## <a name="related-articles"></a>Artigos relacionados

- [Realce do revelação](https://docs.microsoft.com/windows/uwp/design/style/reveal)
- [Projetando para Xbox e TV](/windows/uwp/design/devices/designing-for-tv)
- [Interações de gamepad e controle remoto](https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions)
- [Amostra de elementos visuais de foco](http://go.microsoft.com/fwlink/p/?LinkID=619895)
- [Efeitos de composição](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [Ciência no sistema: Design Fluente e profundidade](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [Ciência no sistema: Fluent Design e luz](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
