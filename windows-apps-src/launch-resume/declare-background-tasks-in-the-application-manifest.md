---
author: TylerMSFT
title: Declarar tarefas em segundo plano no manifesto do aplicativo
description: "Habilite o uso de tarefas em segundo plano declarando-as como extensões no manifesto do aplicativo."
ms.assetid: 6B4DD3F8-3C24-4692-9084-40999A37A200
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 364edc93c52d3c7c8cbe5f1a85c8ca751eb44b35
ms.lasthandoff: 02/07/2017

---

# <a name="declare-background-tasks-in-the-application-manifest"></a>Declarar tarefas em segundo plano no manifesto do aplicativo


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**APIs Importantes**

-   [**Esquema de BackgroundTasks**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)

Habilite o uso de tarefas em segundo plano declarando-as como extensões no manifesto do aplicativo.

> [!Important]
>  Este artigo é específico para tarefas em segundo plano fora do processo. Tarefas em segundo plano no processo =não são declaradas no manifesto.

As tarefas em segundo plano fora do processo devem ser declaradas no manifesto do aplicativo, ou seu aplicativo não será capaz de registrá-los (uma exceção será gerada). Além disso, as tarefas em segundo plano fora do processo devem ser declaradas no manifesto do aplicativo para passar certificação.

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

    **Observação**  Certifique-se de listar cada um dos tipos de gatilho que você está usado ou a tarefa em segundo plano não registrará com os tipos de gatilho não declarados (o método [**Register**](https://msdn.microsoft.com/library/windows/apps/br224772) falhará e gerará uma exceção).

    Este exemplo de trecho indica o uso de gatilhos de evento do sistema e notificações por push:

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.BackgroundTaskClass">
    <BackgroundTasks>
        <Task Type="systemEvent" />
        <Task Type="pushNotification" />
    </BackgroundTasks>
</Extension>
```


## <a name="add-additional-background-task-extensions"></a>Adicionar outras extensões de tarefa em segundo plano

Repita a etapa 2 para cada classe adicional de tarefa em segundo plano registrada pelo aplicativo.

O exemplo a seguir mostra o elemento Application completo da [amostra de tarefa em segundo plano]( http://go.microsoft.com/fwlink/p/?linkid=227509). Ele ilustra o uso de 2 classes de tarefa em segundo plano com um total de 3 tipos de gatilho. Copie a seção Extensões desse exemplo e modifique-a conforme o necessário para declarar tarefas em segundo plano no manifesto do seu aplicativo.

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

## <a name="declare-your-background-task-to-run-in-a-different-process"></a>Declare sua tarefa em segundo plano para ser executada em um processo diferente

Nova funcionalidade no Windows 10, versão 1507, permite que você execute a tarefa em segundo plano em um processo diferente do BackgroundTaskHost.exe (o processo em que tarefas em segundo plano são executadas por padrão).  Há duas opções: executar no mesmo processo de seu aplicativo em primeiro plano; executar em uma instância do BackgroundTaskHost.exe separada de outras instâncias das tarefas em segundo plano do mesmo aplicativo.  

### <a name="run-in-the-foreground-application"></a>Executar no aplicativo em primeiro plano

Aqui está o XML de exemplo que declara uma tarefa em segundo plano que é executada no mesmo processo como o aplicativo em primeiro plano. Observe o atributo `Executable`:

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="ExecModelTestBackgroundTasks.ApplicationTriggerTask" Executable="$targetnametoken$.exe">
        <BackgroundTasks>
            <Task Type="systemEvent" />
        </BackgroundTasks>
    </Extension>
</Extensions>
```

> [!Note]
> Use o elemento Executável apenas com tarefas em segundo plano que o exigem, como [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032).  

### <a name="run-in-a-different-background-host-process"></a>Executar em um processo de host de segundo plano diferente

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


## <a name="related-topics"></a>Tópicos relacionados


* [Depurar uma tarefa em segundo plano](debug-a-background-task.md)
* [Registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md)

