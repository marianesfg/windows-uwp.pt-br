---
author: PatrickFarley
title: Práticas recomendadas de atividades de usuários
description: Este guia descreve as práticas recomendadas para criar e atualizar as atividades do usuário.
keywords: atividade do usuário, atividades do usuário, linha do tempo, cortana retorma de onde você parou, cortana retoma de onde eu parei, project rome
ms.author: pafarley
ms.date: 08/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: b11151df981a4b5ce233458581d21e405be9844c
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4425653"
---
# <a name="user-activities-best-practices"></a>Práticas recomendadas de atividades de usuários

Este guia descreve as práticas recomendadas para criar e atualizar as atividades do usuário. Para obter uma visão geral do recurso atividades do usuário no Windows, consulte [continuar atividade do usuário, mesmo entre dispositivos](https://docs.microsoft.com/windows/uwp/launch-resume/useractivities). Ou, consulte a [seção de atividades do usuário](https://docs.microsoft.com/windows/project-rome/user-activities/) do projeto Roma para as implementações de atividades em outras plataformas de desenvolvimento.

## <a name="when-to-create-or-update-user-activities"></a>Quando criar ou atualizar atividades do usuário

Como cada aplicativo é diferente, cabe a cada desenvolvedor para determinar a melhor maneira de mapear ações dentro do aplicativo para atividades do usuário. Suas atividades do usuário serão exibidas na Cortana e linha do tempo, que se concentram na produtividade dos usuários crescentes e eficiência, ajudando-os voltar ao conteúdo visitadas no passado.

**Diretrizes gerais**

* **Registre uma única atividade para um grupo de ações do usuário relacionados.** Isso é especialmente relevante para listas de reprodução de música ou programas de TV: uma única atividade pode ser atualizada em intervalos regulares para refletir o progresso do usuário. Nesse caso, você terá uma atividade de usuário único com vários itens de histórico que representam períodos de envolvimento em vários dias ou semanas. O mesmo se aplica a atividades com base em documento no qual o usuário faz gradual progresso dentro de seu aplicativo.
* **Armazenar dados do usuário na nuvem.** Se você quiser dar suporte a atividades entre dispositivos, você precisará certificar-se de que o conteúdo necessário para se envolver novamente essa atividade é armazenado em um local na nuvem. Atividades específicas do dispositivo serão exibido na linha do tempo no dispositivo em que a atividade foi criada, mas talvez não apareçam em outros dispositivos.
* **Não crie atividades para ações que os usuários não precisam continuar.** Se seu aplicativo é usado para concluir operações únicas e simples, que não continue além do status, você provavelmente não precisará criar uma atividade do usuário.
* **Não crie atividades para Ações concluídas por outros usuários.** Se uma conta externa envia ao usuário uma mensagem ou @-mentions -los em seu aplicativo, você não deve criar uma atividade para isso. Esse tipo de ação é melhor servido por notificações da Central de ações.
  * Cenários de colaboração são uma exceção: se vários usuários estão trabalhando na mesma atividade juntos (como um documento do Word), haverá casos em que outro usuário tiver feito alterações depois que o usuário. Nesse caso, convém atualizar a atividade existente para refletir as alterações feitas no documento. Isso envolve a atualizar os dados de conteúdo de atividade do usuário existentes sem criar um novo Item de histórico.

**Diretrizes para tipos específicos de aplicativos**

Enquanto cada aplicativo é diferente, a maioria dos aplicativos cairá em um dos seguintes padrões de interação.
* **Aplicativos baseados em documento** — criar uma atividade por documento, com um ou mais itens de histórico refletindo períodos de uso. É importante atualizar sua atividade de como as alterações são feitas no documento.
* **Jogos** — criar uma atividade para cada Salvar jogo ou o mundo. Se seu jogo dá suporte a apenas uma única sequência dos níveis, você pode publicar novamente a mesma atividade ao longo do tempo, embora talvez você queira atualizar os dados de conteúdo para mostrar o progresso ou realizações mais recentes.
* **Aplicativos de utilidade** — se não houver nada dentro de seu aplicativo que os usuários precisariam deixar e retomada, você não precisa usar atividades do usuário. Um bom exemplo é um aplicativo simples como calculadora.
* **Aplicativos de linha de negócios** — muitos aplicativos existem para gerenciar tarefas simples ou fluxos de trabalho. Criar uma atividade para cada fluxo de trabalho separado acessado por meio do seu aplicativo (por exemplo, relatórios de despesas cada seria uma atividade separada, para que o usuário pode, em seguida, clique em uma atividade para ver se um determinado relatório foi aprovado).
* **Aplicativos de reprodução de mídia** — criar uma atividade por agrupamento lógico de conteúdo (como um conteúdo de reprodução, programa ou autônomo). A pergunta subjacente para os desenvolvedores de aplicativos é se uma cada parte do conteúdo (episódio da TV, música) será considerado como conteúdo de autônomo ou parte de uma coleção. Como regra geral, se o usuário opte por para reproduzir uma coleção ou conteúdo sequencial, a coleção como um todo é a atividade. Se ele optar por reproduzir uma peça única de conteúdo, que uma parte do conteúdo é a atividade. Consulte diretrizes mais específicas abaixo.
  * **Música: álbum/artista/gênero** — se o usuário seleciona um álbum, artista ou gênero e visitas **Reproduzir**, essa coleção é a atividade; não gravar uma atividade separada para cada música. Para coleções curtas como um único álbum ou coleções que está sendo reproduzidas em ordem aleatória, você talvez não precise atualizar a atividade para refletir a posição atual do usuário. Para reprodução longa sequencial, como um álbum ou playlist, a gravação de sua posição dentro do álbum pode fazer sentido.
  * **Música: inteligente, listas de reprodução** — aplicativos que reproduzem música em ordem aleatória devem gravar uma única atividade para essa lista de reprodução. Se o usuário reproduz a lista de reprodução uma segunda vez, você deve criar registros de histórico adicional para a mesma atividade. A posição atual do usuário na lista de reprodução de gravação não é necessário porque a ordenação é aleatória.
  * **Série de TV** — se seu aplicativo está configurado para reproduzir o próximo episódio depois atual for concluída, você deve escrever uma única atividade para a série de TV. Como reproduzir os vários episódios por várias sessões de visualização, você atualizará a atividade para refletir a posição atual da série e vários registros de histórico serão criados.
  * **Filme** — um filme é uma peça única de conteúdo e deve ter seu próprio registro de histórico. Se o usuário parar de assistir a filmes parte vias por meio, é desejável para registrar sua posição. Quando quiserem retomá-lo no futuro, a atividade pode retomar o filme onde pararam, ou até mesmo perguntar ao usuário se ele desejam continuar ou iniciar no início.

## <a name="user-activity-design"></a>Design de atividade do usuário

Atividades do usuário consistem em três componentes: um URI de ativação, dados visuais e metadados de conteúdo.
* A ativação do URI é um URI que pode ser passado para um aplicativo ou experiência para retomar o aplicativo com um contexto específico. Normalmente, esses links assumem a forma de manipulador de protocolo para um esquema (por exemplo, "Meu-app://page2?action=edit"). Cabe ao desenvolvedor determinar como parâmetros URI serão realizados pelo seu aplicativo. Consulte [processar ativação de URI](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation) para obter mais informações.
* Os dados visual, consiste em um conjunto de propriedades necessárias e opcionais (por exemplo: título, descrição ou elementos de cartão adaptável), permitir que os usuários identificar visualmente uma atividade. Veja a seguir para obter diretrizes sobre como criar elementos visuais de cartão adaptável para sua atividade.
* Os metadados do conteúdo são dados JSON que podem ser usados para agrupar e recuperar as atividades em um contexto específico. Normalmente, isso assume a forma de http://schema.org dados. Veja a seguir para obter diretrizes sobre como preencher esses dados.

### <a name="adaptive-card-design-guidelines"></a>Diretrizes de design de cartão adaptáveis

Quando as atividades aparecem na linha do tempo, eles são exibidos usando a [estrutura de cartão adaptável](https://docs.microsoft.com/adaptive-cards/). Se o desenvolvedor não fornecer um cartão adaptável para cada atividade, linha do tempo criará automaticamente um cartão simple com base no nome/ícone do aplicativo, o campo obrigatório de título e o campo Descrição opcional. 

Os desenvolvedores de aplicativos são incentivados a fornecer cartões personalizados usando o esquema de JSON de cartão adaptável simples. Consulte a [documentação de cartões adaptáveis](https://docs.microsoft.com/adaptive-cards/authoring-cards/getting-started) para instruções técnicas sobre como construir objetos de cartão adaptável. Consulte as diretrizes abaixo para a criação de cartões adaptáveis em atividades do usuário.
* Usar imagens
  * Use uma imagem exclusiva para cada atividade, se possível. O nome do aplicativo e o ícone serão automaticamente exibidas ao lado do cartão da atividade; imagens adicionais ajudarão os usuários a localizar a atividade que estão procurando.
  * Imagens não devem incluir o texto que o usuário deve ler. Esse texto não estará disponível para os usuários com necessidades de acessibilidade e não pode ser pesquisado.
  * Se a imagem não conterá texto e pode ser cortada para sobre uma proporção de 2:1, você deve usá-lo como uma imagem de plano de fundo. Isso resulta em um cartão de atividade em negrito que será destacar na linha do tempo. A imagem será escurecida um pouco para garantir que o texto permaneça visível no cartão e você é incentivado a usar somente o nome da atividade nesse caso, como texto menor pode ficar difíceis de ler.
  * Se a imagem não pode ser cortada para 2:1, você deve colocá-la dentro o cartão de atividade.  
    * Se a taxa de proporção quadrada ou retrato, ancoragem a imagem à direita do cartão sem margens.
    * Se a taxa de proporção é paisagem, âncora a imagem para o canto superior direito do cartão.
* Cada atividade é necessária para fornecer um nome de atividade, que sempre deve ser mostrada.
  * Esse nome deve ser exibido no canto superior esquerdo do cartão usando a opção de texto em negrito grande. É importante que o nome seja facilmente reconhecido, como essa é a única parte usuários verão quando a atividade é mostrada em cenários da Cortana. Mostrar o mesmo nome na linha do tempo torna mais fácil para os usuários naveguem um grande número de atividades.
* Use o mesmo estilo visual para todas as atividades do seu aplicativo, para que os usuários possam localizar facilmente atividades do seu aplicativo na linha do tempo.
  * Por exemplo, atividades devem todos usam a mesma cor de plano de fundo.
* Use as informações de texto complementares com moderação. 
  * Evite preenchendo o cartão com texto e usar apenas as informações complementares que ajuda os usuários na localização a atividade direita ou reflete as informações de estado (por exemplo, o progresso atual em uma determinada tarefa).

### <a name="content-metadata-guidelines"></a>Diretrizes de metadados de conteúdo

Atividades do usuário também podem conter metadados de conteúdo, usam o Windows e a Cortana para categorizar atividades e gerar inferências. Atividades, em seguida, podem ser agrupadas em torno de um tópico específico, como um local (se o usuário está investigando férias), o objeto (se o usuário está investigando algo) ou a ação (se o usuário é comprar um produto específico em todos os sites e aplicativos diferentes). É uma boa ideia para representar os substantivos e verbos envolvidos em uma atividade. 

No exemplo a seguir, os metadados do conteúdo JSON, seguindo os padrões de [Schema.org](https://schema.org/), representam o cenário: "John jogado reuniões irritadas com Steve."

```json
// John played angry birds with Steve.
{
  "@context": "http://schema.org",
  "@type": "PlayAction",
  "agent": {
    "@type": "Person",
    "name": "John"
  },
  "object": {
    "@type": "MobileApplication",
    "name": "Angry Birds."
  },
  "participant": {
    "@type": "Person",
    "name": "Steve"
  }
}
```

## <a name="key-apis"></a>APIs-chave

* [Namespace UserActivities](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities)

## <a name="related-topics"></a>Tópicos relacionados

* [Atividades do usuário (documentos do projeto Roma)](https://docs.microsoft.com/windows/project-rome/user-activities/)
* [Cartões adaptáveis](https://docs.microsoft.com/adaptive-cards/)
* [Visualizador de cartões adaptáveis, amostras](http://adaptivecards.io/)
* [Manipular a ativação do URI](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
* [Envolvimento com seus clientes em qualquer plataforma usando o Microsoft Graph, Feed de atividades e cartões adaptáveis](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph/)
