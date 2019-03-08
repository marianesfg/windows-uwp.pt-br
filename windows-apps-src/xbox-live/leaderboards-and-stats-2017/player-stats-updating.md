---
title: Atualizando estatísticas de 2017
description: Saiba como atualizar estatísticas de jogador do Xbox Live, usando as estatísticas de 2017.
ms.assetid: 019723e9-4c36-4059-9377-4a191c8b8775
ms.date: 08/24/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, estatísticas de player, estatísticas de 2017
ms.localizationpriority: medium
ms.openlocfilehash: d5b37c008e6aa719b641321c5e5a1c3360b20786
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631821"
---
# <a name="updating-stats-2017"></a>Atualizando estatísticas de 2017

Atualizar estatísticas, enviando o valor mais recente para o Xbox Live serviço usando o `StatsManager` APIs que serão discutidas abaixo.

Cabe a seu título para acompanhar as estatísticas de player e você chamar `StatsManager` atualizá-los conforme apropriado.  `StatsManager` irá armazenar em buffer todas as alterações e libere-as para o serviço periodicamente.  O título pode liberar manualmente.

> [!NOTE]
> Não limpa estatísticas com muita frequência.  Caso contrário, o título será taxa limitada.  É uma prática recomendada liberar a no máximo uma vez a cada 5 minutos.

### <a name="multiple-devices"></a>Vários dispositivos

É possível que um player para executar seu título em vários dispositivos.  Nesse caso, você precisa fazer determinado esforço para manter as coisas em sincronia.

Por exemplo, se um jogador tem 15 headshots seu Xbox em casa.  Posteriormente, eles tem 10 headshots mais seu Xbox em casa de um amigo.  Você precisa enviar o valor stat de 25 no segundo dispositivo.  Mas você não teria como saber isso sem alguma forma sincronizar essas informações.

Há algumas maneiras de fazer isso:

1. Store-los usando [armazenamento conectado](../storage-platform/connected-storage/connected-storage-technical-overview.md).  Normalmente, você usaria o armazenamento conectado para por usuário salvar dados.  Esses dados são mantidos em sincronizado entre diferentes dispositivos para um determinado usuário.
2. Use seu próprio serviço web para manter estatísticas em sincronia, se você já tiver um para execução de tarefas do auxiliar de seu título.

### <a name="offline"></a>Offline

Conforme mencionado acima, o título é responsável por manter o controle de estatísticas do player e, portanto, responsbile para dar suporte a cenários offline. 

### <a name="examples"></a>Exemplos

Vamos percorrer um exemplo para unir esses conceitos.

Um estado comum em um jogo de corrida é hora de obter uma visão.  Normalmente, menor é melhor para essas estatísticas.  Portanto, você criaria um stat e associado do placar de líderes, onde inferior é melhor.  Em outras palavras, este placar de líderes poderia ser classificado em ordem crescente.

O título da seria manter o controle de tempos de visão geral do usuário durante sua sessão de reprodução.  Somente se eles tinham um tempo de visão geral menor do que da melhor maneira possível anterior, você atualizaria o Gerenciador de estatísticas.

Você pode controlar melhor seu anterior em uma das seguintes maneiras:
1. De salvamento de arquivos usando o armazenamento conectado.
2. Seu próprio serviço web.

O serviço substituirá o valor stat importa.  Portanto, mesmo se você fosse atualizado com um tempo de visão geral que é maior que da melhor maneira possível anterior, em seguida, da melhor maneira possível anterior será sobrescrita.

Portanto, certifique-se em seu título, que você está enviando apenas os valores de stat adequados com base em seu cenário de jogo.  Em alguns casos os valores mais baixos talvez seja melhores, em alguns casos mais alto pode ser melhor, ou algo mais inteiramente.

## <a name="programming-guide"></a>Guia de programação

Normalmente, seu fluxo para o uso de estatísticas é:

1. Inicializar o `StatsManager` API, passando um usuário local.
1. Como um usuário reproduz por meio de seu título, atualizar stat valores usando o `set_stat` funções.
1. Essas atualizações stat serão liberadas periodicamente e escritas com o Xbox Live.  Você também pode fazer isso manualmente.

