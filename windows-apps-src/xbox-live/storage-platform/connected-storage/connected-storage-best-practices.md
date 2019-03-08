---
title: Conectado práticas recomendadas de armazenamento
description: Recomendações sobre como obter o melhor desempenho e a experiência de armazenamento conectada
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox, um armazenamento conectado
ms.localizationpriority: medium
ms.openlocfilehash: dbdb8f95e9afc44a3280d897e74f74d327f94b97
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632921"
---
# <a name="connected-storage-best-practices"></a>Práticas recomendadas de Armazenamento Conectado

Os desenvolvedores devem dividir salvar dados em agrupamentos lógicos que são atualizáveis independentemente em vez de escrever salva monolítica. Isso permite que os títulos reduzir a quantidade de dados que eles gravam em várias situações, reduzindo o consumo de ambos os recursos locais e carregar o uso de largura de banda. A API também permite que os títulos atualizar mais de um item de dados em uma operação atômica que é garantido que tenha êxito completamente ou não entrarão em vigor em todos os (por exemplo no caso de salvar intermediário de uma falha grave).

Como o Xbox One permite que os usuários alternar rapidamente entre os títulos, os desenvolvedores devem criar seu título para manter seu estado atual pronto para salvar em curto prazo, antecipadamente ao recebimento de um evento de suspensão, que pode ocorrer praticamente a qualquer momento. Os usos de API de armazenamento conectados RAM fora a reserva do título como o primeiro ponto de armazenamento a fim de maximizar a velocidade de gravação do título durante um curto suspender a janela de tempo. O sistema, em seguida, persiste os dados para o armazenamento durável, reconcilia-lo com quaisquer outras gravações de dados desde o último carregamento e agenda carregamentos de dados. Depois de armazenados e enfileirados para carregamento, o sistema é robusto para várias falhas, como falha de energia ou perda de conectividade de rede.

## <a name="when-to-load-a-users-connected-storage-space-data"></a>Quando carregar dados de espaço de armazenamento conectado a um usuário

O tempo de execução para carregar o espaço de dados de armazenamento conectado a um usuário pode variar. Aplicativos devem executar esta ação durante a execução do principal, em vez de em resposta a partir de um usuário sair ou em resposta ao recebimento de uma notificação de suspensão do sistema.
Em geral, os aplicativos devem carregar um espaço de armazenamento conectado como um usuário faz logon e indica desejo para reproduzir, a menos que o jogo está em um modo no qual ele está determinada nenhuma funcionalidade de salvar é necessária. Você também deve considerar alinhando o carregamento de um espaço de armazenamento conectados com longas sequências de carregamento de dados, para que as operações podem ser executadas em paralelo.
Depois que seu aplicativo carregou dados de espaço de armazenamento conectado a um usuário, ele deve manter esse valor para futuras salva. Manter um espaço de armazenamento conectados ao longo do tempo não tem efeitos negativos no desempenho ou a robustez. Porque o carregamento de um espaço de armazenamento conectados faz com que uma verificação de sincronização com a nuvem se o sistema está online, liberando e carregar novamente o espaço de armazenamento conectado a um usuário durante a lenta ou condições de rede não confiável podem fazer com que o usuário veja uma interface do usuário da sincronização até que o sistema expire. Conectado armazenamento espaços não precisam ser liberados explicitamente para fazer com que a sincronização de nuvem. Uma vez o manipulador de conclusão especificado em uma `SubmitUpdatesAsync` chamada retornou com `AsyncStatus::Completed`, o sistema se encarrega de sincronização com a nuvem, se o aplicativo libera o `ConnectedStorageSpace` objeto.

## <a name="when-to-save"></a>Quando salvar

Sempre que um aplicativo receber uma notificação de suspensão, o aplicativo pelo menos deve salvar os dados relevantes, permitindo que o sistema retornar a um estado contextualmente apropriado para o usuário.

Se seu projeto de jogo usa periódica e automática ou salva iniciada pelo usuário, o armazenamento conectado pode ser chamada com mais frequência do que quando receber uma notificação de suspensão; fazer isso é uma boa maneira de reduzir o risco de perda de dados devido à perda de energia ou uma falha.
Se você estiver desenvolvendo seu jogo com o XDK, quando um usuário sai, o objeto de usuário para que o usuário permaneça válido e, nesse momento, o aplicativo pode realizar final salve operações usando o armazenamento conectado.

## <a name="robustness"></a>Robustez

Como os dados salvos é sempre sincronizados para a nuvem, um bug no código do aplicativo que faz com que o aplicativo falha e de dados salvos poderia ser feito o backup na nuvem e distribuído entre dispositivos. Para impedir que os usuários tenham um aplicativo que falha em cada inicialização, projetar o aplicativo para garantir que:

-   Os usuários podem acessar um ponto no aplicativo no qual eles podem gerenciar o estado salvo, mesmo se alguns dados salvos estão malformados.
-   O aplicativo pode lidar com dados corrompidos automaticamente, recuperando o máximo possível de dados e reinicializar todo o resto para um estado seguro.

## <a name="use-cases-for-save-game-designs"></a>Casos de uso para designs de jogo de salvar

O design de um jogo de salvar o sistema que faz melhor uso de contêineres no espaço de armazenamento conectados depende do tipo de aplicativo:

### <a name="single-save"></a>Salvar único

Para aplicativos que usam um único, estilo de campanha de salvar o sistema, como uma primeira shooter de pessoa:

