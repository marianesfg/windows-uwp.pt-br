---
title: Memória e desempenho do Gerenciador de Redes Sociais
description: Descreve considerações de desempenho e memória ao usar o Gerenciador de API do Gerenciador social Xbox Live.
ms.assetid: 2540145e-b8e2-4ab5-9390-65e2c9b19792
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, Gerenciador de redes sociais, pessoas
ms.localizationpriority: medium
ms.openlocfilehash: 7c6a0a31c8fe82faa644a59147060f9323d42eb0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618451"
---
# <a name="social-manager-memory-and-performance-overview"></a>Memória de Gerenciador social e visão geral do desempenho

## <a name="memory"></a>Memória
Social Manager agora permite que ele alocou memória persistente a serem mantidos no espaço de título. Um alocador de memória personalizado para uso pelo Gerenciador de redes sociais pode ser especificado ao chamar `xbox_live_services_settings::set_memory_allocation_hooks()`. Será esse gancho de alocador de memória também pode ser usado para alocações de memória grande futuras que usa o Xbox Live API (XSAPI).

O pior caso para alocação de memória pelo Gerenciador de redes sociais por padrão deve ser aproximadamente 4,75 mb (1). Não há mais sobrecarga que o Gerenciador de social poderá alocar com base nas atualizações RTA e HTTP. Se a criação de um `xbox_social_user_group` na lista, cada adicionado membro ocupará kb 2.43 adicionais. Se um usuário é adicionado por meio de `create_social_social_user_group_from_list`, `update_social_user_group`, ou o usuário adicionar um amigo fora o título, isso pode causar um realloc localizar o espaço de memória contígua.

(1) mb o 4,75 proveniente = 1000 `xbox_social_user` 2.43 KB * 2. O 2 é que o Gerenciador de redes sociais mantém um buffer de memória duplo.

## <a name="additional-user-limits"></a>Limites de usuários adicionais
Atualmente, o Gerenciador de social tem uma restrição de 100 usuários adicionados ao gráfico. Isso ocorre devido a dois problemas:

1. O número máximo de assinaturas de RTA que um usuário pode ter é 1100. Se um usuário local tiver 1000 amigos, que deixa apenas 100 para assinaturas adicionais.
2. A quantidade máxima de tamanho de lote de usuários que podem ser enviados para PeopleHub está atualmente em torno de 100.

Social Manager se comunica a isso, não permitindo que um grupo de usuários social de lista conter mais de 100 usuários. Há um limite global de 100 usuários adicionais total que pode ser no sistema a qualquer momento por meio de `create_social_user_group_from_list`.

## <a name="processing-time"></a>Tempo Médio de Processamento
Gerenciador de social terá na pior das hipóteses caso 1100 usuários. As características de desempenho do Gerenciador de redes sociais no Xbox One tem um pior caso de 0,3 ms a ms 0,5 para `do_work` dependendo do número de gráficos sociais criado. O caso comum é 0,01 ms para nenhum MS criado e até 0,2 de grupos para um grupo com 1.000 usuários nele.
