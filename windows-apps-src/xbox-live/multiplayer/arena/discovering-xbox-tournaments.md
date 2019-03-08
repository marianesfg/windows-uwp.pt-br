---
title: Descobrindo torneios Xbox
description: Saiba como criar a interface do usuário para a descoberta de torneio do jogo.
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, arena, torneio, experiência do usuário
ms.localizationpriority: medium
ms.openlocfilehash: 71a065d6d4da290f2ce3345c7da0026d4c116335
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632471"
---
# <a name="discovering-xbox-tournaments"></a>Descobrindo torneios Xbox

Os jogadores têm várias opções no painel Xbox ou o aplicativo do Xbox em um PC, para saber mais sobre como participar ou observar um torneio. Também há oportunidades para aumentar a descoberta de torneio de dentro de seu título.  

Um jogador pode encontrar torneios Arena habilitados por:

* Abrindo Hub de um jogo executado recentemente de torneios da página inicial do Xbox.
* Visitar um perfil de player para torneios reproduzida, execução ou registrado para.
* Respondendo a uma equipe procurando por grupo (LFG) publicar em um clube pertencem.
* Assistir a um fluxo de torneio no aplicativo Mixer, site ou o pivô Watch no Hub/Arena do jogo.
* Visitar um Hub de jogo no painel do Xbox.

## <a name="promoting-tournaments-in-game"></a>Promovendo torneios no jogo

As pessoas assistir torneios porque eles estão interessados no título. Jogos do Xbox tem uma oportunidade única para capturar os jogadores naquele momento crucial quando eles estão procurando novas formas de estender sua experiência de jogo.

> [!TIP]
> **Recomendação de experiência do usuário:** Promova torneios em momentos de tomada de decisão.
>
> Aumente o reconhecimento de torneios às vezes quando os jogadores precisam decidir se deseja manter a reprodução ou sair (por exemplo, a inicialização do jogo, menu principal, menu com vários participantes ou final de correspondência).

### <a name="1--use-a-dynamic-announcement-feature-example-message-of-the-day"></a>1.  Usar um recurso de comunicado dinâmico (exemplo: Mensagem do dia)

Títulos que criaram um recurso de comunicado em sua interface de usuário do jogo normalmente usam um sistema de gerenciamento de conteúdo que permite atualizações instantâneas. Isso é útil para enviar conteúdo novo para jogadores sem depender de uma atualização de conteúdo ou liberação de conteúdo que pode ser baixada. Por exemplo, Halo usa um recurso de "Mensagem do dia" para anúncios da comunidade de superfície e dicas. Promoção do torneio é um ajuste perfeito para esse cenário.

**Impacto do usuário**

* Jogadores são apresentados para uma nova oportunidade de jogos competitiva antes de iniciar uma sessão de jogo.
* Exposição é intermitente (no lançamento, combinado com outros anúncios).
* Para saber mais, jogadores serão direcionados para deixar o jogo e vá até a Arena Hub.
* O recurso não requer uma atualização de conteúdo para preencher.
* Relevância, além de medição de tempo da promoção permanece flexível.

###### <a name="ui-example-message-of-the-day"></a>Exemplo de interface do usuário: 'Message do dia'

![Mensagem de torneio do dia](../../images/arena/arena-ux-motd.png)

#### <a name="a-main-heading-ex-play-watch-learn"></a>A. Main título: Jogar, assistir e aprender  

Informa os jogadores sobre torneios todos ou um específico.

#### <a name="b-go-to-arena-hub"></a>B. Vá até a Arena Hub  

Link profundo para jogos Hub/torneios - recomendados ao promover torneios todos relacionados ao seu jogo.  
– ou –  
Link profundo ao jogo/torneio/detalhes do Hub - recomendados quando anunciar um torneio específico.

#### <a name="c-game-exit-confirmation"></a>C. Confirmação de saída de jogo

Elementos de tela A e B levam o usuário o jogo para o painel do Xbox. Defina essa expectativa na interface de usuário do seu título antes do jogador torna uma decisão.

###### <a name="ui-example-exit-game-confirmation"></a>Exemplo de interface do usuário: Sair da confirmação de jogo
![Confirmação de jogos do torneio sair](../../images/arena/arena-ux-exit-confirm.png)

### <a name="2--promote-tournaments-on-the-main-menu"></a>2.  Promover torneios no Menu principal

Neste exemplo, o anúncio é um anúncio interativo para um evento futuro. Ele fornece informações suficientes para convencer o jogador saber mais. Na **selecionar**, o jogador é levou a uma página de detalhes de torneios no Hub Arena, onde é possível gerenciar seu registro.

###### <a name="ui-example-a-tournament-ad-displayed-alongside-the-main-menu"></a>Exemplo de interface do usuário: Um anúncio de torneio exibido ao lado do menu principal

![](../../images/arena/arena-ux-promo.png)

> [!TIP]
> **Recomendação de experiência do usuário**  
> O anúncio deve incluir detalhes suficientes para ajudar os jogadores a decidir se desejam saber mais. Por exemplo, ***descrição***, ***popularidade***, e ***tempo***.

