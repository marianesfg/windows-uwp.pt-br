---
title: Práticas recomendadas para offline
description: Saiba mais sobre as práticas recomendadas para lidar com cenários offline com títulos do Xbox Live habilitado.
ms.assetid: 6290dd67-1145-4fe2-8ada-c3a29a9ad29a
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox, offline
ms.localizationpriority: medium
ms.openlocfilehash: 5680b045873ebf6eed1422cb915f3f53df748790
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618721"
---
# <a name="best-practices-for-offline"></a>Práticas recomendadas para offline

Xbox One foi projetado como um dispositivo conectado, e as melhores experiências, como jogos com vários participantes e streaming de vídeo, exigem conectividade. No entanto, Xbox One oferece suporte a muitas formas de reprodução offline. Este tópico discute como lidar com a reprodução off-line.

## <a name="overview"></a>Visão geral

Um número crescente de consumidores em todo o mundo têm acesso à Internet. No entanto, há ainda lugares do mundo em que a conectividade é imprevisível, e há vezes quando roteadores falharem, fibra será recortado, falhas de servidores ou serviços sem fio descartar.

Para dar suporte o conjunto mais amplo de consumidores e experiências, Xbox One permite para casos comuns em que a conectividade com a Internet é perdida de tempos em tempos ou completamente não está disponível. Seu jogo é informado sobre falhas de conectividade, e você é livre para decidir como reagir — se o jogo completo que continua, fazer downgrade para o modo offline ou terminando o jogo completamente.

## <a name="normal-online-operation"></a>Operação online normal

Em geral, consoles Xbox One operam em um estado totalmente conectada, em que o usuário tem uma conexão de Internet estável e acesso completo ao Xbox Live e a serviços de terceiros. Esse estado conectado permite que o serviço Xbox Live validar periodicamente o estado de console, para fornecer atualizações e para executar outros serviços em segundo plano que se beneficiam de seu jogo e os usuários.

É recomendável que você supõe que os usuários tenham conectividade online a maioria das vezes.

## <a name="offline-play-principles"></a>Princípios de reprodução offline

Há casos em que uma conexão online não está disponível. Para reproduzir off-line, Xbox One foi desenvolvido com os seguintes princípios em mente:

-   Mais importante, mantenha os usuários jogando apesar dos problemas de conectividade.

-   Manter usuários jogando mesmo se eles não têm nenhuma conexão em todos os.

-   Fazer tocar offline simples e previsível para os usuários, mantendo ainda o espírito de uma experiência sempre conectados.

## <a name="offline-modes"></a>Modos offline

Há dois alto nível *perda de conectividade* cenários:

-   Perda completa de serviços de Internet

-   Perda de um ou mais serviços online

Em cada um desses modos, há uma variedade de situações que podem surgir. Eles são explicados abaixo com exemplos de cenários offline comuns que afetam o jogo.

## <a name="offline-scenario-no-internet-service-upon-game-start"></a>Cenário offline: nenhum serviço de Internet ao jogo Iniciar

Jogos podem declarar a mesmos como um dos três tipos:

-   *Xbox Live necessário*: todos os modos de jogo exigem conectividade com a Internet.

-   *Xbox Live Gold necessário*: todos os modos de jogo exigem conectividade com a Internet e uma associação Xbox Live Gold.

-   *Xbox Live não é necessário*: o jogo tem pelo menos um modo de jogo que não exige conectividade com a Internet. Tecnicamente, esse tipo não é explicitamente declarado no manifesto do aplicativo. Um aplicativo que não declara a mesmo como um dos dois primeiros tipos é considerado "Não obrigatório, o Xbox Live" ou o suporte offline.

Quando um jogo for iniciado pelo usuário e o console está offline, o sistema verifica a declaração de jogo de conectividade no manifesto do aplicativo. Se o jogo exigir conectividade (qualquer um dos dois primeiros casos acima), o sistema exibe uma mensagem para o usuário automaticamente e não inicia o título.

Se o console estiver offline, o sistema só iniciará títulos que têm pelo menos um modo de jogo que não exige conectividade. Em outras palavras, o sistema não iniciará "Xbox necessários ao vivo" ou "Ouro de Xbox Live required" títulos.

## <a name="offline-scenario-connectivity-lost-during-gameplay"></a>Cenário offline: conectividade perdida durante o jogo

Se um jogo já está em execução quando a conectividade for perdida, o título será notificado pelo sistema. Se o jogo não está usando para serviços online, continue a sessão sem interrupção. Se o jogo estiver usando ativamente a serviços online, alterne para um modo no qual esses serviços não são mais necessários ou informar o player que a sessão de jogos está terminando devido ao estado offline.

Títulos que são declarados "Xbox Live required" ou "Ouro de Xbox Live necessária" será automaticamente suspensa pelo sistema quando o console perde toda a conectividade de rede por um período de tempo e o sistema fornecerá automaticamente uma mensagem de erro para o usuário.

Como com qualquer outro cenário que envolve a suspensão do jogo, você deve salvar o estado para que o usuário não perder os dados e pode rapidamente retorne ao estado depois de retomar a conectividade.

## <a name="offline-scenario-when-a-single-xbox-live-service-is-down"></a>Cenário offline: quando um único serviço Xbox Live está inativo

