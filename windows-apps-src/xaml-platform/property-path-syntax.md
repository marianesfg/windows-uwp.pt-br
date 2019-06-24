---
description: Você pode usar a classe PropertyPath e a sintaxe de cadeia de caracteres para instanciar um valor PropertyPath em XAML ou em código.
title: Sintaxe de Property-path
ms.assetid: FF3ECF47-D81F-46E3-BE01-C839E0398025
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 57532c45bdf6c2b8feb2af1277be74a0f8b2c759
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320297"
---
# <a name="property-path-syntax"></a>Sintaxe de Property-path


Você pode usar a classe [**PropertyPath**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyPath) e a sintaxe de cadeia de caracteres para instanciar um valor **PropertyPath** vem XAML ou em código. Valores **PropertyPath** são usados por vinculação de dados. Uma sintaxe semelhante é usada para direcionar animações em storyboard. Nos dois cenários, um caminho de propriedade descreve uma passagem de uma ou mais relações objeto-propriedade que eventualmente são resolvidas para uma única propriedade.

Você pode definir uma cadeia de caracteres de caminho de propriedade a um atributo em XAML. Você pode usar a mesma sintaxe de cadeia de caracteres para construir um [**PropertyPath**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyPath) que define uma [**Associação**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) em código ou definir um destino de animação em código usando [**SetTargetProperty**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.settargetproperty). Há duas áreas de recurso diferentes no Windows Runtime que usam um caminho de propriedade: vinculação de dados e direcionamento de animação. O direcionamento da animação não cria valores de sintaxe de Property-path subjacentes na implementação do Tempo de Execução do Windows; ele mantém as informações como uma cadeia de caracteres, mas os conceitos de passagem objeto-propriedade são bastante semelhantes. A vinculação de dados e o direcionamento da animação avaliam um caminho de propriedade de maneira um pouco diferente, então vamos descrever a sintaxe do caminho de propriedade separadamente para cada um.

## <a name="property-path-for-objects-in-data-binding"></a>Caminho de propriedade para objetos na vinculação de dados

No Tempo de Execução do Windows, você pode associar ao valor de destino de qualquer propriedade de dependência. O valor de propriedade de origem para uma vinculação de dados não precisa ser uma propriedade de dependência; pode ser uma propriedade em um objeto comercial (por exemplo, uma classe escrita em linguagem Microsoft .NET ou C++). Ou o objeto de origem do valor da associação pode ser um objeto de dependência existente já definido pelo aplicativo. A fonte pode ser referenciada por um simples nome de propriedade ou por um percurso das relações objeto-propriedade no gráfico do objeto do objeto comercial.

Você pode associar a um valor de propriedade individual ou pode associar a uma propriedade de destino que mantenha listas ou coleções. Se sua fonte for uma coleção ou se caminho especificar uma propriedade de coleção, o mecanismo de vinculação de dados corresponde aos itens de coleção da fonte do destino de associação, resultando em comportamentos como popular um [**ListBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox) com uma lista de itens de uma coleção de fonte de dados sem precisar prever os itens específicos em tal coleção.

### <a name="traversing-an-object-graph"></a>Passando um gráfico de objeto

O elemento da sintaxe que indica o percurso de uma relação objeto-propriedade em um gráfico de objeto é o caractere ponto ( **.** ). Cada ponto em uma cadeia de caracteres de caminho de propriedade indica uma divisão entre um objeto (à esquerda do ponto) e uma propriedade desse objeto (à direita do ponto). A cadeia de caracteres é avaliada da esquerda para a direita, o que permite passar por várias relações objeto-propriedade. Vamos ver um exemplo:

``` syntax
"{Binding Path=Customer.Address.StreetAddress1}"
```

Veja como esse caminho é avaliado:

1.  O objeto de contexto de dados (ou uma [**Fonte**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.source) especificada pela mesma [**Associação**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding)) é pesquisado por uma propriedade chamada "Customer".
2.  O objeto que é o valor da propriedade "Customer" é pesquisado por uma propriedade chamada "Address".
3.  O objeto que é o valor da propriedade "Address" é pesquisado por uma propriedade chamada "StreetAddress1".

Em cada um dessas etapas, o valor é tratados como um objeto. O tipo do resultado só é verificado quando a associação é aplicada a uma propriedade específica. Esse exemplo falharia se "Address" fosse apenas um valor de cadeia de caracteres que não expusesse que parte da cadeia é o endereço da rua. Normalmente, a associação aponta para os valores de propriedade aninhados específicos de um objeto de negócios que tem uma estrutura de informações conhecida e clara.

### <a name="rules-for-the-properties-in-a-data-binding-property-path"></a>Regras de propriedades em um caminho de propriedade de vinculação de dados

-   Todas as propriedades referenciadas por um caminho de propriedade devem ser públicas no objeto de negócios da origem.
-   A propriedade final (a propriedade que é nomeada por último no caminho) deve ser pública e mutável – não é possível associar a valores estáticos.
-   A propriedade final deve ser ler/escrever se esse caminho é usado como a informação [**Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) para uma associação de bidirecional.

### <a name="indexers"></a>Indexadores

