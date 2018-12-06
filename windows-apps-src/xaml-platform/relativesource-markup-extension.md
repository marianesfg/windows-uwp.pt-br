---
description: Fornece um meio para especificar o origem de uma associação em termos de uma relação relativa no gráfico do objeto de tempo de execução.
title: Extensão de marcação RelativeSources
ms.assetid: B87DEF36-BE1F-4C16-B32E-7A896BD09272
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 26cde97f82e6962d530721f1e0230138e5917016
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8746040"
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
| {RelativeSource Self} | Produz um valor de [<strong>Mode</strong>](https://msdn.microsoft.com/library/windows/apps/br209915) de <strong>Self</strong>. O elemento de destino deve ser usado como fonte para essa associação. Isso é útil para associar uma propriedade de um elemento a outra propriedade do mesmo elemento. |
| {RelativeSource TemplatedParent} | Produz um [<strong>ControlTemplate</strong>](https://msdn.microsoft.com/library/windows/apps/br209391) que é aplicado como a origem dessa associação. Isso é útil para aplicar informações em tempo de execução a associações em nível de modelo. | 

## <a name="remarks"></a>Comentários

Um [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) pode definir [**Binding.RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) como um atributo em um elemento de objeto **Binding** ou como um componente em uma extensão de marcação [{Binding}](binding-markup-extension.md). É por isso que são exibidas duas sintaxes XAML diferentes.

**RelativeSource** é semelhante à [extensão de marcação {Binding}](binding-markup-extension.md).  É uma extensão de marcação capaz de retornar instâncias de si, suportando uma construção baseada em cadeias de caracteres que, essencialmente, passa um argumento para o construtor. Nesse caso, o argumento passado é o valor de [**Mode**](https://msdn.microsoft.com/library/windows/apps/br209915).

O modo **Self** é útil para associar uma propriedade de um elemento a outra propriedade do mesmo elemento, e é uma variação da associação com [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828), mas não exige o nome e a autorreferência do elemento. Se você associar uma propriedade de um elemento a outra propriedade do mesmo elemento, as propriedades devem usar o mesmo tipo de propriedade, ou você também terá que usar um [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826) na associação para converter os valores. Por exemplo, é possível usar [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) como origem de [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) sem conversão, mas seria necessário um conversor para usar [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/br209419) como origem de [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br209006).

Veja um exemplo a seguir. Esse [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) usa uma [extensão de marcação {Binding}](binding-markup-extension.md) de forma que sua [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) e [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) sejam sempre iguais e ele as renderize como um quadrado. Somente a altura é definida como um valor fixo. Para esse **Rectangle**, seu [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) padrão é **null**, e não **this**. Assim, para estabelecer a origem de contexto de dados como sendo o próprio objeto (e habilitar a associação com suas outras propriedades) usamos o argumento `RelativeSource={RelativeSource Self}` no uso da extensão de marcação {Binding}.

```XML
<Rectangle
  Fill="Orange" Width="200"
  Height="{Binding RelativeSource={RelativeSource Self}, Path=Width}"
/>
```

Outro uso de `RelativeSource={RelativeSource Self}` é como uma maneira de definir um [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) do objeto como si próprio.  Por exemplo, você pode ver essa técnica em alguns exemplos do SDK onde a classe [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) foi estendida com uma propriedade personalizada que já está fornecendo um modelo de exibição pronto para sua própria vinculação de dados, como: `<common:LayoutAwarePage ... DataContext="{Binding DefaultViewModel, RelativeSource={RelativeSource Self}}">`

**Observação**o uso de XAML para **RelativeSource** mostra apenas o uso para o qual ele se destina: definir um valor para [**Binding. RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) em XAML como parte de uma expressão de associação. Teoricamente, outros usos são possíveis no caso da definição de uma propriedade na qual o valor é [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209913).

## <a name="related-topics"></a>Tópicos relacionados

* [Visão geral do XAML](xaml-overview.md)
* [Vinculação de dados em detalhes](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [Extensão de marcação {Binding}](binding-markup-extension.md)
* [**Associação**](https://msdn.microsoft.com/library/windows/apps/br209820)
* [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209913)

