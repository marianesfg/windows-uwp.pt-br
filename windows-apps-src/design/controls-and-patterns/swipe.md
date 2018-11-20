---
author: jwmsft
pm-contact: kisai
design-contact: ksulliv
dev-contact: Shmazlou
doc-status: Published
Description: Swipe commanding is a touch accelerator for context menus.
title: Deslizar o dedo
label: Swipe
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: daea428d6fad34116dac743655162c2f32315bea
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7282770"
---
# <a name="swipe"></a>Deslizar o dedo

O comando de deslizar o dedo é um acelerador para menus de contexto que permite aos usuários acessar facilmente as ações comuns do menu apenas tocando, sem precisar alterar estados no aplicativo.

> **APIs importantes**: [SwipeControl](/uwp/api/windows.ui.xaml.controls.swipecontrol), [SwipeItem](/uwp/api/windows.ui.xaml.controls.swipeitem), [classe ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView)

![Executar e revelar o tema claro](images/LightThemeSwipe.png)

## <a name="is-this-the-right-control"></a>Este é o controle correto?

O comando de passar o dedo economiza espaço. É útil em situações onde o usuário pode executar a mesma operação em vários itens em rápida sucessão. E ele oferece "ações rápidas" em itens que não precisam de um pop-up completo ou mudança de estado dentro da página.

Você deve usar o comando de passar o dedo quando tiver um grupo potencialmente grande de itens, e cada item tem 1 a 3 ações que um usuário pode executar regularmente. Essas ações podem incluir, mas não se limitam a:

- Excluir
- Marcar ou arquivar um item
- Salvar ou baixar
- Responder

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem um aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/SwipeControl">abrir o aplicativo e ver o SwipeControl em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obter o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>Resumo do vídeo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev015/player]

## <a name="how-does-swipe-work"></a>Como o recurso de deslizar o dedo funciona?

Comandos de passar o dedo UWP tem dois modos: [revelar](/uwp/api/windows.ui.xaml.controls.swipemode) e [Execute](/uwp/api/windows.ui.xaml.controls.swipemode). Ele também dá suporte a quatro direções de passar o dedo diferentes: para cima, para baixo, para a esquerda e direita.

### <a name="reveal-mode"></a>Modo de revelação

No modo de revelação, o usuário desliza o dedo em um item para abrir um menu de um ou mais comandos e deve tocar explicitamente em um comando para executá-lo. Quando o usuário desliza o dedo e solta um item, o menu permanece aberto até que um comando seja selecionado ou o menu seja fechado novamente ao deslizar o dedo novamente, tocar ou rolar o item aberto na tela.

![Deslizar o dedo para revelar](images/SwipeCommand-Reveal_v2.gif)

Revelar é um modo de deslizar o dedo mais seguro e versátil, podendo ser usado para a maioria dos tipos de ações de menu, até mesmo potencialmente ações destrutivas, como exclusão.

Quando o usuário seleciona uma das opções de menu na abertura da revelação e deixa em modo de repouso, o comando para aquele item é invocado e o controle de passar o dedo é fechado.

### <a name="execute-mode"></a>Modo de execução

No modo de execução, o usuário desliza o dedo sobre um item para revelar e executar um único comando ao deslizar o dedo uma vez. Se o usuário soltar o item sendo arrastado antes de passar o dedo além do limite, o menu fecha e o comando não é executado. Se o usuário desliza o dedo além do limite e solta o item, o comando é executado imediatamente.

![Deslizar o dedo para executar](images/SwipeCommand_Delete_v2.gif)

Se o usuário não soltar o dedo depois que o limite é alcançado, e puxar o item deslizado fechado novamente, o comando não é executado e nenhuma ação é executada para aquele item.

O modo de execução fornece um feedback visual por meio da orientação de cores e rótulos ao deslizar o dedo sobre um item.

O modo de execução é usado da melhor forma quando a ação que o usuário está realizando é mais comum.

Ele também pode ser usado para ações destrutivas mais como excluir um item. No entanto, tenha em mente que Execute requer apenas uma ação de passar o dedo em uma direção, em vez de revelação, o que exige que o usuário clique explicitamente em um botão.

### <a name="swipe-directions"></a>Direções de deslizar o dedo

A função de deslizar o dedo funciona em todas as direções cardinais: para cima, para baixo, esquerda e direita. Cada direção pode conter seus próprios itens ou conteúdo de deslizar o dedo, mas apenas uma instância de uma direção pode ser definida por vez em um único elemento com esse recurso.