-   Colocar todos os dados em um único contêiner e gravar sempre no mesmo contêiner, identificado pelo nome.
-   Considere a possibilidade de expor uma opção para redefinir todos os dados que limpará todos os dados salvos de um usuário, caso queira começar a experimentar o aplicativo desde o início, nenhum progresso anterior que está sendo mantido.

### <a name="multiple-saves"></a>Vários salva

Para aplicativos que têm um número fixo de salvar slots, vamos dizer que cinco, há duas maneiras de usar contêineres para salvar dados de jogos:

-   Store todos os 5 slots dentro de um contêiner de nome fixo usando blob de 1 para cada slot de salvamento. Usando esse método, todos os slots de 5 serão totalmente sincronizados e disponível, ou, no caso em que a sincronização falhar em qualquer momento, nenhum dos slots serão sincronizados e permanecerão em seu estado anterior. Se um usuário executa o aplicativo offline em dois consoles diferentes, salvando progresso no slot 1 no console do primeiro e no slot 2 no console do segundo, o usuário deve escolher quais dados devem ser retidos sobre como conectar os dois consoles Xbox Live; a lógica de mesclagem para contêineres produzirá um conflito.
-   Store cada slot em um contêiner com seu próprio nome. Isso permite que o progresso independente em cada slot, até mesmo em vários computadores que podem estar offline. No entanto, se um usuário cancela parcialmente por meio de uma sincronização, é possível que apenas alguns dos slots estará disponível durante a sessão; alguns dos contêineres podem não ter sido concluída baixar. Nesse caso, o usuário é notificado de que a sincronização estava incompleta e alguns dos dados de nuvem não está no console local.

A abordagem é usada, o aplicativo deve fornecer o usuário com a interface do usuário para excluir salva individual de slots.

### <a name="warning"></a>Aviso

**Não armazene dados dependentes em contêineres.** Não armazene dados com as dependências entre mais de um contêiner. Contêineres podem ser separados devido à sincronização incompleta, perda de energia ou outras condições de erro. Dados armazenados em um único contêiner devem ser autossuficientes e autoconsistente.

### <a name="tips"></a>Dicas

**Não é desencorajar os usuários de desativar o console ou navegar externamente.** O título não deve desencorajar para os usuários de desativar o console ou navegar para fora de seu aplicativo ao salvar. No Xbox 360, se o usuário desliga o sistema enquanto está salvando seu título, os dados do usuário não será salvo. No Xbox One, seu título recebe um evento de suspensão e tem 1 segundo para usar a API de armazenamento conectados para salvar o estado. O sistema garante que os dados são confirmados corretamente para o disco rígido antes de ele completamente desligado ou entra em seu estado de baixa energia. O mesmo processo de suspensão ocorre se o usuário ejetar o disco do seu título para reproduzir o outro.

**Manter espaços de armazenamento conectados.** Manter objetos ConnectedStorageSpace em vez de tentar carregá-los sempre que ocorrer um evento de leitura ou gravação. Não há nenhum efeito negativo no desempenho ou a robustez causada, mantendo um objeto ConnectedStorageSpace por um longo período.

**Mantenha os tamanhos de dados pequenos.** Manter o tamanho dos dados salvos pequeno. Todos os dados de usuário no armazenamento conectado é carregado para a nuvem quando o console estiver online. Otimize seus formatos de dados para garantir que os atrasos mínimo e o uso de largura de banda.

**Verifique se que os usuários não se importa sem salvar.** Verifique se há erros de OutOfLocalStorage retornados de GetForUserAsync e SubmitUpdatesAsync e os usuários da consulta para ver se eles realmente querem jogar sem salvar. Se um usuário indicar que deseja salvar jogos, repita a operação.

**Verifique a cota do usuário e o prompt para limpar o espaço.** Verifique se há um erro de QuotaExceeded retornado por SubmitUpdatesAsync. Se seu aplicativo recebe essa mensagem, notifique os usuários que não é possível salvar mais dados até que a liberação de espaço e apresentação-los com a interface do usuário que permite que eles façam isso. Cada usuário obtém 64 ou 256 MB de dados por aplicativo, e cada título XDK obtém 64 MB de armazenamento por máquina que é local para o console.

**Salve o estado dos menus de restauração mais tarde.** Salve o estado de menu e outras configurações de aplicativo, além de salvar os dados de jogos de núcleo. Se o usuário desempenha o outro aplicativo e, em seguida, volta para o seu, restaurá-los para um estado de menu contextualmente apropriadas.
Responda às alterações do usuário conectado. Os usuários podem entrar enquanto seu aplicativo está suspenso. Quando o aplicativo é retomado, ele deve determinar se o conjunto de usuários conectado foi alterado. Considere a possibilidade de navegar para um local apropriado dentro do aplicativo, como um menu, quando isso ocorre.

**Fornecem a interface do usuário para o gerenciamento de dados salvos.** Seu aplicativo deve fornecer uma interface do usuário que permite aos usuários gerenciar seus dados salvos dentro do aplicativo. Para aplicativos com o sistema de salvamento automático, o aplicativo deve oferecer uma opção para redefinir os dados salvos para permitir que os usuários redefinam para um estado de execução padrão.

**Certifique-se de que os usuários sempre podem acessar a interface do usuário para gerenciar os jogos salvos.** Certifique-se de que seu aplicativo pode sempre alcançar a sua interface do usuário de gerenciamento para jogos salvos, mesmo na presença de dados salvos é cheio de bugs. Se salva dados de um usuário se tornar ilegíveis devido a uma aplicativo bug ou corrupção de dados, o aplicativo deve permitir que os usuários recuperem a um estado que não falham ou impedi-los de execução.