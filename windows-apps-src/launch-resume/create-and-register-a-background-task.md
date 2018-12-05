---
title: Criar e registrar uma tarefa em segundo plano fora do processo
description: Crie uma classe de tarefa em segundo plano fora do processo e a registre para ser executada quando seu app não estiver em primeiro plano.
ms.assetid: 4F98F6A3-0D3D-4EFB-BA8E-30ED37AE098B
ms.date: 07/02/2018
ms.topic: article
keywords: Windows 10, uwp, tarefa em segundo plano
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: eb6cde0c3c31f0116c65e5d5dc4a0d8ae4a1b540
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8739858"
---
# <a name="create-and-register-an-out-of-process-background-task"></a>Criar e registrar uma tarefa em segundo plano fora do processo

**APIs Importantes**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

Crie uma classe de tarefa em segundo plano e a registre para ser executada quando seu aplicativo não estiver em primeiro plano. Este tópico demonstra como criar e registrar uma tarefa em segundo plano que é executada em um processo separado do seu aplicativo. Para fazer o trabalho em segundo plano diretamente no aplicativo em primeiro plano, consulte [Criar e registrar uma tarefa em segundo plano no processo](create-and-register-an-inproc-background-task.md).

> [!NOTE]
> Se você usar uma tarefa em segundo plano para reproduzir mídia em segundo plano, consulte [Reproduzir mídia em segundo plano](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio) para obter informações sobre os aprimoramentos no Windows 10, versão 1607, que tornam a tarefa mais fácil.

## <a name="create-the-background-task-class"></a>Criar a classe de tarefa em segundo plano

Você pode executar código em segundo plano criando classes que implementam a interface [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794). Esse código é executado quando um evento específico for acionado pelo uso, por exemplo, [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839) ou [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517).

As seguintes etapas mostram como escrever uma nova classe que implementa a interface [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794).

1.  Crie um novo projeto para tarefas em segundo plano e adicione-o à sua solução. Para fazer isso, clique com botão direito no nó da solução no **Gerenciador de soluções** e selecione **Adicionar** \> **Novo projeto**. Em seguida, selecione o tipo de projeto de **Componente do tempo de execução do Windows** , o nome do projeto e clique Okey.
2.  Referencie o projeto das tarefa em segundo plano no projeto de aplicativo da Plataforma Universal do Windows (UWP). Para um c# ou C++ aplicativo, no seu projeto de aplicativo, clique com botão direito em **referências** e selecione **Adicionar nova referência**. Em **Solução**, selecione **Projetos** e clique para selecionar o nome do seu projeto de tarefa em segundo plano e clique em **Ok**.
3.  Para o projeto de tarefas em segundo plano, adicione uma nova classe que implementa a interface [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) . O método [**Ibackgroundtask**](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) é um ponto de entrada obrigatório que será chamado quando o evento especificado é disparado; Esse método é obrigatório em todas as tarefas em segundo plano.

> [!NOTE]
> A própria classe de tarefa em segundo plano&mdash;e todas as outras classes no projeto de tarefa em segundo plano&mdash;precisam ser classes **públicas** que são **seladas** (ou **final**).

O exemplo de código a seguir mostra um ponto de partida muito básico para uma classe de tarefa em segundo plano.

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

4.  Se você executar um código assíncrono na sua tarefa em segundo plano, então ela precisará usar um adiamento. Se você não usar um adiamento, em seguida, o processo de tarefa em segundo plano poderá ser encerrado inesperadamente se o método **Run** retorna antes que qualquer trabalho assíncrono tem executada até a conclusão.

Solicite o adiamento no método **Executar** antes de chamar o método assíncrono. Salve o adiamento em um membro de classe de dados para que ele pode ser acessado do método assíncrono. Declare o adiamento como concluído após a conclusão do código assíncrono.

O exemplo de código a seguir obtém um adiamento, salva- e libera quando o código assíncrono é concluído.

