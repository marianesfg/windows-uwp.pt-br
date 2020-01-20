---
description: A Revelação é um efeito de iluminação que ajuda a trazer profundidade e foco para os elementos interativos do seu aplicativo.
title: Realce de Revelação
template: detail.hbs
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: conrwi
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 63a7ee8550b72356199645f54b587480275c2bcd
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685078"
---
# <a name="reveal-highlight"></a>Realce de Revelação

![imagem Hero](images/header-reveal-highlight.svg)

O Realce da Revelação é um efeito de iluminação que destaca os elementos interativos, como barras de comandos, quando o usuário move o ponteiro perto deles. 

> **APIs importantes**: [classe RevealBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush), [classe RevealBackgroundBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbackgroundbrush), [classe RevealBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealborderbrush), [classe RevealBrushHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrushhelper), [classe VisualState](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState)

## <a name="how-it-works"></a>Como funciona
O Realce da Revelação chama a atenção para elementos interativos, revelando o contêiner do elemento quando o ponteiro está próximo, como mostrado nesta ilustração:

![Visual da Revelação](images/Nav_Reveal_Animation.gif)

Por meio da exposição das bordas ocultas ao redor de objetos, a Revelação proporciona aos usuários uma melhor compreensão do espaço com o qual eles estão interagindo, ajudando-os a entender as ações disponíveis. Isso é especialmente importante em controles de lista e agrupamentos de botões.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem um aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/Reveal">abrir o aplicativo e ver a Revelação em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>Resumo do vídeo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev013/player]

## <a name="how-to-use-it"></a>Como usá-lo

