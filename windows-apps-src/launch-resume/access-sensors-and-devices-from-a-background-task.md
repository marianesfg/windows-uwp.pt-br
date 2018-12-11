---
title: Acessar sensores e dispositivos a partir de uma tarefa em segundo plano
description: O DeviceUseTrigger permite que seu aplicativo universal do Windows acesse sensores e dispositivos periféricos em segundo plano, mesmo quando seu app em primeiro plano estiver suspenso.
ms.assetid: B540200D-9FF2-49AF-A224-50877705156B
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, tarefa em segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: 13bc8f2558b3e3f15d7329697a41b177777b6e7a
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8891256"
---
# <a name="access-sensors-and-devices-from-a-background-task"></a>Acessar sensores e dispositivos a partir de uma tarefa em segundo plano




[**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) permite que seu aplicativo Universal do Windows acesse sensores e dispositivos periféricos em segundo plano, mesmo quando seu app em primeiro plano estiver suspenso. Por exemplo, dependendo de onde o seu aplicativo estiver sendo executado, ele poderá usar uma tarefa em segundo plano para sincronizar dados com dispositivos ou monitorar sensores. Para ajudar a economizar a duração da bateria e assegurar o consentimento do usuário adequado, o uso do [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) está sujeito a políticas descritas neste tópico.

