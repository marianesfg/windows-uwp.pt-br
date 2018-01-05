---
author: jwmsft
title: Puxar para atualizar com modificadores de origem
description: Criar controles personalizados de puxar para atualizar usando SourceModifiers
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, animação"
ms.localizationpriority: medium
ms.openlocfilehash: 1d85dbe89fe90723c3475b77c6057def684bd60d
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="pull-to-refresh-with-source-modifiers"></a>Puxar para atualizar com modificadores de origem

Neste artigo, abordaremos detalhadamente como usar o recurso SourceModifier de um InteractionTracker e demonstrar seu uso através da criação de um controle personalizado de puxar para atualizar.

## <a name="prerequisites"></a>Pré-requisitos

Aqui, presumimos que você esteja familiarizado com os conceitos abordados neste artigo:

- [Animações controladas por entrada](input-driven-animations.md)
- [Experiências personalizadas de manipulação com o InteractionTracker](interaction-tracker-manipulations.md)
- [Animações baseadas em relações](relation-animations.md)

## <a name="what-is-a-sourcemodifier-and-why-are-they-useful"></a>O que é um SourceModifier e por que eles são úteis?

Assim como os [InertiaModifiers](inertia-modifiers.md), os SourceModifiers proporcionam um controle mais refinado sobre o movimento de um InteractionTracker. Mas, diferente dos InertiaModifiers, que definem o movimento depois que o InteractionTracker entra em inércia, os SourceModifiers definem o movimento enquanto o InteractionTracker ainda está no estado de interação. Nesses casos, você deseja uma experiência diferente do tradicional "controle pelos dedos".

Um exemplo clássico disso é a experiência de puxar para atualizar; quando o usuário puxa a lista para atualizar o conteúdo, e a lista aparece na mesma velocidade do dedo e para após determinada distância, o movimento parece abrupto e mecânico. A experiência mais natural seria apresentar uma sensação de resistência enquanto o usuário interage ativamente com a lista. Essa pequena diferença torna a experiência geral do usuário final na interação com uma lista mais dinâmica e atrativa. Na seção de exemplo, explicaremos em mais detalhes como desenvolver isso.

Existem dois tipos de modificadores de origem:

- DeltaPosition – é o delta entre a posição atual do quadro e a posição anterior do quadro no dedo durante a interação da panorâmico de toque. Esse modificador de origem permite modificar a posição delta da interação antes de enviá-lo para processamento posterior. Este é um parâmetro do tipo Vector3, e o desenvolvedor pode optar por modificar qualquer um dos atributos X, Y ou Z da posição antes de passá-lo para o InteractionTracker.
- DeltaScale - é o delta entre a escala atual do quadro e a escala anterior do quadro aplicada durante a interação do zoom de toque. Esse modificador de origem permite modificar o nível de zoom da interação. Este é um atributo do tipo float que o desenvolvedor pode modificar antes de passá-lo para o InteractionTracker.

Quando o InteractionTracker estiver no estado de interação, ele avaliará cada um dos modificadores de origem atribuídos a ele e determinará se algum deles é aplicável. Isso significa que você pode criar e atribuir vários modificadores de origem a um InteractionTracker. Mas, ao definir cada um, você precisará fazer o seguinte:

1. Definir a condição – uma expressão que define a instrução condicional quando este modificador de origem precisar ser aplicado.
1. Definir DeltaPosition/DeltaScale – a expressão de modificador de origem que altera a DeltaPosition ou a DeltaScale quando a condição definida acima é atendida.

## <a name="example"></a>Exemplo

Agora vamos ver como você pode usar os modificadores de origem para criar uma experiência personalizada de puxar para atualizar com um controle ListView XAML existente. Usaremos uma Tela como "Painel de Atualização", que será empilhada sobre um ListView XAML para criar essa experiência.

Para a experiência de usuário final, queremos criar o efeito de "resistência", pois o usuário está fazendo ativamente a panorâmica da lista (com o toque) e interrompendo a panorâmica quando a posição ultrapassa um determinado ponto.

![Lista com puxar para atualizar](images/animation/city-list.gif)

