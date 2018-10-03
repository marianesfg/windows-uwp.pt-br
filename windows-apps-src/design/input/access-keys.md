---
author: Karl-Bridge-Microsoft
Description: Learn how to improve both the usability and the accessibility of your UWP app by providing an intuitive way for users to quickly navigate and interact with an app's visible UI through a keyboard instead of a pointer device (such as touch or mouse).
title: Diretrizes do design de teclas de acesso
label: Access keys design guidelines
keywords: teclado, tecla de acesso, dica de tecla, acessibilidade, navegação, foco, texto, entrada, interação do usuário
template: detail.hbs
ms.author: kbridge
ms.date: 06/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 8e842d6c5b8e62a9c043c97849fdf17f524ccfc7
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4314200"
---
# <a name="access-keys"></a>Teclas de acesso

As teclas de acesso são atalhos de teclado que melhoram a usabilidade e a acessibilidade dos aplicativos do Windows, oferecendo aos usuários uma forma intuitiva e rápida de navegação e interação com uma interface de usuário do aplicativo visível por meio de um teclado em vez de um dispositivo de ponteiro (como mouse ou touch).

Consulte o tópico [Teclas aceleradoras](keyboard-accelerators.md) para obter detalhes sobre como invocar ações comuns em um aplicativo do Windows com atalhos de teclado. 

> [!NOTE]
> Um teclado é indispensável para usuários com determinadas deficiências (veja [Acessibilidade do teclado](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility)), além de ser uma ferramenta importante para usuários que o preferem como maneira mais eficiente de interagir com um aplicativo.

A Plataforma Universal do Windows (UWP) oferece suporte interno para os controles da plataforma, tanto para teclas de acesso baseadas no teclado como para comentários sobre a interface do usuário associado por meio de indicações visuais, chamadas de Dicas de Tecla.

## <a name="overview"></a>Visão geral

Uma tecla de acesso é uma combinação da tecla Alt e uma ou mais teclas alfanuméricas—às vezes chamado de *mnemônico*—normalmente pressionadas em sequência, e não simultaneamente.

Dicas de tecla são selos exibidos ao lado dos controles que suportam teclas de acesso quando o usuário pressiona a tecla Alt. Cada Dica de Tecla contém as teclas alfanuméricas que ativam o controle associado.

> [!NOTE]
> Atalhos de teclado possuem, automaticamente, suporte para teclas de acesso com um único caractere alfanumérico. Por exemplo, pressionar simultaneamente Alt+F no Word abre o menu Arquivo sem exibir as Dicas de Tecla.

Pressionar a tecla Alt inicializa a funcionalidade teclas de acesso e exibe todas as combinações de teclas disponíveis atualmente em Dicas de Tecla. Pressionamentos subsequentes de teclas são manipulados pela estrutura da tecla de acesso, que rejeita teclas inválidas até que uma tecla de acesso válida seja pressionada, ou até que as teclas Enter, Esc, Tab ou setas sejam pressionadas para desativar as teclas de acesso e devolver o manuseio de teclas ao aplicativo.

Os aplicativos do Microsoft Office oferecem suporte extensivo para as teclas de acesso. A imagem a seguir mostra a aba Página Inicial do Word com teclas de acesso ativadas (observe o suporte aos números e teclas múltiplas).

![Selos da Dica de Tecla para teclas de acesso no Microsoft Word](images/accesskeys/keytip-badges-word.png)

_Selos KeyTip para teclas de acesso no Microsoft Word_

Para adicionar uma tecla de acesso a um controle, use a **propriedade AccessKey**. O valor desta propriedade especifica a sequência de teclas de acesso, o atalho (caso seja um único alfanumérico) e a Dica de Tecla.

``` xaml
<Button Content="Accept" AccessKey="A" Click="AcceptButtonClick" />
```

## <a name="when-to-use-access-keys"></a>Quando usar as teclas de acesso

Recomendamos que você especifique teclas de acesso sempre que for apropriado em sua interface de usuário, e ofereça suporte à elas em todos os controles personalizados.

