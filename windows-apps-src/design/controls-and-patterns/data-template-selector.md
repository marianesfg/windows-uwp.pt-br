---
Description: Use seletores de modelo de dados para personalizar os estilos de seus itens com base nas propriedades do item.
title: Seleção de modelo de dados
label: Data template selection
template: detail.hbs
ms.date: 10/18/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
ms.openlocfilehash: 956ac13dcdc1a2e6367e590bb8885c8722f41e2c
ms.sourcegitcommit: cb7f80100c99d4b6466a819bea191006ec3d616c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73640873"
---
# <a name="data-template-selection-styling-items-based-on-their-properties"></a>Seleção de modelo de dados: Estilizando itens com base em suas propriedades

O design personalizado de controles de coleções é gerenciado por um [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate). Modelos de dados definem como cada item deve ser disposto e estilizado e que a marcação é aplicada a todos os itens na coleção. Este artigo explica como usar um [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) para aplicar modelos de dados diferentes em uma coleção e selecionar qual modelo de dados usar, com base em determinadas propriedades de item ou valores de sua escolha.

> **APIs importantes**: [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector), [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate)

[DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) é uma classe que habilita a lógica de seleção de modelo personalizado. Ela permite que você defina regras que especificam qual modelo de dados usar para determinados itens em uma coleção. Para implementar essa lógica, você cria uma subclasse de DataTemplateSelector no code-behind e define a lógica que determina qual modelo de dados usar para qual categoria de itens (por exemplo, itens de um determinado tipo ou itens com um determinado valor da propriedade etc.). Você declara uma instância dessa classe na seção de recursos do seu arquivo XAML, juntamente com as definições dos modelos de dados que você usará. Você identifica esses recursos com um valor `x:Key`, que permite fazer referência a eles em seu XAML.

## <a name="prerequisites"></a>Pré-requisitos

