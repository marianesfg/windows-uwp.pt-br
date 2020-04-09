---
Description: Você pode definir um layout anexado para uso em contêineres, como o controle ItemsRepeater.
title: AttachedLayout
label: AttachedLayout
template: detail.hbs
ms.date: 03/13/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dc23e86f85c5db3dd10c5cec152047be387d4513
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282295"
---
# <a name="attached-layouts"></a>Layouts anexados

Um contêiner (por exemplo, Panel) que delega sua lógica de layout a outro objeto depende do objeto de layout anexado para fornecer o comportamento do layout para seus elementos filhos.  Um modelo de layout anexado fornece flexibilidade para que um aplicativo altere o layout dos itens em runtime ou compartilhe mais facilmente aspectos do layout entre diferentes partes da interface do usuário (por exemplo, itens nas linhas de uma tabela que parecem estar alinhados em uma coluna).

Neste tópico, abordamos o que está envolvido na criação de um layout anexado (de virtualização e não virtualização), os conceitos e classes as que você precisa compreender e as vantagens e desvantagens que é preciso considerar ao decidir entre eles.

| **Obter a biblioteca de interface do usuário do Windows** |
| - |
| Este controle está incluído como parte da biblioteca de interface do usuário do Windows, um pacote NuGet que contém novos controles e recursos de interface do usuário para aplicativos UWP. Para obter mais informações, inclusive instruções de instalação, consulte a [Visão geral da biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

> **APIs importantes**:

> * [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
> * [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)
> * [Layout](/uwp/api/microsoft.ui.xaml.controls.layout)
>     * [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
>     * [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)
> * [LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext)
>     * [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext)
>     * [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)
> * [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) (versão prévia)

## <a name="key-concepts"></a>Conceitos Principais

A execução do layout requer que duas perguntas sejam respondidas para cada elemento:

1. Qual será o ***tamanho*** desse elemento?

2. Qual será a ***posição*** desse elemento?

O sistema de layout do XAML, que responde a essas perguntas, é abordado brevemente como parte da discussão de [Painéis personalizados](/windows/uwp/design/layout/custom-panels-overview).

### <a name="containers-and-context"></a>Contêineres e contexto

Conceitualmente, o [Panel](/uwp/api/windows.ui.xaml.controls.panel) do XAML cumpre duas funções importantes na estrutura:

1. Ele pode conter elementos filhos e introduz ramificação na árvore de elementos.
2. Ele aplica uma estratégia de layout específica a esses filhos.

Por esse motivo, um Panel em XAML costuma ser sinônimo de layout, porém, tecnicamente falando, ele faz mais do que apenas o layout.

O [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater) também se comporta como o Panel, com a diferença de que não expõe uma propriedade Children que permitiria adicionar ou remover programaticamente filhos UIElement.  Em vez disso, o tempo de vida de seus filhos é gerenciado de forma automática pela estrutura para corresponder a uma coleção de itens de dados.  Embora não seja derivado do Panel, ele se comporta e é tratado pela estrutura como um Panel.

> [!NOTE]
> O [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) é um contêiner, derivado do Panel, que delega sua lógica ao objeto [Layout](/uwp/api/microsoft.ui.xaml.controls.layoutpanel.layout) anexado.  O LayoutPanel está em *Versão prévia* e disponível apenas nas atualizações de *Pré-lançamento* do pacote WinUI.

#### <a name="containers"></a>Contêineres

Conceitualmente, [Panel](/uwp/api/windows.ui.xaml.controls.panel) é um contêiner de elementos que também tem a capacidade de renderizar pixels para um [Background](/uwp/api/windows.ui.xaml.controls.panel.background).  Os Panels fornecem uma maneira de encapsular a lógica de layout comum em um pacote fácil de usar.

O conceito de **layout anexado** torna mais clara a distinção entre as duas funções de contêiner e layout.  Se o contêiner delega sua lógica de layout a outro objeto, chamamos esse objeto de layout anexado, como visto no trecho de código abaixo. Os contêineres herdados do [FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement), como o LayoutPanel, expõem automaticamente as propriedades comuns que fornecem entrada para o processo de layout do XAML (por exemplo, Altura e Largura).

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

Durante o processo de layout, o contêiner conta com o *UniformGridLayout* anexado para medir e organizar seus filhos.

#### <a name="per-container-state"></a>Estado por contêiner

Com um layout anexado, uma única instância do objeto de layout pode ser associada a *muitos* contêineres, como no trecho de código abaixo; portanto, ele não deve depender do contêiner do host nem referenciá-lo diretamente.  Por exemplo:

```xaml
<!-- ... --->
<Page.Resources>
    <ExampleLayout x:Name="exampleLayout"/>
<Page.Resources>

<LayoutPanel x:Name="example1" Layout="{StaticResource exampleLayout}"/>
<LayoutPanel x:Name="example2" Layout="{StaticResource exampleLayout}"/>
<!-- ... --->
```

Para essa situação, o *ExampleLayout* deve considerar cuidadosamente o estado que ele usa em seu cálculo de layout e onde esse estado é armazenado para evitar impactar o layout dos elementos de um painel com o outro.  Isso seria análogo a um Panel personalizado cuja lógica MeasureOverride e ArrangeOverride depende dos valores de suas propriedades *estáticas*.

#### <a name="layoutcontext"></a>LayoutContext

O objetivo do [LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext) é lidar com esses desafios.  Ele fornece ao layout anexado a capacidade de interagir com o contêiner do host, como na recuperação de elementos filhos, sem introduzir uma dependência direta entre os dois. O contexto também permite que o layout armazene qualquer estado necessário que possa estar relacionado aos elementos filhos do contêiner.

Layouts simples e de não virtualização geralmente não precisam manter nenhum estado, fazendo com que isso não seja um problema. No entanto, um layout mais complexo, como Grid, pode optar por manter o estado entre a medida e a organização da chamada para evitar recalcular um valor.

Os layouts de virtualização *geralmente* precisam manter algum estado entre a medida e a organização, bem como entre as passagens de layout iterativas.

#### <a name="initializing-and-uninitializing-per-container-state"></a>Inicializar e cancelar a inicialização do estado por contêiner

Quando um layout é anexado a um contêiner, seu método [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore) é chamado e oferece uma oportunidade de inicializar um objeto para armazenar o estado.

Da mesma forma, quando o layout estiver sendo removido de um contêiner, o método [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore) será chamado.  Isso dá ao layout a oportunidade de limpar qualquer estado associado a esse contêiner.

O objeto de estado do layout pode ser armazenado e recuperado do contêiner com a propriedade [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) no contexto.

### <a name="ui-virtualization"></a>Virtualização da interface do usuário

A virtualização da interface do usuário significa adiar a criação de um objeto de interface do usuário até _que ele seja necessário_.  É uma otimização de desempenho.  Para cenários sem rolagem, determinar _quando necessário_ pode se basear em vários fatores específicos do aplicativo.  Nesses casos, os aplicativos devem considerar o uso do [x:Load](../../xaml-platform/x-load-attribute.md). Ele não requer qualquer manipulação especial em seu layout.

Em cenários baseados em rolagem, como uma lista, a determinação de _quando necessário_ geralmente é baseada em "será visível para um usuário", o que depende muito de onde foi colocado durante o processo de layout e requer considerações especiais.  Esse cenário é focado neste documento.

> [!NOTE]
> Embora não sejam abordados neste documento, os mesmos recursos que habilitam a virtualização da interface do usuário em cenários de rolagem podem ser aplicados em cenários sem rolagem.  Por exemplo, um controle Barra de ferramentas controlado por dados que gerencia a vida útil dos comandos apresentados e responde a alterações no espaço disponível reciclando/movendo elementos entre uma área visível e um menu de estouro.

## <a name="getting-started"></a>Introdução

Primeiro, decida se o layout que você precisa criar deve dar suporte à virtualização da interface do usuário.

**Alguns pontos para se lembrar...**

1. Layouts de não virtualização são mais fáceis de criar. Caso o número de itens seja sempre pequeno, recomendamos a criação de um layout de não virtualização.
2. A plataforma fornece um conjunto de layouts anexados que funcionam com o [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater#change-the-layout-of-items) e o [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) para atender a necessidades comuns.  Familiarize-se com eles antes de decidir se você precisa definir um layout personalizado.
3. Os layouts de virtualização sempre têm um custo/complexidade/sobrecarga de CPU e memória adicionais, em comparação com um layout não virtualizado.  Como regra geral, no caso dos filhos, se o layout a ser gerenciado couber em uma área que seja três vezes maior que o visor, poderá não haver muito ganho com um layout de virtualização. O tamanho de três vezes é discutido em mais detalhes posteriormente neste documento, mas se deve à natureza assíncrona da rolagem no Windows e seu impacto na virtualização.

> [!TIP]
> Como ponto de referência, as configurações padrão do [ListView](/uwp/api/windows.ui.xaml.controls.listview) (e [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)) definem que a reciclagem não começa até que o número de itens seja suficiente para preencher três vezes o tamanho do visor atual.

**Escolha seu tipo base**

![hierarquia de layout anexado](images/xaml-attached-layout-hierarchy.png)

O tipo base de [Layout](/uwp/api/microsoft.ui.xaml.controls.layout) tem dois tipos derivados que servem como ponto de partida para a criação de um layout anexado:

1. [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
2. [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)

## <a name="non-virtualizing-layout"></a>Layout de não virtualização

A abordagem para criar um layout de não virtualização deve parecer familiar para qualquer pessoa que tenha criado um [Painel personalizado](/windows/uwp/design/layout/custom-panels-overview).  Os mesmos conceitos se aplicam.  A principal diferença é que um [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext) é usado para acessar a coleção [Children](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext.children), e o layout pode optar por armazenar o estado.

1. Derive do tipo base [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout) (em vez de Panel).
2. *(Opcional)* Defina as propriedades de dependência que, quando alteradas, invalidarão o layout.
3. _(**Novo**/Opcional)_ Inicialize qualquer objeto de estado exigido pelo layout como parte do [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore). Armazene-o no contêiner do host usando [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) fornecido com o contexto.
4. Substitua [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.measureoverride) e chame o método [Measure](/uwp/api/windows.ui.xaml.uielement.measure) em todos os filhos.
5. Substitua [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.arrangeoverride) e chame o método [Arrange](/uwp/api/windows.ui.xaml.uielement.arrange) em todos os filhos.
6. *(**Novo**/Opcional)* Limpe qualquer estado salvo como parte de [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore).

### <a name="example-a-simple-stack-layout-varying-sized-items"></a>Exemplo: um layout de pilha simples (itens de tamanhos variados)

![MyStackLayout](images/xaml-attached-layout-mystacklayout.png)

Aqui está um layout de pilha de não virtualização muito básico de itens de tamanhos variados. Ele não tem propriedades para ajustar o comportamento do layout. A implementação abaixo ilustra como o layout se baseia no objeto de contexto fornecido pelo contêiner para:

1. Obter a contagem de filhos e
2. Acessar cada elemento filho por índice.

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

## <a name="virtualizing-layouts"></a>Layouts de virtualização

Semelhante a um layout de não virtualização, as etapas de alto nível para um layout de virtualização são as mesmas.  A complexidade reside amplamente na determinação de quais elementos se enquadram no visor e devem ser percebidos.

1. Derive do tipo base [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout).
2. (Opcional) Defina as propriedades de dependência que, quando alteradas, invalidarão o layout.
3. Inicialize qualquer objeto de estado exigido pelo layout como parte de [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore). Armazene-o no contêiner do host usando [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) fornecido com o contexto.
4. Substitua [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) e chame o método [Measure](/uwp/api/windows.ui.xaml.uielement.measure) para cada filho que deve ser percebido.
   1. O método [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) é usado para recuperar um UIElement que foi preparado pela estrutura (por exemplo, associações de dados aplicadas).
5. Substitua [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) e chame o método [Arrange](/uwp/api/windows.ui.xaml.uielement.arrange) para cada filho percebido.
6. (Opcional) Limpe qualquer estado salvo como parte de [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore).

> [!TIP]
> O valor retornado por [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) é usado como o tamanho do conteúdo virtualizado.

Há duas abordagens gerais a serem consideradas ao criar um layout de virtualização.  A escolha de um ou outro depende em grande parte de "como você determinará o tamanho de um elemento".  Se for suficiente conhecer o índice de um item no conjunto de dados, ou os próprios dados determinarem seu tamanho eventual, consideraremos que ele **depende dos dados**.  Esses são mais simples de criar.  Se, no entanto, a única maneira de determinar o tamanho de um item for criar e medir a interface do usuário, diremos que ele **depende do conteúdo**.  Esses são mais complexos.

### <a name="the-layout-process"></a>O processo de layout

Se você estiver criando um layout dependente de conteúdo ou de dados, é importante entender o processo de layout e o impacto da rolagem assíncrona do Windows.

Uma visão (super)simplificada das etapas executadas pela estrutura desde a inicialização até a exibição da interface do usuário na tela é a seguinte:

1. Análise da marcação.

2. Geração de uma árvore de elementos.

3. Transmissão de layout.

4. Transmissão de renderização.

Com a virtualização da interface do usuário, a criação dos elementos que normalmente seriam feitos na etapa 2 é adiada ou finalizada mais cedo, uma vez que seja determinado que conteúdo suficiente foi criado para preencher o visor. Um contêiner de virtualização (por exemplo, ItemsRepeater) é adiado para o layout anexado a fim de orientar esse processo. Ele fornece ao layout anexado um [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) que apresenta as informações adicionais necessárias para um layout de virtualização.

**RealizationRect (ou seja, visor)**

A rolagem no Windows ocorre de forma assíncrona com o thread da interface do usuário. Ela não é controlada pelo layout da estrutura.  Em vez disso, a interação e o movimento ocorrem no compositor do sistema. A vantagem dessa abordagem é que o conteúdo panorâmico sempre pode ser feito a 60 fps.  O desafio, no entanto, é que o "visor", como visto pelo layout, pode ficar um pouco desatualizado em relação ao que realmente está visível na tela. Se um usuário rolar rapidamente, poderá ultrapassar a velocidade do thread da interface do usuário para gerar um novo conteúdo e "aplicar panorâmica com preto". Por esse motivo, muitas vezes é necessário que um layout de virtualização gere um buffer adicional de elementos preparados, suficientes para preencher uma área maior que o visor. Quando sob carga mais pesada durante a rolagem, o usuário ainda recebe conteúdo.

![Realization rect](images/xaml-attached-layout-realizationrect.png)

Como a criação de elementos é cara, a virtualização de contêineres (por exemplo, [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)) fornecerá inicialmente ao layout anexado um [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) que corresponde ao visor. No tempo ocioso, o contêiner pode aumentar o buffer do conteúdo preparado fazendo chamadas repetidas para o layout usando um realization rect cada vez maior. Esse comportamento é uma otimização de desempenho que tenta encontrar um equilíbrio entre o tempo de inicialização rápido e uma boa experiência panorâmica. O tamanho máximo do buffer que o ItemsRepeater gerará é controlado por suas propriedades [VerticalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) e [HorizontalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength).

**Reutilizar elementos (reciclagem)**

O layout deve dimensionar e posicionar os elementos para preencher o [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) toda vez que for executado. Por padrão, [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) reciclará quaisquer elementos não utilizados no final de cada passagem de layout.

O [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) que é passado para o layout como parte de [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) e [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) fornece as informações adicionais necessárias para o layout de virtualização. Alguns dos aspectos mais usados que ele fornece são a capacidade de:

1. Consultar o número de itens nos dados ([ItemCount](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.itemcount)).
2. Recuperar um item específico usando o método [GetItemAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getitemat).
3. Recuperar um [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) que representa o visor e o buffer que o layout deve preencher com os elementos realizados.
4. Solicitar o UIElement para um item específico com o método [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat).

Solicitar um elemento para determinado índice fará com que esse elemento seja marcado como "em uso" para essa passagem do layout. Se o elemento ainda não existir, ele será reconhecido e preparado automaticamente para uso (por exemplo, inflar a árvore da interface do usuário definida em um DataTemplate, processar qualquer associação de dados etc.).  Caso contrário, ele será recuperado de um pool de instâncias existentes.

No final de cada passagem de medida, qualquer elemento reconhecido existente que não foi marcado como "em uso" é automaticamente considerado disponível para reutilização, a menos que a opção [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) tenha sido usada quando o elemento foi recuperado pelo método [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat). A estrutura o move automaticamente para um pool de reciclagem e o disponibiliza. Posteriormente, ele pode ser extraído para uso por um contêiner diferente. A estrutura tenta evitar isso quando possível, já que há algum custo associado à criação de um novo pai de um elemento.

Se um layout de virtualização souber no início de cada medida quais elementos não estarão mais dentro do realization rect, ele poderá otimizar sua reutilização, em vez de depender do comportamento padrão da estrutura. O layout pode mover elementos preventivamente para o pool de reciclagem usando o método [RecycleElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recycleelement).  A chamada desse método antes de solicitar novos elementos faz com que esses elementos existentes estejam disponíveis quando o layout emitir posteriormente uma solicitação [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) para um índice que ainda não esteja associado a um elemento.

O VirtualizingLayoutContext fornece duas propriedades adicionais projetadas para autores de layout, criando um layout dependente de conteúdo. Elas são discutidas em mais detalhes posteriormente.

1. [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) fornece uma _entrada_ opcional para o layout.
2. [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) é uma _saída_ opcional do layout.

## <a name="data-dependent-virtualizing-layouts"></a>Layouts de virtualização dependente de dados

Um layout de virtualização será mais fácil se você souber qual deve ser o tamanho de cada item sem precisar medir o conteúdo a ser exibido.  Neste documento, nós nos referimos a essa categoria de layouts de virtualização simplesmente como **layouts de dados**, pois eles costumam envolver inspeção dos dados.  Com base nos dados, um aplicativo pode escolher uma representação visual com um tamanho conhecido, talvez porque faça parte dos dados ou tenha sido previamente determinada pelo design.

A abordagem geral do layout é:

1. Calcular o tamanho e a posição de cada item.
2. Como parte do [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride):
   1. Usar o [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) para determinar quais itens devem aparecer no visor.
   2. Recuperar o UIElement que deve representar o item com o método [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat).
   3. [Medir](/uwp/api/windows.ui.xaml.uielement.measure) o UIElement com o tamanho pré-calculado.
3. Como parte do [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride), [organizar](/uwp/api/windows.ui.xaml.uielement.arrange) cada UIElement reconhecido com a posição pré-calculada.

> [!NOTE]
> Uma abordagem de layout de dados geralmente é incompatível com a _virtualização de dados_.  Especificamente, quando os únicos dados carregados na memória são os dados necessários para preencher o que está visível para o usuário.  A virtualização de dados não se refere ao carregamento lento ou incremental de dados à medida que o usuário rola para baixo onde esses dados permanecem residentes.  Em vez disso, ela se refere a quando os itens são liberados da memória conforme que são rolados para fora da exibição.  Ter um layout de dados que inspeciona todos os itens de dados como parte de um layout impediria que a virtualização de dados funcionasse conforme o esperado.  Uma exceção é um layout como o UniformGridLayout, que pressupõe que tudo tem o mesmo tamanho.

> [!TIP]
> Se você estiver criando um controle personalizado para uma biblioteca de controle que será usada por outras pessoas em uma ampla variedade de situações, um layout de dados poderá não ser uma opção para você.

### <a name="example-xbox-activity-feed-layout"></a>Exemplo: layout do Feed de atividades do Xbox

A interface do usuário do Feed de Atividades do Xbox usa um padrão repetitivo em que cada linha tem um bloco largo, seguido por dois blocos estreitos que são invertidos na linha subsequente. Nesse layout, o tamanho de cada item é uma função da posição do item no conjunto de dados e o tamanho conhecido dos blocos (largos x estreitos).

![Feed de atividades do Xbox](images/xaml-attached-layout-activityfeedscreenshot.png)

O código abaixo mostra como uma interface de usuário de virtualização personalizada para o feed de atividades pode ilustrar a abordagem geral que você deve adotar para um **layout de dados**.

<table>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para abri-lo e ver o <a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a> em funcionamento com este layout de exemplo.</p>
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

### <a name="optional-managing-the-item-to-uielement-mapping"></a>(Opcional) Gerenciar o mapeamento de itens no UIElement

Por padrão, o [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) mantém um mapeamento entre os elementos reconhecidos e o índice na fonte de dados que eles representam.  Um layout pode gerenciar esse mapeamento, solicitando sempre a opção [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) ao recuperar um elemento por meio do método [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat), o que impede o comportamento padrão de reciclagem automática.  Um layout poderá optar por fazer isso, por exemplo, se for usado apenas quando a rolagem estiver restrita a uma direção e os itens que ele considerar forem sempre contíguos (ou seja, saber o índice do primeiro e do último elemento é suficiente para conhecer todos os elementos que devem ser percebidos).

#### <a name="example-xbox-activity-feed-measure"></a>Exemplo: medida do Feed de atividades do Xbox

O trecho de código abaixo mostra a lógica extra que pode ser adicionada a MeasureOverride no exemplo anterior para gerenciar o mapeamento.

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

## <a name="content-dependent-virtualizing-layouts"></a>Layouts de virtualização dependente de conteúdo

Caso seja preciso medir primeiro o conteúdo da interface do usuário para um item descobrir seu tamanho exato, isso significa que se trata de um layout **dependente do conteúdo**.  Você também pode pensar nele como um layout em que cada item deve se dimensionar, em vez de o layout indicar o tamanho do item. Os layouts de virtualização que se enquadram nessa categoria são mais envolvidos.

> [!NOTE]
> Os layouts dependentes de conteúdo não interrompem a virtualização de dados ou, pelo menos, não deveriam fazer isso.

### <a name="estimations"></a>Estimativas

Os layouts dependentes do conteúdo se baseiam na estimativa para adivinhar o tamanho do conteúdo não percebido e a posição do conteúdo percebido. À medida que essas estimativas mudam, o conteúdo percebido muda regularmente as posições na área de rolagem. Isso pode levar a uma experiência de usuário muito frustrante e dissonante, se não for mitigado. Os possíveis problemas e atenuações são discutidos aqui.

> [!NOTE]
> Os layouts de dados que consideram cada item e sabem o tamanho exato de todos os itens, percebidos ou não, e suas posições, podem evitar esses problemas completamente.

**Ancoragem de rolagem**

O XAML fornece um mecanismo para mitigar mudanças repentinas de visor, fazendo com que os controles de rolagem ofereçam suporte à [ancoragem de rolagem](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) implementando a interface [IScrollAnchorPovider](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider). À medida que o usuário manipula o conteúdo, o controle de rolagem seleciona continuamente um elemento do conjunto de candidatos que foram incluídos para serem acompanhados. Se a posição do elemento âncora for alterada durante o layout, o controle de rolagem mudará automaticamente seu visor para mantê-lo.

O valor do [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) fornecido ao layout pode refletir o elemento âncora atualmente selecionado, escolhido pelo controle de rolagem. Como alternativa, se um desenvolvedor solicitar explicitamente que um elemento seja percebido para um índice com o método [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.getorcreateelement) no [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), esse índice será fornecido como o [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) na próxima passagem de layout. Isso permite que o layout seja preparado para o cenário provável de que um desenvolvedor realize um elemento e, posteriormente, solicite que ele seja visualizado por meio do método [StartBringIntoView](/uwp/api/windows.ui.xaml.uielement.startbringintoview).

O [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) é o índice do item na fonte de dados que um layout dependente de conteúdo deve posicionar primeiro ao estimar a posição de seus itens. Ele deve servir como o ponto de partida para o posicionamento de outros itens percebidos.

**Impacto nas barras de rolagem**

Mesmo com a ancoragem de rolagem, se as estimativas do layout variarem muito, talvez devido a variações significativas no tamanho do conteúdo, a posição da barra de rolagem poderá parecer estar em movimento.  Se a miniatura parecer não acompanhar a posição do ponteiro do mouse ao arrastar, isso poderá atrapalhar o usuário.

Quanto mais preciso o layout estiver em suas estimativas, menor a probabilidade de um usuário ver a barra de rolagem pulando.

### <a name="layout-corrections"></a>Correções de layout

Um layout dependente de conteúdo deve ser preparado para racionalizar sua estimativa com a realidade.  Por exemplo, à medida que o usuário rola para a parte superior do conteúdo, e o layout apresenta o primeiro elemento, pode ser que a posição antecipada do elemento em relação ao elemento a partir do qual ele foi iniciado faça com que apareça em algum lugar que não seja a origem de (x:0, y:0). Quando isso ocorre, o layout pode usar a propriedade [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) para definir a posição calculada como a nova origem do layout.  O resultado líquido é semelhante à ancoragem de rolagem, na qual o visor do controle de rolagem é ajustado automaticamente para levar em consideração a posição do conteúdo, conforme relatado pelo layout.

![Corrigir o LayoutOrigin](images/xaml-attached-layout-origincorrection.png)

### <a name="disconnected-viewports"></a>Visores desconectados

O tamanho retornado do método [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) do layout representa a melhor estimativa do tamanho do conteúdo que pode ser alterado a cada layout sucessivo.  À medida que o usuário rola, o layout será reavaliado continuamente com um [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) atualizado.

Caso um usuário arraste o polegar muito rapidamente, é possível que o visor, da perspectiva do layout, pareça fazer grandes saltos onde a posição anterior não se sobrepõe à posição atual.  Isso ocorre devido à natureza assíncrona da rolagem. Também é possível que um aplicativo que esteja consumindo o layout solicite que um elemento seja exibido para um item que não foi percebido no momento e que se estima estar fora do intervalo atual rastreado pelo layout.

Quando o layout descobre que sua estimativa está incorreta e/ou observa uma alteração inesperada de visor, ele precisa reorientar sua posição inicial.  Os layouts de virtualização fornecidos como parte dos controles XAML são desenvolvidos como layouts dependentes de conteúdo, pois impõem menos restrições à natureza do conteúdo que será mostrado.


### <a name="example-simple-virtualizing-stack-layout-for-variable-sized-items"></a>Exemplo: Layout de pilha de virtualização simples para itens de tamanho variável

A amostra abaixo demonstra um layout de pilha simples para itens de tamanho variável que:

* dá suporte à virtualização da interface do usuário,
* usa estimativas para adivinhar o tamanho dos itens não percebidos,
* está ciente de possíveis alterações descontínuas no visor e
* aplica correções de layout para considerar essas alterações.

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

**Codebehind: Main.cs**

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
