---
author: mcleblanc
title: Declarar tarefas em segundo plano no manifesto do aplicativo
description: Habilite o uso de tarefas em segundo plano declarando-as como extensões no manifesto do aplicativo.
ms.assetid: 6B4DD3F8-3C24-4692-9084-40999A37A200
---

# Declarar tarefas em segundo plano no manifesto do aplicativo


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**APIs Importantes**

-   [**Esquema de BackgroundTasks**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)

Habilite o uso de tarefas em segundo plano declarando-as como extensões no manifesto do aplicativo.

As tarefas em segundo plano devem ser declaradas no manifesto do aplicativo, ou seu aplicativo não será capaz de registrá-los (uma exceção será gerada). Além disso, as tarefas em segundo plano devem ser declaradas no manifesto do aplicativo para passar certificação.

Este tópico considera que você criou uma ou mais classes de tarefa em segundo plano e que o seu aplicativo registra cada tarefa em segundo plano para execução em resposta a pelo menos um gatilho.

## Adicionar extensões manualmente


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

## Adicionar uma extensão de tarefa em segundo plano


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

    > **Observação**  Normalmente, um aplicativo é executado em um processo especial chamado "BackgroundTaskHost.exe". É possível adicionar um elemento Executável ao elemento Extensão, permitindo que a tarefa em segundo plano seja executada no contexto do aplicativo. Use o elemento Executável apenas com tarefas em segundo plano que o exigem, como [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032).    

## Adicionar outras extensões de tarefa em segundo plano


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

## Tópicos relacionados

* [Depurar uma tarefa em segundo plano](debug-a-background-task.md)
* [Registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md)


<!--HONumber=May16_HO2-->


