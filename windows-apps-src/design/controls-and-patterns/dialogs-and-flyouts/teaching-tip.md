---
Description: Uma dica de ensino é um submenu semipersistente e rico em conteúdo que fornece informações contextuais.
title: Dicas de ensino
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
ms.custom: 19H1
ms.localizationpriority: medium
ms.openlocfilehash: dc696c9a57e84e2caade6a2623a72a6048b65621
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319104"
---
# <a name="teaching-tip"></a>Dica de ensino

Uma dica de ensino é um submenu semipersistente e rico em conteúdo que fornece informações contextuais. Geralmente, é usada para informar, lembrar e ensinar os usuários sobre recursos novos e importantes que podem aprimorar a experiência.

**APIs importantes:** [Classe TeachingTip](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.teachingtip?view=winui-2.2)

Uma dica de ensino pode ser um light dismiss ou exigir uma ação explícita para ser fechada. Uma dica de ensino pode apontar para um elemento específico da interface do usuário com sua cauda. Também pode não ter uma cauda ou um alvo.

## <a name="is-this-the-right-control"></a>Esse é o controle correto? 

Use um controle **TeachingTip** para focar a atenção do usuário em atualizações e recursos novos ou importantes, lembrá-lo de opções não essenciais que melhorariam a experiência ou ensinar como uma tarefa deve ser concluída. 

Como a dica de ensino é transitória, não seria o controle recomendado para avisar os usuários sobre erros ou alterações de status importantes.


## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="../images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/TeachingTip">abri-lo e ver o TeachingTip em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Uma dica de ensino pode ter várias configurações, incluindo essas mais notáveis.

Uma dica de ensino pode apontar para um elemento específico da interface do usuário com sua cauda para aumentar a clareza contextual das informações apresentadas. 

![Um aplicativo de exemplo com uma dica de ensino sobre o botão Salvar. O título da dica é "Salvar automaticamente" e o subtítulo é "Salvamos suas alterações conforme são feitas, para que você nunca precise fazê-lo". Há um botão Fechar no canto superior direito da dica de ensino.](../images/teaching-tip-targeted.png)

Quando as informações apresentadas não pertencem a um elemento específico da interface do usuário, é possível remover a cauda e criar uma dica de ensino sem alvo.

![Um aplicativo de exemplo com uma dica de ensino no canto inferior direito. O título da dica é "Salvar automaticamente" e o subtítulo é "Salvamos suas alterações conforme são feitas, para que você nunca precise fazê-lo". Há um botão Fechar no canto superior direito da dica de ensino.](../images/teaching-tip-non-targeted.png)

Uma dica de ensino pode exigir que usuário a ignore por meio do botão "X" no canto superior ou pelo botão "Fechar" na parte inferior. Além disso, uma dica de ensino pode ser habilitada para light dismiss. Nesse caso, não haverá um botão para ignorá-la. Ela desaparecerá quando o usuário rolar a tela ou interagir com outros elementos do aplicativo. Por causa desse comportamento, o light dismiss é a melhor solução quando uma dica precisa ser colocada em uma área rolável. 

![Um aplicativo de exemplo com uma dica com light dismiss no canto inferior direito. O título da dica é "Salvar automaticamente" e o subtítulo é "Salvamos suas alterações conforme são feitas, para que você nunca precise fazê-lo".](../images/teaching-tip-light-dismiss.png)


### <a name="create-a-teaching-tip"></a>Criar uma dica de ensino

Veja o XAML de um controle de dica de ensino com alvo que demonstra o visual padrão de TeachingTip com título e subtítulo. Observe que a dica de ensino pode aparecer em qualquer local na árvore de elementos ou no code-behind. No exemplo a seguir, está localizada em um ResourceDictionary.

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

Veja o resultado quando a página que contém o botão e a dica de ensino é mostrada:

![Um aplicativo de exemplo com uma dica de ensino sobre o botão Salvar. O título da dica é "Salvar automaticamente" e o subtítulo é "Salvamos suas alterações conforme são feitas, para que você nunca precise fazê-lo". Há um botão Fechar no canto superior direito da dica de ensino.](../images/teaching-tip-targeted.png)

### <a name="non-targeted-tips"></a>Dicas sem alvo

