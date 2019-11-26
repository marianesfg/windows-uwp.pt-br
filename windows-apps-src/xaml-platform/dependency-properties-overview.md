---
description: Este tópico explica o sistema de propriedades de dependência que está disponível quando você escreve um aplicativo do Windows Runtime em C++, C# ou Visual Basic com definições de XAML para a interface do usuário.
title: Visão geral das propriedades de dependência
ms.assetid: AD649E66-F71C-4DAA-9994-617C886FDA7E
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 279f0d007be927e29632986ce8178c4e0b9778b3
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259855"
---
# <a name="dependency-properties-overview"></a>Visão geral das propriedades de dependência

Este tópico explica o sistema de propriedades de dependência que está disponível quando você escreve um aplicativo do Windows Runtime em C++, C# ou Visual Basic com definições de XAML para a interface do usuário.

## <a name="what-is-a-dependency-property"></a>O que é uma propriedade de dependência?

Uma propriedade de dependência é um tipo especializado de propriedade. Especificamente, é uma propriedade cujo valor é controlado e influenciado por um sistema de propriedades dedicado que é parte do Tempo de Execução do Windows.

Para dar suporte a uma propriedade de dependência, o objeto que define a propriedade deve ser um [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) (em outras palavras, uma classe que tem a classe base **DependencyObject** em algum lugar de sua herança). Muitos dos tipos que você usa para suas definições de interface do usuário para um aplicativo UWP com XAML serão uma subclasse **DependencyObject** e oferecerão suporte a propriedades de dependência. Entretanto, qualquer tipo que venha de um namespace do Tempo de Execução do Windows que não possua "XAML" em seu nome não dará suporte a propriedades de dependência; as propriedades de tais tipos são propriedades comuns que não terão o comportamento de dependência do sistema de propriedades.

A finalidade das propriedades de dependência é oferecer uma forma sistêmica de computar o valor de uma propriedade com base em outras entradas (outras propriedades, eventos e estados que ocorrem em seu aplicativo enquanto ele é executado). Essas outras entradas podem incluir:

- Entrada externa; por exemplo, preferência do usuário
- Mecanismos de determinação de propriedade just-in-time; por exemplo, vinculação de dados, animações e storyboards.
- Padrões de modelagem para múltiplos usos; por exemplo, recursos e estilos
- Valores conhecidos por meio das relações pai-filho com outros elementos da árvore de objetos

Uma propriedade de dependência representa ou dá suporte a um recurso específico do modelo de programação para definir um aplicativo Windows Runtime com XAML C#para interface do usuário e C++ , Microsoft Visual BasicC++ou extensões de componente Visual (/CX) para código. Esses recursos incluem:

- Associação de dados
- Estilos
- Animações com storyboard
- Comportamento "PropertyChanged"; uma propriedade de dependência pode ser implementada para fornecer retornos de chamada que podem propagar alterações em outras propriedades de dependência
- Usando um valor padrão que vem dos metadados da propriedade
- Utilitário do sistema de propriedades gerais, como [**ClearValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.clearvalue) e pesquisa de metadados

## <a name="dependency-properties-and-windows-runtime-properties"></a>Propriedades de dependência e propriedades do Tempo de execução do Windows

As propriedades de dependência estendem a funcionalidade de propriedade básica do Tempo de Execução do Windows, oferecendo um repositório de propriedades global, interno, que grava todas as propriedades de dependência em um aplicativo no tempo de execução. Trata-se de uma alternativa ao padrão de sustentar uma propriedade com um campo privado na classe property-definition. Você pode pensar em um repositório de propriedades como um conjunto de identificadores de propriedades e de valores existentes para qualquer objeto específico. (desde que seja um [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject)). Em vez de serem identificadas pelo nome, cada propriedade no repositório é identificada por uma instância [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty). Entretanto, o sistema de propriedades oculta principalmente o detalhe dessa implementação: você pode geralmente acessar as propriedades de dependência, usando um nome simples (o nome da propriedade via programação na linguagem do código que você está usando, ou um nome de atributo quando você está escrevendo em XAML).

