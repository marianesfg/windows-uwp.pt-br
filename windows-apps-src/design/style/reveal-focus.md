---
author: cphilippona
description: Revele o que foco é um efeito de iluminação que anima a borda do controle elementos quando o usuário move o foco do teclado ou gamepad a eles.
title: Revelar foco
template: detail.hbs
ms.author: mijacobs
ms.date: 03/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: chphilip
design-contact: ''
dev-contact: stevenki
ms.localizationpriority: medium
ms.openlocfilehash: 7b5fa84efbe20368be55a50ce20c8e6e5d1fe439
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/24/2018
ms.locfileid: "2839588"
---
# <a name="reveal-focus"></a>Revelar foco

![imagem hero](images/header-reveal-focus.svg)

Revele o que foco é um efeito de iluminação para [experiências de 10-pé](/windows/uwp/design/devices/designing-for-tv), como um Xbox e televisão as telas. Ele anima a borda de elementos focalizáveis, como botões, quando o usuário move o foco do gamepad ou do teclado até eles. Ele está desativado por padrão, mas é fácil de habilitar. 

(Para o efeito revelar realçar, um efeito de iluminação que destaca elementos interativos, consulte o [artigo revelar destacar](/windows/uwp/design/style/reveal).)


> **APIs importantes**: [propriedade Application.FocusVisualKind](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind), [enumeração FocusVisualKind](https://docs.microsoft.com/uwp/api/windows.ui.xaml.focusvisualkind) e [propriedade Control.UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals)

## <a name="how-it-works"></a>Como funciona
Revele chamadas atenção aos elementos focados adicionando um brilho animado em torno da borda do elemento:

![Visual do Revelação](images/traveling-focus-fullscreen-light-rf.gif)

Isso é especialmente útil em cenários de 10-pé onde o usuário pode não estar completa Observação atenta toda a tela de TV. 

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo de <strong style="font-weight: semi-bold">Galeria de controles XAML</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/RevealFocus">Abrir o aplicativo e consulte revelar o foco em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-to-use-it"></a>Como usá-lo

Revele que foco está desativado por padrão. Para habilitá-lo:
1. No construtor do app, chame a propriedade [AnalyticsInfo.VersionInfo.DeviceFamily](/uwp/api/windows.system.profile.analyticsversioninfo.DeviceFamily) e verifique se a família de dispositivos atual é `Windows.Xbox`.
2. Se a família de dispositivos for `Windows.Xbox`, defina a propriedade [Application.FocusVisualKind](/uwp/api/windows.ui.xaml.application.FocusVisualKind) como `FocusVisualKind.Reveal`. 

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

Depois de definir a propriedade **FocusVisualKind** , o sistema aplica automaticamente o efeito revelar foco a todos os controles cuja propriedade [UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals) é definida como **True** (o valor padrão para a maioria dos controles). 

## <a name="why-isnt-reveal-focus-on-by-default"></a>Por que não revelar foco por padrão? 
Como você pode ver, é bastante fácil ativar revelar o foco quando o aplicativo detecta que ele é executado em um Xbox. Então, por que o sistema não o ativa para você? Porque o foco revelar aumenta o tamanho do foco visual, que pode causar problemas com o layout da interface do usuário. Em alguns casos, você desejará personalizar o efeito revelar o foco para otimizá-lo para seu aplicativo.

## <a name="customizing-reveal-focus"></a>Personalizando o foco Reveal

Você pode personalizar o efeito revelar foco modificando as propriedades de foco visual para cada controle: [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness), [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness), [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush)e [ FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush). Essas propriedades permitem que você personalize a cor e a espessura do retângulo de foco. (Elas são as mesmas propriedades que você usa para criar [elementos visuais de foco de alta visibilidade](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#high-visibility-focus-visuals).) 

Mas antes de começar a customzing, ele é útil conhecer um pouco mais sobre os componentes que compõem a revelar o foco.

Há três partes em visuais de foco revelar o padrão: a borda principal, da borda secundária e do Reveal com brilho. A borda principal apresenta espessura de **2px** e é moldada em torno da parte *externa* da borda secundária. A borda secundária apresenta espessura de **1px** e é moldada em torno da parte *interna* da borda secundária. Brilho revelar foco tem uma espessura proporcional à espessura da borda principal e executa em torno de *fora* da borda principal.

Além dos elementos estáticos, visuais revelar foco recurso uma luz animada que pulsates em pousar e move na direção do foco ao mover o foco.

![Revelar camadas de foco](images/reveal-breakdown.svg)

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

Para alterar a cor do foco revelar visual, use as propriedades [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) e [FocusVisualSecondaryBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush) .

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

Outra maneira para personalizar o foco revelar é recusar os elementos visuais do foco fornecido pelo sistema desenhando seus próprios usando estados visuais. Para saber mais, consulte a [amostra de elementos visuais de foco](http://go.microsoft.com/fwlink/p/?LinkID=619895).


## <a name="reveal-focus-and-the-fluent-design-system"></a>Revelar o foco e o sistema de Design Fluent

Revele o que foco é um componente do sistema de Design Fluent que adiciona claro ao seu aplicativo. Para saber mais sobre o sistema de Design Fluente e seus outros componentes, consulte a [visão geral do Design Fluente para UWP](../fluent-design-system/index.md).

## <a name="related-articles"></a>Artigos relacionados

- [Revelar realce](https://docs.microsoft.com/windows/uwp/design/style/reveal)
- [Projetando para Xbox e TV](/windows/uwp/design/devices/designing-for-tv)
- [Interações de gamepad e controle remoto](https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions)
- [Amostra de elementos visuais de foco](http://go.microsoft.com/fwlink/p/?LinkID=619895)
- [Efeitos de composição](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [Ciência no sistema: Design Fluente e profundidade](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [Ciência no sistema: Fluent Design e luz](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)