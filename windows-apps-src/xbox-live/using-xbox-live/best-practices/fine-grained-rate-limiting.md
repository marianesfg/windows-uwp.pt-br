---
title: Limitação de taxa de refinada multa de Xbox Live
description: Saiba como o Xbox Live refinada limitação de taxa funciona e como evitar que o título seja classificar limitado.
ms.assetid: ceca4784-9fe3-47c2-94c3-eb582ddf47d6
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox, limitação, limitação de taxa
ms.localizationpriority: medium
ms.openlocfilehash: b79a4f0a873ffaf5dd824a0c9832f61aa4a7d77f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628681"
---
# <a name="xbox-live-fine-grained-rate-limiting"></a>Limitação de taxa de refinada multa de Xbox Live

## <a name="introduction"></a>Introdução

Este artigo fornece uma visão geral do Xbox Live a limitação de taxa refinada tudo bem. Além de resumir o que a limitação de taxa é, que este documento também pretende ajudar você a determinar se você estiver limitado e se você estiver, quais ferramentas e recursos estão à sua disposição.

## <a name="terminology-guide"></a>Guia de terminologia

| Termo         | Definição                                                  |
|--------------|-----------------------------------------------------------------------------
| FGRL         | Refinadas a limitação de taxa                                                  
| XSAPI        | Interface de programa de aplicativo de serviços do Xbox                                 
| CU           | Atualização de conteúdo                                                              
| Intermitência        | Representa um volume de solicitações recebidas em um curto período de tempo          
| Sustentar      | Representa um alto volume de chamadas recebidas constantemente durante um período de tempo
| Título de usuário + | Representa o emparelhamento de um usuário e título como uma entidade                    
| XLTA         | Ferramenta Analisador de rastreamento do Xbox Live, usada para determinar se o título está sendo a limitação de taxa

## <a name="xbox-live-fine-grained-rate-limiting"></a>Xbox Live a limitação de taxa refinada fina

**Bem mais refinado de limitação de taxa** contratou a promover **justa uso** de recursos compartilhados Xbox Live entre títulos diferentes. Essa solução é, como sistemas de limitação mais tradicionais que tem um serviço que mantém uma contagem do número de solicitações de que uma entidade já fez em um determinado período de tempo.

Entidades que atingirem o limite de serviços especificado são movidas para um estado de rejeição em que todas as solicitações recebidas da entidade serão ativadas imediatamente. Entidades só conseguem sair nesse estado quando determinado período de tempo expira, fazendo com que a contagem de entidades associado redefinir.

Tudo bem mais refinado a limitação de taxa usa a mesma mecânica core mencionada acima no entanto, em vez de uma entidade de rastreamento FGRL controla a combinação de **usuário e o título** e o compara a contagem associada a **dois** diferentes **limites** em contraste com a um. Limites de duplo FGRL são aplicados em cada serviço, que significa que a contagem de solicitações para GameClips não afetará a contagem de solicitações para a presença. As seções a seguir entrarão em mais detalhes sobre o usuário e o emparelhamento de título, limitando dupla e o objeto de resposta HTTP 429 limitador.

## <a name="fair-usage"></a>Uso justo

Xbox Live acredita que cada usuário deve ter a mesma alta qualidade experiência não importa qual jogo/aplicativo que ele está em execução. FGRL foi projetado para resolver o cenário a seguir:

O desenvolvedor A acaba de lançar um título que segue todas as o Xbox live, garantindo o uso ideal dos serviços enquanto o desenvolvedor B também acaba de lançar um título de práticas recomendadas no entanto isso tem um bug desconhecido. Esse bug faz com que o título e a cada usuário para enviar spam a presença que resulta no serviço vai sob carga pesada. O serviço reduz a e, eventualmente, interrompe a quebra a experiência para os usuários do desenvolvedor do mesmo se ele fosse o bug do desenvolvedor B que causou o problema. Se FGRL foi implementado o serviço teria sido capaz de parar de receber solicitações da perda se comportando title, permitindo que ele para servir o título de desenvolvedor do seu justa fatia da pizza recursos.

## <a name="title-and-user-granularity"></a>Granularidade de título e o usuário

Título e usuário foram escolhidos como a chave para garantir um uso justo de recursos do Xbox Live. Apenas para o usuário de controle, você criaria um cenário em que a experiência do usuário seria à mercê do cada integração títulos. Por exemplo, a maioria dos títulos de usar o serviço de pessoas já, portanto, para este exemplo, digamos que bem mais refinado a limitação de taxa foi configurada no serviço de pessoas, permitindo que não mais de 100 solicitações em 5 minutos. Se um usuário para um jogo que feitas 100 solicitações em 1 minuto seria excedido o limite e o usuário não seria capaz de fazer mais solicitações para o serviço de pessoas. Imagine que no mesmo período do usuário, em seguida, volta para a tela inicial e clica em sua lista de amigos: Uma vez que o usuário já excederia o limite de chamada de lista de amigos falharia até cinco minutos de intervalo foi decorrido, mesmo que a tela inicial não era responsável por colocar o usuário no estado limitado.

