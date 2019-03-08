---
title: Referência de objeto JSON (JavaScript Object Notation)
assetID: 8efcc6f3-d88a-1b15-bcfc-d79a24581b0a
permalink: en-us/docs/xboxlive/rest/atoc-xboxlivews-reference-json.html
description: " Referência de objeto JSON (JavaScript Object Notation)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c46557e3fb837bebccbb1039fb416f3e9787af2a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57626551"
---
# <a name="javascript-object-notation-json-object-reference"></a>Referência de objeto JSON (JavaScript Object Notation)
 
Notação JSON (JavaScript Object) é uma notação leve, baseada em padrões, orientada a objeto para encapsular os dados na web.
 
Serviços do Xbox Live define objetos JSON que são usados em solicitações e respostas do serviço. Esta seção fornece informações de referência sobre cada objeto JSON usado com serviços do Xbox Live.
 
<a id="ID4EHB"></a>

 
## <a name="in-this-section"></a>Nesta seção

[Conquista (JSON)](json-achievementv2.md)

&nbsp;&nbsp;Um objeto de medalha (versão 2).

[ActivityRecord (JSON)](json-activityrecord.md)

&nbsp;&nbsp;Uma cadeia de caracteres formatada e localizada sobre recursos avançados de presença de um ou mais usuários.

[ActivityRequest (JSON)](json-activityrequest.md)

&nbsp;&nbsp;Uma solicitação para obter informações sobre recursos avançados de presença de um ou mais usuários.

[AggregateSessionsResponse (JSON)](json-aggregatesessionsresponse.md)

&nbsp;&nbsp;Contém os dados agregados para sessões de adequação a um usuário.

[BatchRequest (JSON)](json-batchrequest.md)

&nbsp;&nbsp;Uma matriz de propriedades com o qual filtrar as informações de presença, como usuários, dispositivos e títulos.

[DeviceEndpoint (JSON)](json-deviceendpoint.md)

[DeviceRecord (JSON)](json-devicerecord.md)

&nbsp;&nbsp;Informações sobre um dispositivo, incluindo seu tipo e os títulos de Active Directory nele.

[Comentários (JSON)](json-feedback.md)

&nbsp;&nbsp;Contém informações de comentários sobre um player.

[GameClip (JSON)](json-gameclip.md)

[GameClipsServiceErrorResponse (JSON)](json-gameclipsserviceerrorresponse.md)

&nbsp;&nbsp;Uma parte opcional da resposta para o /clips/ de /scids/ {scid} /users/ {ownerId} {gameClipId} / uris/formato / {gameClipUriType} API.

[GameClipThumbnail (JSON)](json-gameclipthumbnail.md)

&nbsp;&nbsp;Contém as informações relacionadas em uma miniatura individual. Pode haver vários tamanhos por clip e é responsabilidade do cliente para selecionar o adequado para exibição.

[GameClipUri (JSON)](json-gameclipuri.md)

[GameMessage (JSON)](json-gamemessage.md)

&nbsp;&nbsp;Um objeto JSON definindo os dados para uma mensagem na fila de mensagens de uma sessão jogo.

[GameResult (JSON)](json-gameresult.md)

&nbsp;&nbsp;Um objeto JSON que representa dados que descreve os resultados de uma sessão de jogo.

[GameSession (JSON)](json-gamesession.md)

&nbsp;&nbsp;Um objeto JSON que representa dados de jogos para uma sessão com vários participantes.

[GameSessionSummary (JSON)](json-gamesessionsummary.md)

&nbsp;&nbsp;Um objeto JSON que representa dados de resumo para uma sessão de jogo.

[GetClipResponse (JSON)](json-getclipresponse.md)

&nbsp;&nbsp;Encapsula o clipe de jogo.

[HopperStatsResults (JSON)](json-hopperstatsresults.md)

&nbsp;&nbsp;Um objeto JSON que representa as estatísticas para um hopper.

[InitialUploadRequest (JSON)](json-initialuploadrequest.md)

&nbsp;&nbsp;O corpo de um clipe de jogo de POSTAGEM carregue a solicitação.

