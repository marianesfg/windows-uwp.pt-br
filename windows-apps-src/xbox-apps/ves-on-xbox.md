---
title: Shell habilitado para voz (VES) no Xbox
description: Saiba como adicionar suporte de controle de voz aos seus aplicativos UWP no Xbox.
ms.date: 10/19/2017
ms.topic: article
keywords: Shell do Windows 10, UWP, Xbox, fala, habilitado para voz
ms.openlocfilehash: f51ec2c93a904893dc337545f634d04affde10fd
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685187"
---
# <a name="using-speech-to-invoke-ui-elements"></a>Usando a fala para invocar elementos da interface do usuário

O shell habilitado para voz (VES) é uma extensão da plataforma de fala do Windows que permite uma experiência de fala de primeira classe dentro de aplicativos, permitindo que os usuários usem a fala para invocar controles na tela e inserir texto por meio de ditado. O VES se esforça para fornecer uma experiência comum de ponta a ponta que diz respeito a ti em todos os shells e dispositivos do Windows, com o mínimo de esforço necessário dos desenvolvedores de aplicativos.  Para conseguir isso, ele aproveita a plataforma de fala da Microsoft e a estrutura de automação da interface do usuário (UIA).

## <a name="user-experience-walkthrough"></a>Guia de experiência do usuário ##
Veja a seguir uma visão geral do que um usuário teria ao usar o VES no Xbox, e ele deve ajudar a definir o contexto antes de mergulhar nos detalhes de como o VES funciona.

- O usuário ativa o console do Xbox e deseja navegar pelos seus aplicativos para encontrar algo de interesse:

        User: "Hey Cortana, open My Games and Apps"

- O usuário é deixado no modo de escuta ativo (ALM), o que significa que o console agora está escutando o usuário para invocar um controle visível na tela, sem a necessidade de dizer "Ei Cortana" a cada vez.  Agora, o usuário pode alternar para exibir aplicativos e percorrer a lista de aplicativos:

        User: "applications"

- Para rolar a exibição, o usuário pode simplesmente dizer:

        User: "scroll down"

- O usuário vê a arte da caixa para o aplicativo em que está interessado, mas esqueceu o nome.  O usuário solicita que os rótulos de dica de voz sejam exibidos:

        User: "show labels"

- Agora que está claro o que dizer, o aplicativo pode ser iniciado:

        User: "movies and TV"

- Para sair do modo de escuta ativo, o usuário informa ao Xbox para parar de escutar:

        User: "stop listening"

- Posteriormente, uma nova sessão de escuta ativa pode ser iniciada com:

        User: "Hey Cortana, make a selection" or "Hey Cortana, select"

## <a name="ui-automation-dependency"></a>Dependência de automação da interface do usuário ##
VES é um cliente de automação de interface do usuário e se baseia em informações expostas pelo aplicativo por meio de seus provedores de automação de interface do usuário. Essa é a mesma infraestrutura que já está sendo usada pelo recurso de narrador em plataformas Windows.  A automação da interface do usuário permite o acesso programático aos elementos da interface de usuários, incluindo o nome do controle, seu tipo e quais padrões de controle ele implementa.  À medida que a interface do usuário for alterada no aplicativo, VES reagirá a eventos de atualização UIA e reanalisará a árvore de automação da interface do usuário atualizada para localizar todos os itens acionáveis, usando essas informações para criar uma gramática de reconhecimento de fala. 

Todos os aplicativos UWP têm acesso à estrutura de automação da interface do usuário e podem expor informações sobre a interface do usuário independentemente da estrutura de gráficos com base na qual se baseiam (XAML, DirectX/Direct3D, Xamarin, etc.).  Em alguns casos, assim como o XAML, a maior parte do trabalho pesado é feita pela estrutura, reduzindo bastante os trabalhos necessários para dar suporte ao Narrator e ao VES.

