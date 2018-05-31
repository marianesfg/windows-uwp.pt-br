---
title: Visão geral do Xbox Integrated Multiplayer
author: KevinAsgari
description: Saiba mais sobre o Xbox Integrated Multiplayer (XIM), uma solução de rede/multijogador/bate-papo tudo em um para jogos do Xbox Live.
ms.assetid: edbb28e6-1b6c-4f12-a9c6-fa8961de99a8
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5af9a5c386dc620c19183747750081ea1bd5a66d
ms.sourcegitcommit: db09dcb08da5995c46c2729896e56be3774ee5ba
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2018
ms.locfileid: "1563386"
---
# <a name="xbox-integrated-multiplayer-overview"></a>Visão geral do Xbox Integrated Multiplayer

 Xbox Integrated Multiplayer (XIM) é uma interface independente para adicionar facilmente comunicação de rede e bate-papo multijogador em tempo real ao seu jogo com a eficácia dos serviços Xbox Live. Ele é voltado para alguns conceitos principais:

 - **Rede do XIM** - Uma representação lógica de um conjunto de usuários interconectados que participam de uma experiência multijogador específica, além do estado básico que descreve essa coleção. Os participantes podem estar em apenas uma rede do XIM por vez, mas podem se mover diretamente de uma rede do XIM conceitual para outra.
 - `xim_player` - Um objeto que representa um usuário real conectado em um dispositivo local ou remoto e que participa de uma rede do XIM. Um único usuário físico que entra, sai e, em seguida, ingressa novamente na mesma rede do XIM é considerado duas instâncias separadas do player.
 - `xim_authority` - Um objeto que representa uma única entidade com a permissão e a responsabilidade de gerenciar uma visão consistente de estado específico do jogo em uma rede do XIM para todos os participantes. O uso deste objeto é opcional. Sua funcionalidade também não está disponível nesta versão do software.
 - `xim_state_change` - Uma estrutura que representa uma notificação ao dispositivo local em relação a uma alteração assíncrona em alguns aspectos da rede do XIM.
 - `xim::start/finish_processing_state_changes` - O par de métodos chamado pelo aplicativo em cada quadro de interface do usuário para executar operações assíncronas, para recuperar os resultados a serem tratados na forma de estruturas `xim_state_change` e, então, para liberar os recursos associados quando terminar.
 - **Associação** - O processo opcional de descobrir jogadores remotos adicionais com interesses ou níveis de habilidade semelhantes para participar de uma rede do XIM sem a necessidade de uma relação de redes sociais existente.

Basicamente, o aplicativo usa a biblioteca para configurar um conjunto de usuários no dispositivo local para serem movidos para uma rede do XIM como novos jogadores. Como as instâncias do aplicativo em dispositivos remotos fazem a mesma coisa com seus próprios usuários, e cada estado de processamento iniciar + concluir altera continuamente cada quadro de interface do usuário, cada instância participante recebe atualizações do `xim_state_change` que descrevem os `xim_player`s locais e remotos que participam dessa rede do XIM.

Dentro da rede do XIM, o aplicativo pode enviar suas próprias mensagens de dados específicos do jogo, como atualizações de movimento do avatar. Eles podem direcionar outros jogadores ou um `xim_authority` automaticamente selecionado para residir em um dos dispositivos participantes (com base em melhor qualidade de rede, estabilidade, reputação do jogador e outros fatores). Assim, o aplicativo nesse dispositivo `xim_authority` pode enviar mensagens de volta para jogadores, para fins de eficiência de rede e para decidir conflitos. Todas as mensagens recebidas são enviadas para o aplicativo como um `xim_state_change`, indicando a origem e os destinos locais pretendidos.

A comunicação de bate-papo por voz e texto também é fornecida automaticamente entre todos os jogadores onde as configurações de privacidade e de aplicativo permitirem. Para jogadores que tiverem habilitado o controle de voz para texto ou conversão de texto em fala, o XIM executará de forma transparente essa conversão para enviar mensagens de texto que representam o áudio de fala e reproduzir o áudio de fala sintetizada para mensagens de texto de saída, respectivamente.

Quando um jogador para de participar de uma rede do XIM (de forma espontânea ou devido a problemas de conectividade de rede), atualizações de `xim_state_change` são fornecidas para todas as instâncias do aplicativo indicando que `xim_player` saiu. Se o dispositivo selecionado para ser o `xim_authority` se afasta, as instâncias do aplicativo são informadas que a autoridade começou a mover e que devem iniciar a "reconciliação" com qualquer estado específico do jogo, fornecendo, opcionalmente, um buffer de dados para a substituição de `xim_authority` para fins de nova sincronização. A nova autoridade pode interpretar os dados de reconciliação de todos os jogadores conforme desejado e, por sua vez, fornecer um buffer de dados "reconciliados" para todos os jogadores para concluir o processo de movimentação de autoridade e de nova sincronização. O mesmo procedimento básico de reconciliação também é oferecido aos novos dispositivos participantes para que a autoridade possa convenientemente fornecer a eles o estado atual do jogo autoritativo quando seus jogadores ingressarem na rede do XIM, sem nenhum código extra. Observe que as alterações de estado de autoridade atualmente não são fornecidas nesta versão do software.

