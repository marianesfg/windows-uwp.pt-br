---
author: jwmsft
title: Análise de aplicativo
description: Analise seu aplicativo para os problemas de desempenho.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 346e6790c6578bf861ba1dda937eae6d4d50f00f
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7429385"
---
# <a name="app-analysis-overview"></a>Visão geral da análise de aplicativo

Análise de aplicativo é uma ferramenta que fornece aos desenvolvedores uma notificação de ocorrência de problemas de desempenho. A Análise de aplicativos executa seu código de aplicativos em um conjunto de práticas recomendadas e diretrizes de desempenho.

A Análise de aplicativo identifica problemas decorrentes de problemas comuns de desempenho nos quais os aplicativos executam. Quando apropriado, a análise de aplicativo apontará para a ferramenta de linha do tempo, informações de origem e documentação do Visual Studio para lhe fornecer os meios para investigar.

As regras de Análise de aplicativo se referem a uma diretriz ou a práticas recomendadas com base nas quais seu aplicativo está sendo verificado.

## <a name="decoded-image-size-larger-than-render-size"></a>Tamanho da imagem decodificada maior do que o tamanho de renderização

As imagens são capturadas em resoluções muito altas, o que pode resultar em aplicativos usando mais recursos de CPU ao decodificar os dados da imagem e mais memória depois que ela é carregada do disco. Porém, não faz sentido decodificar e salvar uma imagem de alta resolução na memória somente para exibi-la em um tamanho menor do que o nativo. Em vez disso, crie uma versão da imagem no tamanho exato que ela será desenhada na tela usando as propriedades DecodePixelWidth e DecodePixelHeight.

### <a name="impact"></a>Impacto

Exibir imagens em seus tamanhos não nativos pode afetar negativamente o tempo de CPU (por causa da decodificação para o tempo de download e tamanho adequado) e memória.

### <a name="causes-and-solutions"></a>Causas e soluções

#### <a name="image-is-not-being-set-asynchronously"></a>A imagem não está sendo definida assincronamente

