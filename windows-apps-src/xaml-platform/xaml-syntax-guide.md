---
author: jwmsft
description: Explicamos as regras de sintaxe XAML e a terminologia que descreve as restrições ou as opções disponíveis para a sintaxe XAML.
title: Guia de sintaxe do XAML
ms.assetid: A57FE7B4-9947-4AA0-BC99-5FE4686B611D
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1fe2460dfc5ab11a9168f1d1d87207d2b9490026
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7305426"
---
# <a name="xaml-syntax-guide"></a>Guia de sintaxe do XAML


Explicamos as regras de sintaxe XAML e a terminologia que descreve as restrições ou as opções disponíveis para a sintaxe XAML. Você achará este tópico útil se for um usuário iniciante no uso da linguagem XAML, tiver interesse em se atualizar sobre a terminologia ou um aspecto da sintaxe, ou tiver curiosidade sobre o funcionamento da linguagem XAML e queira obter informações complementares e contextuais.

## <a name="xaml-is-xml"></a>XAML é XML

A Extensible Application Markup Language (XAML) tem uma sintaxe básica compilada em XML e, por definição, o que é válido em XAML deve ser válido em XML. Mas o XAML também tem os próprios conceitos de sintaxe que ampliam o XML. Uma determinada entidade XML pode ser válida em XML simples, mas essa sintaxe pode ter um significado diferente e mais completo enquanto XAML. Este tópico explica esses conceitos da sintaxe XAML.

## <a name="xaml-vocabularies"></a>Vocabulários XAML

Uma área em que o XAML difere da maioria dos usos de XML é que o XAML normalmente não é reforçado com um esquema, tal como um arquivo XSD. É por isso que o XAML pretende ser extensível, é isso o que o "X" no acrônimo XAML significa. Quando o XAML é analisado, os elementos e os atributos que você referencia no XAML devem existir em alguma representação de código de suporte, seja nos tipos básicos definidos pelo Windows Runtime ou nos tipos que estendem ou são baseados no Tempo de Execução do Windows. A documentação do SDK às vezes se refere aos tipos que já estão inseridos no Tempo de Execução do Windows e podem ser usados no XAML como se fosse o *vocabulário XAML* para o Tempo de Execução do Windows. O Microsoft Visual Studio ajuda você a produzir marcação que seja válida nesse vocabulário XAML. O Visual Studio também pode incluir seus tipos personalizados para uso de XAML, desde que a origem desses tipos seja referenciada corretamente no projeto. Para saber mais sobre XAML e tipos personalizados, consulte [Namespaces XAML e mapeamento de namespace](xaml-namespaces-and-namespace-mapping.md).

##  <a name="declaring-objects"></a>Declaração de objetos

Com frequência, os programadores pensam em termos de objetos e membros, enquanto uma linguagem de marcação é conceitualizada como elementos e atributos. Basicamente, um elemento declarado em marcação XAML se torna um objeto em uma representação de objeto em tempo de execução subjacente. Para criar um objeto de tempo de execução para seu aplicativo, declare um elemento XAML na marcação XAML. O objeto é criado quando o Tempo de Execução do Windows carrega seu XAML.

Um arquivo XAML sempre tem exatamente um elemento servindo de raiz, o qual declara o objeto que será a raiz conceitual de alguma estrutura de programação, por exemplo, uma página, ou do gráfico de objeto de toda a definição de tempo de execução de um aplicativo.

Em termos de sintaxe XAML, há três maneiras de declarar objetos em XAML:

-   **Diretamente, usando a sintaxe do elemento do objeto:** usa marcas de abertura e fechamento para instanciar um objeto como um elemento na forma XML. Você pode usar essa sintaxe para declarar objetos raiz ou para criar objetos aninhados que definem valores de propriedade.
-   **Indiretamente, usando a sintaxe do atributo:** usa um valor de cadeia de caracteres inline que tem instruções sobre como criar um objeto. O analisador de XAML usa essa cadeia de caracteres para definir o valor de uma propriedade como um valor de referência recém-criado. O suporte a ele é limitado a determinados objetos e propriedades comuns.
-   Usando uma extensão de marcação.

Isso não significa que você sempre terá a opção de qualquer sintaxe para criação de objetos em um vocabulário XAML. Alguns objetos podem ser criados apenas com o uso da sintaxe de elemento de objeto. Outros podem ser criados apenas com a definição inicial em um atributo. Na realidade, os objetos que podem ser criados com a sintaxe de atributo ou de elemento de objeto são relativamente raros nos vocabulários XAML. Mesmo se ambas as formas de sintaxe forem possíveis, uma delas será mais comum por uma questão de estilo.
Também há técnicas que podem ser usadas em XAML para referenciar objetos existentes, em vez de criar novos valores. Os objetos existentes podem ser definidos em outras áreas da XAML ou existir implicitamente por meio de algum comportamento da plataforma e de seus aplicativos ou modelos de programação.