Nem todas as dicas estão associadas a um elemento na tela. Nesses cenários, não defina a propriedade Target. Assim, a dica de ensino será exibida nas bordas da raiz XAML. No entanto, é possível remover a cauda de uma dica de ensino e manter a posição em relação a um elemento da interface do usuário definindo a propriedade TailVisibility como "Recolhida". O exemplo a seguir mostra uma dica de ensino sem alvo.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to.">
</controls:TeachingTip>
```

Neste exemplo, observe que TeachingTip está na árvore de elementos, e não em ResourceDictionary ou no code-behind. Isso não afeta o comportamento. TeachingTip só é exibido quando aberto e não ocupa espaço no layout.

![Um aplicativo de exemplo com uma dica de ensino no canto inferior direito. O título da dica é "Salvar automaticamente" e o subtítulo é "Salvamos suas alterações conforme são feitas, para que você nunca precise fazê-lo". Há um botão Fechar no canto superior direito da dica de ensino.](../images/teaching-tip-non-targeted.png)

### <a name="preferred-placement"></a>Posição preferencial

A dica de ensino replica o comportamento de posicionamento [FlyoutPlacementMode](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode) do submenu com a propriedade TeachingTipPlacementMode. O modo de posicionamento padrão tentará colocar a dica de ensino com alvo acima do alvo e uma dica de ensino sem alvo centralizada na parte inferior da raiz XAML. Assim como no submenu, se o modo de posicionamento preferencial não deixar espaço para a exibição da dica de ensino, outro modo de posicionamento será escolhido automaticamente. 

Para aplicativos que preveem entrada de gamepad, confira [Interações de gamepad e de controle remoto]( https://docs.microsoft.com/en-us/windows/uwp/design/input/gamepad-and-remote-interactions#xy-focus-navigation-and-interaction). É recomendável fazer o teste de acessibilidade de cada dica de ensino no gamepad com todas as configurações possíveis da interface do usuário do aplicativo.

Uma dica de ensino com alvo e PreferredPlacement definido como "BottomLeft" será exibida com a cauda centralizada na parte inferior do alvo. O corpo da dica de ensino ficará deslocado para a esquerda.

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

![Um aplicativo de exemplo com o botão "Salvar" como alvo de uma dica de ensino abaixo de seu canto esquerdo. O título da dica é "Salvar automaticamente" e o subtítulo é "Salvamos suas alterações conforme são feitas, para que você nunca precise fazê-lo". Há um botão Fechar no canto superior direito da dica de ensino.](../images/teaching-tip-targeted-preferred-placement.png)


Uma dica de ensino sem alvo com PreferredPlacement definido como "BottomLeft" será exibida no canto inferior esquerdo da raiz XAML.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft">
</controls:TeachingTip>
```

![Um aplicativo de exemplo com uma dica de ensino no canto inferior esquerdo. O título da dica é "Salvar automaticamente" e o subtítulo é "Salvamos suas alterações conforme são feitas, para que você nunca precise fazê-lo". Há um botão Fechar no canto superior direito da dica de ensino.](../images/teaching-tip-non-targeted-preferred-placement.png)

O diagrama a seguir mostra o resultado de todos os 13 modos PreferredPlacement que podem ser definidos nas dicas de ensino com alvo.
![Ilustração de 13 dicas de ensino, cada uma demonstrando um modo de posicionamento com alvo diferente. Cada dica de ensino é rotulada com o modo que representa. A primeira palavra de um modo de posicionamento indica o lado do alvo em que a dica de ensino aparecerá centralizada. A cauda da dica de ensino sempre ficará localizada no centro do lado do alvo e apontará para ele. Se houver uma segunda palavra no modo de posicionamento, o corpo da dica de ensino não ficará centralizado. Em vez disso, será deslocado para a direção especificada. Por exemplo, o modo de posicionamento "TopRight" fará com que a dica de ensino apareça acima do alvo e deslocada para a direita, com a cauda apontando para baixo, no centro da borda superior do alvo. Como o corpo está deslocado para a direita, a cauda fica na borda mais à esquerda do corpo da dica de ensino, que se estende para além da borda direita do alvo. O modo de posicionamento "Center" é único e fará com que a cauda da dica de ensino aponte para o centro do alvo. A dica de ensino ficará centralizada sobre a parte superior do alvo.](../images/teaching-tip-targeted-preferred-placement-modes.png)

O diagrama a seguir mostra o resultado de todos os 13 modos PreferredPlacement que podem ser definidos nas dicas de ensino sem alvo.
![Ilustração de nove dicas de ensino, cada uma demonstrando um modo de posicionamento sem alvo diferente. Cada dica de ensino é rotulada com o modo que representa. A primeira palavra de um modo de posicionamento indica o lado da raiz XAML em que a dica de ensino aparecerá centralizada. Se houver uma segunda palavra no modo de posicionamento, a dica de ensino se autoposicionará em direção ao canto especificado da raiz XAML. Por exemplo, o modo de posicionamento "TopRight" fará com que a dica de ensino apareça no canto superior direito da raiz XAML. Em todos os modos de posicionamento sem alvo, a ordem das duas palavras não afeta a posição. TopRight é equivalente a RightTop. O modo de posicionamento "Center" é único e fará com que a dica de ensino apareça no centro da raiz XAML, na vertical e na horizontal.](../images/teaching-tip-non-targeted-preferred-placement-modes.png)

