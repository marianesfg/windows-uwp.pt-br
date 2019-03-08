---
title: Visão geral de bate-papo 2 jogo
description: Saiba como adicionar a comunicação de voz para seu jogo usando o Xbox Live jogo bate-papo 2, uma versão atualizada do jogo bate-papo.
ms.date: 10/20/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, bate-papo jogo, jogo bate-papo 2, comunicação de voz
ms.localizationpriority: medium
ms.openlocfilehash: 9f013f8b80cc7bca367c3ef5cd2c0d1da86cc98c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57615821"
---
# <a name="game-chat-2-overview"></a>Visão geral do jogo bate-papo 2

2 de bate-papo jogo permite que você facilmente adicionar comunicação de bate-papo de voz e texto ao seu aplicativo ao mesmo tempo respeitando as configurações de privacidade de seus jogadores e cumprir os requisitos do Xbox para um jogos do Xbox e aplicativos de Hub relacionados ao bate-papo com voz e texto. Para os jogadores que tem habilitado a conversão de fala em texto ou texto em fala por meio da facilidade de acesso - configurações de transcrição de bate-papo do jogo, 2 de bate-papo do jogo transparentemente executará as traduções para criar mensagens de texto que representa a entrada de áudio de fala e play de bate-papo sintetizados áudio de fala para mensagens enviadas do texto de bate-papo, respectivamente.

> [!NOTE]
> Se você estiver procurando por uma referência de API específica, você o encontrará no arquivo Xbox Live API Ajuda em HTML compilado (. chm) que pode ser baixado [aqui](https://aka.ms/xboxliveuwpdocs).

- **Relações de comunicação** -2 de bate-papo do jogo lhe dá controle refinado sobre como os jogadores podem se comunicar com uns dos outros. Em vez de especificar as equipes ou canais, 2 de bate-papo de jogo requer que relações explícitas entre cada par de usuários seja definida. Relações de comunicação de bate-papo 2 jogos suportam único e a comunicação bidirecional entre qualquer par de jogadores. Relações de comunicação de voz e texto podem ser definidas independentemente uns dos outros.

- **Acessibilidade** -2 de bate-papo do jogo dá suporte à conversão de fala em texto e texto em fala. Quando habilitada, 2 de bate-papo do jogo respeitará a preferência de "Transcrição de bate-papo do jogo" de seus jogadores e executará de forma transparente traduções para criar mensagens de texto que representa o áudio de entrada de fala e sintetizada de reproduzir o áudio de fala para texto de saída bate-papo de bate-papo mensagens, respectivamente.

- **Integração do Xbox Live** -2 de bate-papo do jogo usa serviços do Xbox Live para garante que cada jogador preferências e os privilégios são respeitados.

- **Detecção de atividade de voz** -2 de bate-papo do jogo realiza a detecção de atividade de voz para determinar quando os dados de áudio incluem atividades de voz.

- **O controle automático obter** -2 de bate-papo do jogo executa o controle automático obter para minimizar a variação na saída do microfone do usuário.

- **Codecs** -2 de bate-papo do jogo codifica os dados de áudio que devem ser entregues em instâncias remotas do aplicativo. No Xbox One, essa codificação (e decodificação na extremidade receptora) é aceleradas por hardware.

- `chat_manager::start/finish_processing_state_changes` -O par de métodos chamados pelo aplicativo de cada quadro de interface do usuário para realizar operações assíncronas, para recuperar resultados a serem manipulados na forma de `game_chat_state_change` estruturas e, em seguida, para liberar os recursos associados quando terminar.

- `chat_manager::start/finish_processing_data_frames` -O par de métodos usados para conecte 2 bate-papo do jogo na camada de transporte do aplicativo. Esses métodos são chamados pelo aplicativo de cada quadro de rede para recuperar e distribuir `game_chat_data_frame` objetos para instâncias do aplicativo em dispositivos remotos e, em seguida, para liberar os recursos associados quando terminar.

- `chat_manager::process_incoming_data` -O método usado para fornecer dados a 2 bate-papo de jogo que foi entregue pela camada de transporte do aplicativo de uma instância remota de 2 de bate-papo do jogo.

- **Manipulação de áudio em tempo real** -2 de bate-papo do jogo permite que o aplicativo para se inserir no pipeline de áudio de bate-papo, para que ele pode inspecionar ou manipular dados de áudio de bate-papo. Ver [manipulação de áudio em tempo real](real-time-audio-manipulation.md) para obter mais informações.

O aplicativo informa à biblioteca de usuários no dispositivo local e os usuários em dispositivos remotos que são esperados como bate-papo juntos. O aplicativo, em seguida, configura as relações entre cada usuário.

Uma vez configurado, 2 de bate-papo de jogo de sonda de áudio microphone(s) os usuários locais, realiza a detecção de atividade de voz e controle automático de ganho, codifica os dados e expõe os dados de áudio a serem enviadas para instâncias remotas do jogo de bate-papo 2 por meio de `chat_manager::start/finish_processing_data_frames`. O aplicativo deve fornecer os dados contidos no `game_chat_state_change` objeto para as instâncias remotas do jogo 2 de bate-papo especificado está no mesmo objeto. Ao receber os dados, instâncias remotas do aplicativo devem enviar os dados para sua própria instância de 2 de bate-papo do jogo por meio de `chat_manager::process_incoming_data`. 2 de bate-papo jogo decodifica os dados de entrada e, em seguida, renderiza-o para o dispositivo de áudio do usuário local.

2 de bate-papo jogo impõe requisitos de privilégio e privacidade Xbox Live. Dados de áudio não serão gerados por usuários que não têm privilégios de comunicações e dados de áudio não serão renderizados de usuários que estão bloqueados por meio das configurações de privacidade.

Para começar, consulte [2 de bate-papo de jogo usando](using-game-chat-2.md). Se você estiver usando C#, consulte [usando o jogo de bate-papo 2 WinRT as projeções](using-game-chat-2-winrt.md). Se você já tiver uma implementação de bate-papo do jogo, use o [guia de migração de 2 de bate-papo do jogo](game-chat-2-migration.md) para mapear o jogo bate-papo conceitos e padrões de chamada para 2 de bate-papo do jogo semelhanças.

Para obter informações sobre a API de 2 de bate-papo do jogo, baixe a referência de API do Xbox: [XboxAPIs.zip](https://aka.ms/xboxliveuwpdocs)