1.  **As teclas de acesso tornam seu aplicativo mais acessível** para usuários com deficiências motoras, incluindo os usuários que conseguem pressionar apenas uma tecla por vez ou que possuem dificuldades para usar um mouse.

    Uma interface de usuário do teclado bem projetada é um aspecto importante da acessibilidade do software. Ela permite que os usuários com deficiência visual ou que possuam determinadas deficiências motoras naveguem em um aplicativo e interajam com seus recursos. Esses usuários talvez não consigam usar um mouse e, em vez disso, usem diversas tecnologias assistenciais, como ferramentas avançadas de teclado, teclados virtuais, ampliadores de tela, leitores de tela e utilitários de entrada de voz. Para esses usuários, a cobertura de comando abrangente é essencial.

2.  **As teclas de acesso tornam seu aplicativo mais funcional** para usuários avançados que preferem interagir através do teclado.

    Os usuários experientes apresentam, muitas vezes, uma forte preferência pelo teclado, pois os comandos por teclado podem ser inseridos mais rapidamente e não exigem que eles retirem as mãos do teclado. Para esses usuários, eficiência e consistência são cruciais; a abrangência é importante apenas para os comandos mais usados.

## <a name="set-access-key-scope"></a>Definir o âmbito das teclas de acesso

Quando existem muitos elementos na tela que oferecem suporte a teclas de acesso, recomendamos delimitar as teclas de acesso para reduzir a **carga cognitiva**. Isso minimiza o número de teclas de acesso na tela, o que as torna mais fáceis de encontrar e aumenta a eficiência e a produtividade.

Por exemplo, o Microsoft Word fornece dois âmbitos para as teclas de acesso: um primário para as abas da Faixa de Opções e um secundário para comandos da aba selecionada.

As imagens a seguir demonstram os dois âmbitos de teclas de acesso no Word. A primeira mostra as teclas de acesso primárias que permitem que um usuário selecione a aba e outros comandos de alto nível, e a segunda mostra as teclas de acesso secundárias para a aba Página Inicial.

![Teclas de acesso primário no Microsoft Word](images/accesskeys/primary-access-keys-word.png)
_Teclas de acesso primário no Microsoft Word_

![Teclas de acesso secundário no Microsoft Word](images/accesskeys/secondary-access-keys-word.png)
_Teclas de acesso secundário no Microsoft Word_

As teclas de acesso podem ser duplicadas para elementos de âmbitos diferentes. No exemplo anterior, "2" é a tecla de acesso para Desfazer no âmbito principal, e também para "Itálico" no âmbito secundário.

Aqui, mostramos como definir o escopo das teclas de acesso.

``` xaml
<CommandBar x:Name="MainCommandBar" AccessKey="M" >
    <AppBarButton AccessKey="G" Icon="Globe" Label="Go"/>
    <AppBarButton AccessKey="S" Icon="Stop" Label="Stop"/>
    <AppBarSeparator/>
    <AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
        <AppBarButton.Flyout>
            <MenuFlyout>
                <MenuFlyoutItem AccessKey="A" Icon="Globe" Text="Refresh A" />
                <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
                <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
                <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            </MenuFlyout>
        </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarButton AccessKey="B" Icon="Back" Label="Back"/>
    <AppBarButton AccessKey="F" Icon="Forward" Label="Forward"/>
    <AppBarSeparator/>
    <AppBarToggleButton AccessKey="V" Icon="Favorite" Label="Favorite"/>
    <CommandBar.SecondaryCommands>
        <AppBarToggleButton Icon="Like" AccessKey="L" Label="Like"/>
        <AppBarButton Icon="Setting" AccessKey="T" Label="Settings" />
    </CommandBar.SecondaryCommands>
</CommandBar>
```

![Teclas de acesso primárias para o CommandBar](images/accesskeys/primary-access-keys-commandbar.png)

_Âmbito primário do CommandBar e teclas de acesso suportadas_

![Teclas de acesso secundárias para o CommandBar](images/accesskeys/secondary-access-keys-commandbar.png)

_Escopo secundário do CommandBar e teclas de acesso suportadas_

### <a name="windows-10-creators-update-and-older"></a>Atualização do Windows 10 para Criadores e anteriores

Antes do Windows 10 Fall Creators Update, alguns controles, como o CommandBar, não ofereciam suporte a escopos internos das teclas de acesso.

