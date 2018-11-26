---
Description: Search is one of the top ways users can find content in your app. The guidance in this article covers elements of the search experience, search scopes, implementation, and examples of search in context.
title: Pesquisar e localizar na página
ms.assetid: C328FAA3-F6AE-4970-8372-B413F1290C39
label: Search
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: caf0e8e63716f6ba140ef9346257687f0e7293bb
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7699446"
---
# <a name="search-and-find-in-page"></a>Pesquisar e localizar na página

 

A pesquisa é uma das principais maneiras para os usuários encontrarem conteúdo em seu aplicativo. As diretrizes neste artigo abordam elementos da experiência de pesquisa, escopos da pesquisa, implementação e exemplos de pesquisa em contexto.

> **APIs importantes**: [classe AutoSuggestBox](https://msdn.microsoft.com/library/windows/apps/dn633874)

## <a name="elements-of-the-search-experience"></a>Elementos da experiência de pesquisa


**Entrada.** é o modo mais comum de entrada de pesquisa de texto e o foco deste guia. Outros modos de entrada comuns incluem voz e câmera, mas eles geralmente exigem a capacidade de interface com o hardware do dispositivo e podem exigir outros controles ou interface do usuário personalizada no aplicativo.

**Entrada de zero.** Depois que o usuário tiver ativado o campo de entrada, mas antes do usuário ter inserido texto, você pode exibir o que é chamado uma "tela de entrada zero." A tela de entrada zero normalmente aparecerá na tela do aplicativo para que a [sugestão automática](auto-suggest-box.md) substitua esse conteúdo quando o usuário começar a inserir a consulta. Histórico de pesquisa recente, pesquisas mais populares, sugestões de pesquisa contextual e dicas são todos bons candidatos para o estado de entrada zero.

![exemplo da Cortana em uma tela de entrada zero](images/search-cortana-example.png)

 

**Consulta formulação/sugestão automática.** Consulta formulação substitui o conteúdo de entrada zero assim que o usuário começa a inserir a entrada. Conforme o usuário insere uma cadeia de caracteres de consulta, é fornecido um conjunto atualizado continuamente de sugestões de consulta ou opções de desambiguação para ajudar a acelerar o processo de entrada e formular uma consulta eficiente. Esse comportamento de sugestões de consulta é incorporado ao [controle de sugestão automática](auto-suggest-box.md), e também é uma maneira de mostrar o ícone dentro da pesquisa (como um ícone de microfone ou de confirmação). Qualquer comportamento que não se enquadre é atribuído ao aplicativo.

![example of query/formulation sugestão automática](images/search-autosuggest-example.png)

 

**Conjunto de resultados.** Resultados de pesquisa normalmente aparecem diretamente abaixo do campo de entrada de pesquisa. Embora isso não seja um requisito, a justaposição de entrada e resultados mantém o contexto e fornece acesso imediato ao usuário para editar a consulta anterior ou inserir uma nova consulta. Essa conexão pode ser ainda mais comunicada substituindo o texto de dica pela consulta que criou o conjunto de resultados.

Um método para permitir acesso eficiente para editar a consulta anterior e inserir uma nova consulta é realçar a consulta anterior quando o campo for reativado. Dessa forma, qualquer pressionamento de tecla substituirá a cadeia de caracteres anterior, mas a cadeia de caracteres será mantida para que o usuário possa posicionar o cursor para editar ou acrescentar a cadeia de caracteres anterior.

O conjunto de resultados pode aparecer em qualquer formato que comunique melhor o conteúdo. Um [modo de exibição de lista](lists.md) fornece flexibilidade e é adequado para a maioria das pesquisas. Um modo de exibição em grade funciona bem para imagens ou outras mídias, e um mapa pode ser usado para comunicar a distribuição espacial.

## <a name="search-scopes"></a>Escopos da pesquisa


A pesquisa é um recurso comum, e os usuários encontrarão interface do usuário de pesquisa no shell e em muitos aplicativos. Embora os pontos de entrada de pesquisa tendam a ser visualizados da mesma forma, eles podem fornecer acesso aos resultados que variam de amplo (pesquisas na Web ou no dispositivo) a limitado (lista de contatos do usuário). O ponto de entrada de pesquisa deve estar justaposto em relação ao conteúdo que está sendo pesquisado.

Alguns escopos comuns da pesquisa incluem:

**Global** e **contextual/refinado.** Pesquisa várias origens de nuvem e conteúdo local. Resultados variados incluem URLs, documentos, mídia, ações, aplicativos e muito mais.

**Web.** Pesquisa um índice da web. Os resultados incluem páginas, entidades e respostas.

**Meu conteúdo.** Pesquisa em todos os dispositivos, nuvem, gráficos de redes sociais e muito mais. Os resultados variam, mas são restringidos pela conexão à(s) conta(s) de usuário.

Use o texto de dica para comunicar o escopo da pesquisa. Os exemplos incluem:

"Pesquisar o Windows e a Web"

"Pesquisar a lista de contatos"

"Pesquisar a caixa de correio"

"Pesquisar as configurações"

"Procurar um local"

![exemplo de texto de dica de pesquisa](images/search-windowsandweb.png)

 

Ao comunicar efetivamente o escopo de um ponto de entrada de pesquisa, você pode ajudar a garantir que a expectativa do usuário seja atendida pelos recursos da pesquisa realizada e a reduzir a possibilidade de frustração.

## <a name="implementation"></a>Implementação


Para a maioria dos aplicativos, é melhor ter um campo de entrada de texto como o ponto de entrada de pesquisa, o que fornece uma superfície visual proeminente. Além disso, o texto de dica ajuda na capacidade de descoberta e na comunicação do escopo da pesquisa. Quando a pesquisa for uma ação mais secundária ou quando o espaço for limitado, o ícone de pesquisa poderá servir como um ponto de entrada sem o campo de entrada que o acompanha. Quando visualizada como um ícone, assegure-se de que haja espaço para uma caixa de pesquisa modal, conforme visto nos exemplos abaixo.

Antes de clicar no ícone de pesquisa:

![example of a search icon e collapsed search box](images/search-icon-collapsed.png)

 

Depois de clicar no ícone de pesquisa:

![example of a search icon e expeed search box](images/search-icon-expanded.png)

 

A pesquisa sempre usa um glifo de lupa apontando para a direita para o ponto de entrada. O glifo que deve ser usado é Segoe UI Symbol, código de caracteres hexadecimais 0xE0094 e tamanho da fonte geralmente de 15 epx.

O ponto de entrada de pesquisa pode ser colocado em várias áreas diferentes, e seu posicionamento comunica o escopo e o contexto da pesquisa. As pesquisas que coletam resultados em uma experiência ou são externas ao aplicativo estão geralmente localizadas dentro de elementos visuais de aplicativos de nível superior, como barras de comandos globais ou navegação.

Conforme o escopo da pesquisa se torna mais limitado ou contextual, o posicionamento normalmente estará mais diretamente associado ao conteúdo a ser pesquisado, por exemplo, em uma tela, como um cabeçalho de lista, ou em barras de comandos contextuais. Em todos os casos, a conexão entre a entrada de pesquisa e os resultados ou o conteúdo filtrado deve ser visualmente clara.

No caso de listas roláveis, é útil sempre ter a entrada de pesquisa visível. Recomendamos que a entrada de pesquisa seja fixa e a rolagem de conteúdo seja feita atrás dela.

A funcionalidade de entrada zero e formulação de consulta é opcional para pesquisas contextuais/refinadas, em que a lista será filtrada em tempo real pela entrada do usuário. As exceções incluem casos em que sugestões de formatação de consulta podem estar disponíveis, como opções de filtragem de caixa de entrada (para: &lt;cadeia de caracteres de entrada&gt;, de: &lt;cadeia de caracteres de entrada&gt;, assunto: &lt;cadeia de caracteres de entrada&gt;, e assim por diante).

## <a name="example"></a>Exemplo


Os exemplos nesta seção mostram a pesquisa colocada em contexto.

Pesquisa como uma ação na barra de ferramentas do Windows:

![um exemplo de pesquisa como uma ação na barra de ferramentas do Windows](images/search-toolbar-action.png)

 

Pesquisa como uma entrada na tela do aplicativo:

![exemplo de pesquisa em uma tela de aplicativo](images/search-canvas-contacts.png)

 

Pesquisa em um painel de navegação:

![exemplo de pesquisa em um menu de navegação](images/search-navmenu.png)

 

A pesquisa embutida é mais adequada para casos em que a pesquisa é acessada com pouca frequência ou é altamente contextual:

![exemplo de pesquisa embutida](images/patterns-search-results-desktop.png)


## <a name="guidelines-for-find-in-page"></a>Diretrizes de localização na página


O recurso de localização na página permite que os usuários encontrem correspondências de texto no corpo de texto atual. Visualizadores, leitores e navegadores de documentos são os aplicativos mais típicos que fornecem o recurso de localização na página.

## <a name="dos-and-donts"></a>O que fazer e o que não fazer


-   Coloque uma barra de comandos em seu aplicativo com a funcionalidade de localização na página para permitir que o usuário pesquise texto na página. Para obter detalhes de posicionamento, consulte a seção Exemplos.

    -   Os aplicativos que fornecem o recurso de localização na página devem ter todos os controles necessários em uma barra de comandos.
    -   Se o seu aplicativo incluir muitas funcionalidades além da localização na página, você poderá fornecer um botão **Localizar** na barra de comandos de nível superior como ponto de entrada para outra barra de comandos que contém todos os seus controles de localização na página.
    -   A barra de comandos de localização na página deve permanecer visível durante a interação do usuário com o teclado virtual. O teclado virtual é exibido quando um usuário toca na caixa de entrada. A barra de comandos de localização na página deve ser movida para cima, para não ficar escondida atrás do teclado virtual.

    -   O recurso de localização na página deve permanecer disponível enquanto o usuário interage com o modo de exibição. Os usuários precisam interagir com o texto em exibição enquanto usam o recurso de localização na página. Por exemplo, eles podem querer aumentar ou diminuir o zoom em um documento ou aplicar panorâmica no modo de exibição para ler o texto. Assim que o usuário começar a usar o recurso de localização na página, a barra de comandos deve permanecer disponível com um botão **Fechar** para sair desse recurso.

    -   Habilite o atalho de teclado (CTRL+F). Implemente o atalho de teclado CTRL+F para permitir que o usuário invoque rapidamente a barra de comandos de localização na página.

    -   Inclua os elementos básicos da funcionalidade de localização na página. Estes são os elementos de interface do usuário que você precisa usar para implementar o recurso de localização na página:

        -   Caixa de entrada
        -   Botões Anterior e Próximo
        -   Uma contagem de correspondências
        -   Fechar (somente desktop)
    -   A exibição deve realçar as correspondências e rolar para mostrar a próxima correspondência na tela. Os usuários podem se mover rapidamente pelo documento usando os botões **Anterior** e **Próximo** e usando barras de rolagem ou manipulação direta via toque.

    -   A funcionalidade de localização na página deve funcionar junto com a funcionalidade básica de localização na página. Para aplicativos que possuem o recurso Localizar e Substituir, verifique se a localização na página não está interferindo nesse recurso.

-   Inclua um contador de correspondência para indicar ao usuário o número de correspondências de texto existentes na página.
-   Habilite o atalho de teclado (CTRL+F).

## <a name="examples"></a>Exemplos


Fornece uma maneira fácil de acessar o recurso de localização na página. Neste exemplo em uma interface do usuário móvel, "Localizar na página" aparece após dois comandos "Adicionar a..." em um menu expansível:

![exemplo de localização na página 1](images/findinpage-01.png)

 

Depois de selecionar localizar na página, o usuário insere um termo de pesquisa. Sugestões de texto podem aparecer quando um termo de pesquisa está sendo inserido:

![exemplo de localização na página 2](images/findinpage-02.png)

 

Se não houver uma correspondência de texto na pesquisa, uma cadeia de caracteres de texto "Nenhum resultado" deve aparecer na caixa de resultados:

![exemplo de localização na página 3](images/findinpage-03.png)

 

Se houver uma correspondência de texto na pesquisa, o primeiro termo deverá ser destacado em uma cor distinta, com as próximas correspondências em um tom mais sutil dessa mesma paleta de cores, como visto neste exemplo:

![exemplo de localização na página 4](images/findinpage-04.png)

 

Localizar na página tem um contador de correspondência:

![exemplo de contador de pesquisa de localizar na página](images/findinpage-counter.png)




## **<a name="implementing-find-in-page"></a>Implementando a localização na página**

-   Visualizadores de documentos, leitores e navegadores, os tipos de aplicativo mais prováveis para fornecer localizar na página, permitem que o usuário tenha uma experiência de visualização/leitura em tela inteira.
-   A funcionalidade de localização na página é secundária e deve estar localizada em uma barra de comando.

Para obter mais informações sobre como adicionar comandos à sua barra de comando, veja [Barra de comandos](app-bars.md).

 


## <a name="related-articles"></a>Artigos relacionados

* [Caixa de sugestão automática](auto-suggest-box.md)


 

 
