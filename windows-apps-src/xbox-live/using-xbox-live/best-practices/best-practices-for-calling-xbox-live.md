---
title: Práticas recomendadas para chamar o Xbox Live
description: Saiba mais sobre as práticas recomendadas para chamar as APIs do Xbox Live.
ms.assetid: f4c7156b-7736-41e5-9b3d-e87cc8dd2531
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox, práticas recomendadas
ms.localizationpriority: medium
ms.openlocfilehash: 55e05ef7de2e2981f9f5af86623a8d8413ce2c99
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613701"
---
# <a name="best-practices-for-calling-xbox-live"></a>Práticas recomendadas para chamar o Xbox Live

Os serviços do Xbox Live podem ser chamados de duas maneiras principais: usando a API de serviços do Xbox (XSAPI), ou chamando os pontos de extremidade REST diretamente. Independentemente de como o seu código chama o Xbox Live, é importante ter padrões de chamada apropriadas e lógica de repetição.

Para compreender como escrever a lógica de repetição adequada, é necessário saber sobre os dois tipos de pontos de extremidade REST - **idempotente** e **não idempotentes**. Vamos discutir cada um deles abaixo
 
## <a name="non-idempotent-endpoints"></a>Pontos de extremidade não idempotentes

Métodos HTTP que têm efeitos colaterais após repetir chamadas são consideradas **não idempotentes**. Isso significa que se um cliente chamar o ponto de extremidade e um tempo limite da rede ocorre, não é seguro tentar novamente o método porque o recurso pode ter sido atualizado, mas a rede não foi capaz de notificar o chamador de que ela foi bem-sucedida. Em caso de erro, em vez de tentar novamente, o cliente deve primeiro consultar para ver se a chamada foi bem-sucedida. Somente se a chamada não foi bem-sucedida, em seguida, ele deverá repetir.

Na API de serviços do Xbox, algumas APIs internamente são marcados como chamar pontos de extremidade não idempotentes. Isso significa que se ocorrerem falhas ao chamar esses pontos de extremidade, as APIs não repetirá automaticamente o ponto de extremidade.

A lista completa de APIs não idempotentes são:

* game\_server\_platform\_service::allocate\_cluster()
<br>
* game\_server\_platform\_service::allocate\_cluster\_inline()
<br>
* game\_server\_platform\_service::allocate\_session\_host()
<br>
* matchmaking\_service::create\_match\_ticket()
<br>
* multiplayer\_service::write\_session()
<br>
* multiplayer\_service::write\_session\_by\_handle()
<br>
* multiplayer\_service::send\_invites()
<br>
* reputation\_service::submit\_batch\_reputation\_feedback()
<br>
* reputation\_service::submit\_reputation\_feedback()
 

## <a name="idempotent-methods"></a>Métodos de idempotentes

**Idempotente** métodos HTTP por outro lado não deixem efeitos colaterais. Por sua vez, isso significa que eles estão seguros ser repetida. Na API de serviços do Xbox, todos os métodos de idempotentes são automaticamente repetidos em determinadas condições.

A lista completa de APIs de idempotentes são todas as APIs que não foram listadas acima como sendo não idempotentes.


## <a name="retry-logic-best-practices"></a>Práticas recomendadas de lógica de repetição

Para chamadas de idempotentes essas condições devem ser repetidas automaticamente:

* Todos os erros de rede
* 401: Não autorizado
* 408: RequestTimeout
* 429: Número excessivo de solicitações
* 500: InternalError
* 502: BadGateway
* 503: ServiceUnavailable
* 504: GatewayTimeout


Na UWP, 401: Não autorizado é tratado especial. Ele indica que o token de autenticação do Xbox Live expirou, portanto, a API de serviços do Xbox chama-se ao sistema operacional para o token de atualização e, em seguida, executa como uma única nova tentativa.

Quando uma nova tentativa é executada, é recomendável não chamar o serviço até que o tempo de cabeçalho "Retry-After" foi atingido. XSAPI agora implementa essa prática recomendada. Se um código de status HTTP de falha e o cabeçalho "Retry-After" foi retornado para qualquer API, chamadas adicionais para essa mesma API antes da hora Retry-After retornará imediatamente com o erro original sem atingir o serviço.

Ao tentar novamente uma chamada, é prática recomendada executar retirada exponencial com uma variação aleatória para espalhar a carga para o serviço. XSAPI começa com um atraso de padrão de 2 segundos, que é controlado usando o xbox\_live\_contexto\_settings::set\_http\_repetir\_delay(). Isso significa que por padrão em cada repetição faz uma retirada exponencial de 2, 4, 8, segundos etc e ele jitters o atraso entre o atual e a próximo retirada valor com base no tempo de resposta a disseminação ainda mais carga em um conjunto de dispositivos, a tentativa de repetição.

