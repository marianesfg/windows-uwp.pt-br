---
title: Configurando a pesquisa Contextual
description: Saiba como configurar a pesquisa contextual para marcar clipes de jogos e transmissões.
ms.assetid: 6cb2cb10-811a-4b20-9b9b-a3fc59a033c2
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, configuração de serviço, pesquisa contextual, clipes de jogos, de difusão
ms.localizationpriority: medium
ms.openlocfilehash: c8c78f115a160c82a28881a3c551a958cdf4b68c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607921"
---
# <a name="configuring-contextual-search"></a>Configurando a pesquisa Contextual

## <a name="configuration-info"></a>Informações de configuração

### <a name="designing-your-data"></a>Projetar seus dados
Ao configurar o título para pesquisa Contextual, é importante pensar em coisas que tornar seu título exclusivo.  Se um usuário está procurando o conteúdo de vídeo, o que seria prioritários para procurar?  O nível de granularidade que os dados precisa ser?

Pesquisa contextual suporta a capacidade de filtrar e classificar conteúdo, para que seus metadados devem refletir essas construções de pesquisa para garantir uma ótima cobertura.  Uma boa prática regras é pensar em dados do seu jogo em dois pontos:
1. Estado - por exemplo, de jogos.  Nível de caractere, armas, mapa, função, o ponto de verificação/história, DLC
2. Habilidade de usuário - por exemplo. Taxa de K/D, prestígio, classificação, velocidade de tempo de execução

O primeiro pivot é ótimo para filtragem de conteúdo, o segundo é ótimo para classificação.  Além disso, dependendo do tipo de conteúdo que está sendo exposto, níveis variados de granularidade stat são importantes.  Para difusões, supondo que haja um número limitado de difusões Active Directory em um determinado momento, os dados de granularidade inferior garantirá uma alta probabilidade de uma pesquisa bem-sucedida.  Clipes de jogos, aumenta o número continuamente, portanto, os dados mais refinados de granularidade garantirá que os resultados mais precisos.

É muito provável que você já tiver esses dados criados para recursos do Xbox Live, como a presença avançada ou Hero estatísticas, portanto, esta é uma ótima oportunidade para aproveitar já concluíram o trabalho.

Se você estiver usando estatísticas para filtrar o conteúdo, lembre-se de que todas as estatísticas de pesquisa Contextual estão disponíveis para consultar a qualquer momento.  Você deve projetar seus eventos adequadamente para dar suporte a isso.

Por exemplo, se você estiver usando estatísticas como SinglePlayerMap e MultiplayerMap para filtrar o conteúdo, o jogador só será em um ao mesmo tempo.  No entanto, os dois valores serão disponíveis para consulta do serviço a qualquer momento.  É importante que, como você define uma, você desmarque as outras.  Para estatísticas de cadeia de caracteres com base em uma cadeia de caracteres vazia é ótima (Lembre-se de incluir que em sua configuração de interface do usuário como uma opção).

### <a name="configuring-a-stat-for-contextual-search"></a>Configurando uma estatística para pesquisa Contextual
Configurar o título para pesquisa Contextual é fácil depois que você configurou os eventos e as estatísticas que ativam a marcação.  Consulte outras documentações XDP ou Partner Center existente sobre como configurar pesquisa Contextual, se você não ainda estiver familiarizado.

![](../images/contextual_search/config02.png)

A imagem acima é um exemplo da página principal depois que você clicar no bloco pesquisa Contextual na parte de configuração de serviço principal de XDP.

Mesmo se isso for a primeira vez que você visitou nesta página, é possível que você verá estatísticas já configuradas para pesquisa Contextual.  Isso ocorre porque todas as estatísticas de seu herói são configuradas automaticamente para pesquisa Contextual. Você não precisa manter essas estatísticas de herói em sua configuração, mas eles são geralmente excelente para classificar (e já feito para você).

Atualmente, o número máximo de instâncias stat, que você pode configurar para pesquisa Contextual é 10.

Nessa página, cada uma de estatísticas é representada da seguinte maneira: Prioridade – o nome de exibição (nome da instância Stat)

Cada um deles será explicada mais detalhadamente na próxima seção.

![](../images/contextual_search/config01.png)

A imagem acima é a tela que será exibido se você optar por criar uma nova pesquisa Contextual stat ou editar e um existente.

Para configurar com êxito um stat para pesquisa Contextual, conclua as seguintes etapas:
1. Escolha sua instância de stat.

  ![](../images/contextual_search/config03.png)

  Observe que stat somente há suporte para instâncias - stat modelos não são aceitas.  Você também deve estar ciente da visibilidade que você definiu para a instância stat (configurada na parte de estatísticas de XDP ou Partner Center).  Somente as estatísticas que são marcadas como **abrir** aparecerá nas experiências de terceiros.

2. Escolha sua prioridade de stat. Essa é uma maneira para delimitar a importância desta estatística em relação a outras pessoas para experiências/algoritmos de pesquisa.  Os valores aceitáveis são 1 a 10 (1 é a mais alta).  Deixe como 0 ou deixe em branco para que isso seja ignorado.
3. Adicione seu nome de exibição.  Isso é uma cadeia de caracteres localizável que é exposta ao usuário final.
4. Verifique se você usará esta estatística para filtrar ou classificar os resultados.  Você pode verificar tanto em determinadas instâncias (se o valor da estatística é um número - preferencialmente em um intervalo definido pelo).
5. Escolha como seu valor stat é representado.  No caso de você tem 3 opções.
   * Valor bruto – o stat é representado como está e não tem nenhum requisito de intervalo.  Isso é melhor usado para classificação.
   * Defina - os valores são mapeados para cadeias de caracteres localizáveis individuais.  Isso é melhor usado para filtragem.
   * Intervalo - os valores se enquadram dentro de um mínimo e máximo do intervalo.  Esse tipo é ótimo se você deseja filtrar e classificar a partir desses dados.

#### <a name="explaining-sets"></a>Explicando conjuntos
Se você não estiver familiarizado com os conjuntos, eles são uma maneira para mapear valores de stat potencias para cadeias de caracteres localizáveis que podem ser expostas a um usuário final.  Eles são muito importantes para as estatísticas de pesquisa Contextual que você deseja filtrar.

{% bruto %} Como um exemplo, digamos que eu tenha um número inteiro denominado Stat SinglePlayerMap.  Mostrar um monte de números para os usuários finais não faz sentido.  Portanto, em vez disso, posso criar um conjunto de mapeamentos como ```{{0, “Blood Gulch”}, {1, “Lockout”}, {2, “Zanzibar”}}```.  O usuário verá a cadeia de caracteres, mas podemos usar o número como parte da consulta de pesquisa a pesquisa Contextual.  Você pode mapear inteiros ou cadeias de caracteres para cadeias de caracteres localizadas em conjuntos.
{% endraw %}

### <a name="updating-your-contextual-search-document"></a>Atualizar seu documento de pesquisa Contextual
No momento, você pode alterar facilmente as estatísticas em seu documento de pesquisa Contextual como só há suporte para difusão no momento (que é dados voláteis).  Depois que começamos dar suporte à marcação (setembro de 2015), se você decidiu adicionar ou Remover estatísticas de clipe de jogo, você será afetando clipes que já tenham sido indexadas.  Se você adicionar um novo estado, clipes apenas jogos e difusões criados depois que a alteração entrará em vigor serão marcados e expostos em consultas usando a nova estatística. Clipes de jogos antigos não será capaz de executar consultas desta estatística. Se você remover um stat do documento de pesquisa Contextual, o antigo stat anexado ao jogo clipe será inútil e não será retornado.
