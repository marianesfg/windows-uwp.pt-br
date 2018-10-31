---
author: Karl-Bridge-Microsoft
Description: Learn how accelerator keys can improve the usability and accessibility of UWP apps.
title: Aceleradores de teclado
label: Keyboard accelerators
template: detail.hbs
keywords: teclado, acelerador, tecla aceleradora, atalhos do teclado, acessibilidade, navegação, foco, texto, entrada, interações do usuário, gamepad, remoto
ms.author: kbridge
ms.date: 10/10/2017
ms.topic: article
pm-contact: chigy
design-contact: miguelrb
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: dcbb27a87b48a124fe4463578bc32d908f399ccb
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5861729"
---
# <a name="keyboard-accelerators"></a>Aceleradores de teclado

![Teclado do Surface](images/accelerators/accelerators_hero2.png)

Teclas aceleradoras (ou aceleradores de teclado) são atalhos que melhoram a usabilidade e a acessibilidade dos seus aplicativos do Windows, oferecendo aos usuários uma forma intuitiva de invocar ações ou comandos comuns sem navegar na interface de usuário do aplicativo.

Consulte o tópico [Acessar chaves](access-keys.md) para obter detalhes sobre como navegar na interface do usuário de um aplicativo do Windows com atalhos de teclado.

> [!NOTE]
> Um teclado é indispensável para usuários com determinadas deficiências (veja [Acessibilidade do teclado](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility)), além de ser uma ferramenta importante para usuários que o preferem como maneira mais eficiente de interagir com um aplicativo.

## <a name="overview"></a>Visão geral

Aceleradores normalmente incluem as teclas de função F1 a F12 ou alguma combinação de uma chave padrão emparelhada com um ou mais teclas modificadoras (CTRL, Shift).

> [!NOTE]
> Os controles de plataforma UWP têm aceleradores de teclado interno. Por exemplo, o ListView suporta Ctrl + A para selecionar todos os itens na lista e RichEditBox oferece suporte à Ctrl + Tab para inserir uma tabulação na caixa de texto. Esses aceleradores de teclado integrados são chamados de **Aceleradores de controle** e são executados somente se o foco estiver no elemento ou em um de seus filhos. Aceleradores definidos por você usando os APIs de acelerador de teclado discutidas aqui são chamados de **Aceleradores de aplicativo**.

Aceleradores de teclado não estão disponíveis para cada ação, mas são frequentemente associados com comandos expostos nos menus (e devem ser especificados com o conteúdo do item de menu).Aceleradores também podem ser associados com as ações que não têm itens de menu equivalentes. No entanto, como os usuários dependem dos menus de um aplicativo para descobrir e conhecer o conjunto de comandos disponíveis, você deve tentar facilitar ao máximo a descoberta dos aceleradores (usar rótulos ou padrões estabelecidos pode ajudar nisso).

![Aceleradores de teclado descritos em um rótulo de item de menu](images/accelerators/accelerators_menuitemlabel.png)  
*Aceleradores de teclado descritos em um rótulo de item de menu*

## <a name="when-to-use-keyboard-accelerators"></a>Quando usar aceleradores de teclado

Recomendamos que você especifique aceleradores de teclado sempre que for apropriado em sua interface de usuário, e ofereça suporte à eles em todos os controles personalizados.

- Aceleradores de teclado tornam seu aplicativo mais accessiblefor usuários com deficiências motoras, incluindo os usuários que conseguem pressionar apenas uma tecla por vez ou que possuem dificuldades para usar um mouse *

  Uma interface de usuário do teclado bem projetada é um aspecto importante da acessibilidade do software. Ela permite que os usuários com deficiência visual ou que possuam determinadas deficiências motoras naveguem em um aplicativo e interajam com seus recursos. Esses usuários talvez não consigam usar um mouse e, em vez disso, usem diversas tecnologias assistenciais, como ferramentas avançadas de teclado, teclados virtuais, ampliadores de tela, leitores de tela e utilitários de entrada de voz. Para esses usuários, a cobertura de comando abrangente é essencial.

- Aceleradores de teclado tornam seu aplicativo usablefor mais os usuários avançados que preferem interagir através do teclado.

  Os usuários experientes apresentam, muitas vezes, uma forte preferência pelo teclado, pois os comandos por teclado podem ser inseridos mais rapidamente e não exigem que eles retirem as mãos do teclado. Para esses usuários, eficiência e consistência são cruciais; a abrangência é importante apenas para os comandos mais usados.

