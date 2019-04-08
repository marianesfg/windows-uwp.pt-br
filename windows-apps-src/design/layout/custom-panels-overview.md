---
Description: Você pode definir painéis personalizados para layouts XAML derivando uma classe personalizado da classe Panel.
MS-HAID: dev\_ctrl\_layout\_txt.xaml\_custom\_panels\_overview
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Visão geral de painéis personalizados XAML
ms.assetid: 0CD395CD-E2AB-429D-BB49-56A71C5CC35D
label: XAML custom panels overview (Windows apps)
template: detail.hbs
op-migration-status: ready
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9999ebb121916a7804546784ea98ac4e0f4222e5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620341"
---
# <a name="xaml-custom-panels-overview"></a>Visão geral de painéis personalizados XAML

 

Um *painel* é um objeto que fornece um comportamento de layout para elementos filho que ele contém, quando o sistema de layout XAML (Extensible Application Markup Language) é executado e a interface do usuário do aplicativo é renderizada. 


> **APIs importantes**: [**Painel**](https://msdn.microsoft.com/library/windows/apps/br227511), [ **ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711), [ **MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730)

Você pode definir painéis personalizados para layouts XAML derivando uma classe personalizado da classe [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511). Você fornece o comportamento para o seu painel, substituindo [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) e [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711), fornecendo uma lógica que mede e organiza os elementos filhos.

## <a name="the-panel-base-class"></a>A classe base **Panel**


Para definir uma classe de painel personalizado, você pode derivar diretamente da classe [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) ou derivar de uma das classes de painel prático que não são seladas, como [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) ou [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635). É mais fácil derivar de **Panel**, porque pode ser difícil contornar a lógica de layout existente de um painel que já tem o comportamento de layout. Além disso, um painel com o comportamento pode ter propriedades existentes que não são relevantes para os recursos de layout do seu painel.

Do [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511), seu painel personalizado herda as seguintes APIs:

-   A propriedade [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514).
-   As propriedades [**Background**](https://msdn.microsoft.com/library/windows/apps/br227512), [**ChildrenTransitions**](https://msdn.microsoft.com/library/windows/apps/br227515) e [**IsItemsHost**](https://msdn.microsoft.com/library/windows/apps/br227517) e os identificadores de propriedade de dependência. Nenhuma dessas propriedades são virtuais. Dessa forma, não é possível sobrepô-las nem substituí-las. Em geral, você não precisa dessas propriedades para cenários de painel personalizado, nem mesmo para a leitura de valores.
-   Os métodos de substituição de layout [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) e [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711). Originalmente eles foram definidos pelo [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706). A classe de base [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) não os substitui, mas os painéis práticos como [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) tem implementações de substituição que são implementadas como código nativo e são executadas pelo sistema. Fornecer implementações novas (ou adicionais) para **ArrangeOverride** e **MeasureOverride** é a parte difícil do esforço necessário para definir um painel personalizado.
-   Todas as outras APIs de [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706), [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) e [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356), como [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height), [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) e assim por diante. Às vezes, você faz referência a valores dessas propriedades nas substituições de layout, mas eles não são virtuais, de modo que, normalmente, não são sobrepostos ou substituídos.

O foco aqui é descrever os conceitos de layout XAML, para que você possa considerar todas as possibilidades de como um painel personalizado pode e deve se comportar no layout. Se você preferir entrar em detalhes e ver um exemplo de implementação do painel personalizado, consulte [BoxPanel, um exemplo de painel personalizado](boxpanel-example-custom-panel.md).

## <a name="the-children-property"></a>A propriedade **Children**


A propriedade [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) é relevante para um painel personalizado, porque todas as classes derivadas do [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) usam a propriedade **Children** como o lugar para armazenar seus elementos filho contidos em uma coleção. A propriedade **Children** é designada como a propriedade de conteúdo XAML para a classe **Panel**, e todas as classes derivadas de **Panel** podem herdar o comportamento da propriedade de conteúdo XAML. Se uma propriedade for designada a propriedade de conteúdo XAML, isso significa que a marcação XAML poderá omitir um elemento de propriedade ao especificar a propriedade na marcação e os valores serão definidos como filhos de marcação imediata (o "conteúdo"). Por exemplo, se você derivar uma classe chamada **CustomPanel** do **Panel** que não define um novo comportamento, ainda será possível usar essa marcação:

```XAML
<local:CustomPanel>
  <Button Name="button1"/>
  <Button Name="button2"/>
</local:CustomPanel>
```

Quando um analisador XAML lê esta marcação, [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) passa a ser conhecido por ser a propriedade de conteúdo XAML para todos os tipos derivados de [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511), de modo que o analisador adicionará os dois elementos [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) ao valor de [**UIElementCollection**](https://msdn.microsoft.com/library/windows/apps/br227633) da propriedade **Children**. A propriedade de conteúdo XAML facilita um relacionamento pai-filho simplificado na marcação XAML para uma definição de interface do usuário. Para obter mais informações sobre propriedades de conteúdo XAML e como as propriedades de coleção são populadas quando o XAML é analisado. consulte o [Guia de sintaxe XAML](https://msdn.microsoft.com/library/windows/apps/mt185596).

O tipo de conjunto que mantém o valor da propriedade [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) é a classe [**UIElementCollection**](https://msdn.microsoft.com/library/windows/apps/br227633). O **UIElementCollection** é um conjunto fortemente tipado que usa [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) como seu tipo de item forçado. O **UIElement** é um tipo de base herdado por centenas de tipos de elementos práticos de interface do usuário, por isso a aplicação de tipo aqui é deliberadamente solta. Mas isso reforça que não seria possível ter um [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) como um filho direto de um [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511), e isso geralmente significa que apenas os elementos que se esperam que sejam visíveis na interface do usuário e participem no layout serão identificados como elementos filho em um **Panel**.

Normalmente, um painel personalizado aceita qualquer elemento filho [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) por uma definição XAML apenas usando as características da propriedade [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) no estado em que se encontra. Como um cenário avançado, seria possível suportar ainda mais a verificação de tipo de elementos filho, ao iterar sobre o conjunto nas substituições de layout.

Além de loop através do conjunto [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) nas substituições, a lógica do painel também pode ser influenciada por `Children.Count`. Você pode ter lógicas que aloquem espaço, pelo menos em parte, com base no número de itens, em vez de tamanhos desejados e as outras características de itens individuais.

## <a name="overriding-the-layout-methods"></a>Substituindo os métodos de layout


O modelo básico para os métodos de substituição de layout ([**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) e [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)) é que eles devem percorrer através de todos os filhos e chamar o método de layout específico de cada elemento filho. O primeiro ciclo de layout começa quando o sistema de layout XAML define o visual para a janela de raiz. Como cada um dos pais invoca layout nos filhos, isso propaga uma chamada aos métodos de layout a todos os elementos de interface do usuário possível que são supostamente parte de um layout. No layout XAML, há duas fases: medição e, depois, organização.

Você não tem nenhum comportamento de método de layout interno para [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) e [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) da classe de base [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511). Os itens no [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) não renderizarão automaticamente como parte da árvore visual XAML. Você decide se quer tornar os itens conhecidos para o processo de layout, invocando os métodos de layout em cada um dos itens encontrados no **Children** através de um cálculo de layout dentro das implementações **MeasureOverride** e **ArrangeOverride**.

Não há nenhuma razão para chamar implementações de base nas substituições de layout, a menos que você tenha sua própria herança. Os métodos nativos para o comportamento de layout (se existirem) são executados de qualquer forma, e não chamar a implementação base das substituições não impedirá que o comportamento natural aconteça.

Durante o cálculo de medida, a lógica de layout consulta cada elemento filho para o tamanho desejado, chamando o método [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) no elemento filho. Chamar o método **Measure** estabelece o valor para a propriedade [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921). O valor de retorno [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) é o tamanho desejado para o próprio painel.

Durante o cálculo de organização, as posições e os tamanhos de elementos filhos são determinados no espaço x-y e a composição de layout é preparada para renderização. O código deve chamar o [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) em cada elemento filho em [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514), de modo que o sistema de layout detecte que o elemento pertence ao layout. A chamada **Arrange** é um precursor da composição e renderização, que informa o sistema de layout para onde esse elemento vai e quando a composição é enviada para renderização.

Muitas propriedades e valores contribuem para a forma como a lógica de layout funcionará no tempo de execução. Uma maneira de pensar sobre o processo de layout é que os elementos sem filhos (geralmente o elemento mais profundamente aninhado na interface do usuário) são os únicos que podem finalizar as medições primeiro. Eles não têm nenhuma dependência de elementos filho que influenciem no tamanho desejado. Eles podem ter seus próprios tamanhos desejados, e esses tamanhos são sugestões até que o layout realmente seja feito. Então, o cálculo de medição continua subindo a árvore visual até que o elemento raiz tenha suas medidas e todas as medidas possam ser finalizadas.

O layout candidato deve caber dentro da janela do aplicativo atual. Caso contrário, serão cortadas partes da interface do usuário. Os painéis são frequentemente o lugar onde a lógica de corte é determinada. A lógica do painel pode determinar qual o tamanho disponível de dentro da implementação [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) e pode ter que aplicar as restrições de tamanho nos filhos e dividir o espaço entre os filhos, para que tudo se encaixe da melhor forma possível. O resultado do layout é idealmente algo que use várias propriedades de todas as partes do layout, mas ainda se encaixe dentro da janela do aplicativo. Isso exige uma boa implementação para a lógica de layout dos painéis, e também um design de interface criterioso por parte de qualquer código do aplicativo que cria uma interface do usuário usando esse painel. Nenhum design do painel ficará bem se o design geral da interface do usuário incluir mais elementos filhos do que possivelmente caberiam no aplicativo.

Uma grande parte do que faz o trabalho de sistema de layout funcionar é que qualquer elemento com base em [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) já tem um pouco do próprio comportamento inerente ao agir como um filho em um contêiner. Por exemplo, existem várias APIs de **FrameworkElement** que informam o comportamento de layout ou são necessárias para fazer o trabalho de layout. Como por exemplo:

-   [**DesiredSize** ](https://msdn.microsoft.com/library/windows/apps/br208921) (na verdade, um [ **UIElement** ](https://msdn.microsoft.com/library/windows/apps/br208911) propriedade)
-   [**ActualHeight** ](https://msdn.microsoft.com/library/windows/apps/br208707) e [ **ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709)
-   [**Altura** ](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) e [ **largura**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width)
-   [**Margem**](https://msdn.microsoft.com/library/windows/apps/br208724)
-   [**LayoutUpdated** ](https://msdn.microsoft.com/library/windows/apps/br208722) evento
-   [**HorizontalAlignment** ](https://msdn.microsoft.com/library/windows/apps/br208720) e [ **VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208749)
-   [**ArrangeOverride** ](https://msdn.microsoft.com/library/windows/apps/br208711) e [ **MeasureOverride** ](https://msdn.microsoft.com/library/windows/apps/br208730) métodos
-   [**Organizar** ](https://msdn.microsoft.com/library/windows/apps/br208914) e [ **medida** ](https://msdn.microsoft.com/library/windows/apps/br208952) métodos: elas têm implementações nativas definidas no [ **FrameworkElement** ](https://msdn.microsoft.com/library/windows/apps/br208706) nível, que lidar com a ação de layout de nível de elemento

## <a name="measureoverride"></a>**MeasureOverride**


O método [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) tem um valor de retorno que é usado pelo sistema de layout como o ponto de partida [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) para o próprio painel, quando o método [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) é chamado no painel pelo pai no layout. As escolhas lógicas dentro do método são tão importantes quanto o que ele retorna, e a lógica muitas vezes influencia o valor retornado.

Todas as implementações [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) devem fazer um loop através de [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514), e chamar o método [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) em cada elemento filho. Chamar o método **Measure** estabelece o valor para a propriedade [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921). Isso pode informar quanto espaço o próprio painel precisa, bem como a forma que o espaço é dividido entre os elementos ou dimensionados para um elemento filho específico.

Consulte a seguir a estrutura básica de um método [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730):

```CSharp
protected override Size MeasureOverride(Size availableSize)
{
    Size returnSize; //TODO might return availableSize, might do something else
     
    //loop through each Child, call Measure on each
    foreach (UIElement child in Children)
    {
        child.Measure(new Size()); // TODO determine how much space the panel allots for this child, that's what you pass to Measure
        Size childDesiredSize = child.DesiredSize; //TODO determine how the returned Size is influenced by each child's DesiredSize
        //TODO, logic if passed-in Size and net DesiredSize are different, does that matter?
    }
    return returnSize;
}
```

Os elementos muitas vezes têm um tamanho natural no momento em que estão prontos para o layout. Após o cálculo de medida, [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) pode indicar o tamanho natural, se *availableSize* que você calculou para [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) for menor. Se o tamanho natural for maior que *availableSize* que você calculou para **Measure**, **DesiredSize** será restrito a *availableSize*. É assim que a implementação interna de **Measure** se comporta, e as substituições de layout devem levar em conta esse comportamento.

Alguns elementos não têm tamanho natural, porque eles têm valores **Auto** para [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) e [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width). Depois, esses elementos usarão o *availableSize* completo, porque é isso que um valor **Auto** representa: dimensionar o elemento para o tamanho máximo disponível, que o pai de layout de imediato comunica chamando [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) com *availableSize*. Na prática, sempre há alguma medida dimensionada para uma interface do usuário (mesmo que o que é a janela de nível superior.) Eventualmente, a passagem de medida resolve todos os **automática** valores para as restrições de pai e todos os **automática** medidas reais de obter elementos de valor (que você pode obter ao verificar [  **ActualWidth** ](https://msdn.microsoft.com/library/windows/apps/br208709) e [ **ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707), após a conclusão do layout).

É permitido calcular um tamanho para [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) que tenha ao menos uma dimensão infinita, para indicar que o painel pode tentar se dimensionar para caber todo o seu conteúdo. Cada elemento filho que está sendo medido define seu valor [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) usando seu tamanho natural. Em seguida, durante o cálculo de organização, o painel normalmente organiza usando esse tamanho.

Elementos de texto como [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) têm um [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) calculado e [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) com base em sua cadeia de caracteres de texto e propriedades de texto, mesmo que nenhum valor [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) ou [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) esteja definido, e essas dimensões devem ser respeitadas pela lógica de painel. Cortar texto é uma experiência de interface do usuário especificamente ruim.

Mesmo que a implementação não utilize as medidas de tamanho desejadas, é melhor chamar o método [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) em cada elemento filho, porque há comportamentos internos e nativos que são acionados pela chamada de **Measure**. Para que um elemento participe do layout, cada elemento filho deve ter **Measure** chamada nele durante o cálculo de medição e o método [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) chamado nele durante o cálculo de organização. A chamada desses métodos define sinalizadores internos no objeto e preenche valores (como a propriedade [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921)) que a lógica de layout do sistema precisa ao criar a árvore visual e renderiza a interface do usuário.

O valor de retorno [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) baseia-se na lógica do painel interpretando o [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) ou outras considerações de tamanho para cada um dos elementos filho em [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) quando [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) é chamado neles. O que fazer com valores **DesiredSize** dos filhos e o modo como o valor de retorno **MeasureOverride** deve usá-los depende da interpretação da própria lógica. Normalmente, você não adiciona os valores sem modificação, porque a entrada de **MeasureOverride** muitas vezes é um tamanho fixo disponível que está sendo sugerido pelo pai do painel. Se esse tamanho for excedido, o próprio painel poderá ser cortado. Normalmente, você vai comparar o tamanho total dos filhos ao tamanho disponível do painel e fazer ajustes se necessário.

### <a name="tips-and-guidance"></a>Dicas e tutoriais

-   Idealmente, um painel personalizado deve ser adequado para ser o primeiro verdadeiro visual em uma composição de interface do usuário, talvez em um nível imediatamente abaixo de [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503), [**UserControl**](https://msdn.microsoft.com/library/windows/apps/br227647) ou outro elemento que seja a raiz da página XAML. Em implementações [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730), a entrada [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) não é geralmente retornada sem examinar os valores. Se o retorno **Size** tiver um valor **Infinity** nele, isso poderá acionar exceções em tempo de execução de lógica do layout. Um valor **Infinity** pode vir da janela de aplicativo principal, a qual é deslocável e, por isso, não tem uma altura máxima. Outros conteúdos de rolagem podem ter o mesmo comportamento.
-   Outro erro comum em implementações [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) é retornar um novo padrão [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) (os valores de altura e largura são 0). Você pode começar com esse valor, que pode até ser o valor correto se o painel determinar que nenhum dos filhos deve ser renderizado. Mas um **Size** padrão fará com que seu painel não seja dimensionado corretamente pelo host. Ele não solicita espaço na interface do usuário e, portanto, fica sem espaço e não renderiza. Se não fosse o caso, o código do painel poderia funcionar bem, mas o painel ou seu conteúdo ainda não será exibido se ele estiver sendo composto com altura zero e largura zero.
-   Dentro das substituições, evite a tentação de lançar elementos filho para [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) e use propriedades que são calculadas como um resultado de layout, especialmente [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) e [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707). Para a maioria dos cenários comuns, é possível basear a lógica no valor do [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) do filho e você não precisará de nenhuma das propriedades [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) ou [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) relacionadas de um elemento filho. Para os casos especializados em que o tipo de elemento é conhecido e tem informações adicionais, por exemplo, o tamanho natural de um arquivo de imagem, você pode usar as informações especializadas do elemento, pois não é um valor que está ativamente sendo alterado pelos sistemas de layout. Incluir propriedades calculadas de layout como parte da lógica de layout aumenta substancialmente o risco de definir um loop de layout não intencional. Esses loops causam uma condição em que um layout válido não pode ser criado e o sistema pode lançar uma [**LayoutCycleException**](https://msdn.microsoft.com/library/windows/apps/hh673799) se o loop não for recuperável.
-   Os painéis normalmente dividem o espaço disponível entre vários elementos filhos, embora a divisão exata do espaço varie. Por exemplo, [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) implementa a lógica de layout que usa seus valores [**RowDefinition**](https://msdn.microsoft.com/library/windows/apps/br227606) e [**ColumnDefinition**](https://msdn.microsoft.com/library/windows/apps/br209324) para dividir o espaço nas células **Grid**, suportando os valores de dimensionamento de estrela e pixel. Se eles forem valores de pixel, o tamanho disponível para cada filho já é conhecido, então isso é o que será passado como tamanho de entrada para um estilo de grade [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952).
-   Os painéis podem introduzir um espaço reservado para preenchimento entre itens. Se você fizer isso, assegure-se de expor as medições como uma propriedade que é diferente de[**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724) ou de qualquer propriedade **Padding**.
-   Os elementos podem ter valores para a suas propriedades [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) e [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) com base em um cálculo de layout anterior. Se os valores forem alterados, o código de interface do usuário do aplicativo poderá colocar manipuladores para [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/br208722) em elementos se houver lógica especial a ser executada, mas a lógica de painel normalmente não precisa verificar se há alterações com a manipulação de eventos. O sistema de layout já está fazendo as determinações ao executar novamente o layout porque uma propriedade relevante de layout mudou o valor foi alterado e um [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) ou [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) de painel é chamado automaticamente nas circunstâncias adequadas.

## <a name="arrangeoverride"></a>**ArrangeOverride**


O método [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) tem um valor de retorno [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) que é usado pelo sistema de layout ao renderizar o próprio painel, quando o método [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) é chamado no painel pelo pai no layout. É comum que a entrada *finalSize* e o **ArrangeOverride** que retornaram **Size** sejam os mesmos. Se não forem, isso significa que o painel está tentando fazer um tamanho diferente do tamanho que os outros participantes do layout afirmam estar disponível. O tamanho final baseou-se em ter executado anteriormente o cálculo de medição de layout através do código de painel. É por isso que retornar um tamanho diferente não é comum, pois significa que você está ignorando deliberadamente a lógica de medição.

Não retornam um [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) com um componente **Infinity**. Tentar utilizar tal **Size** gera uma exceção de layout interno.

Todas as implementações de [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) devem fazer um loop por meio de [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514), e chamar o método [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) em cada elemento filho. Como [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952), **Arrange** não tem valor de retorno. Ao contrário de **Measure**, nenhuma propriedade calculada fica definida como um resultado (no entanto, o elemento em questão normalmente dispara um evento [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/br208722)).

Aqui está um esqueleto básico de um método [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711):

```CSharp
protected override Size ArrangeOverride(Size finalSize)
{
    //loop through each Child, call Arrange on each
    foreach (UIElement child in Children)
    {
        Point anchorPoint = new Point(); //TODO more logic for topleft corner placement in your panel
       // for this child, and based on finalSize or other internal state of your panel
        child.Arrange(new Rect(anchorPoint, child.DesiredSize)); //OR, set a different Size 
    }
    return finalSize; //OR, return a different Size, but that's rare
}
```

O cálculo de organização de layout pode acontecer sem ser precedido por um cálculo de medição. No entanto, isso só acontece quando o sistema de layout tiver determinado que nenhuma propriedade mudou nada que afetasse as medições anteriores. Por exemplo, se um alinhamento for alterado, não será necessário medir novamente esse elemento específico porque o seu [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) não mudaria quando a escolha de alinhamento for alterada. Por outro lado, se [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) alterar qualquer elemento em um layout, será necessário um novo cálculo de medição. O sistema de layout detecta automaticamente mudanças de medição verdadeiras e invoca o cálculo de medição novamente, e em seguida, executa outro cálculo de organização.

A entrada para [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) toma um valor [**Rect**](https://msdn.microsoft.com/library/windows/apps/br225994). A maneira mais comum de construir este **Rect** é usar o construtor que tem uma entrada [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) e uma entrada [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995). O **Point** é o ponto onde o canto superior esquerdo da caixa delimitadora para o elemento deve ser colocado. O **Size** é a dimensão usada para renderizar esse elemento específico. Você muitas vezes usa o [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) para esse elemento como valor **Size**, porque estabelece o **DesiredSize** para todos os elementos envolvidos no layout foi a finalidade do cálculo de medição de layout. (O cálculo de medição determina todo o dimensionamento dos elementos de forma iterativa, de modo que o sistema de layout possa otimizar como os elementos são colocados uma vez que inicie o cálculo de organização.)

O que normalmente varia entre implementações de [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) é a lógica pela qual o painel determina o componente [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) de como ele organiza a cada filho. Um painel de posicionamento absoluto, como o [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267), usa a informação de posicionamento explícito que ele toma de cada elemento através de valores de [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) e [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/hh759772). Um painel de divisão de espaço como [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) teria operações matemáticas que dividiam o espaço disponível para as células, e cada célula teria um valor de x-y para onde o conteúdo deve ser colocado e organizado. Um painel adaptável como [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635) pode estar se expandindo para caber o conteúdo na dimensão de orientação.

Há ainda influências de posicionamento adicionais nos elementos no layout, além do que você controla diretamente e calcula para [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914). Isso é proveniente da implementação nativa interna de **Arrange** que é comum a todos os tipos derivados de [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) e aumentados por outros tipos, como elementos de texto. Por exemplo, os elementos podem ter margem e alinhamento, e alguns podem ter preenchimento. Essas propriedades podem interagir. Para obter mais informações, consulte [Alinhamento, margem e preenchimento](alignment-margin-padding.md).

## <a name="panels-and-controls"></a>Controles e painéis


Evite colocar a funcionalidade em um painel personalizado que, em vez disso, deve ser criado como um controle personalizado. A função de um painel é apresentar qualquer conteúdo de elemento filho que exista dentro dele, como uma função de layout que acontece automaticamente. O painel pode adicionar decorações ao conteúdo (semelhante à forma como o [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) adiciona a borda em torno do elemento que apresenta), ou executar outros ajustes relacionados ao layout, como preenchimento. Mas isso é o mais longe que você deve ir ao estender a saída de árvore visual para além de relatórios e uso de informações dos filhos.

Se houver qualquer interação que seja acessível ao usuário, será necessário criar um controle personalizado, não um painel. Por exemplo, um painel não deve adicionar visores de rolagem ao conteúdo que apresenta, mesmo que o objetivo seja impedir o corte, porque as barras de rolagem, polegares e assim por diante são peças de controle interativos. (O conteúdo pode ter barras de rolagem, mas será necessário deixar isso para a lógica do filho. Não force-lo adicionando a rolagem como uma operação de layout.) Você pode criar um controle e também criar um painel personalizado que desempenha um papel importante na árvore visual desse controle, quando se trata de apresentar o conteúdo desse controle. Mas o controle e o painel devem ser objetos de código distintos.

Uma razão pela qual a distinção entre controle e painel é importante é a Automação da Interface do Usuário da Microsoft e a acessibilidade. Os painéis oferecem um comportamento de layout visual, não um comportamento lógico. Como um elemento da interface do usuário parece visualmente não ser um aspecto de interface do usuário que seja normalmente importante para os cenários de acessibilidade. Acessibilidade é expor as partes de um aplicativo que sejam logicamente importantes para entender uma interface do usuário. Quando a interação é necessária, os controles devem expor as possibilidades de interação à infraestrutura de Automação da Interface do Usuário. Para obter mais informações, consulte [Pares de automação personalizados](https://msdn.microsoft.com/library/windows/apps/mt297667).

## <a name="other-layout-api"></a>Outra API de layout


Existem outras APIs que fazem parte do sistema de layout, mas não são declaradas pelo [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511). Você pode utilizá-las em uma implementação do painel ou em um controle personalizado que utiliza painéis.

-   [**UpdateLayout**](https://msdn.microsoft.com/library/windows/apps/br208989), [ **InvalidateMeasure**](https://msdn.microsoft.com/library/windows/apps/br208930), e [ **InvalidateArrange** ](https://msdn.microsoft.com/library/windows/apps/br208929) são métodos que Inicie uma passagem de layout. **InvalidateArrange** pode não disparar um cálculo de medição, mas os outros dois fazem isso. Nunca chame esses métodos de dentro de um método de substituição de layout, pois é quase certo que eles vão causar um loop de layout. O código de controle normalmente também não precisa chamá-los. A maioria dos aspectos do layout é acionada automaticamente ao detectar alterações nas propriedades de layout definidas pelo quadro como [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) e assim por diante.
-   [**LayoutUpdated** ](https://msdn.microsoft.com/library/windows/apps/br208722) é um evento que é acionado quando algum aspecto do layout do elemento é alterado. Isso não é específico para painéis; o evento é definido por [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706).
-   [**SizeChanged** ](https://msdn.microsoft.com/library/windows/apps/br208742) é um evento que é acionado somente depois de passagens de layout são finalizados e indica que [ **ActualHeight** ](https://msdn.microsoft.com/library/windows/apps/br208707) ou [ **ActualWidth**  ](https://msdn.microsoft.com/library/windows/apps/br208709) foram alteradas como resultado. Esse é outro evento [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706). Há casos em que [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/br208722) dispara, mas **SizeChanged** não dispara. Por exemplo o conteúdo interno pode ser reorganizado, mas o tamanho dos elementos não é alterado.


## <a name="related-topics"></a>Tópicos relacionados

**Referência**
* [**FrameworkElement.ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)
* [**FrameworkElement.MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730)
* [**Painel**](https://msdn.microsoft.com/library/windows/apps/br227511)

**Conceitos**
* [Alinhamento, margem e preenchimento](alignment-margin-padding.md)
