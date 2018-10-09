---
author: PatrickFarley
ms.assetid: 67a46812-881c-404b-9f3b-c6786f39e72b
title: Personalizar o fluxo de trabalho de impressão
description: Crie experiências de fluxo de trabalho de impressão personalizadas para atender às necessidades de sua organização.
ms.author: pafarley
ms.date: 08/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, impressão
ms.localizationpriority: medium
ms.openlocfilehash: 9e53c15b01a08c8c617529fe074929ce89a68ce9
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2018
ms.locfileid: "4469099"
---
# <a name="customize-the-print-workflow"></a>Personalizar o fluxo de trabalho de impressão

## <a name="overview"></a>Visão geral
Os desenvolvedores podem personalizar a experiência de fluxo de trabalho de impressão através do uso de um app de fluxo de trabalho de impressão. Os apps de fluxo de trabalho de impressão são aplicativos UWP que expandem a funcionalidade dos [apps de dispositivos da Microsoft Store (WSDAs)](https://docs.microsoft.com/windows-hardware/drivers/devapps/); portanto, será útil ter alguma familiaridade com os WSDAs antes de continuar. 

Assim como acontece como os WSDAs, quando o usuário de um aplicativo de origem opta por imprimir algo e navega pela caixa de diálogo de impressão, o sistema verifica se um app de fluxo de trabalho está associado a essa impressora. Se estiver, o app de fluxo de trabalho de impressão é iniciado (principalmente como tarefa em segundo plano; mais detalhes a seguir). Um app de fluxo de trabalho é capaz de alterar o tíquete de impressão (o documento XML que define as configurações de dispositivo de impressora para a tarefa de impressão atual) e o conteúdo XPS real a ser impresso. Opcionalmente, ele pode expor essa funcionalidade ao usuário iniciando uma interface do usuário durante o processo. Depois disso, ele transmite o conteúdo de impressão e o tíquete de impressão para o driver.

Como ele envolve componentes de primeiro e segundo planos e como ele está funcionalmente associado a outros apps, pode ser mais complicado implementar um app de fluxo de trabalho de impressão do que outras categorias de aplicativos UWP. É recomendável que você inspecione o [exemplo de app de fluxo de trabalho](https://github.com/Microsoft/print-oem-samples) enquanto lê este guia, para compreender melhor como os diferentes recursos podem ser implementados. Alguns recursos, como diversas verificações de erro e o gerenciamento da interface do usuário, não foram incluídos neste guia apenas para simplificar.

## <a name="getting-started"></a>Introdução

O app de fluxo de trabalho deve indicar seu ponto de entrada para o sistema de impressão para que ele possa ser iniciado no momento apropriado. Isso é feito inserindo a declaração a seguir no elemento `Application/Extensions` do arquivo *package.appxmanifest* do projeto UWP. 

```xml
<uap:Extension Category="windows.printWorkflowBackgroundTask"  
    EntryPoint="WFBackgroundTasks.WfBackgroundTask" />
```

> [!IMPORTANT] 
> Há muitos cenários em que a personalização de impressão não requer a entrada do usuário. Por esse motivo, os apps de fluxo de trabalho de impressão são executados como tarefas em segundo plano por padrão.

Se um app de fluxo de trabalho estiver associado ao aplicativo de origem que iniciou o trabalho de impressão (consulte a seção posterior para obter instruções sobre isso), o sistema de impressão examinará seus arquivos de manifesto em busca de um ponto de entrada da tarefa em segundo plano.

## <a name="do-background-work-on-the-print-ticket"></a>Realize um trabalho em segundo plano no tíquete de impressão

A primeira coisa que o sistema de impressão faz com o app de fluxo de trabalho é ativar sua tarefa em segundo plano (nesse caso, a classe `WfBackgroundTask` no namespace `WFBackgroundTasks`). No método `Run` da tarefa em segundo plano, você deve converter os detalhes do gatilho da tarefa em uma instância **[PrintWorkflowTriggerDetails](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowtriggerdetails)**. Isso fornecerá a funcionalidade especial a uma tarefa em segundo plano do fluxo de trabalho de impressão. Ele expõe a propriedade **[PrintWorkflowSession](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowtriggerdetails.PrintWorkflowSession)**, que é uma instância de **[PrintWorkFlowBackgroundSession](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession)**. As classes de sessão do fluxo de trabalho de impressão, tanto as de segundo plano como as de primeiro plano, controlarão as etapas sequenciais do app do fluxo de trabalho de impressão. 

Em seguida, registre os métodos de manipulador para os dois eventos acionados por essa classe de sessão. Você definirá esses métodos posteriormente.

```csharp
public void Run(IBackgroundTaskInstance taskInstance) {
    // Take out a deferral here and complete once all the callbacks are done
    runDeferral = taskInstance.GetDeferral();

    // Associate a cancellation handler with the background task.
    taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);

    // cast the task's trigger details as PrintWorkflowTriggerDetails
    PrintWorkflowTriggerDetails workflowTriggerDetails = taskInstance.TriggerDetails as PrintWorkflowTriggerDetails;

    // Get the session manager, which is unique to this print job
    PrintWorkflowBackgroundSession sessionManager = workflowTriggerDetails.PrintWorkflowSession;

    // add the event handler callback routines
    sessionManager.SetupRequested += OnSetupRequested;
    sessionManager.Submitted += OnXpsOMPrintSubmitted;

    // Allow the event source to start
    // This call blocks until all of the workflow callbacks complete
    sessionManager.Start();
}
```

Quando o método `Start` for chamado, o gerenciador de sessão acionará o evento **[SetupRequested](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession.SetupRequested)** primeiro. Esse evento expõe informações gerais sobre a tarefa de impressão, bem como o tíquete de impressão. Neste estágio, o tíquete de impressão pode ser editado em segundo plano. 

```csharp
private void OnSetupRequested(PrintWorkflowBackgroundSession sessionManager, PrintWorkflowBackgroundSetupRequestedEventArgs printTaskSetupArgs) {
    // Take out a deferral here and complete once all the callbacks are done
    Deferral setupRequestedDeferral = printTaskSetupArgs.GetDeferral();

    // Get general information about the source application, print job title, and session ID
    string sourceApplicationName = printTaskSetupArgs.Configuration.SourceAppDisplayName;
    string jobTitle = printTaskSetupArgs.Configuration.JobTitle;
    string sessionId = printTaskSetupArgs.Configuration.SessionId;

    // edit the print ticket
    WorkflowPrintTicket printTicket = printTaskSetupArgs.GetUserPrintTicketAsync();

    // ...
```

Importante: durante a manipulação de **SetupRequested**, o app determinará se um componente de primeiro plano será iniciado. Isso pode depender de uma configuração salva anteriormente no armazenamento local, de um evento ocorrido durante a edição do tíquete de impressão ou pode ser uma configuração estática do app específico.

```csharp
    // ...
    
    if (UIrequested) {
        printTaskSetupArgs.SetRequiresUI();

        // Any data that is to be passed to the foreground task must be stored the app's local storage.
        // It should be prefixed with the sourceApplicationName string and the SessionId string, so that
        // it can be identified as pertaining to this workflow app session.
    }

    // Complete the deferral taken out at the start of OnSetupRequested
    setupRequestedDeferral.Complete();
}
```

## <a name="do-foreground-work-on-the-print-job-optional"></a>Realizar o trabalho em primeiro plano no trabalho de impressão (opcional)

Se o método **[SetRequiresUI](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsetuprequestedeventargs.SetRequiresUI)** foi chamado, o sistema de impressão examinará o arquivo de manifesto em busca do ponto de entrada do aplicativo em primeiro plano. O elemento `Application/Extensions` do arquivo *Package. appxmanifest* arquivo deve ter as linhas a seguir. Substitua o valor de `EntryPoint` pelo nome do app em primeiro plano.

```xml
<uap:Extension Category="windows.printWorkflowForegroundTask"  
    EntryPoint="MyWorkFlowForegroundApp.App" /> 
```

Em seguida, o sistema de impressão chama o método **OnActivated** do ponto de entrada do app em questão. No método **OnActivated** do arquivo _App.xaml.cs_, o app de fluxo de trabalho deve verificar se o tipo de ativação é uma ativação de fluxo de trabalho. Se for, o app de fluxo de trabalho poderá converter os argumentos da ativação em um objeto **[PrintWorkflowUIActivatedEventArgs](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowuiactivatedeventargs)**, que expõe um objeto **[PrintWorkflowForegroundSession](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession)** como propriedade. Esse objeto, assim como seu equivalente em segundo plano na seção anterior, contém os eventos gerados pelo sistema de impressão, e você poderá atribuir manipuladores a eles. Nesse caso, a funcionalidade de manipulação de eventos será implementada em uma classe separada chamada `WorkflowPage`.

Primeiro, no arquivo _App.xaml.cs_:

```csharp
protected override void OnActivated(IActivatedEventArgs args){

    if (args.Kind == ActivationKind.PrintWorkflowForegroundTask) {

        // the app should instantiate a new UI view so that it can properly handle the case when 
        // several print jobs are active at the same time.
        Frame rootFrame = new Frame();
        if (null == Window.Current.Content)
        {
            rootFrame.Navigate(typeof(WorkflowPage));
            Window.Current.Content = rootFrame;
        }

        // Get the main page
        WorkflowPage workflowPage = (WorkflowPage)rootFrame.Content;

        // Make sure the page knows it's handling a foreground task activation
        workflowPage.LaunchType = WorkflowPage.WorkflowPageLaunchType.ForegroundTask;

        // Get the activation arguments
        PrintWorkflowUIActivatedEventArgs printTaskUIEventArgs = args as PrintWorkflowUIActivatedEventArgs;

        // Get the session manager
        PrintWorkflowForegroundSession taskSessionManager = printTaskUIEventArgs.PrintWorkflowSession;

        // Add the callback handlers - these methods are in the workflowPage class
        taskSessionManager.SetupRequested += workflowPage.OnSetupRequested;
        taskSessionManager.XpsDataAvailable += workflowPage.OnXpsDataAvailable;

        // start raising the print workflow events
        taskSessionManager.Start();
    }
}
```

Depois que a interface do usuário anexar manipuladores de eventos e o método **OnActivated** for encerrado, o sistema de impressão acionará o evento **[SetupRequested](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession.SetupRequested)** da interface do usuário a ser manipulado. Esse evento fornece os mesmos dados que o evento de configuração de tarefa em segundo plano fornecido, incluindo as informações de trabalho de impressão e o documento de tíquete de impressão, mas sem a capacidade de solicitar a inicialização da interface do usuário adicional. No arquivo _WorkflowPage.xaml.cs_:

```csharp
internal void OnSetupRequested(PrintWorkflowForegroundSession sessionManager, PrintWorkflowForegroundSetupRequestedEventArgs printTaskSetupArgs) {
    // If anything asynchronous is going to be done, you need to take out a deferral here,
    // since otherwise the next callback happens once this one exits, which may be premature
    Deferral setupRequestedDeferral = printTaskSetupArgs.GetDeferral();

    // Get information about the source application, print job title, and session ID
    string sourceApplicationName = printTaskSetupArgs.Configuration.SourceAppDisplayName;
    string jobTitle = printTaskSetupArgs.Configuration.JobTitle;
    string sessionId = printTaskSetupArgs.Configuration.SessionId;
    // the following string should be used when storing data that pertains to this workflow session
    // (such as user input data that is meant to change the print content later on)
    string localStorageVariablePrefix = string.Format("{0}::{1}::", sourceApplicationName, sessionID);
    
    try
    {
        // receive and store user input
        // ...
    }
    catch (Exception ex)
    {
        string errorMessage = ex.Message;
        Debug.WriteLine(errorMessage);
    }
    finally
    {
        // Complete the deferral taken out at the start of OnSetupRequested
        setupRequestedDeferral.Complete();
    }
}
```

Em seguida, o sistema de impressão acionará o evento **[XpsDataAvailable](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession.XpsDataAvailable)** para a interface do usuário. No manipulador desse evento, o app de fluxo de trabalho pode acessar todos os dados disponíveis para o evento de configuração e ler os dados XPS diretamente, como um fluxo de bytes brutos ou como um modelo de objeto. O acesso aos dados XPS permite que a interface do usuário forneça serviços de visualização de impressão e informações adicionais ao usuário sobre as operações que o app de fluxo de trabalho executará nos dados. 

Como parte desse manipulador de eventos, o app de fluxo de trabalho precisará adquirir um objeto de adiamento se continuar interagindo com o usuário. Sem um adiamento, o sistema de impressão considerará a tarefa da interface do usuário concluída quando o manipulador de eventos **XpsDataAvailable** for fechado ou chamar um método assíncrono. Quando o app tiver coletado todas as informações necessárias através da interação do usuário na interface do usuário, ele deverá concluir o adiamento para que o sistema de impressão possa avançar.

```csharp
internal async void OnXpsDataAvailable(PrintWorkflowForegroundSession sessionManager, PrintWorkflowXpsDataAvailableEventArgs printTaskXpsAvailableEventArgs)
{
    // Take out a deferral
    Deferral xpsDataAvailableDeferral = printTaskXpsAvailableEventArgs.GetDeferral();

    SpoolStreamContent xpsStream = printTaskXpsAvailableEventArgs.Operation.XpsContent.GetSourceSpoolDataAsStreamContent(); 
 
    IInputStream inputStream = xpsStream.GetInputSpoolStream(); 
 
    using (var inputReader = new Windows.Storage.Streams.DataReader(inputStream)) 
    { 
        // Read the XPS data from input stream 
        byte[] xpsData = new byte[inputReader.UnconsumedBufferLength]; 
        while (inputReader.UnconsumedBufferLength > 0) 
        { 
            inputReader.ReadBytes(xpsData); 
            // Do something with the XPS data, e.g. preview 
            // ...                  
        } 
    } 

    // Complete the deferral taken out at the start of this method
    xpsDataAvailableDeferral.Complete();
}
```

Além disso, a instância **[PrintWorkflowSubmittedOperation](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedoperation)** exposta pelos argumentos de evento permite cancelar o trabalho de impressão ou informar que o trabalho foi bem-sucedido, mas não será necessário nenhum trabalho de impressão de saída. Isso é feito chamando o método **[Complete](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedoperation.Complete)** com um valor **[PrintWorkflowSubmittedStatus](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedstatus)**.

> [!NOTE]
> Se o app de fluxo de trabalho cancelar o trabalho de impressão, é altamente recomendável que ele forneça uma notificação do sistema informando por que o trabalho foi cancelado. 


## <a name="do-final-background-work-on-the-print-content"></a>Realize um trabalho em segundo plano final no conteúdo de impressão

Depois que a interface do usuário concluir o adiamento no evento **PrintTaskXpsDataAvailable** (ou se a etapa da interface do usuário tiver sido ignorada), o sistema de impressão acionará o evento **[Submitted](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession.Submitted)** para a tarefa em segundo plano. No manipulador desse evento, o app de fluxo de trabalho pode obter acesso aos mesmos dados fornecidos pelo evento **XpsDataAvailable**. No entanto, diferente dos eventos anteriores, **Submitted** também fornece acesso de *gravação* ao conteúdo final do trabalho de impressão por meio de uma instância **[PrintWorkflowTarget](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowtarget)**. 

O objeto usado para fazer o spool dos dados para impressão final será determinado pelo acesso aos dados, ou seja, se os dados de origem são acessados como um fluxo de bytes brutos ou como um modelo de objeto XPS. Quando o app de fluxo de trabalho acessar os dados de origem por meio de um fluxo de bytes, um fluxo de bytes de saída será fornecido para gravar os dados do trabalho final. Quando o app de fluxo de trabalho acessar os dados de origem por meio do modelo de objeto, um gravador de documento será fornecido para gravar objetos no trabalho de saída. Em ambos os casos, o app de fluxo de trabalho deverá ler todos os dados de origem, modificar os dados necessários e gravar os dados modificados no destino de saída.

Quando a tarefa em segundo plano termina de gravar os dados, ele chamará **Complete** no objeto **PrintWorkflowSubmittedOperation** correspondente. Depois que o app de fluxo de trabalho concluir essa etapa e o manipulador de eventos **Submitted** for fechado, a sessão de fluxo de trabalho será fechada e o usuário poderá monitorar o status do trabalho de impressão final através das caixas de diálogo de impressão padrão.

## <a name="final-steps"></a>Etapas finais

### <a name="register-the-print-workflow-app-to-the-printer"></a>Registrar o app de fluxo de trabalho de impressão na impressora

O app de fluxo de trabalho é associado a uma impressora usando o mesmo tipo de envio de arquivo de metadados que os WSDAs. Na verdade, um único aplicativo UWP pode atuar como um app de fluxo de trabalho e um WSDA que forneça a funcionalidade de configuração da tarefa de impressão. Siga as [etapas correspondentes do WSDA para criar a associação de metadados](https://docs.microsoft.com/windows-hardware/drivers/devapps/step-2--create-device-metadata).

A diferença é que, embora os WSDAs sejam ativados automaticamente para o usuário (o app sempre será iniciado quando o usuário realizar a impressão no dispositivo associado), os apps de fluxo de trabalho não são. Eles têm uma política separada que deve ser definida.

### <a name="set-the-workflow-apps-policy"></a>Definir a política do app de fluxo de trabalho
A política do app de fluxo de trabalho é definida pelos comandos do Powershell no dispositivo executará o app de fluxo de trabalho. Os comandos Set-Printer, Add-Printer (porta existente) e Add-Printer (nova porta WSD) serão modificados para permitir que as políticas de fluxo de trabalho sejam definidas. 
* `Disabled`: Os apps de fluxo de trabalho não serão ativados.
* `Uninitialized`: Os apps de fluxo de trabalho serão ativados se o DCA de fluxo de trabalho for instalado no sistema. Se o app não estiver instalado, ainda assim, a impressão continuará. 
* `Enabled`: O contrato do fluxo de trabalho será ativado se o DCA de fluxo de trabalho for instalado no sistema. Se o app não estiver instalado, a impressão apresentará falha. 

O comando a seguir torna o app de fluxo de trabalho necessário na impressora especificada.
```Powershell
Set-Printer –Name "Microsoft XPS Document Writer" -WorkflowPolicy On
```

Um usuário local pode executar essa política em uma impressora local ou, no caso de implementação corporativa, o administrador da impressora poderá executar essa política no servidor de impressão. Em seguida, a política será sincronizada com todas as conexões de cliente. O administrador da impressora poderá usar essa política sempre que uma nova impressora for adicionada.

## <a name="see-also"></a>Consulte também

[Exemplo do app de fluxo de trabalho](https://github.com/Microsoft/print-oem-samples)

[Windows.Graphics.Printing.Workflow namespace](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow)


