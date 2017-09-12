---
author: mijacobs
description: "Revelação é um novo modelo de interação que ajuda a adicionar mais foco e satisfação ao seu aplicativo."
title: "Revelação"
template: detail.hbs
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: kisai
design-contact: conrwi
dev-contact: jevansa
doc-status: Published
ms.openlocfilehash: d50e3f47faad5fff0ef461a4b5312127a0b9ec9c
ms.sourcegitcommit: 0d5b3daddb3ae74f91178c58e35cbab33854cb7f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2017
---
# <a name="reveal"></a>Revelação

> [!IMPORTANT]
> Este artigo descreve funcionalidades que ainda não foram lançadas e que podem ser modificadas substancialmente antes do lançamento comercial. A Microsoft não oferece nenhuma garantia, explícita ou implícita, com relação às informações fornecidas aqui.

Revelação é um efeito de iluminação que ajuda a trazer profundidade e foco para os elementos interativos do seu aplicativo.

> **APIs importantes**: [classe RevealBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush), [classe RevealBackgroundBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbackgroundbrush), [classe RevealBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealborderbrush), [classe RevealBrushHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrushhelper), [classe VisualState](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.VisualState)

O comportamento Revelação faz isso, revelando partes de elementos em torno de conteúdo heroico (ou focal) quando o mouse ou o retângulo de foco se encontra sobre as áreas desejadas.

![Visual do Revelação](images/Nav_Reveal_Animation.gif)

Por meio da exposição das bordas ocultas ao redor de objetos, o Revelação proporciona aos usuários uma melhor compreensão do espaço com o qual eles estão interagindo, ajudando-os a entender as ações disponíveis. Isso é especialmente importante em controles de lista e controles com backplates.

## <a name="reveal-and-the-fluent-design-system"></a>Revelação e o Sistema de Design Fluent

 O Sistema de Design Fluent ajuda você a criar uma interface do usuário arrojada e moderna que incorpora luz, profundidade, movimento, materiais e escala. Revelação é um componente do Sistema de Design Fluent que acrescenta luz ao seu aplicativo. 

## <a name="what-is-reveal"></a>O que é Revelação?

Existem dois componentes visuais principais no Revelação: o comportamento **Revelação Hover** e o comportamento **Revelação Borda**.

![Camadas do Revelação](images/RevealLayers.png)

A Revelação Hover está diretamente vinculada ao conteúdo que está sendo flutuado (via ponteiro ou entrada de foco), e aplica uma forma halo suave ao redor do item flutuado ou em foco, permitindo que você saiba que pode interagir com ele.

A Revelação Borda é aplicada ao item em foco e aos itens próximos. Isso mostra que esses objetos nas proximidades podem executar ações semelhantes às do objeto em foco.

A divisão de receitas da Revelação é:

- A Revelação Borda ficará em cima de todo o conteúdo, mas nas bordas designadas
- Texto e conteúdo serão exibidos logo abaixo da Revelação Borda
- A Revelação Hover ficará abaixo do conteúdo e do texto
- O backplate (que ativa e habilita a Revelação Hover)
- O plano de fundo (plano de fundo do controle)

