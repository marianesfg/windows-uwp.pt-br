---
ms.assetid: 3A477380-EAC5-44E7-8E0F-18346CC0C92F
title: Virtualização de dados de ListView e GridView
description: Melhore o desempenho e o tempo de inicialização de ListView e GridView por meio da virtualização de dados.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6d53ab5949972c3f58e2c4db27b76fa720fd4b95
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359942"
---
# <a name="listview-and-gridview-data-virtualization"></a>Virtualização de dados de ListView e GridView


**Observação**  para obter mais detalhes, consulte a sessão //build/ [drasticamente aumentar o desempenho quando os usuários interagem com grandes quantidades de dados em um GridView e ListView](https://channel9.msdn.com/Events/Build/2013/3-158).

Melhore o desempenho e o tempo de inicialização de [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) e [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) por meio da virtualização de dados. Para a virtualização da interface do usuário, redução de elementos e atualização progressiva de itens, consulte [Otimização das interfaces do usuário ListView e GridView](optimize-gridview-and-listview.md).

Um método de virtualização de dados é necessário para um conjunto de dados que seja tão grande que não pode ou não deve ser totalmente armazenado na memória de uma vez. Você carrega uma parte inicial na memória (do disco local, rede ou nuvem) e aplica a virtualização de interface do usuário a esse conjunto parcial de dados. Depois, você pode carregar dados incrementalmente ou a partir de pontos arbitrários no conjunto de dados mestre (acesso aleatório) sob demanda. Determinar se a virtualização de dados é adequada para você depende de muitos fatores.

-   O tamanho do conjunto de dados
-   O tamanho de cada item
-   A fonte do conjunto de dados (disco local, rede ou nuvem)
-   O consumo de memória geral do seu aplicativo