Um caminho de propriedade para vinculação de dados pode incluir referências a propriedade indexadas. Isso habilita a associação a listas/vetores ordenados ou a dicionários/mapas. Use colchetes "\[\]" caracteres para indicar uma propriedade indexada. O conteúdo desses colchetes pode ser um inteiro (em uma lista ordenada) ou uma cadeia de caracteres sem aspas (em dicionários). Você também pode associar a um dicionário onde a chave é um inteiro. É possível usar diferentes propriedades indexadas no mesmo caminho com um ponto separando o objeto-propriedade.

Por exemplo, considere um objeto comercial em que haja uma lista de "Times" (lista ordenada), cada uma tendo um dicionário de "Jogadores" onde cada jogador é citado pelo último nome. Um exemplo de caminho de propriedade para um player específico da equipe a segunda é: "Teams\[1\].Players\[Smith\]". (Você usa 1 para indicar o segundo item em "Times" porque a lista é indexada com zero.)

**Observação**  suporte de indexação para fontes de dados do C++ é limitado, consulte [vinculação de dados em profundidade](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

### <a name="attached-properties"></a>Propriedades anexadas

Os caminhos de propriedade podem incluir referências a propriedades anexadas. Como o nome que identifica uma propriedade anexada já inclui um ponto, todos os nomes de propriedades anexadas devem estar entre parênteses para que o ponto não seja tratado como uma etapa de objeto-propriedade. Por exemplo, a cadeia de caracteres em que você quer usar [**Canvas.ZIndex**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc190397(v=vs.95)) como caminho de associação é "(Canvas.ZIndex)". Para saber mais sobre as propriedades anexadas, consulte [Visão geral das propriedades anexadas](attached-properties-overview.md).

### <a name="combining-property-path-syntax"></a>Combinando a sintaxe de caminho de propriedade

Você pode combinar vários elementos da sintaxe de caminho de propriedade em uma única cadeia de caracteres. Por exemplo, é possível definir um caminho de propriedade que referencia uma propriedade indexada anexada, se sua fonte de dados tem uma propriedade assim.

### <a name="debugging-a-binding-property-path"></a>Depurando um caminho de propriedade de associação

Por um caminho de propriedade ser interpretado por um mecanismo de associação e depender de informações que talvez estejam presentes somente no tempo de execução, você deve eliminar erros com frequência de um caminho de propriedade para associação sem poder depender do tempo de design convencional ou suporte de tempo de compilação nas ferramentas de desenvolvimento. Em muitos casos, o resultado do tempo de execução da falha ao resolver um caminho de propriedade é um valor em branco sem erro, porque esse é o comportamento de fallback por design da resolução da associação. Felizmente, o Microsoft Visual Studio fornece um modo de saída de depuração que pode isolar a parte de um caminho de propriedade que especifica uma fonte de associação com falha para a resolução. Para saber mais sobre como usar esse recurso da ferramenta de desenvolvimento, consulte a [seção "Depuração" em Associação de dados em detalhes](../data-binding/data-binding-in-depth.md#debugging).

## <a name="property-path-for-animation-targeting"></a>Caminho de propriedade para direcionamento da animação

As animações dependem do direcionamento de uma propriedade de dependência onde valores de storyboard são aplicados quando a animação é reproduzida. Para identificar o objeto no qual a propriedade que deve ser animada existe, a animação direciona um ele por nome ([atributo x:Name](x-name-attribute.md)). Costuma ser necessário definir um caminho de propriedade que inicia com o objeto identificado como [**Storyboard.TargetName**](https://docs.microsoft.com/dotnet/api/system.windows.media.animation.storyboard.targetname?view=netframework-4.8) e termina com o valor de propriedade de dependência particular em que a animação deve ser aplicada. Tal caminho de propriedade é usada como o valor de [**Storyboard.TargetProperty**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms616983(v=vs.95)).

Para saber mais sobre como definir animações em XAML, consulte [Animações de storyboard](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations).

## <a name="simple-targeting"></a>Direcionamento simples

Se você está animando uma propriedade que existe no próprio objeto direcionado, e se o tipo de propriedade pode ter uma animação aplicada diretamente a ela (em vez de em uma subpropriedade de um valor de propriedade), então você pode simplesmente nomear a propriedade sendo animada sem nenhuma outra qualificação. Por exemplo, se você estiver direcionando uma subclasse [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) como [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle), e estiver aplicando um [**Color**](https://docs.microsoft.com/uwp/api/Windows.UI.Color) animado à propriedade [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill), o seu caminho de propriedade pode ser "Fill".

## <a name="indirect-property-targeting"></a>Direcionamento indireto de propriedade

Você pode animar uma propriedade que é subpropriedade do objeto de destino. Em outras palavras, se existe uma propriedade do objeto de destino que é um objeto, e esse objeto tem propriedades, você deve definir um caminho de propriedade que explique como passar por essa relação objeto-propriedade. Sempre que você estiver especificando um objeto cuja subpropriedade quiser animar, coloque o nome da propriedade em parênteses e especifique a propriedade em formato *typename*.*propertyname*. Por exemplo, para especificar que você quer um valor de objeto da propriedade [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) de um objeto de destino, especifique "(UIElement.RenderTransform)" como a primeira etapa no caminho de propriedade. Esse não é um caminho muito completo porque não há animações aplicáveis a um valor [**Transform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform) diretamente. Então, nesse exemplo, você conclui o caminho de propriedade agora para que a propriedade final seja propriedade de uma **Transform** que pode ser animada por um valor **Double**: "(UIElement.RenderTransform).(CompositeTransform.TranslateX)"

## <a name="specifying-a-particular-child-in-a-collection"></a>Especificando um filho em particular em uma coleção

Para especificar um item filho em uma propriedade de coleção, você pode usar um indexador numérico. Use colchetes "\[\]" valor de índice de caracteres em torno de inteiro. Você pode fazer referência apenas a listas ordenadas, não a dicionários. Como uma coleção não é um valor que pode ser animado, o uso de um indexador nunca pode ser a propriedade final em um caminho de propriedade.

Por exemplo, para especificar que você deseja animar a cor de primeiro parar cor em um [ **LinearGradientBrush** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) que é aplicado a um controle [ **em segundo plano** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.background) propriedade, esse é o caminho da propriedade: "(Background). (GradientBrush.GradientStops) \[0\]. ( GradientStop.Color) ". Observe que o indexador não é a última etapa do caminho e que a última etapa deve fazer referência à propriedade [**GradientStop.Color**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.gradientstop.color) do item 0 da coleção para aplicar um valor animado [**Color**](https://docs.microsoft.com/uwp/api/Windows.UI.Color) a ela.

## <a name="animating-an-attached-property"></a>Animando uma propriedade anexada

Não é comum, mas é possível animar uma propriedade anexada, desde que ela tenha um valor de propriedade que corresponda a um tipo de animação. Como o nome que identifica uma propriedade anexada já inclui um ponto, todos os nomes de propriedades anexadas devem estar entre parênteses para que o ponto não seja tratado como uma etapa de objeto-propriedade. Por exemplo, a cadeia de caracteres para especificar que você quer animar a propriedade anexada [**Grid.Row**](https://docs.microsoft.com/dotnet/api/system.windows.controls.grid.row?view=netframework-4.8) em um objeto, use o caminho de propriedade "(Grid.Row)".

**Observação**  neste exemplo, o valor de [ **Row** ](https://docs.microsoft.com/dotnet/api/system.windows.controls.grid.row?view=netframework-4.8) é um **Int32** tipo de propriedade. então, você não pode animá-lo com uma animação **Double**. Em vez disso, você definiria um [**ObjectAnimationUsingKeyFrames**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames) que tem componentes [**DiscreteObjectKeyFrame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame), onde o [**ObjectKeyFrame.Value**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.objectkeyframe.value) é definido como um inteiro como "0" ou "1".

## <a name="rules-for-the-properties-in-an-animation-targeting-property-path"></a>Regras para as propriedades em um caminho de propriedade de direcionamento de animação

-   O suposto ponto de partida de um caminho de propriedade é o objeto identificado por um [**Storyboard.TargetName**](https://docs.microsoft.com/dotnet/api/system.windows.media.animation.storyboard.targetname?view=netframework-4.8).
-   Todos os objetos e todas as propriedades referenciados ao longo do caminho de propriedade devem ser públicos.
-   A propriedade final (a propriedade que é nomeada por último no caminho) deve ser pública, de leitura-gravação e ser uma propriedade de dependência.
-   A propriedade final deve ter um tipo de propriedade capaz de ser animado por uma das amplas classes de tipos de animação, animações ([**Color**](https://docs.microsoft.com/uwp/api/Windows.UI.Color), animações **Double**, animações [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point), [**ObjectAnimationUsingKeyFrames**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames)).

## <a name="the-propertypath-class"></a>A classe PropertyPath

A classe [**PropertyPath**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyPath) é o tipo de propriedade subjacente de [**Binding.Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) para a situação de associação.

Na maioria das vezes, é possível aplicar um [**PropertyPath**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyPath) em XAML sem usar nenhum código. Mas, em alguns casos, pode ser que você queria definir um objeto **PropertyPath** usando um código e atribuí-lo a uma propriedade em tempo de execução.

[**PropertyPath** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyPath) tem um [ **PropertyPath(String)** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.propertypath.-ctor) construtor e não tem um construtor padrão. A cadeia de caracteres que você passa para esse construtores é definida usando uma sintaxe de caminho de propriedade, como explicado anteriormente. Ela também é a mesma cadeia de caracteres que você usaria para atribuir [**Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) como atributo XAML. O único outro API da classe **PropertyPath** na propriedade [**Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.propertypath.path), que é somente para leitura. Você poderia usar essa propriedade como a cadeia de caracteres de construção para outra instância **PropertyPath**.

## <a name="related-topics"></a>Tópicos relacionados

* [Vinculação de dados em detalhes](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)
* [Animações storyboarded](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations)
* [Extensão de marcação {binding}](binding-markup-extension.md)
* [**PropertyPath**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyPath)
* [**Associação**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding)
* [**Construtor de associação**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.-ctor)
* [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext)