### <a name="declaring-an-object-by-using-object-element-syntax"></a>Declaração de um objeto usando a sintaxe de elemento de objeto

Para declarar um objeto com a sintaxe de elemento de objeto, você cria marcas como esta: `<objectName>  </objectName>`, onde *objectName* é o nome do tipo do objeto que você deseja instanciar. A seguir está um uso de elemento de objeto para declarar um objeto [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267):

```xml
<Canvas>
</Canvas>
```

Se o objeto não contiver outros objetos, você poderá declarar o elemento de objeto usando uma marca de fechamento automático em vez de um par de abertura/fechamento:  `<Canvas />`

### <a name="containers"></a>Contêineres

Muitos objetos usados como elementos da interface do usuário, como o [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267), podem conter outros objetos. Eles ocasionalmente são chamados de contêineres. O exemplo a seguir mostra um contêiner **Canvas** com um elemento, um [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle).

```xml
<Canvas>
  <Rectangle />
</Canvas>
```

### <a name="declaring-an-object-by-using-attribute-syntax"></a>Declaração de um objeto usando a sintaxe de atributo

Como esse comportamento está relacionado à configuração da propriedade, falaremos mais sobre isso nas seções seguintes.

### <a name="initialization-text"></a>Texto de inicialização

Para alguns objetos, você pode declarar novos valores usando o texto interno utilizado como valores de inicialização para construção. Em XAML, essa técnica e sintaxe é chamada de *texto de inicialização*. Conceitualmente, o texto de inicialização é semelhante a chamar um construtor que tem parâmetros. O texto de inicialização é útil para definir os valores iniciais de certas estruturas.

