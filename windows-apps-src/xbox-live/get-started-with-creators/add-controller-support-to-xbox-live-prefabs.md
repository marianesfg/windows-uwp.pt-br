---
title: Adicionar suporte de controlador para pré-fabricados do Xbox Live
description: Adicionar suporte de controlador para o Xbox Live pré-fabricados usando o plug-in Xbox Live Unity
ms.assetid: ''
ms.date: 07/14/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, unity, suporte ao controlador
ms.localizationpriority: medium
ms.openlocfilehash: 4d32ec62b8beec10256ed9a695866c2fd9bdd03e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622871"
---
# <a name="add-controller-support-to-xbox-live-prefabs"></a>Adicionar suporte de controlador para pré-fabricados Xbox Live

> [!IMPORTANT]
> O plug-in do Xbox Live Unity não oferece suporte realizações ou multiplayer online e é recomendado somente para [programa de criadores do Xbox Live](../developer-program-overview.md) membros.

Todos o Xbox Live Unity plug-in de pré-fabricados oferecem suporte a entrada do controlador especificando no Inspetor de.

Por exemplo, digamos que você tenha um objeto do jogo chamado `UserProfile1` que se baseia o `UserProfile` pré-fabricado. Se você gostaria de associar a este objeto do jogo para o jogador 1 e tê-los a entrar com o `A` botão no seu controlador Xbox, simplesmente escrever `joystick 1 button 0` no `Input Controller Button` campo no Inspetor de.

  ![Suporte de controlador no pré-fabricado UserProfile](../images/unity/controller-support-example.png)

## <a name="all-prefab-controller-input-fields"></a>Controlador pré-fabricado todos os campos de entrada
### <a name="userprofile-prefab"></a>UserProfile pré-fabricado
- **Botão do controlador de entrada:** Adiciona e conecta um usuário do Xbox Live.

### <a name="social-prefab"></a>Pré-fabricado social
- **Botão de controlador de filtro de alternância:** Alterna o filtro para mostrar os amigos 'Todos' ou 'Online' amigos.

### <a name="leaderboard-prefab"></a>Placar de líderes pré-fabricado
- **Primeiro botão de controlador:** Leva o player para a primeira página de entradas de placar de líderes.
- **Último botão controlador:** Leva o player para a última página do placar de líderes de entradas.
- **Botão próximo do controlador:** Leva o player para a próxima página de entradas de placar de líderes.
- **Botão Prev controlador:** Leva o player para a página anterior do placar de líderes de entradas.
- **Botão de controlador de atualização:** Atualiza a exibição de placar de líderes.


### <a name="game-save-ui-prefab"></a>Pré-fabricado da interface de usuário do jogo salvar
- **Gere novo botão do controlador:** Gera um novo inteiro salvar dados.
- **Salve o botão de controlador de dados:** Salva os dados atuais no armazenamento conectado.
- **Botão de controlador de dados de carga:** Carrega os dados salvos no armazenamento conectado.
- **Obter informações sobre o botão do controlador:** Recupera informações sobre contêineres salvos no armazenamento conectado.
- **Exclua contêiner controlador botão:** Exclui o contêiner salvo do armazenamento conectado

## <a name="xbox-controller-button-mappings"></a>Mapeamentos de botão de controlador do Xbox

Para os mapeamentos de botão de controlador Xbox no Unity, confira esta [página Wiki de controlador do Unity](https://wiki.unity3d.com/index.php?title=Xbox360Controller).