Um aplicativo pode determinar, de várias maneiras, a rede do XIM de que deseja participar. Muitas vezes o aplicativo é iniciado movendo automaticamente os usuários locais para uma nova rede disponível para os amigos dos usuários, onde os usuários locais podem enviar convites ou ter sua rede do XIM descoberta como uma atividade acessível (via cartões do player, por exemplo). Depois que esses usuários descobertos socialmente estiverem prontos, o aplicativo pode iniciar o processo de "associação" do Xbox Live e mover todos os jogadores para uma nova rede do XIM que também contém jogadores remotos "associados" adicionais para preencher as listas de equipe/adversário conforme desejado. Então, quando essa experiência multijogador é concluída, as instâncias do aplicativo podem mover seus jogadores locais e, opcionalmente, os jogadores de remotos associados previamente originais também, para uma nova rede do XIM privada, ou outra rede do XIM aleatória encontrada por meio de associação. Bate-papo de voz e texto permanecem disponíveis. Essa facilidade de mover jogadores de uma rede do XIM para outra é fundamental para a API e reflete as modernas expectativas de experiências de jogos bons e altamente sociais.

Para começar, consulte [Usando o XIM](xbox-integrated-multiplayer/using-xim.md).

## <a name="xims-relationship-to-other-modules"></a>Relacionamento do XIM com outros módulos

O XIM foi projetado para fornecer uma interface prática e tudo em um para jogos com necessidades básicas para multijogador. Ele engloba a funcionalidade de vários módulos, notavelmente o módulo `multiplayer_manager` da API de serviços do Xbox (XSAPI), a biblioteca `Microsoft::Xbox::GameChat` e a rede multijogador segura `Windows::Networking::XboxLive`, como uma única API simplificada. Isso reduz a quantidade típica de código, tarefas e conceitos envolvidos durante a criação de jogos multijogador que não exigem o máximo de flexibilidade absoluta ou um controle. No entanto, aplicativos cujos requisitos não se alinham com pressuposições simples do XIM podem usar esses componentes diretamente.

Embora o XIM destine-se a eliminar a necessidade de gerenciar coisas como documentos de sessão do Multiplayer Session Directory (MPSD) ou um transporte de rede por meio dos componentes subjacentes, ele não impede seu uso simultâneo/lado a lado como parte de uma malha de lista ou comunicação separadas do jogador. Neste caso, é responsabilidade do aplicativo garantir o uso de recursos de rede cooperativos entre o XIM e seus próprios mecanismos. O XIM atualmente oferece suporte a "reservas fora de banda" para facilitar seu uso como uma solução de bate-papo dedicada cuja lista de usuário é regida exclusivamente pela entrada externa.

O Xbox Live oferece muitos outros recursos valiosos para jogos multijogador, mas que são menos envolvidos, diretamente, na configuração da comunicação de rede e bate-papo com vários participantes e, portanto, não são encapsulados por esse módulo. Os aplicativos são incentivados a buscar na API de serviços do Xbox (XSAPI) feedbacks de jogador, conquistas, placares de líderes, armazenamento e muito mais.


## <a name="using-xim-as-a-dedicated-chat-solution-via-out-of-band-reservations"></a>Usando o XIM como uma solução de bate-papo dedicada através de reservas fora de banda

A maioria dos aplicativos usa o XIM para manipular todos os aspectos de reunião de jogadores. Afinal, um foco sobre a montagem de todos os recursos necessários para dar suporte a cenários completos de multijogador comuns é o motivo pelo qual que ele é chamado "Xbox Integrated Multiplayer". No entanto, alguns aplicativos que implementam suas próprias soluções de redes usando servidores de Internet dedicados também se interessariam pelas vantagens do estabelecimento de comunicação por bate-papo ponto a ponto confiável, de baixa latência e de baixo custo. O XIM reconhece essa necessidade e atualmente suporta um modo de extensão para usufruir da sua comunicação entre pares simplificada para aumentar o gerenciamento de jogadores externo de fora da API do XIM.

> Para obter a documentação detalhada sobre como usar o XIM através de reservas fora de banda, consulte [Reservas do XIM](xbox-integrated-multiplayer/xim-reservations.md).
