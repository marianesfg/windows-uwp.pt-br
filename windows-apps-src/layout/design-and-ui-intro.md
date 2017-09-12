---
author: mijacobs
Description: "Este artigo descreve os recursos, os benefícios e os requisitos da Universal Windows Platform (UWP) de uma perspectiva de design. Descubra o que a plataforma oferece gratuitamente e as ferramentas que estão à disposição."
title: "Introdução ao design de aplicativos UWP (Plataforma Universal do Windows) (aplicativos do Windows)"
ms.assetid: 50A5605E-3A91-41DB-800A-9180717C1E86
label: Intro to UWP app design
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 8db6dbe00c20b6371ae7007f07e628d16467ea9d
ms.sourcegitcommit: 0d5b3daddb3ae74f91178c58e35cbab33854cb7f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2017
---
#  <a name="introduction-to-uwp-app-design"></a>Introdução ao design de aplicativos UWP

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Um aplicativo UWP (Plataforma Universal do Windows) pode ser executado em qualquer dispositivo baseado no Windows, como um telefone, tablet ou computador. 

Projetar um aplicativo que tenha boa aparência em uma variedade tão grande de dispositivos pode ser um grande desafio. Felizmente, a Plataforma Universal do Windows (UWP) fornece um conjunto de recursos internos e blocos de construção universais que ajudarão você a criar uma experiência do usuário que funcione bem com uma variedade de dispositivos, telas e métodos de entrada. Este artigo descreve os recursos de interface do usuário e os benefícios dos aplicativos UWP e fornece algumas diretrizes de design de alto nível para criar seu primeiro aplicativo UWP. 

## <a name="video-summary"></a>Resumo do vídeo

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Designing-Universal-Windows-Platform-apps/player]


<!--
![windows-powered devices](images/1894834-hig-device-primer-01-500.png)
-->

<!--
![A design for an app that runs on windows phone, tablets, and pcs](images/food-truck-finder/uap-foodtruck--md-detail.png)
-->


## <a name="uwp-features"></a>Recursos UWP

Vamos começar analisando alguns recursos disponíveis ao criar um aplicativo UWP.

### <a name="effective-pixels-and-scaling"></a>Dimensionamento e pixels efetivos

Os aplicativos UWP ajustam automaticamente o tamanho de controles, fontes e outros elementos da interface do usuário para que eles sejam legíveis e a interação com eles seja fácil em todos os dispositivos.

Quando seu aplicativo é executado em um dispositivo, o sistema usa um algoritmo para normalizar a forma como os elementos da interface do usuário são exibidos na tela. Esse algoritmo de dimensionamento leva em conta a distância de visualização e a densidade da tela (pixels por polegada) para otimizar o tamanho percebido (em vez do tamanho físico). O algoritmo de dimensionamento garante que uma fonte de 24 px no Surface Hub a 3 m de distância seja tão legível para o usuário quanto uma fonte de 24 px no telefone de 5" a alguns centímetros de distância.

![distâncias de visualização em diferentes dispositivos](images/1910808-hig-uap-toolkit-03.png)

Devido ao modo como o sistema de dimensionamento funciona, ao criar seu aplicativo UWP, você está criando em *pixels efetivos*, não pixels físicos reais. Portanto, como isso impacta a forma em que você projeta o seu aplicativo?

-   Você pode ignorar a densidade de pixels e a resolução de tela real ao projetar. Crie o projeto para obter a resolução efetiva (a resolução em pixels efetivos) de uma classe de tamanho (para obter detalhes, consulte o artigo [Tamanhos de tela e pontos de interrupção](screen-sizes-and-breakpoints-for-responsive-design.md)).

-   Quando o sistema escalona sua interface do usuário, ele faz isso em múltiplos de 4. Para garantir uma aparência nítida, ajuste os designs para a grade de 4x4 pixels: tornar as margens, os tamanhos e as posições dos elementos de interface do usuário um múltiplo de 4 pixels efetivos. Observe que esse requisito não se aplica ao texto; o texto pode ter qualquer tamanho e posição. 

