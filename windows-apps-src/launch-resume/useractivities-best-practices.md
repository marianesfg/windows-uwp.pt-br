---
title: Práticas recomendadas de atividades de usuários
description: Este guia descreve as práticas recomendadas para criação e atualização de atividades do usuário.
keywords: atividade do usuário, atividades do usuário, linha do tempo, cortana retorma de onde você parou, cortana retoma de onde eu parei, project rome
ms.date: 08/23/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e98f5d73cf2d1afb26a823ed417c8980d118485c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589901"
---
# <a name="user-activities-best-practices"></a>Práticas recomendadas de atividades de usuários

Este guia descreve as práticas recomendadas para criação e atualização de atividades do usuário. Para uma visão geral do recurso de atividades do usuário no Windows, consulte [continuar a atividade do usuário, mesmo entre dispositivos](https://docs.microsoft.com/windows/uwp/launch-resume/useractivities). Ou, consulte a [seção de atividades do usuário](https://docs.microsoft.com/windows/project-rome/user-activities/) do projeto Roma para as implementações de atividades em outras plataformas de desenvolvimento.

## <a name="when-to-create-or-update-user-activities"></a>Quando criar ou atualizar as atividades do usuário

Como cada aplicativo é diferente, cabe a cada desenvolvedor para determinar a melhor maneira de mapear ações dentro do aplicativo para as atividades do usuário. Atividades de usuário serão exibidas no Cortana e linha do tempo, que concentram-se em eficiência e produtividade dos usuários de cada vez maior, ajudando-os voltar para o conteúdo que visitaram no passado.

**Diretrizes gerais**

* **Registre uma única atividade para um grupo de ações do usuário relacionados.** Isso é especialmente relevante para listas de reprodução de músicas ou programas de TV: uma única atividade pode ser atualizada em intervalos regulares para refletir o progresso do usuário. Nesse caso, você terá uma única atividade de usuário com vários itens de histórico que representam períodos de compromisso em vários dias ou semanas. O mesmo se aplica às atividades baseadas em documentos em que o usuário progride gradual dentro de seu aplicativo.
* **Store dados do usuário na nuvem.** Se você quiser oferecer suporte a atividades de vários dispositivos, você precisará certificar-se de que o conteúdo necessário para envolver novamente essa atividade é armazenado em um local de nuvem. Atividades específicas de dispositivo aparecerão na linha do tempo no dispositivo em que a atividade foi criada, mas não podem aparecer em outros dispositivos.
* **Não crie atividades para ações que os usuários não precisarão retomar.** Se seu aplicativo é usado para concluir operações simples e únicas que não permanecem status, você provavelmente não precisará criar uma atividade de usuário.
* **Não crie atividades para Ações concluídas por outros usuários.** Se uma conta externa envia ao usuário uma mensagem ou @-mentions -los dentro de seu aplicativo, você não deve criar uma atividade para isso. Esse tipo de ação é mais bem atendido por notificações da Central de ações.
  * Cenários de colaboração são uma exceção: Se vários usuários estão trabalhando na mesma atividade juntos (como um documento do Word), haverá casos em que outro usuário tiver feito alterações depois que o usuário. Nesse caso, convém atualizar a atividade existente para refletir as alterações foram feitas no documento. Isso envolveria atualizando os dados de conteúdo de atividade de usuário existentes sem criar um novo Item de histórico.

**Diretrizes para tipos específicos de aplicativos**

Embora cada aplicativo é diferente, a maioria dos aplicativos se encaixam em um dos seguintes padrões de interação.
* **Aplicativos baseados em documentos** — criar uma atividade por documento, com um ou mais itens de histórico, refletindo os períodos de uso. É importante atualizar sua atividade, como as alterações são feitas no documento.
* **Jogos** — criar uma atividade para cada jogo salvar ou o mundo. Se o seu jogo dá suporte a apenas uma única sequência de níveis, você pode publicar novamente a mesma atividade ao longo do tempo, embora talvez você queira atualizar os dados de conteúdo para mostrar o progresso ou realizações a mais recente.
* **Aplicativos de utilitário** — se não houver nada dentro de seu aplicativo que os usuários precisarão sair e reiniciar, você não precisa usar as atividades do usuário. Um bom exemplo é um aplicativo simples, como a Calculadora.
* **Aplicativos de linha de negócios** — muitos aplicativos existem para o gerenciamento de tarefas simples ou fluxos de trabalho. Criar uma atividade para cada fluxo de trabalho separado, acessado por meio de seu aplicativo (por exemplo, relatórios de despesas cada seria uma atividade separada, para que o usuário pode, em seguida, clique em uma atividade para ver se um relatório específico foi aprovado).
* **Aplicativos de reprodução de mídia** — criar uma atividade por agrupamento lógico de conteúdo (como conteúdo de uma lista de reprodução, o programa ou autônomo). A pergunta subjacente para desenvolvedores de aplicativos é se uma cada parte do conteúdo (episódio de TV, música) contará como conteúdo autônomo ou parte de uma coleção. Como regra geral, se o usuário opta por executar uma coleção ou conteúdo sequencial, a coleção como um todo é a atividade. Se optar por eles desempenham uma única parte do conteúdo, que uma peça de conteúdo é a atividade. Consulte as diretrizes mais específicas abaixo.
  * **Música: Artista/álbum/gênero** — se o usuário seleciona um álbum, artista, ou gênero e ocorrências **reproduzir**, essa coleção é a atividade; não escrever uma atividade separada para cada música. Para coleções curtas como um único álbum ou coleções que está sendo reproduzidas em uma ordem aleatória, talvez não precise atualizar a atividade para refletir a posição do usuário atual. Para reprodução longa sequencial, como um álbum ou uma lista de reprodução, gravar sua posição dentro do álbum pode fazer sentido.
  * **Música: inteligente listas de reprodução** — aplicativos que reproduzir música em ordem aleatória devem gravar uma única atividade para essa lista de reprodução. Se o usuário desempenha a lista de reprodução em uma segunda vez, você precisará criar registros de histórico adicional para a mesma atividade. Gravar a posição atual do usuário na lista de reprodução não é necessário porque a ordenação é aleatória.
  * **Série de TV** — se seu aplicativo estiver configurado para reproduzir próximo episódio, depois que o atual for concluída, você deve escrever uma única atividade para a série de TV. Conforme você os executa vários episódios em várias sessões de exibição, você atualizará sua atividade para refletir a posição atual na série e vários registros de histórico serão criados.
  * **Filme** — é uma única parte do conteúdo de um filme e deve ter seu próprio registro de histórico. Se o usuário para assistir a filmes parte-até, é desejável para registrar sua posição. Quando eles desejam retomá-lo no futuro, a atividade pode retomar o filme de onde parou, ou até mesmo pedir ao usuário se eles desejam retomar ou começar do início.

## <a name="user-activity-design"></a>Design de atividade do usuário

As atividades de usuário consistem em três componentes: um URI de ativação, visual de dados e metadados de conteúdo.
* A ativação URI é um URI que pode ser passado para um aplicativo ou a experiência para retomar o aplicativo com um contexto específico. Normalmente, esses links assumem a forma de manipulador de protocolo para um esquema (por exemplo, "Meu-app://page2?action=edit"). Cabe ao desenvolvedor determinar como os parâmetros do URI serão tratados pelo seu aplicativo. Ver [ativação de lidar com URI](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation) para obter mais informações.
* Os dados de visual, que consiste de um conjunto de propriedades obrigatórias e opcionais (por exemplo: título, descrição ou elementos de cartão adaptável), permitir que os usuários identificar visualmente uma atividade. Consulte abaixo para obter diretrizes sobre como criar visuais do cartão adaptável para sua atividade.
* Os metadados do conteúdo são dados JSON que podem ser usados para agrupar e recuperar as atividades em um contexto específico. Normalmente, isso assume a forma de http://schema.org dados. Consulte abaixo para obter diretrizes sobre como preencher esses dados.

### <a name="adaptive-card-design-guidelines"></a>Diretrizes de design de cartão adaptáveis

Quando as atividades são exibidos na linha do tempo, eles são exibidos usando o [framework cartão adaptável](https://docs.microsoft.com/adaptive-cards/). Se o desenvolvedor não fornecer um cartão adaptável para cada atividade, a linha do tempo criará automaticamente um cartão simple com base no nome/ícone do aplicativo, o campo de título obrigatório e o campo de descrição opcional. 

Os desenvolvedores de aplicativos são incentivados a fornecer cartões personalizados usando o esquema JSON de cartão adaptável simple. Consulte a [documentação de cartões adaptáveis](https://docs.microsoft.com/adaptive-cards/authoring-cards/getting-started) para instruções técnicas sobre como construir objetos do cartão adaptável. Consulte as diretrizes abaixo para criar os cartões adaptáveis em atividades do usuário.
* Usar imagens
  * Use uma imagem exclusiva para cada atividade, se possível. O nome do aplicativo e o ícone serão automaticamente exibidas ao lado do cartão da sua atividade; imagens adicionais ajudarão os usuários a localizar a atividade que está procurando.
  * Imagens não devem incluir o texto que o usuário deve ler. Esse texto não estará disponível para usuários com necessidades de acessibilidade e não pode ser pesquisado.
  * Se a imagem não contém o texto e pode ser recortada para sobre uma proporção de 2:1, você deve usá-lo como uma imagem de plano de fundo. Isso resulta em um cartão de atividade em negrito que destacará na linha do tempo. A imagem será escurecida ligeiramente para garantir que o texto permanece visível no cartão, e você é incentivado a usar apenas o nome da atividade nesse caso, como texto menor pode se tornar difícil de ler.
  * Se a imagem não pode ser recortada para 2:1, você deve colocá-lo dentro do cartão de atividade.  
    * Se a taxa de proporção é quadrado ou retrato, a imagem à direita do cartão sem margens de ancoragem.
    * Se a taxa de proporção é paisagem, a imagem para o canto superior direito da placa de âncora.
* Cada atividade é necessário para fornecer um nome de atividade, que sempre devem ser mostrados.
  * Esse nome deve ser exibido no canto superior esquerdo do cartão usando a opção de texto em negrito grandes. É importante que o nome seja facilmente reconhecível, pois essa é a única parte de usuários serão exibida quando a atividade é mostrada em cenários da Cortana. Mostrar o mesmo nome na linha do tempo torna mais fácil para os usuários procurem um grande número de atividades.
* Use o mesmo estilo visual para todas as atividades em seu aplicativo, para que os usuários possam facilmente localizar atividades do seu aplicativo na linha do tempo.
  * Por exemplo, as atividades devem todos usar a mesma cor do plano de fundo.
* Use as informações de texto suplementar com moderação. 
  * Evite preenchendo o cartão com texto e usar apenas informações complementares que ajuda os usuários a encontrar a atividade correta ou refletem as informações de estado (por exemplo, o progresso atual em uma tarefa específica).

### <a name="content-metadata-guidelines"></a>Diretrizes de metadados de conteúdo

Atividades do usuário também podem conter metadados de conteúdo, usam o Windows e da Cortana para categorizar as atividades e gerar inferências. As atividades, em seguida, podem ser agrupadas em torno de um tópico específico, como um local (se o usuário seja pesquisando férias), o objeto (se o usuário seja pesquisando algo) ou a ação (se o usuário é de obtenção de um produto específico em sites e aplicativos diferentes). É uma boa ideia para representar os substantivos e verbos envolvidos em uma atividade. 

No exemplo a seguir, os metadados do conteúdo JSON, seguindo os padrões de mercado [Schema.org](https://schema.org/), representa o cenário: "John jogado pássaros irritado com Steve."

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
* [Visualizador de cartões adaptáveis, exemplos](https://adaptivecards.io/)
* [Manipular a ativação de URI](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
* [Interação com seus clientes em qualquer plataforma usando o Microsoft Graph, o Feed de atividades e os cartões adaptáveis](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph/)
