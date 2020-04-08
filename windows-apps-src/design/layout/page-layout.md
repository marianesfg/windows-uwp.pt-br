---
title: Layout da página para aplicativos UWP
description: Ao criar seu aplicativo, o primeiro aspecto a ser considerado é a estrutura do layout. Este artigo aborda a estrutura comum de layouts de página básicos, incluindo quais elementos de interface do usuário você precisará e onde eles devem ir em uma página. Em aplicativos UWP, cada página geralmente tem elementos de navegação, comando e conteúdo.
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.openlocfilehash: 7333cebc945715412e3ff1140ca26e1ed5368704
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684543"
---
# <a name="page-layout"></a>Layout de página

Em aplicativos UWP, cada [**Página**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) geralmente tem elementos de navegação, comando e conteúdo. 

Seu aplicativo pode ter várias páginas: quando um usuário inicia um aplicativo UWP, o código do aplicativo cria um [**Quadro**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) para ser colocado dentro da [**Janela**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) do aplicativo. Em seguida, o quadro pode [navegar](../basics/navigate-between-two-pages.md) entre as instâncias da [**Página**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) do aplicativo. 

A maioria das páginas segue uma estrutura de layout comum e este artigo aborda os elementos da interface do usuário que você precisará e onde eles devem ir em uma página. 

![estrutura da página](images/page-components.svg)

## <a name="navigation"></a>Navegação
O layout do aplicativo começa com o modelo de navegação escolhido, que define como os usuários navegam entre páginas em seu aplicativo. Neste artigo, discutiremos dois padrões de navegação comuns: navegação à esquerda e navegação superior. Para obter orientação sobre como escolher outras opções de navegação, confira [Noções básicas de design de navegação para aplicativos UWP](../basics/navigation-basics.md).

![padrões de navegação superior e esquerda](images/top-left-nav.svg)

### <a name="left-nav"></a>Navegação à esquerda
A navegação à esquerda ou o padrão do [painel de navegação](../controls-and-patterns/navigationview.md) geralmente é reservada para navegar em nível de aplicativo e existe no nível mais alto dentro do aplicativo. Isso significa que ele sempre deve estar visível e disponível. Recomendamos a navegação à esquerda quando há mais de cinco itens de navegação ou mais de cinco páginas em seu aplicativo. O padrão de painel de navegação geralmente contém:
- Itens de navegação
- Ponto de entrada nas configurações do aplicativo
- Ponto de entrada nas configurações de conta

O controle [NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview) implementa o padrão de navegação à esquerda para UWP.

Quando um item de navegação é selecionado, o quadro deve navegar até a página do item selecionado.

![painel de navegação expandido](images/navview-expanded.svg)

O botão de menu permite que os usuários expandam e recolham o painel de navegação. Quando o tamanho da tela for maior que 640 px, se você clicar no botão de menu, essa ação recolherá o painel de navegação em uma barra.

![painel de navegação compacto](images/navview-compact.svg)

Quando o tamanho da tela for menor que 640 px, o painel de navegação será totalmente recolhido.

![painel de navegação mínimo](images/navview-minimal.svg)

### <a name="top-nav"></a>Navegação superior

A navegação superior também pode atuar como navegação de nível superior. Embora a navegação à esquerda seja recolhível, a navegação superior está sempre visível. O controle [NavigationView](../controls-and-patterns/navigationview.md) implementa o padrão de navegação e guias superior para UWP.

![navegação superior](images/pivot-large.svg)

## <a name="command-bar"></a>Barra de comandos

Em seguida, talvez você queira fornecer aos usuários acesso fácil às tarefas mais comuns do seu aplicativo. Uma [barra de comandos](../controls-and-patterns/app-bars.md) pode fornecer acesso a comandos em nível de aplicativo ou de página e pode ser usada com qualquer padrão de navegação.

![posicionamento da barra de comandos na parte superior ](images/app-bar-desktop.svg)

A barra de comandos pode ser colocada na parte superior ou inferior da página, o que for melhor para seu aplicativo.

![posicionamento da barra de comandos na parte inferior](images/app-bar-mobile.svg)

## <a name="content"></a>Conteúdo

Por fim, o conteúdo varia muito de acordo com o aplicativo, para que você possa apresentar conteúdo de várias maneiras diferentes. Aqui, descrevemos alguns padrões de página comuns que talvez você queira usar em seu aplicativo. Muitos aplicativos usam alguns ou todos esses padrões comuns de página para exibir diferentes tipos de conteúdo. Da mesma forma, fique à vontade para misturar e combinar esses padrões a fim de otimizar seu aplicativo.

## <a name="landing"></a>Inicial

![Página de aterrissagem](images/hero-screen.svg)

Páginas de aterrissagem, também conhecidas como telas de celebridades, geralmente aparecem no nível superior de uma experiência de aplicativo. A área de superfície grande serve como um palco de aplicativos para realçar o conteúdo que os usuários podem querer procurar e consumir.

## <a name="collections"></a>Coleções

![galeria](images/gridview.svg)

Coleções permitem que os usuários naveguem em grupos de conteúdo ou dados. O [modo de exibição de grade](../controls-and-patterns/item-templates-gridview.md) é uma boa opção para fotos ou conteúdo centrado em mídia e a [exibição de lista](../controls-and-patterns/item-templates-listview.md) é uma boa opção para conteúdo de uso intenso de texto ou dados.

## <a name="masterdetail"></a>Mestre/detalhes

![mestre/detalhes](images/master-detail.svg)

O modelo [mestre/detalhes](../controls-and-patterns/master-details.md) consiste em uma exibição de lista (mestre) e um modo de exibição de conteúdo (detalhes). Ambos os painéis são fixos e têm a rolagem vertical. Quando um item na exibição de lista é selecionado, o modo de exibição de conteúdo é atualizado de maneira correspondente. 

## <a name="forms"></a>Formulários
![formulário](images/form.svg)

Um [formulário](../controls-and-patterns/forms.md) é um grupo de controles que coleta e envia dados de usuários. A maioria, se não todos os aplicativos, usam um formulário para páginas de configurações, portais de logon, hubs de comentários, criação de conta ou outros fins. 

## <a name="sample-apps"></a>Exemplos de aplicativos
Para ver como esses padrões podem ser implementados, confira nossos [aplicativos de exemplo UWP](https://developer.microsoft.com/windows/samples):
- [Player de vídeo BuildCast](https://github.com/Microsoft/BuildCast)
- [Lunch Scheduler](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
- [Livro de Colorir](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [Banco de dados de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database)