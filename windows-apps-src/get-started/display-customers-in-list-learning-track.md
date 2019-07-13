---
title: Acompanhamento de aprendizado – Exibir clientes em uma lista
description: Saiba o que você precisa fazer para exibir uma coleção de objetos de cliente em uma lista.
ms.date: 05/07/2018
ms.topic: article
keywords: introdução, uwp, windows 10, acompanhamento de aprendizado, associação de dados, lista
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: c4d339a1eedb798f11d5567be6a48ec2269cf8ac
ms.sourcegitcommit: 280193dfe5a106fc6b4c85df3ac40535547b855c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67235162"
---
# <a name="display-customers-in-a-list"></a>Exibir clientes em uma lista

A exibição e a manipulação de dados reais na interface do usuário é essencial para a funcionalidade de muitos aplicativos. Este artigo mostrará o que você precisa saber para exibir uma coleção de objetos de cliente em uma lista.

Isso não é um tutorial. Caso precise de um, veja nosso [tutorial de associação de dados](../data-binding/xaml-basics-data-binding.md), que fornecerá uma experiência guiada passo a passo.

Vamos começar com uma rápida apresentação da associação de dados: o que é e como funciona. Em seguida, vamos adicionar um **ListView** à interface do usuário, adicionar a associação de dados e personalizar a associação de dados com recursos adicionais.

## <a name="what-do-you-need-to-know"></a>O que você precisa saber?

A associação de dados é uma maneira de exibir dados de um aplicativo na interface do usuário. Isso permite a *separação de questões* em seu aplicativo, mantendo a interface do usuário separada do outro código. Isso cria um modelo conceitual limpo mais fácil de ler e manter.

Cada associação de dados tem duas partes:

* Uma fonte que fornece os dados a serem associados.
* Um destino na interface do usuário onde os dados são exibidos.

Para implementar uma associação de dados, é necessário adicionar código à fonte que fornece dados para a associação. Você também precisará adicionar uma ou duas extensões de marcação ao seu XAML para especificar as propriedades da fonte de dados. Veja a seguir a diferença chave entre as duas:

* [**x:Bind**](../xaml-platform/x-bind-markup-extension.md) é fortemente tipada e gera código no tempo de compilação para melhorar o desempenho. x:Bind tem como padrão uma associação ocasional, que otimiza a exibição rápida de dados somente leitura que não mudam.
* [**Binding**](../xaml-platform/binding-markup-extension.md) é fracamente tipada e montada no tempo de execução. Isso resulta em um desempenho inferior em comparação com x:Bind. Em quase todos os casos, você deve usar x:Bind em vez de Binding. No entanto, é provável que você a encontre em código mais antigo. Binding é padronizada para transferência de dados unidirecional, a qual é otimizada para dados somente leitura que podem mudar na origem.

