---
author: Xansky
Description: If your app does not provide good keyboard access, users who are blind or have mobility issues can have difficulty using your app or may not be able to use it at all.
ms.assetid: DDAE8C4B-7907-49FE-9645-F105F8DFAD8B
title: Acessibilidade do teclado
label: Keyboard accessibility
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 41c5e018ee56b6a0d26bf2159f62801aa4ab5c3c
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5763317"
---
# <a name="keyboard-accessibility"></a>Acessibilidade do teclado  



Se seu aplicativo não oferecer um bom acesso ao teclado, usuários cegos ou com problemas de mobilidade terão dificuldade para usá-lo ou não poderão usá-lo.

<span id="keyboard_navigation_among_UI_elements"/>
<span id="keyboard_navigation_among_ui_elements"/>
<span id="KEYBOARD_NAVIGATION_AMONG_UI_ELEMENTS"/>

## <a name="keyboard-navigation-among-ui-elements"></a>Navegação de teclado entre elementos de interface de usuário  
Para usar o teclado com um controle, o controle precisa ter foco e, para receber foco (sem usar um ponteiro), o controle precisa ser acessível em um projeto de interface de usuário da interface do usuário pela navegação por tabulação. Por padrão, a ordem de tabulação dos controles é a mesma ordem que eles foram adicionados à superfície de um projeto, listados em XAML ou adicionados programaticamente a um contêiner.

Na maioria dos casos, a ordem padrão baseada em como você define os controles no XAML é a melhor ordem, principalmente porque é a ordem de leitura dos leitores de tela. Contudo, a ordem padrão não corresponde necessariamente à ordem visual. A posição de exibição real poderia depender do contêiner de layout pai e de certas propriedades que você pode definir nos elementos filho para influenciar o layout. Para ter certeza de que seu aplicativo tem uma boa ordem de tabulação, teste esse comportamento. Principalmente se você tiver uma metáfora de grade ou de tabela em seu layout, a ordem de leitura dos usuários poderá ser diferente da ordem de tabulação. Nem sempre isso é um problema. No entanto, convém testar a funcionalidade do aplicativo como interface do usuário tanto de toque quanto acessível por teclado e verificar se ela responde igual para ambas as formas.

Você pode fazer com que a ordem de tabulação corresponda à ordem visual ajustando o XAML. Ou pode substituir a ordem de tabulação padrão definindo a propriedade [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461), conforme mostrado no exemplo a seguir de um layout de [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) que usa a navegação por guia de primeira coluna.

XAML
```xml
<!--Custom tab order.-->
<Grid>
  <Grid.RowDefinitions>...</Grid.RowDefinitions>
  <Grid.ColumnDefinitions>...</Grid.ColumnDefinitions>

  <TextBlock Grid.Column="1" HorizontalAlignment="Center">Groom</TextBlock>
  <TextBlock Grid.Column="2" HorizontalAlignment="Center">Bride</TextBlock>

  <TextBlock Grid.Row="1">First name</TextBlock>
  <TextBox x:Name="GroomFirstName" Grid.Row="1" Grid.Column="1" TabIndex="1"/>
  <TextBox x:Name="BrideFirstName" Grid.Row="1" Grid.Column="2" TabIndex="3"/>

  <TextBlock Grid.Row="2">Last name</TextBlock>
  <TextBox x:Name="GroomLastName" Grid.Row="2" Grid.Column="1" TabIndex="2"/>
  <TextBox x:Name="BrideLastName" Grid.Row="2" Grid.Column="2" TabIndex="4"/>
</Grid>
```

Você pode desejar excluir um controle da ordem de tabulação. Você normalmente faz isso apenas tornando o controle não interativo, por exemplo, definindo sua propriedade [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/BR209419) como **false**. Um controle desabilitado é automaticamente excluído da ordem de tabulação. Às vezes, você pode precisar excluir um controle da ordem de tabulação, mesmo que ele não esteja desabilitado. Nesse caso, você pode definir a propriedade [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/BR209422) como **false**.

