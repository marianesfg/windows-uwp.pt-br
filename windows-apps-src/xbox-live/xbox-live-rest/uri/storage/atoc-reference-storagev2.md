---
title: URIs do Armazenamento de Títulos
assetID: 32bba1e4-0980-785e-c098-a96cd88a8e5f
permalink: en-us/docs/xboxlive/rest/atoc-reference-storagev2.html
description: " URIs do Armazenamento de Títulos"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e0296eff0937ea5075630db0e049c86e2ea2c8ce
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622671"
---
# <a name="title-storage-uris"></a>URIs do Armazenamento de Títulos
 
Esta seção fornece detalhes sobre os endereços de identificador de recurso Universal (URI) e os métodos de protocolo de transporte de hipertexto (HTTP) associados do Xbox Live Services pela *armazenamento do título*.
 
Jogos em execução em todas as plataformas podem usar esse serviço.
 
O domínio para esses URIs é titlestorage.xboxlive.com.
 
<a id="ID4EFB"></a>

 
## <a name="in-this-section"></a>Nesta seção

[/global/scids/{scid}](uri-globalscidsscid.md)

&nbsp;&nbsp;Recupera informações de cota para esse tipo de armazenamento.

[/global/scids/{scid}/data/{path}](uri-globalscidssciddatapath.md)

&nbsp;&nbsp;Lista informações de arquivo em um caminho especificado. 

[/global/scids/{scid}/data/{pathAndFileName},{type}](uri-globalscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Baixa um arquivo.

[/json/users/batch/scids/{scid}/data/{pathAndFileName},json](uri-jsonusersbatchscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Baixa vários arquivos de vários usuários com o mesmo nome de arquivo.

[/json/users/xuid({xuid})/scids/{scid}](uri-jsonusersxuidscidsscid.md)

&nbsp;&nbsp;Recupera informações de cota para esse tipo de armazenamento.

[/json/users/xuid({xuid})/scids/{scid}/data/{path}](uri-jsonusersxuidscidssciddatapath.md)

&nbsp;&nbsp;Lista informações de arquivo em um caminho especificado. 

[/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json](uri-jsonusersxuidscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Downloads, carrega ou exclui um arquivo.

[/sessions/{sessionId}/scids/{scid}](uri-sessionssessionidscidsscid.md)

&nbsp;&nbsp;Recupera informações de cota para esse tipo de armazenamento.

[/sessions/{sessionId}/scids/{scid}/data/{path}](uri-sessionssessionidscidssciddatapath.md)

&nbsp;&nbsp;Lista informações de arquivo em um caminho especificado. 

[/sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type}](uri-sessionssessionidscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Baixa um arquivo.

[/trustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}](uri-trustedplatformusersbatchscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Baixa vários arquivos de vários usuários com o mesmo nome de arquivo.

[/trustedplatform/users/xuid({xuid})/scids/{scid}](uri-trustedplatformusersxuidscidsscid.md)

&nbsp;&nbsp;Recupera informações de cota para esse tipo de armazenamento.

[/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{path}](uri-trustedplatformusersxuidscidssciddatapath.md)

&nbsp;&nbsp;Lista informações de arquivo em um caminho especificado. 

[/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}](uri-trustedplatformusersxuidscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Downloads, carrega ou exclui um arquivo.

[/untrustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}](uri-untrustedplatformusersbatchscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Baixa vários arquivos de vários usuários com o mesmo nome de arquivo.

[/untrustedplatform/users/xuid({xuid})/scids/{scid}](uri-untrustedplatformusersxuidscidsscid.md)

&nbsp;&nbsp;Recupera informações de cota para esse tipo de armazenamento.

[/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{path}](uri-untrustedplatformusersxuidscidssciddatapath.md)

&nbsp;&nbsp;Lista informações de arquivo em um caminho especificado. 

[/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}](uri-untrustedplatformusersxuidscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Downloads, carrega ou exclui um arquivo.
 
<a id="ID4E5C"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EAD"></a>

 
##### <a name="parent"></a>Parent 

[Referência do Universal Resource Identifier (URI)](../atoc-xboxlivews-reference-uris.md)

   