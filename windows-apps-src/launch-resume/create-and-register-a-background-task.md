---
author: TylerMSFT
title: "Criar e registrar uma tarefa em segundo plano que é executada em um processo separado"
description: "Crie uma classe de tarefa em segundo plano e a registre para ser executada quando seu aplicativo não estiver em primeiro plano."
ms.assetid: 4F98F6A3-0D3D-4EFB-BA8E-30ED37AE098B
translationtype: Human Translation
ms.sourcegitcommit: 95c34f70e9610907897cfe9a2bf82aaac408e486
ms.openlocfilehash: 4eb67f8f63134ab33df79b0b98b252b2b27b2dda

---

# Criar e registrar uma tarefa em segundo plano que é executada em um processo separado

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**APIs Importantes**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

Crie uma classe de tarefa em segundo plano e a registre para ser executada quando seu aplicativo não estiver em primeiro plano. Este tópico demonstra como criar e registrar uma tarefa em segundo plano que é executada em um processo separado do seu processo em primeiro plano. Para fazer o trabalho em segundo plano diretamente no aplicativo em primeiro plano, consulte [Criar e registrar uma tarefa em segundo plano de um único processo](create-and-register-a-singleprocess-background-task.md).

> [!Note]
> Se você usar uma tarefa em segundo plano para reproduzir mídia em segundo plano, consulte [Reproduzir mídia em segundo plano](https://msdn.microsoft.com/en-us/windows/uwp/audio-video-camera/background-audio) para obter informações sobre os aprimoramentos no Windows 10, versão 1607, que tornam a tarefa mais fácil.

## Criar a classe de tarefa em segundo plano

Você pode executar código em segundo plano criando classes que implementam a interface [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794). Esse código será executado quando um evento específico for acionado pelo uso de [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839) ou [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517), por exemplo.

As seguintes etapas mostram como escrever uma nova classe que implementa a interface [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794). Antes de iniciar, crie um novo projeto na sua solução para tarefas em segundo plano. Adicione uma nova classe vazia para a sua tarefa em segundo plano e importe o namespace [Windows.ApplicationModel.Background](https://msdn.microsoft.com/library/windows/apps/br224847).

1.  Crie um novo projeto para tarefas em segundo plano e adicione-o à sua solução. Para isso, clique com botão direito do mouse no nó da solução no **Gerenciador de Soluções** e selecione Adicionar-&gt;Novo Projeto. Em seguida, selecione o tipo de projeto **Componente do Tempo de Execução do Windows (Universal do Windows)**, dê um nome ao projeto e clique em OK.
2.  Referencie o projeto das tarefa em segundo plano no projeto de aplicativo da Plataforma Universal do Windows (UWP).

    Para aplicativo em C++, clique com o botão direito do mouse no seu aplicativo de projeto e selecione **Propriedades**. Então, vá para **Propriedades comuns** e clique em **Adicionar nova referência**, marque a caixa de marcação próxima ao projeto de tarefa em segundo plano e clique em **OK** nas duas caixas de diálogo.

    Para um aplicativo C#, no seu projeto de aplicativo, clique com o botão direito em **Referências** e selecione **Adicionar nova referência**. Em **Solução**, selecione **Projetos** e clique para selecionar o nome do seu projeto de tarefa em segundo plano e clique em **Ok**.

3.  Crie uma nova classe que implemente a interface [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794). O método [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) é o ponto de entrada obrigatório que será chamado quando eventos específicos forem acionados. Esse método é obrigatório em todas as tarefas em segundo plano.

    > [!NOTE]
    > A classe da tarefa em segundo plano em si e todas as outras classes no projeto da tarefa em segundo plano precisam ser classes **públicas** **seladas**.

    O seguinte código de exemplo a seguir mostra um ponto inicial bem básico para uma classe de tarefa em segundo plano:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     //
    >     // ExampleBackgroundTask.cs
    >     //
    >
    >     using Windows.ApplicationModel.Background;
    >
    >     namespace Tasks
    >     {
    >         public sealed class ExampleBackgroundTask : IBackgroundTask
    >         {
    >             public void Run(IBackgroundTaskInstance taskInstance)
    >             {
    >                 
    >             }        
    >         }
    >     }
    > ```
    > ```cpp
    >     //
    >     // ExampleBackgroundTask.h
    >     //
    >
    >     #pragma once
    >
    >     using namespace Windows::ApplicationModel::Background;
    >
    >     namespace RuntimeComponent1
    >     {
    >         public ref class ExampleBackgroundTask sealed : public IBackgroundTask
    >         {
    >
    >         public:
    >             ExampleBackgroundTask();
    >
    >             virtual void Run(IBackgroundTaskInstance^ taskInstance);
    >             void OnCompleted(
    >                     BackgroundTaskRegistration^ task,
    >                     BackgroundTaskCompletedEventArgs^ args
    >                     );
    >         };
    >     }
    >
    >     //
    >     // ExampleBackgroundTask.cpp
    >     //
    >
    >     #include "ExampleBackgroundTask.h"
    >
    >     using namespace Tasks;
    >
    >     void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
    >     {
    >
    >     }
    >  ```

4.  Se você executar um código assíncrono na sua tarefa em segundo plano, então ela precisará usar um adiamento. Se você não usar um adiamento, o processo da tarefa em segundo plano poderá ser encerrado de forma inesperada se o método Executar for concluído antes de a chamada de método assíncrona ser concluída.

    Solicite o adiamento no método Executar antes de chamar o método assíncrono. Salve o adiamento em uma variável global para que seja acessado a partir do método assíncrono. Declare o adiamento como concluído após a conclusão do código assíncrono.

    O seguinte exemplo de código obtém um adiamento, salva-o e o libera quando o código assíncrono é concluído:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     BackgroundTaskDeferral _deferral; // Note: defined at class scope so we can mark it complete inside the OnCancel() callback if we choose to support cancellation
    >     public async void Run(IBackgroundTaskInstance taskInstance)
    >     {
    >         _deferral = taskInstance.GetDeferral()
    >         //
    >         // TODO: Insert code to start one or more asynchronous methods using the
    >         //       await keyword, for example:
    >         //
    >         // await ExampleMethodAsync();
    >         //
    >         
    >         _deferral.Complete();
    >     }
    > ```
    > ```cpp
    >     BackgroundTaskDeferral^ deferral = taskInstance->GetDeferral(); // Note: defined at class scope so we can mark it complete inside the OnCancel() callback if we choose to support cancellation
    >     void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
    >     {
    >         //
    >         // TODO: Modify the following line of code to call a real async function.
    >         //       Note that the task<void> return type applies only to async
    >         //       actions. If you need to call an async operation instead, replace
    >         //       task<void> with the correct return type.
    >         //
    >         task<void> myTask(ExampleFunctionAsync());
    >         
    >         myTask.then([=] () {
    >             deferral->Complete();
    >         });
    >     }
    > ```

> [!NOTE]
> Em C#, os métodos assíncronos da tarefa em segundo plano podem ser chamados usando as palavras-chave **async/await**. Em C++, é possível obter um resultado parecido usando uma cadeia de tarefas.

Para mais informações sobre padrões assíncronos, consulte [Programação assíncrona](https://msdn.microsoft.com/library/windows/apps/mt187335). Para exemplos adicionais sobre como usar os adiamentos para evitar que uma tarefa em segundo plano pare antecipadamente, consulte a [amostra de tarefa em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666).

O procedimento abaixo é concluído em uma de suas classes de aplicativo (por exemplo, MainPage.xaml.cs).

> [!NOTE]
> Você também pode criar uma função dedicada a registrar tarefas em segundo plano. Consulte [Registrar uma tarefa em segundo plano](register-a-background-task.md). Nesse caso, em vez de usar as próximas três etapas, você pode simplesmente construir o gatilho e oferecê-lo à função de registro junto com o nome da tarefa, o ponto de entrada da tarefa e (opcionalmente) uma condição.

## Registre a tarefa em segundo plano a ser executada

1.  Veja se a tarefa em segundo plano já está registrada iterando através da propriedade [**BackgroundTaskRegistration.AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787). Esta etapa é importante; se seu aplicativo não verificar a existência de registros de tarefas em segundo plano, ele poderá facilmente registrar a tarefa várias vezes, causando problemas de desempenho e extrapolando o tempo de CPU disponível para a tarefa antes que esta possa ser concluída.

    O seguinte exemplo itera na propriedade AllTasks e define uma variável de sinalização como true se a tarefa já tiver sido registrada:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     var taskRegistered = false;
    >     var exampleTaskName = "ExampleBackgroundTask";
    >
    >     foreach (var task in BackgroundTaskRegistration.AllTasks)
    >     {
    >         if (task.Value.Name == exampleTaskName)
    >         {
    >             taskRegistered = true;
    >             break;
    >         }
    >     }
    > ```
    > ```cpp
    >     boolean taskRegistered = false;
    >     Platform::String^ exampleTaskName = "ExampleBackgroundTask";
    >
    >     auto iter = BackgroundTaskRegistration::AllTasks->First();
    >     auto hascur = iter->HasCurrent;
    >
    >     while (hascur)
    >     {
    >         auto cur = iter->Current->Value;
    >
    >         if(cur->Name == exampleTaskName)
    >         {
    >             taskRegistered = true;
    >             break;
    >         }
    >
    >         hascur = iter->MoveNext();
    >     }
    > ```

2.  Se a tarefa em segundo plano ainda não estiver registrada, use [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) para criar uma instância de sua tarefa em segundo plano. O ponto de entrada da tarefa deve ser o nome de sua classe de tarefa em segundo plano prefixada pelo namespace.

    O gatilho da tarefa em segundo plano controla quando a tarefa será executada. Para obter uma lista dos possíveis gatilhos, consulte [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839).

    Por exemplo, este código cria uma nova tarefa em segundo plano e define-a para ser executada quando o gatilho **TimeZoneChanged** é disparado:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     var builder = new BackgroundTaskBuilder();
    >
    >     builder.Name = exampleTaskName;
    >     builder.TaskEntryPoint = "RuntimeComponent1.ExampleBackgroundTask";
    >     builder.SetTrigger(new SystemTrigger(SystemTriggerType.TimeZoneChange, false));
    > ```
    > ```cpp
    >     auto builder = ref new BackgroundTaskBuilder();
    >
    >     builder->Name = exampleTaskName;
    >     builder->TaskEntryPoint = "RuntimeComponent1.ExampleBackgroundTask";
    >     builder->SetTrigger(ref new SystemTrigger(SystemTriggerType::TimeZoneChange, false));
    > ```

3.  Você pode adicionar uma condição para controlar quando sua tarefa será executada após a ocorrência de um evento de gatilho (opcional). Por exemplo, se você não quiser que a tarefa seja executada até que o usuário esteja presente, use a condição **UserPresent**. Para obter uma lista das possíveis condições, consulte [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

    O código de exemplo abaixo atribui uma condição que exige que o usuário esteja presente:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
    > ```
    > ```cpp
    >     builder->AddCondition(ref new SystemCondition(SystemConditionType::UserPresent));
    > ```

4.  Registre a tarefa em segundo plano chamando o método Register no objeto [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768). Armazene o resultado [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786) para que ele possa ser usado na próxima etapa.

    O código a seguir registra a tarefa em segundo plano e armazena o resultado:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     BackgroundTaskRegistration task = builder.Register();
    >     ```
    > ```cpp
    >     BackgroundTaskRegistration^ task = builder->Register();
    > ```

> [!NOTE]
> Os aplicativos Universais do Windows devem chamar [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar qualquer tipo de gatilho em segundo plano.

Para garantir que seu aplicativo Universal do Windows continue sendo executado corretamente depois que você liberar uma atualização, use o gatilho **ServicingComplete** (veja [SystemTriggerType](https://msdn.microsoft.com/library/windows/apps/br224839)) para realizar quaisquer alterações de configuração pós-atualização, como migrar do banco de dados do aplicativo e registrar tarefas em segundo plano. É recomendável cancelar o registro de tarefas em segundo plano associadas à versão anterior do aplicativo (veja [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471)) e registrar tarefas em segundo plano para a nova versão do aplicativo (veja [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)) neste momento.

Para obter mais informações, consulte [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md).

## Manipular a conclusão da tarefa em segundo plano usando manipuladores de eventos

Registre um método com o [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781). Dessa forma, seu aplicativo poderá obter resultados da tarefa em segundo plano. Quando o aplicativo for iniciado ou retomado, o método mark será chamado se a tarefa em segundo plano tiver sido concluída, desde a última vez que o aplicativo estava em primeiro plano. (O método OnCompleted será chamado imediatamente se a tarefa em segundo plano for concluída enquanto o aplicativo estiver em primeiro plano.)

1.  Escreva um método OnCompleted para manipular a conclusão das tarefas em segundo plano. Por exemplo, o resultado da tarefa em segundo plano pode ocasionar uma atualização da interface do usuário. O volume do método mostrado aqui é necessário para o método do manipulador de eventos OnCompleted, mesmo que este exemplo não use o parâmetro *args*.

    O seguinte código de exemplo reconhece a conclusão da tarefa em segundo plano e chama um método de atualização de interface de usuário de exemplo que leva a cadeia de caracteres da mensagem.

     > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
    >     {
    >         var settings = Windows.Storage.ApplicationData.Current.LocalSettings;
    >         var key = task.TaskId.ToString();
    >         var message = settings.Values[key].ToString();
    >         UpdateUI(message);
    >     }
    > ```
    > ```cpp
    >     void ExampleBackgroundTask::OnCompleted(BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    >     {
    >         auto settings = ApplicationData::Current->LocalSettings->Values;
    >         auto key = task->TaskId.ToString();
    >         auto message = dynamic_cast<String^>(settings->Lookup(key));
    >         UpdateUI(message);
    >     }
    > ```

    > [!NOTE]
    > As atualizações da interface do usuário devem ser executadas assincronamente para evitar atraso do thread da interface do usuário. Por exemplo, consulte o método UpdateUI no [exemplo de tarefa em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666).


2.  Retorne para onde você registrou a tarefa em segundo plano. Depois dessa linha de código, adicione um novo objeto [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781). Forneça o método OnCompleted como o parâmetro para o construtor **BackgroundTaskCompletedEventHandler**.

    O seguinte código de exemplo adiciona um [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781) ao [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786):

     > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
    > ```
    > ```cpp
    >     task->Completed += ref new BackgroundTaskCompletedEventHandler(this, &ExampleBackgroundTask::OnCompleted);
    > ```

## Declarar que o seu aplicativo usa tarefas em segundo plano no manifesto do aplicativo

Antes de o seu aplicativo conseguir executar tarefas em segundo plano, você deve declarar cada tarefa em segundo plano no manifesto do aplicativo. Se o aplicativo tentar registrar uma tarefa em segundo plano com um disparador que não esteja listado no manifesto, o registro falhará.

1.  Abra o designer de manifesto do pacote abrindo o arquivo chamado Package.appxmanifest.
2.  Abra a guia **Declarações**.
3.  Na lista suspensa **Declarações Disponíveis**, selecione **Tarefas em Segundo Plano** e clique em **Adicionar**.
4.  Marque a caixa de seleção **Evento do sistema**.
5.  Na caixa de texto **Ponto de entrada:**, insira o namespace e o nome de sua classe de plano de fundo que, para este exemplo, é RuntimeComponent1.ExampleBackgroundTask.
6.  Feche o designer de manifesto.

    O seguinte elemento Extensions é adicionado ao arquivo Package.appxmanifest para registrar a tarefa em segundo plano:

    ```xml
    <Extensions>
      <Extension Category="windows.backgroundTasks" EntryPoint="RuntimeComponent1.ExampleBackgroundTask">
        <BackgroundTasks>
          <Task Type="systemEvent" />
        </BackgroundTasks>
      </Extension>
    </Extensions>
    ```

## Resumo e próximas etapas

Agora você deve compreender os fundamentos de como escrever uma classe de tarefa em segundo plano, como registrar a tarefa em segundo plano no aplicativo e como fazer o aplicativo reconhecer quando a tarefa em segundo plano é concluída. Você também deve saber atualizar o manifesto do aplicativo para que seu aplicativo possa registrar com êxito a tarefa em segundo plano.

> [!NOTE]
> Baixe o [exemplo de tarefa em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666) para ver exemplos de código semelhantes, no contexto de um aplicativo UWP completo e robusto, que usa tarefas em segundo plano.

Veja os seguintes tópicos relacionados para obter referência de API, diretriz conceitual de tarefa em segundo plano e instruções mais detalhadas para escrever aplicativos que usam tarefas em segundo plano.

> [!NOTE]
> Este artigo destina-se a desenvolvedores do Windows 10 que escrevem aplicativos da Plataforma Universal do Windows (UWP). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

## Tópicos relacionados

**Tópicos de instruções detalhadas de tarefa em segundo plano**

* [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md)
* [Registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md)
* [Usar um gatilho de manutenção](use-a-maintenance-trigger.md)
* [Manipular uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)
* [Executar uma tarefa em segundo plano em um temporizador](run-a-background-task-on-a-timer-.md)
* [Criar e registrar uma tarefa em segundo plano de processo único](create-and-register-a-singleprocess-background-task.md).
[Converter uma tarefa em segundo plano de vários processos em uma tarefa em segundo plano de processo único](convert-multiple-process-background-task.md)  

**Diretrizes da tarefa em segundo plano**

* [Diretrizes de tarefas em segundo plano](guidelines-for-background-tasks.md)
* [Depurar uma tarefa em segundo plano](debug-a-background-task.md)
* [Como disparar eventos de suspensão, retomada e segundo plano em aplicativos da Windows Store (durante a depuração)](http://go.microsoft.com/fwlink/p/?linkid=254345)

**Referência de API de tarefa em segundo plano**

* [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)



<!--HONumber=Aug16_HO4-->


