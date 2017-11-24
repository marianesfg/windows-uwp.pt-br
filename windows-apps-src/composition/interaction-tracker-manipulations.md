---
author: jwmsft
title: "Manipulações personalizadas com o InteractionTracker"
description: "Use as APIs do InteractionTracker para criar experiências de manipulação personalizadas."
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, animação"
localizationpriority: medium
ms.openlocfilehash: 208bc9b15db20838401c6437c682774cf45febdd
ms.sourcegitcommit: 44a24b580feea0f188c7eae36e72e4a4f412802b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2017
---
# <a name="custom-manipulation-experiences-with-interactiontracker"></a>Experiências personalizadas de manipulação com o InteractionTracker

Neste artigo, mostramos como usar o InteractionTracker para criar experiências de manipulação personalizadas.

## <a name="prerequisites"></a>Pré-requisitos

Aqui, presumimos que você esteja familiarizado com os conceitos abordados neste artigo:

- [Animações controladas por entrada](input-driven-animations.md)
- [Animações baseadas em relações](relation-animations.md)

## <a name="why-create-custom-manipulation-experiences"></a>Por que criar experiências de manipulação personalizadas?

Na maioria dos casos, utilizar os controles de manipulação predefinidos é suficiente para criar experiências de interface do usuário. Mas, e se você quiser algo diferente dos controles comuns? E se você quiser criar uma experiência específica controlada por entrada ou ter uma interface do usuário na qual um movimento de manipulação tradicional não é suficiente? É aqui que a criação de experiências personalizadas entra em ação. Elas permitem que designers e desenvolvedores de apps sejam mais criativos; dinamizam experiências de movimento que exemplificam melhor sua identidade visual e linguagem de design personalizada. Desde o início, você tem acesso ao conjunto certo de blocos de construção para personalizar completamente uma experiência de manipulação, desde como o movimento deve responder com o dedo dentro e fora da tela a pontos de ajuste e encadeamento de entradas.

Estes são alguns exemplos comuns que indicam quando você criaria uma experiência de manipulação personalizada:

- Adicionando um deslizamento de dedo personalizado, excluir/ignorar o comportamento
- Efeitos controlados por entrada (a panorâmica provoca o desfoque do conteúdo )
- Controles personalizados com movimentos de manipulação personalizados (ListView personalizado, ScrollViewer personalizado etc.)

![Exemplo de controle de rolagem com deslizamento de dedo](images/animation/swipe-scroller.gif)

![Exemplo de puxar para animar](images/animation/pull-to-animate.gif)

## <a name="why-use-interactiontracker"></a>Por que usar o InteractionTracker?

O InteractionTracker foi incorporado no namespace Windows.UI.Composition.Interactions no SDK versão 10586. O InteractionTracker proporciona:

- **Flexibilidade completa** – queremos que você possa personalizar e adaptar todos os aspectos de uma experiência de manipulação; especificamente, os movimentos exatos que ocorrem durante ou em resposta à entrada. Ao criar uma experiência de manipulação personalizada com o InteractionTracker, todos os botões de que você precisa estarão à sua disposição.
- **Desempenho suave** – um dos desafios das experiências de manipulação é que seu desempenho depende do thread da interface do usuário. Isso pode afetar negativamente qualquer experiência de manipulação quando a interface do usuário estiver ocupada. O InteractionTracker foi criado para utilizar o novo mecanismo de animação que opera em um thread independente a 60 FPS, resultando em um movimento suave.

## <a name="overview-interactiontracker"></a>Visão geral: InteractionTracker

Ao criar experiências de manipulação personalizadas, existem dois componentes principais com os quais você interage. Vamos falar sobre estes primeiro:

- [InteractionTracker](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactiontracker) – o objeto principal que mantém uma máquina de estado cujas propriedades são controladas pela entrada ativa do usuário ou por atualizações e animações diretas. Foi projetado para vincular-se ao CompositionAnimation para criar o movimento de manipulação personalizado.
- [VisualInteractionSource](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.visualinteractionsource) – um objeto complementar que define quando e em que condições a entrada é enviada ao InteractionTracker. Ele define o CompositionVisual usado no teste de acertos e outras propriedades de configuração de entrada.