**Observação**  Lembre-se de que um recurso é habilitado por padrão para o ListView e GridView que exibe os elementos visuais de espaço reservado temporário enquanto o usuário está rolando/movimento panorâmico rapidamente. Conforme os dados são carregados, esses elementos visuais de espaço reservado são substituídos por seu modelo de item. Você pode desativar o recurso definindo [**ListViewBase.ShowsScrollingPlaceholders**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.showsscrollingplaceholders) como false. No entanto, se fizer isso, recomendamos que use o atributo x:Phase para renderizar progressivamente os elementos no seu modelo de item. Consulte [Atualizar os itens ListView e GridView progressivamente](optimize-gridview-and-listview.md#update-items-incrementally).

Aqui estão mais detalhes sobre as técnicas de virtualização de dados incremental e de acesso aleatório.

## <a name="incremental-data-virtualization"></a>Virtualização de dados incremental

A virtualização de dados incremental carrega dados sequencialmente. Um [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) que usa virtualização de dados incremental pode ser usado para exibir uma coleção de um milhão de itens, mas apenas 50 itens são carregados inicialmente. Conforme o usuário faz um movimento panorâmico/rolagem, os próximos 50 são carregados. À medida que os itens são carregados, o tamanho do elevador da barra de rolagem diminui. Para esse tipo de virtualização de dados, você escreve uma classe de fonte de dados que implementa estas interfaces.

-   [**IList**](https://docs.microsoft.com/dotnet/api/system.collections.ilist?redirectedfrom=MSDN)
-   [**INotifyCollectionChanged** ](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged?redirectedfrom=MSDN) (C#/VB) ou [ **IObservableVector&lt;T&gt;**  ](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IObservableVector_T_) (C++/CX)
-   [**ISupportIncrementalLoading**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading)

Uma fonte de dados como essa é uma lista carregada na memória que pode ser estendida continuamente. O controle de itens solicitará itens usando o indexador [**IList**](https://docs.microsoft.com/dotnet/api/system.collections.ilist?redirectedfrom=MSDN) padrão e as propriedades de contagem. A contagem deve representar o número de itens localmente, não o tamanho real do conjunto de dados.

Quando o controle de itens se aproximar do fim dos dados existentes, ele chamará [**ISupportIncrementalLoading.HasMoreItems**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.isupportincrementalloading.hasmoreitems). Se você retornar **true**, ele chamará [**ISupportIncrementalLoading.LoadMoreItemsAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.isupportincrementalloading.loadmoreitemsasync) passando um número aconselhado de itens para carregar. Dependendo de onde você estiver carregando dados (disco local, rede ou nuvem), você pode optar por carregar um número diferente de itens do que o aconselhado. Por exemplo, se seu serviço aceitar lotes de 50 itens, mas o controle de itens apenas solicitar 10, você poderá carregar 50. Carregue os dados do back-end, adicione-os à sua lista e gere uma notificação de alteração via [**INotifyCollectionChanged**](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged?redirectedfrom=MSDN) ou [**IObservableVector&lt;T&gt;** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IObservableVector_T_) para que o controle de itens saiba sobre os novos itens. Retorne também uma contagem dos itens que você realmente carregou. Se você carregar menos itens do que o aconselhado, ou o controle de itens foi panoramizado/rolado ainda mais nesse ínterim, sua fonte de dados será chamada novamente para mais itens e o ciclo continuará. Você pode saber mais, baixando a [exemplo de associação de dados XAML](https://code.msdn.microsoft.com/windowsapps/Data-Binding-7b1d67b5) para Windows 8.1 e reutilizando seu código-fonte em seu aplicativo do Windows 10.

## <a name="random-access-data-virtualization"></a>Virtualização de dados de acesso aleatório

A virtualização de dados de acesso aleatório permite o carregamento de um ponto arbitrário do conjunto de dados. Um [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) que usa a virtualização de dados de acesso aleatório, usada para exibir uma coleção de um milhão de itens, pode carregar os itens 100.000 – 100.050. Se o usuário for para o início da lista, o controle carregará os itens 1 – 50. O elevador da barra de rolagem sempre indica que o **ListView** contém um milhão de itens. A posição do elevador da barra de rolagem é relativa a onde os itens visíveis estão localizados no conjunto de dados inteiro da coleção. Esse tipo de virtualização de dados pode reduzir significativamente os requisitos de memória e os tempos de carregamento da coleção. Para habilitá-lo, você precisa gravar uma classe de fonte de dados que busque dados sob demanda, gerencie um cache local e implemente estas interfaces.

-   [**IList**](https://docs.microsoft.com/dotnet/api/system.collections.ilist?redirectedfrom=MSDN)
-   [**INotifyCollectionChanged** ](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged?redirectedfrom=MSDN) (C#/VB) ou [ **IObservableVector&lt;T&gt;**  ](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IObservableVector_T_) (C++/CX)
-   (Opcionalmente) [**IItemsRangeInfo**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IItemsRangeInfo)
-   (Opcionalmente) [**ISelectionInfo**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ISelectionInfo)

[**IItemsRangeInfo** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IItemsRangeInfo) fornece informações sobre quais itens o controle está usando ativamente. O controle de itens chamará esse método sempre que sua exibição mudar e incluirá esses dois conjuntos de intervalos.

-   O conjunto de itens que estão no visor.
-   Um conjunto de itens não virtualizados que o controle está usando e que talvez não estejam no visor.
    -   Um buffer de itens próximo do visor mantido pelo controle de itens de maneira que o movimento panorâmico do toque seja suave.
    -   O item de foco.
    -   O primeiro item.

Implementando [**IItemsRangeInfo**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IItemsRangeInfo), sua fonte de dados sabe quais itens precisam ser buscados e armazenados em cache, e quando remover os dados de cache que não são mais necessários. **IItemsRangeInfo** usa objetos [**ItemIndexRange**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ItemIndexRange) para descrever um conjunto de itens com base em seu índice na coleção. Isso é para que ele não use ponteiros de item, que podem não ser corretos ou estáveis. **IItemsRangeInfo** foi projetado para ser usado somente por uma única instância de um controle de itens, pois depende de informações de estado para esse controle de itens. Se vários controles de itens precisarem de acesso aos mesmos dados, você precisará de uma instância separada da fonte de dados para cada um. Eles podem compartilhar um cache comum, mas a lógica para remover do cache será mais complicada.

Esta é a estratégia básica para sua fonte de dados de virtualização de dados de acesso aleatório.

-   Quando um item for solicitado
    -   Caso você o tenha disponível na memória, retorne-o.
    -   Caso você não o tenha, retorne um item nulo ou de espaço reservado.
    -   Use a solicitação de um item (ou as informações do intervalo de [**IItemsRangeInfo**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IItemsRangeInfo)) para saber quais itens são necessários, além de buscar dados dos itens do back-end de maneira assíncrona. Depois de recuperar os dados, acione uma notificação de alteração via [**INotifyCollectionChanged**](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged?redirectedfrom=MSDN) ou [**IObservableVector&lt;T&gt;** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IObservableVector_T_) de maneira que o controle de itens saiba mais sobre o novo item.
-   (Opcionalmente) À medida que o visor do controle de itens muda, identifique quais itens da fonte de dados são necessários implementando [**IItemsRangeInfo**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IItemsRangeInfo).

Além disso, a estratégia de quando carregar itens de dados, quantos dados carregar e quais itens manter na memória depende de seu aplicativo. Algumas considerações gerais para lembrar:

-   Faça solicitações assíncronas de dados; não bloqueie o thread de interface do usuário.
-   Encontre o ponto forte no tamanho dos lotes nos que você busca os itens. Prefira os "robustos" aos "tagarelas". Não tão pequenos a ponto de você fazer solicitações pequenas demais; não tão grandes a ponto de levarem muito tempo para recuperar.
-   Considere quantas solicitações você quer que fiquem pendentes ao mesmo tempo. É mais fácil executar uma por vez, mas pode ser muito demorado se o tempo de retorno for alto.
-   Você pode cancelar solicitações de dados?
-   Se estiver usando um serviço hospedado, há um custo por transação?
-   Quais tipos de notificações são fornecidos pelo serviço quando os resultados de uma consulta são alterados? Você saberá se um item foi inserido no índice 33? Se seu serviço aceita consultas com base em no método chave mais deslocamento, isso pode ser melhor do que apenas usar um índice.
-   Quão inteligente você quer ser na pré-busca de itens? Você vai testar e controlar a direção e a velocidade de rolagem para prever quais itens são necessários?
-   Quão agressivo você quer ser na limpeza do cache? Esse é um equilíbrio entre memória e experiência.




