---
Description: Use nested UI to enable multiple actions on a list item
title: Interface do usuário aninhada em itens de lista
label: Nested UI in list items
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 60a29717-56f2-4388-a9ff-0098e34d5896
pm-contact: chigy
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 8edb38b8ae91d836e283a8eb37830850bf504db4
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8736156"
---
# <a name="nested-ui-in-list-items"></a>Interface do usuário aninhada em itens de lista

 

Interface do usuário aninhada é uma interface do usuário (IU) que expõe controles aninhados acionáveis colocados dentro de um contêiner que também pode ter foco independente.

Você pode usar a interface do usuário aninhada para apresentar ao usuário opções adicionais que ajudam a acelerar a execução de ações importantes. No entanto, quanto mais ações você expõe, mais complicada sua interface do usuário se torna. Você precisa tomar mais cuidado ao optar por usar esse padrão de interface do usuário. Este artigo fornece diretrizes para ajudar você a determinar o melhor curso de ação para sua interface do usuário específica.

> **APIs importantes**: [classe ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx), [classe GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx)

Neste artigo, vamos falar sobre a criação da interface do usuário aninhada em itens [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) e [GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx). Embora esta seção não fale sobre outros casos de interface do usuário aninhada, esses conceitos são transferíveis. Antes de começar, você deve estar familiarizado com as diretrizes gerais para usar controles ListView ou GridView em sua interface do usuário, que são encontradas nos artigos [Listas](lists.md) e [Exibição de lista e exibição de grade](listview-and-gridview.md).

Neste artigo, usamos os termos *lista*, *item de lista* e *interface do usuário aninhada* conforme definido aqui:
- *Lista* refere-se a uma coleção de itens contidos em uma exibição de lista ou exibição de grade.
- *Item de lista* refere-se a um item individual no qual o usuário pode executar uma ação em uma lista.
- *Interface do usuário aninhada* refere-se a elementos da interface do usuário em um item de lista nos quais o usuário pode executar uma ação separadamente da execução da ação no item de lista em si.

![Partes da interface do usuário aninhada](images/nested-ui-example-1.png)

> OBSERVAÇÃO&nbsp;&nbsp; Os controles ListView e GridView são derivados da classe [ListViewBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.aspx), portanto, eles têm a mesma funcionalidade, mas exibem dados de modo diferente. Neste artigo, ao falarmos sobre listas, as informações se aplicam aos controles ListView e GridView.

## <a name="primary-and-secondary-actions"></a>Ações principais e secundárias

Ao criar a interface do usuário com uma lista, considere quais ações o usuário pode executar dentre os itens de lista.  

- O usuário pode clicar em um item para executar uma ação?
    - Normalmente, clicar em um item de lista inicia uma ação, mas não precisa.
- Há mais de uma ação que o usuário pode executar?
    - Por exemplo, tocar em um email em uma lista abre o email. No entanto, pode haver outras ações, como excluir o email, que o usuário poderia executar sem abri-lo primeiro. Isso daria ao usuário o benefício de acessar essa ação diretamente na lista.
- Como as ações devem ser expostas ao usuário?
    - Considere todos os tipos de entrada. Algumas formas de interface do usuário aninhada funcionam muito bem com um método de entrada, mas talvez não funcionem com outros métodos.  

A *ação principal* é o que o usuário espera acontecer ao pressionar o item de lista.

*Ações secundárias* são normalmente aceleradores associados a itens de lista. Esses aceleradores podem ser para gerenciamento de lista ou ações relacionadas ao item de lista.

## <a name="options-for-secondary-actions"></a>Opções para ações secundárias

Ao criar a interface do usuário de lista, você precisa primeiro considerar todos os métodos de entrada que a UWP aceita. Para saber mais sobre os diferentes tipos de entrada, consulte a [Cartilha de entrada](../input/index.md).

Depois de confirmar que seu aplicativo aceita todas as entradas que a UWP aceita, você deve decidir se as ações secundárias de seu aplicativo são importantes o suficiente para serem expostas como aceleradores na lista principal. Lembre-se de que quanto mais ações que você expõe, mais complicado sua interface do usuário se torna. Você realmente precisa expor as ações secundárias na lista principal da interface do usuário ou pode colocá-las em outro lugar?

Você pode considerar expor ações adicionais na lista principal da interface do usuário quando essas ações precisam ser acessadas por qualquer entrada sempre.

Se você decidir que não é necessário colocar as ações secundárias na lista principal da interface do usuário, há várias outras maneiras de apresentá-las ao usuário. Aqui estão algumas opções de lugar em que você pode considerar colocar as ações secundárias.