Como máquina de estado, as propriedades do InteractionTracker podem ser controladas por qualquer um destes itens:

- Interação direta do usuário - o usuário final realiza a manipulação diretamente na região de teste de acertos do VisualInteractionSource
- Inércia - na velocidade programática ou a partir de um gesto do usuário, as propriedades do InteractionTracker são animadas sob uma curva de inércia
- CustomAnimation - uma animação personalizada direcionada diretamente a uma propriedade do InteractionTracker

### <a name="interactiontracker-state-machine"></a>Máquina de estado InteractionTracker

Como mencionado anteriormente, o InteractionTracker é uma máquina de estado com quatro estados; cada um deles pode fazer a transição para qualquer um dos outros estados. (Para obter mais informações sobre como o InteractionTracker faz a transição entre esses estados, consulte a documentação de classe do [InteractionTracker](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactiontracker).)

| Estado | Descrição |
|-------|-------------|
| Ocioso | Nenhuma entrada ativa sendo gerada ou animações |
| Interação | Entrada de usuário ativa detectada |
| Inércia | Movimento ativo resultante da entrada ativa ou da velocidade programática |
| CustomAnimation | Movimento ativo resultante de uma animação personalizada |

Em cada um dos casos em que o estado do InteractionTracker muda, é gerado um evento (ou retorno de chamada) que você pode detectar. Para que você possa detectar esses eventos, eles devem implementar a interface do [IInteractionTrackerOwner](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.iinteractiontrackerowner) e criar o objeto do InteractionTracker com o método CreateWithOwner. O diagrama a seguir também descreve quando os diferentes eventos são acionados.

![Máquina de estado InteractionTracker](images/animation/interaction-tracker-diagram.png)

## <a name="using-the-visualinteractionsource"></a>Usando o VisualInteractionSource

Para que o InteractionTracker seja controlado por entrada, você precisará conectar um VisualInteractionSource (VIS) a ele. O VIS é criado como um objeto complementar por meio de um CompositionVisual para definir:

1. A região de teste de acertos em que essa entrada será rastreada e o espaço de coordenadas em que os gestos serão detectados
1. As configurações de entrada que serão detectadas e roteadas incluem:
    - Gestos detectáveis: posições X e Y (panorâmica horizontal e vertical), escala (pinçagem)
    - Inércia
    - Trilhos e encadeamento
    - Modos de redirecionamento: quais dados de entrada são redirecionados automaticamente para o InteractionTracker

> [!NOTE]
> Como o VisualInteractionSource é criado com base na posição de teste de acertos e no espaço de coordenada de um elemento visual, não é recomendado usar um elemento visual que estará em movimento ou em uma posição de mudança.

> [!NOTE]
> Você poderá usar várias instâncias do VisualInteractionSource com o mesmo InteractionTracker se houver várias regiões de teste de acertos. No entanto, o caso mais comum é usar apenas um VIS.

O VisualInteractionSource também é responsável por gerenciar o momento em que os dados de entrada de diferentes modalidades (toque, PTP, caneta) serão roteados para o InteractionTracker. Esse comportamento é definido pela propriedade ManipulationRedirectionMode. Por padrão, toda a entrada do ponteiro é enviada ao thread da interface do usuário, e a entrada do touchpad de precisão é direcionada ao VisualInteractionSource e ao InteractionTracker.

Assim, se você quiser usar os recursos Toque e Caneta (Atualização para Criadores) para conduzir uma manipulação por meio do VisualInteractionSource e do InteractionTracker, chame o método VisualInteractionSource.TryRedirectForManipulation. No breve trecho de um app XAML a seguir, o método é chamado quando um evento pressionado por toque ocorre na parte superior da Grade UIElement:

```csharp
private void root_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Touch)
    {
        _source.TryRedirectForManipulation(e.GetCurrentPoint(root));
    }
}
```

## <a name="tie-in-with-expressionanimations"></a>Vínculo com o ExpressionAnimations

Ao usar o InteractionTracker para gerar uma experiência de manipulação, você interage principalmente com as propriedades Scale e Position. Assim como outras propriedades de CompositionObject, essas propriedades podem ser o alvo e a referência em um CompositionAnimation, mais comumente no ExpressionAnimations.

Para usar o InteractionTracker em uma expressão, faça referência à propriedade Position (ou Scale) do rastreador, como no exemplo abaixo. Como a propriedade do InteractionTracker é modificada devido a qualquer uma das condições descritas anteriormente, a saída da expressão também muda.

```csharp
// With Strings
var opacityExp = _compositor.CreateExpressionAnimation("-tracker.Position");
opacityExp.SetReferenceParameter("tracker", _tracker);

// With ExpressionBuilder
var opacityExp = -_tracker.GetReference().Position;
```

> [!NOTE]
> Ao referenciar a posição do InteractionTracker em uma expressão, você deve negar o valor da expressão resultante para se mover para a direção correta. Isso ocorre porque a progressão do InteractionTracker é feita a partir da origem em um gráfico e permite que você pense sobre a progressão do InteractionTracker em coordenadas do "mundo real", como a distância da sua origem.

## <a name="get-started"></a>Introdução

Para começar a usar o InteractionTracker para criar experiências de manipulação personalizadas:

1. Crie o objeto InteractionTracker usando InteractionTracker.Create ou InteractionTracker.CreateWithOwner.
    - (Se você usar CreateWithOwner, verifique se implementou a interface do IInteractionTrackerOwner.)
1. Defina a posição Min e Max do InteractionTracker recém-criado.
1. Crie o VisualInteractionSource com um CompositionVisual.
    - Assegure que o elemento visual que você transmite tem um tamanho diferente de zero. Caso contrário, ele não passará pelo teste de acertos corretamente.
1. Defina as propriedades do VisualInteractionSource.
    - VisualInteractionSourceRedirectionMode
    - PositionXSourceMode, PositionYSourceMode, ScaleSourceMode
    - Trilhos e encadeamento
1. Adicione o VisualInteractionSource ao InteractionTracker usando InteractionTracker.InteractionSources.Add.
1. Configure TryRedirectForManipulation para o momento em que a entrada de Toque e Caneta for detectada.
    - Para XAML, isso geralmente é feito no evento PointerPressed de UIElement.
1. Crie um ExpressionAnimation que faça referência à posição do InteractionTracker e indique uma propriedade do CompositionObject.

Aqui está um pequeno trecho de código que mostra 1 - 5 em ação:

```csharp
private void InteractionTrackerSetup(Compositor compositor, Visual hitTestRoot)
{ 
    // #1 Create InteractionTracker object
    var tracker = InteractionTracker.Create(compositor);

    // #2 Set Min and Max positions
    tracker.MinPosition = new Vector3(-1000f);
    tracker.MaxPosition = new Vector3(1000f);

    // #3 Setup the VisualInteractionSourc
    var source = VisualInteractionSource.Create(hitTestRoot);

    // #4 Set the properties for the VisualInteractionSource
    source.ManipulationRedirectionMode =
        VisualInteractionSourceRedirectionMode.CapableTouchpadOnly;
    source.PositionXSourceMode = InteractionSourceMode.EnabledWithInertia;
    source.PositionYSourceMode = InteractionSourceMode.EnabledWithInertia;

    // #5 Add the VisualInteractionSource to InteractionTracker
    tracker.InteractionSources.Add(source);
}
```

Para usos mais avançados do InteractionTracker, consulte os seguintes artigos:

- [Criar pontos de ajuste com InertiaModifiers](inertia-modifiers.md)
- [Puxar para atualizar com SourceModifiers](source-modifiers.md)