### <a name="add-a-placement-margin"></a>Adicionar uma margem de posicionamento  

Com a propriedade PlacementMargin, é possível controlar a distância entre a dica de ensino com alvo e o alvo, bem como a distância entre uma dica de ensino sem alvo e as bordas da raiz XAML. Assim como [Margin](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.margin), PlacementMargin tem quatro valores – esquerda, direita, parte superior e parte inferior. Assim, somente os valores relevantes são usados. Por exemplo, PlacementMargin.Left é usado quando a dica está à esquerda do alvo ou da borda da raiz XAML.

O exemplo a seguir mostra uma dica sem alvo com todos os valores de PlacementMargin definidos como 80.

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

![Um aplicativo de exemplo com uma dica de ensino posicionada em direção ao canto inferior direito, mas não totalmente nessa posição. O título da dica é "Salvar automaticamente" e o subtítulo é "Salvamos suas alterações conforme são feitas, para que você nunca precise fazê-lo". Há um botão Fechar no canto superior direito da dica de ensino.](../images/teaching-tip-placement-margin.png)


### <a name="add-content"></a>Adicionar conteúdo

É possível adicionar conteúdo a uma dica de ensino usando a propriedade Content. Se o conteúdo a ser mostrado for maior que o tamanho permitido pela dica de ensino, uma barra de rolagem será automaticamente habilitada para que o usuário possa rolar a área de conteúdo. 

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

![Um aplicativo de exemplo com uma dica de ensino sobre o botão Salvar. O título da dica é "Salvar automaticamente" e o subtítulo é "Salvamos suas alterações conforme são feitas, para que você nunca precise fazê-lo". Na área do conteúdo da dica de ensino, há um CheckBox com o rótulo "Não mostrar dicas no início". Abaixo, há um texto que diz "Será possível alterar as preferências de dica nas Configurações se você mudar de ideia", em que "Configurações" é um link para a página de configurações do aplicativo. Há um botão Fechar no canto superior direito da dica de ensino.](../images/teaching-tip-content.png)

### <a name="add-buttons"></a>Adicionar botões

Por padrão, um botão de fechar "X" é mostrado ao lado do título de uma dica de ensino. O botão Fechar pode ser personalizado com a propriedade CloseButtonContent. Nesse caso, o botão é movido para a parte inferior da dica de ensino.

**Observação: o botão Fechar não aparece nas dicas habilitadas para light dismiss**

É possível adicionar um botão de ação personalizado configurando a propriedade ActionButtonContent e, opcionalmente, as propriedades ActionButtonCommand e ActionButtonCommandParameter.

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

![Um aplicativo de exemplo com uma dica de ensino sobre o botão Salvar. O título da dica é "Salvar automaticamente" e o subtítulo é "Salvamos suas alterações conforme são feitas, para que você nunca precise fazê-lo". Na área do conteúdo da dica de ensino, há um CheckBox com o rótulo "Não mostrar dicas no início". Abaixo, há um texto que diz "Será possível alterar as preferências de dica nas Configurações se você mudar de ideia", em que "Configurações" é um link para a página de configurações do aplicativo. Há dois botões na parte inferior da dica de ensino, um cinza na esquerda com a mensagem "Desabilitar" e um azul na direita com a mensagem "Entendi".](../images/teaching-tip-buttons.png)

### <a name="hero-content"></a>Conteúdo em destaque

É possível adicionar conteúdo de borda a borda a uma dica de ensino configurando a propriedade HeroContent. Para que o conteúdo em destaque fique na parte superior ou inferior da dica de ensino, configure a propriedade HeroContentPlacement.

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

![Um aplicativo de exemplo com uma dica de ensino sobre o botão Salvar. O título da dica é "Salvar automaticamente" e o subtítulo é "Salvamos suas alterações conforme são feitas, para que você nunca precise fazê-lo". Uma imagem de borda a borda de um desenho de um homem colocando arquivos na nuvem está na parte inferior da dica de ensino. Há um botão Fechar no canto superior direito da dica de ensino.](../images/teaching-tip-hero-content.png)

### <a name="add-an-icon"></a>Adicionar um ícone

É possível adicionar um ícone ao lado do título e do subtítulo usando a propriedade IconSource. É recomendável usar ícones nos tamanhos de 16px, 24px e 32px. 

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

![Um aplicativo de exemplo com uma dica de ensino sobre o botão Salvar. O título da dica é "Salvar automaticamente" e o subtítulo é "Salvamos suas alterações conforme são feitas, para que você nunca precise fazê-lo". É possível ver um ícone de disquete ao lado do título e do subtítulo. Há um botão Fechar no canto superior direito da dica de ensino.](../images/teaching-tip-icon.png)

### <a name="enable-light-dismiss"></a>Habilitar o light dismiss