Esta ilustração mostra os elementos de design mapeados para a grade de 4x4 pixels. O elemento de design sempre terá bordas nítidas e ajustadas.

![ajustando à grade de 4x4 pixels](images/rsp-design/epx-4pixelgood.png)

> [!TIP]
> Ao criar modelos de tela em programas de edição de imagens, defina o DPI para 72 e defina as dimensões da imagem para a resolução efetiva da classe de tamanho pretendida. Para obter uma lista de classes de tamanho e resoluções efetivas, consulte o artigo [Tamanhos de tela e pontos de interrupção](screen-sizes-and-breakpoints-for-responsive-design.md).


### <a name="universal-input-and-smart-interactions"></a>Entrada universal e interações inteligentes

Outro recurso interno da UWP é a entrada universal habilitada por meio de interações inteligentes. Embora você possa criar seus aplicativos para dispositivos e modos de entrada específicos, isso não é obrigatório. Isso ocorre porque, por padrão, os aplicativos Universais do Windows dependem de interações inteligentes. Isso significa que você pode criar uma interação de clique sem precisar saber nem definir se o clique vem de um clique do mouse real ou do toque de um dedo.

### <a name="universal-controls-and-styles"></a>Controles e estilos universais


A UWP também fornece alguns blocos de construção úteis que facilitam a criação de aplicativos para várias famílias de dispositivos.

