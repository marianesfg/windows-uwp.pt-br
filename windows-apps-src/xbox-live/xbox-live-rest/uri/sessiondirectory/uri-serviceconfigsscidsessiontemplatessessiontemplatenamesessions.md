---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions
assetID: 8d55818f-99fd-146a-896b-0f100e78799f
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessions.html
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 99d9a82cb419e6598fc1113692b031487950aa8b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656091"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamesessions"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions
Dá suporte a uma operação GET para recuperar um conjunto de modelos de sessão com os nomes de modelo especificado. 
<a id="ID4EO"></a>

 
## <a name="domain"></a>Domínio
sessiondirectory.xboxlive.com  
<a id="ID4ET"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| scid| GUID| Identificador de configuração de serviço (SCID). Parte 1 da sessão de ID.| 
| Palavra-chave| cadeia de caracteres| Uma palavra-chave usada para filtrar resultados para apenas sessões identificados com essa cadeia de caracteres.| 
| xuid| GUID| IDs de usuário do Xbox para os usuários para o qual recuperar as sessões. Os usuários devem estar ativos nas sessões. | 
| reservas| cadeia de caracteres| Valor que indica se a lista de sessões inclui aqueles que os usuários não foram aceitos. Esse parâmetro só pode ser definido como true. Essa configuração exige que o chamador tenha acesso de nível de servidor à sessão ou XUID do chamador de declaração para corresponder o filtro de ID de usuário do Xbox. | 
| inativo| cadeia de caracteres| Valor que indica se a lista de sessões inclui aquelas que os usuários que aceitaram, mas não estão em execução ativamente. Esse parâmetro só pode ser definido como true. | 
| privado| cadeia de caracteres| Valor que indica se a lista de sessões inclui sessões privadas. Esse parâmetro só pode ser definido como true. Ele é válido somente ao consultar suas próprias sessões ou ao consultar o servidor para servidor. Definir esse parâmetro como true requer que o chamador tenha acesso de nível de servidor à sessão ou XUID do chamador de declaração para corresponder o filtro de ID de usuário do Xbox. | 
| visibilidade| cadeia de caracteres| Um valor de enumeração que indica o status de visibilidade usado na filtragem de resultados. Atualmente, esse parâmetro só pode ser definido como aberto para incluir as sessões abertas. Ver <b>MultiplayerSessionVisibility</b>. | 
| version| cadeia de caracteres| Um inteiro positivo que indica a versão principal de sessão ou inferior das sessões para incluir. O valor deve ser menor ou igual à versão do contrato da solicitação de módulo 100. | 
| Take| cadeia de caracteres| Um inteiro positivo que indica o número máximo de sessões para recuperar.| 
  
<a id="ID4EZD"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions)](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionsget.md)

&nbsp;&nbsp;Recupera documentos do modelo de sessão.
 
<a id="ID4EDE"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EFE"></a>

 
##### <a name="parent"></a>Parent 

[URIs do diretório de sessão](atoc-reference-sessiondirectory.md)

   