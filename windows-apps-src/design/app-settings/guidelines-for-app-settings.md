---
author: mijacobs
Description: This article describes best practices for creating and displaying app settings.
title: Diretrizes para configurações de aplicativos
ms.assetid: 2D765E90-3FA0-42F5-A5CB-BEDC14C3F60A
label: Guidelines
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 56a952950fa9f2d9d57d5beaed397dd72f64ea54
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7307516"
---
# <a name="guidelines-for-app-settings"></a>Diretrizes para configurações de aplicativos



Configurações do aplicativo são as partes de seu aplicativo personalizáveis pelo usuário e as partes dinâmicas na página de configurações do aplicativo. Por exemplo, as configurações do aplicativo em um aplicativo leitor de notícias podem permitir que o usuário especifique quais fontes de notícias exibir ou a quantidade de colunas a serem exibidas na tela, enquanto as configurações de um aplicativo de previsão do tempo podem permitir que o usuário escolha entre Celsius e Fahrenheit como a unidade padrão de medida. Este artigo descreve as práticas recomendadas para criar e exibir configurações do aplicativo.


## <a name="should-i-include-a-settings-page-in-my-app"></a>Devo incluir uma página de configurações em meu aplicativo?

Aqui estão exemplos de opções do aplicativo que pertencem a uma página de configurações do aplicativo:

-   Opções de configuração que afetam o comportamento do aplicativo e que não necessitam de reajuste frequente, como a seleção entre Celsius ou Fahrenheit como a unidade padrão de temperatura em um aplicativo de previsão do tempo, a alteração das configurações da conta do aplicativo de email, as configurações de notificações ou opções de acessibilidade.
-   Opções que dependem das preferências do usuário, como músicas, efeitos sonoros ou temas de cores.
-   Informações sobre o aplicativo que não são acessadas com frequência, como a política de privacidade, a ajuda, a versão do aplicativo ou as informações de direitos autorais.