O tipo de base que fornece o suporte ao sistema de propriedades de dependência é [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject). **DependencyObject** define métodos que podem acessar a propriedade de dependência, e as instâncias de uma classe derivada **DependencyObject** suportam internamente o conceito de repositório de propriedades mencionado anteriormente.

Aqui está uma somatória da terminologia que usamos na documentação ao discutir as propriedades de dependência:

| Termo | Descrição |
|------|-------------|
| Propriedade de dependência | Uma propriedade que existe em um identificador [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty) (veja abaixo). Geralmente, esse identificador está disponível como um membro estático da classe derivada **DependencyObject** de definição. |
| Identificador de propriedade de dependência | Um valor constante para identificar a propriedade, é normalmente público e somente leitura. |
| Wrapper de propriedade | As implementações **get** e **set** que podem ser chamadas para uma propriedade do Windows Runtime. Ou, a projeção específica da linguagem da definição original. Uma implementação do wrapper de propriedade **get** chama [**GetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.getvalue), passando o identificador de propriedade de dependência relevante. |

O wrapper da propriedade não é apenas conveniência para os chamadores, ele também apresenta a propriedade de dependência a qualquer processo, ferramenta ou projeção que use as  definições do Windows Runtime para propriedades.

O exemplo a seguir define uma propriedade de dependência personalizada "IsSpinning", conforme definida para C#, e mostra a relação do identificador de propriedade de dependência com o wrapper de propriedade.

```csharp
// IsSpinningProperty is the dependency property identifier
// no need for info in the last PropertyMetadata parameter, so we pass null
public static readonly DependencyProperty IsSpinningProperty =
    DependencyProperty.Register(
        "IsSpinning", typeof(Boolean),
        typeof(ExampleClass), null
    );
// The property wrapper, so that callers can use this property through a simple ExampleClassInstance.IsSpinning usage rather than requiring property system APIs
public bool IsSpinning
{
    get { return (bool)GetValue(IsSpinningProperty); }
    set { SetValue(IsSpinningProperty, value); }
}
```

> [!NOTE]
> O exemplo anterior não se destina como o exemplo completo de como criar uma propriedade de dependência personalizada. Ele tem a finalidade de mostrar os conceitos de propriedade de dependência para qualquer um que prefira conceitos de aprendizagem através de código. Para um exemplo mais complexo, consulte [Propriedades de dependência personalizada](custom-dependency-properties.md).

## <a name="dependency-property-value-precedence"></a>Precedência do valor da propriedade de dependência

Quando você obtém o valor de uma propriedade de dependência, está obtendo um valor que foi definido para essa propriedade por meio de qualquer uma das entradas que participam do sistema de propriedades do Tempo de execução do Windows. A precedência do valor de propriedade de dependência existe de forma que o sistema de propriedades do Windows Runtime possa calcular valores de uma forma previsível, e é importante que você também esteja familiarizado com a ordem de precedência básica. Caso contrário, você pode se encontrar em uma situação em que esteja tentando definir uma propriedade em um nível, mas algo (o sistema, chamadores de terceiros, parte de seu próprio código) a está definindo em outro nível, e você ficará frustrado tentando descobrir qual valor da propriedade é usado e de onde esse valor veio.

Por exemplo, os estilos e modelos destinam-se a ser um ponto de partida compartilhado para estabelecer valores de propriedade e, assim, a aparência de um controle. Mas, em uma ocorrência específica do controle, pode ser preciso alterar esse valor em relação ao valor de modelo comum, como aplicar a esse controle uma cor da tela de fundo diferente ou uma cadeia de caracteres de texto diferente como conteúdo. O sistema de propriedades do Tempo de execução do Windows considera valores locais em precedência maior que os valores fornecidos por estilos e modelos. Isso permite que o cenário de valores específicos do aplicativo substituam os modelos, de modo que os controles sejam úteis para o próprio uso deles na interface do usuário do aplicativo.

### <a name="dependency-property-precedence-list"></a>Lista de precedência das propriedades de dependência

