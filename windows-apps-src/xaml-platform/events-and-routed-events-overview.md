---
description: Descrevemos o conceito de programação de eventos em um aplicativo Windows Runtime, ao C#usar, Visual Basic ou C++ as extensões deC++componente Visual (/CX) como sua linguagem de programação e XAML para sua definição de interface do usuário.
title: Visão geral de eventos e eventos roteados
ms.assetid: 34C219E8-3EFB-45BC-8BBD-6FD937698832
ms.date: 07/12/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9150fd34653e2beeeb8d8c1557cf9f77e95791e3
ms.sourcegitcommit: e0ae346eadda864dcad1453cd1644668549e66e1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68603442"
---
# <a name="events-and-routed-events-overview"></a>Visão geral de eventos e eventos roteados

**APIs importantes**
- [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)
- [**RoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.RoutedEventArgs)

Descrevemos o conceito de programação de eventos em um aplicativo Windows Runtime, ao C#usar, Visual Basic ou C++ as extensões deC++componente Visual (/CX) como sua linguagem de programação e XAML para sua definição de interface do usuário. Você pode atribuir manipuladores de eventos como parte das declarações para elementos da interface do usuário em XAML ou pode adicionar manipuladores no código. O Windows Runtime dá suporte a *eventos roteados*: determinados eventos de entrada e eventos de dados podem ser manipulados por outros objetos além do objeto que acionou o evento. Eventos roteados são úteis quando você define modelos de controle ou usa páginas ou contêineres de layout.

## <a name="events-as-a-programming-concept"></a>Eventos como um conceito de programação

De um modo geral, os conceitos de eventos na programação de um aplicativo do Windows Runtime são semelhantes ao modelo de evento nas linguagens de programação mais usadas. Se você sabe como trabalhar com eventos do Microsoft .NET ou C++, já começa em vantagem. Você não precisa saber muito sobre os conceitos de modelo de evento para executar algumas tarefas básicas – como, por exemplo, anexar manipuladores.

Quando você usa C#, Visual Basic ou C++/CX como linguagem de programação, a interface do usuário é definida na marcação (XAML). Na sintaxe de marcação XAML, alguns dos princípios de conexão de eventos entre elementos de marcação e entidades de código em tempo de execução são similares a outras tecnologias da Web, tais como ASP.NET ou HTML5.

Observe  que o código que fornece a lógica de tempo de execução para uma interface do usuário definida pelo XAML geralmente é conhecido como *code-behind* ou o arquivo code-behind. Nas visualizações de soluções do Microsoft Visual Studio, essa relação é mostrada graficamente, com o arquivo code-behind sendo um arquivo dependente e aninhado em contraposição à página XAML à qual ele se refere.

## <a name="buttonclick-an-introduction-to-events-and-xaml"></a>Button.Click: uma introdução a eventos e XAML

Uma das tarefas de programação mais comuns para um aplicativo do Tempo de Execução do Windows é a captura de entrada do usuário na interface do usuário. Por exemplo, a sua interface do usuário pode ter um botão em que o usuário deve clicar para enviar informações ou mudar o estado.

Você define a interface do usuário para o seu aplicativo do Windows Runtime gerando a XAML. Esse XAML é geralmente o resultado de uma superfície de design no Visual Studio. A XAML também pode ser escrita em um editor de texto sem formatação ou em um editor XAML de terceiros. Durante a geração dessa XAML, você pode conectar manipuladores de eventos de elementos individuais da interface do usuário e, ao mesmo tempo, definir todos os demais atributos que estabelecem valores de propriedade desse elemento da interface do usuário.

Para conectar os eventos em XAML, você pode especificar o nome em forma de cadeia de caracteres do método do manipulador que já definiu ou vai definir posteriormente no code-behind. Por exemplo, esta XAML define um objeto [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) com outras propriedades ([atributo x:Name](x-name-attribute.md), [**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.content)) atribuídas como atributos e transfere um manipulador para o evento [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) do botão referenciando um método chamado `ShowUpdatesButton_Click`:

```xaml
<Button x:Name="showUpdatesButton"
  Content="{Binding ShowUpdatesText}"
  Click="ShowUpdatesButton_Click"/>
```

