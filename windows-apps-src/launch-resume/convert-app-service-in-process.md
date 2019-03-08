---
title: Converter um serviço de app para ser executado no mesmo processo de seu app host
description: Converta o código de serviço de app executado em um processo separado em segundo plano em código que é executado no mesmo processo de seu provedor de serviços de app.
ms.date: 11/03/2017
ms.topic: article
keywords: Windows 10, uwp, o serviço de aplicativo
ms.assetid: 30aef94b-1b83-4897-a2f1-afbb4349696a
ms.localizationpriority: medium
ms.openlocfilehash: a976ac69d289a5582c2f3546227adba707ac5297
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643411"
---
# <a name="convert-an-app-service-to-run-in-the-same-process-as-its-host-app"></a>Converter um serviço de app para ser executado no mesmo processo de seu app host

Um [AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceconnection.aspx) permite que outro aplicativo ative seu aplicativo em segundo plano e inicie uma linha direta de comunicação com ele.

Com a introdução dos serviços de aplicativo no processo, dois aplicativos em primeiro plano em execução podem ter uma linha direta de comunicação por meio de uma conexão de serviço de aplicativo. Serviços de aplicativo agora podem ser executados no mesmo processo que o aplicativo em primeiro plano, o que torna a comunicação entre aplicativos muito mais fácil e elimina a necessidade de separar o código de serviço em um projeto separado.

A transformação de um serviço de aplicativo de modelo fora do processo em um modelo no processo exige duas alterações. A primeira é uma alteração de manifesto.

> ```xml
> <Package
>    ...
>   <Applications>
>       <Application Id=...
>           ...
>           EntryPoint="...">
>           <Extensions>
>               <uap:Extension Category="windows.appService">
>                   <uap:AppService Name="InProcessAppService" />
>               </uap:Extension>
>           </Extensions>
>           ...
>       </Application>
>   </Applications>
> ```

Remover o `EntryPoint` de atributos do `<Extension>` elemento porque agora [OnBackgroundActivated()](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) é o ponto de entrada que será usado quando o serviço de aplicativo é invocado.

A segunda alteração é mover a lógica de serviço do seu projeto de tarefa em segundo plano separado para métodos que podem ser chamados de **OnBackgroundActivated()**.

Agora seu aplicativo pode executar diretamente o serviço de aplicativo. Por exemplo, no App.xaml.cs:

[!NOTE] O código a seguir é diferente daquele fornecido para o exemplo 1 (serviço de out-of-process). O código a seguir é fornecido apenas para fins ilustrativos e não deve ser usado como parte do exemplo 2 (no processo de serviço).  Para continuar a transição do artigo do exemplo 1 (serviço de out-of-process) no exemplo 2 (serviço em processo) continuam a usar o código fornecido por exemplo, 1 em vez do código ilustrativo abaixo.

``` cs
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
...

sealed partial class App : Application
{
  private AppServiceConnection _appServiceConnection;
  private BackgroundTaskDeferral _appServiceDeferral;

  ...

  protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args)
  {
      base.OnBackgroundActivated(args);
      IBackgroundTaskInstance taskInstance = args.TaskInstance;
      AppServiceTriggerDetails appService = taskInstance.TriggerDetails as AppServiceTriggerDetails;
      _appServiceDeferral = taskInstance.GetDeferral();
      taskInstance.Canceled += OnAppServicesCanceled;
      _appServiceConnection = appService.AppServiceConnection;
      _appServiceConnection.RequestReceived += OnAppServiceRequestReceived;
      _appServiceConnection.ServiceClosed += AppServiceConnection_ServiceClosed;
  }

  private async void OnAppServiceRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
  {
      AppServiceDeferral messageDeferral = args.GetDeferral();
      ValueSet message = args.Request.Message;
      string text = message["Request"] as string;

      if ("Value" == text)
      {
          ValueSet returnMessage = new ValueSet();
          returnMessage.Add("Response", "True");
          await args.Request.SendResponseAsync(returnMessage);
      }
      messageDeferral.Complete();
  }

  private void OnAppServicesCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
  {
      _appServiceDeferral.Complete();
  }

  private void AppServiceConnection_ServiceClosed(AppServiceConnection sender, AppServiceClosedEventArgs args)
  {
      _appServiceDeferral.Complete();
  }
}
```

O código acima do método `OnBackgroundActivated` trata a ativação do serviço de aplicativo. Todos os eventos necessários para a comunicação por meio de um [AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceconnection.aspx) são registrados, e o objeto de adiamento da tarefa é armazenado para que ele possa ser marcado como concluído quando a comunicação entre os aplicativos terminar.

Quando o aplicativo recebe uma solicitação e lê o [ValueSet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset.aspx) fornecido para ver se as cadeias de caracteres `Key` e `Value` estão presentes. Se eles estiverem presentes, o serviço de aplicativo retornará um par de valores de cadeias `Response` e `True` para o aplicativo do outro lado do **AppServiceConnection**.

Saiba mais sobre como conectar e se comunicar com outros aplicativos em [Criar e consumir um serviço de aplicativo](https://msdn.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service?f=255&MSPPError=-2147217396).
