---
Description: Saiba como Fluent movimento conceitos básicos se unem em seu aplicativo.
title: 'Movimento na prática: animação em aplicativos UWP'
label: Motion in practice
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8cf010533d2d62559bb8dc0d214e04ab917e62bd
ms.sourcegitcommit: d534f81590d881a18d677a648c59913029837a84
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67535431"
---
# <a name="bringing-it-together"></a>Reunião

O tempo, a suavização, a direção e a gravidade trabalham em conjunto para formar a base do movimento Fluente. Cada um deve ser considerado no contexto em relação ao outros e aplicado adequadamente no contexto do aplicativo.

Veja três maneiras de aplicar os conceitos básicos de movimento fluente em seu aplicativo.

:::row:::
    :::column:::
**Animação implícito** interpolação automática e o intervalo entre os valores em uma alteração de parâmetro para alcançar o movimento de Fluent muito simple usando os valores padronizados.
    :::column-end:::
    :::column:::
**Animação interno** componentes do sistema, como controles comuns e compartilhados de movimento, são "Fluent por padrão". Conceitos básicos foram aplicados de maneira consistente com o seu uso implícito.
    :::column-end:::
    :::column:::
**Seguindo as recomendações de diretrizes de animação personalizada** pode haver ocasiões quando o sistema ainda fornece uma solução de movimento exata para seu cenário. Nesses casos, use as recomendações de fundamentais de linha de base como ponto de partida para suas experiências.
    :::column-end:::
:::row-end:::

**Exemplo de transição**

![animação funcional](images/pageRefresh.gif)

:::row:::
    :::column:::
<b>Limite de encaminhamento de direção:</b><br>
Esmaecer: 150 milhões; Atenuação: Acelere padrão <b>direção para frente em:</b><br>
Deslize para cima 150px: 300 MS; Atenuação: Desacelerar padrão
    :::column-end:::
    :::column:::
<b>Direção para trás Out:</b><br>
Deslizar para baixo de 150px: 150 ms; Atenuação: Acelere padrão <b>direção para trás no:</b><br>
Aplicar fade-in: 300 MS; Atenuação: Desacelerar padrão
    :::column-end:::
:::row-end:::

**Exemplo de objeto**

 ![Movimento de 300 ms](images/control.gif)

:::row:::
    :::column:::
<b>Expanda a direção:</b><br>
Crescimento: 300 MS; Atenuação: Standard
    :::column-end:::
    :::column:::
<b>Direção de contrato:</b><br>
Crescimento: 150 ms; Atenuação: Acelerar o padrão
    :::column-end:::
:::row-end:::

## <a name="examples"></a>Exemplos

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Se você tiver o <strong style="font-weight: semi-bold">da Galeria de controles XAML</strong> aplicativo instalado, clique aqui para <a href="xamlcontrolsgallery:/item/ImplicitTransition">abrir o aplicativo e ver as transições implícitas em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="implicit-animations"></a>Animações implícitas

> Animações implícitas exigem o Windows 10, versão 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou posterior.

Animações implícitas são uma forma simple de obter o movimento Fluent pela interpolação automaticamente entre os valores novos e antigos durante uma alteração de parâmetro.

Implicitamente, você pode animar as alterações para as seguintes propriedades:

- [UIElement](/uwp/api/windows.ui.xaml.uielement)
  - **Opacidade**
  - **Rotação**
  - **Escala**
  - **Tradução**

- [Borda](/uwp/api/windows.ui.xaml.controls.border), [ContentPresenter](/uwp/api/windows.ui.xaml.controls.contentpresenter), ou [painel](/uwp/api/windows.ui.xaml.controls.panel)
  - **Tela de fundo**

Cada propriedade que pode ter alterações animadas implicitamente tem um correspondente _transição_ propriedade. Para animar a propriedade, você deve atribuir um tipo de transição para os respectivos _transição_ propriedade. Esta tabela mostra os _transição_ propriedades e o tipo de transição a ser usado para cada um deles.

| Propriedade animada | Propriedade de transição | Tipo de transição implícita |
| -- | -- | -- |
| [UIElement.Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) | [OpacityTransition](/uwp/api/windows.ui.xaml.uielement.opacitytransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Rotation](/uwp/api/windows.ui.xaml.uielement.rotation) | [RotationTransition](/uwp/api/windows.ui.xaml.uielement.rotationtransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Scale](/uwp/api/windows.ui.xaml.uielement.scale) | [ScaleTransition](/uwp/api/windows.ui.xaml.uielement.scaletransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [UIElement.Translation](/uwp/api/windows.ui.xaml.uielement.translation) | [TranslationTransition](/uwp/api/windows.ui.xaml.uielement.translationtransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [Border.Background](/uwp/api/windows.ui.xaml.controls.border.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.border.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [ContentPresenter.Background](/uwp/api/windows.ui.xaml.controls.contentpresenter.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.contentpresenter.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [Panel.Background](/uwp/api/windows.ui.xaml.controls.panel.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.panel.backgroundtransition)  | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |

Este exemplo mostra como usar a propriedade de opacidade e a transição para tornar um botão Aplicar fade-in quando o controle está habilitado e desaparecer quando ele estiver desabilitado.

```xaml
<Button x:Name="SubmitButton"
        Content="Submit"
        Opacity="{x:Bind OpaqueIfEnabled(SubmitButton.IsEnabled), Mode=OneWay}">
    <Button.OpacityTransition>
        <ScalarTransition />
    </Button.OpacityTransition>
</Button>
```

```csharp
public double OpaqueIfEnabled(bool IsEnabled)
{
    return IsEnabled ? 1.0 : 0.2;
}
```

## <a name="related-articles"></a>Artigos relacionados

- [Visão geral de animação](index.md)
- [Atingir o tempo e atenuação](timing-and-easing.md)
- [Direcionalidade e gravidade](directionality-and-gravity.md)
