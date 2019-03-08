---
title: Enumeração PermissionId
assetID: 3e7ee317-4946-1105-ecd7-1e26346deccb
permalink: en-us/docs/xboxlive/rest/privacy-enum-permissionid.html
description: " Enumeração PermissionId"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: aff463e2268af972536984a00e29348bf0a6859e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594681"
---
# <a name="permissionid-enumeration"></a>Enumeração PermissionId
Fornece detalhes sobre a enumeração PermissionId.
IDs de permissão podem ser usadas com as URLs de validação de permissão:

   * [OBTER (/users/ {requestorId} / permissão/validar)](../uri/privacy/uri-privacyusersrequestoridpermissionvalidateget.md)
   * [POST (/users/ {requestorId} / permissão/validar)](../uri/privacy/uri-privacyusersrequestoridpermissionvalidatepost.md)

Essas IDs incluem verificações diretas em relação a uma determinada configuração para um usuário, como a verificação de uma configuração de privacidade única de um destino ou um ator de privilégio único. Além disso, há permissão IDs que podem ser usados com a permissão de API e incorporar as verificações em relação a várias configurações para ações específicas do usuário.

<a id="ID4EIB"></a>


## <a name="permissions"></a>Permissões

Esses são valores que um chamador pode usar para verificar se uma ação específica pode ser executada. Ao contrário das configurações acima, eles encapsulam as políticas definidas pelo serviço e não podem ser alterados diretamente por usuários, embora na maioria dos casos, as políticas de compilação na parte superior de uma ou mais configurações cujos valores são definidos pelos usuários. Esses são normalmente compostas verificações em relação a mais de uma configuração definida acima. Exemplo: O <b>ViewProfile</b> permissão faz uma verificação de que o destino <b>ShareProfile</b> configuração de privacidade e o solicitante <b>AllowProfileViewing</b> privilégio.

Em geral, é recomendável que os chamadores solicitar uma ID de permissão para ações que precisam ser verificados, em vez de verificar diretamente as configurações de privacidade e privilégios. Isso permite que as políticas de privacidade a ser alterado consistentemente entre o serviço, como novas verificações são incorporadas.

| Nome da permissão| Descrição|
| --- | --- |
| CommunicateUsingText| Verifique se o usuário pode enviar uma mensagem com conteúdo de texto para o usuário de destino|
| CommunicateUsingVideo| Verifique se o usuário pode se comunicar usando o vídeo com o usuário de destino|
| CommunicateUsingVoice| Verifique se o usuário pode se comunicar usando a voz com o usuário de destino|
| ViewTargetProfile| Verifique se o usuário poderá exibir o perfil do usuário de destino|
| ViewTargetGameHistory| Verifique se o usuário pode exibir o histórico de jogo do usuário de destino|
| ViewTargetVideoHistory| Verifique se o usuário poderá exibir o histórico de assistindo vídeo detalhado do usuário de destino|
| ViewTargetMusicHistory| Verifique se o usuário pode exibir o histórico de escuta de música detalhadas do usuário de destino|
| ViewTargetExerciseInfo| Verifique se o usuário poderá exibir as informações de exercício do usuário de destino|
| ViewTargetPresence| Verifique se o usuário poderá exibir o status online do usuário de destino|
| ViewTargetVideoStatus| Verifique se o usuário pode exibir os detalhes do status de vídeo destinos (estendida presença online)|
| ViewTargetMusicStatus| Verifique se o usuário pode exibir os detalhes do status de música de destinos (estendida presença online)|
| ViewTargetUserCreatedContent| Verifique se o usuário poderá exibir o conteúdo criado pelo usuário de outros usuários|
