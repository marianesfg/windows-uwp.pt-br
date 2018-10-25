---
author: TylerMSFT
title: Declarar tarefas em segundo plano no manifesto do aplicativo
description: Habilite o uso de tarefas em segundo plano declarando-as como extensões no manifesto do aplicativo.
ms.assetid: 6B4DD3F8-3C24-4692-9084-40999A37A200
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, tarefa em segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: 00e685085c004cced24b9a42ef2261a26eef10bb
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5481487"
---
# <a name="declare-background-tasks-in-the-application-manifest"></a>Declarar tarefas em segundo plano no manifesto do aplicativo




**APIs Importantes**

-   [**Esquema de BackgroundTasks**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)

Habilite o uso de tarefas em segundo plano declarando-as como extensões no manifesto do aplicativo.

> [!Important]
>  Este artigo é específico para tarefas em segundo plano fora do processo. Tarefas em segundo plano no processo não são declaradas no manifesto.

As tarefas em segundo plano fora do processo devem ser declaradas no manifesto do aplicativo ou ele não será capaz de registrá-las (uma exceção será gerada). Além disso, as tarefas em segundo plano fora do processo devem ser declaradas no manifesto do aplicativo para passar certificação.

Este tópico considera que você criou uma ou mais classes de tarefa em segundo plano e que o seu aplicativo registra cada tarefa em segundo plano para execução em resposta a pelo menos um gatilho.

## <a name="add-extensions-manually"></a>Adicionar extensões manualmente


Abra o manifesto do aplicativo (Package.appxmanifest) e vá para o elemento Application. Crie um elemento Extensions (caso não haja um).

O seguinte trecho foi retirado do [exemplo de tarefa de segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666):

```xml
<Application Id="App"
   ...
   <Extensions>
     <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.SampleBackgroundTask">
       <BackgroundTasks>
         <Task Type="systemEvent" />
         <Task Type="timer" />
       </BackgroundTasks>
     </Extension>
     <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ServicingComplete">
       <BackgroundTasks>
         <Task Type="systemEvent"/>
       </BackgroundTasks>
     </Extension>
   </Extensions>
 </Application>
```

## <a name="add-a-background-task-extension"></a>Adicionar uma extensão de tarefa em segundo plano  

Declare sua primeira tarefa em segundo plano.

Copie o código no elemento Extensions (você adicionará atributos nas próximas etapas).

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="">
      <BackgroundTasks>
        <Task Type="" />
      </BackgroundTasks>
    </Extension>
</Extensions>
```

1.  Altere o atributo EntryPoint para que ele tenha a mesma cadeia de caracteres usada pelo código como o ponto de entrada ao registrar a tarefa em segundo plano (**namespace.classname**).

    Neste exemplo, o ponto de entrada é ExampleBackgroundTaskNameSpace.ExampleBackgroundTaskClassName:

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ExampleBackgroundTaskClassName">
       <BackgroundTasks>
         <Task Type="" />
       </BackgroundTasks>
    </Extension>
</Extensions>
```