**Dica**  *Conexão de eventos* é um termo de programação. Refere-se ao processo ou código por meio do qual você indica que as ocorrências de um evento devem chamar um método do manipulador nomeado. Na maioria dos modelos de código de procedimento, a conexão de eventos é um código "AddHandler" implícito ou explícito que nomeia ambos, o evento e o método, e geralmente envolve uma instância de objeto de destino. Em XAML, o "AddHandler" é implícito, e a conexão de eventos consiste inteiramente em nomear o evento com o nome do atributo de um elemento de objeto, e nomear o manipulador com o valor desse atributo.

Você escreve o manipulador real na linguagem de programação que está usando para todo o código do aplicativo e code-behind. Com o atributo `Click="ShowUpdatesButton_Click"`, você criou um contrato em que, quando a marcação XAML é compilada e analisada, a etapa de compilação de IDE na ação de compilação do ambiente de desenvolvimento e a eventual análise XAML quando as cargas de aplicativo podem encontrar um método chamado `ShowUpdatesButton_Click` como parte do código do aplicativo. `ShowUpdatesButton_Click` deve ser um método que implementa uma assinatura de método compatível (com base em um delegado) para qualquer manipulador do evento [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click). Por exemplo, este código define o manipulador `ShowUpdatesButton_Click`.

```csharp
private void ShowUpdatesButton_Click (object sender, RoutedEventArgs e) 
{
    Button b = sender as Button;
    //more logic to do here...
}
```

```vb
Private Sub ShowUpdatesButton_Click(ByVal sender As Object, ByVal e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    '  more logic to do here...
End Sub
```

```cppwinrt
void winrt::MyNamespace::implementation::BlankPage::ShowUpdatesButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& e)
{
    auto b{ sender.as<Windows::UI::Xaml::Controls::Button>() };
    // More logic to do here.
}
```

```cpp
void MyNamespace::BlankPage::ShowUpdatesButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e) 
{
    Button^ b = (Button^) sender;
    //more logic to do here...
}
```

Neste exemplo, o método `ShowUpdatesButton_Click` baseia-se no delegado de [**RoutedEventHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.routedeventhandler). Você saberia que esse é o delegado a ser usado, porque você verá esse delegado nomeado na sintaxe para o método [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) na página de referência do MSDN.

**A Tip**  Visual Studio fornece uma maneira conveniente de nomear o manipulador de eventos e definir o método de manipulador enquanto você edita XAML. Ao fornecer o nome de atributo do evento no editor de texto XAML, aguarde um pouco até que uma lista do Microsoft IntelliSense seja exibida. Se você clicar em **&lt;Novo Manipulador de Eventos&gt;** na lista, o Microsoft Visual Studio sugerirá um nome de método com base no **x:Name** do elemento (ou nome do tipo), no nome do evento e em um sufixo numérico. Você poderá então clicar com o botão direito do mouse no nome do manipulador de eventos selecionado e clicar em **Navegar até Manipulador de Eventos**. A navegação irá diretamente para a definição recém-inserida do manipulador de eventos, como visualizada na exibição do arquivo de code-behind para a página XAML. O manipulador de eventos já tem a assinatura correta, incluindo o parâmetro *sender* e a classe de dados de evento usada pelo evento. Além disso, se um método de manipulador com a assinatura correta já existir no seu code-behind, o nome desse método aparecerá no menu suspenso de preenchimento automático com a opção **&lt;Novo Manipulador de Eventos&gt;** . Também é possível pressionar a tecla Tab como um atalho, em vez de clicar nos itens da lista do IntelliSense.

## <a name="defining-an-event-handler"></a>Definindo um manipulador de eventos

Para objetos que são elementos da interface do usuário e foram declarados em XAML, o código de manipulador de eventos é definido na classe parcial que opera como o code-behind de uma página XAML. Manipuladores de eventos são métodos que você escreve como parte da classe parcial associada ao XAML. Esses manipuladores de eventos se baseiam nos delegados utilizados por um determinado evento. Os métodos de manipulador de eventos podem ser públicos ou particulares. O acesso particular funciona porque o manipulador e a instância criada no XAML são finalmente unidos pela geração de código. Em geral, recomendamos que você torne particulares os métodos de manipulador de eventos, na classe.

Observação  os manipuladores de C++ eventos para não são definidos em classes parciais, eles são declarados no cabeçalho como um membro de classe privada. As ações de compilação para um projeto em C++ cuidam da geração do código que dá suporte ao sistema de tipos XAML e do modelo code-behind para C++.

### <a name="the-sender-parameter-and-event-data"></a>O parâmetro *sender* e dados do evento

