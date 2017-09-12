---
author: Karl-Bridge-Microsoft
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: 010663320b4011d853c53bc4121f86a14bfbfe0c
ms.sourcegitcommit: a2908889b3566882c7494dc81fa9ece7d1d19580
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2017
---
# <a name="custom-keyboard-interactions"></a>Interações por teclado personalizadas

Forneça experiências abrangentes e consistentes de interação por teclado em seus aplicativos UWP e controles personalizados, tanto para usuários avançados como para aqueles com deficiências e outras necessidades de acessibilidade.

Neste tópico, nosso foco principal é oferecer suporte a entradas de teclado com controles personalizados para aplicativos UWP em computadores. No entanto, uma experiência de teclado bem projetada é importante para dar suporte a ferramentas de acessibilidade, como o Narrador do Windows, utilizando teclados de software, como o teclado por toque e o teclado virtual (OSK).

## <a name="providing-2d-directional-inner-navigation-a-namexyfocuskeyboardnavigation"></a>Fornecer navegação interna direcional 2D <a name="xyfocuskeyboardnavigation">

Use a propriedade **XYFocusKeyboardNavigation** para dar suporte à navegação interna direcional 2D de controles personalizados e grupos de controle com teclado (teclas de seta), gamepad do Xbox (D-pad e botões do joystick esquerdo) e controle remoto do Xbox (D-pad).

**OBSERVAÇÃO** Nos referimos à região de navegação interna de um controle ou um grupo de controle como a *área direcional*.

**XYFocusKeyboardNavigation** possui um valor do tipo **XYFocusKeyboardNavigationMode** com os valores possíveis **Automático** (padrão), **Ativado** ou **Desativado**.

Essa propriedade não afeta a navegação por guias, afeta somente a navegação interna dos elementos filho dentro do seu controle ou grupo de controles. Elementos filho de uma área direcional não devem ser incluídos na navegação por guias.

### <a name="default-behavior"></a>Comportamento padrão

Comportamento de navegação direcional baseia-se nos ancestrais, ou hierarquia herdada, do elemento. Se todos os ancestrais estão no modo padrão, ou definidos como **Automático**, o comportamento de navegação direcional não possui suporte para teclado (gamepad e controle remoto sempre suportam a navegação direcional, a menos que seja explicitamente definido como **Desativado**).

### <a name="custom-behavior"></a>Comportamento personalizado

Definir essa propriedade como **Ativado** permite que seu controle suporte a navegação interna 2D (usando as teclas de seta do teclado) sobre cada UIElement dentro do seu controle.

Ao usar as teclas de seta do teclado, a navegação é restringida dentro da área direcional (pressionar a tecla tab muda o foco para o próximo elemento focalizável fora da área direcional).

**OBSERVAÇÃO** Esse não é o caso quando estiver usando um gamepad ou controle remoto, onde a navegação continua fora da área direcional até o próximo controle focalizável.

Essa propriedade afeta somente a navegação interna com as teclas de seta, ela não afeta a navegação com a tecla tab. Todos os controles mantêm sua hierarquia de ordem de guias esperada.

A imagem a seguir mostra três botões (A, B e C) contidos em uma área direcional, e um quarto botão (D) fora da área direcional.

![área direcional](images/keyboard/directional-area.png)

*Teclas de seta do teclado podem mover o foco entre os botões A-B-C, mas não o D*

O exemplo de código a seguir mostra como a navegação é afetada quando **XYFocusKeyboardNavigation** está habilitado. Usando a imagem anterior, A tem o foco inicial e a tecla tab circula por todos os controles (A -&gt; B -&gt; C -&gt; D, e volta para A) enquanto as teclas de seta estão limitadas à área direcional.

```XAML
<StackPanel Orientation="Horizontal">
      <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
            <Button Content="A" />
            <Button Content="B" />
            <Button Content="C" />
      </StackPanel>
      <Button Content="D" />
</StackPanel>
```

#### <a name="override-directional-navigation"></a>Substituir a navegação direcional

Use as propriedades XYFocusRight/XYFocusLeft/XYFocusTop/XYFocusDown properties para substituir os comportamentos de navegação padrões.