- Como [usar e criar um controle de coleção, como ListView ou GridView.](listview-and-gridview.md)
- Como [personalizar a aparência dos itens usando um DataTemplate.](item-containers-templates.md#data-template)

## <a name="when-not-to-use-a-datatemplateselector"></a>Quando não usar um DataTemplateSelector

Em geral, você não deve dar a cada item em um ListView ou GridView um layout/estilo completamente diferente – isso seria um uso insatisfatório de um DataTemplateSelector e impacta negativamente o desempenho.

Determinados elementos da exibição visual de um item de lista podem ser controlados usando apenas um modelo de dados, por meio da associação de determinadas propriedades. Por exemplo, os itens podem ter ícones diferentes associando-se a uma propriedade de origem de ícone no modelo de dados e dando a cada item um valor diferente para essa propriedade de origem de ícone. Isso alcançaria um desempenho melhor do que usar um DataTemplateSelector.

## <a name="when-to-use-a-datatemplateselector"></a>Quando usar um DataTemplateSelector

Você deve criar um [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) quando desejar usar vários modelos de dados em um controle de coleção. Um `DataTemplateSelector` dá a você a flexibilidade de fazer com que determinados itens se destaquem, enquanto ainda mantém itens em um layout semelhante. Há muitos casos de uso em que um DataTemplateSelector é útil e alguns cenários em que é melhor lembrar novamente o controle e a estratégia que você está usando.

Os controles de coleção normalmente são associados a uma coleção de itens que são todos de um tipo. No entanto, embora os itens possam ser do mesmo tipo, eles podem ter valores diferentes para determinadas propriedades ou representar significados diferentes. Determinados itens também podem ser mais importantes do que outros, ou um item pode ser particularmente importante ou diferente e tem a necessidade de destacar visualmente. Nessas situações, um DataTemplateSelector será muito útil.

Também é possível associar a uma coleção que contém diferentes tipos de itens – a coleção associada pode ter uma mistura de cadeias de caracteres, ints, objetos de classe personalizada e muito mais. Isso torna o DataTemplateSelector especialmente útil, pois pode atribuir modelos de dados diferentes com base no tipo de objeto de um item.

Aqui estão alguns exemplos de quando você pode usar um seletor de modelo de dados:

- Representando diferentes níveis de funcionários em um ListView – cada tipo/nível de funcionário pode precisar de uma tela de fundo de cor diferente para ser facilmente diferenciado.
- Representando itens de venda em uma galeria de produtos que usa um GridView – um item de venda pode precisar de uma tela de fundo vermelha ou uma fonte de cores diferente para destacá-lo de itens com preço regular.
- Representando vencedores/principais fotos em uma galeria de fotos usando o FlipView.
- A necessidade de representar números negativos/positivos em um ListView de maneira diferente ou cadeias de caracteres curtas/longas.

## <a name="create-a-datatemplateselector"></a>Criar um DataTemplateSelector

Ao criar um seletor de modelo de dados, você define a lógica de seleção de modelo em seu código e define os modelos de dados em seu XAML.

### <a name="code-behind-component"></a>Componente code-behind

Para usar um seletor de modelo de dados, primeiro, crie uma subclasse de [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) (uma classe que deriva dele) no code-behind. Em sua classe, declare cada modelo como uma propriedade da classe. Em seguida, substitua o método [SelectTemplateCore](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore) para incluir sua própria lógica de seleção de modelo.

Veja um exemplo de uma subclasse `DataTemplateSelector` simples chamada `MyDataTemplateSelector`.

```csharp
public class MyDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate Normal { get; set; }
    public DataTemplate Accent { get; set; }

    protected override DataTemplate SelectTemplateCore(object item)
    {
        if ((int)item % 2 == 0)
        {
            return Normal;
        }
        else
        {
            return Accent;
        }
    }
}
```

A classe `MyDataTemplateSelector` deriva da classe `DataTemplateSelector` e define primeiro dois objetos [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate): `Normal` e `Accent`. Essas são declarações vazias por enquanto, mas serão "preenchidas" com marcação no arquivo XAML.

O método `SelectTemplateCore` usa um objeto de item (ou seja, cada item da coleção) e é substituído por regras que especificam em que circunstâncias cada `DataTemplate` deve ser retornado. Nesse caso, se o item for um número ímpar, ele receberá o modelo de dados `Accent`. Se for um número par, ele receberá o modelo de dados `Normal`.

### <a name="xaml-component"></a>Componente XAML

Em segundo lugar, você precisa criar uma instância dessa nova classe `MyDataTemplateSelector` na seção de recursos do seu arquivo XAML. Todos os recursos exigem uma `x:Key`, que você usa para fazer a associação à propriedade `ItemTemplateSelector` do seu controle de coleção (em uma etapa posterior). Você também cria duas instâncias de objetos `DataTemplate` e define layout delas na seção de recursos. Atribua esses modelos de dados às propriedades `Accent` e `Normal` declaradas na classe `MyDataTemplateSelector`.

Veja um exemplo dos recursos XAML e a marcação necessária:

```xaml
<Page.Resources>

<DataTemplate x:Key="NormalItemTemplate" x:DataType="x:Int32">
    <Button HorizontalAlignment="Stretch" VerticalAlignment="Stretch" Background="{ThemeResource SystemChromeLowColor}">
        <TextBlock Text="{x:Bind}" />
    </Button>
</DataTemplate>

<DataTemplate x:Key="AccentItemTemplate" x:DataType="x:Int32">
    <Button HorizontalAlignment="Stretch" VerticalAlignment="Stretch" Background="{ThemeResource SystemAccentColor}">
        <TextBlock Text="{x:Bind}" />
    </Button>
</DataTemplate>

<l:MyDataTemplateSelector x:Key="MyDataTemplateSelector"
    Normal="{StaticResource NormalItemTemplate}"
    Accent="{StaticResource AccentItemTemplate}"/>

</Page.Resources>
```

Como você pode ver acima, os dois modelos de dados `Normal` e `Accent` são definidos – eles exibem itens como botões; no entanto, o modelo de dados `Accent` usa um pincel de cor de destaque para a tela de fundo, enquanto o modelo de dados `Normal` usa um pincel de cor cinza (`SystemChromeLowColor`). Esses dois modelos de dados são então atribuídos aos objetos DataTemplate `Normal` e `Accent` que são atributos da classe MyDataTemplateSelector, criados no code-behind de C#.

A última etapa é associar seu `DataTemplateSelector` à propriedade `ItemTemplateSelector` do seu controle de coleções (nesse caso, um ListView). Isso substitui a necessidade de atribuir um valor à propriedade `ItemTemplate`. 

```xaml
<ListView x:Name = "TestListView"
          ItemsSource = "{x:Bind NumbersList}"
          ItemTemplateSelector = "{StaticResource MyDataTemplateSelector}">
</ListView>
```

Depois que o código for compilado, cada item da coleção será executado por meio do método `SelectTemplateCore` substituído em `MyDataTemplateSelector` e será renderizado com o DataTemplate apropriado.

> [!IMPORTANT]
> Ao usar `DataTemplateSelector` com um [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2), você associa o `DataTemplateSelector` à propriedade `ItemTemplate`. `ItemsRepeater` não tem uma propriedade `ItemTemplateSelector`.

## <a name="datatemplateselector-performance-considerations"></a>Considerações de desempenho do DataTemplateSelector

Quando você usa ListView ou GridView com uma grande coleta de dados, a rolagem e o desempenho da panorâmica podem ser uma preocupação. Para manter grandes coleções funcionando bem, há algumas etapas que você pode seguir para melhorar o desempenho de seus modelos de dados. Elas são descritas mais detalhadamente em [Otimização das interfaces do usuário ListView e GridView](/windows/uwp/debug-test-perf/optimize-gridview-and-listview).

- _Redução de elemento por item_ – Mantenha o número de elementos da interface do usuário em um modelo de dados em um mínimo razoável.
- Reciclagem de contêiner com coleções heterogêneas
  - Usar o _evento ChoosingItemContainer_ – Este evento é a maneira mais eficaz de usar diferentes modelos de dados para itens diversificados. Para obter o melhor desempenho, você deve otimizar o cache e selecionar modelos de dados para seus dados específicos.
  - Usar um _seletor de modelo de item_ – Um seletor de modelo de item (`DataTemplateSelector`) deve ser evitado em algumas instâncias devido ao impacto no desempenho.
