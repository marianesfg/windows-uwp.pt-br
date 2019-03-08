---
title: Configuração do serviço Xbox Live criadores
description: Saiba mais sobre a configuração do serviço Xbox Live para o programa de criadores.
ms.assetid: 22b8f893-abb3-426e-9840-f79de0753702
ms.date: 10/03/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 76d25a48caadb908e30e6e1897c19178e2b837e1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662231"
---
# <a name="xbox-live-service-configuration-for-the-creators-program"></a>Configuração do serviço do Xbox Live para o Programa de Criadores

## <a name="what-is-service-configuration"></a>O que é a configuração de serviço?

Talvez você esteja familiarizado com alguns dos recursos do Xbox Live, como [placares de líderes](../leaderboards-and-stats-2017/leaderboards.md) e [armazenamento conectado](../storage-platform/connected-storage/connected-storage-technical-overview.md).

Caso você não estiver, explicaremos resumidamente placares de líderes como um exemplo. Placar de líderes permitem que os jogadores ver um valor que representa uma conquista, em comparação com outros jogadores. Por exemplo pontuações em um jogo arcade, tempos de volta em um jogo de corrida ou headshots em uma primeira pessoa shooter. Mas ao contrário de uma máquina arcade que mostra apenas as maiores pontuações de jogadores que foram reproduzidos no computador físico, com o Xbox Live é possível exibir pontuações mais altas de todo o mundo.

Mas, para que isso aconteça, você precisa executar alguma configuração única para que o Xbox Live Saiba sobre o placar de líderes. Por exemplo, se os valores devem ser classificados em crescente ou decrescente de valor e qual parte dos dados ela deve ser de classificação.

Essa configuração ocorre em [Partner Center](https://partner.microsoft.com/dashboard) para o programa de criadores do Xbox Live e você pode ler [obtendo iniciado com o Xbox Live](get-started-with-xbox-live-creators.md) para saber como obter a configuração.

## <a name="get-your-ids"></a>Obter suas IDs

Para habilitar serviços do Xbox Live, você precisará obter várias IDs para configurar seu ambiente de desenvolvimento e o título. Eles podem ser obtidos ao atualizar a configuração do serviço Xbox Live.

Se você não tiver um título no momento no Partner Center, consulte [criar e testar um novo título criadores](create-and-test-a-new-creators-title.md) para obter orientação.

### <a name="critical-ids"></a>IDs de críticas

Há três IDs que são essenciais para o desenvolvimento de aplicativos para o Xbox One e títulos: a ID de área restrita, a ID de título e a configuração do serviço ID (SCID).

Embora seja necessário ter uma ID de área restrita para configurar seu ambiente de desenvolvimento, a ID do título e o SCID não são necessários para o desenvolvimento inicial, mas são necessários para qualquer uso dos serviços do Xbox Live. Portanto, recomendamos que você obtenha todos os três ao mesmo tempo. Você pode exibir todas as IDs no "Xbox Live" raiz configuração página, conforme mostrado abaixo:

![](../images/getting_started/devcenter_sandbox_id.png)

#### <a name="sandbox-ids"></a>IDs de área restrita

A área restrita fornece isolamento de conteúdo para o seu ambiente durante o desenvolvimento, garantindo que ele esteja limpo para desenvolver e testar seu título. A ID de Sandbox identifica sua área restrita. Um console só pode acessar uma área de segurança a qualquer momento, embora uma área de segurança pode ser acessada por vários consoles.

IDs de área restrita diferenciam maiusculas de minúsculas.

#### <a name="service-configuration-id-scid"></a>ID de configuração de serviço (SCID)

Como parte do desenvolvimento, você criará estatísticas, placares de líderes e diversos outros recursos online. Eles fazem parte da sua configuração de serviço e exigem a SCID para acesso. SCIDs diferenciam maiusculas de minúsculas.

#### <a name="title-id"></a>ID do título

A ID de título identifica exclusivamente seu título aos serviços do Xbox Live. Ele é usado em todo os serviços para permitir que os usuários acessem o conteúdo ao vivo do seu título, estatísticas do usuário, conquistas e assim por diante e para habilitar a funcionalidade para múltiplos jogadores em tempo real.

IDs de título podem ser diferencia maiusculas de minúsculas, dependendo de como e onde eles são usados.

## <a name="publish-your-xbox-live-service-configuration"></a>Publicar a configuração do serviço Xbox Live

Quando você fizer alterações à configuração do Xbox Live para o seu jogo, você precisa publicar as alterações antes que eles são captados pelo restante do Xbox Live e podem ser vistos por seu jogo. Quando você ainda está trabalhando em seu jogo, você publicar sua própria área restrita para desenvolvimento. A área restrita de desenvolvimento permite que você trabalhe nas alterações para seu jogo em um ambiente isolado. Quando o jogo é liberado para o público, a configuração do Xbox Live será publicada automaticamente para a área restrita de varejo.
Por padrão, os Consoles um Xbox e PCs com Windows 10 são na área restrita de varejo.

Na página de configuração do Xbox Live, clique no **teste** botão para publicar a configuração atual do Xbox Live em sua área restrita de desenvolvimento.

![](../images/creators_udc/creators_udc_xboxlive_config_test.png)