A funcionalidade light dismiss fica desabilitada por padrão. No entanto, é possível habilitá-la para que a dica de ensino seja ignorada, por exemplo, quando o usuário rola a tela ou interage com outros elementos do aplicativo. Por causa desse comportamento, o light dismiss é a melhor solução quando uma dica precisa ser colocada em uma área rolável. 

O botão Fechar será automaticamente removido de uma dica de ensino habilitada para light dismiss, para que o comportamento seja identificado pelos usuários. 

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    IsLightDismissEnabled="True">
</controls:TeachingTip>
```

![Um aplicativo de exemplo com uma dica com light dismiss no canto inferior direito. O título da dica é "Salvar automaticamente" e o subtítulo é "Salvamos suas alterações conforme são feitas, para que você nunca precise fazê-lo".](../images/teaching-tip-light-dismiss.png)

### <a name="escaping-the-xaml-root-bounds"></a>Ultrapassar os limites da raiz XAML

No Windows versão 19H1 e posteriores, uma dica de ensino poderá ultrapassar os limites da raiz XAML e da tela se a propriedade ShouldConstrainToRootBounds for configurada. Quando essa propriedade está habilitada, a dica de ensino não tentará ficar dentro dos limites da raiz XAML nem da tela e sempre ficará posicionada no modo PreferredPlacement configurado. É recomendável habilitar a propriedade IsLightDismissEnabled e definir o modo PreferredPlacement mais próximo do centro da raiz XAML para garantir a melhor experiência para os usuários.

Em versões anteriores do Windows, essa propriedade era ignorada e a dica de ensino sempre ficava dentro dos limites da raiz XAML.

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

![Um aplicativo de exemplo com uma dica de ensino fora do canto inferior direito do aplicativo. O título da dica é "Salvar automaticamente" e o subtítulo é "Salvamos suas alterações conforme são feitas, para que você nunca precise fazê-lo". Há um botão Fechar no canto superior direito da dica de ensino.](../images/teaching-tip-escape-xaml-root.png)

### <a name="canceling-and-deferring-close"></a>Cancelar e adiar o fechamento

O evento Closing pode ser usado para cancelar e/ou adiar o fechamento de uma dica de ensino. É possível usá-lo para manter a dica de ensino aberta ou dar tempo para que uma ação ou animação personalizada ocorra. Quando o fechamento de uma dica de ensino é cancelado, IsOpen voltará a ser true. No entanto, ficará como false durante o adiamento. Um fechamento programático também pode ser cancelado. 

**Observação: se nenhuma opção de posicionamento permitir que a dica de ensino completa seja mostrada, ela iterará ao longo do ciclo de vida do evento para forçar o fechamento em vez de ser exibida sem um botão Fechar acessível. Se o aplicativo cancelar o evento Closing, a dica de ensino poderá permanecer aberta sem um botão Fechar acessível.**

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
* As dicas não são permanentes e não devem conter informações ou opções críticas para a experiência de um aplicativo. 
* Tente evitar mostrá-las com muita frequência. É mais provável que uma dica de ensino receba atenção individualizada quando está intercalada ao longo de sessões demoradas ou em várias sessões.    
* As dicas precisam ser breves e o tópico deve ser claro. Pesquisas mostram que, em média, os usuários só leem de três a cinco palavras e compreendem somente duas a três palavras antes de decidir se querem interagir com uma dica.
* A acessibilidade de uma dica de ensino no gamepad não é garantida. Para aplicativos que preveem entrada de gamepad, confira [Interações de gamepad e de controle remoto]( https://docs.microsoft.com/en-us/windows/uwp/design/input/gamepad-and-remote-interactions#xy-focus-navigation-and-interaction). É recomendável fazer o teste de acessibilidade de cada dica de ensino no gamepad com todas as configurações possíveis da interface do usuário do aplicativo.
* Ao habilitar uma dica de ensino para escapar da raiz XAML, é recomendável também habilitar a propriedade IsLightDismissEnabled e definir o modo PreferredPlacement mais próximo do centro da raiz XAML. 

### <a name="reconfiguring-an-open-teaching-tip"></a>Reconfigurar uma dica de ensino aberta

É possível reconfigurar alguns conteúdos e propriedades enquanto a dica de ensino está aberta. As alterações entrarão em vigor imediatamente. Outros conteúdos e propriedades, como a propriedade de ícone, os botões Ação e Fechar e a reconfiguração entre light dismiss e explicit dismiss exigirão que a dica de ensino seja fechada e reaberta para que alterações nessas propriedades entrem em vigor. Alterar o comportamento de ignorar de manual para light dismiss enquanto uma dica de ensino está aberta fará com que o botão Fechar seja removido antes que o comportamento de light dismiss seja habilitado e a dica pode ficar paralisada na tela.
