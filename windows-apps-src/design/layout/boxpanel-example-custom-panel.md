---
author: muhsinking
Description: Learn to write code for a custom Panel class, implementing ArrangeOverride and MeasureOverride methods, and using the Children property.
MS-HAID: dev\_ctrl\_layout\_txt.boxpanel\_example\_custom\_panel
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: BoxPanel, um exemplo de painel personalizado (aplicativos do Windows)
ms.assetid: 981999DB-81B1-4B9C-A786-3025B62B74D6
label: BoxPanel, an example custom panel
template: detail.hbs
op-migration-status: ready
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7d29b85c7ec3ec9ec0114a3a49dff834f859511e
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6184503"
---
# <a name="boxpanel-an-example-custom-panel"></a>BoxPanel, um exemplo de painel personalizado

 

Aprenda a escrever códigos para uma classe personalizada [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511), implementando os métodos [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) e [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) e usando a propriedade [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514). 

> **APIs Importantes**: [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511), [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711),[**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 

O exemplo de código mostra uma implementação de painel personalizado, mas não vamos perder muito tempo explicando os conceitos de layout que influenciam como você pode personalizar cenários de layouts diferentes. Se quiser mais informações sobre esses conceitos de layout e como eles se aplicam ao seu cenário de layout específico, consulte [Visão geral de painéis personalizados XAML](custom-panels-overview.md).

Um *painel* é um objeto que oferece um comportamento de layout para os elementos filhos que contém quando o sistema de layout XAML é executado e a interface do usuário do seu aplicativo é renderizada. Você pode definir painéis personalizados para layouts XAML derivando uma classe personalizado da classe [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511). Você fornece o comportamento para o seu painel ao substituir os métodos [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) e [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) fornecendo lógica que mede e organiza os elementos filhos. Este exemplo é derivado de **Panel**. Quando você inicia pelos métodos **Panel**, **ArrangeOverride** e **MeasureOverride**, não há um comportamento de inicialização. Seu código fornece o gateway pelo qual os elementos filhos passam a ser reconhecidos pelo sistema de layout XAML e são renderizados na interface do usuário. Assim, é muito importante que o seu código seja responsável por todos os elementos filhos e siga os padrões que o sistema de layout espera.

## <a name="your-layout-scenario"></a>Seu cenário de layout

Ao definir um painel personalizado, você está definindo um cenário de layout.

Um cenário de layout é expresso por meio de:

-   O que o painel fará quando possuir elementos filhos
-   Quando o painel tem restrições em seu próprio espaço
-   Como a lógica do painel determina todas as medidas, colocações, posições e dimensionamentos que acabam resultando em um layout de interface do usuário renderizado de filhos.

Com isso em mente, o `BoxPanel` mostrado aqui é para um cenário específico. A fim de manter sobretudo o código neste exemplo, ainda não vamos explicar o cenário em detalhes, mas, em vez disso, vamos nos concentrar nas etapas necessárias e nos padrões de codificação. Se você quiser saber mais sobre o cenário primeiro, vá direto para ["O cenário do `BoxPanel`"](#the-scenario-for-boxpanel) e depois volte para obter o código.

## <a name="start-by-deriving-from-panel"></a>Inicie derivando o **Panel**

Inicie derivando uma classe personalizada de [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511). Provavelmente, a forma mais fácil de fazer isso é definir um arquivo de código separado para essa classe, usando as opções de contexto **Adicionar** | **Novo Item** | **Classe** do menu de um projeto do **Gerenciador de Soluções** no Microsoft Visual Studio. Nomeie a classe (e arquivo) como `BoxPanel`.

O arquivo de modelo para uma classe não inicia com muitas instruções **using** porque não é específica para aplicativos da Plataforma Universal do Windows (UWP). Então, primeiro, adicione as instruções **using**. O arquivo do modelo também inicia com poucas instruções **using** das quais você, provavelmente, não precisa e pode deletar. Aí vai uma lista sugerida de instruções **using** que podem resolver tipos de que você precisará para o código do painel personalizado típico.

```CSharp
using System;
using System.Collections.Generic; // if you need to cast IEnumerable for iteration, or define your own collection properties
using Windows.Foundation; // Point, Size, and Rect
using Windows.UI.Xaml; // DependencyObject, UIElement, and FrameworkElement
using Windows.UI.Xaml.Controls; // Panel
using Windows.UI.Xaml.Media; // if you need Brushes or other utilities
```

Agora que você pode resolver [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511), transforme-o na classe base do `BoxPanel`. Também deixe `BoxPanel` público:

```CSharp
public class BoxPanel : Panel
{
}
```

No nível da classe, defina alguns valores **int** e **double** que serão compartilhados por muitas das suas funções de lógica, mas que não precisarão ser expostos como APIs públicas. No exemplo, eles são chamados: `maxrc`, `rowcount`, `colcount`, `cellwidth`, `cellheight`, `maxcellheight`, `aspectratio`.

Depois de fazer isso, o arquivo do código completo fica assim (removendo os comentários sobre **using**, agora que você sabe por que os temos):

```CSharp
using System;
using System.Collections.Generic;
using Windows.Foundation;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

public class BoxPanel : Panel 
{
    int maxrc, rowcount, colcount;
    double cellwidth, cellheight, maxcellheight, aspectratio;
}
```

A partir de agora, estaremos mostrando uma definição de membro por vez, seja ele uma substituição de método ou algo que forneça suporte como uma propriedade de dependência. Você pode adicioná-los ao esqueleto acima em qualquer ordem, e não mostraremos as instruções **using** ou a definição do escopo de classe novamente nos trechos até que mostremos o código final.

## **<a name="measureoverride"></a>MeasureOverride**


```CSharp
protected override Size MeasureOverride(Size availableSize)
{
    Size returnSize;
    // Determine the square that can contain this number of items.
    maxrc = (int)Math.Ceiling(Math.Sqrt(Children.Count));
    // Get an aspect ratio from availableSize, decides whether to trim row or column.
    aspectratio = availableSize.Width / availableSize.Height;

    // Now trim this square down to a rect, many times an entire row or column can be omitted.
    if (aspectratio > 1)
    {
        rowcount = maxrc;
        colcount = (maxrc > 2 && Children.Count < maxrc * (maxrc - 1)) ? maxrc - 1 : maxrc;
    } 
    else 
    {
        rowcount = (maxrc > 2 && Children.Count < maxrc * (maxrc - 1)) ? maxrc - 1 : maxrc;
        colcount = maxrc;
    }

    // Now that we have a column count, divide available horizontal, that's our cell width.
    cellwidth = (int)Math.Floor(availableSize.Width / colcount);
    // Next get a cell height, same logic of dividing available vertical by rowcount.
    cellheight = Double.IsInfinity(availableSize.Height) ? Double.PositiveInfinity : availableSize.Height / rowcount;
           
    foreach (UIElement child in Children)
    {
        child.Measure(new Size(cellwidth, cellheight));
        maxcellheight = (child.DesiredSize.Height > maxcellheight) ? child.DesiredSize.Height : maxcellheight;
    }
    return LimitUnboundedSize(availableSize);
}
```

O padrão necessário de uma implementação [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) é o loop em cada elemento [**Panel.Children**](https://msdn.microsoft.com/library/windows/apps/br227514). Sempre chame o método [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) em cada um desses elementos. **Measure** tem um parâmetro de tipo [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995). O que está sendo passado aqui é o tamanho que o seu painel está se comprometendo a ter para aquele elemento filho em particular. Então, antes que possa fazer o loop e começar a chamar o **Measure**, você precisa saber quanto espaço cada célula pode dedicar. Do próprio método **MeasureOverride**, você tem o valor *availableSize*. Esse é o tamanho que pai do painel usou quando chamou o **Measure**, que era o gatilho para este **MeasureOverride** ser chamado. Então uma lógica típica é planejar um esquema em que cada elemento filho divide o espaço do *availableSize* geral do painel. Em seguida, você passa cada divisão de tamanho para o **Measure** de cada elemento filho.

É simples como o `BoxPanel` divide o tamanho: ele divide o espaço em um número de caixas controlado, em grande parte, pelo número de itens. As caixas são dimensionadas com base na contagem de linhas e colunas e no tamanho disponível. Às vezes, não é necessária uma linha ou coluna de um quadrado, de modo que ela é descartada e o painel se torna um retângulo em vez de quadrado em termos de sua linha: a proporção da coluna. Para obter mais informações sobre como essa lógica foi atingida, avance para ["O cenário para BoxPanel"](#the-scenario-for-boxpanel).

Então, o que o cálculo da medida faz? Ele define um valor para a propriedade apenas de leitura [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) em cada elemento onde o [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) foi chamado. Ter um valor **DesiredSize** é provavelmente importante quando você organizar o cálculo, porque o **DesiredSize** comunica qual tamanho deve ser ou pode ser durante a organização e a renderização final. Mesmo que você não use **DesiredSize** na sua própria lógica, o sistema ainda precisa dele.

É possível que esse painel seja usado quando o componente altura do *availableSize* for ilimitado. Se for verdade, o painel não precisa ter uma altura conhecida para ser dividida. Nesse caso, a lógica para o cálculo da medida informa cada filho que ele ainda não tem uma altura ilimitada. Ele o faz passando [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) para a chamada [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) para os filhos cujo [**Size.Height**](https://msdn.microsoft.com/library/windows/apps/hh763910) é infinito. Isso é legal. Quando o **Measure** for chamado, a lógica é que o [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) seja definido como o mínimo disso: o que foi passado para **Measure**, ou o tamanho natural do elemento de fatores como [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) e [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) explicitamente definidos.

> [!NOTE]
> A lógica interna do [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635) também tem esse comportamento: **StackPanel** passa um valor de dimensão infinita para [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) em filhos, indicando que não há restrição nos filhos na dimensão de orientação. **StackPanel** costuma dimensionar a si próprio para acomodar todos os filhos em uma pilha que cresce naquela dimensão.

Entretanto, o painel em si não pode devolver um [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) com um valor infinito de [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730); o que se torna uma exceção durante o layout. Então, parte da lógica é descobrir a altura máxima que qualquer filho solicitar e usar tal altura como a altura da célula no caso de isso não vir nas próprias restrições de tamanho do painel. Aqui está a função auxiliar `LimitUnboundedSize` que foi referenciada no código anterior, que pega a altura máxima da célula e a usa para dar ao painel uma altura finita em retorno, além de assegurar que `cellheight` seja um número finito antes de o cálculo de organização ser iniciado:

```CSharp
// This method is called only if one of the availableSize dimensions of measure is infinite.
// That can happen to height if the panel is close to the root of main app window.
// In this case, base the height of a cell on the max height from desired size
// and base the height of the panel on that number times the #rows.

Size LimitUnboundedSize(Size input)
{
    if (Double.IsInfinity(input.Height))
    {
        input.Height = maxcellheight * colcount;
        cellheight = maxcellheight;
    }
    return input;
}
```

## **<a name="arrangeoverride"></a>ArrangeOverride**

```CSharp
protected override Size ArrangeOverride(Size finalSize)
{
     int count = 1
     double x, y;
     foreach (UIElement child in Children)
     {
          x = (count - 1) % colcount * cellwidth;
          y = ((int)(count - 1) / colcount) * cellheight;
          Point anchorPoint = new Point(x, y);
          child.Arrange(new Rect(anchorPoint, child.DesiredSize));
          count++;
     }
     return finalSize;
}
```

O padrão necessário de uma implementação [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) é o loop em cada elemento [**Panel.Children**](https://msdn.microsoft.com/library/windows/apps/br227514). Sempre chame o método [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) em cada um desses elementos.

Observe que não há tantos cálculos como em [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730); o que é comum. O tamanho dos filhos já é conhecido pela própria lógica **MeasureOverride** do painel ou pelo valor [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) de cada filho definido durante o cálculo da medida. Entretanto, ainda precisamos decidir a localização onde cada filho aparecerá no painel. Em um painel típico, cada filho deve renderizar em uma posição diferente. Um painel que cria elementos de sobreposição não é recomendável para cenários comuns (embora não esteja fora de questão criar painéis com sobreposições intencionais, se esse for realmente o cenário pretendido).

Esse painel é organizado pelo conceito de linhas e colunas. O número de linhas e colunas já foi calculado (foi necessário para medir). Agora, o formato das linhas e colunas mais os tamanhos conhecidos de cada célula contribuem com a lógica de definir uma posição de renderização (o `anchorPoint`) para cada elemento que o painel contém. Aquele [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)junto com o [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) já conhecido da medida, são usados como os dois componentes que constroem um [**Rect**](https://msdn.microsoft.com/library/windows/apps/br225994). **Rect** é o tipo de entrada para [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914).

Às vezes, painéis precisam recortar seu conteúdo. Se o fizeram, o tamanho recortado é o tamanho presente em [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921), porque a lógica [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) o define como o mínimo do que foi passado para **Measure** ou outros fatores de tamanho natural. Então não é comum precisar verificar o recorte durante [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914); o recorte acontece com base no **DesiredSize** ser passado em cada chamada **Arrange**.

Nem sempre você precisa de uma contagem enquanto passa pelo loop se todas as informações necessárias para definir a posição de renderização são conhecidas por outros meios. Por exemplo, na lógica de layout [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267), a posição da coleção [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) não importa. Todas as informações necessárias em um **Canvas** é conhecida pela leitura dos valores [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) e [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/hh759772) de filhos como parte da lógica de organização. Acontece que a lógica do `BoxPanel` precisa de uma contagem para comparar o *colcount* para que se saiba quando começar uma nova linha e deslocar o valor *y*.

É comum que a entrada *finalSize* e o [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) que você retorna de uma implementação [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) sejam os mesmos. Para saber o porquê, consulte a seção "**ArrangeOverride**" de [XAML custom panels overview](custom-panels-overview.md).

## <a name="a-refinement-controlling-the-row-vs-column-count"></a>Um refinamento: controlando a contagem de linhas x colunas

Você pode compilar e usar esse painel exatamente como ele é agora. Entretanto, vamos adicionar mais um refinamento. No código que acabamos de mostrar, a lógica coloca a linha extra ou a coluna do lado que é mais longo na taxa de proporção. Mas para um controle maior sobre os formatos das células, pode ser aconselhável escolher um conjunto 4x3 de células em vez de um 3x4, mesmo que a taxa de proporção do próprio painel seja "retrato". Então, adicionaremos uma propriedade de dependência adicional que o cliente do painel pode definir para controlar tal comportamento. Aqui está a definição de propriedade de dependência, que é bem básica:

```CSharp
public static readonly DependencyProperty UseOppositeRCRatioProperty =
   DependencyProperty.Register("UseOppositeRCRatio", typeof(bool), typeof(BoxPanel), null);

public bool UseSquareCells
{
    get { return (bool)GetValue(UseOppositeRCRatioProperty); }
    set { SetValue(UseOppositeRCRatioProperty, value); }
}
```

E aqui está como o uso do `UseOppositeRCRatio` afeta a lógica de medida. O que isso de fato está fazendo é mudando como `rowcount` e `colcount` derivam de `maxrc` e a taxa de proporção verdadeira, e há diferenças de tamanho correspondentes para cada célula por causa disso. Quando `UseOppositeRCRatio` é **true**, ele inverte o valor da taxa de proporção verdadeira antes de usá-la nas contagens de linha e coluna.

```CSharp
if (UseOppositeRCRatio) { aspectratio = 1 / aspectratio;}
```

## <a name="the-scenario-for-boxpanel"></a>O cenário para BoxPanel

O cenário específico para `BoxPanel` é que ele é um painel em que um dos principais determinantes da forma de dividir o espaço é conhecer o número de itens filhos e dividir o espaço conhecido disponível do painel. Os painéis têm naturalmente formas retangulares. Muitos painéis operam dividindo o espaço retangular em vários retângulos, ou seja, o que o [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) faz para suas células. No caso de **Grid**'o tamanho das células é definido pelos valores [**ColumnDefinition**](https://msdn.microsoft.com/library/windows/apps/br209324) e [**RowDefinition**](https://msdn.microsoft.com/library/windows/apps/br227606), e os elementos declaram a célula exata onde entram com as propriedades anexadas [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/hh759795) e [**Grid.Column**](https://msdn.microsoft.com/library/windows/apps/hh759774). Obter um layout bom de um **Grid** costuma exigir o conhecimento do número de elementos filhos antes, para que haja células suficientes e cada elemento filho define suas propriedades anexadas para caber em sua própria célula.

Mas e se o número de filhos for dinâmico? Isso certamente é possível; o seu código de aplicativo pode adicionar itens às coleções, em resposta a qualquer condição dinâmica de tempo de execução que você considere importante o suficiente para valer a atualização de sua interface do usuário. Se você estiver usando vinculação de dados para dar suporte a coleções/objetos de negócios, o recebimento dessas atualizações e a atualização da interface do usuário serão feitos automaticamente, e muitas vezes essa é a técnica preferida de dados (veja [Vinculação de dados em detalhes](https://msdn.microsoft.com/library/windows/apps/mt210946)).

Mas nem todos os cenários de aplicativos passam por vinculação de dados. Às vezes, é preciso criar novos elementos da interface do usuário no tempo de execução e torná-los visíveis. `BoxPanel` é para este cenário. Um mudança no número de itens filhos não é problema para o `BoxPanel` porque ele está usando a contagem de filhos nos cálculos, e ajusta tanto os elementos filhos novos como os existentes em um novo layout, portanto todos eles se encaixem.

Uma situação antecipada para estender mais o `BoxPanel` (não mostrado aqui) poderia ser tanto acomodar filhos dinâmicos quanto usar o [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) de um filho como um fator mais forte para o dimensionamento de células individuais. Essa situação pode usar tamanhos de linha e coluna variáveis ou formatos sem ser em grade para que haja menos espaço "desperdiçado". Isso exige uma estratégia de como vários retângulos de vários tamanhos e proporções podem caber em um retângulo, mantendo a estética e o menor tamanho. `BoxPanel` não faz isso; ele usa uma técnica mais simples para dividir o espaço. `BoxPanel`A técnica é determinar o número mínimo de quadrados que é maior do que a contagem de filhos. Por exemplo, 9 itens caberiam em um quadrado 3x3. 10 itens exigem um quadrado 4x4. Entretanto, costuma ser possível encaixar itens removendo uma linha ou coluna no começo do quadrado, para economizar espaço. No exemplo contagem=10, que se encaixa em um retângulo de 4x3 ou 3x4.

Você pode se perguntar por que o painel não escolheria 5x2 para 10 itens, pois encaixaria o número de itens perfeitamente. Mas, na prática, painéis são dimensionados como retângulos que raramente têm uma taxa de proporção bem orientada. A técnica de quadrados mínimos é uma maneira de influenciar a lógica de dimensionamento para funcionar bem com formas de layout típicas e não incentivar o dimensionamento onde as formas de células obtêm proporções estranhas.

## <a name="related-topics"></a>Tópicos relacionados

**Referência**

* [**FrameworkElement.ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)
* [**FrameworkElement.MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730)
* [**Painel**](https://msdn.microsoft.com/library/windows/apps/br227511)

**Conceitos**

* [Alinhamento, margem e preenchimento](alignment-margin-padding.md)
