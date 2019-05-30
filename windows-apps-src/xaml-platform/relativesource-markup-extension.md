---
description: Fornece um meio para especificar o origem de uma associação em termos de uma relação relativa no gráfico do objeto de tempo de execução.
title: Extensão de marcação RelativeSources
ms.assetid: B87DEF36-BE1F-4C16-B32E-7A896BD09272
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 716ca61fc9925846377157d215ca3326191915b7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371147"
---
# <a name="relativesource-markup-extension"></a>Extensão de marcação {RelativeSource}


Fornece um meio para especificar o origem de uma associação em termos de uma relação relativa no gráfico do objeto de tempo de execução.

## <a name="xaml-attribute-usage-self-mode"></a>Uso de atributos XAML (Modo por conta própria)

``` syntax
<Binding RelativeSource="{RelativeSource Self}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource Self} ...}" .../>
```

## <a name="xaml-attribute-usage-templatedparent-mode"></a>Uso de atributos XAML (Modo TemplatedParent)

``` syntax
<Binding RelativeSource="{RelativeSource TemplatedParent}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource TemplatedParent} ...}" .../>
```

## <a name="xaml-values"></a>Valores XAML

| Termo | Descrição |
|------|-------------|
| {RelativeSource Self} | Produz um valor de [<strong>Mode</strong>](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.relativesource.mode) de <strong>Self</strong>. O elemento de destino deve ser usado como fonte para essa associação. Isso é útil para associar uma propriedade de um elemento a outra propriedade do mesmo elemento. |
| {RelativeSource TemplatedParent} | Produz um [<strong>ControlTemplate</strong>](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) que é aplicado como a origem dessa associação. Isso é útil para aplicar informações em tempo de execução a associações em nível de modelo. | 

## <a name="remarks"></a>Comentários

Um [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) pode definir [**Binding.RelativeSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.relativesource) como um atributo em um elemento de objeto **Binding** ou como um componente em uma extensão de marcação [{Binding}](binding-markup-extension.md). É por isso que são exibidas duas sintaxes XAML diferentes.

**RelativeSource** é semelhante à [extensão de marcação {Binding}](binding-markup-extension.md).  É uma extensão de marcação capaz de retornar instâncias de si, suportando uma construção baseada em cadeias de caracteres que, essencialmente, passa um argumento para o construtor. Nesse caso, o argumento passado é o valor de [**Mode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.relativesource.mode).

O modo **Self** é útil para associar uma propriedade de um elemento a outra propriedade do mesmo elemento, e é uma variação da associação com [**ElementName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.elementname), mas não exige o nome e a autorreferência do elemento. Se você associar uma propriedade de um elemento a outra propriedade do mesmo elemento, as propriedades devem usar o mesmo tipo de propriedade, ou você também terá que usar um [**Converter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter) na associação para converter os valores. Por exemplo, é possível usar [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) como origem de [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) sem conversão, mas seria necessário um conversor para usar [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.isenabled) como origem de [**Visibility**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Visibility).

Aqui está um exemplo. Esse [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) usa uma [extensão de marcação {Binding}](binding-markup-extension.md) de forma que sua [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) e [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) sejam sempre iguais e ele as renderize como um quadrado. Somente a altura é definida como um valor fixo. Para esse **Rectangle**, seu [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) padrão é **null**, e não **this**. Assim, para estabelecer a origem de contexto de dados como sendo o próprio objeto (e habilitar a associação com suas outras propriedades) usamos o argumento `RelativeSource={RelativeSource Self}` no uso da extensão de marcação {Binding}.

```XML
<Rectangle
  Fill="Orange" Width="200"
  Height="{Binding RelativeSource={RelativeSource Self}, Path=Width}"
/>
```

Outro uso de `RelativeSource={RelativeSource Self}` é como uma maneira de definir um [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) do objeto como si próprio.  Por exemplo, você poderá ver essa técnica em alguns dos exemplos do SDK no qual o [ **página** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) classe foi estendida com uma propriedade personalizada que já está fornecendo um modelo de exibição pronto para ir para sua própria associação de dados como: `<common:LayoutAwarePage ... DataContext="{Binding DefaultViewModel, RelativeSource={RelativeSource Self}}">`

**Observação**  uso o XAML para **RelativeSource** mostra apenas o uso para o qual ele destina-se: definindo um valor para [ **Binding.RelativeSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.relativesource)em XAML como parte de uma expressão de associação. Teoricamente, outros usos são possíveis no caso da definição de uma propriedade na qual o valor é [**RelativeSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.RelativeSource).

## <a name="related-topics"></a>Tópicos relacionados

* [Visão geral do XAML](xaml-overview.md)
* [Vinculação de dados em detalhes](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)
* [Extensão de marcação {binding}](binding-markup-extension.md)
* [**Associação**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding)
* [**RelativeSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.RelativeSource)

