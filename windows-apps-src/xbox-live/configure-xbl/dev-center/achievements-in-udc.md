---
title: Conquistas de 2017
description: Descreve como você pode configurar realizações no Partner Center para oferecer remunerações.
ms.assetid: ''
ms.date: 11/10/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, udc, Central de desenvolvedores universal
ms.openlocfilehash: 9ef2365ee560e273108c38570697d599adde4c0b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655571"
---
# <a name="configure-achievements-2017-in-partner-center"></a>Configurar realizações 2017 no Partner Center

> [!IMPORTANT]
> Realizações só são aplicáveis a ID@Xbox ou parceiros gerenciados. Não há suporte para os jogos de participar do programa de criadores do Xbox Live.

Você pode usar [Partner Center](https://partner.microsoft.com/dashboard) para configurar a [realizações 2017](../../achievements-2017/simplified-achievements.md) que estão associados com seu jogo. Adicione uma nova medalha fazendo o seguinte:

1. Navegue até a seção conquistas em seu título, localizado sob **Services** > **Xbox Live** > **realizações**.
2. Clique o **nova medalha** botão e preencha o formulário.  Depois de concluído, clique em **salvar**.

![Captura de tela para criar uma nova medalha no Partner Center](../../images/dev-center/achievement-table.png)

## <a name="description"></a>Descrição
A seção de descrição é onde você pode inserir os conceitos básicos da sua realização, como o nome e as descrições bloqueada/desbloqueado. Você pode adicionar suporte à localização até realizações, acessando o **strings localizadas** seção de configuração de serviço [Partner Center](https://partner.microsoft.com/dashboard).

![Captura de tela dos campos de descrição ao configurar uma medalha de novo no Partner Center](../../images/dev-center/achievements-2.png)

O **medalha nome** campo é o nome de voltado para o público de realização.

O **bloqueado descrição** campo é a descrição que os jogadores verão quando eles não desbloqueado a realização. Ele tem um limite de 100 caracteres.

O **desbloqueada descrição** campo é a descrição que os jogadores verão quando eles conquistou a realização. Ele tem um limite de 100 caracteres.

## <a name="details"></a>Detalhes
A seção de detalhes é usada para associar informações importantes, como a imagem, o tipo de realização, a recompensa de jogos (se houver) e se a realização deve ficar oculto até que desbloqueada.

![Captura de tela de campos de detalhes ao configurar uma medalha de novo no Partner Center](../../images/dev-center/achievements-3.png)

O **ícone de imagem** campo é a imagem que será exibida junto com a realização. Ele deve ser um arquivo png de 1920 x 1080.

**Realizações de base** estão disponíveis para os jogadores quando o jogo inicial foi liberado. **Realizações não Base** estão disponíveis depois que o jogo inicial lançou (por exemplo, com novos DLC de conteúdo).

O **jogos** campo é a quantidade de pontos de jogos que sua realização concederemos quando desbloqueado. Cada medalha pode recompensar entre 0 a 200 pontos.  

**Público** realizações são visíveis para todos os jogadores, independentemente se eles conquistou a realização ou não. **Segredo** realizações ficam ocultos até que eles foram desbloqueados.

**Link profundo de medalha** é uma maneira para que você possa obter um parâmetro de veja o que lhe permite vincular a um ponto no seu jogo em onde a realização pode ser acumulada. O link profundo é retornado na resposta da API de obter. A URL especificada deve conter `ms-xbl-{titleID}://` prefixo.

> [!TIP]
> Links profundos de medalha exigem o TitleId hexadecimal do seu jogo. Você pode encontrá-lo na [programa de instalação do Xbox Live](xbox-live-setup.md) tela no [Partner Center](https://developer.microsoft.com/dashboard).

## <a name="additional-rewards"></a>Recompensas adicionais
Em alguns casos, você talvez queira oferecer uma recompensa no jogo ou a arte final quando um player desbloqueie uma realização. Você pode definir as recompensas (se houver) que estão associados uma realização na **recompensas adicionais** seção. Uma realização pode conter dois recompensas adicionais - um de cada tipo de recompensa. Você pode ler mais sobre o [medalha de remunerações](../../achievements-2017/achievement-rewards.md) artigo.

Para criar uma novo recompensa, clique o **adicionar recompensa** botão na **recompensas adicionais** seção e preencha o formulário.

![Captura de tela da adição de remunerações para uma realização no Partner Center](../../images/dev-center/achievement-reward.png)

### <a name="reward-details"></a>Detalhes de recompensa
Preencha os detalhes de recompensa para associar um prêmio de novo. Depois de concluído, clique em **adicionar**.

![Captura de tela de configurar detalhes do prêmio para uma realização no Partner Center](../../images/dev-center/achievements-5.png)

Há dois tipos de remunerações de medalha que podem ser criados. Eles são:

1. O **arte** tipo pode ser usado se você quiser recompensar o jogador com coisas como arte de alta resolução conceito, desenhos iniciais do design, especialmente criados ativos de arte ou outro arte digital. Ativos de arte são exibidos no painel do Xbox One e podem também serão exibidos nas experiências de complementar ao consultar o serviço conquistas.
2. O **jogos** tipo pode ser usado se você quiser recompensar o jogador com recompensas de jogos personalizadas sem atualizar o título. Alguns cenários possíveis são moeda/pontos de extra no jogo ou acesso a caracteres especiais, armas ou mapas.

O **nome de exibição** campo é o nome da recompensa que os jogadores vão encontrar. Ele tem um limite de 57 caracteres.

O **descrição** campo é a descrição da recompensa que os jogadores verão e devem incluir todas as instruções de resgate. Ele tem um limite de 90 caracteres.

O **imagem** ou **arte** campo é a imagem associada a recompensa. Se o tipo é definido como arte, isso será a arte final que eles serão recompensados. Caso contrário, ele representará a recompensa de jogos que eles receberão. A imagem deve ser um arquivo png de 1920 x 1080.

O **valor no jogo** campo só estará visível se você selecionar o tipo de recompensa da **jogos**. Ele é usado para especificar o valor que é passado para código do jogo que pode ser usado para desbloquear a recompensa no jogo.