A seguir está a ordem definitiva que o sistema de propriedades usa ao atribuir o valor do tempo de execução de uma propriedade de dependência. A maior precedência é listada primeiro. Você encontrará explicações mais detalhadas logo após essa lista.

1. **Valores animados:** animações ativas, animações de estado visual ou animações com um comportamento [**HoldEnd**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FillBehavior). Para ter efeito prático, uma animação aplicada a uma propriedade deve ter precedência sobre o valor básico (não animado), mesmo que esse valor seja definido localmente.
1. **Valor local:** um valor local pode ser definido por meio da conveniência do wrapper da propriedade, que também equivale à configuração como um elemento de atributo ou de propriedade na XAML ou por uma chamada ao método [**SetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.setvalue) usando uma propriedade de uma instância específica. Se você definir um valor local usando uma associação ou um recurso estático, eles atuarão na precedência como se um valor local fosse definido, e as referências de associações ou de recursos serão apagadas se um novo valor local for definido.
1. **Propriedades modelo:** um elemento as tem se tiver sido criado como parte de um modelo (um [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) ou [**DataTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate)).
1. **Setters de estilo:** valores de um [**Setter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter) em estilos de recursos de página ou aplicativo.
1. **Valor padrão:** uma propriedade de dependência pode ter um valor padrão como parte de seus metadados.

### <a name="templated-properties"></a>Propriedades modelo

As propriedades modelo, como um item de precedência, não se aplicam a qualquer propriedade de um elemento que você declare diretamente na marcação da página da XAML. O conceito de propriedade modelo existe apenas para objetos que são criados quando o Tempo de Execução do Windows aplica um modelo XAML a um elemento de interface do usuário e, portanto, define seu visual.

Todas as propriedades definidas a partir de um modelo de controle apresentam valores de algum tipo. Esses valores são quase como um conjunto estendido de valores padrão para o controle e são frequentemente associados aos valores que você pode redefinir posteriormente, definindo os valores de propriedade diretamente. Assim, os valores de conjunto modelo devem ser distinguíveis de um verdadeiro valor local, de modo que qualquer novo valor local possa substituí-lo.

> [!NOTE]
> Em muitos casos, o modelo pode substituir até mesmo valores locais, se o modelo não tiver conseguido expor referências da [extensão de marcação {TemplateBinding}](templatebinding-markup-extension.md) para propriedades que devem ter sido configuráveis nas instâncias. Isso geralmente é feito somente se a propriedade realmente não se destina a ser definida em instâncias, por exemplo, se ela for relevante apenas para o comportamento de visuais e de modelo e não para a função pretendida ou a lógica de tempo de execução do controle que usa o modelo.

### <a name="bindings-and-precedence"></a>Associações e precedência

As operações de associação têm a precedência apropriada para qualquer escopo em que são usadas. Por exemplo, uma [{Associação}](binding-markup-extension.md) aplicada a um valor local age como um valor local, e uma [extensão de marcação {TemplateBinding}](templatebinding-markup-extension.md) de um setter da propriedade se aplica como um setter do estilo. Como as associações devem aguardar até o tempo de execução para obter valores de fontes de dados, o processo de determinar a precedência do valor de propriedade de qualquer propriedade também se estende para o tempo de execução.

As associações não apenas operam com a mesma precedência como um valor local, elas realmente são um valor local, em que a associação é o espaço reservado de um valor que é adiado. Se você tiver uma associação para um valor de propriedade, e você define um valor local nele no tempo de execução, isso substitui totalmente a associação. Similarmente, se você chamar [**SetBinding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.setbinding) para definir uma associação que apenas surge no tempo de execução, você substitui qualquer valor local que tenha aplicado no XAML ou com código executado anteriormente.

### <a name="storyboarded-animations-and-base-value"></a>Animações com storyboard e valor base

