---
title: Adicionar estatísticas de player e placares de líderes ao seu projeto do Unity
description: Saiba como usar o plug-in do Xbox Live Unity para adicionar as estatísticas de player e placares de líderes ao seu projeto do Unity.
ms.assetid: 756b3c31-a459-4ad2-97af-119adcd522b5
ms.date: 10/19/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, Unity, criadores
ms.localizationpriority: medium
ms.openlocfilehash: 17af9afc8a9048e7222115d2afdc6108d25df1d7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658131"
---
# <a name="add-player-stats-and-leaderboards-to-your-unity-project"></a>Adicionar estatísticas de player e placares de líderes ao seu projeto do Unity

> [!IMPORTANT]
> O plug-in do Xbox Live Unity não oferece suporte realizações ou multiplayer online e é recomendado somente para [programa de criadores do Xbox Live](../developer-program-overview.md) membros.

Depois que você adicionou [Xbox Live entrar](unity-prefabs-and-sign-in.md) ao seu projeto do Unity, a próxima etapa é adicionar estatísticas de player e placares de líderes com base nessas estatísticas de player.

Com o [plug-in do Xbox Live Unity](https://github.com/Microsoft/xbox-live-unity-plugin), você pode adicionar facilmente as estatísticas de player e placares de líderes em seu projeto do Unity. Semelhante de entrada em etapas, você pode escolher usar as pré-fabricados incluídos ou anexar os scripts incluídos para seus próprios objetos de jogos personalizados.

## <a name="prerequisites"></a>Pré-requisitos
1. [Configurar o Xbox Live no Unity](configure-xbox-live-in-unity.md)
2. [Entrar no Xbox Live no Unity](unity-prefabs-and-sign-in.md)

## <a name="player-stats"></a>Estatísticas de Player

Uma estatística do player é qualquer estatística interessante que você deseja acompanhar para os jogadores. As estatísticas que acompanham com o Xbox Live devem ser estatísticas, que são relevantes para o player e são exibidas de alguma maneira. Essas estatísticas de player são mais comumente usadas para criar tabelas de classificação que os jogadores podem exibir para determinar como seu desempenho classifica contra outros jogadores. Todas as estatísticas de player são consideradas "em destaque: estatísticas", o que significa que a estatística será exibida na página GameHub do jogo.

Estatísticas de Player devem representar um único valor. O plug-in do Xbox Live Unity contém pré-fabricados para inteiro, duplo e estatísticas de cadeia de caracteres. Além disso, um script é fornecido para um objeto stat base que pode ser estendido para outros tipos de dados.

Para obter mais informações sobre estatísticas de player, consulte [estatísticas do Player](../leaderboards-and-stats-2017/player-stats.md).

> [!NOTE]
> Para usar as estatísticas de player ou placares de líderes com o serviço Xbox Live, você deve ter um usuário conectado com êxito antes de poder acessar todos os dados.

## <a name="using-the-player-stat-prefabs"></a>Usando os player de stat pré-fabricados

Há várias pré-fabricados fornecidos no plug-in do Xbox Live Unity que você pode usar relacionadas a estatísticas de player:

* IntegerStat: Uma estatística que pode ser expresso como um valor inteiro, como o número total de acaba com em uma rodada.
* DoubleStat: Uma estatística que pode ser expresso como flutuante aponte o valor, como uma proporção kill/morte.
* StringStat: Uma estatística que pode ser expresso como um valor de cadeia de caracteres, normalmente uma enumeração, como uma classificação concedidos para uma rodada, como "Ouro", "Prata" ou "Bronze".
* StatPanel: Exemplo de interface do usuário que você pode usar para exibir o valor atual de uma estatística.

Para adicionar um stat de player, simplesmente arraste pré-fabricado que corresponde ao tipo de dados do que a estatística para a cena. No Inspetor do Unity para a estatística, você pode especificar três valores:

* A ID de stat. Isso deve coincidir com a ID configurada no Partner Center e diferencia maiusculas de minúsculas.
* O nome de exibição do stat (esse nome é exibido na interface de usuário pré-fabricado StatPanel).
* O valor inicial do stat quando a cena é iniciado.

Você pode usar o **StatPanel** pré-fabricado para exibir o valor de uma estatística. Para fazer isso, arraste uma **StatPanel** pré-fabricado até a cena. Você pode especificar o stat a exibir arrastando o gameobject stat para o **Stat** campo da **StatPanel** objeto no Inspetor do Unity.

### <a name="manipulating-the-player-stat-values"></a>Manipulando valores stat player

Os objetos de player stat têm um número de funções que você pode chamar para ajustar o valor da estatística. Essas funções podem ser chamadas a partir de outras rotinas ou vinculadas a elementos de interface do usuário. Você pode examinar os **DoubleStat**, **IntegerStat**, e **StringStat** scripts para ver exemplos de funções para alterar o valor da estatística. Você pode modificar ou criar novos scripts para representar as estatísticas com funções e a lógica mais complexa. Novas classes stat devem estender a `StatBase` classe, definida no `StatBase` script.

Por exemplo, como um teste simples, você pode adicionar um botão de interface do usuário à sua cena e na `OnClick` evento do botão, no Inspetor do Unity, adicione uma **IntegerStat** do objeto e, em seguida, chame o `Increment()` função para aumentar o valor da stat por um sempre que você clicar no botão.

Se você tiver o stat também associada a um **StatPanel** objeto, você pode ver o valor de stat sempre que você clicar no botão de atualização.

Sempre que você atualizar seu stats (incremento, diminuição, etc.), os valores são atualizados localmente. Para instalar essas atualizações stat refletidas no Xbox Live, duas coisas devem acontecer. Primeiro, você precisa definir o valor de estatísticas com uma das funções StatisticManager.SetStatistic. Há três `StatisticManager` funções para definir as estatísticas, `StatisticManager.SetStatisticIntegerData(XboxLiveUser user, String statName, Int64 value)`, `StatisticManager.SetStatisticNumberData(XboxLiveUser user, String statName, Double value)`, e `StatisticManager.SetStatisticStringData(XboxLiveUser user, String statName, String value)`. Cada uma dessas funções é usada para definir o valor apropriado para o tipo de dados de seu stat. A segunda coisa que você deve fazer para atualizar suas estatísticas no servidor, é *liberar* os dados locais. Os dados obtém liberados automaticamente a cada 5 minutos, o `StatManagerComponent` script.  Se seu jogo terminar antes de 5 minutos, você precisará certificar-se liberar os dados manualmente pela primeira vez para garantir que você não perca a que o progresso. Para fazer isso, você precisará chamar o `statManagerComponent.RequestFlushToService()` método, certificando-se de chamá-lo para o **XboxLiveUser** o stat está sendo gravado para.

> [!TIP]
> Ele é considerado uma prática recomendada sempre liberar os dados antes de seu jogo termina para certificar-se de que você não perca o progresso.

### <a name="checking-and-verifying-stats"></a>Verificando e verificando as estatísticas

O `StatisticManager` classe tem duas funções que são úteis para verificar as estatísticas de configuradas para um `XboxLiveUser`, `StatisticManager.GetStatisticNames(XboxLiveUser user)` e `StatisticManager.GetStatistic(XboxLiveUser user, String statName)`. `GetStatisticNames()` fornecerá uma `list<string>` preenchido pelos nomes das estatísticas para o XboxLiveUser fornecido. Esses nomes podem ser usados para chamar o valor atual de uma estatística chamando o `GetStatistic()` função. É importante observar que embora você pode ler as estatísticas do serviço Xbox Live estatísticas não é recomendável que você usá-lo para a lógica do jogo, mas em vez disso, para verificar o estado da estatística, depois que ele foi enviado. O serviço destina-se somente para ajudar a executar outros serviços como placar de líderes e não deve ser uma fonte de verdade para estatísticas no jogo. É importante que seu título lidar com toda a lógica de estatísticas como nenhuma verificação é feita no seu estatística pelo serviço Xbox e ele simplesmente aceita qualquer valor atribuído a ele como o status atual.

## <a name="leaderboards"></a>Placares de líderes

Um placar de líderes representa uma lista numerada ordenada dos jogadores que atingiram o "melhor" valor de uma estatística. Por exemplo, um placar de líderes pode listar as pessoas que atingiram o tempo mais rápido em uma visão geral de corrida, para que os jogadores podem comparar seu tempo de corrida melhor contra os melhores tempos de corrida obtida com outros jogadores.

Placar de líderes baseiam-se as estatísticas de player que são enviadas para o serviço Xbox Live, o jogo. Portanto, placar de líderes dados é somente leitura, pois você não pode modificá-los diretamente.

O plug-in do Xbox Live Unity fornece um pré-fabricado placar de líderes de exemplo que você pode usar para entender como implementar placares de líderes em seu jogo.

Para obter mais informações sobre o placar de líderes, consulte [placares de líderes](../leaderboards-and-stats-2017/leaderboards.md).

## <a name="using-the-leaderboard-prefabs"></a>Usando as pré-fabricados placar de líderes

O plug-in do Xbox Live Unity contém dois pré-fabricados para placares de líderes:

* Placar de líderes: Um objeto que representa um placar de líderes e contém a interface do usuário simple para exibir os valores de placar de líderes.
* LeaderboardEntry: Um objeto que representa uma única linha de um placar de líderes.

Você pode arrastar uma **placar de líderes** pré-fabricado até a cena. No Inspetor do Unity, você pode definir os seguintes atributos:

* STAT: O gameobject stat esse placar de líderes associado.
* Tipo de placar de líderes: O escopo dos resultados que devem ser retornadas para as entradas de placar de líderes.
* Contagem de entrada: O número de linhas de dados a serem exibidos por página.

> [!NOTE]
> A parte Stat de placar de líderes pré-fabricado é inicialmente em branco. Tente arrastar uma das stat pré-fabricados mencionados acima no slot gameobject para teste.

No editor do Unity, o **placar de líderes** pré-fabricado sempre exibirá os mesmos dados de simulação independentemente das configurações do Inspetor. Você deve criar e exportar seu projeto para o Visual Studio e entre com um usuário autorizado para ver os valores de dados reais. Para obter mais informações, consulte [configurar o Xbox Live no Unity](configure-xbox-live-in-unity.md).

## <a name="see-also"></a>Consulte também

* [Logon no Xbox Live no Unity](unity-prefabs-and-sign-in.md)
* [Configurar o Xbox Live no Unity](configure-xbox-live-in-unity.md)
* [A exemplo de placar de líderes de cena](setup-leaderboard-example-scene.md)
* [Obter dados de placar de líderes](unity-leaderboard-from-scratch.md)
