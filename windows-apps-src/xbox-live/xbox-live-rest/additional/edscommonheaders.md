---
title: Cabeçalhos comuns de EDS
assetID: 91297c9e-709d-7886-1da0-8a896c4953f4
permalink: en-us/docs/xboxlive/rest/edscommonheaders.html
description: " Cabeçalhos comuns de EDS"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 51a85383ee75fa51cb8376451ac955dcc4fa2edf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630701"
---
# <a name="eds-common-headers"></a>Cabeçalhos comuns de EDS

<a id="ID4EO"></a>



## <a name="entertainment-discovery-services-eds-common-request-headers"></a>Cabeçalhos de solicitação comuns (EDS) de serviços de descoberta de entretenimento

| Nome do cabeçalho| Descrição| Necessário?| Observações|
| --- | --- | --- | --- |
| <b>x-xbl-contract-version</b>| Versão do serviço de EDS| sim| 3.2|
| <b>x-xbl-client-type</b>| Cabeçalho de tipo de cliente| sim| Converse com a equipe para obter seu próprio tipo de cliente.|
| <b>x-xbl-client-version</b>| Versão do cliente| sim| Qualquer cadeia de caracteres não vazia.|
| <b>x-xbl-parent-ig</b>| Guid de impressão| sim| Usado para rastrear a solicitação nos logs e em outras chamadas de serviço.|
| <b>x-xbl-device-type</b>| Tipo de dispositivo| sim| Dispositivo que está representando o cliente.|
| <b>Aceitar</b>| Aceite o tipo| sim| XML ou JSON.|
| <b>Autorização</b>| Cabeçalho de autenticação| sim|  |
| <b>x-xbl-autoSuggest-seed-text</b>| Texto de semente de sugestão automática| não| Usado para BI e relevância|
| <b>x-xbl-search-term</b>| Termo de pesquisa| não|  |
| <b>x-xbl-método de entrada</b>| Método de entrada usado pelo usuário| não| Controlador, fala, Kinect.|
| <b>x-xbl-kinect-enabled</b>| Kinect habilitado| não| Sim/não.|
| <b>x-xbl-speech-session-id</b>| ID da sessão de fala| não| Se a sessão foi iniciada usando fala.|
| <b>x-xbl-client-id</b>| Id de cliente anônimo| não| Usado para relatórios de BI e a relevância.|
| <b>x-xbl-device-id</b>| ID do Dispositivo| não| Usado para relatórios de BI e a relevância.|
| <b>x-xbl-user-agent</b>| Agente do usuário cliente| não| Usado para o BI. "&lt;nome > /&lt;versão > (&lt;versão do sistema operacional >; &lt;plataforma >; &lt;recurso >; &lt;fabricar >; &lt;modelo >) ".|
| <b>x-xbl-parent-ig</b>| Guid anterior de impressão para chamadas de "Chained"| não (mas altamente recomendado)| É importante para relevância de BI. Por exemplo, IG de uma chamada procurar é pai IG para a seguinte chamada de detalhes.|
| <b>delid</b>| Identidade de delegado| não| Usado por serviços internos para trabalhar em nome do usuário.|

## <a name="common-response-headers"></a>Cabeçalhos de resposta comuns

| Nome do cabeçalho| Descrição| Necessário?| Observações|
| --- | --- | --- | --- | --- | --- | --- | --- |
| <b>Cache</b>| Cabeçalhos de cache| sim| Ver <a href="https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9"> http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9 </a>.|
| <b>x-xbl-errors</b>| Erros| não| Lista de erros de vários provedores de dados.|
| <b>x-xbl-traceid</b>| Id de rastreamento de logs| sim| Usado para rastrear os logs de solicitação específica.|
| <b>x-server-name</b>| Ofuscado nome do servidor que manipula a solicitação.| sim| Usado para a depuração.|

<a id="ID4EECAC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EGCAC"></a>


##### <a name="parent"></a>Parent  

[Referência adicional](atoc-xboxlivews-reference-additional.md)


<a id="ID4ESCAC"></a>


##### <a name="further-information"></a>Informações adicionais

[URIs de Marketplace](../uri/marketplace/atoc-reference-marketplace.md)
