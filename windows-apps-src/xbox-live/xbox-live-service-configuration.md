---
title: Configuração do serviço Xbox Live
description: Saiba como configurar o serviço Xbox Live para o seu jogo.
ms.assetid: 631c415b-5366-4a84-ba4f-4f513b229c32
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, configuração de serviço
ms.localizationpriority: medium
ms.openlocfilehash: d12c66e61a189c13ddbcd96dd99caa351206ecf6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640901"
---
# <a name="xbox-live-service-configuration"></a>Configuração do serviço Xbox Live

## <a name="what-is-service-configuration"></a>O que é a configuração de serviço?

Talvez você esteja familiarizado com alguns dos recursos do Xbox Live, como [realizações](achievements-2017/achievements.md), [placares de líderes](leaderboards-and-stats-2017/leaderboards.md) e [cruzamento](multiplayer/multiplayer-concepts.md#smartmatch-matchmaking).

Caso você não estiver, explicaremos resumidamente placares de líderes como um exemplo. Placar de líderes permitem que os jogadores ver um valor que representa uma conquista, em comparação com outros jogadores.  Por exemplo pontuações em um jogo arcade, tempos de volta em um jogo de corrida ou headshots em uma primeira pessoa shooter. Mas ao contrário de uma máquina arcade que mostra apenas as maiores pontuações de jogadores que foram reproduzidos no computador físico, com o Xbox Live é possível exibir pontuações mais altas de todo o mundo.

Mas, para que isso aconteça, você precisa executar alguma configuração única para que o Xbox Live Saiba sobre o placar de líderes. Por exemplo, se os valores devem ser classificados em crescente ou decrescente de valor e qual parte dos dados ela deve ser de classificação.

Essa configuração ocorre [Partner Center](https://partner.microsoft.com/dashboard) maioria das vezes. Mas certos os desenvolvedores usarão [Portal de desenvolvedor do Xbox (XDP)](https://xdp.xboxlive.com).

Se você estiver desenvolvendo seu título como parte do programa de criadores do Xbox Live, você usa [Partner Center](https://partner.microsoft.com/dashboard), e você pode ler [obtendo iniciado com o Xbox Live](get-started-with-creators/get-started-with-xbox-live-creators.md) para saber como obter a configuração.

Se você for um ID@Xbox desenvolvedor ou trabalhar com um editor que é um Microsoft Partner, continue lendo.

## <a name="choose-your-development-portal"></a>Escolha seu portal de desenvolvimento

Conforme mencionado acima, há dois portais diferentes que podem ser usados para configurar serviços do Xbox Live. Centro de parceiro em [ https://partner.microsoft.com/dashboard ](https://partner.microsoft.com/dashboard) e o Portal de desenvolvimento do Xbox (XDP) em [ https://xdp.xboxlive.com ](https://xdp.xboxlive.com).

Partner Center é recomendado para todos os títulos no futuro, mas para determinados recursos, você talvez ainda queira usar XDP. Esta seção o ajudará a alertá-lo onde configurar seu título.

Você pode encontrar informações sobre páginas de configuração de serviço específico, dependendo de seu portal escolhido:

* [Configuração do Partner Center](configure-xbl/windows-dev-center.md)
* [A configuração do Portal de desenvolvimento do Xbox](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/atoc-service-configuration) - para acessar este link, você deve ter uma conta Microsoft (MSA) que foi habilitada para acesso completo ao Xbox Live.

Se você já tiver um título configurado, você pode rolar para baixo até [obter suas IDs de](#get_ids) para saber como obter os vários identificadores necessários para configurar o seu título.

### <a name="pcmobile-uwp-game-only"></a>Jogo de PC/móvel UWP apenas
Partner Center é recomendado para configurar e gerenciar os jogos UWP que são executados somente em computadores com Windows 10 e/ou dispositivos móveis Windows 10.

#### <a name="using-xdp-to-configure-uwp-titles"></a>Usando XDP para configurar os títulos UWP

Você talvez queira usar XDP para configurar os títulos UWP, se você tiver um dos seguintes requisitos:

1. Você está usando Arena.
2. Você tem configuração de permissões, grupos e usuários existentes no XDP que você deseja continuar a usar.
3. Você está usando ferramentas que funcionam somente em XDP, como a ferramenta de torneios ou o Visualizador de histórico de sessão do diretório de sessões com vários participantes.
4. Você está desenvolvendo um título que terá play de plataforma cruzada entre um jogo do Xbox XDK uma com base e a versão UWP PC/móvel do mesmo jogo.

Se você não se enquadram em uma dessas categorias, você deve usar o Partner Center. Caso contrário, você pode ver abaixo para saber como usar XDP para configurar um título UWP.

Usando XDP para configurar serviços do Xbox Live para aplicativos UWP tem algumas limitações importantes:

* **Depois que a configuração do serviço de um jogo Xbox Live é publicada para o certificado/varejo no XDP, não há nenhum voltando!** A configuração de serviço Xbox Live para que jogos precisa permanecer no XDP durante a vida útil do título do jogo.
* **Não há nenhum caminho de migração do XDP Partner Center.** Se você iniciar a configuração do Xbox Live no XDP, você deve recriá-lo manualmente no Partner Center se você deseja movê-lo.

Feitas essas considerações de dois, é recomendável usar Partner Center para jogos de PC/móvel, a menos que você se encaixam em uma das categorias de descritas acima.

### <a name="cross-play-between-xbox-one-and-pcmobile-games"></a>Cross-play entre os jogos do Xbox One e PC/móvel ###
Jogos de vários dispositivos entre o Xbox One e o PC, conhecido como cross-play, é uma experiência de showcase Windows 10. Nesse cenário, o Xbox One e PC versões de um jogo compartilham a mesma configuração do Xbox Live.

Este cenário também aborda o caso em que você tem um jogo do Xbox XDK um existente e deseja adicionar suporte de cross-play para uma versão UWP do seu jogo.

Para implementar a cross-play, faça o seguinte:

* **Use XDP para configurar e publicar seu jogo XDK.** Partner Center não oferece suporte a jogos do Xbox XDK um neste momento.
* **Use uma única Xbox Live configuração de serviço que você criou no XDP para as versões de um jogo UWP e o XDK.** XDP agora tem novos recursos que permitem que um jogo compartilhar uma única configuração de serviço Xbox Live entre a versão do XDK e a versão UWP do jogo.
* **Use a Central de parceiro para criar e publicar seu jogo UWP.** No entanto, não use Partner Center para configurar os serviços do Xbox Live, pois seu jogo usará a configuração de serviço que você criou no XDP.
* **Não divida a configuração do serviço Xbox Live entre XDP e Partner Center.** XDP e Partner Center não são cientes uns dos outros, e uma configuração de serviço de uma fonte de publicação substitui a configuração publicada a partir de outra fonte. Isso pode fazer com que os dados de usuário sejam perdidas (conquistas ausentes, salvamento apagado, etc.) que pode criar uma experiência de usuário ruim. Por esse motivo, **Exigimos que 100% da configuração de serviço Xbox Live é feita em XDP cross-play XDK + UWP jogos.**

Há mais detalhes sobre esse processo, incluindo os itens que são *não* Self-service na [guia de Introdução cruzada jogue](get-started-with-partner/get-started-with-cross-play-games.md) guia

### <a name="separate-versions-of-xbox-one-and-pcmobile-games-that-are-not-cross-play"></a>Versões separadas de jogos do Xbox One e PC/móvel que não estão entre-play
Você pode optar por manter o seu Xbox uma versão de um jogo separado da versão do PC/móvel do mesmo jogo. Nesse caso, crie dois produtos separados e siga as orientações para Xbox XDK um e o jogo UWP PC/móvel apenas, respectivamente.

Você não pode usar a mesma configuração de serviço para ambas as versões, nesse caso, e você deve criar manualmente a configuração de serviço para cada versão separada do seu jogo, tanto no XDP no Partner Center adequadamente.

<a name="get_ids"></a>

## <a name="get-your-ids"></a>Obter suas IDs

Para habilitar serviços do Xbox Live, você precisará obter várias IDs para configurar o seu kit de desenvolvimento e seu título. Eles podem ser obtidos ao fazer a configuração do serviço Xbox Live.

Se você atualmente não tem um título em XDP ou Partner Center, consulte a seção acima [portais de configuração do serviço Xbox Live](#xbox_live_portals) para obter orientação.

### <a name="critical-ids"></a>IDs de críticas

Há três IDs que são essenciais para o desenvolvimento de aplicativos para o Xbox One e títulos: a ID de área restrita, a ID de título e o SCID.

Embora seja necessário ter uma identificação de área restrita para usar um kit de desenvolvimento, a ID do título e o SCID não são necessários para o desenvolvimento inicial, mas são necessários para qualquer uso dos serviços do Xbox Live. Portanto, recomendamos que você obtenha todos os três ao mesmo tempo.

#### <a name="sandbox-ids"></a>IDs de área restrita

A área restrita fornece isolamento de conteúdo para o kit de desenvolvimento durante o desenvolvimento, garantindo que você tenha um ambiente limpo para desenvolver e testar seu título. A ID de Sandbox identifica sua área restrita. Um console só pode acessar uma área de segurança a qualquer momento, embora uma área de segurança pode ser acessada por vários consoles.

IDs de área restrita diferenciam maiusculas de minúsculas.

**Partner Center**

Se você estiver configurando seu título no Partner Center, você obter sua ID de área restrita no "Xbox Live" raiz configuração página, conforme mostrado abaixo:

![](images/getting_started/devcenter_sandbox_id.png)

**XDP**

Se você estiver configurando seu título em XDP, você obtém sua ID de área restrita da página Visão geral do seu produto, conforme mostrado abaixo:

![](images/getting_started/xdp_sandbox_id.png)

#### <a name="service-configuration-id-scid"></a>ID de configuração de serviço (SCID)

Como parte do desenvolvimento, você criará um host de outros recursos online, conquistas e eventos. Eles fazem parte da sua configuração de serviço e exigem a SCID para acesso.

SCIDs diferenciam maiusculas de minúsculas.

**Partner Center**

Para recuperar seu SCID no Partner Center, navegue até a seção de serviços do Xbox Live e vá para *programa de instalação do Xbox Live* . Seu SCID é exibida na tabela mostrada abaixo:

![](images/getting_started/devcenter_scid.png)

**XDP**

Para recuperar seu SCID em XDP, navegue até a seção "Instalação do produto" sob o título e você verá a ID do título e SCID.

![](images/getting_started/xdp_scid.png)

#### <a name="title-id"></a>ID do título

A ID de título identifica exclusivamente seu título aos serviços do Xbox Live. Ele é usado em todo os serviços para permitir que os usuários acessem o conteúdo ao vivo do seu título, estatísticas do usuário, conquistas e assim por diante e para habilitar a funcionalidade para múltiplos jogadores em tempo real.

IDs de título podem ser diferencia maiusculas de minúsculas, dependendo de como e onde eles são usados.

**Partner Center**

A ID do título no Partner Center for encontrada na mesma tabela como o SCID na *programa de instalação do Xbox Live* página.

**XDP**

Sua ID de título em XDP é obtido do mesmo local como o SCID é.

<a name="xbox_live_portals"></a>
