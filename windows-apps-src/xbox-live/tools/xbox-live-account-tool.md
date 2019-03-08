---
title: Ferramenta do Xbox Live conta
description: Saiba como usar a ferramenta de conta do Xbox Live para rapidamente criar contas de teste para testar seu título Xbox Live habilitado.
ms.assetid: ec5959f9-1c60-4aa4-94a6-5d8bdcf77a96
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox, teste, contas de teste
ms.localizationpriority: medium
ms.openlocfilehash: dead4e62e41b7b597ba9a578ee8f174386529937
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632821"
---
# <a name="xbox-live-account-tool"></a>Ferramenta do Xbox Live conta

## <a name="what-is-xbox-live-account-tool"></a>O que é a ferramenta de conta do Xbox Live?
A ferramenta de conta do Xbox Live é uma ferramenta projetada para ajudar os desenvolvedores de título a configurar contas de desenvolvimento existente para testar cenários de jogos. Por exemplo, você pode usar a ferramenta de conta do Xbox Live para alterar o nome de jogador de uma conta de desenvolvedor ou adicionar rapidamente os seguidores de 1000 a lista de amigos da conta.

## <a name="what-can-i-do-with-xbox-live-account-tool"></a>O que pode fazer com a ferramenta de conta do Xbox Live?
Você pode:
  1. Exibir configurações de perfil do usuário, XUID e privilégios do Active Directory
  2. Adicionar uma lista de seguidores ao grafo social do usuário, a partir de um arquivo de texto ou csv uma plataforma de desenvolvedor do Xbox
  3. Gerenciar a lista de amigos do usuário: favorito, dos Favoritos, bloquear e desbloquear usuários você execute e ver se elas seguem você volta
  4. Alterar reputação de desenvolvimento do usuário (e ver os valores de reputação brutos stat imediatamente)
  5. Alterar o nome de jogador do usuário

## <a name="where-can-i-find-xbox-live-account-tool"></a>Onde encontrar a ferramenta de conta do Xbox Live?
A ferramenta de conta do Xbox Live pode ser encontrada como parte do pacote do Xbox Live Tools [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools).

## <a name="how-do-i-log-in"></a>Como fazer logon?
Será necessário as credenciais do usuário que você deseja gerenciar e especifique a área de segurança correta. Certifique-se de que a conta de desenvolvimento tem acesso à área restrita, caso contrário, o logon poderá falhar. A ferramenta foi criada com as contas de desenvolvimento usando uma área restrita em mente.

## <a name="can-i-use-a-retail-account-or-does-it-have-to-be-a-sandboxed-account"></a>Posso usar uma conta de varejo ou ele tem que ser uma conta em área restrita?
Você certamente pode usar a ferramenta de conta do Xbox Live para gerenciar uma conta de varejo, mas nem todos os recursos têm suporte. Por exemplo, você não pode alterar a reputação do usuário de varejo.

## <a name="how-do-i-change-a-dev-users-gamertag"></a>Como alterar o gamertag do desenvolvimento do usuário?
Navegue até a guia "Gamertag" e insira um nome de jogador. Gamertags deve conter apenas números, letras e espaços e pode ter somente 15 caracteres. Desenvolvimento conta gamertags deve começar com um 2. Atualmente, há suporte para apenas uma alteração.

## <a name="how-do-i-see-my-block-list"></a>Como posso ver minha lista de bloqueios?
Navegue até a guia "Pessoas" e selecione o cabeçalho da coluna "Bloqueado" para classificar por usuários que estão atualmente bloqueados.

## <a name="how-do-i-follow-a-large-group-of-users"></a>Como posso seguir um grande grupo de usuários?
Se você tiver uma lista de XUIDs que você deseja seguir, copie-os em um arquivo de texto. Navegue até a guia "Follow", selecione "Lista de importação" e escolha o arquivo. Os XUIDs devem preencher na exibição de lista. Clique em "Confirmar alterações" para seguir os usuários.

## <a name="how-do-i-block-someone"></a>Como bloquear uma pessoa?
Navegue até a guia "Pessoas" e selecione o usuário ou usuários que você deseja bloquear. Pressione o botão "bloco" e ele vai ser bloqueados em lotes. Se você perceber um erro, simplesmente novamente mais tarde.

## <a name="how-do-i-change-my-dev-accounts-repuation"></a>Como altero reputação da minha conta de desenvolvimento?
Navegue até a guia "Reputação". Selecione os padrões que você deseja e pressione o botão "Confirmar alterações" para enviar a solicitação. Você verá o stat reputação valores alterados se a solicitação for bem-sucedida.

## <a name="what-do-the-values-in-the-reputation-tab-mean"></a>O que significam os valores na guia reputação?
Em geral reputação é computada de reputação de subpropriedades três: Fairplay (realizar com vários participantes), conteúdo gerado pelo usuário (clipes de vídeo e assim por diante) e as comunicações (mensagens e voz). Os valores brutos de cada categoria podem variar de 0 para 75, onde mais quer dizer que reputação do usuário é melhor. O OverallStatIsBad informa se o usuário tem reputação "Evitar Me".

## <a name="whats-the-black-area-at-the-bottom"></a>O que é a área preta na parte inferior?
Para manter você informado, achamos que seria útil se a saída de depuração é impresso na interface do usuário. Área lhe dizer o que a ferramenta está acontecendo e quaisquer erros de impressão é executado em.

## <a name="wheres-my-gamerpic"></a>Onde está meu gamerpic?
Estamos cientes de um bug que algumas contas de desenvolvimento não obtêm um gamerpic gerado automaticamente no momento da criação de conta.

## <a name="why-are-things-happening-so-slowly"></a>Por que as coisas que acontecem tão lento?
Para as operações de lote (como adicionar seguidores), a ferramenta executa automaticamente lotes para impedir que os tamanhos de solicitação enorme.

## <a name="how-do-i-run-xbox-live-account-tool"></a>Como posso executar a ferramenta de conta do Xbox Live?
Extraia o Xbox Live SDK para uma pasta e duas vezes no arquivo Tools\XboxLiveAccountTool.exe para executá-lo.

## <a name="i-have-a-feature-request-how-do-i-get-my-feature-incorporated"></a>Eu tenho uma solicitação de recurso! Como obter o meu recurso incorporado?
Converse com seu representante da Microsoft com quaisquer solicitações de recurso e, veremos o que podemos fazer.
