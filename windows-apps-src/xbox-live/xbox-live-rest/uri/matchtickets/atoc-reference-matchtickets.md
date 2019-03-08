---
title: URIs para organizar partida
assetID: 667b02a9-6f34-8165-001b-ee8782575202
permalink: en-us/docs/xboxlive/rest/atoc-reference-matchtickets.html
description: " URIs para organizar partida"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c52abca7ed49d4a5e14520095ae944938b86f093
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590461"
---
# <a name="matchmaking-uris"></a>URIs para organizar partida
 
Esta seção fornece detalhes sobre os endereços de identificador de recurso Universal (URI) e os métodos associados do protocolo HTTP (Hypertext Transport) de serviços do Xbox Live para o serviço de cruzamento. 
 
<a id="ID4E6"></a>

 
## <a name="domain"></a>Domínio
momatch.xboxlive.com  
<a id="ID4EEB"></a>

 
## <a name="service-version"></a>Versão do serviço
 
Os chamadores desses URIs HTTP/REST devem passar o valor 103 ou posterior para X-Xbl--versão do contrato, o cabeçalho HTTP que especifica a versão do serviço de serviços de descoberta de entretenimento (EDS). 
  
<a id="ID4ELB"></a>

 
## <a name="system-objects-and-properties"></a>Propriedades e objetos do sistema
 
No momento, todas as configurações do serviço para que haja correspondência ocorre manualmente, usando a parte de configuração de serviço a [Portal de desenvolvedor do Xbox (XDP)](https://xdp.xboxlive.com) ou [Partner Center](https://partner.microsoft.com/dashboard). Algumas informações de emparelhamento também são refletidas nos objetos definidos para o MPSD. 
 
Os principais objetos JSON usados para configurar o emparelhamento são definidos no [MatchTicket (JSON)](../../json/json-matchticket.md) e [HopperStatsResults (JSON)](../../json/json-hopperstatsresults.md). Observe que todos correspondem tíquetes deve definir um **ticketSessionRef** objeto para fornecer uma referência a uma sessão para múltiplos jogadores que contém o player ou jogadores que desejam ser correspondido com outras pessoas. 
  
<a id="ID4EBC"></a>

 
## <a name="in-this-section"></a>Nesta seção

[/serviceconfigs/{scid}/hoppers/{hoppername}](uri-serviceconfigsscidhoppershoppername.md)

&nbsp;&nbsp;Dá suporte a uma operação POST para criar tíquetes de correspondência. 

[/serviceconfigs/{scid}/hoppers/{name}/stats](uri-serviceconfigsscidhoppershoppernamestats.md)

&nbsp;&nbsp;Dá suporte a uma operação GET para recuperar as estatísticas para um hopper.

[/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}](uri-scidhoppernameticketid.md)

&nbsp;&nbsp;Dá suporte a uma operação de exclusão para um tíquete de correspondência.
 
<a id="ID4ENC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EPC"></a>

   [MatchTicket (JSON)](../../json/json-matchticket.md)

 [HopperStatsResults (JSON)](../../json/json-hopperstatsresults.md)

 [URIs do diretório de sessão](../sessiondirectory/atoc-reference-sessiondirectory.md)

  
<a id="ID4E2C"></a>

 
##### <a name="parent"></a>Parent 

[Referência do Universal Resource Identifier (URI)](../atoc-xboxlivews-reference-uris.md)

   