## <a name="specify-a-keyboard-accelerator"></a>Especificar um acelerador de teclado

Use as APIs [KeyboardAccelerator](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.keyboardaccelerator.-ctor) para criar aceleradores de teclado em aplicativos UWP. Com essas APIs, você não precisa manipular vários eventos KeyDown para detectar a combinação de teclas pressionadas e você pode localizar aceleradores nos recursos de aplicativo.

Recomendamos que você defina aceleradores de teclado para as ações mais comuns em seu aplicativo e documento usando o rótulo de item de menu ou a dica de ferramenta. Neste exemplo, nós declaramos aceleradores de teclado apenas para os comandos Renomear e Copiar.

``` xaml
<CommandBar Margin="0,200" AccessKey="M">
  <AppBarButton 
    Icon="Share" 
    Label="Share" 
    Click="OnShare" 
    AccessKey="S" />
  <AppBarButton 
    Icon="Copy" 
    Label="Copy" 
    ToolTipService.ToolTip="Copy (Ctrl+C)" 
    Click="OnCopy" 
    AccessKey="C">
    <AppBarButton.KeyboardAccelerators>
      <KeyboardAccelerator 
        Modifiers="Control" 
        Key="C" />
    </AppBarButton.KeyboardAccelerators>
  </AppBarButton>

  <AppBarButton 
    Icon="Delete" 
    Label="Delete" 
    Click="OnDelete" 
    AccessKey="D" />
  <AppBarSeparator/>
  <AppBarButton 
    Icon="Rename" 
    Label="Rename" 
    ToolTipService.ToolTip="Rename (F2)" 
    Click="OnRename" 
    AccessKey="R">
    <AppBarButton.KeyboardAccelerators>
      <KeyboardAccelerator 
        Modifiers="None" Key="F2" />
    </AppBarButton.KeyboardAccelerators>
  </AppBarButton>

  <AppBarButton 
    Icon="SelectAll" 
    Label="Select" 
    Click="OnSelect" 
    AccessKey="A" />
  
  <CommandBar.SecondaryCommands>
    <AppBarButton 
      Icon="OpenWith" 
      Label="Sources" 
      AccessKey="S">
      <AppBarButton.Flyout>
        <MenuFlyout>
          <ToggleMenuFlyoutItem Text="OneDrive" />
          <ToggleMenuFlyoutItem Text="Contacts" />
          <ToggleMenuFlyoutItem Text="Photos"/>
          <ToggleMenuFlyoutItem Text="Videos"/>
        </MenuFlyout>
      </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarToggleButton 
      Icon="Save" 
      Label="Auto Save" 
      IsChecked="True" 
      AccessKey="A"/>
  </CommandBar.SecondaryCommands>

</CommandBar>
```

![Acelerador de teclado descrita em uma dica de ferramenta](images/accelerators/accelerators_tooltip.png)  
***Acelerador de teclado descrita em uma dica de ferramenta***

O objeto [UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) tem uma coleção [KeyboardAccelerator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator), [KeyboardAccelerators](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.KeyboardAccelerators), onde você especifica seus objetos KeyboardAccelerator personalizados e define as teclas para acelerador de teclado:

-   **[Key](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Key)** - a [VirtualKey](https://docs.microsoft.com/uwp/api/windows.system.virtualkey) usada para o acelerador de teclado.

-   **[Modifiers](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Modifiers)** - a [VirtualKeyModifiers](https://docs.microsoft.com/uwp/api/windows.system.virtualkeymodifiers) usada para o acelerador de teclado. Se Modificadores não estiver definido, o valor padrão é Nenhum.

> [!NOTE]
> Aceleradores de chave única (A, Excluir, F2, barra de espaços, Esc, chave de multimídia) e aceleradores de múltiplas chaves (Ctrl + Shift + M) são permitidos. No entanto, as teclas virtuais de Gamepad não são permitidas.

## <a name="scoped-accelerators"></a>Aceleradores analisados

Alguns aceleradores funcionam apenas em escopos específicos, enquanto outros funcionam em todo o aplicativo.

Por exemplo, o Microsoft Outlook inclui os aceleradores a seguir:
-   CTRL + B, Ctrl + I e ESC funcionam apenas no escopo do formulário de enviar email
-   CTRL + 1 e Ctrl + 2 funcionam em todo o aplicativo

### <a name="context-menus"></a>Menus de contexto

Ações de menu de contexto afetam apenas áreas específicas ou elementos, como os caracteres selecionados em um editor de texto ou uma música em uma playlist. Por esse motivo, recomendamos definir o escopo dos aceleradores de teclado para itens de menu de contexto para o pai do menu de contexto.

Use a propriedade [ScopeOwner](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.ScopeOwner) para especificar o escopo do acelerador de teclado. Esse código demonstra como implementar um menu de contexto em um ListView com aceleradores de teclado analisados:

``` xaml
<ListView x:Name="MyList">
  <ListView.ContextFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Share" Icon="Share"/>
      <MenuFlyoutItem Text="Copy" Icon="Copy">
        <MenuFlyoutItem.KeyboardAccelerators>
          <KeyboardAccelerator 
            Modifiers="Control" 
            Key="C" 
            ScopeOwner="{x:Bind MyList }" />
        </MenuFlyoutItem.KeyboardAccelerators>
      </MenuFlyoutItem>
      
      <MenuFlyoutItem Text="Delete" Icon="Delete" />
      <MenuFlyoutSeparator />
      
      <MenuFlyoutItem Text="Rename">
        <MenuFlyoutItem.KeyboardAccelerators>
          <KeyboardAccelerator 
            Modifiers="None" 
            Key="F2" 
            ScopeOwner="{x:Bind MyList}" />
        </MenuFlyoutItem.KeyboardAccelerators>
      </MenuFlyoutItem>
      
      <MenuFlyoutItem Text="Select" />
    </MenuFlyout>
    
  </ListView.ContextFlyout>
    
  <ListViewItem>Track 1</ListViewItem>
  <ListViewItem>Alternative Track 1</ListViewItem>

</ListView>
```

O atributo ScopeOwner do elemento MenuFlyoutItem.KeyboardAccelerators marca o acelerador como no escopo em vez de global (o padrão é null ou global). Para obter mais detalhes, consulte a seção **Resolvendo aceleradores** posteriormente neste tópico.

## <a name="invoke-a-keyboard-accelerator"></a>Invocar um acelerador de teclado 

O objeto [KeyboardAccelerator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator) usa o [padrão de controle de automação de interface do usuário (UIA)](https://msdn.microsoft.com/library/windows/desktop/ee671194(v=vs.85).aspx) para executar uma ação quando um acelerador é invocado.

A UIA [padrões de controle] expõem a funcionalidade de controle comum. Por exemplo, o controle de botão implementa o [Invoke](https://msdn.microsoft.com/library/windows/desktop/ee671279(v=vs.85).aspx) padrão de controle para dar suporte ao evento de clique (normalmente um controle é invocado clicando em, clicando duas vezes em ou pressionando Enter, um atalho de teclado predefinido ou alguma combinação alternativa dos pressionamentos de teclas). Quando um acelerador de teclado é usado para invocar um controle, a estrutura XAML pesquisa se o controle implementa o padrão de controle Invoke e, em caso afirmativo, o ativa (não é necessário escutar o evento KeyboardAcceleratorInvoked).

No exemplo a seguir, o controle +S aciona o evento Click, porque o botão implementa o padrão Invoke.

``` xaml 
<Button Content="Save" Click="OnSave">
  <Button.KeyboardAccelerators>
    <KeyboardAccelerator Key="S" Modifiers="Control" />
  </Button.KeyboardAccelerators>
</Button>
```

Se um elemento implementa vários padrões de controle, apenas um pode ser ativado por meio de um acelerador. Os padrões de controle são priorizados da seguinte maneira:
1.  Invocar (Botão)
2.  Alternar (Caixa de seleção)
3.  Seleção (ListView)
4.  Expandir/Recolher (ComboBox) 

Se nenhuma correspondência for identificada, o acelerador é inválido e uma mensagem de depuração é fornecida ("nenhum padrão de automação para esse componente foi encontrado. Implementar todos os comportamento desejados evento Invocado. Definir a propriedade Handled como true em seu manipulador de eventos suprime essa mensagem.")

## <a name="custom-keyboard-accelerator-behavior"></a>Comportamento de acelerador de teclado personalizado

O evento Invoked do objeto [KeyboardAccelerator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator) é disparado quando o acelerador é executado. O evento do objeto [KeyboardAcceleratorInvokedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs) inclui as seguintes propriedades:
- **Manipulado** (booleano): Muda essa configuração para true e impede que o evento dispare o padrão de controle e interrompa a propagação de eventos de acelerador. O padrão é False.
- **Elemento** (DependencyObject): O objeto que contém o acelerador.

Aqui, demonstramos como definir uma coleção de aceleradores de teclado e como manipular o evento Invoked.

``` xaml
<ListView x:Name="MyListView">
  <ListView.KeyboardAccelerators>
    <KeyboardAccelerator Key="A" Modifiers="Control,Shift" Invoked="SelectAllInvoked" />
    <KeyboardAccelerator Key="F5" Invoked="RefreshInvoked"  />
  </ListView.KeyboardAccelerators>
</ListView>   
```

``` csharp
void SelectAllInvoked (KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
  CustomSelectAll(MyListView);
  args.Handled = true;
}

void RefreshInvoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
  Refresh(MyListView);
  args.Handled = true;
}
```

## <a name="override-default-keyboard-behavior"></a>Substituir o comportamento padrão do teclado

Em alguns casos, talvez seja necessário substituir o comportamento padrão de teclas específicas, como a tecla Backspace ou a tecla Enter. Por exemplo, 

## <a name="disable-a-keyboard-accelerator"></a>Desativar um acelerador de teclado 

Se um controle estiver desabilitado, o acelerador associado também está desabilitado. No exemplo a seguir, como a propriedade IsEnabled do ListView é definida como false, acelerador Control +A associado não pode ser invocado.

``` xaml
<ListView >
  <ListView.KeyboardAccelerators>
    <KeyboardAccelerator Key="A" 
      Modifiers="Control"
      Invoked="CustomListViewSelecAllInvoked" />
  </ListView.KeyboardAccelerators>
  
  <TextBox>
    <TextBox.KeyboardAccelerators>
      <KeyboardAccelerator 
        Key="A" 
        Modifiers="Control" 
        Invoked="CustomTextSelecAllInvoked" 
        IsEnabled="False" />
    </TextBox.KeyboardAccelerators>
  </TextBox>

<ListView>
```

Controles de pai e filho podem compartilhar o mesmo acelerador. Nesse caso, o controle principal pode ser invocado mesmo que o secundário tenha foco e seu acelerador esteja desabilitado.

## <a name="screen-readers-and-keyboard-accelerators"></a>Leitores de tela e aceleradores de teclado 

Leitores de tela, como o Narrador podem anunciar a combinação de teclas de acelerador de teclado para os usuários. Por padrão, esse é cada modificador (na ordem de enumeração VirtualModifiers) seguido pela chave (e separados por sinais de "+"). Você pode personalizá-lo por meio da propriedade [AcceleratorKey](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.AcceleratorKeyProperty) AutomationProperties anexada. Se mais de um acelerador for especificado, somente o primeiro é anunciado.

Neste exemplo, o AutomationProperty.AcceleratorKey retorna a cadeia de caracteres "Control + Shift + A":

``` xaml
<ListView x:Name="MyListView">
  <ListView.KeyboardAccelerators>

    <KeyboardAccelerator 
      Key="A" 
      Modifiers="Control,Shift" 
      Invoked="CustomSelectAllInvoked" />
      
    <KeyboardAccelerator 
      Key="F5" 
      Modifiers="None" 
      Invoked="RefreshInvoked" />

  </ListView.KeyboardAccelerators>

</ListView>   
```

> [!NOTE] 
> A configuração AutomationProperties.AcceleratorKey não habilita a funcionalidade de teclado, apenas indica à estrutura UIA quais teclas são usadas.

## <a name="common-keyboard-accelerators"></a>Aceleradores de teclado comuns

Recomendamos que você deixe os aceleradores de teclado consistentes em todos os aplicativos UWP. Os usuários têm de memorizar os aceleradores de teclado e esperarem os mesmos resultados (ou semelhantes).

Talvez isso não seja sempre possível devido às diferenças na funcionalidade entre aplicativos.

| **Edição** | **Acelerador de teclado comum** |
| ------------- | ----------------------------------- |
| Iniciar o modo de edição | Ctrl + E |
| Selecionar todos os itens em um controle focado ou em uma janela | Ctrl + A |
| Pesquisar e substituir | Ctrl + H |
| Desfazer | Ctrl + Z |
| Refazer | Ctrl + Y |
| Exclui a seleção e a copia para a área de transferência | Ctrl + X |
| Copiar uma seleção para a área de transferência | Ctrl + C, Ctrl + Inserir |
| Cola o conteúdo da área de transferência | Ctrl + V, Shift + Inserir |
| Cola o conteúdo da área de transferência (com opções) | Ctrl + Alt + V |
| Renomeia um item | F2 |
| Adicionar um novo item | Ctrl + N |
| Adicionar um novo item secundário | Ctrl + Shift + N |
| Excluir o item selecionado (com desfazer) | Del, Ctrl+D |
| Excluir o item selecionado (sem desfazer) | Shift + Del |
| Negrito | Ctrl + B |
| Sublinhado | Ctrl + U |
| Itálico | Ctrl + I |

| **Navegação** | |
| ------------- | ----------------------------------- |
| Encontre conteúdo em um controle com foco ou janela | Ctrl + F |
| Vá para o próximo resultado da busca | F3 |

| **Outras ações** | |
| ------------- | ----------------------------------- |
| Adicionar favoritos | Ctrl + D | 
| Atualizar | F5 ou Ctrl+R | 
| Ampliar | Ctrl + + | 
| Reduzir | Ctrl + - | 
| Zoom para o modo de exibição padrão | Ctrl + 0 | 
| Salvar | Ctrl + S | 
| Fechar | Ctrl + W | 
| Imprimir | Ctrl + P | 

Observe que algumas dessas combinações não são válidas para versões localizadas do Windows. Por exemplo, na versão em espanhol do Windows, Ctrl + N é usado para negrito em vez de Ctrl + B. É recomendável fornecer aceleradores de teclado localizados, se o aplicativo for localizado.

## <a name="usability-affordances-for-keyboard-accelerators"></a>Funcionalidades de usabilidade para aceleradores de teclado

### <a name="tooltips"></a>Dicas de ferramenta

Como aceleradores de teclado normalmente não são descritos diretamente na interface do usuário do seu aplicativo UWP, você pode melhorar a detectabilidade por meio das [dicas de ferramenta](../controls-and-patterns/tooltips.md), que são exibidas automaticamente quando o usuário move o foco, pressiona e segura um item ou passa o ponteiro do mouse sobre um controle. A dica de ferramenta pode identificar se um controle tem um acelerador de teclado associado e, em caso afirmativo, qual é a combinação de teclas aceleradoras.

**Windows 10, versão 1803 (atualização de abril de 2018) e mais recente**

Por padrão, quando são declarados aceleradores de teclado, todos os controles (exceto [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem) e [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)) apresentam as combinações de teclas correspondentes em uma dica de ferramenta.

> [!NOTE] 
> Se um controle tiver mais de um acelerador definido, apenas o primeiro será apresentado.

![Dica de ferramenta de tecla aceleradora](images/accelerators/accelerators_tooltip_savebutton_small.png)

*Combinação de teclas aceleradoras em dica de ferramenta*

Para o [botão](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button), [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton)e [AppBarToggleButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbartogglebutton) objetos, o Acelerador de teclado é anexado a dica de ferramenta do controle padrão. Para [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) e [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)) objetos, o Acelerador de teclado é exibido com o texto do submenu.

> [!NOTE]
> Especificando uma dica de ferramenta (consulte Button1 no exemplo a seguir) substitui esse comportamento.

```xaml
<StackPanel x:Name="Container" Grid.Row="0" Background="AliceBlue">
    <Button Content="Button1" Margin="20"
            Click="OnSave" 
            KeyboardAcceleratorPlacementMode="Auto" 
            ToolTipService.ToolTip="Tooltip">
        <Button.KeyboardAccelerators>
            <KeyboardAccelerator  Key="A" Modifiers="Windows"/>
        </Button.KeyboardAccelerators>
    </Button>
    <Button Content="Button2"  Margin="20"
            Click="OnSave" 
            KeyboardAcceleratorPlacementMode="Auto">
        <Button.KeyboardAccelerators>
            <KeyboardAccelerator  Key="B" Modifiers="Windows"/>
        </Button.KeyboardAccelerators>
    </Button>
    <Button Content="Button3"  Margin="20"
            Click="OnSave" 
            KeyboardAcceleratorPlacementMode="Auto">
        <Button.KeyboardAccelerators>
            <KeyboardAccelerator  Key="C" Modifiers="Windows"/>
        </Button.KeyboardAccelerators>
    </Button>
</StackPanel>
```

![Dica de ferramenta de tecla aceleradora](images/accelerators/accelerators-button-small.png)

*Combinação de teclas aceleradoras acrescentada a dica de ferramenta do botão padrão*

```xaml
<AppBarButton Icon="Save" Label="Save">
    <AppBarButton.KeyboardAccelerators>
        <KeyboardAccelerator Key="S" Modifiers="Control"/>
    </AppBarButton.KeyboardAccelerators>
</AppBarButton>
```

![Dica de ferramenta de tecla aceleradora](images/accelerators/accelerators-appbarbutton-small.png)

*Combinação de teclas aceleradoras acrescentada a dica de ferramenta do AppBarButton padrão*

```xaml
<AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
    <AppBarButton.Flyout>
        <MenuFlyout>
            <MenuFlyoutItem AccessKey="A" Icon="Refresh" Text="Refresh A">
                <MenuFlyoutItem.KeyboardAccelerators>
                    <KeyboardAccelerator Key="R" Modifiers="Control"/>
                </MenuFlyoutItem.KeyboardAccelerators>
            </MenuFlyoutItem>
            <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
            <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
            <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            <ToggleMenuFlyoutItem AccessKey="E" Icon="Globe" Text="ToggleMe">
                <MenuFlyoutItem.KeyboardAccelerators>
                    <KeyboardAccelerator Key="Q" Modifiers="Control"/>
                </MenuFlyoutItem.KeyboardAccelerators>
            </ToggleMenuFlyoutItem>
        </MenuFlyout>
    </AppBarButton.Flyout>
</AppBarButton>
```

![Dica de ferramenta de tecla aceleradora](images/accelerators/accelerators-appbar-menuflyoutitem-small.png)

*Combinação de teclas aceleradoras acrescentada ao texto do MenuFlyoutItem*

Controle o comportamento de apresentação usando a propriedade [KeyboardAcceleratorPlacementMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.KeyboardAcceleratorPlacementMode), que aceita dois valores: [Automático](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardacceleratorplacementmode) ou [Oculto](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardacceleratorplacementmode).    

```xaml
<Button Content="Save" Click="OnSave" KeyboardAcceleratorPlacementMode="Auto">
    <Button.KeyboardAccelerators>
        <KeyboardAccelerator Key="S" Modifiers="Control" />
    </Button.KeyboardAccelerators>
</Button>
```

Em alguns casos, talvez você precise apresentar uma dica de ferramenta referente a outro elemento (geralmente um objeto de contêiner). Por exemplo, um controle de pivô que exibe a dica de ferramenta para um PivotItem com o cabeçalho do pivô. 

Aqui, mostramos como usar a propriedade KeyboardAcceleratorPlacementTarget para exibir a combinação de teclas do acelerador de teclado para um botão Salvar com o contêiner de grade em vez do botão.

```xaml
<Grid x:Name="Container" Padding="30">
  <Button Content="Save"
    Click="OnSave"
    KeyboardAcceleratorPlacementMode="Auto"
    KeyboardAcceleratorPlacementTarget="{x:Bind Container}">
    <Button.KeyboardAccelerators>
      <KeyboardAccelerator  Key="S" Modifiers="Control" />
    </Button.KeyboardAccelerators>
  </Button>
</Grid>
```

### <a name="labels"></a>Rótulos

Em alguns casos, recomendamos o uso de um rótulo do controle para identificar se o controle tem um acelerador de teclado associado e, em caso afirmativo, qual é a combinação de teclas aceleradoras. 

Alguns controles de plataforma fazem isso por padrão, especificamente os objetos [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem) e [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem), enquanto o [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) e o [AppBarToggleButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbartogglebutton) fazem isso quando aparecem no menu de estouro do [CommandBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar).

![Aceleradores de teclado descritos em um rótulo de item de menu](images/accelerators/accelerators_menuitemlabel.png)  
*Aceleradores de teclado descritos em um rótulo de item de menu*

Você pode substituir o texto de acelerador padrão para o rótulo por meio da propriedade [KeyboardAcceleratorTextOverride](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton.KeyboardAcceleratorTextOverride) dos controles [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem), [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem), [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) e [AppBarToggleButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbartogglebutton) (use um único espaço para nenhum texto). 

> [!NOTE] 
> O texto de substituição não será apresentado se o sistema não conseguir detectar um teclado conectado (você pode verificar isso por conta própria por meio da propriedade [KeyboardPresent](https://docs.microsoft.com/uwp/api/windows.devices.input.keyboardcapabilities.KeyboardPresent)).

## <a name="advanced-concepts"></a>Conceitos avançados

Aqui, revisamos alguns aspectos de baixo nível de aceleradores de teclado.

### <a name="when-an-accelerator-is-invoked"></a>Quando um acelerador é invocado

Aceleradores são compostos de dois tipos de chaves: modificadores e não modificadores. Teclas modificadoras incluem Shift, Menu, Control e a chave do Windows, que são expostas por meio de [VirtualKeyModifiers](http://msdn.microsoft.com/library/windows/apps/xaml/Windows.System.VirtualKeyModifiers). Não modificadores são quaisquer tecla virtual, como excluir, F3, barra de espaços, Esc e todos alfanuméricos e chaves de pontuação. Um acelerador de teclado é invocado quando o usuário pressionar uma tecla não modificadora enquanto pressionar e segurar uma ou mais teclas modificadoras. Por exemplo, se o usuário pressionar Ctrl + Shift + M, quando o usuário pressiona M, a estrutura verifica os modificadores (Ctrl e Shift) e dispara o acelerador, se ele existir.

> [!NOTE]
> Por design, o acelerador repete automaticamente (por exemplo, quando o usuário pressiona Ctrl + Shift e, em seguida, mantém pressionado M, o acelerador é invocado repetidamente até M ser liberado). Esse comportamento não pode ser modificado.

### <a name="input-event-priority"></a>Prioridade de eventos de entrada
Eventos de entrada ocorrerem em uma sequência específica que você pode interceptar e manipular com base nos requisitos do seu aplicativo. 

#### <a name="the-keydownkeyup-bubbling-event"></a>O evento de propagação KeyDown/KeyUp 

Em XAML, um pressionamento de teclas é processado como se houvesse entrado apenas um pipeline de propagação. Esse pipeline de entrada é usado pelos eventos KeyDown/KeyUp e caracteres de entrada. Por exemplo, se um elemento tem o foco e o usuário pressionar uma tecla, um evento KeyDown é acionado no elemento, seguido pelo elemento pai do elemento, e assim por diante para cima na árvore, até a propriedade args.Handled ser true.

O evento KeyDown também é usado por alguns controles para implementar os aceleradores de controle interno. Quando um controle tem um acelerador de teclado, ele manipula o evento KeyDown, que significa que não haverá propagação de eventos KeyDown. Por exemplo, a RichEditBox oferece suporte à cópia com Ctrl + C. Quando Ctrl for pressionado, o evento KeyDown é disparado e se propaga, mas quando o usuário pressiona C ao mesmo tempo, o evento KeyDown está marcado como Handled e não é gerado (a menos que o parâmetro handledEventsToo de [UIElement.AddHandler](http://msdn.microsoft.com/library/windows/apps/xaml/Windows.UI.Xaml.UIElement.AddHandler) seja definido como true).

#### <a name="the-characterreceived-event"></a>O evento CharacterReceived

Como o evento [CharacterReceived](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.CharacterReceived) é acionado após o evento [KeyDown](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.KeyDown) para controles de texto, como TextBox, você pode cancelar o caractere de entrada no manipulador de eventos KeyDown.

#### <a name="the-previewkeydown-and-previewkeyup-events"></a>Os eventos PreviewKeyDown e PreviewKeyUp

Os eventos de entrada de visualização são disparados antes de quaisquer outros eventos. Se você não manipular esses eventos, o acelerador para o elemento que tem o foco é acionado, seguido pelo evento KeyDown. Ambos os eventos são propagados até serem manipulados.


![Sequência de evento de tecla](images/accelerators/accelerators_keyevents.png)
***Sequência de evento de tecla***

Ordem dos eventos:

Visualizar eventos KeyDown...
Método OnKeyDown de acelerador de aplicativo, aceleradores de aplicativo do evento KeyDown no método OnKeyDown parente no evento KeyDown parente no parente (propagação na raiz)...
PreviewKeyUp events KeyUpEvents do evento CharacterReceived

Quando o evento de acelerador é manipulado, o evento KeyDown também é marcado como manipulado. O evento KeyUp permanece sem tratamento.

### <a name="resolving-accelerators"></a>Resolvendo aceleradores

Um evento de acelerador de teclado se propaga do elemento que tem o foco até a raiz. Se o evento não é manipulado, a estrutura XAML procura por outros aceleradores fora do escopo do aplicativo fora do caminho de propagação.

Quando dois aceleradores de teclado são definidos com a mesma combinação de teclas, o primeiro encontrado na árvore visual de acelerador de teclado é invocado.

Aceleradores de teclado analisados são invocados somente quando o foco está dentro de um escopo específico. Por exemplo, em uma grade que contém dezenas de controles, um acelerador de teclado pode ser invocado para um controle somente quando o foco está dentro da grade (o proprietário do escopo).

### <a name="scoping-accelerators-programmatically"></a>Escopo de aceleradores programaticamente

O método [UIElement.TryInvokeKeyboardAccelerator](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.uielement.tryinvokekeyboardaccelerator) invoca quaisquer aceleradores correspondentes na subárvore do elemento.

O método [UIElement.OnProcessKeyboardAccelerators](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.uielement.onprocesskeyboardaccelerators) é executado antes do acelerador de teclado. Esse método transmite um objeto [ProcessKeyboardAcceleratorArgs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.processkeyboardacceleratoreventargs) que contém a chave, o modificador e um booliano que indica se o acelerador de teclado é manipulado. Se estiver marcado como manipulado, o acelerador de teclado se propaga (para que o acelerador de teclado externo nunca seja invocado).

> [!NOTE]
> OnProcessKeyboardAccelerators sempre é acionado, se manipulado ou não (semelhante ao evento OnKeyDown). Você deve verificar se o evento foi marcado como manipulados.

Neste exemplo, usamos OnProcessKeyboardAccelerators e TryInvokeKeyboardAccelerator para aceleradores de teclado do escopo para o objeto de página:

``` csharp
protected override void OnProcessKeyboardAccelerators(
  ProcessKeyboardAcceleratorArgs args)
{
  if(args.Handled != true)
  {
    this.TryInvokeKeyboardAccelerator(args);
    args.Handled = true;
  }
}
```

### <a name="localize-the-accelerators"></a>Localizar os aceleradores

É recomendável fazer a localização de todos os aceleradores de teclado. Você pode fazer isso com o arquivo UWP padrão (.resw) e o atributo x:Uid nas declarações XAML. Neste exemplo, o Windows Runtime carrega automaticamente os recursos.

![Localização de acelerador de teclado com o arquivo de recursos UWP](images/accelerators/accelerators_localization.png)
***Localização acelerador de teclado com o arquivo de recursos UWP***

``` xaml
<Button x:Uid="myButton" Click="OnSave">
  <Button.KeyboardAccelerators>
    <KeyboardAccelerator x:Uid="myKeyAccelerator" Modifiers="Control"/>
  </Button.KeyboardAccelerators>
</Button>
```

### <a name="setup-an-accelerator-programmatically"></a>Configurar um acelerador programaticamente

Aqui está um exemplo de forma programática definindo um acelerador:

``` csharp
void AddAccelerator(
  VirtualKeyModifiers keyModifiers, 
  VirtualKey key, 
  TypedEventHandler<KeyboardAccelerator, KeyboardAcceleratorInvokedEventArgs> handler )
  {
    var accelerator = 
      new KeyboardAccelerator() 
      { 
        Modifiers = keyModifiers, Key = key
      };
    accelerator.Invoked = handler;
    this.KeyAccelerators.Add(accelerator);
  }
```

> [!NOTE]
> KeyboardAccelerator não é compartilhável, a mesma KeyboardAccelerator não pode ser adicionada a vários elementos.

### <a name="override-keyboard-accelerator-behavior"></a>Substituir o comportamento de acelerador de teclado

Você pode manipular o evento [KeyboardAccelerator.Invoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Invoked) para substituir o comportamento de KeyboardAccelerator padrão.

Esse exemplo mostra como substituir o comando "Selecionar tudo" (acelerador de teclado Ctrl + A) em um controle ListView personalizado. Também definimos a propriedade Handled como true para impedir que o evento continue sendo propagado.

```csharp
public class MyListView : ListView
{
  …
  protected override void OnKeyboardAcceleratorInvoked(KeyboardAcceleratorInvokedEventArgs args) 
  {
    if(args.Accelerator.Key == VirtualKey.A 
      && args.Accelerator.Modifiers == KeyboardModifiers.Control)
    {
      CustomSelectAll(TypeOfSelection.OnlyNumbers); 
      args.Handled = true;
    }
  }
  …
}
```

## <a name="related-articles"></a>Artigos relacionados

* [Interações por teclado](keyboard-interactions.md)
* [Teclas de acesso](access-keys.md)

**Exemplos**
* [XAML Controls Gallery (também conhecido como XamlUiBasics)](https://github.com/Microsoft/Windows-universal-samples/tree/c2aeaa588d9b134466bbd2cc387c8ff4018f151e/Samples/XamlUIBasics)


 

