---
author: kbridge
Description: "Aprenda a melhorar a usabilidade e a acessibilidade do seu aplicativo UWP, oferecendo uma forma intuitiva de navegação e interação aos usuários, com uma interface de usuário do aplicativo visível através de um teclado em vez de um dispositivo de ponteiro (como mouse ou toque)."
title: Diretrizes do design de teclas de acesso
label: Access keys design guidelines
keywords: "teclado, tecla de acesso, dica de tecla, acessibilidade, navegação, foco, texto, entrada, interação do usuário"
template: detail.hbs
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: ae8bd60311bc7ead44ee3c9a137a233888be55f3
ms.sourcegitcommit: 0fa9ae00117e8e6b04ed38956e605bb74c1261c6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2017
---
# <a name="access-keys"></a>Teclas de acesso

As teclas de acesso podem melhorar a usabilidade e a acessibilidade do seu aplicativo Windows, oferecendo uma forma intuitiva de navegação e interação aos usuários, com uma interface de usuário do aplicativo visível através de um teclado em vez de um dispositivo de ponteiro (como mouse ou toque).

> [!NOTE]
> Um teclado é indispensável para usuários com determinadas deficiências (veja [Acessibilidade do teclado](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility)), além de ser uma ferramenta importante para usuários que o preferem como maneira mais eficiente de interagir com um aplicativo.

A Plataforma Universal do Windows (UWP) oferece suporte interno para os controles da plataforma, tanto para teclas de acesso baseadas no teclado como para feedback da interface de usuário associado através de indicações visuais, chamadas dicas de tecla.

## <a name="overview"></a>Visão geral

Uma tecla de acesso é uma combinação da tecla Alt e uma ou mais teclas alfanuméricas—às vezes chamado de *mnemônico*—normalmente pressionadas em sequência, e não simultaneamente.

Dicas de tecla são selos exibidos ao lado dos controles que suportam teclas de acesso quando o usuário pressiona a tecla Alt. Cada dica de tecla contém as teclas alfanuméricas que ativam o controle associado.

> [!NOTE]
> Atalhos de teclado possuem, automaticamente, suporte para teclas de acesso com um único caractere alfanumérico. Por exemplo, pressionar simultaneamente Alt+F no Word abre o menu Arquivo sem a exibição de dicas de tecla.

Pressionar a tecla Alt inicializa a funcionalidade teclas de acesso e exibe todas as combinações de teclas disponíveis atualmente em dicas de tecla. Pressionamentos subsequentes de teclas são manipulados pela estrutura da tecla de acesso, que rejeita teclas inválidas até que uma tecla de acesso válida seja pressionada, ou até que as teclas Enter, Esc, Tab ou setas sejam pressionadas para desativar as teclas de acesso e devolver o manuseio de teclas ao aplicativo.

Os aplicativos do Microsoft Office oferecem suporte extensivo para as teclas de acesso. A imagem a seguir mostra a aba Página Inicial do Word com teclas de acesso ativadas (observe o suporte aos números e teclas múltiplas).

![Selos KeyTip para teclas de acesso no Microsoft Word](images/accesskeys/keytip-badges-word.png)

_Selos KeyTip para teclas de acesso no Microsoft Word_

Para adicionar uma tecla de acesso a um controle, use a **propriedade AccessKey**. O valor desta propriedade especifica a sequência de teclas de acesso, o atalho (caso seja um único alfanumérico) e a dica de tecla.

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

![Teclas de acesso primárias no Microsoft Word](images/accesskeys/primary-access-keys-word.png)

_Teclas de acesso primárias no Microsoft Word_

![Teclas de acesso secundárias no Microsoft Word](images/accesskeys/secondary-access-keys-word.png)

Teclas de acesso secundárias no Microsoft Word

As teclas de acesso podem ser duplicadas para elementos de âmbitos diferentes. No exemplo anterior, "2" é a tecla de acesso para Desfazer no âmbito principal, e também para "Itálico" no âmbito secundário.

Alguns controles, como a CommandBar, ainda não suportam os âmbitos internos das teclas de acesso e, consequentemente, é necessário implementá-lo sozinho. O exemplo a seguir demonstra como dar suporte aos SecondaryCommands da CommandBar com as teclas de acesso disponíveis, assim que um comando pai é invocado (semelhante à Faixa de Opções no Word).

``` C#
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


``` xaml
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

![Teclas de acesso primárias para o CommandBar](images/accesskeys/primary-access-keys-commandbar.png)

_Âmbito primário do CommandBar e teclas de acesso suportadas_

![Teclas de acesso secundárias para o CommandBar](images/accesskeys/secondary-access-keys-commandbar.png)

