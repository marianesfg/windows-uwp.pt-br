---
title: /users/{requestorId}/permission/validate
assetID: 400a9721-bf43-76df-4cd1-9f2ae6ca5035
permalink: en-us/docs/xboxlive/rest/uri-privacyusersrequestoridpermissionvalidate.html
description: " /users/{requestorId}/permission/validate"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4a062fd417bae37fd66c944e0e534ef7a50de5fa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599361"
---
# <a name="usersrequestoridpermissionvalidate"></a>/users/{requestorId}/permission/validate
 
  * [Parâmetros de URI](#ID4EQ)
 
<a id="ID4EQ"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| requestorId| cadeia de caracteres| Obrigatório. Identificador do usuário que está executando a ação. Os valores possíveis são <code>xuid({xuid})</code> e <code>me</code>. Isso deve ser um usuário que fez logon. Valor de exemplo: <code>xuid(0987654321)</code>.| 
  
<a id="ID4ETB"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[OBTER (/users/ {requestorId} / permissão/validar)](uri-privacyusersrequestoridpermissionvalidateget.md)

&nbsp;&nbsp;Obtém uma resposta Sim ou não sobre se o usuário tem permissão para executar a ação especificada com um usuário de destino.

[POST (/users/ {requestorId} / permissão/validar)](uri-privacyusersrequestoridpermissionvalidatepost.md)

&nbsp;&nbsp;Obtém um conjunto de respostas de Sim ou não sobre se o usuário tem permissão para executar as ações especificadas com um conjunto de usuários de destino.
 
<a id="ID4EAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ECC"></a>

   [URIs de privacidade](atoc-reference-privacyv2.md)

 [Enumeração PermissionId](../../enums/privacy-enum-permissionid.md)

   