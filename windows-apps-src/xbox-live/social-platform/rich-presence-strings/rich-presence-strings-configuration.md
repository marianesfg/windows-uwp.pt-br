---
title: Configuração de presença avançada
description: Saiba como configurar cadeias de caracteres de presença de Rich do Xbox Live.
ms.assetid: 7b08d46d-d3aa-42eb-93f2-ecd9338fccea
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox, recursos avançados de presença
ms.localizationpriority: medium
ms.openlocfilehash: d69aaadaa1f64d1cb6eb40b52016a330d924b9b9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590471"
---
# <a name="rich-presence-configuration"></a>Configuração de presença avançada

Haverá uma parte do Xbox Developer Portal (XDP) dedicados à configuração de cadeias de caracteres de presença avançada. Até que o que está disponível em XDP, configuração manual deve ser usada. Configuração manual significa definir as cadeias de caracteres em um modelo de planilha fornecido pelo seu gerente de conta de desenvolvedor e enviá-la para seu gerente de conta de desenvolvedor para a ingestão no ambiente.

-   **Cadeias de caracteres**
-   **Enumerações**
-   **Ingestão**
-   **Exemplo de jogo**
-   **Configurando as estatísticas de presença avançada**
-   **Exemplo de configuração de enumeração**
-   **Exemplo de configuração de cadeia de caracteres**


## <a name="strings"></a>Cadeias de caracteres

Para suas cadeias de caracteres de presença avançada, você deve definir os conjuntos de cadeia de caracteres. Cada conjunto de cadeia de caracteres deve ser fornecido um nome amigável a ser usado como um identificador. Cada conjunto de cadeia de caracteres criado deve conter um grupo de cadeias de caracteres localizadas adequadamente para as localidades em que o título será lançado. Além de cadeias de caracteres localizadas, uma cadeia de caracteres com neutralidade de cultura também deve ser fornecida. Essa cadeia de caracteres será usada se uma solicitação feita para um local que não é fornecido.

Cada conjunto de cadeia de caracteres também pode fazer usar dados de jogos de por parametrizar as cadeias de caracteres para melhorar ainda mais-los. Um conjunto de cadeia de caracteres pode ter tantos parâmetros como desejar, desde que as cadeias de caracteres resultantes não passam pela limitações de caracteres. Consulte [cadeias de caracteres de presença avançada: As políticas e as limitações](rich-presence-strings-policies-and-limitations.md) para obter mais detalhes.


## <a name="enumerations"></a>Enumerações

As enumerações descritas abaixo são definidas por meio de eventos da plataforma de dados.


## <a name="statistics"></a>Estatísticas

Você pode usar valores de estatística numéricos em cadeias de caracteres de presença avançada. Por exemplo, em um shooter, talvez você queira a cadeia de caracteres de presença avançada para mostrar quantos acaba com o player ficou até o momento. A configuração precisa apenas Mostrar que o parâmetro deve ser substituído pela estatística "Kill contagem". A estatística "Kill contagem" foi criada por meio do sistema por meio de eventos de esquema comuns para a plataforma de dados e deve ser definida por meio da configuração de estatísticas. Em seguida, quando a cadeia de caracteres é lida por outro usuário, o serviço acesse e recuperar o valor mais atualizado para a estatística de "Contagem de eliminar", permitindo que a cadeia de caracteres esteja sempre atualizada.


## <a name="state"></a>Estado

Você também pode optar por usar as estatísticas que representam valores de estado real em suas cadeias de caracteres de presença avançada. Por exemplo, em um jogo de corrida, você pode usar uma cadeia de caracteres de presença avançada para mostrar qual carro que o jogador está orientando no momento, ou quais mapa ou a controlar o jogador está disputando no. Há duas etapas para configurar informações de estado:

1.  Para obter informações de estado, defina enumerações para qualquer cadeia de caracteres no jogo. Por exemplo, se você tiver cinco mapas no seu jogo e deseja que os mapas de fazer parte de suas cadeias de caracteres de presença avançada, em seguida, os mapas de cinco precisam ser enumerados em cadeias de caracteres localizadas também.
2.  Vincule as enumerações para estatísticas. Aqui, novamente, o serviço irá recuperar o valor mais recente da plataforma de dados pesquisando o valor da estatística. Ele então determina qual valor de enumeração a estatística se traduz em.


