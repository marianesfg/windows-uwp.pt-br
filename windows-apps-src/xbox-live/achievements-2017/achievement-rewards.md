---
title: Prêmios por conquistas
description: Descreve como você pode configurar uma realização para oferecer remunerações.
ms.assetid: b6fc5bdb-ba7b-4687-985e-894182f066da
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, realização, recompensas
ms.localizationpriority: medium
ms.openlocfilehash: 0fbbcb06a2aba927301cf982d09fdb55192ec84c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617691"
---
# <a name="achievement-rewards"></a>Prêmios por conquistas

O diagrama a seguir ilustra como um desenvolvedor pode gerenciar o ciclo de vida de um título. O novo sistema de medalha foi projetado para elevar o nosso mecânico familiarizado com muito mais flexibilidade — em como as conquistas sejam desbloqueadas, em como e quando eles são adicionados e em quais benefícios, elas fornecem aos usuários — que capacita o desenvolvedor para o título de execução como um serviço por Adicionar valor e manter o envolvimento do usuário ao longo do tempo.

##### <a name="figure-1---how-a-title-might-drive-user-behavior"></a>Figura 1.   Como um título pode conduzir o comportamento do usuário. #####
![rewarding_achievements](../images/omega/achievements_overview_01_drive_behavior.png)

### <a name="flexible-options-for-rewarding-achievement"></a>Opções flexíveis para recompensando medalha ###
Com o Xbox One, expandimos o sistema de medalha para dar suporte a opções mais flexíveis de recompensa. Jogos permanecem como uma recompensa valiosa que rastreia uma pontuação de jogos simples e comum para o usuário entre o ecossistema do Xbox Live, mas agora você — o desenvolvedor ou o editor — pode usar realizações como um mecanismo de entrega para um intervalo muito maior de remunerações, tanto de dentro de seu título e fora de seu título.

Uma realização pode ser configurada com vários prêmios, até uma recompensa de cada tipo de recompensa. Uma realização também pode ser configurada com nenhum recompensa explícita; Nesse caso, ícone da conquista atua como uma notificação visual para o jogador que adquiriu a realização.

Xbox Live suporta os seguintes tipos de remunerações:

* jogos
* Arte
* Recompensas no aplicativo

#### <a name="gamerscore"></a>jogos ####
Nós estamos comprometidos em preservar a integridade do valor de jogos que foi criado com nossos usuários Xbox Live. Há apenas um jogos por usuário! Qualquer jogos que concede a um usuário em plataformas existentes de Xbox Live, como Xbox 360, Xbox One ou Windows 10 contará em direção a um único jogos para o usuário.

Quando um usuário desbloqueia uma medalha de jogos, Xbox Live aumenta automaticamente jogos do usuário com o valor configurado.

Há restrições em que os títulos podem oferecer jogos como um prêmio em suas realizações. Consulte os documentos de política no https://developer.xboxlive.com/ as informações mais recentes.

#### <a name="art"></a>Arte ####
Você tem alguma arte conceito interessante que seus designers desenhou no início da fase de concepção do seu título? Você tem imagens bonitos e de alta resolução que podem decorar seu aplicativo de hub quando visitar players? Talvez o seu aplicativo dá suporte a várias aparências? Com a recompensa de arte, você poderá ligar iberação lindas experiências em seus títulos e além do que os jogadores devem obter.

Arte de alta resolução conceito, desenhos iniciais do design, especialmente criado ativos de arte e outros ativos de arte digital podem ser oferecidos como um prêmio aos usuários para desbloquear uma realização. Esses ativos podem ser exibidos no painel de um do Xbox experiência e podem ser exibidos nas experiências de complementar consultando o serviço conquistas para recuperar os metadados pertinentes.

#### <a name="in-app-rewards"></a>Recompensas no aplicativo ####
Estamos introduzindo recompensas no aplicativo para dar aos desenvolvedores muito mais flexibilidade e controle sobre as recompensas que oferece uma realização. Recompensas no aplicativo permitem que você usar conquistas para oferecer remunerações de jogos personalizadas diretamente para seus usuários sem necessariamente atualizar seu título. Você simplesmente configurar a recompensa de realização com um código, a ID ou a frase que reconheça seu título, e quando o usuário desbloqueia a realização, Xbox LIVE enviará esse código ao seu título, informando, assim, o título da recompensa para fornecer ao usuário.

A recompensa em si cabe a você, o desenvolvedor. Ideias de recompensa incluem:

* Moeda extra no jogo ou pontos
* Acesso a um caractere especial, armas ou mapa
* Um multiplicador de temporários experiência.

### <a name="configuring-in-app-rewards"></a>Configurando as recompensas no aplicativo ###
Configurar uma recompensa no aplicativo para uma realização é bastante simples. O proprietário de medalha deve fornecer um nome de recompensa, uma descrição de recompensa e um ícone de recompensa além de um valor de recompensa. Esse valor de recompensa é determinado pelo desenvolvedor e deve ser algo que ambos o jogo pode interpretar e manipular corretamente ou que o usuário pode inserir como parte de uma experiência de resgate de recompensa título específico.

Um exemplo de um valor de recompensa o jogo pode interpretar pode ser um número de 5 dígitos ou um especial de cadeia de caracteres que o jogo ou serviço do jogo sabe mapas a um determinado item do jogo. Os desenvolvedores podem querer aproveitar o serviço de armazenamento gerenciado título (TMS) para que seja fácil adicionar novos valores de recompensa ao longo do tempo que o jogo entenderá como ler.

Um exemplo de um valor de recompensa que o usuário deve enviar pode ser um código especial ou uma cadeia de caracteres que o usuário insere em uma experiência de resgate dentro do título, dentro de um aplicativo complementar, ou no site do desenvolvedor.

### <a name="redeeming-in-app-rewards"></a>Resgatar recompensas no aplicativo ###
Uma recompensa no aplicativo entra em vigor quando o usuário resgata a recompensa no jogo. Títulos devem estar cientes de que um usuário desbloqueou uma realização que é configurada com uma recompensa no aplicativo para que o título corretamente pode oferecer a recompensa ao usuário. Para fazer isso, os títulos devem fazer o seguinte:

1. Consulte o serviço realizações após a inicialização de título ou retomada do título da suspensão para ver quais realizações desbloqueadas têm recompensas no aplicativo e obter o código de recompensa para cada um. Isso sempre deve ser feito para garantir que você capture qualquer realizações que podem ter sido desbloqueadas, enquanto o título não estava em execução ou em outro console.  

    Para consultar, você pode usar os URIs realizações RESTful ou as APIs no Namespace de Microsoft.Xbox.Services.Achievements.

2. Registre-se para receber uma notificação quando uma das suas realizações é desbloqueada. Isso é opcional, embora provavelmente é desejável para a maioria dos títulos. Observe que títulos só receberão essa notificação se o título está realmente em execução quando ocorre o desbloqueio. Essa é outra razão por que a etapa anterior é importante.

   Para se registrar para uma notificação de medalha, use o método de AchievementNotifier.GetTitleIdFilteredSource. Este tópico inclui o código de exemplo que ilustra como registrar.
