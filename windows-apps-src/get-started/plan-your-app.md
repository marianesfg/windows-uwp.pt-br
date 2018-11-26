---
title: Criando um aplicativo da plataforma Universal do Windows (UWP) complexo
description: Nas equipes de design da Microsoft, nosso processo de criação de aplicativo consiste em cinco estágios distintos - conceito, estrutura, dinâmica, visual e protótipo. Recomendamos que você adote um processo semelhante e se divirta criando novas experiências para o mundo aproveitar.
ms.assetid: 9A5189CD-3B97-4967-8E7D-36D25F04F244
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 05a69cfdf96d6c7d3426b8d1ba414a42ff48a117
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7691837"
---
#  <a name="building-a-complex-universal-windows-platform-uwp-app"></a>Criando um aplicativo da plataforma Universal do Windows (UWP) complexo

Nas equipes de design da Microsoft, nosso processo de criação de aplicativo consiste em cinco estágios distintos: conceito, estrutura, dinâmica, visual e protótipo. Recomendamos que você adote um processo semelhante e se divirta criando novas experiências para o mundo aproveitar.

## <a name="concept"></a>Conceito

**Foque no seu aplicativo**

Ao planejar o seu aplicativo Plataforma Universal do Windows (UWP), você deve determinar não apenas o que ele fará e para quem é, mas também o que terá de bom nele. No núcleo de cada grande aplicativo há um conceito forte, que fornece uma base sólida.

Por exemplo, você quer criar um aplicativo de fotos. Pensando nas razões que levam os usuários a trabalhar, salvar e compartilhar suas fotos, você percebe que eles querem reviver lembranças, interagir com outras pessoas por meio das fotos e manter as fotos protegidas. E o seu aplicativo precisa fazer tudo isso da melhor maneira possível, então você usa essas metas de experiência como um guia para o restante do processo de design.

**De que trata o seu aplicativo?** Comece com um conceito amplo e relacione tudo o que o seu aplicativo pode fazer pelos usuários.

Por exemplo, digamos que você queira criar um aplicativo que ajude as pessoas a planejar suas viagens. Aqui estão algumas ideias que você pode esboçar na parte de trás de um guardanapo:

-   Pegar mapas de todos os locais em um itinerário e leve-os com você na viagem.
-   Informar-se sobre eventos especiais que acontecem durante a sua estada na cidade.
-   Deixar os colegas de viagem criar listas separadas, mas compartilháveis, de atividades e atrações imperdíveis.
-   Deixar os colegas de viagem compilar todas as suas fotos e as compartilhar com os amigos e familiares.
-   Saber sobre destinos recomendados com base nos preços de voos.
-   Encontre uma lista consolidada de ofertas em restaurantes, lojas e atividades próximo ao seu destino.

![um design para um aplicativo de viagens](images/ux-triptracker-tab-phone-700.png)

**Qual é o destaque do aplicativo?** Volte uma etapa e veja a sua lista de ideias para saber se algo em especial chama a sua atenção. Experimente reduzir a lista a apenas uma situação na qual queira se concentrar. No processo, você pode eliminar muitas ideias boas, mas dizer "não" a elas é crucial para melhorar determinada situação.

Depois de escolher uma situação individual, decida como você explicaria a uma pessoa comum, descrevendo em uma frase, o que seu aplicativo faz de melhor. Por exemplo:

-   Meu aplicativo de viagens é ótimo em ajudar amigos a criar itinerários colaborativos para viagens em grupo.
-   Meu aplicativo de ginástica é ótimo em permitir que amigos acompanhem a evolução de seus exercícios e compartilhem suas conquistas entre si.
-   Meu aplicativo de compras é ótimo para ajudar famílias a coordenar as compras semanais no mercado para não duplicarem ou perderem uma compra.

![um design para uma ferramenta de colaboração](images/ux-collaboration-tabphone-700.png)

Essa é declaração "o que há de bom" do seu aplicativo e pode guiar muitas decisões de design e compensações que você faz quando cria o seu aplicativo. Foque nas situações em que você quer os usuários usem o seu aplicativo. Tome cuidado para não transformar essa lista em uma lista de recursos. Ela deve ser sobre o que os seus usuários poderão fazer, em vez de o que o seu aplicativo poderá fazer.