## <a name="ingestion"></a>Ingestão

Depois de concluir sua planilha, enviá-lo ao seu gerente de conta de desenvolvedor para a ingestão. A equipe de presença avançada examinar sua configuração e verifique se ele está configurado corretamente e, em seguida, ele tenha introduzido no serviço.


## <a name="example-game"></a>Exemplo de jogo

Para o restante do exemplo, digamos que eu estou trabalhando no início de jogo de bucket de mais recente. Há muitos tipos diferentes de buckets no meu jogo, como madeira buckets, buckets prata e ouro buckets. Posso optar por criar uma estatística para cada tipo de bucket iniciado. Também há locais diferentes no meu jogo no qual iniciar os buckets, como montanhas, o Deserto e praia. Depois que um bucket é iniciado, ele elimina as, portanto, você precisa sempre procurar mais buckets. Alguns buckets exigem que você tenha engrenagem especial (é necessário um forte o suficiente para suportar um lançamento de inicialização) para iniciar com êxito o bucket. O jogo inclui um modo de vários jogadores também, portanto se suficiente participantes estiver tentando iniciar o bucket ao mesmo tempo, você não precisa de inicialização apropriado.

Vamos dar uma olhada no qual configuração de cadeia de caracteres de presença avançada é semelhante para esse jogo.


## <a name="configuring-statistics-for-rich-presence"></a>Configurando as estatísticas de presença avançada

Para usar uma estatística gerada pela plataforma de dados em suas cadeias de caracteres de presença avançada, o serviço precisa conhecer os nomes das estatísticas. Você deve incluir essas informações na seção do modelo de planilha que pede que você especifique quais estatísticas que você está usando em suas cadeias de caracteres.


## <a name="enumeration-configuration-example"></a>Exemplo de configuração de enumeração

Aqui está um exemplo de definição de algumas das enumerações. Primeiro, criamos uma enumeração para os mapas no jogo. O jogo tem três mapas, o qual obtém um nome amigável para que podemos consultá-las facilmente. Em seguida, para cada localidade, podemos criar uma cadeia de caracteres localizada para esse nome de mapa. Observe que há uma padrão geral de localização como também uma cultura neutra.

Em seguida, vamos examinar as armas no jogo. Há três armas em meu jogo, onde eles nomes amigáveis e cadeias de caracteres localizadas. Fazemos isso para todas as enumerações.

Enumeração | Relacionadas de estatística | Nome Amigável | Locale | String
----------- | ----------------- | ------------- | ------ | ----
Mapa | CurrentMap | Map_Mountains | padrão | Montanhas
 |  |  |  | pt | Montanhas
 |  |  |  | en-US | Montanhas
 |  |  |  | en-GB | Montanhas
 |  |  |  |  de | Gebirge
 | |  |  | etc |
 | |  | Map_Desert | padrão | Deserto
 |  ||  |  pt | Deserto
 |  ||  |  en-US | Deserto
 |  ||  |   en-GB | Deserto
 |  ||  |  de | Wuste
 |  ||  |  etc |
| |  |  Map_Beach | padrão | Praia
 ||    |  | pt | Praia
 ||    |  | en-US | Praia
 ||    |  | en-GB | Praia
 ||    |  | de | Strand
 | |   |  | etc |
Inicialização | CurrentWeapon | Boot_Light | padrão | Claro
 |  ||  |  pt | Claro
 |  ||  |   en-US | Claro
 |  ||  |   en-GB | Claro
 |  ||  |   de | Leicht
 |  ||  |   etc  |
 |  | |  Boot_Medium | padrão | Médio
 |  |  |  | pt | Médio
 |  |  |  | en-US | Médio
 |  |  |  | en-GB | Médio
 |  |  |  | de | Mittel
 |  |  |  | etc |
 |  | |  Boot_Strong | padrão | Forte
 |  |  |  | pt | Forte
 |  |  |  | en-US | Forte
 |  |  |  | en-GB | Forte
 |  |  |  | de | Gritante
 |  ||  | etc