[InitialUploadResponse (JSON)](json-initialuploadresponse.md)

[inventoryItem (JSON)](json-inventoryitem.md)

&nbsp;&nbsp;O item de estoque core representa o item padrão na qual um direito pode ser concedido.

[LastSeenRecord (JSON)](json-lastseenrecord.md)

&nbsp;&nbsp;Informações sobre quando o sistema viu um usuário, disponível quando o usuário não tem nenhum DeviceRecord válido pela última vez.

[MatchTicket (JSON)](json-matchticket.md)

&nbsp;&nbsp;Um objeto JSON que representa um tíquete de correspondência, usado pelos players para localizar outros jogadores através do diretório de sessão para múltiplos jogadores (MPSD).

[MediaAsset (JSON)](json-mediaasset.md)

&nbsp;&nbsp;Os ativos de mídia associados com a realização ou suas recompensas.

[MediaRecord (JSON)](json-mediarecord.md)

[MediaRequest (JSON)](json-mediarequest.md)

[MultiplayerActivityDetails (JSON)](json-multiplayeractivitydetails.md)

&nbsp;&nbsp;Um objeto JSON que representa o **Microsoft.Xbox.Services.Multiplayer.MultiplayerActivityDetails**.

[MultiplayerSessionReference (JSON)](json-multiplayersessionreference.md)

&nbsp;&nbsp;Um objeto JSON que representa o **MultiplayerSessionReference**. 

[MultiplayerSessionRequest (JSON)](json-multiplayersessionrequest.md)

&nbsp;&nbsp;O objeto JSON de solicitação é passada para uma operação em um **MultiplayerSession** objeto.

[MultiplayerSession (JSON)](json-multiplayersession.md)

&nbsp;&nbsp;Um objeto JSON que representa o **MultiplayerSession**. 

[PagingInfo (JSON)](json-paginginfo.md)

&nbsp;&nbsp;Contém informações de paginação de resultados que são retornados em páginas de dados.

[PeopleList (JSON)](json-peoplelist.md)

&nbsp;&nbsp;Coleção de [pessoa](json-person.md) objetos.

[PermissionCheckBatchRequest (JSON)](json-permissioncheckbatchrequest.md)

&nbsp;&nbsp;Coleção de objetos PermissionCheckBatchRequest.

[PermissionCheckBatchResponse (JSON)](json-permissioncheckbatchresponse.md)

&nbsp;&nbsp;Verifique os resultados de uma permissão de lote para obter uma lista de valores de permissão para vários usuários.

[PermissionCheckBatchUserResponse (JSON)](json-permissioncheckbatchuserresponse.md)

&nbsp;&nbsp;Verifique os motivos de uma permissão de lote para a lista de valores de permissão para um usuário de destino único.

[PermissionCheckResponse (JSON)](json-permissioncheckresponse.md)

&nbsp;&nbsp;Os resultados de uma verificação de um único usuário para uma configuração de permissão única em relação a um usuário de destino único.

[PermissionCheckResult (JSON)](json-permissioncheckresult.md)

&nbsp;&nbsp;Os resultados de uma verificação de um único usuário para uma configuração de permissão única em relação a um usuário de destino único.

[Pessoa (JSON)](json-person.md)

&nbsp;&nbsp;Metadados sobre uma única pessoa no sistema de pessoas.

[PersonSummary (JSON)](json-personsummary.md)

&nbsp;&nbsp;Coleção de [Person (JSON)](json-person.md) objetos.

[Player (JSON)](json-player.md)

&nbsp;&nbsp;Contém dados para um player em uma sessão de jogo.

[PresenceRecord (JSON)](json-presencerecord.md)

&nbsp;&nbsp;Dados sobre a presença online de um único usuário.

[Profile (JSON)](json-profile.md)

&nbsp;&nbsp;As configurações de perfil pessoal para um usuário.

[Progressão (JSON)](json-progression.md)

&nbsp;&nbsp;Progressão do usuário para desbloquear a realização.

[Propriedade (JSON)](json-property.md)