_Âmbito secundário do CommandBar e teclas de acesso suportadas_

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

## <a name="position-key-tips"></a>Posicionar as dicas de tecla

As dicas de tecla são exibidas como selos flutuantes em relação ao seu respectivo elemento da interface de usuário, considerando a presença de outros elementos da interface, outras dicas de tecla e as bordas da tela.

Normalmente, a localização padrão da dica de tecla é suficiente e oferece suporte interno para interface de usuário adaptável.

![Exemplo de posicionamento automático da dica de tecla](images/accesskeys/auto-keytip-position.png)

_Exemplo de posicionamento automático da dica de tecla_

No entanto, case precise de um controle maior sobre o posicionamento da dica de tecla, recomendamos o seguinte:

1.  **Princípio da associação óbvia**: O usuário pode facilmente associar o controle com a dica de tecla.

    a.  A KeyTip deve estar **próxima** ao elemento que possui a tecla de acesso (o proprietário).  
    b.  A KeyTip deve **evitar cobrir elementos habilitados** que possuam teclas de acesso.   
    c.  Se uma KeyTip não pode ser colocada perto do seu proprietário, ela deve sobrepô-lo. 

2.  **Detectabilidade**: O usuário pode facilmente descobrir o controle com a dica de tecla.

    a.  A KeyTip nunca **sobrepõe** outras dicas de tecla.  

3.  **Verificação fácil:** O usuário pode ler as principais dicas de tecla facilmente.

    a.  As KeyTips devem estar **alinhadas** entre si e com o elemento da interface de usuário.
    b.  As KeyTips devem ser **agrupadas** o máximo possível. 

### <a name="relative-position"></a>Posição relativa

Use a propriedade **KeyTipPlacementMode** para personalizar o posicionamento da dica de tecla por elemento ou por grupo.

Os modos de posicionamento são: Superior, Inferior, Direito, Esquerdo, Oculto, Central e Automático.

![Modos de posicionamento da dica de tecla](images/accesskeys/keytip-postion-modes.png)

_Modos de posicionamento da dica de tecla_

A linha de centro do controle é usada para calcular os alinhamentos vertical e horizontal da KeyTip.

O exemplo a seguir mostra como definir o posicionamento da dica de tecla de um grupo de controles usando a propriedade KeyTipPlacementMode de um recipiente StackPanel.

``` xaml
<StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" KeyTipPlacementMode="Top">
  <Button Content="File" AccessKey="F" />
  <Button Content="Home" AccessKey="H" />
  <Button Content="Insert" AccessKey="N" />
</StackPanel>
```

### <a name="offsets"></a>Deslocamentos

Use as propriedades KeyTipHorizontalOffset e KeyTipVerticalOffset de um elemento para obter um controle ainda mais fino sobre o posicionamento da dica de tecla.

> [!NOTE]
> Deslocamentos não podem ser definidos quando o KeyTipPlacementMode está definido como Automático.

A propriedade KeyTipHorizontalOffset indica o quanto mover a dica de tecla para a direita ou esquerda. o exemplo mostra como definir os deslocamentos da dica de tecla para um botão.

![Modos de posicionamento da dica de tecla](images/accesskeys/keytip-offsets.png)

_Definir deslocamentos horizontal e vertical para uma dica de tecla_

``` xaml
<Button
  Content="File"
  AccessKey="F"
  KeyTipPlacementMode="Bottom"
  KeyTipHorizontalOffset="20"
  KeyTipVerticalOffset="-8" />
```

### <a name="screen-edge-alignment-screen-edge-alignment-listparagraph"></a>Alinhamento de borda de tela {#screen-edge-alignment .ListParagraph}

O posicionamento de uma dica de tecla é ajustado automaticamente com base nas bordas da tela para garantir que a dica esteja completamente visível. Quando isso ocorre, a distância entre o controle e o ponto de alinhamento da dica de tecla pode diferir dos valores especificados para os deslocamentos horizontal e vertical.

![Modos de posicionamento da dica de tecla](images/accesskeys/keytips-screen-edge.png)

_As bordas da tela fazem com que a dica de tecla se reposicione automaticamente_

## <a name="style-key-tips"></a>Estilos da dica de tecla

Recomendamos usar o suporte da dica de tecla interno para temas da plataforma, incluindo o alto contraste.

Caso precise especificar seus próprios estilos da dica de tecla, use os recursos KeyTipFontSize (tamanho da fonte), KeyTipFontFamily (família de fontes), KeyTipBackground (plano de fundo), KeyTipForeground (primeiro plano), KeyTipPadding (preenchimento), KeyTipBorderBrush (cor das bordas) e KeyTipBorderThemeThickness (espessura das bordas) do aplicativo.

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