Todos os elementos que podem ter o foco estão geralmente na ordem de tabulação por padrão. A exceção é que determinados tipos de exibição de texto, como [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565), podem ter foco para serem acessados pela área de transferência para seleção de texto; no entanto, eles não estão na ordem de tabulação porque elementos de texto estáticos não costumam estar na ordem de tabulação. Normalmente, eles não são interativos (não podem ser invocados nem requerem entrada de texto, mas dão suporte ao [Padrão de controle de texto](https://msdn.microsoft.com/library/windows/desktop/Ee671194) que aceita a localização e o ajuste de pontos de seleção no texto). O texto não deve ter a conotação de que a definição de um foco para ele ativará alguma ação que seja possível. Os elementos de texto ainda serão detectados por tecnologias adaptativas, e lerão em voz alta em leitores de tela, mas isso depende de técnicas diferentes da busca desses elementos na ordem de tabulação prática.

Independentemente de você ajustar os valores de [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) ou usar a ordem padrão, estas regras são aplicáveis:

* Elementos da interface do usuário com [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) igual a 0 são adicionados à ordem de tabulação com base na ordem de declaração no XAML ou nas coleções filhas.
* Elementos da interface do usuário com [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) maior que 0 são adicionados à ordem de tabulação com base no valor de **TabIndex**.
* Os elementos da interface do usuário com [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) menor que 0 são adicionados à ordem de tabulação e aparecem antes de qualquer valor zero. Isso é bem diferente de como o HTML manipula seu atributo **tabindex** (e não havia suporte a **tabindex** negativo nas especificações HTML antigas).

<span id="keyboard_navigation_within_a_UI_element"/>
<span id="keyboard_navigation_within_a_ui_element"/>
<span id="KEYBOARD_NAVIGATION_WITHIN_A_UI_ELEMENT"/>

## <a name="keyboard-navigation-within-a-ui-element"></a>Navegação de teclado entre elementos de interface do usuário  
Para os elementos compostos, é importante garantir uma navegação interior adequada entre os elementos contidos. Um elemento composto pode gerenciar seu filho ativo atual para reduzir a sobrecarga de ter todas os elementos filhos com capacidade de foco. Esse elemento composto é incluído na ordem de guias e trata os eventos de navegação de teclado sozinho. Muitos dos controles compostos já têm alguma lógica de navegação interna incorporada à manipulação de eventos do controle. Por exemplo, a passagem por seta-chave de itens é habilitada por padrão nos controles [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878), [**GridView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview), [**ListBox**](https://msdn.microsoft.com/library/windows/apps/BR242868) e [**FlipView**](https://msdn.microsoft.com/library/windows/apps/BR242678).

<span id="keyboard_activation"/>
<span id="KEYBOARD_ACTIVATION"/>

## <a name="keyboard-alternatives-to-pointer-actions-and-events-for-specific-control-elements"></a>Alternativas de teclado para ações do ponteiro e eventos para elementos de controle específico  
Permita que os elementos de interface de usuário que podem ser clicados também possam ser invocados usando o teclado. Para usar o teclado com um elemento da interface do usuário, o elemento deve ter foco. Somente as classes que derivam de [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) suportam o foco e a navegação de guias.

Para os elementos de interface do usuário que podem ser invocados, implemente manipuladores de eventos de teclado para as teclas de barra de espaço e Enter. Isso torna o suporte de acessibilidade básica de teclado completo e permite que os usuários utilizem cenários básicos de aplicativos apenas com o teclado, ou seja, os usuários podem acessar todos os elementos interativos da interface do usuário e ativar a funcionalidade padrão.

Em casos em que um elemento que você deseja usar na interface do usuário não pode ter foco, você pode criar seu próprio controle personalizado. Você deve definir a propriedade [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/BR209422) como **true** para habilitar o foco e deve fornecer uma indicação visual do estado do foco criando um estado visual que decora a interface do usuário com um indicador de foco. Contudo, é frequentemente mais fácil usar a composição de controle, para que o suporte para paradas de tabulação, foco e pares e padrões de Automação de Interface do Usuário da Microsoft possa ser tratados pelo controle no qual você escolhe compor o seu conteúdo.

Por exemplo, em vez de tratar um evento pressionado por ponteiro em um [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752), você poderia encapsular esse elemento em um [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) para conseguir suporte ao ponteiro, teclado e foco.

XAML
```xml
<!--Don't do this.-->
<Image Source="sample.jpg" PointerPressed="Image_PointerPressed"/>

<!--Do this instead.-->
<Button Click="Button_Click"><Image Source="sample.jpg"/></Button>
```

<span id="keyboard_shortcuts"/>
<span id="KEYBOARD_SHORTCUTS"/>

## <a name="keyboard-shortcuts"></a>Atalhos do teclado  
Além de implementar a navegação pelo teclado e ativação para o seu aplicativo, é recomendável implementar atalhos para a funcionalidade do seu aplicativo. A navegação por tabulação fornece um bom nível básico de suporte ao teclado, mas com formulários complexos, você poderia desejar adicionar suporte às teclas de atalho também. Isso pode tornar o seu aplicativo mais eficiente para usar, mesmo para as pessoas que usam dispositivos de teclado e de ponteiro.

Um *atalho* é uma combinação de teclas que aumenta a produtividade, fornecendo uma maneira eficiente de o usuário acessar a funcionalidade do aplicativo. Existem dois tipos de atalho:

* Uma *tecla de acesso* é um atalho para um elemento da interface do usuário no seu aplicativo. Teclas de acesso consistem na tecla Alt mais uma tecla de letra.
* Uma *tecla aceleradora* é um atalho para um comando do aplicativo. Seu aplicativo pode ter uma interface do usuário que corresponde exatamente ao comando. Teclas aceleradoras consistem na tecla Ctrl mais uma tecla de letra.

É fundamental oferecer aos usuários que dependem de leitores de tela e outras tecnologias assistenciais uma maneira fácil de descobrir as teclas de atalho do seu aplicativo. Comunique as teclas de atalho usando dicas de ferramentas, nomes acessíveis, descrições acessíveis ou alguma outra forma de comunicação na tela. No mínimo, as teclas de atalho devem ser bem documentadas no conteúdo de Ajuda do seu aplicativo.

Você pode documentar as teclas de acesso pelos leitores de tela definindo a propriedade anexada [**AutomationProperties.AccessKey**](https://msdn.microsoft.com/library/windows/apps/Hh759763) para uma cadeia de caracteres que descreve a tecla de atalho. Há também uma propriedade anexada [**AutomationProperties.AcceleratorKey**](https://msdn.microsoft.com/library/windows/apps/Hh759762) para documentar teclas de atalho não mnemônicas, apesar de leitores de tela geralmente tratarem as duas propriedades da mesma forma. Tente documentar as teclas de atalho de várias formas, usando dicas de ferramentas, propriedades de automação e documentação de Ajuda escrita.

O seguinte exemplo demonstra como documentar as teclas de atalho dos botões de reproduzir, pausar e parar mídia.

XAML
```xml
<Grid KeyDown="Grid_KeyDown">

  <Grid.RowDefinitions>
    <RowDefinition Height="Auto" />
    <RowDefinition Height="Auto" />
  </Grid.RowDefinitions>

  <MediaElement x:Name="DemoMovie" Source="xbox.wmv"
    Width="500" Height="500" Margin="20" HorizontalAlignment="Center" />

  <StackPanel Grid.Row="1" Margin="10"
    Orientation="Horizontal" HorizontalAlignment="Center">

    <Button x:Name="PlayButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+P"
      AutomationProperties.AcceleratorKey="Control P">
      <TextBlock>Play</TextBlock>
    </Button>

    <Button x:Name="PauseButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+A"
      AutomationProperties.AcceleratorKey="Control A">
      <TextBlock>Pause</TextBlock>
    </Button>

    <Button x:Name="StopButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+S"
      AutomationProperties.AcceleratorKey="Control S">
      <TextBlock>Stop</TextBlock>
    </Button>
  </StackPanel>
</Grid>
```

> [!IMPORTANT]
> A definição de [**AutomationProperties.AcceleratorKey**](https://msdn.microsoft.com/library/windows/apps/Hh759762) ou [**AutomationProperties.AccessKey**](https://msdn.microsoft.com/library/windows/apps/Hh759763) não habilita a funcionalidade de teclado. Ela só relata à estrutura de Automação da IU quais teclas devem ser usadas, para que essas informações possam ser repassadas aos usuários através de tecnologias assistivas. A implementação referente à manipulação de teclas ainda precisa ser feita em código, e não em XAML. Você ainda precisa anexar manipuladores para eventos [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/BR208941) ou [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/BR208942) no controle relevante para realmente implementar o comportamento de atalhos de teclado em seu aplicativo. Além disso, a decoração de texto sublinhado para uma tecla de acesso não é fornecida automaticamente. Você deve sublinhar explicitamente o texto para a tecla específica em seu mnemônico como formatação [**Underline**](https://msdn.microsoft.com/library/windows/apps/BR209982) embutida se desejar mostrar texto sublinhado na interface do usuário.

Para fins de simplicidade, o exemplo anterior omite o uso de recursos para cadeias de caracteres, como "Ctrl+A". Contudo, considere também as teclas de atalho durante o processo de localização. A localização de teclas de atalho é relevante porque a escolha da chave a ser usada como a chave de atalho normalmente depende do rótulo de texto visível para o elemento.

Para saber mais sobre as diretrizes de implementação de teclas de atalho, consulte [Teclas de atalho](http://go.microsoft.com/fwlink/p/?linkid=221825) nas Diretrizes de interação com a experiência do usuário do Windows.

<span id="Implementing_a_key_event_handler"/>
<span id="implementing_a_key_event_handler"/>
<span id="IMPLEMENTING_A_KEY_EVENT_HANDLER"/>

### <a name="implementing-a-key-event-handler"></a>Implementando um manipulador de eventos de chave  
Os eventos de entrada, como eventos de chave, usam um conceito de evento denominado *eventos roteados*. Um evento roteado pode emergir por meio de elementos filho de um controle composto, de tal forma que um pai de controle comum possa manipular eventos para vários elementos filhos. Esse modelo de evento é conveniente para definir ações de teclas de atalho para um controle que contém várias partes compostas que não podem ter foco ou fazer parte da ordem de tabulação.

Para obter um código de exemplo que mostra como escrever um manipulador de eventos de tecla que inclui a verificação de modificadores como a tecla Ctrl, consulte [Interações por teclado](https://msdn.microsoft.com/library/windows/apps/Mt185607).

<span id="Keyboard_navigation_for_custom_controls"/>
<span id="keyboard_navigation_for_custom_controls"/>
<span id="KEYBOARD_NAVIGATION_FOR_CUSTOM_CONTROLS"/>

## <a name="keyboard-navigation-for-custom-controls"></a>Navegação de teclado para controles personalizados  
Recomendamos o uso de teclas de direção como atalhos de teclado para navegar entre os elementos filho, nos casos onde os elementos filho têm uma relação espacial entre si. Se os nós de exibição de árvore tiverem subelementos separados para lidar com expansão e recolhimento e ativação de nós, use as teclas de seta para esquerda e direita para obter a funcionalidade de expansão e recolhimento. Se você tem um controle orientado que oferece suporte à passagem direcional dentro do conteúdo de controle, use as teclas de direção adequadas.

Geralmente, você implementa a manipulação de chave personalizada para controles personalizados incluindo uma substituição dos métodos [**OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/hh967982.aspx) e [**OnKeyUp**](https://msdn.microsoft.com/library/windows/apps/hh967983.aspx) como parte da lógica da classe.

<span id="An_example_of_a_visual_state_for_a_focus_indicator"/>
<span id="an_example_of_a_visual_state_for_a_focus_indicator"/>
<span id="AN_EXAMPLE_OF_A_VISUAL_STATE_FOR_A_FOCUS_INDICATOR"/>

## <a name="an-example-of-a-visual-state-for-a-focus-indicator"></a>Um exemplo do estado visual de um indicador de foco  
Mencionamos anteriormente que qualquer controle personalizado que habilita o usuário a focá-lo deve ter um indicador de foco visual. Normalmente esse indicador de foco é tão simples quanto desenhar uma forma de retângulo imediatamente ao redor do retângulo delimitador normal do controle. O [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) de foco visual é um elemento de par no restante da composição do controle em um modelo de controle, mas é definido inicialmente com um [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) valor de **Collapsed** porque o controle não tem foco ainda. Depois, quando o controle obtém o foco, é invocado um estado visual que define especificamente a **Visibility** do foco visual como **Visible**. Depois que o foco é movido para outro lugar, outro estado visual é chamado, e a **Visibility** se torna **Collapsed**.

Todos os controles XAML padrão exibem um indicador de foco visual adequado quando estão em foco (se puderem ser focalizados). Há também aparências potencialmente diferentes dependendo do tema escolhido do usuário (principalmente se o usuário estiver usando um modo de alto contraste.) Se estiver usando os controles XAML em sua interface do usuário e não substituir os modelos de controle, você não precisa fazer nada mais para obter os indicadores de foco visual nos controles que se comportam e exibem corretamente. Mas se você pretende criar um novo modelo para um controle ou se tiver curiosidade para saber como os controles XAML fornecem os indicadores de foco visual, o restante desta seção explica como isso é feito no XAML e na lógica de controle.

Aqui estão alguns exemplos de XAML provenientes do modelo XAML padrão para um [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265).

XAML
```xml
<ControlTemplate TargetType="Button">
...
    <Rectangle
      x:Name="FocusVisualWhite"
      IsHitTestVisible="False"
      Stroke="{ThemeResource FocusVisualWhiteStrokeThemeBrush}"
      StrokeEndLineCap="Square"
      StrokeDashArray="1,1"
      Opacity="0"
      StrokeDashOffset="1.5"/>
    <Rectangle
      x:Name="FocusVisualBlack"
      IsHitTestVisible="False"
      Stroke="{ThemeResource FocusVisualBlackStrokeThemeBrush}"
      StrokeEndLineCap="Square"
      StrokeDashArray="1,1"
      Opacity="0"
      StrokeDashOffset="0.5"/>
...
</ControlTemplate>
```

Até o momento, essa é apenas a composição. Para controlar a visibilidade do indicador do foco, defina os estados visuais que alternam a propriedade [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992). Para fazer isso, use a propriedade anexada [**VisualStateManager.VisualStateGroups**](https://msdn.microsoft.com/library/windows/apps/Hh738505), conforme aplicada ao elemento raiz que define a composição.

XAML
```xml
<ControlTemplate TargetType="Button">
  <Grid>
    <VisualStateManager.VisualStateGroups>
       <!--other visual state groups here-->
       <VisualStateGroup x:Name="FocusStates">
         <VisualState x:Name="Focused">
           <Storyboard>
             <DoubleAnimation
               Storyboard.TargetName="FocusVisualWhite"
               Storyboard.TargetProperty="Opacity"
               To="1" Duration="0"/>
             <DoubleAnimation
               Storyboard.TargetName="FocusVisualBlack"
               Storyboard.TargetProperty="Opacity"
               To="1" Duration="0"/>
         </VisualState>
         <VisualState x:Name="Unfocused" />
         <VisualState x:Name="PointerFocused" />
       </VisualStateGroup>
     <VisualStateManager.VisualStateGroups>
<!--composition is here-->
   </Grid>
</ControlTemplate>
```

Observe como somente um dos estados nomeados ajusta a [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) diretamente, enquanto os outros estão aparentemente vazios. A maneira como os estados visuais funcionam é que, tão logo o controle use outro estado da mesma classe [**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/BR209014), todas as animações aplicadas pelo estado anterior são canceladas imediatamente. Como a **Visibility** padrão da composição é **Collapsed**, isso significa que o retângulo não aparecerá. A lógica de controle controla isso escutando eventos de foco como [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/BR208927) e mudando os estados com [**GoToState**](https://msdn.microsoft.com/library/windows/apps/BR209025). Muitas vezes, isso já está manipulado para você, se você estiver usando um controle padrão ou personalizando com base em um controle que já tem esse comportamento.

<span id="Keyboard_accessibility_and_Windows_Phone"/>
<span id="keyboard_accessibility_and_windows_phone"/>
<span id="KEYBOARD_ACCESSIBILITY_AND_WINDOWS_PHONE"/>

## <a name="keyboard-accessibility-and-windows-phone"></a>Acessibilidade do teclado e o Windows Phone
Um dispositivo Windows Phone normalmente não tem um teclado de hardware dedicado. Porém, um SIP (Soft Input Panel) pode oferecer suporte a vários cenários de acessibilidade do teclado. Os leitores de tela podem ler entradas de texto no SIP **Texto**, incluindo exclusão de anúncios. Os usuários podem saber onde os seus dedos estão, pois o leitor de tela pode detectar se o usuário está usando as teclas e lê o nome da tecla pressionada em voz alta. Além disso, alguns dos conceitos de acessibilidade orientados pelo teclado podem ser mapeados com os comportamentos de tecnologia adaptativa relacionados que não usam teclado. Por exemplo, mesmo que um SIP não inclua uma tecla Tab, o Narrador reconhece um gesto de toque que é o equivalente a pressionar a tecla Tab, de modo que ter uma ordem de tabulação útil pelos controles em uma interface do usuário ainda é um princípio importante de acessibilidade. As teclas de setas que são usadas para navegar pelas partes dentro de controles complexos também são suportadas por meio de gestos de toque do Narrador. Quando o foco atinge um controle que não seja entrada de texto, o Narrador suporta um gesto que chama a ação desse controle.

Os atalhos do teclado não são normalmente relevantes para os aplicativos no Windows Phone porque um SIP não inclui as teclas Control ou Alt.

<span id="related_topics"/>

## <a name="related-topics"></a>Tópicos relacionados  
* [Acessibilidade](accessibility.md)
* [Interações por teclado](https://msdn.microsoft.com/library/windows/apps/Mt185607)
* [Amostra de teclado virtual](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)
* [Amostra de acessibilidade XAML](http://go.microsoft.com/fwlink/p/?linkid=238570)