Animações com storyboard agem em relação ao conceito de um *valor base*. O valor base é o valor que é determinado pelo sistema de propriedades usando sua precedência, mas omitindo essa última etapa de procura de animações. Por exemplo, um valor base deve vir de um modelo de controle, ou pode vir da configuração de um valor local em uma instância de um controle. De qualquer forma, aplicar uma animação substituirá esse valor base e aplicará o valor animado durante o tempo em que a animação continua a ser executada.

Para uma propriedade animada, o valor base ainda pode ter um efeito no comportamento da animação, se essa animação não especificar explicitamente **From** e **To**, ou se a animação reverte a propriedade para seu valor básico quando concluída. Nesses casos, quando uma animação não estiver mais sendo executada, o resto da precedência é usado novamente.

Entretanto, uma animação que especifica um **To** com um comportamento [**HoldEnd**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FillBehavior) pode substituir um valor local até que a animação seja removida, mesmo que ela pareça estar visualmente interrompida. Conceitualmente, ela é como uma animação que é executada infinitamente mesmo que não haja uma animação visual na interface do usuário.

Várias animações podem ser aplicadas a uma única propriedade. Cada uma dessas animações pode ter sido definida para substituir valores base que vêm de pontos diferentes na precedência de valor. Entretanto, essas animações serão executadas simultaneamente no tempo de execução, e isso frequentemente significa que elas devem combinar seus valores porque cada animação tem a mesma influência no valor. Isso depende exatamente de como as animações são definidas, e do tipo do valor que está sendo animado.

Para saber mais, consulte [Animações com storyboard](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations).

### <a name="default-values"></a>Valores padrão

O estabelecimento do valor padrão para uma propriedade de dependência com um valor [**PropertyMetadata**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyMetadata) é explicado com mais detalhes no tópico [Propriedades de dependência personalizada](custom-dependency-properties.md).

As propriedades de dependência ainda possuem valores padrão, mesmo que esses valores padrão não tenham sido definidos explicitamente nos metadados dessas propriedades. A menos que tenham sido alterados por metadados, os valores padrão das propriedades de dependência do Tempo de Execução do Windows são geralmente um dos seguintes:

- Uma propriedade que usa um objeto de tempo de execução ou o tipo básico **Object** (um *tipo de referência*) possui um valor padrão **null**. Por exemplo, [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) é **null** até que seja definido deliberadamente ou herdado.
- Uma propriedade que usa um valor básico, como números ou um valor booliano (um *tipo de valor*) usa um padrão esperado para esse valor. Por exemplo, 0 para inteiros e números de ponto flutuante, **false** para um booliano.
- Uma propriedade que usa uma estrutura do Tempo de Execução do Windows possui um valor padrão que é obtido pela chamada ao construtor padrão implícito dessa estrutura. Esse construtor usa os padrões de cada um dos campos de valor básico da estrutura. Por exemplo, um padrão de um valor [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) é inicializado com seus valores **X** e **Y** como 0.
- Uma propriedade que usa uma enumeração possui um valor padrão do primeiro membro definido nessa enumeração. Verifique a referência a enumerações específicas para ver qual é o valor padrão.
- Uma propriedade que usa uma cadeia de caracteres ([**System.String**](https://docs.microsoft.com/dotnet/api/system.string) para .NET, [**Platform::String**](https://docs.microsoft.com/cpp/cppcx/platform-string-class) para C++/CX) tem um valor padrão de uma cadeia de caracteres vazia ( **""** ).
- As propriedades de coleção não são normalmente implementadas como propriedades de dependência, por razões discutidas mais adiante neste tópico. Mas se você implementar uma propriedade de coleção personalizada e quiser que ela seja uma propriedade de dependência, lembre-se de evitar um *singleton não intencional* conforme descrito quase no final de [Propriedades de dependência personalizada](custom-dependency-properties.md).

## <a name="property-functionality-provided-by-a-dependency-property"></a>Funcionalidade de propriedade fornecida por uma propriedade de dependência

### <a name="data-binding"></a>Associação de dados

Uma propriedade de dependência pode ter seu valor definido por meio de uma vinculação de dados. A vinculação de dados usa a sintaxe da [extensão de marcação {Binding}](binding-markup-extension.md) em XAML, [extensão de marcação {x:Bind}](x-bind-markup-extension.md) ou a classe [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) no nó. Para uma propriedade associada aos dados, a determinação do valor final da propriedade é adiada até o tempo de execução. Nesse momento, o valor é obtido de uma fonte de dados. A função que o sistema de propriedades de dependência exerce aqui é permitir um comportamento de espaço reservado para operações como carregamento do XAML quando o valor ainda não é conhecido, e o fornecer o valor no runtime por meio da interação com o mecanismo de vinculação de dados do Windows Runtime.

O exemplo a seguir define o valor [**Text**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.text) para um elemento [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock), usando uma associação no XAML. A associação usa um contexto de dados herdado e um objeto da fonte de dados. (Nenhum destes é exibido no exemplo curto; para obter uma amostra mais completa que mostre o contexto e a fonte, consulte [Vinculação de dados em detalhes](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).)

```xaml
<Canvas>
  <TextBlock Text="{Binding Team.TeamName}"/>
</Canvas>
```

Você também pode estabelecer associações usando código em vez de XAML. Veja [**SetBinding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.setbinding).

> [!NOTE]
> Associações como essa são tratadas como um valor local para fins de precedência de valor da propriedade de dependência. Se você definir outro valor local para uma propriedade que originalmente tinha um valor [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding), você substituirá totalmente a associação, não apenas o valor de tempo de execução da associação. As associações {x: Bind} são implementadas usando o código gerado que definirá um valor local para a propriedade. Se você definir um valor local para uma propriedade que está usando {x:Bind}, em seguida, esse valor será substituído na próxima vez que a associação for avaliada, por exemplo, quando observar uma propriedade alterar em seu objeto de origem.

### <a name="binding-sources-binding-targets-the-role-of-frameworkelement"></a>Associar origens, associar destinos, a função de FrameworkElement

Para ser a origem de uma associação, uma propriedade não precisa ser uma propriedade de dependência; você geralmente pode usar qualquer propriedade como uma origem de associação, embora isso dependa de sua linguagem de programação e cada uma tem determinadas margens. Entretanto, para ser o destino de uma [extensão de marcação {Binding}](binding-markup-extension.md) ou [**Associação**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding), essa propriedade deve ser uma propriedade de dependência. A {x:Bind} não têm esse requisito enquanto usa código gerado para aplicar a seus valores de associação.

Se você estiver criando uma associação no código, observe que a API [**SetBinding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.setbinding) é definida apenas para [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement). Entretanto, você pode criar uma definição de associação usando [**BindingOperations**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindingOperations) em vez disso e, assim fazer referência a qualquer propriedade de [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject).

Tanto para código ou XAML, lembre-se que [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) é uma propriedade de [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement). Usando uma forma de herança de propriedade pai-filha (geralmente estabelecida na marcação XAML), o sistema de associações pode resolver um **DataContext** que existe em um elemento pai. Essa herança pode avaliar inclusive se o objeto filho (que possui a propriedade de destino) não é **FrameworkElement** e, portanto, não mantém seu próprio valor **DataContext**. Entretanto, o elemento pai que está sendo herdado deve ser um **FrameworkElement** para definir e manter o **DataContext**. Como alternativa, você deve definir a associação para que ela possa funcionar com um valor **null** para **DataContext**.

Conectar a associação não é a única coisa que é necessária para a maioria dos cenários de associação. Para uma associação unidirecional ou bidirecional ser eficaz, a propriedade de origem deve dar suporte a notificações de alteração que se propagam para o sistema de associação e, portanto, o destino. Para fontes de associação personalizadas, isso significa que a propriedade deve ser uma propriedade de dependência ou o objeto deve suportar [**INotifyPropertyChanged**](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged). As coleções devem dar suporte a [**INotifyCollectionChanged**](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged). Determinadas classes dão suporte a uma dessas interfaces em suas implementações para que sejam úteis como classes básicas para situações de vinculação de dados; um exemplo dessa classe é [**ObservableCollection&lt;T&gt;** ](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1). Para saber mais sobre vinculação de dados e como a vinculação de dados se relaciona ao sistema de propriedades, consulte [Vinculação de dados em detalhes](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

> [!NOTE]
> Os tipos listados aqui dão suporte a Microsoft .NET fontes de dados. As fontes de dados C++/CX usam interfaces diferentes para notificação de alteração ou comportamento observável, consulte a seção [Vinculação de dados em detalhes](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

### <a name="styles-and-templates"></a>Estilos e modelos

Estilos e modelos são dois dos principais cenários para que as propriedades sejam definidas como propriedades de dependência. Os estilos são úteis para definir as propriedades que definem a interface do usuário do aplicativo. Os estilos são definidos como recursos em XAML, seja como uma entrada em uma coleção de [**Resources**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.resources), seja em arquivos XAML separados, por exemplo, em dicionários de recursos de tema. Os estilos interagem com o sistema de propriedades porque eles contêm setters para propriedades. A propriedade mais importante aqui é a propriedade [**Control.Template**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.template) de um [**Control**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control), que define a maior parte da aparência visual e do estado visual do **Control**. Para saber mais sobre estilos e obter alguns exemplos de XAML que definem um [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) e usam setters, consulte [Definindo o estilo de controles](https://docs.microsoft.com/windows/uwp/controls-and-patterns/styling-controls).

Os valores que vêm de estilos ou modelos são valores adiados, semelhantes a associações. Tanto que os usuários de controles podem remodelar controles ou redefinir estilos. E é por isso que os setters de propriedade em estilos só podem atuar em propriedades de dependência, não em propriedades comuns.

### <a name="storyboarded-animations"></a>Animações com storyboard

Você pode animar o valor de uma propriedade de dependência usando uma animação com storyboard. As animações com storyboard no Tempo de Execução do Windows não são meramente decorações visuais. É mais útil pensar em animações como sendo uma técnica de máquina de estado que pode definir os valores de propriedades individuais ou de todas as propriedades e visuais de um controle, e alterar esses valores ao longo do tempo.

Para ser animada, a propriedade de destino da animação deve ser uma propriedade de dependência. Além disso, para ser animado, o tipo de valor da propriedade de destino deve ter suporte de um dos tipos existentes de animação derivados de [**Timeline**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Timeline). Valores de [**Color**](https://docs.microsoft.com/uwp/api/Windows.UI.Color), [**Double**](https://docs.microsoft.com/dotnet/api/system.double) e [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) podem ser animados por meio de técnicas de interpolação ou quadro-chave. A maioria dos outros valores pode ser animada por meio de quadros-chave discretos **Object**.

Quando uma animação é aplicada e está em execução, o valor animado opera em uma precedência maior que qualquer valor (como um valor local) que a propriedade tenha. As animações também têm um comportamento [**HoldEnd**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FillBehavior) opcional que pode fazer com que apliquem valores de propriedade, mesmo que a animação visualmente pareça estar parada.

O princípio de máquina de estado é consagrado pelo uso de animações com storyboard como parte do modelo de estado [**VisualStateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualStateManager) para controles. Para saber mais sobre animações com storyboard, consulte [Animações com storyboard](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations). Para saber mais sobre **VisualStateManager** e definir estados visuais para controles, consulte [Animações com storyboard para estados visuais](https://docs.microsoft.com/previous-versions/windows/apps/jj819808(v=win.10)) ou [Modelos de controle](../design/controls-and-patterns/control-templates.md).

### <a name="property-changed-behavior"></a>Comportamento de propriedade alterado

O comportamento de propriedade alterado é a origem da parte "dependência" da terminologia de propriedade de dependência. A manutenção de valores válidos de uma propriedade em que outra propriedade pode influenciar o valor da primeira propriedade é um problema de desenvolvimento difícil em muitas estruturas. No sistema de propriedades do Windows Runtime, cada propriedade de dependência pode especificar um retorno de chamada que é invocado quando o respectivo valor de propriedade é alterado. Esse retorno de chamada pode ser usado para notificar ou alterar valores de propriedades relacionados de forma geralmente síncrona. Muitas propriedades de dependência existentes têm um comportamento de propriedade alterado. Você também pode adicionar comportamento semelhante de retorno de chamada para propriedades de dependência personalizadas e implementar os seus próprios retornos de chamada de propriedade alterada. Consulte [Propriedades de dependência personalizadas](custom-dependency-properties.md) para ver um exemplo.

O Windows 10 apresenta o método [**RegisterPropertyChangedCallback**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.registerpropertychangedcallback). Isso permite que o código do aplicativo se registre para receber notificações de alteração quando a propriedade de dependência especificada é alterada em uma instância de [**DependencyObject**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject).

### <a name="default-value-and-clearvalue"></a>Valor padrão e **ClearValue**

Uma propriedade de dependência pode ter um valor padrão definido como parte de seus metadados de propriedade. Para uma propriedade de dependência, seu valor padrão não se torna irrelevante depois que a propriedade tiver sido definida pela primeira vez. O valor padrão talvez se aplique novamente no tempo de execução sempre que outro determinante na precedência do valor desaparece. (A precedência do valor das propriedades de dependência é discutida na próxima seção.) Por exemplo, você pode deliberadamente remover um valor de estilo ou uma animação que se aplica a uma propriedade, mas quer que o valor seja um padrão razoável após fazê-lo. O valor padrão de propriedade de dependência pode fornecer esse valor, sem a necessidade de definir especificamente o valor de cada propriedade como um passo extra.

Você pode definir deliberadamente uma propriedade para o valor padrão, mesmo depois de a ter definido com um valor local. Para redefinir um valor para ser o padrão novamente e também para permitir outros participantes na precedência que possam substituir o padrão, mas não um valor local, chame o método [**ClearValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.clearvalue) (referência à propriedade para limpar como um parâmetro do método). Você nem sempre quer que a propriedade use literalmente o valor padrão, mas limpar o valor local e reverter para o valor padrão pode permitir outro item na precedência que você quer que atue agora, como usar o valor que veio de um setter de estilo em um modelo de controle.

## <a name="dependencyobject-and-threading"></a>**DependencyObject** e threading

Todas as instâncias de [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) devem ser criadas no thread da interface do usuário que está associado à [**Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) mostrada por um aplicativo do Tempo de Execução do Windows. Embora seja necessário que cada **DependencyObject** seja criada no thread da interface do usuário principal, os objetos podem ser acessados usando uma referência de dispatcher de outros threads acessando a propriedade [**Dispatcher**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.dispatcher). Em seguida, você pode chamar métodos como [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) no objeto [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) e executar o código dentro das regras de restrições de thread no thread de interface do usuário.

Os aspectos de threading de [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) são relevantes porque em geral isso significa que somente o código que é executado no thread da interface do usuário pode mudar ou até mesmo ler o valor de uma propriedade de dependência. Em geral, é possível evitar os problemas com threading em códigos de interface do usuário típicos que usam corretamente padrões **async** e threads de trabalho em segundo plano. Normalmente, você tem problemas de threading relacionados a **DependencyObject** somente quando define seus próprios tipos de **DependencyObject** e tenta usá-los para fontes de dados ou outros cenários em que um **DependencyObject** não é necessariamente apropriado.

## <a name="related-topics"></a>Tópicos relacionados

### <a name="conceptual-material"></a>Material conceitual

- [Propriedades de dependência personalizadas](custom-dependency-properties.md)
- [Visão geral das propriedades anexadas](attached-properties-overview.md)
- [Vinculação de dados em detalhes](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)
- [Animações storyboarded](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations)
- [Criando componentes de Windows Runtime](https://docs.microsoft.com/previous-versions/windows/apps/hh441572(v=vs.140))
- [Usuário XAML e exemplo de controles personalizados](https://code.msdn.microsoft.com/windowsapps/XAML-user-and-custom-a8a9505e)

## <a name="apis-related-to-dependency-properties"></a>APIs relacionadas a propriedades de dependência

- [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject)
- [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty)

