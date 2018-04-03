---
author: cphilippona
description: O foco do Revelação é um efeito de iluminação que anima a borda de elementos focalizáveis quando o usuário move o foco do gamepad ou do teclado até eles.
title: Foco do Revelação
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
ms.localizationpriority: high
ms.openlocfilehash: 0a5f3dca3c8310bddbcd63c814d2d883151ff1f3
ms.sourcegitcommit: ef5a1e1807313a2caa9c9b35ea20b129ff7155d0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/08/2018
---
# <a name="reveal-focus"></a>Foco do Revelação

O foco do Revelação é um efeito de iluminação para [experiências de 3 metros](/windows/uwp/design/devices/designing-for-tv), como Xbox One e telas de televisão. Ele anima a borda de elementos focalizáveis, como botões, quando o usuário move o foco do gamepad ou do teclado até eles. Ele está desativado por padrão, mas é fácil de habilitar. 

(Para saber mais sobre o efeito de realce do Revelação, um efeito de iluminação que destaca elementos interativos, consulte o [artigo do realce do Revelação](/windows/uwp/design/style/reveal).)


> **APIs importantes**: [propriedade Application.FocusVisualKind](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_FocusVisualKind), [enumeração FocusVisualKind](https://docs.microsoft.com/uwp/api/windows.ui.xaml.focusvisualkind) e [propriedade Control.UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_UseSystemFocusVisuals)

## <a name="how-it-works"></a>Como funciona
O foco do Revelação chama atenção a elementos em foco adicionando um brilho animado ao redor da borda do elemento:

![Visual do Revelação](images/traveling-focus-fullscreen-light-rf.gif)

Isso é especialmente útil em cenários de 3 metros em que o usuário pode não estar prestando atenção total à tela de TV inteira. 

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/RevealFocus">abri-lo e ver o foco do Revelação em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-to-use-it"></a>Como usá-lo

O foco do Revelação está desativado por padrão. Para habilitá-lo:
1. No construtor do app, chame a propriedade [AnalyticsInfo.VersionInfo.DeviceFamily](/uwp/api/windows.system.profile.analyticsversioninfo#Windows_System_Profile_AnalyticsVersionInfo_DeviceFamily) e verifique se a família de dispositivos atual é `Windows.Xbox`.
2. Se a família de dispositivos for `Windows.Xbox`, defina a propriedade [Application.FocusVisualKind](/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_FocusVisualKind) como `FocusVisualKind.Reveal`. 

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

Após definir a propriedade **FocusVisualKind**, o sistema aplica automaticamente o efeito de foco do Revelação a todos os controles em que a propriedade [UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_UseSystemFocusVisuals) esteja definida como **True** (o valor padrão da maioria dos controles). 

## <a name="why-isnt-reveal-focus-on-by-default"></a>Por que o foco do Revelação não está ativado por padrão? 
Como você pode ver, é muito fácil ativar o foco do Revelação quando o app detecta que ele está sendo executado em um Xbox. Então, por que o sistema não o ativa para você? Porque o foco do Revelação aumenta o tamanho do elemento visual de foco, o que pode causar problemas com o layout da interface do usuário. Em alguns casos, pode ser interessante que você personalize o efeito de foco do Revelação para otimizá-lo no seu app.

## <a name="customizing-reveal-focus"></a>Personalizando o foco do Revelação

Você pode personalizar o efeito de foco do Revelação modificando as propriedades dos elementos visuais de foco para cada controle: [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualPrimaryThickness), [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualSecondaryThickness), [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualPrimaryBrush) e [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualSecondaryBrush). Essas propriedades permitem que você personalize a cor e a espessura do retângulo de foco. (Elas são as mesmas propriedades que você usa para criar [elementos visuais de foco de alta visibilidade](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#high-visibility-focus-visuals).) 

Mas, antes de começar a personalizar, vale a pena saber um pouco mais sobre os componentes que fazem parte do foco do Revelação.

Há três partes nos elementos visuais de foco do Revelação padrão: a borda principal, a borda secundária e o brilho do Revelação. A borda principal tem espessura de **2 px** e é moldada em torno da parte *externa* da borda secundária. A borda secundária tem espessura de **1 px** e é moldada em torno da parte *interna* da borda principal. O brilho do foco do Revelação tem espessura proporcional à espessura da borda principal e é moldado em torno da parte *externa* da borda principal.

Além dos elementos estáticos, os elementos visuais de foco do Revelação contam com uma luz animada que pulsa quando está em repouso e que se move na direção do foco quando o foco é movido.

![Camadas do foco do Revelação](images/reveal-breakdown.svg)

## <a name="customize-the-border-thickness"></a>Personalizar a espessura da borda

Para alterar a espessura dos tipos de borda de um controle, use estas propriedades:

| Tipo de borda | Propriedade |
| --- | --- |
| Principal, brilho   | [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualPrimaryThickness)<br/> (A alteração da borda principal também altera a espessura do brilho proporcionalmente.)   |
| Secundária   | [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualSecondaryThickness)   |


Este exemplo altera a espessura da borda de um botão do elemento visual de foco:

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1"/>
```

## <a name="customize-the-margin"></a>Personalizar a margem

A margem é o espaço entre os limites dos elementos visuais do controle e o início da borda secundária dos elementos visuais de foco. A margem padrão está a 1 px de distância dos limites do controle. Você pode editar essa margem em uma base por controle alterando a propriedade [FocusVisualMargin](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualMargin):

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1" FocusVisualMargin="-3"/>
```

Uma margem negativa move a borda para mais longe do centro do controle, e uma margem positiva move a borda para mais perto do centro do controle.

## <a name="customize-the-color"></a>Personalizar a cor

Para alterar a cor do elemento visual de foco do Revelação, use as propriedades [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualPrimaryBrush) e [FocusVisualSecondaryBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualSecondaryBrush).

| Propriedade | Recurso padrão | Valor do recurso padrão |
| ---- | ---- | --- | 
| [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualPrimaryBrush) | SystemControlRevealFocusVisualBrush  | SystemAccentColor |
| [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualSecondaryBrush)  | SystemControlFocusVisualSecondaryBrush  | SystemAltMediumColor |

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

    <!-- Override Reveal focus default resources. -->
    <SolidColorBrush x:Key="SystemControlRevealFocusVisualBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    <SolidColorBrush x:Key="SystemControlFocusVisualSecondaryBrush" Color="{ThemeResource SystemAccentColor}"/>
</Application.Resources>
```

Para obter mais informações sobre como modificar a cor do elemento visual de foco, consulte [Personalização e identidade visual de cores](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#color-branding--customizing).


## <a name="show-just-the-glow"></a>Mostrar apenas o brilho

Se você gostaria de usar apenas o brilho sem o elemento visual de foco principal ou secundário, basta definir a propriedade [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualPrimaryBrush) do controle como `Transparent` e [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualSecondaryThickness) como `0`. Nesse caso, o brilho adotará a cor da tela de fundo do controle para dar uma sensação de que não há borda. Você pode modificar a espessura do brilho usando [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualPrimaryThickness).

```xaml

<!-- Show just the glow -->
<Button
    FocusVisualPrimaryBrush="Transparent"
    FocusVisualSecondaryThickness="0" />    
```


## <a name="use-your-own-focus-visuals"></a>Use seus próprios elementos visuais de foco

Outra maneira de personalizar o foco do Revelação é recusar os elementos visuais de foco fornecidos pelo sistema desenhando seus próprios estados visuais de uso. Para saber mais, consulte a [amostra de elementos visuais de foco](http://go.microsoft.com/fwlink/p/?LinkID=619895).


## <a name="reveal-focus-and-the-fluent-design-system"></a>O foco do Revelação e o Sistema de Design Fluente

O foco do Revelação é um componente do Sistema de Design Fluente que adiciona iluminação ao seu app. Para saber mais sobre o sistema de Design Fluente e seus outros componentes, consulte a [visão geral do Design Fluente para UWP](../fluent-design-system/index.md).

## <a name="related-articles"></a>Artigos relacionados

- [Realce do Revelação](https://docs.microsoft.com/windows/uwp/design/style/reveal)
- [Projetando para Xbox e TV](/windows/uwp/design/devices/designing-for-tv)
- [Interações de gamepad e controle remoto](https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions)
- [Amostra de elementos visuais de foco](http://go.microsoft.com/fwlink/p/?LinkID=619895)
- [Efeitos de composição](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [Ciência no sistema: Design Fluente e profundidade](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [Ciência no sistema: Fluent Design e luz](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
