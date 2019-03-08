---
title: POST (/users/xuid({xuid})/deleteuserdata)
assetID: 8be13ff9-5d42-43a1-f2fa-d550d845a552
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddeleteuserdatapost.html
description: " POST (/users/xuid({xuid})/deleteuserdata)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dab43079dbba3729ff39f3a2116c377c3b73142a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604061"
---
# <a name="post-usersxuidxuiddeleteuserdata"></a>POST (/users/xuid({xuid})/deleteuserdata)
Redefine completamente os dados de reputação de um usuário de teste. Somente para teste.

  * [Comentários](#ID4EQ)
  * [Parâmetros de URI](#ID4E5)
  * [Autorização](#ID4EJB)
  * [Cabeçalhos de solicitação necessários](#ID4E3B)
  * [Códigos de status HTTP](#ID4EHC)
  * [Corpo da resposta](#ID4EJF)

<a id="ID4EQ"></a>


## <a name="remarks"></a>Comentários

Chamar essa API, removerá todos os itens de comentários e dados de reputação de um usuário. Parceiros podem chamar essa API em qualquer área restrita, exceto de varejo. A equipe de imposição pode chamar essa API com qualquer ID de área restrita.

O domínio para esses URIs é `reputation.xboxlive.com`. Esse URI é sempre chamado na porta 10443.

<a id="ID4E5"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- |
| xuid| inteiro sem sinal de 64 bits| Xbox usuário ID (XUID) do usuário cujos dados estão sendo excluídos.|

<a id="ID4EJB"></a>


## <a name="authorization"></a>Autorização

Para a área de restrita do varejo **PartnerClaim** da equipe de imposição.

Para todas as outras áreas restritas, **PartnerClaim** e **SandboxIdClaim**.

<a id="ID4E3B"></a>


## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários

**Content-Type: application/json** e **Xbl X-versão contrato** (versão atual é 101).

<a id="ID4EHC"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP

O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).

| Código| Frase de motivo| Descrição|
| --- | --- | --- | --- | --- | --- |
| 200| OK| A sessão foi recuperada com êxito.|
| 400| Solicitação Inválida| Serviço não conseguiu compreender a solicitação malformada. Normalmente, um parâmetro inválido.|
| 401| Não autorizado| A solicitação exige autenticação do usuário.|
| 404| Não encontrado| O recurso especificado não pôde ser encontrado.|
| 500| Erro Interno do Servidor| O servidor encontrou uma condição inesperada que o impediu de atender à solicitação.|
| 503| Serviço Indisponível| A solicitação foi restringida, tente a solicitação novamente após o valor de repetição do cliente em segundos (por exemplo, 5 segundos mais tarde).|

<a id="ID4EJF"></a>


## <a name="response-body"></a>Corpo da resposta

NONE em caso de êxito; Caso contrário, uma [ServiceError (JSON)](../../json/json-serviceerror.md) documento.

<a id="ID4EWF"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EYF"></a>


##### <a name="parent"></a>Parent

[/users/xuid({xuid})/deleteuserdata](uri-usersxuiddeleteuserdata.md)
