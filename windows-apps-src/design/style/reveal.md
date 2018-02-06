---
author: mijacobs
description: "Revelação é um efeito de iluminação que ajuda a trazer profundidade e foco para os elementos interativos do seu aplicativo."
title: "Realce do Revelação"
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
ms.localizationpriority: high
ms.openlocfilehash: 8ba0d9939d7ab1d9826ed2848e476499f09c628f
ms.sourcegitcommit: 4b522af988273946414a04fbbd1d7fde40f8ba5e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2018
---
# <a name="reveal-highlight"></a>Realce do Revelação

Revelação é um efeito de iluminação que ajuda a trazer profundidade e foco para os elementos interativos do seu aplicativo.

> **APIs importantes**: [classe RevealBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush), [classe RevealBackgroundBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbackgroundbrush), [classe RevealBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealborderbrush), [classe RevealBrushHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrushhelper), [classe VisualState](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.VisualState)

O comportamento Revelação faz isso revelando o contêiner de conteúdo clicável quando o ponteiro está próximo.

![Visual do Revelação](images/Nav_Reveal_Animation.gif)

Por meio da exposição das bordas ocultas ao redor de objetos, o Revelação proporciona aos usuários uma melhor compreensão do espaço com o qual eles estão interagindo, ajudando-os a entender as ações disponíveis. Isso é especialmente importante em controles de lista e agrupamentos de botões.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem um aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/Reveal">abrir o aplicativo e ver o Revelação em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obter o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obter o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>Resumo do vídeo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev013/player]

## <a name="reveal-and-the-fluent-design-system"></a>Revelação e o Sistema de Design Fluent

 O Sistema de Design Fluent ajuda você a criar uma interface do usuário arrojada e moderna que incorpora luz, profundidade, movimento, materiais e escala. Revelação é um componente do Sistema de Design Fluent que acrescenta luz ao seu aplicativo. Para saber mais, consulte a [visão geral do Fluent Design para UWP](../fluent-design-system/index.md).

## <a name="how-to-use-it"></a>Como usá-lo

O Revelação funciona automaticamente para alguns controles. Para outros controles, você pode habilitar o Revelação atribuindo um estilo especial para o controle.

## <a name="controls-that-automatically-use-reveal"></a>Controles que usam Revelação automaticamente

- [**ListView**](../controls-and-patterns/lists.md)
- [**GridView**](../controls-and-patterns/lists.md)
- [**TreeView**](../controls-and-patterns/tree-view.md)
- [**NavigationView**](../controls-and-patterns/navigationview.md)
- [**AutosuggestBox**](../controls-and-patterns/auto-suggest-box.md)
- [**MediaTransportControl**](../controls-and-patterns/media-playback.md)
- [**CommandBar**](../controls-and-patterns/app-bars.md)
- [**ComboBox**](../controls-and-patterns/lists.md)

Estas ilustrações mostram o efeito de revelação em vários controles diferentes:

![Exemplos do Revelação](images/RevealExamples_Collage.png)

## <a name="enabling-reveal-on-other-common-controls"></a>Habilitando Revelação em outros controles comuns

Se você tiver um cenário onde deve ser aplicado Revelação (esses controles são conteúdo principal e/ou são usados em uma orientação de lista ou coleção), fornecemos estilos de recurso opcionais que permitem habilitar a Revelação para esses tipos de situações.

Esses controles não têm a Revelação por padrão, uma vez que são controles menores que geralmente são ajudantes dos principais pontos de foco da sua aplicação; no entanto cada aplicativo é diferente, e se esses controles são os mais utilizados em seu aplicativo, nós criamos alguns estilos para auxiliá-lo com isso:

| Nome do Controle   | Nome do Recurso |
|----------|:-------------:|
| Botão |  ButtonRevealStyle |
| ToggleButton | ToggleButtonRevealStyle |
| RepeatButton | RepeatButtonRevealStyle |
| AppBarButton | AppBarButtonRevealStyle |
| SemanticZoom | SemanticZoomRevealStyle |

Para aplicar esses estilos, basta atualizar a propriedade Estilo da seguinte forma:

```XAML
<Button Content="Button Content" Style="{StaticResource ButtonRevealStyle}"/>
```

## <a name="enabling-reveal-on-custom-controls"></a>Habilitando Revelação em controles personalizados

Quando o assunto é decidir se o controle personalizado deve ou não ter Revelação, como regra geral, é preciso ter um agrupamento de elementos interativos relacionados a um recurso ou ação abrangente que você deseja realizar no aplicativo.

Por exemplo, os itens do NavigationView estão relacionados à navegação de página. Os botões do CommandBar estão relacionados a ações do menu ou ações de recurso de página. Os botões do MediaTransportControl abaixo relacionam-se à mídia sendo reproduzida.