Normalmente você usa uma sintaxe de elemento de objeto com texto de inicialização se você quer um valor de estrutura com **x:Key**, para que ele possa existir em [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794). Você poderá fazer isso se compartilhar o valor dessa estrutura entre várias propriedades de destino. Para algumas estruturas, não é possível usar a sintaxe do atributo para definir os valores da estrutura: o texto de inicialização é a única maneira de produzir um recurso [**CornerRadius**](https://msdn.microsoft.com/library/windows/apps/br242343), [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864), [**GridLength**](https://msdn.microsoft.com/library/windows/apps/br208754) ou [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723) útil e compartilhável.

Este exemplo abreviado usa o texto de inicialização para especificar valores para [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864), especificando neste caso valores que definem **Esquerda** e **Direita** como 20, e **Superior** e **Inferior** como 10. Este exemplo mostra a **Espessura** criada como um recurso inserido e, em seguida, a referência para esse recurso. Para saber mais sobre o texto de inicialização de [**Espessura**](https://msdn.microsoft.com/library/windows/apps/br208864), veja [**Espessura**](https://msdn.microsoft.com/library/windows/apps/br208864).

```xml
<UserControl ...>
  <UserControl.Resources>
    <Thickness x:Key="TwentyTenThickness">20,10</Thickness>
    ....
  </UserControl.Resources>
  ...
  <Grid Margin="{StaticResource TwentyTenThickness}">
  ...
  </Grid>
</UserControl ...>
```

**Observação**algumas estruturas não podem ser declaradas como elementos de objeto. Não há suporte ao texto de inicialização e eles não podem ser usados como recursos. Você deve usar uma sintaxe de atributo para definir as propriedades com esses valores em XAML. Esses tipos são: [**Duration**](https://msdn.microsoft.com/library/windows/apps/br242377), [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/br210411), [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870), [**Rect**](https://msdn.microsoft.com/library/windows/apps/br225994) e [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995).

## <a name="setting-properties"></a>Definição de propriedades

Você pode definir propriedades de objeto declarados usando a sintaxe de elemento de objeto. Há muitas maneiras de definir propriedades em XAML:

-   Por meio da sintaxe de atributo.
-   Por meio da sintaxe de elemento de propriedade.
-   Por meio da sintaxe de elemento, onde o conteúdo (texto interno ou elementos filho) define a propriedade de conteúdo XAML de um objeto.
-   Por meio de uma sintaxe de coleção (geralmente, a sintaxe de coleção implícita).

Assim como a declaração de objeto, essa lista não implica que nenhuma propriedade poderia ser definida com cada uma das técnicas. Algumas propriedades dão suporte para apenas uma das técnicas.
Algumas propriedades dão suporte a mais de uma forma; por exemplo, há propriedades que podem usar sintaxe de elemento de propriedade ou sintaxe de atributo. O que é possível depende da propriedade e do tipo de objeto usado por ela. Na referência da API do Tempo de Execução do Windows, você verá os usos de XAML possíveis na seção **Sintaxe**. Às vezes, existe um uso alternativo que funcionaria mas seria muito detalhado. Esses usos detalhados nem sempre são mostrados porque estamos tentando mostrar as práticas recomendadas ou os cenários da vida real para uso dessa propriedade no XAML. A diretriz da sintaxe XAML é fornecida nas seções  **Uso de XAML** das páginas de referência das propriedades que podem ser definidas no XAML.

Algumas propriedades de objetos não podem ser definidas em XAML de maneira alguma e só podem ser definidas com código. Normalmente é mais apropriado trabalhar com essas propriedades em code-behind, e não em XAML.

Uma propriedade somente leitura não pode ser definida em XAML. Mesmo em código, o tipo proprietário teria de dar suporte a alguma outra maneira de defini-lo, como uma sobrecarga de construtor, um método auxiliar ou um suporte de propriedade calculada. Uma propriedade calculada depende dos valores de outras propriedades configuráveis, bem como possivelmente um evento com manipulação interna; esses recursos ficam disponíveis no sistema de propriedades de dependência. Para saber mais sobre como as propriedades de dependência são úteis para o suporte de propriedade calculada, consulte [Visão geral das propriedades de dependência](dependency-properties-overview.md).

A sintaxe de coleção em XAML faz parecer que você está configurando uma propriedade somente leitura, mas isso não é verdade. Consulte mais adiante a seção "Definição de uma propriedade usando uma sintaxe de coleção" neste tópico.

### <a name="setting-a-property-by-using-attribute-syntax"></a>Definição de uma propriedade usando a sintaxe de atributo

Definir um valor de atributo é a forma típica com a qual você define um valor de propriedade em uma linguagem de marcação, como XML ou HTML. A definição de atributos XAML é semelhante à maneira de definir valores de atributos em XML. O nome do atributo é especificado em algum ponto dentro das marcas depois do nome do elemento, separado deste por pelo menos um espaço em branco. O nome do atributo é seguido de um sinal de igual. O valor do atributo é inserido entre aspas. As aspas podem ser duplas ou simples, desde que sejam correspondentes entre si, circunscrevendo o valor. O valor do atributo em si deve ser expressado como uma cadeia de caracteres. A cadeia de caracteres normalmente contém números, mas, para o XAML, todos os valores de atributo são valores de cadeia de caracteres até que o analisador XAML se envolva e faça alguma conversão básica de valores.

Este exemplo usa a sintaxe de quatro atributos para definir as propriedades [**Name**](https://msdn.microsoft.com/library/windows/apps/br208735), [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width), [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) e [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill) de um objeto [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle).

```xml
<Rectangle Name="rectangle1" Width="100" Height="100" Fill="Blue" />
```

### <a name="setting-a-property-by-using-property-element-syntax"></a>Definição de uma propriedade usando a sintaxe de elemento de propriedade

Muitas propriedades de um objeto podem ser definidas com a sintaxe de elemento de propriedade. Um elemento de propriedade tem aparência semelhante a este: `<`*object*`.`*property*`>`.

Para usar a sintaxe de elemento de propriedade, você cria elementos de propriedade XAML para a propriedade que deseja definir. Em XML padrão, esse elemento seria simplesmente considerado um elemento com um ponto em seu nome. No entanto, em XAML, o ponto no nome do elemento o identifica como um elemento de propriedade, onde *property* é membro de *object* na implementação de um modelo de objeto subjacente. Para usar a sintaxe de elemento de propriedade, deve ser possível especificar um elemento de objeto para "preencher" as marcas de elemento de propriedade. Um elemento de propriedade sempre terá algum conteúdo (elemento único, vários elementos ou texto interno); não faz sentido ter um elemento de propriedade de fechamento automático.

Na gramática a seguir, *property* é o nome da propriedade que você deseja definir e *propertyValueAsObjectElement* é um único elemento de objeto, cuja expectativa é satisfazer os requisitos de tipo de valor da propriedade.

`<`*objeto*`>`

`<`*object*`.`*property*`>`

*propertyValueAsObjectElement*

`</`*object*`.`*property*`>`

`</`*objeto*`>`

O exemplo a seguir usa a sintaxe de elemento de propriedade para definir o [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill) de um [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) com um elemento de objeto [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962). (Em **SolidColorBrush**, [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) é definido como um atributo). O resultado analisado desse XAML é idêntico ao exemplo anterior de XAML que define **Fill** usando a sintaxe de atributo.

```xml
<Rectangle
  Name="rectangle1"
  Width="100" 
  Height="100"
> 
  <Rectangle.Fill> 
    <SolidColorBrush Color="Blue"/> 
  </Rectangle.Fill>
</Rectangle>
```

### <a name="xaml-vocabularies-and-object-oriented-programming"></a>Vocabulários XAML e programação orientada a objeto

As propriedades e eventos como aparecem enquanto membros XAML de um tipo XAML do Windows Runtime normalmente são herdadas de tipos base. Considere este exemplo: `<Button Background="Blue" .../>` A propriedade [**Background**](https://msdn.microsoft.com/library/windows/apps/br209395) não é uma propriedade declarada imediatamente na classe [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265). Em vez disso, **Background** é herdado da classe base [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390). Na realidade, se você analisar o tópico de referência de **Button** verá que as listas de membros contêm pelo menos um membro herdado de cada uma das classes base sucessivas: [**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/br227736), [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390), [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706), [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911), [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356). Na lista **Propriedades**, todas as propriedades somente leitura e as propriedades de coleção são herdadas em um sentido de vocabulário XAML. Eventos (como os vários eventos [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)) também são herdados.

Se você usar a referência de Windows Runtime para diretriz XAML, o nome do elemento mostrado numa sintaxe ou mesmo em exemplo de código às vezes será para o tipo que originariamente define a propriedade, porque esse tópico de referência é compartilhado por todos os tipos possíveis que o herdam de uma classe base. Se você usar o IntelliSense para XAML do Visual Studio no editor de XML, o IntelliSense e seus menus suspensos farão um excelente trabalho de unir a herança e fornecer uma precisa lista de atributos disponíveis para configuração, depois que você começar um elemento de objeto para uma instância de classe.

### <a name="xaml-content-properties"></a>Propriedades de conteúdo XAML

Alguns tipos definem uma de suas propriedades para que ela permita a sintaxe de conteúdo XAML. Para a propriedade de conteúdo XAML de um tipo, você pode omitir o elemento da propriedade em questão ao especificá-lo em XAML. Como alternativa, você pode definir a propriedade como um valor de texto interno, inserindo esse texto interno diretamente nas marcas de elemento de objeto do tipo proprietário. As propriedades de conteúdo XAML dão suporte a sintaxe de marcação simples dessa propriedade e tornam o XAML mais legível aos humanos com a redução do aninhamento.

Se uma sintaxe de conteúdo XAML estiver disponível, essa sintaxe será mostrada nas seções "XAML" da **Sintaxe** dessa propriedade na documentação de referência do Windows Runtime. Por exemplo, a página de propriedade [**Child**](https://msdn.microsoft.com/library/windows/apps/br209258) para [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) mostra a sintaxe de conteúdo XAML em vez da sintaxe de elemento de propriedade para definir o valor **Border.Child** de objeto único d **Border**, assim:

```xml
<Border>
  <Button .../>
</Border>
```

Se a propriedade declarada como propriedade de conteúdo XAML for o tipo **Object** ou o tipo **String**, a sintaxe de conteúdo XAML dará suporte ao que é basicamente texto interno no modelo de documento XML: uma cadeia de caracteres entre as marcas de objeto de abertura e fechamento. Por exemplo, a página de propriedade [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) de [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) mostra a sintaxe de conteúdo XAML que tem um valor de texto interno para definir **Text**, mas a cadeia de caracteres "Text" nunca aparece na marcação. Aqui está um uso de exemplo:

```xml
<TextBlock>Hello!</TextBlock>
```

Se existir uma propriedade de conteúdo XAML para uma classe, isso será indicado no tópico de referência da classe, na seção "Atributos". Procure o valor do [**ContentPropertyAttribute**](https://msdn.microsoft.com/library/windows/apps/br228011). Esse atributo usa um campo nomeado "Name". O valor de "Name" é o nome da propriedade dessa classe que é a propriedade de conteúdo XAML. Por exemplo, na página de referência [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250), você verá isto: ContentProperty("Name=Child").

Uma regra de sintaxe XAML importante que deve ser mencionada é que não é possível misturar a propriedade de conteúdo XAML com outros elementos de propriedade definidos no elemento. A propriedade de conteúdo XAML deve ser definida inteiramente antes de quaisquer elementos de propriedade, ou inteiramente depois. Por exemplo, este é um XAML inválido:

``` syntax
<StackPanel>
  <Button>This example</Button>
  <StackPanel.Resources>
    <SolidColorBrush x:Key="BlueBrush" Color="Blue"/>
  </StackPanel.Resources>
  <Button>... is illegal XAML</Button>
</StackPanel>
```

## <a name="collection-syntax"></a>Sintaxe de coleção

Todas as sintaxes mostradas até o momento definem propriedades de objetos únicos. No então, vários cenários de interface do usuário exigem que um determinado elemento pai tenha diversos elementos filho. Por exemplo, uma interface do usuário de um formulário de entrada precisa de vários elementos de caixa de texto, alguns rótulos e talvez um botão "Enviar". Mesmo assim, se você usasse um modelo de objeto de programação para acessar esses vários elementos, eles geralmente seriam itens de uma única propriedade de coleção, em vez de cada item ser o valor de propriedades distintas. A linguagem XAML dá suporte para diversos elementos filho, além de dar suporte para um modelo típico de coleção subjacente tratando as propriedades que usam um tipo de coleção como implícitas e realizando um tratamento especial para quaisquer elementos filho de um tipo de coleção.

Muitas propriedades de coleção também são identificadas como propriedade de conteúdo XAML da classe. A combinação entre processamento de coleção implícito e sintaxe de conteúdo XAML com frequência é vista em tipos usados para composição de controles, como painéis, modos de exibição ou controles de item. Os exemplos a seguir mostram o XAML mais simples possível para a composição de dois elementos de interface do usuário par dentro de um [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635).

```xml
<StackPanel>
  <TextBlock>Hello</TextBlock>
  <TextBlock>World</TextBlock>
</StackPanel>
```

### <a name="the-mechanism-of-xaml-collection-syntax"></a>O mecanismo da sintaxe de coleção XAML

À primeira vista, pode parecer que o XAML está habilitando um "conjunto" de propriedades de coleção somente leitura. Na verdade, o que o XAML habilita neste caso é a adição de itens a uma coleção existente. A linguagem e os processadores XAML que implementam suporte a XAML dependem de uma convenção de tipos de coleção subjacente para habilitar essa sintaxe. Normalmente há uma propriedade subjacente, como um indexador ou uma propriedade **Items** que faz referência a itens específicos da coleção. Essa propriedade geralmente não é explícita na sintaxe XAML. Para coleções, o mecanismo subjacente para análise de XAML não é uma propriedade, mas um método. Especificamente, o método **Add** na maioria dos casos. Quando o processador XAML encontra um ou mais elementos de objeto em uma sintaxe de coleção XAML, cada um desses objetos é criado com base em um elemento e, em seguida, cada novo objeto é adicionado em ordem à coleção contêiner, chamando o método **Add**.

Quando um analisador XAML adiciona itens a uma coleção, é a lógica do método **Add** que determina se um determinado elemento XAML tem permissão para ser um item filho do objeto de coleção. Muitos tipos de coleção são fortemente tipados pela implementação subjacente, o que significa que o parâmetro de entrada de **Add** espera que qualquer item passado deve corresponder ao tipo do parâmetro **Add**.

Para propriedades de coleção, tenha cuidado ao tentar especificar a coleção explicitamente como um elemento de objeto. Um analisador XAML criará um novo objeto sempre que encontrar um elemento de objeto. Se a propriedade de coleção que você está tentando usar for somente leitura, isso poderá gerar uma exceção da análise XAML. Basta usar a sintaxe de coleção implícita para não ver essa exceção.

## <a name="when-to-use-attribute-or-property-element-syntax"></a>Quando usar a sintaxe de atributo ou de elemento de propriedade

Todas as propriedades com suporte em XAML dão suporte para sintaxe de elemento de propriedade ou de atributo para configuração direta de valores, mas é possível que não deem suporte para cada sintaxe de maneira intercambiável. Algumas propriedades dão suporte para ambas as sintaxes e outras dão suporte para opções de sintaxe adicionais, como uma propriedade de conteúdo XAML. O tipo de uma sintaxe XAML compatível com uma propriedade depende do tipo de objeto que essa propriedade usa como seu tipo de propriedade. Se o tipo de propriedade for um tipo primitivo, como um duplo (flutuante ou decimal), um inteiro, um booliano ou uma cadeia de caracteres, a propriedade sempre dará suporte à sintaxe de atributo.

Também será possível usar a sintaxe de atributo para definir uma propriedade se o tipo de objeto usado para definir essa propriedade puder ser criado por meio do processamento de uma cadeia de caracteres. Para primitivos, esse sempre é o caso; a conversão de tipos é inserida no analisador. Entretanto, outros tipos de objeto também podem ser criados usando uma cadeia de caracteres especificada como um valor de atributo, em vez de um elemento de objeto em um elemento de propriedade. Para que isso funcione, é necessário haver uma conversão de tipo subjacente, com suporte dessa propriedade específica ou com suporte geralmente para todos os valores que usam esse tipo de propriedade. O valor da cadeia de caracteres do atributo é usado para definir as propriedades que são importantes para a inicialização do novo valor de objeto. Um conversor de tipo específico também pode criar diferentes subclasses de um tipo de propriedade comum, dependendo de como ele exclusivamente processa informações na cadeia de caracteres. Os tipos de objeto que dão suporte para esse comportamento têm uma gramática especial listada na seção de sintaxe da documentação de referência. Como exemplo, a sintaxe XAML de [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) mostra como uma sintaxe de atributo pode ser usada para criar um novo valor [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) para qualquer propriedade do tipo **Brush** (e há muitas propriedades **Brush** no XAML do Windows Runtime).

## <a name="xaml-parsing-logic-and-rules"></a>Lógica e regras de análise XAML

Às vezes, é elucidativo ler o XAML de maneira semelhante ao modo como um analisador XAML deve lê-lo: como um conjunto de tokens de cadeias de caracteres encontrados em ordem linear. Uma analisador XAML deve interpretar esses tokens sob um conjunto de regras que fazem parte da definição de como o XAML funciona.

Definir um valor de atributo é a forma típica com a qual você define um valor de propriedade em uma linguagem de marcação, como XML ou HTML. Na sintaxe a seguir, *objectName* é o objeto para o qual você deseja criar uma instância, *propertyName* é o nome da propriedade que você deseja definir para esse objeto e *propertyValue* é o valor a ser definido.

```xml
<objectName propertyName="propertyValue" .../>

-or-

<objectName propertyName="propertyValue">

...<!--element children -->

</objectName>
```

Qualquer uma dessas sintaxes permite que você declare um objeto e defina uma propriedade para esse objeto. Apesar de o primeiro exemplo ser um elemento único na marcação, existem etapas discretas em relação a como um processador XAML analisa essa marcação.

Em primeiro lugar, a presença do elemento de objeto indica que deve ser criada uma instância para um novo objeto *objectName* deve ser instanciado. Somente depois que essa instância existir a propriedade de instância *propertyName* poderá ser definida para ela.

Outra regra do XAML é que os atributos de um elemento devem ser capazes de ser definidos em qualquer ordem. Por exemplo, não há diferença entre `<Rectangle Height="50" Width="100" />` e `<Rectangle Width="100"  Height="50" />`. A ordem de uso é uma questão de estilo.

**Observação**os designers de XAML geralmente promovem convenções de ordenação se você usar superfícies de design diferentes do editor de XML, mas é possível editar livremente esse XAML mais tarde, para reordenar os atributos ou introduzir novos.

## <a name="attached-properties"></a>Propriedades anexadas

O XAML estende o XML ao adicionar um elemento de sintaxe conhecido como *propriedade anexada*. De modo semelhante à sintaxe de elemento de propriedade, a sintaxe de propriedade anexada contém um ponto e esse ponto tem um significado especial para a análise XAML. Especificamente, o ponto separa o provedor do proprietário da propriedade anexada e do nome da propriedade.

Em XAML, você define propriedades anexadas usando a sintaxe *AttachedPropertyProvider*.*PropertyName* Aqui está um exemplo de como definir a propriedade anexada [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) em XAML:

```xml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

É possível definir a propriedade anexada em elementos que não têm uma propriedade com esse nome no tipo de suporte e, sendo assim, eles funcionam de maneira semelhante a uma propriedade global ou a um atributo definido por um namespace XML diferente como o atributo **xml:space**.

No XAML do Windows Runtime, você verá as propriedades anexadas que dão suporte a estes cenários:

-   Elementos filho podem informar aos painéis de contêiner pai como deve ser seu comportamento no layout: [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267), [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704), [**VariableSizedWrapGrid**](https://msdn.microsoft.com/library/windows/apps/br227651).
-   Os usos de controle podem influenciar o comportamento de uma parte de controle importante que vem do modelo de controle: [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527), [**VirtualizingStackPanel**](https://msdn.microsoft.com/library/windows/apps/br227689).
-   Usando um serviço que está disponível em uma classe relacionada, onde o serviço e a classe que o utiliza não compartilha a herança: [**Typography**](https://msdn.microsoft.com/library/windows/apps/hh702143), [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/br209021), [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/br209081), [**ToolTipService**](https://msdn.microsoft.com/library/windows/apps/br227609).
-   Direcionamento de animações: [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490).

Para saber mais, veja [Visão geral de propriedades anexadas](attached-properties-overview.md).

## <a name="literal--values"></a>Valores "{" literais

Como o símbolo da chave de abertura \{ é o início da sequência de extensão de marcação, use uma sequência de escape para especificar um valor de cadeia de caracteres literais que comece com "\{". A sequência de escape é "\{\}". Por exemplo, para especificar um valor de cadeia de caracteres que seja uma única chave de abertura, especifique o valor de atributo como "\{\}\{". Você também pode usar as aspas alternativas (por exemplo, um caractere **'** em um valor de atributo delimitado por **""**) para fornecer um valor "\{" como uma cadeia de caracteres.

**Observação**"\}" funciona também se ele está dentro de um atributo entre aspas.
 
## <a name="enumeration-values"></a>Valores de enumeração

Muitas propriedades na API de Tempo de Execução do Windows usam enumerações como valores. Se o membro for uma propriedade de leitura-gravação, você poderá definir essa propriedade fornecendo um valor de atributo. Identifique qual valor de enumeração será usado como valor da propriedade usando o nome não qualificado do nome da constante. Por exemplo, veja como definir [**UIElement.Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) em XAML:`<Button Visibility="Visible"/>`. Aqui "Visível" como uma cadeia de caracteres é mapeado diretamente para uma constante nomeada da enumeração [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br209006), **Visible**.

-   Não use uma forma qualificada, pois ela não funcionará. Por exemplo, este é um XAML inválido: `<Button Visibility="Visibility.Visible"/>`.
-   Não use o valor da constante. Em outras palavras, não confie no valor inteiro da enumeração que aparece explícita ou implicitamente, dependendo de como a enumeração foi definida. Embora isso pareça funcionar, não é uma boa prática tanto no XAML como em código, pois você está confiando no que poderia ser um detalhe de implementação transitório. Por exemplo, não faça isto: `<Button Visibility="1"/>`.

**Observação**nos tópicos de referência para APIs que usam XAML e enumerações, clique no link para o tipo de enumeração na seção de **valor de propriedade** da **sintaxe**. Isso vincula à página de enumeração na qual você descobre as constantes nomeadas referentes a essa enumeração.

As enumerações podem ser relacionadas a sinalizador, ou seja, atribuídas com **FlagsAttribute**. Se você precisar especificar uma combinação de valores para uma enumeração de sinalizadores como um valor de atributo XAML, use o nome de cada constante de enumeração, com uma vírgula (,) entre cada nome, sem espaços. Atributos do tipo sinalizador não são comuns no vocabulário XAML do Windows Runtime, mas [**ManipulationModes**](https://msdn.microsoft.com/library/windows/apps/br227934) é um exemplo em que definir um valor de enumeração do tipo sinalizador em XAML não tem suporte.

## <a name="interfaces-in-xaml"></a>Interfaces em XAML

Raramente, você verá uma sintaxe XAML em que o tipo de propriedade seja uma interface. Em sistema de tipo XAML, um tipo que implementa essa interface é aceitável como um valor quando analisado. Deve haver uma instância criada desse tipo disponível para ser usada como o valor. Você vê uma interface usada como tipo na sintaxe XAML para as propriedades [**Command**](https://msdn.microsoft.com/library/windows/apps/br227740) e [**CommandParameter**](https://msdn.microsoft.com/library/windows/apps/br227741) de [**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/br227736). Essas propriedades permitem padrões de design MVVM (Model-View-ViewModel), em que a interface de **ICommand** é o contrato que define como os modos de exibição e os modelos interagem.

## <a name="xaml-placeholder-conventions-in-windows-runtime-reference"></a>Referência de convenções de espaço reservado XAML no Tempo de Execução do Windows

Se você examinou algum dos tópicos da seção **Sintaxe** relacionados às APIs do Tempo de Execução do Windows que podem usar XAML, provavelmente viu que a sintaxe inclui alguns espaços reservados. Sintaxe XAML é diferente das extensões de componente c#, Microsoft Visual Basic ou VisualC + + (C++ c++ /CX) sintaxe porque a sintaxe XAML é uma sintaxe de uso. Ela sugere seu uso eventual de seus próprios arquivos XAML, mas sem ser muito rígida com relação aos valores que você pode utilizar. Normalmente, o uso descreve um tipo de gramática que combina literais e espaços reservados e define alguns dos espaços reservados na seção **Valores de XAML**.

Quando você vê nomes de tipos/elementos na sintaxe XAML de uma propriedade, o nome que aparece refere-se ao tipo que originalmente define a propriedade. Porém, o XAML do Windows Runtime permite um modelo de herança de classe para as classes baseadas em [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356). Dessa forma, você sempre pode usar um atributo em uma classe que não seja literalmente a classe de definição, mas que em vez disso derive da classe que definiu primeiro a propriedade/atributo. Por exemplo, você pode definir [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) como um atributo em qualquer classe derivada de [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) usando uma herança mais profunda. Por exemplo: `<Button Visibility="Visible" />`. Portanto, não entenda tão literalmente o nome do elemento mostrado em qualquer sintaxe de uso de XAML; a sintaxe pode ser viável para elementos que representam essa classe, e também elementos que representam uma classe derivada. Nos casos em que é raro ou impossível que o tipo mostrado como o elemento de definição seja usado em no mundo real, o nome desse tipo será deliberadamente colocado em minúsculas na sintaxe. Por exemplo, a sintaxe que você vê em **UIElement.Visibility** é:

``` syntax
<uiElement Visibility="Visible"/>
-or-
<uiElement Visibility="Collapsed"/>
```

Muitas seções de sintaxe XAML incluem espaços reservados no "Uso" que depois são definidos em uma seção **Valores XAML** que fica logo abaixo da seção **Sintaxe**.

As seções de uso XAML também empregam diversos espaços reservados generalizados. Esses espaços reservados não são sempre redefinidos nos **Valores XAML**, já que você deduz ou eventualmente sabe o que eles representam. Achamos que a maioria dos leitores ficaria cansada de tanto vê-los nos **Valores XAML** por isso os deixamos de fora das definições. Para referência, veja uma lista de alguns desses espaços reservados e o que eles significam em termos gerais:

-   *object*: teoricamente, qualquer valor de objeto, mas em geral quase limitado a determinados tipos de objetos, como a opção entre cadeia de caracteres ou objeto, e você deve conferir os Comentários na página de referência para saber mais.
-   *object* *property*: *object* *property* junto com outros é usado nos casos em que a sintaxe mostrada refere-se a um tipo que pode ser usado como valor de atributo em várias propriedades. Por exemplo, o **Xaml Attribute Usage** mostrado para [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) inclui: <*object* *property*="*predefinedColorName*"/>
-   *eventhandler*: aparece como o valor de atributo de cada sintaxe XAML mostrada para um atributo de evento. O que você especifica aqui é o nome de uma função de manipulador de eventos. Essa função tem que ser definida no code-behind da página XAML. No nível de programação, essa função tem que corresponder à assinatura do delegado do evento que você está manipulando, senão o código de seu aplicativo não vai ser compilado. Mas isso é realmente uma consideração de programação, e não de XAML; portanto, nós não sugerimos nada sobre o tipo de delegado na sintaxe XAML. Para saber que delegado você deve implementar em um evento, veja a seção **Informações do evento** do tópico de referência do evento, na linha da tabela chamada **Delegado**.
-   *enumMemberName*: mostrado na sintaxe do atributo de todas as enumerações. Existe um espaço reservado similar para as propriedades que usam um valor de enumeração, mas ele normalmente prefixa o espaço reservado com uma dica do nome da enumeração. Por exemplo, a sintaxe mostrada em [**FrameworkElement.FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) é <*frameworkElement***FlowDirection**="* flowDirectionMemberName*"/>. Se você está em uma dessas páginas de referência de propriedades, clique no link para o tipo de enumeração que aparece na seção **Valor da Propriedade**, ao lado do texto **Tipo:**. Para o valor do atributo de uma propriedade que usa essa enumeração, você pode utilizar qualquer cadeia de caracteres que esteja listada na coluna **Membro** da lista **Membros**.
-   *double*, *int*, *string*, *bool*: são tipos primitivos conhecidos da linguagem XAML. Se você está programando com o C# ou o Visual Basic, esses tipos são projetados para tipos equivalentes do Microsoft .NET, como [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx), [**Int32**](https://msdn.microsoft.com/library/windows/apps/xaml/system.int32.aspx) [**String**](https://msdn.microsoft.com/library/windows/apps/xaml/system.string.aspx) e [**Boolean**](https://msdn.microsoft.com/library/windows/apps/xaml/system.boolean.aspx), e você pode usar qualquer membro nesses tipos do .NET ao trabalhar com seus valores definidos por XAML no code-behind do .NET. Se você está programando com o C++/CX, vai usar os tipos primitivos do C++, mas também poderá considerar esses equivalentes aos tipos definidos pelo namespace da [**Platform**](https://msdn.microsoft.com/library/windows/apps/xaml/hh710417.aspx), por exemplo [**Platform::String**](https://msdn.microsoft.com/library/windows/apps/xaml/hh755812.aspx). Às vezes, há outras restrições de valor para propriedades específicas. Normalmente, elas aparecem na seção **Valor da propriedade** ou na seção "Comentários", e não na seção XAML, porque qualquer restrição desse tipo é válida para usos de código e de XAML.

## <a name="tips-and-tricks-notes-on-style"></a>Dicas e truques, anotações sobre estilo

-   As extensões de marcação em geral são descritas na [visão geral de XAML](xaml-overview.md) principal. No entanto, a extensão de marcação de maior impacto nas orientações fornecidas neste tópico é a extensão de marcação [StaticResource](staticresource-markup-extension.md) (e relacionada à [ThemeResource](themeresource-markup-extension.md)). A função da extensão de marcação StaticResource serve para habilitar a fatoração de seu XAML em recursos reutilizáveis provenientes de um [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) XAML. Você quase sempre define os modelos de controle e os estilos relacionados em um **ResourceDictionary**. Você normalmente define as partes menores de uma definição de modelo de controle ou um estilo específico de aplicativo em um **ResourceDictionary** também, por exemplo, [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) para uma cor que seu aplicativo usa mais de uma vez para diferentes partes da interface do usuário. Usando StaticResource, qualquer propriedade que de outro modo exigiria a definição de um uso de elemento de propriedade agora pode ser definida em sintaxe de atributo. Mas os benefícios da fatoração de XAML para reutilização vão além da mera simplificação da sintaxe no nível da página. Para obter mais informações, consulte [Referências de recursos de ResourceDictionary e XAML](https://msdn.microsoft.com/library/windows/apps/mt187273).
-   Você verá várias convenções diferentes sobre como espaços em branco e alimentações de linha são aplicados em exemplos de XAML. Em particular, há diferentes convenções sobre como romper elementos de objeto que têm muitos atributos diferentes definidos. Isso é apenas uma questão de estilo. O editor XML do Visual Studio aplica algumas regras de estilo padrão ao editar XAML, mas você pode mudá-las nas configurações. Há somente alguns casos em que o espaço em branco em um arquivo XAML é considerado significativo; para saber mais, consulte [XAML e espaço em branco](xaml-and-whitespace.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Visão geral do XAML](xaml-overview.md)
* [Namespaces XAML e mapeamento de namespace](xaml-namespaces-and-namespace-mapping.md)
* [Referências de recursos de ResourceDictionary e XAML](https://msdn.microsoft.com/library/windows/apps/mt187273)
 

