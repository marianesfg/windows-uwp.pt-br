---
Description: Você pode definir um layouts anexado para uso com contêineres, como o controle ItemsRepeater.
title: AttachedLayout
label: AttachedLayout
template: detail.hbs
ms.date: 03/13/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6ff73b13acb5f5970bb79755b0bf5706fb12545a
ms.sourcegitcommit: c10d7843ccacb8529cb1f53948ee0077298a886d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58913986"
---
# <a name="attached-layouts"></a>Layouts anexados

Um contêiner (por exemplo, o painel) que delega a sua lógica de layout a outro objeto baseia-se no objeto de layout anexados para fornecer o comportamento de layout para seu filho elementos.  Um modelo de layout anexados fornece flexibilidade para alterar o layout de itens em tempo de execução de um aplicativo, ou mais compartilham facilmente os aspectos do layout entre diferentes partes da interface do usuário (por exemplo, os itens nas linhas de uma tabela que parecem ser alinhado dentro de uma coluna).

Neste tópico, abordamos o que está envolvido na criação de um anexado layout (virtualização e não virtualizando), os conceitos e as classes que você precisará entender, e as vantagens e desvantagens que você precisará considerar ao decidir entre eles.

| **Obter a biblioteca de interface do usuário do Windows** |
| - |
| Esse controle é incluído como parte da biblioteca de interface do usuário do Windows, um pacote do NuGet que contém os novos controles e recursos de interface do usuário para aplicativos UWP. Para obter mais informações, incluindo instruções de instalação, consulte o [visão geral da biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

> **APIs importantes**:

> * [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
> * [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)
> * [Layout](/uwp/api/microsoft.ui.xaml.controls.layout)
>     * [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
>     * [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)
> * [LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext)
>     * [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext)
>     * [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)
> * [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) (visualização)

## <a name="key-concepts"></a>Conceitos Principais

Execução do layout exige que duas perguntas respondidas para cada elemento:

1. O que ***tamanho*** esse elemento será?

2. O que será o ***posição*** desse elemento ser?

Sistema de layout do XAML, que responde a essas perguntas, é abordado brevemente como parte da discussão de [painéis personalizados](/windows/uwp/design/layout/custom-panels-overview).

### <a name="containers-and-context"></a>Contêineres e contexto

Do conceitualmente, XAML [painel](/uwp/api/windows.ui.xaml.controls.panel) preenche duas funções importantes do Framework:

1. Ele pode conter elementos filho e apresenta ramificações na árvore de elementos.
2. Ele se aplica a uma estratégia de layout específico para os filhos.

Por esse motivo, um painel em XAML era sinônimo de layout, mas tecnicamente falando, faz mais do que apenas o layout.

O [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater) também se comporta como o painel, mas, ao contrário do painel, ele não expõe uma propriedade de filhos que permitiria que os filhos de UIElement programaticamente adicionando ou removendo.  Em vez disso, o tempo de vida de seus filhos são gerenciadas automaticamente pelo framework para corresponder a uma coleção de itens de dados.  Embora ele não é derivado do painel, ele se comporta e é tratado pela estrutura, como um painel.

> [!NOTE]
> O [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) é um contêiner, derivado de painel, que delega a lógica para o anexo [Layout](/uwp/api/microsoft.ui.xaml.controls.layoutpanel.layout) objeto.  LayoutPanel está em *versão prévia* e está atualmente disponível apenas na *pré-lançamento* descarta do pacote WinUI.

#### <a name="containers"></a>Contêineres

Conceitualmente, [painel](/uwp/api/windows.ui.xaml.controls.panel) é um contêiner de elementos que também tem a capacidade de renderizar pixels para um [plano de fundo](/uwp/api/windows.ui.xaml.controls.panel.background).  Os painéis oferecem uma maneira de encapsular a lógica de layout comum em um pacote de fácil de usar.

O conceito de **anexados layout** faz a distinção entre as duas funções do contêiner e o layout mais clara.  Se o contêiner delega a sua lógica de layout a outro objeto seria chamamos esse objeto o layout anexado como visto no trecho a seguir. Contêineres que herdam [FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement), como o LayoutPanel automaticamente expõem as propriedades comuns que fornecem a entrada para o processo de layout do XAML (por exemplo, altura e largura).

```xaml
<LayoutPanel>
    <LayoutPanel.Layout>
        <UniformGridLayout/>
    </LayoutPanel.Layout>
    <Button Content="1"/>
    <Button Content="2"/>
    <Button Content="3"/>
</LayoutPanel>
```

Durante o processo de layout o contêiner se baseia no anexo *UniformGridLayout* para medir e organizar seus filhos.

#### <a name="per-container-state"></a>Estado por contêiner

Com um layout anexado, uma única instância do objeto de layout pode ser associada *muitos* contêineres, como no trecho a seguir; portanto, ele não deve depender ou referenciar diretamente o contêiner de host.  Por exemplo: 

```xaml
<!-- ... --->
<Page.Resources>
    <ExampleLayout x:Name="exampleLayout"/>
<Page.Resources>

<LayoutPanel x:Name="example1" Layout="{StaticResource exampleLayout}"/>
<LayoutPanel x:Name="example2" Layout="{StaticResource exampleLayout}"/>
<!-- ... --->
```

Para essa situação *ExampleLayout* devem considerar cuidadosamente o estado em que ele usa em seu cálculo de layout e onde esse estado é armazenado para evitar afetar o layout de elementos em um painel com outras.  Seria análogo a um painel personalizado cuja lógica MeasureOverride e ArrangeOverride depende dos valores de seu *estático* propriedades.

#### <a name="layoutcontext"></a>LayoutContext

A finalidade de [LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext) é lidar com esses desafios.  Ele fornece o layout anexado a capacidade de interagir com o contêiner de host, assim como a recuperação de elementos filho, sem introduzir uma dependência direta entre os dois. O contexto também permite que o layout armazenar qualquer estado que ele requer que possam estar relacionado a elementos-filho do contêiner.

Layouts simples e não virtualizando geralmente não é necessário manter qualquer estado, tornando-o um não problema. Um layout mais complexo, como grade, no entanto, pode optar por manter o estado entre a medida e organizar a chamada para evitar a computação novamente um valor.

Virtualizando layouts *geralmente* precisa para manter algum estado entre a medida e organizar, bem como entre passagens de layout iterativo.

#### <a name="initializing-and-uninitializing-per-container-state"></a>Inicialização e desinicialização o estado por contêiner

Quando um layout é anexado a um contêiner, suas [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore) é chamado de método e oferece a oportunidade de inicializar um objeto para armazenar o estado.

Da mesma forma, quando o layout está sendo removido de um contêiner, o [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore) método será chamado.  Isso o layout de uma oportunidade para limpar qualquer estado que ele tinha associado com esse contêiner.

O objeto de estado do layout pode ser armazenado com e recuperado do contêiner com o [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) propriedade no contexto.

### <a name="ui-virtualization"></a>Virtualização de interface do usuário

Virtualização de interface do usuário significa atrasar a criação de um objeto de interface do usuário até _quando ela for necessária_.  É uma otimização de desempenho.  Para cenários de não rolam determinando _quando necessário_ pode se basear em qualquer número de coisas que são específicos do aplicativo.  Nesses casos, aplicativos devem considerar o uso de [x: carregar](../../xaml-platform/x-load-attribute.md). Ele não exige nenhuma manipulação especial em seu layout.

Em cenários com base em rolagem como uma lista, determinando _quando necessário_ é normalmente baseado em "será visível para um usuário" que depende muito de onde ele foi colocado durante o processo de layout e requer considerações especiais.  Esse cenário é o foco deste documento.

> [!NOTE]
> Embora não seja abordado neste documento, os mesmos recursos que permitem a virtualização de interface do usuário em cenários de rolagem podia ser aplicados em cenários não rolam.  Por exemplo, um controlado por dados controle ToolBar que gerencia o tempo de vida dos comandos a ele apresenta e responde às alterações no espaço disponível, reciclagem / mover elementos entre uma área visível e um menu de estouro.

## <a name="getting-started"></a>Introdução

Primeiro, decida se o layout que você precisa criar deve dar suporte a virtualização de interface do usuário.

**Algumas coisas para ter em mente...**

1. Layouts de virtualização de não são mais fáceis de autor. Se o número de itens sempre será pequeno, em seguida, a criação de um layout de virtualização não é recomendável.
2. A plataforma fornece um conjunto de layouts anexados que funcionam com o [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater#change-the-layout-of-items) e [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) para abordar necessidades comuns.  Familiarize-se com aqueles antes de decidir que você precisa definir um layout personalizado.
3. Layouts de virtualização sempre têm algumas adicional da CPU e o custo/complexidade/sobrecarga de memória em comparação com um layout não virtualizando.  Como regra geral se os filhos o layout será preciso gerenciar provavelmente será melhor em uma área que é 3 vezes o tamanho do visor, então talvez não haja muita ganho de um layout de virtualização. O tamanho de 3 x é discutido em detalhes mais adiante neste documento, mas é devido à natureza assíncrona de rolagem no Windows e seu impacto sobre a virtualização.

> [!TIP]
> Como um ponto de referência, as configurações padrão para o [ListView](/uwp/api/windows.ui.xaml.controls.listview) (e [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)) são que a reciclagem não começa até que o número de itens é suficientes para preencher a 3 vezes o tamanho do visor atual.

**Escolha seu tipo base**

![hierarquia de layout anexado](images/xaml-attached-layout-hierarchy.png)

A base [Layout](/uwp/api/microsoft.ui.xaml.controls.layout) tipo tem dois tipos derivados que servem como o ponto inicial para a criação de um layout anexado:

1. [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
2. [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)

## <a name="non-virtualizing-layout"></a>Virtualização de não Layout

A abordagem para a criação de um layout de virtualização não deve ser familiar para qualquer pessoa que tenha criado uma [painel personalizado](/windows/uwp/design/layout/custom-panels-overview).  Os mesmos conceitos se aplicam.  A principal diferença é que um [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext) é usado para acessar o [filhos](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext.children) coleta e layout podem optar por armazenar o estado.

1. Derivar do tipo de base [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout) (em vez do painel).
2. *(Opcional)*  Definir propriedades de dependência que, quando alteradas serão invalida o layout.
3. _(**New**/opcional)_ inicializar qualquer objeto de estado necessário pelo layout como parte do [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore). Guarde-lo com o contêiner de host usando o [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) fornecido com o contexto.
4. Substituir a [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.measureoverride) e chamar o [medida](/uwp/api/windows.ui.xaml.uielement.measure) método em todos os filhos.
5. Substituir a [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.arrangeoverride) e chamar o [organizar](/uwp/api/windows.ui.xaml.uielement.arrange) método em todos os filhos.
6. *(**New**/opcional)* limpar qualquer estado salvo como parte do [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore).

### <a name="example-a-simple-stack-layout-varying-sized-items"></a>Exemplo: Um Layout de pilha simples (itens de tamanhos variados)

![MyStackLayout](images/xaml-attached-layout-mystacklayout.png)

Aqui está um layout de pilha não virtualizando, com muito básico de itens de tamanhos variados. Ele não tem nenhuma propriedade para ajustar o comportamento do layout. A implementação a seguir ilustra como o layout se baseia no objeto de contexto fornecido pelo contêiner para:

1. Obter a contagem de filhos, e
2. Acesse cada elemento filho por índice.

```csharp
public class MyStackLayout : NonVirtualizingLayout
{
    protected override Size MeasureOverride(NonVirtualizingLayoutContext context, Size availableSize)
    {
        double extentHeight = 0.0;
        foreach (var element in context.Children)
        {
            element.Measure(availableSize);
            extentHeight += element.DesiredSize.Height;
        }

        return new Size(availableSize.Width, extentHeight);
    }

    protected override Size ArrangeOverride(NonVirtualizingLayoutContext context, Size finalSize)
    {
        double offset = 0.0;
        foreach (var element in context.Children)
        {
            element.Arrange(
                new Rect(0, offset, finalSize.Width, element.DesiredSize.Height));
            offset += element.DesiredSize.Height;
        }

        return finalSize;
    }
}
```

```xaml
 <LayoutPanel MaxWidth="196">
    <LayoutPanel.Layout>
        <local:MyStackLayout/>
    </LayoutPanel.Layout>

    <Button HorizontalAlignment="Stretch">1</Button>
    <Button HorizontalAlignment="Right">2</Button>
    <Button HorizontalAlignment="Center">3</Button>
    <Button>4</Button>

</LayoutPanel>
```

## <a name="virtualizing-layouts"></a>Virtualizando Layouts

Semelhante a um layout de virtualização não, as etapas de alto nível para um layout de virtualização são os mesmos.  A complexidade é amplamente na determinação de quais elementos cairá dentro do visor e devem ser realizados.

1. Derivar do tipo de base [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout).
2. (Opcional) Definir suas propriedades de dependência que, quando alteradas serão invalida o layout.
3. Inicializar qualquer objeto de estado que serão necessários pelo layout como parte do [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore). Guarde-lo com o contêiner de host usando o [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) fornecido com o contexto.
4. Substituir a [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) e chamar o [medida](/uwp/api/windows.ui.xaml.uielement.measure) método para cada filho que deve ser obtido.
   1. O [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) método é usado para recuperar um UIElement que foi preparado pela estrutura (por exemplo, associações de dados aplicadas).
5. Substituir a [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) e chamar o [organizar](/uwp/api/windows.ui.xaml.uielement.arrange) método para cada filho realizado.
6. (Opcional) Limpe qualquer estado salvo como parte do [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore).

> [!TIP]
> O valor retornado pela [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) é usado como o tamanho do conteúdo virtualizado.

Há duas abordagens gerais a serem considerados ao criar um layout de virtualização.  Escolha um ou outro amplamente depende "como você determinará o tamanho de um elemento".  Se o suficiente para saber o índice de um item no conjunto de dados ou os dados em si determina seu tamanho eventual, então podemos consideraria **dependente de dados**.  Esses são mais simples de criar.  Se, no entanto, é a única maneira de determinar o tamanho de um item é criar e medir a interface do usuário e em seguida, nós diremos **dependente do conteúdo**.  Esses são mais complexas.

### <a name="the-layout-process"></a>O processo de Layout

Se você estiver criando um dados ou o layout do conteúdo dependente, é importante compreender o processo de layout e o impacto de rolagem assíncrona do Windows.

(Via) é que, uma exibição simplificada das etapas executadas pelo framework de inicialização para exibir a interface do usuário na tela:

1. Ele analisa a marcação.

2. Gera uma árvore de elementos.

3. Executa uma passagem de layout.

4. Executa uma passagem de renderização.

Com a virtualização de interface do usuário, criando os elementos que normalmente seriam feitos na etapa 2 é atrasada ou terminou no início de uma vez seu foi determinado que o conteúdo suficiente foi criado para preencher o visor. Adia um contêiner de virtualização (por exemplo, ItemsRepeater) para seu layout anexado para dirigirem este processo. Ele fornece o layout anexado com um [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) que revela as informações adicionais que precisa de um layout de virtualização.

**O RealizationRect (ou seja, o visor)**

Rolar no Windows acontece assíncrono para o thread de interface do usuário. Ele não é controlado pelo layout da estrutura.  Em vez disso, a interação e a movimentação ocorre no compositor do sistema. A vantagem dessa abordagem é que o movimento panorâmico conteúdo sempre pode ser feito em 60fps.  O desafio, no entanto, é que "visor", como visto pelo layout, pode ser um pouco desatualizado em relação a que é realmente visível na tela. Se o usuário rola rapidamente, eles podem superam a velocidade do thread da interface do usuário para gerar o novo conteúdo e "deslocar para preto". Por esse motivo, geralmente é necessário para um layout de virtualização gerar um buffer adicional de preparada elementos suficientes para preencher uma área maior do que o visor. Quando sob carga mais pesada durante a rolagem o usuário ainda é apresentado com o conteúdo.

![Realização rect](images/xaml-attached-layout-realizationrect.png)

Como a criação de elemento é dispendiosa, virtualizando contêineres (por exemplo, [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)) fornecerá inicialmente o layout anexado com um [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) que corresponde ao visor. No tempo ocioso do contêiner pode atingir o buffer de conteúdo preparado fazendo repetidas chamadas para o layout usando um cada vez maiores Rect de realização. Esse comportamento é uma otimização de desempenho que tenta obter um equilíbrio entre a hora de início rápido e uma boa experiência de panorâmica. O tamanho máximo do buffer que gerará o ItemsRepeater é controlado pelo seu [VerticalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) e [HorizontalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) propriedades.

**Reutilização de elementos (reciclagem)**

O layout é esperado para dimensionar e posicionar os elementos para preencher a [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) sempre que ele é executado. Por padrão o [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) reciclará quaisquer elementos não utilizados no final da cada passagem de layout.

O [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) que é passado para o layout como parte do [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) e [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) fornece as informações adicionais um precisa de layout de virtualização. São algumas das coisas mais comumente usadas, que ele fornece a capacidade de:

1. O número de itens nos dados de consulta ([ItemCount](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.itemcount)).
2. Recuperar um determinado item usando o [GetItemAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getitemat) método.
3. Recuperar um [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) que representa o visor e o buffer que o layout deve preencher com percebi elementos.
4. Solicitar o UIElement para um item específico com o [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) método.

Solicitar um elemento para um determinado índice fará com que esse elemento a ser marcado como "em uso" para essa passagem do layout. Se o elemento ainda não existir, então será percebido e automaticamente preparado para ser usado (por exemplo, aumentando a árvore de interface do usuário definida em um DataTemplate, processamento de qualquer associação de dados, etc.).  Caso contrário, ele será recuperado de um pool de instâncias existentes.

No final de cada passagem de medida, todos os existentes, percebeu elemento que não foi marcada como "em uso" será automaticamente considerado disponível para reutilização, a menos que a opção de [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) foi usado quando o elemento foi recuperado por meio de [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) método. A estrutura automaticamente move para um pool de reciclagem e o torna disponível. Ele pode, posteriormente, ser extraído para uso por um contêiner diferente. O framework tenta evitar isso quando possível, pois não há algum custo associado a reassociação em um elemento.

Se souber de um layout de virtualização no início de cada medida que elementos não caiam dentro do rect realização pode otimizar sua reutilização. Em vez de contar o comportamento da estrutura padrão. O layout preventivamente pode mover elementos para o pool de reciclagem, usando o [RecycleElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recycleelement) método.  Chamar esse método antes de solicitar novos elementos faz com que esses elementos existentes estar disponível quando o layout posteriormente emite um [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) solicitação para um índice que ainda não está associado a um elemento.

O VirtualizingLayoutContext fornece duas propriedades adicionais, projetadas para autores de layout, criação de um layout de conteúdo dependente. Eles são discutidos em mais detalhes posteriormente.

1. Um [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) que fornece um recurso opcional _entrada_ ao layout.
2. Um [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) que é um recurso opcional _saída_ do layout.

## <a name="data-dependent-virtualizing-layouts"></a>Layouts de virtualização dependente de dados

Um layout de virtualização é mais fácil se você souber o que o tamanho de cada item deve ser sem a necessidade de medir o conteúdo para mostrar.  Este documento simplesmente chamaremos a esta categoria de virtualização de layouts como **layouts de dados** , pois geralmente envolvem inspecionando os dados.  Com base nos dados de um aplicativo pode escolher uma representação visual com um tamanho conhecido - talvez porque sua parte dos dados ou anteriormente foi determinado por design.

A abordagem geral é para o layout para:

1. Calcula um tamanho e a posição de cada item.
2. Como parte do [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride):
   1. Use o [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) para determinar quais itens devem aparecer dentro do visor.
   2. Recuperar o UIElement que deve representar o item com o [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) método.
   3. [Medida](/uwp/api/windows.ui.xaml.uielement.measure) o UIElement com o tamanho pré-calculados.
3. Como parte do [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride), [organizar](/uwp/api/windows.ui.xaml.uielement.arrange) cada percebeu UIElement com a posição pré-calculados.

> [!NOTE]
> Uma abordagem de layout de dados geralmente é incompatível com _virtualização de dados_.  Especificamente, em que os únicos dados carregados na memória são que os dados necessários para preencher o que é visível para o usuário.  Virtualização de dados não está se referindo ao carregamento lento ou incremental de dados como um usuário rola para baixo de onde os dados permanecem residentes.  Em vez disso, ele estará se referindo ao quando itens são liberadas da memória conforme eles estão colocados fora da exibição.  Tendo um layout de dados que inspeciona cada item de dados como parte de um layout de dados impediria a virtualização de dados funcionem conforme o esperado.  Uma exceção é um layout, como o UniformGridLayout que pressupõe que tudo o que tem o mesmo tamanho.

> [!TIP]
> Se você estiver criando um controle personalizado para uma biblioteca de controle que será usado por outras pessoas em uma ampla variedade de situações de um layout de dados não pode ser uma opção para você.

### <a name="example-xbox-activity-feed-layout"></a>Exemplo: Layout de Feed de atividades do Xbox

A interface do usuário para o Feed de atividades do Xbox usa um padrão de repetição em que cada linha tem um grande bloco, seguido por dois blocos estreitos que é invertida na linha subsequente. Nesse layout, o tamanho de cada item é uma função da posição do item no conjunto de dados e o tamanho conhecido para os blocos (vs amplas restringir).

![Feed de atividades do Xbox](images/xaml-attached-layout-activityfeedscreenshot.png)

O código abaixo explica quais um personalizado virtualização de interface do usuário para a feed de atividades pode ser para ilustrar geral a abordagem pode levar um **layout de dados**.

<table>
<td>
    <p>Se você tiver o <strong style="font-weight: semi-bold">da Galeria de controles XAML</strong> aplicativo instalado, clique aqui para abrir o aplicativo e ver o <a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a> in action com esse layout de exemplo.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

#### <a name="implementation"></a>Implementação

```csharp
/// <summary>
///  This is a custom layout that displays elements in two different sizes
///  wide (w) and narrow (n). There are two types of rows 
///  odd rows - narrow narrow wide
///  even rows - wide narrow narrow
///  This pattern repeats.
/// </summary>

public class ActivityFeedLayout : VirtualizingLayout // STEP #1 Inherit from base attached layout
{
    // STEP #2 - Parameterize the layout
    #region Layout parameters

    // We'll cache copies of the dependency properties to avoid calling GetValue during layout since that
    // can be quite expensive due to the number of times we'd end up calling these.
    private double _rowSpacing;
    private double _colSpacing;
    private Size _minItemSize = Size.Empty;

    /// <summary>
    /// Gets or sets the size of the whitespace gutter to include between rows
    /// </summary>
    public double RowSpacing
    {
        get { return _rowSpacing; }
        set { SetValue(RowSpacingProperty, value); }
    }

    /// <summary>
    /// Gets or sets the size of the whitespace gutter to include between items on the same row
    /// </summary>
    public double ColumnSpacing
    {
        get { return _colSpacing; }
        set { SetValue(ColumnSpacingProperty, value); }
    }

    public Size MinItemSize
    {
        get { return _minItemSize; }
        set { SetValue(MinItemSizeProperty, value); }
    }

    public static readonly DependencyProperty RowSpacingProperty =
        DependencyProperty.Register(
            nameof(RowSpacing),
            typeof(double),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(0, OnPropertyChanged));

    public static readonly DependencyProperty ColumnSpacingProperty =
        DependencyProperty.Register(
            nameof(ColumnSpacing),
            typeof(double),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(0, OnPropertyChanged));

    public static readonly DependencyProperty MinItemSizeProperty =
        DependencyProperty.Register(
            nameof(MinItemSize),
            typeof(Size),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(Size.Empty, OnPropertyChanged));

    private static void OnPropertyChanged(DependencyObject obj, DependencyPropertyChangedEventArgs args)
    {
        var layout = obj as ActivityFeedLayout;
        if (args.Property == RowSpacingProperty)
        {
            layout._rowSpacing = (double)args.NewValue;
        }
        else if (args.Property == ColumnSpacingProperty)
        {
            layout._colSpacing = (double)args.NewValue;
        }
        else if (args.Property == MinItemSizeProperty)
        {
            layout._minItemSize = (Size)args.NewValue;
        }
        else
        {
            throw new InvalidOperationException("Don't know what you are talking about!");
        }

        layout.InvalidateMeasure();
    }

    #endregion

    #region Setup / teardown // STEP #3: Initialize state

    protected override void InitializeForContextCore(VirtualizingLayoutContext context)
    {
        base.InitializeForContextCore(context);

        var state = context.LayoutState as ActivityFeedLayoutState;
        if (state == null)
        {
            // Store any state we might need since (in theory) the layout could be in use by multiple
            // elements simultaneously
            // In reality for the Xbox Activity Feed there's probably only a single instance.
            context.LayoutState = new ActivityFeedLayoutState();
        }
    }

    protected override void UninitializeForContextCore(VirtualizingLayoutContext context)
    {
        base.UninitializeForContextCore(context);

        // clear any state
        context.LayoutState = null;
    }

    #endregion

    #region Layout // STEP #4,5 - Measure and Arrange

    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        if (this.MinItemSize == Size.Empty)
        {
            var firstElement = context.GetOrCreateElementAt(0);
            firstElement.Measure(new Size(double.PositiveInfinity, double.PositiveInfinity));

            // setting the member value directly to skip invalidating layout
            this._minItemSize = firstElement.DesiredSize;
        }

        // Determine which rows need to be realized.  We know every row will have the same height and
        // only contain 3 items.  Use that to determine the index for the first and last item that
        // will be within that realization rect.
        var firstRowIndex = Math.Max(
            (int)(context.RealizationRect.Y / (this.MinItemSize.Height + this.RowSpacing)) - 1,
            0);
        var lastRowIndex = Math.Min(
            (int)(context.RealizationRect.Bottom / (this.MinItemSize.Height + this.RowSpacing)) + 1,
            (int)(context.ItemCount / 3));

        // Determine which items will appear on those rows and what the rect will be for each item
        var state = context.LayoutState as ActivityFeedLayoutState;
        state.LayoutRects.Clear();

        // Save the index of the first realized item.  We'll use it as a starting point during arrange.
        state.FirstRealizedIndex = firstRowIndex * 3;

        // ideal item width that will expand/shrink to fill available space
        double desiredItemWidth = Math.Max(this.MinItemSize.Width, (availableSize.Width - this.ColumnSpacing * 3) / 4);

        // Foreach item between the first and last index,
        //     Call GetElementOrCreateElementAt which causes an element to either be realized or retrieved
        //       from a recycle pool
        //     Measure the element using an appropriate size
        //
        // Any element that was previously realized which we don't retrieve in this pass (via a call to
        // GetElementOrCreateAt) will be automatically cleared and set aside for later re-use.
        // Note: While this work fine, it does mean that more elements than are required may be
        // created because it isn't until after our MeasureOverride completes that the unused elements
        // will be recycled and available to use.  We could avoid this by choosing to track the first/last
        // index from the previous layout pass.  The diff between the previous range and current range
        // would represent the elements that we can pre-emptively make available for re-use by calling
        // context.RecycleElement(element).
        for (int rowIndex = firstRowIndex; rowIndex < lastRowIndex; rowIndex++)
        {
            int firstItemIndex = rowIndex * 3;
            var boundsForCurrentRow = CalculateLayoutBoundsForRow(rowIndex, desiredItemWidth);

            for (int columnIndex = 0; columnIndex < 3; columnIndex++)
            {
                var index = firstItemIndex + columnIndex;
                var rect = boundsForCurrentRow[index % 3];
                var container = context.GetOrCreateElementAt(index);

                container.Measure(
                    new Size(boundsForCurrentRow[columnIndex].Width, boundsForCurrentRow[columnIndex].Height));

                state.LayoutRects.Add(boundsForCurrentRow[columnIndex]);
            }
        }

        // Calculate and return the size of all the content (realized or not) by figuring out
        // what the bottom/right position of the last item would be.
        var extentHeight = ((int)(context.ItemCount / 3) - 1) * (this.MinItemSize.Height + this.RowSpacing) + this.MinItemSize.Height;

        // Report this as the desired size for the layout
        return new Size(desiredItemWidth * 4 + this.ColumnSpacing * 2, extentHeight);
    }

    protected override Size ArrangeOverride(VirtualizingLayoutContext context, Size finalSize)
    {
        // walk through the cache of containers and arrange
        var state = context.LayoutState as ActivityFeedLayoutState;
        var virtualContext = context as VirtualizingLayoutContext;
        int currentIndex = state.FirstRealizedIndex;

        foreach (var arrangeRect in state.LayoutRects)
        {
            var container = virtualContext.GetOrCreateElementAt(currentIndex);
            container.Arrange(arrangeRect);
            currentIndex++;
        }

        return finalSize;
    }

    #endregion
    #region Helper methods

    private Rect[] CalculateLayoutBoundsForRow(int rowIndex, double desiredItemWidth)
    {
        var boundsForRow = new Rect[3];

        var yoffset = rowIndex * (this.MinItemSize.Height + this.RowSpacing);
        boundsForRow[0].Y = boundsForRow[1].Y = boundsForRow[2].Y = yoffset;
        boundsForRow[0].Height = boundsForRow[1].Height = boundsForRow[2].Height = this.MinItemSize.Height;

        if (rowIndex % 2 == 0)
        {
            // Left tile (narrow)
            boundsForRow[0].X = 0;
            boundsForRow[0].Width = desiredItemWidth;
            // Middle tile (narrow)
            boundsForRow[1].X = boundsForRow[0].Right + this.ColumnSpacing;
            boundsForRow[1].Width = desiredItemWidth;
            // Right tile (wide)
            boundsForRow[2].X = boundsForRow[1].Right + this.ColumnSpacing;
            boundsForRow[2].Width = desiredItemWidth * 2 + this.ColumnSpacing;
        }
        else
        {
            // Left tile (wide)
            boundsForRow[0].X = 0;
            boundsForRow[0].Width = (desiredItemWidth * 2 + this.ColumnSpacing);
            // Middle tile (narrow)
            boundsForRow[1].X = boundsForRow[0].Right + this.ColumnSpacing;
            boundsForRow[1].Width = desiredItemWidth;
            // Right tile (narrow)
            boundsForRow[2].X = boundsForRow[1].Right + this.ColumnSpacing;
            boundsForRow[2].Width = desiredItemWidth;
        }

        return boundsForRow;
    }

    #endregion
}

internal class ActivityFeedLayoutState
{
    public int FirstRealizedIndex { get; set; }

    /// <summary>
    /// List of layout bounds for items starting with the
    /// FirstRealizedIndex.
    /// </summary>
    public List<Rect> LayoutRects
    {
        get
        {
            if (_layoutRects == null)
            {
                _layoutRects = new List<Rect>();
            }

            return _layoutRects;
        }
    }

    private List<Rect> _layoutRects;
}
```

### <a name="optional-managing-the-item-to-uielement-mapping"></a>(Opcional) Gerenciando o Item para o mapeamento de UIElement

Por padrão, o [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) mantém um mapeamento entre os elementos realizados e o índice na fonte de dados que eles representam.  Um layout pode optar por gerenciar esse mapeamento em si por meio da solicitação sempre a opção de [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) ao recuperar um elemento por meio de [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) método que impede que o padrão comportamento de reciclagem de automático.  Um layout pode optar por fazer isso, por exemplo, se ele só será usado quando a rolagem é restrito a única direção e os itens que ele considera sempre serão contíguos (ou seja, sabendo que o índice do primeiro e o último elemento é suficiente saber todos os elementos que devem ser rea lized).

#### <a name="example-xbox-activity-feed-measure"></a>Exemplo: Medida de Feed de atividades do Xbox

O trecho a seguir mostra a lógica adicional que pode ser adicionada ao MeasureOverride no exemplo anterior para gerenciar o mapeamento.

```csharp
    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        //...

        // Determine which items will appear on those rows and what the rect will be for each item
        var state = context.LayoutState as ActivityFeedLayoutState;
        state.LayoutRects.Clear();

         // Recycle previously realized elements that we know we won't need so that they can be used to
        // fill in gaps without requiring us to realize additional elements.
        var newFirstRealizedIndex = firstRowIndex * 3;
        var newLastRealizedIndex = lastRowIndex * 3 + 3;
        for (int i = state.FirstRealizedIndex; i < newFirstRealizedIndex; i++)
        {
            context.RecycleElement(state.IndexToElementMap.Get(i));
            state.IndexToElementMap.Clear(i);
        }

        for (int i = state.LastRealizedIndex; i < newLastRealizedIndex; i++)
        {
            context.RecycleElement(context.IndexElementMap.Get(i));
            state.IndexToElementMap.Clear(i);
        }

        // ...

        // Foreach item between the first and last index,
        //     Call GetElementOrCreateElementAt which causes an element to either be realized or retrieved
        //       from a recycle pool
        //     Measure the element using an appropriate size
        //
        for (int rowIndex = firstRowIndex; rowIndex < lastRowIndex; rowIndex++)
        {
            int firstItemIndex = rowIndex * 3;
            var boundsForCurrentRow = CalculateLayoutBoundsForRow(rowIndex, desiredItemWidth);

            for (int columnIndex = 0; columnIndex < 3; columnIndex++)
            {
                var index = firstItemIndex + columnIndex;
                var rect = boundsForCurrentRow[index % 3];
                UIElement container = null;
                if (state.IndexToElementMap.Contains(index))
                {
                    container = state.IndexToElementMap.Get(index);
                }
                else
                {
                    container = context = context.GetOrCreateElementAt(index, ElementRealizationOptions.ForceCreate | ElementRealizationOptions.SuppressAutoRecycle);
                    state.IndexToElementMap.Add(index, container);
                }

                container.Measure(
                    new Size(boundsForCurrentRow[columnIndex].Width, boundsForCurrentRow[columnIndex].Height));

                state.LayoutRects.Add(boundsForCurrentRow[columnIndex]);
            }
        }

        // ...
   }

internal class ActivityFeedLayoutState
{
    // ...
    Dictionary<int, UIElement> IndexToElementMap { get; set; }
    // ...
}
```

## <a name="content-dependent-virtualizing-layouts"></a>Layouts de virtualização de conteúdo dependente

Se você primeiro deve medir o conteúdo de interface do usuário para um item descobrir seu tamanho exato, então é uma **dependente do conteúdo do layout**.  Você também pode considerar dele como um layout em que cada item deve dimensionar em si em vez do layout informando o item de seu tamanho. Layouts de virtualização que se enquadram nesta categoria são mais envolvidos.

> [!NOTE]
> Layouts de conteúdo dependente não (não deve) quebrar a virtualização de dados.

### <a name="estimations"></a>Estimativas

Layouts de conteúdo dependente dependem de estimativa de adivinhar o tamanho do conteúdo não realizado e a posição do conteúdo realizado. Como alterar essas estimativas-lo fará com que o conteúdo realizado regularmente mudar posições dentro da área rolável. Isso pode levar a uma experiência de usuário muito frustrante e brusca se não for atenuado. Os possíveis problemas e mitigações são discutidas aqui.

> [!NOTE]
> Layouts de dados que considerar todos os itens e sabe o tamanho exato de todos os itens, realizados ou não, bem como suas posições podem evitar esses problemas inteiramente.

**Ancoragem de rolagem**

XAML fornece um mecanismo para atenuar turnos de visor repentinos tendo suporte a controles de rolagem [rolagem ancoragem](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) Implementando a [IScrollAnchorPovider](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) interface. Como o usuário manipula o conteúdo, o controle de rolagem continuamente seleciona um elemento do conjunto de candidatos que estavam aceito em ser rastreado. Se a posição do elemento âncora muda durante o layout do controle de rolagem automaticamente desloca o visor para manter o visor.

O valor da [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) fornecido para o layout pode refletir esse elemento de âncora selecionado no momento escolhido pelo controle de rolagem. Como alternativa, se um desenvolvedor solicita explicitamente que um elemento ser percebido de um índice com o [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.getorcreateelement) método na [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), em seguida, o que o índice é determinado como o [ RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) na próxima passagem de layout. Isso permite que o layout estar preparado para o cenário provável que um desenvolvedor percebe que um elemento e, subsequentemente, solicita que ele ser colocado na exibição por meio de [StartBringIntoView](/uwp/api/windows.ui.xaml.uielement.startbringintoview) método.

O [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) é o índice do item na fonte de dados que um layout de conteúdo dependente deve posicionar primeiro ao calcular a posição de seus itens. Ele deve servir como ponto de partida para o posicionamento de outros itens realizados.

**Impacto sobre as barras de rolagem**

Mesmo com rolagem ancoragem, se as estimativas do layout variam muito, talvez devido a variações significativas no tamanho do conteúdo e, em seguida, a posição do controle deslizante para a barra de rolagem pode aparecer ir ao redor.  Isso pode ser brusca para um usuário se não for exibido o elevador controlar a posição do ponteiro do mouse quando eles estão arrastando-o.

Mais precisos o layout pode estar em suas estimativas, menor a probabilidade um usuário verá elevador da barra de rolagem saltar.

### <a name="layout-corrections"></a>Correções de layout

Um layout de conteúdo dependente deve estar preparado para racionalizar sua estimativa com realidade.  Por exemplo, conforme o usuário rola para a parte superior do conteúdo e o layout percebe que o primeiro elemento, ele poderá considerar que a posição do elemento antecipados relativo ao elemento do qual ele iniciou faria com que ele apareça em qualquer lugar que não seja a origem (x: 0 y:0). Quando isso ocorrer, o layout pode usar o [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) propriedade para definir a posição em que ele é calculado como a nova origem de layout.  O resultado é semelhante ao rolar na qual visor de rolagem do controle é ajustado automaticamente à conta para a posição do conteúdo conforme relatado pelo layout de ancoragem.

![Corrigindo o LayoutOrigin](images/xaml-attached-layout-origincorrection.png)

### <a name="disconnected-viewports"></a>Visores desconectado

O tamanho retornado do layout [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) método representa a melhor estimativa do tamanho do conteúdo que pode ser alterados a cada layout sucessiva.  Conforme o usuário rola o layout será reavaliado continuamente com atualizada [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect).

Se um usuário arrasta o elevador muito rapidamente, em seguida, é possível para o visor, da perspectiva de layout, para fazer grandes salta em que a posição anterior não se sobreponha a posição atual agora.  Isso é devido à natureza assíncrona de rolagem. Também é possível que um aplicativo que está consumindo o layout para solicitar que um elemento ser colocado em modo de exibição para um item que não é percebido no momento e é calculado para definir o layout fora do intervalo atual controlado pelo layout.

Quando o layout descobre seu Palpite está incorreta e/ou vê um deslocamento do visor inesperado, ele precisa reorientar sua posição inicial.  Os layouts de virtualização que são fornecidos como parte dos controles XAML são desenvolvidos como layouts de conteúdo dependente como eles colocam menos restrições sobre a natureza do conteúdo que será mostrado.


### <a name="example-simple-virtualizing-stack-layout-for-variable-sized-items"></a>Exemplo: Layout de pilha de virtualização simples para itens de tamanho variável

O exemplo a seguir demonstra um layout de pilha simples de tamanho variável itens que:

* dá suporte à virtualização de interface do usuário
* usa as estimativas de adivinhar o tamanho dos itens não realizados,
* está ciente do potencial turnos de visor descontínuas, e
* aplica correções de layout para levar em conta essas mudanças.

**Uso: Marcação**

```xaml
<ScrollViewer>

  <ItemsRepeater x:Name="repeater" >
    <ItemsRepeater.Layout>

      <local:VirtualizingStackLayout />

    </ItemsRepeater.Layout>
    <ItemsRepeater.ItemTemplate>
      <DataTemplate x:Key="item">
        <UserControl IsTabStop="True" UseSystemFocusVisuals="True" Margin="5">
          <StackPanel BorderThickness="1" Background="LightGray" Margin="5">
            <Image x:Name="recipeImage" Source="{Binding ImageUri}"  Width="100" Height="100"/>
              <TextBlock x:Name="recipeDescription"
                         Text="{Binding Description}"
                         TextWrapping="Wrap"
                         Margin="10" />
          </StackPanel>
        </UserControl>
      </DataTemplate>
    </ItemsRepeater.ItemTemplate>
  </ItemsRepeater>

</ScrollViewer>
```

**Code-behind: Main.cs**

```csharp
string _lorem = @"Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam laoreet erat vel massa rutrum, eget mollis massa vulputate. Vivamus semper augue leo, eget faucibus nulla mattis nec. Donec scelerisque lacus at dui ultricies, eget auctor ipsum placerat. Integer aliquet libero sed nisi eleifend, nec rutrum arcu lacinia. Sed a sem et ante gravida congue sit amet ut augue. Donec quis pellentesque urna, non finibus metus. Proin sed ornare tellus. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam laoreet erat vel massa rutrum, eget mollis massa vulputate. Vivamus semper augue leo, eget faucibus nulla mattis nec. Donec scelerisque lacus at dui ultricies, eget auctor ipsum placerat. Integer aliquet libero sed nisi eleifend, nec rutrum arcu lacinia. Sed a sem et ante gravida congue sit amet ut augue. Donec quis pellentesque urna, non finibus metus. Proin sed ornare tellus.";

var rnd = new Random();
var data = new ObservableCollection<Recipe>(Enumerable.Range(0, 300).Select(k =>
               new Recipe
               {
                   ImageUri = new Uri(string.Format("ms-appx:///Images/recipe{0}.png", k % 8 + 1)),
                   Description = k + " - " + _lorem.Substring(0, rnd.Next(50, 350))
               }));

repeater.ItemsSource = data;
```

**Código: VirtualizingStackLayout.cs**

```csharp
// This is a sample layout that stacks elements one after
// the other where each item can be of variable height. This is
// also a virtualizing layout - we measure and arrange only elements
// that are in the viewport. Not measuring/arranging all elements means
// that we do not have the complete picture and need to estimate sometimes.
// For example the size of the layout (extent) is an estimation based on the
// average heights we have seen so far. Also, if you drag the mouse thumb
// and yank it quickly, then we estimate what goes in the new viewport.

// The layout caches the bounds of everything that are in the current viewport.
// During measure, we might get a suggested anchor (or start index), we use that
// index to start and layout the rest of the items in the viewport relative to that
// index. Note that since we are estimating, we can end up with negative origin when
// the viewport is somewhere in the middle of the extent. This is achieved by setting the
// LayoutOrigin property on the context. Once this is set, future viewport will account
// for the origin.
public class VirtualizingStackLayout : VirtualizingLayout
{
    // Estimation state
    List<double> m_estimationBuffer = Enumerable.Repeat(0d, 100).ToList();
    int m_numItemsUsedForEstimation = 0;
    double m_totalHeightForEstimation = 0;

    // State to keep track of realized bounds
    int m_firstRealizedDataIndex = 0;
    List<Rect> m_realizedElementBounds = new List<Rect>();

    Rect m_lastExtent = new Rect();

    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        var viewport = context.RealizationRect;
        DebugTrace("MeasureOverride: Viewport " + viewport);

        // Remove bounds for elements that are now outside the viewport.
        // Proactive recycling elements means we can reuse it during this measure pass again.
        RemoveCachedBoundsOutsideViewport(viewport);

        // Find the index of the element to start laying out from - the anchor
        int startIndex = GetStartIndex(context, availableSize);

        // Measure and layout elements starting from the start index, forward and backward.
        Generate(context, availableSize, startIndex, forward:true);
        Generate(context, availableSize, startIndex, forward:false);

        // Estimate the extent size. Note that this can have a non 0 origin.
        m_lastExtent = EstimateExtent(context, availableSize);
        context.LayoutOrigin = new Point(m_lastExtent.X, m_lastExtent.Y);
        return new Size(m_lastExtent.Width, m_lastExtent.Height);
    }

    protected override Size ArrangeOverride(VirtualizingLayoutContext context, Size finalSize)
    {
        DebugTrace("ArrangeOverride: Viewport" + context.RealizationRect);
        for (int realizationIndex = 0; realizationIndex < m_realizedElementBounds.Count; realizationIndex++)
        {
            int currentDataIndex = m_firstRealizedDataIndex + realizationIndex;
            DebugTrace("Arranging " + currentDataIndex);

            // Arrange the child. If any alignment needs to be done, it
            // can be done here.
            var child = context.GetOrCreateElementAt(currentDataIndex);
            var arrangeBounds = m_realizedElementBounds[realizationIndex];
            arrangeBounds.X -= m_lastExtent.X;
            arrangeBounds.Y -= m_lastExtent.Y;
            child.Arrange(arrangeBounds);
        }

        return finalSize;
    }

    // The data collection has changed, since we are maintaining the bounds of elements
    // in the viewport, we will update the list to account for the collection change.
    protected override void OnItemsChangedCore(VirtualizingLayoutContext context, object source, NotifyCollectionChangedEventArgs args)
    {
        InvalidateMeasure();
        if (m_realizedElementBounds.Count > 0)
        {
            switch (args.Action)
            {
                case NotifyCollectionChangedAction.Add:
                    OnItemsAdded(args.NewStartingIndex, args.NewItems.Count);
                    break;
                case NotifyCollectionChangedAction.Replace:
                    OnItemsRemoved(args.OldStartingIndex, args.OldItems.Count);
                    OnItemsAdded(args.NewStartingIndex, args.NewItems.Count);
                    break;
                case NotifyCollectionChangedAction.Remove:
                    OnItemsRemoved(args.OldStartingIndex, args.OldItems.Count);
                    break;
                case NotifyCollectionChangedAction.Reset:
                    m_realizedElementBounds.Clear();
                    m_firstRealizedDataIndex = 0;
                    break;
                default:
                    throw new NotImplementedException();
            }
        }
    }

    // Figure out which index to use as the anchor and start laying out around it.
    private int GetStartIndex(VirtualizingLayoutContext context, Size availableSize)
    {
        int startDataIndex = -1;
        var recommendedAnchorIndex = context.RecommendedAnchorIndex;
        bool isSuggestedAnchorValid = recommendedAnchorIndex != -1;

        if (isSuggestedAnchorValid)
        {
            if (IsRealized(recommendedAnchorIndex))
            {
                startDataIndex = recommendedAnchorIndex;
            }
            else
            {
                ClearRealizedRange();
                startDataIndex = recommendedAnchorIndex;
            }
        }
        else
        {
            // Find the first realized element that is visible in the viewport.
            startDataIndex = GetFirstRealizedDataIndexInViewport(context.RealizationRect);
            if (startDataIndex < 0)
            {
                startDataIndex = EstimateIndexForViewport(context.RealizationRect, context.ItemCount);
                ClearRealizedRange();
            }
        }

        // We have an anchorIndex, realize and measure it and
        // figure out its bounds.
        if (startDataIndex != -1 & context.ItemCount > 0)
        {
            if (m_realizedElementBounds.Count == 0)
            {
                m_firstRealizedDataIndex = startDataIndex;
            }

            var newAnchor = EnsureRealized(startDataIndex);
            DebugTrace("Measuring start index " + startDataIndex);
            var desiredSize = MeasureElement(context, startDataIndex, availableSize);

            var bounds = new Rect(
                0,
                newAnchor ?
                    (m_totalHeightForEstimation / m_numItemsUsedForEstimation) * startDataIndex : GetCachedBoundsForDataIndex(startDataIndex).Y,
                availableSize.Width,
                desiredSize.Height);
            SetCachedBoundsForDataIndex(startDataIndex, bounds);
        }

        return startDataIndex;
    }


    private void Generate(VirtualizingLayoutContext context, Size availableSize, int anchorDataIndex, bool forward)
    {
        // Generate forward or backward from anchorIndex until we hit the end of the viewport
        int step = forward ? 1 : -1;
        int previousDataIndex = anchorDataIndex;
        int currentDataIndex = previousDataIndex + step;
        var viewport = context.RealizationRect;
        while (IsDataIndexValid(currentDataIndex, context.ItemCount) &&
            ShouldContinueFillingUpSpace(previousDataIndex, forward, viewport))
        {
            EnsureRealized(currentDataIndex);
            DebugTrace("Measuring " + currentDataIndex);
            var desiredSize = MeasureElement(context, currentDataIndex, availableSize);
            var previousBounds = GetCachedBoundsForDataIndex(previousDataIndex);
            Rect currentBounds = new Rect(0,
                                          forward ? previousBounds.Y + previousBounds.Height : previousBounds.Y - desiredSize.Height,
                                          availableSize.Width,
                                          desiredSize.Height);
            SetCachedBoundsForDataIndex(currentDataIndex, currentBounds);
            previousDataIndex = currentDataIndex;
            currentDataIndex += step;
        }
    }

    // Remove bounds that are outside the viewport, leaving one extra since our
    // generate stops after generating one extra to know that we are outside the
    // viewport.
    private void RemoveCachedBoundsOutsideViewport(Rect viewport)
    {
        int firstRealizedIndexInViewport = 0;
        while (firstRealizedIndexInViewport < m_realizedElementBounds.Count &&
               !Intersects(m_realizedElementBounds[firstRealizedIndexInViewport], viewport))
        {
            firstRealizedIndexInViewport++;
        }

        int lastRealizedIndexInViewport = m_realizedElementBounds.Count - 1;
        while (lastRealizedIndexInViewport >= 0 &&
            !Intersects(m_realizedElementBounds[lastRealizedIndexInViewport], viewport))
        {
            lastRealizedIndexInViewport--;
        }

        if (firstRealizedIndexInViewport > 0)
        {
            m_firstRealizedDataIndex += firstRealizedIndexInViewport;
            m_realizedElementBounds.RemoveRange(0, firstRealizedIndexInViewport);
        }

        if (lastRealizedIndexInViewport >= 0 && lastRealizedIndexInViewport < m_realizedElementBounds.Count - 2)
        {
            m_realizedElementBounds.RemoveRange(lastRealizedIndexInViewport + 2, m_realizedElementBounds.Count - lastRealizedIndexInViewport - 3);
        }
    }

    private bool Intersects(Rect bounds, Rect viewport)
    {
        return !(bounds.Bottom < viewport.Top ||
            bounds.Top > viewport.Bottom);
    }

    private bool ShouldContinueFillingUpSpace(int dataIndex, bool forward, Rect viewport)
    {
        var bounds = GetCachedBoundsForDataIndex(dataIndex);
        return forward ?
            bounds.Y < viewport.Bottom :
            bounds.Y > viewport.Top;
    }

    private bool IsDataIndexValid(int currentDataIndex, int itemCount)
    {
        return currentDataIndex >= 0 && currentDataIndex < itemCount;
    }

    private int EstimateIndexForViewport(Rect viewport, int dataCount)
    {
        double averageHeight = m_totalHeightForEstimation / m_numItemsUsedForEstimation;
        int estimatedIndex = (int)(viewport.Top / averageHeight);
        // clamp to an index within the collection
        estimatedIndex = Math.Max(0, Math.Min(estimatedIndex, dataCount));
        return estimatedIndex;
    }

    private int GetFirstRealizedDataIndexInViewport(Rect viewport)
    {
        int index = -1;
        if (m_realizedElementBounds.Count > 0)
        {
            for (int i = 0; i < m_realizedElementBounds.Count; i++)
            {
                if (m_realizedElementBounds[i].Y < viewport.Bottom &&
                   m_realizedElementBounds[i].Bottom > viewport.Top)
                {
                    index = m_firstRealizedDataIndex + i;
                    break;
                }
            }
        }

        return index;
    }

    private Size MeasureElement(VirtualizingLayoutContext context, int index, Size availableSize)
    {
        var child = context.GetOrCreateElementAt(index);
        child.Measure(availableSize);

        int estimationBufferIndex = index % m_estimationBuffer.Count;
        bool alreadyMeasured = m_estimationBuffer[estimationBufferIndex] != 0;
        if (!alreadyMeasured)
        {
            m_numItemsUsedForEstimation++;
        }

        m_totalHeightForEstimation -= m_estimationBuffer[estimationBufferIndex];
        m_totalHeightForEstimation += child.DesiredSize.Height;
        m_estimationBuffer[estimationBufferIndex] = child.DesiredSize.Height;

        return child.DesiredSize;
    }

    private bool EnsureRealized(int dataIndex)
    {
        if (!IsRealized(dataIndex))
        {
            int realizationIndex = RealizationIndex(dataIndex);
            Debug.Assert(dataIndex == m_firstRealizedDataIndex - 1 ||
                dataIndex == m_firstRealizedDataIndex + m_realizedElementBounds.Count ||
                m_realizedElementBounds.Count == 0);

            if (realizationIndex == -1)
            {
                m_realizedElementBounds.Insert(0, new Rect());
            }
            else
            {
                m_realizedElementBounds.Add(new Rect());
            }

            if (m_firstRealizedDataIndex > dataIndex)
            {
                m_firstRealizedDataIndex = dataIndex;
            }

            return true;
        }

        return false;
    }

    // Figure out the extent of the layout by getting the number of items remaining
    // above and below the realized elements and getting an estimation based on
    // average item heights seen so far.
    private Rect EstimateExtent(VirtualizingLayoutContext context, Size availableSize)
    {
        double averageHeight = m_totalHeightForEstimation / m_numItemsUsedForEstimation;

        Rect extent = new Rect(0, 0, availableSize.Width, context.ItemCount * averageHeight);

        if (context.ItemCount > 0 && m_realizedElementBounds.Count > 0)
        {
            extent.Y = m_firstRealizedDataIndex == 0 ?
                            m_realizedElementBounds[0].Y :
                            m_realizedElementBounds[0].Y - (m_firstRealizedDataIndex - 1) * averageHeight;

            int lastRealizedIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count;
            if (lastRealizedIndex == context.ItemCount - 1)
            {
                var lastBounds = m_realizedElementBounds[m_realizedElementBounds.Count - 1];
                extent.Y = lastBounds.Bottom;
            }
            else
            {
                var lastBounds = m_realizedElementBounds[m_realizedElementBounds.Count - 1];
                int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count;
                int numItemsAfterLastRealizedIndex = context.ItemCount - lastRealizedDataIndex;
                extent.Height = lastBounds.Bottom + numItemsAfterLastRealizedIndex * averageHeight - extent.Y;
            }
        }

        DebugTrace("Extent " + extent + " with average height " + averageHeight);
        return extent;
    }

    private bool IsRealized(int dataIndex)
    {
        int realizationIndex = dataIndex - m_firstRealizedDataIndex;
        return realizationIndex >= 0 && realizationIndex < m_realizedElementBounds.Count;
    }

    // Index in the m_realizedElementBounds collection
    private int RealizationIndex(int dataIndex)
    {
        return dataIndex - m_firstRealizedDataIndex;
    }

    private void OnItemsAdded(int index, int count)
    {
        // Using the old indexes here (before it was updated by the collection change)
        // if the insert data index is between the first and last realized data index, we need
        // to insert items.
        int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count - 1;
        int newStartingIndex = index;
        if (newStartingIndex > m_firstRealizedDataIndex &&
            newStartingIndex <= lastRealizedDataIndex)
        {
            // Inserted within the realized range
            int insertRangeStartIndex = newStartingIndex - m_firstRealizedDataIndex;
            for (int i = 0; i < count; i++)
            {
                // Insert null (sentinel) here instead of an element, that way we do not
                // end up creating a lot of elements only to be thrown out in the next layout.
                int insertRangeIndex = insertRangeStartIndex + i;
                int dataIndex = newStartingIndex + i;
                // This is to keep the contiguousness of the mapping
                m_realizedElementBounds.Insert(insertRangeIndex, new Rect());
            }
        }
        else if (index <= m_firstRealizedDataIndex)
        {
            // Items were inserted before the realized range.
            // We need to update m_firstRealizedDataIndex;
            m_firstRealizedDataIndex += count;
        }
    }

    private void OnItemsRemoved(int index, int count)
    {
        int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count - 1;
        int startIndex = Math.Max(m_firstRealizedDataIndex, index);
        int endIndex = Math.Min(lastRealizedDataIndex, index + count - 1);
        bool removeAffectsFirstRealizedDataIndex = (index <= m_firstRealizedDataIndex);

        if (endIndex >= startIndex)
        {
            ClearRealizedRange(RealizationIndex(startIndex), endIndex - startIndex + 1);
        }

        if (removeAffectsFirstRealizedDataIndex &&
            m_firstRealizedDataIndex != -1)
        {
            m_firstRealizedDataIndex -= count;
        }
    }

    private void ClearRealizedRange(int startRealizedIndex, int count)
    {
        m_realizedElementBounds.RemoveRange(startRealizedIndex, count);
        if (startRealizedIndex == 0)
        {
            m_firstRealizedDataIndex = m_realizedElementBounds.Count == 0 ? 0 : m_firstRealizedDataIndex + count;
        }
    }

    private void ClearRealizedRange()
    {
        m_realizedElementBounds.Clear();
        m_firstRealizedDataIndex = 0;
    }

    private Rect GetCachedBoundsForDataIndex(int dataIndex)
    {
        return m_realizedElementBounds[RealizationIndex(dataIndex)];
    }

    private void SetCachedBoundsForDataIndex(int dataIndex, Rect bounds)
    {
        m_realizedElementBounds[RealizationIndex(dataIndex)] = bounds;
    }

    private Rect GetCachedBoundsForRealizationIndex(int relativeIndex)
    {
        return m_realizedElementBounds[relativeIndex];
    }

    void DebugTrace(string message, params object[] args)
    {
        Debug.WriteLine(message, args);
    }
}
```

## <a name="related-articles"></a>Artigos relacionados

- [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
- [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
