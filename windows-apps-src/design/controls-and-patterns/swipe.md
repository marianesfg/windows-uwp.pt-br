---
pm-contact: kisai
design-contact: ksulliv
dev-contact: Shmazlou
doc-status: Published
Description: O comando de deslizar o dedo é um acelerador de toque para menus de contexto.
title: Swipe
label: Swipe
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 315edbddccc51b7e742bf9beffad8497a104ce03
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80614090"
---
# <a name="swipe"></a>Swipe

O comando de deslizar o dedo é um acelerador para menus de contexto que permite aos usuários acessar facilmente as ações comuns do menu apenas tocando, sem precisar alterar estados no aplicativo.

> **APIs da biblioteca de interface do usuário do Windows**: [SwipeControl](/uwp/api/microsoft.ui.xaml.controls.swipecontrol), [SwipeItem](/uwp/api/microsoft.ui.xaml.controls.swipeitem)
>
> **APIs da plataforma**: [SwipeControl](/uwp/api/windows.ui.xaml.controls.swipecontrol), [SwipeItem](/uwp/api/windows.ui.xaml.controls.swipeitem), [classe ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView)

![Executar e revelar o tema claro](images/LightThemeSwipe.png)

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

O comando de deslizar o dedo economiza espaço. É útil em situações em que o usuário pode executar a mesma operação em vários itens em rápida sucessão. E ele oferece "ações rápidas" em itens que não precisam de um pop-up completo ou mudança de estado dentro da página.

Você deve usar o comando de deslizar o dedo quando tiver um grupo potencialmente grande de itens e cada item tiver 1 a 3 ações que um usuário pode executar regularmente. Essas ações podem incluir, mas não se limitam a:

- Excluir
- Marcar ou arquivar
- Salvar ou baixar
- Responder

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/SwipeControl">abri-lo e ver o SwipeControl em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>Resumo do vídeo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev015/player]

## <a name="how-does-swipe-work"></a>Como o recurso de deslizar o dedo funciona?

O comando de deslizar o dedo na UWP tem dois modos: [Revelar](/uwp/api/windows.ui.xaml.controls.swipemode) e [Executar](/uwp/api/windows.ui.xaml.controls.swipemode). Também é compatível com quatro direções de diferentes para deslizar o dedo: para cima, para baixo, para a esquerda e direita.

### <a name="reveal-mode"></a>Modo de revelação

No modo de revelação, o usuário desliza o dedo em um item para abrir um menu de um ou mais comandos e deve tocar explicitamente em um comando para executá-lo. Quando o usuário desliza o dedo e solta um item, o menu permanece aberto até que um comando seja selecionado ou o menu seja fechado novamente ao deslizar o dedo mais uma vez, tocar ou rolar o item aberto na tela.

![Deslizar o dedo para revelar](images/SwipeCommand-Reveal_v2.gif)

Revelar é um modo de deslizar o dedo mais seguro e versátil, podendo ser usado para a maioria dos tipos de ações de menu, até mesmo potencialmente ações destrutivas, como exclusão.

Quando o usuário seleciona uma das opções de menu na abertura da revelação e deixa em modo de repouso, o comando para aquele item é invocado e o controle de deslizar o dedo é fechado.

### <a name="execute-mode"></a>Modo de execução

No modo de execução, o usuário desliza o dedo sobre um item para revelar e executar um único comando ao deslizar o dedo uma vez. Se o usuário soltar o item antes de deslizar o dedo além do limite, o menu fecha e o comando não é executado. Se o usuário desliza o dedo além do limite e solta o item, o comando é executado imediatamente.

![Deslizar o dedo para executar](images/SwipeCommand_Delete_v2.gif)

Se o usuário não soltar o dedo depois que o limite é alcançado e puxar o item deslizado fechado novamente, o comando não é executado e nenhuma ação é executada para aquele item.

O modo de execução fornece um feedback visual por meio da orientação de cores e rótulos ao deslizar o dedo sobre um item.

O modo de execução é usado da melhor forma quando a ação que o usuário está realizando é mais comum.

Ele também pode ser usado para ações destrutivas mais como excluir um item. No entanto, tenha em mente que Executar requer apenas uma ação de deslizar o dedo em uma direção, o contrário de Revelar, que exige que o usuário clique explicitamente em um botão.

### <a name="swipe-directions"></a>Direções de deslizar o dedo

Deslizar o dedo funciona em todas as direções cardinais: para cima, para baixo, esquerda e direita. Cada direção pode conter seus próprios itens ou conteúdo de deslizar o dedo, mas apenas uma instância de uma direção pode ser definida por vez em um único elemento com esse recurso.

Por exemplo, não é possível ter duas definições [LeftItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.LeftItems) no mesmo SwipeControl.

## <a name="how-to-create-a-swipe-command"></a>Como criar um comando de Deslizar o dedo

Os comandos de deslizar o dedo têm dois componentes que você precisa definir:

- O [SwipeControl](/uwp/api/windows.ui.xaml.controls.swipecontrol), que encapsula o conteúdo. Em uma coleção, como um ListView, isso fica dentro de DataTemplate.
- Os itens de menu de deslizar o dedo, que é um ou mais objetos [SwipeItem](/uwp/api/windows.ui.xaml.controls.swipeitem) colocados nos contêineres direcional do controle de passar o dedo: [LeftItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.LeftItems), [RightItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.RightItems), [TopItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.TopItems) ou [BottomItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.BottomItems)

O conteúdo de deslizar o dedo pode ser colocado embutido ou definido na seção Recursos da página ou aplicativo.

