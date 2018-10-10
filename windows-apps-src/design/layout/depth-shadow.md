---
author: serenaz
description: Profundidade Z ou relativo de profundidade e sombra são duas maneiras de incorporar profundidade em seu aplicativo para ajudar os usuários a se concentrarem Naturalidade e eficiência.
title: Profundidade Z e sombra para aplicativos UWP
template: detail.hbs
ms.author: sezhen
ms.date: 02/12/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: chigy
design-contact: balrayit
ms.localizationpriority: medium
ms.openlocfilehash: a1433b131b994ee2b1323909bc7c195e00f43cde
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4501016"
---
# <a name="z-depth-and-shadow"></a>Profundidade Z e sombra

![profundidade True](images/elevation-shadow/depth.svg)

O sistema de profundidade fluente usa físicos conceitos como 3D posicionamento, claro, e pode ser percebida sombra reinventar digital da interface do usuário em um ambiente físico com camadas mais. Profundidade Z ou relativo de profundidade e sombra são duas maneiras de incorporar profundidade em seu aplicativo UWP.

## <a name="what-is-z-depth"></a>O que é profundidade z?

Profundidade Z é a distância entre duas superfícies ao longo do eixo z, e ilustra como fechar um objeto é o visualizador.

![profundidade z](images/elevation-shadow/elevation.svg)

### <a name="why-use-z-depth"></a>Por que usar profundidade z?

No mundo físico, podemos tende a se concentrar em objetos que estão mais próximos para nós. Podemos aplicar esse instinto espacial para digital da interface do usuário, também. Por exemplo, se você colocar um elemento mais próximo ao usuário, em seguida, o usuário instintivamente concentrará no elemento. Colocação de elementos de interface do usuário móvel mais perto no eixo z, você pode estabelecer uma hierarquia visual entre objetos, ajudando os usuários a completar tarefas Naturalidade e eficiência em seu aplicativo. 

![profundidade z no menu de conteúdo](images/elevation-shadow/whyelevation.svg)

Além fornecer significativa hierarquia visual, profundidade z também permite que você crie experiências que fluam perfeitamente de 2D em ambientes 3D, dimensionamento seu aplicativo em todos os dispositivos e fatores forma. 

![profundidade z em 2d a 3d](images/elevation-shadow/elevation-2d3d.svg)

### <a name="how-is-z-depth-perceived"></a>Como profundidade z é percebida?

Com base em como percebemos profundidade do mundo físico, aqui estão várias técnicas que podem ser usadas para mostrar proximidade na interface do usuário digital.