2.  Modifique a lista de atributos Tipo de Tarefa para indicar o tipo do registro de tarefa usado com essa tarefa em segundo plano. Se a tarefa em segundo plano for registrada com vários tipos de gatilho, adicione outros elementos Task e atributos Type para cada um deles.

    **Observação**Certifique-se de listar cada um dos tipos de gatilho que você estiver usando, ou a tarefa em segundo plano não registrará com os tipos de gatilho não declarados (o método [**registrar**](https://msdn.microsoft.com/library/windows/apps/br224772) falhará e gerará uma exceção).

    Este exemplo de trecho indica o uso de gatilhos de evento do sistema e notificações por push:

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.BackgroundTaskClass">
    <BackgroundTasks>
        <Task Type="systemEvent" />
        <Task Type="pushNotification" />
    </BackgroundTasks>
</Extension>
```

### <a name="add-multiple-background-task-extensions"></a>Adicionar múltiplas extensões de tarefa em segundo plano

Repita a etapa 2 para cada classe adicional de tarefa em segundo plano registrada pelo aplicativo.

O exemplo a seguir mostra o elemento Application completo da [amostra de tarefa em segundo plano]( http://go.microsoft.com/fwlink/p/?linkid=227509). Ele ilustra o uso de 2 classes de tarefa em segundo plano com um total de 3 tipos de gatilho. Copie a seção Extensões desse exemplo e modifique-a conforme o necessário para declarar tarefas em segundo plano no manifesto do aplicativo.

```xml
<Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="BackgroundTask.App">
        <uap:VisualElements
          DisplayName="BackgroundTask"
          Square150x150Logo="Assets\StoreLogo-sdk.png"
          Square44x44Logo="Assets\SmallTile-sdk.png"
          Description="BackgroundTask"

          BackgroundColor="#00b2f0">
          <uap:LockScreen Notification="badgeAndTileText" BadgeLogo="Assets\smalltile-Windows-sdk.png" />
            <uap:SplashScreen Image="Assets\Splash-sdk.png" />
            <uap:DefaultTile DefaultSize="square150x150Logo" Wide310x150Logo="Assets\tile-sdk.png" >
                <uap:ShowNameOnTiles>
                    <uap:ShowOn Tile="square150x150Logo" />
                    <uap:ShowOn Tile="wide310x150Logo" />
                </uap:ShowNameOnTiles>
            </uap:DefaultTile>
        </uap:VisualElements>

      <Extensions>
        <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.SampleBackgroundTask">
          <BackgroundTasks>
            <Task Type="systemEvent" />
            <Task Type="timer" />
          </BackgroundTasks>
        </Extension>
        <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ServicingComplete">
          <BackgroundTasks>
            <Task Type="systemEvent"/>
          </BackgroundTasks>
        </Extension>
      </Extensions>
    </Application>
</Applications>
```

## <a name="declare-where-your-background-task-will-run"></a>Declarar onde a tarefa em segundo plano será executada

Você pode especificar onde as tarefas em segundo plano são executadas:

* Por padrão, elas são executadas no processo BackgroundTaskHost.exe.
* No mesmo processo do aplicativo em primeiro plano.
* Use `ResourceGroup` para inserir diversas tarefas em segundo plano no mesmo processo de hospedagem ou para separá-las em processos diferentes.
* Use `SupportsMultipleInstances` para executar o processo em segundo plano em um novo processo que obtém seus próprio limites de recursos (cpu, memória) sempre que um novo gatilho é acionado.

### <a name="run-in-the-same-process-as-your-foreground-application"></a>Execute no mesmo processo do aplicativo em primeiro plano.

Aqui está o XML de exemplo que declara uma tarefa em segundo plano, a qual é executada no mesmo processo como o aplicativo em primeiro plano.

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="ExecModelTestBackgroundTasks.ApplicationTriggerTask">
        <BackgroundTasks>
            <Task Type="systemEvent" />
        </BackgroundTasks>
    </Extension>
</Extensions>
```

Ao especificar o **EntryPoint**, o aplicativo recebe um retorno de chamada para o método especificado quando o gatilho é acionado. Se você não especificar um **EntryPoint**, o aplicativo recebe o retorno de chamada por meio de [OnBackgroundActivated()](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx).  Consulte [Criar e registrar uma tarefa em segundo plano no processo](create-and-register-an-inproc-background-task.md) para obter mais detalhes.

### <a name="specify-where-your-background-task-runs-with-the-resourcegroup-attribute"></a>Especifique onde sua tarefa em segundo plano é executada com o atributo ResourceGroup.

Aqui está o XML de exemplo que declara uma tarefa em segundo plano que é executada em um processo do BackgroundTaskHost.exe, mas em um separado de outras instâncias de tarefas em segundo plano do mesmo aplicativo. Observe o atributo `ResourceGroup`, que identifica quais tarefas em segundo plano serão executadas juntas.

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.SessionConnectedTriggerTask" ResourceGroup="foo">
      <BackgroundTasks>
        <Task Type="systemEvent" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimeZoneTriggerTask" ResourceGroup="foo">
      <BackgroundTasks>
        <Task Type="systemEvent" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimerTriggerTask" ResourceGroup="bar">
      <BackgroundTasks>
        <Task Type="timer" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.ApplicationTriggerTask" ResourceGroup="bar">
      <BackgroundTasks>
        <Task Type="general" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.MaintenanceTriggerTask" ResourceGroup="foobar">
      <BackgroundTasks>
        <Task Type="general" />
      </BackgroundTasks>
    </Extension>
</Extensions>
```

### <a name="run-in-a-new-process-each-time-a-trigger-fires-with-the-supportsmultipleinstances-attribute"></a>Execute em um novo processo sempre que um gatilho é acionado com o atributo SupportsMultipleInstances

Esse exemplo declara uma tarefa em segundo plano executada em um novo processo que obtém seus próprios limites de recursos (memória e CPU) sempre que um novo gatilho é acionado. Observe o uso de `SupportsMultipleInstances`, que habilita esse comportamento. Para usar esse atributo, você deve usar o SDK versão '10.0.15063' (Windows 10 para criadores) ou superior.

```xml
<Package
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    ...
    <Applications>
        <Application ...>
            ...
            <Extensions>
                <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimerTriggerTask">
                    <BackgroundTasks uap4:SupportsMultipleInstances=“True”>
                        <Task Type="timer" />
                    </BackgroundTasks>
                </Extension>
            </Extensions>
        </Application>
    </Applications>
```

> [!NOTE]
> Você não pode especificar `ResourceGroup` ou `ServerName` juntamente com `SupportsMultipleInstances`.

## <a name="related-topics"></a>Tópicos relacionados

* [Depurar uma tarefa em segundo plano](debug-a-background-task.md)
* [Registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md)