### <a name="initialization"></a>Inicialização

Você chama o `StatsManager` com um usuário local para inicializar a API com as informações necessárias.

Veja abaixo um exemplo

```cpp
std::shared_ptr<stats_manager> statsManager = stats_manager::get_singleton_instance();
statsManager->add_local_user(user);
statsManager->do_work();  // returns stat_event_type::local_user_added
```

```csharp
Microsoft.Xbox.Services.Statistics.Manager.StatisticManager statManager = StatisticManager.SingletonInstance;
statManager.AddLocalUser(user);
statEvent = statManager.DoWork();
```

### <a name="writing-stats"></a>Estatísticas de gravação

Você escreve estatísticas usando o `stats_manager::set_stat` família de funções.  Há três variantes dessa função para cada tipo de dados:

* `set_stat_number` para flutuações.
* `set_stat_integer` para números inteiros.
* `set_stat_string` para cadeias de caracteres.

Quando você chama esses, as atualizações de estatísticas são armazenadas em cache localmente no dispositivo.  Periodicamente, eles serão liberados Xbox Live.

Você tem a opção de liberação manualmente de estatísticas por meio de `stats_manager::request_flush_to_service` API.  Observe que, se você chamar essa função com muita frequência, você será limitada de taxa.  Isso não significa que o stat nunca será atualizado.  Ele simplesmente significa que a atualização ocorrerá quando o tempo limite expirar.

```cpp
statsManager->set_stat_integer(user, L"numHeadshots", 20);
statsManager->request_flush_to_service(user); // requests flush to service, performs a do_work
statsManager->do_work();  // applies the stat changes, returns stat_update_complete after flush to service
```

```csharp
statManager.SetStatisticIntegerData(user, statName, (long)statValue);
statManager.RequestFlushToService(user);
statManager.DoWork();
```

#### <a name="example"></a>Exemplo

Digamos que você tenha uma primeira pessoa shooter.  Durante uma correspondência, você pode acumular as seguintes estatísticas:

| Nome STAT | Formato |
|-----------|--------|
| Acaba com melhor por rodada | Inteiro |
| Acaba com o tempo de vida | Inteiro |
| Tempo de vida mortes | Inteiro |
| Taxa de eliminação/morte do tempo de vida | Número |

Conforme o jogador percorre a correspondência, você deve incrementar o *elimina por Round*, *tempo de vida elimina* e *mortes de tempo de vida* localmente.

No final da correspondência, você faria o seguinte:
1. Compare a acaba com que eles têm em turno, com suas melhores anteriores.  Se ele for maior, em seguida, atualize `StatsManager`.
2. Atualizar seu tempo de vida elimina e mortes com os novos valores e atualizar `StatsManager`.
3. Calcular acaba com/mortes e atualizar `StatsManager`

Observe que, para 1 e 2, você precisa saber os valores de stat anteriores.  Consulte as seções acima para as práticas recomendadas sobre como recuperar esses.

Qualquer uma dessas estatísticas pode corresponder a um placar de líderes, que será discutido no próximo artigo.

### <a name="flushing-stats"></a>Estatísticas de liberação

Você pode liberar manualmente as estatísticas usando `stats_manager::request_flush_to_service`.  Você talvez queira fazer isso se desejar exibir um placar de líderes.

Por exemplo, se você tivesse um placar de líderes para `Lifetime Kills` no exemplo acima, você desejaria certificar-se de que as atualizações stat correspondente a esta estatística tinham foi liberadas para o servidor antes de exibir o placar de líderes.  Dessa forma os placares de líderes reflete o progresso de mais recente do player.

### <a name="cleanup"></a>Cleanup
Quando fecha o título, remova o usuário do Gerenciador de estatísticas. Isso liberará os valores de stat mais recentes para o serviço.

```cpp
statsManager->remove_local_user(user);
statsManager->do_work();  // applies the stat changes, returns local_user_removed after flush to service
```

```csharp
statManager.RemoveLocalUser(user);
statManager.DoWork();
```