- **Escala** Objetos futuramente parecem menores que os objetos mais próximos do mesmo tamanho. Este é o método é difícil demonstrar efetivamente no espaço 2D, portanto, ele geralmente não é recomendado. No entanto, você pode usar o dimensionamento e [sombra](#what-is-shadow) para criar uma simulação efetiva de objetos juntar para o usuário em 2D.

    ![proximidade com escala](images/elevation-shadow/elevation-scale.svg)

- **Atmosfera** Objetos podem ser exibidos mais distante e fora de foco com uma sobreposição de "smoky" ou outro efeito atmosféricas.

    ![proximidade com atmosfera](images/elevation-shadow/elevation-atmosphere.svg)

- **Movimento** Velocidade relativa pode ser usada para demonstrar proximidade: objetos mais próximos movem mais rapidamente do que os objetos distantes em segundo plano. Para saber como implementar esse efeito, consulte [Paralaxe](../motion/parallax.md).

    ![proximidade com movimento](images/elevation-shadow/elevation-motion.svg)

### <a name="recommendations-for-z-depth"></a>Recomendações de profundidade z

Reduza o número de planos com privilégios elevados para fornecer o foco visual. A maioria dos cenários, dois planos é suficiente: um para itens de primeiro plano (alta proximidade) e outro para itens de plano de fundo (proximidade baixa). Se você tiver vários itens com privilégios elevados que não se sobrepuserem, agrupá-los o mesmo plano (isto é, em primeiro plano) para reduzir o número de planos.

![profundidade z dentro de um aplicativo](images/elevation-shadow/app-depth.svg)

## <a name="what-is-shadow"></a>O que é sombra?

![sombra](images/elevation-shadow/shadow.svg)

Sombra é uma maneira de perceber elevação. Quando há luz acima de um objeto com privilégios elevados, há uma sombra na superfície de abaixo. Quanto maior o objeto, maior e mais suave se torna a sombra. Observe que não é necessário ter sombras objetos com privilégios elevados, mas sombras indicar a elevação.

Em aplicativos UWP, sombras devem ser proposital, não estética. Se sombras prejudicam em foco e a produtividade, em seguida, limite o uso de sombra.

Você pode usar sombras com as APIs de DropShadow ou ThemeShadow.

## <a name="themeshadow"></a>ThemeShadow

O ThemeShadow tipo pode ser aplicado a qualquer elemento XAML para desenhar sombras adequadamente com base em x, y, z coordenadas. ThemeShadow ajusta automaticamente para outras especificações ambientais:

- Se adapte a alterações na iluminação, tema do usuário, o ambiente de aplicativo e shell.
- Sombras elementos automaticamente com base em sua elevação.
- Mantém os elementos em sincronia como mover e alterar a elevação.
- Sombras mantém consistente em todo e em todos os aplicativos.

Aqui estão exemplos de ThemeShadow em diferentes elevações com os temas claros e escuros:

![sombras inteligentes com tema claro](images/elevation-shadow/smartshadow-light.svg)

![sombras inteligentes com tema escuro](images/elevation-shadow/smartshadow-dark.svg)

### <a name="themeshadow-in-common-controls"></a>Controles de ThemeShadow em comum

Os seguintes controles comuns automaticamente usará ThemeShadow para projetar sombras:

- [Caixas de diálogo e submenus](../controls-and-patterns/dialogs.md)
- [NavigationView](../controls-and-patterns/navigationview.md)
- [Controle de transporte de mídia](../controls-and-patterns/media-playback.md)
- [Menu de contexto](../controls-and-patterns/menus.md)
- [Barra de comandos](../controls-and-patterns/app-bars.md)
- [Sugestão automática](../controls-and-patterns/auto-suggest-box.md), [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [seletores de calendário/data/hora](../controls-and-patterns/date-and-time.md), [Dica de ferramenta](../controls-and-patterns/tooltips.md)
- [Teclas de acesso](../input/access-keys.md)

### <a name="themeshadow-in-popups"></a>ThemeShadow em pop-ups

ThemeShadow converte automaticamente sombras quando aplicado a qualquer elemento XAML em um [pop-up](/uwp/api/windows.ui.xaml.controls.primitives.popup). Ele será projetar sombras sobre o conteúdo do aplicativo em segundo plano por trás-lo e quaisquer outros abrir pop-ups abaixo dela.

Para usar ThemeShadow com pop-ups, use o `Shadow` propriedade para aplicar um ThemeShadow a um elemento XAML. Em seguida, elevar o elemento de outros elementos atrás dela, por exemplo, usando o componente de z do `Translation` propriedade.
Para a maioria dos pop-up da interface do usuário, a elevação padrão recomendado em relação ao conteúdo de plano de fundo do aplicativo é 32 pixels efetivos.

Este exemplo mostra um retângulo em um pop-up Projetando uma sombra sobre o conteúdo de plano de fundo do aplicativo e quaisquer outros pop-ups atrás dela:

```xaml
<Popup>
    <Rectangle x:Name="PopupRectangle" Fill="White" Height="48" Width="96">
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

![sombra do exemplo de código](images/elevation-shadow/smartshadow-example.svg)

### <a name="themeshadow-in-other-elements"></a>ThemeShadow em outros elementos

Para fazer isso de um elemento XAML que não estiver em um pop-up, você deve especificar explicitamente os outros elementos que podem receber a sombra no `ThemeShadow.Receivers` coleção.

Este exemplo mostra dois botões que projetar sombras em uma grade por trás deles:

```xaml
<Grid x:Name="BackgroundGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.Resources>
        <ThemeShadow x:Name="SharedShadow" />
    </Grid.Resources>

    <Button x:Name="Button1" Content="Button 1" Shadow="{StaticResource SharedShadow}" Margin="10" />

    <Button x:Name="Button2" Content="Button 2" Shadow="{StaticResource SharedShadow}" Margin="120" />
</Grid>
```

```csharp
/// Add BackgroundGrid as a shadow receiver and elevate the casting buttons above it
SharedShadow.Receivers.Add(BackgroundGrid);

Button1.Translation += new Vector3(0, 0, 16);
Button2.Translation += new Vector3(0, 0, 32);
```

### <a name="performance-best-practices-for-themeshadow"></a>Práticas recomendadas de desempenho para ThemeShadow

1. Limite o número de elementos de receptor personalizado para o mínimo necessário. 

2. Se vários elementos do receptor estão no mesma elevação, em seguida, tente combiná-los ao direcionar um elemento pai única em vez disso.

3. Se vários elementos converta o mesmo tipo de sombra para os mesmos elementos receptor, em seguida, adicione a sombra como um recurso compartilhado e reutilizá-lo.

## <a name="drop-shadow"></a>Sombra

DropShadow não está respondendo automaticamente para seu ambiente e não use fontes de luz. Por exemplo implementações, consulte a [Classe DropShadow](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow).

## <a name="which-shadow-should-i-use"></a>Quais sombra devo usar?

| Propriedade | ThemeShadow | DropShadow |
| - | - | - | - |
| **Min SDK** | RS5 | 14393 |
| **Capacidade de adaptação** | Sim | Não |
| **Personalização** | Não | Sim |
| **Fonte de luz** | Automática (global por padrão, mas pode substituir por aplicativo) | Nenhum(a) |
| **Suporte em ambientes 3D** | Sim | Não |

- Em geral, é recomendável usar ThemeShadow, que se adapta automaticamente ao seu ambiente.
- Se você tiver mais avançada cenários para sombras personalizadas, em seguida, use DropShadow, que permite a personalização maior.
- Para trás compatibilidade, use DropShadow.
- Para questões sobre desempenho, limitar o número de sombras ou usar DropShadow.
- No HMDs em 3D true, use ThemeShadow. Como DropShadow desenha em um deslocamento especificado do elemento visual que ele é pai, do lado do vai parecer que está flutuando no espaço. Por outro lado, ThemeShadow é renderizada sobre os elementos visuais definidos como receptores.
