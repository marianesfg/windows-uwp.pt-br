---
ms.assetid: 41E1B4F1-6CAF-4128-A61A-4E400B149011
title: Vinculação de dados em detalhes
description: Vinculação de dados é uma maneira da interface do usuário do seu aplicativo exibir dados e, opcionalmente, ficar em sincronia com esses dados.
---
# Vinculação de dados em detalhes

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos do Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


** APIs importantes **

-   [**Classe Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820)
-   [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713)
-   [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899)

**Observação**  Este tópico descreve detalhadamente os recursos de vinculação de dados. Para obter uma introdução breve e prática, consulte [Visão geral da vinculação de dados](data-binding-quickstart.md).

 

A vinculação de dados é uma maneira de a interface do usuário do seu aplicativo exibir dados e, opcionalmente, ficar em sincronia com esses dados. A vinculação de dados permite separar a preocupação dos dados da preocupação da interface do usuário, e isso resulta em um modelo conceitual mais simples, bem como melhor legibilidade, capacidade de teste e capacidade de manutenção do seu aplicativo.

Você pode usar a vinculação de dados simplesmente para exibir os valores de uma fonte de dados quando a interface do usuário é mostrada primeiro, mas não para responder a alterações nesses valores. Isso é chamado de associação única, e funciona bem para dados cujos valores não mudam durante o tempo de execução. Além disso, você pode optar por "observar" os valores e atualizar a interface do usuário quando eles mudarem. Isso é chamado de associação unidirecional, e funciona bem para dados somente leitura. Por fim, você pode optar por observar e atualizar, para que as alterações que o usuário faz aos valores da interface do usuário sejam automaticamente enviadas de volta para a fonte de dados. Isso é chamado de associação bidirecional, e funciona bem para dados somente gravação. Aqui estão alguns exemplos.

-   Você pode usar a associação unidirecional para associar um [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) à foto do usuário atual.
-   Você pode usar a associação unidirecional para associar um [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) a uma coleção de artigos de notícias em tempo real agrupados pela seção de jornal.
-   Você pode usar a associação bidirecional para associar um [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) ao nome do cliente em um formulário.

