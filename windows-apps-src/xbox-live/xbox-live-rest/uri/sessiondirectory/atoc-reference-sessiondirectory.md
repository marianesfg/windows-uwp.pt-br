---
title: URIs de diretório de sessão
assetID: e3ba951d-b21f-0014-c358-2603d549d118
permalink: en-us/docs/xboxlive/rest/atoc-reference-sessiondirectory.html
description: " URIs de diretório de sessão"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9492ff3272af830404a546c9b01d62178adbac96
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651191"
---
# <a name="session-directory-uris"></a>URIs de diretório de sessão

Esta seção fornece detalhes sobre os endereços de identificador de recurso Universal (URI) e os métodos associados do protocolo HTTP (Hypertext Transport) de serviços do Xbox Live para o diretório de sessão com vários participantes (MPSD).


> [!NOTE] 
> Apenas os títulos de jogos em execução em um Xbox 360, em um dispositivo Windows Phone ou em Xbox.com podem usar o URIs do diretório de sessões.  


  * [Domínio](#ID4EUB)
  * [Versão do serviço](#ID4EZB)
  * [Propriedades e objetos do sistema](#ID4EAC)
  * [Identificadores](#ID4EBE)

<a id="ID4EUB"></a>


## <a name="domain"></a>Domínio
sessiondirectory.xboxlive.com  
<a id="ID4EZB"></a>


## <a name="service-version"></a>Versão do serviço

Os chamadores desses URIs de REST devem passar o valor 104/105 ou posterior para X-Xbl--versão do contrato, o cabeçalho HTTP que especifica a versão do serviço de serviços de descoberta de entretenimento (EDS).

<a id="ID4EAC"></a>


## <a name="system-objects-and-properties"></a>Propriedades e objetos do sistema

Configuração de suas sessões e modelos, o MPSD usa um número de objetos JSON de sessão que está em conformidade com esquemas fixas que impõe o diretório e o interpreta. Durante as chamadas para os métodos compatíveis com o diretório de sessões vários URIs, esses objetos são validados e mesclados, com base em esquemas com suporte. Os principais objetos JSON associados à configuração com vários participantes são:

   *  [MultiplayerActivityDetails (JSON)](../../json/json-multiplayeractivitydetails.md)
   *  [MultiplayerSession (JSON)](../../json/json-multiplayersession.md)
   *  [MultiplayerSessionReference (JSON)](../../json/json-multiplayersessionreference.md)
   *  [MultiplayerSessionRequest (JSON)](../../json/json-multiplayersessionrequest.md)


Objetos JSON associados que lidam especificamente com jogos são:

   *  [GameMessage (JSON)](../../json/json-gamemessage.md)
   *  [GameResult (JSON)](../../json/json-gameresult.md)
   *  [GameSession (JSON)](../../json/json-gamesession.md)
   *  [GameSessionSummary (JSON)](../../json/json-gamesessionsummary.md)


<a id="ID4EBE"></a>


## <a name="handles"></a>Identificadores

Para apenas Multiplayer de 2015, as sessões podem ser acessadas por meio de identificadores de sessão. Vários URIs foram adicionadas para fornecer funcionalidade para dar suporte a identificadores.  
<a id="ID4EFE"></a>


## <a name="in-this-section"></a>Nesta seção

[/handles](uri-handles.md)

&nbsp;&nbsp;Dá suporte a uma operação POST para definir a sessão para a atividade do usuário atual a ser exibido no Xbox One experiência de usuário do painel e convidar membros da sessão, se necessário.

[/handles/{handleId}](uri-handleshandleid.md)

&nbsp;&nbsp;Dá suporte a operações DELETE e GET para identificadores de sessão especificadas pelo identificador.

[/handles/{handleId}/session](uri-handleshandleidsession.md)

&nbsp;&nbsp;Dá suporte a operações PUT e GET para uma sessão, usando o identificador de referência.

[/handles/query](uri-handlesquery.md)

&nbsp;&nbsp;Dá suporte a operações de POST para criar consultas para identificadores de sessão.

[/serviceconfigs/{scid}/batch](uri-serviceconfigsscidbatch.md)

&nbsp;&nbsp;Dá suporte a uma operação POST para uma consulta de lote no nível do identificador de configuração de serviço.

[/serviceconfigs/{scid}/sessions](uri-serviceconfigsscidsessions.md)

&nbsp;&nbsp;Dá suporte a uma operação GET para recuperar um conjunto de documentos de sessão.

[/serviceconfigs/{scid}/sessiontemplates](uri-serviceconfigsscidsessiontemplates.md)

&nbsp;&nbsp;Dá suporte a uma operação GET para recuperar um conjunto de modelos de sessão MPSD.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}](uri-serviceconfigsscidsessiontemplatessessiontemplatename.md)

&nbsp;&nbsp;Dá suporte a uma operação GET para recuperar um conjunto de nomes de modelo de sessão.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch](uri-serviceconfigscidsessiontemplatessessiontemplatenamebatch.md)

&nbsp;&nbsp;Dá suporte a uma operação POST para criar uma consulta em lotes no nível de modelo da sessão.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessions.md)

&nbsp;&nbsp;Dá suporte a uma operação GET para recuperar um conjunto de modelos de sessão com os nomes de modelo especificado.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.md)

&nbsp;&nbsp;Dá suporte a operações PUT e GET para criar e recuperar as sessões.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersindex.md)

&nbsp;&nbsp;Dá suporte a uma operação de exclusão para remover o membro de sessão especificada.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme.md)

&nbsp;&nbsp;Dá suporte a uma operação de exclusão para remover membros de sessão.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersservername.md)

&nbsp;&nbsp;Dá suporte a uma operação de exclusão para remover o servidor especificado de uma sessão.

<a id="ID4ESF"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EUF"></a>

   [URIs de cruzamento](../matchtickets/atoc-reference-matchtickets.md)


<a id="ID4E1F"></a>


##### <a name="parent"></a>Parent

[Referência do Universal Resource Identifier (URI)](../atoc-xboxlivews-reference-uris.md)
