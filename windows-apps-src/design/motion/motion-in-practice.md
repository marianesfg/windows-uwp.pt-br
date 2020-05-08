---
Description: Saiba como os conceitos básicos de movimentos fluentes se reúnem em seu aplicativo.
title: Movimento na prática – animação em aplicativos do Windows
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
ms.openlocfilehash: 45ab6c593b9e20f778e4b352a8b284cefe57c9a8
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970321"
---
# <a name="bringing-it-together"></a>Reunião

O tempo, a suavização, a direção e a gravidade trabalham em conjunto para formar a base do movimento Fluente. Cada um deve ser considerado no contexto em relação ao outros e aplicado adequadamente no contexto do aplicativo.

Veja três maneiras de aplicar os conceitos básicos de movimento fluente em seu aplicativo.

:::row:::
    :::column:::
**Animação implícita** A interpolação e o tempo automáticos entre valores em um parâmetro mudam para obter um movimento fluente muito simples usando os valores padronizados.
    :::column-end:::
    :::column:::
**Animação interna** Os componentes do sistema, como controles comuns e movimentos compartilhados, são "Fluent por padrão". Os conceitos básicos foram aplicados de maneira consistente com o uso implícito.
    :::column-end:::
    :::column:::
**Personalizar Animação seguindo as recomendações de orientação** Pode haver ocasiões em que o sistema ainda não fornece uma solução de movimento exata para seu cenário. Nesses casos, use as recomendações fundamentais de linha de base como um ponto de partida para suas experiências.
    :::column-end:::
:::row-end:::

**Exemplo de transição**

![animação funcional](images/pageRefresh.gif)

:::row:::
    :::column:::
<b>Encaminhamento de direção:</b><br>
Fade out: 150m; Atenuação: direção de aceleração padrão para <b>frente:</b><br>
Deslizar para cima 150px: 300 MS; Atenuação: desaceleração padrão
    :::column-end:::
    :::column:::
<b>Direção de retrocesso:</b><br>
Deslizar para baixo 150px: 150ms; Atenuação: a direção de aceleração padrão é <b>regressiva em:</b><br>
Fade in: 300 MS; Atenuação: desaceleração padrão
    :::column-end:::
:::row-end:::

**Exemplo de objeto**

 ![Movimento de 300 ms](images/control.gif)

:::row:::
    :::column:::
<b>Expansão de direção:</b><br>
Crescimento: 300 MS; Atenuação: padrão
    :::column-end:::
    :::column:::
<b>Contrato de direção:</b><br>
Crescimento: 150ms; Atenuação: aceleração padrão
    :::column-end:::
:::row-end:::

## <a name="examples"></a>Exemplos

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Se você tiver o aplicativo da <strong style="font-weight: semi-bold">Galeria de controles XAML</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/ImplicitTransition">abrir o aplicativo e ver as transições implícitas em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="implicit-animations"></a>Animações implícitas

> Animações implícitas exigem o Windows 10, versão 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou posterior.

Animações implícitas são uma maneira simples de alcançar um movimento fluente, interpolando automaticamente entre os valores antigos e novos durante uma alteração de parâmetro.

Você pode animar implicitamente as alterações nas seguintes propriedades:

- [UIElement](/uwp/api/windows.ui.xaml.uielement)
  - **Opacidade**
  - **Rotação**
  - **Escala**
  - **Tradução**

- [Borda](/uwp/api/windows.ui.xaml.controls.border), [ContentPresenter](/uwp/api/windows.ui.xaml.controls.contentpresenter)ou [painel](/uwp/api/windows.ui.xaml.controls.panel)
  - **Informações**

Cada propriedade que pode ter alterações animadas implicitamente tem uma propriedade de _transição_ correspondente. Para animar a propriedade, atribua um tipo de transição à propriedade de _transição_ correspondente. Esta tabela mostra as propriedades de _transição_ e o tipo de transição a ser usado para cada uma delas.

| Propriedade animada | Propriedade Transition | Tipo de transição implícita |
| -- | -- | -- |
| [UIElement. Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) | [OpacityTransition](/uwp/api/windows.ui.xaml.uielement.opacitytransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Rotation](/uwp/api/windows.ui.xaml.uielement.rotation) | [RotationTransition](/uwp/api/windows.ui.xaml.uielement.rotationtransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Scale](/uwp/api/windows.ui.xaml.uielement.scale) | [ScaleTransition](/uwp/api/windows.ui.xaml.uielement.scaletransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [UIElement.Translation](/uwp/api/windows.ui.xaml.uielement.translation) | [TranslationTransition](/uwp/api/windows.ui.xaml.uielement.translationtransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [Borda. plano de fundo](/uwp/api/windows.ui.xaml.controls.border.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.border.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [ContentPresenter. Background](/uwp/api/windows.ui.xaml.controls.contentpresenter.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.contentpresenter.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [Painel. plano de fundo](/uwp/api/windows.ui.xaml.controls.panel.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.panel.backgroundtransition)  | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |

Este exemplo mostra como usar a propriedade Opacity e a transição para fazer com que um botão apareça quando o controle estiver habilitado e desaparecer quando for desabilitado.

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

- [Visão geral do movimento](index.md)
- [Tempo e suavização](timing-and-easing.md)
- [Direcionalidade e gravidade](directionality-and-gravity.md)
