---
author: jwmsft
ms.assetid: F912161D-3767-4F35-88C0-E1ECDED692A2
title: Melhore o desempenho de coleta de lixo
description: Aplicativos da Plataforma Universal do Windows (UWP) escritos em C# e Visual Basic obtém gerenciamento de memória automático do coletor de lixo .NET. Esta seção resume as melhores práticas de comportamento e desempenho para o coletor de lixo .NET em aplicativos UWP.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 31279de84b8f00e4489a7aae962caa231bb16dc1
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5812533"
---
# <a name="improve-garbage-collection-performance"></a>Melhore o desempenho de coleta de lixo


Aplicativos da Plataforma Universal do Windows (UWP) escritos em C# e Visual Basic obtém gerenciamento de memória automático do coletor de lixo .NET. Esta seção resume as práticas recomendadas de comportamento e desempenho para o coletor de lixo do .NET nos aplicativos UWP. Para obter mais informações sobre o funcionamento do coletor de lixo do .NET e sobre as ferramentas de depuração e análise de desempenho do coletor de lixo, consulte [Coleta de lixo](https://msdn.microsoft.com/library/windows/apps/xaml/0xy59wtx.aspx).

**Observação**precisar intervir no comportamento padrão do coletor de lixo é indicativo de problemas gerais de memória com seu aplicativo. Para obter mais informações, consulte [Ferramenta de uso de memória durante a depuração no Visual Studio 2015](http://blogs.msdn.com/b/visualstudioalm/archive/2014/11/13/memory-usage-tool-while-debugging-in-visual-studio-2015.aspx). Este tópico se aplica somente a C# e Visual Basic.

 

O coletor de lixo determina quando deve ser executado, equilibrando o consumo de memória do heap gerenciado e a quantidade de trabalho que uma coleta de lixo precisa executar. Uma das maneiras em que o coletor de lixo faz isso é dividindo o heap em gerações e coletando somente parte do heap na maior parte do tempo. Existem três gerações na pilha gerenciada:

-   Geração 0. Essa geração contém objetos recém-alocados, a menos que eles tenham 85 KB ou mais; nesse caso, eles farão parte da pilha de objetos grande. A pilha de objetos grande é coletada com coletas de geração 2. As coletas da geração 0 são o tipo de coleta que ocorre com mais frequência e limpam objetos de vida curta, como variáveis locais.
-   Geração 1. Essa geração contém objetos que sobreviveram às coletas de geração 0. Ela serve de buffer entre as gerações 0 e 2. As coletas de geração 1 ocorrem com menos frequência do que as de geração 0 e limpam objetos temporários que estavam ativos durante as coletas de geração 0 anteriores. Uma coleta de geração 1 também coleta geração 0.
-   Geração 2. Essa geração contém objetos de vida longa que sobreviveram às coletas de geração 0 e de geração 1. As coletas de geração 2 são as menos frequentes e coletam toda a pilha gerenciada, incluindo a pilha de objetos grande que contém objetos com 85 KB ou mais.

É possível medir o desempenho do coletor de lixo em 2 aspectos: o tempo que ele leva para fazer a coleta de lixo e o consumo de memória do heap gerenciado. Se você tiver um aplicativo pequeno com um tamanho de heap inferior a 100 MB, concentre-se na redução do consumo de memória. Se você tem um aplicativo com um heap gerenciado maior que 100 MB, concentre-se apenas em reduzir o tempo da coleta de lixo. Consulte aqui como você pode ajudar o coletor de lixo .NET a ter um desempenho melhor.

## <a name="reduce-memory-consumption"></a>Reduzir o consumo de memória

### <a name="release-references"></a>Referências de versão

Uma referência a um objeto em seu aplicativo impede que o objeto, e todos os objetos que ele referencia, seja coletado. O compilador .NET faz um bom trabalho de detecção de quando uma variável não está mais em uso, de forma que os objetos mantidos por ela se tornem qualificados para a coleta. Mas em alguns casos pode não ser óbvio que alguns objetos façam referência a outros objetos porque parte do elemento gráfico do objeto pode ser de propriedade de bibliotecas usadas por seu aplicativo. Para aprender sobre as ferramentas e técnicas para descobrir quais objetos sobrevivem a uma coleta de lixo, consulte [Coleta de lixo e desempenho](https://msdn.microsoft.com/library/windows/apps/xaml/ee851764.aspx).

### <a name="induce-a-garbage-collection-if-its-useful"></a>Induzir uma coleta de lixo se ela for útil

Induza uma coleta de lixo somente depois de medir o desempenho de seu aplicativo e determinar que a indução de uma coleta melhorará esse desempenho.

É possível induzir uma coleta de lixo de uma geração chamando [**GC.Collect(n)**](https://msdn.microsoft.com/library/windows/apps/xaml/y46kxc5e.aspx), em que n é a geração que você deseja coletar (0, 1 ou 2).

**Observação**, recomendamos que você não imponha uma coleta de lixo em seu aplicativo porque o coletor de lixo usa várias heurísticas para determinar o melhor momento para executar uma coleta e impor uma coleta é em muitos casos, um uso desnecessário da CPU. Mas se você souber que tem um grande número de objetos em seu aplicativo que não são mais usados e desejar retornar essa memória para o sistema, poderá ser adequado impor uma coleta de lixo. Por exemplo, você pode induzir uma coleta no final de uma sequência de carregamento em um jogo para liberar memória antes de começar a jogar.
 
Para evitar a indução inadvertida de coletas de lixo em excesso, é possível definir [**GCCollectionMode**](https://msdn.microsoft.com/library/windows/apps/xaml/bb495757.aspx) como **Optimized**. Esse recurso instrui o coletor de lixo a iniciar a coleta somente se ele determinar que a coleta será produtiva o bastante para ser justificada.

## <a name="reduce-garbage-collection-time"></a>Reduzir o tempo de coleta de lixo

Esta seção se aplica se você tiver analisado seu aplicativo e observado tempos de coleta de lixo longos. Os tempos de pausa relacionados à coleta de lixo incluem: o tempo levado na execução de uma única passagem de coleta de lixo e o tempo total gasto por seu aplicativo em coletas de lixo. O tempo que uma coleta demora depende da quantidade de dados ao vivo o coletor precisa analisar. A geração 0 e a geração 1 são limitadas por tamanho, mas a geração 2 continua a crescer à medida que objetos de vida longa estejam ativos em seu aplicativo. Isso significa que os tempos de coleta para a geração 0 e a geração 1 são limitados, enquanto que as coletas de geração 2 podem demorar mais. A frequência em que coletas de lixo serão executadas dependerá muito da quantidade de memória alocada, uma vez que uma coleta de lixo libera memória para satisfazer solicitações de alocação.

Ocasionalmente, o coletor de lixo pausa seu aplicativo para executar trabalho, mas não pausa necessariamente seu aplicativo o tempo todo em que estiver fazendo a coleta. Os tempos de pausa geralmente não são percebidos pelo usuário em seu aplicativo, em especial para as coletas de geração 0 e geração 1. O recurso [Coleta de lixo em segundo plano](https://msdn.microsoft.com/library/windows/apps/xaml/ee787088.aspx#background-garbage-collection) do coletor de lixo do .NET permite que as coletas de Geração 2 sejam executadas concomitantemente enquanto seu aplicativo estiver em execução e só pausará seu aplicativo por períodos curtos. Mas nem sempre será possível fazer uma coleta de Geração 2 como uma coleta em segundo plano. Nesse caso, a pausa poderá ser percebida pelo usuário se você tiver uma pilha grande o suficiente (com mais de 100 MB).

Coletas de lixo frequentes podem contribuir para maior consumo de CPU (e, portanto, de energia), tempos de carregamento maiores ou taxas de quadro menores em seu aplicativo. A seguir, algumas técnicas que você pode usar para reduzir o tempo de coleta de lixo e as pausas relacionadas à coleta em seu aplicativo UWP gerenciado.

### <a name="reduce-memory-allocations"></a>Reduzir as alocações de memória

Se você não alocar qualquer objeto, o coletor de lixo não será executado, a menos que haja uma condição de pouca memória no sistema. A redução da quantidade de memória alocada se traduz diretamente em coleta de lixo menos frequentes.

Se, em algumas seções de seu aplicativo, as pausas forem totalmente indesejáveis, você poderá pré-alocar os objetos necessários com antecedência durante um período de desempenho menos crítico. Por exemplo, um jogo poderia alocar todos os objetos necessários para jogar durante a tela de carregamento de um nível e não fazer alocações ao jogar. Isso evita pausas enquanto o usuário está jogando e pode resultar em uma taxa de quadros maior e mais consistente.

### <a name="reduce-generation-2-collections-by-avoiding-objects-with-a-medium-length-lifetime"></a>Reduza as coletas da geração 2 evitando objetos com tempo médio de vida

As coletas de lixo geracionais são melhor executadas quando você tem objetos com vida realmente curta ou realmente longa em seu aplicativo. Os objetos com vida curta são coletados nas coletas de geração 0 e geração 1 mais baratas e os objetos com vida longa são promovidos para a geração 2, que é coletada com menos frequência. Os objetos de vida longa são aqueles que estão em uso por toda a duração de seu aplicativo ou durante um período significativo de seu aplicativo, como durante uma página ou um nível de jogo específico.

Se você criar objetos com frequência com uma vida útil temporária, mas longa o suficiente para serem promovidos para a geração 2, mais coletas caras de geração 2 acontecerão. Talvez você possa reduzir as coletas de geração 2 reciclando objetos existentes ou liberando os objetos com mais rapidez.

Um exemplo comum de objetos com vida útil de médio prazo contém objetos usados para a exibição de itens em uma lista que pode ser rolada pelo usuário. Se objetos forem criados quando itens da lista forem rolados para exibição e não forem mais referenciados à medida que itens da lista forem rolados para fora da exibição, tipicamente seu aplicativo terá um grande número de coletas de geração 2. Em situações como essa, você pode pré-alocar e reutilizar um conjunto de objetos para os dados que são ativamente mostrados ao usuário e usar objetos de vida curta para carregar informações à medida que os itens da lista forem exibidos.

### <a name="reduce-generation-2-collections-by-avoiding-large-sized-objects-with-short-lifetimes"></a>Reduza as coletas da geração 2 evitando objetos grandes com tempo curto de vida

Qualquer objeto com 85 KB ou mais é alocado na pilha de objetos grandes (LOH) e é coletado como parte da geração 2. Se você tiver variáveis temporárias, como buffers, com mais de 85 KB, uma coleta de geração 2 as limpará. Limitar as variáveis temporárias a menos de 85 KB reduz o número de coletas de geração 2 em seu aplicativo. Uma técnica comum é criar um pool de buffers de reutilizar objetos do pool para evitar alocações temporárias grandes.

### <a name="avoid-reference-rich-objects"></a>Evitar objetos com muitas referências

O coletor de lixo determina quais objetos estão vivos seguindo referências entre objetos, começando das raiz de seu aplicativo. Para obter mais informações, consulte [O que acontece durante a coleta de lixo](https://msdn.microsoft.com/library/windows/apps/xaml/ee787088.aspx#what-happens-during-a-garbage-collection). Se um objeto contiver muitas referências, haverá mais trabalho para o coletor de lixo. Uma técnica comum (especialmente com objetos grandes) é converter objetos com muitas referências em objetos sem referências (por exemplo, em vez de armazenar uma referência, armazenar um índice). É claro que essa técnica só funciona quando for logicamente possível fazer isso.

Substituir referências de objeto por índices pode ser uma alteração prejudicial e complicada para seu aplicativo e é mais eficiente para objetos grandes com um grande número de referências. Faça isso somente se estiver observando tempos muito grandes de coleta de lixo em seu aplicativo com relação a objetos com muitas referências.

 

 