Títulos devem estar no controle de como gastar tempo repetir uma chamada. Usando XSAPI, os desenvolvedores têm controle direto isso usando o função xbox\_live\_contexto\_settings::set\_http\_timeout\_window(). Por padrão, isso é definido como 20 segundos. Definir isso como 0 segundo efetivamente desativará a lógica de repetição. XSAPI agora também ajusta dinamicamente o tempo limite interno com base em quanto tempo esquerdo ainda resta no http\_timeout\_window(). Os controles de tempo limite HTTP internos quanto o sistema operacional passa a fazer a operação de rede HTTP antes de anular. A chamada não será repetida, a menos que permanece pelo menos 5 segundos, à esquerda no http\_timeout\_window() para fornecer um tempo razoável suficiente para a conclusão da chamada. Essa regra não se aplica a primeira chamada, por isso a configuração http\_timeout\_window() como 0 é aceitável e resultará em uma única chamada. Essa lógica tem o efeito que http\_timeout\_window() é muito mais determinista quando a chamada de API será retornada. Se um cabeçalho "Retry-After" foi retornado, nenhum reties serão feitas até após a hora "Retry-After" foi atingida. Se a hora "Retry-After" é posterior o http\_timeout\_window() e, em seguida, a chamada de retorno no final do http\_timeout\_window().


## <a name="error-handling"></a>Tratamento de erros

Os desenvolvedores do título devem **sempre** usar tratamento de erro apropriada **cada** chamada de serviço, eles precisam assegurar que eles estão manipulando respostas com falha corretamente.
 
Existem muitas condições do mundo real que podem resultar em uma solicitação com o Xbox Live para retornar códigos de falha, como

1.  Rede não está disponível. Por exemplo, o dispositivo perdido 4g perdido Wi-Fi, ou a rede foi desativado.
2.  Muita carga nos serviços de carga (503)
3.  Ocorreu uma falha no serviço (500)
4.  Número excessivo de solicitações onde enviados para o serviço (429)
5.  Conflito de operação (412) de gravação. Por exemplo, outro jogador em uma sessão para múltiplos jogadores enviado uma alteração primeiro
6.  O usuário foi vetado ou não tem permissão
7.  Usuário tiver assinado-out

Manipulador de erro apropriado é crucial para garantir que o jogo funcione corretamente nas seguintes condições.

XSAPI tem dois tipos de padrões de tratamento de erros. Um padrão ao usar as APIs do WinRT da C + + c++ /CLI CX, C\#, ou Javascript e outro padrão ao usar as novas APIs do C++. Detalhes completos sobre as práticas recomendadas de tratamento de erros, veja a página de doc Xbox Live "Tratamento de erros" em para um vídeo que aborda esse assunto, consulte a palestra na [ *Xfest 2015 vídeos* ](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2015.aspx) chamado *XSAPI: C++, nenhuma exceção!*


## <a name="best-calling-patterns"></a>Melhores padrões de chamada

### <a name="usebatching-requests"></a>Use solicitações de envio em lote

Alguns pontos de extremidade dá suporte a envio em lote ou de um conjunto de solicitações de agregação em uma única chamada. Por exemplo, com o serviço de perfil do Xbox Live é possível obter o perfil de um único usuário ou um conjunto de perfis de usuários. Portanto, se você precisar de um perfis de usuário para um conjunto de usuários, seria muito ineficiente para chamar o ponto de extremidade ou a API, um por vez para cada perfil de usuário. Cada chamada adiciona muita sobrecarga de autenticação. Portanto, em vez disso, passe todos os usuários que você deseja obter informações sobre uma vez para a API, para que o ponto de extremidade pode processar todos os perfis de usuário ao mesmo tempo e retornar uma única resposta.

### <a name="use-the-real-time-activity-rta-service-instead-of-polling"></a>Usar o serviço de atividade em tempo Real (RTA) em vez de sondagem

Outra prática recomendada é usar a sondagem periódica em vez disso, do serviço de atividade em tempo Real (RTA). Este serviço expõe um websocket que envia uma notificação aos clientes quando recursos de destino são alterados no serviço. O serviço de RTA fornece notificações sobre alterações de presença, alterações de estatísticas, alterações de documento de sessão para múltiplos jogadores e alterações da relação de redes sociais. Para saber o que o cliente de informações está interessado, o cliente deve primeiro assinar o item por websocket. Isso evita que o serviço para detectar alterações, pois você saberá exatamente quando o item é alterado de sondagem.

Assine o expõe XSAPI o RTA de serviço como um conjunto de APIs que os clientes podem usar. Cada uma dessas APIs têm correspondentes \* \_alterado\_manipulador APIs que levar em uma função de retorno de chamada que será chamada quando um item for alterado.

* presence\_service::subscribe\_to\_device\_presence\_change
<br>
* presence\_service::subscribe\_to\_title\_presence\_change
<br>
* user\_statistics\_service::subscribe\_to\_statistic\_change
<br>
* social\_service::subscribe\_to\_social\_relationship\_change<br>
 

## <a name="use-xbox-live-client-side-managers"></a>Usar gerenciadores de lado cliente Xbox Live

Novo no XSAPI agora temos um conjunto de gerentes que funcionam como máquinas de cache e estado que fazer todas as a pesada levantando para determinados cenários.


### <a name="social-manager"></a>Gerenciador de social