Como alternativa, limitando com base apenas no título geraria um resultado igualmente injusto. Definir um limite por título ignoraria a popularidade dos títulos e as solicitações seriam apenas ser primeiro chegada até que o limite for alcançado.

O emparelhamento de usuário e o título garante que nenhum título usa mais recursos do que o que é apropriado devido ao número de usuários ativos ao mesmo tempo dando cada usuário uma fatia da pizza recursos de consistente.

![](../../images/FGRL.png)

O diagrama acima mostra uma exibição de alto nível de como a solicitação é tratada. Primeiro, a solicitação é gerada e, em seguida, recebida pelo serviço desejado. Ao receber a solicitação, o sistema verifica para ver quantas vezes o usuário e o título junto tem acessado o serviço. Se a solicitação estiver abaixo do limite, em seguida, ela será processada normalmente. Se a solicitação é considerada como em ou acima do limite, os serviços serão solte-o e em vez disso, retornar uma resposta 429. A resposta indica quanto tempo até o período reverte ao longo e as solicitações de usuário e o título podem ser manipuladas.

## <a name="burst-and-sustain-limits"></a>Intermitente e manter limites

Tradicionalmente, a limitação de taxa consiste em um limite por ponto de extremidade que for acompanhado ao longo de um determinado período de tempo. Esse período representa a quantidade de tempo que uma contagem de solicitação de entidades é rastreada. No final do período, a contagem de entidades é redefinida como 0 para começar a rastreá-lo novamente.

Essa abordagem funciona para a maioria das API do porém não era flexível o suficiente para jogos e aplicativos que chamam o Xbox live. A solução acima pressupõe que as pessoas forem chamados em uma constante manor previsível a consistente. No caso do Xbox Live, dependendo do serviço e aplicativo/solicitante title, os padrões de chamada são totalmente diferentes.

Escolha apenas um limite nesse caso, exigiria comprometer em ambas as extremidades do espectro de padrão de chamada. A solução do Xbox Live usa dois períodos e limites. O período de menor é chamado o período de intermitente, enquanto a mais de um maior é conhecido como o período de manter. O tempo de intermitência de período para FGRL sempre é 15 segundos, enquanto a mantenha sempre é de 300 segundos (5 minutos) então durante um período de manter de 5 minutos há 20 intermitente períodos. Ambos intermitente e limites de sustentar acompanhando ao mesmo tempo e como tal, contagem de solicitações ao mesmo tempo. A intermitência e o limite de manter são definidas no serviço, que significa que cada serviço tem seu próprio contagem de intermitente e manter. Para ajudá-lo a entender como esses dois limites funcionam juntos a tabela abaixo mostra um usuário reproduzir um título que está fazendo um número de solicitações de um serviço que implementou FGRL. Nesse caso, o limite de intermitente é de 30 solicitações em 15 segundos e o limite de manter é 100 solicitações em 5 minutos.

| Período de tempo (segundos)  | Solicitações por período de intermitente  | Solicitações por período de sustentar  | \# de solicitações limitadas dentro do intervalo de 15 s | Qual limite? (intermitência, sustentar, ou ambos)  |
|-------------|--------------|----------------|-----------------------------------------------------|---------------------------|
| 0-15        | 35           | 35             | 5                                                   | Intermitência                     |
| 15-30       | 28           | 63             | 0                                                   | N/D                       |
| 30-45       | 21           | 84             | 0                                                   | N/D                       |
| 45-60       | 36           | 120            | 20                                                  | Ambos                      |
| 60-75       | 24           | 144            | 24                                                  | Sustentar                   |
| …           | …            | …              |                                                     | …                         |
| 285-300     | 4            | 148            |                                                     | Sustentar                   |

A tabela que mostra os primeiros 15 segundos os percursos do usuário de intermitente limitar fazendo solicitações de 35. Essas solicitações extras 5 são descartadas e 5 429 respostas são enviadas. Vale a pena observar que esses 5 solicitação entanto limitada ainda contam para o limite de manter. Depois que o limite é ultrapassado nenhuma solicitação é deixar passar conforme mostrado quando os limites corrida na segunda marca de 45 e novamente quando apenas 4 solicitações são feitas com o segundo 285 marcam.

## <a name="http-429-response-object"></a>HTTP 429 objeto de resposta