O exemplo a seguir mostra como oferecer suporte aos SecondaryCommands da CommandBar com as teclas de acesso disponíveis assim que um comando pai é invocado (semelhante à Faixa de Opções no Word).

```xaml
<local:CommandBarHack x:Name="MainCommandBar" AccessKey="M" >
    <AppBarButton AccessKey="G" Icon="Globe" Label="Go"/>
    <AppBarButton AccessKey="S" Icon="Stop" Label="Stop"/>
    <AppBarSeparator/>
    <AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
        <AppBarButton.Flyout>
            <MenuFlyout>
                <MenuFlyoutItem AccessKey="A" Icon="Globe" Text="Refresh A" />
                <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
                <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
                <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            </MenuFlyout>
        </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarButton AccessKey="B" Icon="Back" Label="Back"/>
    <AppBarButton AccessKey="F" Icon="Forward" Label="Forward"/>
    <AppBarSeparator/>
    <AppBarToggleButton AccessKey="V" Icon="Favorite" Label="Favorite"/>
    <CommandBar.SecondaryCommands>
        <AppBarToggleButton Icon="Like" AccessKey="L" Label="Like"/>
        <AppBarButton Icon="Setting" AccessKey="T" Label="Settings" />
    </CommandBar.SecondaryCommands>
</local:CommandBarHack>
```

```csharp
public class CommandBarHack : CommandBar
{
    CommandBarOverflowPresenter secondaryItemsControl;
    Popup overflowPopup;

    public CommandBarHack()
    {
        this.ExitDisplayModeOnAccessKeyInvoked = false;
        AccessKeyInvoked += OnAccessKeyInvoked;
    }

    protected override void OnApplyTemplate()
    {
        base.OnApplyTemplate();

        Button moreButton = GetTemplateChild("MoreButton") as Button;
        moreButton.SetValue(Control.IsTemplateKeyTipTargetProperty, true);
        moreButton.IsAccessKeyScope = true;

        // SecondaryItemsControl changes
        secondaryItemsControl = GetTemplateChild("SecondaryItemsControl") as CommandBarOverflowPresenter;
        secondaryItemsControl.AccessKeyScopeOwner = moreButton;

        overflowPopup = GetTemplateChild("OverflowPopup") as Popup;
    }

    private void OnAccessKeyInvoked(UIElement sender, AccessKeyInvokedEventArgs args)
    {
        if (overflowPopup != null)
        {
            overflowPopup.Opened += SecondaryMenuOpened;
        }
    }

    private void SecondaryMenuOpened(object sender, object e)
    {
        //This is not neccesay given we are automatically pushing the scope.
        var item = secondaryItemsControl.Items.First();
        if (item != null && item is Control)
        {
            (item as Control).Focus(FocusState.Keyboard);
        }
        overflowPopup.Opened -= SecondaryMenuOpened;
    }
}
```

## <a name="avoid-access-key-collisions"></a>Evitar colisões de teclas de acesso

Colisões de teclas de acesso ocorrem quando dois ou mais elementos no mesmo âmbito possuem teclas de acesso duplicadas, ou iniciam com os mesmos caracteres alfanuméricos.

O sistema resolve as teclas de acesso duplicadas processando a tecla de acesso do primeiro elemento adicionado à árvore visual, ignorando todos os outros.

Quando várias teclas de acesso começam com o mesmo caractere (por exemplo, "A", "A1" e "AB"), o sistema processa a tecla de acesso com caractere único e ignora todas as outras.

Evite colisões usando teclas de acesso exclusivas ou delimitando os comandos.

## <a name="choose-access-keys"></a>Escolher as teclas de acesso

Considere o seguinte quando for escolher as teclas de acesso:

-   Use um único caractere para minimizar a quantidade de teclas pressionadas e ofereça suporte às teclas de aceleração por padrão (Alt+AccessKey)
-   Evite usar mais de dois caracteres
-   Evite colisões de teclas de acesso
-   Evite caracteres que são difíceis de diferenciar de outros caracteres, como a letra "I" e o número "1" ou a letra "O" e o número "0"
-   Use precedentes conhecidos de outros aplicativos populares como o Word ("F" para "Arquivo", "H" para "Página Inicial" e assim por diante)
-   Use o primeiro caractere do nome do comando ou um caractere com fácil associação ao comando, o que ajuda a lembrar do atalho.
    -   Se a primeira letra já estiver atribuída, usa a letra que está mais próxima da primeira letra do nome do comando ("N" para Inserir)
    -   Use uma consoante característica do nome do comando ("W" para Exibição)
    -   Use uma vogal do nome do comando.

