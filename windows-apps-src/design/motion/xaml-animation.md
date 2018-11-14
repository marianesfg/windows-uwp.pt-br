---
author: Jwmsft
ms.assetid: 0C8DEE75-FB7B-4E59-81E3-55F8D65CD982
title: Visão geral de animações
description: Use as animações da biblioteca de animações do Windows Runtime para integrar a aparência do Windows ao seu app.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d7c3c4a9e46ce38298d7dcdd50477c4de0e9960c
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6188891"
---
# <a name="animations-in-xaml"></a>Animações em XAML

As animações UWP podem aprimorar seu app adicionando movimento e interatividade. Usando as animações da biblioteca de animações do Windows Runtime, você pode integrar a aparência do Windows ao seu aplicativo. Este tópico resume as animações e fornece exemplos de cenários típicos nos quais cada uma é usada.

> [!TIP]
> Os controles do Windows Runtime para XAML incluem determinados tipos de animações como comportamentos internos que vêm de uma biblioteca de animações. Usando esses controles em seu aplicativo, você pode obter a aparência animada sem precisar programá-lo.

As animações da Biblioteca de Animação do Windows Runtime oferecem estas vantagens:

-   Animações que se alinham às [Diretrizes para animações](https://msdn.microsoft.com/library/windows/apps/Dn611854)
-   Transições rápidas e flexíveis entre os estados da interface do usuário, as quais informam, mas não distraem o usuário
-   Comportamento visual que indica transições dentro de um aplicativo para o usuário

Por exemplo, quando o usuário adiciona um item a uma lista, em vez do novo item aparecer instantaneamente na lista, ele aparece como uma animação no local. Os outros itens da lista aparecem como animação nas novas posições por um período curto, dando espaço para o item adicionado. Esse comportamento de transição torna a interação do controle mais aparente para o usuário.

O Windows 10, versão 1607, introduz uma nova API [**ConnectedAnimationService**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.connectedanimationservice.aspx) para a implementação de animações onde um elemento parece se animar entre os modos de exibição durante uma navegação. Essa API tem um padrão de uso diferente das outras APIs da Biblioteca de Animação. O uso de **ConnectedAnimationService** é explicado na [página de referência](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.connectedanimationservice.aspx).

A Biblioteca de Animação não fornece animações para todos os cenários possíveis. Há casos em que convém criar uma animação personalizada em XAML. Para saber mais, consulte [Animações com storyboard](storyboarded-animations.md).

Além disso, para determinados cenários avançados como animação de um item com base na posição de rolagem de um ScrollViewer, os desenvolvedores podem querer usar interoperação de Camada Visual para implementar animações personalizadas. Para obter mais informações, consulte [Camada Visual](https://msdn.microsoft.com/windows/uwp/composition/visual-layer).

## <a name="types-of-animations"></a>Tipos de animações

O sistema de animações do Windows Runtime e a biblioteca de animações cumprem a meta maior de habilitar controles e outras partes da interface do usuário para que tenham um comportamento animado. Há vários tipos distintos de animações.

-   *Transições de tema* são aplicadas automaticamente quando determinadas condições mudam na interface do usuário, envolvendo controles ou elementos de tipos predefinidos de interface do usuário do Windows Runtime para XAML. Elas são chamadas de *transições de temas* porque as animações dão suporte à aparência do Windows e definem o que todos os aplicativos fazem em determinados cenários de interface do usuário quando mudam de um modo de interação para outro. As transições de tema fazem parte da biblioteca de animações.
-   *Animações de tema* são animações para uma ou mais propriedades de tipos de interface de usuário predefinidos do Windows Runtime para XAML. As animações de tema são diferentes das transições de temas porque as animações de tema tem por destino um elemento específico e existem em estados visuais específicos dentro de um controle, enquanto as transições de temas são atribuídas a propriedades do controle que existem fora dos estados visuais e influenciam as transições entre esses estados. Muitos dos controles XAML do Windows Runtime incluem animações de tema dentro dos storyboards que fazem parte do modelo do controle, com as animações ativadas pelos estados visuais. Contanto que você não esteja modificando os modelos, terá essas animações de tema inseridas disponíveis para os controles na interface do usuário. No entanto, se você substituir os modelos, estará removendo as animações de tema do controle inserido também. Para recuperá-las, você deve definir um storyboard que inclua animações de tema dentro do conjunto do controle dos estados visuais. Você também pode executar animações de tema a partir de storyboards que não estão dentro de estados visuais e pode iniciá-las com o método [**Begin**](https://msdn.microsoft.com/library/windows/apps/BR210491), mas isso é menos comum. As animações de tema fazem parte da biblioteca de animação.
-   *Transições visuais* são aplicadas quando um controle faz a transição de um de seus estados visuais definidos para outro estado. Elas são animações personalizadas que você escreve e normalmente estão relacionadas ao modelo personalizado que você escreve para um controle e as definições de estados visuais nesse modelo. A animação só é executada durante o tempo entre os estados, que normalmente é um curto período de tempo, sendo no máximo alguns segundos. Para obter mais informações, consulte a seção ["Transição visual" das animações com storyboard para estados visuais](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808#VisualTransition).
-   *Animações com storyboard* animam o valor de uma propriedade de dependência do Windows Runtime com o tempo. Storyboards podem ser definidos como parte de uma transição visual ou disparados no tempo de execução pelo aplicativo. Para saber mais, consulte [Animações com storyboard](storyboarded-animations.md). Para saber mais sobre as propriedades de dependência e onde elas existem, consulte [Visão geral das propriedades de dependência](https://msdn.microsoft.com/library/windows/apps/Mt185583).
-   *Animações conectadas* fornecidas pela nova API [**ConnectedAnimationService**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.connectedanimationservice.aspx) permitem que os desenvolvedores criem com facilidade um efeito onde um elemento parece se animar entre os modos de exibição durante uma navegação. Essa API está disponível a partir do Windows 10, versão 1607. Consulte [**ConnectedAnimationService**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.connectedanimationservice.aspx) para obter mais informações.

## <a name="animations-available-in-the-library"></a>Animações disponíveis na biblioteca

As seguintes animações são fornecidas na Biblioteca de Animação. Clique no nome da animação para aprender mais sobre o uso principal dos cenários, como defini-las e para ver um exemplo da animação.

-   [Transição de página](#page-transition): anima transições de página em um [**quadro**](https://msdn.microsoft.com/library/windows/apps/br242682).
-   [Transição de conteúdo e entrada](#content-transition-and-entrance-transition): anima uma parte ou conjunto de conteúdo dentro e fora da exibição.
-   [Fade in, fade out e fading cruzado](#fade-in-out-and-crossfade): exibe os elementos ou controles transitórios, ou atualiza uma área de conteúdo.
-   [Ponteiro para cima/baixo](#pointer-up-down): fornece feedback visual de um toque ou clique em um bloco.
-   [Reposicionar](#reposition): move um elemento para uma nova posição.
-   [Mostrar/ocultar pop-up](#show-hide-popup): exibe a interface do usuário contextual sobre a exibição.
-   [Mostrar/ocultar interface do usuário de borda](#show-hide-edge-ui): desliza a interface do usuário baseada em borda, incluindo interfaces grandes como painéis, para dentro ou para fora da exibição.
-   [Alterações em itens de lista](#list-item-changes): adiciona ou exclui um item de uma lista ou reordena os itens.
-   [Arrastar e soltar](#drag-drop): fornece um feedback visual durante uma operação de arrastar e soltar.

### <a name="page-transition"></a>Transição da página

Use transições da página para animar a navegação dentro de um aplicativo. Como quase todos os aplicativos usam algum tipo de navegação, as animações de transição da página são o tipo mais comum de animação de tema usado por aplicativos. Consulte [**NavigationThemeTransition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.navigationthemetransition) para obter mais informações sobre as APIs de transição da página.



### <a name="content-transition-and-entrance-transition"></a>Transição de conteúdo e transição de entrada

Use animações de transição de conteúdo ([**ContentThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR243103)) para mover um conteúdo ou conjunto de conteúdos para dentro ou para fora da exibição atual. Por exemplo, as animações de transição de conteúdo mostram o conteúdo que não estava pronto para exibição quando a página carregou pela primeira vez, ou quando o conteúdo muda em uma seção de uma página.

[**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210288) representa um movimento que pode ser aplicado ao conteúdo quando uma página ou seção grande da interface do usuário é carregada pela primeira vez. Dessa forma, a primeira aparência do conteúdo pode oferecer um feedback diferente da alteração do conteúdo. [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210288) é equivalente a um [**NavigationThemeTransition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.navigationthemetransition) com os parâmetros padrão, mas pode ser usado fora de um [**quadro**](https://msdn.microsoft.com/library/windows/apps/br242682).
 
 
<span id="fade-in-out-and-crossfade"/>

### <a name="fade-inout-and-crossfade"></a>Fade in, fade out e fading cruzado

Use animações fade in e fade out para mostrar ou ocultar controles ou interface do usuário transiente. Em XAML, elas são representadas como [**FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210298) e [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302). Um exemplo está na barra de aplicativos na qual novos controles podem aparecer devido à interação do usuário. Outro exemplo seria uma barra de rolagem ou indicador panorâmico transitório que desaparece lentamente (fade out) depois que não é detectada nenhuma entrada do usuário por algum tempo. O aplicativos também devem usar a animação de fade in ao fazerem a transição de um item de espaço reservado até o item final conforme o conteúdo é carregado dinamicamente.

Use uma animação de fading cruzado para suavizar a transição quando o estado de um item está mudando; por exemplo, aquando o aplicativo atualiza o conteúdo atual de uma exibição. A biblioteca de animação XAML não fornece uma animação de fading cruzado dedicada (não há equivalente para [**crossFade**](https://msdn.microsoft.com/library/windows/apps/BR212661)), mas você pode obter o mesmo resultado usando [**FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210298) e [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) com tempo sobreposto.

<span id="pointer-up-down"/>

### <a name="pointer-updown"></a>Ponteiro para cima/para baixo

Use as animações [**PointerUpThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/Hh969168) e [**PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/Hh969164) para dar feedback visual ao usuário sobre o êxito ao tocar ou clicar em um bloco. Por exemplo, quando um usuário clica ou toca em um bloco, a animação do ponteiro para baixo é reproduzida. Após a liberação do toque ou do clique, a animação de ponteiro para cima é reproduzida.

### <a name="reposition"></a>Reposicionar

Use as animações de reposicionar ([**RepositionThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210421) ou [**RepositionThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210429)) para mover um elemento para uma nova posição. Por exemplo, a movimentação dos cabeçalhos em um controle de itens usa a animação de reposicionar.

<span id="show-hide-popup"/>

### <a name="showhide-popup"></a>Mostrar/ocultar popup

Use [**PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210383) e [**PopOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210391) quando mostrar e ocultar um [**Popup**](https://msdn.microsoft.com/library/windows/apps/BR227842) ou uma interface do usuário contextual semelhante sobre a exibição atual. [**PopupThemeTransition**](https://msdn.microsoft.com/library/windows/apps/Hh969172) é uma transição de tema que é um feedback útil se você deseja fazer o light dismiss de um popup.

<span id="show-hide-edge-ui"/>

### <a name="showhide-edge-ui"></a>Mostrar/ocultar interface do usuário de borda

Use a animação [**EdgeUIThemeTransition**](https://msdn.microsoft.com/library/windows/apps/Hh702324) para deslizar interfaces do usuário pequenas e baseadas em borda para dentro ou fora da exibição. Por exemplo, use essas animações quando mostrar uma barra de aplicativos personalizada na parte superior ou inferior da tela ou de uma superfície de IU sobre erros e avisos na parte superior da tela.

Use a animação [**PaneThemeTransition**](https://msdn.microsoft.com/library/windows/apps/Hh969160) para mostrar e ocultar um painel. Isso é para interfaces do usuário grandes baseadas em borda, como um teclado personalizado ou um painel de tarefas.

### <a name="list-item-changes"></a>Alterações de itens de lista

Use a animação [**AddDeleteThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR243047) para adicionar comportamento animado quando adicionar ou excluir um item de uma lista existente. Para adicionar, a transição vai primeiro reposicionar os itens existentes na lista para abrir espaço para os novos itens e, depois, adicionar novos itens. Para exclusões, a transição remove itens de uma lista e, se necessário, reposiciona os demais itens da lista depois que os itens excluídos são removidos.

Há também uma animação [**ReorderThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210409) separada que pode ser aplicada se um item mudar de posição em uma lista. Isso é animado de maneira diferente da exclusão de um item e da adição dele em um novo lugar com as animações associadas de exclusão/adição.

Observe que essas animações são incluídas nos modelos padrão [**ListView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) e [**GridView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx), portanto, você não precisa adicionar manualmente essas animações se já estiver usando esses controles.

<span id="drag-drop"/>

### <a name="dragdrop"></a>Arrastar/soltar

Use as animações de arrastar ([**DragItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243173), [**DragOverThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243177)) e soltar ([**DropTargetItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243185)) para dar feedback visual quando o usuário arrastar ou soltar um item.

Quando ativas, as animações mostram ao usuário que a lista pode ser reorganizada ao redor de um item solto. Isso é útil para usuários que sabem onde o item será colocado em uma lista se for solto no local atual. As animações fornecem feedback visual de que um item arrastado pode ser solto entre dois outros itens da lista e que esses itens sairão do caminho.

## <a name="using-animations-with-custom-controls"></a>Usando animações com controles personalizados

A tabela a seguir resume as recomendações para uso da animação certa quando você cria uma versão personalizada destes controles do Windows Runtime:

| Tipo de interface do usuário | Animação recomendada |
|---------|-----------------------|
| Caixa de diálogo | [**FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210298) e [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) |
| Submenu | [**PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.popinthemeanimation.popinthemeanimation.aspx) e [**PopOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.popoutthemeanimation.popoutthemeanimation) |
| Dica de ferramenta | [**FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210298) e [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) |
| Menu de contexto | [**PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.popinthemeanimation.popinthemeanimation.aspx) e [**PopOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.popoutthemeanimation.popoutthemeanimation) |
| Barra de comandos | [**EdgeUIThemeTransition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.edgeuithemetransition.edgeuithemetransition) |
| Painel de tarefas ou painel baseado em borda | [**PaneThemeTransition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.panethemetransition.panethemetransition) |
| Conteúdo de qualquer contêiner de interface do usuário | [**ContentThemeTransition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.contentthemetransition.contentthemetransition) |
| Para controles ou caso nenhuma outra animação se aplique | [**FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.fadeinthemeanimation.fadeinthemeanimation.aspx) e [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) |

 

## <a name="transition-animation-examples"></a>Exemplos de animação de transição

Em condições ideias, seu aplicativo usa animações para aprimorar a interface do usuário ou para torná-la mais atraente sem incomodar os usuários. Uma maneira de fazer isso é aplicando transições animadas à interface do usuário de modo que quando um elemento entra ou sai da tela ou sofre qualquer alteração, a animação chama a atenção do usuário para a alteração. Por exemplo, seus botões podem esmaecer, em vez de simplesmente aparecer e desaparecer. Nós criamos diversas APIs que podem ser usadas para fazer transições de animação típicas ou recomendadas consistentes. O exemplo a seguir mostra como aplicar uma animação a um botão para que ele deslize rapidamente na exibição.

```xml
<Button Content="Transitioning Button">
     <Button.Transitions>
         <TransitionCollection> 
             <EntranceThemeTransition/>
         </TransitionCollection>
     </Button.Transitions>
 </Button>
 ```

Neste código, adicionamos o objeto [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210288) à coleção de transição do botão. Agora, quando o botão for renderizado pela primeira vez, ele deslizará rapidamente na exibição em vez de simplesmente desaparecer. Você pode definir algumas propriedades no objeto de animação para ajustar o quanto ele desliza e de qual direção, mas o objetivo real é oferecer uma API simples para um cenário específico, ou seja, para criar uma entrada chamativa.

Você também pode definir temas de animação de transição nos recursos de estilo do aplicativo. Assim, é possível aplicar o efeito de maneira uniforme. Este exemplo equivale ao anterior, exceto por ser aplicado usando um [**Style**](https://msdn.microsoft.com/library/windows/apps/BR208849):

```xml
<UserControl.Resources>
     <Style x:Key="DefaultButtonStyle" TargetType="Button">
         <Setter Property="Transitions">
             <Setter.Value>
                 <TransitionCollection>
                     <EntranceThemeTransition/>
                 </TransitionCollection>
             </Setter.Value>
        </Setter>
    </Style>
</UserControl.Resources>
      
<StackPanel x:Name="LayoutRoot">
    <Button Style="{StaticResource DefaultButtonStyle}" Content="Transitioning Button"/>
</StackPanel>
```

Os exemplos anteriores aplicam uma transição de tema a um controle individual, contudo, as transições de temas são ainda mais interessantes quando você as aplica a um contêiner de objetos. Quando você faz isso, todos os objetos filho do contêiner participam da transição. No exemplo a seguir, uma [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210288) é aplicada a uma [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) de retângulos.

```xml
<!-- If you set an EntranceThemeTransition animation on a panel, the
     children of the panel will automatically offset when they animate
     into view to create a visually appealing entrance. -->        
<ItemsControl Grid.Row="1" x:Name="rectangleItems">
    <ItemsControl.ItemContainerTransitions>
        <TransitionCollection>
            <EntranceThemeTransition/>
        </TransitionCollection>
    </ItemsControl.ItemContainerTransitions>
    <ItemsControl.ItemsPanel>
        <ItemsPanelTemplate>
            <WrapGrid Height="400"/>
        </ItemsPanelTemplate>
    </ItemsControl.ItemsPanel>
            
    <!-- The sequence children appear depends on their order in 
         the panel's children, not necessarily on where they render
         on the screen. Be sure to arrange your child elements in
         the order you want them to transition into view. -->
    <ItemsControl.Items>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
    </ItemsControl.Items>
</ItemsControl>
```

Os retângulos filho do elemento [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) são exibidos um após o outro de maneira visualmente agradável em vez de todos ao mesmo tempo, como aconteceria se você tivesse aplicado essa animação aos retângulos individualmente.

Veja uma demonstração desta animação:

![Animação mostrando a transição de um retângulo filho na exibição](./images/animation-child-rectangles.gif)

Os objetos filho de um contêiner também podem refluir quando um ou mais filhos mudarem de posição. No exemplo a seguir, aplicamos uma [**RepositionThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210429) à grade de retângulos. Quando você remove um dos retângulos, todos os outros refluem para suas novas posições.

```xml
<Button Content="Remove Rectangle" Click="RemoveButton_Click"/>
        
<ItemsControl Grid.Row="1" x:Name="rectangleItems">
    <ItemsControl.ItemContainerTransitions>
        <TransitionCollection>
                    
            <!-- Without this, there would be no animation when items 
                 are removed. -->
            <RepositionThemeTransition/>
        </TransitionCollection>
    </ItemsControl.ItemContainerTransitions>
    <ItemsControl.ItemsPanel>
        <ItemsPanelTemplate>
            <WrapGrid Height="400"/>
        </ItemsPanelTemplate>
    </ItemsControl.ItemsPanel>
            
    <!-- All these rectangles are just to demonstrate how the items
         in the grid re-flow into position when one of the child items
         are removed. -->
    <ItemsControl.Items>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
    </ItemsControl.Items>
</ItemsControl>
```

```cs
private void RemoveButton_Click(object sender, RoutedEventArgs e)
{
    if (rectangleItems.Items.Count > 0)
    {    
        rectangleItems.Items.RemoveAt(0);
    }                         
}
```

```cpp
// .h
private:
void RemoveButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e);

//.cpp
void BlankPage::RemoveButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e)
{
    if (rectangleItems->Items->Size > 0)
    {    
        rectangleItems->Items->RemoveAt(0);
    }
}
```

Você pode aplicar várias animações de transição a um único objeto ou contêiner de objetos. Por exemplo, se você deseja que a lista de retângulos seja animada na exibição e também quando eles mudarem de posição, aplique [**RepositionThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210429) e [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210288) desta maneira:

```xml
...
<ItemsControl.ItemContainerTransitions>
    <TransitionCollection>
        <EntranceThemeTransition/>                    
        <RepositionThemeTransition/>
    </TransitionCollection>
</ItemsControl.ItemContainerTransitions>
...      
```

Há vários efeitos de transição para criar animações nos seus elementos da interface do usuário à medida que são adicionados, removidos, reorganizados etc. Os nomes de todas essas APIs contêm "ThemeTransition":

| API | Descrição |
|-----|-------------|
| [**NavigationThemeTransition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.navigationthemetransition) | Fornece uma animação de personalidade do Windows para a navegação da página em um [**quadro**](https://msdn.microsoft.com/library/windows/apps/br242682). |
| [**AddDeleteThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR243047) | Fornece o comportamento de transição animada para quando controles adicionam ou excluem filhos ou conteúdo. Geralmente o controle é um contêiner de itens. |
| [**ContentThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR243103) | Fornece o comportamento de transição animada para quando o conteúdo de um controle está mudando. Você pode aplicar isso além de [**AddDeleteThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR243047). |
| [**EdgeUIThemeTransition**](https://msdn.microsoft.com/library/windows/apps/Hh702324) | Fornece o comportamento de transição animada para uma transição de interface do usuário de borda (pequena). |
| [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210288) | Fornece o comportamento de transição animada para quando os controles aparecerem pela primeira vez. |
| [**PaneThemeTransition**](https://msdn.microsoft.com/library/windows/apps/Hh969160) | Fornece o comportamento de transição animada para uma transição de painel (interface do usuário grande). |
| [**PopupThemeTransition**](https://msdn.microsoft.com/library/windows/apps/Hh969172) | Fornece o comportamento de transição animada que se aplica a componentes pop-in de controles (por exemplo, interface do usuário do tipo dica de ferramenta em um objeto) à medida que eles aparecem. |
| [**ReorderThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210409) | Fornece o comportamento de transição animada para quando muda a organização de itens de controles de exibição. Normalmente, isso ocorre como resultado de uma operação de arrastar-e-soltar. Controles e temas diferentes podem ter características diferentes para as animações. |
| [**RepositionThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210429) | Fornece o comportamento de transição animada para quando os controles aparecerem mudam de posição. |

 

## <a name="theme-animation-examples"></a>Exemplos de animação de tema

As animações de transição são simples de aplicar. Mas você pode ter um pouco mais de controle sobre o tempo e a ordem dos efeitos de animação. Você pode usar animações de tema para ter mais controle sobre o comportamento da animação ao usar um tema consistente. As animações de temas também exigem menos marcações do que animações personalizadas. Neste exemplo, usamos [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) para fazer o fade out da exibição de um retângulo.

```xml
<StackPanel>    
    <StackPanel.Resources>
        <Storyboard x:Name="myStoryboard">
            <FadeOutThemeAnimation TargetName="myRectangle" />
        </Storyboard>
    </StackPanel.Resources>
    <Rectangle PointerPressed="Rectangle_Tapped" x:Name="myRectangle"  
              Fill="Blue" Width="200" Height="300"/>
</StackPanel>
```

```cs
// When the user taps the rectangle, the animation begins.
private void Rectangle_Tapped(object sender, PointerRoutedEventArgs e)
{
    myStoryboard.Begin();
}
```

```vb
' When the user taps the rectangle, the animation begins.
Private Sub Rectangle_Tapped(sender As Object, e As PointerRoutedEventArgs)
    myStoryboard.Begin()
End Sub
```

```cpp
//.h
void Rectangle_Tapped(Platform::Object^ sender, Windows::UI::Xaml::Input::PointerRoutedEventArgs^ e);

//.cpp
void BlankPage::Rectangle_Tapped(Object^ sender, PointerRoutedEventArgs^ e)
{
    myStoryboard->Begin();
}
```

Diferentemente de animações de transição, uma animação de tema não tem um gatilho interno (a transição) que a executa automaticamente. Você deve usar um [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) para conter uma animação de tema quando defini-la em XAML. Também é possível alterar o comportamento padrão da animação. Por exemplo, você pode tornar mais lento o fade-out aumentando o valor de tempo de [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) em [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302).

**Observação**para fins de Mostrar técnicas básicas de animação, estamos usando código do aplicativo para iniciar a animação chamando métodos de [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490). Você pode controlar como as animações de **Storyboard** são executadas usando os métodos [**Begin**](https://msdn.microsoft.com/library/windows/apps/BR210491), [**Stop**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.stop), [**Pause**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.pause.aspx) e [**Resume**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.resume.aspx) **Storyboard**. No entanto, esse não o modo como você geralmente inclui animações de biblioteca em aplicativos. Em vez disso, o comum é você integrar as animações de biblioteca dos estilos e modelos XAML aplicados aos controles ou elementos. Aprender sobre modelos e estados visuais é um pouco mais complicado. Entretanto, nós abordamos como usar animações de biblioteca em estados visuais como parte do tópico [Animações com storyboard para estados visuais](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808).

 

Você pode aplicar várias outras animações de temas aos elementos da sua interface do usuário para criar efeitos de animação. Os nomes de todas essas APIs contêm "ThemeAnimation":

| API | Descrição |
|-----|-------------|
| [**DragItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243173) | Representa a animação pré-configurada que se aplica a elementos de item que estão sendo arrastados. |
| [**DragOverThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243177) | Representa a animação pré-configurada que se aplica aos elementos de item sob um elemento que está sendo arrastado. |
| [**DropTargetItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243185) | A animação pré-configurada que se aplica a elementos de reprodução automática em potencial. |
| [**FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210298) | A animação de opacidade pré-configurada que se aplica a controles quando eles aparecem pela primeira vez. |
| [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) | A animação de opacidade pré-configurada que se aplica a controles quando eles são ocultados ou removidos da interface do usuário. |
| [**PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/Hh969164) | A animação pré-configurada para ação do usuário que toca ou clica em um item ou um elemento. |
| [**PointerUpThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/Hh969168) | A animação pré-configurada para ação do usuário que é executada depois que um usuário toca em um item ou um elemento e a ação é liberada. |
| [**PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210383) | A animação pré-configurada aplicada aos componentes pop-in dos controles quando eles aparecem. Esta animação combina opacidade e tradução. |
| [**PopOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210391) | A animação pré-configurada aplicada aos componentes pop-in dos controles quando eles são fechados ou removidos. Esta animação combina opacidade e tradução. |
| [**RepositionThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210421) | A animação pré-configurada para um objeto enquanto ele é reposicionado. |
| [**SplitCloseThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210454) | A animação pré-configurada que oculta uma interface do usuário de destino usando uma animação em um estilo de abertura e fechamento [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.combobox.aspx). |
| [**SplitOpenThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210472) | A animação pré-configurada que revela uma interface do usuário de destino usando uma animação em um estilo de abertura e fechamento [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.combobox.aspx). |
| [**DrillInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.drillinthemeanimation) | Representa uma animação pré-configurada que é executada quando um usuário avança em uma hierarquia lógica, como de uma página mestra para uma página de detalhes. |
| [**DrillOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.drilloutthemeanimation.aspx) | Representa uma animação pré-configurada que é executada quando um usuário retrocede em uma hierarquia lógica, como de uma página de detalhes para uma página mestra. |

 

## <a name="create-your-own-animations"></a>Crie suas próprias animações

Quando as animações de tema não são suficientes para suas necessidades, você pode criar suas próprias animações. Você anima os objetos por meio de um ou mais dos valores de suas propriedades. Por exemplo, você pode animar a largura de um retângulo, o ângulo de um [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932) ou o valor de cor de um botão. Chamamos esse tipo de animação personalizada de animação de storyboard, para distingui-la das animações da biblioteca que o Windows Runtime já fornece como um tipo de animação pré-configurada. Para animações de storyboard, use uma animação que pode alterar valores de um determinado tipo (por exemplo [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136) para animar um **Double**) e colocar essa animação dentro de um [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) para controlá-lo.

Para ser animada, a propriedade que você está animando deve ser uma *propriedade de dependência*. Para saber mais sobre as propriedades de dependência, consulte [Visão geral das propriedades de dependência](https://msdn.microsoft.com/library/windows/apps/Mt185583). Para obter mais informações sobre a criação de animações de storyboard personalizadas, incluindo como direcionar e controlá-las, consulte [animações de storyboard](storyboarded-animations.md).

A maior área do aplicativo de definição da interface do usuário do XAML, onde você definirá animações de storyboard personalizadas se estiver definindo estados visuais para controles no XAML. Você fará isso porque está criando uma nova classe de controle ou porque está remodelando um controle existente que tem estados visuais no seu modelo de controle. Para obter mais informações, consulte [Animações de storyboard para estados visuais](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808).

 

 




