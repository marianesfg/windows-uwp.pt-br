---
title: Xbox integrado com vários participantes
description: Saiba mais sobre o Xbox Integrated Multiplayer (XIM), uma solução de rede/multijogador/bate-papo tudo em um para jogos do Xbox Live.
ms.assetid: edbb28e6-1b6c-4f12-a9c6-fa8961de99a8
ms.date: 01/25/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, com vários participantes integrado do xbox
localizationpriority: medium
ms.openlocfilehash: aa82b1042f710d81d83b98767802c7538fd53fba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641771"
---
# <a name="xbox-integrated-multiplayer-xim"></a>XIM (Xbox Integrated Multiplayer)

- [Visão geral](#overview)
- [Conceitos](#concepts)
- [Recursos](#features)
- [Relação com outros módulos](#relationship-to-other-modules)

## <a name="overview"></a>Visão geral

Xbox Integrated Multiplayer (XIM) é uma interface independente para adicionar facilmente comunicação de rede e bate-papo multijogador em tempo real ao seu jogo com a eficácia dos serviços Xbox Live. A interface XIM não exige um projeto para escolher entre a compilação com C + + c++ /CLI CX versus C++ tradicionais; ele pode ser usado com qualquer um. A implementação também não gera exceções como um meio de relatórios para que você pode consumi-lo facilmente em projetos sem exceções se preferencial de erros não fatais.

Para começar, consulte [Usando o XIM](xbox-integrated-multiplayer/using-xim.md). Se você estiver usando C#, consulte [XIM usando C# ](xbox-integrated-multiplayer/using-xim-cs.md).

## <a name="concepts"></a>Conceitos

XIM é orientado a alguns conceitos-chave:

- Rede XIM - uma representação lógica de um conjunto de usuários interconectados que participam de uma experiência para múltiplos jogadores em particular, bem como estado básico que descreve a coleção. Os participantes podem estar em apenas uma rede do XIM por vez, mas podem se mover diretamente de uma rede do XIM conceitual para outra.
- Para que haja correspondência – o processo opcional de descobrir jogadores remotos adicionais com interesses semelhantes ou níveis de habilidades para participar de uma rede XIM sem a necessidade de uma relação de redes sociais existente.
- Consultando - o processo opcional de descoberta XIM redes sem a necessidade de uma relação social existente entre os participantes.
- `xim_player` -Um objeto que representa um usuário individual humano entrou em um dispositivo local ou remoto e a participação em uma rede XIM. Um único usuário físico que entra, sai e, em seguida, ingressa novamente na mesma rede do XIM é considerado duas instâncias separadas do player.
- `xim_state_change` -Uma estrutura que representa uma notificação para o dispositivo local em relação uma alteração assíncrona em algum aspecto da rede XIM.
- `xim::start_processing_state_changes` e `xim::finish_processing_state_changes` -o par de métodos chamados pelo aplicativo de cada quadro de interface do usuário para realizar operações assíncronas, para recuperar resultados a serem manipulados na forma de `xim_state_change` estruturas e, em seguida, para liberar os recursos associados quando terminar.

Em um nível muito alto, o aplicativo de jogo usa a biblioteca XIM para configurar um conjunto de usuários conectado-in no dispositivo local a ser movido para uma rede XIM como players de novo. O aplicativo chama `xim::start_processing_state_changes` e `xim::finish_processing_state_changes` cada quadro de interface do usuário. Instâncias do aplicativo em dispositivos remotos adicionar seus usuários em uma rede XIM, todas as instâncias participantes é fornecida `xim_state_change` atualizações que descreve o local e remoto `xim_player`s ingressar na rede XIM. Quando um jogador para de participar de uma rede do XIM (de forma espontânea ou devido a problemas de conectividade de rede), atualizações de `xim_state_change` são fornecidas para todas as instâncias do aplicativo indicando que `xim_player` saiu.

Um aplicativo pode determinar, de várias maneiras, a rede do XIM de que deseja participar. Muitas vezes o aplicativo é iniciado movendo automaticamente os usuários locais para uma nova rede disponível para os amigos dos usuários, onde os usuários locais podem enviar convites ou ter sua rede do XIM descoberta como uma atividade acessível (via cartões do player, por exemplo). Depois que esses usuários descobertos socialmente estiverem prontos, o aplicativo pode iniciar o processo de "associação" do Xbox Live e mover todos os jogadores para uma nova rede do XIM que também contém jogadores remotos "associados" adicionais para preencher as listas de equipe/adversário conforme desejado. Então, quando essa experiência multijogador é concluída, as instâncias do aplicativo podem mover seus jogadores locais e, opcionalmente, os jogadores de remotos associados previamente originais também, para uma nova rede do XIM privada, ou outra rede do XIM aleatória encontrada por meio de associação. Bate-papo de voz e texto permanecem disponíveis. Essa facilidade de mover jogadores de uma rede do XIM para outra é fundamental para a API e reflete as modernas expectativas de experiências de jogos bons e altamente sociais.

Em vez de um modelo cliente-servidor, uma rede XIM é logicamente uma malha totalmente conectada de dispositivos de ponto a ponto. Conforme descrito na seção deste documento, qualquer player pode enviar diretamente para qualquer outra por meio da API. Todos os métodos que afetam o estado da rede XIM como um todo podem ser invocados por qualquer dispositivo participante.

XIM usa a resolução de conflitos de gravação-last-wins simples se o aplicativo caso contrário, não impede que mais de um participante modificando o mesmo estado da rede XIM efetivamente simultaneamente. Isso significa que XIM não impõe qualquer conceitos de função para "host" ou "servidor". XIM também não restringe suas próprias conceitos, como suporte para migração de funções definidas pelo aplicativo para outro participante, quando um jogador sai de uma rede XIM de aplicativos.

## <a name="features"></a>Recursos

- Fornece o jogo com a comunicação de bate-papo com voz e texto que observa e respeita as configurações de privacidade do player

    A comunicação de bate-papo por voz e texto também é fornecida automaticamente entre todos os jogadores onde as configurações de privacidade e de aplicativo permitirem. Para jogadores que tiverem habilitado o controle de voz para texto ou conversão de texto em fala, o XIM executará de forma transparente essa conversão para enviar mensagens de texto que representam o áudio de fala e reproduzir o áudio de fala sintetizada para mensagens de texto de saída, respectivamente.

- Permite que o jogo enviar suas próprias mensagens de dados específicos do jogo

    Dentro da rede do XIM, o aplicativo pode enviar suas próprias mensagens de dados específicos do jogo, como atualizações de movimento do avatar. Todas as mensagens recebidas são enviadas para o aplicativo como um `xim_state_change`, indicando a origem e os destinos locais pretendidos.

- Funções como uma solução de bate-papo dedicado por meio de reservas de out-of-band

    Para obter a documentação detalhada sobre como usar o XIM através de reservas fora de banda, consulte [Reservas do XIM](xbox-integrated-multiplayer/xim-reservations.md).

- Sem exceções e pode ser usado com qualquer um dos C + + c++ /CX ou C++ tradicionais

## <a name="relationship-to-other-modules"></a>Relação com outros módulos

O XIM foi projetado para fornecer uma interface prática e tudo em um para jogos com necessidades básicas para multijogador. Ele engloba a funcionalidade de vários módulos, notavelmente o módulo `multiplayer_manager` da API de serviços do Xbox (XSAPI), a biblioteca `xbox::services::game_chat_2` e a rede multijogador segura `Windows::Networking::XboxLive`, como uma única API simplificada. Isso reduz a quantidade típica de código, tarefas e conceitos envolvidos durante a criação de jogos multijogador que não exigem o máximo de flexibilidade absoluta ou um controle. No entanto, aplicativos cujos requisitos não se alinham com pressuposições simples do XIM podem usar esses componentes diretamente.

Embora o XIM destine-se a eliminar a necessidade de gerenciar coisas como documentos de sessão do Multiplayer Session Directory (MPSD) ou um transporte de rede por meio dos componentes subjacentes, ele não impede seu uso simultâneo/lado a lado como parte de uma malha de lista ou comunicação separadas do jogador. Neste caso, é responsabilidade do aplicativo garantir o uso de recursos de rede cooperativos entre o XIM e seus próprios mecanismos. O XIM atualmente oferece suporte a "reservas fora de banda" para facilitar seu uso como uma solução de bate-papo dedicada cuja lista de usuário é regida exclusivamente pela entrada externa.

O Xbox Live oferece muitos outros recursos valiosos para jogos multijogador, mas que são menos envolvidos, diretamente, na configuração da comunicação de rede e bate-papo com vários participantes e, portanto, não são encapsulados por esse módulo. Aplicativos são incentivados a ser procurado para a API de serviços do Xbox (XSAPI) realizações de player, placares de líderes, armazenamento e muito mais.
