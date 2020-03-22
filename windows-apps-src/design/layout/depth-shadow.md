---
author: knicholasa
description: A profundidade Z, ou a profundidade relativa, e a sombra são duas maneiras de incorporar a profundidade em seu aplicativo para ajudar os usuários a se concentrarem naturalmente e com eficiência.
title: Profundidade Z e sombra para aplicativos UWP
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
pm-contact: chigy
ms.localizationpriority: medium
ms.openlocfilehash: 216974ba564a192f94473469f3a7a49191ef2192
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081393"
---
# <a name="z-depth-and-shadow"></a>Profundidade Z e sombra

![Um gif que mostra quatro retângulos cinzas que são empilhados diagonalmente, um no topo do outro. O GIF é animado para que as sombras apareçam e desapareçam.](images/elevation-shadow/shadow.gif)

Criar uma hierarquia visual de elementos em sua interface do usuário torna a interface do usuário fácil de verificar e transmite o que é importante para se concentrar. Elevação, o ato de trazer os elementos selecionados da interface do usuário para a frente, é geralmente usado para atingir tal hierarquia no software. Este artigo discute como criar a elevação em um aplicativo UWP usando a profundidade z e sombra.

A profundidade Z é um termo usado entre os criadores de aplicativos 3D para denotar a distância entre duas superfícies ao longo do eixo z. Ele ilustra como fechar um objeto é para o visualizador. Imagine-o como um conceito semelhante às coordenadas x/y, mas na direção z.

## <a name="why-use-z-depth"></a>Por que usar profundidade z?

No mundo físico, temos a tendência de se concentrar em objetos que estão mais próximos de nós. Também podemos aplicar esse instinto espacial à interface do usuário digital. Por exemplo, se você colocar um elemento mais próximo do usuário, o usuário instintivamenterá o foco no elemento. Ao mover elementos da interface do usuário mais próximo no eixo z, você pode estabelecer a hierarquia visual entre objetos, ajudando os usuários a concluir tarefas de forma natural e eficiente em seu aplicativo.

## <a name="what-is-shadow"></a>O que é sombra?

Sombra é uma maneira que um usuário percebe a elevação. A luz acima de um objeto elevado cria uma sombra na superfície abaixo. Quanto maior o objeto, maior e mais flexível a sombra se torna. Objetos com privilégios elevados na interface do usuário não precisam ter sombras, mas ajudam a criar a aparência da elevação.

Em aplicativos UWP, as sombras devem ser usadas de forma proposital, em vez de estética. Usar muitas sombras diminuirá ou eliminará a capacidade da sombra de concentrar o usuário.

Se você usar controles padrão, as sombras ThemeShadow serão incorporadas automaticamente à sua interface do usuário. No entanto, você pode incluir manualmente as sombras em sua interface do usuário usando as APIs ThemeShadow ou DropShadow. 

## <a name="themeshadow"></a>ThemeShadow

O tipo [ThemeShadow](/uwp/api/windows.ui.xaml.media.themeshadow) pode ser aplicado a qualquer elemento XAML para desenhar sombras adequadamente com base em coordenadas x, y, z. O ThemeShadow também se ajusta automaticamente para outras especificações ambientais:

- Adapta-se às alterações na iluminação, tema do usuário, ambiente de aplicativo e Shell.
- Aplica sombras aos elementos automaticamente com base em sua profundidade z. 
- Mantém os elementos em sincronia à medida que eles se movem e alteram a elevação.
- Mantém as sombras consistentes em todos os aplicativos.

Aqui está como o ThemeShadow foi implementado em um MenuFlyout. O MenuFlyout tem uma experiência interna em que a superfície principal é elevada para 32px e cada menu em cascata adicional é aberto + 8px acima do menu em que ele é aberto.

