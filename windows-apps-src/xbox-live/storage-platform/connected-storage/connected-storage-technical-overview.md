---
title: Visão geral técnica do Armazenamento Conectado
description: Uma análise aprofundada sobre o funcionamento interno de armazenamento conectados.
ms.assetid: a0bacf59-120a-4ffc-85e1-fbeec5db1308
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox, armazenamento conectado
ms.localizationpriority: medium
ms.openlocfilehash: 6eddd11a370b8dcadc5108fe00539c2c6d1d9d1a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57624051"
---
# <a name="connected-storage"></a>Armazenamento Conectado

> [!NOTE]
> Este documento foi escrito originalmente para desenvolvedores gerenciados parceiro Xbox One.  Parte do Xbox One conteúdo específico, como o armazenamento Local e o título pode ser ignorado para UWP no Windows.  O conteúdo conceitual e a API neste documento ainda são relevantes.  Entre em contato com seu representante da Microsoft (se aplicável) com dúvidas.

O armazenamento e salvar modelos no Xbox One são muito diferentes no Xbox 360; Xbox One tem um modelo de aplicativo mais flexível que dá suporte a aplicativo rápido alternar, vários aplicativos simultâneos, e rápido suspender e retomar de aplicativos. Dados armazenados usando a API de armazenamento conectados automaticamente usa o perfil móvel para os usuários em vários consoles Xbox One e também estão disponíveis para uso offline.

Este tópico aborda:

-   Usando a API de armazenamento conectados para jogos salvo de armazenamento e dados de aplicativo no Xbox One.
-   Práticas recomendadas para o design do aplicativo salve sistemas, para que eles se integram bem com os benefícios de experiência do usuário fornecidos pelo modelo de aplicativo do Xbox One.
-   Como o sistema de armazenamento conectados maximiza a velocidade com que seu aplicativo pode salvar os dados
-   Como o sistema lida com conflitos de dados e sincronização de dados de vários consoles sem a necessidade de interface do usuário do aplicativo.
-   O modelo de resiliência de armazenamento conectados, que é projetado para que o aplicativo sempre tem uma exibição consistente de autoatendimento dos dados armazenados em contêineres individuais, mesmo no caso de perda de energia ou conectividade de rede interrompida.

> [!NOTE]
> O termo *aplicativo*, conforme usado aqui, refere-se a qualquer aplicativo em execução no console, incluindo jogos.

## <a name="overview"></a>Visão geral

O modelo de aplicativo Xbox One permite que os usuários usem vários aplicativos ao mesmo tempo, que significa que seu aplicativo não pode solicitar a um usuário a aguardar para os dados a ser salvo antes de desativar o console ou passar para outro aplicativo. Usuários Xbox One também aproveitará ter seus dados que usam perfis móveis automaticamente entre os consoles, de modo que cada console Xbox One enorme quantidade de seu próprio console. A plataforma Xbox One fornece a API de armazenamento conectados para ajudar a atender a esses requisitos de seu aplicativo.

O sistema de armazenamento conectado permite que os aplicativos armazenam dados como um ou mais *blobs* na *contêineres*. Quando um aplicativo salva os dados, ele é rapidamente copiado da partição exclusiva para a partição compartilhada para que as tarefas de armazenamento dos dados no disco e carregá-lo no armazenamento do Xbox Live título podem ser tratadas fora do tempo de vida do seu aplicativo.

Quando seu aplicativo solicita dados de um usuário específico do sistema de armazenamento conectado, o sistema verifica com a nuvem para dados atualizados automaticamente e notifica os usuários caso eles precisem esperar fazer o download dos dados. O sistema também solicita que os usuários escolham entre dados conflitantes em alguns casos, como quando um usuário tem desempenhado off-line em mais de um único console, ou quando outro console está carregando dados para esse usuário salvos.

Seu aplicativo também tem uma quantidade dedicada, mas limitada, de espaço de armazenamento de nuvem para cada usuário, para que os usuários não precisarão fazer escolhas difíceis sobre como excluir permanentemente os dados de um aplicativo para liberar espaço para salva do outro aplicativo. No entanto, há uma quantidade limitada de espaço de armazenamento no disco rígido Xbox One para armazenar em cache salva localmente, portanto, o sistema fornece uma experiência de usuário para liberar espaço em cache local. Os usuários estão no controle o que é armazenado em cache localmente; nunca perderão o acesso aos dados que ele está preocupado quando lidam com off-line. O sistema de armazenamento conectado também permite que uma pequena quantidade de dados independente de usuário sejam armazenadas localmente para o aplicativo. Esses dados por máquina são movidos e não são carregados para a nuvem.

O sistema de armazenamento conectados cuida do gerenciamento de energia do sistema para que as gravações no disco rígido e carregamentos para a nuvem pendentes serão tratadas automaticamente Xbox One — não é necessário para títulos exibir a interface do usuário para dizer "Salvar em andamento, por favor, não desligue o console."

### <a name="the-xbox-one-app-model-and-app-navigation"></a>A navegação de aplicativo e o modelo de aplicativo Xbox One