Quando a contagem de usuário e o título associada é em ou acima de qualquer um de intermitente ou sustentar limite o serviço não manipulará a solicitação e em vez disso, retornará uma resposta HTTP 429. Significa o código HTTP 429 "muitas solicitações" será acompanhada por um cabeçalho que contém um valor de "repetição após X segundos". FGRL 429 contém uma nova tentativa após o cabeçalho, que especifica a quantidade de tempo que as entidades de chamada devem esperar antes de tentar novamente. Os desenvolvedores que usam XSAPI não precisará se preocupe, pois XSAPI honra e manipula o cabeçalho Retry-After. A resposta real conterá os seguintes campos:

| Nome do campo      | Tipo de valor | Exemplo                | Definição                       |
|-----------------|------------|------------------------|----------------------------------|
| Versão         | Inteiro    | `"version":1`          |                                  |
| currentRequests | Inteiro    | `"currentRequests":13` | Número total de solicitações enviadas    |
| maxRequests     | Inteiro    | `"maxRequests":10`     | Número total de solicitações permitidas |
| periodInSeconds | Inteiro    | `“periodInSeconds”:15` | Janela de tempo                      |
| Tipo            | String     | `“type”:”burst”`       | Tipo de limite de limitação              |

## <a name="implemented-limits"></a>Limites implementados

Os seguintes serviços implementou limites FGRL, com a imposição desses limites em vigor desde **maio de 2016**. Para reiterar, esses limites será o mesmo em todas as áreas restritas e títulos. **Qualquer título que foi publicado por meio da plataforma de desenvolvedor do Xbox ou Partner Center e fornecido antes de maio de 2016 será considerado herdado e, portanto, isentos.**

| **Nome** | **Limite de intermitência** (15 segundos por usuário por título) | **Limite de sustentar** (300 segundos por usuário por título) | **Limite de certificação** (10 x Sustained, 300 segundos por usuário por título) |
|----------------------------|---------------------------|----------------------------|----------------------------|
| Leitura de estatísticas                 | 100                       | 300                        | 3000                       |
| Perfil                    | 10                        | 30                         | 300                        |
| MPSD                       | 30                        | 300                        | 3000                       |
| Presença                   | 10, 3 de gravação de leitura          | Leitura gravação 30 100         | 1000 de leitura, gravação 300       |
| Redes sociais                     | 10                        | 30                         | 300                        |
| Placares de líderes               | 30                        | 100                        | 1000                       |
| Conquistas               | 100                       | 300                        | 3000                       |
| Correspondência inteligente                | 10                        | 100                        | 1000                       |
| Postagens do usuário                 | 100                       | 300                        | 3000                       |
| Gravação de estatísticas                | 100                       | 300                        | 3000                       |
| Privacidade                    | 10                        | 30                         | 300                        |
| Clubes                      | 10                        | 30                         | 300                        |

A tabela acima representa a lista atual dos serviços que foram selecionadas para FGRL. Essa lista não é final como novos serviços e os serviços existentes podem ser adicionados. Quando um serviço fará a ser adicionado a tabela será atualizada e um aviso será feito. Os limites representados na tabela também não devem ser considerados como finalizado. Como alterar de serviços e evoluir isso será muito os limites, no entanto, você será notificado e as isenções herdadas necessárias serão feitas.

Desde **abril de 2018** títulos não irá passar na certificação se eles excederem a mantenha limitar (limite no qual taxa de limitação entra em vigor) por 10 vezes.  Por exemplo, se o limite prolongado na qual FGRL entra em vigor é definido como 300 chamadas em 300 segundos, conforme especificado na tabela acima, títulos ou acima 3000 chamadas em 300 segundos falhará certificação. 

## <a name="service-mapping-and-title-effects-of-rate-limiting"></a>Mapeamento de serviço e efeitos de título de limitação de taxa

| **Nome** | **Ponto de extremidade de serviço** | **Impacto do jogo Anticipiated de FGRL**
| --- | :---: | --- 
| Leitura de estatísticas | userstats.xboxlive.com | Realizações ou placares de líderes entradas não atualizada ou recuperada.
| Perfil | profile.xboxlive.com | Dados do jogador não atualizado ou exibidos corretamente.
| MPSD | sessiondirectory.xboxlive.com | Junções/convites não serão concluídos corretamente, sessões não criada ou atualizada corretamente que pode causar falhas de título.
| Presença | presence.xboxlive.com | Presença de jogos do jogador não seria precisa.
| Redes sociais | social.xboxlive.com | Afeta todas as gravações de amigos (por exemplo, adicionando um amigo, tornar alguém Favoritos etc.) e pode afetar as leituras de amigo (por exemplo, buscar minha lista de amigos). Os desenvolvedores são incentivados a chamar o peoplehub para leitura, em vez de social.xboxlive.com.
| Placares de líderes | leaderboards.xboxlive.com | Experiência do usuário do jogo para o placar de líderes seria não preencher/atualização.
| Conquistas | achievements.xboxlive.com | Experiência do usuário no jogo para realizações desbloqueada não seria atualizada.
| Correspondência inteligente | momatch.xboxlive.com | Correspondências poderiam não ser configuradas com êxito.
| Postagens do usuário | userposts.xboxlive.com | Postagens do usuário não seriam exibido.
| Gravação de estatísticas | statswrite.xboxlive.com | Realizações ou placares de líderes entradas não atualizadas.
| Privacidade | privacy.xboxlive.com | Falhas de privacidade podem resultar em acesso bloqueado para todos os chamadores.
| Clubes | Clubhub.xboxlive.com | Player pode não ser capaz de ver suas paus no jogo.

