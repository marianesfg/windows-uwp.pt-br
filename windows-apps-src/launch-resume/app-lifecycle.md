---
author: TylerMSFT
title: Ciclo de vida do aplicativo
description: "Este tópico descreve o ciclo de vida de um aplicativo da Plataforma Universal do Windows (UWP), desde o momento em que ele for ativado até que ele seja fechado."
ms.assetid: 6C469E77-F1E3-4859-A27B-C326F9616D10
ms.sourcegitcommit: 213384a194513a0f98a5f37e7f0e0849bf0a66e2
ms.openlocfilehash: 8451942c05d5d44cafba243f7cbebceedbe86fc0

---

# Ciclo de vida do aplicativo


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**APIs importantes**

-   [**Classe Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324)
-   [**Namespace Windows.ApplicationModel.Activation**](https://msdn.microsoft.com/library/windows/apps/br224766)

Este tópico descreve o ciclo de vida de um aplicativo da Plataforma Universal do Windows (UWP), desde o momento em que ele for ativado até que ele seja fechado. Vários usuários espalham seu trabalho e reproduzem em vários dispositivos e aplicativos. Os usuários agora esperam que seu aplicativo se lembre de seu estado conforme eles executam várias tarefas no dispositivo. Por exemplo, eles esperam que a página seja rolada para a mesma posição e todos os controles estejam no mesmo estado de antes. Compreendendo o ciclo de vida do aplicativo de iniciar, suspender e retomar, você pode fornecer esse tipo de comportamento uniforme.

## Estado de execução do aplicativo


Esta ilustração representa as transições entre os estados de execução do aplicativo. Descrevemos esses estados e eventos nas várias seções a seguir. Para saber mais sobre cada transição de estado e qual deve ser a resposta do aplicativo, consulte a referência da enumeração [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694).

![diagrama de estado mostrando as transições entre estados de execução do aplicativo](images/state-diagram.png)

## Implantação


Para que um aplicativo seja ativado, ele deve primeiro ser implantado. Seu aplicativo é implantado quando um usuário instala seu aplicativo ou quando você usa o Visual Studio para compilar e executar seu aplicativo durante o desenvolvimento e o teste. Para saber mais sobre isso e sobre cenários de implantação avançada, consulte [Pacotes de aplicativos e implantação](https://msdn.microsoft.com/library/windows/apps/hh464929).

## Inicialização do aplicativo


Um aplicativo é iniciado quando está no estado **NotRunning** e o usuário toque no bloco do aplicativo na tela inicial ou na lista de aplicativos. Aplicativos usados com frequência também podem ser pré-inicializados para otimizar a capacidade de resposta (consulte [Tratar pré-inicialização do aplicativo](handle-app-prelaunch.md)). Um aplicativo pode estar no estado **NotRunning** porque nunca foi iniciado, porque estava em execução e depois travou ou porque foi suspenso e não pôde ser mantido na memória, sendo terminado pelo sistema. Iniciar é diferente de ativação. A ativação ocorre quando o aplicativo é ativado por meio de um contrato ou uma extensão, como o contrato de Pesquisa.

O método [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) é chamado quando um aplicativo é iniciado, inclusive quando o aplicativo está suspenso no momento na memória. O parâmetro [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731) contém o estado anterior do seu aplicativo e os argumentos de ativação.

Quando o usuário muda para o aplicativo terminado, o sistema envia os argumentos [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731), com [**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) definido como **Launch** e [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) definido como **Terminated** ou **ClosedByUser**. O aplicativo deve carregar seus dados salvos e atualizar o conteúdo exibido.

Se o valor de [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) for **NotRunning**, o aplicativo deverá reiniciar como se estivesse sendo iniciado pela primeira vez.

Quando um aplicativo é iniciado, o Windows exibe a tela inicial do aplicativo. Para configurar a tela inicial, consulte [Adicionando uma tela inicial](https://msdn.microsoft.com/library/windows/apps/xaml/hh465331).

Enquanto a tela inicial é exibida, seu aplicativo deve preparar sua interface do usuário. As principais tarefas do aplicativo são registrar manipuladores de eventos e configurar qualquer interface do usuário personalizada necessária para carregar a página inicial. Essas tarefas só devem levar alguns segundos. Se um aplicativo precisar solicitar dados da rede ou recuperar grandes quantidades de dados do disco, essas atividades deverão ser concluídas fora da ativação. Um aplicativo pode usar sua própria interface do usuário de carregamento personalizada ou uma tela inicial estendida enquanto aguarda a conclusão dessas operações de longa duração. Consulte [Exibir uma tela inicial por mais tempo](create-a-customized-splash-screen.md) e [Exemplo de tela inicial](http://go.microsoft.com/fwlink/p/?linkid=234889) para obter mais informações. Depois que o aplicativo conclui a ativação, ele entra no estado **Running** e a tela inicial desaparece (e todos os recursos e objetos são limpos).

## Ativação do aplicativo


Um aplicativo pode ser ativado pelo usuário por meio de diversas extensões e contratos, como o contrato de Compartilhamento. Para obter uma lista das maneiras de ativar o aplicativo, consulte [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693).

A classe [**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324) define métodos que você pode substituir para manipular os vários tipos de ativação. Vários tipos de ativação têm um método específico que você pode substituir, como [**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331), [**OnSearchActivated**](https://msdn.microsoft.com/library/windows/apps/br242336) etc. Para os demais tipos de ativação, substitua o método [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330).

O código de ativação de seu aplicativo pode verificar por que foi ativado e se já estava no estado **Running**.

O aplicativo pode restaurar dados salvos anteriormente durante a ativação no caso de o sistema operacional ter terminado o aplicativo e de o usuário tê-lo reiniciado em seguida. O Windows poderá encerrar o aplicativo depois de o ter suspendido por vários motivos. O usuário pode fechar manualmente seu aplicativo, ou sair, ou o sistema pode estar sendo executado com poucos recursos. Se o usuário iniciar o aplicativo depois de terminado pelo Windows, o aplicativo receberá um retorno de chamada [**Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) e o usuário verá a tela inicial do aplicativo até que este seja ativado. Você pode usar esse evento para determinar se o aplicativo precisa restaurar os dados que ele salvou na última vez em que foi suspenso ou se você deve carregar os dados padrão do aplicativo. Como a tela inicial está em exibição, o código do aplicativo pode alocar tempo de processamento para realizar isso sem que haja atraso aparente para o usuário, embora preocupações já mencionadas sobre operações de execução longa também se apliquem caso você esteja reiniciando ou continuando.

Os dados do evento [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) incluem uma propriedade [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) que informa em qual estado o aplicativo estava antes de ser ativado. Essa propriedade é um dos valores da enumeração [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694):

| Motivo do término                                                        | Valor da propriedade [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) | Ação a ser tomada          |
|-------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|-------------------------|
| Terminado pelo sistema (por exemplo, devido a restrições de recursos)       | **Terminado**                                                                                          | Restaurar dados de sessão    |
| Fechado pelo usuário ou terminado pelo usuário via um processo                             | **ClosedByUser**                                                                                        | Iniciar com dados padrão |
| Término inesperado ou o aplicativo não foi executado durante a *sessão de usuário atual* | **NotRunning**                                                                                          | Iniciar com dados padrão |

 

**Observação**
            *A sessão de usuário atual* se baseia no logon no Windows. Desde que o usuário atual não tenha explicitamente desconectado, desligado, ou o Windows não tenha sido reiniciado por outros motivos, a sessão de usuário atual persiste em eventos, como autenticação de tela de bloqueio, troca de usuário etc.

 

[
              **PreviousExecutionState**
            ](https://msdn.microsoft.com/library/windows/apps/br224729) também pode ter um valor **Running** ou **Suspended**, mas nesses casos o aplicativo não foi encerrado anteriormente e, portanto, você não precisa restaurar nenhum dado porque tudo já está na memória.

**Observação**  

Se você fizer logon usando a conta do administrador do computador, não poderá ativar nenhum aplicativo UWP.

Para saber mais, consulte [Extensões de aplicativo](https://msdn.microsoft.com/library/windows/apps/hh464906).

### **OnActivated** versus ativações específicas

O método [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) é o meio pelo qual devem ser tratados todos os tipos de ativação possíveis. No entanto, é mais comum usar métodos diferentes para tratar a maioria dos tipos de ativação e usar **OnActivated** somente como o método de contingência para os tipos de ativação menos comuns. Por exemplo, [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324) tem um método [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) que é chamado como um retorno de chamada sempre que [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693) é **Launch**, e essa é a ativação típica para a maioria dos aplicativos. Há mais 6 métodos **On\*** para ativações específicas: [**OnCachedFileUpdaterActivated**](https://msdn.microsoft.com/library/windows/apps/hh701797), [**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331), [**OnFileOpenPickerActivated**](https://msdn.microsoft.com/library/windows/apps/hh701799), [**OnFileSavePickerActivated**](https://msdn.microsoft.com/library/windows/apps/hh701801), [**OnSearchActivated**](https://msdn.microsoft.com/library/windows/apps/br242336), [**OnShareTargetActivated**](https://msdn.microsoft.com/library/windows/apps/hh701806). Os modelos iniciais para um aplicativo XAML têm uma implementação para **OnLaunched** e um manipulador para [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341).

## Suspensão de aplicativo


O sistema suspende o aplicativo sempre que o usuário alternar para outro aplicativo, para a área de trabalho ou para a tela inicial. Seu aplicativo também pode ser suspenso quando o dispositivo entra em um estado de baixo consumo de energia. O sistema retoma o seu aplicativo sempre que o usuário alterna de volta para ele. Quando o sistema retoma o aplicativo, o conteúdo das variáveis e estruturas de dados é o mesmo de antes da suspensão do aplicativo pelo sistema. O sistema restaura o aplicativo exatamente como ele havia parado, de maneira que o usuário tem impressão de que ele estava sendo executado em segundo plano.

Quando o usuário move um aplicativo para o segundo plano, o Windows aguarda alguns segundos para ver se o usuário voltará imediatamente ao aplicativo para que a transição seja rápida. Se o usuário não voltar dentro dessa janela de tempo, o Windows suspenderá o aplicativo.

Se for necessário realizar trabalho assíncrono quando o aplicativo estiver suspenso, você precisará adiar a conclusão da suspensão até que o trabalho seja concluído. Você pode usar o método [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690) no objeto [**SuspendingOperation**](https://msdn.microsoft.com/library/windows/apps/br224688) (disponível via argumentos de evento) para atrasar a conclusão da suspensão até que você chame o método [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) no objeto retornado [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684).

O sistema tenta manter o aplicativo e seus dados na memória enquanto ele está suspenso. Entretanto, caso não tenha recursos suficientes para manter o aplicativo na memória, o sistema encerra o aplicativo. Os aplicativos não recebem notificações de que estão sendo encerrados. Sendo assim, a única oportunidade que você tem para salvar os dados do aplicativo é durante a suspensão. Quando um aplicativo determina que foi ativado depois de ter sido encerrado, ele deve carregar os dados salvos durante a suspensão, para que o aplicativo fique no mesmo estado que antes da suspensão. Quando o usuário retorna a um aplicativo suspenso que tenha sido terminado, o aplicativo deve restaurar seus dados de aplicativo no método [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335). O sistema não notifica um aplicativo quando é terminado, por isso seu aplicativo deve salvar seus dados de aplicativo e liberar recursos exclusivos e identificadores de arquivo quando suspenso, e restaurá-los quando ativado após o término.

Se um aplicativo tiver registrado um manipulador de evento para o evento [**Application.Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341), esse código será chamado imediatamente antes da suspensão do aplicativo. Você pode usar o manipulador de eventos para salvar o aplicativo e os dados do usuário. Recomenda-se que você use as APIs de dados de aplicativo com essa finalidade porque há garantia de que elas serão concluídas antes que o aplicativo entre no estado **Suspended**. Para obter mais informações, consulte [Armazene e recupere configurações e outros dados de aplicativo](https://msdn.microsoft.com/library/windows/apps/mt299098).

Você também deve liberar recursos e identificadores de arquivo exclusivos para que outros aplicativos tenham acesso a eles quando o seu aplicativo não os estiver usando. Exemplos de recursos exclusivos incluem câmeras, dispositivos de E/S, dispositivos externos e recursos de rede. Liberar explicitamente os indicadores de arquivos e recursos exclusivos ajuda a garantir que outros aplicativos possam acessá-los quando não estiverem sendo usados pelo seu aplicativo. Quando o aplicativo for ativado após o encerramento, ele deverá reabrir seus indicadores de arquivos e recursos exclusivos.

Normalmente, seu aplicativo deve salvar seu estado e liberar recursos e identificadores de arquivo imediatamente ao manipular o evento de suspensão, e o código não deve levar mais de um segundo para ser concluído. Se um aplicativo não retornar do evento de suspensão dentro de alguns segundos, o Windows entenderá que o aplicativo parou de responder e o terminará.

Existem alguns cenários em que o aplicativo deve continuar a ser executado para concluir tarefas em segundo plano. Por exemplo, seu aplicativo pode continuar a reproduzir o áudio em segundo plano; para saber mais, consulte [Áudio em segundo plano](https://msdn.microsoft.com/library/windows/apps/mt282140). Além disso, as operações de transferência em segundo plano continuam mesmo se o seu aplicativo estiver suspenso ou for encerrado. Para obter mais informações, consulte [Como baixar um arquivo](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj152726.aspx#downloading_a_file_using_background_transfer).

Para obter diretrizes, consulte [Início, retomada e tarefas em segundo plano](https://msdn.microsoft.com/library/windows/apps/hh465088).

**Observação sobre a depuração com Visual Studio:  **o Visual Studio impede que o Windows suspenda um aplicativo que esteja anexado ao depurador. Isso ocorre para permitir que o usuário exiba a interface de usuário de depuração do Visual Studio enquanto o aplicativo está em execução. Quando você está depurando um aplicativo, é possível enviar a ele um evento de suspensão usando o Visual Studio. Verifique se a barra de ferramentas **Local de Depuração** está sendo mostrada e clique no ícone de **Suspender**.

## Visibilidade do aplicativo


Quando o usuário muda do seu aplicativo para outro, seu aplicativo não fica mais visível mas permanece no estado **Running** até que o Windows o suspenda. Se o usuário mudar do seu aplicativo para outro mas ativá-lo ou retornar a ele antes que seja suspenso, seu aplicativo não fica mais visível mas permanece no estado **Running**.

Seu aplicativo não recebe um evento de ativação quando a visibilidade é alterada porque o aplicativo ainda está em execução. O Windows simplesmente alterna de/para o aplicativo conforme necessário. Se o aplicativo precisar realizar alguma operação quando o usuário mudar de aplicativo e voltar, trate o evento [**Window.VisibilityChanged**](https://msdn.microsoft.com/library/windows/apps/hh702458).

Não confie em uma ordem específica desses eventos.

## Retomada do aplicativo


Um aplicativo suspenso pode ser retomado quando o usuário alterna para ele ou quando é o aplicativo ativo quando o dispositivo sai de um estado de baixo consumo de energia.

Para uma enumeração dos estados que seu aplicativo pode assumir quando for retomado, consulte [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694). Quando um aplicativo é retomado a partir do estado **Suspended**, ele entra no estado **Running** e continua de onde estava quando foi suspenso. Nenhum dado de aplicativo armazenado na memória é perdido. Portanto, a maioria dos aplicativos não precisa fazer nada na retomada. No entanto, o aplicativo pôde ter ficado suspenso por horas ou mesmo dias. Se o aplicativo tiver conexões de conteúdo ou de rede que possam ter ficado obsoletas, elas deverão ser atualizadas quando o aplicativo for retomado. Se um aplicativo tiver registrado um manipulador de evento para o evento [**Application.Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339), ele será chamado quando o aplicativo for retomado a partir do estado **Suspended**. Você pode atualizar o conteúdo e os dados do aplicativo usando esse manipulador de evento.

Se um aplicativo suspenso for ativado para participar de uma extensão ou de contratos do aplicativo, ele receberá primeiro o evento **Resuming** e, depois, o evento **Activated**.

Enquanto um aplicativo estiver suspenso, ele não receberá nenhum evento da rede para os quais tenha se registrado para receber. Esses eventos de rede não são colocados em fila, eles são simplesmente perdidos. Sendo assim, o aplicativo deve testar o status da rede quando for retomado.

**Observação**  Como o evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) não é gerado do thread da interface do usuário, um dispatcher deve ser usado se o código no manipulador de retomada se comunicar com a sua interface.

 

Para obter diretrizes, consulte [Início, retomada e tarefas em segundo plano](https://msdn.microsoft.com/library/windows/apps/hh465088).

## Fechamento do aplicativo


Em geral, os usuários não precisam fechar os aplicativos; eles podem deixar que o Windows os gerencie. No entanto, os usuários podem optar por fechar um aplicativo usando o gesto de fechar ou pressionando Alt+F4 no Windows, ou usando o Alternador de Tarefas no Windows Phone.

Não há nenhum evento especial para indicar que o usuário fechou o aplicativo.

Depois que um aplicativo é fechado pelo usuário, ele é suspenso e, em seguida, terminado, e entra no estado **NotRunning**.

No Windows 8.1 e versões posteriores, depois que um aplicativo é fechado pelo usuário, o aplicativo é removido da tela e da lista de mudança, mas não é explicitamente terminado.

Se um aplicativo tiver registrado um manipulador de evento para o evento **Suspending**, ele será chamado quando o aplicativo for suspenso. Você pode usar esse manipulador de evento para salvar dados relevantes do aplicativo e do usuário no armazenamento persistente.

**Fechamento de fechamento pelo usuário:  **se o aplicativo precisar realizar tarefas diferentes quando é fechado pelo usuário e quando é fechado pelo Windows, use o manipulador de evento para determinar se o aplicativo foi encerrado pelo usuário ou pelo Windows. Consulte as descrições dos estados **ClosedByUser** e **Terminated** na referência da enumeração [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694).

Recomendamos que os aplicativos não sejam fechados programaticamente a menos que isso seja absolutamente necessário. Por exemplo, se um aplicativo detectar um vazamento de memória, ele poderá fechar para garantir a segurança dos dados pessoais do usuário. Quando você fecha um aplicativo de forma programática, o sistema trata isso como uma falha de aplicativo.

## Falha de aplicativo


A experiência de falha do sistema é projetada para que os usuários voltem ao que eles estavam fazendo o mais rápido possível. Você não deve fornecer uma caixa de diálogo de aviso ou outra notificação porque isso atrasará o usuário.

Se o aplicativo falhar, parar de responder ou gerar uma exceção, um relatório do problema será enviado para a Microsoft segundo as [configurações de comentários e diagnóstico](http://go.microsoft.com/fwlink/p/?LinkID=614828). A Microsoft fornece um subconjunto dos dados de erro no relatório de problema para você para que você possa usá-lo para melhorar o aplicativo. Você pode ver esses dados na página Qualidade do aplicativo no Painel.

Quando o usuário ativa um aplicativo após um travamento, o manipulador de evento de ativação recebe em [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694) o valor **NotRunning**, e deve exibir os dados e a interface do usuário iniciais. Após um travamento, não use sistematicamente os dados do aplicativo que usaria para **Resuming** com **Suspended** porque esses dados podem estar corrompidos; consulte [Guidelines for app suspend and resume](https://msdn.microsoft.com/library/windows/apps/hh465088).

## Remoção do aplicativo


Quando um usuário exclui seu aplicativo, ele é removido, juntamente com os dados locais. A remoção de um aplicativo não afeta os dados do usuário que foram armazenados em locais comuns, como as bibliotecas de Documentos ou Imagens.

## Ciclo de vida do aplicativo e os modelos de projeto do Visual Studio


O código básico que é relevante para o ciclo de vida do aplicativo é fornecido nos modelos de projeto iniciais do Visual Studio. O aplicativo básico manipula a ativação de inicialização, oferece um local para restaurar os dados do aplicativo e exibe a interface do usuário principal, mesmo antes que você adicione seu próprio código. Para obter mais informações, veja [Modelos de projeto em C#, VB e C++ para aplicativos](https://msdn.microsoft.com/library/windows/apps/hh768232).

## APIS chave do ciclo de vida do aplicativo


-   [
              Namespace **Windows.ApplicationModel**
            ](https://msdn.microsoft.com/library/windows/apps/br224691)
-   [
              Namespace **Windows.ApplicationModel.Activation**
            ](https://msdn.microsoft.com/library/windows/apps/br224766)
-   [
              Namespace **Windows.ApplicationModel.Core**
            ](https://msdn.microsoft.com/library/windows/apps/br205865)
-   [
              Classe **Windows.UI.Xaml.Application**
            ](https://msdn.microsoft.com/library/windows/apps/br242324) (XAML)
-   [
              Classe **Windows.UI.Xaml.Window**
            ](https://msdn.microsoft.com/library/windows/apps/br209041) (XAML)

**Observação**  
Este artigo se destina a desenvolvedores do Windows 10 que escrevem aplicativos da Plataforma Universal do Windows (UWP). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Tópicos relacionados


* [Diretrizes para suspensão e retomada de aplicativo](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [Manipular pré-inicialização do aplicativo](handle-app-prelaunch.md)
* [Manipular a ativação do aplicativo](activate-an-app.md)
* [Manipular a suspensão do aplicativo](suspend-an-app.md)
* [Manipular a retomada do aplicativo](resume-an-app.md)

 

 



<!--HONumber=Jun16_HO5-->