Para obter mais informações sobre a automação da interface do usuário, consulte [conceitos básicos de automação da IU](https://msdn.microsoft.com/library/ms753107(v=vs.110).aspx "Fundamentos de automação da interface do usuário").

## <a name="control-invocation-name"></a>Nome de invocação de controle ##
O VES emprega o seguinte heurístico para determinar qual frase deve ser registrada com o reconhecedor de fala como o nome do controle (ou seja, o que o usuário precisa falar para invocar o controle).  Essa também é a frase que será exibida no rótulo da dica de voz.

Origem do nome em ordem de prioridade:

1. Se o elemento tiver uma propriedade anexada `LabeledBy`, VES usará o `AutomationProperties.Name` deste rótulo de texto.
2. `AutomationProperties.Name` do elemento.  Em XAML, o conteúdo de texto do controle será usado como o valor padrão para `AutomationProperties.Name`.
3. Se o controle for um ListItem ou Button, VES procurará o primeiro elemento filho com um `AutomationProperties.Name`válido.

## <a name="actionable-controls"></a>Controles acionáveis ##
VES considera um controle acionável se ele implementa um dos seguintes padrões de controle de automação:

- **InvokePattern** (por exemplo, Botão) – representa os controles que iniciam ou executam uma única ação não ambígua e não mantêm o estado quando ativados.

- **TogglePattern** (por exemplo, Caixa de seleção) – representa um controle que pode percorrer um conjunto de Estados e manter um estado uma vez definido.

- **SelectionItemPattern** (por exemplo, Caixa de combinação) – representa um controle que atua como um contêiner para uma coleção de itens filho selecionáveis.

- **ExpandCollapsePattern** (por exemplo, Caixa de combinação) – representa os controles que visualmente expandem para exibir conteúdo e recolher para ocultar o conteúdo.

- **ScrollPattern** (por exemplo, Lista) – representa os controles que atuam como contêineres roláveis para uma coleção de elementos filho.

## <a name="scrollable-containers"></a>Contêineres roláveis ##
Para contêineres roláveis que dão suporte a ScrollPattern, o VES escutará comandos de voz como "rolar para a esquerda", "rolar para a direita", etc. e invocará a rolagem com os parâmetros apropriados quando o usuário disparar um desses comandos.  Os comandos Scroll são injetados com base no valor das propriedades `HorizontalScrollPercent` e `VerticalScrollPercent`.  Por exemplo, se `HorizontalScrollPercent` for maior que 0, "rolar para a esquerda" será adicionado, se for menor que 100, "rolar para a direita" será adicionado e assim por diante.

## <a name="narrator-overlap"></a>Sobreposição do narrador ##
O aplicativo do narrador também é um cliente de automação da interface do usuário e usa a propriedade `AutomationProperties.Name` como uma das fontes para o texto que ele lê para o elemento da interface do usuário selecionado no momento.  Para fornecer uma experiência de acessibilidade melhor, muitos desenvolvedores de aplicativos Reclassificaram para sobrecarregar a propriedade `Name` com texto descritivo longo com o objetivo de fornecer mais informações e contexto quando lidos pelo narrador.  No entanto, isso causa um conflito entre os dois recursos: VES precisa de frases curtas que correspondam ou correspondam ao texto visível do controle, enquanto o narrador se beneficia de frases mais longas e mais descritivas para dar um melhor contexto.

Para resolver isso, começando com a atualização do Windows 10 para criadores, o Narrator foi atualizado para examinar também a propriedade `AutomationProperties.HelpText`.  Se essa propriedade não estiver vazia, o Narrator falará sobre o conteúdo além de `AutomationProperties.Name`.  Se `HelpText` estiver vazio, o Narrator só lerá o conteúdo do nome.  Isso permitirá que cadeias de caracteres mais descritivas sejam usadas quando necessário, mas mantém uma frase amigável de reconhecimento de fala mais curta na propriedade `Name`.

![](images/ves_narrator.jpg)

Para obter mais informações, consulte [Propriedades de automação para suporte de acessibilidade na interface do usuário](https://msdn.microsoft.com/library/ff400332(vs.95).aspx "Propriedades de automação para suporte de acessibilidade na interface do usuário").

## <a name="active-listening-mode-alm"></a>ALM (modo de escuta ativo) ##
### <a name="entering-alm"></a>Inserindo o ALM ###
No Xbox, o VES não está constantemente ouvindo a entrada de fala.  O usuário precisa inserir o modo de escuta ativo explicitamente dizendo:

- "Ei Cortana, Select" ou
- "Ei Cortana, fazer uma seleção"

Há vários outros comandos da Cortana que também deixam o usuário em escuta ativa após a conclusão, por exemplo, "Ei Cortana, entrar" ou "Ei Cortana, Go Home". 

A inserção do ALM terá o seguinte efeito:

- A sobreposição da Cortana será mostrada no canto superior direito, informando ao usuário que ele pode dizer o que eles veem.  Enquanto o usuário estiver falando, os fragmentos de frase reconhecidos pelo reconhecedor de fala também serão mostrados nesse local.
- VES analisa a árvore UIA, localiza todos os controles acionáveis, registra o texto na gramática do reconhecimento de fala e inicia uma sessão de escuta contínua.

    ![](images/ves_overlay.png)

### <a name="exiting-alm"></a>Saindo do ALM ###
O sistema permanecerá no ALM enquanto o usuário estiver interagindo com a interface de usuário usando voz.  Há duas maneiras de sair do ALM:

- O usuário diz explicitamente, "parar de escutar" ou
- Um tempo limite ocorrerá se não houver um reconhecimento positivo dentro de 17 segundos após a inserção do ALM ou desde o último reconhecimento positivo

## <a name="invoking-controls"></a>Invocando controles ##
Quando no ALM, o usuário pode interagir com a interface do usuário usando voz.  Se a interface do usuário estiver configurada corretamente (com propriedades de nome correspondentes ao texto visível), o uso de voz para executar ações deve ser uma experiência perfeita e natural.  O usuário deve ser capaz de apenas dizer o que vê na tela.

## <a name="overlay-ui-on-xbox"></a>Sobrepor interface do usuário no Xbox ##
O nome VES deriva para um controle pode ser diferente do texto visível real na interface do usuário.  Isso pode ser devido à propriedade `Name` do controle ou ao elemento `LabeledBy` anexado que está sendo explicitamente definido como uma cadeia de caracteres diferente.  Ou, o controle não tem texto de GUI, mas apenas um elemento Icon ou image.

Nesses casos, os usuários precisam de uma maneira de ver o que precisa ser dito para invocar esse controle.  Portanto, uma vez na escuta ativa, as dicas de voz podem ser exibidas dizendo "Mostrar rótulos".  Isso faz com que os rótulos da dica de voz apareçam na parte superior de cada controle acionável.

Há um limite de 100 rótulos, portanto, se a interface do usuário do aplicativo tiver mais controles acionáveis do que 100, haverá alguns que não terão rótulos de dica de voz mostrados.  Os rótulos que são escolhidos nesse caso não são determinísticos, pois depende da estrutura e da composição da interface do usuário atual como enumerados pela primeira vez na árvore UIA.

Depois que os rótulos de dica de voz são mostrados, não há nenhum comando para ocultá-los, eles permanecerão visíveis até que ocorra um dos seguintes eventos:

- o usuário invoca um controle
- o usuário navega para fora da cena atual
- o usuário diz "parar de escutar"
- o modo de escuta ativo atinge o tempo limite

## <a name="location-of-voice-tip-labels"></a>Local dos rótulos de dica de voz ##
Os rótulos de dicas de voz são horizontais e verticalmente centralizados dentro do BoundingRectangle do controle.  Quando os controles são pequenos e rigidamente agrupados, os rótulos podem se sobrepor/se tornarem obscuros por outros e o VES tentará enviar esses rótulos por push para separá-los e garantir que fiquem visíveis.  No entanto, não há garantia de que isso funcione 100% do tempo.  Se houver uma interface do usuário muito cheia, provavelmente fará com que alguns rótulos sejam obscurecidos por outras pessoas. Examine sua interface do usuário com "Mostrar rótulos" para garantir que haja espaço adequado para a visibilidade da dica de voz.

![](images/ves_labels.png)

## <a name="combo-boxes"></a>Caixas de combinação ##
Quando uma caixa de combinação é expandida, cada item individual na caixa de combinação obtém seu próprio rótulo de dica de voz e, muitas vezes, estará sobre os controles existentes por trás da lista suspensa.  Para evitar a apresentação de muddles e confusos de rótulos (onde os rótulos de item da caixa de combinação são combinados com os rótulos de controles por trás da caixa de combinação) quando uma caixa de combinação é expandida, somente os rótulos para seus itens filho serão mostrados;  todos os outros rótulos de dica de voz ficarão ocultos.  O usuário pode selecionar um dos itens suspensos ou "fechar" a caixa de combinação.

- Rótulos em caixas de combinação recolhidas:

    ![](images/ves_combo_closed.png)

- Rótulos na caixa de combinação expandida:

    ![](images/ves_combo_open.png)


## <a name="scrollable-controls"></a>Controles roláveis ##
Para controles roláveis, as dicas de voz para os comandos de rolagem serão centralizadas em cada uma das bordas do controle.  As dicas de voz só serão mostradas para as direções de rolagem que são acionáveis, por exemplo, se a rolagem vertical não estiver disponível, "rolar para cima" e "rolar para baixo" não será mostrado.  Quando várias regiões roláveis estiverem presentes, o VES usará ordinais para diferenciá-las (por exemplo, "Rolar para a direita 1", "rolar para a direita 2", etc.).

![](images/ves_scroll.png) 

## <a name="disambiguation"></a>Desambiguidade ##
Quando vários elementos da interface do usuário têm o mesmo nome ou o reconhecedor de fala corresponde a vários candidatos, o VES entrará no modo de desambiguação.  Neste modo, os rótulos de dica de voz serão mostrados para os elementos envolvidos para que o usuário possa selecionar o correto. O usuário pode cancelar o modo de desambiguação dizendo "Cancelar".

Por exemplo:

- No modo de escuta ativo, antes da desambiguidade; o usuário diz "sou ambíguo":

    ![](images/ves_disambig1.png) 

- Ambos os botões correspondidos; desambiguidade iniciada:

    ![](images/ves_disambig2.png) 

- Mostrando a ação de clique quando "Select 2" foi escolhido:

    ![](images/ves_disambig3.png) 
 
## <a name="sample-ui"></a>Interface de usuário de exemplo ##
Aqui está um exemplo de uma interface do usuário baseada em XAML, definindo o AutomationProperties.Name de várias maneiras:

    <Page
        x:Class="VESSampleCSharp.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:VESSampleCSharp"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">
        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <Button x:Name="button1" Content="Hello World" HorizontalAlignment="Left" Margin="44,56,0,0" VerticalAlignment="Top"/>
            <Button x:Name="button2" AutomationProperties.Name="Launch Game" Content="Launch" HorizontalAlignment="Left" Margin="44,106,0,0" VerticalAlignment="Top" Width="99"/>
            <TextBlock AutomationProperties.Name="Day of Week" x:Name="label1" HorizontalAlignment="Left" Height="22" Margin="168,62,0,0" TextWrapping="Wrap" Text="Select Day of Week:" VerticalAlignment="Top" Width="137"/>
            <ComboBox AutomationProperties.LabeledBy="{Binding ElementName=label1}" x:Name="comboBox" HorizontalAlignment="Left" Margin="310,57,0,0" VerticalAlignment="Top" Width="120">
                <ComboBoxItem Content="Monday" IsSelected="True"/>
                <ComboBoxItem Content="Tuesday"/>
                <ComboBoxItem Content="Wednesday"/>
                <ComboBoxItem Content="Thursday"/>
                <ComboBoxItem Content="Friday"/>
                <ComboBoxItem Content="Saturday"/>
                <ComboBoxItem Content="Sunday"/>
            </ComboBox>
            <Button x:Name="button3" HorizontalAlignment="Left" Margin="44,156,0,0" VerticalAlignment="Top" Width="213">
                <Grid>
                    <TextBlock AutomationProperties.Name="Accept">Accept Offer</TextBlock>
                    <TextBlock Margin="0,25,0,0" Foreground="#FF5A5A5A">Exclusive offer just for you</TextBlock>
                </Grid>
            </Button>
        </Grid>
    </Page>


Usar o exemplo acima é a aparência da interface do usuário com e sem rótulos de dicas de voz.
 
- No modo de escuta ativo, sem rótulos mostrados:

    ![](images/ves_alm_nolabels.png) 

- No modo de escuta ativo, depois que o usuário diz "Mostrar rótulos":

    ![](images/ves_alm_labels.png) 

No caso de `button1`, o XAML preenche automaticamente a propriedade `AutomationProperties.Name` usando texto do conteúdo de texto visível do controle.  É por isso que há um rótulo de dica de voz, embora não haja um conjunto de `AutomationProperties.Name` explícito.

Com `button2`, definimos explicitamente o `AutomationProperties.Name` como algo diferente do texto do controle.

Com `comboBox`, usamos a propriedade `LabeledBy` para fazer referência a `label1` como a origem do `Name`de automação e, em `label1`, definimos o `AutomationProperties.Name` como uma frase mais natural do que é renderizado na tela ("dia da semana" em vez de "selecionar dia da semana").

Por fim, com `button3`, VES captura o `Name` do primeiro elemento filho, pois `button3` ele mesmo não tem um conjunto de `AutomationProperties.Name`.

## <a name="see-also"></a>Veja também
- [Conceitos básicos de automação da interface do usuário](https://msdn.microsoft.com/library/ms753107(v=vs.110).aspx "Fundamentos de automação da interface do usuário")
- [Propriedades de automação para suporte de acessibilidade na interface do usuário](https://msdn.microsoft.com/library/ff400332(vs.95).aspx "Propriedades de automação para suporte de acessibilidade na interface do usuário")
- [Perguntas frequentes](frequently-asked-questions.md)
- [UWP no Xbox One](index.md)
