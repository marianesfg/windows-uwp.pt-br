---
author: jwmsft
ms.assetid: 24351dad-2ee3-462a-ae78-2752bb3374c2
title: Otimizar a atividade em segundo plano
description: Crie apps UWP que funcionem com o sistema para usar tarefas em segundo plano economizando a bateria.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 932dd3c89933eab9baefe6ff2c45359db6efbb14
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6972394"
---
# <a name="optimize-background-activity"></a>Otimizar a atividade em segundo plano

Os aplicativos universais do Windows devem funcionar consistentemente bem em todas as famílias de dispositivos. Em dispositivos alimentados por bateria, o consumo de energia é um fator fundamental na experiência geral do usuário com seu aplicativo. Uma bateria que dure um dia inteiro é um recurso desejável para cada usuário, mas isso exige a eficiência de todos os softwares instalados no dispositivo, incluindo seus próprios. 

O comportamento da tarefa em segundo plano certamente é o fator mais importante no custo total de energia de um app. Uma tarefa em segundo plano é qualquer atividade do programa que tenha sido registrada no sistema para executar sem que o aplicativo seja aberto. Veja [Criar e registrar uma tarefa em segundo plano fora do processo](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task) para saber mais.

## <a name="background-activity-permissions"></a>Permissões de atividade em segundo plano

Em dispositivos desktop e móveis que executam o Windows 10, versão 1607 ou posteriores, os usuários podem exibir seu "uso da bateria pelo app" na seção Bateria do app Configurações. Aqui, ele verá uma lista de aplicativos e a porcentagem da duração da bateria que cada app consumiu (fora a carga da bateria que foi usada desde a última cobrança). Para os aplicativos UWP incluídos nessa lista, os usuários podem selecionar o app para abrir os controles relacionados à atividade em segundo plano.

![uso da bateria por app](images/battery-usage-by-app.png)

### <a name="background-permissions-on-mobile"></a>Permissões em segundo plano no celular

Em dispositivos móveis, os usuários verão uma lista de botões de opção que especificam a configuração de permissão de tarefas em segundo plano para o app. A atividade em segundo plano pode ser definida como "Sempre permitido," "Nunca permitido," ou "Gerenciado pelo Windows", o que significa que a atividade em segundo plano do app é controlada pelo sistema de acordo com inúmeros fatores. 

![Botões de opção de permissões de tarefas em segundo plano](images/background-task-permissions.png)

### <a name="background-permissions-on-desktop"></a>Permissões em segundo plano no desktop

Em dispositivos desktop, a configuração "Gerenciado pelo Windows" é apresentada como um botão de alternância, definido como **Ativado** por padrão. Se o usuário alternar para **Desativado**, verá uma caixa de seleção com a qual ele poderá definir manualmente as permissões de atividade em segundo plano. Quando a caixa é marcada, o app poderá executar tarefas em segundo plano em todos os momentos. Quando a caixa for desmarcada, a atividade em segundo plano será desabilitada.

![botão de permissões de tarefa em segundo plano ativado](images/background-task-permissions-on.png)

![botão de permissões de tarefa em segundo plano desativado](images/background-task-permissions-off.png)

Em seu aplicativo, você pode usar o valor de enumeração [**BackgroundAccessStatus**](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.background.backgroundaccessstatus) retornado por uma chamada para o método [**BackgroundExecutionManager.RequestAccessAsync()**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync.aspx) para determinar sua configuração de permissão de atividades em segundo plano atual.