É recomendável usar **x:Bind** sempre que possível e mostraremos isso em trechos deste artigo. Para saber mais sobre as diferenças, consulte a [Comparação entre os recursos {x:Bind} e {Binding}](../data-binding/data-binding-in-depth.md#xbind-and-binding-feature-comparison).

## <a name="create-a-data-source"></a>Criar uma fonte de dados

Primeiro, você precisará de uma classe para representar os dados do cliente. Para que você tenha um ponto de referência, mostraremos o processo neste exemplo básico:

```csharp
public class Customer
{
    public string Name { get; set; }
}
```

## <a name="create-a-list"></a>Criar uma lista

Para poder exibir clientes, você precisa criar a lista para armazená-los. O [ListView](../design/controls-and-patterns/listview-and-gridview.md) é um controle XAML básico ideal para essa tarefa. No momento, o ListView requer uma posição na página e em breve exigirá um valor para a propriedade **ItemSource**.

```xaml
<ListView ItemsSource=""
    HorizontalAlignment="Center"
    VerticalAlignment="Center"/>
```

Depois de associar os dados ao ListView, incentivamos você a retornar à documentação e experimentar a personalização de sua aparência e seu layout para melhor atender às suas necessidades.

## <a name="bind-data-to-your-list"></a>Associar dados à sua lista

Agora que você criou uma interface do usuário básica para manter suas associações, é necessário configurar a fonte para fornecê-las. Veja a seguir um exemplo de como isso pode ser feito:

```csharp
public sealed partial class MainPage : Page
{
    public ObservableCollection<Customer> Customers { get; }
        = new ObservableCollection<Customer>();

    public MainPage()
    {
        this.InitializeComponent();
          // Add some customers
        this.Customers.Add(new Customer() { Name = "NAME1"});
        this.Customers.Add(new Customer() { Name = "NAME2"});
        this.Customers.Add(new Customer() { Name = "NAME3"});
    }
}
```
```xaml
<ListView ItemsSource="{x:Bind Customers}"
    HorizontalAlignment="Center"
    VerticalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Customer">
            <TextBlock Text="{x:Bind Name}"/>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

A [Visão geral da Associação de Dados](../data-binding/data-binding-quickstart.md#binding-to-a-collection-of-items) guia você por um problema semelhante na seção sobre associação a uma coleção de itens. Nosso exemplo aqui mostra as seguintes etapas cruciais:

* No code-behind da sua interface do usuário, crie uma propriedade do tipo **ObservableCollection<T>** para manter os objetos de cliente.
* Associe **ItemSource** de ListView a essa propriedade.
* Forneça um **ItemTemplate** básico para ListView, que vai configurar como cada item na lista é exibido.

Fique à vontade para consultar a documentação do [ListView](../design/controls-and-patterns/listview-and-gridview.md) se quiser personalizar o layout, adicionar uma seleção de itens ou ajustar o **DataTemplate** recém-criado. Mas e se você desejar editar seus clientes?

## <a name="edit-your-customers-through-the-ui"></a>Editar clientes pela interface do usuário

Você exibiu clientes em uma lista, mas a associação de dados permite que você faça mais. E se fosse possível editar os dados diretamente na interface do usuário? Para fazer isso, primeiramente vamos falar sobre os três modos de associação de dados:

* *Ocasional*: essa associação de dados é ativada apenas uma vez e não reage às mudanças.
* *Unidirecional*: essa associação de dados atualizará a interface do usuário com todas as alterações feitas na fonte de dados.
* *Bidirecional*: essa associação de dados atualizará a interface do usuário com todas as alterações feitas na fonte de dados e também atualizará os dados com todas as alterações feitas na interface do usuário.

Se você seguiu os trechos de código anteriores, a associação feitas usa x:Bind e não especifica um modo, transformando-a em uma associação Ocasional. Se você desejar editar seus clientes diretamente na interface do usuário, será necessário alterá-la para uma associação Bidirecional para que as alterações dos dados sejam passadas para os objetos de cliente. Para saber mais, consulte [Associação de dados em detalhes](../data-binding/data-binding-in-depth.md).

A associação bidirecional também atualizará a interface do usuário se a fonte de dados for alterada. Para que isso funcione, você deve implementar [**INotifyPropertyChanged**](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?redirectedfrom=MSDN) na origem e verificar se os setters de propriedade emitem o evento **PropertyChanged**. É prática comum que eles chamem um método auxiliar como o **OnPropertyChanged**, conforme mostrado abaixo:

```csharp
public class Customer : INotifyPropertyChanged
{
    private string _name;
    public string Name
    {
        get => _name;
        set
        {
            if (_name != value)
                {
                    _name = value;
                    this.OnPropertyChanged();
                }
        }
    }
    public event PropertyChangedEventHandler PropertyChanged;

    public void OnPropertyChanged([CallerMemberName] string propertyName = null) =>
        this.PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
}
```
Em seguida, transforme o texto de ListView em editável usando um **TextBox** em vez de um **TextBlock** e certifique-se de definir o **Modo** em suas associações de dados como **Bidirecional**.

```xaml
<ListView ItemsSource="{x:Bind Customers}"
    HorizontalAlignment="Center"
    VerticalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Customer">
            <TextBox Text="{x:Bind Name, Mode=TwoWay}"/>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Uma maneira rápida de garantir que isso funcione é adicionar um segundo ListView com os controles TextBox e as associações Unidirecionais. Os valores na segunda lista serão alterados automaticamente enquanto você edita a primeira.

> [!NOTE]
> A edição diretamente dentro de um ListView é uma maneira simples de mostrar a associação bidirecional em ação, mas pode resultar em complicações de usabilidade. Se quiser que seu aplicativo vá mais longe, considere o uso de [outros controles XAML](../design/controls-and-patterns/controls-and-events-intro.md) para editar seus dados e manter o ListView como somente para exibição.

## <a name="going-further"></a>Aprofundamento

Agora que você já criou uma lista de clientes com a associação bidirecional, fique à vontade para consultar os documentos enviados e experimentar. Você também pode conferir o [tutorial de associação de dados](../data-binding/xaml-basics-data-binding.md) se desejar um passo a passo de associações básicas e avançadas ou investigar os controles, como o [padrão mestre/detalhes](../design/controls-and-patterns/master-details.md), para criar uma interface do usuário mais robusta.

## <a name="useful-apis-and-docs"></a>APIs e documentos úteis

Veja a seguir um resumo rápido de APIs e outras documentações úteis que ajudarão você a começar a trabalhar com a Associação de Dados.

### <a name="useful-apis"></a>APIs úteis

| API | Descrição |
|------|---------------|
| [Modelo de dados](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate) | Descreve a estrutura visual de um objeto de dados, permitindo a exibição de elementos específicos da interface do usuário. |
| [x:Bind](../xaml-platform/x-bind-markup-extension.md) | Documentação sobre a extensão de marcação x:Bind recomendada. |
| [Binding](../xaml-platform/binding-markup-extension.md) | Documentação sobre a extensão de marcação de Associação antiga. |
| [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) | Um controle da interface do usuário que exibe itens de dados em uma pilha vertical. |
| [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) | Um controle de texto básico para exibir os dados de texto editáveis na interface do usuário. |
| [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?redirectedfrom=MSDN) | A interface para tornar dados observáveis, fornecendo-os a uma associação de dados. |
| [ItemsControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) | A propriedade **ItemsSource** dessa classe permite que um ListView associe-se a uma fonte de dados. |

### <a name="useful-docs"></a>Documentos úteis

| Tópico | Descrição |
|-------|----------------|
| [Vinculação de dados em detalhes](../data-binding/data-binding-in-depth.md) | Uma visão geral básica dos princípios da associação de dados |
| [Visão geral da Associação de Dados](../data-binding/data-binding-quickstart.md) | Informações conceituais detalhadas sobre a associação de dados. |
| [Exibição de Lista](../design/controls-and-patterns/listview-and-gridview.md) | Informações sobre como criar e configurar um ListView, incluindo a implementação de um **DataTemplate** |

## <a name="useful-code-samples"></a>Exemplos de código úteis

| Exemplo de código | Descrição |
|-----------------|---------------|
| [Tutorial de associação de dados](../data-binding/xaml-basics-data-binding.md) | Uma experiência guiada passo a passo das noções básicas da associação de dados. |
| [ListView e GridView](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView) | Explore ListViews mais elaboradas com associação de dados. |
| [QuizGame](https://github.com/Microsoft/Windows-appsample-networkhelper) | Consulte a associação de dados em ação, incluindo a classe **BindableBase** (na pasta "Common") para uma implementação padrão de **INotifyPropertyChanged**. |