Os controles que obtém o Revelação não precisam estar relacionados entre si; eles só precisam estar em uma área de alta densidade e servir um objetivo maior.

Para habilitar o Revelação em controles personalizados ou controles remodelados, você pode ir para o estilo do controle em questão, no Estado Visual do modelo desse controle, e especificar Revelação na grade raiz:

```xaml
<VisualState x:Name="PointerOver">
    <VisualState.Setters>
        <Setter Target="RootGrid.(RevealBrush.State)" Value="PointerOver" />
        <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPointerOver}" />
        <Setter Target="ContentPresenter.BorderBrush" Value="Transparent"/>
        <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPointerOver}" />
    </VisualState.Setters>
</VisualState>
```

É importante observar que o Revelação precisa de pincel e setters em seu Estado Visual para funcionar plenamente. Apenas definir um pincel do controle como um dos nossos recursos de pincel do Revelação não habilitará o Revelação para esse controle. Por outro lado, ter apenas os destinos ou as configurações sem os valores serem pincéis do Revelação também não habilitará o Revelação.

Criamos um conjunto de pincéis de Revelação do sistema que você pode usar para personalizar sua interface do usuário. Por exemplo, você pode usar o pincel **ButtonRevealBackground** para criar um plano de fundo de botão personalizado ou o pincel **ListViewItemRevealBackground** para listas personalizadas e assim por diante.

(Para saber mais sobre como os recursos funcionam em XAMl, confira o artigo [Dicionário de recursos XAML](../controls-and-patterns/resourcedictionary-and-xaml-resource-references.md).)

### <a name="reveal-on-listview-controls-with-nested-buttons"></a>Revelação em controles ListView com botões aninhados

Se tiver um [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) e também tiver botões ou conteúdo invocável aninhados dentro dos seus elementos [ListViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewitem), você deve habilitar o Revelação para os itens aninhados.

No caso de botões ou controles semelhantes a botões em um ListViewItem, basta definir a propriedade [Estilo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_Style) do controle como o recurso estático**ButtonRevealStyle**. 

![Revelação aninhado](images/NestedListContent.png)

Esse exemplo permite Revelação em vários botões dentro de um ListViewItem. 

```XAML
<ListViewItem>
    <StackPanel Orientation="Horizontal">
        <TextBlock Margin="5">Test Text: lorem ipsum.</TextBlock>
        <StackPanel Orientation="Horizontal">
            <Button Content="&#xE71B;" FontFamily="Segoe MDL2 Assets" Width="45" Height="45" Margin="5" Style="{StaticResource ButtonRevealStyle}"/>
            <Button Content="&#xE728;" FontFamily="Segoe MDL2 Assets" Width="45" Height="45" Margin="5" Style="{StaticResource ButtonRevealStyle}"/>
            <Button Content="&#xE74D;" FontFamily="Segoe MDL2 Assets" Width="45" Height="45" Margin="5" Style="{StaticResource ButtonRevealStyle}"/>
         </StackPanel>
    </StackPanel>
</ListViewItem>
```

### <a name="listviewitempresenter-with-reveal"></a>ListViewItemPresenter com Revelação

As listas são um pouco especiais em XAML e, no caso do Revelação, será preciso definir o Gerenciador de Estado Visual para o Revelação apenas dentro do ListViewItemPresenter:

```XAML
<ListViewItemPresenter>
<!-- ContentTransitions, SelectedForeground, etc. properties -->
RevealBackground="{ThemeResource ListViewItemRevealBackground}"
RevealBorderThickness="{ThemeResource ListViewItemRevealBorderThemeThickness}"
RevealBorderBrush="{ThemeResource ListViewItemRevealBorderBrush}">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
        <VisualState x:Name="Normal" />
        <VisualState x:Name="Selected" />
        <VisualState x:Name="PointerOver">
            <VisualState.Setters>
                <Setter Target="Root.(RevealBrush.State)" Value="PointerOver" />
            </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="PointerOverSelected">
                <VisualState.Setters>
                    <Setter Target="Root.(RevealBrush.State)" Value="PointerOver" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="PointerOverPressed">
                <VisualState.Setters>
                    <Setter Target="Root.(RevealBrush.State)" Value="Pressed" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Pressed">
                <VisualState.Setters>
                    <Setter Target="Root.(RevealBrush.State)" Value="Pressed" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="PressedSelected">
                <VisualState.Setters>
                    <Setter Target="Root.(RevealBrush.State)" Value="Pressed" />
                </VisualState.Setters>
            </VisualState>
            </VisualStateGroup>
                <VisualStateGroup x:Name="EnabledGroup">
                    <VisualState x:Name="Enabled" />
                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                             <Setter Target="Root.RevealBorderThickness" Value="0"/>
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</ListViewItemPresenter>
```