## <a name="localize-access-keys"></a>Localizar teclas de acesso

Se o seu aplicativo se aplica a vários idiomas, você também deve **considerar diferentes teclas de acesso para cada região**. Por exemplo, "H" para "Home" em en-US e "I" para "Incio" em es-ES.

Use a extensão x:Uid na marcação para aplicar recursos de local, conforme mostrado aqui:

``` xaml
<Button Content="Home" AccessKey="H" x:Uid="HomeButton" />
```
Recursos para cada idioma são adicionados às respectivas pastas de Caracteres no projeto:

![Pastas de caracteres dos recursos em inglês e espanhol](images/accesskeys/resource-string-folders.png)

_Pastas de caracteres dos recursos em inglês e espanhol_

As teclas de acesso para diferentes regiões são especificadas no arquivo resources.resw do projeto:

![Especificar a propriedade AccessKey especificada no arquivo resources.resw](images/accesskeys/resource-resw-file.png)

_Especificar a propriedade AccessKey especificada no arquivo resources.resw_

Para obter mais informações, consulte [Traduzindo recursos da interface de usuário ](https://msdn.microsoft.com/library/windows/apps/xaml/Hh965329(v=win.10).aspx)

## <a name="key-tip-positioning"></a>Posicionamento da Dica de Tecla

As dicas de tecla são exibidas como selos flutuantes em relação ao seu respectivo elemento de interface do usuário, considerando a presença de outros elementos da interface, outras Dicas de Tecla e as bordas da tela.

Normalmente, a localização padrão da Dica de Tecla é suficiente e oferece suporte interno para interface de usuário adaptável.

![Exemplo de posicionamento automático da Dica de Tecla](images/accesskeys/auto-keytip-position.png)

_Exemplo de posicionamento automático da Dica de Tecla_

No entanto, caso precise de um controle maior sobre o posicionamento da Dica de Tecla, recomendamos o seguinte:

1.  **Princípio da associação óbvia**: O usuário pode facilmente associar o controle à Dica de Tecla.

    a.  A Dica de Tecla deve estar **próxima** ao elemento que possui a tecla de acesso (o proprietário).  
    b.  A Dica de Tecla deve **evitar cobrir elementos habilitados** que possuam teclas de acesso.   
    c.  Se uma Dica de Tecla não puder ser colocada perto do seu proprietário, ela deve sobrepô-lo. 

2.  **Detectabilidade**: O usuário pode descobrir o controle rapidamente com a Dica de Tecla.

    a.  A Dica de Tecla nunca **sobrepõe** outras Dicas de Tecla.  

3.  **Verificação fácil:** O usuário pode ler as Dicas de Tecla facilmente.

    a.  As Dicas de Tecla devem estar **alinhadas** entre si e com o elemento de interface do usuário.
    b.  As Dicas de Tecla devem ser **agrupadas** o máximo possível. 

### <a name="relative-position"></a>Posição relativa

Use a propriedade **KeyTipPlacementMode** para personalizar o posicionamento da Dica de Tecla por elemento ou por grupo.

Os modos de posicionamento são: Superior, Inferior, Direito, Esquerdo, Oculto, Central e Automático.

![Modos de posicionamento da dica de tecla](images/accesskeys/keytip-postion-modes.png)

_Modos de posicionamento da dica de tecla_

A linha de centro do controle é usada para calcular os alinhamentos vertical e horizontal da Dica de Tecla.

O exemplo a seguir mostra como definir o posicionamento da Dica de Tecla de um grupo de controles usando a propriedade KeyTipPlacementMode de um contêiner StackPanel.

``` xaml
<StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" KeyTipPlacementMode="Top">
  <Button Content="File" AccessKey="F" />
  <Button Content="Home" AccessKey="H" />
  <Button Content="Insert" AccessKey="N" />
</StackPanel>
```

### <a name="offsets"></a>Deslocamentos

Use as propriedades KeyTipHorizontalOffset e KeyTipVerticalOffset de um elemento para obter um controle ainda mais granular da localização da Dica de Tecla.

> [!NOTE]
> Deslocamentos não poderão ser definidos quando o KeyTipPlacementMode estiver definido como Automático.

A propriedade KeyTipHorizontalOffset indica o quanto mover a Dica de Tecla para a direita ou para a esquerda. o exemplo mostra como definir os deslocamentos da Dica de Tecla para um botão.

![Modos de posicionamento da dica de tecla](images/accesskeys/keytip-offsets.png)

_Definir deslocamentos horizontal e vertical para uma Dica de Tecla_

``` xaml
<Button
  Content="File"
  AccessKey="F"
  KeyTipPlacementMode="Bottom"
  KeyTipHorizontalOffset="20"
  KeyTipVerticalOffset="-8" />
```

### <a name="screen-edge-alignment-screen-edge-alignment-listparagraph"></a>Alinhamento de borda de tela {#screen-edge-alignment .ListParagraph}

A localização de uma Dica de Tecla é ajustada automaticamente com base nas bordas da tela para garantir que a Dica de Tecla esteja completamente visível. Quando isso ocorre, a distância entre o controle e o ponto de alinhamento da Dica de Tecla pode diferir dos valores especificados para os deslocamentos horizontal e vertical.

![Modos de posicionamento da dica de tecla](images/accesskeys/keytips-screen-edge.png)

_As bordas da tela fazem com que a Dica de Tecla se reposicione automaticamente_

## <a name="key-tip-style"></a>Estilo da Dica de Tecla

Recomendamos usar o suporte interno da Dica de Tecla para temas da plataforma, incluindo o alto contraste.

Caso precise especificar seus próprios estilos da Dica de Tecla, use recursos de aplicativo, como KeyTipFontSize (tamanho da fonte), KeyTipFontFamily (família de fontes), KeyTipBackground (plano de fundo), KeyTipForeground (primeiro plano), KeyTipPadding (preenchimento), KeyTipBorderBrush (cor das bordas) e KeyTipBorderThemeThickness (espessura das bordas).

![Modos de posicionamento da dica de tecla](images/accesskeys/keytip-customization.png)

_Opções de personalização da dica de tecla_

Este exemplo demonstra como alterar esses recursos do aplicativo:

 ```xaml  
<Application.Resources>
  <SolidColorBrush Color="DarkGray" x:Key="MyBackgroundColor" />
  <SolidColorBrush Color="White" x:Key="MyForegroundColor" />
  <SolidColorBrush Color="Black" x:Key="MyBorderColor" />
  <StaticResource x:Key="KeyTipBackground" ResourceKey="MyBackgroundColor" />
  <StaticResource x:Key="KeyTipForeground" ResourceKey="MyForegroundColor" />
  <StaticResource x:Key="KeyTipBorderBrush" ResourceKey="MyBorderColor"/>
  <FontFamily x:Key="KeyTipFontFamily">Consolas</FontFamily>
  <x:Double x:Key="KeyTipContentThemeFontSize">18</x:Double>
  <Thickness x:Key="KeyTipBorderThemeThickness">2</Thickness>
  <Thickness x:Key="KeyTipThemePadding">4,4,4,4</Thickness>
</Application.Resources>
```

## <a name="access-keys-and-narrator"></a>Teclas de acesso e Narrador

A estrutura XAML expõe Propriedades de Automação que permitem que clientes da Automação da interface de usuário descubram informações sobre elementos da interface de usuário.

Se você especificar a propriedade AccessKey em um controle de UIElement ou TextElement, você pode usar a propriedade [AutomationProperties.AccessKey](https://msdn.microsoft.com/library/windows/apps/hh759763) para obter esse valor. Os clientes de acessibilidade, como o Narrador, lê o valor dessa propriedade cada vez que um elemento recebe o foco.

## <a name="related-articles"></a>Artigos relacionados

* [Interações por teclado](keyboard-interactions.md)
* [Aceleradores de teclado](keyboard-accelerators.md)

**Exemplos**
* [XAML Controls Gallery (também conhecido como XamlUiBasics)](https://github.com/Microsoft/Windows-universal-samples/tree/c2aeaa588d9b134466bbd2cc387c8ff4018f151e/Samples/XamlUIBasics)