O aplicativo está usando SetSource() em vez de SetSourceAsync(). Você sempre deve evitar usar [**SetSource**](https://msdn.microsoft.com/library/windows/apps/BR243255) e usar, ao invés, [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522) quando estiver definindo um stream para decodificar imagens de forma assíncrona. 

#### <a name="image-is-being-called-when-the-imagesource-is-not-in-the-live-tree"></a>A imagem está sendo chamada quando o ImageSource não está em uma árvore ativa

O BitmapImage está conectado à árvore XAML ativa após a configuração do conteúdo com SetSourceAsync ou UriSource. Você sempre deve anexar uma [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/BR243235) à árvore ativa antes de definir a origem. Sempre que um elemento de imagem ou um pincel for especificado na marcação, esse será automaticamente o caso. Exemplos são fornecidos abaixo. 

**Exemplos de árvore ativa**

Exemplo 1 (bom) - Uniform Resource Identifier (URI) especificado em marcação.

```xml
<Image x:Name="myImage" UriSource="Assets/cool-image.png"/>
```

Exemplo 2 marcação - URI especificado em code-behind.

```xml
<Image x:Name="myImage"/>
```

Exemplo 2 code-behind (bom) - conectando o BitmapImage à arvore antes de definir o UriSource.

```vb
var bitmapImage = new BitmapImage();
myImage.Source = bitmapImage;
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
```

Exemplo 2 code-behind (ruim) — Configurando o UriSource do BitmapImage antes de conectá-lo à árvore.

```vb
var bitmapImage = new BitmapImage();
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
myImage.Source = bitmapImage;
```

#### <a name="image-brush-is-non-rectangular"></a>O pincel de imagem não é retangular 

Quando uma imagem é usada para um pincel não retangular, a imagem usará um caminho de rasterização por software, que não dimensionará as imagens de forma alguma. Além disso, ele deve armazenar uma cópia da imagem na memória de hardware e software. Por exemplo, se uma imagem for usada como um pincel para uma elipse, a imagem inteira potencialmente grande será armazenada duas vezes internamente. Ao usar um pincel não retangular, seu aplicativo deverá pré-dimensionar suas imagens para aproximadamente o tamanho em que elas serão renderizadas.

Como alternativa, você pode definir um tamanho de decodificação explícito para criar uma versão da imagem no tamanho exato que ela será desenhada na tela usando as propriedades [ **DecodePixelWidth** ](https://msdn.microsoft.com/library/windows/apps/BR243243) e [ **DecodePixelHeight** ](https://msdn.microsoft.com/library/windows/apps/BR243241).

```xml
<Image>
    <Image.Source>
    <BitmapImage UriSource="ms-appx:///Assets/highresCar.jpg" 
                 DecodePixelWidth="300" DecodePixelHeight="200"/>
    </Image.Source>
</Image>
```

As unidades para [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) e [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) são, por padrão, pixels físicos. A propriedade [**DecodePixelType**](https://msdn.microsoft.com/library/windows/apps/Dn298545) pode ser usada para alterar esse comportamento: definir **DecodePixelType** como **Logical** resulta no tamanho de decodificação respondendo automaticamente pelo fator de escala atual do sistema, similar a outros conteúdos XAML. Portanto, em geral, seria adequado definir **DecodePixelType** como **Logical**, se, por exemplo, você quiser que **DecodePixelWidth** e **DecodePixelHeight** correspondam às propriedades de altura e largura do controle de imagem em que a imagem será exibida. Com o comportamento padrão de uso de pixels físicos, você deve considerar o fator de escala atual do sistema por conta própria; e deve detectar as notificações de alteração de escala no caso de o usuário alterar suas preferências de exibição.

Em alguns casos em que um tamanho de decodificação apropriado não puder ser determinado antecipadamente, você deverá consultar a decodificação automática de tamanho correto do XAML, que fará o máximo para tentar decodificar a imagem no tamanho apropriado se um DecodePixelWidth/DecodePixelHeight explícito não for especificado.

Você deve definir um tamanho de decodificação explícito, se souber o tamanho do conteúdo da imagem com antecedência. Você também deve definir, juntamente, [**DecodePixelType**](https://msdn.microsoft.com/library/windows/apps/Dn298545) como **Logical**, se o tamanho de decodificação fornecido for relativo a outros tamanhos de elemento XAML. Por exemplo, se você definir explicitamente o tamanho do conteúdo com Image.Width e Image.Height, poderá definir DecodePixelType como DecodePixelType.Logical para usar as mesmas dimensões de pixel lógico como um controle Image e, em seguida, usar explicitamente BitmapImage.DecodePixelWidth e/ou BitmapImage.DecodePixelHeight para controlar o tamanho da imagem para obter uma economia de memória potencialmente grande.

Observe que Image.Stretch devem ser considerado ao determinar o tamanho do conteúdo decodificado.

#### <a name="images-used-inside-of-bitmapicons-fall-back-to-decoding-to-natural-size"></a>Imagens usadas dentro do BitmapIcons retornam para a decodificação de tamanho natural 

Defina um tamanho de decodificação explícito para criar uma versão da imagem no tamanho exato que ela será desenhada na tela usando as propriedades [ **DecodePixelWidth** ](https://msdn.microsoft.com/library/windows/apps/BR243243) e [ **DecodePixelHeight** ](https://msdn.microsoft.com/library/windows/apps/BR243241).

#### <a name="images-that-appear-extremely-large-on-screen-fall-back-to-decoding-to-natural-size"></a>Imagens que aparecem muito grandes na tela retornam para decodificação de tamanho natural 

Imagens que aparecem muito grandes na tela retornam para decodificação de tamanho natural. Defina um tamanho de decodificação explícito para criar uma versão da imagem no tamanho exato que ela será desenhada na tela usando as propriedades [ **DecodePixelWidth** ](https://msdn.microsoft.com/library/windows/apps/BR243243) e [ **DecodePixelHeight** ](https://msdn.microsoft.com/library/windows/apps/BR243241).

#### <a name="image-is-hidden"></a>Imagem oculta

A imagem é oculta definindo Opacity como 0 ou Visibility como Collapsed no elemento de imagem host, no pincel ou em qualquer elemento pai. Imagens não visíveis na tela por causa de corte ou transparência podem retornar para a decodificação de tamanho natural. 

#### <a name="image-is-using-ninegrid-property"></a>A imagem está usando a propriedade NineGrid

Quando uma imagem é usada para um [**NineGrid**](https://msdn.microsoft.com/library/windows/apps/BR242756), ela usará um caminho de rasterização de software, que não dimensionará imagens de jeito nenhum. Além disso, ele deve armazenar uma cópia da imagem na memória de hardware e software. Quando for usar um **NineGrid**, seu aplicativo deve pré-dimensionar as imagens para aproximadamente o tamanho em que elas serão renderizadas.

Imagens que usam a propriedade NineGrid retornam para a decodificação de tamanho natural. Considere a adição do efeito ninegrid à imagem original.

#### <a name="decodepixelwidth-or-decodepixelheight-are-set-to-a-size-thats-larger-than-the-image-will-appear-on-screen"></a>DecodePixelWidth ou DecodePixelHeight são definidas para um tamanho maior que a imagem que aparecerá na tela 

Se DecodePixelWidth/Height forem configurados explicitamente maiores do que a imagem que será exibida na tela, então o aplicativo vai, desnecessariamente, usar memória extra, até 4 bytes por pixel, o que se torna rapidamente caro para imagens grandes. A imagem também será dimensionada para baixo usando dimensionamento bilinear, o que pode fazer com que ela pareça embaçada para fatores de escala grandes.

#### <a name="image-is-decoded-as-part-of-producing-a-drag-and-drop-image"></a>A imagem é decodificada como parte de produção de uma imagem de arrastar e soltar

Defina um tamanho de decodificação explícito para criar uma versão da imagem no tamanho exato que ela será desenhada na tela usando as propriedades [ **DecodePixelWidth** ](https://msdn.microsoft.com/library/windows/apps/BR243243) e [ **DecodePixelHeight** ](https://msdn.microsoft.com/library/windows/apps/BR243241).

## <a name="collapsed-elements-at-load-time"></a>Elementos recolhidos em tempo de carregamento

Um padrão comum em apps é ocultar elementos da interface do usuário inicialmente e mostrá-las posteriormente. Na maioria dos casos, esses elementos devem ser adiados usando x:Load ou x:DeferLoadStrategy para evitar pagar o custo de criar o elemento em tempo de carregamento.

Isso inclui os casos em que um booleano para conversor de visibilidade é usado para ocultar itens até um momento posterior.

### <a name="impact"></a>Impacto

Elementos recolhidos são carregados junto com outros elementos e contribuem para um aumento no tempo de carregamento.

### <a name="cause"></a>Causa

Essa regra foi disparada porque um elemento foi recolhido em tempo de carregamento. Recolher um elemento ou definir sua opacidade como 0 não impede que o elemento seja criado. Essa regra pode ser causada por um aplicativo que usa um valor booleano para o conversor de visibilidade padrão é false.

### <a name="solution"></a>Solução

Usando [x:Load attribute](../xaml-platform/x-load-attribute.md) ou [x:DeferLoadStrategy](https://msdn.microsoft.com/library/windows/apps/Mt204785), você pode atrasar o carregamento de uma parte da interface do usuário e carregá-la quando necessário. Isso é bom para atrasar a interface do usuário de processamento que não é visível no primeiro quadro. Você pode optar por carregar o elemento quando necessário ou como parte de um conjunto de lógica atrasada. Para disparar o carregamento, chame findName no elemento que você deseja carregar. x:Load estende os recursos de x:DeferLoadStrategy, permitindo que os elementos sejam descarregados e que o estado de carregamento seja controlado via x:Bind.

Em alguns casos, usar findName para mostrar uma parte da interface do usuário pode não ser a solução. Isso é verdadeiro se você está esperando obter uma parte significativa da interface do usuário no clique de um botão com latência muito baixa. Nesse caso, convém compensar a latência de interfaces do usuário mais rápidas às custas de memória adicional; nesse caso, você deve usar X:DeferLoadStrategy e definir Visibility como Collapsed no elemento que deseja observar. Depois que a página tiver sido carregada e o thread da interface do usuário estiver livre, você poderá chamar findName quando necessário para carregar os elementos. Os elementos não ficarão visíveis para o usuário até que você defina a Visibilidade do elemento como Visible.

## <a name="listview-is-not-virtualized"></a>ListView não está virtualizado

A virtualização da interface do usuário é o aprimoramento mais importante que você pode fazer para melhorar o desempenho da coleta. Isso significa que os elementos de interface do usuário que representam os itens são criados por demanda. Para uma associação de controle de itens para uma coleção de 1.000 itens, seria um desperdício de recursos criar a interface do usuário para todos os itens ao mesmo tempo, pois eles não podem ser todos exibidos ao mesmo tempo. ListView e GridView (e outros controles derivados de ItemsControl padrão) executam a virtualização de interface do usuário para você. Quando os itens estão quase sendo rolados para a exibição (a algumas páginas distância), a estrutura gera a interface do usuário para os itens e os armazena em cache. Quando torna-se improvável que os itens sejam mostrados novamente, a estrutura recupera a memória.

A virtualização de interface do usuário é apenas um dos vários fatores importantes para melhorar o desempenho da coleta. A redução da complexidade dos itens de coleta e a virtualização de dados são os dois outros aspectos importantes para melhorar o desempenho da coleta. Para obter mais informações sobre como melhorar o desempenho da coleta dentro de ListViews e GridViews, consulte os artigos em [Otimização da interface do usuário de ListView e GridView](https://msdn.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview) e [Virtualização de dados de ListView e Gridview](https://msdn.microsoft.com/windows/uwp/debug-test-perf/listview-and-gridview-data-optimization).

### <a name="impact"></a>Impacto

Um ItemsControl não virtualizado aumentará o uso de recursos e o tempo de carga carregando mais dos seus itens secundários do que o necessário.

### <a name="cause"></a>Causa

O conceito de um visor é crítico para a virtualização da interface do usuário, pois a estrutura precisa criar os elementos que provavelmente serão exibidos. No geral, o visor de um ItemsControl é a extensão do controle lógico. Por exemplo, o visor de uma ListView é a largura e a altura do elemento ListView. Alguns painéis permitem espaço ilimitado para elementos filho, como exemplo há ScrollViewer e um Grid, com linhas ou colunas dimensionadas automaticamente. Quando um ItemsControl virtualizado é colocado em um painel assim, ele ocupa espaço suficiente para exibir todos os itens, o que destrói a virtualização. 

### <a name="solution"></a>Solução

Restaure a virtualização configurando uma largura e uma altura no ItemsControl que você está usando.

## <a name="ui-thread-blocked-or-idle-during-load"></a>Thread de interface do usuário bloqueada ou ociosa durante o carregamento

O bloqueio do thread de interface do usuário se refere a chamadas síncronas para funções que executam fora do thread que bloqueia o thread da interface do usuário.  

Para obter uma lista completa das práticas recomendadas para melhorar o desempenho de inicialização do seu aplicativo, consulte as [Práticas recomendadas para o desempenho de inicialização do seu aplicativo](https://msdn.microsoft.com/windows/uwp/debug-test-perf/best-practices-for-your-app-s-startup-performance) e [Manter o thread de interface do usuário responsivo](https://msdn.microsoft.com/windows/uwp/debug-test-perf/keep-the-ui-thread-responsive).

### <a name="impact"></a>Impacto

Um thread de interface do usuário bloqueado ou ocioso durante o tempo de carregamento impedirá o layout e outras operações de interface do usuário, aumentando o tempo de inicialização.

### <a name="cause"></a>Causa

O código da plataforma para a interface do usuário e o código do aplicativo para a interface do usuário são executados no mesmo thread de interface do usuário. Apenas uma instrução pode ser executada nesse thread de cada vez. Então, se o código do aplicativo demorar muito para processar um evento, a estrutura não poderá executar o layout ou acionar novos eventos que representam a interação do usuário. A capacidade de resposta do seu aplicativo está relacionada à disponibilidade do thread de interface do usuário para processar o trabalho.

### <a name="solution"></a>Solução

Seu aplicativo pode ser interativo mesmo que haja partes do aplicativo que não estão totalmente funcionais. Por exemplo, se o seu aplicativo exibe dados que levam algum tempo para serem recuperados, você poderá fazer com que o código seja executado de forma independente do código de inicialização do aplicativo, recuperando os dados de forma assíncrona. Quando os dados estiverem disponíveis, preencha a interface do usuário do aplicativo com os dados. Para ajudar a manter a capacidade de resposta de seu aplicativo, a plataforma fornece versões assíncronas de várias APIs. Uma API assíncrona impede que seu thread de execução ativo bloqueie por muito tempo. Para chamar uma API do thread da interface do usuário, use a versão assíncrona, se ela estiver disponível.

## <a name="binding-is-being-used-instead-of-xbind"></a>{Binding} está sendo usado em vez de {x: Bind}

Esta regra é acionada quando seu aplicativo usa uma instrução {Binding}. Para melhorar o desempenho do aplicativo, os aplicativos devem considerar usar {x: Bind}.

### <a name="impact"></a>Impacto

{Binding} é executado precisa de mais tempo e mais memória para executar do que {x: Bind}.

### <a name="cause"></a>Causa

O aplicativo está usando {Binding} em vez de {x: Bind}. {Binding} traz com ele um conjunto de trabalho não trivial e sobrecarga para a CPU. Criar uma extensão {Binding} causa uma série de alocações, e atualizar um destino de associação pode causar reflexão e conversão boxing.

### <a name="solution"></a>Solução

Use a extensão de marcação {x: Bind}, que compila as associações no momento da compilação. As associações {x:Bind} (normalmente chamadas de associações compiladas) têm excelente desempenho, fornecem validação de tempo de compilação de suas expressões de associação e dão suporte à depuração permitindo que você defina pontos de interrupção nos arquivos de código que são gerados como a classe parcial para sua página. 

Observe que x:Bind não é adequado em todos os casos, como cenários de associação tardia. Para obter uma lista completa dos casos não cobertos por {x:Bind}, consulte a documentação do {x:Bind}.

## <a name="xname-is-being-used-instead-of-xkey"></a>x:Name está sendo usado em vez de x:Key

ResourceDictionaries geralmente são usados para armazenar seus recursos em um nível um pouco global, ou seja, os recursos aos quais seu aplicativo fará referência em vários lugares; por exemplo, estilos, pincéis, modelos e assim por diante. No geral, otimizamos ResourceDictionaries para não instanciar recursos, a menos que seja solicitado. Mas há alguns lugares onde você precisa ter um pouco de cuidado.

### <a name="impact"></a>Impacto

Qualquer recurso com X:Name será instanciado assim que ResourceDictionary for criado. Isso ocorre porque x:Name instrui a plataforma de que seu aplicativo precisa de acesso de campo a esse recurso de forma que a plataforma precise criar algo para o qual irá criar um referência.

### <a name="cause"></a>Causa

Seu aplicativo está definindo X:Name em um recurso.

### <a name="solution"></a>Solução

Use X:Key em vez de X:Name quando não estiver fazendo referência a recursos a partir de code-behind.

## <a name="collections-control-is-using-a-non-virtualizing-panel"></a>O controle de coletas está usando um painel sem virtualização

Se você oferece um modelo de painel de itens personalizado (consulte ItemsPanel), certifique-se de usar um painel de virtualização como ItemsWrapGrid ou ItemsStackPanel. Se você usar VariableSizedWrapGrid, WrapGrid ou StackPanel, não obterá a virtualização. Além disso, os seguintes eventos ListView serão gerados somente quando você estiver usando um ItemsWrapGrid ou um ItemsStackPanel: ChoosingGroupHeaderContainer, ChoosingItemContainer e ContainerContentChanging.

A virtualização da interface do usuário é o aprimoramento mais importante que você pode fazer para melhorar o desempenho da coleta. Isso significa que os elementos de interface do usuário que representam os itens são criados por demanda. Para uma associação de controle de itens para uma coleção de 1.000 itens, seria um desperdício de recursos criar a interface do usuário para todos os itens ao mesmo tempo, pois eles não podem ser todos exibidos ao mesmo tempo. ListView e GridView (e outros controles derivados de ItemsControl padrão) executam a virtualização de interface do usuário para você. Quando os itens estão quase sendo rolados para a exibição (a algumas páginas distância), a estrutura gera a interface do usuário para os itens e os armazena em cache. Quando torna-se improvável que os itens sejam mostrados novamente, a estrutura recupera a memória.

A virtualização de interface do usuário é apenas um dos vários fatores importantes para melhorar o desempenho da coleta. A redução da complexidade dos itens de coleta e a virtualização de dados são os dois outros aspectos importantes para melhorar o desempenho da coleta. Para obter mais informações sobre como melhorar o desempenho da coleta dentro de ListViews e GridViews, consulte os artigos em [Otimização da interface do usuário de ListView e GridView](https://msdn.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview) e [Virtualização de dados de ListView e Gridview](https://msdn.microsoft.com/windows/uwp/debug-test-perf/listview-and-gridview-data-optimization).

### <a name="impact"></a>Impacto

Um ItemsControl não virtualizado aumentará o uso de recursos e o tempo de carga carregando mais dos seus itens secundários do que o necessário.

### <a name="cause"></a>Causa

Você está usando um painel que não oferece suporte à virtualização.

### <a name="solution"></a>Solução

Use um painel de virtualização como ItemsWrapGrid ou ItemsStackPanel.

## <a name="accessibility-uia-elements-with-no-name"></a>Acessibilidade: elementos UIA sem nome

Em XAML, você pode fornecer um nome, definindo AutomationProperties.Name. Vários pares de automação fornecem um nome padrão para UIA se AutomationProperties.Name está definido. 

### <a name="impact"></a>Impacto

Se um usuário atingir um elemento sem nome, ele geralmente não terá nenhuma maneira de saber a que o elemento está relacionado. 

### <a name="cause"></a>Causa

O nome UIA do elemento é nulo ou está em branco. Esta regra verifica o que o UIA vê, não o valor de AutomationProperties.Name.

### <a name="solution"></a>Solução

Defina a propriedade AutomationProperties.Name no XAML do controle para uma cadeia de caracteres localizada apropriada.

Às vezes, a correção de aplicativo não é fornecer um nome, e sim remover o elemento UIA de tudo, com exceção das árvores brutas. Você pode fazer isso no XAML, definindo AutomationProperties.AccessibilityView = “Raw”.

## <a name="accessibility-uia-elements-with-the-same-controltype-should-not-have-the-same-name"></a>Acessibilidade: elementos UIA com o mesmo Controltype não devem ter o mesmo nome

Dois elementos UIA com o mesmo pai UIA não devem ter o mesmo Nome e ControlType. Há problema em ter dois controles com o mesmo Nome que tenham ControlTypes diferentes. 

Essa regra não verifica nomes duplicados com pais diferentes. No entanto, na maioria dos casos, você não deve duplicar Nomes e ControlTypes dentro de uma janela inteira, mesmo com pais diferentes. Os casos nos quais os nomes duplicados em uma janela são aceitáveis estão duas listas com itens idênticos. Nesse caso, os itens da lista devem ter Nomes e ControlTypes idênticos.

### <a name="impact"></a>Impacto

Se um usuário atingir um elemento com o mesmo Nome e ControlType como outro elemento com o mesmo pai UIA, o usuário não poderá observar a diferença entre os elementos.

### <a name="cause"></a>Causa

Dois elementos UIA com o mesmo pai UIA têm o mesmo Nome e ControlType.

### <a name="solution"></a>Solução

Defina um nome em XAML usando AutomationProperties.Name. Em listas nas quais isso normalmente ocorre, use a associação para associar o valor de AutomationProperties.Name a uma fonte de dados.


