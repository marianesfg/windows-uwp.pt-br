---
author: Karl-Bridge-Microsoft
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: 8389d1f089d507a087693879a793b95572d7860b
ms.sourcegitcommit: 5ece992c31870df4c089360ef47501bd4ce14fa9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2017
---
# <a name="managing-focus-navigation"></a>Gerenciando a navegação de foco

Muitas ferramentas de acessibilidade de inserção para teclado, como o Narrador do Windows, o gamepad e o controle remoto compartilham um mecanismo subjacente comum para mover o foco pela interface do usuário de seus aplicativos. Leia mais sobre foco visual e a navegação no documento de [Interação com o teclado](keyboard-interactions.md), bem como o documento [Projeto para Xbox e TV](designing-for-tv.md#xy-focus-navigation-and-interaction).

Em seguida, forneça uma maneira independente de entrada para que o aplicativo mova na interface do usuário, o que permite a você escrever um único código para o aplicativo funcionar bem com diversos tipos de entrada.

## <a name="navigation-strategies-properties-to-fine-tune-focus-movements"></a>Propriedades de estratégias de navegação para ajustar movimentos de foco

Use as propriedades de estratégia de navegação XYFocus para especificar qual controle deve receber o foco com base na tecla de seta pressionada. Estas propriedades são:

-   XYFocusUpNavigationStrategy
-   XYFocusDownNavigationStrategy
-   XYFocusLeftNavigationStrategy
-   XYFocusRightNavigationStrategy

Essas propriedades não têm um valor de tipo **XYFocusNavigationStrategy**. Se for definido como **Automático**, o comportamento do elemento tem por base os ancestrais do elemento. Se todos os elementos são definidos como **Automático**, a **Projeção** é usada.

Essas estratégias de navegação se aplicam ao teclado, gamepad e controle remoto.

### <a name="projection"></a>Projeção

A estratégia Projeção move o foco para o primeiro elemento encontrado durante a projeção da borda do elemento focalizado na direção da navegação.

> [!NOTE]
> Outros fatores, como o elemento focalizado anteriormente e a proximidade ao eixo da direção de navegação, podem influenciar o resultado.

![projeção](images/keyboard/projection.png)

*O foco se move de A para D na navegação para baixo com base na projeção da borda inferior de A*

### <a name="navigationdirectiondistance"></a>NavigationDirectionDistance

A estratégia NavigationDirectionDistance move o foco para o elemento mais próximo ao eixo da direção de navegação.

A borda do retângulo delimitador correspondente à direção de navegação é estendida e projetada para identificar os destinos de candidatos. O primeiro elemento encontrado é identificado como o destino. No caso de vários candidatos, o elemento mais próximo é identificado como o destino. Se ainda houver vários candidatos, o elemento na extremidade superior esquerda é identificado como o candidato.

![distância da direção de navegação](images/keyboard/navigation-direction-distance.png)

*O foco se move de A para C e, em seguida, de C para B na navegação para baixo*


### <a name="rectilineardistance"></a>RectilinearDistance

A estratégia RectilinearDistance move o foco para o elemento mais próximo com base na distância 2D mais curta (métrica de Manhattan). Essa distância é calculada ao adicionar a distância principal e a secundária de cada candidato potencial. Em um vínculo, o primeiro elemento à esquerda será selecionado se a direção for para cima ou para baixo, ou o primeiro elemento até a parte superior estiver selecionado ou se a direção for para a direita ou esquerda.

Na imagem a seguir, mostramos que quando A tem foco, o foco é movido para B porque:

-   Distância (A, B, Para baixo) = 10 + 0 = 10
-   Distância (A, C, Para baixo) = 30 + 0 = 30
-   Distância (A, D, Para baixo) 30 + 0 = 30

![Distância retilínea](images/keyboard/rectilinear-distance.png)

*Quando A tem foco, o foco é movido para B ao usar a estratégia de RectilinearDistance*

A imagem a seguir mostra como a estratégia de navegação é aplicada ao elemento em si, não os elementos filho (diferente de XYFocusKeyboardNavigation).

Por exemplo, quando E tem foco, pressionar para a direita move o foco até F usando a estratégia RectilinearDistance, mas quando D tem foco, pressionar à direita move o foco para H ao usar a estratégia Projection.

Observe que a estratégia de contêiner principal (Distância da direção de navegação) não é aplicada. Ela é usada somente quando o foco está na H, F ou G.

![estratégias de navegação diferentes](images/keyboard/different-navigation-strategies.png)

*Comportamentos de foco diferentes com base na estratégia de navegação aplicada*

O exemplo a seguir simula o comportamento do painel de Blocos no menu Iniciar do Windows 10. Nesse caso, pressionar o seta para cima e para baixo usa a estratégia NavigationDirectionDistance, e pressionar a seta para a esquerda e direita usa a estratégia Projection.

```XAML
<start:TileGridView
        XYFocusKeyboardNavigation ="Default"
        XYFocusUpNavigationStrategy=" NavigationDirectionDistance "
        XYFocusDownNavigationStrategy=" NavigationDirectionDistance "
        XYFocusLeftNavigationStrategy="Projection"
        XYFocusRightNavigationStrategy="Projection"
/>
```

## <a name="move-focus-programmatically"></a>Mover o foco de modo programático

Use o método [FocusManager.TryMoveFocus](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.Input.FocusManager.TryMoveFocus) ou [FocusManager.FindNextElement](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.Input.FocusManager.FindNextFocusableElement) para mover o foco de modo programático.

TryMoveFocus define o foco como se uma chave de navegação tiver sido pressionada.
FindNextElement retorna o elemento (um DependencyObject) para o qual TryMoveFocus se moveria.

**OBSERVAÇÃO** Recomendamos usar o método **FindNextElement** em vez de **FindNextFocusableElement**. FindNextFocusableElement tenta retornar um UIElement, mas se o próximo elemento focalizável for um Hiperlink, ele retorna null, pois um Hiperlink não é um UIElement. Em vez disso, o método FindNextElement retorna um DependencyObject.

### <a name="find-a-focus-candidate-in-a-scope"></a>Encontre um candidato de foco em um escopo

FocusManager.FindNextElement e FocusManager.TryMoveFocus permitem que você personalize o próximo comportamento de pesquisa de elemento focalizável. Você pode pesquisar em uma árvore de interface do usuário específica ou excluir elementos em uma área de navegação específica.

Este exemplo é de uma implementação de um jogo de Damas e demonstra como usar os métodos FindNextElement e TryMoveFocus:

```XAML
<StackPanel Orientation="Horizontal"
                VerticalAlignment="Center"
                HorizontalAlignment="Center" >
        <Button Content="Start Game" />
        <Button Content="Undo Movement" />
        <Grid x:Name="TicTacToeGrid" KeyDown="OnKeyDown">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="50" />
                <ColumnDefinition Width="50" />
                <ColumnDefinition Width="50" />
            </Grid.ColumnDefinitions>
            <Grid.RowDefinitions>
                <RowDefinition Height="50" />
                <RowDefinition Height="50" />
                <RowDefinition Height="50" />
            </Grid.RowDefinitions>
            <myControls:TicTacToeCell Grid.Column="0" Grid.Row="0" x:Name="Cell00" />
            <myControls:TicTacToeCell Grid.Column="1" Grid.Row="0" x:Name="Cell10"/>
            <myControls:TicTacToeCell Grid.Column="2" Grid.Row="0" x:Name="Cell20"/>
            <myControls:TicTacToeCell Grid.Column="0" Grid.Row="1" x:Name="Cell01"/>
            <myControls:TicTacToeCell Grid.Column="1" Grid.Row="1" x:Name="Cell11"/>
            <myControls:TicTacToeCell Grid.Column="2" Grid.Row="1" x:Name="Cell21"/>
            <myControls:TicTacToeCell Grid.Column="0" Grid.Row="2" x:Name="Cell02"/>
            <myControls:TicTacToeCell Grid.Column="1" Grid.Row="2" x:Name="Cell22"/>
            <myControls:TicTacToeCell Grid.Column="2" Grid.Row="2" x:Name="Cell32"/>
        </Grid>
    </StackPanel>
```
```csharp
private void OnKeyDown(object sender, KeyRoutedEventArgs e)
        {
            DependencyObject candidate = null;

            var options = new FindNextElementOptions ()
            {
                SearchRoot = TicTacToeGrid,
                NavigationStrategy = NavigationStrategyMode.Heuristic
            };

            switch (e.Key)
            {
                case Windows.System.VirtualKey.Up:
                    candidate = FocusManager.FindNextElement(FocusNavigationDirection.Up, options);
                    break;
                case Windows.System.VirtualKey.Down:
                    candidate = FocusManager.FindNextElement(FocusNavigationDirection.Down, options);
                    break;
                case Windows.System.VirtualKey.Left:
                    candidate = FocusManager.FindNextElement(FocusNavigationDirection.Left, options);
                    break;
                case Windows.System.VirtualKey.Right:
                    candidate = FocusManager.FindNextElement(FocusNavigationDirection.Right, options);
                    break;
            }
         //Note you should also consider whether is a Hyperlink, WebView, or TextBlock.
            if (candidate != null && candidate is Control)
            {
                (candidate as Control).Focus(FocusState.Keyboard);
            }
        }
```

**OBSERVAÇÃO** FocusNavigationDirection.Left e FocusNavigationDirection.Right são lógicas (próximo e longe) de suporte aos cenários RTL. (isso corresponde o restante do Xaml.)

A pesquisa para candidatos de foco pode ser ajustada usando a classe FindNextElementOptions. Este objeto tem as seguintes propriedades:

-   **SearchRoot** DependencyObject: para definir o escopo da pesquisa de candidatos aos filhos deste DependencyObject. O valor NULL indica que a árvore visual inteira do aplicativo.
-   **ExclusionRect** Rect: para excluir da pesquisa os itens que, quando são renderizados, se sobrepõem a pelo menos um pixel desse retângulo.
-   **HintRect** Rect: possíveis candidatos são calculados utilizando o item focalizado como referência. Esse retângulo permite que os desenvolvedores especifiquem outra referência em vez do elemento focalizado. O retângulo é um retângulo "fictício" usado somente para cálculos. Nunca é convertido em um retângulo real e adicionado à árvore visual.
-   **XYFocusNavigationStrategyOverride** XYFocusNavigationStrategyOverride: a estratégia de navegação usada para mover o foco. O tipo é uma enumeração com os valores None (que é o valor zero), Auto, RectilinearDistance, NavigationDirectionDistance e Projection.
    **Importante** **SearchRoot** não é usado para calcular a área (geométrica) renderizada ou para obter os candidatos na área. Consequentemente, se as transformações são aplicadas aos descendentes do DependencyObject que as coloca fora da área direcional, esses elementos ainda são considerados candidatos.

A sobrecarga de opções de FindNextElement não pode ser usada com a navegação por tabulação, pois há suporte apenas para navegação direcional.

A imagem a seguir ilustra essas opções.

Por exemplo, quando B tem foco, chamar FindNextElement (com várias opções definidas para indicar que o foco se move para a direita) define o foco em I. Os motivos são:

1.  A referência não é B, é A devido a HintRect
2.  C é excluído porque o candidato em potencial deve estar em MyPanel (o SearchRoot)
3.  F é excluído porque sobrepõe o retângulo de exclusões

![dicas de navegação](images/keyboard/navigation-hints.png)

*Comportamento de foco com base em dicas de navegação*

### <a name="no-focus-candidate-available"></a>Nenhum candidato de foco disponível

O evento UIElement.NoFocusCandidateFound é disparado quando o usuário tenta mover o foco com as teclas tab ou seta, mas não há nenhum candidato possível na direção especificada. Esse evento não é disparado para FocusManager.TryMoveFocus().

Como é um evento roteado, ele sobe do elemento focalizado por meio de sucessivos objetos pais até a raiz da árvore de objetos. Dessa forma, você pode manipular o evento onde for apropriado.

<a name="split-view-code-sample"></a> O exemplo a seguir mostra uma Grade que abre uma exibição dividida quando o usuário tenta mover o foco para a esquerda do controle focalizável na extrema esquerda, conforme descrito em [Projeto para documentação de TV](designing-for-tv.md#navigation-pane).

```XAML
<Grid NoFocusCandidateFound="OnNoFocusCandidateFound">  ...</Grid>
```

```csharp
private void OnNoFocusCandidateFound (UIElement sender, NoFocusCandidateFoundEventArgs args)
{
            if(args.NavigationDirection == FocusNavigationDirection.Left)
            {
         if(args.InputDevice == FocusInputDeviceKind.Keyboard ||
            args.InputDevice == FocusInputDeviceKind.GameController )
                {
                OpenSplitPaneView();
                }
            args.Handled = true;
            }
}
```

## <a name="focus-events"></a>Eventos de foco

Os eventos [UIElement.GotFocus](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.UIElement.GotFocus) e UIElement.LostFocus são acionados quando um controle recebe ou perde o foco, respectivamente. Esse evento não é disparado para FocusManager.TryMoveFocus().

Como é um evento roteado, ele sobe do elemento focalizado por meio de sucessivos objetos pais até a raiz da árvore de objetos. Dessa forma, você pode manipular o evento onde for apropriado.

Os eventos **UIElement.GettingFocus** e **UIElement.LosingFocus** também surgem do controle, mas *antes* da mudança de foco. Isso oferece ao aplicativo a oportunidade de redirecionar ou cancelar a mudança de foco.

Observe que os eventos GettingFocus e LosingFocus são síncronos, enquanto os eventos GotFocus e LostFocus são assíncronos. Por exemplo, se o foco se move porque o aplicativo chama o método Control.Focus(), GettingFocus é acionado durante a chamada, mas GotFocus é gerado após a chamada.

UIElements pode vincular aos eventos GettingFocus e LosingFocus e alterar o destino (usando a propriedade NewFocusedElement) antes de mover o foco. Mesmo se o alvo foi alterado, o evento ainda se move e outro pai pode alterar o destino novamente.

Para evitar problemas de reentrância, as exceções são geradas se você tentar mover o foco (usando FocusManager.TryMoveFocus ou Control.Focus) enquanto esses eventos estão sendo propagados.

Esses eventos são acionados independentemente do motivo para mover o foco (por exemplo, navegação por guias, navegação XYFocus, programática).

A imagem a seguir mostra como fazer isso, ao mover de A para a direita, o XYFocus escolhe LVI4 como um candidato. O LVI4 dispara o evento GettingFocus, onde o ListView tem a oportunidade de reatribuir foco a LVI3.

![eventos de foco](images/keyboard/focus-events.png)

*eventos de foco*

Este exemplo de código mostra como manipular o evento OnGettingFocus e redirecionar o foco com base no local onde o foco foi definido anteriormente.

```XAML
<StackPanel Orientation="Horizontal">
    <Button Content="A" />
    <ListView x:Name="MyListView" SelectedIndex="2" GettingFocus="OnGettingFocus">
        <ListViewItem>LV1</ListViewItem>
        <ListViewItem>LV2</ListViewItem>
        <ListViewItem>LV3</ListViewItem>
        <ListViewItem>LV4</ListViewItem>
        <ListViewItem>LV5</ListViewItem>
    </ListView>
</StackPanel>
```

```csharp
private void OnGettingFocus(UIElement sender, GettingFocusEventArgs args)
  {

                 //Redirect the focus only when the focus comes from outside of the ListView.
       // move the focus to the selected item
            if (MyListView.SelectedIndex != -1 && IsNotAChildOf(MyListView, args.OldFocusedElement))
            {
                var selectedContainer = MyListView.ContainerFromItem(MyListView.SelectedItem);
                if (args.FocusState == FocusState.Keyboard && args.NewFocusedElement != selectedContainer)
                {
                    args.NewFocusedElement = MyListView.ContainerFromItem(MyListView.SelectedItem);
                    args.Handled = true;
                }
            }        
  }
```

Este exemplo de código mostra como manipular o evento OnLosingFocus para um objeto CommandBar e aguarda para definir o foco até o menu DropDown ser fechado.

```XAML
<CommandBar x:Name="MyCommandBar" LosingFocus="OnLosingFocus">
     <AppBarButton Icon="Back" Label="Back" />
     <AppBarButton Icon="Stop" Label="Stop" />
     <AppBarButton Icon="Play" Label="Play" />
     <AppBarButton Icon="Forward" Label="Forward" />

     <CommandBar.SecondaryCommands>
         <AppBarButton Icon="Like" Label="Like" />
         <AppBarButton Icon="Share" Label="Share" />
     </CommandBar.SecondaryCommands>
 </CommandBar>
```

```csharp
private void OnLosingFocus(UIElement sender, LosingFocusEventArgs args)
{
          if (MyCommandBar.IsOpen == true && IsNotAChildOf(MyCommandBar, args.NewFocusedElement))
          {
              args.Cancel = true;
              args.Handled = true;
          }
}
```

### <a name="losingfocus-and-gettingfocus-event-order"></a>Ordem dos eventos LosingFocus e GettingFocus

LosingFocus e GettingFocus são eventos síncronos, portanto, o foco não será movido enquanto esses eventos são propagados. Entretanto, como LostFocus e GotFocus são eventos assíncronos, não há nenhuma garantia de que o foco não se moverá novamente antes do manipulador ser executado. A única garantia é que o manipulador LostFocus é executado antes do manipulador GotFocus.

A ordem de execução é:

1.  Sincronizar LosingFocus: se LosingFocus redefine o foco para o elemento que perdia o foco ou ao definir o Cancelar = true, nenhum outro evento
2.  Sincronizar GetingFocus: se GettingFocus redefine o foco para o elemento que perdia o foco ou ao definir o Cancelar = true, nenhum outro evento
3.  LostFocus assíncrono
4.  GotFocus assíncrono

## <a name="find-the-first-and-last-focusable-element-a-namefindfirstfocusableelement"></a>Encontre o primeiro e o último elemento focalizável <a name="findfirstfocusableelement">

Os métodos **FocusManager.FindFirstFocusableElement** e **FocusManager.FindLastFocusableElement** permitem que você mova o foco para o primeiro ou o último elemento focalizável no escopo de um objeto (a árvore de elementos de um UIElement) ou a árvore de texto de um TextElement. Você especifica o escopo ao chamar esses métodos. Se o argumento for nulo, o escopo será a janela atual.

**Observação** Esses métodos podem retornar nulo se não houver nenhum item focalizável no escopo.

<a name="popup-ui-code-sample"></a>O exemplo de código a seguir mostra como especificar que os botões de uma CommandBar têm um comportamento direcional deliminador descrito no artigo [Interações de teclado](keyboard-interactions.md#popup-ui).

```XAML
<CommandBar x:Name="MyCommandBar" LosingFocus="OnLosingFocus">
            <AppBarButton Icon="Back" Label="Back" />
            <AppBarButton Icon="Stop" Label="Stop" />
            <AppBarButton Icon="Play" Label="Play" />
            <AppBarButton Icon="Forward" Label="Forward" />

            <CommandBar.SecondaryCommands>
                <AppBarButton Icon="Like" Label="Like" />
                <AppBarButton Icon="ReShare" Label="Share" />
            </CommandBar.SecondaryCommands>
</CommandBar>
```

```csharp
private void OnLosingFocus(UIElement sender, LosingFocusEventArgs args)
        {
            if (IsNotAChildOf(MyCommandBar, args.NewFocussedElement))
            {
                DependencyObject candidate = null;
                if (args.Direction == FocusNavigationDirection.Left)
                {
                    candidate = FocusManager.FindLastFocusableElement(MyCommandBar);
                }
                else if (args.Direction == FocusNavigationDirection.Right)
                {
                    candidate = FocusManager.FindFirstFocusableElement(MyCommandBar);
                }
                if (candidate != null)
                {
                    args.NewFocusedElement = candidate;
                    args.Handled = true;
                }
            }
        }
```