Xbox One permite que os usuários alternar rapidamente entre vários aplicativos. Aplicativos bem escritos permitem aos usuários escolher onde eles pararam durante sua última atividade com o contexto relevante carregando rapidamente, mesmo se o aplicativo tiver sido desligado desde a última vez em que o usuário acessou.

Consoles Xbox One podem executar apenas um aplicativo na partição exclusiva por vez. Apresentar uma experiência de usuário comutação rápida requer que o aplicativo em execução no momento exclusivo para desligar rapidamente quando o usuário deseja executar outra. Quando um usuário tenta mudar para outro aplicativo, o sistema envia o aplicativo ativo uma notificação de suspensão. Durante esse tempo, o aplicativo deve salvar o estado relevante e retornar a partir de sua função de suspensão. O sistema impõe um limite de tempo máximo de 1 segundo para esta operação. Se o aplicativo não tiver retornado dentro de 1 segundo, o sistema de modo forçado encerra o aplicativo. Aplicativos não podem impedir que os usuários navegar externamente, assim como acontece com o modelo de navegação em modernos smartphones e tablets.

Também há outros casos em que um aplicativo exclusivo é suspenso, como o sistema inserindo um estado ocioso ou de baixa energia quando o usuário pressiona o botão de energia no console. Depois que o aplicativo está suspenso, ele poderá ser retomado sem serem descarregados pelo sistema. Isso habilita a funcionalidade de rápida retomar. A coisa importante a saber é que aplicativos suspensos também podem ser encerrados ou retomados. O aplicativo sempre deve salvar estado, caso ela será encerrada.

Para funcionar bem com o modelo de aplicativo Xbox One, aplicativos devem estar preparados para serializar o estado em um buffer de memória rapidamente para que o estado relevante pode ser salvo dentro do período de tempo de suspensão de 1 segundo.

Observe que há uma diferença entre dados salvos sobre o jogo de usuário e dados sobre o estado do aplicativo, como o local dentro de menus. Além de economizar o jogo quando o aplicativo está suspenso, você deve considerar a persistir o estado de menu, se o usuário estiver no meio definindo uma configuração ou personalizar um caractere fora do mecanismo de jogo principal.

Os usuários podem deixar um jogo suspendido por muito tempo. Considere a possibilidade de oferecer uma experiência diferente quando o jogo é retomado após uma suspensão longo. Descartando um usuário de volta em meio a uma batalha da sua campanha pode ser uma experiência brusca e inesperada, se o usuário ainda não executados por duas semanas, enquanto que uma quebra de uma hora seria mais comum e garante um rápido retorno ao jogo.

Para obter mais informações sobre o modelo de aplicativo Xbox One, consulte os seguintes recursos:

