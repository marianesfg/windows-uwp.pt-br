---
title: Códigos de status HTTP padrão
assetID: 7a19de56-7acd-ad2b-e8e6-53126991093b
permalink: en-us/docs/xboxlive/rest/httpstatuscodes.html
description: " Códigos de status HTTP padrão"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c77972e88dbf4950f716594ee61220d1734a7fef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635551"
---
# <a name="standard-http-status-codes"></a>Códigos de status HTTP padrão
 
O protocolo HTTP (Hypertext Transport) padrão descreve alguns dos códigos de status que são retornados pelo servidor em resposta a uma solicitação do cliente. Métodos de serviços do Xbox Live retornam códigos de status de conformidade de protocolo HTTP para descrever o status da solicitação.
 
Aqui está uma lista dos códigos de status retornados pelos serviços do Xbox Live e seus significados típicos.
 
<a id="ID4EAB"></a>

 
## <a name="xbox-live-services-status-codes"></a>Códigos de Status de serviços do Xbox Live
 
| Código| Frase de motivo| Descrição| 
| --- | --- | --- | 
| 200| OK| A solicitação foi bem-sucedida.| 
| 201| Criado| A entidade foi criada.| 
| 202| Aceito| A solicitação foi aceita, mas ainda não foi concluída.| 
| 204| Sem conteúdo| A solicitação foi concluída, mas não tem conteúdo retornar.| 
| 301| Movido permanentemente| O serviço foi movido para um URI diferente.| 
| 302| Encontrado| O recurso solicitado reside temporariamente em um URI diferente.| 
| 307| Redirecionamento temporário| O URI para esse recurso foi alterado temporariamente.| 
| 400| Solicitação Inválida| Serviço não conseguiu compreender a solicitação malformada. Normalmente, um parâmetro inválido.| 
| 401| Não autorizado| A solicitação exige autenticação do usuário.| 
| 403| Proibido| A solicitação não é permitida para o usuário ou serviço.| 
| 404| Não encontrado| O recurso especificado não pôde ser encontrado.| 
| 406| Não aceitável| Não há suporte para a versão do recurso.| 
| 408| Tempo Limite da Solicitação| A solicitação demorou muito para ser concluída.| 
| 409| Conflito| A solicitação não foi concluída devido a um conflito com o estado atual do recurso.| 
| 410| Passou| O recurso solicitado não está mais disponível.| 
| 412| Falha na pré-condição| O servidor não atende a uma das precondições que o solicitante colocou na solicitação.| 
| 416| Intervalo solicitado não satisfatório| O intervalo solicitado não está disponível.| 
| 500| Erro Interno do Servidor| O servidor encontrou uma condição inesperada que o impediu de atender à solicitação.| 
| 501| Não implementado| O servidor não oferece suporte à funcionalidade necessária para atender à solicitação.| 
| 503| Serviço Indisponível| A solicitação foi restringida, tente a solicitação novamente após o valor de repetição do cliente em segundos (por exemplo, 5 segundos mais tarde).| 
 

> [!NOTE] 
> Alguns recursos e métodos fornecem informações específicas sobre o significado dos códigos de status específico dentro do contexto desse recurso ou método. Para obter mais detalhes, consulte a documentação para os recursos ou os métodos que você está usando. 

  
<a id="ID4E3BAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4E5BAC"></a>

 
##### <a name="parent"></a>Parent  

[Referência adicional](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4EKCAC"></a>

 
##### <a name="reference--universal-resource-identifier-uri-referenceuriatoc-xboxlivews-reference-urismd"></a>Referência [Universal Resource Identifier (URI) referência](../uri/atoc-xboxlivews-reference-uris.md)

 [Referência adicional](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4EZCAC"></a>

 
##### <a name="external-links--w3org-http11-status-code-definitionshttpswwww3orgprotocolsrfc2616rfc2616-sec10htmlsec10"></a>Links externos [W3.org: Definições de código de Status HTTP/1.1](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10)

   