<!--
<div class=”microsoft-internal-note”>
To create your own Reveal lighting effect for static comps or prototype purposes, see the full [uni design guidance](http://uni/DesignDepot.FrontEnd/#/ProductNav/3020/1/dv/?t=Resources%7CToolkit%7CReveal&f=Neon) for this effect in illustrator.
</div>
-->

## <a name="how-to-use-it"></a>Como usá-la

A Revelação expõe a geometria em torno de seu cursor quando necessário, e faz com que ela desapareça suavemente assim que você se mova.

A Revelação é melhor usada quando habilitada no conteúdo principal do seu aplicativo (conteúdo heróico), os quais possuem possui limites implícitos e backplates. A Revelação deve, também, ser usada em controles de coleção ou de lista.

## <a name="controls-that-automatically-use-reveal"></a>Controles que usam Revelação automaticamente

- [**ListView**](../controls-and-patterns/lists.md)
- [**TreeView**](../controls-and-patterns/tree-view.md)
- [**NavigationView**](../controls-and-patterns/navigationview.md)
- [**AutosuggestBox**](../controls-and-patterns/auto-suggest-box.md)

## <a name="enabling-reveal-on-other-common-controls"></a>Habilitando Revelação em outros controles comuns

Se você tiver um cenário onde deve ser aplicada Revelação (esses controles são conteúdo principal e/ou são usados em uma orientação de lista ou coleção), fornecemos estilos de recurso opcionais que permitem habilitar a Revelação para esses tipos de situações.

Esses controles não têm a Revelação por padrão, uma vez que são controles menores que geralmente são ajudantes dos principais pontos de foco da sua aplicação; no entanto cada aplicativo é diferente, e se esses controles são os mais utilizados em seu aplicativo, nós criamos alguns estilos para auxiliá-lo com isso:

| Nome do Controle   | Nome do Recurso |
|----------|:-------------:|
| Botão |  ButtonRevealStyle |
| ToggleButton | ToggleButtonRevealStyle |
| RepeatButton | RepeatButtonRevealStyle |
| AppBarButton | AppBarButtonRevealStyle |
| SemanticZoom | SemanticZoomRevealStyle |
| ComboBoxItem | ComboxBoxItemRevealStyle |

Para aplicar esses estilos, basta atualizar a propriedade Estilo da seguinte forma:

```XAML
<Button Content="Button Content" Style="{StaticResource ButtonRevealStyle}"/>
```

> [!NOTE]
> A versão 16190 do SDK não disponibiliza esses estilos automaticamente. Para obtê-los, você precisa copiá-los manualmente do generic.xaml em seu aplicativo. Em geral, o eneric.xaml localiza-se em C:\Program Files (x86)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\10.0.16190.0\Generic. Esse problema será corrigido em uma compilação posterior. 

## <a name="enabling-reveal-on-custom-controls"></a>Habilitando a Revelação em controles personalizados

Para habilitar a Revelação em controles personalizados ou controles remodelados, você pode ir para o estilo do controle em questão, no Estado Visual do modelo desse controle, especifique Revelação na grade raiz:

```xaml
<VisualState x:Name="PointerOver">
  <VisualState.Setters>
    <Setter Target="RootGrid.(RevealBrushHelper.State)" Value="PointerOver" />
    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPointerOver}"/>
    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBorderBrushPointerOver}"/>
    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPointerOver}"/>
  </VisualState.Setters>
</VisualState>
```

Para obter o efeito de atração da Revelação, você precisará adicionar a mesma RevealBrushHelper para o PressedState também:

```xaml
<VisualState x:Name="Pressed">
  <VisualState.Setters>
    <Setter Target="RootGrid.(RevealBrushHelper.State)" Value="Pressed" />
    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPressed}"/>
    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBorderBrushPressed}"/>
    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPressed}"/>
  </VisualState.Setters>
</VisualState>
```


Usar nosso recurso do sistema Revelação significa que iremos manipular todas as alterações de tema para você.

## <a name="dos-and-donts"></a>O que fazer e o que não fazer
- Colocar Revelação nos elementos em que o usuário pode executar ações sobre - Revelação não deve estar em conteúdo estático
- Mostrar Revelação em listas ou coleções de dados
- Aplicar Revelação ao conteúdo claramente confinado com backplates
- Não mostre Revelação em telas de fundo estáticas, texto ou imagens com as quais você não possa interagir
- Não aplicar Revelação em conteúdo flutuante não relacionado
- Não use Revelação em situações únicas, isoladas, como diálogos de conteúdo ou notificações
- Não use Revelação em decisões de segurança, uma vez que ela pode tirar a atenção da mensagem que você deseja entregar ao usuário

## <a name="related-articles"></a>Artigos relacionados

- [Classe RevealBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)
- [**Acrílico**](acrylic.md)
- [**Efeitos de Composição**](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
