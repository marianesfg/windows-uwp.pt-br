---
title: Layout de página para aplicativos UWP
description: Ao projetar seu aplicativo, a primeira coisa a considerar é a estrutura de layout. Este artigo aborda a estrutura comum dos layouts de página básica, incluindo quais elementos de interface do usuário que você precisará e onde eles devem ir em uma página. Em aplicativos UWP, cada página geralmente tem elementos de conteúdo, command e navegação.
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.openlocfilehash: edda9948e70b1757ddb46fae45a579bb2fdb8de1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633711"
---
# <a name="page-layout"></a>Layout de página

Em aplicativos UWP, cada [ **página** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) geralmente tem elementos de conteúdo, command e navegação. 

Seu aplicativo pode ter várias páginas: quando um usuário inicia um aplicativo UWP, o código do aplicativo cria uma [ **quadro** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) colocar dentro do aplicativo [ **janela** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window). O quadro pode então [navegue](../basics/navigate-between-two-pages.md) entre o aplicativo [ **página** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) instâncias. 

A maioria das páginas seguem uma estrutura de layout comum, e este artigo aborda qual interface do usuário elementos que você precisará e onde eles devem ir em uma página. 

![estrutura da página](images/page-components.svg)

## <a name="navigation"></a>Navegação
O layout do aplicativo começa com o modelo de navegação que você escolher, que define como os usuários navegam entre páginas em seu aplicativo. Neste artigo, abordaremos dois padrões comuns de navegação: esquerda da barra de navegação e menu de navegação superior. Para obter orientação sobre como escolher outras opções de navegação, consulte [Noções básicas sobre design de navegação para aplicativos UWP](../basics/navigation-basics.md).

![padrões de navegação superior e esquerda](images/top-left-nav.svg)

### <a name="left-nav"></a>Barra de navegação esquerda
Deixado nav, ou o [painel de navegação](../controls-and-patterns/navigationview.md) padrão, é geralmente está reservado para navegação de nível de aplicativo e existe no nível mais alto no aplicativo, que significa que ela deve ser sempre visível e disponível. Recomendamos a barra de navegação esquerda quando há mais de cinco itens de navegação ou mais de cinco páginas em seu aplicativo. O padrão do painel de navegação normalmente contém:
- Itens de navegação
- Ponto de entrada para as configurações do aplicativo
- Ponto de entrada para as configurações de conta

O [NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview) controle implementa o padrão de navegação à esquerda para a UWP.

Quando um item de navegação é selecionado, o quadro deve navegar para a página do item selecionado.

![Painel de navegação expandido](images/navview-expanded.svg)

O botão de menu permite que os usuários expandir e recolher o painel de navegação. Quando o tamanho da tela é maior que 640 px, clicando no botão de menu recolhe o painel de navegação em uma barra.

![Painel de navegação compact](images/navview-compact.svg)

Quando o tamanho da tela é menor do que 640 px, o painel de navegação totalmente é recolhido.

![Painel de navegação mínima](images/navview-minimal.svg)

### <a name="top-nav"></a>Top nav

Barra de navegação superior também pode atuar como navegação de nível superior. Enquanto a barra de navegação esquerda é recolhível, barra de navegação superior está sempre visível. O [NavigationView](../controls-and-patterns/navigationview.md) controle implementa o padrão de navegação e guias principais para UWP.

![Painel de navegação superior](images/pivot-large.svg)

## <a name="command-bar"></a>Barra de comandos

Em seguida, você talvez queira fornecer aos usuários acesso fácil a tarefas mais comuns do seu aplicativo. Um [barra de comandos](../controls-and-patterns/app-bars.md) pode fornecer acesso aos comandos de nível de aplicativo ou página, e ele pode ser usado com qualquer padrão de navegação.

![posicionamento da barra de comando na parte superior ](images/app-bar-desktop.svg)

Barra de comandos pode ser colocada na parte superior ou inferior da página, o que for melhor para seu aplicativo.

![posicionamento da barra de comando na parte inferior](images/app-bar-mobile.svg)

## <a name="content"></a>Conteúdo

Por fim, conteúdo varia amplamente de aplicativo para aplicativo, portanto, você pode apresentar o conteúdo de muitas maneiras diferentes. Aqui, descrevemos alguns padrões comuns de página que você talvez queira usar em seu aplicativo. Muitos aplicativos usam alguns ou todos esses padrões comuns de página para exibir diferentes tipos de conteúdo. Da mesma forma, fique à vontade para misturar e combinar esses padrões para otimizar o seu aplicativo.

## <a name="landing"></a>Inicial

![Página de aterrissagem](images/hero-screen.svg)

Páginas de aterrissagem, também conhecidas como telas de celebridades, geralmente aparecem no nível superior de uma experiência de aplicativo. A área de superfície grande serve como um palco de aplicativos para destacar o conteúdo que os usuários podem querer procurar e consumir.

## <a name="collections"></a>Coleções

![galeria](images/gridview.svg)

Coleções permitem que os usuários naveguem grupos de conteúdo ou dados. [Modo de exibição de grade](../controls-and-patterns/item-templates-gridview.md) é uma boa opção para fotos ou conteúdo centrado em mídia, e [exibição de lista](../controls-and-patterns/item-templates-listview.md) é uma boa opção para conteúdo de uso intenso de texto ou dados.

## <a name="masterdetail"></a>Mestre/detalhes

![mestre/detalhes](images/master-detail.svg)

O modelo [mestre/detalhes](../controls-and-patterns/master-details.md) consiste em uma exibição de lista (mestre) e um modo de exibição de conteúdo (detalhes). Ambos os painéis são fixos e têm a rolagem vertical. Quando um item na exibição de lista é selecionado, a exibição de conteúdo é atualizada de forma correspondente. 

## <a name="forms"></a>Formulários
![formulário](images/form.svg)

Um [formulário](../controls-and-patterns/forms.md) é um grupo de controles que coleta e envia dados de usuários. A maioria, se não todos os aplicativos, usam uma forma de páginas de configurações, portais de logon, hubs de comentários, criação de conta ou outros fins. 

## <a name="sample-apps"></a>Exemplos de aplicativos
Para ver como esses padrões podem ser implementados, Confira nossos [aplicativos de exemplo do UWP](https://developer.microsoft.com/en-us/windows/samples):
- [BuildCast Video Player](https://github.com/Microsoft/BuildCast)
- [Lunch Scheduler](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
- [Livro de Colorir](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [Banco de dados de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database)