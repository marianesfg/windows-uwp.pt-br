---
ms.openlocfilehash: 15379e51f8c272d0cc1e888684322104186bb200
ms.sourcegitcommit: 7a1d5198345d114c58287d8a047eadc4fe10f012
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59249498"
---
# <a name="teaching-tip"></a>Dica de ensino

Uma dica de ensino é um submenu semipersistente e ricas em conteúdo que fornece informações contextuais. Ele geralmente é usado para informar, lembrando e ensinando os usuários sobre os recursos novos e importantes que podem melhorar sua experiência.

**APIs importantes:** [Classe TeachingTip](https://docs.microsoft.com/en-us/uwp/api/microsoft.ui.xaml.controls.teachingtip)

Uma dica de ensino pode ser de descarte suave ou exigem uma ação explícita para fechar. Uma dica de ensino pode direcionar a um elemento de interface do usuário específico com cauda e também ser usada sem uma parte final ou de destino.

## <a name="is-this-the-right-control"></a>Esse é o controle correto? 

Use uma **TeachingTip** controle para focalizar atenção de um usuário novo ou importantes atualizações e recursos, lembrá-lo sobre um usuário de opções não essenciais que seria melhorar sua experiência ou ensinar a um usuário como uma tarefa deve ser concluída. 

Porque a dica de ensino for transitória, não seria o controle recomendado para avisar os usuários sobre erros ou alterações de status importantes.


## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="../images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o <strong style="font-weight: semi-bold">da Galeria de controles XAML</strong> aplicativo instalado, clique aqui para <a href="xamlcontrolsgallery:/item/TeachingTip">abrir o aplicativo e ver o TeachingTip em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Uma dica de ensino pode ter várias configurações, incluindo dos notáveis.

Uma dica de ensino pode direcionar um elemento de interface do usuário específico com cauda para melhorar a clareza contextual das informações que ela está apresentando. 

![Um aplicativo de exemplo com uma dica de ensino direcionando o salvamento botão. O título da dica lê "Salvando automaticamente" e o subtítulo lê "Podemos salvar suas alterações conforme avança, para que você nunca precise". Há um botão Fechar no canto superior direito da dica de ensino.](../images/teaching-tip-targeted.png)

Quando as informações apresentadas não pertencem a um determinado elemento de interface do usuário, uma dica de ensino nontargeted pode ser criada, removendo a parte final.

![Um aplicativo de amostra com uma dica de ensino no canto inferior direito. O título da dica lê "Salvando automaticamente" e o subtítulo lê "Podemos salvar suas alterações conforme avança, para que você nunca precise". Há um botão Fechar no canto superior direito da dica de ensino.](../images/teaching-tip-non-targeted.png)

Uma dica de ensino pode exigir que o usuário para descartá-la por meio de um botão "X" no canto superior ou de "Fechar" na parte inferior. Uma dica de ensino podem também ser descarte suave e não habilitado neste caso, existe é nenhum botão descartar e a dica de ensino irá ignorar em vez disso, quando o usuário rola ou interage com outros elementos do aplicativo. Devido a esse comportamento de descarte suave e dicas são a melhor solução quando uma dica precisa ser colocada em uma área rolável. 

![Um aplicativo de exemplo com uma dica de ensino no canto inferior direito de descarte suave. O título da dica lê "Salvando automaticamente" e o subtítulo lê "Podemos salvar suas alterações conforme avança, para que você nunca precise".](../images/teaching-tip-light-dismiss.png)


### <a name="create-a-teaching-tip"></a>Criar uma dica de ensino

Aqui está o XAML para um destino ensino controle que demonstra o aspecto padrão do TeachingTip com um título e subtítulo da dica. Observe que a dica de ensino pode aparecer em qualquer lugar na árvore de elementos ou code-behind. No exemplo a seguir, ele está localizado em um ResourceDictionary.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Save automatically"
            Subtitle="When you save your file to OneDrive, we save your changes as you go - so you never have to.">
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

C#
```C#
public MainPage()
{
    this.InitializeComponent();

    if(!HaveExplainedAutoSave())
    {
        AutoSaveTip.IsOpen = true;
        SetHaveExplainedAutoSave();
    }
}
```

Aqui está o resultado quando a página que contém o botão e ensinar a dica é mostrada:

![Um aplicativo de exemplo com uma dica de ensino direcionando o salvamento botão. O título da dica lê "Salvando automaticamente" e o subtítulo lê "Podemos salvar suas alterações conforme avança, para que você nunca precise". Há um botão Fechar no canto superior direito da dica de ensino.](../images/teaching-tip-targeted.png)

### <a name="non-targeted-tips"></a>Dicas de não-alvo

Nem todas as dicas estão relacionados a um elemento na tela. Para esses cenários, não defina a propriedade de destino e a dica de ensino em vez disso, será exibido em relação às bordas da raiz do xaml. No entanto, uma dica de ensino pode ter a cauda removida enquanto mantém o posicionamento em relação a um elemento de interface do usuário definindo a propriedade TailVisibility para "Collapsed". O exemplo a seguir é de uma dica de ensino não sejam o alvo.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to.">
</controls:TeachingTip>
```

Observe que, neste exemplo o TeachingTip está na árvore de elementos em vez de em um dicionário de recurso ou no code-behind. Isso não tem nenhum efeito no comportamento. o TeachingTip só exibe quando aberto e ocupa nenhum espaço de layout.

![Um aplicativo de amostra com uma dica de ensino no canto inferior direito. O título da dica lê "Salvando automaticamente" e o subtítulo lê "Podemos salvar suas alterações conforme avança, para que você nunca precise". Há um botão Fechar no canto superior direito da dica de ensino.](../images/teaching-tip-non-targeted.png)

### <a name="preferred-placement"></a>Posicionamento preferencial

Dica de ensino replica do submenu [FlyoutPlacementMode](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode) comportamento de posicionamento com a propriedade TeachingTipPlacementMode. O modo de posicionamento padrão tentará colocar uma dica de ensino destino acima de seu destino e uma dica de ensino não sejam o alvo centralizado na parte inferior da raiz do xaml. Como com o submenu, se o modo preferido de posicionamento não seria deixar espaço para a dica de ensino Mostrar, outro modo de posicionamento será automaticamente escolhido. 

Para aplicativos que preveem gamepad entrada, consulte [interações gamepad e controle remoto]( https://docs.microsoft.com/en-us/windows/uwp/design/input/gamepad-and-remote-interactions#xy-focus-navigation-and-interaction). Incentivamos a acessibilidade de gamepad de cada dica de ensino usando todas as possíveis configurações de interface do usuário de um aplicativo de teste.

Uma dica de ensino de destino com sua PreferredPlacement definido como "BottomLeft" será exibida com a parte final centralizada na parte inferior do seu destino com o corpo da dica de ensino voltou para a esquerda.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            PreferredPlacement="BottomLeft">
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![Um aplicativo de exemplo com um botão "Salvar" que está sendo direcionado por uma dica de ensino sob o canto esquerdo. O título da dica lê "Salvando automaticamente" e o subtítulo lê "Podemos salvar suas alterações conforme avança, para que você nunca precise". Há um botão Fechar no canto superior direito da dica de ensino.](../images/teaching-tip-targeted-preferred-placement.png)


Uma dica de não sejam o alvo ensinando com seu PreferredPlacement definido como "BottomLeft" será exibida no canto inferior esquerdo da raiz do xaml.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft">
</controls:TeachingTip>
```

![Um aplicativo de amostra com uma dica de ensino no canto inferior esquerdo. O título da dica lê "Salvando automaticamente" e o subtítulo lê "Podemos salvar suas alterações conforme avança, para que você nunca precise". Há um botão Fechar no canto superior direito da dica de ensino.](../images/teaching-tip-non-targeted-preferred-placement.png)

O diagrama a seguir ilustra o resultado de todos os modos de 13 PreferredPlacement que podem ser definidas para o destino ensinando dicas.
![Três objetos rotulada "destino" com destino ensinando dicas usadas ao redor deles para mostrar modos de destino de posicionamento preferencial da dica de ensino. Centralizado no meio do primeiro destino é uma dica de ensino direcionada rotulada como "Center" apontando para baixo no destino cauda. Centralizado acima o primeiro destino é uma dica de ensino direcionada rotulada como "Top" apontando para baixo no destino cauda. Centralizado à direita do primeiro destino é uma dica de ensino direcionada rotulada como "Right" apontando para o destino cauda esquerda. Centralizada abaixo primeiro destino é uma dica de ensino direcionada rotulada como "Baixo", apontando para cima no destino cauda. Centralizado à esquerda do primeiro destino é uma dica de ensino direcionada rotulada como "Left" que aponta para a direita no destino cauda. À esquerda do segundo destino é uma dica de ensino rotulada "LeftTop" que aponta para o destino à direita e seu corpo mudou para cima. Acima da meta a segunda é uma dica de ensino rotulada "TopLeft" que aponta para baixo para o destino e tem seu corpo deslocados para a esquerda. À direita do segundo o destino é uma dica de ensino rotulada como "RightBottom" que aponte para a esquerda no destino e seu corpo mudou para baixo. Abaixo da segunda meta é uma dica de ensino rotulada "BottomRight" que aponta para cima no destino e seu corpo mudou rightward. Acima da meta a terceiro é uma dica de ensino rotulada "Superior direito" que aponta para baixo para o destino e seu corpo mudou rightward. À direita da terceira destino é uma dica de ensino rotulada como "RightTop" que aponte para a esquerda no destino e seu corpo mudou para cima. Abaixo da meta de terceiro é uma dica de ensino rotulada "BottomLeft" que aponta para cima no destino e tem seu corpo deslocados para a esquerda. À esquerda do terceiro destino é uma dica de ensino rotulada "LeftBottom" que aponta para o destino à direita e seu corpo mudou para baixo.](../images/teaching-tip-targeted-preferred-placement-modes.png)

O diagrama a seguir ilustra o resultado de todos os modos de 13 PreferredPlacement que podem ser definidas para obter dicas de ensino não sejam o alvo.
![Uma janela de aplicativo mostrando nove dicas de ensino não sejam o alvo para demonstrar os modos de não sejam o alvo preferencial posicionamento da dica de ensino. A dica de ensino no canto superior esquerdo do aplicativo é rotulado como "TopLeft ou LeftTop." A dica de ensino centralizada na borda superior do aplicativo é rotulado como "Top". A dica de ensino no canto superior direito do aplicativo é rotulado como "Superior direito ou RightTop." A dica de ensino centralizada na borda esquerda do aplicativo é rotulado como "Left". A dica de ensino centralizada no meio do aplicativo é rotulado como "Center".
A dica de ensino centralizada na extremidade direita do aplicativo é rotulado como "Right". A dica de ensino no canto inferior esquerdo do aplicativo é rotulado como "BottomLeft ou LeftBottom." A dica de ensino centralizada na parte inferior do aplicativo é rotulado como "Inferior". A dica de ensino no canto inferior direito do aplicativo é rotulado como "BottomRight ou RightBottom."](../images/teaching-tip-non-targeted-preferred-placement-modes.png)

### <a name="add-a-placement-margin"></a>Adicionada uma margem de posicionamento  

Você pode controlar quanto uma dica de ensino de destino é definida além de seu destino e quanto uma dica de ensino não sejam o alvo é definida além das bordas da raiz xaml usando a propriedade PlacementMargin. Como o [margem](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.margin), PlacementMargin possui quatro valores – à esquerda, direita, superior e inferior – apenas os valores relevantes são usados. Por exemplo, PlacementMargin.Left se aplica quando a dica é deixada de destino ou na borda esquerda da raiz do xaml.

O exemplo a seguir mostra uma dica não sejam o alvo de com esquerda/superior/direito/inferior do PlacementMargin definidos como 80.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft"
    PlacementMargin="80">
</controls:TeachingTip>
```

![Um aplicativo de amostra com uma dica de ensino posicionado na direção, mas não totalmente contra, o canto inferior direito. O título da dica lê "Salvando automaticamente" e o subtítulo lê "Podemos salvar suas alterações conforme avança, para que você nunca precise". Há um botão Fechar no canto superior direito da dica de ensino.](../images/teaching-tip-placement-margin.png)


### <a name="add-content"></a>Adicionar conteúdo

Conteúdo pode ser adicionado a uma dica de ensino usando a propriedade de conteúdo. Se houver mais conteúdo para mostrar que o que permitirá que o tamanho de uma dica de ensino, uma barra de rolagem será habilitada automaticamente para permitir que o usuário role a área de conteúdo. 

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to.">
                <StackPanel>
                    <CheckBox x:Name="HideTipsCheckBox" Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                    <TextBlock>You can change your tip preferences in <Hyperlink NavigateUri="app:/item/SettingsPage">Settings</Hyperlink> if you change your mind.</TextBlock>
                </StackPanel>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![Um aplicativo de exemplo com uma dica de ensino direcionando o salvamento botão. O título da dica lê "Salvando automaticamente" e o subtítulo lê "Podemos salvar suas alterações conforme avança, para que você nunca precise". Na área de conteúdo da dica de ensino é uma caixa de seleção rotulada como "Não mostrar dicas na inicialização" e abaixo está o texto "Você pode alterar as preferências de dica em configurações, se você mudar de ideia" onde "Configurações" são um link para a página de configurações do aplicativo. Há um botão Fechar no canto superior direito da dica de ensino.](../images/teaching-tip-content.png)

### <a name="add-buttons"></a>Adicionar botões

Por padrão, um padrão de "X" botão Fechar é mostrado ao lado do título de uma dica de ensino. Botão Fechar pode ser personalizado com a propriedade CloseButtonContent, caso o botão é movido para a parte inferior da dica de ensino.

**Observação: Nenhum botão Fechar será exibido no descarte suave e dicas habilitadas**

Um botão de ação personalizado pode ser adicionado, definindo a propriedade ActionButtonContent (e, opcionalmente, o ActionButtonCommand e as propriedades de ActionButtonCommandParameter).

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources> 
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            ActionButtonContent="Disable"
            ActionButtonCommand="DisableAutoSave"
            CloseButtonContent="Got it!">
                <StackPanel>
                    <CheckBox x:Name="HideTipsCheckBox" Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                    <TextBlock>You can change your tip preferences in <Hyperlink NavigateUri="app:/item/SettingsPage">Settings</Hyperlink> if you change your mind.</TextBlock>
                </StackPanel>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![Um aplicativo de exemplo com uma dica de ensino direcionando o salvamento botão. O título da dica lê "Salvando automaticamente" e o subtítulo lê "Podemos salvar suas alterações conforme avança, para que você nunca precise". Na área de conteúdo da dica de ensino é uma caixa de seleção rotulada como "Não mostrar dicas na inicialização" e abaixo está o texto "Você pode alterar as preferências de dica em configurações, se você mudar de ideia" onde "Configurações" são um link para a página de configurações do aplicativo. Na parte inferior do ensino, há dois botões, um cinza à esquerda que lê "Desabilitar" e uma azul à direita que lê "entendi!"](../images/teaching-tip-buttons.png)

### <a name="hero-content"></a>Conteúdo do Hero

Borda para o conteúdo de borda pode ser adicionada a uma dica de ensino, definindo a propriedade HeroContent. O local do conteúdo de hero pode ser definido na parte superior ou inferior de uma dica de ensino, definindo a propriedade HeroContentPlacement.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources> 
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to.">
            <controls:TeachingTip.HeroContent>
                <Image Source="Assets/cloud.png" />
            </controls:TeachingTip.HeroContent>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![Um aplicativo de exemplo com uma dica de ensino direcionando o salvamento botão. O título da dica lê "Salvando automaticamente" e o subtítulo lê "Podemos salvar suas alterações conforme avança, para que você nunca precise". Na parte inferior da dica de ensino é uma imagem de borda a borda de um homem de desenho animado colocar os arquivos na nuvem. Há um botão Fechar no canto superior direito da dica de ensino.](../images/teaching-tip-hero-content.png)

### <a name="add-an-icon"></a>Adicionar um ícone

Pode ser adicionado a um ícone ao lado do título e subtítulo usando a propriedade IconSource. Tamanhos de ícones recomendados incluem 16px, 24px e 32px. 

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            <controls:TeachingTip.IconSource>
                <controls:SymbolIconSource Symbol="Save" />
            </controls:TeachingTip.IconSource>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![Um aplicativo de exemplo com uma dica de ensino direcionando o salvamento botão. O título da dica lê "Salvando automaticamente" e o subtítulo lê "Podemos salvar suas alterações conforme avança, para que você nunca precise". À esquerda do título e subtítulo é um ícone de disquete. Há um botão Fechar no canto superior direito da dica de ensino.](../images/teaching-tip-icon.png)

### <a name="enable-light-dismiss"></a>Habilitar descarte suave

Funcionalidade de descarte suave é desabilitada por padrão, mas ele pode ser habilitado para que uma dica de ensino irá ignorar, por exemplo, quando o usuário rola ou interage com outros elementos do aplicativo. Devido a esse comportamento de descarte suave e dicas são a melhor solução quando uma dica precisa ser colocada em uma área rolável. 

Botão Fechar serão automaticamente removido de uma dica de ensino habilitado de descarte suave para identificar seu comportamento para os usuários de descarte suave. 

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    IsLightDismissEnabled="True">
</controls:TeachingTip>
```

![Um aplicativo de exemplo com uma dica de ensino no canto inferior direito de descarte suave. O título da dica lê "Salvando automaticamente" e o subtítulo lê "Podemos salvar suas alterações conforme avança, para que você nunca precise".](../images/teaching-tip-light-dismiss.png)

### <a name="escaping-the-xaml-root-bounds"></a>Os limites de raiz xaml de escape

No Windows dica de 1 e acima, um ensino 19h versão pode escapar dos limites da raiz xaml e a tela, definindo a propriedade ShouldConstrainToRootBounds. Quando essa propriedade estiver habilitada, uma dica de ensino não tentará manter-se nos limites de raiz xaml nem a tela e sempre será a posição no conjunto de modo PreferredPlacement. Incentivamos a habilitar a propriedade IsLightDismissEnabled e definir o modo de PreferredPlacement mais próximo do centro da raiz do xaml para garantir a melhor experiência para os usuários.

Em versões anteriores do Windows, essa propriedade será ignorada e a dica de ensino sempre permanece dentro dos limites da raiz do xaml.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomRight"
    PlacementMargin="-80,-50,0,0"
    ShouldConstrainToRootBounds="False">
</controls:TeachingTip>
```

![Um aplicativo de amostra com uma dica de ensino fora inferior direita do aplicativo. O título da dica lê "Salvando automaticamente" e o subtítulo lê "Podemos salvar suas alterações conforme avança, para que você nunca precise". Há um botão Fechar no canto superior direito da dica de ensino.](../images/teaching-tip-escape-xaml-root.png)

### <a name="canceling-and-deferring-close"></a>Cancelando e o adiamento de fechamento

O evento de fechamento pode ser usado para cancelar e/ou adiar o fechamento de uma dica de ensino. Isso pode ser usado para manter a dica de ensino aberto ou aguardar para que uma ação ou animação personalizada ocorra. Quando o fechamento de uma dica de ensino for cancelado, IsOpen voltará como true, no entanto, ele permanecerá false durante o adiamento. Um fechamento programático também pode ser cancelado. 

**Observação: Se nenhuma opção de posicionamento permitiria que uma dica de ensino Mostrar completamente, Dica de ensino irá iterar por meio de seu ciclo de vida do evento para forçar um fechamento em vez de exibi-lo sem um botão Fechar acessível. Se o aplicativo cancela o evento de fechamento, a dica de ensino pode permanecer aberta sem um botão Fechar acessível.**

XAML
```XAML
<controls:TeachingTip x:Name="EnableNewSettingsTip"
    Title="New ways to protect your privacy!"
    Subtitle="Please close this tip and review our updated privacy policy and privacy settings."
    Closing="OnTipClosing">
</controls:TeachingTip>
```

C#
```C#
public void OnTipClosing(object sender, TeachingTipClosingEventArgs args)
{
    if (args.Reason == TeachingTipCloseReason.CloseButton)
    {
        using(args.GetDeferral())
        {
            bool success = await UpdateUserSettings(User thisUsersID);
            if(!success)
            {
                //We were not able to update the settings! Don't close the tip and display the reason why.
                args.Cancel = true;
                ShowLastErrorMessage();
            }
        }
    }
}
```

## <a name="remarks"></a>Comentários

### <a name="related-articles"></a>Artigos relacionados 

* [Caixas de diálogo e submenus](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/dialogs-and-flyouts/index)

### <a name="recommendations"></a>Recomendações
* Dicas são impermanent e não devem conter informações ou opções que são essenciais para a experiência de um aplicativo. 
* Tente evitar mostrar dicas de ensino com muita frequência. Dicas de ensino têm mais probabilidade de cada receber atenção individual quando elas estiverem escalonadas em toda a sessões longas ou em várias sessões.    
* Mantenha seu tópico claro e sucinta de dicas. Pesquisas mostram que os usuários, em média, somente de leitura 3 a 5 palavras e compreender apenas palavras de 2 a 3 antes de decidir se deseja interagir com uma dica.
* Não é garantida Gamepad acessibilidade de uma dica de ensino. Para aplicativos que preveem gamepad entrada, consulte [interações gamepad e controle remoto]( https://docs.microsoft.com/en-us/windows/uwp/design/input/gamepad-and-remote-interactions#xy-focus-navigation-and-interaction). Incentivamos a acessibilidade de gamepad de cada dica de ensino usando todas as possíveis configurações de interface do usuário de um aplicativo de teste.
* Ao habilitar uma dica de ensino escapar a raiz do xaml, incentivamos a também habilitar a propriedade IsLightDismissEnabled e definir o modo de PreferredPlacement mais próximo do centro da raiz do xaml. 

### <a name="reconfiguring-an-open-teaching-tip"></a>Reconfigurar uma dica de ensino aberto

Alguns conteúdos e propriedades podem ser reconfiguradas enquanto a dica de ensino está aberta e entrarão em vigor imediatamente. Outras propriedades e o conteúdo, como a propriedade de ícone, os botões de ação e fechar e reconfigurar entre de descarte suave e explícito ignorar todos exigirá a dica de ensino deve ser fechado e reaberto para que as alterações entrem em vigor essas propriedades. Observe que alterar o comportamento de exoneração do manual-descartar para descartar luz enquanto uma dica de ensino está aberta fará com que a dica de ensino a ter seu botão Fechar removido antes do descarte suave e comportamento está habilitado e a dica pode permanecer presa na tela.
