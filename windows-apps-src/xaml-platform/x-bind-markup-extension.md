---
author: jwmsft
description: "A extensão de marcação xBind é uma alternativa a Binding. xBind não tem alguns dos recursos de Binding, mas ele é executado em menos tempo e usando menos memória do que Binding e suporta melhor a depuração."
title: "Extensão de marcação xBind"
ms.assetid: 529FBEB5-E589-486F-A204-B310ACDC5C06
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: ba08e426fea4c494276978d96cf0b36f6956bdb8
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="xbind-markup-extension"></a>Extensão de marcação {x:Bind}

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**Observação** Para obter informações gerais sobre o uso da vinculação de dados no seu aplicativo com **{x:Bind}** (e para todas as comparações entre **{x:Bind}** e **{Binding}**), consulte [Vinculação de dados em detalhes](https://msdn.microsoft.com/library/windows/apps/mt210946).

A extensão de marcação **{x: Bind}** — nova no Windows 10 — é uma alternativa ao **{Binding}**. **{x:Bind}** não tem alguns dos recursos de **{Binding}**, mas ele é executado em menos tempo e usando menos memória do que **{Binding}** e suporta melhor a depuração.

Em tempo de compilação XAML, **{x: Bind}** é convertido em código que irá obter um valor de uma propriedade em uma fonte de dados e defini-lo na propriedade especificada na marcação. O objeto de associação, opcionalmente, pode ser configurado para observar mudanças no valor da propriedade de origem de dados e atualizar-se com base nessas alterações. Ele também pode ser configurado opcionalmente para enviar as alterações em seu próprio valor de volta para a propriedade de origem. Os objetos de associação criados por **{x:Bind}** e **{Binding}** são em grande parte funcionalmente equivalentes. Mas o **{x:Bind}** executa o código de finalidade especial, que ele gera no tempo de compilação, e o **{Binding}** usa inspeção de objeto de tempo de execução de finalidade geral. Consequentemente, as associações **{x:Bind}** (normalmente chamadas de associações compiladas) têm excelente desempenho, fornecem validação de tempo de compilação de suas expressões de associação e dão suporte à depuração permitindo que você defina pontos de interrupção nos arquivos de código que são gerados como a classe parcial para sua página. Esses arquivos podem ser encontrados em sua pasta `obj`, com nomes como (para C#) `<view name>.g.cs`.

**Aplicativos de amostra que demonstram {x:Bind}**

-   [amostra de {x:Bind}](http://go.microsoft.com/fwlink/p/?linkid=619989)
-   [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame)
-   [Amostra de noções básicas de interface do usuário XAML](http://go.microsoft.com/fwlink/p/?linkid=619992)

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML

``` syntax
<object property="{x:Bind}" .../>
-or-
<object property="{x:Bind propertyPath}" .../>
-or-
<object property="{x:Bind bindingProperties}" .../>
-or-
<object property="{x:Bind propertyPath, bindingProperties}" .../>
```

| Termo | Descrição |
|------|-------------|
| _propertyPath_ | Uma cadeia de caracteres que especifica o caminho de propriedade para a associação. Mais informações estão na seção [caminho de propriedade](#property-path) abaixo. |
| _bindingProperties_ |
| _propName_=_value_\[, _propName_=_value_\]* | Uma ou mais propriedades de associação que são especificadas usando uma sintaxe de par nome/valor. |
| _propName_ | O nome da cadeia de caracteres da propriedade a ser definida no objeto Binding. Por exemplo, "Converter". |
| _value_ | O valor a se definir a propriedade. A sintaxe do argumento depende da propriedade que está sendo definida. Veja um exemplo de uso de _propName_=_value_ em que o valor é uma extensão de marcação: `Converter={StaticResource myConverterClass}`. Para obter mais informações, consulte a seção [Propriedades que você pode definir com {x: Bind}](#properties-that-you-can-set-with-xbind) a seguir. | 

## <a name="property-path"></a>Caminho de propriedade

*PropertyPath* define a propriedade **Path** de uma expressão **{x:Bind}**. **Path** é um caminho de propriedade que especifica o valor da propriedade, da subpropriedade, do campo ou do método ao qual você está se associando (à origem). Você pode mencionar o nome da propriedade **Path** explicitamente: `{Binding Path=...}`. Ou você pode omiti-lo: `{Binding ...}`.

### <a name="property-path-resolution"></a>Resolução de caminho da propriedade

**{x:Bind}** não usa o **DataContext** como uma fonte padrão; em vez disso, usa o controle de página ou do usuário propriamente dito. Portanto, ele aparecerá no code-behind de sua página ou controle de usuário para métodos, propriedades e campos. Para expor seu modelo de exibição **{x:Bind}**, você geralmente deseja adicionar novos campos ou propriedades para o code-behind para sua página ou controle de usuário. Etapas de um caminho de propriedade são delimitadas por pontos (.), e você pode incluir vários delimitadores para percorrer subpropriedades sucessivas. Use o ponto delimitador independentemente da linguagem de programação usada para implementar o objeto sendo associado.

Por exemplo: em uma página, **Text="{x:Bind Employee.FirstName}"** procurará por um membro **Funcionário** na página e, em seguida, um membro **FirstName** no objeto retornado por **Funcionário**. Se você estiver associando o controle de um item a uma propriedade que contém dependentes de um funcionário, o seu caminho de propriedade pode ser "Employee.Dependents", o modelo de item do controle do item exibiria os itens em "Dependents".

Para C++/CX, **{x:Bind}** não é possível associar campos particulares e propriedades no modelo de página ou dados – você precisará ter uma propriedade pública para que seja associável. A área de superfície para associação precisa ser exposta como classes/interfaces CX para que possamos obter os metadados relevantes. O atributo **\[Bindable\]** não deve ser necessário.

Com **x:Bind**, você não precisa usar **ElementName=xxx** como parte da expressão de associação. Com **x:Bind**, você pode usar o nome do elemento como a primeira parte do caminho para a associação, porque elementos nomeados tornam-se campos dentro do controle de página ou de usuário que representa a origem da associação raiz

### <a name="collections"></a>Coleções

Se a fonte de dados for uma coleção, um caminho de propriedade pode especificar itens na coleção pela posição ou índice. Por exemplo, "Teams\[0\].Players", em que o "\[\]" literal contém o "0" que solicita o primeiro item em uma coleção com índice zero.

Para usar um indexador, o modelo precisa implementar **IList&lt;T&gt;** ou **IVector&lt;T&gt;** no tipo de propriedade que vai ser indexada. Se o tipo da propriedade indexada der suporte a **INotifyCollectionChanged** ou **IObservableVector** e a associação for OneWay ou TwoWay, então registrará e escutará notificações de alteração nessas interfaces. A lógica de detecção de alteração irá atualizar com base em todas as alterações de coleção, mesmo que não afete o valor indexado específico. Isso ocorre porque a lógica de escuta é comum em todas as instâncias da coleção.

Se a fonte de dados for um dicionário ou um mapa, um caminho de propriedade poderá especificar itens na coleção pelo nome da cadeia de caracteres. Por exemplo **&lt;TextBlock Text="{x:Bind Players\['John Smith'\]" /&gt;** irá procurar um item no dicionário chamado "John Smith". O nome precisa estar entre aspas, e aspas simples ou duplas podem ser usadas. O acento circunflexo (^) pode ser usado no escape de citações em cadeias de caracteres. Normalmente é mais fácil usar aspas alternativas do que as usadas para o atributo XAML.

Para usar um indexador da cadeia de caracteres, o modelo precisa implementar **IDictionary&lt;string, T&gt;** ou **IMap&lt;string, T&gt;** no tipo da propriedade que será indexada. Se o tipo da propriedade indexada suportar **IObservableMap** e a associação for OneWay ou TwoWay, então registrará e escutará notificações de alteração nessas interfaces. A lógica de detecção de alteração irá atualizar com base em todas as alterações de coleção, mesmo que não afete o valor indexado específico. Isso ocorre porque a lógica de escuta é comum em todas as instâncias da coleção.

### <a name="attached-properties"></a>Propriedades Anexadas

Para associar a propriedades anexadas, você precisa colocar o nome de classe e propriedade entre parênteses depois do ponto. Por exemplo **Text="{x:Bind Button22.(Grid.Row)}"**. Se a propriedade não é declarada em um namespace Xaml, você precisará prefixá-la com um namespace xml, que você deve mapear para um namespace de código no head do documento.

### <a name="casting"></a>Transmissão

Associações compiladas são fortemente tipadas e resolverão o tipo de cada etapa em um caminho. Se o tipo retornado não tiver o membro, falhará no momento da compilação. Você pode especificar uma conversão para informar o tipo real do objeto de associação. No caso a seguir, **obj** é uma propriedade do objeto de tipo, mas que contém uma caixa de texto, para que possamos usar **Text="{x:Bind ((TextBox)obj).Text}"** ou **Text="{x:Bind obj.(TextBox.Text)}"**.
O campo **groups3** em **Text="{x:Bind ((data:SampleDataGroup)groups3\[0\]).Title}"** é um dicionário de objetos, portanto, você deve convertê-lo para **data:SampleDataGroup**. Observe o uso do prefixo de namespace xml **data:** para mapear o tipo de objeto para um namespace de código que não seja parte do namespace XAML padrão.

_Observação: A sintaxe de conversão em estilo C# é mais flexível do que a sintaxe de propriedade anexada, e será a sintaxe recomendada futuramente._

## <a name="functions-in-binding-paths"></a>Funções em caminhos de associação

Desde o Windows 10, versão 1607, **{x: Bind}** dá suporte ao uso de uma função como a etapa de folha do caminho de associação. Assim, é possível
- Uma maneira mais simples de conseguir a conversão de valor
- Uma maneira para associações dependerem de mais de um parâmetro

> [!NOTE]
> Para usar funções com **{x:Bind}**, a versão do SDK de alvo mínima do aplicativo deve ser 14393 ou posterior. Você não poderá usar funções quando o aplicativo se destinar a versões anteriores do Windows 10. Para saber mais sobre as versões de destino, consulte [Código adaptável de versão](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

No exemplo a seguir, o primeiro e o segundo planos do item estão associados a funções para fazer uma conversão com base no parâmetro de cor

``` Xamlmarkup
<DataTemplate x:DataType="local:ColorEntry">
    <Grid Background="{x:Bind Brushify(Color)}" Width="240">
        <TextBlock Text="{x:Bind ColorName}" Foreground="{x:Bind TextColor(Color)}" Margin="10,5" />
    </Grid>
</DataTemplate>
```
``` C#
class ColorEntry
{
    public string ColorName { get; set; }
    public Color Color { get; set; }

    public static SolidColorBrush Brushify(Color c)
    {
        return new SolidColorBrush(c);
    }

    public static SolidColorBrush TextColor(Color c)
    {
        return new SolidColorBrush(((c.R * 0.299 + c.G * 0.587 + c.B * 0.114) > 150) ? Colors.Black : Colors.White);
    }
}

```
### <a name="function-syntax"></a>Sintaxe da função
``` Syntax
Text="{x:Bind MyModel.Order.CalculateShipping(MyModel.Order.Weight, MyModel.Order.ShipAddr.Zip, 'Contoso'), Mode=OneTime}"
             |      Path to function         |    Path argument   |       Path argument       | Const arg |  Bind Props
```

### <a name="path-to-the-function"></a>Caminho para a função
O caminho para a função é especificado como outros caminhos de propriedade e pode incluir pontos (.), indexadores ou conversões para localizar a função.

As funções estáticas podem ser especificadas usando-se a sintaxe XMLNamespace:ClassName.MethodName. Por exemplo **&lt;CalendarDatePicker Date="\{x:Bind sys:DateTime.Parse(TextBlock1.Text)\}" /&gt;** será mapeado para a função DateTime.Parse, pressupondo-se que **xmlns:sys="using:System"** esteja especificado na parte superior da página.

Se o modo for OneWay/TwoWay, o caminho da função terá detecção de alteração realizada nele, e a associação será reavaliada se houver alterações nesses objetos.

A função associada precisa:
- Estar acessível para o código e os metadados – assim, internal / private funcionam em C#, mas C++/CX precisarão de métodos que sejam métodos WinRT públicos
- A sobrecarga se baseia no número de argumentos, e não no tipo, e ela tentará se comparar com a primeira sobrecarga com muitos argumentos
- Os tipos de argumento precisam corresponder aos dados passados – não fazemos conversões de restrição
- O tipo de retorno da função precisa corresponder ao tipo da propriedade que está usando a associação


### <a name="function-arguments"></a>Argumentos de função
Vários argumentos de função podem ser especificados, separados por vírgula (,)
- Caminho de associação – a mesma sintaxe como se você estivesse associando diretamente esse objeto.
  - Se o modo for OneWay/TwoWay, a detecção de alteração será realizada, e a associação será reavaliada mediante a alteração do objeto
- Cadeia de caracteres constante entre aspas – aspas são necessárias para designá-la como uma cadeia de caracteres. O acento circunflexo (^) pode ser usado no escape de citações em cadeias de caracteres
- Número da constante - por exemplo -123,456
- Booliano – especificado como "x:True" ou "x:False"

### <a name="two-way-function-bindings"></a>Associações de função bidirecional
Em um cenário de associação bidirecional, uma segunda função deve ser especificada para a direção inversa da associação. Isso é feito usando-se a propriedade de associação **BindBack**, por exemplo **Text="\{x:Bind a.MyFunc(b), BindBack=a.MyFunc2\}"**. A função deve utilizar um argumento que é o valor que precisa ser reenviado para o modelo.

## <a name="event-binding"></a>Associação de evento

Associação de evento é um recurso exclusivo para associação compilada. Permite que você especifique o manipulador para um evento usando uma associação, em vez de ele ter de ser um método no code-behind. Por exemplo: **Click="{x:Bind rootFrame.GoForward}"**.

Para eventos, o método de destino não deve ser sobrecarregado e também deve:

-   Corresponder à assinatura do evento.
-   OU não ter parâmetros.
-   OU ter o mesmo número de parâmetros de tipos que podem ser atribuídos com base nos tipos dos parâmetros de evento.

No code-behind gerado, associação compilada manipula o evento e o encaminha para o método no modelo, avaliando o caminho da expressão de associação quando o evento ocorre. Isso significa que, diferentemente das associações de propriedade, não controla as alterações no modelo.

Para saber mais sobre a sintaxe de cadeia de caracteres de um caminho de propriedade, veja [Sintaxe de caminho de propriedade](property-path-syntax.md), tendo em mente as diferenças descritas aqui para **{x:Bind}**.

##  <a name="properties-that-you-can-set-with-xbind"></a>Propriedades que você pode definir com {x: Bind}


**{x:Bind}** é ilustrado com a sintaxe de espaço reservado *bindingProperties* porque há várias propriedades de leitura/gravação que podem ser definidas na extensão de marcação. As propriedades podem ser definidas em qualquer ordem com pares *propName*=*value* separados por vírgula. Observe que você não pode incluir quebras de linha na expressão de associação. Algumas das propriedades exigem tipos que não têm uma conversão de tipo específica, então são necessárias extensões de marcação próprias aninhadas no **{x:Bind}**.

Essas propriedades funcionam da mesma forma que as propriedades da classe [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820).

| Propriedade | Descrição |
|----------|-------------|
| **Caminho** | Consulte a seção [Caminho de propriedade](#property-path) acima. |
| **Conversor** | Especifica o objeto de conversor que é chamado pelo mecanismo de associação. O conversor pode ser definido em XAML, mas somente se você se referir a uma instância de objeto atribuída em uma referência [{StaticResource} markup extension](staticresource-markup-extension.md) ao objeto no dicionário de recursos. |
| **ConverterLanguage** | Especifica a cultura a ser usada pelo conversor. (Se você estiver configurando **ConverterLanguage** também deverá configurar **Converter**.) A cultura é definida como um identificador baseado em padrões. Para saber mais, veja [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880). |
| **ConverterParameter** | Especifica o parâmetro do conversor que pode ser usado na lógica de conversão. (Se estiver definindo **ConverterParameter**, você também deve definir **Converter**). A maioria dos conversores usam lógica simples que obtém todas as informações que precisam do valor calculado para converter, e não precisam de um valor **ConverterParameter**. O parâmetro **ConverterParameter** é para implementações moderadamente avançadas que têm mais de uma lógica que controla o que for passado em **ConverterParameter**. Você também pode escrever um conversor que usa valores diferentes de cadeias de caracteres, mas isso é incomum; veja Comentários em [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827) para saber mais. |
| **FallbackValue** | Especifica um valor a ser exibido quando a fonte ou caminho não podem ser resolvidos. |
| **Mode** | Especifica o mode de associação como uma dessas cadeias de caracteres: "OneTime", "OneWay" ou "TwoWay". O padrão é "OneTime". Observe que isso é diferente do padrão para **{Binding}**, que é "OneWay" na maioria dos casos. |
| **TargetNullValue** | Especifica um valor a ser exibido quando o valor de origem é solucionado, mas é explicitamente **null**. |
| **BindBack** | Especifica uma função a ser usada na direção inversa de uma associação bidirecional. | 

**Observação** Se você estiver convertendo a marcação de **{Binding}** para **{x:Bind}**, examine as diferenças nos valores padrão da propriedade **Mode**.
 
## <a name="remarks"></a>Comentários

Como **{x:Bind}** usa código gerado para atingir seus benefícios, requer informações de tipo em tempo de compilação. Isso significa que você não pode associar a propriedades onde você não souber o tipo antes do tempo. Por isso, você não pode usar **{x:Bind}** com a propriedade **DataContext**, que é do tipo **Object** e também está sujeita a alterações no tempo de execução.

Ao usar **{x:Bind}** com modelos de dados, você deve indicar o tipo associado definindo um **x:DataType**, conforme mostrado no exemplo a seguir. Você pode também definir o tipo para um tipo de classe de interface ou base, e então use projeções, se necessário, para formular uma expressão completa.

As associações compiladas dependem da geração de código. Portanto, se você usar **{x:Bind}** em um dicionário de recursos, o dicionário de recursos precisará ter uma classe code-behind. Veja [Dicionários de recursos com {x:Bind}](../data-binding/data-binding-in-depth.md#resource-dictionaries-with-x-bind) para obter um exemplo de código.

Páginas e controles de usuário que incluem associações compiladas terão uma propriedade "Bindings" no código gerado. Entre elas estão os seguintes métodos:
- **Update()** - Isso atualizará os valores de todas as associações compiladas. Uma associação unidirecional/bidirecional terá os ouvintes conectados para detectar alterações.
- **Initialize()** - Se as associações ainda não tiverem sido inicializadas, ela chamará Update() para inicializá-las
- **StopTracking()** - Ela desconectará todos os ouvintes criados para associações uni e bidirecionais. Elas podem ser reinicializadas usando-se o método Update().

> [!NOTE]
> A partir do Windows 10, versão 1607, a estrutura XAML fornece um Booliano integrado para conversor de Visibilidade. O conversor mapeia **true** para o valor de enumeração **Visible** e **falso** para **Collapsed** para que você possa associar uma propriedade de Visibilidade a um Boolenao sem criar um conversor. Para usar o conversor integrado, a versão do SDK de alvo mínimo do seu aplicativo deve ser 14393 ou posterior. Você não poderá usá-lo se seu aplicativo for voltado para versões anteriores do Windows 10. Para saber mais sobre as versões de destino, consulte [Código adaptável de versão](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

**Dica** Se você precisar especificar uma única chave para um valor, como em [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) ou [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827), coloque uma barra invertida antes dela: `\{`. De forma alternativa, colo a cadeia de caracteres inteira que contém a chave que precisa de escape entre apóstrofos; por exemplo, `ConverterParameter='{Mix}'`.

[**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826), [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) e **ConverterLanguage** estão todos relacionados à situação de conversão de um valor ou tipo de uma fonte de associação a um tipo ou valor que é compatível com a propriedade do destino da associação. Para saber mais, veja a seção "Conversões de dados" em [Vinculação de dados em detalhes](https://msdn.microsoft.com/library/windows/apps/mt210946).

**{x:Bind}** é apenas uma extensão de marcação, sem nenhuma maneira de criar ou manipular essas associações de forma programática. Para saber mais sobre extensões de marcação, veja [Visão geral do XAML](xaml-overview.md).

## <a name="examples"></a>Exemplos

```XML
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

Este exemplo usa XAML **{x: Bind}** com uma propriedade **ListView.ItemTemplate**. Observe a declaração de um valor **x: DataType**.

```XML
  <DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```