Existem dois tipos de associação, e ambos normalmente são declarados na marcação da interface do usuário. Você pode optar por usar a [extensão de marcação {x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) ou a [extensão de marcação {Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782). E você ainda pode usar uma combinação das duas no mesmo aplicativo, inclusive no mesmo elemento de interface do usuário. A {x:Bind} é nova para Windows 10 e tem um desempenho melhor. {Binding} tem mais recursos. Todos os detalhes descritos neste tópico se aplicam a ambos os tipos de associação, a menos que explicitamente declarado o contrário.

**Aplicativos de amostra que demonstram {x:Bind}**

-   [Amostra de {x:Bind}](http://go.microsoft.com/fwlink/p/?linkid=619989).
-   [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame).
-   [Amostra de noções básicas de interface do usuário XAML](http://go.microsoft.com/fwlink/p/?linkid=619992).

**Aplicativos de amostra que demonstram {Binding}**

-   Baixe o aplicativo [Bookstore1](http://go.microsoft.com/fwlink/?linkid=532950).
-   Baixe o aplicativo [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952).

Cada associação envolve essas partes
------------------------------------

-   Uma *origem de associação*. Essa é a fonte de dados para a associação e pode ser uma instância de qualquer classe que tem membros cujos valores você deseja exibir em sua interface do usuário.
-   Um *destino da associação*. Este é um [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/BR242362) da [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) em sua interface do usuário que exibe os dados.
-   Um *objeto de associação*. É a parte que transfere os valores de dados da origem ao destino e, opcionalmente, do destino para a origem. O objeto de associação é criado no tempo de carregamento XAML da extensão de marcação [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) ou [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782).

Nas seções a seguir, vamos dar uma olhada na origem da associação, no destino da associação e no objeto de associação. E vincularemos as seções com o exemplo de associação do conteúdo de um botão a uma propriedade de string chamada **NextButtonText**, que pertence a uma classe chamada **HostViewModel**.

Origem de associação
--------------

Consulte uma implementação muito rudimentar de uma classe que poderíamos usar como uma fonte de associação.

**Observação**  Se você estiver usando [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) com extensões de componente do Visual C++ (C ++/CX), então, precisará adicionar o atributo [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) à classe de origem da associação. Se você estiver usando [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783), então não precisará desse atributo. Consulte [Adicionando um modo de exibição de detalhes](data-binding-quickstart.md#adding-a-details-view) para um trecho de código.

```csharp
public class HostViewModel
{
    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText { get; set; }
}
```

Essa implementação de **HostViewModel**, e sua propriedade **NextButtonText**, só são apropriados para associação unidirecional. Mas associações unidirecionais e bidirecionais são extremamente comuns, e, nesses tipos de associação, a interface do usuário é atualizada automaticamente em resposta às alterações nos valores de dados de origem da associação. Para que esses tipos de associação funcionem corretamente, você precisa fazer sua fonte de associação "observável" para o objeto de associação. Assim, em nosso exemplo, se quisermos fazer associação unidirecional ou bidirecional à propriedade **NextButtonText**, todas as alterações que acontecem no tempo de execução para o valor da propriedade precisam ser feitas observáveis para o objeto de associação.

Uma maneira de fazer isso é derivar a classe que representa sua fonte de associação de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/BR242356), e expor um valor de dados por meio de um [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/BR242362). É assim que um [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) se torna observável. **FrameworkElements** são boas fontes de associação prontas para usar.

Uma maneira mais simples de tornar uma classe observável — e uma necessária para as classes que já têm uma classe base — é implementar [**System.ComponentModel.INotifyPropertyChanged**](T:System.ComponentModel.INotifyPropertyChanged). Isso envolve apenas a implementação de um único evento denominado **PropertyChanged**. Um exemplo usando **HostViewModel** está abaixo.

**Observação**  Para C++/CX, você implementa [**Windows::UI::Xaml::Data::INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899), e a classe de origem de associação deve ter o [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) ou implementar [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878).

``` csharp
public class HostViewModel : INotifyPropertyChanged
{
    private string nextButtonText;

    public event PropertyChangedEventHandler PropertyChanged = delegate { };

    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText
    {
        get { return this.nextButtonText; }
        set
        {
            this.nextButtonText = value;
            this.OnPropertyChanged();
        }
    }

    public void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        // Raise the PropertyChanged event, passing the name of the property whose value has changed.
        this.PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

Agora a propriedade **NextButtonText** é observável. Quando você cria uma associação unidirecional ou bidirecional com essa propriedade (mostraremos posteriormente), o objeto de associação resultante assina o evento **PropertyChanged**. Quando esse evento é acionado, o manipulador do objeto de associação recebe um argumento que contém o nome da propriedade que foi alterada. É assim que o objeto de associação sabe qual valor de propriedade usar e ler novamente.

Assim, você não precisa implementar o padrão mostrado acima várias vezes, você só precisa derivar a classe de base **BindableBase** que encontrará na amostra [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame) (na pasta "Comum"). Consulte um exemplo de como fica.

``` csharp
public class HostViewModel : BindableBase
{
    private string nextButtonText;

    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText
    {
        get { return this.nextButtonText; }
        set { this.SetProperty(ref this.nextButtonText, value); }
    }
}
```

Gerar o evento **PropertyChanged** com um argumento de [**String.Empty**](T:System.String) ou **null** indica que todas as propriedades não indexadas no objeto devem ser lidas novamente. Você pode gerar o evento para indicar que as propriedades no indexador do objeto foram alteradas com um argumento de "Item\[*indexer*\]" para indexadores específicos (em que *indexer* é o valor do índice), ou um valor de "Item\[\]" para todos os indexadores.

Uma origem de associação pode ser tratada como um único objeto cujas propriedades contêm dados ou como uma coleção de objetos. No código C# e Visual Basic, você pode usar a associação unidirecional para um objeto que implementa [**List(Of T)**](T:System.Collections.Generic.List%601) para exibir uma coleção que não muda no tempo de execução. Para uma coleção observável (observar quando itens são adicionados e removidos da coleção), use associação unidirecional a [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601). No código C++, você pode associar a [**Vector&lt;T&gt;**](https://msdn.microsoft.com/en-us/library/dn858385.aspx) para coleções observáveis e não observáveis. Para associar às suas próprias classes de coleção, use a diretriz da tabela a seguir.

| Cenário                                                        | C# e VB (CLR)                                                                                                                                                                                                                                                                                                                                                                                                                   | C++/CX                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|-----------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Associar a um objeto.                                              | Pode ser qualquer objeto.                                                                                                                                                                                                                                                                                                                                                                                                                 | O objeto deve ter [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) ou implementar [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878).                                                                                                                                                                                                                                                                                                             |
| Obter atualizações de alterações de propriedades de um objeto associado.                | O objeto deve implementar [**System.ComponentModel. INotifyPropertyChanged**](T:System.ComponentModel.INotifyPropertyChanged).                                                                                                                                                                                                                                                                                                         | O objeto deve implementar [**Windows.UI.Xaml.Data. INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899).                                                                                                                                                                                                                                                                                                                                                           |
| Associar a uma coleção.                                           | [**List(Of T)**](T:System.Collections.Generic.List%601)                                                                                                                                                                                                                                                                                                                                                                            | [**Platform::Collections::Vector&lt;T&gt;**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh441570.aspx)                                                                                                                                                                                                                                                                                                                                                                                         |
| Obter atualizações de alterações de uma coleção associada.          | [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601)                                                                                                                                                                                                                                                                                                                                        | [**Platform::Collections::Vector&lt;T&gt;**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh441570.aspx)                                                                                                                                                                                                                                                                                                                                                                                         |
| Implemente uma coleção com suporte para associação.                   | Estenda [**List(Of T)**](T:System.Collections.Generic.List%601) ou implemente [**IList**](T:System.Collections.IList), [**IList**](T:System.Collections.Generic.IList%601)(Of [**Object**](T:System.Object)), [**IEnumerable**](T:System.Collections.IEnumerable) ou [**IEnumerable**](T:System.Collections.Generic.IEnumerable%601)(Of **Object**). Não há suporte para a associação a **IList(Of T)** genérico e a **IEnumerable(Of T)**. | Implemente [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979), [**IBindableIterable**](https://msdn.microsoft.com/library/windows/apps/Hh701957), [**IVector**](https://msdn.microsoft.com/library/windows/apps/BR206631)&lt;[**Object**](T:System.Object)^&gt;, [**IIterable**](https://msdn.microsoft.com/library/windows/apps/BR226024)&lt;**Object**^&gt;, **IVector**&lt;[**IInspectable**](https://msdn.microsoft.com/library/BR205821)\*&gt; ou **IIterable**&lt;**IInspectable**\*&gt;. Não há suporte para a associação a **IVector&lt;T&gt;** genérico e **IIterable&lt;T&gt;**. |
| Implementar uma coleção com suporte a atualizações de alterações de coleções. | Estenda [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601) ou implemente [**IList**](T:System.Collections.IList) (não genérico) e [**INotifyCollectionChanged**](T:System.Collections.Specialized.INotifyCollectionChanged).                                                                                                                                                               | Implemente [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979) e [**IBindableObservableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701974).                                                                                                                                                                                                                                                                                                                       |
| Implemente uma coleção com suporte para carregamento incremental.       | Estenda [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601) ou implemente [**IList**](T:System.Collections.IList) (não genérico) e [**INotifyCollectionChanged**](T:System.Collections.Specialized.INotifyCollectionChanged). Além disso, implemente [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916).                                                          | Implemente [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979), [**IBindableObservableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701974) e [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916).                                                                                                                                                                                                                                         |

 
Você pode associar controles de lista a fontes de dados arbitrariamente grandes e ainda assim obter um alto desempenho, usando o carregamento incremental. Por exemplo, você pode associar controles de lista a resultados de consulta de imagens do Bing sem precisar carregar todos os resultados de uma vez. Em vez disso, você carrega apenas alguns resultados imediatamente e carrega resultados adicionais conforme o necessário. Para dar suporte ao carregamento incremental, você deve implementar [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916) em uma fonte de dados que dê suporte à notificação de alterações de coleção. Quando o mecanismo de vinculação de dados solicitar mais dados, a fonte de dados deverá fazer as solicitações apropriadas, integrar os resultados e, depois, enviar as notificações apropriadas para atualizar a interface do usuário.

Destino da associação
--------------

Nos dois exemplos a seguir, a propriedade **Button.Content** é o destino da associação, e seu valor é definido como uma extensão de marcação que declara o objeto de associação. Primeiro [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) é mostrado e depois [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782). Declarar as associações na marcação é o caso comum (é conveniente, legível e editável). Porém, você pode evitar a marcação e criar imperativamente (programaticamente) uma instância da classe [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820), se precisar.

<!-- XAML lang specifier not yet supported in OP. Using XML for now. -->
``` xml
<Button Content="{x:Bind ...}" ... />
```

``` xml
<Button Content="{Binding ...}" ... />
```

Binding object declared using {x:Bind}
--------------------------------------

There's one step we need to do before we author our [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) markup. We need to expose our binding source class from the class that represents our page of markup. We do that by adding a property (of type **HostViewModel** in this case) to our **HostView** page class.

``` csharp
namespace QuizGame.View
{
    public sealed partial class HostView : Page
    {
        public HostView()
        {
            this.InitializeComponent();
            this.ViewModel = new HostViewModel();
        }
    
        public HostViewModel ViewModel { get; set; }
    }
}
```

Quando concluído, podemos dar uma olhada na marcação que declara o objeto de associação. O exemplo a seguir usa o mesmo destino da associação **Button.Content** que usamos na seção "Destino da associação" anteriormente, e mostra a vinculação à propriedade **HostViewModel.NextButtonText**.

``` xml
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

Notice the value that we specify for **Path**. This value is interpreted in the context of the page itself, and in this case the path begins by referencing the **ViewModel** property that we just added to the **HostView** page. That property returns a **HostViewModel** instance, and so we can dot into that object to access the **HostViewModel.NextButtonText** property. And we specify **Mode**, to override the [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) default of one-time.

The [**Path**](https://msdn.microsoft.com/library/windows/apps/BR209820-path) property supports a variety of syntax options for binding to nested properties, attached properties, and integer and string indexers. For more info, see [Property-path syntax](https://msdn.microsoft.com/library/windows/apps/Mt185586). Binding to string indexers gives you the effect of binding to dynamic properties without having to implement [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878). For other settings, see [{x:Bind} markup extension](https://msdn.microsoft.com/library/windows/apps/Mt204783).

**Note**  Changes to [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/BR209683-text) are sent to a two-way bound source when the [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) loses focus, and not after every user keystroke.

**DataTemplate and x:DataType**

Inside a [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348) (whether used as an item template, a content template, or a header template), the value of **Path** is not interpreted in the context of the page, but in the context of the data object being templated. So that its bindings can be validated (and efficient code generated for them) at compile-time, a **DataTemplate** needs to declare the type of its data object using **x:DataType**. The example given below could be used as the **ItemTemplate** of an items control bound to a collection of **SampleDataGroup** objects.

``` xml
<DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

**Weakly-typed objects in your Path**

Consider for example that you have a type named SampleDataGroup, which implements a string property named Title. And you have a property MainPage.SampleDataGroupAsObject, which is of type object but which actually returns an instance of SampleDataGroup. The binding `<TextBlock Text="{x:Bind SampleDataGroupAsObject.Title}"/>` will result in a compile error because the Title property is not found on the type object. The remedy for this is to add a cast to your Path syntax like this: `<TextBlock Text="{x:Bind SampleDataGroupAsObject.(data:SampleDataGroup.Title)}"/>`. Here's another example where Element is declared as object but is actually a TextBlock: `<TextBlock Text="{x:Bind Element.Text}"/>`. And a cast remedies the issue: `<TextBlock Text="{x:Bind Element.(TextBlock.Text)}"/>`.

**Se os dados são carregados de forma assíncrona,**

código para dar suporte a **{x:Bind}** é gerado em tempo de compilação nas classes parciais para suas páginas. Esses arquivos podem ser encontrados em sua pasta `obj`, com nomes como (para C#) `<view name>.g.cs`. The generated code includes a handler for your page's [**Loading**](https://msdn.microsoft.com/library/windows/apps/BR208706-loading) event, and that handler calls the **Initialize** method on a generated class that represent's your page's bindings. **Initialize** in turn calls **Update** to begin moving data between the binding source and the target. **Loading** is raised just before the first measure pass of the page or user control. So if your data is loaded asynchronously it may not be ready by the time **Initialize** is called. So, after you've loaded data, you can force one-time bindings to be initialized by calling `this->Bindings->Update();`. Se você só precisa de associações únicas para dados carregados de forma assíncrona, então é muito mais barato inicializá-las assim do que ter associações unidirecionais e ouvir as alterações. Se os seus dados não sofrerem mudanças detalhadas, e se é provável que sejam atualizados como parte de uma ação específica, então, você pode tornar suas associações únicas e forçar uma atualização manual a qualquer momento com uma chamada para **Update**.

**Limitações**

**{x:Bind}** não é adequado para cenários de associação tardia, como navegar na estrutura de dicionário de um objeto JSON, nem "duck typing" que é uma forma fraca de digitação com base em correspondências lexicais em nomes de propriedade ("se ele anda, nada, grasna como um pato, então é um pato"). Com "duck typing", uma associação à propriedade Age seria atendida igualmente com um objeto Pessoa ou Vinho. Para esses cenários, use **{Binding}**.

O objeto de associação declarado com o uso de {Binding}
---------------------------------------

[{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) presume, por padrão, que você está associando ao [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) de sua página de marcação. Portanto, definiremos o **DataContext** de nossa página como uma instância de nossa classe de origem de associação (do tipo **HostViewModel**, nesse caso). O exemplo a seguir mostra a marcação que declara o objeto de associação. Usamos o mesmo destino da associação **Button.Content** usado na seção "Destino da associação" anteriormente e associamos à propriedade **HostViewModel.NextButtonText**.

``` xml
<Page xmlns:viewmodel="using:QuizGame.ViewModel" ... >
    <Page.DataContext>
        <viewmodel:HostViewModel/>
    </Page.DataContext>
    ...
    <Button Content="{Binding Path=NextButtonText}" ... />
</Page>
```

Notice the value that we specify for **Path**. This value is interpreted in the context of the page's [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713), which in this example is set to an instance of **HostViewModel**. The path references the **HostViewModel.NextButtonText** property. We can omit **Mode**, because the [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) default of one-way works here.

The default value of [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) for a UI element is the inherited value of its parent. You can of course override that default by setting **DataContext** explicitly, which is in turn inherited by children by default. Setting **DataContext** explicitly on an element is useful when you want to have multiple bindings that use the same source.

A binding object has a **Source** property, which defaults to the [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) of the UI element on which the binding is declared. You can override this default by setting **Source**, **RelativeSource**, or **ElementName** explicitly on the binding (see [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) for details).

Inside a [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348), the [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) is set to the data object being templated. The example given below could be used as the **ItemTemplate** of an items control bound to a collection of any type that has string properties named **Title** and **Description**.

``` xml
<DataTemplate x:Key="SimpleItemTemplate">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{Binding Title}"/>
      <TextBlock Text="{Binding Description"/>
    </StackPanel>
  </DataTemplate>
```

**Note**  By default, changes to [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/BR209683-text) are sent to a two-way bound source when the [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) loses focus. To cause changes to be sent after every user keystroke, set **UpdateSourceTrigger** to **PropertyChanged** on the binding in markup. You can also completely take control of when changes are sent to the source by setting **UpdateSourceTrigger** to **Explicit**. You then handle events on the text box (typically [**TextBox.TextChanged**](https://msdn.microsoft.com/library/windows/apps/BR209683-textchanged)), call [**GetBindingExpression**](https://msdn.microsoft.com/library/windows/apps/BR208706-getbindingexpression) on the target to get a [**BindingExpression**](https://msdn.microsoft.com/library/windows/apps/BR209820expression) object, and finally call [**BindingExpression.UpdateSource**](https://msdn.microsoft.com/library/windows/apps/BR209820expression-updatesource) to programmatically update the data source.

The [**Path**](https://msdn.microsoft.com/library/windows/apps/BR209820-path) property supports a variety of syntax options for binding to nested properties, attached properties, and integer and string indexers. For more info, see [Property-path syntax](https://msdn.microsoft.com/library/windows/apps/Mt185586). Binding to string indexers gives you the effect of binding to dynamic properties without having to implement [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878). The [**ElementName**](https://msdn.microsoft.com/library/windows/apps/BR209820-elementname) property is useful for element-to-element binding. The [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/BR209820-relativesource) property has several uses, one of which is as a more powerful alternative to template binding inside a [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/BR209391). For other settings, see [{Binding} markup extension](https://msdn.microsoft.com/library/windows/apps/Mt204782) and the [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820) class.

What if the source and the target are not the same type?
--------------------------------------------------------

If you want to control the visibility of a UI element based on the value of a boolean property, or if you want to render a UI element with a color that's a function of a numeric value's range or trend, or if you want to display a date and/or time value in a UI element property that expects a string, then you'll need to convert values from one type to another. There will be cases where the right solution is to expose another property of the right type from your binding source class, and keep the conversion logic encapsulated and testable there. But that isn't flexible nor scalable when you have large numbers, or large combinations, of source and target properties. In that case you'll want to use something known as a value converter. This section describes how to implement and consume a value converter.

Here's a value converter, suitable for a one-time or a one-way binding, that converts a [**DateTime**](T:System.DateTime) value to a string value containing the month. The class implements [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/BR209903).

``` csharp
public class DateToStringConverter : IValueConverter
{
    // Define the Convert method to convert a DateTime value to 
    // a month string.
    public object Convert(object value, Type targetType, 
        object parameter, string language)
    {
        // value is the data from the source object.
        DateTime thisdate = (DateTime)value;
        int monthnum = thisdate.Month;
        string month;
        switch (monthnum)
        {
            case 1:
                month = "January";
                break;
            case 2:
                month = "February";
                break;
            default:
                month = "Month not found";
                break;
        }
        // Return the value to pass to the target.
        return month;
    }

    // ConvertBack is not implemented for a OneWay binding.
    public object ConvertBack(object value, Type targetType, 
        object parameter, string language)
    {
        throw new NotImplementedException();
    }
}
```

``` vbnet
Public Class DateToStringConverter
    Implements IValueConverter

    ' Define the Convert method to change a DateTime object to
    ' a month string.
    Public Function Convert(ByVal value As Object, -
        ByVal targetType As Type, ByVal parameter As Object, -
        ByVal language As String) As Object -
        Implements IValueConverter.Convert

        ' value is the data from the source object.
        Dim thisdate As DateTime = CType(value, DateTime)
        Dim monthnum As Integer = thisdate.Month
        Dim month As String
        Select Case (monthnum)
            Case 1
                month = "January"
            Case 2
                month = "February"
            Case Else
                month = "Month not found"
        End Select
        ' Return the value to pass to the target.
        Return month

    End Function

    ' ConvertBack is not implemented for a OneWay binding.
    Public Function ConvertBack(ByVal value As Object, -
        ByVal targetType As Type, ByVal parameter As Object, -
        ByVal language As String) As Object -
        Implements IValueConverter.ConvertBack

        Throw New NotImplementedException

    End Function
End Class
```

E aqui está como consumir esse conversor de valor em sua marcação de objeto de associação.

``` xml
<UserControl.Resources>
  <local:DateToStringConverter x:Key="Converter1"/>
</UserControl.Resources>

...

<TextBlock Grid.Column="0" 
  Text="{x:Bind ViewModel.Month, Converter={StaticResource Converter1}}"/>

<TextBlock Grid.Column="0" 
  Text="{Binding Month, Converter={StaticResource Converter1}}"/>
```

The binding engine calls the [**Convert**](https://msdn.microsoft.com/library/windows/apps/BR209903-convert) and [**ConvertBack**](https://msdn.microsoft.com/library/windows/apps/BR209903-convertback) methods if the [**Converter**](https://msdn.microsoft.com/library/windows/apps/BR209820-converter) parameter is defined for the binding. When data is passed from the source, the binding engine calls **Convert** and passes the returned data to the target. When data is passed from the target (for a two-way binding), the binding engine calls **ConvertBack** and passes the returned data to the source.

The converter also has optional parameters: [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/BR209820-converterlanguage), which allows specifying the language to be used in the conversion, and [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/BR209820-converterparameter), which allows passing a parameter for the conversion logic. For an example that uses a converter parameter, see [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/BR209903).

**Note**  If there is an error in the conversion, do not throw an exception. Instead, return [**DependencyProperty.UnsetValue**](https://msdn.microsoft.com/library/windows/apps/BR242362-unsetvalue), which will stop the data transfer.

To display a default value to use whenever the binding source cannot be resolved, set the **FallbackValue** property on the binding object in markup. This is useful to handle conversion and formatting errors. It is also useful to bind to source properties that might not exist on all objects in a bound collection of heterogeneous types.

If you bind a text control to a value that is not a string, the data binding engine will convert the value to a string. If the value is a reference type, the data binding engine will retrieve the string value by calling [**ICustomPropertyProvider.GetStringRepresentation**](https://msdn.microsoft.com/library/windows/apps/BR209878-getstringrepresentation) or [**IStringable.ToString**](https://msdn.microsoft.com/library/Dn302136) if available, and will otherwise call [**Object.ToString**](M:System.Object.ToString). Note, however, that the binding engine will ignore any **ToString** implementation that hides the base-class implementation. Subclass implementations should override the base class **ToString** method instead. Similarly, in native languages, all managed objects appear to implement [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878) and [**IStringable**](https://msdn.microsoft.com/library/Dn302135). However, all calls to **GetStringRepresentation** and **IStringable.ToString** are routed to **Object.ToString** or an override of that method, and never to a new **ToString** implementation that hides the base-class implementation.

Resource dictionaries with {x:Bind}
-----------------------------------

The [{x:Bind} markup extension](https://msdn.microsoft.com/library/windows/apps/Mt204783) depends on code generation, so it needs a code-behind file containing a constructor that calls **InitializeComponent** (to initialize the generated code). You re-use the resource dictionary by instantiating its type (so that **InitializeComponent** is called) instead of referencing its filename. Here's an example of what to do if you have an existing resource dictionary and you want to use {x:Bind} in it.

TemplatesResourceDictionary.xaml

``` xml
<ResourceDictionary
    x:Class="ExampleNamespace.TemplatesResourceDictionary"
    .....
    xmlns:examplenamespace="using:ExampleNamespace">
    
    <DataTemplate x:Key="EmployeeTemplate" x:DataType="examplenamespace:IEmployee">
        <Grid>
            <TextBlock Text="{x:Bind Name}"/>
        </Grid>
    </DataTemplate>
</ResourceDictionary>
```

TemplatesResourceDictionary.xaml.cs

``` csharp
using Windows.UI.Xaml.Data;
 
namespace ExampleNamespace
{
    public partial class TemplatesResourceDictionary
    {
        public TemplatesResourceDictionary()
        {
            InitializeComponent();
        }
    }
}
```

MainPage.xaml

``` xml
<Page x:Class="ExampleNamespace.MainPage"
    ....
    xmlns:examplenamespace="using:ExampleNamespace">

    <Page.Resources>
        <ResourceDictionary>
            .... 
            <ResourceDictionary.MergedDictionaries>
                <examplenamespace:TemplatesResourceDictionary/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>
</Page>
```

Event binding and ICommand
--------------------------

[{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) supports a feature called event binding. With this feature, you can specify the handler for an event using a binding, which is an additional option on top of handling events with a method on the code-behind file. Let's say you have a **RootFrame** property on your **MainPage** class.

``` csharp
    public sealed partial class MainPage : Page
    {
        ....    
        public Frame RootFrame { get { return Window.Current.Content as Frame; } }
    }
```

Em seguida, você pode associar um evento **Click** de um botão a um método no objeto **Frame** retornado pela propriedade **RootFrame** como este. Observe que nós também associamos a propriedade **IsEnabled** do botão a outro membro do mesmo **Frame**.

``` xml
    <AppBarButton Icon="Forward" IsCompact="True"
    IsEnabled="{x:Bind RootFrame.CanGoForward, Mode=OneWay}"
    Click="{x:Bind RootFrame.GoForward}"/>
```

Overloaded methods cannot be used to handle an event with this technique. Also, if the method that handles the event has parameters then they must all be assignable from the types of all of the event's parameters, respectively. In this case, [**Frame.GoForward**](https://msdn.microsoft.com/library/windows/apps/BR242693) is not overloaded and it has no parameters (but it would still be valid even if it took two **object** parameters). [**Frame.GoBack**](https://msdn.microsoft.com/library/windows/apps/Dn996568) is overloaded, though, so we can't use that method with this technique.

The event binding technique is similar to implementing and consuming commands (a command is a property that returns an object that implements the [**ICommand**](T:System.Windows.Input.ICommand) interface). Both [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) and [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) work with commands. So that you don't have to implement the command pattern multiple times, you can use the **DelegateCommand** helper class that you'll find in the [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame) sample (in the "Common" folder).

## Binding to a collection of folders or files

You can use the APIs in the [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346) namespace to retrieve folder and file data. However, the various **GetFilesAsync**, **GetFoldersAsync**, and **GetItemsAsync** methods do not return values that are suitable for binding to list controls. Instead, you must bind to the return values of the [**GetVirtualizedFilesVector**](https://msdn.microsoft.com/library/windows/apps/Hh701422), [**GetVirtualizedFoldersVector**](https://msdn.microsoft.com/library/windows/apps/Hh701428), and [**GetVirtualizedItemsVector**](https://msdn.microsoft.com/library/windows/apps/Hh701430) methods of the [**FileInformationFactory**](https://msdn.microsoft.com/library/windows/apps/BR207501) class. The following code example from the [StorageDataSource and GetVirtualizedFilesVector sample](http://go.microsoft.com/fwlink/p/?linkid=228621) shows the typical usage pattern. Remember to declare the **picturesLibrary** capability in your app package manifest, and confirm that there are pictures in your Pictures library folder.

``` csharp
        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var library = Windows.Storage.KnownFolders.PicturesLibrary;
            var queryOptions = new Windows.Storage.Search.QueryOptions();
            queryOptions.FolderDepth = Windows.Storage.Search.FolderDepth.Deep;
            queryOptions.IndexerOption = Windows.Storage.Search.IndexerOption.UseIndexerWhenAvailable;

            var fileQuery = library.CreateFileQueryWithOptions(queryOptions);

            var fif = new Windows.Storage.BulkAccess.FileInformationFactory(
                fileQuery,
                Windows.Storage.FileProperties.ThumbnailMode.PicturesView,
                190,
                Windows.Storage.FileProperties.ThumbnailOptions.UseCurrentScale,
                false
                );

            var dataSource = fif.GetVirtualizedFilesVector();
            this.PicturesListView.ItemsSource = dataSource;
        }
```

Você normalmente usará essa abordagem para criar um modo de exibição somente leitura das informações de arquivos e de pastas. Você pode criar associações bidirecionais às propriedades de arquivo e pasta para, por exemplo, permitir que os usuários classifiquem uma música em um modo de exibição de músicas. Porém, as alterações não serão persistidas até que você chame o método **SavePropertiesAsync** apropriado (por exemplo, [**MusicProperties.SavePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/BR207760)). Você deve confirmar as alterações quando o item perde o foco porque isso ativa uma redefinição de seleção.

Observe que a associação bidirecional que usa essa técnica funciona apenas em locais indexados, por exemplo, em Música. Você pode determinar se um local é indexado chamando o método [**FolderInformation.GetIndexedStateAsync**](https://msdn.microsoft.com/library/windows/apps/BR207627).

Observe também que um vetor virtualizado pode retornar **null** para alguns itens antes de ele preencher seu valor. Por exemplo, você deve verificar se há **null** antes de usar o valor [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770) de um limite de controle de lista para um vetor virtualizado ou use [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/BR209768).

Associando a dados agrupados por uma chave
---

Se você usar um conjunto simples de itens (livros, por exemplo, representados por uma classe **BookSku**) e agrupar os itens usando uma propriedade comum, como uma chave (a propriedade **BookSku.AuthorName**, por exemplo), o resultado será chamado de dados agrupados. Quando você agrupa os dados, não é mais uma coleção simples. Dados agrupados são uma coleção de objetos de grupo, onde cada objeto de grupo tem a) uma chave e b) uma coleção de itens cuja propriedade corresponde a essa chave. Para aproveitar o exemplo de livros novamente, o resultado de agrupar os livros pelo nome do autor resulta em uma coleção de grupos de nome do autor onde cada grupo tem a) uma chave, que é um nome do autor, e b) uma coleção de **BookSku**s cuja propriedade **AuthorName** corresponde à chave do grupo.

Em geral, para exibir uma coleção, você associa [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/BR242828) de um controle de itens (como [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) ou [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705)) diretamente a uma propriedade que retorna uma coleção. Se for uma coleção simples de itens, você não precisa fazer nada especial. Mas se for uma coleção de objetos de grupo (como ao associar a dados agrupados), você precisa dos serviços de um objeto intermediário chamado [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) que fica entre o controle de itens e a origem da associação. Você associa **CollectionViewSource** à propriedade que retorna dados agrupados, e associa o controle de itens a **CollectionViewSource**. Uma vantagem extra de um **CollectionViewSource** é que ele acompanha o item atual e, assim, você pode manter mais de um controle de itens em sincronia, associando todos ao mesmo **CollectionViewSource**. Você também pode acessar o item atual de forma programática usando a propriedade [**ICollectionView.CurrentItem**](https://msdn.microsoft.com/library/windows/apps/BR209857) do objeto retornado pela propriedade [**CollectionViewSource.View**](https://msdn.microsoft.com/library/windows/apps/BR209833-view).

Para ativar o recurso de agrupamento de um [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833), defina [**IsSourceGrouped**](https://msdn.microsoft.com/library/windows/apps/BR209833-issourcegrouped) como **true**. Se você também precisa definir a propriedade [**ItemsPath**](https://msdn.microsoft.com/library/windows/apps/BR209833-itemspath) depende exatamente de como você cria seus objetos de grupo. Há duas maneiras de criar um objeto de grupo: o padrão "é-um-grupo" e o padrão "tem-um-grupo". No padrão "é-um-grupo", o objeto de grupo é derivado de um tipo de coleção (por exemplo, **List&lt;T&gt;**), portanto, o objeto de grupo, na verdade, é o grupo de itens. Com esse padrão, você não precisa definir **ItemsPath**. No padrão "tem-um-grupo", o objeto de grupo tem uma ou mais propriedades de um tipo de coleção (como **List&lt;T&gt;**), portanto, o grupo "tem um" grupo de itens na forma de uma propriedade (ou vários grupos de itens na forma de várias propriedades). Com esse padrão, é necessário definir **ItemsPath** como o nome da propriedade que contém o grupo de itens.

O exemplo a seguir ilustra o padrão "tem-um-grupo". A classe page tem uma propriedade chamada [**ViewModel**](https://msdn.microsoft.com/library/windows/apps/BR208713), que retorna uma instância de nosso modelo de exibição. O [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) associa à propriedade **Authors** do modelo de exibição (**Authors** é a coleção de objetos de grupo) e também especifica que é a propriedade **Author.BookSkus** que contém os itens agrupados. Por fim, o [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) está associado a **CollectionViewSource**, e tem seu estilo de grupo definido para que possa renderizar os itens em grupos.

``` csharp
    <Page.Resources>
        <CollectionViewSource
        x:Name="AuthorHasACollectionOfBookSku"
        Source="{x:Bind ViewModel.Authors}"
        IsSourceGrouped="true"
        ItemsPath="BookSkus"/>
    </Page.Resources>
    ...

    <GridView
    ItemsSource="{Binding Source={StaticResource AuthorHasACollectionOfBookSku}}" ...>
        <GridView.GroupStyle>
            <GroupStyle
                HeaderTemplate="{StaticResource AuthorGroupHeaderTemplateWide}" ... />
        </GridView.GroupStyle>
    </GridView>
```

Note that the [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/BR242828) must use [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) (and not [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)) because it needs to set the **Source** property to a resource. To see the above example in the context of the complete app, download the [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) sample app. Unlike the markup shown above, [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) uses {Binding} exclusively.

You can implement the "is-a-group" pattern in one of two ways. One way is to author your own group class. Derive the class from **List&lt;T&gt;** (where *T* is the type of the items). For example, `public class Author : List<BookSku>`. A segunda maneira é usar uma expressão [LINQ](http://msdn.microsoft.com/library/bb397926.aspx) para criar dinamicamente objetos de grupo (e uma classe de grupo) de valores de propriedade dos itens **BookSku**. Essa abordagem (manter somente uma lista simples de itens e agrupá-los em tempo real) é típica de um aplicativo que acessa dados de um serviço de nuvem. Você tem a flexibilidade para agrupar livros por autor ou por gênero (por exemplo) sem a necessidade de classes de grupos especiais, como **Author** e **Genre**.

O exemplo a seguir ilustra o padrão "é-um-grupo" usando [LINQ](http://msdn.microsoft.com/library/bb397926.aspx). Desta vez, agrupamos livros por gênero, exibido com o nome do gênero nos cabeçalhos de grupo. Isso é indicado pelo caminho de propriedade "Key" na referência ao valor [**Key**](P:System.Linq.IGrouping%602.Key) do grupo.

``` csharp
    using System.Linq;

    ...

    private IOrderedEnumerable<IGrouping<string, BookSku>> genres;

    public IOrderedEnumerable<IGrouping<string, BookSku>> Genres
    {
        get
        {
            if (this.genres == null)
            {
                this.genres = from book in this.bookSkus
                group book by book.genre into grp
                orderby grp.Key select grp;
            }
            return this.genres;
        }
    }
```

Remember that when using [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) with data templates we need to indicate the type being bound to by setting an **x:DataType** value. If the type is generic then we can't express that in markup so we need to use [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) instead in the group style header template.

``` xml
    <Grid.Resources>
        <CollectionViewSource x:Name="GenreIsACollectionOfBookSku"
        Source="{Binding Genres}"
        IsSourceGrouped="true"/>
    </Grid.Resources>
    <GridView ItemsSource="{Binding Source={StaticResource GenreIsACollectionOfBookSku}}">
        <GridView.ItemTemplate x:DataType="local:BookTemplate">
            <DataTemplate>
                <TextBlock Text="{x:Bind Title}"/>
            </DataTemplate>
        </GridView.ItemTemplate>
        <GridView.GroupStyle>
            <GroupStyle>
                <GroupStyle.HeaderTemplate>
                    <DataTemplate>
                        <TextBlock Text="{Binding Key}"/>
                    </DataTemplate>
                </GroupStyle.HeaderTemplate>
            </GroupStyle>
        </GridView.GroupStyle>
    </GridView>
```

A [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/Hh702601) control is a great way for your users to view and navigate grouped data. The [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) sample app illustrates how to use the **SemanticZoom**. In that app, you can view a list of books grouped by author (the zoomed-in view) or you can zoom out to see a jump list of authors (the zoomed-out view). The jump list affords much quicker navigation than scrolling through the list of books. The zoomed-in and zoomed-out views are actually **ListView** or **GridView** controls bound to the same **CollectionViewSource**.

![An illustration of a SemanticZoom](images/sezo.png)

When you bind to hierarchical data—such as subcategories within categories—you can choose to display the hierarchical levels in your UI with a series of items controls. A selection in one items control determines the contents of subsequent items controls. You can keep the lists synchronized by binding each list to its own [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) and binding the **CollectionViewSource** instances together in a chain. This is called a master/details (or list/details) view. For more info, see [How to bind to hierarchical data and create a master/details view](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md).

Diagnosing and debugging data binding problems
-----------------------------------------------

Your binding markup contains the names of properties (and, for C#, sometimes fields and methods). So when you rename a property, you'll also need to change any binding that references it. Forgetting to do that leads to a typical example of a data binding bug, and your app either won't compile or won't run correctly.

The binding objects created by [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) and [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) are largely functionally equivalent. But {x:Bind} has type information for the binding source, and it generates source code at compile-time. With {x:Bind} you get the same kind of problem detection that you get with the rest of your code. That includes compile-time validation of your binding expressions, and debugging by setting breakpoints in the source code generated as the partial class for your page. These classes can be found in the files in your `obj` folder, with names like (for C#) `<view name>.g.cs`). Se você tiver um problema com uma associação, ative **Interromper Exceções sem Tratamento** no depurador do Microsoft Visual Studio. O depurador interromperá a execução nesse ponto e, em seguida, você pode depurar o que deu errado. O código gerado por {x:Bind} segue o mesmo padrão para cada parte do gráfico de nós de origem de associação, e você pode usar as informações da janela **Pilha de Chamadas** para ajudar a determinar a sequência de chamadas que levaram ao problema.

[{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) não tem informações de tipo para a origem da associação. Mas, quando você executa o aplicativo com o depurador anexado, os possíveis erros de associação aparecem na janela **Saída** do Visual Studio.

Criando associações no código
-------------------------

**Observação**  Esta seção só se aplica a [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782), pois você não pode criar associações [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) no código. No entanto, alguns dos mesmos benefícios de {x:Bind} podem ser obtidos com [**DependencyProperty.RegisterPropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/BR242356-registerpropertychangedcallback), o que permite que você se registre para receber notificações de alteração em qualquer propriedade de dependência.

Você também pode conectar elementos da interface de usuário aos dados usando código de procedimentos em vez de XAML. Para fazer isso, crie um novo objeto [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820), defina as propriedades apropriadas, depois chame [**FrameworkElement.SetBinding**](https://msdn.microsoft.com/library/windows/apps/BR208706-setbinding) ou [**BindingOperations.SetBinding**](https://msdn.microsoft.com/library/windows/apps/BR209820operations-setbinding). A criação de associações de forma programática é útil quando você deseja escolher os valores da propriedade de associação no tempo de execução ou compartilhar uma única associação entre vários controles. Observe, porém, que você não pode alterar os valores da propriedade de associação depois que chamar **SetBinding**.

O exemplo a seguir mostra como implementar uma associação em código.

``` xml
<TextBox x:Name="MyTextBox" Text="Text"/>
```

```csharp
// Create an instance of the MyColors class 
// that implements INotifyPropertyChanged.
MyColors textcolor = new MyColors();

// Brush1 is set to be a SolidColorBrush with the value Red.
textcolor.Brush1 = new SolidColorBrush(Colors.Red);

// Set the DataContext of the TextBox MyTextBox.
MyTextBox.DataContext = textcolor;

// Create the binding and associate it with the text box.
Binding binding = new Binding() { Path = new PropertyPath("Brush1") };
MyTextBox.SetBinding(TextBox.ForegroundProperty, binding);
```

``` vbnet
' Create an instance of the MyColors class 
' that implements INotifyPropertyChanged. 
Dim textcolor As New MyColors()

' Brush1 is set to be a SolidColorBrush with the value Red. 
textcolor.Brush1 = New SolidColorBrush(Colors.Red)

' Set the DataContext of the TextBox MyTextBox. 
MyTextBox.DataContext = textcolor

' Create the binding and associate it with the text box.
Dim binding As New Binding() With {.Path = New PropertyPath("Brush1")}
MyTextBox.SetBinding(TextBox.ForegroundProperty, binding)
```

Comparação de recursos de {x:Bind} e {Binding}
------------------------------------------

| Recurso | {x:Bind} | {Binding} | Observações |
|---------|----------|-----------|-------|
| Path é a propriedade padrão | `{x:Bind a.b.c}` | `{Binding a.b.c}` | | 
| Propriedade Path | `{x:Bind Path=a.b.c}` | `{Binding Path=a.b.c}` | Em x:Bind, Path tem a raiz em Page por padrão, não em DataContext. | 
| Indexador | `{x:Bind Groups[2].Title}` | `{Binding Groups[2].Title}` | Associa ao item especificado na coleção. Somente indexações baseadas em números inteiros são permitidas. | 
| Propriedades anexadas | `{x:Bind Button22.(Grid.Row)}` | `{Binding Button22.(Grid.Row)}` | Propriedades anexadas são especificadas usando parênteses. Se a propriedade não for declarada em um namespace XAML, então será necessário prefixá-la com um namespace xml, que deve ser mapeado para um namespace de código no cabeçalho do documento. | 
| Transmissão | `{x:Bind groups[0].(data:SampleDataGroup.Title)}` | Não necessário < | Transmissões são especificadas usando parênteses. Se a propriedade não for declarada em um namespace XAML, então será necessário prefixá-la com um namespace xml, que deve ser mapeado para um namespace de código no cabeçalho do documento. | 
| Conversor | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}}` | Os conversores devem ser declarados na raiz de Page/ResourceDictionary, ou em App.xaml. | 
| ConverterParameter, ConverterLanguage | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | Os conversores devem ser declarados na raiz de Page/ResourceDictionary, ou em App.xaml. | 
| TargetNullValue | `{x:Bind Name, TargetNullValue=0}` | `{Binding Name, TargetNullValue=0}` | Usado quando a folha da expressão de associação é nula. Use aspas simples para um valor de string. | 
| FallbackValue | `{x:Bind Name, FallbackValue='empty'}` | `{Binding Name, FallbackValue='empty'}` | Usado quando alguma parte do caminho para a associação (exceto a folha) é nula. | 
| ElementName | `{x:Bind slider1.Value}` | `{Binding Value, ElementName=slider1}` | Com {x:Bind}, você está associando a um campo; Caminho fica na raiz na página por padrão e, assim, qualquer elemento nomeado pode ser acessado por meio de seu campo. | 
| RelativeSource: Self | `<Rectangle x:Name="rect1" Width="200" Height="{x:Bind rect1.Width}" ... />` | `<Rectangle Width="200" Height="{Binding Width, RelativeSource={RelativeSource Self}}" ... />` | With {x:Bind}, name the element and use its name in Path. | 
| RelativeSource: TemplatedParent | Not supported | `{Binding <path>, RelativeSource={RelativeSource TemplatedParent}}` | Associação de modelo regular pode ser usada nos modelos de controle para a maioria dos usos. Mas use TemplatedParent onde você precisa usar um conversor ou uma associação bidirecional.< | 
| Fonte | Sem suporte | `<ListView ItemsSource="{Binding Orders, Source={StaticResource MyData}}"/>` | For {x:Bind} use a property or a static path instead. | 
| Mode | `{x:Bind Name, Mode=OneWay}` | `{Binding Name, Mode=TwoWay}` | Mode can be OneTime, OneWay, or TwoWay. {x:Bind} defaults to OneTime; {Binding} defaults to OneWay. | 
| UpdateSourceTrigger | Not supported | `<Binding UpdateSourceTrigger="[Default | PropertyChanged | Explicit]"/>` | {x:Bind} usa o comportamento PropertyChanged para todos os casos, exceto TextBox onde aguarda perder o foco para atualizar a origem. | 




<!--HONumber=Mar16_HO1-->


