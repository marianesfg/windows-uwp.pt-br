---
title: URIs de imagem do jogador
assetID: 811ab696-c433-aa54-90d8-66614ad09901
permalink: en-us/docs/xboxlive/rest/atoc-reference-gamerpic.html
description: " URIs de imagem do jogador"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1a8ba79784ed73ae62e7fe8d65c626c3ebc6003a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618381"
---
# <a name="gamerpic-uris"></a>URIs de imagem do jogador
 
Esta seção fornece detalhes sobre os endereços de identificador de recurso Universal (URI) Gamerpic e os métodos de protocolo de transporte de hipertexto (HTTP) associados do Xbox Live Services pela *gamerpics*.
 
O domínio para esses URIs é `gamerpics.xboxlive.com`.
 
O Gamerpic Service foi projetado para dar aos usuários mais opções de personalização, concedendo a ela um título a capacidade de permitir que o usuário gerar um gamerpic de seu jogo caractere (um caractere de jogo nesse cenário se refere a um jogo protagonist; ele pode ser uma pessoa um carro, uma nave espacial ou qualquer outra entidade que o usuário controle no título).
 
O fluxo básico de geração de um título gamerpic é da seguinte maneira:
 
   * O título fornece ao usuário a capacidade de criar uma imagem de seus caracteres do jogo. 
     * Caso contrário, o título pode, em seguida, o usuário que não tiverem o privilégio apropriado da mensagem.
     * Se o usuário tem o privilégio, o usuário pode continuar a criar seu gamerpic de caractere.
  
   * O usuário cria a imagem e o título envia o arquivo. PNG 1080 x 1080 para o serviço de gamerpic.
   * O serviço armazena a imagem e define a imagem como gamerpic de novo do usuário.
   * Experiências de qualquer chamada para gamerpic do usuário obterá a imagem atualizada.
  
A capacidade de definir um título gamerpic é controlada por um privilégio somente a imposição (211). Se a imposição revoga o privilégio, o usuário será impedido de salvar gamerpic um título e o serviço retornará 403. Títulos devem chamar CheckPrivilege para verificar que os usuários têm permissão para compartilhar conteúdo (priv 211).
 
No momento, para usar esse serviço, seu título deve estar na lista de permissões. Para solicitar aprovação, envie um email `slsgamerpics@microsoft.com`.
 
<a id="ID4EGC"></a>

 
## <a name="in-this-section"></a>Nesta seção

[/users/me/gamerpic](uri-usersmegamerpic.md)

&nbsp;&nbsp;Acessa um gamerpic 1080 x 1080.
 
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>Parent 

[Referência do Universal Resource Identifier (URI)](../atoc-xboxlivews-reference-uris.md)

   