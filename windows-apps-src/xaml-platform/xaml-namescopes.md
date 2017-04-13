---
author: jwmsft
description: "Um namescope XAML armazena relacionamentos entre os nomes de objetos definidos por XAML e suas instâncias equivalentes. Esse conceito é semelhante ao significado mais abrangente do termo namescope em outras linguagens e tecnologias de programação."
title: Namescopes XAML
ms.assetid: EB060CBD-A589-475E-B83D-B24068B54C21
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 973207d480327ced18b6136796ef92d5d5b81dd5
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="xaml-namescopes"></a>Namescopes XAML

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Um *namescope XAML* armazena relacionamentos entre os nomes de objetos definidos por XAML e suas instâncias equivalentes. Esse conceito é semelhante ao significado mais abrangente do termo *namescope* em outras linguagens e tecnologias de programação.

## <a name="how-xaml-namescopes-are-defined"></a>Como os namescopes XAML são definidos?

Os nomes em namescopes XAML permitem que o código do usuário faça referência a objetos inicialmente declarados em XAML. O resultado interno de analisar XAML é que o tempo de execução cria um conjunto de objetos que retêm alguns ou todos os relacionamentos que esses objetos tinham nas declarações XAML. Esses relacionamentos são mantidos como propriedades específicas dos objetos criados ou expostos como métodos utilitários nas APIs de modelo de programação.

O uso mais comum de um nome em um namescope XAML é como referência direta a uma instância de objeto, habilitada pelo ciclo de compilação de marcação como uma ação de compilação do projeto, combinada a um método **InitializeComponent** gerado nos modelos de classe parcial.

Você também pode usar o método utilitário [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) por conta própria em tempo de execução para retornar uma referência a objetos que foram definidos com um nome na marcação XAML.

### <a name="more-about-build-actions-and-xaml"></a>Mais sobre ações de compilação e XAML

Tecnicamente, o que ocorre é que a própria linguagem XAML passa por um ciclo do compilador de marcação ao mesmo tempo em que a linguagem XAML e a classe parcial definida para code-behind são compiladas juntas. Cada elemento de objeto com um atributo **Name** ou [atributo x:Name](x-name-attribute.md) definido na marcação gera um campo interno com um nome que corresponde ao nome XAML. Inicialmente, esse campo fica vazio. Então, a classe gera um método **InitializeComponent** chamado apenas após o carregamento de toda a linguagem XAML. Na lógica **InitializeComponent** cada campo interno é preenchido com o valor de retorno [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) para a cadeia de nome equivalente. Você pode observar esta infraestrutura olhando os arquivos ".g" (gerados) criados para cada página XAML na subpasta /obj de um projeto de aplicativo do Windows Runtime após a compilação. Você também pode ver os campos e o método **InitializeComponent** como membros dos assemblies resultantes quando refletir sobre eles ou examinar o conteúdo da linguagem de sua interface.