```csharp
BackgroundTaskDeferral _deferral; // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation
public async void Run(IBackgroundTaskInstance taskInstance)
{
    _deferral = taskInstance.GetDeferral()
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
> Em C#, os métodos assíncronos da tarefa em segundo plano podem ser chamados usando as palavras-chave **async/await**. No C++ c++ /CX, um resultado semelhante pode ser obtido por meio de uma cadeia da tarefa.

Para mais informações sobre padrões assíncronos, consulte [Programação assíncrona](https://msdn.microsoft.com/library/windows/apps/mt187335). Para exemplos adicionais sobre como usar os adiamentos para evitar que uma tarefa em segundo plano pare antecipadamente, consulte a [amostra de tarefa em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666).

O procedimento abaixo é concluído em uma de suas classes de aplicativo (por exemplo, MainPage.xaml.cs).

> [!NOTE]
> Você também pode criar uma função dedicada a registrar tarefas em segundo plano&mdash;consulte [registrar uma tarefa em segundo plano](register-a-background-task.md). Nesse caso, em vez de usar as próximas três etapas, você pode simplesmente construir o gatilho e oferecê-lo à função de registro juntamente com o nome da tarefa, ponto de entrada da tarefa e (opcionalmente) uma condição.

## <a name="register-the-background-task-to-run"></a>Registre a tarefa em segundo plano a ser executada

1.  Descubra se a tarefa em segundo plano já está registrada Iterando através da propriedade [**Alltasks**](https://msdn.microsoft.com/library/windows/apps/br224787) . Esta etapa é importante; se seu aplicativo não verificar a existência de registros de tarefas em segundo plano, ele poderá facilmente registrar a tarefa várias vezes, causando problemas de desempenho e extrapolando o tempo de CPU disponível para a tarefa antes que esta possa ser concluída.

O exemplo a seguir itera na propriedade **AllTasks** e define uma variável de sinalizador como true se a tarefa já está registrada.

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

2.  Se a tarefa em segundo plano ainda não estiver registrada, use [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) para criar uma instância de sua tarefa em segundo plano. O ponto de entrada da tarefa deve ser o nome de sua classe de tarefa em segundo plano prefixada pelo namespace.

O gatilho da tarefa em segundo plano controla quando a tarefa será executada. Para obter uma lista dos possíveis gatilhos, consulte [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839).

Por exemplo, esse código cria uma nova tarefa em segundo plano e define para ser executada quando o gatilho **TimeZoneChanged** ocorre:

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

3.  Você pode adicionar uma condição para controlar quando sua tarefa será executada após a ocorrência de um evento de gatilho (opcional). Por exemplo, se você não quiser que a tarefa seja executada até que o usuário esteja presente, use a condição **UserPresent**. Para obter uma lista das possíveis condições, consulte [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

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

4.  Registre a tarefa em segundo plano chamando o método Register no objeto [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768). Armazene o resultado [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786) para que ele possa ser usado na próxima etapa.

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
> Os aplicativos Universais do Windows devem chamar [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar qualquer tipo de gatilho em segundo plano.

Para garantir que seu aplicativo Universal do Windows continue sendo executado corretamente depois que você liberar uma atualização, use o gatilho **ServicingComplete** (veja [SystemTriggerType](https://msdn.microsoft.com/library/windows/apps/br224839)) para realizar quaisquer alterações de configuração pós-atualização, como migrar do banco de dados do aplicativo e registrar tarefas em segundo plano. É recomendável cancelar o registro de tarefas em segundo plano associadas à versão anterior do aplicativo (veja [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471)) e registrar tarefas em segundo plano para a nova versão do aplicativo (veja [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)) neste momento.

Para obter mais informações, consulte [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md).

## <a name="handle-background-task-completion-using-event-handlers"></a>Manipular a conclusão da tarefa em segundo plano usando manipuladores de eventos

Registre um método com o [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781). Dessa forma, seu aplicativo poderá obter resultados da tarefa em segundo plano. Quando o aplicativo é iniciado ou retomado, o método marcado será chamado se a tarefa em segundo plano foi concluída desde a última vez em que o aplicativo estava em primeiro plano. (O método OnCompleted será chamado imediatamente se a tarefa em segundo plano for concluída enquanto o aplicativo estiver em primeiro plano.)

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
> As atualizações da interface do usuário devem ser executadas assincronamente para evitar atraso do thread da interface do usuário. Por exemplo, consulte o método UpdateUI no [exemplo de tarefa em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666).

2.  Retorne para onde você registrou a tarefa em segundo plano. Depois dessa linha de código, adicione um novo objeto [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781). Forneça o método OnCompleted como o parâmetro para o construtor **BackgroundTaskCompletedEventHandler**.

O seguinte código de exemplo adiciona um [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781) ao [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786):

```csharp
task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
```

```cppwinrt
task.Completed({ this, &MainPage::OnCompleted });
```

```cpp
task->Completed += ref new BackgroundTaskCompletedEventHandler(this, &MainPage::OnCompleted);
```

## <a name="declare-in-the-app-manifest-that-your-app-uses-background-tasks"></a>Declarar no manifesto do aplicativo que seu aplicativo usa tarefas em segundo plano

Antes de o seu aplicativo conseguir executar tarefas em segundo plano, você deve declarar cada tarefa em segundo plano no manifesto do aplicativo. Se o aplicativo tentar registrar uma tarefa em segundo plano com um gatilho que não esteja listado no manifesto, o registro da tarefa em segundo plano falhará com um erro de "classe de tempo de execução não registrada".

1.  Abra o designer de manifesto do pacote abrindo o arquivo chamado Package.appxmanifest.
2.  Abra a guia **Declarações**.
3.  Na lista suspensa **Declarações Disponíveis**, selecione **Tarefas em Segundo Plano** e clique em **Adicionar**.
4.  Marque a caixa de seleção **Evento do sistema**.
5.  No **ponto de entrada:** textbox, insira o namespace e nome de sua classe de plano de fundo que, para este exemplo é Tasks.ExampleBackgroundTask.
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
> Baixe o [exemplo de tarefa em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666) para ver exemplos de código semelhantes, no contexto de um aplicativo UWP completo e robusto, que usa tarefas em segundo plano.

Veja os seguintes tópicos relacionados para obter referência de API, diretriz conceitual de tarefa em segundo plano e instruções mais detalhadas para escrever aplicativos que usam tarefas em segundo plano.

## <a name="related-topics"></a>Tópicos relacionados

**Tópicos de instruções detalhadas de tarefa em segundo plano**

* [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md)
* [Registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md)
* [Usar um gatilho de manutenção](use-a-maintenance-trigger.md)
* [Manipular uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)
* [Executar uma tarefa em segundo plano em um temporizador](run-a-background-task-on-a-timer-.md)
* [Criar e registrar uma tarefa em segundo plano no processo](create-and-register-an-inproc-background-task.md).
* [Converter uma tarefa em segundo plano fora do processo em uma tarefa em segundo plano no processo](convert-out-of-process-background-task.md)  

**Diretrizes da tarefa em segundo plano**

* [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md)
* [Depurar uma tarefa em segundo plano](debug-a-background-task.md)
* [Como disparar eventos de suspensão, retomada e segundo plano em aplicativos UWP (durante a depuração)](http://go.microsoft.com/fwlink/p/?linkid=254345)

**Referência de API de tarefa em segundo plano**

* [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)
