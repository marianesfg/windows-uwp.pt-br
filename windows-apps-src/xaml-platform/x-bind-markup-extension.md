---
description: A extensão de marcação xBind é uma alternativa de alto desempenho para associação. xBind-novo para o Windows 10 – é executado em menos tempo e menos memória do que a associação e dá suporte à melhor depuração.
title: Extensão de marcação xBind
ms.assetid: 529FBEB5-E589-486F-A204-B310ACDC5C06
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d148df8de9086aaaec004525c3ee4865e4320c4e
ms.sourcegitcommit: eb24481869d19704dd7bcf34e5d9f6a9be912670
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/17/2020
ms.locfileid: "79453356"
---
# <a name="xbind-markup-extension"></a>Extensão de marcação {x:Bind}

**Observação**  para obter informações gerais sobre como usar a associação de dados em seu aplicativo com **{x:bind}** (e para uma comparação completa entre **{x:bind}** e **{Binding}** ), consulte [ligação de dados em profundidade](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

A **{x:bind}** Markup Extension – New for Windows 10 — é uma alternativa para **{Binding}** . **{x:bind}** é executado em menos tempo e menos memória que **{Binding}** e dá suporte à depuração melhor.

Em tempo de compilação XAML, **{x: Bind}** é convertido em código que irá obter um valor de uma propriedade em uma fonte de dados e defini-lo na propriedade especificada na marcação. O objeto de associação, opcionalmente, pode ser configurado para observar mudanças no valor da propriedade de origem de dados e atualizar-se com base nessas alterações (`Mode="OneWay"`). Ele também pode ser configurado opcionalmente para enviar as alterações em seu próprio valor de volta para a propriedade de origem (`Mode="TwoWay"`).

Os objetos de associação criados por **{x:Bind}** e **{Binding}** são em grande parte funcionalmente equivalentes. Mas o **{x:Bind}** executa o código de finalidade especial, que ele gera no tempo de compilação, e o **{Binding}** usa inspeção de objeto de runtime de finalidade geral. Consequentemente, as associações **{x:Bind}** (normalmente chamadas de associações compiladas) têm excelente desempenho, fornecem validação de tempo de compilação de suas expressões de associação e dão suporte à depuração permitindo que você defina pontos de interrupção nos arquivos de código que são gerados como a classe parcial para sua página. Esses arquivos podem ser encontrados em sua pasta `obj`, com nomes como (para C#) `<view name>.g.cs`.

> [!TIP]
> **{x:Bind}** possui um modo padrão de **OneTime**, ao contrário de **{Binding}** , que possui um modo padrão de **OneWay**. Este foi escolhido por razões de desempenho, já que o uso de **OneWay** faz com que mais código seja gerado para conectar e manipular a detecção de alteração. Você pode especificar explicitamente um modo para usar a associação OneWay ou TwoWay. É possível usar, também, o [x:DefaultBindMode](x-defaultbindmode-attribute.md) para alterar o modo padrão para **{x:Bind}** de um segmento específico da árvore de marcação. O modo especificado aplica-se a qualquer expressão **{x:Bind}** nesse elemento e seus filhos, que não especificam explicitamente um modo como parte da vinculação.

**Aplicativos de exemplo que demonstram {x:Bind}**

-   [{exemplo de x:Bind}](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind)
-   [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper)
-   [Galeria de controles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery)

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML

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

| Termo | Descrição |
|------|-------------|
| _propertyPath_ | Uma cadeia de caracteres que especifica o caminho de propriedade para a associação. Mais informações estão na seção [caminho de propriedade](#property-path) abaixo. |
| _bindingproperties_ |
| _propname_=_valor_\[, _propname_=_valor_\]* | Uma ou mais propriedades de associação que são especificadas usando uma sintaxe de par nome/valor. |
| _propName_ | O nome da cadeia de caracteres da propriedade a ser definida no objeto Binding. Por exemplo, "Converter". |
| _value_ | O valor a se definir a propriedade. A sintaxe do argumento depende da propriedade que está sendo definida. Veja um exemplo de uso de _propName_=_value_ em que o valor é uma extensão de marcação: `Converter={StaticResource myConverterClass}`. Para obter mais informações, consulte a seção [Propriedades que você pode definir com {x: Bind}](#properties-that-you-can-set-with-xbind) a seguir. |

## <a name="examples"></a>Exemplos

```XAML
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

Este exemplo usa XAML **{x: Bind}** com uma propriedade **ListView.ItemTemplate**. Observe a declaração de um valor **x: DataType**.

```XAML
  <DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

## <a name="property-path"></a>Caminho de propriedade

*PropertyPath* define a propriedade **Path** de uma expressão **{x:Bind}** . **Path** é um caminho de propriedade que especifica o valor da propriedade, da subpropriedade, do campo ou do método ao qual você está se associando (à origem). Você pode mencionar o nome da propriedade **Path** explicitamente: `{x:Bind Path=...}`. Ou você pode omiti-lo: `{x:Bind ...}`.

### <a name="property-path-resolution"></a>Resolução de caminho da propriedade

**{x:Bind}** não usa o **DataContext** como uma fonte padrão; em vez disso, usa o controle de página ou do usuário propriamente dito. Portanto, ele aparecerá no code-behind de sua página ou controle de usuário para métodos, propriedades e campos. Para expor seu modelo de exibição **{x:Bind}** , você geralmente deseja adicionar novos campos ou propriedades para o code-behind para sua página ou controle de usuário. Etapas de um caminho de propriedade são delimitadas por pontos (.), e você pode incluir vários delimitadores para percorrer subpropriedades sucessivas. Use o ponto delimitador independentemente da linguagem de programação usada para implementar o objeto sendo associado.

Por exemplo: em uma página, **Text="{x:Bind Employee.FirstName}"** procurará por um membro **Funcionário** na página e, em seguida, um membro **FirstName** no objeto retornado por **Funcionário**. Se você estiver associando o controle de um item a uma propriedade que contém dependentes de um funcionário, o seu caminho de propriedade pode ser "Employee.Dependents", o modelo de item do controle do item exibiria os itens em "Dependents".

Para C++/CX, **{x:Bind}** não é possível associar campos particulares e propriedades no modelo de página ou dados – você precisará ter uma propriedade pública para que seja associável. A área de superfície para associação precisa ser exposta como classes/interfaces CX para que possamos obter os metadados relevantes. O atributo de **\]\[vinculável** não deve ser necessário.

Com **x:Bind**, você não precisa usar **ElementName=xxx** como parte da expressão de associação. Em vez disso, você pode usar o nome do elemento como a primeira parte do caminho para a associação porque os elementos nomeados se tornam campos dentro da página ou controle de usuário que representa a origem da Associação raiz.

### <a name="collections"></a>Coleções

Se a fonte de dados for uma coleção, um caminho de propriedade pode especificar itens na coleção pela posição ou índice. Por exemplo, "Teams\[0\]. Players ", em que o literal"\[\]"inclui o" 0 "que solicita o primeiro item em uma coleção com índices zero.

Para usar um indexador, o modelo precisa implementar **IList&lt;T&gt;** ou **IVector&lt;T&gt;** no tipo de propriedade que vai ser indexada. (Observe que IReadOnlyList&lt;T&gt; e IVectorView&lt;T&gt; não dão suporte à sintaxe do indexador.) Se o tipo da propriedade indexada oferecer suporte a **INotifyCollectionChanged** ou **IObservableVector** e a associação for OneWay ou TwoWay, ele será registrado e escutará as notificações de alteração nessas interfaces. A lógica de detecção de alteração irá atualizar com base em todas as alterações de coleção, mesmo que não afete o valor indexado específico. Isso ocorre porque a lógica de escuta é comum em todas as instâncias da coleção.

Se a fonte de dados for um dicionário ou um mapa, um caminho de propriedade poderá especificar itens na coleção pelo nome da cadeia de caracteres. Por exemplo **&lt;TextBlock Text = "{X:bind players\[' John Smith '\]}"/&gt;** procurará um item no dicionário chamado "John Smith". O nome precisa estar entre aspas, e aspas simples ou duplas podem ser usadas. O acento circunflexo (^) pode ser usado no escape de citações em cadeias de caracteres. Normalmente, é mais fácil usar aspas alternativas daquelas usadas para o atributo XAML. (Observe que IReadOnlyDictionary&lt;T&gt; e IMapView&lt;T&gt; não dão suporte à sintaxe do indexador.)

Para usar um indexador da cadeia de caracteres, o modelo precisa implementar **IDictionary&lt;string, T&gt;** ou **IMap&lt;string, T&gt;** no tipo da propriedade que será indexada. Se o tipo da propriedade indexada suportar **IObservableMap** e a associação for OneWay ou TwoWay, então registrará e escutará notificações de alteração nessas interfaces. A lógica de detecção de alteração irá atualizar com base em todas as alterações de coleção, mesmo que não afete o valor indexado específico. Isso ocorre porque a lógica de escuta é comum em todas as instâncias da coleção.

### <a name="attached-properties"></a>Propriedades Anexadas

Para associar a [Propriedades anexadas](./attached-properties-overview.md), você precisa colocar o nome da classe e da propriedade entre parênteses após o ponto. Por exemplo **Text="{x:Bind Button22.(Grid.Row)}"** . Se a propriedade não é declarada em um namespace Xaml, você precisará prefixá-la com um namespace xml, que você deve mapear para um namespace de código no head do documento.

### <a name="casting"></a>Conversão

Associações compiladas são fortemente tipadas e resolverão o tipo de cada etapa em um caminho. Se o tipo retornado não tiver o membro, falhará no momento da compilação. Você pode especificar uma conversão para informar o tipo real do objeto de associação.

No caso a seguir, **obj** é uma propriedade do objeto de tipo, mas que contém uma caixa de texto, para que possamos usar **Text="{x:Bind ((TextBox)obj).Text}"** ou **Text="{x:Bind obj.(TextBox.Text)}"** .

O campo **groups3** em **text = "{x:Bind ((data: SampleDataGroup) groups3\[0\]). Title} "** é um dicionário de objetos, portanto, você deve convertê-lo em **dados: SampleDataGroup**. Observe o uso do prefixo de namespace xml **data:** para mapear o tipo de objeto para um namespace de código que não seja parte do namespace XAML padrão.

_Observação: a C#sintaxe de conversão-Style é mais flexível do que a sintaxe de propriedade anexada e é a sintaxe recomendada no futuro._

#### <a name="pathless-casting"></a>Conversão de caminho

O analisador de ligação nativo não fornece uma palavra-chave para representar `this` como um parâmetro de função, mas oferece suporte à conversão sem caminhos (por exemplo, `{x:Bind (x:String)}`), que pode ser usada como um parâmetro de função. Portanto, `{x:Bind MethodName((namespace:TypeOfThis))}` é uma maneira válida de executar o que é conceitualmente equivalente a `{x:Bind MethodName(this)}`.

Exemplo:

`Text="{x:Bind local:MainPage.GenerateSongTitle((local:SongItem))}"`

```xaml
<Page
    x:Class="AppSample.MainPage"
    ...
    xmlns:local="using:AppSample">

    <Grid>
        <ListView ItemsSource="{x:Bind Songs}">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:SongItem">
                    <TextBlock
                        Margin="12"
                        FontSize="40"
                        Text="{x:Bind local:MainPage.GenerateSongTitle((local:SongItem))}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</Page>
```

```csharp
namespace AppSample
{
    public class SongItem
    {
        public string TrackName { get; private set; }
        public string ArtistName { get; private set; }

        public SongItem(string trackName, string artistName)
        {
            ArtistName = artistName;
            TrackName = trackName;
        }
    }

    public sealed partial class MainPage : Page
    {
        public List<SongItem> Songs { get; }
        public MainPage()
        {
            Songs = new List<SongItem>()
            {
                new SongItem("Track 1", "Artist 1"),
                new SongItem("Track 2", "Artist 2"),
                new SongItem("Track 3", "Artist 3")
            };

            this.InitializeComponent();
        }

        public static string GenerateSongTitle(SongItem song)
        {
            return $"{song.TrackName} - {song.ArtistName}";
        }
    }
}
```

## <a name="functions-in-binding-paths"></a>Funções em caminhos de associação

Desde o Windows 10, versão 1607, **{x: Bind}** dá suporte ao uso de uma função como a etapa de folha do caminho de associação. Esse é um recurso poderoso para DataBinding que permite vários cenários na marcação. Consulte [associações de função](../data-binding/function-bindings.md) para obter detalhes.

## <a name="event-binding"></a>Associação de evento

Associação de evento é um recurso exclusivo para associação compilada. Permite que você especifique o manipulador para um evento usando uma associação, em vez de ele ter de ser um método no code-behind. Por exemplo: **Click="{x:Bind rootFrame.GoForward}"** .

Para eventos, o método de destino não deve ser sobrecarregado e também deve:

- Corresponder à assinatura do evento.
- OU não ter parâmetros.
- OU ter o mesmo número de parâmetros de tipos que podem ser atribuídos com base nos tipos dos parâmetros de evento.

No code-behind gerado, associação compilada manipula o evento e o encaminha para o método no modelo, avaliando o caminho da expressão de associação quando o evento ocorre. Isso significa que, diferentemente das associações de propriedade, não controla as alterações no modelo.

Para saber mais sobre a sintaxe de cadeia de caracteres de um caminho de propriedade, veja [Sintaxe de caminho de propriedade](property-path-syntax.md), tendo em mente as diferenças descritas aqui para **{x:Bind}** .

## <a name="properties-that-you-can-set-with-xbind"></a>Propriedades que você pode definir com {x: Bind}

**{x:Bind}** é ilustrado com a sintaxe de espaço reservado *bindingProperties* porque há várias propriedades de leitura/gravação que podem ser definidas na extensão de marcação. As propriedades podem ser definidas em qualquer ordem com pares *propName*=*value* separados por vírgula. Observe que você não pode incluir quebras de linha na expressão de associação. Algumas das propriedades exigem tipos que não têm uma conversão de tipo específica, então são necessárias extensões de marcação próprias aninhadas no **{x:Bind}** .

Essas propriedades funcionam da mesma forma que as propriedades da classe [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding).

| Propriedade | Descrição |
|----------|-------------|
| **Multi-Path** | Consulte a seção [Caminho de propriedade](#property-path) acima. |
| **Convertido** | Especifica o objeto de conversor que é chamado pelo mecanismo de associação. O conversor pode ser definido em XAML, mas somente se você se referir a uma instância de objeto atribuída em uma referência [{StaticResource} markup extension](staticresource-markup-extension.md) ao objeto no dicionário de recursos. |
| **ConverterLanguage** | Especifica a cultura a ser usada pelo conversor. (Se você estiver configurando **ConverterLanguage** também deverá configurar **Converter**.) A cultura é definida como um identificador baseado em padrões. Para saber mais, veja [**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage). |
| **ConverterParameter** | Especifica o parâmetro do conversor que pode ser usado na lógica de conversão. (Se estiver definindo **ConverterParameter**, você também deve definir **Converter**). A maioria dos conversores usam lógica simples que obtém todas as informações que precisam do valor calculado para converter, e não precisam de um valor **ConverterParameter**. O parâmetro **ConverterParameter** é para implementações moderadamente avançadas que têm mais de uma lógica que controla o que for passado em **ConverterParameter**. Você também pode escrever um conversor que usa valores diferentes de cadeias de caracteres, mas isso é incomum; veja Comentários em [**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter) para saber mais. |
| **FallbackValue** | Especifica um valor a ser exibido quando a fonte ou o caminho não podem ser resolvidos. |
| **Modo** | Especifica o mode de associação como uma dessas cadeias de caracteres: "OneTime", "OneWay" ou "TwoWay". O padrão é "OneTime". Observe que isso é diferente do padrão para **{Binding}** , que é "OneWay" na maioria dos casos. |
| **TargetNullValue** | Especifica um valor a ser exibido quando o valor de origem é solucionado, mas é explicitamente **null**. |
| **BindBack** | Especifica uma função a ser usada na direção inversa de uma associação bidirecional. |
| **UpdateSourceTrigger** | Especifica quando enviar as alterações de volta a partir do controle para o modelo na associações de TwoWay. O padrão para todas as propriedades, exceto TextBox. Text, é PropertyChanged; TextBox. Text é LostFocus.|

> [!NOTE]
> Se você estiver convertendo a marcação de **{Binding}** em **{x:Bind}** , examine as diferenças nos valores padrão da propriedade **Mode**.
> [**x:DefaultBindMode**](https://docs.microsoft.com/windows/uwp/xaml-platform/x-defaultbindmode-attribute) pode ser usado para alterar o modo padrão de x:BIND para um segmento específico da árvore de marcação. O modo selecionado aplicará qualquer expressão x:Bind nesse elemento e seus filhos, que não especificam explicitamente um modo como parte da vinculação. OneTime é mais eficiente do que OneWay, uma vez que usar OneWay fará com que mais código seja gerado para conectar e manipular a detecção de alteração.

## <a name="remarks"></a>Comentários

Como **{x:Bind}** usa código gerado para atingir seus benefícios, requer informações de tipo em tempo de compilação. Isso significa que você não pode associar a propriedades onde você não souber o tipo antes do tempo. Por isso, você não pode usar **{x:Bind}** com a propriedade **DataContext**, que é do tipo **Object** e também está sujeita a alterações no tempo de execução.

Ao usar **{x:bind}** com modelos de dados, você deve indicar o tipo que está sendo associado definindo um valor de **x:DataType** , conforme mostrado na seção de [exemplos](#examples) . Você pode também definir o tipo para um tipo de classe de interface ou base, e então use projeções, se necessário, para formular uma expressão completa.

As associações compiladas dependem da geração de código. Portanto, se você usar **{x:Bind}** em um dicionário de recursos, o dicionário de recursos precisará ter uma classe code-behind. Veja [Dicionários de recursos com {x:Bind}](../data-binding/data-binding-in-depth.md#resource-dictionaries-with-x-bind) para obter um exemplo de código.

Páginas e controles de usuário que incluem associações compiladas terão uma propriedade "Bindings" no código gerado. Entre elas estão os seguintes métodos:

- **Update()** - Isso atualizará os valores de todas as associações compiladas. Uma associação unidirecional/bidirecional terá os ouvintes conectados para detectar alterações.
- **Initialize()** - Se as associações ainda não tiverem sido inicializadas, ela chamará Update() para inicializá-las
- **StopTracking()** - Ela desconectará todos os ouvintes criados para associações uni e bidirecionais. Elas podem ser reinicializadas usando-se o método Update().

> [!NOTE]
> A partir do Windows 10, versão 1607, a estrutura XAML fornece um Booliano integrado para conversor de Visibilidade. O conversor mapeia **true** para o valor de enumeração **Visible** e **falso** para **Collapsed** para que você possa associar uma propriedade de Visibilidade a um Boolenao sem criar um conversor. Observe que isso não é um recurso de associação de função e sim de associação de propriedade. Para usar o conversor integrado, a versão do SDK de alvo mínimo do seu aplicativo deve ser 14393 ou posterior. Você não poderá usá-lo se seu aplicativo for voltado para versões anteriores do Windows 10. Para saber mais sobre as versões de destino, consulte [Código adaptável de versão](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

  **Tip** se você precisar especificar uma chave única para um valor, como em [**Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) ou [**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter), preceda-a com uma barra invertida: `\{`. De forma alternativa, colo a cadeia de caracteres inteira que contém a chave que precisa de escape entre apóstrofos; por exemplo, `ConverterParameter='{Mix}'`.

O [**conversor**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter), [**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage) e **ConverterLanguage** estão todos relacionados ao cenário de conversão de um valor ou tipo da origem da associação em um tipo ou valor que é compatível com a propriedade de destino de associação. Para saber mais, veja a seção "Conversões de dados" em [Vinculação de dados em detalhes](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

**{x:Bind}** é apenas uma extensão de marcação, sem nenhuma maneira de criar ou manipular essas associações de forma programática. Para saber mais sobre extensões de marcação, veja [Visão geral do XAML](xaml-overview.md).