**Observação** Especificamente para extensões de componentes Visual C++ (aplicativos C++/CX), um campo de suporte para uma referência **x:Name** não é criado para o elemento raiz de um arquivo XAML. Se você precisa mencionar o objeto raiz do code-behind C++/CX, use outras APIs ou outro percurso de árvore. Por exemplo, você pode chamar [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) para um elemento filho nomeado conhecido e depois chamar [**Parent**](https://msdn.microsoft.com/library/windows/apps/br208739).

## <a name="creating-objects-at-run-time-with-xamlreaderload"></a>Criando objetos em tempo de execução com XamlReader.Load

A linguagem XAML também pode ser usada como entrada de cadeia para o método [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048), que atua de forma análoga à operação inicial de análise da origem XAML. **XamlReader.Load** cria uma nova árvore de objetos desconectada em tempo de execução. Essa árvore pode ser anexada a algum ponto da árvore de objetos principal. Você deve conectar a árvore de objetos criada de forma explícita, seja adicionando-a a uma coleção de propriedades de conteúdo, como **Children**, ou definindo alguma outra propriedade que use um valor de objeto (por exemplo, carregar um novo [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101) para um valor de propriedade [**Fill**](https://msdn.microsoft.com/library/windows/apps/br243378)).

### <a name="xaml-namescope-implications-of-xamlreaderload"></a>Implicações de XamlReader.Load relativas ao namescope XAML

O namescope XAML preliminar definido pela nova árvore de objetos criada por [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) avalia a exclusividade de todos os nomes definidos na linguagem XAML fornecida. Se, neste ponto, esses nomes no XAML fornecido não forem exclusivos internamente, **XamlReader.Load** emitirá uma exceção. A árvore de objetos desconectada não tenta mesclar seu namescope XAML com o namescope XAML do aplicativo principal (se ou quando está conectada à árvore de objetos do aplicativo principal). Após a conexão das árvores, o aplicativo fica com uma árvore de objetos unificada, mas ela contém namescopes XAML discretos. As divisões ocorrem nos pontos de conexão entre os objetos, onde você define alguma propriedade como valor retornado de uma chamada **XamlReader.Load**.

A complicação de ter namescopes XAML discretos e desconectados é que as chamadas ao método [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) (bem como as referências de objetos gerenciados) deixam de operar em relação a um namescope XAML unificado. Em vez disso, o objeto em particular em que é chamado o **FindName** implicará o escopo, sendo esse escopo o namescope XAML em que o objeto da chamada se encontra. No caso de referência direta de objeto gerenciado, o escopo está implícito pela classe em que o código existe. Normalmente, o code-behind para interação (em tempo de execução) de uma "página" de conteúdo do aplicativo existe na classe parcial que apoia a "página" raiz. Por isso, o namescope XAML é o namescope XAML raiz.

Se você chamar [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) para obter um objeto nomeado no namescope XAML raiz, o método não encontrará os objetos de um namescope XAML discreto criado por [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048). De modo semelhante, se você chamar **FindName** de um objeto obtido do namescope XAML discreto, o método não encontrará os objetos nomeados na namescope XAML raiz.

O problema do namescope XAML discreto afeta somente a localização de objetos por nome em namescopes XAML mediante o uso da chamada [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715).

Para obter referências a objetos definidos em um namescope XAML diferente, você pode usar várias técnicas:

-   Navegue por toda a árvore em etapas discretas com propriedades de coleção e/ou [**Parent**](https://msdn.microsoft.com/library/windows/apps/br208739) cuja existência é conhecida na estrutura da árvore de objetos (como a coleção retornada por [**Panel.Children**](https://msdn.microsoft.com/library/windows/apps/br227514)).
-   Quando você faz a chamada de um namescope XAML discreto e deseja o namescope XAML raiz, é sempre fácil obter uma referência à janela principal exibida no momento. Você pode obter a raiz visual (o elemento XAML raiz, também conhecido como origem do conteúdo) da janela do aplicativo atual em uma linha de código com a chamada `Window.Current.Content`. Em seguida, você pode converter em [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) e chamar [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) a partir desse escopo.
-   Quando você faz a chamada do namescope XAML raiz e deseja um objeto em um namescope XAML discreto, a melhor coisa a fazer é planejar antecipadamente o código e manter uma referência ao objeto retornado por [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) e adicionado em seguida à árvore de objetos principal. Agora, esse objeto é válido para chamar [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) no namescope XAML discreto. Você pode manter esse objeto disponível como uma variável global ou transferi-lo usando parâmetros de método.
-   Examinando a árvore visual, é possível evitar completamente as considerações a respeito de namescope XAML e nomes. A API [**VisualTreeHelper**](https://msdn.microsoft.com/library/windows/apps/br243038) permite percorrer a árvore visual em termos de objetos pais e coleções de filhos. Isso é feito puramente com base em posições e índices.

## <a name="xaml-namescopes-in-templates"></a>Namescopes XAML em modelos

Os modelos em XAML permitem reutilizar e reaplicar conteúdo de forma simples, mas os modelos também podem incluir elementos com nomes definidos no nível de modelo. O mesmo modelo pode ser usado várias vezes em uma página. Por esse motivo, os modelos definem seus próprios namescopes XAML, independente da página que os contém, na qual o estilo ou modelo é aplicado. Considere este exemplo:

```xml
<Page
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
  xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"  >
  <Page.Resources>
    <ControlTemplate x:Key="MyTemplate">
      ....
      <TextBlock x:Name="MyTextBlock" />
    </ControlTemplate>
  </Page.Resources>
  <StackPanel>
    <SomeControl Template="{StaticResource MyTemplate}" />
    <SomeControl Template="{StaticResource MyTemplate}" />
  </StackPanel>
</Page>
```

Aqui, o mesmo modelo é aplicado a dois controles diferentes. Se os modelos não tiverem namescopes XAML discretos, o nome "MyTextBlock", usado no modelo, causará um conflito de nomes. Cada instanciação do modelo tem seu próprio namescope XAML. Por isso, neste exemplo, cada namescope XAML do modelo instanciado contém exatamente um nome. Porém, o namescope XAML raiz não contém o nome de cada modelo.

Em função dos namescopes XAML separados, localizar elementos nomeados em um modelo do escopo da página na qual o modelo é aplicado exige uma técnica diferente. Em vez de chamar [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) em algum objeto na árvore de objetos, primeiro é preciso obter o objeto com o modelo aplicado e chamar [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416). Se você for um autor de controles e estiver gerando uma convenção em que um elemento nomeado específico em um modelo aplicado é o destino de um comportamento definido para o controle, será possível usar o método **GetTemplateChild** do código de implementação de controle. O método **GetTemplateChild** é protegido. Por isso, somente o autor de controles tem acesso a ele. Além disso, há convenções que os autores de controles devem seguir para nomear partes e partes de modelos, bem como para registrá-las como valores de atributos aplicados à classe do controle. Essa técnica torna os nomes de partes importantes detectáveis aos usuários de controles que desejam aplicar um modelo diferente (o que demandaria a substituição das partes nomeadas para manter a funcionalidade do controle).

## <a name="related-topics"></a>Tópicos relacionados

* [Visão geral do XAML](xaml-overview.md)
* [Atributo x:Name](x-name-attribute.md)
* [Início rápido: modelos de controle](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)
* [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048)
* [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)
 

