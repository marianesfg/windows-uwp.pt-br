---
title: Introdução ao cruzada jogue
description: Aprenda a desenvolver jogos play cruzada que são executados no PC e Xbox One consoles.
ms.assetid: 6c8e9d08-a3d2-4bfc-90ee-03c8fde3e66d
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, cross-play, reproduzir em qualquer lugar
ms.localizationpriority: medium
ms.openlocfilehash: 79b0c211291d8a27456126ea0378f9093d1dccab
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646721"
---
# <a name="get-started-with-cross-play-games"></a>Introdução ao cruzada jogue

## <a name="summary"></a>Resumo

Com o lançamento do Windows 10, os desenvolvedores de jogos poderá liberar únicos produtos no Xbox One (como um jogo XDK) e no Windows 10 (como um jogo UWP). Em alguns casos, os desenvolvedores deseja permitir a reprodução entre esses jogos, onde as versões de seus jogos Xbox One e Windows 10 são unificadas em todos os serviços Xbox Live, como com vários participantes, salvar jogos, conquistas e muito mais. Para habilitar a cross-play, esses jogos compartilharão uma única configuração de serviço de ID de título e o Xbox Live em versões de XDK e UWP do jogo.

Ingestão de um jogo hoje XDK + UWP requer 4 etapas principais:

1.  Criar seu produto UWP no Partner Center

2.  Criar seu jogo XDK no XDP, selecionando as plataformas que você deseja compartilhar configuração XBL em

3.  Associar as informações de produto da UWP para seu produto XDK XDP

4.  Configurar e publicar o Xbox Live por meio de XDP

A finalidade deste documento é ainda mais em detalhes essas etapas de 4 para torná-lo mais fácil possível para os funcionários do Xbox para a ingestão de jogos de multiplataforma do XDK + UWP

## <a name="terminology"></a>Terminologia

### <a name="scenario-terms"></a>Termos de cenário

1.  Cross-Play: Um jogo que está lançando em mais de uma plataforma, mas compartilha um único Xbox ID do título e configuração de serviço. O resultado final é que ambas as versões do jogo compartilham a mesma Xbox Live configuração - conquistas, placares de líderes, jogos, com vários participantes e muito mais.

