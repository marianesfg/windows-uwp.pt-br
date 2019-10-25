---
description: A extensão de marcação xBind é uma alternativa de alto desempenho para associação. xBind-novo para o Windows 10 – é executado em menos tempo e menos memória do que a associação e dá suporte à melhor depuração.
title: extensão de marcação xBind
ms.assetid: 529FBEB5-E589-486F-A204-B310ACDC5C06
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a25797f50ee76542b8f9543cb76453d2916368ac
ms.sourcegitcommit: 82d202478ab4d3011c5ddd2e852958c34336830d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/22/2019
ms.locfileid: "72715862"
---
# <a name="xbind-markup-extension"></a>{extensão de marcação de x:Bind}

**Observação**  para obter informações gerais sobre como usar a associação de dados em seu aplicativo com **{x:bind}** (e para uma comparação completa entre **{x:bind}** e **{Binding}** ), consulte [ligação de dados em profundidade](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

A **{x:bind}** Markup Extension – New for Windows 10 — é uma alternativa para **{Binding}** . **{x:bind}** é executado em menos tempo e menos memória que **{Binding}** e dá suporte à depuração melhor.

Em tempo de compilação XAML, **{x:bind}** é convertido em código que receberá um valor de uma propriedade em uma fonte de dados e o definirá na propriedade especificada em marcação. O objeto de associação pode, opcionalmente, ser configurado para observar as alterações no valor da propriedade da fonte de dados e se atualizar com base nessas alterações (`Mode="OneWay"`). Ele também pode ser configurado opcionalmente para enviar por push as alterações em seu próprio valor para a propriedade Source (`Mode="TwoWay"`).

Os objetos de associação criados por **{x:bind}** e **{Binding}** são amplamente funcionalmente equivalentes. Mas **{x:bind}** executa o código de finalidade especial, que é gerado em tempo de compilação, e **{Binding}** usa a inspeção de objeto de tempo de execução de uso geral. Consequentemente, **{x:bind}** associações (muitas vezes conhecidas como associações compiladas) têm ótimo desempenho, fornecem validação de tempo de compilação de suas expressões de associação e dão suporte à depuração, permitindo que você defina pontos de interrupção nos arquivos de código que são gerado como a classe parcial para sua página. Esses arquivos podem ser encontrados na pasta `obj`, com nomes como (para C#)`<view name>.g.cs`.

> [!TIP]
> **{x:bind}** tem um modo padrão de **OneTime**, ao contrário de **{Binding}** , que tem um modo padrão de **OneWay**. Isso foi escolhido por motivos de desempenho, pois o uso de **OneWay** faz com que mais código seja gerado para a conexão e lide com a detecção de alterações. Você pode especificar explicitamente um modo para usar a associação OneWay ou TwoWay. Você também pode usar [x:DefaultBindMode](x-defaultbindmode-attribute.md) para alterar o modo padrão de **{x:bind}** para um segmento específico da árvore de marcação. O modo especificado se aplica a qualquer expressão **{x:bind}** nesse elemento e a seus filhos, que não especificam explicitamente um modo como parte da associação.

**Aplicativos de exemplo que demonstram {x:Bind}**

-   [{exemplo de x:Bind}](https://go.microsoft.com/fwlink/p/?linkid=619989)
-   [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper)
-   [Exemplo básico da interface do usuário XAML](https://go.microsoft.com/fwlink/p/?linkid=619992)

## <a name="xaml-attribute-usage"></a>Uso de atributo XAML

``` syntax
<object property="{x:Bind}" .../>
-or-
<object property="{x:Bind propertyPath}" .../>
-or-
<object property="{x:Bind bindingProperties}" .../>
-or-
<object property="{x:Bind propertyPath, bindingProperties}" .../>
-or-
<object property="{x:Bind pathToFunction.functionName(functionParameter1, functionParameter2, ...), bindingProperties}" .../>
```

| Prazo | DESCRIÇÃO |
|------|-------------|
| _propertyPath_ | Uma cadeia de caracteres que especifica o caminho da propriedade para a associação. Mais informações estão na seção [caminho da propriedade](#property-path) abaixo. |
| _bindingproperties_ |
| _propname_=_valor_\[, _propname_=_valor_\]* | Uma ou mais propriedades de associação que são especificadas usando uma sintaxe de par nome/valor. |
| _propName_ | O nome da cadeia de caracteres da propriedade a ser definida no objeto de associação. Por exemplo, "converter". |
| _valor_ | O valor para o qual definir a propriedade. A sintaxe do argumento depende da propriedade que está sendo definida. Aqui está um exemplo de uso de _valor_ de _propName_=em que o valor é, em si, uma extensão de marcação: `Converter={StaticResource myConverterClass}`. Para obter mais informações, consulte [Propriedades que podem ser definidas com a seção {x:bind}](#properties-that-you-can-set-with-xbind) abaixo. |

## <a name="examples"></a>Exemplos

```XAML
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

Este XAML de exemplo usa **{x:bind}** com uma propriedade **ListView. ItemTemplate** . Observe a declaração de um valor de **x:DataType** .

```XAML
  <DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

## <a name="property-path"></a>Caminho da propriedade

*PropertyPath* define o **caminho** para uma expressão **{x:bind}** . **Path** é um caminho de propriedade especificando o valor da propriedade, subpropriedade, campo ou método ao qual você está ligando (a origem). Você pode mencionar o nome da propriedade **path** explicitamente: `{x:Bind Path=...}`. Ou você pode omiti-lo: `{x:Bind ...}`.

### <a name="property-path-resolution"></a>Resolução do caminho da propriedade

**{x:bind}** não usa **DataContext** como uma fonte padrão — em vez disso, ele usa a página ou o próprio controle de usuário. Portanto, ele examinará o code-behind de sua página ou controle de usuário para propriedades, campos e métodos. Para expor seu modelo de exibição para **{x:bind}** , normalmente você desejará adicionar novos campos ou propriedades ao code-behind para sua página ou controle de usuário. As etapas em um caminho de propriedade são delimitadas por pontos (.), e você pode incluir vários delimitadores para atravessar as subpropriedades sucessivas. Use o delimitador de ponto independentemente da linguagem de programação usada para implementar o objeto que está sendo associado.

Por exemplo: em uma página, **text = "{X:bind Employee. FirstName}"** procurará um membro **Employee** na página e, em seguida, um membro **FirstName** no objeto retornado por **Employee**. Se você estiver associando um controle de itens a uma propriedade que contém os dependentes de um funcionário, seu caminho de propriedade poderá ser "Employee. depends", e o modelo de item do controle itens cuidaria da exibição dos itens em "dependentes".

Para C++o/CX, **{x:bind}** não pode se associar a campos e propriedades particulares na página ou no modelo de dados – você precisará ter uma propriedade pública para que ele seja ligável. A área de superfície para associação precisa ser exposta como classes/interfaces CX para que possamos obter os metadados relevantes. O atributo de **\]\[vinculável** não deve ser necessário.

Com o **x:bind**, você não precisa usar **ElementName = xxx** como parte da expressão de associação. Em vez disso, você pode usar o nome do elemento como a primeira parte do caminho para a associação porque os elementos nomeados se tornam campos dentro da página ou controle de usuário que representa a origem da Associação raiz. 


### <a name="collections"></a>Coleções

Se a fonte de dados for uma coleção, um caminho de propriedade poderá especificar itens na coleção por sua posição ou índice. Por exemplo, "Teams\[0\]. Players ", em que o literal"\[\]"inclui o" 0 "que solicita o primeiro item em uma coleção com índices zero.

Para usar um indexador, o modelo precisa implementar **IList&lt;t&gt;** ou **IVector&lt;t&gt;** no tipo da propriedade que será indexada. (Observe que IReadOnlyList&lt;T&gt; e IVectorView&lt;T&gt; não dão suporte à sintaxe do indexador.) Se o tipo da propriedade indexada oferecer suporte a **INotifyCollectionChanged** ou **IObservableVector** e a associação for OneWay ou TwoWay, ele será registrado e escutará as notificações de alteração nessas interfaces. A lógica de detecção de alteração será atualizada com base em todas as alterações de coleção, mesmo que isso não afete o valor indexado específico. Isso ocorre porque a lógica de escuta é comum em todas as instâncias da coleção.

Se a fonte de dados for um dicionário ou um mapa, um caminho de propriedade poderá especificar itens na coleção por seu nome de cadeia de caracteres. Por exemplo **&lt;TextBlock Text = "{X:bind players\[' John Smith '\]"/&gt;** procurará um item no dicionário chamado "John Smith". O nome precisa ser colocado entre aspas e aspas simples ou duplas podem ser usadas. O Hat (^) pode ser usado para escapar as aspas em cadeias de caracteres. Normalmente, é mais fácil usar aspas alternativas daquelas usadas para o atributo XAML. (Observe que IReadOnlyDictionary&lt;T&gt; e IMapView&lt;T&gt; não dão suporte à sintaxe do indexador.)

Para usar um indexador de cadeia de caracteres, o modelo precisa implementar **IDictionary&lt;cadeia de caracteres, t&gt;** ou **IMap&lt;cadeia de caracteres, t&gt;** no tipo da propriedade que será indexada. Se o tipo da propriedade indexada der suporte a **IObservableMap** e a associação for OneWay ou TwoWay, ela será registrada e escutará por notificações de alteração nessas interfaces. A lógica de detecção de alteração será atualizada com base em todas as alterações de coleção, mesmo que isso não afete o valor indexado específico. Isso ocorre porque a lógica de escuta é comum em todas as instâncias da coleção.

### <a name="attached-properties"></a>Propriedades anexadas

Para associar a [Propriedades anexadas](./attached-properties-overview.md), você precisa colocar o nome da classe e da propriedade entre parênteses após o ponto. Por exemplo, **text = "{X:bind Button22. ( Grid. Row)} "** . Se a propriedade não for declarada em um namespace XAML, será necessário prefixar-a com um namespace XML, que você deve mapear para um namespace de código no cabeçalho do documento.

### <a name="casting"></a>Elenco

As associações compiladas são fortemente tipadas e resolverão o tipo de cada etapa em um caminho. Se o tipo retornado não tiver o membro, ele falhará no momento da compilação. Você pode especificar uma conversão para informar a associação do tipo real do objeto. No caso a seguir, **obj** é uma propriedade do tipo Object, mas contém uma caixa de texto, portanto, podemos usar **text = "{X:Bind ((TextBox) obj). Text} "** ou **text =" {x:bind obj. (TextBox. Text)} "** .
O campo **groups3** em **text = "{x:Bind ((data: SampleDataGroup) groups3\[0\]). Title} "** é um dicionário de objetos, portanto, você deve convertê-lo em **dados: SampleDataGroup**. Observe o uso do prefixo XML **Data:** namespace para mapear o tipo de objeto para um namespace de código que não faz parte do namespace XAML padrão.

_Observação: a C#sintaxe de conversão-Style é mais flexível do que a sintaxe de propriedade anexada e é a sintaxe recomendada no futuro._

## <a name="functions-in-binding-paths"></a>Funções em caminhos de associação

A partir do Windows 10, versão 1607, **{x:bind}** dá suporte ao uso de uma função como a etapa folha do caminho de associação. Esse é um recurso poderoso para DataBinding que permite vários cenários na marcação. Consulte [associações de função](../data-binding/function-bindings.md) para obter detalhes.

## <a name="event-binding"></a>Associação de evento

A associação de eventos é um recurso exclusivo para associação compilada. Ele permite que você especifique o manipulador para um evento usando uma associação, em vez de ter que ser um método no code-behind. Por exemplo: **clique = "{X:bind rootFrame. GoForward}"** .

Para eventos, o método de destino não deve ser sobrecarregado e também deve:

- Corresponde à assinatura do evento.
- OU não têm parâmetros.
- OU ter o mesmo número de parâmetros de tipos que podem ser atribuídas a partir dos tipos de parâmetros de evento.

No code-behind gerado, a associação compilada manipula o evento e o roteia para o método no modelo, avaliando o caminho da expressão de associação quando o evento ocorre. Isso significa que, ao contrário das associações de propriedade, ele não controla as alterações no modelo.

Para obter mais informações sobre a sintaxe de cadeia de caracteres para um caminho de propriedade, consulte [sintaxe de caminho de propriedade](property-path-syntax.md), tendo em mente as diferenças descritas aqui para **{x:bind}** .

## <a name="properties-that-you-can-set-with-xbind"></a>Propriedades que você pode definir com {x:Bind}

**{x:bind}** é ilustrado com a sintaxe de espaço reservado *bindproperties* porque há várias propriedades de leitura/gravação que podem ser definidas na extensão de marcação. As propriedades podem ser definidas em qualquer ordem com pares de *valores* de *propName*=separados por vírgula. Observe que você não pode incluir quebras de linha na expressão de associação. Algumas das propriedades exigem tipos que não têm uma conversão de tipo, portanto, elas exigem extensões de marcação de suas próprias aninhadas dentro de **{x:bind}** .

Essas propriedades funcionam praticamente da mesma forma que as propriedades da classe de [**Associação**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) .

| Propriedade | DESCRIÇÃO |
|----------|-------------|
| **Caminho** | Consulte a seção [caminho da propriedade](#property-path) acima. |
| **Convertido** | Especifica o objeto do conversor que é chamado pelo mecanismo de associação. O conversor pode ser definido em XAML, mas somente se você se referir a uma instância de objeto que você atribuiu em uma referência de [extensão de marcação {StaticResource}](staticresource-markup-extension.md) a esse objeto no dicionário de recursos. |
| **ConverterLanguage** | Especifica a cultura a ser usada pelo conversor. (Se você estiver configurando **ConverterLanguage** , você também deverá definir o **conversor**.) A cultura é definida como um identificador baseado em padrões. Para obter mais informações, consulte [**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage). |
| **ConverterParameter** | Especifica o parâmetro do conversor que pode ser usado na lógica do conversor. (Se você estiver configurando **ConverterParameter** , você também deverá definir o **conversor**.) A maioria dos conversores usa uma lógica simples que obtém todas as informações necessárias do valor passado para converter e não precisa de um valor **ConverterParameter** . O parâmetro **ConverterParameter** é para implementações de conversores de forma moderada que têm mais de uma lógica que desativa as que são passadas em **ConverterParameter**. Você pode escrever um conversor que usa valores diferentes de cadeias de caracteres, mas isso não é comum, consulte comentários em [**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter) para obter mais informações. |
| **FallbackValue** | Especifica um valor a ser exibido quando a origem ou o caminho não puder ser resolvido. |
| **Modo** | Especifica o modo de associação, como uma destas cadeias de caracteres: "OneTime", "OneWay" ou "TwoWay". O padrão é "OneTime". Observe que isso difere do padrão para **{Binding}** , que é "OneWay" na maioria dos casos. |
| **TargetNullValue** | Especifica um valor a ser exibido quando o valor de origem é resolvido, mas é explicitamente **nulo**. |
| **BindBack** | Especifica uma função a ser usada para a direção inversa de uma associação bidirecional. |
| **UpdateSourceTrigger** | Especifica quando enviar por push as alterações de volta do controle para o modelo em associações de TwoWay. O padrão para todas as propriedades, exceto TextBox. Text, é PropertyChanged; TextBox. Text é LostFocus.|

> [!NOTE]
> Se você estiver convertendo a marcação de **{Binding}** para **{x:bind}** , lembre-se das diferenças em valores padrão para a propriedade **Mode** .
> [**x:DefaultBindMode**](https://docs.microsoft.com/windows/uwp/xaml-platform/x-defaultbindmode-attribute) pode ser usado para alterar o modo padrão de x:BIND para um segmento específico da árvore de marcação. O modo selecionado aplicará todas as expressões x:Bind nesse elemento e seus filhos, que não especificam explicitamente um modo como parte da associação. O OneTime é mais eficaz do que OneWay, uma vez que usar OneWay fará com que mais código seja gerado para a conexão e manipule a detecção de alterações.

## <a name="remarks"></a>Comentários

Como **{x:bind}** usa código gerado para atingir seus benefícios, ele requer informações de tipo em tempo de compilação. Isso significa que você não pode associar a propriedades em que você não conhece o tipo antes do tempo. Por isso, você não pode usar **{x:bind}** com a propriedade **DataContext** , que é do tipo **Object**, e também está sujeito a alterações em tempo de execução.

Ao usar **{x:bind}** com modelos de dados, você deve indicar o tipo que está sendo associado definindo um valor de **x:DataType** , conforme mostrado na seção de [exemplos](#examples) . Você também pode definir o tipo como uma interface ou tipo de classe base e, em seguida, usar conversões, se necessário, para formular uma expressão completa.

As associações compiladas dependem da geração de código. Portanto, se você usar **{x:bind}** em um dicionário de recursos, o dicionário de recursos precisará ter uma classe code-behind. Consulte [dicionários de recursos com {x:bind}](../data-binding/data-binding-in-depth.md#resource-dictionaries-with-x-bind) para obter um exemplo de código.

Páginas e controles de usuário que incluem associações compiladas terão uma propriedade "bindings" no código gerado. Isso inclui os seguintes métodos:

- **Update ()** – isso atualizará os valores de todas as associações compiladas. Qualquer associação unidirecional/bidirecional terá os ouvintes conectados para detectar alterações.
- **Initialize ()** – se as associações ainda não tiverem sido inicializadas, ele chamará Update () para inicializar as associações
- **StopTracking ()** -isso desvinculará todos os ouvintes criados para associações unidirecionais e bidirecionais. Eles podem ser inicializados novamente usando o método Update ().

> [!NOTE]
> A partir do Windows 10, versão 1607, a estrutura XAML fornece um booliano interno ao conversor de visibilidade. O conversor mapeia **verdadeiro** para o valor de enumeração **visível** e **false** para **recolhido** para que você possa associar uma propriedade de visibilidade a um booliano sem criar um conversor. Observe que isso não é um recurso de associação de função, somente Associação de propriedade. Para usar o conversor interno, a versão do SDK de destino mínimo do seu aplicativo deve ser 14393 ou posterior. Você não pode usá-lo quando seu aplicativo se destina a versões anteriores do Windows 10. Para obter mais informações sobre as versões de destino, consulte [código adaptável de versão](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

  **Tip** se você precisar especificar uma chave única para um valor, como em [**Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) ou [**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter), preceda-a com uma barra invertida: `\{`. Como alternativa, coloque toda a cadeia de caracteres que contém as chaves que precisam de escape em um conjunto de aspas secundária, por exemplo `ConverterParameter='{Mix}'`.

O [**conversor**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter), [**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage) e **ConverterLanguage** estão todos relacionados ao cenário de conversão de um valor ou tipo da origem da associação em um tipo ou valor que é compatível com a propriedade de destino de associação. Para obter mais informações e exemplos, consulte a seção "conversões de dados" de [vinculação de dados em profundidade](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

**{x:bind}** é apenas uma extensão de marcação, sem formas de criar ou manipular essas associações programaticamente. Para obter mais informações sobre extensões de marcação, consulte [visão geral de XAML](xaml-overview.md).