![Uma captura de tela de ThemeShadow aplicada a um MenuFlyout com três menus abertos e aninhados. O primeiro menu é 32px elevado e cada menu subsequente que é aberto no menu anterior é elevado 8px, de modo que ele deixa uma sombra distinta no plano de fundo.](images/elevation-shadow/themeshadow-menuflyout.png)

### <a name="themeshadow-in-common-controls"></a>ThemeShadow em controles comuns

Os seguintes controles comuns usarão automaticamente o ThemeShadow para converter sombras de profundidade de 32px, a menos que especificado de outra forma:

- [Menu de contexto](../controls-and-patterns/menus.md), [barra de comandos](../controls-and-patterns/app-bars.md), [submenu de barras de comandos, barra](../controls-and-patterns/command-bar-flyout.md)de [menus](../controls-and-patterns/menus.md#create-a-menu-bar)
- [Caixas de diálogo e submenus](../controls-and-patterns/dialogs.md) (caixa de diálogo em 64px)
- [NavigationView](../controls-and-patterns/navigationview.md)
- [ComboBox](../controls-and-patterns/combo-box.md), [DropDownButton, SplitButton, ToggleSplitButton](../controls-and-patterns/buttons.md)
- [TeachingTip](../controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)
- [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 
- [Seletores de calendário/data/hora](../controls-and-patterns/date-and-time.md)
- [Dica de ferramenta](../controls-and-patterns/tooltips.md) (16px)
- [Controle de transporte de mídia](../controls-and-patterns/media-playback.md#media-transport-controls), [InkToolbar](../controls-and-patterns/inking-controls.md)
- [Animação conectada](../motion/connected-animation.md)

Observação: os submenus só aplicarão ThemeShadow quando compilados no Windows 10 versão 1903 ou em um SDK mais recente.

### <a name="themeshadow-in-popups"></a>ThemeShadow em pop-ups

Geralmente, é o caso de a interface do usuário do seu aplicativo usar um pop-up para cenários em que você precisa de atenção e ação rápida do usuário. Esses são ótimos exemplos quando a sombra deve ser usada para ajudar a criar hierarquia na interface do usuário do seu aplicativo.

O ThemeShadow automaticamente converte sombras quando aplicado a qualquer elemento XAML em um [pop-up](/uwp/api/windows.ui.xaml.controls.primitives.popup). Ele converterá sombras no conteúdo de segundo plano do aplicativo por trás dele e qualquer outro Popup aberto abaixo dele.

Para usar ThemeShadow com pop-ups, use a propriedade `Shadow` para aplicar um ThemeShadow a um elemento XAML. Em seguida, eleve o elemento de outros elementos por trás dele, por exemplo, usando o componente z da propriedade `Translation`.
Para a maioria das interfaces do usuário pop-up, a elevação padrão recomendada relativa ao conteúdo de segundo plano do aplicativo é de 32 pixels efetivos.

Este exemplo mostra um retângulo em um pop-up que converte uma sombra no conteúdo de segundo plano do aplicativo e quaisquer outros pop-ups por trás dele:

```xaml
<Popup>
    <Rectangle x:Name="PopupRectangle" Fill="Lavender" Height="48" Width="96">
        <Rectangle.Shadow>
            <ThemeShadow />
        </Rectangle.Shadow>
    </Rectangle>
</Popup>
```

```csharp
// Elevate the rectangle by 32px
PopupRectangle.Translation += new Vector3(0, 0, 32);
```

![Um pop-up retangular único com uma sombra.](images/elevation-shadow/PopupRectangle.png)

### <a name="disabling-default-themeshadow-on-custom-flyout-controls"></a>Desabilitando ThemeShadow padrão em controles de submenu personalizados

Os controles com base em [submenu](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.flyout), [DatePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepickerflyout), [MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.menuflyout) ou [TimePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepickerflyout) usam automaticamente ThemeShadow para converter uma sombra.

Se a sombra padrão não parecer correta no conteúdo do controle, você poderá desabilitá-la definindo a propriedade [IsDefaultShadowEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flyoutpresenter.isdefaultshadowenabled) como `false` no FlyoutPresenter associado:

```xaml
<Flyout>
    <Flyout.FlyoutPresenterStyle>
        <Style TargetType="FlyoutPresenter">
            <Setter Property="IsDefaultShadowEnabled" Value="False" />
        </Style>
    </Flyout.FlyoutPresenterStyle>
</Flyout>
```

### <a name="themeshadow-in-other-elements"></a>ThemeShadow em outros elementos

Em geral, incentivamos você a pensar cuidadosamente sobre o uso de sombra e limitar seu uso a casos em que ele introduz uma hierarquia visual significativa. No entanto, fornecemos uma maneira de converter uma sombra de qualquer elemento da interface do usuário caso você tenha cenários avançados que precisem dela.

Para converter uma sombra de um elemento XAML que não está em um popup, você deve especificar explicitamente os outros elementos que podem receber a sombra na coleção de `ThemeShadow.Receivers`. Os receptores não podem ser um ancestral do elenco na árvore visual.

Este exemplo mostra dois retângulos que convertem sombras em uma grade por trás deles:

```xaml
<Grid>
    <Grid.Resources>
        <ThemeShadow x:Name="SharedShadow" />
    </Grid.Resources>

    <Grid x:Name="BackgroundGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" />

    <Rectangle x:Name="Rectangle1" Height="100" Width="100" Fill="Turquoise" Shadow="{StaticResource SharedShadow}" />

    <Rectangle x:Name="Rectangle2" Height="100" Width="100" Fill="Turquoise" Shadow="{StaticResource SharedShadow}" />
</Grid>
```

```csharp
/// Add BackgroundGrid as a shadow receiver and elevate the casting buttons above it
SharedShadow.Receivers.Add(BackgroundGrid);

Rectangle1.Translation += new Vector3(0, 0, 16);
Rectangle2.Translation += new Vector3(120, 0, 32);
```

![Dois retângulos turquesa ao lado um do outro, ambos com sombras.](images/elevation-shadow/SharedShadow.png)

### <a name="performance-best-practices-for-themeshadow"></a>Práticas recomendadas de desempenho para ThemeShadow

1. O sistema define um limite de cinco pares de receptores de coerção e desativará a sombra se isso for excedido. Aderir ao limite imposto pelo sistema de cinco pares de receptores de coerção.

2. Limitar o número de elementos do receptor personalizado ao mínimo necessário.

3. Se vários elementos de receptor estiverem na mesma elevação, tente combiná-los direcionando um único elemento pai em vez disso.

4. Se vários elementos converterem o mesmo tipo de sombra nos mesmos elementos do destinatário, adicione a sombra como um recurso compartilhado e reutilize-a.

## <a name="drop-shadow"></a>Sombra

O DropShadow não responde automaticamente ao seu ambiente e não usa fontes de luz. Por exemplo, implementações, consulte a [classe DropShadow](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow).

## <a name="which-shadow-should-i-use"></a>Qual sombra devo usar?

| Propriedade | ThemeShadow | DropShadow |
| - | - | - |
| **Mínimo de SDK** | Windows 10 versão 1903 | 14393 |
| **Adaptação** | Sim | Não |
| **Customization** | Não | Sim |
| **Fonte de luz** | Automático (global por padrão, mas pode substituir por aplicativo) | Nenhum |
| **Com suporte em ambientes 3D** | Sim | Não |

- Tenha em mente que a finalidade da sombra é fornecer uma hierarquia significativa, não como um simples tratamento Visual.
- Em geral, é recomendável usar ThemeShadow, que se adapta automaticamente ao seu ambiente.
- Para se preocupar com o desempenho, limite o número de sombras, use outro tratamento Visual ou use DropShadow.
- Se você tiver cenários mais avançados para obter a hierarquia visual, considere usar outro tratamento Visual (por exemplo, cor). Se a sombra for necessária, use DropShadow.