O Gerenciador de Social faz o trabalho pesado em torno de listas de amigos e perfis. Ele manterá seus amigos lista, seus perfis e seus dados de presença atualizado usando o serviço RTA. O Gerenciador de expõe uma API síncrona que é o mecanismo de jogo muito amigável. Jogos podem chamar suas APIs com frequência, como ele mantém um cache na memória, as informações mais recentes do serviço. Para obter mais informações, consulte a página de documentação "Introdução ao Manager Social" Xbox Live

### <a name="multiplayer-manager"></a>Gerenciador de vários jogadores

Para o gerenciamento de sessão para múltiplos jogadores, o Gerenciador de vários jogadores é uma solução de perfeito para jogos com vários participantes tradicionais. A API do Gerenciador de vários jogadores inclui gerenciamento de sessão e lista de player, manipula convites do jogos, junção em andamento, para que haja correspondência e conecta-se à sua solução de rede existente. Ele faz o trabalho pesado em torno de implementar fluxos para múltiplos jogadores tradicionais. Para obter mais informações, consulte a página de documentação "Introdução ao Multiplayer Manager" Xbox Live


## <a name="throttling-fine-grained-rate-limiting"></a>Limitação (limitação de taxa de refinada de BOM)

Serviços do Xbox Live têm limitação em vigor para impedir que qualquer dispositivo único colocar carga extrema no serviço. É importante saber quando o título foi limitado. Você pode informar se o título foi limitado de algumas maneiras diferentes:


### <a name="monitor-for-http-status-code-429"></a>Monitor para o código de Status HTTP 429

Você pode usar o Fiddler e observe se um código de Status HTTP 429 será retornada. A resposta JSON conterá detalhes sobre como o ponto de extremidade foi limitado. Por exemplo:

```json
{
  "version":1,
  "currentRequests":13,
  "maxRequests":10,
  "periodInSeconds":120,
  "limitType":"Rate"
}
```

Se você estiver usando XSAPI, APIs retornarão um http\_status\_429\_muito\_muitos\_solicita o erro e definir a mensagem de erro a ser detalhes sobre como a API foi limitada.

### <a name="debug-asserts"></a>Declarações de depuração

Ao usar XSAPI, se a chamada é limitada em uma área restrita do desenvolvedor e usando um build de depuração do título, declarará imediatamente permitir que o desenvolvedor sabe que ocorreu uma limitação. Isso é para evitar erro de limitação 429 acidentalmente ausentes devido ao código escrito incorretamente. Se você desejar desabilitar essas declarações para continuar trabalhando sem corrigir o código incorreto, você pode chamar essa API:


> xboxLiveContext -&gt;Settings () -&gt;desabilite\_declara\_para\_xbox\_live\_limitação\_na\_dev\_ as áreas restritas (xbox\_live\_contexto\_acelerador\_setting::this\_código\_precisa\_para\_ser\_alterado\_ao\_evitar\_limitação);

mas observe que essa API não impedirá que o título do que está sendo limitado. O título ainda será limitado. Isso simplesmente desativa as declarações ao desenvolvimento em áreas restritas em durante o uso de um build de depuração. 

### <a name="xbox-live-trace-analyzer-tool"></a>Ferramenta Analisador de rastreamento do Xbox Live

Outra opção é gravar um rastreamento de chamadas de Xbox Live e, em seguida, analisar esse rastreamento usando o [ferramenta Analisador de rastreamento do Xbox Live.](https://docs.microsoft.com/windows/uwp/xbox-live/tools/analyze-service-calls)

Para gravar um rastreamento, você pode usar Fiddler para registrar um. Arquivo SAZ, ou usando o log de rastreamento interno de XSAPI. Para obter mais informações, como usar o turno em rastreamentos em XSAPI consulte a página de documentação "Analisar chamadas para serviços do Xbox Live" Xbox Live. Depois que um rastreamento, o Xbox Live rastreamento analisador ferramenta avisará ao detectar limitado de chamadas.

## <a name="is-xbox-live-up"></a>É o Xbox Live para cima?

Xbox Live é uma coleção de microsserviços que expõem recursos do Xbox Live, como perfil, amigos e presença, estatísticas, placares de líderes, conquistas, com vários participantes e o relacionamento de pessoas. Não há um único servidor ou o ponto de extremidade que define se o Xbox Live estiver em execução. Se um único servidor ficar inativo, o restante dos microsserviços Xbox Live são amplamente independente e deve estar operacional.

Se um único serviço sofrer uma interrupção temporária, é importante saber se essa chamada de serviço for de missão crítica para o seu jogo. Tente fornecer experiência razoável, embora haja intermitentes de rede ou problemas de serviço. Por exemplo, se o serviço de presença está retornando falha chamar provavelmente não é crítico para o seu jogo. Então, simplesmente relatar ao usuário a última presença conhecido em vez de relatar Xbox Live está inativo.

Também Xbox Live segue o modelo de consistência de consistência eventual. Isso significa que, se não há novas atualizações são feitas, que, eventualmente, todas as solicitações para esse recurso relatará o último valor atualizado. Na verdade, isso significa que se propaga um curto período em que as informações estão obsoletas como os dados.