Aqui está a mesma imagem do exemplo anterior, mostrando os três botões (A, B e C) contidos em uma área direcional e o quarto botão (D) fora dela.

![área direcional](images/keyboard/directional-area.png)

*Teclas de seta do teclado podem mover o foco entre os botões A-B-C, e fora para o D*

Este exemplo de código demonstra como substituir o comportamento de navegação padrão para a tecla da seta Direita permitindo que ela navegue para um controle fora da área direcional. Observe que não é possível entrar novamente na área direcional usando a tecla da seta esquerda.

```XAML
<StackPanel Orientation="Horizontal">
      <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
            <Button Content="A" />
            <Button Content="B" />
            <Button Content="C" XYFocusRight="{x:Bind ButtonD}" />
      </StackPanel>
      <Button Content="D" x:Name="ButtonD"/>
</StackPanel>
```

Para obter mais detalhes, consulte [Estratégias de Navegação XYFocus](#set-the-tab-navigation-behavior) posteriormente neste tópico.

#### <a name="restrict-navigation-with-disabled"></a>Restringir a navegação no modo Desativado

Defina **XYFocusKeyboardNavigation** para **Desativado** para restringir a navegação por teclas de seta dentro de uma área direcional.

**OBSERVAÇÃO** Definir essa propriedade como Desativado não afeta a navegação para o controle em si, apenas os elementos filho do controle.

No exemplo de código a seguir, o pai StackPanel tem **XYFocusKeyboardNavigation** definida como **Ativado** e o elemento filho, C, tem **XYFocusKeyboardNavigation** definida como **Desativado**. Apenas os elementos filho de C têm a navegação por tecla desativada.

```XAML
<StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
        <Button Content="A" />
        <Button Content="B" />
        <Button Content="C" XYFocusKeyboardNavigation="Disabled" >
            ...
        </Button>
</StackPanel>
```

#### <a name="use-nested-directional-areas"></a>Usar áreas direcionais localizadas

É possível ter vários níveis de áreas direcionais localizadas. Se todos os elementos pai tem **XYFocusKeyboardNavigation** definida como **Ativado**, os limites de região são ignorados pela navegação com teclas de seta.

Aqui está uma imagem que mostra três botões (A, B e C) contidos em uma área direcional localizada, e um quarto botão (D) fora dela

![área direcional localizada](images/keyboard/nested-directional-area.png)

*Teclas de seta do teclado podem mover o foco entre os botões A-B-C, mas não o D*

Este exemplo de código demonstra como especificar que áreas direcionais localizadas suportam a navegação com teclas de seta sobre os limites de região.

```XAML
<StackPanel Orientation="Horizontal">
        <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation ="Enabled">
            <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation ="Enabled">
                <Button Content="A" />
                <Button Content="B" />
            </StackPanel>
            <Button Content="C" />
        </StackPanel>
        <Button Content="D" />
 </StackPanel>
```

Aqui está uma imagem que mostra quatro botões (A, B, C e D) onde A e B estão contidos dentro de uma área direcional localizada, e C e D estão contidos em uma área diferente. Como o elemento pai tem **XYFocusKeyboardNavigation** definida como **Desativado**, o limite de cada área localizada não pode ser ultrapassado utilizando as teclas de seta.

![área não direcional](images/keyboard/non-directional-area.png)

*Teclas de seta do teclado podem mover o foco entre os botões A e B e entre os botões C e D, mas não entre as regiões*

Este exemplo de código demonstra como especificar áreas direcionais localizadas que não suportam a navegação com teclas de seta sobre os limites de região.

```XAML
<StackPanel Orientation="Horizontal">
  <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
    <Button Content="A" />
    <Button Content="B" />
  </StackPanel>
  <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
    <Button Content="C" />
    <Button Content="D" />
  </StackPanel>
</StackPanel>
```

Aqui está um exemplo mais complexo de áreas direcionais localizadas onde:

-   se A tiver o foco, só é possível navegar para E (e vice-versa) pois existe um limite de área não direcional que torna B, C e D inacessíveis através das teclas de seta
-   se B tiver o foco, só é possível navegar para C (e vice-versa) pois D está fora da área direcional e o limite da área não direcional bloqueia o acesso à A e E
-   se D tem o foco, a tecla Tab deve ser usada para navegar entre os controles, uma vez que não é possível a navegação por teclas de seta

![área não direcional localizada](images/keyboard/nested-non-directional-area.png)

*Teclas de seta do teclado podem mover o foco entre os botões A e E, e entre os botões B e C, mas não entre outras regiões*

Este exemplo de código demonstra como especificar áreas direcionais localizadas que suportam a navegação complexa com teclas de seta sobre os limites de região.

```XAML
<StackPanel  Orientation="Horizontal" XYFocusKeyboardNavigation ="Enabled">
  <Button Content="A" />
    <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation ="Disabled">
      <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation ="Enabled">
        <Button Content="B" />
        <Button Content="C" />
      </StackPanel>
        <Button Content="D" />
    </StackPanel>
  <Button Content="E" />
</StackPanel>
```

## <a name="set-the-tab-navigation-behavior-a-nametab-navigation"></a>Definir o comportamento de navegação de guia <a name="tab-navigation">

A propriedade[TabFocusNavigation](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.Controls.Control.TabNavigation) do UIElement. especifica o comportamento de navegação de guia para toda a árvore de objeto (ou área direcional).

[TabFocusNavigation](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.Controls.Control.TabNavigation) possui um valor do tipo **TabFocusNavigationMode** com os valores possíveis **Uma única vez**, **Ciclo** **ou Local** (padrão).

Na imagem a seguir, dependendo da navegação de guia da área direcional, o foco é movido das seguintes maneiras:

-   Uma única vez: A, B1, C, A
-   Local: A, B1, B2, B3, B4, B5, C, A
-   Ciclo: A, B1, B2, B3, B4, B5, B1, B2, B3, B4, B5, (circulando entre os B's)

![navegação de guia](images/keyboard/tab-navigation.png)

*Comportamento do foco com base no modo de navegação de guia*

O exemplo de código a seguir demonstra o uso de TabFocusNavigation no modo Uma única vez.

```XAML
<Button Content="X" Click="OnAClick"/>
<StackPanel Orientation="Horizontal" XYFocusNavigation ="Local"
   TabFocusNavigation ="Local">
   <Button Content="A" Click="OnAClick"/>
   <StackPanel Orientation="Horizontal" TabFocusNavigation ="Once">
        <Button Content="B" Click="OnBClick"/>
        <Button Content="C" Click="OnCClick"/>
        <Button Content="D" Click="OnDClick"/>
   </StackPanel>
   <Button Content="E" Click="OnBClick"/>
</StackPanel>
```

*A Navegação por Guia quando o foco está em X é: A,B,E,X*

#### <a name="about-tabfocusnavigation-and-tabindex"></a>Sobre TabFocusNavigation e TabIndex

A propriedade UIElement.TabFocusNavigation possui o mesmo comportamento que a propriedade Control.TabNavigation, incluindo o funcionamento com TabIndex.

Quando um controle não tem possui TabIndex especificado, a estrutura atribui a ele um valor de índice maior que o maior valor de índice atual (e a prioridade mais baixa). Ele resolve a ambiguidade escolhendo o primeiro elemento na árvore visual. A estrutura resolve os índices de Guia por âmbito. Os filhos de um controle são considerados um âmbito, e se uma dessas crianças possuir filhos, estes fazem parte de outro âmbito.

Na imagem a seguir, dependendo da navegação de guia da área direcional e do índice de guia dos elementos, o foco é movido das seguintes maneiras:

-   Uma única vez: A, B3, C, A.
-   Local: A, B3, B4, B5, B1, B2, C, A.
-   Ciclo: A, B3, B4, B5, B1, B2, B3, B4, B5, B1, B2, (circulando entre os B’s)

![índice de guia](images/keyboard/tab-index.png)

*Comportamento de foco com base no modo de navegação de guia e índices de tabulação*

Observe como a área direcional é considerada um âmbito e como a navegação do foco se move para o controle com a maior prioridade primeiro: B3. Na verdade, existem dois âmbitos: um para A, Área Direcional e C. E outro para a Área direcional. Como a área direcional não é uma TabStop, a estrutura alterna o âmbito para procurar o melhor candidato, e então recursivamente através dos elementos filho.