**O funil de design**

É muito tentador—tendo pensado em uma ideia que você gostou—ir em frente e desenvolver o aplicativo; talvez, até começar de certa forma a produção. Mas digamos que você faça isso e, em seguida, uma outra ideia interessante venha à mente. É natural que você seja tentado a ficar com a ideia em que você já investiu, independentemente dos méritos relativos das duas ideias. Se você tivesse pensado na outra ideia no início do processo! Bem, o funil de design é uma técnica que ajuda a descobrir suas melhores ideias o mais cedo possível.

O termo "funil" vem de sua forma. Na extremidade ampla do funil, muitas ideias entram e cada uma é percebida como um artefato de baixíssima fidelidade do design (um esboço, talvez, ou um parágrafo de texto). Como essa coleção de ideias percorre em direção à parte mais estreita do funil, o número de ideias é diminuído, enquanto a fidelidade dos artefatos que representam as ideias aumenta. Cada artefato deve capturar apenas as informações necessárias para julgar uma ideia em relação à outra ou para responder uma pergunta em particular, como "isso é utilizável ou intuitivo?". *Não gaste mais tempo e esforço em cada fase do que isso*. Algumas ideias vão cair no esquecimento quando você testá-las, e você vai aceitar isso porque não vai investir nelas mais do que o necessário para julgar a ideia. As ideias que sobrevivem para avançar ainda mais no funil receberão sucessivamente tratamentos de alta fidelidade. No final, você terá um único artefato de design que represente a ideia vencedora. Essa é a ideia que ganhou por causa de seus méritos, e não apenas porque veio em primeiro lugar. Você terá projetado o melhor aplicativo que pôde.

## <a name="structure"></a>Estrutura


**Organização deixa tudo mais fácil**

![organização deixa tudo mais fácil](images/ux-vision-and-process-organization.png)

Quando você estiver feliz com o seu conceito, estará preparado para o próximo estágio—criar o plano gráfico do seu aplicativo. A arquitetura da informação (AI) dá ao seu conteúdo a integridade estrutural de que ele precisa. Ela ajuda a definir o modelo navegacional do seu aplicativo e, consequentemente, a identidade dele. Planejando como o seu conteúdo será organizado—e como os seus usuários descobrirão tal conteúdo—você pode ter uma ideia melhor de como vai ser a experiência dos usuários em relação com seu aplicativo.