### <a name="put-secondary-actions-on-the-detail-page"></a>Colocar as ações secundárias na página de detalhes

Colocar as ações secundárias na página para onde o item de lista navega quando é pressionado. Quando você usa o padrão mestre/detalhes, a página de detalhes é geralmente um bom lugar para colocar ações secundárias.

Para saber mais, veja o [Padrão mestre/detalhes](master-details.md).

### <a name="put-secondary-actions-in-a-context-menu"></a>Colocar as ações secundárias em um menu de contexto

Coloque as ações secundárias em um menu de contexto que o usuário possa acessar ao clicar com o botão direito ou ao pressionar e segurar. Isso oferece a vantagem de permitir que o usuário execute uma ação, como excluir um email, sem precisar carregar a página de detalhes. Também é recomendável disponibilizar essas opções na página de detalhes, pois os menus de contexto devem ser aceleradores, em vez da interface do usuário principal.

Para expor as ações secundárias quando a entrada é de um gamepad ou controle remoto, recomendamos que você use um menu de contexto.

Para obter mais informações, consulte [Menus de contexto e submenus](menus.md).

### <a name="put-secondary-actions-in-hover-ui-to-optimize-for-pointer-input"></a>Colocar as ações secundárias na interface do usuário de foco para otimizar para entrada de ponteiro

Se você espera que seu aplicativo seja usado com frequência com entrada de ponteiro, como mouse e caneta, e deseja disponibilizar ações secundárias prontamente somente para essas entradas, é possível mostrar as ações secundárias apenas em foco. Esse acelerador fica visível somente quando uma entrada de ponteiro é usada, então, use as outras opções para dar suporte a outros tipos de entrada também.

![Interface do usuário aninhada mostrada em foco](images/nested-ui-hover.png)


Para obter mais informações, consulte [Interações por mouse](../input/mouse-interactions.md).

## <a name="ui-placement-for-primary-and-secondary-actions"></a>Posicionamento da interface do usuário para ações principais e secundárias

Se você decidir que as ações secundárias devem ser expostas na interface do usuário da lista principal, recomendamos as diretrizes a seguir.

Quando você criar um item de lista com ações principais e secundárias, coloque a ação principal à esquerda e as ações secundárias à direita. Em culturas de leitura da esquerda para a direita, os usuários associam as ações no lado esquerdo do item de lista como a ação principal.

Nestes exemplos, falaremos sobre a interface do usuário de lista em que o item flui mais horizontalmente (é mais larga do que sua altura). No entanto, você pode ter itens de lista que têm a forma mais quadrados ou a altura maior do que a largura. Normalmente, esses itens são usados em uma grade. Para esses itens, se a lista não rolar verticalmente, você poderá colocar as ações secundárias na parte inferior do item de lista, em vez do lado direito.

## <a name="consider-all-inputs"></a>Considerar todas as entradas

Ao decidir usar a interface do usuário aninhada, avalie também a experiência do usuário com todos os tipos de entrada. Conforme mencionado anteriormente, a interface do usuário aninhada funciona bem para alguns tipos de entrada. No entanto, ela nem sempre funciona bem para outros tipos. Em particular, entradas por teclado, controlador e remotas podem ter dificuldade para acessar elementos da interface do usuário aninhada. Certifique-se de seguir as orientações para garantir que sua UWP funcione com todos os tipos de entrada.

## <a name="nested-ui-handling"></a>Manipulação da interface do usuário aninhada

Quando você tiver mais de uma ação aninhada no item de lista, recomendamos estas orientações para manipular a navegação com teclado, gamepad, controle remoto ou outra entrada sem ponteiro.

### <a name="nested-ui-where-list-items-perform-an-action"></a>Interface do usuário aninhada onde os itens de lista executam uma ação

Se a interface do usuário de lista com elementos aninhados aceitar ações como invocar, seleção (única ou vários) ou operações de arrastar e soltar, recomendamos estas técnicas de seta para navegar pelos elementos da interface do usuário aninhada.

![Partes da interface do usuário aninhada](images/nested-ui-navigation.png)

**Gamepad**

Quando a entrada é de um gamepad, forneça esta experiência de usuário:

- De **A**, a tecla de direção para a direita coloca o foco em **B**.
- De **B**, a tecla de direção para a direita coloca o foco em **C**.
- De **C**, a tecla de direção para a direita não funciona ou, se houver um elemento de interface do usuário focalizável à direita da lista, coloca o foco nela.
- De **C**, a tecla de direção para a esquerda coloca o foco em **B**.
- De **B**, a tecla de direção para a esquerda coloca o foco em **A**.
- De **A**, a tecla de direção para a esquerda não funciona ou, se houver um elemento de interface do usuário focalizável à direita da lista, coloca o foco nela.
- De **A**, **B** ou **C**, a tecla de direção para baixo coloca o foco em **D**.
- Do elemento de interface do usuário à esquerda do item de lista, a tecla de direção para a direita coloca o foco em **A**.
- Do elemento de interface do usuário à direita do item de lista, a tecla de direção para a esquerda coloca o foco em **A**.

**Teclado**

Quando a entrada é de um teclado, esta é a experiência que o usuário tem:

- De **A**, a tecla tab coloca o foco em **B**.
- De **B**, a tecla tab coloca o foco em **C**.
- De **C**, a tecla tab coloca o foco no próximo elemento de interface do usuário focalizável na ordem de tabulação.
- De **C**, as teclas shift + tab colocam o foco em **B**.
- De **B**, as teclas shift + tab ou a tecla de seta para a esquerda colocam o foco em **A**.
- De **A**, as teclas shift + tab colocam o foco no próximo elemento de interface do usuário focalizável na ordem de tabulação inversa.
- De **A**, **B** ou **C**, a tecla de seta para baixo coloca o foco em **D**.
- Do elemento de interface do usuário à esquerda do item de lista, a tecla tab coloca o foco em **A**.
- Do elemento de interface do usuário à direita do item de lista, as teclas shift e tab colocam o foco em **C**.

Para chegar a essa interface do usuário, defina [IsItemClickEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isitemclickenabled.aspx) como **true** em sua lista. [SelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx) pode ter qualquer valor.

