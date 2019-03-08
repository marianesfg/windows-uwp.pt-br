---
title: Práticas recomendadas de RTA
description: Saiba mais sobre as práticas recomendadas ao usar o serviço de atividade do Xbox Live em tempo real.
ms.assetid: 543b78e3-d06b-4969-95db-2cb996a8bbd3
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, a atividade em tempo real
ms.localizationpriority: medium
ms.openlocfilehash: 7733aab9330c316ad5938cf9a2ef763e06f19b9a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613561"
---
# <a name="real-time-activity-rta-best-practices"></a>Práticas recomendadas de atividade (RTA) em tempo real
Essas práticas recomendadas ajudará você a aproveitar ao máximo uso do seu título RTA.


## <a name="the-basics"></a>Os conceitos básicos

RTA usa sessões do WebSocket para criar uma conexão persistente com o cliente. Isso é como o serviço fornecerá estatísticas para você. Um cliente precisa enviar uma solicitação de conexão autenticada e RTA usará o token fornecido na solicitação para verificar a conexão pode ser feita e, em seguida, selecioná-lo.

Depois que a conexão tiver sido estabelecida, seu aplicativo pode fazer solicitações para inscrever-se às estatísticas específicas. Em uma assinatura com êxito, o RTA retornará o valor atual e alguns metadados adicionais, como o tipo de estatística, como parte da carga de resposta. Em seguida, RTA para encaminhar todas as atualizações que ocorrem com a estatística, enquanto o cliente está inscrito.

Quando o título não requer atualizações em tempo real em uma estatística, ele deve encerrar sua assinatura para essa estatística.


## <a name="handling-disconnects"></a>Desconecta de manipulação

Títulos devem estar cientes de que, quando o token de autenticação do usuário expira, sua sessão será encerrada pelo serviço. O título deve ser capaz de detectar quando tal evento ocorrer, reconectar-se e assinar novamente todas as estatísticas que ele foi assinado anteriormente.

Conexões de RTA são fechadas após duas horas, por design, o que forçará o cliente para se reconectar. Isso é feito porque o token de autenticação para a conexão é armazenada em cache para economizar largura de banda de mensagem. Eventualmente, esse token irá expirar. Fechando a conexão e forçar o cliente para reconectar-se o cliente é forçado a atualizar o token de autenticação.

Um cliente também pode obter desconectado devido a ISP do usuário com problemas ou quando o processo para o título é suspenso. Quando isso acontece, um evento de WebSocket é gerado para informar o cliente. Em geral, é melhor prática para ser capaz de lidar com desconecta do serviço.

> [!WARNING]
> Se um cliente usa RTA para sessões com vários participantes e está desconectado por trinta segundos, o [Directory(MPSD) de sessão para múltiplos jogadores](../multiplayer/multiplayer-appendix/multiplayer-session-directory.md) detecta que a sessão RTA é fechada e é acionada logoff da sessão de usuário. Cabe ao cliente RTA para detectar quando a conexão é fechada e iniciar uma reconexão e assinar novamente antes do MPSD encerra a sessão.

## <a name="managing-subscriptions"></a>Gerenciamento de assinaturas

Em relação ao gerenciamento de assinaturas quando um token expira, o título deve controlar em todos os horários que foram feitas as assinaturas. Após inscrever-se com êxito, RTA retorna um identificador exclusivo para cada estatística inscrito. Em todas as atualizações futuras para essa estatística, o identificador será usado em vez do nome da estatística. Isso é feito para economizar largura de banda. Os clientes são responsáveis por manter seu próprio mapeamento das estatísticas para IDs de assinatura RTA.


## <a name="unsubscribing"></a>Cancelando a inscrição

Ter assinaturas não utilizadas não é recomendado. O serviço limita o número de assinaturas que um usuário pode ter por título em um determinado momento. Se você estiver assinando tudo e qualquer coisa, você pode atingir esse limite, e isso pode impedir que a assinatura para algumas estatísticas importantes. (Para obter mais informações sobre limites de assinatura, consulte **limita**, mais adiante neste tópico.)

Por exemplo, seu título apenas talvez seja necessário uma assinatura a uma cena determinados. Quando o usuário insere essa cena, o título deve assinar. Quando o usuário deixa essa cena, essas estatísticas devem ser canceladas. Da mesma forma, há uma mensagem de cancelamento da assinatura-todos que pode ser usada se nenhuma assinatura forem necessários.

Depois de cancelar a assinatura, o identificador de assinatura para essa estatística não será usado.


## <a name="awareness-of-latent-items-in-the-queue"></a>Reconhecimento de latentes itens na fila

Ao cancelar a assinatura de uma estatística, é possível que haja uma atualização para a estatística ainda está no processo atingir seu cliente. Portanto, mesmo se seu título cancelou a assinatura apenas de uma estatística, lembre-se de que ele ainda pode obter uma ou duas atualizações relacionadas a essa estatística.

A recomendação é para ignorar as atualizações de estatísticas quando o identificador de assinatura não é reconhecido.


## <a name="ignore-messages-you-do-not-understand"></a>Ignorar mensagens que você não entender

É possível que o protocolo de mensagem será atualizado. Para manter seu aplicativo independente de quaisquer novas mensagens, é recomendável que seu título simplesmente descartar tipos de mensagem desconhecido.


## <a name="throttles"></a>Restrições

RTA impõe determinadas restrições do serviço:

-   UserStats: 1000
-   Presença: 2500

Se um cliente atingir o limite de limitação que ele vai receber um erro como parte de uma chamada de assinar/cancelar a assinatura ou será desconectado. Em ambos os casos, para obter mais informações sobre a limitação de limitação que foi atingida serão disponíveis para o cliente juntamente com a mensagem de erro ou mensagem de desconexão.

Ao desenvolver seu título, tenha esses conceitos em mente. Se você estiver fazendo algo extremo, você pode ter uma experiência de aplicativo degradado porque o serviço poderia ser limitando suas chamadas. Direito agora, o serviço permite que 1.000 assinaturas para as estatísticas por instância de um aplicativo. Para que, uma instância de um aplicativo também pode assinar o comprimento da lista inteira de pessoas do usuário para atualizações de presença. Esse número está sendo ajustado, portanto, ele pode mudar em versões futuras.