Isso significa que se deve anexar ao fim da coleção de propriedades no ListViewItemPresenter com os setters específicos de Estado Visual.

### <a name="full-template-example"></a>Exemplo de modelo completo

Veja um modelo inteiro sobre a aparência de um botão Revelação:

```XAML
<Style TargetType="Button" x:Key="ButtonStyle1">
    <Setter Property="Background" Value="{ThemeResource ButtonRevealBackground}" />
    <Setter Property="Foreground" Value="{ThemeResource ButtonForeground}" />
    <Setter Property="BorderBrush" Value="{ThemeResource ButtonRevealBorderBrush}" />
    <Setter Property="BorderThickness" Value="2" />
    <Setter Property="Padding" Value="8,4,8,4" />
    <Setter Property="HorizontalAlignment" Value="Left" />
    <Setter Property="VerticalAlignment" Value="Center" />
    <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}" />
    <Setter Property="FontWeight" Value="Normal" />
    <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}" />
    <Setter Property="UseSystemFocusVisuals" Value="True" />
    <Setter Property="FocusVisualMargin" Value="-3" />
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="Button">
                <Grid x:Name="RootGrid" Background="{TemplateBinding Background}">

                    <VisualStateManager.VisualStateGroups>
                        <VisualStateGroup x:Name="CommonStates">
                            <VisualState x:Name="Normal">

                                <Storyboard>
                                    <PointerUpThemeAnimation Storyboard.TargetName="RootGrid" />
                                </Storyboard>
                            </VisualState>

                            <VisualState x:Name="PointerOver">
                                <VisualState.Setters>
                                    <Setter Target="RootGrid.(RevealBrush.State)" Value="PointerOver" />
                                    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPointerOver}" />
                                    <Setter Target="ContentPresenter.BorderBrush" Value="Transparent"/>
                                    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPointerOver}" />
                                </VisualState.Setters>

                                <Storyboard>
                                    <PointerUpThemeAnimation Storyboard.TargetName="RootGrid" />
                                </Storyboard>
                            </VisualState>

                            <VisualState x:Name="Pressed">
                                <VisualState.Setters>
                                    <Setter Target="RootGrid.(RevealBrush.State)" Value="Pressed" />
                                    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPressed}" />
                                    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBackgroundPressed}" />
                                    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPressed}" />
                                </VisualState.Setters>

                                <Storyboard>
                                    <PointerDownThemeAnimation Storyboard.TargetName="RootGrid" />
                                </Storyboard>
                            </VisualState>

                            <VisualState x:Name="Disabled">
                                <VisualState.Setters>
                                    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundDisabled}" />
                                    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBorderBrushDisabled}" />
                                    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundDisabled}" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>

              </VisualStateManager.VisualStateGroups>
                    <ContentPresenter x:Name="ContentPresenter"
                    BorderBrush="{TemplateBinding BorderBrush}"
                    BorderThickness="{TemplateBinding BorderThickness}"
                    Content="{TemplateBinding Content}"
                    ContentTransitions="{TemplateBinding ContentTransitions}"
                    ContentTemplate="{TemplateBinding ContentTemplate}"
                    Padding="{TemplateBinding Padding}"
                    HorizontalContentAlignment="{TemplateBinding HorizontalContentAlignment}"
                    VerticalContentAlignment="{TemplateBinding VerticalContentAlignment}"
                    AutomationProperties.AccessibilityView="Raw" />
                </Grid>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

## <a name="dos-and-donts"></a>O que fazer e o que não fazer
- Use o Revelação nos elementos em que o usuário pode executar uma ação (botões, seleções)
- Use o Revelação em agrupamentos de elementos interativos que não têm separadores visuais por padrão (listas, barras de comandos)
- Use o Revelação em áreas com uma grande quantidade de elementos interativos
- Não use o Revelação em conteúdo estático (telas de fundo, texto)
- Não use o Revelação em situações únicas, isoladas
- Não use o Revelação em itens muito grandes (com mais de 500epx)
- Não use Revelação em decisões de segurança, uma vez que ela pode tirar a atenção da mensagem que você deseja entregar ao usuário

## <a name="how-we-designed-reveal"></a>Como projetamos o Revelação

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

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Amostra do XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

- [Classe RevealBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)
- [Acrílico](acrylic.md)
- [Efeitos de Composição](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [Fluent Design para UWP](../fluent-design-system/index.md)
- [Ciência no sistema: Fluent Design e profundidade](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [Ciência no sistema: Fluent Design e luz](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