## <a name="string-configuration-example"></a>Exemplo de configuração de cadeia de caracteres

Agora que as enumerações e as estatísticas tiverem sido definidas, você pode criar suas cadeias de caracteres.

Nesta tabela de exemplo abaixo, a primeira coluna é um nome amigável para ajudar com a referência a um conjunto de cadeia de caracteres em vez de outro. Neste exemplo, decidimos usar a cadeia de caracteres "playingMap" para o primeiro conjunto de cadeia de caracteres e, em seguida, "totalKicked" para o segundo conjunto de cadeia de caracteres e, em seguida, "multiplayer" para o terceiro.

As duas colunas juntas. Esses são os pares de cadeia de caracteres de localidade para a cadeia de caracteres de presença avançada. No primeiro exemplo, queremos usar a cadeia de caracteres em inglês "tocado em {0}". O {0} token será substituído por um valor. Podemos, em seguida, localizar a cadeia de caracteres em outras localidades. Assim como em enumerações, deve haver um padrão, bem como cadeias de caracteres com neutralidade de cultura a ser usado no caso em que um usuário está usando uma localidade que você ainda não especificou um par de cadeia de caracteres de localidade. As localidades usadas para enumerações, deve coincidir com as localidades para suas cadeias de caracteres.

A última coluna é para os parâmetros usados para preencher os espaços em branco na cadeia de caracteres de presença avançada. Se você quiser que o serviço de presença para pesquisar o valor do parâmetro no serviço de estatísticas, você deve usar o nome de estatística para se referir ao parâmetro aqui. Usando seu estatísticas em suas cadeias de caracteres permitirá que você defina sua cadeia de caracteres e, em seguida, não se preocupe. Se sua cadeia de caracteres tem o nome do mapa nele e você estiver usando o CurrentMap Stat para preencher o espaço em branco, em seguida, o serviço atualizará sua cadeia de caracteres apropriadamente, conforme os jogadores viajam de um mapa para o mapa no jogo. Se você não usar as estatísticas em suas cadeias de caracteres de presença avançada, não há parâmetros podem ser especificados.

No exemplo a seguir, queremos usar o mapa atual gerado por estatísticas na primeira cadeia de caracteres e, em seguida, a estatística BucketsKicked na segunda cadeia de caracteres. Observe que o terceiro exemplo não inclui quaisquer parâmetros. A cadeia de caracteres é definida por conta própria. Ele não faz referência a todas as estatísticas.

Nome Amigável | Locale | String | Parâmetros
--- | --- | --- | ---
playingMap | padrão | Executando no mapa:{0} | CurrentMap
 |  | pt | Executando no mapa:{0} |
 |  | en-US | Executando no mapa:{0} |
 |  | en-GB | Executando no mapa:{0} |
 |  | de | Spielt auf Karte: {0} |
 |  | etc. | |
totalKicked | padrão | Iniciado {0} Buckets! | BucketsKicked
 |  | pt | Iniciado {0} Buckets! |
 |  | en-US | Iniciado {0} Buckets! |
 |  | en-GB | Iniciado {0} Buckets! |
 |  | de | {0} Eimer getreten! |
 |  | etc. | |
vários jogadores | padrão | Reprodução com vários participantes |
 |  | pt | Reprodução com vários participantes |
 |  | en-US | Reprodução com vários participantes |
 |  | en-GB | Reprodução com vários participantes |
 |  | de | Spielt Mehrspieler |
 |  | etc. | 

Não há nenhum limite no quantas cadeias de caracteres que você pode criar, mas você deve ter pelo menos uma cadeia de caracteres do título.

"BucketsKicked" é apenas um número que pode obter substituído em sua cadeia de caracteres. "CurrentMap" é um exemplo de uma estatística em que a cadeia de caracteres é proveniente da enumeração. Quando seu título define presença avançada para uma dessas cadeias de caracteres, usamos a configuração para saber quais parâmetros são necessários.
