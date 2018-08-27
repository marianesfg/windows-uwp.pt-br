---
author: PatrickFarley
title: Práticas recomendadas de atividades de usuário
description: Este guia descreve as práticas recomendadas para criação e atualização de atividades do usuário.
keywords: atividade do usuário, atividades do usuário, linha do tempo, cortana retorma de onde você parou, cortana retoma de onde eu parei, project rome
ms.author: pafarley
ms.date: 08/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: b11151df981a4b5ce233458581d21e405be9844c
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "2854979"
---
# <a name="user-activities-best-practices"></a>Práticas recomendadas de atividades de usuário

Este guia descreve as práticas recomendadas para criação e atualização de atividades do usuário. Para obter uma visão geral do recurso de atividades do usuário no Windows, consulte [continuar atividade do usuário, mesmo entre dispositivos](https://docs.microsoft.com/windows/uwp/launch-resume/useractivities). Ou, consulte a [seção de atividades do usuário](https://docs.microsoft.com/windows/project-rome/user-activities/) do Project Roma para as implementações de atividades em outras plataformas de desenvolvimento.

## <a name="when-to-create-or-update-user-activities"></a>Quando criar ou atualizar as atividades do usuário

Como cada aplicativo é diferente, cabe a cada desenvolvedor para determinar a melhor maneira de mapear ações dentro do aplicativo para as atividades do usuário. Suas atividades do usuário serão exibidas em Cortana e cronograma, concentram-se na eficiência e produtividade dos usuários crescente, ajudando-os voltar ao conteúdo que eles visitaram no passado.

**Diretrizes gerais**

* **Registre uma única atividade para um grupo de ações do usuário relacionados.** Isso é especialmente relevante para listas de reprodução de música ou TV mostra: uma única atividade pode ser atualizada em intervalos regulares para refletir o progresso do usuário. Nesse caso, você terá uma única atividade do usuário com vários itens de histórico representando os períodos de compromisso em vários dias ou semanas. O mesmo se aplica às atividades de documento baseado em que o usuário fizer gradual andamento dentro de seu aplicativo.
* **Armazenar dados de usuário na nuvem.** Se você deseja oferecer suporte às atividades de referência cruzada de dispositivo, você precisará certificar-se de que o conteúdo necessário para participar novamente essa atividade está armazenado em um local de nuvem. Atividades específicas de dispositivo serão exibido na linha do tempo no dispositivo em que a atividade foi criada, mas podem não aparecer em outros dispositivos.
* **Não crie atividades para ações que os usuários não precisarão retomar.** Se seu aplicativo for usado para concluir as operações únicas e simples, que não persiste de status, você provavelmente não precisará criar uma atividade do usuário.
* **Não crie atividades para Ações concluídas por outros usuários.** Se uma conta externa envia ao usuário uma mensagem ou @-mentions -los em seu aplicativo, você não deve criar uma atividade para que isso. Esse tipo de ação é atendido por notificações de centro de ação.
  * Cenários de colaboração são uma exceção: se vários usuários estiverem funcionando na mesma atividade juntas (por exemplo, um documento do Word), haverá casos em que outro usuário fez alterações depois que o usuário. Nesse caso, convém atualizar a atividade existente para refletir as alterações feitas ao documento. Isso envolveria atualizando os dados de conteúdo de atividade do usuário existentes sem criar um novo Item de histórico.

**Diretrizes para tipos específicos de aplicativos**

Enquanto todos os aplicativos são diferentes, a maioria dos aplicativos se encaixam em uma das seguintes padrões de interação.
* **Aplicativos baseados no documento** — criar uma atividade por documento, com um ou mais itens de histórico refletindo os períodos de uso. É importante atualizar sua atividade, como as alterações são feitas ao documento.
* **Jogos** — criar uma atividade para cada jogo salvar ou do mundo. Se seu jogo suporta apenas uma única sequência dos níveis, você pode publicar novamente a mesma atividade ao longo do tempo, embora talvez você deseje atualizar os dados de conteúdo para mostrar as últimas realizações ou progresso.
* **Utilitário apps** — se não há nada dentro de seu aplicativo que os usuários precisarão sair e continuar, você não precisará usar as atividades do usuário. Um bom exemplo é um aplicativo simple como calculadora.
* **Aplicativos de linha de negócios** — muitos aplicativos existirem para gerenciar tarefas simples ou os fluxos de trabalho. Criar uma atividade para cada fluxo de trabalho separado, acessada por meio do seu aplicativo (por exemplo, relatórios de despesas cada seria uma atividade separada, para que o usuário poderia, em seguida, clicar em uma atividade para ver se um determinado relatório foi aprovado).
* **Aplicativos de reprodução de mídia** — criar uma atividade por um agrupamento lógico de conteúdo (por exemplo, um conteúdo de lista de reprodução, programa ou autônomo). A pergunta subjacente para desenvolvedores de aplicativos é se uma cada parte do conteúdo (episódio TV, música) contagens como conteúdo autônomos ou parte de uma coleção. Como regra geral, se o usuário optar para reproduzir um conjunto ou conteúdo sequencial, a coleção como um todo é a atividade. Se optar por eles para reproduzir um único item de conteúdo, que uma peça do conteúdo é a atividade. Consulte mais específicas diretrizes abaixo.
  * **Música: álbum/artista/gênero** — se o usuário seleciona um álbum, artista ou gênero e visitas **Reproduzir**, essa coleção é a atividade; Não escreva uma atividade separada para cada música. Para conjuntos de curtos como um único álbum ou conjuntos que está sendo reproduzidos em ordem aleatória, você não pode precisar atualizar a atividade para refletir a posição do usuário atual. Para reprodução sequencial longa, como um álbum ou uma lista de reprodução, a gravação de sua posição dentro do álbum pode fazer sentido.
  * **Música: inteligente de listas de reprodução** — aplicativos que tocar música em ordem aleatória devem registrar uma única atividade para essa lista de reprodução. Se o usuário reproduz a lista de reprodução de uma segunda vez, você deve criar registros do histórico adicional para a mesma atividade. A posição atual do usuário na lista de reprodução de gravação não é necessária porque a ordenação é aleatória.
  * **Série de TV** — se seu aplicativo estiver configurado para reproduzir o próximo episódio depois atual for concluída, você deve gravar uma única atividade para a série de TV. Conforme você reproduzir os vários episódios em várias sessões de visualização, você vai atualizar sua atividade para refletir a posição atual na série e vários registros de histórico serão criados.
  * **Filme** — um filme é um único item de conteúdo e deve ter seu próprio registro de histórico. Se o usuário parar assistindo a maneira de parte do filme através de, é desejável para registrar sua posição. Quando desejam retomá-lo no futuro, a atividade pode retomar a execução do filme onde eles pararam ou, até mesmo, peça que o usuário se desejam retomar ou iniciar no início.

## <a name="user-activity-design"></a>Design de atividade do usuário

As atividades do usuário consistem em três componentes: um URI de ativação, visuais dados e metadados do conteúdo.
* A ativação do URI é um URI que pode ser passado para um aplicativo ou a experiência para retomar o aplicativo com um contexto específico. Em geral, esses links assumem a forma de manipulador de protocolo para um esquema (por exemplo, "Meu-app://page2?action=edit"). É até o desenvolvedor determine como parâmetros URI serão tratados pelo seu aplicativo. Consulte a [ativação de lidar com o URI](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation) para obter mais informações.
* Os dados de visuais, consistindo em um conjunto de propriedades obrigatórios e opcionais (por exemplo: título, descrição ou elementos de cartão adaptável), permitir que os usuários identificar visualmente uma atividade. Veja abaixo para obter diretrizes sobre como criar elementos visuais do cartão adaptável para sua atividade.
* Os metadados do conteúdo são dados JSON que podem ser usados para agrupar e recuperar atividades em um contexto específico. Normalmente, isso assume a forma de http://schema.org dados. Veja abaixo para obter diretrizes sobre como preencher esses dados.

### <a name="adaptive-card-design-guidelines"></a>Diretrizes de design de cartão adaptáveis

Quando atividades aparecerem na linha do tempo, eles são exibidos usando o [cartão adaptável framework](https://docs.microsoft.com/adaptive-cards/). Se o desenvolvedor não ofereça uma placa adaptável para cada atividade, o cronograma criará automaticamente um cartão simple com base no campo Descrição opcional, necessário campo título e o ícone da nome app. 

Desenvolvedores de aplicativos são incentivados a fornecer cartões personalizados usando o esquema de JSON de cartão adaptável simple. Consulte a [documentação de cartões adaptável](https://docs.microsoft.com/adaptive-cards/authoring-cards/getting-started) para técnica instruções sobre como criar objetos de cartão adaptável. Consulte as diretrizes abaixo para projetar cartões adaptável em atividades do usuário.
* Use imagens
  * Use uma imagem exclusiva para cada atividade, se possível. Seu nome de aplicativo e o ícone automaticamente serão exibidos ao lado da placa da sua atividade; imagens adicionais ajudarão os usuários a localizar a atividade que estão procurando.
  * Imagens não devem incluir o texto que o usuário espera-se para ler. Esse texto não estarão disponível para usuários com necessidades de acessibilidade e não pode ser pesquisado.
  * Se a imagem não contém texto e pode ser cortada para sobre uma proporção de 2:1, você deve usá-lo como uma imagem de plano de fundo. Isso resulta em um cartão de atividade negrito que destacará na linha do tempo. A imagem será escurecida ligeiramente para garantir que o texto permanece visível no cartão, e você é incentivados a usar somente o nome da atividade nesse caso, como texto menor pode se tornar difícil de ler.
  * Se a imagem não pode ser cortada como 2:1, você deverá colocá-lo dentro da placa de atividade.  
    * Se a taxa de proporção for quadrada ou retrato, ancore a imagem no lado direito do cartão sem margens.
    * Se a taxa de proporção é paisagem, âncora a imagem ao canto superior direito do cartão.
* Cada atividade é necessário fornecer um nome de atividade, que sempre devem ser exibidas.
  * Esse nome deve ser exibido no canto superior esquerdo da placa usando a opção de grandes de texto em negrito. É importante que o nome é facilmente reconhecível, pois essa é a única parte usuários verão quando a atividade é mostrada em cenários de Cortana. Mostrar o mesmo nome na linha do tempo torna mais fácil para os usuários naveguem um grande número de atividades.
* Use o mesmo estilo visual para todas as atividades do seu aplicativo, para que os usuários possam facilmente localizar atividades do seu aplicativo na linha do tempo.
  * Por exemplo, as atividades devem todos usar a mesma cor de plano de fundo.
* Use as informações de texto complementares esporadicamente. 
  * Evite preenchendo o cartão com texto e use apenas informações complementares que auxilia usuários encontrar a atividade direita ou refletem as informações de estado (por exemplo, o progresso atual em uma determinada tarefa).

### <a name="content-metadata-guidelines"></a>Diretrizes de metadados do conteúdo

As atividades do usuário também podem conter metadados de conteúdo, Windows e Cortana usam para categorizar atividades e gerar inferências. Atividades, em seguida, podem ser agrupadas em torno de um tópico específico, como um local (se o usuário está pesquisando férias), o objeto (se o usuário estiver pesquisando algo) ou a ação (se o usuário é de compras para um produto específico entre sites e aplicativos diferentes). É uma boa ideia para representar os substantivos e os verbos envolvidos em uma atividade. 

No exemplo a seguir, os metadados do conteúdo JSON, seguindo os padrões de [Schema.org](https://schema.org/), representam o cenário: "John reproduzida pássaros irritados Steve."

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

* [Atividades do usuário (docs Roma Project)](https://docs.microsoft.com/windows/project-rome/user-activities/)
* [Cartões adaptáveis](https://docs.microsoft.com/adaptive-cards/)
* [Visualizador de cartões adaptáveis, amostras](http://adaptivecards.io/)
* [Manipular a ativação do URI](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
* [Envolvimento com seus clientes em qualquer plataforma usando o Microsoft Graph, Feed de atividades e cartões adaptáveis](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph/)
