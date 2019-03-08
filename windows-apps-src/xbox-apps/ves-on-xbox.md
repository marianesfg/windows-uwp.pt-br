---
title: Voz habilitada Shell (VES) no Xbox
description: Saiba como adicionar suporte a controle de voz para seus aplicativos UWP do Xbox.
ms.date: 10/19/2017
ms.topic: article
keywords: Windows 10, uwp, xbox, fala, shell habilitado para voz
ms.openlocfilehash: ea51216c804754e98c3bac459b79fb75dd9369cc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596051"
---
# <a name="using-speech-to-invoke-ui-elements"></a>Usando a fala para invocar os elementos de interface do usuário

Voz habilitada Shell (VES) é uma extensão para o Windows plataforma de fala que permite uma experiência de primeira classe fala dentro de aplicativos, permitindo que os usuários usar a fala para invocar na tela controles e a inserção de texto por meio de ditado. VES se esforça para fornecer uma experiência comum de consulte-it-it say de ponta a ponta em todos os Shells do Windows e dispositivos, com o mínimo esforço exigido dos desenvolvedores de aplicativo.  Para fazer isso, ele aproveita a plataforma de fala da Microsoft e a estrutura de automação de interface do usuário (UIA).

## <a name="user-experience-walkthrough"></a>Instruções passo a passo de experiência do usuário ##
Veja a seguir uma visão geral da qual um usuário experiência ao usar VES Xbox e ele deve ajudar a definir o contexto antes de se aprofundar nos detalhes de como funciona a VES.

- Usuário liga o console Xbox e deseja procurar por meio de seus aplicativos para encontrar algo interessante:

        User: "Hey Cortana, open My Games and Apps"

- Usuário é deixado no Active Directory escutando modo (ALM), que significa que o console agora está escutando para o usuário invocar um controle que está visível na tela, sem a necessidade de dizer: "Ei Cortana" cada vez.  Agora, o usuário pode alternar para exibir aplicativos e percorra a lista de aplicativos:

        User: "applications"

- Para rolar a exibição, o usuário pode simplesmente dizer:

        User: "scroll down"

- Usuário vê a arte da caixa para o aplicativo está interessado, mas esqueceu o nome.  Usuário solicita para rótulos de dica de voz a ser exibido:

        User: "show labels"

- Agora que fique claro o que quer dizer, o aplicativo pode ser iniciado:

        User: "movies and TV"

- Para sair do modo de escuta ativo, o usuário diz ao Xbox para parar de escutar:

        User: "stop listening"

- No futuro, uma nova sessão de escuta ativa pode ser iniciada com:

        User: "Hey Cortana, make a selection" or "Hey Cortana, select"

## <a name="ui-automation-dependency"></a>Dependência de automação de interface do usuário ##
VES é um cliente de automação de interface do usuário e se baseia em informações expostas pelo aplicativo por meio de seus provedores de automação de interface do usuário. Isso é a mesma infraestrutura que já está sendo usada pelo recurso de Narrador em plataformas Windows.  Automação de interface do usuário permite acesso programático aos elementos de interface do usuário, incluindo o nome do controle, seu tipo e padrões de controle de que ele implementa.  A interface do usuário que as alterações no aplicativo, o VES reagir a eventos de atualização UIA e analisar novamente a árvore de automação de interface do usuário atualizada para localizar todos os itens acionáveis, usando essas informações para criar uma gramática de reconhecimento de fala. 

Todos os aplicativos UWP têm acesso para a estrutura de automação de interface do usuário e podem expor informações sobre a interface do usuário independente de qual estrutura de elementos gráficos que eles são integrados ao (XAML, DirectX/Direct3D, Xamarin, etc.).  Em alguns casos, como o XAML, a maioria do trabalho pesado é feita pela estrutura, reduzindo significativamente o trabalho necessário para dar suporte ao Narrador e VES.