**OBSERVAÇÃO:** O mapeamento de API mais recente é atualizado regularmente e podem ser encontrado em [mapeamento de API de analisador de rastreamento ao vivo](https://github.com/Microsoft/xbox-live-trace-analyzer/blob/master/Source/XboxLiveTraceAnalyzer.APIMap.csv).

## <a name="faq"></a>Perguntas frequentes

## <a name="how-can-i-determine-i-am-being-throttled-and-what-steps-can-i-take"></a>Como posso determinar eu estou sendo limitado e as etapas que posso fazer?

Consulte a [Xbox Live práticas recomendadas](best-practices-for-calling-xbox-live.md) documentar conforme ele conterá as etapas para aperfeiçoar seu padrão de chamada, bem como obter uma explicação de como os gerenciadores de XSAPI Social e com vários participantes e a asserção XSAPI podem ser usados para notificar você e atenua problemas de limitação.

Outra opção é gravar um rastreamento de chamadas de Xbox Live e, em seguida, analisar esse rastreamento usando o [ferramenta Analisador de rastreamento do Xbox Live](https://docs.microsoft.com/windows/uwp/xbox-live/tools/analyze-service-calls).  Para gravar um rastreamento, você pode usar Fiddler para registrar um. Arquivo SAZ, ou usar o log de rastreamento interno de XSAPI. Para obter mais informações, como usar o turno em rastreamentos em XSAPI consulte a página de documentação "Analisar chamadas para serviços do Xbox Live" Xbox Live. Depois que um rastreamento, o Xbox Live rastreamento analisador ferramenta avisará ao detectar limitado de chamadas.

Você pode encontrar as práticas recomendadas de papel no docs GDNP e o SDK e o XDK 1602 e superior.

### <a name="can-limits-change"></a>Podem alterar os limites?

A intenção é que os limites publicados não serão alterados ao longo do tempo. No entanto, se surgir a necessidade, é possível que alguns dos limites podem ser feita mais rígidas; Nesse caso, títulos já lançados para o varejo estarão isentos os limites atualizados.

### <a name="are-more-services-going-to-get-limits"></a>Mais serviços serão obter limites?

Sim, os novos serviços e os serviços mais pode e será ser a criação de limites. No entanto, como a primeira versão FGRL você será notificado e as precauções adequadas serão executadas.

### <a name="when-will-these-changes-take-effect"></a>Quando essas alterações entrarão em vigor?

Limites de taxa forem aplicados desde **maio de 2016**.  Desde **abril de 2018**, títulos, excedendo especificado sustentada limites de 10 vezes ou mais não passa pelo processo de certificação do Xbox.

### <a name="what-if-we-cant-adhere-to-the-limits"></a>E se podemos não puder aderir aos limites?

Consulte a [práticas recomendadas da Xbox Live](best-practices-for-calling-xbox-live.md) e verifique se você estiver seguindo estas etapas.  Considere também usar o [manager social](../../social-platform/intro-to-social-manager.md) se você estiver sendo taxa limitada com qualquer um dos serviços sociais.

Se, depois de seguir essas etapas, você não ainda consegue permanecem sob os limites, entre em contato com seu gerente de conta do desenvolvedor.

**OBSERVAÇÃO: Títulos em ou acima de 10 vezes o limite de manter especificado serão permitidos para passar o certificado depois de abril de 2018**.  Por exemplo, se o limite prolongado na qual FGRL entra em vigor é definido como 300 chamadas em 300 segundos, conforme especificado na tabela acima, títulos ou acima 3000 chamadas em 300 segundos falhará certificação.

### <a name="what-about-my-existing-title"></a>E o título da minha existente?

Títulos de nenhum no varejo antes de abril de 2018 são considerados herdado e estão isentos.

### <a name="content-updates"></a>Atualizações de conteúdo?

Para um título herdado ou isento, atualizações de conteúdo também serão isentas, embora, recomendamos que você aproveite as ferramentas e os ativos para otimizar os aspectos de integração de serviço do seu jogo.

### <a name="can-i-get-an-exemption-for-my-game-until-i-can-make-a-content-update"></a>Posso obter uma isenção para o meu jogo até que posso fazer uma atualização de conteúdo?

Fale com seu gerente de conta de desenvolvedor.