-   [A troca de aplicativos modernos para a sala de estar](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Documents/Xfest%202012/Xfest%202012%20-%20Modern%20Application%20Switching%20for%20the%20Living%20Room.pptx), uma apresentação Xfest 2012
-   [A experiência de Shell](https://developer.xboxlive.com/_layouts/xna/download.ashx?file=PROD-D_Experience.pptx&folder=platform/xfest2013), uma apresentação na Xfest 2013
-   [Gerenciamento de tempo de vida de processo para o Xbox One](https://developer.xboxlive.com/en-us/platform/development/education/Documents/PLM%20for%20Xbox%20One.aspx), um white paper sobre GDN

> [!NOTE]
> Algumas apresentações no [2012 Xfest](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2012.aspx) e [Xfest 2013](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2013.aspx) contêm informações que estão desatualizadas devido ao aviso que Xbox One dá suporte a reprodução off-line.


### <a name="storage-options-on-xbox-one"></a>Opções de armazenamento no Xbox One

Xbox One fornece várias opções de armazenamento, cada um com seus próprios benefícios e restrições. Aplicativos talvez precise usar uma combinação de opções, dependendo dos requisitos de aplicativos.


<a name="connected-storage"></a>Armazenamento conectado
-----------------
Armazenamento conectado foi projetado para ajudar os aplicativos a salvar os dados do jogo Xbox One e outros estados-dados de aplicativo relevante que devem ser usado entre os consoles. A conectado API de armazenamento, específicas para Xbox One, ajuda a salvar e carregar esses dados. A API funciona em combinação com o modelo de aplicativo Xbox One.

A API de armazenamento conectado fornece os seguintes recursos:

-   Aplicativos rapidamente podem salvar até 16 MB de dados por vez em um buffer de memória na partição do sistema, que, em seguida, é armazenado em cache localmente em HDD pelo sistema e carregado para a nuvem.
- Para os parceiros gerenciados e ID@Xbox desenvolvedores:
  - 256 MB por usuário/aplicativo de armazenamento em nuvem.
- Para desenvolvedores do programa de criadores do Xbox Live:
  - 64 MB por usuário/aplicativo de armazenamento em nuvem.
-   Resposta robusta para falhas de energia — aplicativos não precisam lidar com dados parciais que está sendo salvos.
-   Dados são carregados automaticamente para a nuvem, mesmo quando o aplicativo não está em execução.
-   Dados estão disponíveis entre os consoles Xbox One que estão conectados ao Xbox Live.
-   Xbox Live lida com gerenciamento de sincronização e a conflitos entre dispositivos sem a necessidade de envolvimento pelo aplicativo.

Para obter mais informações sobre a API de armazenamento conectados, consulte a seção apropriada xblesdk.chm (que documenta as APIs do SDK do Xbox Live extensão) no SDK do Xbox Live.


<a name="xbox-live-title-storage"></a>Armazenamento do Xbox Live título
-----------------------
O serviço de armazenamento do título oferece uma API de REST de plataforma cruzada para o armazenamento de dados com os seguintes recursos:

-   Fornece dados de compartilhamento entre várias plataformas, aplicativos e usuários
-   Dá suporte a binário, JSON e arquivos de configuração
-   Para os parceiros gerenciados e ID@Xbox desenvolvedores:
    - 256 MB por usuário/aplicativo de armazenamento em nuvem
    - 256 MB de por armazenamento global do título
- Para desenvolvedores do programa de criadores do Xbox Live:
  -   64 MB por usuário/aplicativo de armazenamento em nuvem
  -   256 MB de por armazenamento global do título

Requisitos para usar o serviço:

-   O console Xbox One deve estar online para acessar o serviço
-   Todas as interações de serviço devem ser concluídas enquanto o aplicativo está em execução; transferência de dados não será concluída automaticamente em segundo plano.

Para obter mais informações, consulte *armazenamento de título do Xbox Live*, na documentação do XDK.


<a name="local-temporary-storage"></a>Armazenamento temporário local
-----------------------
No console, um aplicativo tem acesso ao armazenamento temporário local com as seguintes características:

-   2 GB de armazenamento de disco rígido dedicado, acessível pelo caminho de t:\\.
-   O conteúdo desse armazenamento pode ser removido quando o aplicativo não está em execução.

Para obter mais informações sobre o armazenamento local, consulte o armazenamento Local, na documentação do XDK.


<a name="configuring-your-app-for-connected-storage"></a>Configurar um aplicativo para o armazenamento conectado
------------------------------------------
Quando você usa a API de armazenamento conectados, todos os leia e operações de gravação são associadas com um Xbox Live principal serviço configuração de ID (SCID), definido no arquivo de manifesto do aplicativo, appxmanifest. XML:

```xml
      <Extensions>
        <mx:Extension Category="xbox.live">
        <mx:XboxLive TitleId="<your title ID>" PrimaryServiceConfigId="<your SCID>"
        RequireXboxLive="<boolean indicating Live requirement>" />
        </mx:Extension>
      </Extensions>
```

Para obter mais informações sobre como adquirir o título de ID e SCID para seu aplicativo, consulte *configuração de backup de áreas de segurança para desenvolvimento de Xbox Live*, na documentação do XDK.



## <a name="connected-storage-system-concepts"></a>Armazenamento conectado: conceitos de sistema

Esta seção descreve os componentes do sistema de armazenamento conectado, seus relacionamentos e seus usos adequados.

### <a name="connected-storage-space"></a>Espaço de armazenamento conectada

Em um alto nível, todos os dados no sistema de armazenamento conectado está associado um usuário ou um computador (por exemplo, um individuais Xbox One console). Todos os dados salvos por um aplicativo para um determinado usuário ou computador são armazenados em um espaço de armazenamento conectados.

Cada usuário de seu aplicativo obtém um espaço de armazenamento conectados com um limite de armazenamento total de 256 MB. É importante observar que esse armazenamento é dedicado ao seu aplicativo sozinho — ele não é compartilhado com outros aplicativos.

Seu aplicativo também tem 64 MB de espaço em um espaço de armazenamento conectados local para a máquina. Esse espaço de armazenamento é independente de usuários e pode ser acessado, mesmo se nenhum usuário estiver conectado.

Para adquirir um espaço de armazenamento conectados, o aplicativo chama o *método ConnectedStorageSpace.GetForUserAsync* ou o *ConnectedStorageSpace.GetForMachineAsync método*. Essa uma operação potencialmente demorada, especialmente se o usuário tem dados salvos em um dispositivo e está retomando o jogo pela primeira vez em outro dispositivo. Para obter detalhes sobre esse processo e as condições de erro possíveis que podem ocorrer enquanto o aplicativo está esperando para adquirir um espaço de armazenamento conectados, consulte *sincronizando um espaço de armazenamento conectados*, mais adiante neste documento.

Depois que o aplicativo adquire uma **ConnectedStorageSpace** objeto, chamadas para todos os métodos sob o *Storage Namespace* que usar esse objeto ou outros objetos derivados dele, não dependem de uma resposta de serviços da web para ser concluída. No entanto, porque o acesso para o Xbox um HDD não é exclusivo para o aplicativo do Active Directory, limites superiores do estritos sobre o desempenho desses métodos não pode ser garantidos.


### <a name="connected-storage-containers-and-blobs"></a>Armazenamento conectado: contêineres e blobs

O *contêiner de armazenamento conectados*, ou *contêiner* para abreviar, é a unidade básica de armazenamento. Cada espaço de armazenamento conectados pode conter vários contêineres, conforme mostrado no diagrama a seguir.

**Figura 1.  Espaço de armazenamento conectados (por título/máquina ou por título/usuário)**

![](../../images/connected_storage/connected_storage_space_containers.png) Dados são armazenados em contêineres como um ou mais buffers chamado *blobs*. O diagrama a seguir ilustra a representação interna do sistema de contêineres no disco. Para cada contêiner, há um arquivo de contêiner que contém referências ao arquivo de dados para cada blob no contêiner.

**Figura 2.  Diagrama de um contêiner**

![](../../images/connected_storage/container_storage_blobs.png)

Para armazenar dados em um contêiner, chame o *ConnectedStorageContainer.SubmitUpdatesAsync método*, fornecendo um mapa de nomes e blobs (objetos de Buffer). Todas as alterações descritas em uma **SubmitUpdatesAsync** chamada sejam aplicadas automaticamente, ou seja, a todos os blobs são atualizados conforme solicitado, ou toda a operação será encerrada e o contêiner permanecerá em seu estado antes da chamada.

Individuais que usam operações de salvamento **SubmitUpdatesAsync** são limitados a 16 MB de dados por vez.


### <a name="submitupdatesasync-behavior"></a>Comportamento de SubmitUpdatesAsync

Quando **SubmitUpdatesAsync** é chamado, os buffers fornecidos para a chamada são rapidamente copiados da partição de aplicativo em um espaço de memória dedicada na partição do sistema. Depois que a memória tiver sido copiada com êxito para a partição do sistema, o manipulador de conclusão fornecido na **SubmitUpdatesAsync** chamada é invocada dentro do aplicativo, que indica para o aplicativo que é seguro liberar a memória alocada localmente para os dados.

O sistema, em seguida, salva os blobs de disco rígido do console e conclui a operação com uma atualização de contêiner final que confirme a operação inteira no contêiner.

Há um limite de 16 MB na memória na partição compartilhada para recebimento **SubmitUpdatesAsync** dados. Se uma chamada para **SubmitUpdatesAsync** não podem ser atendidas imediatamente pelo sistema porque não há memória suficiente livre no buffer de 16 MB dedicada, a chamada foi enfileirada para manutenção. O sistema continuamente transfere os dados do buffer de 16 MB para o disco rígido e atualizações em fila são atendidas na ordem em que foram solicitados como espaço se torna disponível no buffer de 16 MB.

**Figura 3.  Comportamento de SubmitUpdatesAsync**

![](../../images/connected_storage/submitupdatesasync_behavior.png) Carregando para a nuvem acontece de maneira semelhante: Os blobs individuais são carregados para o serviço e a operação de atualização é confirmada por uma atualização final para um arquivo de contêiner que faz referência a todos os outros blobs carregados. Em um carregamento para a nuvem, essa consolidação em uma atualização única e final garante que todos os dados referenciados em um **SubmitUpdatesAsync** chamada é confirmada em sua totalidade ou o contêiner permanecerá inalterado. Dessa forma, mesmo se um sistema fica offline ou queda de energia durante uma operação de upload de um usuário poderia passar para outro console Xbox One, baixar dados de nuvem e continuar a jogar com uma exibição consistente de todos os contêineres.

> [!IMPORTANT]
> Dependências de dados entre os contêineres não são seguras.  Os resultados do indivíduo *SubmitUpdatesAsync* chamadas têm garantia de ser aplicado inteiramente, ou nenhum.

**SubmitUpdatesAsync** chamadas não devem presumir que um futuro **SubmitUpdatesAsync** chamada será concluída com êxito para deixar o contêiner em um estado válido. Em outras palavras, os aplicativos não podem confiar em mais de um **SubmitUpdatesAsync** chamada para salvar todos os dados necessários em um contêiner. Cada **SubmitUpdatesAsync** chamada deve deixar o conteúdo do contêiner especificado em um estado válido para o aplicativo leia mais tarde.

Para ilustrar esse problema, considere um cenário em que um contêiner controla a quantidade de ouro e mantidos por um caractere chamado Bob de alimentos. O título pode armazenar dois blobs, denominados *alimentos* e *ouro*. Bob começa com 100 gold e nenhum alimentos no seu inventário.

**Figura 4.  Cenário de exemplo: Roberto inicia com 100 ouro.**

![](../../images/connected_storage/submitupdatesasync_example_scenario1.png)

Bob agora passa a 50 ouro. O título prepara uma **SubmitUpdatesAsync** chamar, que atualiza o valor do blob gold para 50.

O sistema captura o blob atualizado e informações sobre a atualização de contêiner para o buffer de atualizações. Em seguida, o sistema copia o valor do novo blob para o disco rígido.

**Figura 5.  O sistema captura as informações atualizadas e copia os valores para o disco rígido.**

![](../../images/connected_storage/submitupdatesasync_example_scenario2.png)

Por fim, o sistema atualiza o arquivo de contêiner na unidade de disco rígido para referenciar o novo blob. Por fim, o sistema remove o blob não referenciado em uma operação de coleta de lixo.

**Figura 6.  O sistema atualiza o arquivo de contêiner em que o disco rígido e remove o blob não referenciado.**

![](../../images/connected_storage/submitupdatesasync_example_scenario3.png)

Observe que os blobs mais usar por **SubmitUpdatesAsync** chamada, quanto mais tempo é necessário para concluir as operações atômicas necessárias das operações de sistema de arquivo para armazenar os dados de modo eficiente. A granularidade de armazenamento de dados no exemplo anterior é pequena demais, mas ele se destina a ilustrar claramente o comportamento da atualização atômica dos vários blobs em um contêiner.


### <a name="updating-multiple-blobs--the-wrong-way"></a>Atualizando vários blobs — a maneira errada

Considere um cenário em que o Bob deseja comprar alguns alimentos. Para manter a simplicidade, diremos que 1 unidade de ouro compra 1 unidade de alimentos e Bob deseja comprar 25 unidades de alimentos. O aplicativo pode emitir uma **SubmitUpdatesAsync** chamada para adicionar 25 unidades de alimentos e, em seguida, outro para subtrair de 25 unidades de ouro do Bob\_contêiner de inventário. Mas mesmo que os manipuladores concluídos para ambos **SubmitUpdatesAsync** chamadas tiveram sido chamadas, há um potencial para resultados incorretos devido a eventos, como perda de energia, o que pode interromper os dados sejam gravadas no disco rígido, ou um Sincronização incompleta para a nuvem. Os diagramas a seguir explicam as etapas executadas pelo sistema e o resultado de uma perda de energia em qualquer uma das etapas.

Suponha que os dados de ambos **SubmitUpdatesAsync** chamadas já está no buffer de atualização do sistema e manipuladores de conclusão do título para ambas as chamadas são chamados.

Primeiro, o sistema grava os dados para o novo valor do blob de alimentos no disco.

**Figura 7.  O sistema grava o valor do blob alimentos no disco.**

![](../../images/connected_storage/update_method_wrong_way_1.png) Em seguida, o sistema atualiza o contêiner para fazer referência ao valor recém-criados. Como ilustra o diagrama a seguir, se a energia foram perdida após essa etapa e antes do próximo, Bob acabaria com uma boa dose, ganhando alimentos 25 sem a necessidade de ouro correspondente deduzido do seu inventário.

**Figura 8.  O sistema atualiza o contêiner para fazer referência ao valor recém-criados.**

![](../../images/connected_storage/update_method_wrong_way_2.png)

Em seguida, o sistema grava os dados para o novo valor do blob de ouro no disco. O valor para ouro referenciado pelo Bob\_contêiner de inventário ainda não tiver sido atualizada e Bob tem 25 ouro mais do que ele deve — mas ainda estamos um pouco mais para o resultado desejado.

**Figura 9.  O sistema grava os dados para o novo valor do blob de ouro no disco.**

![](../../images/connected_storage/update_method_wrong_way_3.png)

Por fim, o sistema atualiza o arquivo de contêiner para fazer referência ao blob recém-criados para ouro — o resultado desejado.

**Figura 10.  O sistema atualiza o arquivo de contêiner para fazer referência ao blob de ouro recém-criados.**

![](../../images/connected_storage/update_method_wrong_way_4.png)

### <a name="updating-multiple-blobs--the-right-way"></a>Atualizando vários blobs — da maneira correta

O modo adequado para garantir que a quantidade de ouro e alimentos no inventário do Bob atomicamente é atualizada, sem a possibilidade de estado intermediário incorreto devido à perda de energia, é atualizar ambos os blobs em uma única **SubmitUpdatesAsync** chamar. O sistema será, em seguida, execute as seguintes etapas.

Primeiro, o sistema grava os dados para o novo valor do blob de alimentos no disco.

**Figura 11.  O sistema grava os dados para o novo valor do blob de alimentos.**

![](../../images/connected_storage/update_method_right_way_1.png) Em seguida, o sistema grava os dados para o novo valor do blob de ouro no disco.

**Figura 12.  O sistema grava os dados para o novo valor do blob gold.**

![](../../images/connected_storage/update_method_right_way_2.png) Por fim, o sistema atualiza o arquivo de contêiner para fazer referência a ambos os novos blobs.

**Figura 13.  O sistema atualiza o arquivo de contêiner para fazer referência a ambos os novos blobs.**

![](../../images/connected_storage/update_method_right_way_3.png) Embora esse exemplo seja muito simple, ilustra a importância de realizar todas as modificações nos dados em um contêiner que deve ser aplicado atomicamente, emitindo um único **SubmitUpdatesAsync** chamar com todas as atualizações desejadas. Ao fazer isso para o caso de compra de alimentos com gold, o aplicativo evita uma condição de corrida potenciais que poderia atualizar somente um dos valores e deixar o caractere com muita ouro incorretamente.

### <a name="performance-characteristics-and-considerations"></a>Considerações e as características de desempenho

O buffer de atualização de 16 MB na partição compartilhado permite que um número limitado de operações de atualização a serem executadas muito rapidamente. A velocidade na qual o sistema pode persistir os dados no disco depende do volume de dados no buffer e o número de blobs. Como cada blob é gravada no disco com resiliência, quanto maior o número de blobs no buffer, mais tempo demora para mantê-los no disco.

Figura 13 mostra um exemplo para o tempo de processamento para um **SubmitUpdatesAsync** operação a cada 2 segundos com dois 512 k atualizações de blob e outro 1024 k blob atualização, quando não há nenhuma outra atividade de disco rígido no sistema. O sistema pode operar em um estado estável, cada atualização dentro de 14 – 18ms de processamento.

**Figura 14.  Tempo de processamento de uma operação de SubmitUpdatesAsync cada 2 segundos com dois 512 k atualizações de blob e outro 1024k blob update e nenhuma outra atividade de disco rígido.**

![](../../images/connected_storage/submitupdatesasync_proc_time_mixed_size_fixed_interval.png) A Figura 14 mostra o tempo de processamento para três mil 1024 blobs em vários intervalos de tempo.

O sistema pode processar essas atualizações em intervalos de 3 segundos no estado estável de 87ms. Aumentar a frequência para uma vez a cada 2 segundos, o sistema ainda pode processar atualizações no estado estável de 87ms.

Reduzir o intervalo como 1 segundo entre as atualizações altera o comportamento de estado estável. O sistema pode processar 60 atualizações no 87ms por atualização, mas cada atualização além do que leva significativamente mais tempo, atingindo um tempo de processamento de estado estacionário de 500 milissegundos em segundo lugar por atualização, com significativa flutuação. Isso ocorre porque o buffer de memória de 16 MB está sendo preenchido mais rápido do que ele pode liberar os dados em disco; as atualizações são forçadas a aguardar atualizações anteriores a serem gravados.

O efeito aumenta drasticamente ao atualizar o intervalo para uma atualização de cada 0,5 segundo. O sistema pode processar atualizações apenas 7 neste intervalo novamente cada 87ms por atualização, antes de atingir um estado estável no qual cada atualização demorar mais de 1 segundo para ser processado com variações muito altas.

**Figura 15.  Tempo de processamento de três mil 1024 blobs em vários intervalos de tempo.**

![](../../images/connected_storage/submitupdatesasync_proc_time_fixed_size_various_intervals.png) Esses são apenas exemplos ilustrativos. Seu aplicativo geralmente não deve ser salvando dados isso muitas vezes, mas ele também não geralmente estar operando em um ambiente livre de e/s de disco.

É importante compreender as características do sistema com base nesses exemplos — medir o aplicativo em várias condições operacionais, garantindo que o salvamento de operações podem ser concluídas em menos de 1 segundo durante o seu aplicativo do suspender o manipulador.


## <a name="synchronizing-a-connected-storage-space"></a>Sincronizando um espaço de armazenamento conectada

-   Verificação de conectividade
-   Aquisição de bloqueio
-   Lógica de listagem, de comparação e mesclagem de contêiner
-   Download do contêiner

Quando seu aplicativo solicita acesso a um espaço de armazenamento conectados, o sistema executa um processo de sincronização para manter os dados do usuário salvos em um estado consistente entre os consoles Xbox One e disponibilizar os próprios dados para reprodução offline. Como sincronizar pode levar quantidades variadas de tempo e pode exigir que o usuário tomar decisões, o sistema pode exibir interface do usuário para o usuário em vários estágios do processo.

O usuário pode navegar para fora de seu aplicativo pressionando o botão Xbox a qualquer momento, mesmo se a sincronização de interface do usuário está ativo. O sistema oculta a interface do usuário e a sincronização continuará quanto possível sem interação do usuário. Quando o usuário navega de volta para o aplicativo, a interface do usuário é exibido novamente, a menos que a sincronização foi concluída. O sistema nunca faz uma suposição sobre uma seleção do usuário quando a interface do usuário está oculto.

Como o sistema não exibe nenhuma sincronização da interface do usuário quando o usuário está na tela inicial e a renderização do seu aplicativo ainda está visível no bloco do aplicativo grande, é importante que o aplicativo renderizar visuais contextualmente apropriadas durante uma **GetForUserAsync** chamada está sendo concluída. O processamento contínuo indica ao usuário que o aplicativo ainda será interativo e está aguardando dados a serem carregados.

O diagrama a seguir descreve a sequência de alto nível que o sistema segue quando um aplicativo solicita um espaço de armazenamento conectados. Se toda a sequência demora mais do que alguns segundos, a sincronização desenhados pelo sistema da interface do usuário é exibida.

**Figura 16.  Sequência seguida pelo sistema quando um aplicativo solicita o espaço de armazenamento conectados.**

![](../../images/connected_storage/app_requests_connected_storage_space.png) O sistema passa pelas fases a seguir quando ele as atende uma **GetForUserAsync** solicitação:

-   Verificação de conectividade
-   Aquisição de bloqueio
-   Lógica de listagem, de comparação e mesclagem de contêiner
-   Download do contêiner

### <a name="connectivity-check"></a>Verificação de conectividade

Para iniciar a manutenção de um **GetForUserAsync** de solicitação, o sistema verifica a conectividade. Se o console estiver offline, o processo de sincronização inteiro será ignorado e o espaço de armazenamento conectados para o usuário especificado está marcado como offline para a sessão atual. Todos os dados modificados serão sincronizados com o armazenamento em nuvem na próxima vez que o aplicativo acessa o espaço de armazenamento conectados desse mesmo usuário e o sistema pode alcançar o serviço de armazenamento do título. Nenhuma interface do usuário nunca é mostrado para esse caso.

Para obter mais informações sobre o tratamento offline fora do contexto de armazenamento conectados, consulte *resiliência de interrupção de serviço para Xbox um*.

### <a name="lock-acquisition"></a>Aquisição de bloqueio

Depois de verificar a conectividade, o sistema tenta adquirir acesso exclusivo para o espaço de armazenamento de nuvem associado ao seu aplicativo e o usuário atual. Isso é feito colocando um arquivo de bloqueio na área de armazenamento conectado do armazenamento do título. Se o console estiver online, pode acessar o serviço e é capaz de adquirir o bloqueio em um curto período de tempo, nenhuma interface do usuário é apresentado e continua o processo de sincronização.

Depois que o sistema tiver adquirido um bloqueio para um espaço de armazenamento conectados específico e retornou uma instância de um espaço de armazenamento conectados ao seu aplicativo, nenhuma das API do seu aplicativo chama operando em dados dentro do espaço de armazenamento conectados vai bloquear solicitações da web bem-sucedida. O bloqueio fornece proteção suficiente, para que, mesmo se um usuário Desconecte o cabo de rede do sistema depois que seu aplicativo tiver adquirido um espaço de armazenamento conectados, as chamadas à API funcionará com base nos dados disponíveis localmente.

Há alguns cenários possíveis de erro durante a etapa de aquisição de bloqueio:

 **Sincronizando a interface do usuário** se o console está online, mas não tenha adquirido o bloqueio do serviço em um curto período, uma interface do usuário "sincronização" é exibida.

 **Interromper o bloqueio** se o usuário teve o aplicativo em outro console, pois ele ou ela última execução atual, é possível que o outro console tem acesso exclusivo ao espaço de armazenamento e está no meio de carregamento de dados. Também é possível que outro console foi iniciado o carregamento de dados, mas perdeu sua conexão ou energia antes de concluir.

Ambos os casos são denominados *contenção de bloqueio*, e em qualquer caso, o sistema apresenta a IU para explicar que o outro console está carregando dados. O usuário pode esperar para que esse processo concluir ou trabalhar com os dados disponíveis no momento na nuvem. Se o usuário optar por trabalhar com dados na nuvem, o sistema leva o bloqueio em si (interrompe o bloqueio), ao adquirir acesso exclusivo para o armazenamento em nuvem para o usuário e o aplicativo. O carregamento do console do é cancelado, e continua o processo de sincronização.

### <a name="container-listing-comparison-and-merger-logic"></a>Lógica de listagem, de comparação e mesclagem de contêiner

Depois de adquirir um bloqueio, o sistema solicita uma lista de todos os contêineres na nuvem para o aplicativo em particular e o usuário. Em seguida, ele compara o conteúdo do disco rígido local com os dados na nuvem e procede de acordo com os resultados da comparação:

 **Correspondem a dados locais na nuvem** se não houve nenhuma alteração de outros consoles, e os dados na nuvem e local difíceis de unidade é idêntica e, em seguida, a sincronização for concluída, o manipulador de conclusão do **GetForUserAsync**chamada é invocada no momento, e seu aplicativo pode continuar a carrega e salva.

 **Nenhum dado local** se a nuvem tem dados, mas o console local não tiver uma, os dados da nuvem seja baixados localmente. Isso pode ocorrer, por exemplo, quando o usuário estiver em execução na casa de um amigo pela primeira vez.

 **Mesmos contêineres, modificados localmente e na nuvem** se o usuário tiver modificado os contêineres na nuvem executando em outro console e tiver modificado os mesmos contêineres, ao usar o console atual offline, os dados não podem ser mesclados automaticamente. O usuário será solicitado para escolher quais dados manter. No caso de conflitos, o usuário pode escolher uma política de substituição: Os dados locais ou dados de nuvem sempre são mantidos, ou o usuário pode selecionar **Cancelar** e adiar a fazer sua escolha. Se o usuário escolhe a nuvem ou locais de dados como uma política de substituição, contêineres com o mesmo nome — mas com conteúdo diferente — será resolvido adequadamente.

Se o usuário seleciona **Cancelar**, o título terá acesso ao salvar o sistema em um estado não resolvido, como se o usuário estava tentando reproduzir offline. Nesse caso, a resolução de conflitos da interface do usuário é apresentada novamente na próxima vez em que o aplicativo solicita acesso ao espaço de armazenamento conectada, se o console estiver online.

### <a name="container-download"></a>Download do contêiner

Depois que todos os conflitos foram resolvidos, o sistema tem todas as informações necessárias para identificar quais contêineres precisam ser baixados da nuvem. Todos os contêineres necessários serão baixados, o manipulador de conclusão do *ConnectedStorageSpace.GetForUserAsync método* será invocada no momento, e seu aplicativo pode continuar a carrega e salva.

Alguns erros possíveis durante esta etapa:

**Armazenamento local insuficiente**  
No caso de espaço no disco rígido local para os contêineres necessários, são apresentadas aos usuários da interface do usuário solicitando que liberar espaço em disco removendo dados salvos localmente. Para ajudá-los a evitar a exclusão permanente de dados importantes que não são feitos o backup na nuvem, a interface do usuário indica claramente os dados que é o cache local simplesmente e dados que é exclusivos para o console atual.

Quando a interface do usuário é apresentado ao usuário:

-   Se o usuário libera espaço suficiente, a sincronização continua e é concluída.
-   Se o usuário cancelar fora da interface do usuário sem liberar espaço suficiente, o manipulador de conclusão do **GetForUserAsync** chamar retorna **OutOfLocalStorage**— consulte *ConnectedStorageErrorStatus Enumeração*. O aplicativo deve confirmar que o usuário deseja reproduzir sem ser capaz de salvar os dados. Se o usuário concorde, o aplicativo deve continuar sem salvar os dados desse usuário. Se o usuário indica que deseja salvar os dados quando estiver em execução, o aplicativo deve ser repetida a **GetForUserAsync** chamar, que, em seguida, exibirá a interface do usuário para liberar espaço.

**O usuário cancela a sincronização**  
Se o usuário não deseja aguardar a conclusão da sincronização e seleciona Cancelar, o usuário será informado de que nem todos os dados salvos estarão disponíveis. O manipulador de conclusão do **GetForUserAsync** chamada será invocada no momento, e o aplicativo pode continuar a carrega e salva.

**Tempo limite da rede**  
Se os dados de baixar o tempo limite devido a um problema com a conectividade de rede ou a disponibilidade do serviço, o usuário recebe a opção para repetir a sincronização. Se ele optar por não, o usuário será informado de que nem todos os dados salvos estarão disponíveis. O manipulador de conclusão do **GetForUserAsync** chamada será invocada no momento, e o aplicativo pode continuar a carrega e salva.

## <a name="development-tools"></a>Ferramentas de desenvolvimento

Duas ferramentas ajudarão você com o uso do aplicativo de armazenamento conectados de desenvolvimento: XbStorage e Fiddler.

### <a name="managing-connected-storage-with-xbstorage"></a>Gerenciando o armazenamento conectado com XbStorage

XbStorage é uma ferramenta de desenvolvimento que permite gerenciar os dados de local de armazenamento conectados no kit de desenvolvimento de Xbox One de um computador de desenvolvimento.

A ferramenta permite limpar os espaços de armazenamento conectados local do disco rígido, bem como importando e exportando o espaços de armazenamento individuais usuário ou computador-conectados usando arquivos XML.

Quando uma operação é executada em um espaço de armazenamento conectados local, o sistema se comporta como se essa operação tinha sido executada pelo próprio aplicativo. Copiando os dados de um espaço de armazenamento conectados em um arquivo local faz a sincronização com a nuvem antes de copiar.

Da mesma forma, copiando dados de um arquivo XML em que o computador de desenvolvimento para um contêiner de armazenamento conectados no kit de desenvolvimento de Xbox One faz com que o console iniciar o upload de dados para a nuvem. Há uma exceção: se o kit de desenvolvimento não é possível adquirir o bloqueio ou se houver um conflito entre os contêineres no console e aqueles na nuvem. Nesse caso, o console se comporta como se o usuário tenha decidido não resolver o conflito-por exemplo, selecionando uma versão do contêiner para manter – e o console se comporta como se ele estava em execução off-line até a próxima vez em que o título é iniciado.

O comando de reinicialização do XbStorage limpa o armazenamento local de dados salvos todos os 'SCIDs e dos usuários, mas não altera os dados armazenados na nuvem. Isso é útil para definir um console para o estado que estaria em se um usuário foram roaming para um console e baixar dados de nuvem após a execução de um título.

Para obter mais informações sobre XbStorage, consulte *gerenciar o armazenamento conectado (xbstorage.exe)*, na documentação do XDK.

### <a name="monitoring-connected-storage-network-activity-using-fiddler"></a>Monitorando a atividade de rede de armazenamento conectados usando o Fiddler

Ele pode ser útil determinar se o seu console está interagindo com o serviço quando são executadas operações de armazenamento de nuvem. Usando o Fiddler pode ajudar a determinar se o seu console está fazendo chamadas para o serviço com êxito ou se ele está encontrando erros de autorização. Para obter informações sobre como configurar o Fiddler em um Xbox One, consulte *como usar o Fiddler com o Xbox One*, nesta documentação XDK.

## <a name="resources"></a>Recursos

Além dos recursos sugeridos acima, a seguir pode ser úteis no desenvolvimento de seu aplicativo ou o título:

-   Conectado visão geral de armazenamento, na documentação do XDK
-   [Gerenciamento de tempo de vida de processo](https://developer.xboxlive.com/_layouts/xna/download.ashx?file=ProcessLifetimeManagement_08_2013_qfe5.zip&folder=platform/aug2013xdk_qfe5/samples), um exemplo disponível de exemplos na rede de desenvolvedor de jogos (GDN)
-   ["Processo de gerenciamento de tempo de vida (PLM) para o Xbox One"](https://developer.xboxlive.com/en-us/platform/development/education/Documents/PLM%20for%20Xbox%20One.aspx), um white paper disponível de White Papers sobre GDN