O manipulador que você escreve para o evento pode acessar dois valores que ficam disponíveis como entrada para cada caso em que o seu manipulador é invocado. O primeiro desses valores é *sender*, que é uma referência ao objeto ao qual o manipulador está conectado. O parâmetro *sender* é digitado como o tipo **Object** de base. Uma técnica comum consiste em converter *sender* em um tipo mais preciso. Essa técnica é útil se você pretende verificar ou alterar o estado no próprio objeto *sender*. Com base no design do seu aplicativo, você geralmente espera um tipo que seja seguro para converter *sender*, baseado no local onde o manipulador foi anexado ou em outras especificidades do design.

O segundo valor são dados de evento. Esse valor geralmente aparece em definições de sintaxe como o parâmetro *e*. Você pode descobrir quais propriedades estão disponíveis para os dados do evento verificando o parâmetro *e* do delegado atribuído ao evento específico que você está manipulando e usando o IntelliSense ou o Pesquisador de Objetos no Visual Studio. Se preferir, use a documentação de referência do Tempo de Execução do Windows.

Para alguns eventos, os valores de propriedade específicos dos dados de evento são tão importantes quanto saber que o evento ocorreu. Isso é especialmente verdadeiro para os eventos de entrada. Para os eventos de ponteiro, a posição do ponteiro quando ocorreu o evento pode ser importante. Para eventos de teclado, todas as teclas possíveis acionam um evento [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) e um evento [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup). Para determinar a tecla que foi pressionada pelo usuário, acesse os [**KeyRoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs) disponíveis para o manipulador de eventos. Para saber mais sobre como manipular eventos de entrada, consulte [Interações de teclado](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions) e [Identificar entrada do ponteiro](https://docs.microsoft.com/windows/uwp/input-and-devices/handle-pointer-input). Eventos de entrada e cenários de entrada costumam incluir considerações adicionais e não são abordados neste tópico; por exemplo, captura de ponteiro para eventos de ponteiro e teclas modificadoras e códigos de teclas da plataforma para eventos de teclado.

### <a name="event-handlers-that-use-the-async-pattern"></a>Manipuladores de eventos que usam o padrão **async**

Em alguns casos, você desejará usar APIs que usam um padrão **async** em um manipulador de eventos. Por exemplo, talvez você use [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) em [**AppBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) para exibir um seletor de arquivos e interagir com ele. No entanto, muitas das APIs de seletor de arquivos são assíncronas. Elas precisam ser chamadas dentro de um escopo **async**/awaitable, e o compilador fará essa imposição. O que você pode fazer é adicionar a palavra-chave **Async** ao manipulador de eventos, de modo que o manipulador agora é **Async** **void**. Agora, o seu manipulador de eventos pode fazer chamadas **async**/awaitable.

Para conhecer um exemplo de manipulação de eventos de interação do usuário usando o padrão **async**, consulte [Acesso a arquivos e seletores](https://docs.microsoft.com/previous-versions/windows/apps/jj655411(v=win.10)) (parte da série [Criar seu primeiro aplicativo do Windows Runtime em C# ou Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/hh974581(v=win.10))). Veja também [Chamar APIs assíncronas em C).

## <a name="adding-event-handlers-in-code"></a>Adicionando manipuladores de eventos em código

A XAML não é a única forma de atribui um manipulador de eventos a um objeto. Para adicionar manipuladores de eventos a qualquer objeto em código, inclusive objetos não utilizáveis em XAML, você pode usar a sintaxe específica a uma linguagem para adicionar manipuladores de eventos.

Em C#, a sintaxe é usar o operador `+=`. Você registra o manipulador fazendo referência ao nome do método manipulador de eventos no lado direito do operador.

Se você está usando código para adicionar manipuladores de eventos a objetos que aparecem na interface do usuário em tempo de execução, uma prática comum é adicionar esses manipuladores em resposta a um evento de ciclo de vida do objeto ou de retorno de chamada – como, por exemplo, [**Loaded**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded) ou [**OnApplyTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate), de maneira que os manipuladores de eventos no objeto relevante estejam prontos para eventos iniciados pelo usuário no tempo de execução. Este exemplo mostra uma estrutura de tópicos XAML da estrutura da página e, em seguida, fornece a sintaxe da linguagem C# para adicionar um manipulador de eventos a um objeto.

```xaml
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

**Observe que existe**umasintaxemaisdetalhada  . Em 2005, a linguagem C# adicionou um recurso chamado inferência de delegado, o qual permite que um compilador infira a nova instância do delegado e habilita a sintaxe anterior, mais simples. A sintaxe detalhada é funcionalmente idêntica ao exemplo anterior, mas cria explicitamente uma nova instância do delegado antes de registrá-la, assim, não tirando proveito da inferência do delegado. Essa sintaxe explícita é menos comum, mas você ainda pode vê-la em alguns exemplos de código.

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

Observe  que o Visual Studio e sua superfície de design XAML geralmente promovem a técnica de manipulação de instância em vez da palavra-chave **Handles** . Isso é porque estabelecer as ligações dos manipuladores de eventos em XAML é parte do típico fluxo de trabalho entre designer e desenvolvedor, e a técnica da palavra-chave **Handles** é incompatível com o estabelecimento de ligações com os manipuladores de eventos em XAML.

Em C++/CX, você também usa a **+=** sintaxe, mas há diferenças no formato básico: C#

- Não há inferência de delegado, portanto, você deve usar **ref new** para a instância do delegado.
- O construtor do delegado possui dois parâmetros e requer o objeto de destino como o primeiro parâmetro. Tipicamente, você especifica **this**.
- O construtor delegate requer o endereço do método como o segundo parâmetro, de maneira que o operador de referência **&** preceda o nome do método.

```cppwinrt
textBlock1().PointerEntered({this, &MainPage::TextBlock1_PointerEntered });
```

```cpp
textBlock1->PointerEntered += 
ref new PointerEventHandler(this, &BlankPage::textBlock1_PointerEntered);
```

### <a name="removing-event-handlers-in-code"></a>Removendo manipuladores de eventos em código

Normalmente não é necessário remover manipuladores de eventos em código, mesmo se você adicioná-los em código. O comportamento de vida útil do objeto para a maioria dos objetos do Windows Runtime como páginas e controles destruirá os objetos quando forem desconectados da [**Janela**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) principal e de sua árvore visual, e todas as referências a delegados serão destruídas também. O .NET faz isso através de coleta de lixo, e o Tempo de Execução do Windows com C++/CX usa referências fracas por padrão.

Há alguns casos em que você quer remover manipuladores de eventos explicitamente. Elas incluem:

- Manipuladores adicionados para eventos estáticos, que não podem sofrer a coleta de lixo de maneira convencional. Exemplos de eventos estáticos na API do Windows Runtime são os eventos das classes [**CompositionTarget**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CompositionTarget) e [**Clipboard**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.Clipboard).
- Teste o código quando quiser que o tempo da remoção dos manipuladores seja imediato ou quando quiser trocar antigos/novos manipuladores de eventos para um evento em tempo de execução.
- A implementação de um acessador **remove** personalizado.
- Eventos estáticos personalizados.
- Manipuladores para navegações de página.

[**FrameworkElement.** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.unloaded) Unloaded ou [**Page. NavigatedFrom**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom) são possíveis gatilhos de eventos que têm posições apropriadas no gerenciamento de estado e no tempo de vida do objeto, de modo que você pode usá-los para remover manipuladores de outros eventos.

Por exemplo, você pode remover um manipulador de eventos **chamado\_textBlock1 PointerEntered** do objeto de destino **textBlock1** usando este código.

```csharp
textBlock1.PointerEntered -= textBlock1_PointerEntered;
```

```vb
RemoveHandler textBlock1.PointerEntered, AddressOf textBlock1_PointerEntered
```

Você também pode remover manipuladores para os casos em que o evento foi adicionado através de um atributo XAML, o que significa que o manipulador foi adicionado em código gerado. Isso é mais fácil de fazer se você fornecer um valor **Name** para o elemento em que o manipulador foi anexado, porque isso proporciona uma referência de objeto para o código posteriormente; entretanto, você também pode percorrer a árvore do objeto para encontrar a referência de objeto necessária em casos em que o objeto não tem **Name**.

Se você precisar remover um manipulador de eventos em C++/CX, precisará de um token de registro, que deveria ter recebido do valor de retorno do registro de manipulador de eventos `+=`. Isso ocorre porque o valor usado para o lado direito do cancelamento de registro de `-=` na sintaxe C++/CX é o token, e não o nome do método. Para C++/CX, você não pode remover os manipuladores que foram adicionados como um atributo XAML porque o código gerado C++/CX não salva um token.

## <a name="routed-events"></a>Eventos roteados

O Windows Runtime com C#, Microsoft Visual Basic ou C++/CX aceita o conceito de um evento roteado para um conjunto de eventos presentes na maioria dos elementos da interface do usuário. Esses eventos são para cenários de entrada e interação do usuário e são implementados na classe base [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement). Veja a seguir uma lista de eventos de entrada que são eventos roteados:

- [**BringIntoViewRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)
- [**CharacterReceived**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.characterreceived)
- [**ContextCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextcanceled)
- [**ContextRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextrequested)
- [**DoubleTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.doubletapped)
- [**DragEnter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragenter)
- [**DragLeave**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragleave)
- [**DragOver**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragover)
- [**DragStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragstarting)
- [**Suspensa**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.drop)
- [**DropCompleted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dropcompleted)
- [**GettingFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gettingfocus)
- [**Ocorre**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus)
- [**Pressionado**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.holding)
- [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown)
- [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup)
- [**LosingFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.losingfocus)
- [**Perda**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus)
- [**ManipulationCompleted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)
- [**ManipulationDelta**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationdelta)
- [**ManipulationInertiaStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationinertiastarting)
- [**ManipulationStarted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationstarted)
- [**ManipulationStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationstarting)
- [**NoFocusCandidateFound**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.nofocuscandidatefoundeventargs)
- [**PointerCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercanceled)
- [**PointerCaptureLost**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost)
- [**PointerEntered**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered)
- [**PointerExited**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited)
- [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved)
- [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed)
- [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased)
- [**PointerWheelChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**PreviewKeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.previewkeydown.md)
- [**PreviewKeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.previewkeyup.md)
- [**PointerWheelChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**RightTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.righttapped)
- [**Cada**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.tapped)

Um evento roteado é um evento que é potencialmente passado adiante (*roteado*) de um objeto filho para cada um dos seus sucessivos objetos pais em uma árvore de objetos. A estrutura XAML da interface do usuário se aproxima dessa árvore, sendo que a raiz da árvore é o elemento raiz do XAML. A verdadeira árvore de objetos pode variar um pouco do aninhamento de elementos XAML porque a árvore de objetos não inclui recursos da linguagem XAML – como marcas de elementos de propriedades. Você pode conceber o evento roteado como *decorrente* de qualquer elemento filho de elemento de objeto XAML que aciona o evento em direção ao elemento de objeto pai que o contém. O evento e seus dados podem ser manipulados em vários objetos ao longo da rota de evento. Se nenhum elemento tiver manipuladores, a rota se mantém potencialmente em movimento até que o elemento raiz seja alcançado.

Se conhecer tecnologias da Web, como DHTML (HTML Dinâmico) ou HTML5, talvez você já esteja familiarizado com o conceito de evento  *propagação*.

Quando um evento roteado se propaga pela rota de evento, todos os manipuladores de eventos anexados acessam uma instância compartilhada de dados de evento. Portanto, se houver dados de evento que possam ser gravados por um manipulador, todas as alterações feitas nos dados de evento serão passadas para o próximo manipulador e poderão não representar mais os dados de evento originais do evento. Quando um evento apresenta um comportamento de evento roteado, a documentação de referência inclui comentários e outras anotações sobre o comportamento roteado.

### <a name="the-originalsource-property-of-routedeventargs"></a>A propriedade **OriginalSource** de **RoutedEventArgs**

Quando um evento se propaga uma rota de eventos acima, *sender* não é mais o mesmo objeto que ergueu o evento. Em vez disso, *sender* é o objeto no qual o manipulador que está sendo invocado está conectado.

Em alguns casos, *sender* não é o objeto de interesse e, em vez disso, você está interessado em informações como, por exemplo, em quais dos possíveis objetos filho o ponteiro está focalizado quando um evento de ponteiro é disparado ou qual objeto em uma interface do usuário maior tinha o foco quando um usuário pressionou uma tecla do teclado. Nesses casos, você pode usar o valor da propriedade [**OriginalSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.routedeventargs.originalsource). Em todos os pontos da rota, a **OriginalSource** relata o objeto original que acionou o evento, e não o local onde o manipulador foi conectado. Contudo, para eventos de entrada de [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement), esse objeto original costuma ser um objeto não imediatamente visível na XAML de definição de interface do usuário em nível de página. Em vez disso, o objeto original pode ser uma parte modelo de um controle. Por exemplo, se o usuário passa com o ponteiro sobre a borda de um [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button), para a maioria dos eventos de ponteiro, **OriginalSource** é uma parte modelo [**Border**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border) em [**Template**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.template), e não o **Button** propriamente dito.

  A bolha de evento de entrada TIP é especialmente útil se você estiver criando um controle modelo. Qualquer controle que tenha um modelo pode ter um novo modelo aplicado pelo seu consumidor. O consumidor que está tentando recriar um modelo de trabalho pode eliminar inadvertidamente uma parte da manipulação de eventos declarada no modelo padrão. Você ainda pode fornecer manipulação de eventos em nível de controle anexando manipuladores como parte da substituição de [**OnApplyTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate) na definição de classe. Em seguida, pode apanhar os eventos de entrada propagados rumo à raiz do controle na instanciação.

### <a name="the-handled-property"></a>A propriedade **Handled**

Várias classes de dados de eventos para eventos roteados específicos contêm uma propriedade chamada **Handled**. Para ver exemplos, confira [**PointerRoutedEventArgs.Handled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.handled), [**KeyRoutedEventArgs.Handled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled) e [**DragEventArgs.Handled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.drageventargs.handled). Em todos os casos, **Handled** é uma propriedade booliana configurável.

A definição da propriedade **Handled** como **true** influencia o comportamento do sistema de eventos. Quando **Handled** é **true**, o roteamento é interrompido na maioria dos manipuladores de eventos; o evento não prossegue na rota para notificar outros manipuladores anexados sobre esse caso de evento específico. Cabe a você decidir sobre o significado de "manipulado" no contexto do evento e como o seu aplicativo responde a ele. Basicamente, **Handled** é um protocolo simples que permite que o código do aplicativo afirme que uma ocorrência de um evento não precisa se propagar para nenhum contêiner: a lógica do seu aplicativo se encarregou do que precisa ser feito. Por outro lado, porém, você precisa garantir que não está lidando com eventos que provavelmente devem ser propagados para que comportamentos internos do sistema ou de controles possam agir. Por exemplo, a manipulação de eventos de baixo nível dentro das partes ou itens de um controle de seleção pode ser prejudicial. O controle de seleção pode estar à procura de eventos de entrada para saber que a seleção deve mudar.

Nem todos os eventos roteados podem cancelar um rota dessa maneira, e você pode deduzir isso porque eles não terão uma propriedade **Handled**. Por exemplo, [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus) e [**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus) são propagados, mas isso sempre acontece em direção à raiz, e suas classes de dados de eventos não têm uma propriedade **Handled** capaz de influenciar esse comportamento.

##  <a name="input-event-handlers-in-controls"></a>Manipuladores de eventos de entrada em controles

Às vezes, controles específicos do Windows Runtime usam o conceito **Handled** internamente para eventos de entrada. Isso pode dar a impressão de que um evento de entrada nunca ocorre, pois o código do usuário não pode manipulá-lo. Por exemplo, a classe [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) inclui lógica que manipula deliberadamente o evento de entrada geral [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed). Isso ocorre porque botões acionam um evento [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) que é iniciado por entrada do ponteiro pressionado, assim como por outros modos de entrada como teclas de manipulação, tais como a tecla Enter, que podem invocar o botão quando são focadas. Para os objetivos do design de classe **Button**, o evento de dados brutos é manipulado conceitualmente. Os consumidores da classe, como seu código de usuário, podem, em vez disso, interagir com o evento relevante de controle **Click**. Tópicos para classes de controle específicas da API do Windows Runtime costumam registrar o comportamento de manipulação de eventos implementado pela classe. Em alguns casos, é possível alterar o comportamento por meio da substituição dos métodos **On**_Event_. Por exemplo, você pode alterar como a sua classe derivada [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) reage à entrada de tecla substituindo [**Control.OnKeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.onkeydown).

##  <a name="registering-handlers-for-already-handled-routed-events"></a>Registrando manipuladores para eventos roteados já manipulados

Nós explicamos antes que a definição de **Handled** como **true** impede a chamada da maioria dos manipuladores. Mas o método [**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler) oferece uma técnica para você anexar um manipulador que seja sempre invocado para a rota, mesmo que algum outro manipulador já tenha definido **Handled** como **true** nos dados compartilhados do evento. Essa técnica é útil quando você usa um controle que manipulou o evento na sua composição interna ou para a lógica específica do controle, mas você ainda quer responder a ele em uma instância de controle ou na interface do usuário do seu aplicativo. Porém, essa técnica deve ser usada com cuidado, pois pode opor-se ao propósito de **Handled** e possivelmente violar as interações pretendidas de um controle.

Apenas os eventos roteados que têm um identificador de evento roteado correspondente podem usar a técnica de manipulação de eventos [**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler), pois o identificador é uma entrada obrigatória do método **AddHandler**. Veja a documentação de referência de [**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler) para obter uma lista de eventos que têm identificadores de eventos roteados disponíveis. Para a maior parte, essa é a mesma lista dos eventos roteados que mostramos anteriormente. A exceção é que os dois últimos na lista: [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus) e [**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus) não têm um identificador de evento roteado, portanto, você não pode usar **AddHandler** para eles.

## <a name="routed-events-outside-the-visual-tree"></a>Eventos roteados fora da árvore de objetos

Certos objetos participam de uma relação com a árvore visual primária que é conceitualmente semelhante a possuir uma sobreposição sobre os elementos visuais principais. Esses objetos não são parte das relações entre pai e filho comuns que conectam todos os três elementos à raiz visual. Esse é o caso para qualquer [**Popup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup) ou [**ToolTip**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToolTip) exibido. Se quiser manipular eventos roteados em um **Popup** ou em uma **ToolTip**, posicione os manipuladores sobre elementos específicos da interface do usuário que estão no **Popup** ou na **ToolTip**, e não nos elementos **Popup** ou **ToolTip**. Não dependa do roteamento dentro de qualquer composição executada para o conteúdo do **Popup** ou da **ToolTip**. Isso porque o roteamento de eventos para eventos roteados funciona apenas ao longo da árvore visual principal. Um **Popup** ou uma **ToolTip** não é considerado um pai de elementos subsidiários da interface do usuário e nunca recebe o evento roteado, mesmo que esteja tentando usar algo como a tela de fundo padrão de **Popup** como a área de captura de eventos de entrada.

## <a name="hit-testing-and-input-events"></a>Teste de clique e eventos de entrada

Determinar se um elemento está visível para a entrada por mouse, toque e de caneta é chamado de *teste de clique*. Para ações de toque e também para eventos específicos de interação ou de manipulação resultantes de uma ação de toque, é preciso que o elemento esteja visível para teste de clique, para ser a origem do evento e acionar o evento associado à ação. Caso contrário, a ação passa pelo elemento em direção a qualquer elemento subjacente ou aos elementos pai na árvore visual que interage com essa entrada. Há vários fatores que influenciam o teste de clique, mas é possível determinar se um determinado elemento pode acionar eventos de entrada verificando sua propriedade [**IsHitTestVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.ishittestvisible). Esta propriedade retorna **true** apenas quando o elemento segue estes critérios:

- O valor de propriedade [**Visibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) do elemento é [**Visible**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Visibility).
- O valor da propriedade **Background** ou **Fill** do elemento não é **null**. Um valor de [**pincel**](/uwp/api/Windows.UI.Xaml.Media.Brush) **nulo** resulta em transparência e invisibilidade de teste de clique. (Para tornar um elemento transparente, mas também visível para teste de clique, use um pincel [**Transparent**](https://docs.microsoft.com/uwp/api/windows.ui.colors.transparent) em vez de **null**.)

**Observação**  **Background** e **Fill** não são definidos por [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) e, em vez disso, são definidos por diferentes classes derivadas, como [**Control**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control) e [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape). As implicações dos pincéis usados para as propriedades primeiro plano e tela de fundo são as mesmas para teste de clique e eventos de entrada, independentemente da subclasse que implementa as propriedades.

- Se o elemento for um controle, o valor da sua propriedade [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.isenabled) deverá ser **true**.
- É preciso que o elemento tenha dimensões reais em layout. Um elemento em que [**ActualHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight) e [**ActualWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) são iguais a 0 não acionará eventos.

Alguns controles têm regras especiais para teste de hit. Por exemplo, o [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) não tem uma propriedade **Background**, mas ainda pode executar o teste de clique em toda a região de suas dimensões. Os controles [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) e [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) são pressionados com o pressionamento de suas dimensões de retângulo definidas, independentemente do conteúdo transparente, como o canal alfa, no arquivo de origem de mídia que está sendo exibido. Os controles [**WebView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) têm comportamento de teste de clique especial porque a entrada pode ser tratada pelos eventos HTML hospedado e acionar script.

A maioria das classes [**Panel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Panel) e [**Border**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border) não executa teste de clique na tela de fundo, mas essas classes ainda podem manipular os eventos de entrada do usuário que são roteados dos elementos que os contêm.

Você pode determinar quais elementos estão localizados na mesma posição de um evento de entrada do usuário, independentemente de se os elementos são passíveis de teste de hit. Para fazer isso, chame o método [**FindElementsInHostCoordinates**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates). Como o nome sugere, esse método localiza os elementos em um local em relação a um elemento host especificado. Entretanto, transformações aplicadas e mudanças de layout podem ajustar o sistema de coordenadas relativas de um elemento e, portanto, influenciar quais elementos são encontrados em um determinado local.

## <a name="commanding"></a>Execução de comandos

Um pequeno número de elementos da interface do usuário dão suporte a *comandos*. Comandos usam eventos roteados relacionados a entrada na sua implementação subjacente e permite o processamento de entrada relacionada da interface do usuário (uma determinada ação do ponteiro, uma tecla de aceleração específica) invocando um único manipulador de comandos. Se comandos estiverem disponíveis para um elemento da interface do usuário, considere usar as respectivas APIs de comando em vez de qualquer evento de entrada à parte. Em geral, você usa uma referência **Binding** em propriedades de uma classe que define o modelo de exibição para dados. As propriedades mantêm comandos nomeados que implementam o padrão de comandos **ICommand** específico de cada linguagem. Para obter mais informações, consulte [**ButtonBase.Command**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command).

## <a name="custom-events-in-the-windows-runtime"></a>Eventos personalizados no Windows Runtime

Para fins de definir eventos personalizados, o modo de adicionar o evento e o que isso significa para seu design de classe dependem bastante da linguagem de programação sendo usada.

- Para C# e Visual Basic, você está definindo um evento de CLR. Você pode usar o padrão de evento .NET, desde que não esteja usando acessadores personalizados(**add**/**remove**). Outras dicas:
    - Para o manipulador de eventos, é uma ótima ideia usar [**System.EventHandler<TEventArgs>** ](https://docs.microsoft.com/dotnet/api/system.eventhandler-1?redirectedfrom=MSDN) porque ele tem conversão interna para o delegado de evento genérico do Windows Runtime [**EventHandler<T>** ](https://docs.microsoft.com/uwp/api/windows.foundation.eventhandler).
    - Não use como base de sua classe de dados de eventos o [**System.EventArgs**](https://docs.microsoft.com/dotnet/api/system.eventargs?redirectedfrom=MSDN) porque ele não é convertido para o Windows Runtime. Use uma classe de dados de eventos existente ou nenhuma classe base.
    - Se estiver usando acessadores personalizados, veja [Eventos personalizados e acessadores de eventos nos Componentes de Tempo de Execução do Windows](https://docs.microsoft.com/previous-versions/windows/apps/hh972883(v=vs.140)).
    - Se não tiver certeza do que é o padrão de evento .NET padrão, veja [Definindo eventos para classes personalizadas do Silverlight](https://docs.microsoft.com/previous-versions/windows/). Esse tópico foi escrito para o Microsoft Silverlight mas ainda é uma ótima referência do código e dos conceitos para o padrão de evento .NET padrão.
- Para C++/CX, veja [Eventos (C++/CX)](https://docs.microsoft.com/cpp/cppcx/events-c-cx).
    - Use referências nomeadas mesmo para seus próprios usos de eventos personalizados. Não use lambda para eventos personalizados, pois ele pode criar uma referência circular.

Você não pode declarar um evento roteado personalizado para o Tempo de Execução do Windows; eventos roteados são limitados ao conjunto proveniente do Tempo de Execução do Windows.

A definição de um evento personalizado normalmente é feita como parte do exercício de definir um controle personalizado. É um padrão comum ter uma propriedade de dependência que tem um retorno de chamada de propriedades alteradas, e também definir um evento personalizado que é acionado pelo retorno de chamada de propriedades de dependência em alguns ou todos os casos. Os consumidores do seu controle não têm acesso ao retorno de chamada de propriedades alteradas que você definiu, mas ter um evento de notificação disponível é o próximo passo interessante. Para saber mais, consulte [Propriedades de dependência personalizada](custom-dependency-properties.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Visão geral do XAML](xaml-overview.md)
* [Início Rápido: Entrada por toque](https://docs.microsoft.com/previous-versions/windows/apps/hh465387(v=win.10))
* [Interações de teclado](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions)
* [Eventos e delegados do .NET](https://go.microsoft.com/fwlink/p/?linkid=214364)
* [Criando componentes de Windows Runtime](https://docs.microsoft.com/previous-versions/windows/apps/hh441572(v=vs.140))
* [**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler)