-   **Controles universais**

    A UWP fornece um conjunto de controles universais que têm garantia de funcionar bem em todos os dispositivos com Windows. Este conjunto de controles universais inclui tudo, desde os controles de formulário comuns, como botão de opção e caixa de texto, até controles sofisticados como o modo de exibição de grade e o modo de exibição de lista, que podem gerar listas de itens com base em um fluxo de dados e um modelo. Esses controles têm reconhecimento de entrada e são implantados com o conjunto adequado de opções de entrada, estados de eventos e a funcionalidade geral da família de cada dispositivo.

    Para obter uma lista completa desses controles e os padrões que você pode criar com base neles, consulte a seção [Controles e padrões](https://dev.windows.com/design/controls-patterns).

-   **Estilos universais**

    Seu UWP app obtém automaticamente um conjunto de estilos padrão que oferece estes recursos:

    -   Um conjunto de estilos que automaticamente proporciona ao seu aplicativo um tema claro ou escuro (sua escolha) e pode incorporar a preferência de cor de destaque do usuário.

        ![temas claro e escuro](images/1910808-hig-uap-toolkit-01.png)

    -   Uma rampa de tipos baseada em Segoe que garante que o texto do aplicativo tenha aparência nítida em todos os dispositivos.
    -   Animações padrão para interações.
    -   Suporte automático para modos de alto contraste. Os nossos estilos foram criados com alto contraste em mente para que, quando executados em um dispositivo em modo de alto contraste, o seu aplicativo seja exibido corretamente.
    -   Suporte automático para outros idiomas. Nossos estilos padrão selecionam automaticamente a fonte correta para cada idioma que tem suporte no Windows. Você pode até mesmo usar vários idiomas no mesmo aplicativo, e eles serão exibidos corretamente.
    -   Suporte interno para a ordem de leitura da direita para a esquerda.

    Você pode personalizar esses estilos padrão para dar um toque pessoal ao seu aplicativo, ou pode substituí-los completamente por seus próprios estilos para criar uma experiência visual única. Por exemplo, aqui está um design de um aplicativo de previsão de tempo com um estilo visual único:

    ![um aplicativo de previsão de tempo com seu próprio estilo visual](images/weather/uwp-weather-tab-phone-700.png)

## <a name="uwp-and-the-fluent-design-system"></a>UWP e o Sistema de Design Fluent

 O Sistema de Design Fluent ajuda você a criar uma interface do usuário limpa e moderna que incorpora luz, profundidade, movimento, materiais e escala. O Design Fluent está sendo aplicado em dispositivos e aplicativos Windows 10 para criar experiências belas, intuitivas e envolventes. 
 
 Como você pode incorporar o Design Fluent ao seu aplicativo? Estamos constantemente adicionando novos controles e recursos para facilitar isso. Aqui está uma lista dos recursos atuais de Design Fluent para UWP:  

* [Acrílico](../style/acrylic.md) é um tipo de pincel que cria superfícies semitransparentes.
* [Paralaxe](../style/parallax.md) adiciona perspectiva, profundidade e movimento tridimensional à rolagem de conteúdo, como listas, por exemplo.
* [Revelar](../style/reveal.md) usa luz para criar um efeito de foco que ilumina elementos interativos da interface do usuário. 
* [Animações conectadas](../style/connected-animation.md) fornecem transições de cena suaves que melhoram a usabilidade mantendo o contexto e fornecendo continuidade. 

Também atualizamos nossas [diretrizes de design](https://developer.microsoft.com/windows/apps/design) (que você está lendo) para que elas sejam baseadas em princípios de Design Fluent.

## <a name="the-anatomy-of-a-typical-uwp-app"></a>A anatomia de um aplicativo UWP típico

Agora que descrevemos os blocos de construção de aplicativos UWP, vamos ver como reuni-los para criar uma interface do usuário.

Uma interface do usuário moderna é uma coisa complexa, composta de texto, formas, cores e animações que, em última análise, são compostas de pixels individuais da tela do dispositivo que você está usando. Quando você começa a criar uma interface do usuário, o grande número de opções pode ser complicado.

Para simplificar, vamos definir a anatomia de um aplicativo de uma perspectiva de design. Digamos que um aplicativo é composto de páginas e telas. Cada página tem uma interface do usuário, formada por três tipos de elementos de interface do usuário: elementos de navegação, comando e conteúdo.



<table class="uwpd-noborder" >
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><img src="images/1895065-hig-anatomyofanapp-02.png" alt="Navigation, command, and content areas of an address book app" /></p>
<p></p></td>
<td align="left"><strong>Elementos de navegação</strong>
<p>Os elementos de navegação ajudam os usuários a escolher o conteúdo que desejam exibir. Exemplos de elementos de navegação incluem [guias e pivôs](../controls-and-patterns/tabs-pivot.md), [hiperlinks](../controls-and-patterns/hyperlinks.md) e [painéis de navegação](../controls-and-patterns/navigationview.md).</p>
<p>Os elementos de navegação são abordados em detalhes no artigo [Noções básicas de design de navegação](navigation-basics.md).</p>
<strong>Elementos de comando</strong>
<p>Os elementos de comando iniciam ações, como manipulação, gravação ou compartilhamento de conteúdo. Exemplos de elementos de comando incluem [botão](../controls-and-patterns/buttons.md) e a [barra de comandos](../controls-and-patterns/app-bars.md). Os elementos de comando também podem incluir atalhos do teclado que não ficam realmente visíveis na tela.</p>
<p>Elementos de comando são abordados em detalhes no artigo [Noções básicas de design de comando](commanding-basics.md).</p>
<strong>Elementos de conteúdo</strong>
<p>Os elementos de conteúdo exibem o conteúdo do aplicativo. Para um aplicativo de pintura, o conteúdo pode ser um desenho; para um aplicativo de notícias, o conteúdo pode ser um artigo de notícias.</p>
<p>Os elementos de conteúdo são abordados em detalhes no artigo [Noções básicas de design do conteúdo](content-basics.md).</p></td>
</tr>
</tbody>
</table>

 

No mínimo, um aplicativo tem uma tela inicial e uma página inicial que define a interface do usuário. Um aplicativo típico tem várias páginas e telas, e os elementos de navegação, comando e conteúdo podem mudar de uma página para outra.

Ao decidir sobre os elementos da interface do usuário corretos para seu aplicativo, você também pode considerar os dispositivos e os tamanhos de tela em que o aplicativo será executado.

## <a name="tailoring-your-app-for-specific-devices-and-screen-sizes"></a>Adaptar seu aplicativo para dispositivos e tamanhos de tela específicos.

Os aplicativos UWP usam pixels efetivos para garantir que seus elementos de design serão legíveis e utilizáveis em todos os dispositivos do Windows. Sendo assim, por que você desejaria personalizar a interface do usuário do seu aplicativo para uma família de dispositivos específica?

**Observação**  
Antes de prosseguirmos, o Windows não oferece uma forma de o seu aplicativo detectar o dispositivo específico em que o está sendo executado. Ele pode informar a família de dispositivos (móvel, área de trabalho etc) em que o aplicativo está sendo executado, a resolução efetiva e a quantidade de espaço na tela disponível para o aplicativo (o tamanho da janela do aplicativo).

 

-   **Para tornar o uso do espaço mais eficiente e reduzir a necessidade de navegar**

    Se você criar um aplicativo para ter uma boa aparência em um dispositivo que tenha uma tela pequena, como um telefone, o aplicativo poderá ser utilizado em um PC com uma tela muito maior, mas provavelmente haverá algum desperdício de espaço. Você pode personalizar o aplicativo para exibir mais conteúdo quando a tela tiver acima de um determinado tamanho. Por exemplo, um aplicativo de compras pode exibir uma categoria de mercadoria de cada vez em um telefone, mas mostrar várias categorias e produtos simultaneamente em um PC ou laptop.

    Ao colocar mais conteúdo na tela, você reduz a quantidade de navegação que o usuário precisa executar.

-   **Para tirar proveito dos recursos dos dispositivos**

    Certos dispositivos têm mais probabilidade de ter determinados recursos de dispositivo. Por exemplo, telefones têm probabilidade de ter um sensor de localização e uma câmera, enquanto um computador pode não ter nenhum dos dois. Seu aplicativo pode detectar quais recursos estão disponíveis e habilitar recursos que os utilizam.

-   **Otimizar para entrada**

    A biblioteca de controles universais funciona com todos os tipos de entrada (toque, caneta, teclado, mouse), mas você ainda pode otimizar para certos tipos de entrada reorganizando seus elementos de interface do usuário. Por exemplo, se você colocar elementos de navegação na parte inferior da tela, será mais fácil para os usuários de telefone acessá-los, mas a maioria dos usuários de computador espera ver os elementos de navegação na parte superior da tela.

## <a name="responsive-design-techniques"></a>Técnicas de design responsivo


Quando você otimiza a interface do usuário de seu aplicativo para larguras de tela específicas, dizemos que você está criando um design responsivo. Aqui estão as seis técnicas de design responsivo que você pode usar para personalizar a interface do usuário do seu aplicativo.

### <a name="reposition"></a>Reposicionar

Você pode alterar o local e a posição dos elementos de interface do usuário do aplicativo para obter o máximo de cada dispositivo. Neste exemplo, o modo de exibição retrato no telefone ou phablet necessita de uma interface do usuário de rolagem, pois somente um quadro completo é visível por vez. Quando o aplicativo é convertido em um dispositivo que permite dois quadros completos na tela, seja na orientação retrato ou paisagem, o quadro B pode ocupar um espaço dedicado. Se você estiver usando uma grade para posicionamento, poderá manter a mesma grade quando os elementos da interface do usuário forem reposicionados.

![Reposicionar](images/rsp-design/rspd-reposition.png)

Neste design de exemplo para um aplicativo de fotos, o aplicativo de fotos reposiciona seu conteúdo em telas maiores.

![Um projeto para um aplicativo que reposiciona conteúdo em telas maiores](images/rsp-design/rspd-reposition-type1.png)

### <a name="resize"></a>Redimensionar

Você pode otimizar o tamanho de quadro, ajustando as margens e o tamanho dos elementos da interface do usuário. Isso poderia permitir que você, como o exemplo aqui mostra, aumentasse a experiência de leitura em uma tela maior, simplesmente aumentando o quadro de conteúdo.

![Redimensionando elementos de design](images/rsp-design/rspd-resize.png)

### <a name="reflow"></a>Refluxo

Alterando o fluxo de elementos da interface do usuário com base no dispositivo e na orientação, seu aplicativo pode oferecer uma exibição ideal do conteúdo. Por exemplo, ao passar para uma tela maior, talvez faça sentido alternar contêineres maiores, adicionar colunas e gerar itens de lista de maneira diferente.

Este exemplo mostra como uma única coluna de conteúdo de rolagem vertical no telefone ou phablet pode refluir em uma tela maior para exibir duas colunas de texto.

![Refluindo elementos de design](images/rsp-design/rspd-reflow.png)

### <a name="showhide"></a>Mostrar/ocultar

Você pode mostrar ou ocultar elementos da interface do usuário com base no estado real da tela ou quando o dispositivo oferecer suporte a funcionalidades adicionais, situações específicas ou orientações de tela preferenciais.

Neste exemplo com guias, a guia do meio com o ícone de câmera pode ser específica ao aplicativo no telefone ou phablet e não ser aplicável a dispositivos maiores; é por isso que ela é revelada no dispositivo à direita. Outro exemplo comum de revelar ou ocultar a interface do usuário se aplica aos controles do media player, onde o conjunto de botões é reduzido em dispositivos menores e expandido em dispositivos maiores. O media player no computador, por exemplo, pode lidar com muito mais funcionalidades na tela do que em um telefone.

![Ocultando elementos de design](images/rsp-design/rspd-revealhide.png)

Parte da técnica de revelar ou ocultar inclui escolher quando exibir mais metadados. Quando o estado real é fundamental, como em um telefone ou phablet, é melhor mostrar uma quantidade mínima de metadados. Em um notebook ou computador desktop, uma quantidade significativa de metadados pode ser mostrada. Alguns exemplos de mostrar ou ocultar metadados incluem:

-   Em um aplicativo de email, você pode exibir o avatar do usuário.
-   Em um aplicativo de música, você pode exibir mais informações sobre um álbum ou artista.
-   Em um aplicativo de vídeo, você pode exibir mais informações sobre um filme ou um programa, como mostrar detalhes do elenco e da equipe.
-   Em qualquer aplicativo, você pode separar colunas e revelar mais detalhes.
-   Em qualquer aplicativo, você pode pegar algo que esteja empilhado verticalmente e dispor horizontalmente. Ao passar do telefone ou phablet para dispositivos maiores, os itens de lista empilhados podem mudar para revelar as linhas de itens de lista e colunas de metadados.

### <a name="replace"></a>Substituir

Esta técnica permite que você alterne a interface do usuário de uma classe de tamanho de dispositivo ou orientação específica. Neste exemplo, o painel de navegação e sua interface de usuário compacta, transitória, funciona bem em um dispositivo menor, mas em um dispositivo maior, guias podem ser uma melhor opção.

![Substituindo elementos de design](images/rsp-design/rspd-replace.png)

###  <a name="re-architect"></a>Rearquitetar

Você pode recolher ou bifurcar a arquitetura do seu aplicativo para melhor direcionar dispositivos específicos. Neste exemplo, ir do dispositivo à esquerda para o dispositivo à direita demonstra a junção de páginas.

![um exemplo de rearquitetura de uma interface do usuário](images/rsp-design/rspd-rearchitect.png)

Veja um exemplo dessa técnica aplicada ao design para um aplicativo inicial inteligente.

![um exemplo de um design que usa a técnica de design responsivo de rearquitetura](images/rsp-design/rspd-rearchitect-type1.png)

## <a name="tools-and-design-toolkits"></a>Ferramentas e kits de ferramentas de design

Fornecemos uma variedade de ferramentas para ajudá-lo a projetar seu aplicativo UWP. Consulte nossa [página de kits de ferramentas de design](../design-downloads/index.md) para ter acesso a kits de ferramentas do XD, Illustrator, Photoshop, Framer e Sketch, bem como ferramentas de design adicionais e download de fontes. 

Para configurar sua máquina para que ela realmente codifique aplicativos UWP, consulte nosso artigo [Introdução &gt; Prepare-se para começar](../get-started/get-set-up.md). 

## <a name="related-articles"></a>Artigos relacionados

- [O que é um aplicativo UWP?](https://msdn.microsoft.com/library/windows/apps/dn726767.aspx)
- [Kits de ferramentas de design](../design-downloads/index.md)

 
