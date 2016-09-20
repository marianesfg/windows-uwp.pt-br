---
author: jwmsft
description: "A extensão de marcação xBind é uma alternativa a Binding. xBind não tem alguns dos recursos de Binding, mas ele é executado em menos tempo e usando menos memória do que Binding e suporta melhor a depuração."
title: "Extensão de marcação xBind"
ms.assetid: 529FBEB5-E589-486F-A204-B310ACDC5C06
ms.sourcegitcommit: 98b9bca2528c041d2fdfc6a0adead321737932b4
ms.openlocfilehash: ceb5562ae08d7cc966f80fdb7e23f12afe040430

---

# Extensão de marcação {x:Bind}

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


            **Observação** Para obter informações gerais sobre o uso da vinculação de dados no seu aplicativo com **{x:Bind}** (e para todas as comparações entre **{x:Bind}** e **{Binding}**), consulte [Vinculação de dados em detalhes](https://msdn.microsoft.com/library/windows/apps/mt210946).

A extensão de marcação **{x: Bind}** — nova no Windows 10 — é uma alternativa ao **{Binding}**. 
           **{x:Bind}** não tem alguns dos recursos de **{Binding}**, mas ele é executado em menos tempo e usando menos memória do que **{Binding}** e suporta melhor a depuração.

No tempo de carregamento do XAML, **{x:Bind}** é convertido no que parece um objeto de associação, e esse objeto obtém um valor de uma propriedade numa fonte de dados. O objeto de associação, opcionalmente, pode ser configurado para observar mudanças no valor da propriedade de origem de dados e atualizar-se com base nessas alterações. Ele também pode ser configurado opcionalmente para enviar as alterações em seu próprio valor de volta para a propriedade de origem. Os objetos de associação criados por **{x:Bind}** e **{Binding}** são em grande parte funcionalmente equivalentes. Mas o **{x:Bind}** executa o código de finalidade especial, que ele gera no tempo de compilação, e o **{Binding}** usa inspeção de objeto de tempo de execução de finalidade geral. Consequentemente, as associações **{x:Bind}** (normalmente chamadas de associações compiladas) têm excelente desempenho, fornecem validação de tempo de compilação de suas expressões de associação e dão suporte à depuração permitindo que você defina pontos de interrupção nos arquivos de código que são gerados como a classe parcial para sua página. Esses arquivos podem ser encontrados em sua pasta `obj`, com nomes como (para C#) `<view name>.g.cs`.

**Aplicativos de amostra que demonstram {x:Bind}**

-   [amostra de {x:Bind}](http://go.microsoft.com/fwlink/p/?linkid=619989)
-   [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame)
-   [Amostra de noções básicas de interface do usuário XAML](http://go.microsoft.com/fwlink/p/?linkid=619992)

## Uso do atributo XAML

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
| 
            _propName_
            =
            _value_\[, _propName_=_value_\]* | Uma ou mais propriedades de associação que são especificadas usando uma sintaxe de par nome/valor. |
| _propName_ | O nome da cadeia de caracteres da propriedade a ser definida no objeto Binding. Por exemplo, "Converter". | 
| _value_ | O valor a se definir a propriedade. A sintaxe do argumento depende da propriedade que está sendo definida. Veja um exemplo de uso de _propName_=_value_ em que o valor é uma extensão de marcação: `Converter={StaticResource myConverterClass}`. Para obter mais informações, consulte a seção [Propriedades que você pode definir com {x: Bind}](#properties-you-can-set) a seguir. | 

## Caminho de propriedade


            *PropertyPath* define a propriedade **Path** de uma expressão **{x:Bind}**. 
            **Path** é um caminho de propriedade que especifica o valor da propriedade, da subpropriedade, do campo ou do método ao qual você está se associando (à origem). Você pode mencionar o nome da propriedade **Path** explicitamente: `{Binding Path=...}`. Ou você pode omiti-lo: `{Binding ...}`.


           **{x:Bind}** não usa o **DataContext** como uma fonte padrão; em vez disso, usa o controle de página ou do usuário propriamente dito. Portanto, ele aparecerá no code-behind de sua página ou controle de usuário para métodos, propriedades e campos. Para expor seu modelo de exibição **{x:Bind}**, você geralmente deseja adicionar novos campos ou propriedades para o code-behind para sua página ou controle de usuário. Etapas de um caminho de propriedade são delimitadas por pontos (.), e você pode incluir vários delimitadores para percorrer subpropriedades sucessivas. Use o ponto delimitador independentemente da linguagem de programação usada para implementar o objeto sendo associado.

Por exemplo: em uma página, **Text="{x:Bind Employee.FirstName}"** procurará por um membro **Funcionário** na página e, em seguida, um membro **FirstName** no objeto retornado por **Funcionário**. Se você estiver associando o controle de um item a uma propriedade que contém dependentes de um funcionário, o seu caminho de propriedade pode ser "Employee.Dependents", o modelo de item do controle do item exibiria os itens em "Dependents".

Para C++/CX, **{x:Bind}** não é possível associar campos particulares e propriedades no modelo de página ou dados – você precisará ter uma propriedade pública para que seja associável. A área de superfície para associação precisa ser exposta como classes/interfaces CX para que possamos obter os metadados relevantes. O atributo **\[Bindable\]** não deve ser necessário.

Se a fonte de dados for uma colação, um caminho de propriedade pode especificar itens na coleção pela posição ou índice. Por exemplo, "Teams\[0\].Players", em que o "\[\]" literal contém o "0" que solicita o primeiro item em uma coleção com índice zero.

Para usar um indexador, o modelo precisa implementar **IList&lt;T&gt;** ou **IVector&lt;T&gt;** no tipo de propriedade que vai ser indexada. Se o tipo da propriedade indexada der suporte a **INotifyCollectionChanged** ou **IObservableVector** e a associação for OneWay ou TwoWay, então registrará e escutará notificações de alteração nessas interfaces. A lógica de detecção de alteração irá atualizar com base em todas as alterações de coleção, mesmo que não afete o valor indexado específico. Isso ocorre porque a lógica de escuta é comum em todas as instâncias da coleção.

Para associar a propriedades anexadas, você precisa colocar o nome de classe e propriedade entre parênteses depois do ponto. Por exemplo **Text="{x:Bind Button22.(Grid.Row)}"**. Se a propriedade não é declarada em um namespace Xaml, você precisará prefixá-la com um namespace xml, que você deve mapear para um namespace de código no head do documento.

Associações compiladas são fortemente tipadas e resolverão o tipo de cada etapa em um caminho. Se o tipo retornado não tiver o membro, falhará no momento da compilação. Você pode especificar uma conversão para informar o tipo real do objeto de associação. No caso a seguir, **obj** é uma propriedade do objeto de tipo, mas que contém uma caixa de texto, para que possamos usar **Text="{x:Bind obj.(TextBox.Text)}"**.

O campo **groups3** em **Text="{x:Bind groups3\[0\].(data:SampleDataGroup.Title)}"** é um dicionário de objetos, portanto, você deve convertê-lo para **data:SampleDataGroup**. Observe o uso do prefixo de namespace **data:** para mapear o tipo de objeto para um namespace que não seja parte do namespace XAML padrão.

Com **x:Bind**, você não precisa usar **ElementName=xxx** como parte da expressão de associação. Com **x:Bind**, você pode usar o nome do elemento como a primeira parte do caminho para a associação, porque elementos nomeados tornam-se campos dentro do controle de página ou de usuário que representa a origem da associação raiz.

Associação de evento é um novo recurso para associação compilada. Permite que você especifique o manipulador para um evento usando uma associação, em vez de ele ter de ser um método no code-behind. Por exemplo: **Click="{x:Bind rootFrame.GoForward}"**.

Para eventos, o método de destino não deve ser sobrecarregado e também deve:

-   Corresponder à assinatura do evento.
-   OU não ter parâmetros.
-   OU ter o mesmo número de parâmetros de tipos que podem ser atribuídos com base nos tipos dos parâmetros de evento.

No code-behind gerado, associação compilada manipula o evento e o encaminha para o método no modelo, avaliando o caminho da expressão de associação quando o evento ocorre. Isso significa que, diferentemente das associações de propriedade, não controla as alterações no modelo.

Para saber mais sobre a sintaxe de cadeia de caracteres de um caminho de propriedade, veja [Sintaxe de caminho de propriedade](property-path-syntax.md), tendo em mente as diferenças descritas aqui para **{x:Bind}**.

##  Propriedades que você pode definir com {x: Bind}



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


            **Observação** Se você estiver convertendo a marcação de **{Binding}** para **{x:Bind}**, examine as diferenças nos valores padrão da propriedade **Mode**.
 
## Comentários

Como **{x:Bind}** usa código gerado para atingir seus benefícios, requer informações de tipo em tempo de compilação. Isso significa que você não pode associar a propriedades onde você não souber o tipo antes do tempo. Por isso, você não pode usar **{x:Bind}** com a propriedade **DataContext**, que é do tipo **Object** e também está sujeita a alterações no tempo de execução.

Ao usar **{x:Bind}** com modelos de dados, você deve indicar o tipo associado definindo um **x:DataType**, conforme mostrado no exemplo a seguir. Você pode também definir o tipo para um tipo de classe de interface ou base, e então use projeções, se necessário, para formular uma expressão completa.

As associações compiladas dependem da geração de código. Portanto, se você usar **{x:Bind}** em um dicionário de recursos, o dicionário de recursos precisará ter uma classe code-behind. Veja [Dicionários de recursos com {x:Bind}](../data-binding/data-binding-in-depth.md#resource-dictionaries-with-x-bind) para obter um exemplo de código.


            **Importante** Se você definir um valor local para uma propriedade que anteriormente tinha uma extensão de marcação **{x:Bind}** para fornecer um valor local, a associação será totalmente removida.


            **Dica** Se você precisar especificar uma única chave para um valor, como em [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) ou [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827), coloque uma barra invertida antes dela: `\{`. De forma alternativa, colo a cadeia de caracteres inteira que contém a chave que precisa de escape entre apóstrofos; por exemplo, `ConverterParameter='{Mix}'`.


            [
              **Converter**
            ](https://msdn.microsoft.com/library/windows/apps/br209826), [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) e **ConverterLanguage** são todos relacionados à situação de conversão de um valor ou tipo de uma fonte de associação a um tipo ou valor que é compatível com a propriedade do destino da associação. Para saber mais, veja a seção "Conversões de dados" em [Vinculação de dados em detalhes](https://msdn.microsoft.com/library/windows/apps/mt210946).


            **{x:Bind}** é apenas uma extensão de marcação, sem nenhuma maneira de criar ou manipular essas associações de forma programática. Para saber mais sobre extensões de marcação, veja [Visão geral do XAML](xaml-overview.md).

## Exemplos

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




<!--HONumber=Jun16_HO4-->


