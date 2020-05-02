---
author: knicholasa
description: Profundidade Z, ou profundidade relativa, e sombra são duas maneiras de incorporar profundidade ao seu aplicativo para ajudar os usuários a se concentrarem de maneira natural e eficiente.
title: Profundidade Z e sombra para aplicativos UWP
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
pm-contact: chigy
ms.localizationpriority: medium
ms.openlocfilehash: 216974ba564a192f94473469f3a7a49191ef2192
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80081393"
---
# <a name="z-depth-and-shadow"></a>Profundidade Z e sombra

![Um gif mostrando quatro retângulos cinza empilhados na diagonal, um em cima do outro. O GIF é animado para que as sombras apareçam e desapareçam.](images/elevation-shadow/shadow.gif)

Criar uma hierarquia visual de elementos torna a interface do usuário fácil de examinar e definir no que é mais importante se concentrar. A elevação, o ato de trazer os elementos selecionados da interface do usuário para a frente, é geralmente usada para atingir essa hierarquia no software. Este artigo discute como criar elevação em um aplicativo UWP usando profundidade Z e sombra.

Profundidade Z é um termo usado entre os criadores de aplicativos 3D para indicar a distância entre duas superfícies ao longo do eixo z. Ela ilustra o quanto um objeto está próximo do visualizador. Imagine-a como um conceito semelhante às coordenadas x/y, mas na direção z.

## <a name="why-use-z-depth"></a>Por que usar a profundidade Z?

No mundo físico, tendemos a nos concentrar em objetos que estão mais próximos de nós. Também podemos aplicar esse instinto espacial à interface do usuário digital. Por exemplo, se você aproximar um elemento do usuário, ele se concentrará instintivamente no elemento. Ao aproximar os elementos da interface do usuário no eixo z, você pode estabelecer uma hierarquia visual entre os objetos, ajudando os usuários a concluir tarefas de maneira natural e eficiente em seu aplicativo.

## <a name="what-is-shadow"></a>O que é sombra?

A sombra é uma maneira de o usuário perceber a elevação. A luz acima de um objeto elevado cria uma sombra na superfície abaixo. Quanto mais alto o objeto, maior e mais suave a sombra se torna. Objetos elevados na sua interface do usuário não precisam ter sombras, mas ajudam a criar a aparência de elevação.

Nos aplicativos UWP, as sombras devem ser usadas de maneira proposital, e não estética. O uso de muitas sombras diminuirá ou eliminará a capacidade da sombra de focalizar o usuário.

Se você usar controles padrão, as sombras do ThemeShadow serão incorporadas automaticamente à sua interface do usuário. No entanto, você pode incluir sombras manualmente na interface do usuário usando as APIs ThemeShadow ou DropShadow. 

## <a name="themeshadow"></a>ThemeShadow

O tipo [ThemeShadow](/uwp/api/windows.ui.xaml.media.themeshadow) pode ser aplicado a qualquer elemento XAML para desenhar sombras adequadamente com base nas coordenadas x, y, z. O ThemeShadow também se ajusta automaticamente a outras especificações ambientais:

- Adapta-se a alterações na iluminação, tema do usuário, ambiente do aplicativo e shell.
- Aplica sombras a elementos automaticamente com base na profundidade Z. 
- Mantém os elementos sincronizados conforme eles se movem e mudam de elevação.
- Mantém as sombras consistentes em todos os aplicativos e entre eles.

Confira aqui como ThemeShadow foi implementado em um MenuFlyout. O MenuFlyout tem uma experiência integrada, em que a superfície principal é elevada para 32 px e cada menu em cascata adicional é aberto + 8px acima do menu em que é aberto.

![Captura de tela do ThemeShadow aplicada a um MenuFlyout com três menus abertos e aninhados. O primeiro menu é elevado em 32 px, e cada menu subsequente aberto no menu anterior é elevado em 8 px a mais, deixando uma sombra distinta no fundo.](images/elevation-shadow/themeshadow-menuflyout.png)

### <a name="themeshadow-in-common-controls"></a>ThemeShadow em controles comuns

Os seguintes controles comuns usarão automaticamente o ThemeShadow para projetar sombras com profundidade de 32 px, a menos que especificado de outra forma:

- [Menu de contexto](../controls-and-patterns/menus.md), [barra de comandos](../controls-and-patterns/app-bars.md), [submenu da barra de comandos](../controls-and-patterns/command-bar-flyout.md), [barra de menus](../controls-and-patterns/menus.md#create-a-menu-bar)
- [Caixas de diálogo e submenus](../controls-and-patterns/dialogs.md) (diálogo a 64 px)
- [NavigationView](../controls-and-patterns/navigationview.md)
- [ComboBox](../controls-and-patterns/combo-box.md), [DropDownButton, SplitButton, ToggleSplitButton](../controls-and-patterns/buttons.md)
- [TeachingTip](../controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)
- [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 
- [Seletores de calendário/data/hora](../controls-and-patterns/date-and-time.md)
- [Dica de ferramenta](../controls-and-patterns/tooltips.md) (16 px)
- [Controle de transporte de mídia](../controls-and-patterns/media-playback.md#media-transport-controls), [InkToolbar](../controls-and-patterns/inking-controls.md)
- [Animação conectada](../motion/connected-animation.md)

Observação: Os submenus aplicarão o ThemeShadow apenas quando compilados no Windows 10 versão 1903 ou em um SDK mais recente.

### <a name="themeshadow-in-popups"></a>ThemeShadow em pop-ups

Geralmente, a interface do usuário do seu aplicativo usa um pop-up para cenários em que você precisa da atenção e ação rápida do usuário. Esses são ótimos exemplos de quando a sombra deve ser usada para ajudar a criar hierarquia na interface do usuário do seu aplicativo.

O ThemeShadow lança sombras automaticamente quando aplicado a qualquer elemento XAML em uma [Pop-up](/uwp/api/windows.ui.xaml.controls.primitives.popup). Ele projetará sombras no conteúdo em segundo plano do aplicativo por trás dele e em qualquer outra pop-up aberta abaixo dele.

Para usar o ThemeShadow com pop-ups, use a propriedade `Shadow` para aplicar um ThemeShadow a um elemento XAML. Em seguida, eleve o elemento de outros elementos por trás dele, por exemplo, usando o componente z da propriedade `Translation`.
Para a maioria das interfaces do usuário pop-up, a elevação padrão recomendada relativa ao conteúdo em segundo plano do aplicativo é de 32 pixels efetivos.

Este exemplo mostra um retângulo em uma pop-up projetando uma sombra no conteúdo em segundo plano do aplicativo e em qualquer outra pop-up por trás dele:

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

![Uma pop-up retangular única com sombra.](images/elevation-shadow/PopupRectangle.png)

### <a name="disabling-default-themeshadow-on-custom-flyout-controls"></a>Desativar o ThemeShadow padrão em controles de submenu personalizados

Controles baseados em [Flyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.flyout), [DatePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepickerflyout), [MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.menuflyout) ou [TimePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepickerflyout) usam o ThemeShadow automaticamente para projetar uma sombra.

Se a sombra padrão não parecer correta no conteúdo do seu controle, você poderá desativá-la configurando a propriedade [IsDefaultShadowEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flyoutpresenter.isdefaultshadowenabled) como `false` no FlyoutPresenter associado:

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

Em geral, recomendamos que você pense cuidadosamente na utilização da sombra e limite o uso aos casos em que ela introduz hierarquia visual significativa. No entanto, fornecemos uma maneira de projetar uma sombra de qualquer elemento da interface do usuário, caso você tenha cenários avançados que precisem dela.

Para converter a sombra de um elemento XAML que não esteja em uma pop-up, você deve especificar explicitamente os outros elementos que podem receber a sombra na coleção `ThemeShadow.Receivers`. Os receptores não podem ser ancestrais do projetor na árvore visual.

Este exemplo mostra dois retângulos que projetam sombras em uma grade atrás deles:

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

![Dois retângulos turquesas um ao lado do outro, ambos com sombras.](images/elevation-shadow/SharedShadow.png)

### <a name="performance-best-practices-for-themeshadow"></a>Práticas recomendadas de desempenho para ThemeShadow

1. O sistema define um limite de cinco pares de projetor-receptor e desativará a sombra se isso for excedido. Atenha-se ao limite imposto pelo sistema de cinco pares de projetor-receptor.

2. Limite o número de elementos receptores personalizados ao mínimo necessário.

3. Se vários elementos receptores estiverem na mesma elevação, tente combiná-los visando um elemento pai único.

4. Se vários elementos projetarem o mesmo tipo de sombra nos mesmos elementos receptores, adicione a sombra como um recurso compartilhado e reutilize-a.

## <a name="drop-shadow"></a>Sombra

DropShadow não responde automaticamente ao ambiente e não usa fontes de luz. Para exemplos de implementações, confira a [Classe DropShadow](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow).

## <a name="which-shadow-should-i-use"></a>Qual sombra devo usar?

| Propriedade | ThemeShadow | DropShadow |
| - | - | - |
| **SDK mín** | Windows 10, versão 1903 | 14393 |
| **Capacidade de adaptação** | Sim | Não |
| **Personalização** | Não | Sim |
| **Fonte de luz** | Automático (global por padrão, mas pode haver substituição conforme o aplicativo) | Não |
| **Suporte para ambientes 3D** | Sim | Não |

- Lembre-se de que o objetivo da sombra é fornecer uma hierarquia significativa, não como um simples tratamento visual.
- Geralmente, recomendamos o uso do ThemeShadow, que se adapta de modo automático ao ambiente.
- Por questões de desempenho, limite o número de sombras, use outro tratamento visual ou use DropShadow.
- Se você tiver cenários mais avançados para atingir a hierarquia visual, considere usar outro tratamento visual (por exemplo, cor). Se for necessária sombra, use DropShadow.