## <a name="browsing-tournaments-in-game"></a>Navegação torneios no jogo

As APIs de Arena fornecem dados suficientes para o título criar uma experiência de navegação dentro do jogo. Se você pretende criar um recurso de navegação, adicione uma **torneios** ponto de entrada na seção com vários participantes.

> [!TIP]
> **Recomendações de experiência do usuário**  
> Torneios presentes como um modo de jogo com vários participantes.  
> Como o Xbox Arena é um novo recurso, certifique-se de mantê-lo visível no nível da camada 1 ou camada 2.

###### <a name="ui-example-tournaments-listed-as-an-additional-mode-in-the-multiplayer-section"></a>Exemplo de interface do usuário: Torneios listados como um modo adicional na seção com vários participantes

![Modo de torneio](../../images/arena/arena-ux-tournament-mode.png)

### <a name="provide-a-comprehensive-list-of-tournaments"></a>Fornece uma lista abrangente de torneios

Capacite os jogadores para verificar rapidamente o status de torneios que atualmente estão registrados no e para procurar torneios entre sessões do jogo.

#### <a name="user-impact"></a>Impacto do usuário

* Minimiza precisa do jogador deixar o jogo para verificar o status de torneio no painel.
* Serve como uma ótima solução de backup se jogadores perderem notificações do Xbox Arena do sistema.
* Envolvem custos adicionais para o desenvolvedor de jogos gerenciar.
* Leva os jogadores a Arena interface do usuário para ingressar, registro, fazer check-in, formação de equipe e a propagação.

> [!NOTE]  
> As APIs da Arena não fornecer uma consulta para torneios gerados pelo usuário (club gerado).


> [!TIP]
> **Recomendação de experiência do usuário**  
> Crie uma interface do usuário que pode ser dimensionado e fornecem métodos para jogadores gerenciar uma grande lista de torneios.

### <a name="filters"></a>Filtros

Filtrar torneios por **todos os**, **Active** (que abrange tudo até o torneio termine), **cancelado**, e **concluído** estados, e torneios um jogador possui ou participarão de (por exemplo, meu torneios).

###### <a name="ui-example"></a>Exemplo de interface do usuário:

![Tela de filtros do torneio](../../images/arena/arena-ux-filters.png)

> [!NOTE]  
> É possível, adicionar filtros personalizados, fora a API suporta *, mas não recomendado*.

O título pode filtrar por outras propriedades, depois que ele faz a solicitação, mas ele não é possível especificá-los no parâmetro de consulta. Isso torna a filtragem por outras propriedades não confiáveis para esse tipo de interface do usuário. As APIs da Arena recuperar um número especificado de título de resultados de cada vez — por exemplo, o título pode especificar uma **maxItems** valor de 10 torneios. Isso ajuda a gerenciar o desempenho de seu título e minimiza o risco de sobrecarregar o usuário com uma lista grande. No entanto, todos os filtros personalizados fornecidos pelo seu jogo seriam aplicada somente aos 10 itens solicitados no momento. Isso pode resultar em um número inconsistente de resultados filtrados após cada chamada à API. Por exemplo, o primeiro conjunto de 10 torneios retornado pode conter 2 relacionados a um filtro personalizado, como 'Execução'. Mas, nos próximos 10 pode mostrar 9 tais itens e assim por diante. O único modo confiável para seu título preencher completamente a cada filtro personalizado é solicitar torneio cada Active Directory e, em seguida, filtre-los, que não é recomendável.

#### <a name="filter-recommendations"></a>Recomendações de filtro:

##### <a name="all"></a>ALL

Exibir tudo **Active**, **cancelado**, e **concluído** torneios, classificados pelos mais recentes.

Resultados:

* Estado do torneio
* Data de início do torneio
* Status do torneio — por exemplo, o registro aberto
* Link profundo torneio na página de detalhes no painel de controle

##### <a name="my-tournaments"></a>MEU TORNEIOS

Torneios de modo que um participante está atualmente registrado no.

Resultados:

* Torneios que o usuário conectado no momento está participando.
* Status do torneio
* Um método para inserir uma correspondência de torneio (para o status 'pronto match')
* Link profundo do torneio na página de detalhes no painel de controle

##### <a name="active"></a>ACTIVE DIRECTORY

Exibir todos os torneios que foram criados: futuro, aberto para o registro, check-in e reprodução.

Resultados:

* Torneios que estão abertos para se registrar para que o usuário atual não já ingressou
* Tempo restante até que o registro é fechado

##### <a name="completedcanceled"></a>CONCLUÍDO/CANCELADA

Exibir resultados recentes. Torneios que acabou de se encerrar têm duração ao longo do tempo, em vez de simplesmente desapareça depois que ela seja concluída.

Resultados:

* Link profundo do torneio na página de detalhes no painel de controle
* Status do torneio
* Data/hora de término

##### <a name="canceled"></a>CANCELADO

Torneios da exibição que foram cancelados.

Resultados:

* Link profundo do torneio na página de detalhes no painel de controle
* Data/hora de cancelamento

> [!div class="nextstepaction"]
> [Ingressar em um torneio usando a interface do usuário Arena](arena-ux-join-tournament.md)
