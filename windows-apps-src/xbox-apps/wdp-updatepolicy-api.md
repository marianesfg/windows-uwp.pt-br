---
author: payzer
title: "Referência de API da política de atualização devkit do desenvolvedor Xbox do portal de dispositivos"
description: "Saiba como definir programaticamente a política de atualização para o console."
translationtype: Human Translation
ms.sourcegitcommit: 8f02e0c2f6fa30a3ac56945347c5bec253189bd8
ms.openlocfilehash: f9313d3c8b93ba13074c547f1f63c9f3204f0f58

---

OBSERVAÇÃO: Esta API estará disponível na próxima prévia do desenvolvedor.

# Referência de API da política de atualização do sistema   
Você pode usar essa API para ver qual política de atualização é aplicada ao console e alterar a política de atualização para uma nova.

IMPORTANTE: A maioria dos consoles receberá uma resposta "acesso negado" ao tentar chamar essa API. Isso porque nem todos os consoles de desenvolvimento têm a capacidade de alterar a política de atualização.

Esta API afeta a política de atualização para consoles em modo de desenvolvedor, e não para consoles de varejo.

## Obter a política de atualização do console

**Solicitação**

Você pode usar a solicitação a seguir para obter a política de atualização do console.

Método      | URI da Solicitação
:------     | :-----
GET | /ext/update/policy
<br />
**Parâmetros do URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**

- Nenhuma

**Resposta**   
A resposta é uma matriz JSON que contém membros do grupo de atualizações de sistema do console. Cada objeto tem os seguintes campos:   

Id – (cadeia de caracteres) A ID do grupo de atualização.   
Nome amigável– (cadeia de caracteres) O nome de exibição do grupo de atualização.   
Descrição – ("cadeia de caracteres") A descrição do grupo de atualização.
IsDevKitGroup – (true | false) indica se o grupo de atualização se destina a compilações de desenvolvedor.
ResourceSetID - (cadeia de caracteres) Ignore - usado pela infraestrutura de atualização do sistema.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | A solicitação foi bem-sucedida
4XX | Códigos de erro
5XX | Códigos de erro

## Definir a política de atualização do sistema do console
Você pode usar essa API para alternar a associação ao grupo de atualização de sistema do console.

Observação: Os consoles só podem estar em um grupo de atualização do sistema por vez.

**Solicitação**

Você pode usar a solicitação a seguir para definir a associação ao grupo de atualização do sistema de um conole.

Método      | URI da Solicitação
:------     | :-----
POST | /ext/update/policy
<br />
**Parâmetros do URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**   
O corpo da solicitação é um objeto JSON contendo os seguintes campos:   
GroupIdToJoin – (cadeia de caracteres) A ID do grupo de atualização do sistema do qual você deseja que o console participe.  
GroupIdToLeave – (cadeia de caracteres) A ID do grupo de atualização do sistema que você deseja que o console saia.

Todos os campos são necessários.

Os GroupIDs possíveis são:   
* Nenhuma atualização - "b2dbed33-2845-44cc-a7a1-4a9afb29d8d9"   
* Recuperação de produção mais recente - "7432ae99-8c09-48dd-99f9-a2697499e240"   
* Recuperação da visualização mais recente - "a8153054-1a1b-47cc-acc9-9aed90c1f8db"    

**Resposta**   

- Nenhuma

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | A solicitação foi bem-sucedida
4XX | Códigos de erro
5XX | Códigos de erro

<br />
**Famílias de dispositivos disponíveis**

* Windows Xbox




<!--HONumber=Aug16_HO3-->