Tudo isso é para dizer que se seu aplicativo não implementa o gerenciamento responsável de atividades em segundo plano, o usuário pode negar permissões em segundo plano para seu aplicativo por completo, o que não é desejável para ambas as partes. Se seu app tiver permissão negada para ser executado em segundo plano, mas exigir que a atividade em segundo plano seja concluído uma ação para o usuário; você pode notificar o usuário e apontá-los para o app Configurações. Isso pode ser realizado por [Iniciar o app Configurações](https://docs.microsoft.com/en-us/windows/uwp/launch-resume/launch-settings-app) para a página Apps de Segundo Plano ou Detalhes de Uso da Bateria.

## <a name="work-with-the-battery-saver-feature"></a>Trabalhar com o recurso de economia de bateria
Economia de Bateria é um recurso do sistema que os usuários podem definir em Configurações. Isso corta todas as atividades em segundo plano de todos os aplicativos quando o nível de bateria fica abaixo de um limite definido pelo usuário, *exceto* para a atividade em segundo plano de aplicativos que foram definidos como "Sempre permitido".

Verifique o status do modo de Economia de Bateria no app ao consultar a propriedade [**PowerManager.EnergySaverStatus**](https://docs.microsoft.com/en-us/uwp/api/windows.system.power.energysaverstatus). É um valor de enumeração: **EnergySaverStatus.Disabled**, **EnergySaverStatus.Off** ou **EnergySaverStatus.On**. Se seu app exigir atividade em segundo plano e não estiver definido como "Sempre permitido", ele deverá manipular **EnergySaverStatus.On** ao notificar o usuário de que as tarefas em segundo plano determinadas não serão executadas até que a Economia de Bateria seja desativada. Embora o gerenciamento de atividades em segundo plano seja o principal objetivo do recurso Economia de Bateria, seu aplicativo pode fazer ajustes adicionais para conservar ainda mais a energia quando a Economia de Bateria está ativada.  Quando a Economia de Bateria está ativada, seu aplicativo pode reduzir o uso de animações, parar a sondagem de localização ou atrasar sincronizações e backups. 

## <a name="further-optimize-background-tasks"></a>Otimizar ainda mais as tarefas em segundo plano
As etapas adicionais a seguir devem ser seguidas ao registrar suas tarefas em segundo plano para torná-las mais cientes do consumo da bateria.

### <a name="use-a-maintenance-trigger"></a>Usar um gatilho de manutenção 
O objeto [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.maintenancetrigger.aspx) pode ser usado em vez de um objeto [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.systemtrigger.aspx) para determinar quando uma tarefa em segundo plano é iniciada. As tarefas que usam gatilhos de manutenção serão executadas somente quando o dispositivo estiver conectado à alimentação CA e podem ser executadas por mais tempo. Consulte [Usar um gatilho de manutenção](https://msdn.microsoft.com/windows/uwp/launch-resume/use-a-maintenance-trigger) para obter instruções.

### <a name="use-the-backgroundworkcostnothigh-system-condition-type"></a>Use o tipo de condição de sistema **BackgroundWorkCostNotHigh**.
As condições do sistema devem ser atendidas para que as tarefas em segundo plano sejam executadas (consulte [Definir condições para executar uma tarefa em segundo plano](https://msdn.microsoft.com/windows/uwp/launch-resume/set-conditions-for-running-a-background-task) para obter mais informações). O custo do trabalho em segundo plano é uma medida que indica o impacto de energia *relativo* da execução da tarefa em segundo plano. Uma tarefa em execução quando o dispositivo está conectado à alimentação CA pode ser marcada como **baixa** (pouco/nenhum impacto sobre a bateria). Uma tarefa em execução quando o dispositivo está funcionando com a energia da bateria com a tela desligada é marcado como **alta** porque há presumivelmente pouca atividade do programa em execução no dispositivo no momento, então, a tarefa em segundo plano teria um custo relativo maior. Uma tarefa em execução quando o dispositivo está funcionando com a energia bateria com a tela *ligada* é marcada como **média**, pois presumivelmente já existe alguma atividade do programa em execução, e a tarefa em segundo plano contribuiria um pouco mais para o custo de energia. A condição de sistema **BackgroundWorkCostNotHigh** simplesmente atrasa a capacidade de execução da tarefa até que a tela seja ligada ou o dispositivo seja conectado à alimentação CA.

## <a name="test-battery-efficiency"></a>Testar a eficiência da bateria

Teste seu aplicativo em dispositivos reais em quaisquer cenários de alto consumo de energia. É uma boa ideia testar seu aplicativo em vários dispositivos diferentes, com a Economia de Bateria ativada e desativada e em ambientes de intensidade de rede diferentes.

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e registrar uma tarefa em segundo plano fora do processo](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)  
* [Planejando para o desempenho](https://msdn.microsoft.com/windows/uwp/debug-test-perf/planning-and-measuring-performance)  

