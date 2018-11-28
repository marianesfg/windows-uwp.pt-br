---
title: Criar e registrar uma tarefa em segundo plano no processo
description: Crie e registre uma tarefa no processo que é executada no mesmo processo de seu app em primeiro plano.
ms.date: 11/03/2017
ms.topic: article
keywords: Windows 10, uwp, tarefa em segundo plano
ms.assetid: d99de93b-e33b-45a9-b19f-31417f1e9354
ms.localizationpriority: medium
ms.openlocfilehash: 2a59fe6056661289726fdaa6c2dd26e90d5e3fad
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7849577"
---
# <a name="create-and-register-an-in-process-background-task"></a>Criar e registrar uma tarefa em segundo plano no processo

**APIs importantes**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

Este tópico demonstra como criar e registrar uma tarefa em segundo plano que é executada no mesmo processo do seu aplicativo.

As tarefas em segundo plano no processo são mais simples de implementar do que as tarefas em segundo plano fora do processo. No entanto, elas são menos flexíveis. Se o código em execução em uma tarefa em segundo plano no processo travar, ele prejudicará seu aplicativo. Observe também que [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx), [DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) e **IoTStartupTask** não podem ser usado com o modelo no processo. Ativar uma tarefa de VoIP em segundo plano dentro de seu aplicativo também não é possível. Esses gatilhos e tarefas ainda têm suporte usando o modelo de tarefa em segundo plano fora do processo.

Lembre-se de que a atividade em segundo plano pode ser encerrada mesmo ao ser executada dentro do processo em primeiro plano do aplicativo se ele executar os limites de tempo de execução. Para algumas finalidades, a resiliência de separar o trabalho em uma tarefa em segundo plano que é executada em um processo separado ainda é útil. Manter o trabalho em segundo plano como uma tarefa separada do aplicativo em primeiro plano pode ser a melhor opção para o trabalho que não exige comunicação com o aplicativo em primeiro plano.

## <a name="fundamentals"></a>Conceitos básicos

O modelo no processo aprimora o ciclo de vida do aplicativo com notificações aprimoradas quando o aplicativo está em primeiro plano ou em segundo plano. Dois novos eventos estão disponíveis no objeto do Aplicativo para essas transições: [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) e [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground). Esses eventos se adaptam ao ciclo de vida do aplicativo com base no estado de visibilidade do seu aplicativo Leia mais sobre esses eventos e como eles afetam o ciclo de vida do aplicativo em [ciclo de vida do aplicativo](app-lifecycle.md).

Em um nível alto, você manipulará o evento **EnteredBackground** para executar seu código que será executado enquanto seu aplicativo é executado em segundo plano e manipule **LeavingBackground** para saber quando seu aplicativo foi movido para o primeiro plano.

## <a name="register-your-background-task-trigger"></a>Registre o gatilho da tarefa em segundo plano

A atividade em segundo plano no processo é registrada de modo semelhante às atividades em segundo plano fora do processo. Todos os gatilhos em segundo plano começam com o registro usando [BackgroundTaskBuilder](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundtaskbuilder.aspx?f=255&MSPPError=-2147217396). O construtor facilita o registro de uma tarefa em segundo plano por meio da definição de todos os valores necessários em um só lugar:

> [!div class="tabbedCodeSnippets"]
> ```cs
> var builder = new BackgroundTaskBuilder();
> builder.Name = "My Background Trigger";
> builder.SetTrigger(new TimeTrigger(15, true));
> // Do not set builder.TaskEntryPoint for in-process background tasks
> // Here we register the task and work will start based on the time trigger.
> BackgroundTaskRegistration task = builder.Register();
> ```

