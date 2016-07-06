---
author: jwmsft
description: "Descrevemos o conceito de programação de eventos em um aplicativo do Windows Runtime quando você usa as extensões de componente (C++/CX) C#, Visual Basic ou Visual C++ como linguagem de programação e XAML para a definição da interface do usuário."
title: "Visão geral de eventos e eventos roteados"
ms.assetid: 34C219E8-3EFB-45BC-8BBD-6FD937698832
translationtype: Human Translation
ms.sourcegitcommit: 36bc5dcbefa6b288bf39aea3df42f1031f0b43df
ms.openlocfilehash: 4e4e21789dd76ad691f3828d23c73adcfc31efdf

---

# Visão geral de eventos e eventos roteados

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**APIs importantes**
-   [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)
-   [**RoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br208809)

Descrevemos o conceito de programação de eventos em um aplicativo do Windows Runtime quando você usa as extensões de componente (C++/CX) C#, Visual Basic ou Visual C++ como linguagem de programação e XAML para a definição da interface do usuário. Você pode atribuir manipuladores de eventos como parte das declarações para elementos da interface do usuário em XAML ou pode adicionar manipuladores no código. O Windows Runtime dá suporte a *eventos roteados*: determinados eventos de entrada e eventos de dados podem ser manipulados por outros objetos além do objeto que acionou o evento. Eventos roteados são úteis quando você define modelos de controle ou usa páginas ou contêiners de layout.

## Eventos como um conceito de programação

De um modo geral, os conceitos de eventos na programação de um aplicativo do Windows Runtime são semelhantes ao modelo de evento nas linguagens de programação mais usadas. Se você sabe como trabalhar com eventos do Microsoft .NET ou C++, já começa em vantagem. Você não precisa saber muito sobre os conceitos de modelo de evento para executar algumas tarefas básicas – como, por exemplo, anexar manipuladores.

Quando você usa C#, Visual Basic ou C++/CX como linguagem de programação, a interface do usuário é definida na marcação (XAML). Na sintaxe de marcação XAML, alguns dos princípios de conexão de eventos entre elementos de marcação e entidades de código em tempo de execução são similares a outras tecnologias da Web, tais como ASP.NET ou HTML5.

**Observação**  O código que fornece a lógica do tempo de execução para uma interface do usuário definida em XAML costuma ser chamado de *code-behind* ou arquivo code-behind. Nas visualizações de soluções do Microsoft Visual Studio, essa relação é mostrada graficamente, com o arquivo code-behind sendo um arquivo dependente e aninhado em contraposição à página XAML à qual ele se refere.

## Button.Click: uma introdução a eventos e XAML

Uma das tarefas de programação mais comuns para um aplicativo do Tempo de Execução do Windows é a captura de entrada do usuário na interface do usuário. Por exemplo, a sua interface do usuário pode ter um botão em que o usuário deve clicar para enviar informações ou mudar o estado.

Você define a interface do usuário para o seu aplicativo do Windows Runtime gerando a XAML. Esse XAML é geralmente o resultado de uma superfície de design no Visual Studio. A XAML também pode ser escrita em um editor de texto sem formatação ou em um editor XAML de terceiros. Durante a geração dessa XAML, você pode conectar manipuladores de eventos de elementos individuais da interface do usuário e, ao mesmo tempo, definir todos os demais atributos que estabelecem valores de propriedade desse elemento da interface do usuário.