Uma boa IA não só facilita cenários de usuários, mas ajuda você a imaginar as telas principais para começar. O aplicativo [audível](http://go.microsoft.com/fwlink/p/?LinkID=268089) , por exemplo, lança diretamente em um hub que fornece acesso à biblioteca, loja, notícias e estatísticas do usuário. A experiência é focada, para que os usuários podem obter e desfrutar de audiobooks rapidamente. Níveis mais profundos do aplicativo focam em tarefas mais específicas.

Para obter diretrizes relacionadas, veja [Noções básicas de design de navegação](../design/basics/navigation-basics.md).

## <a name="dynamics"></a>Dynamics

**Executar seu conceito**

Se a fase de concepção é sobre a definição de propósito de seu aplicativo, o estágio de dinâmica é todo sobre a execução desse propósito. Isso pode ser alcançado de muitas formas, como usar esboços para delinear os fluxos de página (como você vai de um lugar ao próximo dentro do aplicativo para chegar ao objetivo) e pensas sobre a voz e as palavras usadas na IU do seu aplicativo. Wireframes são ferramentas rápidas de baixa fidelidade que ajudam-no a tomar decisões críticas sobre o fluxo de usuários do seu aplicativo.

O fluxo de seu aplicativo deve estar diretamente relacionado à declaração de excelência e ajudar os usuários a realizar determinada situação que você quer destacar. Os ótimos aplicativos têm fluxos fáceis de aprender e requerem esforço mínimo. Comece a pensar em um nível tela-a-tela—veja seu aplicativo como se estivesse usando-o pela primeira vez. Quando identificar cenários de usuários para as páginas que você criar, você vai dar às pessoas exatamente o que elas querem, sem muitos toques de tela desnecessários. A dinâmica também tem a ver com movimento. As capacidades de movimento corretas irão determinar a fluidez e facilidade de uso de uma página para a próxima.

Técnicas comuns que ajudam a concluir esta etapa:

-   Faça o esboço do fluxo: o que vem primeiro, o que vem depois?
-   Crie um rascunho sequencial do fluxo: como os usuários devem navegar pela interface para concluir o fluxo?
-   Faça o protótipo: experimente o fluxo com um protótipo rápido.

**O que os usuários devem conseguir fazer?** Por exemplo, o aplicativo de viagens é "ótimo para ajudar amigos a criar itinerários colaborativos para viagens em grupo". Vamos listar os fluxos que queremos habilitar:

-   Criar uma viagem com informações gerais.
-   Convide amigos para participar de uma viagem.
-   Participar da viagem de um amigo.
-   Ver itinerários recomendados por outros viajantes.
-   Adicionar destinos e atividades às viagens.
-   Editar e comentar sobre destinos e atividades que os amigos adicionaram.
-   Compartilhe itinerários para amigos e parentes seguirem.

## <a name="visual"></a>Visual

**Falar sem palavras**

![um design para um aplicativo de fazer coquetéis](images/ux-cocktailcreator-tab-phone.png)

Depois de estabelecer a dinâmica do seu aplicativo, você pode fazer com que seu aplicativo brilhe com o acabamento visual correto. Ótimos visuais definem não só o visual de seu aplicativo, mas sua sensação e forma que ganha vida através da animação e movimento. Sua escolha da paleta de cores, ícone, e gráficos são apenas alguns exemplos dessa linguagem visual.

Todos os aplicativos têm a sua própria identidade original, então explore os sentidos visuais que você pode tomar com o seu aplicativo. O conteúdo deve orientar o visual; não deixe que a aparência defina o seu conteúdo.

## <a name="prototype"></a>Protótipo

**Refinar sua obra de arte**

Criar o protótipo é um estágio no *funil de design*—uma técnica sobre a qual já falamos—em que o artefato que representa a sua ideia é desenvolvido para além do esboço, mas ainda é menos complicado que um aplicativo concluído. Um protótipo pode ser um fluxo de telas desenhadas à mão mostrado para um usuário. A pessoa que executa o teste pode responder a estímulos do usuário, colocando diferentes telas para baixo, ou colando ou descolando pequenos pedaços de IU nas páginas, para simular um aplicativo em execução. Ou, um protótipo pode ser um aplicativo muito simples que simula alguns fluxos de trabalho, desde que o operador siga um roteiro e pressione os botões certos. Nesta fase, as ideias começam a realmente ganham vida e seu trabalho duro é testado a sério. Quando prototipar áreas de seu aplicativo, leve o tempo necessário para esculpir e refinar os componentes que precisam mais.

Para novos desenvolvedores, não é demais realçar: a criação de grandes aplicativos é um processo iterativo. Recomendamos que você prototipe cedo e frequentemente. Como qualquer esforço criativo, os melhores aplicativos são o produto de um processo intensivo de teste e erro.

## <a name="decide-what-features-to-include"></a>Decidir quais recursos serão incluídos

Quando se sabe o que os usuários querem e como ajudá-los a conseguir o que querem, você pode dar uma olhada nas ferramentas específicas na sua caixa de ferramentas. Conheça a Plataforma Universal do Windows (UWP) e associe recursos com as necessidades do seu aplicativo. Certifique-se de seguir as [diretrizes para a experiência do usuário](https://developer.microsoft.com/windows/apps/design) para cada recurso.
<!--need URL for landing page -->

Técnicas comuns:

-   Pesquisa de plataforma: descubra os recursos que a plataforma oferece e como usá-los.
-   Diagramas de associação: conecte seus fluxos aos recursos.
-   Protótipo: teste os recursos para garantir que eles fazem o que você precisa.

**Contratos**seu aplicativo pode participar de contratos do aplicativo que permitem fluxos de usuário amplos, entre aplicativos e entre recursos.

-   **Compartilhar**permitem que os usuários compartilhem conteúdo do seu aplicativo com outras pessoas por meio de outros aplicativos e também recebam conteúdo compartilhável de outras pessoas e aplicativos.
-   **Reproduzir em**permitem que os usuários gostam de áudio, vídeo ou imagens do seu aplicativo para outros dispositivos em sua rede doméstica.
-   **Seletor de arquivos e extensões de seletor de arquivo** Permita que os usuários carreguem e salvem seus arquivos de sistema de arquivos local, dispositivos de armazenamento conectados, grupo doméstico ou até mesmo de outros aplicativos. Você também pode oferecer uma extensão de seletor de arquivos para que outros aplicativos possam rodar o conteúdo do seu aplicativo.

Para saber mais, consulte as [extensões e contratos de aplicativo](https://msdn.microsoft.com/library/windows/apps/hh464906).
<!-- Win 8 page. Should have replacement. -->

**Diferentes exibições, fatores forma e configurações de hardware**Windows coloca os usuários no comando e seu aplicativo na linha de frente. Você provavelmente deseja que a interface do usuário do seu aplicativo chame a atenção independentemente do tipo de dispositivo, do modo de entrada, da orientação de exibição, da configuração de hardware e das circunstâncias que o usuário decida usá-lo.

**Tocar primeiro**Windows oferece uma experiência de toque única que faz mais do que apenas emular a funcionalidade do mouse.

Por exemplo, zoom semântico é uma forma de toque otimizado para navegar por uma grande quantidade de conteúdo. Os usuários podem fazer movimentos panorâmicos ou rolar por categorias de conteúdo e depois ampliar essas categorias para ver informações cada vez mais detalhadas. Você pode usar isso para apresentar o seu conteúdo de forma mais prática, visual e informativa do que com a navegação tradicional e os padrões de layout, como guias.

Claro, você pode se beneficiar de inúmeras vantagens de interações de toque, como girar, aplicar panorâmica, passar o dedo, dentre outras. Saiba mais sobre [Toque e outras interações do usuário](../design/input/input-primer.md).

**Interessante e novo**Certifique-se de que seu aplicativo pareça novo e interesse aos usuários com estas experiências padrão:

-   **Animações**Use sua biblioteca de animações para deixar seu aplicativo rápido e fluido para seus usuários. Ajude os usuários a entenderem as alterações de contexto e vincule experiências com transições visuais. Saiba mais sobre [animações na interface do usuário](../graphics/animations-overview.md).
-   **Notificações do sistema**permite que os usuários saibam sobre conteúdos sensíveis ao tempo ou pessoalmente relevantes por meio de notificações do sistema e convide-os para o seu aplicativo, mesmo quando o aplicativo é fechado. Saiba mais sobre [blocos, selos e notificações do sistema](../design/shell/tiles-and-notifications/index.md).
-   **Blocos de aplicativo**fornece atualizações recentes e relevantes para atrair os usuários de volta ao seu aplicativo. Há mais sobre isso na próxima seção. Saiba mais sobre [blocos de aplicativos](../design/shell/tiles-and-notifications/creating-tiles.md).

**Personalização**

-   **Configurações**permite que os usuários criem a experiência que quiserem ao salvar configurações do aplicativo. Consolide todas as suas configurações em uma tela, e então permita que os usuários configurem o seu aplicativo usando um mecanismo comum com o qual já estejam familiarizados. Saiba mais sobre [adicionar configurações de aplicativos](../design/app-settings/app-settings-and-data.md).
-   **Roaming**criar uma experiência contínua entre dispositivos fazendo roaming de dados que permite que os usuários retomem uma tarefa exatamente onde pararam e preserva a experiência do usuário que eles mais se importam, independentemente do dispositivo que eles estão usando. Facilite o uso do seu aplicativo em qualquer lugar—na cozinha, no computador da família ou de trabalho, no tablet pessoal, e outros fatores forma—mantendo configurações e estados com roaming. Saiba mais sobre [gerenciamento de dados de aplicativos](../design/app-settings/store-and-retrieve-app-data.md) e consulte [Diretrizes de dados de aplicativo em roaming](https://msdn.microsoft.com/library/windows/apps/hh465094).
-   **Blocos de usuário**  tornar seu aplicativo mais pessoal aos usuários carregando a imagem de bloco de usuário deles ou permitir que os usuários definam conteúdos do seu aplicativo como bloco pessoal no Windows.

**Recursos do dispositivo**Certifique-se de que seu aplicativo tira proveito dos recursos dos dispositivos atuais.

-   **Gestos de proximidade**permitem que os usuários conectem dispositivos com outros usuários que estão fisicamente próximos, fisicamente encostando os dois dispositivos (jogos para vários jogadores). Saiba mais sobre [proximidade e toques](https://msdn.microsoft.com/library/windows/apps/hh465229).
-   **Câmeras e dispositivos de armazenamento externo**conecte seus usuários a suas câmeras internas ou conectado para conversas ou conferências, gravar vlogs, tirar fotos de perfil, documentar o mundo à volta deles ou qualquer atividade em seu aplicativo seja bom. Saiba mais sobre o [acesso a conteúdo em armazenamento removível](https://msdn.microsoft.com/library/windows/apps/hh465189).
-   **Acelerômetros e outros sensores**   Dispositivos vêm com vários sensores atualmente. O seu aplicativo pode esmaecer ou clarear o seu visor com base na luz ambiente, redirecionar o fluxo da IU caso o usuário girar o visor, ou reagir a qualquer movimento físico. Saiba mais sobre [sensores](../devices-sensors/sensors.md).
-   **Localização geográfica**Use informações de localização geográfica dados da web padrão ou de sensores de geolocalização para ajudar os usuários a circular, encontrar a posição em um mapa ou receber notificações sobre pessoas e atividades próximas e destinos. Saiba mais sobre [localização geográfica](https://msdn.microsoft.com/library/windows/apps/hh465139).

Vamos considerar o aplicativo de viagens novamente. Para ser ótimo em ajudar amigos a criar de forma colaborativa itinerários de viagens em grupo, você pode usar alguns destes recursos:

-   Compartilhamento: os usuários compartilham viagens futuras e seus itinerários em várias redes sociais para dividir sua expectativa pré-viagem com seus amigos e familiares.
-   Pesquisa: os usuários procuram e encontram atividades ou destinos a partir de itinerários compartilhados ou públicos de outras pessoas, que eles podem incluir em suas próprias viagens.
-   Notificações: os usuários são avisados quando companheiros de viagem atualizam os itinerários.
-   Configurações: os usuários configuram o aplicativo de acordo com sua preferência, por exemplo, qual viagem deve gerar notificações ou quais grupos sociais têm permissão para pesquisar itinerários dos usuários.
-   Zoom semântico: os usuários navegam pela linha do tempo de seu itinerário e ampliam o zoom para ver mais detalhes da longa lista de atividades que planejaram.
-   Blocos do usuário: os usuários escolhem a imagem que querem exibir quando compartilharem suas viagens com amigos.

## <a name="decide-how-to-monetize-your-app"></a>Decida como rentabilizar seu aplicativo

Você tem várias opções para ganhar dinheiro com o seu aplicativo. Se você decidir usar anúncios ou promoções, pode criar sua interface de acordo com esses recursos. Para obter mais informações, consulte o tópico sobre [planejamento para monetização](../monetize/index.md).

## <a name="design-the-ux-for-your-app"></a>Projetar a experiência do usuário do aplicativo

Isso diz respeito aos conceitos básicos corretos. Agora que você sabe o que o seu aplicativo faz de melhor e descobriu os fluxos para os quais deseja dar suporte, pode começar a pensar nos conceitos básicos do design da experiência do usuário.

**Como você deve organizar o conteúdo da interface do usuário?**   A maior parte do conteúdo do aplicativo pode ser organizada por meio de agrupamentos ou hierarquias. O que você escolhe como o agrupamento de nível superior do seu conteúdo deve corresponder ao foco da sua declaração de excelência.

Usando o aplicativo de viagens como exemplo, há várias maneiras de agrupar itinerários. Se o foco do aplicativo for descobrir destinos interessantes, você pode agrupá-los com base em interesses, como aventura, diversão sob o sol ou refúgios românticos. Entretanto, como o foco do aplicativo é planejar viagens com amigos, faz mais sentido organizar itinerários baseados em círculos sociais, como familiares, amigos ou colegas de trabalho.

Escolher como você quer agrupar o conteúdo ajuda a decidir que páginas ou visualizações são necessárias no seu aplicativo. Consulte Noções básicas da interface do usuário para obter mais informações.

**Como apresentar o conteúdo da interface do usuário?** Depois de decidir como organizar a interface do usuário, você pode definir metas de experiência do usuário que especifiquem como a interface do usuário é construída e apresentada ao usuário. Em qualquer situação, você deve garantir que o seu usuário possa continuar usando e aproveitando o seu aplicativo o mais rápido possível. Para fazer isso, decida quais partes da interface do usuário precisam ser apresentadas primeiro e verifique se essas partes estão completas antes de perder tempo construindo as partes não críticas.

No aplicativo de viagens, provavelmente, a primeira coisa que o usuário irá querer fazer no aplicativo é encontrar um itinerário de viagem específico. Para apresentar essa informação o mais rápido possível, você deve mostrar a lista de viagens primeiro, usando um controle **ListView**.

![um design para o seletor de itinerário em um aplicativo de viagens](images/ux-app-travel-cc-a-1-180.png)

Depois de mostrar a lista de viagens, você pode começar a carregar outros recursos, como um feed de notícias sobre viagens de amigos.

**De quais comandos e superfícies de IU você precisa?**   Analise os fluxos que você identificou. Para cada fluxo, crie um esboço das etapas que os usuários devem seguir.

Vamos dar uma olhada no fluxo "Compartilhar itinerários para que amigos e familiares sigam". Vamos supor que o usuário já criou uma viagem. O compartilhamento do itinerário de uma viagem pode precisar destas etapas:

1.  O usuário abre o aplicativo e vê uma lista de viagens que ele criou.
2.  O usuário toca na viagem que quer compartilhar.
3.  Os detalhes da viagem aparecem na tela.
4.  O usuário acessa uma interface para iniciar o compartilhamento.
5.  O usuário seleciona ou insere o email ou nome do amigo com quem quer compartilhar a viagem.
6.  O usuário acessa uma interface para finalizar o compartilhamento.
7.  O seu aplicativo atualiza os detalhes da viagem com a lista de pessoas com quem o usuário compartilhou sua viagem.

Durante esse processo, você começa a ver que IU você precisa criar e os detalhes adicionais que você precisa descobrir (como escrever um texto cliché de email padrão para amigos que ainda não usam o aplicativo). Você também pode começar a eliminar etapas desnecessárias. Talvez o usuário não precise realmente ver os detalhes da viagem antes de compartilhá-la, por exemplo. Quanto mais limpo o fluxo, mais fácil usá-lo.

Para saber mais sobre como usar superfícies diferentes, confira <!--[Command design basics](../design/basics/commanding-basics.md)-->.

**Qual deve ser a aparência do fluxo?** Quando definir as etapas que o usuário realizará, você pode transformar esse fluxo em metas de desempenho. Para saber mais, consulte [Planejar o desempenho](../debug-test-perf/planning-and-measuring-performance.md).

**Como você deve organizar comandos?** Usar seu esboço das etapas do fluxo para identificar os comandos que você precisa criar. Depois, pense nos locais onde usar estes comandos em seu aplicativo.

-   **Sempre tente usar o conteúdo.** Sempre que possível, permitem que os usuários manipulem diretamente o conteúdo na tela do aplicativo, em vez de adicionar comandos que act no conteúdo. Por exemplo, no aplicativo de viagens, permita que os usuários reorganizem seu itinerário, arrastando e soltando as atividades em uma lista na tela, em vez de selecionar a atividade e usar botões de comando Para cima ou Para baixo.
-   **Se não puder, use o conteúdo.** Coloque comandos em uma destas superfícies de IU se você não puder usar o conteúdo:

    -   Na [barra de comandos](https://msdn.microsoft.com/library/windows/apps/hh465302): você deve colocar a maioria dos comandos na barra de comandos, que costuma estar oculta até que o usuário toque para deixá-la invisível.
    -   Na tela do aplicativo: se o usuário estiver em uma página ou modo de exibição que tenha uma única finalidade, você pode oferecer comandos para essa finalidade diretamente na tela. Deve haver muito pouco desses comandos presentes.
    -   Em um [menu de contexto](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/menus): você pode usar menus de contexto para ações da área de transferência (como recortar, copiar e colar) ou para comandos que se aplicam ao conteúdo que não pode ser selecionado (como a adição de uma tachinha a um local no mapa).

**Decida como dispor o seu aplicativo em cada modo de exibição.** Windows dá suporte a orientações retrato e paisagem e ao redimensionamento de aplicativos para qualquer largura, desde tela cheia a largura mínima. Você quer que o seu aplicativo tenha uma boa aparência e funcione perfeitamente em qualquer site, em qualquer tela, em ambas as orientações. Isso significa que você precisa planejar o layout dos elementos da interface para diferentes tamanhos e exibições. Ao fazer isso, a IU do seu aplicativo muda de maneira fluida para atender às necessidades e preferências do usuário.

![designs móveis e de computador para um aplicativo](images/ux-budgettracker1-md-notablet.png)

Para obter mais informações sobre a criação de diferentes tamanhos de tela, consulte [Tamanhos de tela e pontos de interrupção para um design responsivo](https://docs.microsoft.com/en-us/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design).

## <a name="make-a-good-first-impression"></a>Causar uma boa primeira impressão

Pense naquilo que você quer que os usuários pensem, sintam ou façam assim que iniciarem o seu aplicativo. Revise a sua declaração de excelência. Mesmo que você não tenha a chance de informar pessoalmente seus usuários sobre a especialidade do seu aplicativo, pode transmitir a mensagem a eles ao passar a sua primeira impressão. Tire vantagem disto:

**Bloco e notificações**  o bloco é o rosto do seu aplicativo. Entre os vários aplicativos encontrados na tela Inicial do usuário, o que vai fazer o usuário querer abrir o seu? Crie um bloco que destaque a marca do seu aplicativo e mostre o que ele tem de melhor. Use notificações de bloco para o seu aplicativo parecer sempre novo e relevantes, atraindo o usuário de volta para o seu aplicativo várias vezes.

**Tela inicial**a tela inicial deve carregar mais rápido possível e permanece na tela somente quando necessário para inicializar o estado do aplicativo. O que você mostra na tela inicial deve expressar a personalidade do seu aplicativo.

**Primeira inicialização**antes dos usuários se inscrever para seu serviço, fazerem login conta ou adicionarem seu próprio conteúdo, o que eles verão? Tente demonstrar o valor do seu aplicativo antes de solicitar informações dos usuários. Considere mostrar amostra de conteúdos para que as pessoas possam dar uma olha e entender o que o seu aplicativo faz antes de você pedir a eles que confirmem.

**Home page**a home page é onde você traz usuários cada vez que eles iniciam o seu aplicativo. O conteúdo aqui deve ter um foco claro e, imediatamente, apresentar para que o seu aplicativo foi feito. Dê um objetivo maior a essa página e confie que as pessoas explorem o restante do seu aplicativo. Concentre-se em eliminar as distrações na página de destino, e não na descoberta.

## <a name="validate-your-design"></a>Valide seu design

Antes de você se aprofundar muito no desenvolvimento do seu aplicativo, deve validar o seu design ou protótipo de acordo com diretrizes, impressões de usuário e exigências para evitar ter que refazer o trabalho depois. Cada recurso tem um conjunto de diretrizes de experiência do usuário para ajudá-lo a refinar seu aplicativo e um conjunto de requisitos da loja que você precisa cumprir para vender seu aplicativo na Microsoft Store. Você pode usar o [Kit de Certificação de Aplicativos Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) para testar a conformidade técnica com os requisitos da Loja. Você também pode usar as ferramentas de desempenho no Microsoft Visual Studio para garantir que o usuário tenha uma excelente experiência em todos os cenários.

Use as [Diretrizes detalhadas de experiência do usuário para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/hh465424) para manter o foco em recursos importantes. Use as [ferramentas de desempenho do Visual Studio](https://msdn.microsoft.com/library/windows/apps/hh696636.aspx) para analisar o desempenho de cada uma das situações do seu aplicativo.