> [!NOTE]
> Os aplicativos Universais do Windows devem chamar [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar qualquer tipo de gatilho em segundo plano.
> Para garantir que seu aplicativo Universal do Windows continue a ser executado corretamente depois que você liberar uma atualização, chame [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) e, em seguida, chame [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) quando seu aplicativo for iniciado após a atualização. Para obter mais informações, consulte [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md).

Para atividades em segundo plano no processo, não defina `TaskEntryPoint.` Deixar em branco habilita o ponto de entrada padrão, um novo método protegido no objeto Application chamado [OnBackgroundActivated()](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx).

Quando um gatilho estiver registrado, será acionado com base no tipo de disparador definido no método [SetTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundtaskbuilder.settrigger.aspx). No exemplo acima, é usado um [TimeTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.timetrigger.aspx) que será acionado 15 minutos a partir do momento em que ele foi registrado.

## <a name="add-a-condition-to-control-when-your-task-will-run-optional"></a>Adicione uma condição para controlar quando sua tarefa será executada (opcional)

Você pode adicionar uma condição para controlar quando sua tarefa será executada após a ocorrência de um evento de gatilho. Por exemplo, se você não quiser que a tarefa seja executada até que o usuário esteja presente, use a condição **UserPresent**. Para obter uma lista das possíveis condições, consulte [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

O código de exemplo abaixo atribui uma condição que exige que o usuário esteja presente:

> [!div class="tabbedCodeSnippets"]
> ```cs
> builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
> ```

## <a name="place-your-background-activity-code-in-onbackgroundactivated"></a>Coloque seu código de atividade em segundo plano em OnBackgroundActivated()

Coloque o código de atividade em segundo plano em [OnBackgroundActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) para responder ao seu gatilho em segundo plano quando ele for acionado. **OnBackgroundActivated** pode ser tratado como [ibackgroundtask. Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396). O método tem um parâmetro [BackgroundActivatedEventArgs](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.activation.backgroundactivatedeventargs.aspx) , que contém tudo o que fornece o método **Run** . Por exemplo, em App.xaml.cs:

``` cs
using Windows.ApplicationModel.Background;

...

sealed partial class App : Application
{
  ...

  protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args)
  {
      base.OnBackgroundActivated(args);
      IBackgroundTaskInstance taskInstance = args.TaskInstance;
      DoYourBackgroundWork(taskInstance);  
  }
}
```

Para obter um exemplo de **OnBackgroundActivated** mais sofisticado, consulte [converter um serviço de aplicativo para ser executado no mesmo processo de seu aplicativo host](convert-app-service-in-process.md).

## <a name="handle-background-task-progress-and-completion"></a>Manipular progresso e conclusão de tarefas em segundo plano

O progresso e a conclusão da tarefa podem ser monitorados da mesma forma para tarefas em segundo plano de vários processos (veja [Monitorar o progresso e a conclusão da tarefa em segundo plano](monitor-background-task-progress-and-completion.md)), mas você provavelmente achará que você pode rastreá-los mais facilmente usando variáveis para acompanhar o status de progresso ou conclusão em seu aplicativo. Essa é uma das vantagens de ter o código de atividade em segundo plano em execução no mesmo processo de seu aplicativo.

## <a name="handle-background-task-cancellation"></a>Manipular cancelamento de tarefas em segundo plano

As tarefas em segundo plano no processo serão canceladas da mesma forma que as tarefas em segundo plano fora do processo (consulte [Manipular uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)). Lembre-se que seu manipulador de eventos **BackgroundActivated** deve sair antes de ocorrer o cancelamento ou todo o processo será finalizado. Se seu aplicativo em primeiro plano fechar inesperadamente quando você cancelar a tarefa em segundo plano, verifique se o manipulador foi encerrado antes de ocorrer o cancelamento.

## <a name="the-manifest"></a>O manifesto

Ao contrário das tarefas em segundo plano fora do processo, você não precisa adicionar informações sobre a tarefa em segundo plano no manifesto do pacote para executar tarefas em segundo plano no processo.

## <a name="summary-and-next-steps"></a>Resumo e próximas etapas

Agora, você deve compreender as noções básicas de como escrever uma tarefa em segundo plano no processo.

Veja os seguintes tópicos relacionados para obter referência de API, diretriz conceitual de tarefa em segundo plano e instruções mais detalhadas para escrever aplicativos que usam tarefas em segundo plano.

## <a name="related-topics"></a>Tópicos relacionados

**Tópicos de instruções detalhadas de tarefa em segundo plano**

* [Converter uma tarefa em segundo plano fora do processo em uma tarefa em segundo plano no processo](convert-out-of-process-background-task.md)
* [Criar e registrar uma tarefa em segundo plano fora do processo](create-and-register-a-background-task.md)
* [Reproduzir mídia em segundo plano](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)
* [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md)
* [Registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md)
* [Usar um gatilho de manutenção](use-a-maintenance-trigger.md)
* [Manipular uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)
* [Executar uma tarefa em segundo plano em um temporizador](run-a-background-task-on-a-timer-.md)

**Diretrizes da tarefa em segundo plano**

* [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md)
* [Depurar uma tarefa em segundo plano](debug-a-background-task.md)
* [Como disparar eventos de suspensão, retomada e segundo plano em aplicativos UWP (durante a depuração)](http://go.microsoft.com/fwlink/p/?linkid=254345)

**Referência de API de tarefa em segundo plano**

* [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)