O código de trabalho dessa experiência pode ser encontrado no [repositório de laboratórios de desenvolvimento de interface do usuário do Windows no GitHub](https://github.com/Microsoft/WindowsUIDevLabs). Esta é a instrução passo a passo de como criar essa experiência.
No código de marcação XAML, você terá o seguinte:

```xaml
<StackPanel Height="500" MaxHeight="500" x:Name="ContentPanel" HorizontalAlignment="Left" VerticalAlignment="Top" >
 <Canvas Width="400" Height="100" x:Name="RefreshPanel" >
<Image x:Name="FirstGear" Source="ms-appx:///Assets/Loading.png" Width="20" Height="20" Canvas.Left="200" Canvas.Top="70"/>
 </Canvas>
 <ListView x:Name="ThumbnailList"
 MaxWidth="400"
 Height="500"
ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.IsScrollInertiaEnabled="False" ScrollViewer.IsVerticalScrollChainingEnabled="True" >
 <ListView.ItemTemplate>
 ……
 </ListView.ItemTemplate>
 </ListView>
</StackPanel>
```

Como a ListView (`ThumbnailList`) é um controle XAML que já faz a rolagem, você precisará que a rolagem seja encadeada até o item pai (`ContentPanel`) quando esta atingir o primeiro item e não puder mais avançar a rolagem. (É no ContentPanel que você aplicará os modificadores de origem). Para que isso aconteça, é necessário definir ScrollViewer.IsVerticalScrollChainingEnabled como **true** na marcação ListView. Você também precisará definir o modo de encadeamento no VisualInteractionSource para **Always**.

Você precisa definir o manipulador PointerPressedEvent com o parâmetro _handledEventsToo_ como **true**. Sem essa opção, o PointerPressedEvent não será encadeado até o ContentPanel, pois o controle ListView marcará esses eventos como manipulados e eles não serão enviados à cadeia visual.

```csharp
//The PointerPressed handler needs to be added using AddHandler method with the //handledEventsToo boolean set to "true"
//instead of the XAML element's "PointerPressed=Window_PointerPressed",
//because the list view needs to chain PointerPressed handled events as well.
ContentPanel.AddHandler(PointerPressedEvent, new PointerEventHandler( Window_PointerPressed), true);
```

Agora, você está pronto para vincular isso ao InteractionTracker. Comece configurando o InteractionTracker, o VisualInteractionSource e a expressão que aproveitará a posição do InteractionTracker.

```csharp
// InteractionTracker and VisualInteractionSource setup.
_root = ElementCompositionPreview.GetElementVisual(Root);
_compositor = _root.Compositor;
_tracker = InteractionTracker.Create(_compositor);
_interactionSource = VisualInteractionSource.Create(_root);
_interactionSource.PositionYSourceMode = InteractionSourceMode.EnabledWithInertia;
_interactionSource.PositionYChainingMode = InteractionChainingMode.Always;
_tracker.InteractionSources.Add(_interactionSource);
float refreshPanelHeight = (float)RefreshPanel.ActualHeight;
_tracker.MaxPosition = new Vector3((float)Root.ActualWidth, 0, 0);
_tracker.MinPosition = new Vector3(-(float)Root.ActualWidth, -refreshPanelHeight, 0);

// Use the Tacker's Position (negated) to apply to the Offset of the Image.
// The -{refreshPanelHeight} is to hide the refresh panel
m_positionExpression = _compositor.CreateExpressionAnimation($"-tracker.Position.Y - {refreshPanelHeight} ");
m_positionExpression.SetReferenceParameter("tracker", _tracker);
_contentPanelVisual.StartAnimation("Offset.Y", m_positionExpression);
```

Com isso configurado, o painel de atualização ficará fora do visor em sua posição inicial e o usuário verá somente o listView. Quando a panorâmica atingir o ContentPanel, o evento PointerPressed será acionado, momento em que você solicitará que o sistema use o InteractionTracker para acionar a experiência de manipulação.

```csharp
private void Window_PointerPressed(object sender, PointerRoutedEventArgs e)
{
if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Touch) {
 // Tell the system to use the gestures from this pointer point (if it can).
 _interactionSource.TryRedirectForManipulation(e.GetCurrentPoint(null));
 }
}
```

> [!NOTE]
> Se não for necessário encadear os eventos Handled, a adição de PointerPressedEvent poderá ser feita diretamente através da marcação XAML usando o atributo (`PointerPressed="Window_PointerPressed"`).

A próxima etapa é configurar os modificadores de origem. Você usará dois modificadores de origem para obter esse comportamento: _Resistance_ e _Stop_.

- Resistance – Move a DeltaPosition.Y na metade da velocidade até atingir a altura do RefreshPanel.

```csharp
CompositionConditionalValue resistanceModifier = CompositionConditionalValue.Create (_compositor);
ExpressionAnimation resistanceCondition = _compositor.CreateExpressionAnimation(
 $"-tracker.Position.Y < {pullToRefreshDistance}");
resistanceCondition.SetReferenceParameter("tracker", _tracker);
ExpressionAnimation resistanceAlternateValue = _compositor.CreateExpressionAnimation(
 "source.DeltaPosition.Y / 3");
resistanceAlternateValue.SetReferenceParameter("source", _interactionSource);
resistanceModifier.Condition = resistanceCondition;
resistanceModifier.Value = resistanceAlternateValue;
```

- Stop – Interromperá a movimentação quando todo o RefreshPanel estiver na tela.

```csharp
CompositionConditionalValue stoppingModifier = CompositionConditionalValue.Create (_compositor);
ExpressionAnimation stoppingCondition = _compositor.CreateExpressionAnimation(
 $"-tracker.Position.Y >= {pullToRefreshDistance}");
stoppingCondition.SetReferenceParameter("tracker", _tracker);
ExpressionAnimation stoppingAlternateValue = _compositor.CreateExpressionAnimation("0");
stoppingModifier.Condition = stoppingCondition;
stoppingModifier.Value = stoppingAlternateValue;
Now add the 2 source modifiers to the InteractionTracker.
List<CompositionConditionalValue> modifierList = new List<CompositionConditionalValue>()
{ resistanceModifier, stoppingModifier };
_interactionSource.ConfigureDeltaPositionYModifiers(modifierList);
```

Este diagrama oferece uma visualização da configuração dos SourceModifiers.

![Diagrama de panorâmica](images/animation/source-modifiers-diagram.png)

Agora, com os SourceModifiers, você perceberá que, ao fazer a panorâmica do ListView para baixo e atingir o primeiro item, o painel de atualização será puxado a uma velocidade equivalente à metade da velocidade da panorâmica, até atingir a altura do RefreshPanel, quando ele será interrompido.

No exemplo completo, uma animação de quadro chave é usada para girar um ícone durante a interação na tela do RefreshPanel. Qualquer conteúdo pode ser usado em seu lugar ou utilize a posição do InteractionTracker para acionar essa animação separadamente.