Para acessar sensores ou dispositivos periféricos em segundo plano, crie uma tarefa em segundo plano que use o [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337). Para ver um exemplo que mostra como isso é feito em um computador, consulte [Custom USB device sample](http://go.microsoft.com/fwlink/p/?LinkId=301975 ). Para obter um exemplo em um telefone, consulte o [Exemplo de sensores em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=393307).

> [!Important]
> **DeviceUseTrigger** não pode ser usado com tarefas em segundo plano no processo. As informações neste tópico se aplicam somente a tarefas em segundo plano que são executadas fora do processo.

## <a name="device-background-task-overview"></a>Visão geral da tarefa em segundo plano do dispositivo

Quando o aplicativo não estiver mais visível para o usuário, o Windows suspenderá ou encerrará seu aplicativo para recuperar memória e recursos da CPU. Isso permite que outros aplicativos sejam executados em primeiro plano e reduz o consumo de bateria. Quando isso acontece, sem a ajuda de uma tarefa em segundo plano, todos os eventos de dados em andamento são perdidos. O Windows oferece o gatilho de tarefa em segundo plano, [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337), para permitir que o seu aplicativo execute sincronia de longa execução e monitore operações em dispositivos e sensores em segundo plano, mesmo que o aplicativo esteja suspenso. Para saber mais sobre o ciclo de vida do aplicativo, consulte [Iniciando, retomada e as tarefas em segundo plano](index.md). Para saber mais sobre tarefas em segundo plano, consulte [Support your app with background tasks](support-your-app-with-background-tasks.md).

**Observação**em um aplicativo Universal do Windows, sincronizar um dispositivo em segundo plano exige que o usuário tenha aprovado a sincronização em segundo plano pelo seu aplicativo. O dispositivo deve também estar conectado ou vinculado ao computador, com E/S ativa e ter permissão de um máximo de 10 minutos de atividade em segundo plano. Mais detalhes sobre imposição da política serão descritos posteriormente neste tópico.

### <a name="limitation-critical-device-operations"></a>Limitação: operações de dispositivo críticas

Algumas operações críticas de dispositivos, como atualizações de firmware de longa execução não podem ser executadas com o [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337). Elas podem ser executadas somente no computador por um aplicativo privilegiado que usa o [**DeviceServicingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297315). Um *aplicativo privilegiado* é um aplicativo que o fabricante do dispositivo autorizou a executar tais operações. Os metadados do dispositivo são usados para especificar qual aplicativo, se houver, foi designado como o aplicativo privilegiado para um dispositivo. Para obter mais informações, consulte [Sincronização e atualização de dispositivos para aplicativos de dispositivos da Microsoft Store](http://go.microsoft.com/fwlink/p/?LinkId=306619).

## <a name="protocolsapis-supported-in-a-deviceusetrigger-background-task"></a>Protocolos/APIs com suporte em uma tarefa em segundo plano do DeviceUseTrigger.

Tarefas em segundo plano que usam o [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) permitem que o seu aplicativo se comunique através de muitos protocolos/APIs, sendo que a maioria deles não tem suporte para tarefas em segundo plano disparadas pelo sistema. Há suporte em aplicativos Universal do Windows para os seguintes itens.

| Protocolo         | DeviceUseTrigger em um aplicativo Universal do Windows                                                                                                                                                    |
|------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| USB              | ![esse protocolo funciona em.](images/ap-tools.png)                                                                                                                                            |
| HID              | ![esse protocolo funciona em.](images/ap-tools.png)                                                                                                                                            |
| Bluetooth RFCOMM | ![esse protocolo funciona em.](images/ap-tools.png)                                                                                                                                            |
| GATT Bluetooth   | ![esse protocolo funciona em.](images/ap-tools.png)                                                                                                                                            |
| MTP              | ![esse protocolo funciona em.](images/ap-tools.png)                                                                                                                                            |
| Rede com fio    | ![esse protocolo funciona em.](images/ap-tools.png)                                                                                                                                            |
| Rede e Wi-Fi    | ![esse protocolo funciona em.](images/ap-tools.png)                                                                                                                                            |
| IDeviceIOControl | ![deviceservicingtrigger oferece suporte a ideviceiocontrol](images/ap-tools.png)                                                                                                                       |
| API de sensores      | ![deviceservicingtrigger oferece suporte a apis de sensores universais](images/ap-tools.png) (limitado a sensores na [família de dispositivos universais](https://msdn.microsoft.com/library/windows/apps/dn894631)) |

## <a name="registering-background-tasks-in-the-app-package-manifest"></a>Registrando tarefas em segundo plano no manifesto do pacote do aplicativo

O seu aplicativo executará operações de sincronização e atualização em código que é executado como parte de uma tarefa em segundo plano. Esse código está incorporado em uma classe do Windows Runtime que implementa [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) (ou em uma página dedica JavaScript ou a aplicativos JavaScript). Para usar uma tarefa em segundo plano do [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337), o seu aplicativo deve declarar isso no arquivo de manifesto do aplicativo, como faz para tarefas em segundo plano disparadas pelo sistema.

Neste exemplo de manifesto de pacote do aplicativo, **DeviceLibrary.SyncContent** é o ponto de entrada para uma tarefa em segundo plano que usa o [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337).

```xml
<Extensions>
  <Extension Category="windows.backgroundTasks" EntryPoint="DeviceLibrary.SyncContent">
    <BackgroundTasks>
      <m2:Task Type="deviceUse" />
    </BackgroundTasks>
  </Extension>
</Extensions>
```

## <a name="introduction-to-using-deviceusetrigger"></a>Introdução ao uso do DeviceUseTrigger

Para usar o [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337), siga estas etapas básicas. Para saber mais sobre tarefas em segundo plano, consulte [Support your app with background tasks](support-your-app-with-background-tasks.md).

1.  Seu aplicativo registra sua tarefa em segundo plano no manifesto do aplicativo e incorpora o código da tarefa em segundo plano em uma classe do Windows Runtime que implementa IBackgroundTask ou em uma página JavaScript dedicada a aplicativos JavaScript.
2.  Quando o seu aplicativo iniciar, ele criará e configurará um objeto trigger do tipo [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337), e armazenará a instância do gatilho para uso futuro.
3.  Seu aplicativo verifica se a tarefa em segundo plano já foi registrada antes. Caso contrário, ele registra o gatilho novamente. Observe que o seu aplicativo não tem permissão para configurar condições sobre a tarefa associada a este gatilho.
4.  Quando o seu aplicativo precisar disparar a tarefa em segundo plano, ele deve chamar primeiro o [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) para verificar se o aplicativo é capaz de solicitar uma tarefa em segundo plano.
5.  Se o aplicativo puder solicitar uma tarefa em segundo plano, o método de ativação [**RequestAsync**](https://msdn.microsoft.com/library/windows/apps/dn297341) no objeto trigger do dispositivo.
6.  Sua tarefa em segundo plano não é acelerada como outras tarefas em segundo plano (não há cota de tempo na CPU), mas será executada com prioridade reduzida para manter os aplicativos em primeiro plano capazes de responder.
7.  O Windows fará a validação com base no tipo de gatilho que as políticas necessárias foram atendidas, incluindo a solicitação do consentimento do usuário para a operação antes de iniciar a tarefa em segundo plano.
8.  O Windows monitora as condições do sistema e o tempo de execução da tarefa e, se necessário, cancela a tarefa se as condições necessárias não forem mais atendidas.
9.  Quando as tarefas em segundo plano relatarem progresso ou conclusão, o seu aplicativo receberá esses eventos através de eventos em progresso e concluídos na tarefa registrada.

**Importante**  ao usar o [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337), considere esses pontos importantes:

-   A capacidade de disparar de forma programática tarefas em segundo plano que usam o [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) foi introduzida no Windows 8.1 e Windows Phone 8.1.

-   O Windows faz cumprir algumas políticas para assegurar o consentimento do usuário ao atualizar dispositivos periféricos no computador.

-   Fazem-se cumprir políticas adicionais para economizar a duração da bateria do usuário ao sincronizar e atualizar dispositivos periféricos.

-   Tarefas em segundo plano que usam o [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) podem ser canceladas pelo Windows quando certos requisitos da política não são atendidos, incluindo um tempo de segundo plano máximo (tempo cronológico). É importante considerar os requisitos dessa política ao se usar essas tarefas em segundo plano para interagir com o seu dispositivo periférico.

**Dica**para ver como as tarefas em segundo plano trabalham, baixe uma amostra. Para ver um exemplo que mostra como isso é feito em um computador, consulte [Custom USB device sample](http://go.microsoft.com/fwlink/p/?LinkId=301975 ). Para obter um exemplo em um telefone, consulte o [Exemplo de sensores em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=393307).
 
## <a name="frequency-and-foreground-restrictions"></a>Frequência e restrições de primeiro plano

Não há restrições quanto à frequência com que o seu aplicativo pode iniciar operações, mas ele pode executar apenas uma operação de tarefa em segundo plano [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) por vez (isso não afeta outros tipos de tarefas em segundo plano) e iniciar uma tarefa em segundo plano somente quando o seu aplicativo estiver em primeiro plano. Quando o seu aplicativo não estiver em primeiro plano, ele não pode iniciar uma tarefa em segundo plano com **DeviceUseTrigger**. Seu aplicativo não pode iniciar uma segunda tarefa em segundo plano do **DeviceUseTrigger** antes de a primeira tarefa em segundo plano ser concluída.

## <a name="device-restrictions"></a>Restrições de dispositivo

Enquanto cada aplicativo estiver limitado a registrar e executar apenas uma tarefa em segundo plano do [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337), o dispositivo (no qual o seu aplicativo está sendo executado pode permitir que vários aplicativos registrem e executem tarefas em segundo plano do **DeviceUseTrigger**. Dependendo do dispositivo, pode haver limite no número total de tarefas em segundo plano do **DeviceUseTrigger** de todos os aplicativos. Isso ajuda a economizar bateria nos dispositivos com restrições de recursos. Para ver mais detalhes, consulte a tabela a seguir.

A partir de uma única tarefa em segundo plano do [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337), o seu aplicativo pode acessar um número ilimitado de dispositivos ou sensores - limitado apenas pelas APIs e protocolos com suporte listados na tabela anterior.

## <a name="background-task-policies"></a>Políticas de tarefas em segundo plano

O Windows faz cumprir políticas quando o seu aplicativo usa uma tarefa em segundo plano do [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337). Se essas políticas não forem atendidas, a tarefa em segundo plano pode ser cancelada. É importante considerar os requisitos dessa política ao se usar esse tipo de tarefa em segundo plano para interagir com dispositivos ou sensores.

### <a name="task-initiation-policies"></a>Políticas de início de tarefa

Esta tabela indica quais políticas de início de tarefa se aplicam a um aplicativo Universal do Windows.

| Política | DeviceUseTrigger em um aplicativo Universal do Windows |
|--------|---------------------------------------------|
| O seu aplicativo está em primeiro plano ao acionar a tarefa em segundo plano. | ![a política se aplica](images/ap-tools.png) |
| O dispositivo está conectado ao sistema (ou no intervalo de um dispositivo sem fio). | ![a política se aplica](images/ap-tools.png) |
| O dispositivo é acessível pelo aplicativo usando as APIs periféricas do dispositivo suportado (as APIs do Windows Runtime para USB, HID, Bluetooth, Sensores etc.). Se o seu aplicativo não puder acessar o dispositivo ou o sensor, o acesso à tarefa em segundo plano será negado. | ![a política se aplica](images/ap-tools.png) |
| O ponto de entrada de tarefa em segundo plano fornecido pelo aplicativo está registrado no manifesto do pacote do aplicativo. | ![a política se aplica](images/ap-tools.png) |
| Apenas uma tarefa em segundo plano do [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/dn297337) está em execução por aplicativo. | ![a política se aplica](images/ap-tools.png) |
| O número máximo de tarefas em segundo plano do [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/dn297337) ainda não foi alcançado no dispositivo (em que o seu app está sendo executado). | **Família de dispositivos da área de trabalho**: um número ilimitado de tarefas pode ser registrado e executado paralelamente. **Família de dispositivos móveis**: uma tarefa em um dispositivo de 512 MB; do contrário, duas tarefas podem ser registradas e executadas paralelamente. |
| O número máximo de dispositivos periféricos e sensores que o seu aplicativo pode acessar de uma única tarefa em segundo plano do [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/dn297337) ao usar as APIs/protocolos com suporte. | ilimitado |
| Sua tarefa em segundo plano consome 400 ms de tempo de CPU (supondo uma CPU de 1 GHz) a cada minuto quando a tela estiver bloqueada, ou a cada 5 minutos quando a tela não estiver bloqueada. A falha ao atender essa política pode resultar no cancelamento da tarefa. | ![a política se aplica](images/ap-tools.png) |

### <a name="runtime-policy-checks"></a>Verificações da política de tempo de execução

O Windows impõe os requisitos da política de tempo de execução a seguir, enquanto sua tarefa está em execução em segundo plano. Se algum dos requisitos de tempo de execução deixar de ser verdadeiro, o Windows cancelará sua tarefa em segundo plano no dispositivo.

Esta tabela indica quais políticas de tempo de execução se aplicam a um aplicativo Universal do Windows.

| Verificação de política | DeviceUseTrigger em um aplicativo Universal do Windows |
|--------------|:-------------------------------------------:|
| O dispositivo está conectado ao sistema (ou no alcance de um dispositivo sem fio). | ![a verificação de política se aplica](images/ap-tools.png) |
| A tarefa está executando E/S regular para o dispositivo (1 E/S a cada 5 segundos). | ![a verificação de política se aplica](images/ap-tools.png) |
| O aplicativo não cancelou a tarefa. | ![a verificação de política se aplica](images/ap-tools.png) |
| Limite de tempo no relógio de parede – a quantidade total de tempo em que a tarefa do app pode ser executada em segundo plano. | **Família de dispositivos da área de trabalho:**: 10 minutos. **Família de dispositivos móveis:**: sem limite de tempo. Para conservar os recursos, não mais do que uma ou duas tarefas podem ser executadas de uma vez. |
| O aplicativo não foi encerrado. | ![a verificação de política se aplica](images/ap-tools.png) |

## <a name="best-practices"></a>Práticas recomendadas

As seguintes práticas são recomendadas para aplicativos que usam as tarefas em segundo plano do [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337).

### <a name="programming-a-background-task"></a>Programando uma tarefa em segundo plano

Usar a tarefa em segundo plano do [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) do seu dispositivo assegura que qualquer operação de sincronização ou monitoramento iniciada a partir do aplicativo em primeiro plano continue sendo executada no segundo plano caso os seus usuários troquem de aplicativo e o seu aplicativo em primeiro plano seja suspenso pelo Windows. Recomendamos que você siga esse modelo geral para registrar, disparar e cancelar o registro das tarefas em segundo plano:

1.  Chame [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) para verificar se o aplicativo é capaz de solicitar uma tarefa em segundo plano. Isso deve ser feito antes de se registrar uma tarefa em segundo plano.

2.  Registre a tarefa em segundo plano antes de solicitar o gatilho.

3.  Conecte manipuladores de eventos em andamento e concluídos ao seu gatilho. Quando o seu aplicativo retornar da suspensão, o Windows fornecerá ao seu aplicativo todos os eventos em andamento ou concluídos na fila que podem ser usados para determinar o status de suas tarefas em segundo plano.

4.  Escolha objeto aberto de dispositivo ou sensor quando disparar sua tarefa em segundo plano do [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) para que tais dispositivos e sensores possa ser abertos e usados pela sua tarefa em segundo plano.

5.  Registre o gatilho.

6.  Considere cuidadosamente o impacto na bateria com relação ao acesso a um dispositivo ou sensor a partir de uma tarefa em segundo plano. Por exemplo, executar o intervalo de relatório de um sensor com muita frequência poderia fazer com que a tarefa fosse executada com tanta frequência que isso esgotaria rapidamente a bateria do telefone.

7.  Quando sua tarefa em segundo plano for concluída, cancele o registro dela.

8.  Registrar para eventos de cancelamento a partir de sua classe de tarefas em segundo plano. Registrar para eventos de cancelamento permitirá que o código de sua tarefa em segundo plano pare corretamente a execução de sua tarefa em segundo plano quando cancelada pelo Windows ou seu aplicativo em primeiro plano.

9.  Na saída do aplicativo (não na suspensão), cancele o registro e cancele todas as tarefas em execução se o seu aplicativo não precisar mais delas. Em sistemas com restrições de recursos, como telefones com pouca memória, isso permitirá que outros aplicativos usem uma tarefa em segundo plano do [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337).

    -   Quando seu aplicativo sai, cancela o registro e tarefas em execução.

    -   Quando o seu aplicativo for encerrado, suas tarefas em segundo plano serão canceladas e todos os manipuladores de eventos existentes serão desconectados de suas tarefas em segundo plano existentes. Isso impedirá que você determine o estado de suas tarefas em segundo plano. Cancelar o registro e cancelar a tarefa em segundo plano permitirá que seu código de cancelamento pare corretamente suas tarefas em segundo plano.

### <a name="cancelling-a-background-task"></a>Cancelar uma tarefa em segundo plano

Para cancelar uma tarefa em execução no segundo plano a partir do seu aplicativo em primeiro plano, use o método Unregister no objeto [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786) que você usa no seu aplicativo para registrar a tarefa em segundo plano do [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337). Cancelar o registro da sua tarefa em segundo plano usando o método [**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229869) em **BackgroundTaskRegistration** levará a infraestrutura da tarefa em segundo plano a cancelá-la.

O método [**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229869), adicionalmente, leva um valor Booleano verdadeiro ou falso para indicar se instâncias em execução na sua tarefa em segundo plano devem ser canceladas sem deixar que elas sejam concluídas. Para obter mais informações, consulte a referência de API para **Unregister**.

Além de [**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229869), o seu aplicativo também precisará chamar [**BackgroundTaskDeferral.Complete**](https://msdn.microsoft.com/library/windows/apps/hh700504). Isso informa ao sistema que a operação assíncrona associada a uma tarefa em segundo plano foi concluída.
