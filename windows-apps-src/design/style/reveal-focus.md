---
description: Revele o que foco é um efeito de iluminação que anima a borda de elementos focalizáveis quando o usuário move o foco do teclado ou gamepad a eles.
title: Revelar o foco
template: detail.hbs
ms.date: 03/01/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: chphilip
design-contact: ''
dev-contact: stevenki
ms.localizationpriority: medium
ms.openlocfilehash: 7bcceb8d44b6d92cab05a9c077531b3fe1b05c79
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651751"
---
# <a name="reveal-focus"></a>Revelar o foco

![imagem hero](images/header-reveal-focus.svg)

Revelação de foco é um efeito de iluminação para [experiências de 10 pés](/windows/uwp/design/devices/designing-for-tv), como Xbox One e telas de televisão. Ele anima a borda de elementos focalizáveis, como botões, quando o usuário move o foco do gamepad ou do teclado até eles. Ele está desativado por padrão, mas é fácil de habilitar. 

(Para o efeito de revelar realçar um efeito de iluminação que realça os elementos interativos, consulte o [revelar realçar artigo](/windows/uwp/design/style/reveal).)


> **APIs importantes**: [Propriedade Application.FocusVisualKind](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind), [enum FocusVisualKind](https://docs.microsoft.com/uwp/api/windows.ui.xaml.focusvisualkind), [Control.UseSystemFocusVisuals propriedade](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals)

## <a name="how-it-works"></a>Como funciona
Revele chamadas concentrar a atenção para elementos com foco, adicionando um brilho animado em torno da borda do elemento:

![Visual do Revelação](images/traveling-focus-fullscreen-light-rf.gif)

Isso é especialmente útil em cenários de 10 pés em que o usuário pode não estar pagando sua atenção total a toda a tela de TV. 

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o <strong style="font-weight: semi-bold">da Galeria de controles XAML</strong> aplicativo instalado, clique aqui para <a href="xamlcontrolsgallery:/item/RevealFocus">abrir o aplicativo e ver o foco revelar em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-to-use-it"></a>Como usá-lo

Revele o que foco está desativado por padrão. Para habilitá-lo:
1. No construtor do app, chame a propriedade [AnalyticsInfo.VersionInfo.DeviceFamily](/uwp/api/windows.system.profile.analyticsversioninfo.DeviceFamily) e verifique se a família de dispositivos atual é `Windows.Xbox`.
2. Se a família de dispositivos for `Windows.Xbox`, defina a propriedade [Application.FocusVisualKind](/uwp/api/windows.ui.xaml.application.FocusVisualKind) como `FocusVisualKind.Reveal`. 

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

Depois de definir a **FocusVisualKind** propriedade, o sistema automaticamente aplica o efeito de foco de revelar a todos os controles cuja [UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals) estiver definida como **verdadeiro** (o valor padrão para a maioria dos controles). 

## <a name="why-isnt-reveal-focus-on-by-default"></a>Por que não revelar foco em por padrão? 
Como você pode ver, é razoavelmente fácil ativar revelar foco quando o aplicativo detecte que está executando um Xbox. Então, por que o sistema não o ativa para você? Como foco revelar aumenta o tamanho do foco visual, que pode causar problemas com o layout da interface do usuário. Em alguns casos, você desejará personalizar o efeito de revelar o foco para otimizá-lo para seu aplicativo.

## <a name="customizing-reveal-focus"></a>Personalizando a revelação de foco

Você pode personalizar o efeito de foco revelar modificando as propriedades visuais de foco para cada controle: [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness), [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness), [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush), e [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush). Essas propriedades permitem que você personalize a cor e a espessura do retângulo de foco. (Elas são as mesmas propriedades que você usa para criar [elementos visuais de foco de alta visibilidade](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#high-visibility-focus-visuals).) 

Mas antes de iniciar personalizando, ele é útil saber um pouco mais sobre os componentes que compõem a revelar o foco.

Há três partes para os visuais de foco revelar padrão: borda primária, secundária borda e a revelação brilham. A borda principal apresenta espessura de **2px** e é moldada em torno da parte *externa* da borda secundária. A borda secundária apresenta espessura de **1px** e é moldada em torno da parte *interna* da borda secundária. Brilho revelar foco possui uma espessura proporcional à espessura da borda primária e é executado em torno de *fora* da borda do primária.

Além dos elementos estáticos, visuais de foco revelar apresentam uma luz animada que pulsates quando estiver no repousa e move na direção de foco ao mover o foco.

![Revelar as camadas de foco](images/reveal-breakdown.svg)

## <a name="customize-the-border-thickness"></a>Personalizar a espessura da borda

Para alterar a espessura dos tipos de borda de um controle, use estas propriedades:

| Tipo de borda | Propriedade |
| --- | --- |
| Principal, brilho   | [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness)<br/> (A alteração da borda principal também altera a espessura do brilho proporcionalmente.)   |
| Secundário   | [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness)   |


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

Para alterar a cor do foco revelar visual, use o [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) e [FocusVisualSecondaryBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush) propriedades.

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

Outra maneira de personalizar a revelar o foco é recusar os visuais de foco fornecido pelo sistema ao desenhar sua própria solução usando estados visuais. Para saber mais, consulte a [amostra de elementos visuais de foco](https://go.microsoft.com/fwlink/p/?LinkID=619895).


## <a name="reveal-focus-and-the-fluent-design-system"></a>Revelar o foco e o sistema de Design Fluent

Revele o que foco é um componente de sistema de Design Fluent que adiciona a luz ao seu aplicativo. Para saber mais sobre o sistema de Design Fluente e seus outros componentes, consulte a [visão geral do Design Fluente para UWP](../fluent-design-system/index.md).

## <a name="related-articles"></a>Artigos relacionados

- [Revelar realce](https://docs.microsoft.com/windows/uwp/design/style/reveal)
- [Projetando para TV e Xbox](/windows/uwp/design/devices/designing-for-tv)
- [Interações de Gamepad e controle remoto](https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions)
- [Amostra de elementos visuais de foco](https://go.microsoft.com/fwlink/p/?LinkID=619895)
- [Efeitos de composição](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [Ciência no sistema: Profundidade e Design Fluent](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [Ciência no sistema: Luz e Design Fluent](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