&nbsp;&nbsp;Contém dados de propriedade fornecidos pelo cliente para critérios de relacionamento de pessoas da solicitação.

[QueryClipsResponse (JSON)](json-queryclipsresponse.md)

&nbsp;&nbsp;Encapsula a lista de clipes de jogos retornados juntamente com informações de paginação para a lista.

[quotaInfo (JSON)](json-quota.md)

&nbsp;&nbsp;Contém informações sobre um grupo de título de cota.

[Requisito (JSON)](json-requirement.md)

&nbsp;&nbsp;Os critérios de desbloqueio para a realização e quanto o usuário é para enfrentá-los.

[ResetReputation (JSON)](json-resetreputation.md)

&nbsp;&nbsp;Contém as novas pontuações de reputação base ao qual as pontuações de um usuário existente devem ser alteradas.

[Reward (JSON)](json-reward.md)

&nbsp;&nbsp;A recompensa associada com a realização.

[RichPresenceRequest (JSON)](json-richpresencerequest.md)

&nbsp;&nbsp;Solicitação para obter informações sobre quais informações de presença avançada devem ser usadas.

[ServiceError (JSON)](json-serviceerror.md)

&nbsp;&nbsp;Contém informações sobre o erro retornado quando uma chamada para o serviço falhou.

[ServiceErrorResponse (JSON)](json-serviceerrorresponse.md)

&nbsp;&nbsp;Quando um erro de serviço é encontrado, será retornado um código de erro HTTP apropriado. Opcionalmente, o serviço também pode incluir um objeto ServiceErrorResponse conforme definido abaixo. Em ambientes de produção, menos dados podem ser incluídos.

[SessionEntry (JSON)](json-sessionentry.md)

&nbsp;&nbsp;Contém dados para uma sessão de adequação.

[TitleAssociation (JSON)](json-titleassociation.md)

&nbsp;&nbsp;Um título que é associado com a realização.

[TitleBlob (JSON)](json-titleblob.md)

&nbsp;&nbsp;Contém informações sobre um título do armazenamento.

[TitleRecord (JSON)](json-titlerecord.md)

&nbsp;&nbsp;Informações sobre um título, incluindo seu nome e um carimbo de hora da última modificação.

[TitleRequest (JSON)](json-titlerequest.md)

&nbsp;&nbsp;Solicitação para obter informações sobre um título.

[UpdateMetadataRequest (JSON)](json-updatemetadatarequest.md)

&nbsp;&nbsp;Os metadados que devem ser atualizados para um clipe.

[Usuário (JSON)](json-user.md)

&nbsp;&nbsp;Contém dados de placar de líderes de usuário.

[UserClaims (JSON)](json-userclaims.md)

&nbsp;&nbsp;Retorna informações sobre o usuário autenticado atual.

[UserList (JSON)](json-userlist.md)

&nbsp;&nbsp;Uma coleção de [usuário](json-user.md) objetos.

[UserSettings (JSON)](json-usersettings.md)

&nbsp;&nbsp;Retorna as configurações para o usuário autenticado atual.

[UserTitle (JSON)](json-usertitlev2.md)

&nbsp;&nbsp;Contém dados do título do usuário.

[VerifyStringResult (JSON)](json-verifystringresult.md)

&nbsp;&nbsp;Correspondente a cada cadeia de caracteres enviada para códigos de resultado [/system/strings/validate](../uri/stringserver/uri-systemstringsvalidate.md).

[XuidList (JSON)](json-xuidlist.md)

&nbsp;&nbsp;Lista de XUIDs na qual executar uma operação.
 
<a id="ID4ENH"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EPH"></a>

 
##### <a name="parent"></a>Parent 

[Referência RESTful de serviços do Xbox Live](../atoc-xboxlivews-reference.md)

  
<a id="ID4EZH"></a>

 
##### <a name="external-links-ecma-international-standard-262-ecmascript-language-specificationhttpswwwecma-internationalorgpublicationsfilesecma-stecma-262pdf"></a>Links externos [ECMA International Standard 262: Especificação da linguagem ECMAScript](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-262.pdf)

   