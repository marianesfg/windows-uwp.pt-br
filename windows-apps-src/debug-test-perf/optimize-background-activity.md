---
author: PatrickFarley
ms.assetid: 24351dad-2ee3-462a-ae78-2752bb3374c2
title: Utilizar os recursos de economia de bateria
description: Crie apps UWP que funcionem com o sistema para usar tarefas em segundo plano economizando a bateria.
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 045dfeb4696a4854b114d88da2a2cbb75d621a58
ms.lasthandoff: 02/07/2017

---

# <a name="optimize-background-activity"></a>Otimizar a atividade em segundo plano

Os aplicativos universais do Windows devem funcionar consistentemente bem em todas as famílias de dispositivos. Em dispositivos alimentados por bateria, o consumo de energia é um fator fundamental na experiência geral do usuário com seu app. Uma bateria que dure um dia inteiro é um recurso desejável para cada usuário, mas isso exige a eficiência de todos os softwares instalados no dispositivo, incluindo seus próprios. 

O comportamento da tarefa em segundo plano certamente é o maior fator no custo total de energia de um app. Uma tarefa em segundo plano é qualquer atividade do programa que tenha sido registrada no sistema para executar sem que o app seja aberto. Consulte [Criar e registrar uma tarefa em segundo plano fora do processo](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task) para obter mais informações.

## <a name="background-activity-allowance"></a>Provisão de atividades em segundo plano

No Windows 10, versão 1607, os usuários podem ver o "uso da bateria por app" na seção **Bateria** do app Configurações. Aqui, ele verá uma lista de apps e a porcentagem da duração da bateria (fora a carga da bateria que foi usada desde a última cobrança) que cada app tem consumido. 

![uso da bateria por app](images/battery-usage-by-app.png)

Para os apps UWP dessa lista, os usuários têm certo controle sobre como o sistema trata suas atividades em segundo plano. A atividade em segundo plano pode ser especificada com o valor "Sempre permitido", "Gerenciado pelo Windows" (a configuração padrão) ou "Nunca permitido" (mais detalhes sobre isso abaixo). Use o valor de enumeração **BackgroundAccessStatus** retornado do método [**BackgroundExecutionManager.RequestAccessAsync()**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync.aspx) para ver que provisão a atividade em segundo plano seu app tem.

![permissões de tarefas em segundo plano](images/background-task-permissions.png)

Tudo isso é para dizer que se seu app não implementa o gerenciamento responsável de atividades em segundo plano, o usuário pode negar permissões em segundo plano para seu app por completo, o que não é desejável para ambas as partes.

## <a name="work-with-the-battery-saver-feature"></a>Trabalhar com o recurso de economia de bateria
Economia de Bateria é um recurso do sistema que os usuários podem definir em Configurações. Isso corta todas as atividades em segundo plano de todos os apps quando o nível de bateria fica abaixo de um limite definido pelo usuário, *exceto* para a atividade em segundo plano de apps que foram definidos como "Sempre permitido".

Se seu app estiver marcado como "Gerenciado pelo Windows" e chamar **BackgroundExecutionManager.RequestAccessAsync()** para registrar uma atividade em segundo plano enquanto a Economia de Bateria estiver ativada, ele retornará um valor **DeniedSubjectToSystemPolicy**. Seu app deve resolver isso notificando o usuário que as tarefas em segundo plano não serão executadas até que a Economia de Bateria seja desativada e que elas foram registradas novamente no sistema. Se uma tarefa em segundo plano já tiver sido registrada para executar, e a Economia de Bateria estiver ativada no momento de seu gatilho, a tarefa não será executada e o usuário não será notificado. Para reduzir a chance de isso acontecer, a prática recomendada é programar seu app para registrar novamente suas tarefas em segundo plano sempre que ele for aberto.

Embora o gerenciamento de atividades em segundo plano seja o principal objetivo do recurso Economia de Bateria, seu app pode fazer ajustes adicionais para conservar ainda mais a energia quando a Economia de Bateria está ativada. Verifique o status do modo de Economia de Bateria no app consultando a propriedade [**PowerManager.PowerSavingMode**](https://msdn.microsoft.com/library/windows/apps/windows.phone.system.power.powermanager.powersavingmode.aspx). É um valor de enumeração: **PowerSavingMode.Off** ou **PowerSavingMode.On**. Quando a Economia de Bateria está ativada, seu app pode reduzir o uso de animações, parar a sondagem de localização ou atrasar sincronizações e backups. 

## <a name="further-optimize-background-tasks"></a>Otimizar ainda mais as tarefas em segundo plano
As etapas adicionais a seguir devem ser seguidas ao registrar suas tarefas em segundo plano para torná-las mais cientes do consumo da bateria.

Use um gatilho de manutenção. O objeto [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.maintenancetrigger.aspx) pode ser usado em vez de um objeto [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.systemtrigger.aspx) para determinar quando uma tarefa em segundo plano é iniciada. As tarefas que usam gatilhos de manutenção serão executadas somente quando o dispositivo estiver conectado à alimentação CA e podem ser executadas por mais tempo. Consulte [Usar um gatilho de manutenção](https://msdn.microsoft.com/windows/uwp/launch-resume/use-a-maintenance-trigger) para obter instruções.

Use o tipo de condição de sistema **BackgroundWorkCostNotHigh**. As condições do sistema devem ser atendidas para que as tarefas em segundo plano sejam executadas (consulte [Definir condições para executar uma tarefa em segundo plano](https://msdn.microsoft.com/windows/uwp/launch-resume/set-conditions-for-running-a-background-task) para obter mais informações). O custo do trabalho em segundo plano é uma medida que indica o impacto de energia *relativo* da execução da tarefa em segundo plano. Uma tarefa em execução quando o dispositivo está conectado à alimentação CA pode ser marcada como **baixa** (pouco/nenhum impacto sobre a bateria). Uma tarefa em execução quando o dispositivo está funcionando com a energia da bateria com a tela desligada é marcado como **alta** porque há presumivelmente pouca atividade do programa em execução no dispositivo no momento, então, a tarefa em segundo plano teria um custo relativo maior. Uma tarefa em execução quando o dispositivo está funcionando com a energia bateria com a tela *ligada* é marcada como **média**, pois presumivelmente já existe alguma atividade do programa em execução, e a tarefa em segundo plano contribuiria um pouco mais para o custo de energia. A condição de sistema **BackgroundWorkCostNotHigh** simplesmente atrasa a capacidade de execução da tarefa até que a tela seja ligada ou o dispositivo seja conectado à alimentação CA.

## <a name="test-battery-efficiency"></a>Testar a eficiência da bateria

Certifique-se de testar seu app em dispositivos reais em quaisquer cenários de alto consumo de energia. É uma boa ideia testar seu app em vários dispositivos diferentes, com a Economia de Bateria ativada e desativada e em ambientes de intensidade de rede diferentes.

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e registrar uma tarefa em segundo plano fora do processo](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)  
* [Planejando para o desempenho](https://msdn.microsoft.com/windows/uwp/debug-test-perf/planning-and-measuring-performance)  