Para conectar os eventos em XAML, você pode especificar o nome em forma de cadeia de caracteres do método do manipulador que já definiu ou vai definir posteriormente no code-behind. Por exemplo, esta XAML define um objeto [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) com outras propriedades ([atributo x:Name](x-name-attribute.md), [**Content**](https://msdn.microsoft.com/library/windows/apps/br209366)) atribuídas como atributos e transfere um manipulador para o evento [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) do botão referenciando um método chamado `showUpdatesButton_Click`:

```XML
<Button x:Name="showUpdatesButton"
  Content="{Binding ShowUpdatesText}"
  Click="showUpdatesButton_Click"/>
```

**Dica**
            *Conexão de eventos* é um termo de programação. Refere-se ao processo ou código por meio do qual você indica que as ocorrências de um evento devem chamar um método do manipulador nomeado. Na maioria dos modelos de código de procedimento, a conexão de eventos é um código "AddHandler" implícito ou explícito que nomeia ambos, o evento e o método, e geralmente envolve uma instância de objeto de destino. Em XAML, o "AddHandler" é implícito, e a conexão de eventos consiste inteiramente em nomear o evento com o nome do atributo de um elemento de objeto, e nomear o manipulador com o valor desse atributo.

Você escreve o manipulador real na linguagem de programação que está usando para todo o código do aplicativo e code-behind. Com o atributo `Click="showUpdatesButton_Click"`, você criou um contrato em que, quando a marcação XAML é compilada e analisada, a etapa de compilação de IDE na ação de compilação do ambiente de desenvolvimento e a eventual análise XAML quando as cargas de aplicativo podem encontrar um método chamado `showUpdatesButton_Click` como parte do código do aplicativo. `showUpdatesButton_Click` deve ser um método que implementa uma assinatura de método compatível (com base em um delegado) para qualquer manipulador do evento [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737). Por exemplo, este código define o manipulador `showUpdatesButton_Click`.

> [!div class="tabbedCodeSnippets"]
```csharp
private void showUpdatesButton_Click (object sender, RoutedEventArgs e) {
    Button b = sender as Button;
    //more logic to do here...
}
```
```vb
Private Sub showUpdatesButton_Click(ByVal sender As Object, ByVal e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    '  more logic to do here...
End Sub
```
```cpp
void MyNamespace::BlankPage::showUpdatesButton_Click(Platform::Object^ sender, Windows::UI::Xaml::Input::RoutedEventArgs^ e) {
    Button^ b = (Button^) sender;
    //more logic to do here...
}
```

Neste exemplo, o método `showUpdatesButton_Click` baseia-se no delegado de [**RoutedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br208812). Você saberia que esse é o delegado a ser usado, porque você verá esse delegado nomeado na sintaxe para o método [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) na página de referência do MSDN.

**Dica**  O Visual Studio fornece um jeito bem conveniente de nomear o manipulador de eventos e definir o método do manipulador enquanto você está editando a XAML. Ao fornecer o nome de atributo do evento no editor de texto XAML, aguarde um pouco até que uma lista do Microsoft IntelliSense seja exibida. Se você clicar em **&lt;Novo Manipulador de Eventos&gt;** na lista, o Microsoft Visual Studio sugerirá um nome de método com base no **x:Name** do elemento (ou nome do tipo), no nome do evento e em um sufixo numérico. Você poderá então clicar com o botão direito do mouse no nome do manipulador de eventos selecionado e clicar em **Navegar até Manipulador de Eventos**. A navegação irá diretamente para a definição recém-inserida do manipulador de eventos, como visualizada na exibição do arquivo de code-behind para a página XAML. O manipulador de eventos já tem a assinatura correta, incluindo o parâmetro *sender* e a classe de dados de evento usada pelo evento. Além disso, se um método de manipulador com a assinatura correta já existir no seu code-behind, o nome desse método aparecerá no menu suspenso de preenchimento automático com a opção **&lt;Novo Manipulador de Eventos&gt;**. Também é possível pressionar a tecla Tab como um atalho, em vez de clicar nos itens da lista do IntelliSense.

## Definindo um manipulador de eventos

Para objetos que são elementos da interface do usuário e foram declarados em XAML, o código de manipulador de eventos é definido na classe parcial que opera como o code-behind de uma página XAML. Manipuladores de eventos são métodos que você escreve como parte da classe parcial associada ao XAML. Esses manipuladores de eventos se baseiam nos delegados utilizados por um determinado evento. Os métodos de manipulador de eventos podem ser públicos ou particulares. O acesso particular funciona porque o manipulador e a instância criada no XAML são finalmente unidos pela geração de código. Em geral, recomendamos que você torne particulares os métodos de manipulador de eventos, na classe.

**Observação**  Manipuladores de eventos para C++ não são definidos em classes parciais, mas declarados no cabeçalho como membros de uma classe particular. As ações de compilação para um projeto em C++ cuidam da geração do código que dá suporte ao sistema de tipos XAML e do modelo code-behind para C++.

### O parâmetro *sender* e dados do evento

O manipulador que você escreve para o evento pode acessar dois valores que ficam disponíveis como entrada para cada caso em que o seu manipulador é invocado. O primeiro desses valores é *sender*, que é uma referência ao objeto ao qual o manipulador está conectado. O parâmetro *sender* é digitado como o tipo **Object** de base. Uma técnica comum consiste em converter *sender* em um tipo mais preciso. Essa técnica é útil se você pretende verificar ou alterar o estado no próprio objeto *sender*. Com base no design do seu aplicativo, você geralmente espera um tipo que seja seguro para converter *sender*, baseado no local onde o manipulador foi anexado ou em outras especificidades do design.

O segundo valor são dados de evento. Esse valor geralmente aparece em definições de sintaxe como o parâmetro *e*. Você pode descobrir quais propriedades estão disponíveis para os dados do evento verificando o parâmetro *e* do delegado atribuído ao evento específico que você está manipulando e usando o IntelliSense ou o Pesquisador de Objetos no Visual Studio. Se preferir, use a documentação de referência do Tempo de Execução do Windows.

Para alguns eventos, os valores de propriedade específicos dos dados de evento são tão importantes quanto saber que o evento ocorreu. Isso é especialmente verdadeiro para os eventos de entrada. Para os eventos de ponteiro, a posição do ponteiro quando ocorreu o evento pode ser importante. Para eventos de teclado, todas as teclas possíveis acionam um evento [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) e um evento [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942). Para determinar a tecla que foi pressionada pelo usuário, acesse os [**KeyRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943072) disponíveis para o manipulador de eventos. Para saber mais sobre como manipular eventos de entrada, consulte [Interações de teclado](https://msdn.microsoft.com/library/windows/apps/mt185607) e [Identificar entrada do ponteiro](https://msdn.microsoft.com/library/windows/apps/mt404610). Eventos de entrada e cenários de entrada costumam incluir considerações adicionais e não são abordados neste tópico; por exemplo, captura de ponteiro para eventos de ponteiro e teclas modificadoras e códigos de teclas da plataforma para eventos de teclado.

### Manipuladores de eventos que usam o padrão **async**

Em alguns casos, você desejará usar APIs que usam um padrão **async** em um manipulador de eventos. Por exemplo, talvez você use [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) em [**AppBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) para exibir um seletor de arquivos e interagir com ele. No entanto, muitas das APIs de seletor de arquivos são assíncronas. Elas precisam ser chamadas dentro de um escopo **async**/awaitable, e o compilador fará essa imposição. Portanto, o que você pode fazer é adicionar a palavra-chave **async** ao manipulador de eventos, de forma que ele agora seja **async****void**. Agora, o seu manipulador de eventos pode fazer chamadas **async**/awaitable.

Para conhecer um exemplo de manipulação de eventos de interação do usuário usando o padrão **async**, consulte [Acesso a arquivos e seletores](https://msdn.microsoft.com/library/windows/apps/jj655411) (parte da série [Criar seu primeiro aplicativo do Windows Runtime em C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/hh974581)). Consulte também [Chamar APIs assíncronas em C).

## Adicionando manipuladores de eventos em código

A XAML não é a única forma de atribui um manipulador de eventos a um objeto. Para adicionar manipuladores de eventos a qualquer objeto em código, inclusive objetos não utilizáveis em XAML, você pode usar a sintaxe específica a uma linguagem para adicionar manipuladores de eventos.

Em C#, a sintaxe é usar o operador `+=`. Você registra o manipulador fazendo referência ao nome do método manipulador de eventos no lado direito do operador.

Se você está usando código para adicionar manipuladores de eventos a objetos que aparecem na interface do usuário em tempo de execução, uma prática comum é adicionar esses manipuladores em resposta a um evento de ciclo de vida do objeto ou de retorno de chamada – como, por exemplo, [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) ou [**OnApplyTemplate**](https://msdn.microsoft.com/library/windows/apps/br208737), de maneira que os manipuladores de eventos no objeto relevante estejam prontos para eventos iniciados pelo usuário no tempo de execução. Este exemplo mostra uma estrutura de tópicos XAML da estrutura da página e, em seguida, fornece a sintaxe da linguagem C# para adicionar um manipulador de eventos a um objeto.

```xml
<Grid x:Name="LayoutRoot" Loaded="LayoutRoot_Loaded">
  <StackPanel>
    <TextBlock Name="textBlock1">Put the pointer over this text</TextBlock>
...
  </StackPanel>
</Grid>
```

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += textBlock1_PointerEntered;
    textBlock1.PointerExited += textBlock1_PointerExited;
}
```

**Observação**  Há uma sintaxe mais detalhada. Em 2005, a linguagem C# adicionou um recurso chamado inferência de delegado, o qual permite que um compilador infira a nova instância do delegado e habilita a sintaxe anterior, mais simples. A sintaxe detalhada é funcionalmente idêntica ao exemplo anterior, mas cria explicitamente uma nova instância do delegado antes de registrá-la, assim, não tirando proveito da inferência do delegado. Essa sintaxe explícita é menos comum, mas você ainda pode vê-la em alguns exemplos de código.

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += new PointerEventHandler(textBlock1_PointerEntered);
    textBlock1.PointerExited += new MouseEventHandler(textBlock1_PointerExited);
}
```

Há duas possibilidades para sintaxe do Visual Basic. Uma é paralela à sintaxe de C# e anexa manipuladores diretamente a instâncias. Isso exige a palavra-chave **AddHandler** e também o operador **AddressOf** que desfaz a referência ao nome do método manipulador.

A outra opção para sintaxe do Visual Basic consiste em usar a palavra-chave **Handles** em manipuladores de eventos. Essa técnica é adequada em casos nos quais espera-se que manipuladores existam em objetos desde o carregamento e persistam ao longo do ciclo de vida do objeto. O uso de **Handles** em um objeto definido em XAML requer que você forneça um **Name** / **x:Name**. Esse nome se torna o qualificador de instâncias necessário para a parte *Instance.Event* da sintaxe **Handles**. Nesse caso, você não precisa de um manipulador de eventos baseado no ciclo de vida do objeto; as conexões **Handles** são criadas quando você compila sua página XAML.

```vb
Private Sub textBlock1_PointerEntered(ByVal sender As Object, ByVal e As PointerRoutedEventArgs) Handles textBlock1.PointerEntered
' ...
End Sub
```

**Observação**  O Visual Studio e a sua superfície de design em XAML costumam promover a técnica de manipulação de instâncias em vez da palavra-chave **Handles**. Isso é porque estabelecer as ligações dos manipuladores de eventos em XAML é parte do típico fluxo de trabalho entre designer e desenvolvedor, e a técnica da palavra-chave **Handles** é incompatível com o estabelecimento de ligações com os manipuladores de eventos em XAML.

Em C++, você também usa a sintaxe **+=**, mas há diferenças no formulário básico em C#:

-   Não há inferência de delegado, portanto, você deve usar **ref new** para a instância do delegado.
-   O construtor do delegado possui dois parâmetros e requer o objeto de destino como o primeiro parâmetro. Tipicamente, você especifica **this**.
-   O construtor delegate requer o endereço do método como o segundo parâmetro, de maneira que o operador de referência **&** preceda o nome do método.

```cpp
textBlock1->PointerEntered += 
ref new PointerEventHandler(this,&BlankPage::textBlock1_PointerExited);
```

### Removendo manipuladores de eventos em código

Normalmente não é necessário remover manipuladores de eventos em código, mesmo se você adicioná-los em código. O comportamento de vida útil do objeto para a maioria dos objetos do Windows Runtime como páginas e controles destruirá os objetos quando forem desconectados da [**Janela**](https://msdn.microsoft.com/library/windows/apps/br209041) principal e de sua árvore visual, e todas as referências a delegados serão destruídas também. O .NET faz isso através de coleta de lixo, e o Tempo de Execução do Windows com C++/CX usa referências fracas por padrão.

Há alguns casos em que você quer remover manipuladores de eventos explicitamente. Eles incluem:

-   Manipuladores adicionados para eventos estáticos, que não podem sofrer a coleta de lixo de maneira convencional. Exemplos de eventos estáticos na API do Windows Runtime são os eventos das classes [**CompositionTarget**](https://msdn.microsoft.com/library/windows/apps/br228126) e [**Clipboard**](https://msdn.microsoft.com/library/windows/apps/br205867).
-   Teste o código quando quiser que o tempo da remoção dos manipuladores seja imediato ou quando quiser trocar antigos/novos manipuladores de eventos para um evento em tempo de execução.
-   A implementação de um acessador **remove** personalizado.
-   Eventos estáticos personalizados.
-   Manipuladores para navegações de página.

[
              **FrameworkElement.Unloaded**
            ](https://msdn.microsoft.com/library/windows/apps/br208748) ou [**Page.NavigatedFrom**](https://msdn.microsoft.com/library/windows/apps/br227507) são possíveis disparadores de eventos que têm posições apropriadas em gerenciamento de estados e vida útil de objetos de maneira que você pode usá-los para remover manipuladores para outros eventos.

Por exemplo, é possível remover um manipulador de eventos chamado **textBlock1\_PointerEntered** do objeto de destino **textBlock1** usando esse código.

> [!div class="tabbedCodeSnippets"]
```csharp
textBlock1.PointerEntered -= textBlock1_PointerEntered;
```
```vb
RemoveHandler textBlock1.PointerEntered, AddressOf textBlock1_PointerEntered
```

Você também pode remover manipuladores para os casos em que o evento foi adicionado através de um atributo XAML, o que significa que o manipulador foi adicionado em código gerado. Isso é mais fácil de fazer se você fornecer um valor **Name** para o elemento em que o manipulador foi anexado, porque isso proporciona uma referência de objeto para o código posteriormente; entretanto, você também pode percorrer a árvore do objeto para encontrar a referência de objeto necessária em casos em que o objeto não tem **Name**.

Se você precisar remover um manipulador de eventos em C++/CX, precisará de um token de registro, que deveria ter recebido do valor de retorno do registro de manipulador de eventos `+=`. Isso ocorre porque o valor usado para o lado direito do cancelamento de registro de `-=` na sintaxe C++/CX é o token, e não o nome do método. Para C++/CX, você não pode remover os manipuladores que foram adicionados como um atributo XAML porque o código gerado C++/CX não salva um token.

## Eventos roteados

O Windows Runtime com C#, Microsoft Visual Basic ou C++/CX aceita o conceito de um evento roteado para um conjunto de eventos presentes na maioria dos elementos da interface do usuário. Esses eventos são para cenários de entrada e interação do usuário e são implementados na classe base [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911). Veja a seguir uma lista de eventos de entrada que são eventos roteados:

-   [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/br208922)
-   [**DragEnter**](https://msdn.microsoft.com/library/windows/apps/br208923)
-   [**DragLeave**](https://msdn.microsoft.com/library/windows/apps/br208924)
-   [**DragOver**](https://msdn.microsoft.com/library/windows/apps/br208925)
-   [**Drop**](https://msdn.microsoft.com/library/windows/apps/br208926)
-   [**Holding**](https://msdn.microsoft.com/library/windows/apps/br208928)
-   [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941)
-   [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)
-   [**ManipulationCompleted**](https://msdn.microsoft.com/library/windows/apps/br208945)
-   [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946)
-   [**ManipulationInertiaStarting**](https://msdn.microsoft.com/library/windows/apps/br208947)
-   [**ManipulationStarted**](https://msdn.microsoft.com/library/windows/apps/br208950)
-   [**ManipulationStarting**](https://msdn.microsoft.com/library/windows/apps/br208951)
-   [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964)
-   [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965)
-   [**PointerEntered**](https://msdn.microsoft.com/library/windows/apps/br208968)
-   [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969)
-   [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970)
-   [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971)
-   [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972)
-   [**PointerWheelChanged**](https://msdn.microsoft.com/library/windows/apps/br208973)
-   [**RightTapped**](https://msdn.microsoft.com/library/windows/apps/br208984)
-   [**Tapped**](https://msdn.microsoft.com/library/windows/apps/br208985)
-   [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/br208927)
-   [**LostFocus**](https://msdn.microsoft.com/library/windows/apps/br208943)

Um evento roteado é um evento que é potencialmente passado adiante (*roteado*) de um objeto filho para cada um dos seus sucessivos objetos pais em uma árvore de objetos. A estrutura XAML da interface do usuário se aproxima dessa árvore, sendo que a raiz da árvore é o elemento raiz do XAML. A verdadeira árvore de objetos pode variar um pouco do aninhamento de elementos XAML porque a árvore de objetos não inclui recursos da linguagem XAML – como marcas de elementos de propriedades. Você pode conceber o evento roteado como *decorrente* de qualquer elemento filho de elemento de objeto XAML que aciona o evento em direção ao elemento de objeto pai que o contém. O evento e seus dados podem ser manipulados em vários objetos ao longo da rota de evento. Se nenhum elemento tiver manipuladores, a rota se mantém potencialmente em movimento até que o elemento raiz seja alcançado.

Se conhecer tecnologias da Web, como DHTML (HTML Dinâmico) ou HTML5, talvez você já esteja familiarizado com o conceito de evento  *propagação*.

Quando um evento roteado se propaga pela rota de evento, todos os manipuladores de eventos anexados acessam uma instância compartilhada de dados de evento. Portanto, se houver dados de evento que possam ser gravados por um manipulador, todas as alterações feitas nos dados de evento serão passadas para o próximo manipulador e poderão não representar mais os dados de evento originais do evento. Quando um evento apresenta um comportamento de evento roteado, a documentação de referência inclui comentários e outras anotações sobre o comportamento roteado.

### A propriedade **OriginalSource** de **RoutedEventArgs**

Quando um evento se propaga uma rota de eventos acima, *sender* não é mais o mesmo objeto que ergueu o evento. Em vez disso, *sender* é o objeto no qual o manipulador que está sendo invocado está conectado.

Em alguns casos, *sender* não é o objeto de interesse e, em vez disso, você está interessado em informações como, por exemplo, em quais dos possíveis objetos filho o ponteiro está focalizado quando um evento de ponteiro é disparado ou qual objeto em uma interface do usuário maior tinha o foco quando um usuário pressionou uma tecla do teclado. Nesses casos, você pode usar o valor da propriedade [**OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810). Em todos os pontos da rota, a **OriginalSource** relata o objeto original que acionou o evento, e não o local onde o manipulador foi conectado. Contudo, para eventos de entrada de [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911), esse objeto original costuma ser um objeto não imediatamente visível na XAML de definição de interface do usuário em nível de página. Em vez disso, o objeto original pode ser uma parte modelo de um controle. Por exemplo, se o usuário passa com o ponteiro sobre a borda de um [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265), para a maioria dos eventos de ponteiro, **OriginalSource** é uma parte modelo [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) em [**Template**](https://msdn.microsoft.com/library/windows/apps/br209465), e não o **Button** propriamente dito.

**Dica**  A propagação de eventos de entrada é especialmente útil na criação de um controle modelo. Qualquer controle que tenha um modelo pode ter um novo modelo aplicado pelo seu consumidor. O consumidor que está tentando recriar um modelo de trabalho pode eliminar inadvertidamente uma parte da manipulação de eventos declarada no modelo padrão. Você ainda pode fornecer manipulação de eventos em nível de controle anexando manipuladores como parte da substituição de [**OnApplyTemplate**](https://msdn.microsoft.com/library/windows/apps/br208737) na definição de classe. Em seguida, pode apanhar os eventos de entrada propagados rumo à raiz do controle na instanciação.

### A propriedade **Handled**

Várias classes de dados de eventos para eventos roteados específicos contêm uma propriedade chamada **Handled**. Para ver exemplos, confira [**PointerRoutedEventArgs.Handled**](https://msdn.microsoft.com/library/windows/apps/hh943079), [**KeyRoutedEventArgs.Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) e [**DragEventArgs.Handled**](https://msdn.microsoft.com/library/windows/apps/br242375). Em todos os casos, **Handled** é uma propriedade booliana configurável.

A definição da propriedade **Handled** como **true** influencia o comportamento do sistema de eventos. Quando **Handled** é **true**, o roteamento é interrompido na maioria dos manipuladores de eventos; o evento não prossegue na rota para notificar outros manipuladores anexados sobre esse caso de evento específico. Cabe a você decidir sobre o significado de "manipulado" no contexto do evento e como o seu aplicativo responde a ele. Basicamente, **Handled** é um protocolo simples que permite que o código do aplicativo afirme que uma ocorrência de um evento não precisa se propagar para nenhum contêiner: a lógica do seu aplicativo se encarregou do que precisa ser feito. Por outro lado, porém, você precisa garantir que não está lidando com eventos que provavelmente devem ser propagados para que comportamentos internos do sistema ou de controles possam agir. Por exemplo, a manipulação de eventos de baixo nível dentro das partes ou itens de um controle de seleção pode ser prejudicial. O controle de seleção pode estar à procura de eventos de entrada para saber que a seleção deve mudar.

Nem todos os eventos roteados podem cancelar um rota dessa maneira, e você pode deduzir isso porque eles não terão uma propriedade **Handled**. Por exemplo, [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/br208927) e [**LostFocus**](https://msdn.microsoft.com/library/windows/apps/br208943) são propagados, mas isso sempre acontece em direção à raiz, e suas classes de dados de eventos não têm uma propriedade **Handled** capaz de influenciar esse comportamento.

##  Manipuladores de eventos de entrada em controles

Às vezes, controles específicos do Windows Runtime usam o conceito **Handled** internamente para eventos de entrada. Isso pode dar a impressão de que um evento de entrada nunca ocorre, pois o código do usuário não pode manipulá-lo. Por exemplo, a classe [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) inclui lógica que manipula deliberadamente o evento de entrada geral [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971). Isso ocorre porque botões acionam um evento [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) que é iniciado por entrada do ponteiro pressionado, assim como por outros modos de entrada como teclas de manipulação, tais como a tecla Enter, que podem invocar o botão quando são focadas. Para os objetivos do design de classe **Button**, o evento de dados brutos é manipulado conceitualmente. Os consumidores da classe, como seu código de usuário, podem, em vez disso, interagir com o evento relevante de controle **Click**. Tópicos para classes de controle específicas da API do Windows Runtime costumam registrar o comportamento de manipulação de eventos implementado pela classe. Em alguns casos, é possível alterar o comportamento por meio da substituição dos métodos **On***Event*. Por exemplo, você pode alterar como a sua classe derivada [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) reage à entrada de tecla substituindo [**Control.OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/hh967982).

##  Registrando manipuladores para eventos roteados já manipulados

Nós explicamos antes que a definição de **Handled** como **true** impede a chamada da maioria dos manipuladores. Mas o método [**AddHandler**](https://msdn.microsoft.com/library/windows/apps/hh702399) oferece uma técnica para você anexar um manipulador que seja sempre invocado para a rota, mesmo que algum outro manipulador já tenha definido **Handled** como **true** nos dados compartilhados do evento. Essa técnica é útil quando você usa um controle que manipulou o evento na sua composição interna ou para a lógica específica do controle, mas você ainda quer responder a ele em uma instância de controle ou na interface do usuário do seu aplicativo. Porém, essa técnica deve ser usada com cuidado, pois pode opor-se ao propósito de **Handled** e possivelmente violar as interações pretendidas de um controle.

Apenas os eventos roteados que têm um identificador de evento roteado correspondente podem usar a técnica de manipulação de eventos [**AddHandler**](https://msdn.microsoft.com/library/windows/apps/hh702399), pois o identificador é uma entrada obrigatória do método **AddHandler**. Veja a documentação de referência de [**AddHandler**](https://msdn.microsoft.com/library/windows/apps/hh702399) para obter uma lista de eventos que têm identificadores de eventos roteados disponíveis. Para a maior parte, essa é a mesma lista dos eventos roteados que mostramos anteriormente. A exceção é que os dois últimos na lista, [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/br208927) e [**LostFocus**](https://msdn.microsoft.com/library/windows/apps/br208943), não têm um identificador de eventos roteados, por isso você não pode usar **AddHandler** para eles.

## Eventos roteados fora da árvore de objetos

Certos objetos participam de uma relação com a árvore visual primária que é conceitualmente semelhante a possuir uma sobreposição sobre os elementos visuais principais. Esses objetos não são parte das relações entre pai e filho comuns que conectam todos os três elementos à raiz visual. Esse é o caso para qualquer [**Popup**](https://msdn.microsoft.com/library/windows/apps/br227842) ou [**ToolTip**](https://msdn.microsoft.com/library/windows/apps/br227608) exibido. Se quiser manipular eventos roteados em um **Popup** ou em uma **ToolTip**, posicione os manipuladores sobre elementos específicos da interface do usuário que estão no **Popup** ou na **ToolTip**, e não nos elementos **Popup** ou **ToolTip**. Não dependa do roteamento dentro de qualquer composição executada para o conteúdo do **Popup** ou da **ToolTip**. Isso porque o roteamento de eventos para eventos roteados funciona apenas ao longo da árvore visual principal. Um **Popup** ou uma **ToolTip** não é considerado um pai de elementos subsidiários da interface do usuário e nunca recebe o evento roteado, mesmo que esteja tentando usar algo como a tela de fundo padrão de **Popup** como a área de captura de eventos de entrada.

## Teste de clique e eventos de entrada

Determinar se um elemento está visível para a entrada por mouse, toque e de caneta é chamado de *teste de clique*. Para ações de toque e também para eventos específicos de interação ou de manipulação resultantes de uma ação de toque, é preciso que o elemento esteja visível para teste de clique, para ser a origem do evento e acionar o evento associado à ação. Caso contrário, a ação passa pelo elemento em direção a qualquer elemento subjacente ou aos elementos pai na árvore visual que interage com essa entrada. Há vários fatores que influenciam o teste de clique, mas é possível determinar se um determinado elemento pode acionar eventos de entrada verificando sua propriedade [**IsHitTestVisible**](https://msdn.microsoft.com/library/windows/apps/br208933). Esta propriedade retorna **true** apenas quando o elemento segue estes critérios:

-   O valor de propriedade [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) do elemento é [**Visible**](https://msdn.microsoft.com/library/windows/apps/br209006).
-   O valor da propriedade **Background** ou **Fill** do elemento não é **null**. Um valor **null**[**Brush**](https://msdn.microsoft.com/library/windows/apps/br228076) resulta em transparência e invisibilidade do teste de clique. (Para tornar um elemento transparente, mas também visível para teste de clique, use um pincel [**Transparent**](https://msdn.microsoft.com/library/windows/apps/hh748061) em vez de **null**.)

**Observação**
            **Background** e **Fill** não são definidos por [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) e, em vez disso, são definidos por diferentes classes derivadas, como [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390) e [**Shape**](https://msdn.microsoft.com/library/windows/apps/br243377). As implicações dos pincéis usados para as propriedades primeiro plano e tela de fundo são as mesmas para teste de clique e eventos de entrada, independentemente da subclasse que implementa as propriedades.

-   Se o elemento for um controle, o valor da sua propriedade [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/br209419) deverá ser **true**.
-   É preciso que o elemento tenha dimensões reais em layout. Um elemento em que [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) e [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) são iguais a 0 não acionará eventos.

Alguns controles têm regras especiais para teste de hit. Por exemplo, o [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) não tem uma propriedade **Background**, mas ainda pode executar o teste de clique em toda a região de suas dimensões. Os controles [
              **Image**
            ](https://msdn.microsoft.com/library/windows/apps/br242752) e [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) podem executar teste de clique nas dimensões do retângulo definido, independentemente do conteúdo transparente – por exemplo, canal alfa no arquivo de origem de mídia que está sendo exibido. Os controles [
              **WebView**
            ](https://msdn.microsoft.com/library/windows/apps/br227702) têm um comportamento especial de teste de clique, pois a entrada pode ser manipulada pelo HTML hospedado e acionar eventos de script.

A maioria das classes [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) e [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) não executa teste de clique na tela de fundo, mas essas classes ainda podem manipular os eventos de entrada do usuário que são roteados dos elementos que os contêm.

Você pode determinar quais elementos estão localizados na mesma posição de um evento de entrada do usuário, independentemente de se os elementos são passíveis de teste de hit. Para fazer isso, chame o método [**FindElementsInHostCoordinates**](https://msdn.microsoft.com/library/windows/apps/br243039). Como o nome sugere, esse método localiza os elementos em um local em relação a um elemento host especificado. Entretanto, transformações aplicadas e mudanças de layout podem ajustar o sistema de coordenadas relativas de um elemento e, portanto, influenciar quais elementos são encontrados em um determinado local.

## Comando

Um pequeno número de elementos da interface do usuário dão suporte a *comandos*. Comandos usam eventos roteados relacionados a entrada na sua implementação subjacente e permite o processamento de entrada relacionada da interface do usuário (uma determinada ação do ponteiro, uma tecla de aceleração específica) invocando um único manipulador de comandos. Se comandos estiverem disponíveis para um elemento da interface do usuário, considere usar as respectivas APIs de comando em vez de qualquer evento de entrada à parte. Em geral, você usa uma referência **Binding** em propriedades de uma classe que define o modelo de exibição para dados. As propriedades mantêm comandos nomeados que implementam o padrão de comandos **ICommand** específico de cada linguagem. Para obter mais informações, consulte [**ButtonBase.Command**](https://msdn.microsoft.com/library/windows/apps/br227740).

## Eventos personalizados no Windows Runtime

Para fins de definir eventos personalizados, o modo de adicionar o evento e o que isso significa para seu design de classe dependem bastante da linguagem de programação sendo usada.

-   Para C# e Visual Basic, você está definindo um evento de CLR. Você pode usar o padrão de evento .NET, desde que não esteja usando acessadores personalizados(**add**/**remove**). Outras dicas:
    -   Para o manipulador de eventos, é uma ótima ideia usar [**System.EventHandler<TEventArgs>**](https://msdn.microsoft.com/library/windows/apps/xaml/db0etb8x.aspx) porque ele tem conversão interna para o delegado de evento genérico do Windows Runtime [**EventHandler<T>**](https://msdn.microsoft.com/library/windows/apps/br206577).
    -   Não use como base de sua classe de dados de eventos o [**System.EventArgs**](https://msdn.microsoft.com/library/windows/apps/xaml/system.eventargs.aspx) porque ele não é convertido para o Windows Runtime. Use uma classe de dados de eventos existente ou nenhuma classe base.
    -   Se estiver usando acessadores personalizados, veja [Eventos personalizados e acessadores de eventos nos Componentes de Tempo de Execução do Windows](https://msdn.microsoft.com/library/windows/apps/xaml/hh972883.aspx).
    -   Se não tiver certeza do que é o padrão de evento .NET padrão, veja [Definindo eventos para classes personalizadas do Silverlight](http://msdn.microsoft.com/library/dd833067.aspx). Esse tópico foi escrito para o Microsoft Silverlight mas ainda é uma ótima referência do código e dos conceitos para o padrão de evento .NET padrão.
-   Para C++/CX, veja [Eventos (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh755799.aspx).
    -   Use referências nomeadas mesmo para seus próprios usos de eventos personalizados. Não use lambda para eventos personalizados, pois ele pode criar uma referência circular.

Você não pode declarar um evento roteado personalizado para o Tempo de Execução do Windows; eventos roteados são limitados ao conjunto proveniente do Tempo de Execução do Windows.

A definição de um evento personalizado normalmente é feita como parte do exercício de definir um controle personalizado. É um padrão comum ter uma propriedade de dependência que tem um retorno de chamada de propriedades alteradas, e também definir um evento personalizado que é acionado pelo retorno de chamada de propriedades de dependência em alguns ou todos os casos. Os consumidores do seu controle não têm acesso ao retorno de chamada de propriedades alteradas que você definiu, mas ter um evento de notificação disponível é o próximo passo interessante. Para saber mais, consulte [Propriedades de dependência personalizada](custom-dependency-properties.md).

## Tópicos relacionados

* [Visão geral da XAML](xaml-overview.md)
* [Guia de início rápido: entrada por toque](https://msdn.microsoft.com/library/windows/apps/xaml/hh465387)
* [Interações por teclado](https://msdn.microsoft.com/library/windows/apps/mt185607)
* [Eventos .NET e delegados](http://go.microsoft.com/fwlink/p/?linkid=214364)
* [Criando componentes do Tempo de Execução do Windows](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx)
* [**AddHandler**](https://msdn.microsoft.com/library/windows/apps/hh702399)
 




<!--HONumber=Jun16_HO4-->