2.  Partner Center: O [portal](https://partner.microsoft.com/dashboard) onde você pode reservar as identidades de aplicativo para uso em UWP hoje e a instalação do Xbox Live configuração de desenvolvimento para a UWP.

3.  XDP: Xbox Portal do desenvolvedor, que existe atualmente para ingestão, configurar e publicar jogos Xbox um XDK e SRA e verá o uso adicional de ingestão, configurar e publicar jogos XDK + UWP Cross-Play.

### <a name="identity-terms"></a>Termos de identidade

1.  ID do título: Essa é a ID de título do Xbox, usado para identificar cada jogo para Xbox Live. Uma ID de título é mapeado para um único produto, que pode abranger várias plataformas.

2.  ID de configuração de serviço (SCID): Cada título de Xbox (identificado por uma ID de título) tem uma ID de configuração de serviço correspondente (também conhecido como SCID). Essa ID permite que o Xbox Live identificar exclusivamente as regras / configuração a ser usado ao interagir com seu título.

3.  Nome de família de pacotes (PFN): Esta é uma identidade atribuída a cada produto criado no Partner Center. Depois que você associa seu UWP para a identidade deste produto Partner Center, ele adotará essa PFN. PFNs são identificadores de produto exclusiva, que podem abranger várias plataformas. PFNs são 1:1 com IDs de título do Xbox.

4.  ID do aplicativo MSA: Também conhecida como ID de cliente do MSA, isso é outra identidade de aplicativo atribuída MSA no momento da criação de produto no Partner Center. Essa identidade ajuda os serviços da Microsoft a identificar seu aplicativo. IDs de aplicativo do MSA são 1:1 com PFNs (e de acordo com as IDs de título do Xbox).

## <a name="scenario-overview"></a>Visão geral do cenário

### <a name="what-is-cross-play"></a>O que é Play cruzada?

Demonstração de uma experiência de Windows 10; Cross-play é jogos de vários dispositivos entre o PC e Xbox One com jogos compartilhando uma única configuração do Xbox Live entre versões de dispositivo para destacar em cenários como multiplayer entre dispositivos, conquistas e placares de líderes, e salva de jogo.

### <a name="what-are-the-pros-and-cons-of-cross-play"></a>Quais são os prós e contras do Play cruzada?

Cross-Play provavelmente é a abordagem certa para você, se você quiser que as versões do seu jogo para o XDK e UWP:

-   Envolva-se em vários dispositivos com vários participantes (vs Xbox One. PC) pelo menos um modo de jogo com vários participantes

-   Compartilhar um único salvamento de jogo que o usuário pode usar em ambos os dispositivos

-   Ter um único conjunto de realizações & jogos / desafia / placares de líderes que ele podem avança contra fogo em ambos os dispositivos

Cross-Play provavelmente não é a abordagem certa para você se:

-   Você deseja impedir que os jogadores de PC e Xbox One do seu jogo se envolver com vários participantes em todos os dispositivos em todos os modos de jogo com vários participantes

-   Você deseja manter Xbox One e jogo PC salvar separado (talvez por motivos de segurança ou relação de confiança)

-   Você deseja que as versões do Xbox One e PC do seu jogo para ter jogos separados (também conhecido como os usuários que adquirirem Xbox One e PC podem receber os jogos de 1000 para cada plataforma, em vez de 1.000 compartilhado)

Em geral, Play cruzada adiciona o maior valor para:

-   Livre-to-Play / Xbox jogar em qualquer lugar, que enfatizam a continuidade entre as versões do jogo Xbox One e PC

-   Jogos com vários dispositivos com vários participantes entre o PC e o Xbox One

**OBSERVAÇÃO**: Play cruzada está disponível tanto para novos jogos que liberando simultaneamente as versões do seu jogo XDK e UWP, bem como os jogos que já foram enviados a um XDK, mas estão adicionando uma versão UWP.

### <a name="what-are-the-restrictions-of-cross-play"></a>Quais são as restrições de jogo cruzada?

Até que o Xbox One dá suporte a jogos UWP, jogos entre-Play exigem uma XDK (para o console Xbox One) e a versão UWP (para o computador com Windows 10) do jogo.

Qualquer XDK + UWP Cross-Play jogo vem com algumas restrições importantes:

1.  **Títulos XDK devem ser incluídos em XDP**. Tanto de uma configuração de serviço e a experiência de publicação de linha principal, Partner Center não está equipado para oferecer suporte a títulos XDK com base.

2.  **Uma configuração de serviço único criada no XDP pode ser usado tanto pela versão de um jogo XDK e UWP**. Adicionamos novos recursos ao XDP para permitir que um jogo compartilhar uma configuração de serviço única entre suas versões XDK e UWP. A versão UWP ainda precisarão ser publicados no Partner Center para pacotes / catálogo, mas a publicação de todas as configuração de serviço pode ser feito em XDP.

3.  **Configuração de serviço não pode ser dividida entre XDP e Partner Center**. Centro de parceiro e XDP não estão cientes um do outro – publicar em um sobregravações qualquer existente publica de outra. Isso tem o potencial para irremediavelmente interromper a configuração de serviço e criar experiências de usuário terrível (fuga conquistas, perdeu o salvamento, etc.) e, consequentemente Exigimos que 100% da configuração de serviço a ser feito no XDP para jogos de cross-play XDK + UWP.

### <a name="create-your-uwp-product-in-partner-center"></a>Criar seu produto UWP no Partner Center

Crie um produto UWP no Partner Center, seguindo o guia: [Adicionando o Xbox Live para um projeto novo ou existente do UWP](get-started-with-visual-studio-and-uwp.md)

## <a name="setup-your-xdk-product-in-xdp"></a>Instalação do produto XDK XDP

Agora que a UWP é criado, você está pronto para instalação do produto XDK XDP. Se você ainda não tiver um título do XDK, você deve criar uma.

### <a name="create-your-xdp-product"></a>Criar seu produto XDP

Trabalhar com seu gerente de conta para criar um novo produto em seu editor em XDP ([https://xdp.xboxlive.com/](https://xdp.xboxlive.com/User/Publisher)).

Ao criar o produto no XDP, certifique-se de que você role até a parte inferior da seção esquerda da interface do usuário para selecionar as plataformas. Verifique todas as plataformas que você **pretende algum dia** lançar o jogo em com a integração de cross-play Xbox Live.

Depois de selecionar as plataformas, especifique o tipo de acesso a recursos para o seu jogo (provavelmente um título XDK) e os mecanismos de versão pretendida para este produto.


![](../images/ingesting_crossplay_games_xdp/image4.png)
### <a name="update-your-xdp-product-platforms"></a>Atualizar suas plataformas de produto XDP

Se você tiver um produto existente do XDK em XDP, ele precisa ser atualizado para dar suporte a plataforma PC. Para fazer isso, uma vez no produto, navegue até a instalação do produto &gt; tipo de plataforma.

![](../images/ingesting_crossplay_games_xdp/image10.png)

Nessa página, selecione as plataformas que você deseja dar suporte (as opções são Xbox One, limite de PC e Windows Mobile). Quando estiver satisfeito com sua seleção, selecione o botão "Enviar plataformas".

Essa alteração imediatamente será feita ao vivo (sem necessidade de uma configuração de serviço, catálogo ou binário de publicação para que isso entrarão em vigor). Observe que essa configuração abrange áreas restritas – você não pode ter tipos diferentes de plataforma por área restrita para o seu jogo.

### <a name="enter-your-msa-app-id"></a>Insira sua ID do aplicativo MSA

Depois que o produto XDP é criado, vá para a página de instalação do produto para o produto inserir a ID do aplicativo MSA criado anteriormente. Instalação do produto pode ser contatada, clicando no mais à esquerda "caixa de status" na barra de tarefas para o produto em XDP, como mostrado abaixo.

![](../images/ingesting_crossplay_games_xdp/image11.png)

Depois que você pode torná-lo para a página de instalação do produto, selecione a seção "Configuração de ID do aplicativo". Nessa área, você pode inserir a ID do aplicativo MSA que você recuperou e coloque-o no campo "ID do aplicativo", conforme mostrado abaixo.

![](../images/ingesting_crossplay_games_xdp/image12.png)

**Você** **não é necessário inserir o nome e o publicador atribuição**, **e especificamente não deve usar o link "Obter a ID do aplicativo" na página**, como você já tiver uma ID do aplicativo MSA você precisa inserir neste campo e não quiser que um novo gerado para seu aplicativo.

Depois de inserir sua ID do aplicativo MSA no campo "ID do aplicativo", clique no botão "Enviar configuração de ID de aplicativo". Isso economizará suas informações de ID de aplicativo do MSA com segurança Xbox Live – sempre que você fizer uma solicitação para recuperar um XToken de seu UWP, a declaração de título contida agora mapeará para este produto XDP (desde que a UWP é corretamente usando a identidade do manifesto AppX, você cria 1!d no Partner Center!)

### <a name="enter-the-partner-center-pfn-into-xdp"></a>Insira o PFN Partner Center em XDP

Embora as etapas acima são suficientes para obter seu jogo UWP autenticado e usando o Xbox Live com a configuração de serviço que você cria e publica no XDP, certos recursos do Xbox Live (como convites para múltiplos jogadores) deve estar ciente do PFN do jogo UWP para funcionar corretamente.

Para fazer isso, navegue até a instalação do produto &gt; associação do Dev Center

![](../images/ingesting_crossplay_games_xdp/image13.png)

Nessa página, insira o PFN para seu aplicativo da UWP (como recuperado na seção 4.1.1), em seguida, selecione o botão "Salvar".

Essa configuração não é feita ao vivo imediatamente. Ele é colocado em tempo real por meio do futuro publica de configuração de serviço para uma área restrita. Como tal, essa informação é uma área restrita e precisa ser publicado para cada área restrita para estar disponível.

### <a name="flag-your-app-for-xbox-cert-in-partner-center"></a>Sinalizador de seu aplicativo para Xbox Cert no Partner Center

Hoje, obter seu jogo reconhecido como o Xbox Live habilitado para Xbox Cert requer a intervenção manual. Trabalhar com seu gerente de versão para sinalizar o seu aplicativo é o que Xbox Live habilitado no Partner Center para detecção de SmartCert.

### <a name="update-your-uwp-game-in-the-xbox-app"></a>Atualizar seu jogo UWP no aplicativo do Xbox

Normalmente, o App o Xbox usa uma ID de título gerado automaticamente para todos os jogos UWP a potência que do Xbox Live experiências no App Xbox. Para fazer com que o seu jogo UWP use corretamente a identificação de título gerado pelo XDP no App Xbox, uma atualização de dados precisa ser feita dentro do Partner Center **antes de enviar seu jogo para a versão UWP.**

Para fazer isso, entre em contato com sua mãe e informe que deseja atualizar sua ID de título de aplicativo do Xbox para o nome do título.  Certifique-se de incluir a ID de título, criado no XDP (visível no XDP na instalação do produto &gt; detalhes do produto) e a URL para o Windows 10 para a UWP (visível no Partner Center sob o gerenciamento de aplicativo &gt; identidade de aplicativo).

## <a name="configure-xbox-live-in-xdp"></a>Configurar o Xbox Live em XDP

### <a name="service-configuration"></a>Configuração de serviço

Com nossos XDP Partner Center produtos e corretamente configurado e associado, você é livre para definir sua configuração de Xbox Live compartilhada dentro de XDP, como você faria normalmente para um título XDK.

**LEMBRETE** – acoplado jogos XDK + UWP devem, sob nenhuma circunstância, habilitar, configurar ou publicar a configuração do serviço Xbox Live por meio do Partner Center. Deixar de seguir essas diretrizes permanentemente pode prejudicar a configuração para o seu jogo Xbox Live.

### <a name="catalog-configuration"></a>Configuração do catálogo

Para um jogo XDK + UWP, configuração de um catálogo precisa ser configurado de duas vezes: uma vez na XDP para o XDK e também no Partner Center para a UWP.

Para a configuração de XDP, isso é exatamente o mesmo que um produto XDK normal. Para a configuração do Partner Center, etapas mais detalhadas podem ser encontradas [aqui](https://dev.windows.com/en-us/publish).

<table>
  <tr>
    <td>
      Para jogos de UWP, apenas um conjunto limitado de configuração do catálogo deve ser configurado. Em particular, informações de Marketing deve ser preenchidas para jogos de UWP, como configuração de serviço consome as cadeias de caracteres inseridas aqui para qualquer XDP configure jogo do Xbox Live. Além disso, disponibilidades devem ser configurado com "não disponível" para garantir que o jogo nunca aparecerá no catálogo do Xbox One se/quando as informações de catálogo para o jogo for publicado.
    </td>
  </tr>
</table>

### <a name="binary-configuration"></a>Configuração binária

Para um jogo XDK + UWP, configuração binária precisa ser configurado de duas vezes: uma vez na XDP para o XDK e também no Partner Center para a UWP.

Para a configuração de XDP, isso é exatamente o mesmo que um produto XDK normal. Para a configuração do Partner Center, etapas mais detalhadas podem ser encontradas [aqui](https://dev.windows.com/en-us/publish).

<table>
  <tr>
    <td>
      Para jogos de UWP, não é necessário para a configuração de binário em XDP. Você pode ignorar esta etapa inteiramente.
    </td>
  </tr>
</table>

## <a name="publish-in-xdp"></a>Publicar no XDP

Em termos gerais, a publicação de um jogo UWP + XDK segue o mesmo processo de publicação separadamente um título do XDK em XDP e o UWP no Partner Center. Como tal, a abaixo se concentra em processo exclusivo ou considerações devem ser feitas para jogos entre-play.

### <a name="dev-sandbox-publishing"></a>Publicação de área restrita de desenvolvimento

### <a name="no-dev-sandbox-catalog-equivalent-for-microsoft-store"></a>Nenhum equivalente do catálogo de área restrita de desenvolvimento do Microsoft Store

Enquanto XDP permite que você publique o seu catálogo de & binário para a versão de área restrita de desenvolvimento do Xbox One catálogo, o catálogo da Microsoft Store tem suporte da área restrita. Como tal, teste sua UWP em uma área restrita de desenvolvimento requer que você carregar esse UWP e reproduzi-lo diretamente. Isso não afeta o Xbox Live testes, mas pode alterar o padrão de processos de teste.

<table>
  <tr>
    <td>
      Para um jogo somente UWP, ele ainda é necessário para publicar o XDK informações de título de catálogo para desbloquear a configuração do serviço de publicação, mesmo que o jogo somente UWP tem presença de catálogo não Xbox One.
    </td>
  </tr>
</table>

### <a name="cert-sandbox-publishing"></a>Publicação de área restrita do certificado

### <a name="coordination-between-xdp-publishing-and-partner-center-publishing"></a>Coordenação entre publicação XDP e Partner Center

XDP, mover do certificado para o varejo é duas ações separadas e explícitas. No entanto, no Partner Center, o processo de envio move automaticamente seu jogo por meio de certificação e logon no varejo. Como tal, há uma ordem de operações para respeitar.

Quando você estiver pronto para ir para a certificação, você deve seguir as etapas a seguir na ordem:

1.  Publicar seu produto XDK no XDP CERT (incluindo o catálogo, binários e configuração de serviço)

2.  Iniciar o envio do Partner Center para o seu produto UWP

    1.  **Certifique-se de selecionar "Publicar este aplicativo manualmente" ou "somente \[data\]" no campo de data de publicação!** Se você não fizer isso, seu jogo UWP pode ser liberado para varejo automaticamente sem a intervenção do usuário

<table>
  <tr>
    <td>
      Para um jogo somente UWP, ele ainda é necessário para publicar sua configuração do catálogo e o serviço de certificado no XDP antes de iniciar o envio do Partner Center, mesmo que você não tiver um binário de título do XDK que você está publicando.
    </td>
  </tr>
</table>

### <a name="retail-sandbox-publishing"></a>Publicação de área restrita de varejo

### <a name="coordination-between-xdp-publishing-and-partner-center-publishing"></a>Coordenação entre publicação XDP e Partner Center

Depois de certificação do seu título XDK foi concluída e o UWP saiu certificação e está pronto para publicar, você estará pronto para publicar seu aplicativo para o varejo. Novamente, é importante seguir esse processo em uma ordem específica para garantir que seu título XDK e UWP fiquem alinhados.

1.  Quando estiver pronto, publicar seu produto XDK no XDP para varejo (incluindo o catálogo, binários e configuração de serviço)

2.  No Partner Center, na página de certificação de seu produto, selecione "Publicar agora" Se você selecionou anteriormente "Publicar este aplicativo manualmente", ou aguardar a publicação ocorrer se você tiver selecionado "somente \[data\]".

Depois de concluir essas etapas, você deve ter seu título XDK e um jogo UWP publicados para o mundo, com uma configuração de serviço compartilhado no varejo. Parabéns!

<table>
  <tr>
    <td>
      Para um jogo somente UWP, ele ainda é necessário para publicar seu catálogo e configuração do serviço para o varejo no XDP antes da publicação for concluída no Partner Center, caso contrário, a UWP lançada não poderão acessar o Xbox Live.
    </td>
  </tr>
</table>
