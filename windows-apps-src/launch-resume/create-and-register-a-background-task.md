---
title: Criar e registrar uma tarefa em segundo plano fora do processo
description: Crie uma classe de tarefa em segundo plano fora do processo e a registre para ser executada quando seu app não estiver em primeiro plano.
ms.assetid: 4F98F6A3-0D3D-4EFB-BA8E-30ED37AE098B
ms.date: 02/27/2019
ms.topic: article
keywords: windows 10, uwp, background task
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 2124a7141740ef9c16273714864587feff4268f3
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258684"
---
# <a name="create-and-register-an-out-of-process-background-task"></a>Criar e registrar uma tarefa em segundo plano fora do processo

**APIs importantes**

-   [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
-   [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
-   [**BackgroundTaskCompletedEventHandler**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler)

Crie uma classe de tarefa em segundo plano e a registre para ser executada quando seu aplicativo não estiver em primeiro plano. Este tópico demonstra como criar e registrar uma tarefa em segundo plano que é executada em um processo separado do seu aplicativo. Para fazer o trabalho em segundo plano diretamente no aplicativo em primeiro plano, consulte [Criar e registrar uma tarefa em segundo plano no processo](create-and-register-an-inproc-background-task.md).

> [!NOTE]
> Se você usar uma tarefa em segundo plano para reproduzir mídia em segundo plano, consulte [Reproduzir mídia em segundo plano](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio) para obter informações sobre os aprimoramentos no Windows 10, versão 1607, que tornam a tarefa mais fácil.

## <a name="create-the-background-task-class"></a>Criar a classe de tarefa em segundo plano

Você pode executar código em segundo plano criando classes que implementam a interface [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask). This code runs when a specific event is triggered by using, for example, [**SystemTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) or [**MaintenanceTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger).

As seguintes etapas mostram como escrever uma nova classe que implementa a interface [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask).

1.  Crie um novo projeto para tarefas em segundo plano e adicione-o à sua solução. To do this, right-click on your solution node in the **Solution Explorer** and select **Add** \> **New Project**. Then select the **Windows Runtime Component** project type, name the project, and click OK.
2.  Referencie o projeto das tarefa em segundo plano no projeto de aplicativo da Plataforma Universal do Windows (UWP). For a C# or C++ app, in your app project, right-click on **References** and select **Add New Reference**. Em **Solução**, selecione **Projetos** e clique para selecionar o nome do seu projeto de tarefa em segundo plano e clique em **Ok**.
3.  To the background tasks project, add a new class that implements the [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) interface. The [**IBackgroundTask.Run**](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) method is a required entry point that will be called when the specified event is triggered; this method is required in every background task.

> [!NOTE]
> The background task class itself&mdash;and all other classes in the background task project&mdash;need to be **public** classes that are **sealed** (or **final**).

The following sample code shows a very basic starting point for a background task class.

```csharp
// ExampleBackgroundTask.cs
using Windows.ApplicationModel.Background;

namespace Tasks
{
    public sealed class ExampleBackgroundTask : IBackgroundTask
    {
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            
        }        
    }
}
```

```cppwinrt
// First, add ExampleBackgroundTask.idl, and then build.
// ExampleBackgroundTask.idl
namespace Tasks
{
    [default_interface]
    runtimeclass ExampleBackgroundTask : Windows.ApplicationModel.Background.IBackgroundTask
    {
        ExampleBackgroundTask();
    }
}

// ExampleBackgroundTask.h
#pragma once

#include "ExampleBackgroundTask.g.h"

namespace winrt::Tasks::implementation
{
    struct ExampleBackgroundTask : ExampleBackgroundTaskT<ExampleBackgroundTask>
    {
        ExampleBackgroundTask() = default;

        void Run(Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance);
    };
}

namespace winrt::Tasks::factory_implementation
{
    struct ExampleBackgroundTask : ExampleBackgroundTaskT<ExampleBackgroundTask, implementation::ExampleBackgroundTask>
    {
    };
}

// ExampleBackgroundTask.cpp
#include "pch.h"
#include "ExampleBackgroundTask.h"

namespace winrt::Tasks::implementation
{
    void ExampleBackgroundTask::Run(Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance)
    {
        throw hresult_not_implemented();
    }
}
```

```cpp
// ExampleBackgroundTask.h
#pragma once

using namespace Windows::ApplicationModel::Background;

namespace Tasks
{
    public ref class ExampleBackgroundTask sealed : public IBackgroundTask
    {

    public:
        ExampleBackgroundTask();

        virtual void Run(IBackgroundTaskInstance^ taskInstance);
        void OnCompleted(
            BackgroundTaskRegistration^ task,
            BackgroundTaskCompletedEventArgs^ args
        );
    };
}

// ExampleBackgroundTask.cpp
#include "ExampleBackgroundTask.h"

using namespace Tasks;

void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
{
}
```

4.  Se você executar um código assíncrono na sua tarefa em segundo plano, então ela precisará usar um adiamento. If you don't use a deferral, then the background task process can terminate unexpectedly if the **Run** method returns before any asynchronous work has run to completion.

Request the deferral in the **Run** method before calling the asynchronous method. Save the deferral to a class data member so that it can be accessed from the asynchronous method. Declare o adiamento como concluído após a conclusão do código assíncrono.

The following sample code gets the deferral, saves it, and releases it when the asynchronous code is complete.

```csharp
BackgroundTaskDeferral _deferral; // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation
public async void Run(IBackgroundTaskInstance taskInstance)
{
    _deferral = taskInstance.GetDeferral();
    //
    // TODO: Insert code to start one or more asynchronous methods using the
    //       await keyword, for example:
    //
    // await ExampleMethodAsync();
    //

    _deferral.Complete();
}
```

```cppwinrt
// ExampleBackgroundTask.h
...
private:
    Windows::ApplicationModel::Background::BackgroundTaskDeferral m_deferral{ nullptr };

// ExampleBackgroundTask.cpp
...
Windows::Foundation::IAsyncAction ExampleBackgroundTask::Run(
    Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance)
{
    m_deferral = taskInstance.GetDeferral(); // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation.
    // TODO: Modify the following line of code to call a real async function.
    co_await ExampleCoroutineAsync(); // Run returns at this point, and resumes when ExampleCoroutineAsync completes.
    m_deferral.Complete();
}
```

```cpp
void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
{
    m_deferral = taskInstance->GetDeferral(); // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation.

    //
    // TODO: Modify the following line of code to call a real async function.
    //       Note that the task<void> return type applies only to async
    //       actions. If you need to call an async operation instead, replace
    //       task<void> with the correct return type.
    //
    task<void> myTask(ExampleFunctionAsync());

    myTask.then([=]() {
        m_deferral->Complete();
    });
}
```

> [!NOTE]
> Em C#, os métodos assíncronos da tarefa em segundo plano podem ser chamados usando as palavras-chave **async/await**. In C++/CX, a similar result can be achieved by using a task chain.

Para mais informações sobre padrões assíncronos, consulte [Programação assíncrona](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps). Para exemplos adicionais sobre como usar os adiamentos para evitar que uma tarefa em segundo plano pare antecipadamente, consulte a [amostra de tarefa em segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask).

O procedimento abaixo é concluído em uma de suas classes de aplicativo (por exemplo, MainPage.xaml.cs).

> [!NOTE]
> You can also create a function dedicated to registering background tasks&mdash;see [Register a background task](register-a-background-task.md). In that case, instead of using the next three steps, you can simply construct the trigger and provide it to the registration function along with the task name, task entry point, and (optionally) a condition.

## <a name="register-the-background-task-to-run"></a>Registre a tarefa em segundo plano a ser executada

1.  Find out whether the background task is already registered by iterating through the [**BackgroundTaskRegistration.AllTasks**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.alltasks) property. Esta etapa é importante; se seu aplicativo não verificar a existência de registros de tarefas em segundo plano, ele poderá facilmente registrar a tarefa várias vezes, causando problemas de desempenho e extrapolando o tempo de CPU disponível para a tarefa antes que esta possa ser concluída.

The following example iterates on the **AllTasks** property and sets a flag variable to true if the task is already registered.

```csharp
var taskRegistered = false;
var exampleTaskName = "ExampleBackgroundTask";

foreach (var task in BackgroundTaskRegistration.AllTasks)
{
    if (task.Value.Name == exampleTaskName)
    {
        taskRegistered = true;
        break;
    }
}
```

```cppwinrt
std::wstring exampleTaskName{ L"ExampleBackgroundTask" };

auto allTasks{ Windows::ApplicationModel::Background::BackgroundTaskRegistration::AllTasks() };

bool taskRegistered{ false };
for (auto const& task : allTasks)
{
    if (task.Value().Name() == exampleTaskName)
    {
        taskRegistered = true;
        break;
    }
}

// The code in the next step goes here.
```

```cpp
boolean taskRegistered = false;
Platform::String^ exampleTaskName = "ExampleBackgroundTask";

auto iter = BackgroundTaskRegistration::AllTasks->First();
auto hascur = iter->HasCurrent;

while (hascur)
{
    auto cur = iter->Current->Value;

    if(cur->Name == exampleTaskName)
    {
        taskRegistered = true;
        break;
    }

    hascur = iter->MoveNext();
}
```

2.  Se a tarefa em segundo plano ainda não estiver registrada, use [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) para criar uma instância de sua tarefa em segundo plano. O ponto de entrada da tarefa deve ser o nome de sua classe de tarefa em segundo plano prefixada pelo namespace.

O gatilho da tarefa em segundo plano controla quando a tarefa será executada. Para obter uma lista dos possíveis gatilhos, consulte [**SystemTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType).

For example, this code creates a new background task and sets it to run when the **TimeZoneChanged** trigger occurs:

```csharp
var builder = new BackgroundTaskBuilder();

builder.Name = exampleTaskName;
builder.TaskEntryPoint = "Tasks.ExampleBackgroundTask";
builder.SetTrigger(new SystemTrigger(SystemTriggerType.TimeZoneChange, false));
```

```cppwinrt
if (!taskRegistered)
{
    Windows::ApplicationModel::Background::BackgroundTaskBuilder builder;
    builder.Name(exampleTaskName);
    builder.TaskEntryPoint(L"Tasks.ExampleBackgroundTask");
    builder.SetTrigger(Windows::ApplicationModel::Background::SystemTrigger{
        Windows::ApplicationModel::Background::SystemTriggerType::TimeZoneChange, false });
    // The code in the next step goes here.
}
```

```cpp
auto builder = ref new BackgroundTaskBuilder();

builder->Name = exampleTaskName;
builder->TaskEntryPoint = "Tasks.ExampleBackgroundTask";
builder->SetTrigger(ref new SystemTrigger(SystemTriggerType::TimeZoneChange, false));
```

3.  Você pode adicionar uma condição para controlar quando sua tarefa será executada após a ocorrência de um evento de gatilho (opcional). Por exemplo, se você não quiser que a tarefa seja executada até que o usuário esteja presente, use a condição **UserPresent**. Para obter uma lista das possíveis condições, consulte [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType).

O código de exemplo abaixo atribui uma condição que exige que o usuário esteja presente:

```csharp
builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
```

```cppwinrt
builder.AddCondition(Windows::ApplicationModel::Background::SystemCondition{ Windows::ApplicationModel::Background::SystemConditionType::UserPresent });
// The code in the next step goes here.
```

```cpp
builder->AddCondition(ref new SystemCondition(SystemConditionType::UserPresent));
```

4.  Registre a tarefa em segundo plano chamando o método Register no objeto [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder). Armazene o resultado [**BackgroundTaskRegistration**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) para que ele possa ser usado na próxima etapa.

O código a seguir registra a tarefa em segundo plano e armazena o resultado:

```csharp
BackgroundTaskRegistration task = builder.Register();
```

```cppwinrt
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ builder.Register() };
```

```cpp
BackgroundTaskRegistration^ task = builder->Register();
```

> [!NOTE]
> Os aplicativos Universais do Windows devem chamar [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) antes de registrar qualquer tipo de gatilho em segundo plano.

Para garantir que seu aplicativo Universal do Windows continue sendo executado corretamente depois que você liberar uma atualização, use o gatilho **ServicingComplete** (veja [SystemTriggerType](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)) para realizar quaisquer alterações de configuração pós-atualização, como migrar do banco de dados do aplicativo e registrar tarefas em segundo plano. É recomendável cancelar o registro de tarefas em segundo plano associadas à versão anterior do aplicativo (veja [**RemoveAccess**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess)) e registrar tarefas em segundo plano para a nova versão do aplicativo (veja [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync)) neste momento.

Para obter mais informações, consulte [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md).

## <a name="handle-background-task-completion-using-event-handlers"></a>Manipular a conclusão da tarefa em segundo plano usando manipuladores de eventos

Registre um método com o [**BackgroundTaskCompletedEventHandler**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler). Dessa forma, seu aplicativo poderá obter resultados da tarefa em segundo plano. When the app is launched or resumed, the marked method will be called if the background task has completed since the last time the app was in the foreground. (O método OnCompleted será chamado imediatamente se a tarefa em segundo plano for concluída enquanto o aplicativo estiver em primeiro plano.)

1.  Escreva um método OnCompleted para manipular a conclusão das tarefas em segundo plano. Por exemplo, o resultado da tarefa em segundo plano pode ocasionar uma atualização da interface do usuário. O volume do método mostrado aqui é necessário para o método do manipulador de eventos OnCompleted, mesmo que este exemplo não use o parâmetro *args*.

O seguinte código de exemplo reconhece a conclusão da tarefa em segundo plano e chama um método de atualização de interface de usuário de exemplo que leva a cadeia de caracteres da mensagem.

```csharp
private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
{
    var settings = Windows.Storage.ApplicationData.Current.LocalSettings;
    var key = task.TaskId.ToString();
    var message = settings.Values[key].ToString();
    UpdateUI(message);
}
```

```cppwinrt
void UpdateUI(winrt::hstring const& message)
{
    MyTextBlock().Dispatcher().RunAsync(Windows::UI::Core::CoreDispatcherPriority::Normal, [=]()
    {
        MyTextBlock().Text(message);
    });
}

void OnCompleted(
    Windows::ApplicationModel::Background::BackgroundTaskRegistration const& sender,
    Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
{
    // You'll previously have inserted this key into local settings.
    auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings().Values() };
    auto key{ winrt::to_hstring(sender.TaskId()) };
    auto message{ winrt::unbox_value<winrt::hstring>(settings.Lookup(key)) };

    UpdateUI(message);
}
```

```cpp
void MainPage::OnCompleted(BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
{
    auto settings = ApplicationData::Current->LocalSettings->Values;
    auto key = task->TaskId.ToString();
    auto message = dynamic_cast<String^>(settings->Lookup(key));
    UpdateUI(message);
}
```

> [!NOTE]
> As atualizações da interface do usuário devem ser executadas assincronamente para evitar atraso do thread da interface do usuário. Por exemplo, consulte o método UpdateUI no [exemplo de tarefa em segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask).

2.  Retorne para onde você registrou a tarefa em segundo plano. Depois dessa linha de código, adicione um novo objeto [**BackgroundTaskCompletedEventHandler**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler). Forneça o método OnCompleted como o parâmetro para o construtor **BackgroundTaskCompletedEventHandler**.

O seguinte código de exemplo adiciona um [**BackgroundTaskCompletedEventHandler**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler) ao [**BackgroundTaskRegistration**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration):

```csharp
task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
```

```cppwinrt
task.Completed({ this, &MainPage::OnCompleted });
```

```cpp
task->Completed += ref new BackgroundTaskCompletedEventHandler(this, &MainPage::OnCompleted);
```

## <a name="declare-in-the-app-manifest-that-your-app-uses-background-tasks"></a>Declare in the app manifest that your app uses background tasks

Antes de o seu aplicativo conseguir executar tarefas em segundo plano, você deve declarar cada tarefa em segundo plano no manifesto do aplicativo. If your app attempts to register a background task with a trigger that isn't listed in the manifest, the registration of the background task will fail with a "runtime class not registered" error.

1.  Abra o designer de manifesto do pacote abrindo o arquivo chamado Package.appxmanifest.
2.  Abra a guia **Declarações**.
3.  Na lista suspensa **Declarações Disponíveis**, selecione **Tarefas em Segundo Plano** e clique em **Adicionar**.
4.  Marque a caixa de seleção **Evento do sistema**.
5.  In the **Entry point:** textbox, enter the namespace and name of your background class which is for this example is Tasks.ExampleBackgroundTask.
6.  Feche o designer de manifesto.

O seguinte elemento Extensions é adicionado ao arquivo Package.appxmanifest para registrar a tarefa em segundo plano:

```xml
<Extensions>
  <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ExampleBackgroundTask">
    <BackgroundTasks>
      <Task Type="systemEvent" />
    </BackgroundTasks>
  </Extension>
</Extensions>
```

## <a name="summary-and-next-steps"></a>Resumo e próximas etapas

Agora você deve compreender os fundamentos de como escrever uma classe de tarefa em segundo plano, como registrar a tarefa em segundo plano no aplicativo e como fazer o aplicativo reconhecer quando a tarefa em segundo plano é concluída. Você também deve saber atualizar o manifesto do aplicativo para que seu aplicativo possa registrar com êxito a tarefa em segundo plano.

> [!NOTE]
> Baixe o [exemplo de tarefa em segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) para ver exemplos de código semelhantes, no contexto de um aplicativo UWP completo e robusto, que usa tarefas em segundo plano.

Veja os seguintes tópicos relacionados para obter referência de API, diretriz conceitual de tarefa em segundo plano e instruções mais detalhadas para escrever aplicativos que usam tarefas em segundo plano.

## <a name="related-topics"></a>Tópicos relacionados

**Detailed background task instructional topics**

* [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md)
* [Registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md)
* [Usar um gatilho de manutenção](use-a-maintenance-trigger.md)
* [Tratar uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)
* [Executar uma tarefa em segundo plano em um temporizador](run-a-background-task-on-a-timer-.md)
* [Criar e registrar uma tarefa em segundo plano em processamento](create-and-register-an-inproc-background-task.md).
* [Convert an out-of-process background task to an in-process background task](convert-out-of-process-background-task.md)  

**Background task guidance**

* [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md)
* [Depurar uma tarefa em segundo plano](debug-a-background-task.md)
* [How to trigger suspend, resume, and background events in UWP apps (when debugging)](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)

**Background Task API Reference**

* [**Windows.ApplicationModel.Background**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background)