Para obter mais informações sobre a automação de interface do usuário, consulte [fundamentos de automação de interface do usuário](https://msdn.microsoft.com/en-us/library/ms753107(v=vs.110).aspx "fundamentos de automação de interface do usuário").

## <a name="control-invocation-name"></a>Nome do controle de invocação ##
VES emprega o heurístico a seguir para determinar o que a frase para registrar com o reconhecedor de fala como o nome do controle (ie. o que o usuário precisa falar para invocar o controle).  Isso também é a frase que será exibido no rótulo de dica de voz.

Fonte do nome em ordem de prioridade:

1. Se o elemento tem um `LabeledBy` propriedade anexada, VES usará o `AutomationProperties.Name` deste rótulo de texto.
2. `AutomationProperties.Name` o elemento.  No XAML, o conteúdo de texto do controle será usado como o valor padrão para `AutomationProperties.Name`.
3. Se o controle for um botão ou item de lista, VES procurará o primeiro elemento filho com uma validade `AutomationProperties.Name`.

## <a name="actionable-controls"></a>Controles acionáveis ##
VES considera acionáveis se ele implementa os seguintes padrões de controle de automação um controle:

- **InvokePattern** (eg. Botão)-representa os controles que iniciam ou executam uma ação única não ambígua e não mantêm o estado quando ativado.

- **TogglePattern** (por exemplo. Caixa de seleção) – representa um controle que pode percorrer um conjunto de estados e manter um estado definido uma vez.

- **SelectionItemPattern** (por exemplo. Caixa de combinação) – representa um controle que atua como um contêiner para uma coleção de itens filhos selecionáveis.

- **ExpandCollapsePattern** (eg. Caixa de combinação) - representa os controles que visualmente expandem para exibir o conteúdo e recolher para ocultar o conteúdo.

- **ScrollPattern** (por exemplo. Lista) – representa controles que atuam como contêineres roláveis para uma coleção de elementos filho.

## <a name="scrollable-containers"></a>Contêineres roláveis ##
Para contêineres roláveis que dão suporte que a ScrollPattern, VES escutará de voz comandos como "rolagem esquerda", "rolar para a direita", etc. e invocará rolagem com os parâmetros apropriados quando o usuário aciona um desses comandos.  Comandos de rolagem são inseridos com base no valor da `HorizontalScrollPercent` e `VerticalScrollPercent` propriedades.  Por exemplo, se `HorizontalScrollPercent` é maior que 0, "à esquerda rolagem" será adicionado, se ele for menor que 100, "rolar para a direita" será adicionado e assim por diante.

## <a name="narrator-overlap"></a>Sobreposição de Narrador ##
O aplicativo do Narrador também é um cliente de automação de interface do usuário e usa o `AutomationProperties.Name` a propriedade como uma das fontes para o texto que ele lê para o elemento de interface do usuário atualmente selecionado.  Para proporcionar uma melhor experiência de acessibilidade aplicativo muitos desenvolvedores recorreram à sobrecarga de `Name` propriedade com um texto descritivo longa com o objetivo de fornecer mais informações e contexto quando lida pelo Narrador.  No entanto, isso causa um conflito entre os dois recursos: VES precisa frases curtas que coincidam com ou em conjunto com o texto visível do controle, enquanto os benefícios do Narrador de frases mais descritivos, mais longa para dar um melhor contexto.

Para resolver esse problema, começando com o Windows 10 Creators Update, o Narrador foi atualizado para também examinar o `AutomationProperties.HelpText` propriedade.  Se essa propriedade não estiver vazia, o Narrador falará seu conteúdo além `AutomationProperties.Name`.  Se `HelpText` está vazio, Narrador só lê o conteúdo do nome.  Isso permitirá que mais descritivas cadeias de caracteres a ser usado quando for necessário, mas mantém uma frase de amigável de reconhecimento mais curta, do fala no `Name` propriedade.

![](images/ves_narrator.jpg)

Para obter mais informações, consulte [propriedades de automação para suporte de acessibilidade na interface do usuário](https://msdn.microsoft.com/en-us/library/ff400332(vs.95).aspx "propriedades de automação para suporte de acessibilidade na interface do usuário").

## <a name="active-listening-mode-alm"></a>Modo de escuta ativo (ALM) ##
### <a name="entering-alm"></a>Inserir o ALM ###
Xbox, VES não está escutando constantemente para entrada de fala.  O usuário precisa entrar no modo de escuta ativo explicitamente dizendo:

- "Ei Cortana, selecione", ou
- "Ei Cortana, faça uma seleção de"

Há vários outros comandos do Cortana que também deixam o usuário no Active Directory escutando após a conclusão, por exemplo, "Ei Cortana, de entrada" ou "Ei Cortana, ir para página inicial". 

Inserir ALM terá o seguinte efeito:

- A sobreposição de Cortana aparecerá no canto superior direito, informando ao usuário dizer o que eles veem.  Enquanto o usuário está falando, fragmentos de frase que são reconhecidos pelo reconhecedor de fala também serão mostrados neste local.
- VES analisa a árvore UIA, localiza todos os controles acionáveis, registra seu texto na gramática de reconhecimento de fala e inicia uma sessão de escuta contínua.

    ![](images/ves_overlay.png)

### <a name="exiting-alm"></a>Saindo do ALM ###
O sistema permanecerá no ALM, enquanto o usuário está interagindo com a interface do usuário usando a voz.  Há duas maneiras para sair do ALM:

- O usuário explicitamente diz "parar de escutar", ou
- Um tempo limite ocorrerá se não houver um reconhecimento positivo dentro de inserção de ALM ou desde o último reconhecimento positivo e 17 segundos

## <a name="invoking-controls"></a>Chamando controles ##
Quando estiver no ALM que o usuário pode interagir com a interface do usuário usando a voz.  Se a interface do usuário está configurado corretamente (com propriedades de nome corresponde ao texto visível), usando a voz para executar ações deve ser uma experiência perfeita de natural.  O usuário deve ser capaz de dizer o que vê na tela.

## <a name="overlay-ui-on-xbox"></a>Sobreposição de interface do usuário do Xbox ##
O nome VES deriva de um controle pode ser diferente do que o texto real visível na interface do usuário.  Isso pode ser devido à `Name` propriedade do controle ou o anexo `LabeledBy` elemento a ser definido explicitamente como cadeia de caracteres diferente.  Ou, o controle não tem texto GUI, mas apenas um elemento de imagem ou ícone.

Nesses casos, os usuários precisam de uma maneira de ver o que precisa ser dito para invocar um controle desse tipo.  Portanto, uma vez no Active Directory escutando, dicas de voz podem ser exibidas dizendo "Mostrar rótulos".  Isso faz com que voz rótulos dica apareça na parte superior de cada controle acionável.

Há um limite de 100 rótulos, portanto, se a interface do usuário do aplicativo tem controles mais acionáveis que 100 haverá algumas que não terão rótulos de dica de voz mostrados.  Quais rótulos são escolhidos nesse caso, não é determinístico, pois ele depende da estrutura e a composição da interface do usuário atual como inicialmente enumerados na árvore UIA.

Depois que os rótulos de dica de voz são mostrados, não há nenhum comando para ocultá-los, eles permanecerão visíveis até que um dos seguintes eventos ocorrer:

- o usuário invoca um controle
- usuário navega para fora a cena atual
- o usuário diz "parar de escutar"
- modo de escuta ativo atinge o tempo limite

## <a name="location-of-voice-tip-labels"></a>Local dos rótulos de dica de voz ##
Rótulos de dica de voz são horizontalmente e verticalmente centralizados dentro BoundingRectangle do controle.  Quando os controles são pequenos e estreitamente agrupado, os rótulos podem se tornar sobreposição/obscurecida por outras pessoas e VES tentará enviar esses rótulos separadamente para separá-los e verifique se eles estão visíveis.  No entanto, isso não é garantido para trabalhar 100% do tempo.  Se houver uma interface do usuário bastante lotado, provavelmente irá resultar em alguns rótulos seja obscurecidos por outros. Examine sua interface do usuário com "Mostrar rótulos de" para garantir que haja espaço adequado para a visibilidade de dica de voz.

![](images/ves_labels.png)

## <a name="combo-boxes"></a>Caixas de combinação ##
Quando uma caixa de combinação é expandida em cada item individual na caixa de combinação obtém seu próprio rótulo de dica de voz e muitas vezes eles estarão no topo de controles existentes por trás da lista suspensa.  Para evitar a apresentação de um Torne cheio e confuso de rótulos (onde rótulos de item de caixa de combinação são misturados com os rótulos dos controles por trás de caixa de combinação) quando uma caixa de combinação é expandida somente os rótulos para seus itens filho serão mostrados;  todos os outros rótulos de dica de voz serão ocultados.  O usuário pode, em seguida, selecione um dos itens de lista suspensa ou caixa de combinação de "Fechar".

- Rótulos em caixas de combinação recolhida:

    ![](images/ves_combo_closed.png)

- Rótulos de caixa de combinação expandido:

    ![](images/ves_combo_open.png)


## <a name="scrollable-controls"></a>Controles roláveis ##
Para controles roláveis, as dicas de voz para os comandos de rolagem serão centralizadas em cada uma das bordas do controle.  Dicas de voz só serão mostradas para as instruções de rolagem que são acionáveis, por exemplo, se a rolagem vertical não estiver disponível, "Role para cima" e "role" não será mostrada.  Quando houver várias regiões roláveis VES usará ordinais para diferenciá-las (por exemplo. "Role para a direita 1", "Role 2 à direita", etc.).

![](images/ves_scroll.png) 

## <a name="disambiguation"></a>Desambiguidade ##
Quando vários elementos de interface do usuário têm o mesmo nome ou o reconhecedor de fala correspondido vários candidatos, VES entrará no modo de desambiguidade.  Esta dica de voz de modo rótulos serão mostrados para os elementos envolvidos para que o usuário pode selecionar o correto. O usuário pode cancelar fora do modo de Desambiguidade dizendo "Cancelar".

Por exemplo:

- No modo de escuta ativo, antes de Desambiguidade; usuário diz: "Estou I ambíguas":

    ![](images/ves_disambig1.png) 

- Ambos os botões correspondidos; Introdução de Desambiguidade:

    ![](images/ves_disambig2.png) 

- Mostrando clique em ação quando "Selecionar 2" foi escolhido:

    ![](images/ves_disambig3.png) 
 
## <a name="sample-ui"></a>Exemplo de interface do usuário ##
Aqui está um exemplo de um XAML com base em interface do usuário, definindo o AutomationProperties.Name de várias maneiras:

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


Usando o exemplo acima aqui é a aparência de interface do usuário com e sem rótulos de dica de voz.
 
- No Active Directory escutando modo, sem rótulos mostrados:

    ![](images/ves_alm_nolabels.png) 

- No Active Directory escutando modo, depois que o usuário disser "Mostrar rótulos":

    ![](images/ves_alm_labels.png) 

No caso de `button1`, popula automaticamente XAML o `AutomationProperties.Name` propriedade usando o texto do conteúdo de texto visível do controle.  Isso ocorre porque há um rótulo de dica de voz, mesmo que não existe uma explícita `AutomationProperties.Name` definido.

Com o `button2`, definimos explicitamente o `AutomationProperties.Name` como algo diferente o texto do controle.

Com `comboBox`, usamos o `LabeledBy` propriedade a referenciar `label1` como a origem da automação `Name`e, na `label1` definimos o `AutomationProperties.Name` para uma frase mais natural que o que é renderizado na tela ("Dia da semana" em vez disso que "Selecione dia da semana").

Por fim, com `button3`, VES capta a `Name` do primeiro elemento filho desde `button3` em si não tem um `AutomationProperties.Name` definido.

## <a name="see-also"></a>Consulte também
- [Fundamentos de automação de interface do usuário](https://msdn.microsoft.com/en-us/library/ms753107(v=vs.110).aspx "fundamentos de automação de interface do usuário")
- [Propriedades de automação para suporte de acessibilidade na interface do usuário](https://msdn.microsoft.com/en-us/library/ff400332(vs.95).aspx "propriedades de automação para suporte de acessibilidade na interface do usuário")
- [Perguntas frequentes](frequently-asked-questions.md)
- [UWP no Xbox One](index.md)