Por exemplo, não é possível ter duas definições [LeftItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.LeftItems) no mesmo SwipeControl.

## <a name="how-to-create-a-swipe-command"></a>Como criar um comando de Deslizar o dedo

Passe o dedo comandos têm dois componentes que você precisa definir:

- O [SwipeControl](/uwp/api/windows.ui.xaml.controls.swipecontrol), que encapsula seu conteúdo. Em uma coleção, como um ListView, isso fica dentro de seu DataTemplate.
- Os itens de menu de passar o dedo, que é um ou mais [SwipeItem](/uwp/api/windows.ui.xaml.controls.swipeitem) objetos colocados em contêineres direcional do controle de passar o dedo: [LeftItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.LeftItems), [RightItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.RightItems), [TopItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.TopItems), ou [BottomItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.BottomItems)

Passe o dedo conteúdo pode ser colocado embutido ou definido na seção Resources da sua página ou aplicativo.

Aqui, um exemplo de um SwipeControl em um texto. Ele mostra a hierarquia de elementos XAML necessário para criar um comando de passar o dedo.

```xaml
<SwipeControl HorizontalAlignment="Center" VerticalAlignment="Center">
    <SwipeControl.LeftItems>
        <SwipeItems>
            <SwipeItem Text="Pin">
                <SwipeItem.IconSource>
                    <SymbolIconSource Symbol="Pin"/>
                </SwipeItem.IconSource>
            </SwipeItem>
        </SwipeItems>
    </SwipeControl.LeftItems>

     <!-- Swipeable content -->
    <Border Width="180" Height="44" BorderBrush="Black" BorderThickness="2">
        <TextBlock Text="Swipe to Pin" Margin="4,8,0,0"/>
    </Border>
</SwipeControl>
```

Agora vamos dar uma olhada em um exemplo mais completo de como você faria normalmente usar comandos de passar o dedo em uma lista. Neste exemplo, você irá configurar um comando de exclusão que usa o modo de execução e um menu de outros comandos que usa o modo de revelação. Os dois conjuntos de comandos são definidos na seção Resources da página. Você vai aplicar os comandos de passar o dedo até os itens em um ListView.

Primeiro, crie os itens de passar o dedo, que representam os comandos, como recursos de nível de página. SwipeItem usa um [IconSource](/uwp/api/windows.ui.xaml.controls.iconsource) como seu ícone. Crie os ícones como recursos, também.

```xaml
<Page.Resources>
    <SymbolIconSource x:Key="ReplyIcon" Symbol="MailReply"/>
    <SymbolIconSource x:Key="DeleteIcon" Symbol="Delete"/>
    <SymbolIconSource x:Key="PinIcon" Symbol="Pin"/>

    <SwipeItems x:Key="RevealOptions" Mode="Reveal">
        <SwipeItem Text="Reply" IconSource="{StaticResource ReplyIcon}"/>
        <SwipeItem Text="Pin" IconSource="{StaticResource PinIcon}"/>
    </SwipeItems>

    <SwipeItems x:Key="ExecuteDelete" Mode="Execute">
        <SwipeItem Text="Delete" IconSource="{StaticResource DeleteIcon}"
                   Background="Red"/>
    </SwipeItems>
</Page.Resources>
```

Lembre-se de manter os itens de menu que você tem em seu conteúdo de deslizar o dedo com rótulos de texto curtos e concisos. Essas ações devem ser as principais que um usuário pode executar várias vezes em um curto período.

A configuração da função de deslizar o dedo para funcionar em uma coleção ou ListView é exatamente igual a definir um único comando de passar o dedo (exemplo acima), exceto que você define o SwipeControl em um DataTemplate para que possa ser aplicado a cada item na coleção.

Aqui está um ListView com o SwipeControl aplicada em seu item de DataTemplate. As propriedades LeftItems e RightItems referência aos itens de passar o dedo que você criou como recursos.