Os comandos que fazem parte do fluxo de trabalho normal do aplicativo (por exemplo, alterar o tamanho do pincel em um aplicativo de desenho) não devem estar na página de configurações. Para saber mais sobre o posicionamento de comandos, consulte [Noções básicas de design de comandos](https://msdn.microsoft.com/library/windows/apps/dn958433).

## <a name="general-recommendations"></a>Recomendações gerais


-   Mantenha as páginas de configurações simples e utilize controles binários (ativar/desativar). Os [switches de alternância](../controls-and-patterns/toggles.md) geralmente são o melhor controle para uma configuração binária.
-   Para configurações que permitem aos usuários escolher um item em um conjunto de até cinco opções relacionadas mutuamente exclusivas, use [botões de opção](../controls-and-patterns/radio-button.md).
-   Crie um ponto de entrada para todas as configurações do aplicativo na página de configurações de seu aplicativo.
-   Mantenha suas configurações simples. Defina padrões inteligentes e mantenha o número de configurações em um mínimo.
-   Quando um usuário muda uma configuração, o aplicativo deve refletir essa mudança imediatamente.
-   Não inclua comandos que fazem parte do fluxo de trabalho comum do aplicativo.

## <a name="entry-point"></a>Ponto de entrada


A maneira como os usuários acessam a página de configurações do aplicativo deve ser baseada no layout do aplicativo.

**Painel de navegação**

Para um layout de painel de navegação, as configurações do aplicativo devem ser o último item na lista de opções de navegação e serem fixadas na parte inferior:

![ponto de entrada de configurações do aplicativo para o painel de navegação](images/appsettings-entrypoint-navpane.png)

**Barra de aplicativos**

Se você estiver usando uma [barra de aplicativos](../controls-and-patterns/app-bars.md) ou barra de ferramentas, coloque o ponto de entrada de configurações como o último item no menu de excedentes "Mais". Se for importante para o aplicativo ter maior capacidade de descoberta do ponto de entrada de configurações, coloque-o diretamente na barra de aplicativos, e não na área de excedentes.

![ponto de entrada de configurações do aplicativo para a barra de aplicativos](images/appsettings-entrypoint-tabs.png)

**Hub**

Se você estiver usando um layout de Hub, o ponto de entrada de configurações do aplicativo deve ser colocado no menu de excedentes "Mais" de uma barra de aplicativos.

**Guias/pivôs**

Para um layout de guias ou pivôs, não recomendamos colocar o ponto de entrada das configurações do aplicativo como um dos primeiros itens da navegação. Em vez disso, o ponto de entrada das configurações do aplicativo deve ser colocado no menu de excedentes "Mais" de uma barra de aplicativos.

**Mestre/detalhes**

Em vez de esconder totalmente o ponto de entrada das configurações do aplicativo em um painel de detalhes mestres, faça com que ele seja o último item fixado no nível superior do painel mestre.

## <a name="layout"></a>Layout


Tanto na área de trabalho quanto no celular, a janela de configurações do aplicativo deve ser aberta em tela inteira e preencher toda a janela. Se o menu de configurações do aplicativo tiver até quatro grupos de nível superior, esses grupos deverão estar em uma coluna de cascata para baixo.

Área de trabalho:

![layout da página de configurações do aplicativo na área de trabalho](images/appsettings-layout-navpane-desktop.png)

Celular:

![layout da página de configurações do aplicativo em celular](images/appsettings-layout-navpane-mobile.png)

## <a name="color-mode-settings"></a>Configurações de "Modo de cor"


Se seu aplicativo permite que os usuários escolham o modo de cor do aplicativo, apresente essas opções usando [botões de opção](../controls-and-patterns/radio-button.md) ou uma [caixa de combinação](../controls-and-patterns/lists.md#drop-down-lists) com o cabeçalho "Escolher um modo de aplicativo". Leia as opções
- Claro
- Escuro
- Padrão do Windows

Também recomendamos adicionar um hiperlink para a página de cores do aplicativo Configurações do Windows onde os usuários podem acessar e modificar o modo de aplicativo padrão. Use a cadeia de caracteres "Configurações de cores do Windows" para o texto de hiperlink.

![Seção "Escolher um modo"](images/appsettings_mode.png)

<!--
<div class="microsoft-internal-note">
Detailed redlines showing preferred text strings for the "Choose a mode" section are available on [UNI](http://uni/DesignDepot.FrontEnd/#/ProductNav/2543/0/dv/?t=Windows%7CControls%7CColorMode&f=RS2).
</div>
-->

## <a name="about-section-and-feedback-button"></a>Seção Sobre e botão Fornecer feedback


É recomendável colocar a seção "Sobre este aplicativo" em seu aplicativo como uma página dedicada ou em sua própria seção. Se você desejar um botão "Enviar comentários", coloque-o na parte inferior da página "Sobre este aplicativo".

No subcabeçalho "Legal", coloque quaisquer "Termos de uso" e "Política de privacidade" (deve ser [botões de hiperlink](../controls-and-patterns/hyperlinks.md) com disposição do texto), bem como informações legais adicionais, como direitos autorais.

![seção "sobre este aplicativo" com o botão "fornecer feedback"](images/appsettings-about.png)


## <a name="recommended-page-content"></a>Conteúdo recomendado da página


Assim que você tiver uma lista dos itens que deseja incluir na página de configurações do aplicativo, considere estas diretrizes:

-   Agrupe configurações semelhantes ou relacionadas em um rótulo de configurações.
-   Tente manter o número total de configurações em um máximo de quatro ou cinco.
-   Exiba as mesmas configurações, independentemente do contexto do aplicativo. Se algumas configurações não forem relevantes em determinado contexto, desabilite-as no submenu Configurações do aplicativo.
-   Use rótulos descritivos de uma palavra para configurações. Por exemplo, chame a configuração de "Contas" em vez de "Configurações de contas" para configurações relacionadas a contas. Se você deseja somente uma opção para as configurações e elas não permitirem um rótulo descritivo, use "Opções" ou "Padrões".
-   Se uma configuração se vincular diretamente à Web e não a um submenu, informe isso ao usuário com uma dica visual, como "Ajuda (online)" ou "Fóruns na Web" formatada como um [hiperlink](../controls-and-patterns/hyperlinks.md). Procure agrupar vários links para a Web em um submenu com uma única configuração. Por exemplo, uma configuração"Sobre" pode abrir um submenu com links para os termos de uso, a política de privacidade e o suporte do aplicativo.
-   Combine configurações menos utilizadas em uma única entrada para que as configurações mais comuns possam ter sua própria entrada. Coloque conteúdo ou links que incluam somente informações em uma configuração "Sobre".
-   Não duplique a funcionalidade no painel "Permissões". O Windows fornece esse painel por padrão e não é possível modificá-lo.

-   Adicionar conteúdo de configurações a submenus Configurações
-   Apresente o conteúdo de cima para baixo em uma única coluna, com rolagem, se necessário. Limite a rolagem para no máximo de duas vezes a altura da tela.
-   Use os seguintes controles para configurações do aplicativo:

    -   [Switches de alternância](../controls-and-patterns/toggles.md): para permitir que os usuários definam valores como "ativado" ou "desativado".
    -   [Botões de opção](../controls-and-patterns/radio-button.md): para permitir que os usuários escolham um item de um conjunto de até cinco opções relacionadas exclusivas.
    -   [Caixa de entrada de texto](../controls-and-patterns/text-block.md): para permitir que os usuários insiram texto. Use o tipo da caixa de entrada de texto que corresponde ao tipo de texto que você está obtendo do usuário, como um email ou senha.
    -   [Hiperlinks](../controls-and-patterns/hyperlinks.md): para conduzir o usuário para outra página dentro do aplicativo ou para um site externo. Quando um usuário clicar em um hiperlink, o submenu Configurações será ignorado.
    -   [Botões](../controls-and-patterns/buttons.md): para permitir que os usuários iniciem uma ação imediata sem ignorar o submenu Configurações atual.
-   Adicione uma mensagem descritiva se um dos controles estiver desativado. Coloque esta mensagem acima do controle desativado.
-   Anime o conteúdo e os controles como um bloco único depois que o submenu Configurações e o cabeçalho forem animados. Anime o conteúdo usando a animação [**enterPage**](https://msdn.microsoft.com/library/windows/apps/br212672) ou [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br210288) com um deslocamento à esquerda de 100px.
-   Use cabeçalhos de seção, parágrafos e rótulos para ajudar a organizar e esclarecer conteúdo, se necessário.
-   Se for necessário repetir configurações, use um nível adicional da interface do usuário ou um modelo expandir/recolher, mas evite hierarquias com mais de dois níveis. Por exemplo, um aplicativo de previsão do tempo que disponibiliza configurações por cidade pode listar as cidades e deixar que o usuário toque na cidade para abrir um novo submenu ou expandir para mostrar as opções de configurações.
-   Se o carregamento de controles ou de conteúdo da Web for demorado, use um controle de progresso indeterminado para indicar aos usuários que as informações estão sendo carregadas. Para obter mais informações, consulte [Diretrizes de controles de progresso](https://msdn.microsoft.com/library/windows/apps/hh465469).
-   Não use botões para navegação ou para confirmar alterações. Use hiperlinks para navegar para outras páginas e, em vez de usar um botão para confirmar as mudanças, salve-as automaticamente nas configurações do aplicativo quando um usuário ignorar o submenu Configurações.



## <a name="related-articles"></a>Artigos relacionados

* [Noções básicas de design de comandos](https://msdn.microsoft.com/library/windows/apps/dn958433)
* [Diretrizes de controles de progresso](https://msdn.microsoft.com/library/windows/apps/hh465469)
* [Armazenar e recuperar dados de aplicativo](https://msdn.microsoft.com/library/windows/apps/mt299098)
* [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br210288)