Para saber o código para implementar isso, consulte a seção [Exemplo](#example) deste artigo.

### <a name="nested-ui-where-list-items-do-not-perform-an-action"></a>Interface do usuário aninhada onde os itens de lista não executam uma ação

Você pode usar uma exibição de lista porque ela fornece o comportamento de virtualização e rolagem otimizado, mas não têm uma ação associada a um item de lista. Essas interfaces do usuário geralmente usam o item de lista somente para agrupar elementos e garantir a rolagem como um conjunto.

Esse tipo de interface do usuário tende a ser muito mais complicado do que os exemplos anteriores, com muitos elementos aninhados nos quais o usuário pode executar uma ação.

![Partes da interface do usuário aninhada](images/nested-ui-grouping.png)


Para chegar a essa interface do usuário, defina as propriedades a seguir em sua lista:
- [SelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx) como **None**.
- [IsItemClickEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isitemclickenabled.aspx) como **false**.
- [IsFocusEngagementEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isfocusengagementenabled.aspx) como **true**.

```xaml
<ListView SelectionMode="None" IsItemClickEnabled="False" >
    <ListView.ItemContainerStyle>
         <Style TargetType="ListViewItem">
             <Setter Property="IsFocusEngagementEnabled" Value="True"/>
         </Style>
    </ListView.ItemContainerStyle>
</ListView>
```

Quando os itens de lista não executam uma ação, recomendamos estas orientações para manipular a navegação com um gamepad ou teclado.

**Gamepad**

Quando a entrada é de um gamepad, forneça esta experiência de usuário:

- Do item de lista, a tecla de direção para baixo coloca o foco no próximo item de lista.
- Do item de lista, a tecla para a esquerda/direita não funciona ou, se houver um elemento de interface do usuário focalizável à direita da lista, coloca o foco nela.
- Do item de lista, o botão 'A' coloca o foco na interface do usuário aninhada na ordem de prioridade de cima para baixo, da esquerda para a direita.
- Enquanto estiver dentro da interface de usuário aninhada, siga o modelo de navegação de foco XY.  O foco só pode navegar pela interface do usuário aninhada contida no item de lista atual até que o usuário pressione o botão 'B', que volta o foco para o item de lista.

**Teclado**

Quando a entrada é de um teclado, esta é a experiência que o usuário tem:

- Do item de lista, a tecla de seta para baixo coloca o foco no próximo item de lista.
- Do item de lista, pressionar a tecla para a esquerda/direita não executa qualquer operação.
- Do item da lista, pressionar a tecla tab coloca o foco na próxima parada de tabulação entre o item da interface do usuário aninhada.
- De um dos itens da interface do usuário aninhada, pressionar a tecla tab percorre os itens da interface do usuário aninhada na ordem de tabulação.  Depois que todos os itens da interface do usuário aninhada são percorridos, coloca o foco no próximo controle na ordem de tabulação após ListView.
- A combinação Shift + Tab tem o comportamento na direção inversa do comportamento da tecla Tab.

## <a name="example"></a>Exemplo

Este exemplo mostra como implementar uma [interface do usuário aninhada onde os itens de lista executam uma ação](#nested-ui-where-list-items-perform-an-action).

```xaml
<ListView SelectionMode="None" IsItemClickEnabled="True"
          ChoosingItemContainer="listview1_ChoosingItemContainer"/>
```

```csharp
private void OnListViewItemKeyDown(object sender, KeyRoutedEventArgs e)
{
    // Code to handle going in/out of nested UI with gamepad and remote only.
    if (e.Handled == true)
    {
        return;
    }

    var focusedElementAsListViewItem = FocusManager.GetFocusedElement() as ListViewItem;
    if (focusedElementAsListViewItem != null)
    {
        // Focus is on the ListViewItem.
        // Go in with Right arrow.
        Control candidate = null;

        switch (e.OriginalKey)
        {
            case Windows.System.VirtualKey.GamepadDPadRight:
            case Windows.System.VirtualKey.GamepadLeftThumbstickRight:
                var rawPixelsPerViewPixel = DisplayInformation.GetForCurrentView().RawPixelsPerViewPixel;
                GeneralTransform generalTransform = focusedElementAsListViewItem.TransformToVisual(null);
                Point startPoint = generalTransform.TransformPoint(new Point(0, 0));
                Rect hintRect = new Rect(startPoint.X * rawPixelsPerViewPixel, startPoint.Y * rawPixelsPerViewPixel, 1, focusedElementAsListViewItem.ActualHeight * rawPixelsPerViewPixel);
                candidate = FocusManager.FindNextFocusableElement(FocusNavigationDirection.Right, hintRect) as Control;
                break;
        }

        if (candidate != null)
        {
            candidate.Focus(FocusState.Keyboard);
            e.Handled = true;
        }
    }
    else
    {
        // Focus is inside the ListViewItem.
        FocusNavigationDirection direction = FocusNavigationDirection.None;
        switch (e.OriginalKey)
        {
            case Windows.System.VirtualKey.GamepadDPadUp:
            case Windows.System.VirtualKey.GamepadLeftThumbstickUp:
                direction = FocusNavigationDirection.Up;
                break;
            case Windows.System.VirtualKey.GamepadDPadDown:
            case Windows.System.VirtualKey.GamepadLeftThumbstickDown:
                direction = FocusNavigationDirection.Down;
                break;
            case Windows.System.VirtualKey.GamepadDPadLeft:
            case Windows.System.VirtualKey.GamepadLeftThumbstickLeft:
                direction = FocusNavigationDirection.Left;
                break;
            case Windows.System.VirtualKey.GamepadDPadRight:
            case Windows.System.VirtualKey.GamepadLeftThumbstickRight:
                direction = FocusNavigationDirection.Right;
                break;
            default:
                break;
        }

        if (direction != FocusNavigationDirection.None)
        {
            Control candidate = FocusManager.FindNextFocusableElement(direction) as Control;
            if (candidate != null)
            {
                ListViewItem listViewItem = sender as ListViewItem;

                // If the next focusable candidate to the left is outside of ListViewItem,
                // put the focus on ListViewItem.
                if (direction == FocusNavigationDirection.Left &&
                    !listViewItem.IsAncestorOf(candidate))
                {
                    listViewItem.Focus(FocusState.Keyboard);
                }
                else
                {
                    candidate.Focus(FocusState.Keyboard);
                }
            }

            e.Handled = true;
        }
    }
}

private void listview1_ChoosingItemContainer(ListViewBase sender, ChoosingItemContainerEventArgs args)
{
    if (args.ItemContainer == null)
    {
        args.ItemContainer = new ListViewItem();
        args.ItemContainer.KeyDown += OnListViewItemKeyDown;
    }
}
```

```csharp
// DependencyObjectExtensions.cs definition.
public static class DependencyObjectExtensions
{
    public static bool IsAncestorOf(this DependencyObject parent, DependencyObject child)
    {
        DependencyObject current = child;
        bool isAncestor = false;

        while (current != null && !isAncestor)
        {
            if (current == parent)
            {
                isAncestor = true;
            }

            current = VisualTreeHelper.GetParent(current);
        }

        return isAncestor;
    }
}
```