Há outras situações em que serviços on-line específicos talvez não estão disponíveis mesmo que a conectividade com a Internet é bom.

Por exemplo, um único serviço Xbox Live poderia estar offline por um curto período de tempo. Nesse caso, a chamada específico ao serviço atingirá o tempo limite ou relatar um erro ao jogo. Você deve lidar adequadamente com estados de serviço offline exatamente como você lidaria com essas situações, no Xbox 360 ou no Windows.

No mínimo, o jogo não deve falhar ou travar. Se o jogo não pode continuar sem o serviço, reportar a situação para o usuário e permitir que o usuário continue em outra área do jogo em que o serviço não é necessário.

Na melhor das hipóteses, continue a jogabilidade e o cache de dados para enviar mais tarde (se o jogo estava escrevendo para o serviço) ou fazer suposições razoáveis sobre os dados (se o jogo estava lendo do serviço).

## <a name="offline-scenario-when-a-third-party-service-is-down"></a>Cenário offline: quando um serviço de terceiros está inativo

Se o seu jogo se baseia em um serviço de terceiros online, seu jogo também deverá ser resiliente se esse serviço falhar. Se o serviço falhar, em seguida, seu jogo deve não falhar ou travar. Você pode relatar o erro de serviço para o usuário se o jogo não pode continuar. Idealmente, jogabilidade deve continuar ou você deve permitir que o usuário continue em uma área do jogo que não exige o serviço online.

## <a name="offline-scenario-when-xbox-live-cloud-compute-is-down"></a>Cenário offline: quando a computação de nuvem Xbox Live está inoperante

Um dos apresenta Xbox One é o poder da nuvem. Alguns jogos podem depender completamente um serviço sempre conectados, como computação de nuvem Xbox Live, que permite o acesso a recursos de computação adicionais ou servidores de jogo sempre disponível. Nesse modo sempre conectado foi permitido e incentivado onde ele aprimora a experiência de jogadores.

Se seu jogo usa esse modo, é recomendável que seu jogo seja resistente a interrupções de serviço (de vários segundos e até um minuto) que são devido a qualquer perda total de conectividade com a Internet ou de um serviço de nuvem específico. No entanto, o jogo não é necessário ter um modo offline. Se o jogo realmente precisa de conectividade e a conectividade não estiver disponível, em seguida, informar ao usuário e encerrar a sessão de jogo.

## <a name="xbox-requirements"></a>Requisitos do Xbox

O requisito mais importante ao lidar com cenários offline é a estabilidade do jogo. Seja a perda de conectividade completo ou apenas a perda de um serviço online específico, seu jogo deve não parar de responder, falhas ou fazer com que o usuário perder o estado. Seu jogo deve ter um sistema robusto para lidar com cenários de tempo limite de rede e de manipulação de erros retornados de qualquer API que acessa um serviço online.

Jogos não são necessários para dar suporte a reprodução off-line. Se o seu jogo simplesmente não pode continuar devido a uma perda de conectividade do serviço, em seguida, notificar o usuário encerrar a sessão de jogos e retornar ao menu principal ou estado interativo inicial.

## <a name="best-practices"></a>Práticas recomendadas

Aqui estão as práticas recomendadas para lidar com situações offline:

-   Jogos para assumir que os usuários tenham conectividade online a maioria do tempo de design.

-   Se isso fizer sentido para seu projeto de jogo, considere criar modos de jogo que permitem que o usuário tenha uma experiência agradável, mesmo se o console estiver em um estado offline.

-   Os serviços podem se tornar indisponíveis. Conexões podem falhar. Compile para as APIs que seja possível tempo limite, ou as condições de falha de relatório quando um serviço online está inoperante ou quando a conectividade com a Internet é perdida de tratamento de erros robusto. Sempre que possível, mantenha os usuários jogando Apesar desses problemas.

-   Obedecer ao Xbox requisitos (XRs). Não pare ou falhe.

-   Ao receber uma notificação de suspensão do PLM título, salve estado, para que o usuário não perde os dados e possa retornar rapidamente a esse estado ao retomar o jogo.

-   Sinalizador seu título adequadamente no manifesto do aplicativo. Marca seu título apenas como "Xbox Live required" se precisam de todos os modos de jogo de conectividade.

-   Jogos Xbox One têm permissão para usar e confiam nos serviços online em todos os modos de jogos. Se o jogo simplesmente não pode continuar devido a uma perda de conectividade do serviço, em seguida, notificar o usuário, encerrar a sessão de jogos e retornar ao menu principal ou estado interativo inicial.

-   Não confie no serviço de Ajuda do Xbox para mensagens de erro e Ajuda relacionados aos estados offline. O serviço Xbox ajuda requer uma conexão com o Xbox Live.

## <a name="conclusion"></a>Conclusão

Xbox One foi projetado para um mundo sempre conectado. No entanto, no caminho para essa visão, o console permite que os jogadores aproveitando a cenários offline. Jogos devem ser robustos diante de falhas de conectividade. Para jogos com experiências de reprodução off-line, permitir que seus jogadores para aproveitar essas experiências ao máximo. Para jogos que estão sempre online quando a conectividade de rede for perdida, normalmente retornam o usuário para um estado offline.
