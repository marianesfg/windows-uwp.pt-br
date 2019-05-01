---
author: knicholasa
description: Profundidade Z ou relativo profundidade e sombra são duas maneiras de incorporar a profundidade em seu aplicativo para ajudar os usuários foquem forma natural e eficiente.
title: Profundidade Z e sombra para aplicativos UWP
template: detail.hbs
ms.author: nichola
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
pm-contact: chigy
ms.localizationpriority: medium
ms.openlocfilehash: ab49f13d3938e55750ce523f9e0d4ae241304763
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63817743"
---
# <a name="z-depth-and-shadow"></a>Profundidade Z e sombra

![Um gif mostrando quatro retângulos cinza que são empilhados na diagonal, um sobre o outro. Gif é animado para que sombras aparecem e desaparecem.](images/elevation-shadow/shadow.gif)

Criando uma hierarquia visual de elementos em sua interface do usuário facilita a interface do usuário verificar e transmite o que é importante ter em mente. Elevação de privilégio, o ato de colocar selecionar elementos de interface do usuário para a frente, geralmente é usada para atingir tal uma hierarquia em software. Este artigo discute como criar elevação em um aplicativo UWP usando a profundidade z e TI sombra.

Profundidade Z é um termo usado entre os criadores de aplicativos 3D para denotar a distância entre duas superfícies ao longo do eixo z. Ele ilustra como fechar um objeto é o visualizador. Pense nisso como um conceito semelhante a x / y coordenadas, mas na direção z.

## <a name="why-use-z-depth"></a>Por que usar profundidade z?

No mundo físico, tendemos a nos concentrar em objetos que estão mais próximos para nós. Podemos aplicar esse instinto espacial para o digital da interface do usuário também. Por exemplo, se você colocar um elemento mais próximo ao usuário, em seguida, o usuário instintivamente se concentrará no elemento. Pela colocação de elementos de interface do usuário móvel mais próximo no eixo z, você pode estabelecer uma hierarquia visual entre objetos, ajudando os usuários a concluir tarefas de forma natural e eficiente em seu aplicativo.

## <a name="what-is-shadow"></a>O que é a sombra?

Sombra é uma maneira de um usuário percebe que a elevação. Luz acima de um objeto com privilégios elevados cria uma sombra na superfície de abaixo. Quanto maior o objeto, o maior e mais suave se torna a sombra. Objetos com privilégios elevados em sua interface do usuário não precisam ter sombras, mas eles ajudam a criar a aparência de elevação.

Em aplicativos UWP, sombras devem ser usadas de forma proposital em vez de estética. Usar um número excessivo de sombras diminuir ou eliminar a capacidade de se concentrar o usuário da sombra.

Se você usar controles padrão, sombras ThemeShadow serão incorporadas automaticamente sua interface do usuário. No entanto, você pode incluir sombras manualmente em sua interface do usuário usando o ThemeShadow ou as APIs de DropShadow. 

## <a name="themeshadow"></a>ThemeShadow

O tipo pode ser aplicado a qualquer elemento XAML para desenhar de ThemeShadow sombreia adequadamente com base em x, y, z coordenadas. ThemeShadow também ajusta automaticamente para outras especificações ambientais:

- Se adapta às alterações na iluminação, tema do usuário, ambiente de aplicativo e shell.
- Aplica-se sombras aos elementos automaticamente com base em sua profundidade z. 
- Mantém os elementos em sincronização conforme eles se movam e alterar a elevação.
- Sombras mantém consistente em toda aplicativos.

Aqui está como ThemeShadow foi implementado em uma MenuFlyout. MenuFlyout possui um interno na experiência que a principal superfície é elevada para 32px e cada menu em cascata adicional é aberto + 8px acima do menu, que ele abre do.

![Uma captura de tela de ThemeShadow aplicado a um MenuFlyout com três menus aninhados, abertos. O primeiro menu é 32px com privilégios elevados e cada menu subsequente que é aberta no menu anterior é elevado 8px mais, de modo que ela deixa uma sombra distinta no plano de fundo.](images/elevation-shadow/themeshadow-menuflyout.png)

### <a name="themeshadow-in-common-controls"></a>ThemeShadow controla em comum

Os seguintes controles comuns automaticamente usará ThemeShadow converter sombras de 32px profundidade, a menos que especificado o contrário:

- [Menu de contexto](../controls-and-patterns/menus.md), [barra de comandos](../controls-and-patterns/app-bars.md), [submenu da barra de comandos](../controls-and-patterns/command-bar-flyout.md), [barra de menus](../controls-and-patterns/menus.md#create-a-menu-bar)
- [Caixas de diálogo e submenus](../controls-and-patterns/dialogs.md) (caixa de diálogo 64px)
- [NavigationView](../controls-and-patterns/navigationview.md)
- [ComboBox](../controls-and-patterns/combo-box.md), [DropDownButton, SplitButton, ToggleSplitButton](../controls-and-patterns/buttons.md)
- [TeachingTip](../controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)
- [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 
- [Seletores de calendário/data/hora](../controls-and-patterns/date-and-time.md)
- [Tooltip](../controls-and-patterns/tooltips.md) (16px)
- [Controle de transporte de mídia](../controls-and-patterns/media-playback.md#media-transport-controls), [InkToolbar](../controls-and-patterns/inking-controls.md)
- [Animação conectada](../motion/connected-animation.md)

Observação: Submenus serão aplicadas somente ThemeShadow quando compilado no Windows 10 versão 1903 ou um SDK mais recente.

### <a name="themeshadow-in-popups"></a>ThemeShadow no pop-ups

Geralmente é o caso que interface do usuário do seu aplicativo usa um pop-up para cenários em que você precisa de atenção do usuário e a ação rápida. Estes são exemplos de ótimos quando sombra deve ser usada para ajudar a criar a hierarquia na interface de usuário do seu aplicativo.

ThemeShadow converte automaticamente sombras quando aplicado a qualquer elemento XAML em um [pop-up](/uwp/api/windows.ui.xaml.controls.primitives.popup). Ela será convertida sombras sobre o conteúdo do plano de fundo do aplicativo por trás de ele e quaisquer outros abrir pop-ups abaixo dele.

Para usar ThemeShadow com pop-ups, use o `Shadow` propriedade para aplicar um ThemeShadow a um elemento XAML. Em seguida, elevar o elemento de outros elementos por trás dela, por exemplo, usando o componente z do `Translation` propriedade.
Para a maioria dos pop-up da interface do usuário, a elevação padrão recomendado relativa ao conteúdo de tela de fundo do aplicativo é 32 pixels relevantes.

Este exemplo mostra um retângulo em um pop-up Projetando uma sombra sobre o conteúdo de tela de fundo do aplicativo e qualquer outros pop-ups por trás dele:

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

![Um único retangular pop-up com uma sombra.](images/elevation-shadow/PopupRectangle.png)

### <a name="disabling-default-themeshadow-on-custom-flyout-controls"></a>Desabilitando padrão controla a ThemeShadow no submenu personalizado

Controles com base em [submenu](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.flyout), [DatePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepickerflyout), [MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.menuflyout) ou [TimePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepickerflyout) automaticamente usar ThemeShadow para converter um sombra.

Se a sombra padrão não parece correta em seu conteúdo do controle, em seguida, você pode desabilitá-lo definindo a [IsDefaultShadowEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flyoutpresenter.isdefaultshadowenabled) propriedade `false` sobre o FlyoutPresenter associado:

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

Em geral incentivamos você a pensar cuidadosamente sobre o uso de sombra e limitar seu uso para casos em que ele apresenta significativa hierarquia visual. No entanto, fornecemos uma maneira de converter uma sombra de qualquer elemento de interface do usuário no caso de você ter cenários avançados que exigem a ele.

Para converter uma sombra de um elemento XAML que não esteja em um pop-up, você deve especificar explicitamente os outros elementos que podem receber a sombra no `ThemeShadow.Receivers` coleção. Receptores não podem ser um ancestral do caster na árvore visual.

Este exemplo mostra dois retângulos que convertido sombras para uma grade por trás delas:

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

![Dois retângulos turquesa próximos um do outro, ambos com sombras.](images/elevation-shadow/SharedShadow.png)

### <a name="performance-best-practices-for-themeshadow"></a>Práticas recomendadas de desempenho para ThemeShadow

1. O sistema define um limite de pares de receptor caster 5 e será desligado sombra se isto for excedido. Continuar com o limite imposto pelo sistema de pares de receptor caster 5.

2. Limite o número de elementos receptora personalizada ao mínimo necessário.

3. Se vários elementos de receptor estão no mesma elevação, tente combiná-los direcionando um elemento pai único em vez disso.

4. Se vários elementos irá converter o mesmo tipo de sombra para os mesmos elementos do receptor, em seguida, adicione a sombra como um recurso compartilhado e reutilizá-lo.

## <a name="drop-shadow"></a>Sombra

DropShadow não está respondendo automaticamente a seu ambiente e não usa fontes de luz. Por exemplo implementações, consulte o [DropShadow classe](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow).

## <a name="which-shadow-should-i-use"></a>Quais sombra devo usar?

| Propriedade | ThemeShadow | DropShadow |
| - | - | - | - |
| **Min SDK** | Windows 10 versão 1903 | 14393 |
| **Capacidade de adaptação** | Sim | Não |
| **Personalização** | Não | Sim |
| **Fonte de luz** | Automático (global por padrão, mas pode ser substituído por aplicativo) | Nenhuma |
| **Tem suporte em ambientes 3D** | Sim | Não |

- Tenha em mente que a finalidade de sombra é fornecer hierarquia significativa, não como um tratamento visual simple.
- Em geral, é recomendável usar ThemeShadow, que se adapta automaticamente a seu ambiente.
- Para questões de desempenho, limitar o número de sombras, usar outro tratamento visual ou use DropShadow.
- Se você tiver cenários mais avançados para alcançar a hierarquia visual, considere usar outro tratamento visual (por exemplo, cor). Se a sombra é necessária, use DropShadow.