A Revelação funciona automaticamente para alguns controles. Para outros controles, você pode habilitar a Revelação atribuindo um estilo especial ao controle, como descrito nas seções [Como habilitar a Revelação em outros controles](#enabling-reveal-on-other-controls) e [Como habilitar a Revelação em controles personalizados](#enabling-reveal-on-custom-controls) deste artigo.

## <a name="controls-that-automatically-use-reveal"></a>Controles que usam a Revelação automaticamente

- [**ListView**](../controls-and-patterns/lists.md)
- [**GridView**](../controls-and-patterns/lists.md)
- [**TreeView**](../controls-and-patterns/tree-view.md)
- [**NavigationView**](../controls-and-patterns/navigationview.md)
- [**MediaTransportControl**](../controls-and-patterns/media-playback.md)
- [**CommandBar**](../controls-and-patterns/app-bars.md)

Estas ilustrações mostram Realce da Revelação em vários controles diferentes:

![Exemplos da Revelação](images/RevealExamples_Collage.png)


## <a name="enabling-reveal-on-other-controls"></a>Como habilitar a Revelação em outros controles

Se você tiver um cenário onde deve ser aplicada a Revelação (esses controles são conteúdo principal e/ou são usados em uma orientação de lista ou coleção), fornecemos estilos de recurso opcionais que permitem habilitar a Revelação para esses tipos de situações.

Esses controles não têm a Revelação por padrão, uma vez que são controles menores que geralmente são ajudantes dos principais pontos de foco da sua aplicação; no entanto cada aplicativo é diferente, e se esses controles são os mais utilizados em seu aplicativo, nós criamos alguns estilos para auxiliá-lo com isso:

| Nome do controle   | Nome do Recurso |
|----------|:-------------:|
| Botão |  ButtonRevealStyle |
| ToggleButton | ToggleButtonRevealStyle |
| RepeatButton | RepeatButtonRevealStyle |
| AppBarButton | AppBarButtonRevealStyle |
| AppBarToggleButton | AppBarToggleButtonRevealStyle |
| GridViewItem (Revelação sobre o conteúdo) | GridViewItemRevealBackgroundShowsAboveContentStyle |

Para aplicar estes estilos, basta definir a propriedade [Style](/uwp/api/Windows.UI.Xaml.Style) do controle:

```xaml
<Button Content="Button Content" Style="{ThemeResource ButtonRevealStyle}"/>
```

### <a name="reveal-in-themes"></a>Revelação em temas

A Revelação muda um pouco de acordo com o tema solicitado da configuração do usuário, do controle ou do aplicativo. No tema Escuro, a Borda e a Luz de foco da Revelação são brancas, mas, no tema Claro, apenas as Bordas são escurecidas para cinza-claro.

![Revelação Escura e Clara](images/Dark_vs_LightReveal.png)

Para habilitar as bordas brancas enquanto estiver no tema claro, basta definir o tema solicitado no controle como Escuro.

```xaml
<Grid RequestedTheme="Dark">
    <Button Content="Button" Click="Button_Click" Style="{ThemeResource ButtonRevealStyle}"/>
</Grid>
```

Ou altere o TargetTheme no RevealBorderBrush para Escuro. Lembre-se: se o TargetTheme for definido como Escuro, a Revelação será branco. Mas, se ele estiver definido como Claro, as bordas da Revelação serão cinzas.

```xaml
 <RevealBorderBrush x:Key="MyLightBorderBrush" TargetTheme="Dark" Color="{ThemeResource SystemAccentColor}" FallbackColor="{ThemeResource SystemAccentColor}" />
```

## <a name="enabling-reveal-on-custom-controls"></a>Como habilitar a Revelação em controles personalizados

Você pode adicionar a Revelação a controles personalizados. Antes de fazer isso, é importante saber um pouco mais sobre como o efeito da Revelação funciona. A Revelação é composta por dois efeitos separados: **Borda de Revelação** e **Luz de foco de Revelação**.

- **Borda** mostra as bordas dos elementos interativos quando um ponteiro está próximo. Esse efeito mostra que esses objetos próximos podem executar ações semelhantes às do objeto em foco.
- **Luz de foco** aplica uma forma de halo suave ao redor do item focalizado ou focado e executa uma animação ao clicar. 

![Camadas de Revelação](images/RevealLayers.png)

<!-- The Reveal recipe breakdown is:

- Border reveal will be on top of all content but on the designated edges
- Text and content will be displayed directly under Border Reveal
- Hover reveal will be beneath content and text
- The backplate (that turns on and enables Hover Reveal)
- The background (background of control) -->


Esses efeitos são definidos por dois pincéis: 
* A Borda de Revelação é definido por **RevealBorderBrush**
* A Luz de foco de Revelação é definido por **RevealBackgroundBrush**

```xaml
<RevealBorderBrush x:Key="MyRevealBorderBrush" TargetTheme="Light" Color="{ThemeResource SystemAccentColor}" FallbackColor="{ThemeResource SystemAccentColor}"/>
<RevealBackgroundBrush x:Key="MyRevealBackgroundBrush" TargetTheme="Light" Color="{StaticResource SystemAccentColor}" FallbackColor="{StaticResource SystemAccentColor}" />
```
Na maioria dos casos, lidamos com o uso de ambos ativando a Revelação automaticamente para determinados controles. No entanto, outros controles precisarão ser habilitados por meio da aplicação de um estilo ou da alteração de seus modelos diretamente.

### <a name="when-to-add-reveal"></a>Quando adicionar a Revelação
Você pode adicionar a Revelação aos seus controles personalizados. Mas, antes disso, considere o tipo de controle e como ele se comporta. 
* Se seu controle personalizado é um único elemento interativo e não tem controles semelhantes que compartilham seu espaço (por exemplo, itens de menu em um menu), é provável que o controle personalizado não precise da Revelação.  
* Se você tiver um agrupamento de conteúdo ou elementos interativos relacionados, é provável que essa região do seu aplicativo precise da Revelação. Isso é conhecido normalmente como uma superfície de [comando](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/collection-commanding).

Por exemplo, um botão isolado não deve usar a Revelação, mas um conjunto de botões em uma barra de comandos deve usar a Revelação.

<!-- For example, NavigationView's items are related to page navigation. CommandBar's buttons relate to menu actions or page feature actions. MediaTransportControl's buttons beneath all relate to the media being played. -->

### <a name="using-the-control-template-to-add-reveal"></a>Como usar o modelo de controle para adicionar a Revelação 
Para habilitar a Revelação em controles personalizados ou controles remodelados, você deve modificar o modelo de controle do controle. A maioria dos modelos de controle tem uma grade na raiz. Atualize o [VisualState](/uwp/api/windows.ui.xaml.visualstate) dessa grade raiz para usar a Revelação:

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

É importante observar que a Revelação precisa do pincel e dos setters em seu Estado Visual para funcionar corretamente. A simples definição de um pincel do controle como um dos recursos de pincel da Revelação não habilitará a Revelação para esse controle. Por outro lado, apenas os destinos ou as configurações sem os valores como pincéis da Revelação também não habilitarão a Revelação.

Para saber mais sobre como modificar modelos de controle, confira o artigo [Modelos de controle XAML](../controls-and-patterns/control-templates.md).

Criamos um conjunto de pincéis da Revelação do sistema que você pode usar para personalizar o seu modelo. Por exemplo, você pode usar o pincel **ButtonRevealBackground** para criar um plano de fundo de botão personalizado ou o pincel **ListViewItemRevealBackground** para listas personalizadas e assim por diante. (Para saber mais sobre como os recursos funcionam em XAML, confira o artigo [Dicionário de recursos XAML](../controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)).

### <a name="full-template-example"></a>Exemplo de modelo completo

Veja a seguir um modelo completo da aparência de um botão da Revelação:

```xaml
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

### <a name="fine-tuning-the-reveal-effect-on-a-custom-control"></a>Como ajustar o efeito da Revelação em um controle personalizado 

Ao habilitar a Revelação em um controle personalizado ou remodelado ou em uma superfície de comando personalizada, estas dicas podem ajudar a otimizar o efeito:
 
* Nos itens adjacentes com tamanhos que não estão alinhados na altura ou largura (especialmente em listas): remova o comportamento de abordagem da borda e mantenha as bordas mostradas somente no foco.
* Para itens de comando que entram e saem do estado desabilitado com frequência: coloque o pincel de abordagem da borda nos backplates dos elementos, bem como em suas bordas para enfatizar seu estado.
* Para elementos de comandos adjacentes que estão tão próximos que chegam a se tocar: adicione uma margem de 1px entre os dois elementos. 

## <a name="dos-and-donts"></a>O que fazer e o que não fazer
### <a name="do"></a>Você deve
- Usar a Revelação nos elementos em que o usuário pode executar muitas ações (CommandBars, menus de navegação)
- Usar a Revelação em agrupamentos de elementos interativos que não têm separadores visuais por padrão (listas, faixas de opções)
- Usar a Revelação em áreas com uma grande quantidade de elementos interativos (cenários de comando)
- Colocar espaços de margem de 1px entre os itens da Revelação

### <a name="dont"></a>O que não fazer
- Não usar a Revelação em conteúdo estático (telas de fundo, texto)
- Não usar a Revelação em pop-ups, submenus ou listas suspensas
- Não usar a Revelação em situações únicas e isoladas
- Não usar a Revelação em itens muito grandes (com mais de 500epx)
- Não usar a Revelação em decisões de segurança, uma vez que ele pode tirar a atenção da mensagem que você precisa transmitir ao usuário


## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery) – veja todos os controles XAML em um formato interativo.

## <a name="reveal-and-the-fluent-design-system"></a>Revelação e o Sistema de Design Fluente

 O Sistema de Design Fluente ajuda a criar uma interface do usuário arrojada e moderna que incorpora luz, profundidade, movimento, materiais e escala. Revelação é um componente do Sistema de Design Fluente que acrescenta luz ao seu aplicativo. Para saber mais, confira a [Visão geral do Design Fluente para UWP](/windows/apps/fluent-design-system).

## <a name="related-articles"></a>Artigos relacionados

- [Classe RevealBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)
- [Acrílico](acrylic.md)
- [Efeitos de composição](https://docs.microsoft.com/windows/uwp/graphics/composition-effects)
- [Design Fluente para UWP](/windows/apps/fluent-design-system)
- [Ciência no sistema: Design Fluente e profundidade](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [Ciência no sistema: Design Fluente e iluminação](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