Veja um exemplo de um SwipeControl em um texto. Ele mostra a hierarquia de elementos XAML necessária para criar um comando de deslizar o dedo.

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

Agora vamos dar uma olhada em um exemplo mais completo de como você usaria normalmente comandos de deslizar o dedo em uma lista. Neste exemplo, você configurará um comando de exclusão que usa o modo de execução e um menu de outros comandos que usa o modo de revelação. Os dois conjuntos de comandos são definidos na seção Recursos da página. Você aplicará os comandos de deslizar o dedo até os itens em um ListView.

Primeiro, crie os itens de deslizar o dedo, que representam os comandos, como recursos de nível de página. SwipeItem usa um [IconSource](/uwp/api/windows.ui.xaml.controls.iconsource) como ícone. Crie os ícones como recursos também.

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

Lembre-se de manter os itens de menu no conteúdo de deslizar o dedo com rótulos de texto curtos e concisos. Essas ações devem ser as principais que um usuário pode executar várias vezes em um curto período.

A configuração da função de deslizar o dedo para funcionar em uma coleção ou ListView é exatamente igual a definir um único comando de deslizar o dedo (exemplo acima), exceto que você define o SwipeControl em um DataTemplate para que possa ser aplicado a cada item na coleção.

Veja um ListView com o SwipeControl aplicada ao item de DataTemplate. As propriedades LeftItems e RightItems fazem referência aos itens de deslizar o dedo que você criou como recursos.

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

## <a name="handle-an-invoked-swipe-command"></a>Manipular um comando de deslizar o dedo invocado

Para executar uma ação em um comando de deslizar o dedo, você manipula o evento [Invoked](/uwp/api/windows.ui.xaml.controls.swipeitem.Invoked). (Para saber mais sobre como um usuário pode invocar um comando, confira a seção _Como o recurso de deslizar o dedo funciona?_ neste artigo.) Normalmente, um comando de deslizar o dedo fica em um ListView ou um cenário de lista. Nesse caso, quando um comando é invocado, você desejará executar uma ação nesse item.

Veja como manipular o evento Invoked no item de deslizar o dedo de _exclusão_ que você criou anteriormente.

```xaml
<SwipeItems x:Key="ExecuteDelete" Mode="Execute">
    <SwipeItem Text="Delete" IconSource="{StaticResource DeleteIcon}"
               Background="Red" Invoked="Delete_Invoked"/>
</SwipeItems>
```

O item de dados é o DataContext do SwipeControl. No código, você pode acessar o item que foi deslizado obtendo a propriedade SwipeControl.DataContext dos argumentos de evento, como mostrado aqui.

```csharp
 private void Delete_Invoked(SwipeItem sender, SwipeItemInvokedEventArgs args)
 {
     sampleList.Items.Remove(args.SwipeControl.DataContext);
 }
```

> [!NOTE]
> Aqui, os itens foram adicionados diretamente à coleção ListView.Items para fins de simplicidade, portanto, o item também é excluído da mesma maneira. Se, em vez disso, você definir o ListView.ItemsSource como uma coleção, o que é mais comum, você precisará excluir o item da coleção de origem.

Nesse exemplo em particular, você removeu o item da lista, então o estado visual final do item deslizado não é importante. No entanto, em situações em que você simplesmente deseja realizar uma ação e, em seguida, recolher novamente o item de deslizar o dedo, você pode definir a propriedade [BehaviorOnInvoked](/uwp/api/windows.ui.xaml.controls.swipeitem.BehaviorOnInvoked) dos valores de enumeração [SwipeBehaviorOnInvoked](/uwp/api/windows.ui.xaml.controls.swipebehavioroninvoked).

- **Automático**
  - No modo de execução, o item de deslizar o dedo aberto permanecerá aberto quando invocado.
  - No modo de revelação, o item de deslizar o dedo aberto será recolhido quando invocado.
- **Close**
  - Quando o item é invocado, o controle de deslizar o dedo é sempre recolhido e volta ao normal independentemente do modo.
- **RemainOpen**
  - Quando o item é invocado, o controle de deslizar o dedo sempre fica aberto independentemente do modo.

Aqui, um item de deslizar o dedo de _resposta_ é definido para fechar depois que ele é invocado.

```xaml
<SwipeItem Text="Reply" IconSource="{StaticResource ReplyIcon}"
           Invoked="Reply_Invoked"
           BehaviorOnInvoked = "Close"/>
```

## <a name="dos-and-donts"></a>O que fazer e o que não fazer

- Não use o item de deslizar o dedo no FlipViews, nos Hubs ou nos Pivôs. A combinação pode ser confusa para o usuário devido a direções conflitantes de deslizar o dedo.
- Não combine a ação de deslizar o dedo na horizontal com a navegação horizontal ou a ação de deslizar o dedo na vertical com a navegação vertical.
- Verifique se o usuário está deslizando o dedo na mesma ação e se ela é consistente em todos os itens com o recurso de deslizar o dedo.
- Deslize o dedo nas principais ações que um usuário deseja executar.
- Use o recurso de deslizar o dedo nos itens em que a mesma ação é repetida muitas vezes.
- Use o recurso de deslizar o dedo na horizontal nos itens mais largos e na vertical em itens mais altos.
- Use rótulos de texto curtos e concisos.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

- [Exibição de lista e exibição de grade](listview-and-gridview.md)
- [Contêineres e modelos de itens](item-containers-templates.md)
- [Deslizar para atualizar](pull-to-refresh.md)