```xaml
<ListView x:Name="sampleList" Width="300">
    <ListView.ItemContainerStyle>
        <Style TargetType="ListViewItem">
            <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
            <Setter Property="VerticalContentAlignment" Value="Stretch"/>
        </Style>
    </ListView.ItemContainerStyle>
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="x:String">
            <SwipeControl x:Name="ListViewSwipeContainer"
                          LeftItems="{StaticResource RevealOptions}"
                          RightItems="{StaticResource ExecuteDelete}"
                          Height="60">
                <StackPanel Orientation="Vertical">
                    <TextBlock Text="{x:Bind}" FontSize="18"/>
                    <StackPanel Orientation="Horizontal">
                        <TextBlock Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit..." FontSize="12"/>
                    </StackPanel>
                </StackPanel>
            </SwipeControl>
        </DataTemplate>
    </ListView.ItemTemplate>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

## <a name="handle-an-invoked-swipe-command"></a>Acessar um comando de deslizar o dedo invocado

Para executar uma ação em um comando de passar o dedo, você manipula seu evento [Invoked](/uwp/api/windows.ui.xaml.controls.swipeitem.Invoked). (Para obter mais informações sobre um como um usuário pode invocar um comando, examine o _como passar o dedo funciona? _seção anteriormente neste artigo.) Normalmente, um comando de passar o dedo é em um ListView ou cenário de lista. Nesse caso quando um comando é invocado, você desejará executar uma ação nesse item.

Aqui está como manipular o evento Invoked no _excluir_ item de passar o dedo, você criou anteriormente.

```xaml
<SwipeItems x:Key="ExecuteDelete" Mode="Execute">
    <SwipeItem Text="Delete" IconSource="{StaticResource DeleteIcon}"
               Background="Red" Invoked="Delete_Invoked"/>
</SwipeItems>
```

O item de dados é o DataContext do SwipeControl. Em seu código, você pode acessar o item que foi passado obtendo a propriedade SwipeControl.DataContext dos argumentos de evento, como mostrado aqui.

```csharp
 private void Delete_Invoked(SwipeItem sender, SwipeItemInvokedEventArgs args)
 {
     sampleList.Items.Remove(args.SwipeControl.DataContext);
 }
```

> [!NOTE]
> Aqui, os itens foram adicionados diretamente à coleção ListView.Items para fins de simplicidade, portanto, o item também é excluído da mesma maneira. Se você definir em vez disso, o ListView.ItemsSource como uma coleção, o que é mais comum, você precisa excluir o item da coleção de origem.

Nesse exemplo em particular, você removido o item da lista, então o estado visual final do dedo nelas item não for importante. No entanto, em situações em que você simplesmente deseja realizar uma ação e, então, passar o dedo recolher novamente, você pode definir a propriedade um [BehaviorOnInvoked](/uwp/api/windows.ui.xaml.controls.swipeitem.BehaviorOnInvoked) dos valores de enumeração [SwipeBehaviorOnInvoked](/uwp/api/windows.ui.xaml.controls.swipebehavioroninvoked).

- **Automático**
  - No modo de execução, o item de passar o dedo aberto permanecerá aberto quando invocado.
  - No modo de revelação, o item de passar o dedo aberto permanecerá recolhido quando invocado.
- **Fechar**
  - Quando o item é invocado, a ação de deslizar o dedo é sempre recolhida e volta ao normal independentemente do modo
- **RemainOpen**
  - Quando o item é invocado, a ação de deslizar o dedo permanece recolhida e volta ao normal independentemente do modo

Aqui, um _resposta_ item de passar o dedo é definido para fechar depois que ele é invocado.

```xaml
<SwipeItem Text="Reply" IconSource="{StaticResource ReplyIcon}"
           Invoked="Reply_Invoked"
           BehaviorOnInvoked = "Close"/>
```

## <a name="dos-and-donts"></a>O que fazer e o que não fazer

- Não use passe o dedo FlipViews, Hubs ou Pivôs. A combinação pode ser confusa para o usuário devido a direções conflitantes de deslizar o dedo.
- Não combine a ação de deslizar o dedo na navegação horizontal ou deslizar o dedo na vertical com a navegação vertical.
- Certifique-se de que o usuário está deslizando o dedo na mesma ação e se ela é consistente em todos os itens com o recurso de deslizar o dedo.
- Use passar o dedo para as principais ações que um usuário deseja executar.
- Não use o recurso de deslizar o dedo em itens onde a mesma ação é repetida muitas vezes
- Use o recurso de deslizar o dedo na horizontal nos itens mais largos e na vertical em itens mais altos.
- Use rótulos de texto curta e concisa.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

- [Exibição de lista e exibição de grade](listview-and-gridview.md)
- [Contêineres e modelos de itens](item-containers-templates.md)
- [Puxar para atualizar](pull-to-refresh.md)
